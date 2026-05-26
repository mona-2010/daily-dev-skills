---
name: supabase-integration
description: "Step-by-step workflow to integrate Supabase into an existing Next.js project. Use when: setting up database, auth, or storage in Next.js; connecting frontend to Supabase; configuring server/client clients; adding middleware for auth."
user-invocable: true
---

# Setting Up Supabase in an Existing Next.js Project

A practical guide to integrating Supabase into an existing Next.js application using production-grade folder structure and best practices.

---

# Why Supabase?

Supabase provides:

* PostgreSQL database
* Authentication
* Storage
* Real-time subscriptions
* Row-Level Security (RLS)
* Edge functions

It is commonly used as a scalable backend solution for modern Next.js applications.

---

# Recommended Stack

* Next.js App Router
* TypeScript
* Supabase
* Tailwind CSS
* React Hook Form
* Zod

---

# Step 1 — Install Supabase Packages

Run:

```bash id="jlwm1"
npm install @supabase/supabase-js
```

Optional SSR package:

```bash id="jlwm2"
npm install @supabase/ssr
```

---

# Step 2 — Create Supabase Project

Go to:

[Supabase Dashboard](https://supabase.com/dashboard?utm_source=chatgpt.com)

Create:

* organization
* project
* database password

After project creation:

Navigate to:

```txt id="jlwm3"
Project Settings → API
```

Copy:

* Project URL
* anon public key

---

# Step 3 — Setup Environment Variables

Create:

```txt id="jlwm4"
.env.local
```

Add:

```env id="jlwm5"
NEXT_PUBLIC_SUPABASE_URL=your_project_url

NEXT_PUBLIC_SUPABASE_ANON_KEY=your_anon_key
```

Important:

* NEVER expose service role keys
* ONLY expose NEXT_PUBLIC variables to frontend

---

# Step 4 — Recommended Folder Structure

```txt id="jlwm6"
src/
│
├── lib/
│   └── supabase/
│       ├── client.ts
│       ├── server.ts
│       └── middleware.ts
│
├── services/
│
├── hooks/
│
├── features/
│
└── types/
```

---

# Why Separate Supabase Clients?

Enterprise applications separate:

* browser client
* server client
* middleware client

This improves:

* authentication handling
* security
* maintainability
* scalability

---

# Step 5 — Create Browser Client

# src/lib/supabase/client.ts

```ts id="jlwm7"
import { createClient } from "@supabase/supabase-js";

export const supabase = createClient(
  process.env.NEXT_PUBLIC_SUPABASE_URL!,
  process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
);
```

This client is used in:

* client components
* browser interactions
* frontend requests

---

# Step 6 — Create Server Client

# src/lib/supabase/server.ts

```ts id="jlwm8"
import { createClient } from "@supabase/supabase-js";

export function createServerSupabaseClient() {
  return createClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
  );
}
```

Used for:

* server components
* route handlers
* server-side fetching

---

# Step 7 — Test Database Connection

Example query:

```ts id="jlwm9"
const { data, error } = await supabase
  .from("users")
  .select("*");
```

Always handle errors:

```ts id="jlwm10"
if (error) {
  console.error(error);
}
```

---

# Step 8 — Create Service Layer

Avoid direct database queries inside components.

Bad:

```tsx id="jlwm11"
const { data } = await supabase
  .from("users")
  .select("*");
```

Good:

```txt id="jlwm12"
services/user.service.ts
```

Example:

```ts id="jlwm13"
import { supabase } from "@/lib/supabase/client";

export async function getUsers() {
  const { data, error } = await supabase
    .from("users")
    .select("*");

  if (error) {
    throw error;
  }

  return data;
}
```

Benefits:

* reusable logic
* centralized queries
* easier testing
* cleaner UI

---

# Step 9 — Add Authentication

Enable providers:

```txt id="jlwm14"
Authentication → Providers
```

Examples:

* Google
* GitHub
* Email/Password

---

# Email Login Example

```ts id="jlwm15"
const { error } = await supabase.auth.signInWithPassword({
  email,
  password,
});
```

---

# Signup Example

```ts id="jlwm16"
const { error } = await supabase.auth.signUp({
  email,
  password,
});
```

---

# Logout Example

```ts id="jlwm17"
await supabase.auth.signOut();
```

---

# Step 10 — Protect Routes

Example middleware structure:

```txt id="jlwm18"
middleware.ts
```

Use middleware to:

* protect dashboards
* validate sessions
* handle redirects
* restrict private routes

---

# Recommended Authentication Flow

```txt id="jlwm19"
User Login
    ↓
Supabase Auth
    ↓
Session Created
    ↓
Middleware Validation
    ↓
Protected Route Access
```

---

# Step 11 — Enable Row-Level Security (RLS)

VERY IMPORTANT.

Without RLS:

* anyone can access database tables

Enable:

```sql id="jlwm20"
alter table users
enable row level security;
```

---

# Example Policy

```sql id="jlwm21"
create policy "Users can view own profile"
on users
for select
using (auth.uid() = id);
```

---

# Why RLS Matters

RLS provides:

* database-level security
* user-level access control
* protection against unauthorized access

Enterprise systems ALWAYS use RLS.

---

# Step 12 — Generate TypeScript Types

Install CLI:

```bash id="jlwm22"
npm install supabase --save-dev
```

Generate types:

```bash id="jlwm23"
npx supabase gen types typescript \
--project-id your_project_id \
> src/types/database.types.ts
```

Benefits:

* type safety
* autocomplete
* reduced bugs
* better developer experience

---

# Recommended Enterprise Architecture

```txt id="jlwm24"
Component
   ↓
Hook
   ↓
Service Layer
   ↓
Supabase Client
   ↓
Database
```

Avoid querying Supabase directly everywhere.

---

# Common Mistakes

# ❌ Direct Queries Inside Components

Bad for scalability.

---

# ❌ Exposing Service Role Key

Never expose:

```env id="jlwm25"
SUPABASE_SERVICE_ROLE_KEY
```

to frontend code.

---

# ❌ Disabling RLS

This creates serious security risks.

---

# ❌ Using One Massive Supabase File

Separate:

* client
* server
* middleware
* services

---

# ❌ Storing Everything in Global State

Supabase already manages sessions efficiently.

Avoid unnecessary complexity.

---

# Recommended Production Setup

## Frontend

* Next.js
* TypeScript
* Tailwind CSS

## State

* React Query
* Zustand / Redux Toolkit

## Validation

* Zod
* React Hook Form

## Backend

* Supabase
* PostgreSQL
* RLS Policies

---

# Enterprise Best Practices

* Use feature-based architecture
* Separate services from components
* Generate TypeScript database types
* Enable RLS on every table
* Use middleware for route protection
* Centralize environment variables
* Avoid duplicated queries
* Handle loading and error states properly

---

# Final Advice For Junior Developers

Supabase makes backend development extremely fast, but poor architecture can still create unmaintainable applications.

Focus on:

* clean separation of concerns
* reusable services
* secure authentication
* proper folder structure
* scalable architecture

Build systems that are maintainable for teams, not just personal projects.

---

# Key Takeaways

* Separate Supabase clients properly
* Never expose service role keys
* Use RLS for database security
* Centralize queries inside services
* Protect routes using middleware
* Generate TypeScript database types
* Follow scalable frontend architecture
