# Replit Setup Guide — Organic SMM Pro

Yeh project Lovable se Replit pr import karne ke liye fully ready hai. 100% same backend (Lovable Cloud / Supabase) use hota hai — koi data ya feature loss nahi hoga.

---

## 🚀 Quick Start (Replit pe)

### 1. Import from GitHub
Replit pe **Create Repl → Import from GitHub** → repo URL paste karo (`xbhishekh/organicsmm`).

### 2. Environment Variables (Replit Secrets)
Replit **Tools → Secrets** kholo aur ye 3 secrets add karo:

| Key | Value |
|---|---|
| `VITE_SUPABASE_PROJECT_ID` | `xyjnkuvhogwjydawpjnt` |
| `VITE_SUPABASE_URL` | `https://xyjnkuvhogwjydawpjnt.supabase.co` |
| `VITE_SUPABASE_PUBLISHABLE_KEY` | (anon key from `.env.example`) |

> Ye same Lovable Cloud backend hai — saare tables, edge functions, RLS, cron jobs, providers, users, orders, wallet, subscriptions sab as-is kaam karenge.

### 3. Run
Bas **Run** button dabao. `.replit` file pre-configured hai:
- `npm install` automatic chalega
- Dev server `0.0.0.0:5000` pe start hoga
- Replit webview me preview dikhega

### 4. Deploy (Production)
Replit **Deployments → Static** select karo. Build command (`npm run build`) aur publish dir (`dist`) already configured hain.

---

## 📦 Kya included hai

- ✅ Full frontend (React + Vite + TS + Tailwind + shadcn/ui)
- ✅ `.replit` + `replit.nix` config (Node 20, port 5000)
- ✅ `.env.example` template
- ✅ Supabase client (`src/integrations/supabase/client.ts`) — already wired
- ✅ Saare edge functions (`supabase/functions/`) — Lovable Cloud pe deployed hain
- ✅ Saare migrations (`supabase/migrations/`) — already applied
- ✅ Cron jobs, triggers, RLS policies — already live

> ⚠️ Backend Lovable Cloud (Supabase project `xyjnkuvhogwjydawpjnt`) pe hi rahega. Replit sirf frontend host karega. Agar tum apna alag Supabase chahte ho to alag migration run karna padega.

---

## 🔍 Verify Working Features (Replit pe khud check karne ke liye)

1. Home page load ho raha hai
2. `/auth` pe signup + login working
3. Dashboard pe wallet, stats dikh rahe
4. `/services` pe service list load
5. `/order` pe naya order place ho raha
6. `/engagement-order` pe bundle order
7. `/orders` history dikh rahi
8. `/wallet` pe Razorpay deposit flow
9. `/support` live chat working
10. Admin login (`zyrofit.my@gmail.com`) → `/admin` access
11. Admin pe users, orders, services, providers, deposits, subscriptions
12. Realtime wallet update working

---

## 🤖 Master Verification Prompt (Replit AI ko dena)

Niche wala prompt copy karke Replit ke AI Agent / Assistant ko de dena. Wo ek-ek karke saari functionality verify kar dega.

