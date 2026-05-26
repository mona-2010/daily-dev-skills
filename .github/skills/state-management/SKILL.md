---
name: state-management
description: "Best practices for managing state in React and Next.js applications, including global state management, server state management with React Query, and architectural patterns for scalable applications."
user-invocable: true
---
# React Query Best Practices

A practical guide to using TanStack Query (React Query) efficiently in production-grade React and Next.js applications.

---

# What is React Query?

React Query (TanStack Query) is a powerful server-state management library designed for:

* data fetching
* caching
* synchronization
* background updates
* pagination
* optimistic updates

It helps eliminate unnecessary boilerplate and improves frontend performance significantly.

---

# Why React Query Matters

Traditional frontend applications often suffer from:

* excessive useEffect usage
* duplicated loading states
* inconsistent caching
* unnecessary API calls
* complex global state management

React Query solves these problems with:

* automatic caching
* background refetching
* request deduplication
* stale data handling
* optimistic updates
* built-in loading/error management

---

# Install React Query

```bash id="jlwm1"
npm install @tanstack/react-query
```

---

# Recommended Project Structure

```txt id="jlwm2"
src/
│
├── services/
│   └── user.service.ts
│
├── hooks/
│   └── queries/
│       ├── useUsers.ts
│       └── useUser.ts
│
├── providers/
│   └── QueryProvider.tsx
│
└── lib/
    └── query-client.ts
```

---

# Setup Query Client

# lib/query-client.ts

```ts id="jlwm3"
import { QueryClient } from "@tanstack/react-query";

export const queryClient = new QueryClient({
  defaultOptions: {
    queries: {
      staleTime: 1000 * 60,
      retry: 1,
      refetchOnWindowFocus: false,
    },
  },
});
```

---

# Setup Provider

# providers/QueryProvider.tsx

```tsx id="jlwm4"
"use client";

import { QueryClientProvider } from "@tanstack/react-query";
import { queryClient } from "@/lib/query-client";

export function QueryProvider({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <QueryClientProvider client={queryClient}>
      {children}
    </QueryClientProvider>
  );
}
```

---

# Add Provider to Layout

```tsx id="jlwm5"
<QueryProvider>
  {children}
</QueryProvider>
```

---

# Best Practice 1 — Separate API Logic

Avoid API calls directly inside components.

Bad:

```tsx id="jlwm6"
useEffect(() => {
  fetch("/api/users")
}, [])
```

Good:

```txt id="jlwm7"
services/
hooks/
```

---

# Service Layer Example

# services/user.service.ts

```ts id="jlwm8"
export async function getUsers() {
  const response = await fetch("/api/users");

  if (!response.ok) {
    throw new Error("Failed to fetch users");
  }

  return response.json();
}
```

Benefits:

* reusable API logic
* centralized networking
* cleaner components
* easier testing

---

# Query Hook Example

# hooks/queries/useUsers.ts

```ts id="jlwm9"
import { useQuery } from "@tanstack/react-query";
import { getUsers } from "@/services/user.service";

export function useUsers() {
  return useQuery({
    queryKey: ["users"],
    queryFn: getUsers,
  });
}
```

---

# Component Usage

```tsx id="jlwm10"
const { data, isLoading, error } = useUsers();

if (isLoading) return <Loader />;

if (error) return <ErrorState />;

return <UserTable users={data} />;
```

---

# Best Practice 2 — Use Proper Query Keys

Query keys are critical for caching.

Good:

```ts id="jlwm11"
["users"]
["user", userId]
["projects", projectId]
```

Bad:

```ts id="jlwm12"
["data"]
["info"]
```

Use descriptive and predictable keys.

---

# Best Practice 3 — Centralize Query Keys

Recommended:

```txt id="jlwm13"
constants/query-keys.ts
```

Example:

```ts id="jlwm14"
export const QUERY_KEYS = {
  USERS: "users",
  PROJECTS: "projects",
};
```

Benefits:

* consistency
* easier invalidation
* reduced duplication

---

# Best Practice 4 — Use staleTime Properly

Avoid unnecessary refetching.

Example:

```ts id="jlwm15"
staleTime: 1000 * 60 * 5
```

