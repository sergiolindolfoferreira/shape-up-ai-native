# Agent Database Access for Debugging

**Purpose:** Enable AI agents to diagnose data-related bugs independently using read-only database replicas.

**Status:** Production-ready pattern used by PILL

---

## Why Database Access for Agents?

Traditional debugging workflow:
```
Bug reported → Developer checks logs → 
Developer runs manual SQL queries → 
Developer sends results to agent → 
Agent analyzes → Suggests fix →
Developer tests → Deploy
```

With direct database access:
```
Bug reported → Agent queries database directly →
Agent identifies root cause → Proposes fix →
Developer reviews PR → Deploy
```

**Time saved:** 2-4 hours per data bug  
**Risk:** Zero (read-only replicas)

---

## Architecture

```
┌─────────────────┐
│ Production DB   │ (Azure SQL / PostgreSQL / MySQL)
└────────┬────────┘
         │ Replication
         ↓
┌─────────────────┐
│ Read-Only       │ ← Agent connects here
│ Replica         │   (diagnosis only)
└─────────────────┘

┌─────────────────┐
│ Development DB  │ ← Agent tests fixes here
└─────────────────┘   (can write)
```

### Key Principles

1. **Never touch production directly**
   - Use read-only replicas for diagnosis
   - Use development databases for testing fixes
   - All fixes go through code → PR → review → deploy

2. **Security layers**
   - Read-only database user (explicitly deny writes)
   - IP whitelist (agent machine only)
   - Credentials in `.gitignore`d files
   - Regular password rotation

3. **Observability**
   - Log all agent queries
   - Monitor replica performance
   - Alert on suspicious patterns

---

## Prerequisites

### 1. Database Setup

**Create Read-Only Replica:**

**Azure SQL:**
```bash
# In Azure Portal
SQL Database → Geo-replication → Add replica
# Configure in same region for low latency
```

**PostgreSQL:**
```bash
# Configure streaming replication
# Or use managed service (RDS Read Replica, Cloud SQL Replica)
```

**Create Read-Only User:**

```sql
-- PostgreSQL example
CREATE ROLE agent_readonly WITH LOGIN PASSWORD 'strong_password';
GRANT CONNECT ON DATABASE mydb TO agent_readonly;
GRANT USAGE ON SCHEMA public TO agent_readonly;
GRANT SELECT ON ALL TABLES IN SCHEMA public TO agent_readonly;
ALTER DEFAULT PRIVILEGES IN SCHEMA public 
  GRANT SELECT ON TABLES TO agent_readonly;

-- Azure SQL example
USE master;
CREATE LOGIN [AgentReadOnly] WITH PASSWORD = 'strong_password';
USE [YourDatabase];
CREATE USER [AgentReadOnly] FOR LOGIN [AgentReadOnly];
ALTER ROLE db_datareader ADD MEMBER [AgentReadOnly];
GRANT VIEW DEFINITION TO [AgentReadOnly];
-- Explicitly deny writes (defensive)
DENY INSERT, UPDATE, DELETE, ALTER, CREATE, DROP TO [AgentReadOnly];
```

**Configure Firewall:**
```bash
# Allow only agent machine IP
# For Azure: Portal → SQL Server → Networking → Firewall rules
# Add: agent-machine-name | [Agent IP] | [Agent IP]
```

### 2. Agent Machine Setup

**macOS:**
```bash
# Install Homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# For Azure SQL
brew install unixodbc
brew tap microsoft/mssql-release https://github.com/Microsoft/homebrew-mssql-release
HOMEBREW_NO_ENV_FILTERING=1 ACCEPT_EULA=y brew install msodbcsql18

# For PostgreSQL/MySQL
brew install postgresql  # or mysql

# Python dependencies
pip3 install pyodbc      # For Azure SQL / SQL Server
pip3 install psycopg2    # For PostgreSQL
pip3 install mysql-connector-python  # For MySQL
```

**Linux:**
```bash
# Azure SQL
curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
curl https://packages.microsoft.com/config/ubuntu/$(lsb_release -rs)/prod.list > /etc/apt/sources.list.d/mssql-release.list
apt-get update
ACCEPT_EULA=Y apt-get install -y msodbcsql18
pip3 install pyodbc

# PostgreSQL
apt-get install -y postgresql-client libpq-dev
pip3 install psycopg2-binary

# MySQL
apt-get install -y mysql-client python3-dev
pip3 install mysql-connector-python
```

