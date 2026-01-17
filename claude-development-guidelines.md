# Claude Development Guidelines

This document provides Claude with project preferences, standards, and decision-making guidance for your development projects.

---

## My Configuration

> **Note:** This section is populated by running the setup assistant (`claude_start.md`).

| Setting | Value |
|---------|-------|
| Projects Folder | _Not configured_ |
| Name/Company | _Not configured_ |
| Email | _Not configured_ |
| GitHub Username | _Not configured_ |
| Default Auth | _Not configured_ |
| Default Theme | _Not configured_ |

---

## Table of Contents

1. [Related Documentation](#related-documentation)
2. [Default Tech Stack](#default-tech-stack)
3. [Project Structure](#project-structure)
4. [Infrastructure Organization](#infrastructure-organization)
5. [GitHub Repository Best Practices](#github-repository-best-practices)
6. [Authentication](#authentication-decision)
7. [Google Cloud Platform Usage](#google-cloud-platform-usage)
8. [Railway Configuration](#railway-configuration)
9. [Stripe Integration](#stripe-integration)
10. [Resend Email Configuration](#resend-email-configuration)
11. [Environment Variables](#environment-variables)
12. [Prisma Configuration](#prisma-configuration)
13. [Preventing Product Crosstalk](#preventing-product-crosstalk)
14. [Styling](#styling)
15. [Things to Avoid](#things-to-avoid)
16. [Quick Start Checklist](#quick-start-checklist-for-new-projects)
17. [Quick Reference](#quick-reference)

---

## Related Documentation

### Style Guides (Separate from this document)

Style guides are maintained separately so this document can be shared without imposing specific visual styles.

| Document | Purpose |
|----------|---------|
| [dark-theme.md](./dark-theme.md) | Dark mode design patterns and components |
| [light-theme.md](./light-theme.md) | Light mode design patterns and components |

> **At project start, ask:** "Should this project use dark theme, light theme, or support both?" Then reference the appropriate style guide(s).

---

## Default Tech Stack

Always use these technologies unless explicitly told otherwise:

### Frontend
| Category | Technology | Notes |
|----------|------------|-------|
| Framework | **React 18+** | Latest stable version |
| Build Tool | **Vite** | Not Create React App |
| Language | **TypeScript** | Strict mode enabled |
| Styling | **Tailwind CSS** | With configuration from style guides |
| Components | **shadcn/ui + Radix UI** | For UI primitives |
| Icons | **Lucide React** | Consistent icon library |
| State (client) | **Zustand** | Lightweight, simple state |
| State (server) | **TanStack React Query** | For API data fetching/caching |

### Backend
| Category | Technology | Notes |
|----------|------------|-------|
| Runtime | **Node.js (Latest LTS)** | Check nodejs.org for current LTS |
| Framework | **Express.js** | With TypeScript |
| ORM | **Prisma** | Always use Prisma for database access |
| Database | **PostgreSQL** | Hosted on Railway |
| Sessions | **@quixo3/prisma-session-store** | Prisma-based session storage |
| Dev Runtime | **tsx** | For TypeScript execution in development |

### Infrastructure & Services
| Category | Service | Notes |
|----------|---------|-------|
| Hosting | **Railway** | All-in-one for app + database |
| Database | **Railway PostgreSQL** | Provisioned through Railway |
| Email | **Resend** | Default email provider for all projects |
| Payments | **Stripe** | With separate endpoints + metadata filtering |
| Auth Provider | **Google OAuth** | Via Passport.js (discuss email/password at project start) |

### Security Middleware (Always Include)
- **helmet** - Security headers
- **cors** - CORS configuration
- **express-rate-limit** - Rate limiting
- **csrf-csrf** - CSRF protection (for session-based auth)

---

## Project Structure

**Always use the monorepo pattern** for full-stack projects:

```
project-name/
├── client/                 # React frontend
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── hooks/
│   │   ├── lib/
│   │   ├── styles/
│   │   └── main.tsx
│   ├── package.json
│   ├── tsconfig.json
│   ├── tailwind.config.js
│   └── vite.config.ts
├── server/                 # Express backend
│   ├── src/
│   │   ├── routes/
│   │   ├── middleware/
│   │   ├── services/
│   │   ├── lib/
│   │   └── index.ts
│   ├── prisma/
│   │   └── schema.prisma
│   ├── package.json
│   └── tsconfig.json
├── package.json            # Root workspace config
├── railway.json            # Railway deployment config
├── .env.example            # Environment template
├── .gitignore
└── README.md
```

### Root package.json Scripts (Standard)

```json
{
  "scripts": {
    "dev": "concurrently \"npm run dev:server\" \"npm run dev:client\"",
    "dev:server": "cd server && npm run dev",
    "dev:client": "cd client && npm run dev",
    "build": "npm run build:client && npm run build:server",
    "build:client": "cd client && npm run build",
    "build:server": "cd server && npm run build",
    "start": "cd server && npm start",
    "db:migrate": "cd server && npx prisma migrate dev",
    "db:push": "cd server && npx prisma db push",
    "db:generate": "cd server && npx prisma generate"
  }
}
```

### Standard Ports

| Service | Port |
|---------|------|
| Backend API | **3000** |
| Frontend Dev | **5173** |

Configure these in environment variables:
```bash
PORT=3000
CLIENT_URL=http://localhost:5173
```

---

## Infrastructure Organization

All products share common infrastructure accounts with proper isolation:

| Service | Strategy | Isolation Method |
|---------|----------|------------------|
| Stripe | One account, multiple products | Separate webhook endpoints + metadata filtering |
| GitHub | One account, multiple repos | Private repos by default, one repo per product |
| Railway | One account, multiple projects | One Railway project per product |
| Google Cloud | One account | Only for OAuth and Google API access |

---

## GitHub Repository Best Practices

### Repository Visibility

**All repositories should be private by default.** Only make a repository public if:
- It's an open-source project intended for community use
- It contains no proprietary code, API keys, or business logic
- You've audited the entire git history for sensitive data

### Repository Naming Conventions

```bash
# Product repositories
product-name                # Main product codebase
product-name-api            # If API is separate from frontend
product-name-docs           # Public documentation (if open-source)

# Avoid
my-app                      # Too generic
test-project                # Non-descriptive
product-name-v2             # Use branches, not new repos for versions
```

### Repository Settings Checklist

For each new repository:

- [ ] Set visibility to **Private**
- [ ] Enable branch protection on `main`:
  - Require pull request reviews (optional for solo projects)
  - Require status checks to pass (if CI configured)
  - Do not allow force pushes
- [ ] Add `.gitignore` appropriate for your stack
- [ ] Configure repository secrets for CI/CD (never commit secrets)

### Sensitive Data Protection

```bash
# .gitignore - Include these patterns in every repo
.env
.env.*
!.env.example
*.pem
*.key
*.p12
credentials.json
service-account.json
.gcp-credentials.json
node_modules/
```

If you accidentally commit secrets:
1. Rotate the exposed credentials immediately
2. Use `git filter-branch` or BFG Repo-Cleaner to remove from history
3. Force push (only time this is acceptable)

---

## Authentication Decision

**At the start of every new project with user accounts, ask the user:**

> "What authentication method would you like to use?"
>
> **Option 1: Google OAuth Only**
> - Pros: Simpler implementation, no password management, no password reset flows, users trust Google's security
> - Cons: Users must have a Google account, some users prefer not to use OAuth
>
> **Option 2: Google OAuth + Email/Password**
> - Pros: More user choice, doesn't require Google account
> - Cons: More complex, need password hashing, reset flows, email verification

Default recommendation: **Google OAuth only** for B2B/professional tools, **Both options** for consumer-facing apps.

### Google OAuth Implementation
- Use **passport-google-oauth20** strategy
- Store user profile from Google (email, name, avatar)
- Create session after successful OAuth callback

---

## Google Cloud Platform Usage

**GCP is NOT for hosting.** Only use GCP for:

1. **Google OAuth** - Authentication via Google accounts
2. **Google APIs** - Drive, Sheets, Calendar, etc. when needed
3. **Service Accounts** - For server-to-server Google API access

All hosting, databases, and secrets go through **Railway**.

### Setting Up Google OAuth

1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Create a new project (or select existing)
3. Enable the Google+ API or People API
4. Go to Credentials > Create Credentials > OAuth Client ID
5. Configure OAuth consent screen
6. Add authorized redirect URIs:
   - Development: `http://localhost:3000/auth/google/callback`
   - Production: `https://your-app.railway.app/auth/google/callback`
7. Copy Client ID and Client Secret to Railway environment variables

### Google APIs (Drive, Sheets, Calendar, etc.)

Use GCP service accounts when your app needs server-to-server API access:

#### Creating a Service Account

1. Go to [Google Cloud Console](https://console.cloud.google.com)
2. Select your project
3. Go to IAM & Admin > Service Accounts
4. Click "Create Service Account"
5. Give it a descriptive name (e.g., "sheets-api-access")
6. Grant only the specific roles needed
7. Create and download a JSON key

#### Using Service Accounts in Code

```typescript
import { google } from 'googleapis';

// For server-to-server API access
const auth = new google.auth.GoogleAuth({
  keyFile: process.env.GOOGLE_APPLICATION_CREDENTIALS,
  scopes: ['https://www.googleapis.com/auth/spreadsheets.readonly'],
});

const sheets = google.sheets({ version: 'v4', auth });

// Read from a spreadsheet
const response = await sheets.spreadsheets.values.get({
  spreadsheetId: 'your-spreadsheet-id',
  range: 'Sheet1!A1:D10',
});
```

#### Environment Variables for Google APIs

```bash
# For server-to-server API access (service account)
GOOGLE_APPLICATION_CREDENTIALS=./service-account.json  # Local dev only

# In production, set the JSON content directly
GOOGLE_SERVICE_ACCOUNT_KEY={"type":"service_account",...}
```

**Never commit service account keys.** In production, use Railway environment variables to store the JSON content.

---

## Railway Configuration

### Standard railway.json

```json
{
  "$schema": "https://railway.app/railway.schema.json",
  "build": {
    "builder": "NIXPACKS",
    "buildCommand": "npm run build"
  },
  "deploy": {
    "startCommand": "npm start",
    "healthcheckPath": "/health",
    "restartPolicyType": "ON_FAILURE",
    "restartPolicyMaxRetries": 10
  }
}
```

### Railway Environment Variables

Set these in Railway dashboard (never commit):
- `DATABASE_URL` - Auto-provided by Railway PostgreSQL
- `SESSION_SECRET` - Generate secure random string
- `STRIPE_SECRET_KEY` - From Stripe dashboard
- `STRIPE_WEBHOOK_SECRET` - From Stripe webhook endpoint
- `RESEND_API_KEY` - From Resend dashboard
- `GOOGLE_CLIENT_ID` - From Google Cloud Console
- `GOOGLE_CLIENT_SECRET` - From Google Cloud Console

---

## Stripe Integration

### Webhook Strategy: Defense in Depth

Use **BOTH** separate endpoints AND metadata filtering:

1. **Separate webhook endpoints** - Each product gets its own endpoint URL in Stripe
2. **Metadata validation** - Always include and validate `product` identifier

### Setting Up Webhook Endpoints in Stripe Dashboard

1. Go to Stripe Dashboard → Developers → Webhooks
2. Click "Add endpoint"
3. Enter URL: `https://your-app.railway.app/api/webhooks/stripe`
4. Select only the events your app needs:
   - `checkout.session.completed`
   - `customer.subscription.created`
   - `customer.subscription.updated`
   - `customer.subscription.deleted`
   - `invoice.paid`
   - `invoice.payment_failed`
5. Click "Add endpoint"
6. **Copy the webhook signing secret (`whsec_...`)** — this is unique to this endpoint

### Complete Webhook Handler Example

```typescript
import Stripe from 'stripe';
import express from 'express';

const stripe = new Stripe(process.env.STRIPE_SECRET_KEY!);

// IMPORTANT: Use raw body for webhook signature verification
export const stripeWebhookRouter = express.Router();

stripeWebhookRouter.post(
  '/stripe',
  express.raw({ type: 'application/json' }),
  async (req, res) => {
    const sig = req.headers['stripe-signature'] as string;
    let event: Stripe.Event;

    try {
      event = stripe.webhooks.constructEvent(
        req.body,
        sig,
        process.env.STRIPE_WEBHOOK_SECRET!
      );
    } catch (err: any) {
      console.error('Webhook signature verification failed:', err.message);
      return res.status(400).send(`Webhook Error: ${err.message}`);
    }

    try {
      switch (event.type) {
        case 'checkout.session.completed': {
          const session = event.data.object as Stripe.Checkout.Session;

          // Defense in depth - validate product even with separate endpoints
          if (session.metadata?.product !== process.env.PRODUCT_ID) {
            console.log(`Ignoring event for product: ${session.metadata?.product}`);
            break;
          }

          await handleCheckoutCompleted(session);
          break;
        }

        case 'customer.subscription.created':
        case 'customer.subscription.updated': {
          const subscription = event.data.object as Stripe.Subscription;

          if (subscription.metadata?.product !== process.env.PRODUCT_ID) {
            console.log(`Ignoring subscription for product: ${subscription.metadata?.product}`);
            break;
          }

          await handleSubscriptionChange(subscription);
          break;
        }

        case 'customer.subscription.deleted': {
          const subscription = event.data.object as Stripe.Subscription;

          if (subscription.metadata?.product !== process.env.PRODUCT_ID) {
            break;
          }

          await handleSubscriptionCanceled(subscription);
          break;
        }

        case 'invoice.paid': {
          const invoice = event.data.object as Stripe.Invoice;

          if (invoice.subscription) {
            const subscription = await stripe.subscriptions.retrieve(
              invoice.subscription as string
            );
            if (subscription.metadata?.product !== process.env.PRODUCT_ID) {
              break;
            }
          }

          await handleInvoicePaid(invoice);
          break;
        }

        default:
          console.log(`Unhandled event type: ${event.type}`);
      }

      res.json({ received: true });
    } catch (err: any) {
      console.error('Error processing webhook:', err);
      res.status(500).json({ error: 'Webhook handler failed' });
    }
  }
);
```

### Creating Checkout Sessions with Proper Metadata

```typescript
export async function createCheckoutSession(
  userId: string,
  priceId: string,
  successUrl: string,
  cancelUrl: string
) {
  const session = await stripe.checkout.sessions.create({
    mode: 'subscription',
    payment_method_types: ['card'],
    line_items: [
      {
        price: priceId,
        quantity: 1,
      },
    ],
    success_url: successUrl,
    cancel_url: cancelUrl,
    metadata: {
      product: process.env.PRODUCT_ID!,
      userId: userId,
      environment: process.env.NODE_ENV!,
    },
    subscription_data: {
      metadata: {
        product: process.env.PRODUCT_ID!,
        userId: userId,
      },
    },
  });

  return session;
}
```

### Subscription Management Helpers

```typescript
export async function getActiveSubscription(customerId: string) {
  const subscriptions = await stripe.subscriptions.list({
    customer: customerId,
    status: 'active',
    limit: 1,
  });

  // Filter by product
  return subscriptions.data.find(
    sub => sub.metadata?.product === process.env.PRODUCT_ID
  );
}

export async function cancelSubscription(subscriptionId: string) {
  // Verify ownership before canceling
  const subscription = await stripe.subscriptions.retrieve(subscriptionId);

  if (subscription.metadata?.product !== process.env.PRODUCT_ID) {
    throw new Error('Subscription does not belong to this product');
  }

  return stripe.subscriptions.cancel(subscriptionId);
}
```

### Testing Webhooks Locally

```bash
# Install Stripe CLI
brew install stripe/stripe-cli/stripe

# Login to your Stripe account
stripe login

# Forward webhooks to local server
stripe listen --forward-to localhost:3000/api/webhooks/stripe

# The CLI will output a webhook signing secret (whsec_...)
# Use this in your local .env, NOT the production secret
```

```bash
# Trigger test events
stripe trigger checkout.session.completed
stripe trigger customer.subscription.created

# List available events
stripe trigger --help
```

### Stripe Environment Variable Naming

```bash
# Stripe keys
STRIPE_SECRET_KEY=sk_live_...
STRIPE_WEBHOOK_SECRET=whsec_...

# Price IDs (use consistent naming)
STRIPE_PRICE_ID_MONTHLY=price_...
STRIPE_PRICE_ID_ANNUAL=price_...

# For multi-tier pricing
STRIPE_PRICE_ID_BASIC_MONTHLY=price_...
STRIPE_PRICE_ID_BASIC_ANNUAL=price_...
STRIPE_PRICE_ID_PRO_MONTHLY=price_...
STRIPE_PRICE_ID_PRO_ANNUAL=price_...
```

---

## Resend Email Configuration

Always use Resend for transactional email:

```bash
RESEND_API_KEY=re_...
EMAIL_FROM=Product Name <noreply@yourdomain.com>
ADMIN_EMAIL=admin@yourdomain.com
```

### Basic Resend Setup

```typescript
import { Resend } from 'resend';

const resend = new Resend(process.env.RESEND_API_KEY);

export async function sendEmail({
  to,
  subject,
  html,
}: {
  to: string;
  subject: string;
  html: string;
}) {
  return resend.emails.send({
    from: process.env.EMAIL_FROM!,
    to,
    subject,
    html,
  });
}
```

### Email Provider Isolation

When using shared email providers across products, always verify context:

```typescript
export async function sendPurchaseEmail(data: PurchaseEmailData, productId: string) {
  // Validate this is the right product context
  if (productId !== process.env.PRODUCT_ID) {
    console.error(`Email send attempted for wrong product: ${productId}`);
    return;
  }

  await resend.emails.send({
    from: process.env.EMAIL_FROM!,
    to: data.email,
    subject: `Your ${process.env.PRODUCT_NAME} Purchase`,
    html: generateEmailHtml(data),
  });
}
```

---

## Environment Variables

### Naming Conventions

```bash
# Use prefixes to group related variables
STRIPE_SECRET_KEY=sk_live_...
STRIPE_WEBHOOK_SECRET=whsec_...
STRIPE_PRICE_ID_MONTHLY=price_...

# Boolean flags should be explicit
FEATURE_NEW_CHECKOUT=true
DEBUG_MODE=false
EMAIL_ENABLED=true

# Product-specific prefixes when sharing infrastructure
PRODUCT_ID=your-product-id
PRODUCT_NAME=Your Product Name
```

### .env.example (Standard Template)

```bash
# ===========================================
# Application
# ===========================================
NODE_ENV=development
PORT=3000
APP_URL=http://localhost:3000
CLIENT_URL=http://localhost:5173

# ===========================================
# Product Identification
# ===========================================
PRODUCT_ID=your-product-id
PRODUCT_NAME=Your Product Name

# ===========================================
# Database (Railway provides this)
# ===========================================
DATABASE_URL=postgresql://user:password@localhost:5432/dbname

# ===========================================
# Session
# ===========================================
SESSION_SECRET=generate-a-random-string-here

# ===========================================
# Google OAuth
# ===========================================
GOOGLE_CLIENT_ID=your-client-id.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=your-client-secret

# ===========================================
# Stripe
# ===========================================
STRIPE_SECRET_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...
STRIPE_PRICE_ID_MONTHLY=price_...
STRIPE_PRICE_ID_ANNUAL=price_...

# ===========================================
# Email (Resend)
# ===========================================
RESEND_API_KEY=re_...
EMAIL_FROM=Product Name <noreply@yourdomain.com>
ADMIN_EMAIL=admin@yourdomain.com

# ===========================================
# Feature Flags (optional)
# ===========================================
FEATURE_NEW_CHECKOUT=false
DEBUG_MODE=false
```

### Multi-Environment Setup

```bash
# Development (.env.local)
NODE_ENV=development
APP_URL=http://localhost:3000
CLIENT_URL=http://localhost:5173
STRIPE_SECRET_KEY=sk_test_...

# Staging (Railway staging environment)
NODE_ENV=staging
APP_URL=https://staging.yourapp.com
CLIENT_URL=https://staging.yourapp.com
STRIPE_SECRET_KEY=sk_test_...  # Still use test keys

# Production (Railway production environment)
NODE_ENV=production
APP_URL=https://yourapp.com
CLIENT_URL=https://yourapp.com
STRIPE_SECRET_KEY=sk_live_...  # Live keys only in production
```

### Environment Validation

Always validate required environment variables at startup:

```typescript
// server/src/lib/env.ts
const requiredVars = [
  'DATABASE_URL',
  'SESSION_SECRET',
  'PRODUCT_ID',
  'GOOGLE_CLIENT_ID',
  'GOOGLE_CLIENT_SECRET',
] as const;

const optionalVars = {
  NODE_ENV: 'development',
  PORT: '3000',
} as const;

export function validateEnv() {
  const missing = requiredVars.filter(key => !process.env[key]);

  if (missing.length > 0) {
    throw new Error(`Missing required environment variables: ${missing.join(', ')}`);
  }

  // Set defaults for optional vars
  for (const [key, defaultValue] of Object.entries(optionalVars)) {
    if (!process.env[key]) {
      process.env[key] = defaultValue;
    }
  }
}

// Call at app startup
validateEnv();
```

### Typed Environment Variables

```typescript
// types/env.d.ts
declare global {
  namespace NodeJS {
    interface ProcessEnv {
      NODE_ENV: 'development' | 'production' | 'test';
      PORT: string;
      DATABASE_URL: string;
      SESSION_SECRET: string;
      PRODUCT_ID: string;
      STRIPE_SECRET_KEY: string;
      STRIPE_WEBHOOK_SECRET: string;
      RESEND_API_KEY: string;
      EMAIL_FROM: string;
      APP_URL: string;
      CLIENT_URL: string;
      GOOGLE_CLIENT_ID: string;
      GOOGLE_CLIENT_SECRET: string;
    }
  }
}

export {};
```

### Feature Flags Pattern

```typescript
// server/src/lib/features.ts
export const features = {
  newCheckout: process.env.FEATURE_NEW_CHECKOUT === 'true',
  betaFeatures: process.env.FEATURE_BETA === 'true',
  debugMode: process.env.DEBUG_MODE === 'true',
} as const;

// Usage
if (features.newCheckout) {
  // Use new checkout flow
}
```

### Rate Limiting Configuration

```typescript
import rateLimit from 'express-rate-limit';

// General API rate limit
export const apiLimiter = rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: parseInt(process.env.RATE_LIMIT_MAX || '100'),
  message: { error: 'Too many requests, please try again later' },
});

// Strict limit for auth endpoints
export const authLimiter = rateLimit({
  windowMs: 60 * 60 * 1000, // 1 hour
  max: 5,
  message: { error: 'Too many login attempts, please try again later' },
});

// Apply to routes
app.use('/api/', apiLimiter);
app.use('/auth/', authLimiter);
```

### Security Rules

```bash
# NEVER commit these patterns - add to .gitignore
.env
.env.local
.env.*.local
*.pem
*.key
credentials.json

# NEVER use production keys in development
STRIPE_SECRET_KEY=sk_test_...  # Development
STRIPE_SECRET_KEY=sk_live_...  # Production (set in Railway)

# NEVER log sensitive variables
console.log(process.env)  # BAD - leaks secrets
console.log({ port: process.env.PORT, env: process.env.NODE_ENV })  # GOOD
```

---

## Prisma Configuration

### Standard Schema Header

```prisma
generator client {
  provider = "prisma-client-js"
}

datasource db {
  provider = "postgresql"
  url      = env("DATABASE_URL")
}
```

### Session Model (for @quixo3/prisma-session-store)

```prisma
model Session {
  id        String   @id
  sid       String   @unique
  data      String
  expiresAt DateTime
}
```

### User Model (Standard)

```prisma
model User {
  id            String    @id @default(uuid())
  email         String    @unique
  name          String?
  avatar        String?
  googleId      String?   @unique
  createdAt     DateTime  @default(now())
  updatedAt     DateTime  @updatedAt

  // Add relationships as needed
}
```

### Database Isolation (Multi-Product)

If multiple products share a database, include product identifier:

```prisma
model License {
  id          String   @id @default(uuid())
  productId   String   // 'tasktracker-pro', 'my-other-app'
  licenseKey  String   @unique
  // ...

  @@index([productId])
}
```

```typescript
// Always filter by product
const licenses = await prisma.license.findMany({
  where: { productId: process.env.PRODUCT_ID }
});
```

---

## Preventing Product Crosstalk

When running multiple products on shared infrastructure, follow these practices:

### Stripe Webhook Isolation

| Layer | Protection |
|-------|------------|
| Separate endpoints | Each product only receives its own events |
| Metadata filtering | Catches any misconfiguration; prevents crosstalk |

### Email Isolation
- Use product-specific `EMAIL_FROM` addresses
- Validate `productId` before sending any email

### Database Isolation
- Include `productId` field in shared tables
- Always filter queries by `productId`

---

## Styling

**Styles are maintained in separate documents** so these development guidelines can be shared independently.

At project start, determine the theme preference:
- **Dark theme only** → Reference `dark-theme.md`
- **Light theme only** → Reference `light-theme.md`
- **Both themes (toggle)** → Reference both files

The style documents contain:
- CSS variables and design tokens
- Color palettes
- Typography (fonts, sizes, weights)
- Component patterns (cards, buttons, badges, etc.)
- Tailwind configuration

---

## Things to Avoid

### Never Use
- **Supabase** - Stick with Prisma + Railway PostgreSQL
- **GCP for hosting** - Use Railway instead
- **GCP Secret Manager** - Use Railway environment variables
- **connect-pg-simple** - Use @quixo3/prisma-session-store instead
- **Create React App** - Use Vite

### Don't Commit
- `.env` files (only `.env.example`)
- `*.pem`, `*.key` files
- `credentials.json`, `service-account.json`
- `node_modules/`

---

## Quick Start Checklist for New Projects

### Initial Setup
- [ ] Create monorepo structure (client/, server/, root package.json)
- [ ] Set up Vite + React + TypeScript + Tailwind in client/
- [ ] Set up Express + TypeScript + Prisma in server/
- [ ] Add shadcn/ui and configure with style guide colors
- [ ] Create `.gitignore` with all sensitive patterns
- [ ] Create GitHub repo (private by default)

### Authentication
- [ ] **Ask user about authentication preference** (OAuth only vs OAuth + email/password)
- [ ] Set up Google OAuth in GCP Console
- [ ] Implement Passport.js with Google strategy
- [ ] Add Prisma session store (@quixo3/prisma-session-store)

### Infrastructure
- [ ] Create railway.json deployment config
- [ ] Provision Railway PostgreSQL database
- [ ] Set up environment variables in Railway

### Payments & Email
- [ ] Set up Stripe with separate webhook endpoint
- [ ] Add `PRODUCT_ID` to environment and Stripe metadata
- [ ] Configure Resend for transactional email

### Security & Validation
- [ ] Add security middleware (helmet, cors, rate-limit, csrf)
- [ ] Create .env.example with all required variables
- [ ] Add typed environment declarations
- [ ] Validate environment variables at startup

### Styling
- [ ] **Ask user about theme preference** (dark, light, or both)
- [ ] Reference the appropriate style guide(s): `dark-theme.md`, `light-theme.md`
- [ ] Apply design tokens and component patterns from the style guides

---

## Quick Reference

### Stripe Event Types to Subscribe To

| Event | When to Use |
|-------|-------------|
| `checkout.session.completed` | One-time purchases, subscription start |
| `customer.subscription.created` | New subscription created |
| `customer.subscription.updated` | Plan change, payment method update |
| `customer.subscription.deleted` | Subscription canceled |
| `invoice.paid` | Recurring payment successful |
| `invoice.payment_failed` | Payment failed (send reminder) |

### Common Debugging Commands

```bash
# Check Railway logs
railway logs

# Check Stripe webhook events
stripe events list --limit 10

# Resend a webhook event
stripe events resend evt_xxxxx

# Test Stripe connection
stripe config --list
```

---

## Version History

| Date | Change |
|------|--------|
| _Initial_ | Starter kit version |
