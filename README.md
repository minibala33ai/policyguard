# 🛡️ PolicyGuard - Insurance Renewal & Agent Management Portal

**PolicyGuard** is a lightweight, responsive single-file web application built for insurance agents and agencies. It simplifies tracking policy expirations, managing client databases, communicating via WhatsApp/Phone, and handling multi-agent access with license key control.

---

## 🌟 Key Features

### 1. ⏱️ Expiring Policies Radar
* **Dynamic Countdown:** Automatically calculates remaining days until policy expiration against the current date.
* **Custom Filter Windows:** Filter upcoming policy renewals within 2, 5, 7, 15, or 30 days.
* **Instant Quick Actions:** Direct links to launch **📞 Phone Calls** or pre-filled **💬 WhatsApp Messages** directly to the customer.

### 2. 👥 Customer & Policy Vault
* **Comprehensive Directory:** Search through all client records by customer name, mobile number, policy number, or status.
* **Status Controls:** Toggle policies between `Active` and `Inactive` status.
* **Inline Actions:** Edit customer/policy parameters or delete individual customer entries seamlessly.

### 3. 📤 Import & Export Capabilities
* **Excel/CSV Export:** Export selected client records into downloadable `.xlsx` / `.csv` spreadsheets.
* **Bulk Excel Import:** Upload spreadsheets containing customer data to automatically populate the policy vault.

### 4. ⚙️ Admin Console & License Management
* **Single-Use License Keys:** Master admins can generate 12-character license keys to register new insurance agents.
* **Agent Subscription Control:** Track agent subscription expiration dates and extend accounts (+1 Month, +6 Months, +1 Year) or disable inactive agents.
* **Configurable Platform Fees:** Customize annual platform subscription prices.

---

## 🚀 Quick Start / Local Setup

Because PolicyGuard is built as a single-file application (`index.html`), it requires **no backend installation or build processes**.

### Step 1: Open the Application
1. Download or save `index.html` on your computer.
2. Double-click `index.html` to open it in any modern web browser (Google Chrome, Microsoft Edge, Brave, Safari, or Mozilla Firefox).

---

## 🔑 Default Credentials

The system comes pre-configured with two user accounts stored in `localStorage`:

### 👑 Admin Account
* **Role:** Master Admin
* **Email:** `minibala33ai@gmail.com`
* **Password:** `Hock@150998`
* **Privileges:** Access to Admin Console, License Key Generation, Agent Management, and full Policy Vault.

### 💼 Test Agent Account
* **Role:** Agent
* **Email:** `agent@gmail.com`
* **Password:** `agent`
* **Privileges:** Manage assigned client policies, add new policies, import/export customer data.

---

## 📁 Excel Import Format Guide

When uploading customer spreadsheets via the **Import Excel / CSV** button, ensure your spreadsheet headers match any of the supported names below:

| Target Property | Accepted Column Headers | Example |
| :--- | :--- | :--- |
| **Customer Name** | `Customer Name`, `Customer`, `Name` | Rajesh Sharma |
| **Mobile Number** | `Mobile Number`, `Mobile`, `Phone` | 9876543210 |
| **Policy Number** | `Policy Number`, `Policy`, `Policy No` | POL-887712 |
| **Company** | `Insurance Company`, `Company` | HDFC Ergo |
| **Type** | `Policy Category`, `Type` | Motor Insurance |
| **Premium Amount** | `Premium (INR)`, `Premium` | 14500 |
| **Expiry Date** | `Expiry Date`, `Expiry` | 2026-04-28 (`YYYY-MM-DD`) |

---

## 🛠️ Technical Architecture

* **Frontend Framework:** Plain HTML5 & Vanilla JavaScript (ES6+)
* **Styling:** [Tailwind CSS CDN](https://tailwindcss.com/)
* **Excel Engine:** [SheetJS (XLSX) CDN](https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js)
* **Storage Engine:** Browser `localStorage` (Keys: `policyguard_db`)

---

## ❓ Frequently Asked Questions & Troubleshooting

### Q: Where is my data saved?
All policy records, user credentials, and license keys are saved securely inside your web browser's **Local Storage** (`localStorage`). Clearing your browser history/cache will remove local data unless you export your data first.

### Q: Why isn't WhatsApp opening when clicking the button?
Make sure the client's mobile number includes 10 digits without leading zeroes or country codes. The app automatically prepends the standard country code (`+91`).

### Q: How do I reset data to default?
Open your browser's Developer Console (`F12`), type `localStorage.removeItem('policyguard_db');` and press **Enter**, then refresh the page.
