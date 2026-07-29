# goodi moodi POS

## Deploy in 4 steps

### Step 1 — Set up Supabase
1. Go to [supabase.com](https://supabase.com) → New project
2. Open **SQL Editor** → paste the entire contents of `schema.sql` → Run
3. Go to **Project Settings → API**
4. Copy **Project URL** and **anon / public key**

### Step 2 — Add your Supabase credentials
Open `index.html` and replace these two lines near the top of the `<script>` block:
```js
const SUPABASE_URL  = 'https://YOUR_PROJECT_REF.supabase.co'
const SUPABASE_KEY  = 'YOUR_ANON_KEY_HERE'
```

### Step 3 — Push to GitHub
```bash
git init
git add .
git commit -m "goodi moodi POS"
git remote add origin https://github.com/YOUR_USERNAME/goodi-moodi.git
git push -u origin main
```

### Step 4 — Deploy on Vercel
1. Go to [vercel.com](https://vercel.com) → New Project
2. Import your GitHub repo
3. Framework Preset: **Other** (static site)
4. Click **Deploy** — done ✅

---

## Files
| File | Purpose |
|------|---------|
| `index.html` | The entire POS app |
| `schema.sql` | Run once in Supabase SQL Editor |
| `vercel.json` | Vercel routing config |

## Supabase tables
| Table | Stores |
|-------|--------|
| `products` | Inventory items |
| `sales` | Order headers |
| `sale_items` | Line items per order |
| `stock_adjustments` | Manual stock removal log |