---

## Implementation

### 1. Credential Management

**Never commit credentials!** Store in `.gitignore`d file:

```bash
# ~/.agent-config/db-credentials.env
# (Add to .gitignore!)

PROD_DB_HOST=replica.database.windows.net
PROD_DB_NAME=myapp_production
PROD_DB_USER=agent_readonly
PROD_DB_PASSWORD=strong_password_here

DEV_DB_HOST=dev.database.windows.net
DEV_DB_NAME=myapp_development
DEV_DB_USER=agent_dev
DEV_DB_PASSWORD=dev_password_here
```

### 2. Database Helper Module

**`db_helper.py`** - Generic template:

```python
#!/usr/bin/env python3
"""
Database Helper for AI Agent
Supports: Azure SQL, PostgreSQL, MySQL
"""

import os
from typing import List, Tuple, Optional, Any

# Choose your driver
# import pyodbc          # Azure SQL / SQL Server
# import psycopg2        # PostgreSQL
# import mysql.connector # MySQL

class DatabaseHelper:
    """Manage connections to production replicas and development databases"""
    
    def __init__(self, env_file: str = "~/.agent-config/db-credentials.env"):
        """Load credentials from environment file"""
        self.load_credentials(os.path.expanduser(env_file))
    
    def load_credentials(self, env_file: str):
        """Parse environment file (key=value format)"""
        if not os.path.exists(env_file):
            raise FileNotFoundError(f"Credentials file not found: {env_file}")
        
        with open(env_file) as f:
            for line in f:
                line = line.strip()
                if line and not line.startswith('#'):
                    key, value = line.split('=', 1)
                    os.environ[key] = value
    
    # === Azure SQL / SQL Server ===
    
    def connect_prod(self):
        """Connect to production replica (read-only)"""
        import pyodbc
        conn_str = (
            f"DRIVER={{ODBC Driver 18 for SQL Server}};"
            f"Server={os.getenv('PROD_DB_HOST')};"
            f"Database={os.getenv('PROD_DB_NAME')};"
            f"Uid={os.getenv('PROD_DB_USER')};"
            f"Pwd={os.getenv('PROD_DB_PASSWORD')};"
            f"Encrypt=yes;"
            f"TrustServerCertificate=no;"
            f"Connection Timeout=30;"
        )
        return pyodbc.connect(conn_str)
    
    def connect_dev(self):
        """Connect to development database (read-write)"""
        import pyodbc
        conn_str = (
            f"DRIVER={{ODBC Driver 18 for SQL Server}};"
            f"Server={os.getenv('DEV_DB_HOST')};"
            f"Database={os.getenv('DEV_DB_NAME')};"
            f"Uid={os.getenv('DEV_DB_USER')};"
            f"Pwd={os.getenv('DEV_DB_PASSWORD')};"
            f"Encrypt=yes;"
            f"TrustServerCertificate=no;"
            f"Connection Timeout=30;"
        )
        return pyodbc.connect(conn_str)
    
    # === PostgreSQL ===
    
    # def connect_prod(self):
    #     """Connect to production replica (read-only)"""
    #     import psycopg2
    #     return psycopg2.connect(
    #         host=os.getenv('PROD_DB_HOST'),
    #         database=os.getenv('PROD_DB_NAME'),
    #         user=os.getenv('PROD_DB_USER'),
    #         password=os.getenv('PROD_DB_PASSWORD'),
    #         sslmode='require'
    #     )
    
    # === MySQL ===
    
    # def connect_prod(self):
    #     """Connect to production replica (read-only)"""
    #     import mysql.connector
    #     return mysql.connector.connect(
    #         host=os.getenv('PROD_DB_HOST'),
    #         database=os.getenv('PROD_DB_NAME'),
    #         user=os.getenv('PROD_DB_USER'),
    #         password=os.getenv('PROD_DB_PASSWORD'),
    #         ssl_disabled=False
    #     )
    
    # === Query Helpers ===
    
    def query_prod(self, sql: str, params: Optional[Tuple] = None) -> List[Any]:
        """Execute SELECT query on production replica"""
        conn = self.connect_prod()
        cursor = conn.cursor()
        
        if params:
            cursor.execute(sql, params)
        else:
            cursor.execute(sql)
        
        rows = cursor.fetchall()
        conn.close()
        return rows
    
    def query_dev(self, sql: str, params: Optional[Tuple] = None) -> List[Any]:
        """Execute query on development database (can write)"""
        conn = self.connect_dev()
        cursor = conn.cursor()
        
        if params:
            cursor.execute(sql, params)
        else:
            cursor.execute(sql)
        
        rows = cursor.fetchall()
        conn.close()
        return rows
    
    def get_table_columns(self, table_name: str, use_prod: bool = True) -> List[Tuple[str, str]]:
        """Get column names and types for a table"""
        sql = """
            SELECT COLUMN_NAME, DATA_TYPE 
            FROM INFORMATION_SCHEMA.COLUMNS 
            WHERE TABLE_NAME = ?
            ORDER BY ORDINAL_POSITION
        """
        
        if use_prod:
            return self.query_prod(sql, (table_name,))
        else:
            return self.query_dev(sql, (table_name,))


# Singleton instance
db = DatabaseHelper()


# Convenience functions for quick access
def query_prod(sql: str, params: Optional[Tuple] = None) -> List[Any]:
    """Quick access to production replica"""
    return db.query_prod(sql, params)


def query_dev(sql: str, params: Optional[Tuple] = None) -> List[Any]:
    """Quick access to development database"""
    return db.query_dev(sql, params)


# Test connections
if __name__ == "__main__":
    print("Testing database connections...")
    
    try:
        rows = query_prod("SELECT TOP 1 * FROM users")
        print(f"✅ Production replica: {len(rows)} rows")
    except Exception as e:
        print(f"❌ Production failed: {e}")
    
    try:
        rows = query_dev("SELECT * FROM users LIMIT 1")
        print(f"✅ Development: {len(rows)} rows")
    except Exception as e:
        print(f"❌ Development failed: {e}")
```

