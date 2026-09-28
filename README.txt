DREAM EDUCATION LIMITED — LIVE PORTAL PACKAGE

What this package contains
1. index.html — complete responsive portal UI.
2. assets/dream-education-logo.jpg — the supplied Dream Education Limited logo.
3. config.example.js — Supabase connection template.
4. supabase/schema.sql — database tables, security policies and server-side draw function.

How to make it live
A. Create a free Supabase project at https://supabase.com
B. Open SQL Editor and run supabase/schema.sql.
C. In Supabase Authentication > Users, create five users with email/password.
   Do NOT enable public sign-up.
D. Add one row per user to public.profiles. Set one trusted account to role='admin' and link member_id.
   Example:
   insert into public.profiles(id,full_name,role,member_id)
   values('USER_UUID','Zahirul Islam','admin',(select id from public.members where full_name='Zahirul Islam'));
E. Copy config.example.js to config.js and add the Supabase Project URL and anon/public key.
   NEVER use the Supabase service_role key in this website.
F. Deploy the folder to Netlify, Vercel or Cloudflare Pages. No build command is required.
G. Share the resulting HTTPS URL with the five members.

Draw behaviour
- Every month the database holds one current cycle.
- The draw function uses PostgreSQL server-side random selection.
- The selected member is immediately marked ineligible for the rest of that cycle.
- After the fifth member wins, the function creates a new cycle and makes all five members eligible again.
- The draw has a database row and unique Draw ID.
- Multiple people clicking at the same time cannot create two winners because the current cycle is locked in the transaction.
- The browser automatically calls the draw function once the scheduled time is reached while members are online. For a true unattended draw when nobody is online, add a Supabase scheduled Edge Function/pg_cron job to call the same function.

Security
- Browser uses only the Supabase anon/public key.
- Row Level Security is enabled.
- Members can read shared records but cannot directly insert/update draw results.
- The draw state is changed by a SECURITY DEFINER Postgres function.
- Create accounts yourself; do not enable public registration.

Important
This is a technical portal package, not legal or financial advice. Before using it for real money, confirm the arrangement complies with applicable law and your company policies. The sample schema marks all initial contributions as paid for testing; change statuses to reflect the real arrangement before going live.


QUICK SETUP FIX
1. Keep index.html and config.js in the same folder.
2. Open config.js in Notepad.
3. Replace PASTE-YOUR-SUPABASE-PUBLISHABLE-KEY-HERE with your Supabase Publishable key (sb_publishable_...).
4. Save the file.
5. Open index.html from this extracted folder (not from inside the ZIP).
6. If the page still says configuration is missing, make sure the file is named exactly config.js and not config.js.txt.

The portal no longer depends on an eligible_count column in the cycles table; it calculates the eligible count from cycle_members.
