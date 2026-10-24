# 🛡️ PolicyGuard - Cloud-Synchronized Insurance Portal

PolicyGuard is a mobile-responsive, cloud-synchronized web application designed for insurance agents and administrators. It streamlines client policy tracking, automates renewal reminders, logs payout/commission breakdowns, and enables real-time synchronization across desktop and mobile devices.

---

## ✨ Features

- **⏱️ Expiring Policies Radar:** Auto-filters policies expiring within custom windows (2, 5, 7, 15, or 30 days).
- **🚗 Motor-Specific Tracking:** Dedicated support for Motor Insurance categories (Bike, Car, Bus, Van, Others) with Vehicle Number and IDV (Insured Declared Value).
- **💰 Commission & Payout Management:** Track Customer Payout Discounts and Net Agent Commissions per policy.
- **📜 Policy History & Renewal Log:** Full audit log of renewals with past durations, start/expiry dates, and year-by-year commission history.
- **💬 Direct Outreach:** One-click direct calling (`tel:`) and auto-formatted WhatsApp renewal messages pre-filled with client, vehicle, and policy details.
- **📥 / 📤 Bulk Data Operations:** Import client spreadsheets via Excel (`.xlsx`/`.xls`) and export selected vault records to CSV.
- **🔴 Active/Inactive Status Toggle:** Disable lapsed policies or reactivate client records effortlessly.
- **⚡ Real-Time Cloud Sync:** Built on Supabase (PostgreSQL) for multi-device data updates without manual sync delays.

---

## 🛠️ Tech Stack

- **Frontend:** HTML5, Tailwind CSS (CDN), Vanilla JavaScript (ES6+)
- **Database:** Supabase (PostgreSQL) with Row-Level Security (RLS)
- **Libraries:**
  - `@supabase/supabase-js` (Database API)
  - `xlsx` (Excel parsing & export)
  - Inter Font (Google Fonts)
- **Deployment:** Vercel

---

## 🗄️ Database Setup (Supabase)

Execute the following SQL in your **Supabase SQL Editor** to establish the primary table schema:

```sql
-- Create Policies Table
CREATE TABLE policies (
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

-- Enable Row Level Security (RLS) & Policies
ALTER TABLE policies ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Allow public read" ON policies FOR SELECT USING (true);
CREATE POLICY "Allow public insert" ON policies FOR INSERT WITH CHECK (true);
CREATE POLICY "Allow public update" ON policies FOR UPDATE USING (true);
CREATE POLICY "Allow public delete" ON policies FOR DELETE USING (true);