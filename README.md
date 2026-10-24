# PolicyGuard — Smart Policy & Renewal Management Hub

PolicyGuard is a comprehensive web-based application designed for insurance agents, sub-admins, and main administrators to manage policies, track customer renewals, coordinate payouts, and control user access across teams and groups.

---

## 🌟 Key Features

* **Dynamic Expiry Radar:** Instantly view policies expiring within custom date ranges (2, 5, 7, 15, or 30 days) with quick call and WhatsApp reminder integrations.
* **Customer Vault & Import/Export:** Maintain complete customer policy histories, upload policies via Excel (`.xlsx`), and export selected records.
* **Multi-Tier Role & Group Access:** 
  * **Main Admin (`minibala33ai@gmail.com`):** Complete access to the Admin Console, custom group creation, user status management, and subscription key generation.
  * **Sub-Admins & Agents:** Tailored access restricted to their assigned group and policies.
* **Dynamic Group Management:** Main Admins can create custom group categories directly from the UI on the fly.
* **Audit Logs:** Full snapshot logging for renewals and policy modifications.
* **Supabase Cloud Sync:** Seamless real-time data persistence with Supabase.

---

## 🗄️ Database Setup (Supabase)

To run PolicyGuard with cloud support, execute the following SQL script in your **Supabase SQL Editor**:

```sql
-- 1. Create a table to store custom groups created by the Main Admin
create table if not exists public.groups (
    id serial primary key,
    name text unique not null,
    created_at timestamp with time zone default timezone('utc'::text, now())
);

-- Insert default groups
insert into public.groups (name) values ('Group A'), ('Group B'), ('Group C'), ('Group E')
on conflict (name) do nothing;

-- 2. Ensure your users table supports the group_name column
alter table public.users add column if not exists group_name text default 'Group A';

-- 3. Policies table schema reference
create table if not exists public.policies (
    id text primary key,
    agent_email text not null,
    client_name text not null,
    client_phone text not null,
    policy_number text not null,
    company text not null,
    type text not null,
    sub_type text,
    vehicle_number text,
    idv text,
    premium numeric,
    customer_payout numeric,
    agent_payout numeric,
    start_date text,
    expiry text,
    status text default 'Active',
    history jsonb
);
```

---

## 🚀 Getting Started

1. Download or host the `index.html` file on a local or remote web server.
2. Open `index.html` in any modern web browser.
3. Sign in using the default Main Admin credentials:
   * **Email:** `minibala33ai@gmail.com`
   * **Password:** `Hock@150998`
4. Use the subscription key console to generate and distribute secure registration keys to external agents.