# PolicyGuard - Professional Insurance Management Portal

PolicyGuard is a modern, mobile-responsive web application engineered for insurance agents and agencies to track customer policy renewals, automate client communications via WhatsApp and direct calling, and manage subscription licenses.

---

## 🌟 Key Features

### 1. ⏱️ Expiring Policies Radar
- Filter policies expiring within custom timeframes: **Next 2, 5, 7, 15, or 30 days**.
- Live countdown badges highlighting urgent renewals (e.g., "Expires Today!", "2 days left").

### 2. 📱 One-Click Outreach
- **📞 Direct Mobile Call:** Triggers `tel:` deep-linking to launch phone dialpads automatically with pre-filled numbers.
- **💬 WhatsApp Reminders:** Drafts customized, professional renewal message templates sent directly to customer WhatsApp chats.

### 3. 👥 Customer Profile & Vault Management
- Full CRUD operations: Edit customer details, policy numbers, phone numbers, premium amounts, and expiration dates at any time.
- **Status Toggle:** Instantly flag customers as **Active** or **Inactive** (e.g., if a client moved to another insurer). Inactive clients are preserved in the vault but automatically hidden from renewal alert radars.

### 4. 📤 Bulk Excel Import & Export
- **Bulk Import:** Import hundreds or thousands of existing customer records at once using `.xlsx`, `.xls`, or `.csv` spreadsheets.
- **Selective Export:** Export selected or filtered customer lists back to Excel (`.csv`).

### 5. 📱 Fully Mobile-Responsive Design
- **Collapsible Sidebar:** Features a hamburger menu button (`☰`) and close button (`✕`) for small mobile viewports so navigation never blocks the workspace.
- **Flexible Table Layouts:** Horizontal scrolling tables and adaptive flex containers ensuring full readability on smartphones, tablets, and laptops.

### 6. ⚙️ Admin & License Control
- Master Admin portal to configure annual platform subscription fees.
- Generate single-use license keys (`KEY-XXXX-XXXX-XXXX`) for agent onboarding.
- Manage agent access with one-click subscription extensions (+1 Month, +6 Months, +1 Year).

---

## 📂 File Structure

```text
policyguard/
├── index.html        # Complete Single-File Application (HTML + Tailwind CSS + JS)
└── README.md         # Application Documentation