---
name: scaffold-route
description: Generate a typesafe Next.js App Router route with handler, Zod schema, and a basic test. Use when the user asks to "add an endpoint", "scaffold a route", or "create an API for X".
---

# Scaffold Route

Add a new typesafe API endpoint to a Next.js (App Router) project in one shot, with input validation and a basic test.

## When to use

- User says "add an endpoint", "create an API route", "scaffold POST /api/foo".
- The project is Next.js with the App Router (look for `app/` directory).

## Steps

1. **Locate the project root.** Confirm there's an `app/` directory and that `next` is in `package.json`.

2. **Confirm the route shape with the user** if it's ambiguous:
   - Method (GET / POST / PUT / DELETE)?
   - Path?
   - Inputs and outputs?

3. **Create three files:**

   - `app/api/<path>/route.ts` — the handler.
   - `app/api/<path>/schema.ts` — the Zod input/output schemas.
   - `app/api/<path>/route.test.ts` — one happy-path test, one validation-error test.

4. **Use this handler shape** (App Router conventions):

   ```ts
   import { NextResponse } from "next/server";
   import { InputSchema } from "./schema";

   export async function POST(req: Request) {
     const parsed = InputSchema.safeParse(await req.json());
     if (!parsed.success) {
       return NextResponse.json(
         { error: parsed.error.flatten() },
         { status: 400 },
       );
     }

     // ... do the thing ...

     return NextResponse.json({ ok: true });
   }
   ```

5. **Use this schema shape:**

   ```ts
   import { z } from "zod";

   export const InputSchema = z.object({
     // ...
   });
   export type Input = z.infer<typeof InputSchema>;
   ```

6. **Run the type-checker** (`pnpm tsc --noEmit` or `npm run type-check`) and fix any errors before handing off.

## Don't

- Don't reach for a database client unless the user's project already has one.
- Don't add auth middleware unless the user mentions auth.
- Don't generate OpenAPI / Swagger docs unless asked.
