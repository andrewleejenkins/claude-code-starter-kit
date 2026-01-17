# Claude Code Development Starter Kit

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![GitHub release](https://img.shields.io/github/v/release/andrewleejenkins/claude-code-starter-kit)](https://github.com/andrewleejenkins/claude-code-starter-kit/releases/latest)
[![GitHub stars](https://img.shields.io/github/stars/andrewleejenkins/claude-code-starter-kit)](https://github.com/andrewleejenkins/claude-code-starter-kit/stargazers)
[![macOS](https://img.shields.io/badge/macOS-supported-brightgreen)](https://github.com/andrewleejenkins/claude-code-starter-kit)
[![Windows](https://img.shields.io/badge/Windows-supported-brightgreen)](https://github.com/andrewleejenkins/claude-code-starter-kit)
[![Linux](https://img.shields.io/badge/Linux-supported-brightgreen)](https://github.com/andrewleejenkins/claude-code-starter-kit)

Welcome! This starter kit will help you set up Claude Code as your AI-powered development assistant and configure it with professional development guidelines.

**[Download Latest Release](https://github.com/andrewleejenkins/claude-code-starter-kit/releases/latest/download/claude-code-starter-kit.zip)**

---

## What is Claude Code?

Claude Code is a command-line interface (CLI) tool from Anthropic that lets you work with Claude directly in your terminal. Unlike the web chat interface, Claude Code can:

- **Read and write files** on your computer
- **Run terminal commands** (build, test, deploy)
- **Navigate your codebase** and understand project structure
- **Create entire applications** from scratch
- **Debug issues** by reading error logs and fixing code

Think of it as having a senior developer sitting next to you who can actually touch your code, not just talk about it.

---

## Prerequisites

Before installing Claude Code, you'll need:

1. **Operating System:**
   - **macOS** (Intel or Apple Silicon)
   - **Linux** (most distributions)
   - **Windows 10/11** (with WSL2 recommended, or native support)

2. **Node.js 18+** installed ([download here](https://nodejs.org/))

3. **A code editor** (VS Code recommended - [download here](https://code.visualstudio.com/))

4. **A terminal:**
   - **Mac:** Terminal.app (built-in) or [iTerm2](https://iterm2.com/)
   - **Windows:** Windows Terminal ([download](https://aka.ms/terminal)), PowerShell, or Command Prompt
   - **Linux:** Your default terminal

### Check if Node.js is installed

Open your terminal and run:
```bash
node --version
```

If you see a version number like `v20.x.x`, you're good! If not, [download Node.js](https://nodejs.org/).

---

## Step 1: Install Claude Code

Open your terminal and run:

```bash
npm install -g @anthropic-ai/claude-code
```

This installs Claude Code globally so you can use it from anywhere.

### Verify Installation

```bash
claude --version
```

You should see a version number.

### Troubleshooting Installation

**On Windows (if you get permission errors):**
Run PowerShell or Command Prompt as Administrator, or use:
```bash
npm install -g @anthropic-ai/claude-code --force
```

**On Mac/Linux (if you get permission errors):**
```bash
sudo npm install -g @anthropic-ai/claude-code
```

---

## Step 2: Authenticate with Anthropic

Run Claude Code for the first time:

```bash
claude
```

This will open a browser window to authenticate with your Anthropic account. If you don't have an account, you'll need to create one at [anthropic.com](https://www.anthropic.com).

**Note:** Claude Code requires an Anthropic API subscription. Check [anthropic.com/pricing](https://www.anthropic.com/pricing) for current rates.

---

## Step 3: Set Up Your Projects Folder

Before starting your first project, you need a dedicated folder to store all your projects.

### On Mac/Linux

```bash
# Create a Projects folder in your home directory
mkdir -p ~/Projects

# Navigate to it
cd ~/Projects
```

### On Windows (PowerShell)

```powershell
# Create a Projects folder in your user directory
mkdir $HOME\Projects

# Navigate to it
cd $HOME\Projects
```

### On Windows (Command Prompt)

```cmd
# Create a Projects folder
mkdir %USERPROFILE%\Projects

# Navigate to it
cd %USERPROFILE%\Projects
```

**Where is this folder?**
- **Mac/Linux:** `/Users/YourName/Projects` or `/home/YourName/Projects`
- **Windows:** `C:\Users\YourName\Projects`

---

## Step 4: Set Up This Starter Kit

1. Download this starter kit (ZIP file)
2. Extract it to your `Projects` folder
3. Rename the folder to something like `0. Guides` or `dev-guides`

Your structure should look like:

**Mac/Linux:**
```
~/Projects/
├── 0. Guides/           # This starter kit
│   ├── README.md        # This file
│   ├── claude_start.md  # Onboarding assistant
│   ├── claude-development-guidelines.md
│   ├── dark-theme.md
│   └── light-theme.md
└── (your future projects will go here)
```

**Windows:**
```
C:\Users\YourName\Projects\
├── 0. Guides\           # This starter kit
│   ├── README.md        # This file
│   ├── claude_start.md  # Onboarding assistant
│   ├── claude-development-guidelines.md
│   ├── dark-theme.md
│   └── light-theme.md
└── (your future projects will go here)
```

---

## Step 5: Run the Setup Assistant

Now the fun part! Claude will help you configure everything.

### Start Claude Code in your guides folder

**Mac/Linux:**
```bash
cd ~/Projects/0.\ Guides
claude
```

**Windows (PowerShell):**
```powershell
cd "$HOME\Projects\0. Guides"
claude
```

**Windows (Command Prompt):**
```cmd
cd "%USERPROFILE%\Projects\0. Guides"
claude
```

### Give Claude the setup instructions

Once Claude is running, paste this message:

```
Please read the claude_start.md file and help me complete the initial setup. I'm new to this and would like you to guide me through each step.
```

Claude will:
1. Ask about your development preferences
2. Help you set up your style guides
3. Walk you through creating accounts for GitHub, Railway, Resend, and Google Cloud
4. Configure your development guidelines
5. Get you ready to build your first project!

---

## What's in This Kit?

| File | Purpose |
|------|---------|
| `README.md` | This file - installation and overview |
| `claude_start.md` | Instructions for Claude to onboard you |
| `claude-development-guidelines.md` | Your development standards and preferences |
| `dark-theme.md` | Dark theme style guide (you'll configure this) |
| `light-theme.md` | Light theme style guide (you'll configure this) |

---

## How to Use Claude Code Day-to-Day

### Starting a new project

**Mac/Linux:**
```bash
cd ~/Projects
mkdir my-new-project
cd my-new-project
claude
```

**Windows:**
```powershell
cd $HOME\Projects
mkdir my-new-project
cd my-new-project
claude
```

Then tell Claude what you want to build!

### Working on an existing project

**Mac/Linux:**
```bash
cd ~/Projects/my-existing-project
claude
```

**Windows:**
```powershell
cd $HOME\Projects\my-existing-project
claude
```

Claude will read your codebase and understand the project structure.

### Useful Commands

| Command | What it does |
|---------|--------------|
| `claude` | Start Claude Code in current directory |
| `claude "do something"` | Start with an initial prompt |
| `/help` | Show available commands (inside Claude) |
| `/clear` | Clear conversation history |
| `Ctrl+C` | Exit Claude Code |

---

## Tips for Working with Claude Code

### Be Specific
Instead of: "Make it better"
Try: "Add error handling to the login function that shows user-friendly messages"

### Start Small
Build features incrementally. Don't ask Claude to build an entire app at once.

### Review Changes
Claude will show you what files it wants to create or modify. Review them before approving.

### Use Your Guidelines
Reference your development guidelines when starting projects:

**Mac/Linux:**
```
Please read my development guidelines at ~/Projects/0. Guides/claude-development-guidelines.md and use them for this project.
```

**Windows:**
```
Please read my development guidelines at C:\Users\YourName\Projects\0. Guides\claude-development-guidelines.md and use them for this project.
```

### Save Conversations
Important conversations can be saved. Use `/save` to export the chat.

---

## Key Concepts

Before diving into projects, here are some important concepts you'll encounter when working with Claude Code:

### Planning Mode

**What is it?**
Planning Mode is a way to have Claude think through a problem before writing any code. Instead of immediately jumping into implementation, Claude will:
- Analyze your requirements
- Consider different approaches
- Identify potential challenges
- Create a step-by-step plan for you to review

**When to use it:**
- Starting a new feature or project
- Tackling complex problems with multiple possible solutions
- When you want to understand the approach before committing to it

**How to use it:**
Simply ask Claude to plan first:
```
Before writing any code, please create a plan for building a user authentication system.
```

Or Claude may automatically enter planning mode for complex tasks and ask for your approval before proceeding.

### Unit Tests

**What are they?**
Unit tests are small pieces of code that verify your application works correctly. Think of them as automated checklists that run through your code and confirm each piece does what it's supposed to do.

**Example:** If you have a function that adds two numbers, a unit test would:
1. Call the function with inputs like `add(2, 3)`
2. Check that the result equals `5`
3. Report "pass" or "fail"

**Why they matter:**
- **Catch bugs early** - Tests alert you when changes break existing features
- **Confidence to refactor** - You can improve code knowing tests will catch mistakes
- **Documentation** - Tests show how your code is supposed to behave

**How Claude helps:**
You can ask Claude to write tests for your code:
```
Please write unit tests for the user registration function.
```

Or ask Claude to run existing tests:
```
Run the tests and fix any failures.
```

### Security Audits

**What are they?**
A security audit is a review of your code to find vulnerabilities - weaknesses that hackers could exploit. Common vulnerabilities include:

- **SQL Injection** - When attackers can manipulate database queries
- **XSS (Cross-Site Scripting)** - When attackers can inject malicious scripts into your pages
- **Authentication flaws** - Weak password handling, session hijacking
- **Exposed secrets** - API keys or passwords accidentally committed to code

**Why they matter:**
Security vulnerabilities can lead to:
- User data being stolen
- Your app being taken over
- Legal and financial consequences
- Loss of user trust

**How Claude helps:**
You can ask Claude to review your code for security issues:
```
Please perform a security audit on the authentication module.
```

Or be more specific:
```
Check this API endpoint for SQL injection vulnerabilities.
```

Claude will identify issues, explain the risks, and suggest fixes.

---

## Example Prompts Cheat Sheet

Not sure what to ask Claude? Here are some prompts to get you started:

### Starting a Project
```
Create a new React app with TypeScript and Tailwind CSS
```
```
Set up an Express.js backend with a PostgreSQL database
```
```
Initialize this folder as a new project with your recommended structure
```

### Understanding Code
```
Explain what this code does
```
```
Walk me through how the authentication flow works
```
```
What does this error message mean?
```

### Building Features
```
Add a login page with Google OAuth
```
```
Create a contact form that sends emails
```
```
Add a dark mode toggle to the app
```

### Fixing Problems
```
Fix the error showing in the console
```
```
The login button isn't working - can you debug it?
```
```
Run the tests and fix any failures
```

### Code Quality
```
Review this code and suggest improvements
```
```
Add error handling to this function
```
```
Write unit tests for the user service
```

---

## Understanding Permissions

Claude Code asks for your permission before taking certain actions. This keeps you in control.

### Why Permissions Exist
- **Safety** - Prevents accidental file deletions or system changes
- **Transparency** - You always know what Claude is doing
- **Learning** - Helps you understand what operations are happening

### What Claude Asks Permission For
- **Creating files** - "Can I create `src/components/Button.tsx`?"
- **Editing files** - "Can I modify `package.json`?"
- **Running commands** - "Can I run `npm install express`?"
- **Deleting files** - "Can I delete `old-config.js`?"

### How to Respond
- **Yes/Approve** - Allow the action
- **No/Reject** - Block the action and optionally explain why
- **Always allow** - Trust this type of action for the session

### Tip
If Claude is asking too many permissions for routine tasks, you can adjust settings. But when starting out, it's good to review each action to learn what's happening.

---

## What to Do When Things Go Wrong

Mistakes happen! Here's how to recover:

### If Claude Makes an Unwanted Change

**Option 1: Ask Claude to undo it**
```
Undo that last change
```
```
Revert the file to how it was before
```

**Option 2: Use Git (if you've committed recently)**
```bash
# See what changed
git diff

# Discard all uncommitted changes
git checkout .

# Or discard changes to a specific file
git checkout -- filename.js
```

**Option 3: Use your editor's undo**
If the file is open in VS Code or another editor, `Ctrl+Z` (or `Cmd+Z` on Mac) can undo recent changes.

### If Claude Misunderstands You

Be more specific:
```
That's not quite what I meant. I want [clearer explanation]
```

Or start fresh:
```
Let's try a different approach. Instead of X, let's do Y.
```

### If You're Stuck in a Loop

Sometimes Claude might keep trying the same failing approach:
```
Stop. Let's step back and think about this differently.
```

Or exit and restart:
- Press `Ctrl+C` to exit Claude Code
- Run `claude` to start fresh

### If Something Breaks Badly

1. Don't panic
2. Exit Claude Code (`Ctrl+C`)
3. Check git status: `git status`
4. If needed, reset to last commit: `git checkout .`
5. Restart and try a different approach

---

## Slash Commands Reference

Inside Claude Code, you can use these commands:

| Command | What it does |
|---------|--------------|
| `/help` | Show all available commands |
| `/clear` | Clear the conversation history |
| `/compact` | Compress conversation to save context space |
| `/config` | View or change Claude Code settings |
| `/cost` | Show token usage and estimated costs |
| `/status` | Show current session information |
| `/save` | Export the conversation |
| `/quit` or `Ctrl+C` | Exit Claude Code |

### Tips
- Use `/compact` if Claude seems to be forgetting earlier context
- Check `/cost` periodically to monitor your API usage
- Use `/clear` when starting a completely new task

---

## Understanding Costs

Claude Code uses the Anthropic API, which charges based on usage.

### How Pricing Works

You're charged for **tokens** - units of text that Claude reads and writes:
- **Input tokens** - What you send to Claude (your prompts + file contents Claude reads)
- **Output tokens** - What Claude sends back (responses + code it writes)

### What Affects Cost

- **Conversation length** - Longer conversations use more tokens
- **File size** - Reading large files uses input tokens
- **Code generation** - More code output = more output tokens

### Tips to Manage Costs

1. **Be specific** - Clear prompts mean less back-and-forth
2. **Use `/compact`** - Compresses history to reduce token usage
3. **Start fresh when needed** - Use `/clear` for unrelated tasks
4. **Check usage** - Run `/cost` to see current session costs

### Typical Costs

Small projects and learning typically cost a few dollars per month. Complex, extended coding sessions cost more. Check [anthropic.com/pricing](https://www.anthropic.com/pricing) for current rates.

---

## Environment Variables Explained

You'll hear about "environment variables" and `.env` files a lot. Here's what they are:

### What Are Environment Variables?

Environment variables are settings that your app reads from its environment (the computer it's running on) rather than from the code itself.

**Example:** Instead of writing this in your code:
```javascript
// BAD - password visible in code!
const databasePassword = "supersecret123";
```

You use an environment variable:
```javascript
// GOOD - password comes from environment
const databasePassword = process.env.DATABASE_PASSWORD;
```

### Why Use Them?

1. **Security** - Secrets aren't stored in your code (which might be shared or uploaded to GitHub)
2. **Flexibility** - Different values for development vs. production
3. **Convenience** - Change settings without changing code

### The .env File

A `.env` file stores environment variables locally:
```
DATABASE_URL=postgresql://localhost:5432/myapp
RESEND_API_KEY=re_abc123xyz
GOOGLE_CLIENT_ID=1234567890.apps.googleusercontent.com
STRIPE_SECRET_KEY=sk_test_abc123
```

### Important Rules

1. **Never commit `.env` to Git** - Add it to `.gitignore`
2. **Never share `.env` files** - They contain secrets
3. **Create `.env.example`** - A template showing required variables (without real values)
4. **Set variables in production** - Railway, Vercel, etc. have dashboards for this

### How Claude Helps

When building projects, Claude will:
- Create `.env.example` files showing what variables you need
- Remind you to add `.env` to `.gitignore`
- Help you set up environment variables in Railway or other hosts

---

## Troubleshooting

### "command not found: claude" (Mac/Linux) or "'claude' is not recognized" (Windows)
Node.js might not be in your PATH. Try:
```bash
npx @anthropic-ai/claude-code
```

Or reinstall Node.js and make sure to check "Add to PATH" during installation.

### Authentication issues
Run `claude auth logout` then `claude` to re-authenticate.

### Claude can't find files
Make sure you're in the correct directory before starting Claude. Use `pwd` (Mac/Linux) or `cd` (Windows) to check your current location.

### Permission errors on Mac/Linux
```bash
chmod -R 755 ~/Projects
```

### Permission errors on Windows
Run your terminal as Administrator, or check that you own the folder in Properties > Security.

### WSL2 Recommended for Windows
While Claude Code works natively on Windows, many developers prefer using WSL2 (Windows Subsystem for Linux) for a more Unix-like experience. To install:
1. Open PowerShell as Administrator
2. Run: `wsl --install`
3. Restart your computer
4. Follow the Linux instructions from then on

---

## Getting Help

- **Claude Code Documentation:** [docs.anthropic.com](https://docs.anthropic.com)
- **Report Issues:** [github.com/anthropics/claude-code/issues](https://github.com/anthropics/claude-code/issues)
- **Anthropic Support:** [support.anthropic.com](https://support.anthropic.com)

---

## Next Steps

1. Complete Step 5 (run the setup assistant)
2. Create your first project
3. Build something amazing!

Happy coding!
