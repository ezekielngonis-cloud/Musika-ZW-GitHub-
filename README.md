# Musika ZW full-stack starter

This version separates the public marketplace (`index.html`) from the admin dashboard (`admin.html`). GitHub Pages hosts the static frontend; Supabase provides shared authentication, database, row-level security (RLS), and photo storage.

## Setup
1. Create a project at https://supabase.com/.
2. In Supabase SQL Editor, run `supabase/schema.sql`.
3. Copy the Project URL and public anon/publishable key from Supabase project settings.
4. Paste them into `js/config.js`. The public key is okay to expose only because RLS is enabled; NEVER put a `service_role` key in browser code or GitHub.
5. In Supabase Authentication URL Configuration, add your GitHub Pages URL to allowed URLs.
6. Upload the whole folder contents to your GitHub repository. `index.html`, `admin.html`, `css/`, `js/`, and `supabase/` should be at repository root.
7. Enable GitHub Pages from branch `main`, folder `/ (root)`.

Public site: `https://YOUR-USERNAME.github.io/YOUR-REPOSITORY/`
Admin page: `https://YOUR-USERNAME.github.io/YOUR-REPOSITORY/admin.html`

## Create the first admin
1. Sign up in the public app using the admin's email and confirm the account if email confirmation is enabled.
2. In Supabase SQL Editor, run this statement with the correct email:

```sql
update public.profiles p set role = 'admin'
from auth.users u where p.id = u.id and lower(u.email) = lower('your-admin-email@example.com');
```

Ordinary users cannot set their own role through the app. The admin URL is separate, but the database role and RLS policies are the real security boundary.

## Current scope / important limits
- Account registration/sign-in, shared listings, photo uploads, order requests, seller order updates, support messages, and admin moderation are scaffolded.
- Orders are requests only: no payment or delivery is handled.
- Before a commercial launch, add atomic stock reservation, stricter order-state transitions, audit logs, abuse reporting, seller verification, backups, and review the RLS policies. Test using two accounts and two devices first.
