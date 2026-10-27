# 🛡️ PolicyGuard Hub

**PolicyGuard Hub** is a centralized platform designed to manage, audit, and enforce policy compliance, security guardrails, and automated reminder workflows across your operations. It features real-time alerting, automated policy checks, seamless WhatsApp communication template integration, and persistent cloud synchronization powered by **Supabase**.

---

## 📋 Table of Contents

- [Features](#-features)
- [Architecture & Tech Stack](#-architecture--tech-stack)
- [Prerequisites](#-prerequisites)
- [Database Setup (Supabase SQL)](#-database-setup-supabase-sql)
- [Environment Configuration](#-environment-configuration)
- [Getting Started](#-getting-started)
- [Usage & Features](#-usage--features)
  - [Policy Evaluation & Reminders](#policy-evaluation--reminders)
  - [WhatsApp Template Hub](#whatsapp-template-hub)
- [Database Migration & Maintenance](#-database-migration--maintenance)
- [Contributing](#-contributing)
- [License](#-license)

---

## 🚀 Features

* **Centralized Policy Management:** Store, edit, and organize all organizational policies, security rules, and compliance records in one place.
* **Automated Auditing & Reminders:** Keep track of policy review schedules, track execution dates, and log manual or automated reminders.
* **WhatsApp Template Hub:** Create, customize, and save messaging templates with dynamic placeholders to dispatch policy alerts directly to stakeholders.
* **Hybrid Storage Engine:** Features automatic cloud sync via Supabase, with a seamless **Browser LocalStorage fallback** to ensure zero downtime when offline.
* **Role-Based Access Control (RBAC):** Restrict policy modifications and configuration access based on agent permissions.
* **Audit Logs:** Full visibility into policy updates, reminder dispatches, and user interactions.

---

## 🛠️ Architecture & Tech Stack

* **Frontend:** React / Next.js, Tailwind CSS
* **Database & Auth:** Supabase (PostgreSQL with RLS)
* **Icons & UI Components:** Lucide Icons / Radix UI
* **Deployment:** Vercel / Netlify / Docker

```
 ┌──────────────────────┐      ┌──────────────────────────┐      ┌───────────────────────┐
 │   PolicyGuard Client │ ───> │ Supabase Cloud Database  │ <─── │ WhatsApp Messaging    │
 │ (React / LocalStorage)│      │  (PostgreSQL + RLS)      │      │ (Template Dispatch)   │
 └──────────────────────┘      └──────────────────────────┘      └───────────────────────┘
```

---

## 📋 Prerequisites

Ensure you have the following installed on your local machine:

* **Node.js**: `v18.x` or higher
* **npm** or **yarn** / **pnpm**
* **Supabase Account**: A free or paid account at [supabase.com](https://supabase.com)

---

## 🗄️ Database Setup (Supabase SQL)

Before running the application, set up your PostgreSQL tables in Supabase. Open your **Supabase Dashboard** $\rightarrow$ **SQL Editor**, paste the script below, and click **Run**.

```sql
-- 1. Create Master Policies Table
create table if not exists public.policies (
    id uuid default gen_random_uuid() primary key,
    title text not null,
    description text,
    category text default 'General',
    status text default 'Active',
    severity text default 'MEDIUM',
    rule_definition jsonb default '{}'::jsonb,
    reminded_date text,
    created_at timestamp with time zone default timezone('utc'::text, now()),
    updated_at timestamp with time zone default timezone('utc'::text, now())
);

-- 2. Create WhatsApp Templates Table
create table if not exists public.whatsapp_templates (
    id text primary key,
    agent_email text not null,
    title text not null,
    body text not null,
    created_at timestamp with time zone default timezone('utc'::text, now())
);

-- 3. Create Audit Logs Table
create table if not exists public.policy_audit_logs (
    id uuid default gen_random_uuid() primary key,
    policy_id uuid references public.policies(id) on delete cascade,
    action text not null,
    performed_by text not null,
    details jsonb default '{}'::jsonb,
    timestamp timestamp with time zone default timezone('utc'::text, now())
);

-- 4. Enable Row Level Security (RLS)
alter table public.policies enable row level security;
alter table public.whatsapp_templates enable row level security;
alter table public.policy_audit_logs enable row level security;

-- 5. Create Permissive Access Policies (Adjust as needed for your Auth rules)
create policy "Allow public read/write on policies" on public.policies for all using (true) with check (true);
create policy "Allow public read/write on whatsapp_templates" on public.whatsapp_templates for all using (true) with check (true);
create policy "Allow public read/write on policy_audit_logs" on public.policy_audit_logs for all using (true) with check (true);
```

---

## ⚙️ Environment Configuration

Create a `.env.local` file in the root directory of your project:

```bash
# Supabase Configuration
NEXT_PUBLIC_SUPABASE_URL=https://your-project-ref.supabase.co
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-supabase-anon-key

# App Configuration
NEXT_PUBLIC_APP_ENV=development
```

---

## 📦 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/your-org/policyguard-hub.git
cd policyguard-hub
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Run Development Server

```bash
npm run dev
```

Open [http://localhost:3000](http://localhost:3000) in your browser to view the application.

### 4. Build for Production

```bash
npm run build
npm start
```

---

## 📖 Usage & Features

### Policy Evaluation & Reminders

1. Navigate to the **Policies Dashboard**.
2. Select any active policy to view its current status and compliance logs.
3. Click **"Mark Reminded"** on a policy card to update the `reminded_date`. This action automatically logs an entry in the audit trail and updates Supabase.

### WhatsApp Template Hub

1. Go to **WhatsApp Templates** from the sidebar menu.
2. Draft templates using placeholders like `{agent_name}`, `{policy_title}`, or `{due_date}`.
3. Save templates to sync them to your cloud account across devices.
4. Click **Send via WhatsApp** to quickly trigger pre-filled WhatsApp messages to policy owners.

---

## 🔄 Database Migration & Maintenance

When adding new features or updating policy schema in future releases:

1. **Adding New Columns:** Execute `ALTER TABLE public.policies ADD COLUMN if not exists column_name type;` in Supabase.
2. **Schema Rollback:** Always back up your table definitions from **Supabase $\rightarrow$ Database $\rightarrow$ Schema Visualizer** before making destructive schema modifications.

---

## 🤝 Contributing

We welcome contributions to PolicyGuard Hub!

1. Fork the project repository.
2. Create your Feature Branch (`git checkout -b feature/NewPolicyFeature`).
3. Commit your changes (`git commit -m 'Add new policy feature'`).
4. Push to the branch (`git push origin feature/NewPolicyFeature`).
5. Open a Pull Request.

---

## 📄 License

Distributed under the MIT License. See `LICENSE` for more information.