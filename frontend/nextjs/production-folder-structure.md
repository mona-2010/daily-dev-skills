# Next.js Production Folder Structure Guide

A practical guide to structuring scalable Next.js applications for maintainability, developer experience, and enterprise-grade production systems.

---

# Why Folder Structure Matters

As applications grow, poor project organization quickly becomes difficult to maintain.

A scalable folder structure helps teams:

* onboard developers faster
* reduce code duplication
* improve maintainability
* isolate features cleanly
* scale frontend architecture safely
* improve debugging and testing

Enterprise applications prioritize:

* feature isolation
* separation of concerns
* reusable architecture
* predictable code organization

---

# Recommended Production Structure

```txt id="4u1d3m"
src/
│
├── app/
│   ├── (auth)/
│   ├── dashboard/
│   ├── api/
│   ├── layout.tsx
│   └── page.tsx
│
├── components/
│   ├── ui/
│   ├── shared/
│   ├── forms/
│   └── layouts/
│
├── features/
│   ├── auth/
│   ├── users/
│   ├── onboarding/
│   └── dashboard/
│
├── hooks/
│
├── services/
│
├── store/
│
├── lib/
│
├── utils/
│
├── types/
│
├── constants/
│
├── validation/
│
├── providers/
│
├── middleware/
│
├── config/
│
└── tests/
```

---

# Folder Breakdown

# 1. app/

Contains all Next.js App Router routes.

Example:

```txt id="st9q8w"
app/
├── dashboard/
├── users/
├── settings/
└── api/
```

Use this folder ONLY for:

* routing
* layouts
* server components
* route handlers
* loading/error pages

Avoid placing business logic directly inside routes.

---

# 2. components/

Reusable UI components shared across the application.

Structure:

```txt id="4x9bmr"
components/
├── ui/
├── shared/
├── forms/
└── layouts/
```

## ui/

Contains reusable design-system primitives.

Examples:

```txt id="lm1vqe"
Button.tsx
Input.tsx
Dialog.tsx
Table.tsx
```

## shared/

Shared application-level components.

Examples:

```txt id="7flfbp"
Navbar.tsx
Sidebar.tsx
EmptyState.tsx
Loader.tsx
```

## forms/

Reusable form components.

Examples:

```txt id="v4y0n9"
UserForm.tsx
AddressForm.tsx
```

---

# 3. features/

One of the most important enterprise patterns.

Features isolate domain-specific logic.

Example:

```txt id="2jlwm9"
features/
├── auth/
├── onboarding/
├── dashboard/
└── users/
```

Each feature can contain:

```txt id="v2fqph"
auth/
├── components/
├── hooks/
├── services/
├── validation/
├── types/
└── utils/
```

Benefits:

* modular architecture
* better scalability
* easier refactoring
* improved ownership
* cleaner imports

---

# 4. hooks/

Contains reusable custom React hooks.

Examples:

```txt id="7f8m8m"
useAuth.ts
useDebounce.ts
usePagination.ts
```

Rules:

* hooks should be reusable
* avoid feature-specific logic here
* feature-specific hooks belong inside features/

---

# 5. services/

Contains API calls and backend communication.

Bad Practice:

```tsx id="0w2z0k"
useEffect(() => {
  fetch("/api/users")
}, [])
```

Good Practice:

```txt id="8vjlwm"
services/
└── user.service.ts
```

Example:

```ts id="0q2gcg"
export async function getUsers() {
  return api.get("/users");
}
```

Benefits:

* reusable API logic
* centralized networking
* easier testing
* cleaner UI components

---

# 6. store/

Global state management.

Use ONLY for global state:

* auth state
* theme state
* sidebar state
* onboarding progress

Avoid storing temporary component state globally.

Recommended tools:

* Redux Toolkit
* Zustand

---

# 7. lib/

Third-party library configurations.

Examples:

```txt id="7pn5tz"
supabase.ts
axios.ts
query-client.ts
logger.ts
```