```
You are auditing a freshly imported Lovable → Replit project: "Organic SMM Pro".

STACK: React 18 + Vite 5 + TypeScript + Tailwind + shadcn/ui + Supabase (Lovable Cloud).
BACKEND: Already deployed on Supabase project `xyjnkuvhogwjydawpjnt`. Do NOT recreate.
ENV REQUIRED (in Replit Secrets): VITE_SUPABASE_URL, VITE_SUPABASE_PUBLISHABLE_KEY, VITE_SUPABASE_PROJECT_ID.

TASK: Verify EVERY feature works end-to-end. For each item below: open the route, perform the action, confirm success, and report PASS / FAIL with the exact error if FAIL.

CHECKLIST:

A. Build & Boot
  1. `npm install` runs without errors
  2. `npm run dev` starts on 0.0.0.0:5000
  3. `npm run build` produces dist/ with no errors
  4. No TypeScript errors (`tsc --noEmit`)
  5. No console errors on first page load

B. Auth (src/pages/Auth.tsx, src/hooks/useAuth.tsx)
  6. Signup with new email works (calls edge function `auto-verify-signup`)
  7. Login with email+password works
  8. Profile, wallet, user_roles fetched on login
  9. Logout works and clears session
  10. Protected routes redirect to /auth when logged out

C. User Dashboard (src/pages/Dashboard.tsx)
  11. Wallet balance shows correctly
  12. Order stats (total, pending, completed) load
  13. Recent orders render
  14. Realtime wallet update works (insert a transaction → balance updates without refresh)

D. Services & Order (src/pages/Services.tsx, Order.tsx)
  15. Services list loads from `services` table
  16. Category filter works
  17. Place order: link + qty → calls `place-order` or `user-place-order` edge function
  18. Wallet debited correctly
  19. Order appears in `/orders` with status

E. Engagement / Organic Order (src/pages/EngagementOrder.tsx)
  20. Bundle list loads (`engagement_bundles`)
  21. Place engagement order → `process-engagement-order` edge function
  22. `organic_run_schedule` rows created with staggered timestamps
  23. Cron `execute-organic-runs` picks up due runs and sends to provider
  24. Multi-provider rotation: if a run fails on provider A, next run uses provider B (verify in `organic_run_schedule.provider_id` rotation)
  25. Completed/partial/cancelled orders cleanup after 20 hours (migration `20260603084442`)

F. Wallet & Payments (src/pages/Wallet.tsx)
  26. Razorpay deposit flow: `create-razorpay-order` → checkout → `verify-razorpay-deposit`
  27. Successful payment credits wallet, creates `transactions` row

G. Subscriptions (src/components/subscription/*)
  28. Subscription guard blocks unsubscribed users on gated pages
  29. Subscription request flow works
  30. Cron `check-subscription-expiry` runs

H. Support / Live Chat (src/pages/Support.tsx, src/components/chat/LiveChatWidget.tsx)
  31. User can create ticket
  32. Live chat messages send/receive via realtime channel
  33. Admin sees conversation in `/admin/chat`

I. Admin Panel (src/pages/admin/*)
  34. Only admin role can access `/admin` (AdminGuard)
  35. /admin: dashboard stats load
  36. /admin/users: list users, change roles, manage wallet
  37. /admin/orders: view all orders, cancel, refund
  38. /admin/services: import services from provider (`import-services`), edit pricing
  39. /admin/providers: add/edit/test provider, check balance (`check-provider-balance`)
  40. /admin/deposits: approve/reject deposits
  41. /admin/subscriptions: manage plans + requests
  42. /admin/chat: reply to live chats
  43. /admin/cron-monitor: cron health visible (`cron-status` edge fn)

J. API Access (src/pages/ApiAccess.tsx)
  44. User can generate API key
  45. `public-api` edge function responds to /services, /add, /status, /balance with API key

K. Other
  46. Currency switcher (useCurrency) works with exchange-rates
  47. Maintenance mode toggle (`platform_settings`) shows MaintenanceMode component
  48. Mobile responsive (test viewport 375px)
  49. PWA manifest + service worker (sw.js) registered
  50. Telegram + admin email alerts fire on failures

DELIVERABLE: Markdown report with one line per check: `✅ #N PASS` or `❌ #N FAIL — <error>`. Fix every FAIL before declaring done. Do NOT modify backend (Supabase) — only frontend code. Do NOT remove features. Do NOT change UI design.
```

---

## 🐛 Common Replit Issues

| Issue | Fix |
|---|---|
| Port not exposed | `.replit` me port 5000 already mapped. Restart Repl. |
| Env vars missing | Replit Secrets me 3 keys add karo (upar table) |
| `npm install` slow | Replit pe pehli baar 2-3 min lagta hai, normal |
| Blank page | DevTools console kholo, env vars check karo |
| Auth fail | Supabase URL/anon key galat hai — Secrets verify |

---

Sab kuch ready hai. Bas GitHub se import karo, secrets add karo, Run dabao. ✅