---

## Usage Examples

### Diagnosing a Bug

```python
from db_helper import query_prod

# 1. Find affected records
users = query_prod("""
    SELECT id, email, created_at, status
    FROM users
    WHERE status = 'pending'
      AND created_at >= '2026-02-01'
    ORDER BY created_at DESC
""")

print(f"Found {len(users)} pending users since Feb 1")

# 2. Check related data
for user in users:
    orders = query_prod("""
        SELECT COUNT(*) as order_count
        FROM orders
        WHERE user_id = ?
    """, (user[0],))
    
    print(f"User {user[1]}: {orders[0][0]} orders")

# 3. Identify pattern
# Discovered: Users with 0 orders stuck in 'pending' status
# Root cause: welcome_email_sent flag not set → status never transitions
```

### Testing a Fix

```python
from db_helper import query_dev

# Test UPDATE in development first
query_dev("""
    UPDATE users
    SET status = 'active', welcome_email_sent = TRUE
    WHERE status = 'pending'
      AND created_at < NOW() - INTERVAL '7 days'
      AND welcome_email_sent = FALSE
""")

# Verify
result = query_dev("""
    SELECT COUNT(*) FROM users WHERE status = 'active'
""")
print(f"✅ Fixed {result[0][0]} users in development")

# Now implement fix in code (not direct UPDATE in production!)
```

### Exploring Schema

```python
from db_helper import db

# Discover table structure
columns = db.get_table_columns("orders")
for col in columns:
    print(f"  {col[0]:30s} {col[1]}")

# Find foreign keys
query_prod("""
    SELECT 
        tc.table_name,
        kcu.column_name,
        ccu.table_name AS foreign_table_name
    FROM information_schema.table_constraints AS tc 
    JOIN information_schema.key_column_usage AS kcu
      ON tc.constraint_name = kcu.constraint_name
    JOIN information_schema.constraint_column_usage AS ccu
      ON ccu.constraint_name = tc.constraint_name
    WHERE tc.constraint_type = 'FOREIGN KEY'
      AND tc.table_name = 'orders'
""")
```

---

## Best Practices

### ✅ DO

1. **Always use production replicas for diagnosis**
   - Zero risk to production data
   - Same data as production
   - No performance impact

2. **Test fixes in development first**
   - Validate UPDATE/INSERT/DELETE statements
   - Confirm expected results
   - Check for side effects