---

# 8. utils/

Pure helper functions.

Examples:

```txt id="5xbjje"
formatCurrency.ts
generateSlug.ts
dateFormatter.ts
```

Rules:

* keep functions pure
* avoid side effects
* avoid API calls

---

# 9. types/

Shared TypeScript types.

Examples:

```txt id="drjlwm"
user.types.ts
api.types.ts
auth.types.ts
```

Benefits:

* type consistency
* reusable models
* reduced duplication

---

# 10. validation/

Validation schemas.

Recommended:

* Zod

Example:

```ts id="y1eqbl"
const loginSchema = z.object({
  email: z.string().email(),
  password: z.string().min(8),
});
```

Never place validation logic directly inside components.

---

# 11. providers/

Application-level providers.

Examples:

```txt id="vjlwm1"
ThemeProvider.tsx
QueryProvider.tsx
ReduxProvider.tsx
```

---

# 12. config/

Environment variables and application configuration.

Examples:

```txt id="tdlq1n"
env.ts
site.ts
api.ts
```

Never access environment variables directly everywhere.

Centralize them.

---

# Enterprise Best Practices

## 1. Prefer Feature-Based Architecture

Bad:

```txt id="ysj6o9"
components/
pages/
hooks/
utils/
```

for everything.

Good:

```txt id="2ecjlwm"
features/
  auth/
  dashboard/
  users/
```

Large applications scale much better with feature isolation.

---

# 2. Keep Components Small

Avoid massive files handling:

* fetching
* tables
* forms
* modals
* validation
* permissions

Split responsibilities into focused components.

---

# 3. Separate Business Logic

Avoid business logic inside JSX.

Bad:

```tsx id="ijvvck"
<button onClick={() => {
  // huge logic
}}>
```

Good:

```tsx id="dqjlwm"
services/
hooks/
utils/
```

---

# 4. Use Absolute Imports

Good:

```ts id="jlwm4"
@/components/ui/button
```

Bad:

```ts id="jlwm5"
../../../../components/button
```

Configure:

```json id="jlwm6"
"paths": {
  "@/*": ["./src/*"]
}
```

---

# 5. Avoid Overengineering

Junior developers often:

* create unnecessary abstractions
* overuse context
* overuse global state
* create deeply nested structures

Keep architecture practical.

---

# Common Mistakes

## ❌ Storing everything inside components/

This becomes impossible to maintain.

---

## ❌ Huge page files

Bad:

```txt id="jlwm7"
DashboardPage.tsx → 1200 lines
```

Split logic into:

* components
* hooks
* services
* feature modules

---

## ❌ API calls inside UI

Always separate networking logic.

---

## ❌ Duplicated types

Centralize shared models.

---

## ❌ Random folder naming

Be consistent.

Good:

```txt id="jlwm8"
components/
services/
validation/
```

Bad:

```txt id="jlwm9"
helpers/
misc/
stuff/
random/
```

---

# Example Enterprise Flow

```txt id="jlwm10"
Feature Request
    ↓
Route
    ↓
Feature Module
    ↓
Hook
    ↓
Service
    ↓
API
```

This creates clean separation of concerns.

---

# Recommended Stack

## Frontend

* Next.js
* TypeScript
* Tailwind CSS
* Shadcn UI

## State

* Redux Toolkit
* Zustand

## Forms

* React Hook Form
* Zod

## Fetching

* TanStack Query
* Server Components

---

# Final Advice For Junior Developers

Good architecture is not about creating the most folders.

Good architecture is about:

* predictability
* maintainability
* scalability
* developer experience

Start simple.

Scale gradually.

Refactor incrementally.

Avoid premature optimization.

Focus on writing understandable and maintainable code.

---

# Key Takeaways

* Use feature-based architecture for scalability
* Separate UI, business logic, and API layers
* Keep components small and reusable
* Centralize types, validation, and services
* Prefer maintainability over clever abstractions
* Build systems that teams can scale confidently
