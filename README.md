# Community-Polling-System

# HX-38 — Community Polling & Decision Platform

## Tech Stack
- **Next.js 14** (App Router)
- **Tailwind CSS**
- **Supabase** (PostgreSQL)
- **Recharts** (voting charts)

---

## 🚀 Step 1: Supabase Setup

1. Go to [supabase.com](https://supabase.com) → **New Project**
2. Name it: `community-poll` → Set a password → Create
3. Wait ~2 mins for it to provision
4. Go to **SQL Editor** → paste and run this:

```sql
-- POLLS TABLE
create table polls (
  id uuid default gen_random_uuid() primary key,
  title text not null,
  description text,
  deadline timestamptz not null,
  status text default 'active' check (status in ('active', 'closed')),
  created_by text not null,
  created_at timestamptz default now()
);

-- OPTIONS TABLE
create table options (
  id uuid default gen_random_uuid() primary key,
  poll_id uuid references polls(id) on delete cascade,
  option_text text not null
);

-- VOTES TABLE
create table votes (
  id uuid default gen_random_uuid() primary key,
  poll_id uuid references polls(id) on delete cascade,
  option_id uuid references options(id) on delete cascade,
  voter_name text not null,
  created_at timestamptz default now(),
  unique(poll_id, voter_name)
);

-- Enable RLS but allow all for hackathon
alter table polls enable row level security;
alter table options enable row level security;
alter table votes enable row level security;

create policy "Allow all" on polls for all using (true) with check (true);
create policy "Allow all" on options for all using (true) with check (true);
create policy "Allow all" on votes for all using (true) with check (true);
```

5. Go to **Settings → API** → Copy:
   - `Project URL`
   - `anon public` key

---

## 🛠 Step 2: Local Setup

```bash
# Clone or unzip this project, then:
npm install

# Create .env.local file:
cp .env.example .env.local
# Paste your Supabase URL and anon key inside
```

---

## ▶️ Step 3: Run

```bash
npm run dev
# Open http://localhost:3000
```

---

## 📦 Step 4: Deploy to Vercel

```bash
npx vercel
# Add env vars in Vercel dashboard too
```
