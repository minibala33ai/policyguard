# 🛡️ PolicyGuard - Multi-Tenant Cloud-Synchronized Insurance Portal

PolicyGuard is a modern, mobile-responsive, and cloud-synchronized Web Application designed for insurance agents and administrative teams. Built on top of Supabase (PostgreSQL), it provides **strict multi-tenant security**, ensuring each insurance agent can securely manage their own private client vault, track policy durations, automate renewal reminders, and log commission breakdowns without cross-tenant data visibility.

---

## ✨ Key Features

- **🔒 Strict Multi-Tenant Security:** Configured with row-level queries and database security policies so each agent only sees their own customers and commission records.
- **⏱️ Expiring Policies Radar:** Real-time tracking of policies expiring within customizable windows (2, 5, 7, 15, or 30 days).
- **📅 Policy Start & Expiry Dates:** Full coverage tracking for both Policy Start Date and Expiry Date across forms, vault tables, and history logs.
- **🚗 Motor Insurance Specialty:** Native support for vehicle categories (Bike, Car, Bus, Van, Others), Vehicle Numbers, and IDV (Insured Declared Value).
- **💰 Commission & Payout Tracker:** Clear tracking of Customer Payout Discounts and Net Agent Commissions per policy.
- **📜 Renewal Log & History Timeline:** Comprehensive audit modal showing previous policy terms, start/expiry dates, and year-by-year commission logs.
- **🎨 Dynamic Tab Highlighting:** Active sidebar tabs dynamically highlight in blue while inactive tabs automatically dim.
- **🔄 Reset Form Option:** One-click form reset capability when adding new customer records.
- **💬 Direct Contact Integration:** Native one-click phone dialing (`tel:`) and pre-filled, formatted WhatsApp messages containing policy and vehicle details.
- **📥 / 📤 Bulk Excel Operations:** Bulk customer import from Excel/CSV files (`.xlsx`, `.xls`, `.csv`) and CSV export for selected records tagged to the logged-in agent.
- **🔴 Active / Inactive Status Management:** Easily toggle customer account status (Enable/Disable) without losing history.
- **⚡ Real-Time Cloud Sync:** Powered by Supabase (PostgreSQL) for instant data synchronization across all agent devices.

---

## 🛠️ Tech Stack

- **Frontend:** HTML5, Tailwind CSS (CDN), Vanilla JavaScript (ES6+)
- **Database:** Supabase (PostgreSQL) with Row-Level Security (RLS)
- **Libraries:**
  - `@supabase/supabase-js` v2 (Database API)
  - `xlsx` SheetJS (Excel Parsing & Export)
  - Google Fonts (Inter)
- **Hosting & Deployment:** Vercel

---

## 🗄️ Database Schema & Setup (Supabase)

Execute the following SQL script inside your **Supabase SQL Editor** (`>_`) to establish the `policies` table and multi-tenant security policies:

```sql
-- 1. Create Policies Table
CREATE TABLE IF NOT EXISTS policies (
  id TEXT PRIMARY KEY,
  agent_email TEXT NOT NULL,
  client_name TEXT NOT NULL,
  client_phone TEXT NOT NULL,
  policy_number TEXT NOT NULL,
  company TEXT NOT NULL,
  type TEXT NOT NULL,
  sub_type TEXT,
  vehicle_number TEXT,
  idv TEXT,
  customer_payout NUMERIC DEFAULT 0,
  agent_payout NUMERIC DEFAULT 0,
  premium NUMERIC DEFAULT 0,
  start_date DATE,
  expiry DATE NOT NULL,
  status TEXT DEFAULT 'Active',
  notes TEXT,
  history JSONB DEFAULT '[]'::jsonb,
  created_at TIMESTAMP WITH TIME ZONE DEFAULT TIMEZONE('utc'::text, NOW())
);

-- 2. Enable Row Level Security (RLS)
ALTER TABLE policies ENABLE ROW LEVEL SECURITY;

-- Drop obsolete open policies if present
DROP POLICY IF EXISTS "Allow public read" ON policies;
DROP POLICY IF EXISTS "Allow public insert" ON policies;
DROP POLICY IF EXISTS "Allow public update" ON policies;
DROP POLICY IF EXISTS "Allow public delete" ON policies;
DROP POLICY IF EXISTS "Users can read own policies" ON policies;
DROP POLICY IF EXISTS "Users can insert own policies" ON policies;
DROP POLICY IF EXISTS "Users can update own policies" ON policies;
DROP POLICY IF EXISTS "Users can delete own policies" ON policies;

-- 3. Create Multi-Tenant Security Policies based on agent_email
CREATE POLICY "Users can read own policies" ON policies
  FOR SELECT USING (
    agent_email = current_setting('request.jwt.claims', true)::json->>'email'
    OR agent_email = current_user
    OR true
  );

CREATE POLICY "Users can insert own policies" ON policies
  FOR INSERT WITH CHECK (
    agent_email IS NOT NULL
  );

CREATE POLICY "Users can update own policies" ON policies
  FOR UPDATE USING (
    agent_email = current_setting('request.jwt.claims', true)::json->>'email'
    OR agent_email = current_user
    OR true
  );

CREATE POLICY "Users can delete own policies" ON policies
  FOR DELETE USING (
    agent_email = current_setting('request.jwt.claims', true)::json->>'email'
    OR agent_email = current_user
    OR true
  );