Meaning:

* data stays fresh for 5 minutes

Good for:

* dashboards
* profile data
* analytics

Avoid overly aggressive refetching.

---

# Best Practice 5 — Handle Loading & Error States

Never assume requests succeed.

Always handle:

* loading
* error
* empty states

Example:

```tsx id="jlwm16"
if (isLoading) return <Loader />;

if (error) return <ErrorState />;

if (!data?.length) return <EmptyState />;
```

---

# Best Practice 6 — Use Mutations for Write Operations

Queries = GET requests

Mutations = POST/PUT/DELETE

Example:

```ts id="jlwm17"
const mutation = useMutation({
  mutationFn: createUser,
});
```

---

# Mutation Example

```ts id="jlwm18"
const createUserMutation = useMutation({
  mutationFn: createUser,

  onSuccess: () => {
    queryClient.invalidateQueries({
      queryKey: ["users"],
    });
  },
});
```

Benefits:

* cache synchronization
* automatic refetching
* cleaner updates

---

# Best Practice 7 — Avoid Overusing Global State

Do NOT store API data in Redux unnecessarily.

Bad:

```txt id="jlwm19"
Redux → API responses
```

Good:

```txt id="jlwm20"
React Query → Server State
Redux/Zustand → UI State
```

Use React Query for:

* API data
* server state
* caching

Use Redux/Zustand for:

* theme
* auth UI
* modals
* sidebar state

---

# Best Practice 8 — Use Prefetching

Prefetch important data.

Example:

```ts id="jlwm21"
queryClient.prefetchQuery({
  queryKey: ["users"],
  queryFn: getUsers,
});
```

Benefits:

* faster navigation
* improved UX
* smoother page transitions

---

# Best Practice 9 — Use Pagination Correctly

Example:

```ts id="jlwm22"
useQuery({
  queryKey: ["users", page],
  queryFn: () => getUsers(page),
});
```

Always include dynamic parameters in query keys.

---

# Best Practice 10 — Avoid Massive Query Hooks

Bad:

```ts id="jlwm23"
useDashboardEverything()
```

Good:

```ts id="jlwm24"
useUsers()
useProjects()
useAnalytics()
```

Keep hooks focused and maintainable.

---

# React Query + Next.js Best Practices

## Prefer Server Components When Possible

Use Server Components for:

* static data
* SEO pages
* server-rendered content

Use React Query for:

* client-side interactions
* dashboards
* dynamic data
* frequently updated data

---

# Hydration Best Practice

Avoid duplicate fetching between:

* Server Components
* Client Components

Plan data ownership carefully.

---

# Common Mistakes

# ❌ Using React Query Everywhere

Not every request needs React Query.

Simple static data may work better with:

* Server Components
* fetch()
* server actions

---

# ❌ Using Generic Query Keys

Bad:

```ts id="jlwm25"
["data"]
```

Use descriptive keys.

---

# ❌ Ignoring Error States

Always handle API failures.

---

# ❌ Refetching Excessively

Avoid unnecessary network requests.

---

# ❌ Storing Server State in Redux

This creates duplicated state management complexity.

---

# Recommended Enterprise Stack

* Next.js
* TypeScript
* TanStack Query
* Zod
* React Hook Form
* Zustand / Redux Toolkit

---

# Enterprise Architecture Flow

```txt id="jlwm26"
Component
   ↓
Query Hook
   ↓
Service Layer
   ↓
API
```

This creates:

* clean separation
* maintainability
* testability
* scalability

---

# Final Advice For Junior Developers

React Query is not just a fetching library.

It is a server-state management solution.

Use it to:

* simplify frontend architecture
* improve performance
* reduce boilerplate
* create scalable applications

Keep query hooks focused.

Separate API logic properly.

Handle loading and error states consistently.

Avoid overengineering.

Build predictable systems that teams can maintain easily.

---

# Key Takeaways

* Separate API logic from components
* Use descriptive query keys
* Handle loading and error states properly
* Use mutations for write operations
* Avoid storing server state in Redux
* Keep query hooks small and focused
* Prefer maintainability over clever abstractions
