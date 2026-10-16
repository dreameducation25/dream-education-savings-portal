DREAM EDUCATION LIMITED — LIVE PORTAL PACKAGE

What this package contains
1. index.html — complete responsive portal UI.
2. assets/dream-education-logo.jpg — the supplied Dream Education Limited logo.
3. config.example.js — Supabase connection template.
4. supabase/schema.sql — database tables, security policies and server-side draw function.

How to make it live
A. Create a free Supabase project at https://supabase.com
B. Open SQL Editor and run supabase/schema.sql.
C. In Supabase Authentication > Users, create the required users with email/password.
   Do NOT enable public sign-up.
D. Add one row per user to public.profiles. Set one trusted account to role='admin' and link member_id.
   Example:
   insert into public.profiles(id,full_name,role,member_id)
   values('USER_UUID','Zahirul Islam','admin',(select id from public.members where full_name='Zahirul Islam'));
E. Copy config.example.js to config.js and add the Supabase Project URL and anon/public key.
   NEVER use the Supabase service_role key in this website.
F. Deploy the folder to Netlify, Vercel or Cloudflare Pages. No build command is required.
G. Share the resulting HTTPS URL with your active members.

Draw behaviour
- Every month the database holds one current cycle.
- The draw function uses PostgreSQL server-side random selection.
- The selected member is immediately marked ineligible for the rest of that cycle.
- After all currently eligible members have won, the function creates a new cycle and makes all active members eligible again. There is no maximum number of active members. Each active member contributes BDT 500 per month, so the draw amount scales with the number of active members.
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


UPDATED PROGRAM SETTINGS
- Monthly contribution: BDT 500 per member
- Monthly fund for 5 active members: BDT 2,500
- First new draw: 5 November 2026 at 20:00 Bangladesh time
- Future draws advance monthly on the 5th at the same time
- Existing logo, Supabase login, password reset, and portal UI are retained.

DATABASE RESET
Before using the new program on the existing Supabase project, run:
supabase/RESET_FOR_NOVEMBER_2026.sql

That reset script removes existing draw/cycle/contribution records only. It preserves
members, profiles, Auth users, passwords, and the portal authentication setup.


UPDATED OCTOBER 2026
- Monthly contribution: BDT 500 per active member; total fund scales with active membership.
- First draw: 5 November 2026 at 20:00 Bangladesh time.
- Requested previous draw history reset: run supabase/APPLY_UPDATES.sql ONCE in the existing Supabase SQL Editor. This is destructive to prior draw/cycle/contribution records but preserves login/accounts and members.
- Upload company jingle and suspense music from the admin Sound Controls after running APPLY_UPDATES.sql. Audio files are stored in the Supabase public bucket and can be heard by authenticated members.
- Sound playback depends on browser autoplay restrictions; after login, if sound does not start, use the browser/player sound control once.
- Winner announcement uses the browser's available English (UK) text-to-speech voice. The exact female voice depends on the device/browser and cannot be guaranteed by the website.
- The portal UI and login/logo were preserved.


BUNDLED BRANDING AND AUDIO (October 2026 update)
- assets/dream-education-logo.jpg: exact company logo supplied by the owner.
- assets/company-jingle.mp3: Dream It, Achieve It company jingle supplied by the owner.
- assets/suspense-music.mp3: Countdown Waiting suspense track supplied by the owner.
These assets are included in the ZIP and do not need to be uploaded manually in the portal.
The browser may block automatic audio until the user interacts with the page; signing in is the first user interaction, but browser/device settings can still affect playback.

IMPORTANT BACKEND STEP
This is a static frontend ZIP connected to the existing Supabase project. The ZIP cannot safely apply database changes just by being deployed. To clear old draw history and set the next draw to 5 November 2026 at 8:00 PM Bangladesh time, review and run supabase/APPLY_UPDATES.sql ONCE in the existing Supabase SQL Editor. It deletes existing draws, contributions, cycle_members, and cycles, but keeps member records, profiles, Auth accounts/passwords, and this portal's branding/audio files. Do not run it if you need the old history.
Then deploy the contents of this folder to the existing Cloudflare Worker / GitHub-connected deployment. Keep config.js unchanged unless you intentionally change Supabase project credentials.