3. **Use parameterized queries**
   ```python
   # Good (prevents SQL injection)
   query_prod("SELECT * FROM users WHERE id = ?", (user_id,))
   
   # Bad (vulnerable to SQL injection)
   query_prod(f"SELECT * FROM users WHERE id = {user_id}")
   ```

4. **Log agent queries**
   ```python
   import logging
   logging.info(f"Agent query: {sql} | Params: {params}")
   ```

5. **Close connections properly**
   - Use context managers or ensure `.close()`
   - Helper functions handle this automatically

### ❌ DON'T

1. **NEVER write to production** (replicas prevent this anyway)
2. **NEVER commit credentials**
3. **NEVER run expensive queries** (use LIMIT/TOP)
4. **NEVER bypass code review**
   - Database access is for debugging only
   - All fixes still require PR → review → deploy

5. **NEVER share credentials between developers**
   - Each gets own read-only user
   - Rotate passwords regularly

---

## Security Checklist

- [ ] Read-only replica created
- [ ] Firewall rules configured (specific IPs only)
- [ ] Read-only user created with explicit DENY on writes
- [ ] Credentials file in `.gitignore`
- [ ] Credentials NOT in git history
- [ ] Query logging enabled
- [ ] Team aware of access boundaries
- [ ] Password rotation policy defined (quarterly)

---

## Troubleshooting

### Connection Refused

```bash
# Check firewall rules
# Add agent machine IP to whitelist
# Wait 5 minutes for propagation
```

### Driver Not Found

```bash
# macOS
odbcinst -q -d  # List available drivers

# Reinstall if missing
brew reinstall msodbcsql18
```

### Permission Denied

```sql
-- Verify user permissions
SELECT USER_NAME() as CurrentUser;
SELECT IS_ROLEMEMBER('db_datareader');  -- Should return 1
```

### Slow Queries

```python
# Use LIMIT/TOP
query_prod("SELECT TOP 100 * FROM large_table")

# Add WHERE clauses
query_prod("SELECT * FROM orders WHERE created_at >= ?", (cutoff_date,))
```

---

## Real-World Example

**Problem:** Dashboard showing 0 new signups for February, but customer reports are positive.

**Diagnosis with Database Access (5 minutes):**

```python
# 1. Check if users exist
users = query_prod("""
    SELECT COUNT(*) as count, DATE(created_at) as signup_date
    FROM users
    WHERE created_at >= '2026-02-01'
    GROUP BY DATE(created_at)
    ORDER BY signup_date DESC
""")
# Result: 127 users created in February!

# 2. Check dashboard metrics table
metrics = query_prod("""
    SELECT metric_date, signup_count
    FROM daily_metrics
    WHERE metric_date >= '2026-02-01'
    ORDER BY metric_date DESC
""")
# Result: All signup_count = 0

# 3. Root cause identified
# Job that updates daily_metrics is not running!
```

**Fix:** Restart scheduled job + backfill missing metrics.

**Time saved:** ~3 hours of back-and-forth manual queries

---

## Integration with Shape Up Workflow

### During **Shaping**

Agent can query production to:
- Estimate data volumes
- Identify edge cases
- Validate assumptions about data patterns

### During **Building**

Agent can:
- Diagnose bugs reported during development
- Validate fixes against real data
- Explore schema for new features

### During **Shipping**

Agent can:
- Verify fix in production replica
- Monitor metrics post-deploy
- Generate reports for stakeholders

---

## Cost Considerations

**Azure SQL Replica:** ~$50-150/month (same tier as production)  
**PostgreSQL RDS Read Replica:** ~$30-100/month  
**MySQL RDS Read Replica:** ~$25-80/month

**ROI:**
- Each bug diagnosis: 2-4 hours saved
- If 5 bugs/month: 10-20 hours saved
- At $75/hour: $750-1500/month saved

**Replica pays for itself in week 1.**

---

## References

- [Azure SQL Database Replicas](https://learn.microsoft.com/en-us/azure/azure-sql/database/active-geo-replication-overview)
- [AWS RDS Read Replicas](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_ReadRepl.html)
- [PostgreSQL Streaming Replication](https://www.postgresql.org/docs/current/warm-standby.html)
- [pyodbc Documentation](https://github.com/mkleehammer/pyodbc/wiki)

---

## Contributing

Have improvements to this guide? Open a PR!

This is a living document based on real-world use at **[PILL](https://madebypill.com)** with Shape Up + AI Native methodology.
