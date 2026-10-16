# AMC RECORDS — Music Distribution Platform Starter

A deployable starter for a music-distribution portal with separate artist and admin experiences.

## Stack
- Next.js App Router + TypeScript
- Supabase Auth (email/password), Postgres, and private Storage
- Row Level Security (RLS) for artist-owned records
- Manual royalty entry; optional provider API integration points are documented, not falsely simulated

## Included
- Landing page and role-aware portal shell
- Sign-in / sign-up page using Supabase
- Artist portal: submit releases, view release status, royalty ledger
- Admin portal: review submissions, update release status, enter royalty rows
- Supabase SQL schema with RLS policies
- Private audio/artwork storage bucket setup notes
- Deployment guide for Vercel + Supabase

## Setup
1. Create a Supabase project at https://supabase.com.
2. In Supabase SQL Editor, run `supabase/schema.sql`.
3. In Storage, create a **private** bucket named `release-assets`. Limit uploads and configure policies before production use.
4. Copy `.env.example` to `.env.local` and fill in your Supabase project URL and anon key.
5. Install Node.js 20+ and run:
   ```bash
   npm install
   npm run dev
   ```
6. Open http://localhost:3000.
7. In Supabase Authentication > Users, create your first account. Promote it to admin in SQL:
   ```sql
   update public.profiles set role = 'admin' where email = 'YOUR_EMAIL';
   ```
   Only do this for an account you control. Do not expose the service-role key in browser code.
8. In Supabase Auth URL Configuration, set the local site URL to `http://localhost:3000`; add your production domain later.

## Environment variables
`NEXT_PUBLIC_SUPABASE_URL` and `NEXT_PUBLIC_SUPABASE_ANON_KEY` are safe for the browser when RLS is correctly configured. Never put a Supabase service-role key in a `NEXT_PUBLIC_` variable.

## Deploy
- Push this project to a private GitHub repository.
- Import it into Vercel.
- Set the two environment variables from `.env.local` in Vercel Project Settings.
- Set the production URL in Supabase Auth URL Configuration and add it to redirect URLs.
- Deploy, test artist isolation and admin access with two separate accounts, and only then onboard real artists.

## Important production checklist
- Add email verification, password reset, rate limiting / abuse protection, audit logging, file validation and size limits.
- Keep audio and artwork in private storage; issue short-lived signed URLs when authorized.
- Review copyright ownership and distribution agreements, privacy policy, takedown policy, payout/tax requirements, and Indian data/privacy obligations with qualified professionals.
- Do not claim a release is delivered to Spotify/Apple/YouTube Music until a distributor/label-services partner API or dashboard confirms delivery. This starter tracks internal workflow only.
- Automatic royalties require a real provider contract/API and mapping. The starter supports manual ledger entries and has an integration placeholder; no provider credentials or live integrations are included.
- Payment processing and payouts are not wired to a payment gateway.
