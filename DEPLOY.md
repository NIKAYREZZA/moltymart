# Moltymart Deployment Guide

## Struktur File
```
moltymart/
├── index.html              ← Landing page
├── moltymart-app.html      ← Chat app (Molly)
├── vercel.json             ← Vercel config
└── supabase/
    └── functions/
        └── chat/
            └── index.ts    ← Edge Function (proxy API)
```

---

## Step 1 — Setup Supabase Edge Function

1. Install Supabase CLI:
   ```
   npm install -g supabase
   ```

2. Login:
   ```
   supabase login
   ```

3. Link ke project kamu:
   ```
   supabase link --project-ref vxpegselpekebhzrlxfg
   ```

4. Set secret API key Anthropic:
   ```
   supabase secrets set ANTHROPIC_API_KEY=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
   ```

5. Deploy Edge Function:
   ```
   supabase functions deploy chat
   ```

6. Test endpoint:
   ```
   curl -X POST https://vxpegselpekebhzrlxfg.supabase.co/functions/v1/chat \
     -H "Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9..." \
     -H "Content-Type: application/json" \
     -d '{"system":"You are helpful","messages":[{"role":"user","content":"Hi"}]}'
   ```

---

## Step 2 — Setup Supabase Tabel Waitlist

Di Supabase dashboard → SQL Editor, jalankan:

```sql
CREATE TABLE waitlist (
  id BIGSERIAL PRIMARY KEY,
  email TEXT UNIQUE NOT NULL,
  source TEXT,
  created_at TIMESTAMPTZ DEFAULT NOW()
);

-- Allow public insert (for waitlist form)
ALTER TABLE waitlist ENABLE ROW LEVEL SECURITY;
CREATE POLICY "Allow public insert" ON waitlist FOR INSERT WITH CHECK (true);
```

---

## Step 3 — Deploy ke Vercel

1. Install Vercel CLI:
   ```
   npm install -g vercel
   ```

2. Masuk ke folder project:
   ```
   cd moltymart
   ```

3. Deploy:
   ```
   vercel
   ```
   - Set up project? → Y
   - Project name → moltymart
   - Directory → ./
   - Override settings? → N

4. Production deploy:
   ```
   vercel --prod
   ```

5. Custom domain (opsional):
   - Beli domain di Niagahoster/Rumahweb
   - Di Vercel dashboard → Settings → Domains → Add domain

---

## ✅ Done!

Website live di: https://moltymart.vercel.app
Molly chat app: https://moltymart.vercel.app/moltymart-app.html
