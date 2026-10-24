# 🛡️ PolicyGuard - Cloud-Synchronized Insurance Management Portal

PolicyGuard is a modern, mobile-responsive, and cloud-synchronized web portal designed for insurance agents and administrative teams. It simplifies policy tracking, automates renewal management, logs commissions/payout breakdowns, and seamlessly synchronizes records in real-time across all mobile and desktop devices.

---

## ✨ Key Features

- **⏱️ Expiring Policies Radar:** Real-time tracking of policies expiring within customizable windows (2, 5, 7, 15, or 30 days).
- **📅 Policy Start & Expiry Dates:** Complete duration management tracking both Policy Start Date and Expiry Date across all forms, tables, and renewal logs.
- **🚗 Motor Insurance Specialty:** Native support for vehicle categories (Bike, Car, Bus, Van, Others), Vehicle Numbers, and IDV (Insured Declared Value).
- **💰 Commission & Payout Tracker:** Clear tracking of Customer Payout Discounts and Net Agent Commissions per policy.
- **📜 Renewal Log & History Timeline:** Comprehensive history audit modal showing previous policy terms, start/expiry dates, and year-by-year commission logs.
- **🎨 Dynamic Tab Highlighting:** Active sidebar tabs dynamically highlight in blue while inactive tabs automatically dim.
- **🔄 Reset Form Option:** One-click form reset capability when adding new customer records.
- **💬 Direct Contact Integration:** Native one-click phone dialing (`tel:`) and pre-filled, formatted WhatsApp messages containing policy and vehicle details.
- **📥 / 📤 Bulk Excel Operations:** Bulk customer import from Excel/CSV files (`.xlsx`, `.xls`, `.csv`) and CSV export for selected records.
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

Execute the following SQL script inside your **Supabase SQL Editor** to establish the `policies` table and set up Row Level Security (RLS):

```sql
-- Create Policies Table with Start/Expiry Dates, Motor details, and History JSON
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

-- Enable Row Level Security (RLS) & Public CRUD Policies
ALTER TABLE policies ENABLE ROW LEVEL SECURITY;

DROP POLICY IF EXISTS "Allow public read" ON policies;
DROP POLICY IF EXISTS "Allow public insert" ON policies;
DROP POLICY IF EXISTS "Allow public update" ON policies;
DROP POLICY IF EXISTS "Allow public delete" ON policies;

CREATE POLICY "Allow public read" ON policies FOR SELECT USING (true);
CREATE POLICY "Allow public insert" ON policies FOR INSERT WITH CHECK (true);
CREATE POLICY "Allow public update" ON policies FOR UPDATE USING (true);
CREATE POLICY "Allow public delete" ON policies FOR DELETE USING (true);