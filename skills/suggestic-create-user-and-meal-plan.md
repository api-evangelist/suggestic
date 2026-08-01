---
name: Onboard a user and generate a meal plan
description: Create a Suggestic user, assign a nutrition program, generate their meal plan, and read it back.
api: https://production.suggestic.com/graphql
method: generated
source: https://docs.suggestic.com/graphql/start-here/tutorials-and-walkthroughs/create-a-meal-plan
operations: [createUser, setProgram, generateMealPlan, mealPlan]
---

# Onboard a user and generate a meal plan

Use the Suggestic GraphQL API to onboard an end user and produce a personalized meal plan.

## Auth (server-side)
Send every request to `https://production.suggestic.com/graphql` as HTTP POST with:
- `Authorization: Token <your_api_token>`
- `sg-user: <user_uuid>` — the user you act on behalf of (omit for `createUser`).
- `Content-Type: application/json`

Never expose the API token client-side. For client apps, use a Bearer JWT from `/api/v1/login` instead.

## Steps
1. **createUser** — create the user (no `sg-user` needed). Capture the returned user `id`; use it as `sg-user` for all subsequent calls.
2. **setProgram** — assign the user's nutrition program (`setProgram`) so plan generation has diet + ruleset context.
3. **generateMealPlan** — generate the plan for the user's profile/program. (Use `generateSimpleMealPlan` for a caloric-goal/macro-ratio custom plan.)
4. **mealPlan** — read back the 7-day plan: iterate `mealPlanDay -> meals -> recipe`.
5. Optional: **swapMeals** to replace a recipe, then **shoppingList** to get the aggregated shopping list.

## Conventions
- Pagination: Relay-style `edges { node { ... } }` with `first`/`after` cursors.
- Errors: check the top-level GraphQL `errors[]` array (HTTP is 200 even on errors). Meal-plan generation can fail when the profile/program is incomplete or over-restricted — see `mealPlanConfig` to debug.
- No idempotency key is supported; do not blind-retry mutations.
