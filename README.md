# NexaAI — Premium AI SaaS Platform

<p align="center">
  <img src="https://img.shields.io/badge/Next.js-14-black?style=for-the-badge&logo=next.js" />
  <img src="https://img.shields.io/badge/TypeScript-5.6-blue?style=for-the-badge&logo=typescript" />
  <img src="https://img.shields.io/badge/Tailwind_CSS-3.4-38BDF8?style=for-the-badge&logo=tailwindcss" />
  <img src="https://img.shields.io/badge/Supabase-PostgreSQL-1DB48D?style=for-the-badge&logo=supabase" />
  <img src="https://img.shields.io/badge/Stripe-Payments-635BFF?style=for-the-badge&logo=stripe" />
</p>

A complete, production-grade AI SaaS platform with generative AI tools, Stripe subscriptions, Supabase auth/database, and a secure admin panel.

---

## ✅ What's Implemented

| Area | Status | Notes |
|---|---|---|
| Landing Page | ✅ Complete | Hero, Benefits, Tools Showcase, How It Works, Pricing, FAQ, Footer |
| Auth Pages | ✅ Complete | Login, Signup w/ email confirmation, Forgot password (add reset page similarly) |
| Dashboard Layout | ✅ Complete | Collapsible sidebar, mobile menu, plan badge, role-based nav |
| AI Writer | ✅ Complete | Connected to real server-side AI API, quota enforced |
| AI Chat | ✅ Complete | Pro-only tool with server guard |
| Text Summarizer | ✅ Complete | Executive brief output format |
| Idea Engine | ✅ Complete | Structured idea output with named concepts |
| Content Rewriter | ✅ Complete | Tone-shifting with multiple modes |
| History Page | ✅ Complete | Searchable, filterable, deletable, expandable records |
| Billing Page | ✅ Complete | Plan comparison, Stripe checkout, Stripe portal link |
| Settings Page | ✅ Complete | Profile editor, account deletion |
| Admin Dashboard | ✅ Complete | Role-protected, user table, contact inbox, status management |
| Contact Form | ✅ Complete | Validated, stored in DB, spam-safe |
| Privacy / Terms | ✅ Complete | Draft templates included (customize before launch) |
| Supabase Schema | ✅ Complete | Full migration SQL with RLS, triggers, indexes |
| Stripe Integration | ✅ Complete | Checkout, webhooks, portal, idempotency guard |
| AI Provider Layer | ✅ Complete | OpenAI, Anthropic, Gemini — switchable via env var |
| Quota System | ✅ Complete | Server-side check, atomic usage recording |
| Route Protection | ✅ Complete | Middleware + server-side permission checks |
| Audit Logging | ✅ Complete | Admin actions logged to `admin_audit_logs` table |
| SEO | ✅ Complete | Sitemap, robots.txt, Open Graph, page metadata |

---

## 🚀 Quick Start

