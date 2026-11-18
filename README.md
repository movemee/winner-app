# WINNER — وینر (v0.2)

A Persian RTL mobile-first Life OS MVP with the **user-supplied logo** and **Alibaba Light/Black** fonts. React + TypeScript + Vite + Supabase Auth/Postgres. Deployable as static assets on Cloudflare Pages. No secrets or Supabase service-role keys belong in the browser.

## Quick start
1. Create a Supabase project. In SQL Editor run `supabase/001_init.sql` **once**.
2. Enable Email/Password sign-in in Authentication > Providers. For quick testing, decide whether email confirmation should be enabled; if enabled, verify your email.
3. Copy `.env.example` to `.env` and set `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY` (publishable/anon key, **not service_role**).
4. `npm install` then `npm run dev`.
5. Register an account. Your display name is used in “سلام [name]!” on the dashboard.
6. To give your account the announcement administrator role, run this in Supabase SQL Editor **after signup**, replacing the email:

```sql
alter table public.profiles disable trigger profile_admin_guard;
update public.profiles set is_admin = true
where id = (select id from auth.users where email = 'YOUR_ADMIN_EMAIL');
alter table public.profiles enable trigger profile_admin_guard;
```

7. Refresh the app and use “مدیریت پیام‌ها” in the side menu to publish a quote for all users. Only active announcements are visible; newest active quote is shown.

## Cloudflare Pages
- Push the project to GitHub, create a **Cloudflare Pages** project, select the repo.
- Build command: `npm run build`; build output directory: `dist`.
- Set environment variables `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY` in Pages project settings for Production and Preview.
- Add the deployed domain to Supabase Authentication > URL Configuration (Site URL / Redirect URLs) as needed.
- SPA fallback: `public/_redirects` copied to `dist`.
- A Cloudflare Worker is **not required** for this MVP: the client talks directly to Supabase using RLS. A Worker/Queues/Cron will be required for reliable scheduled notifications and server-only integrations.

## Included
- Email signup/login, profile display name, personalized welcome
- Global administrator-managed announcements (RLS protected)
- Tasks with completion, categories, dates
- Habits with daily completion logs
- Finance commitments with amount and due date
- Fitness sessions, goals, simple calendar and progress overview
- Responsive mobile bottom navigation and side drawer
- Data isolation via Supabase row-level security

## Limitations / not implemented yet
- No push/SMS notification delivery, recurring tasks, payments, receipts, shared workspaces, advanced AI, complete weekly historical analytics, offline-first synchronization or production monitoring.
- Calendar currently lists dated tasks; not a full monthly calendar grid.
- The app has not been verified against **your** Supabase project or live Cloudflare deployment. Test sign-up, data isolation, mobile layout, and admin permissions before public launch.
- Financial fields are planning records only; not banking integration.
- The uploaded logo has a white background, so the app shows it as supplied. A transparent vector export is recommended for production.

## Suggested smoke test
1. Create user A and user B; verify they cannot see each other's tasks.
2. Edit the profile name and reload; confirm greeting updates.
3. Add a task, habit, fitness session, goal and finance item; reload and verify persistence.
4. Mark/unmark tasks and today's habit; verify counts.
5. Grant admin only to A and publish a quote; verify B can read it but cannot create/change it.
6. Check at widths 360px, 390px, 768px and desktop.
