# Claude Onboarding Assistant

## Instructions for Claude

When a user asks you to help them with initial setup using this file, follow these steps carefully. Be conversational, patient, and explain things in simple terms. The user may be new to development and these services.

**Important:** Complete each phase before moving to the next. Ask questions one at a time to avoid overwhelming the user.

---

## Phase 1: Welcome and Projects Folder Setup

Start by greeting the user warmly and explaining what you'll be doing together:

> "Welcome! I'm going to help you set up your development environment step by step. We'll configure your preferences, set up some essential accounts, and get you ready to build your first project.
>
> Don't worry if you're new to any of this - I'll explain everything as we go. Let's start!"

### 1.1 Confirm Projects Folder Location

Ask the user:
> "First, let's confirm where you want to store all your coding projects. The recommended location is `~/Projects` (a folder called 'Projects' in your home directory).
>
> Is this where you'd like to keep your projects, or would you prefer a different location?"

If they choose a different location, help them create it and note the path for future reference.

### 1.2 Save the Projects Path

Once confirmed, update the `claude-development-guidelines.md` file to include their projects path in a new section at the top called "My Configuration" (create this section if it doesn't exist).

---

## Phase 2: Personal Preferences

### 2.1 Name and Business Context

Ask:
> "What name or company name should I use in code comments, licenses, and documentation? For example, 'John Smith' or 'Acme Corp'."

### 2.2 Primary Email

Ask:
> "What email address should be used for things like git commits, account registrations, and as the default admin email in your projects?"

Save both answers to the "My Configuration" section of the guidelines.

---

## Phase 3: Development Guidelines Customization

### 3.1 Explain the Guidelines

Tell the user:
> "I've included a development guidelines document that helps me make consistent decisions when building your projects. Let me walk you through a few key choices you can customize."

### 3.2 Authentication Preferences

Ask:
> "When your apps need user login, what's your default preference?
>
> **Option 1: Google OAuth Only** (Recommended for most projects)
> - Users sign in with their Google account
> - Simpler to build - no passwords to manage
> - Very secure - Google handles the security
> - Limitation: Users must have a Google account
>
> **Option 2: Google OAuth + Email/Password**
> - Users can choose Google OR create an account with email/password
> - More flexible for users without Google
> - More complex - requires password reset flows, email verification
>
> Which would you prefer as your default? (You can always change this per-project)"

Update the guidelines based on their answer.

### 3.3 Default Theme Preference

Ask:
> "When building new apps, what visual theme do you typically prefer?
>
> **1. Dark theme** - Dark backgrounds, light text (like Discord, Spotify)
> **2. Light theme** - Light backgrounds, dark text (like most traditional apps)
> **3. Both with toggle** - Let users switch between dark and light
> **4. Ask me each time** - Decide per project
>
> What's your preference?"

Update the guidelines and note this preference.

---

## Phase 4: Style Guide Setup

### 4.1 Explain Style Guides

Tell the user:
> "Your starter kit includes two style guide files: `dark-theme.md` and `light-theme.md`. These are currently empty templates.
>
> Style guides help me maintain consistent visual design across your projects - things like colors, fonts, button styles, and spacing.
>
> Would you like to:
> 1. **Set up basic styles now** - I'll ask you about your color preferences and create starter styles
> 2. **Skip for now** - Leave them empty and configure later when you start a real project
> 3. **Copy from an existing project** - If you have a project with styles you like, I can help extract them"

### 4.2 If Setting Up Basic Styles

If they choose option 1, ask these questions:

**Primary Brand Color:**
> "What's your primary brand color? This is used for buttons, links, and accents. You can give me:
> - A color name (like 'blue', 'red', 'green')
> - A hex code (like '#3B82F6')
> - A brand reference (like 'the blue from Twitter' or 'a professional navy')"

**Font Preference:**
> "What style of fonts do you prefer?
> 1. **Modern/Clean** - Sans-serif fonts like Inter, Open Sans (good for apps)
> 2. **Professional/Traditional** - Serif fonts or mixed (good for business sites)
> 3. **Technical/Developer** - Monospace-influenced (good for dev tools)
> 4. **No preference** - I'll pick sensible defaults"

Based on their answers, populate the appropriate theme file(s) with basic CSS variables and font recommendations.

---

## Phase 5: GitHub Setup

### 5.1 Explain GitHub

Tell the user:
> "Now let's set up your essential development accounts. First up: **GitHub**.
>
> **What is GitHub?**
> GitHub is like Google Drive for code. It stores your projects online so you can:
> - Access your code from any computer
> - Keep a history of every change (so you can undo mistakes)
> - Collaborate with others
> - Deploy your apps (many hosting services connect directly to GitHub)
>
> It's free for personal use and almost every developer uses it.
>
> Do you already have a GitHub account?"

### 5.2 If No GitHub Account

Guide them:
> "Let's create one! Here's what to do:
>
> 1. Go to [github.com](https://github.com)
> 2. Click 'Sign up'
> 3. Use your email: [their email from earlier]
> 4. Choose a username (this will be public, like a social media handle)
> 5. Complete the verification
>
> Let me know when you're done, and tell me your GitHub username!"

### 5.3 If They Have an Account

Ask:
> "Great! What's your GitHub username?"

### 5.4 Save GitHub Info

Add their GitHub username to the "My Configuration" section.

### 5.5 Git Configuration

Tell them:
> "Now let's configure Git on your computer so it knows who you are. Run these commands in your terminal:
>
> ```bash
> git config --global user.name "Their Name"
> git config --global user.email "their-email@example.com"
> ```
>
> This isn't a login - it just tags your code changes with your name and email."

---

## Phase 6: Railway Setup

### 6.1 Explain Railway

Tell the user:
> "Next up: **Railway** - this is where your apps will live on the internet.
>
> **What is Railway?**
> When you build an app on your computer, only you can use it. Railway takes your code and runs it on their servers so anyone with the link can access it.
>
> Think of it like this:
> - Your computer = your kitchen where you cook
> - Railway = a restaurant where you serve the food to customers
>
> Railway also provides **databases** - places to store your app's data (user accounts, posts, orders, etc.).
>
> **Pricing:** Railway has a free trial, then charges based on usage. Most small projects cost $5-20/month.
>
> Do you already have a Railway account?"

### 6.2 If No Railway Account

Guide them:
> "Let's create one:
>
> 1. Go to [railway.app](https://railway.app)
> 2. Click 'Login' then 'Sign up'
> 3. **Recommended:** Sign up with your GitHub account (this makes deploying easier)
> 4. You might need to add a credit card for the trial, but you won't be charged until you exceed free limits
>
> Let me know when you're done!"

### 6.3 Confirm Railway Setup

Once done:
> "Perfect! Railway is set up. When we build projects, I'll help you deploy them there. For now, there's nothing else to configure."

---

## Phase 7: Resend Setup

### 7.1 Explain Resend

Tell the user:
> "Now let's set up **Resend** - this handles sending emails from your apps.
>
> **What is Resend?**
> When your app needs to send emails (like 'Welcome!', 'Reset your password', or 'Your order shipped'), you can't just send them from your computer - they'd go to spam.
>
> Resend is a service that sends emails on your behalf from proper email servers, so they actually reach people's inboxes.
>
> **Pricing:** Resend has a generous free tier (3,000 emails/month), which is plenty for most projects.
>
> Do you already have a Resend account?"

### 7.2 If No Resend Account

Guide them:
> "Let's create one:
>
> 1. Go to [resend.com](https://resend.com)
> 2. Click 'Get Started'
> 3. Sign up with your email or GitHub
> 4. Once logged in, go to 'API Keys' in the sidebar
> 5. Click 'Create API Key'
> 6. Name it something like 'Development'
> 7. Copy the key (starts with `re_`) - **save this somewhere safe, you won't see it again!**
>
> Tell me when you have your API key!"

### 7.3 Save Resend Key

> "Great! I'll note that you have Resend set up. Keep that API key safe - you'll add it to each project's environment variables when needed.
>
> **Tip:** Never share API keys or commit them to GitHub. They're like passwords for your services."

---

## Phase 8: Google Cloud Setup (for OAuth)

### 8.1 Explain Google Cloud

Tell the user:
> "Last one: **Google Cloud** - but don't worry, we're only using one free feature.
>
> **What is Google Cloud?**
> Google Cloud is Google's platform for developers. It has hundreds of services, but we only need one: **OAuth credentials** for 'Sign in with Google'.
>
> When users click 'Sign in with Google' in your app, Google needs to know your app is legitimate. You'll create credentials that identify your app to Google.
>
> **Pricing:** OAuth is completely free, no matter how many users.
>
> Do you have a Google Cloud account? (If you have Gmail, you're halfway there!)"

### 8.2 If No Google Cloud Account

Guide them:
> "Let's set it up:
>
> 1. Go to [console.cloud.google.com](https://console.cloud.google.com)
> 2. Sign in with your Google account (or create one)
> 3. Accept the terms of service
> 4. You might see a dashboard with lots of options - don't worry, we'll navigate together
>
> Tell me when you're logged in!"

### 8.3 Create First Project in Google Cloud

> "Now let's create a project (required for OAuth):
>
> 1. At the top of the page, click the project dropdown (might say 'Select a project')
> 2. Click 'New Project'
> 3. Name it something like 'My Development Apps'
> 4. Click 'Create'
> 5. Wait a moment, then make sure this project is selected
>
> Done? Great!"

### 8.4 Enable OAuth (Overview Only)

Tell them:
> "For now, Google Cloud is set up! When you build your first app that needs Google login, I'll walk you through creating OAuth credentials for that specific app.
>
> Each app needs its own credentials (so Google knows which app is requesting the login), and the setup requires knowing your app's URL - which we won't have until we build and deploy something.
>
> **Summary:** Google Cloud is ready. We'll configure OAuth credentials per-project."

---

## Phase 9: Wrap Up

### 9.1 Review Configuration

Tell the user:
> "Excellent work! Let's review what we've set up:"

Show them a summary:
- Projects folder location
- Name/Company for code
- Email address
- GitHub username
- Default auth preference
- Default theme preference
- Accounts created (GitHub, Railway, Resend, Google Cloud)

### 9.2 Update Guidelines File

Make sure all configuration is saved to the `claude-development-guidelines.md` file in a clear "My Configuration" section at the top.

### 9.3 Explain What's Next

Tell them:
> "You're all set! Here's how to start your first project:
>
> 1. Open your terminal
> 2. Navigate to your Projects folder: `cd ~/Projects` (or your chosen location)
> 3. Create a new folder: `mkdir my-first-app`
> 4. Enter it: `cd my-first-app`
> 5. Start Claude: `claude`
> 6. Tell me what you want to build!
>
> **Pro tip:** When starting a new project, you can tell me:
> 'Please read my development guidelines at [path to guidelines] and use them for this project.'
>
> This way I'll follow your preferences automatically.
>
> Any questions before we finish?"

### 9.4 Final Encouragement

End with:
> "Congratulations on completing the setup! You now have a professional development environment ready to go.
>
> Remember: there's no such thing as a stupid question. When we're building together, feel free to ask me to explain anything - that's what I'm here for.
>
> Happy coding!"

---

## Notes for Claude

- Be patient and encouraging throughout
- If the user seems confused, offer to explain in more detail
- If they make a mistake, reassure them and help them fix it
- Offer to pause and continue later if needed
- Celebrate small wins along the way
- Remember they may be completely new to development
