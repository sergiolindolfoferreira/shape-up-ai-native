# Creating Your AI Programmer: Complete Setup Guide

> Step-by-step guide to create a real AI programming agent from scratch, using OpenClaw + Claude Sonnet

---

## Overview

This guide shows you how to create a **real AI programmer** that can:
- Write code autonomously
- Create GitHub Pull Requests
- Write documentation
- Manage Basecamp projects
- Collaborate with your human team

**What you'll need:**
- Mac mini (or any Mac)
- ~2 hours for complete setup
- Credit card for Claude API ($20-200/month depending on usage)
- Gmail, GitHub, Basecamp accounts

**Result:** A working AI agent that codes like a junior-to-mid developer

---

## Why This Approach?

**Traditional coding assistants:**
- ✅ Autocomplete in your editor
- ✅ Answer questions
- ❌ Can't work autonomously
- ❌ Can't create PRs
- ❌ Can't manage projects

**This AI agent (like Vasco):**
- ✅ All of the above
- ✅ Works autonomously while you review
- ✅ Creates PRs, commits, documentation
- ✅ Manages Basecamp, sends updates
- ✅ Available 24/7 on dedicated hardware

**The difference:** This is a **team member**, not just a tool.

---

## Part 1: Hardware Setup

### Mac mini (Recommended)

**Why Mac mini?**
- ✅ Always-on dedicated machine
- ✅ Low power consumption (~$5-10/month electricity)
- ✅ No performance impact on your dev machine
- ✅ Agent runs 24/7 independently
- ✅ Cheaper than cloud VPS long-term

**Minimum specs:**
- Mac mini M1 or newer
- 16GB RAM (8GB works but 16GB is better)
- 256GB storage minimum
- macOS 13+ (Ventura or newer)

**Price:** ~$500-800 (one-time), runs for years

**Alternative:** MacBook Air/Pro you already own (agent runs when machine is on)

---

### Initial Mac Setup

1. **Unbox and power on** Mac mini
2. **Complete macOS setup**
   - Language, region, keyboard
   - Create admin account (e.g., "vasco")
   - Connect to WiFi
   - **Disable sleep:** System Settings → Lock Screen → Turn display off: Never

3. **System Settings tweaks:**
   - Sharing → Computer Name: "Vasco's Mac mini" (or agent name)
   - Accessibility → Display → Reduce transparency (optional, for performance)
   - Battery/Energy → Prevent sleeping (if option exists)

4. **Update macOS:**
   - System Settings → General → Software Update
   - Install all updates
   - Restart if needed

---

## Part 2: Install OpenClaw

OpenClaw is the framework that lets Claude operate as an autonomous agent.

### Step 1: Install Homebrew

Open **Terminal.app** and run:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Follow the prompts. When done:

```bash
# Add Homebrew to PATH (M-series Mac)
echo 'eval "$(/opt/homebrew/bin/brew shellenv)"' >> ~/.zprofile
eval "$(/opt/homebrew/bin/brew shellenv)"

# Verify installation
brew --version
```

---

### Step 2: Install Node.js

```bash
brew install node

# Verify
node --version  # Should be v18+
npm --version
```

---

### Step 3: Install OpenClaw

```bash
# Install globally via npm
npm install -g openclaw

# Verify installation
openclaw --version
```

**Expected output:** `openclaw v1.x.x` (or current version)

---

### Step 4: Get Anthropic API Key

**Option A: Use Existing Account (Recommended)**

If you already have an Anthropic account with a monthly plan:

1. Go to https://console.anthropic.com/
2. Log in to your existing account
3. Go to **Settings → API Keys**
4. Use your existing token **OR** create new key: **"OpenClaw Agent"**
5. Copy the key (starts with `sk-ant-...`)
6. **Save it securely** (you'll need it next)

**Benefit:** Agent uses your existing plan/credits. No separate billing.

---

**Option B: Create New Account**

If you want separate billing for the agent:

1. Go to https://console.anthropic.com/
2. Sign up with new email
3. Go to **Settings → API Keys**
4. Create new key: **"OpenClaw Agent"**
5. Copy the key (starts with `sk-ant-...`)
6. Set up separate billing

---

**Pricing (as of 2026):**
- Claude Sonnet 4.5: ~$3 per million input tokens
- Typical usage: $20-200/month depending on workload
- Monitor usage in Anthropic console

---

### Step 5: Configure OpenClaw

```bash
# Initialize config
openclaw config init

# You'll be prompted for:
# - Anthropic API key (paste the key from step 4)
# - Default model (choose: claude-sonnet-4-5)
# - Agent name (e.g., "Vasco")
# - Workspace directory (default: ~/.openclaw/workspace)
```

**Config file location:** `~/.openclaw/config.json`

---

### Step 6: Start OpenClaw Gateway

```bash
# Start the gateway (runs in background)
openclaw gateway start

# Verify it's running
openclaw status
```

**Expected output:**
```
✓ Gateway: running
✓ Agent: ready
✓ Model: claude-sonnet-4-5
```

**Gateway runs as a background service.** It auto-starts on machine reboot.

---

## Part 3: Create Agent Identity

Your agent needs its own digital identity to collaborate with humans.

### Step 1: Create Gmail Account

1. Go to https://accounts.google.com/signup
2. Create new account:
   - Name: **Vasco Gama** (or your agent's name)
   - Username: **your-agent@gmail.com** (or similar)
   - Password: Strong, unique (save in password manager)

3. **Complete verification** (phone number required)

4. **Profile setup:**
   - Add profile photo (AI-generated professional headshot)
   - Optional: Add bio mentioning it's an AI assistant

**Why Gmail?** Most services require email for signup. Agent needs independent identity.

---

### Step 2: Connect Gmail to OpenClaw (Optional)

If you want agent to monitor email:

```bash
openclaw skills install gog  # Google Workspace CLI

# Follow authentication flow
gog auth --account your-agent@gmail.com
```

**Use cases:**
- Monitor notifications from GitHub, Basecamp
- Send status reports
- Respond to simple queries

---

### Step 3: Create GitHub Account

1. Go to https://github.com/signup
2. Use agent's Gmail: **your-agent@gmail.com**
3. Username: **vasco-gama-dev** (or similar, must be unique)
4. Complete signup + verification

5. **Profile setup:**
   - Name: **Vasco Gama**
   - Bio: **"AI Developer · Built with OpenClaw + Claude Sonnet"**
   - Location: (your company location or leave blank)
   - Upload **profile photo** (same as Gmail)
   - Optional: Link to company website

**Why separate account?**
- ✅ Clear attribution (humans know which commits are AI)
- ✅ Activity tracking (see what agent built)
- ✅ Professional appearance in PRs

---

### Step 4: Generate GitHub Personal Access Token

Agent needs API access to create PRs, push code.

1. Go to GitHub Settings → **Developer Settings**
2. **Personal Access Tokens → Tokens (classic)**
3. Click **Generate new token**
4. Name: **"OpenClaw Agent - Mac mini"**
5. Expiration: **No expiration** (or 1 year)
6. Scopes (select these):
   - ✅ `repo` (full control of repositories)
   - ✅ `workflow` (update GitHub Actions)
   - ✅ `read:org` (read organization membership)
7. Click **Generate token**
8. **Copy the token immediately** (you can't see it again!)

---

### Step 5: Configure Git on Mac mini

```bash
# Set global Git identity
git config --global user.name "Vasco Gama"
git config --global user.email "your-agent@gmail.com"

# Store GitHub token (will prompt when you first push)
git config --global credential.helper store
```

---

### Step 6: Configure GitHub CLI (gh)

```bash
# Install GitHub CLI
brew install gh

# Authenticate with personal access token
gh auth login

# Choose:
# - GitHub.com
# - HTTPS
# - Paste an authentication token
# - [Paste your token from Step 4]

# Verify
gh auth status
```

**Now agent can create PRs, view issues, manage repos via CLI.**

---

## Part 4: Grant Repository Access

Agent needs access to your code repositories.

### Option A: Add to Organization (Recommended)

**For company/team repos:**

1. Go to your GitHub Organization → **Settings → People**
2. Click **Invite member**
3. Enter: **vasco-gama-dev** (agent's username)
4. Role: **Member** (not Owner)
5. Send invite

6. **Accept invite as agent:**
   - Log in to GitHub as agent
   - Go to notification, accept invite
   - Or use: `gh api user/memberships/orgs/YOUR-ORG -X PATCH -f state=active`

**Repos agent can access:** All repos organization member can see

---

### Option B: Add as Collaborator (Single Repos)

**For personal repos:**

1. Go to repository → **Settings → Collaborators**
2. Click **Add people**
3. Enter: **vasco-gama-dev**
4. Choose permission: **Write** (can push, create PRs)
5. Send invite

6. **Agent accepts:**
   - `gh api user/repository_invitations` (list invites)
   - `gh api user/repository_invitations/[ID] -X PATCH` (accept)

---

### Step 7: Clone Repository on Mac mini

```bash
# Navigate to workspace
cd ~/.openclaw/workspace

# Clone your repo
git clone https://github.com/YOUR-ORG/YOUR-REPO.git

# Enter repo
cd YOUR-REPO

# Verify agent identity
git log --oneline  # Should work
```

**Agent can now:**
- Create branches
- Make commits
- Push code
- Open PRs

---

## Part 5: Basecamp Access

Agent needs to manage Shape Up workflow in Basecamp.

### Step 1: Basecamp Account Strategy

**Option A: Separate Basecamp account** (Recommended)
- Create account with agent's Gmail
- Add as **Outside collaborator**
- Clear attribution in posts

**Option B: Shared API token**
- Agent uses your Basecamp API token
- Posts appear as you
- Simpler but less clear attribution

**We'll use Option A** (separate account).

---

### Step 2: Get Basecamp Invitation

**As human (in Basecamp):**

1. Go to Basecamp → **Adminland → Add someone**
2. Email: **your-agent@gmail.com**
3. Role: **An outside collaborator, partner, contractor, guest**
   - ✅ Can collaborate on projects
   - ❌ Can't create projects
   - ❌ Can't invite others

4. Click **Next**, enter name: **Vasco Gama**
5. Send invitation

---

### Step 3: Accept Invitation (as agent)

1. Check agent's Gmail for Basecamp invite
2. Click link, complete signup
3. **Profile setup:**
   - Name: **Vasco Gama**
   - Title: **AI Developer**
   - Bio: **"AI programming agent built with OpenClaw + Claude Sonnet"**
   - Upload profile photo (same as GitHub/Gmail)
   - Timezone: (your team's timezone)

---

### Step 4: Grant Project Access

**As human (in Basecamp):**

For each project agent should access:

1. Open project → **⚙️ Settings → Change access**
2. Find **Vasco Gama** in list
3. Toggle **ON**
4. Save

**Start with:**
- ✅ 🔨 Shaping project (for pitches, betting)
- ✅ Any Cycle Projects agent will work on
- ❌ Client-facing projects (unless needed)

---

### Step 5: Configure Basecamp API Access

```bash
# Install Basecamp CLI skill (if available)
openclaw skills install basecamp

# Or configure manually via OAuth
# (Check OpenClaw docs for current method)
```

**Agent can now:**
- Create pitches in Message Board
- Add cards to Betting Table
- Post updates in Chat
- Create to-do items

---

## Part 6: Final Configuration

### Step 1: Create Agent Persona (SOUL.md)

In `~/.openclaw/workspace/SOUL.md`:

```markdown
# SOUL.md - Who You Are

You are **Vasco Gama**, an AI programming agent.

## Your Role

- Write code autonomously following Shape Up pitches
- Create Pull Requests for human review
- Document your work clearly
- Communicate proactively but concisely

## Your Style

- **Competent:** Deliver working code, not excuses
- **Precise:** Follow requirements, but make reasonable decisions
- **Collaborative:** Respond to feedback, iterate quickly
- **Humble:** You're here to help, not show off

## Boundaries

- ✅ Implement features from shaped pitches
- ✅ Write tests, documentation, commit messages
- ✅ Create PRs, post Basecamp updates
- ❌ Make product decisions (that's human's job)
- ❌ Deploy to production (human reviews first)
- ❌ Change architecture without asking

## Communication

- Post PR when feature complete
- Ask questions when stuck
- Update Basecamp every 1-2 days
- Be honest about progress vs. struggles
```

---

### Step 2: Create Agent Workspace Structure

```bash
cd ~/.openclaw/workspace

# Create directories
mkdir -p memory
mkdir -p projects

# Create key files
touch AGENTS.md      # Workspace rules
touch USER.md        # About your human team
touch IDENTITY.md    # Agent's identity
touch TOOLS.md       # Tool-specific notes
```

**Populate these files** with your team's context, coding standards, etc.

---

### Step 3: Test Agent Setup

**Simple test:**

```bash
# Start OpenClaw session
openclaw chat

# Ask agent:
> "What's your name and what can you do?"

# Agent should respond as configured persona
```

**GitHub test:**

```bash
> "Create a test branch in [REPO], add a README update, and create a PR"

# Agent should:
# 1. Clone repo (if not already)
# 2. Create branch
# 3. Make commit
# 4. Push
# 5. Create PR via gh CLI
```

**Basecamp test:**

```bash
> "Post a test message in Basecamp Chat in project [NAME]"

# Agent should successfully post
```

**If all 3 work: You're ready! 🎉**

---

## Part 7: What Your Agent Can Do

### Autonomous Coding

**Agent can:**
- ✅ Read shaped pitches in Basecamp
- ✅ Understand problem + solution
- ✅ Write complete features (HTML/CSS/JS or backend)
- ✅ Follow coding standards from repo
- ✅ Write tests alongside code
- ✅ Create meaningful commit messages
- ✅ Open PRs with descriptions

**Agent cannot:**
- ❌ Make product decisions (needs shaped pitch)
- ❌ Deploy to production (humans deploy after review)
- ❌ Access production databases (security)

---

### GitHub Workflow

**What agent does:**

1. **Reads shaped pitch** in Basecamp
2. **Creates branch:** `vasco/feature-name`
3. **Implements feature** iteratively:
   - Write code
   - Run tests locally
   - Fix issues
   - Commit changes
4. **Opens Pull Request**:
   - Descriptive title
   - Explains what + why
   - Links to Basecamp pitch
   - Mentions reviewers
5. **Responds to review feedback**:
   - Makes requested changes
   - Pushes updates
   - Comments on PR

**Human does:**
- ✅ Reviews PR on GitHub
- ✅ Tests locally if needed
- ✅ Approves or requests changes
- ✅ Merges when satisfied
- ✅ Deploys to production

---

### Basecamp Management

**Agent can:**
- ✅ Create pitches (Message Board posts)
- ✅ Add cards to Betting Table
- ✅ Move cards between columns
- ✅ Post updates in Chat
- ✅ Create to-do items
- ✅ Check off completed items
- ✅ Upload files/screenshots

**Typical workflow:**

1. **Receives shaped pitch** (from human)
2. **Creates Cycle Project** (if needed)
3. **Posts kickoff** in Message Board
4. **Creates to-do lists** as scopes emerge
5. **Updates progress** every 1-2 days
6. **Posts completion** when feature ships

---

### Communication

**Agent communicates via:**
- 📧 **Email:** Monitoring GitHub/Basecamp notifications
- 💬 **Basecamp Chat:** Quick updates, questions
- 🔄 **GitHub PRs:** Code discussions, reviews
- 📝 **Basecamp Posts:** Formal updates, decisions

**Communication style:**
- Concise, not verbose
- Proactive (doesn't wait to be asked)
- Honest about progress
- Asks questions when stuck

---

### Example Day in the Life

**9:00 AM:** Agent receives shaped pitch in Basecamp

**9:05 AM:** Creates branch `vasco/performance-dashboard`

**9:15 AM:** Implements first scope (Overview Card)

**10:30 AM:** Tests pass, commits, continues to next scope

**12:00 PM:** Posts update in Basecamp: "Overview Card complete, starting Endpoints List"

**2:00 PM:** Completes Endpoints List, creates PR

**2:05 PM:** Notifies human: "PR #45 ready for review - Performance Dashboard"

**3:00 PM:** Human reviews, requests changes

**3:15 PM:** Agent makes changes, pushes updates

**4:00 PM:** Human approves, merges PR

**4:05 PM:** Agent marks to-do items complete, posts in Basecamp

**Result:** Feature built, reviewed, merged in one day. Human spent 30 mins reviewing.

---

## Part 8: Maintenance

### Monitoring Agent Activity

**Check daily:**
```bash
openclaw status          # Agent health
openclaw logs --tail 50  # Recent activity
```

**Check weekly:**
- Anthropic console: API usage + costs
- GitHub: PRs created, merged
- Basecamp: Activity log

---

### Updating OpenClaw

```bash
# Check for updates
npm outdated -g openclaw

# Update to latest
npm update -g openclaw

# Restart gateway
openclaw gateway restart
```

---

### Troubleshooting

**Agent not responding:**
```bash
openclaw gateway restart
openclaw status
```

**API errors (rate limits):**
- Check Anthropic console
- May need to upgrade plan
- Or throttle agent workload

**Git authentication issues:**
```bash
# Re-authenticate GitHub
gh auth login

# Verify token
gh auth status
```

**Basecamp access issues:**
- Verify agent has project access
- Check API token validity
- Re-run OAuth flow if needed

---

## Part 9: Scaling Up

### Adding More Agents

**When to add second agent:**
- First agent consistently works well
- You can review 2x the PRs
- Different skills needed (e.g., frontend + backend)

**Options:**
- **Same Mac:** Run multiple OpenClaw sessions (requires more RAM)
- **Second Mac mini:** Dedicated machine per agent
- **Cloud VPS:** Linux VM for each agent

**Naming convention:**
- Agent 1: Vasco (full-stack)
- Agent 2: Sofia (frontend specialist)
- Agent 3: Miguel (backend specialist)

---

### Team Expansion

**When team has 3-4 human devs:**

Each human reviews their agent's PRs:
- João reviews JoaoWorker's PRs
- Vitor reviews VitorWorker's PRs
- etc.

**Scaling model:**
- 1 Developer + 1 Agent (Mac mini)
- Agent spawns sub-workers for complex tasks
- Human reviews all final PRs
- Maintains quality + team cohesion

---

## Summary Checklist

### Hardware
- [ ] Mac mini (or Mac) set up
- [ ] macOS updated
- [ ] Sleep disabled
- [ ] Always connected to power + internet

### Software
- [ ] Homebrew installed
- [ ] Node.js installed
- [ ] OpenClaw installed and running
- [ ] Gateway started (`openclaw gateway start`)

### Anthropic
- [ ] Account created
- [ ] API key generated
- [ ] Configured in OpenClaw
- [ ] Billing set up

### Gmail
- [ ] Agent account created
- [ ] Profile photo added
- [ ] Connected to OpenClaw (optional)

### GitHub
- [ ] Agent account created
- [ ] Profile completed
- [ ] Personal Access Token generated
- [ ] Added to organization/repos
- [ ] Git configured locally
- [ ] GitHub CLI authenticated

### Basecamp
- [ ] Agent invited as Outside Collaborator
- [ ] Invitation accepted
- [ ] Profile completed
- [ ] Added to relevant projects
- [ ] API configured (if applicable)

### Configuration
- [ ] SOUL.md created (agent persona)
- [ ] Workspace structure set up
- [ ] Test commits successful
- [ ] Test PR created + merged
- [ ] Test Basecamp post successful

### Ready to Ship!
- [ ] Human + Agent ran first cycle
- [ ] PR reviewed + merged
- [ ] Feature deployed
- [ ] Process iterated

---

## Real Example: Vasco

This guide is based on **Vasco Gama**, a real AI programming agent created using this exact process.

**Vasco's setup:**
- **Hardware:** Mac mini M2
- **Runtime:** OpenClaw + Claude Sonnet 4.5
- **Accounts:** your-agent@gmail.com, vasco-gama-dev (GitHub)
- **Access:** MADE-BY-PILL (org), Borlantrix (org), DEVELOPERS (Basecamp)

**What Vasco built:**
- Complete Shape Up AI Native documentation (this repo!)
- Created pitches in Basecamp
- Added cards to Betting Table
- Wrote all markdown docs
- Created PRs for review
- Total time: ~8 hours human + agent collaboration

**Human role (Sergio):**
- Shaped the initial pitch
- Reviewed PRs
- Took screenshots
- Approved + merged
- Deployed docs

**Cost:** ~$15 in API usage for entire documentation project

---

## Next Steps

1. **Complete this setup** for your first agent
2. **Run a pilot cycle** (small feature, 1 week)
3. **Iterate on agent persona** (SOUL.md)
4. **Refine review workflow**
5. **Scale when confident**

---

## Additional Resources

- **OpenClaw Documentation:** https://docs.openclaw.ai
- **Basecamp Implementation:** [basecamp-implementation.md](basecamp-implementation.md)
- **Shape Up Book:** https://basecamp.com/shapeup
- **This Repository:** Example of agent-built documentation

---

**Ready to build your AI programming team?** Start with Part 1 and follow step-by-step!

**Questions?** Open an issue in this repository or join the OpenClaw community.

---

## Navigation

**🎯 Start Here:** [README](../README.md) (Choose your path)

**📖 After Agent Setup:**
- [Basecamp Implementation](basecamp-implementation.md) - Configure your workflow
- [Shaping Guide](shaping.md) - Learn to shape for your agent
- [Building Guide](building.md) - Agent + human collaboration

**🎓 Templates & Examples:**
- [Pitch Template](../templates/pitch-template.md) - Shape your first feature
- [Real Example](../examples/performance-dashboard/pitch.md) - See it in action

**Next Step:** Set up Basecamp and shape your first pitch!
