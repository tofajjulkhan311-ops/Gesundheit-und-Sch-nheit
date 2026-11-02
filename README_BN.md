# Vercel + Supabase সেটআপ (বাংলা)

এই প্রজেক্টটি Next.js 16 + Supabase Auth/Database দিয়ে তৈরি। এটি একটি deploy-ready starter; আপনার Vercel/Supabase অ্যাকাউন্টে নিজে থেকে প্রবেশ বা deploy করা হয়নি।

## ১) Supabase project তৈরি
1. https://supabase.com এ গিয়ে নতুন project তৈরি করুন।
2. Project Dashboard → SQL Editor → New query খুলুন।
3. `supabase/schema.sql` ফাইলের সম্পূর্ণ SQL কপি করে Run করুন।
4. Project Settings → API থেকে Project URL এবং Publishable key (বা legacy anon key) নিন।

## ২) প্রথম Admin account তৈরি
1. Supabase → Authentication → Users → Add user নির্বাচন করুন।
2. আপনার নিজের ইমেইল ও শক্তিশালী পাসওয়ার্ড দিয়ে user তৈরি করুন।
3. ওই user-এর UUID কপি করুন।
4. SQL Editor-এ চালান:
   `insert into public.admin_users (user_id) values ('আপনার-UUID');`
5. Public sign-up বন্ধ রাখুন: Authentication → Providers/Sign-in settings-এ Email signups বন্ধ করুন (Dashboard UI-তে নাম সামান্য বদলাতে পারে)। Admin account শুধু Dashboard থেকে তৈরি করবেন।
6. Admin Login URL হবে `/admin/login`।

## ৩) Vercel-এ Deploy
1. ZIP extract করে GitHub-এ একটি নতুন private repository তৈরি করে সব ফাইল push করুন।
2. https://vercel.com এ Login → Add New → Project → GitHub repository Import করুন।
3. Framework Next.js স্বয়ংক্রিয়ভাবে শনাক্ত হওয়ার কথা।
4. Environment Variables যোগ করুন:
   - `NEXT_PUBLIC_SUPABASE_URL` = Supabase Project URL
   - `NEXT_PUBLIC_SUPABASE_ANON_KEY` = Supabase Publishable/anon key
5. Deploy চাপুন। Deploy সফল হলে Vercel একটি `vercel.app` URL দেবে।
6. Environment variable বদলালে নতুন deployment করতে হবে।

## ৪) পরীক্ষা
- Homepage খুলুন।
- `/admin/login` এ Admin email/password দিয়ে login করুন।
- একটি test product যোগ করুন; `is_active` টিক থাকলে homepage-এ দেখা যাবে।
- “Zum Angebot” ক্লিক করলে `/go/{product-id}` দিয়ে click record করে Affiliate URL-এ redirect হবে।
- Dashboard-এর click count হলো redirect count; affiliate network-এর confirmed sale/commission নয়।

## নিরাপত্তা নোট
- Supabase `service_role` key কখনো browser, GitHub, বা `NEXT_PUBLIC_` variable-এ দেবেন না। এই starter-এ service role key লাগে না।
- `.env*` GitHub-এ commit করবেন না। `.env.example`-এ কেবল placeholder আছে।
- RLS policies database স্তরে admin-only product changes enforce করে।
- Admin membership শুধু Supabase dashboard/SQL দিয়ে provision করুন; public signup বন্ধ রাখুন।
- Production-এ deploy করার আগে Supabase Auth URL Configuration-এ আপনার Vercel domain (Site URL এবং Redirect URLs) যোগ করুন।
- Public launch-এর আগে Impressum/Legal Notice, Datenschutzerklärung/Privacy Policy, cookie/consent obligations, affiliate program terms, এবং প্রয়োজনীয় advertising disclosures ঠিক করুন। জার্মান audience target করায় স্থানীয় আইনগত পরামর্শ নেওয়া ভালো।
- Health/weight-loss products-এর জন্য চিকিৎসা-সংক্রান্ত বা নিশ্চিত ওজন কমানোর দাবি লিখবেন না; supplier-approved, evidence-based description ব্যবহার করুন।

## সীমাবদ্ধতা
এই starter-এ password reset UI, multi-admin role hierarchy, image upload storage, search/filter, order tracking বা affiliate network-এর sale/conversion API integration নেই। এগুলো আলাদা করে যোগ করতে হবে।