### Prerequisites
- **Node.js 18+** — [Install](https://nodejs.org)
- **Supabase Account** — [supabase.com](https://supabase.com) (free)
- **Stripe Account** — [stripe.com](https://stripe.com) (free test mode)
- **OpenAI/Anthropic/Gemini API Key** — for AI generation

### 1. Install Dependencies

```bash
cd nexa-ai
npm install
```

### 2. Configure Environment Variables

```bash
cp .env.example .env.local
```

Edit `.env.local` and fill in the required values:

```env
# Supabase
NEXT_PUBLIC_SUPABASE_URL=https://your-project.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=eyJhbGci...
SUPABASE_SERVICE_ROLE_KEY=eyJhbGci...

# AI Provider (choose one)
AI_PROVIDER=openai
OPENAI_API_KEY=sk-proj-...

# Stripe (test mode)
NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY=pk_test_...
STRIPE_SECRET_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...
NEXT_PUBLIC_STRIPE_PRO_MONTHLY_PRICE_ID=price_...
NEXT_PUBLIC_STRIPE_PRO_ANNUAL_PRICE_ID=price_...
NEXT_PUBLIC_STRIPE_BUSINESS_MONTHLY_PRICE_ID=price_...
NEXT_PUBLIC_STRIPE_BUSINESS_ANNUAL_PRICE_ID=price_...
```

### 3. Initialize Supabase Database

1. Open your Supabase dashboard → **SQL Editor**
2. Paste and run: `supabase/migrations/20261021000001_init_schema.sql`
3. Then run: `supabase/seed.sql` (to insert plan data)

### 4. Set Up Stripe Products

In your Stripe Dashboard (Test Mode):
1. Create Product: "NexaAI Pro Creator" → add Monthly ($19/mo) and Annual ($180/yr) prices
2. Create Product: "NexaAI Business Suite" → add Monthly ($49/mo) and Annual ($468/yr) prices
3. Copy price IDs into `.env.local`

### 5. Configure Stripe Webhooks

```bash
# Install Stripe CLI
stripe listen --forward-to localhost:3000/api/stripe/webhook
```

Copy the webhook signing secret into `STRIPE_WEBHOOK_SECRET`.

For production, add a webhook endpoint in your Stripe Dashboard pointing to:
`https://your-domain.com/api/stripe/webhook`

Required events:
- `checkout.session.completed`
- `customer.subscription.updated`
- `customer.subscription.deleted`
- `invoice.payment_failed`

### 6. Run Development Server

```bash
npm run dev
```

Visit `http://localhost:3000` — your NexaAI platform is running!

---

## 📁 Project Structure

```
nexa-ai/
├── app/
│   ├── (auth)/             # Login, Signup, Forgot Password pages
│   ├── (dashboard)/        # Dashboard, Tools, History, Billing, Settings, Admin
│   ├── (marketing)/        # Landing, Pricing, Contact, Privacy, Terms
│   ├── api/
│   │   ├── ai/generate/    # Server-side AI generation endpoint
│   │   ├── contact/        # Contact form submission
│   │   ├── stripe/
│   │   │   ├── checkout/   # Create Stripe checkout session
│   │   │   ├── portal/     # Open Stripe billing portal
│   │   │   └── webhook/    # Verified Stripe webhook handler
│   │   └── admin/          # Admin user and contact management APIs
│   ├── sitemap.ts
│   ├── robots.ts
│   └── globals.css
├── components/
│   ├── dashboard/          # Tool workspace, dashboard widgets
│   ├── marketing/          # Navbar, Hero, Pricing, FAQ, Footer, etc.
│   └── ui/                 # Button, Card, Badge, Input (reusable design system)
├── lib/
│   ├── ai/
│   │   ├── prompts.ts      # Tool definitions and system prompts
│   │   ├── provider.ts     # OpenAI / Anthropic / Gemini abstraction layer
│   │   └── quota.ts        # Server-side quota check and usage recording
│   ├── auth/
│   │   └── permissions.ts  # getAuthSession, requireAuth, requireAdmin
│   ├── stripe/
│   │   ├── client.ts       # Stripe client factory
│   │   └── plans.ts        # Plan configuration and pricing
│   └── supabase/
│       ├── client.ts       # Browser Supabase client
│       ├── server.ts       # Server Supabase client with cookie handling
│       └── admin.ts        # Service-role admin client (server-only)
├── supabase/
│   ├── migrations/         # Full schema SQL with RLS policies
│   └── seed.sql            # Default plan data
├── middleware.ts            # Route protection (auth-gated /dashboard and /admin)
├── .env.example
├── next.config.js
├── tailwind.config.ts
└── tsconfig.json
```

---

## 🔐 Security Architecture

| Layer | Implementation |
|---|---|
| Authentication | Supabase JWT tokens via server cookies |
| Route Protection | Next.js middleware + server-side session check |
| Data Isolation | PostgreSQL Row Level Security (RLS) on all tables |
| Admin Authorization | `role = 'admin'` in profiles table, checked server-side |
| Webhook Verification | Stripe signature verification with `STRIPE_WEBHOOK_SECRET` |
| Quota Enforcement | Server-side only — never trusts client-reported status |
| Secrets Management | All API keys in server environment variables only |
| Audit Logging | All admin operations logged to `admin_audit_logs` |
| Idempotency | `webhook_events` table with unique `provider_event_id` constraint |

---

## 🌍 Deployment on Vercel

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel --prod
```

In Vercel Dashboard:
1. Add all environment variables from `.env.local`
2. Set `NEXT_PUBLIC_APP_URL` to your production domain
3. Update Stripe webhook URL to `https://your-domain.vercel.app/api/stripe/webhook`

---

## 🔧 Required External Accounts Checklist

- [ ] Supabase project created and SQL schema applied
- [ ] `NEXT_PUBLIC_SUPABASE_URL` and keys configured
- [ ] OpenAI API key added (`OPENAI_API_KEY`)
- [ ] Stripe account active, test mode enabled
- [ ] Stripe products and prices created
- [ ] Stripe price IDs added to environment
- [ ] Stripe webhook configured and `STRIPE_WEBHOOK_SECRET` set
- [ ] `.env.local` created from `.env.example` with all real values
- [ ] Legal pages reviewed by qualified counsel before launch

---

## ⚙️ Switching AI Provider

In `.env.local`:
```env
# Options: openai | anthropic | gemini
AI_PROVIDER=anthropic
ANTHROPIC_API_KEY=sk-ant-...
AI_MODEL_NAME=claude-3-5-sonnet-20241022
```

No code changes required — the provider abstraction in `lib/ai/provider.ts` handles it.
