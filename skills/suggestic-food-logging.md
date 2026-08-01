---
name: Log food and read nutrition totals
description: Search foods/recipes, add food-log entries for a user, and read aggregated macro/micronutrient totals.
api: https://production.suggestic.com/graphql
method: generated
source: https://docs.suggestic.com/graphql/start-here/tutorials-and-walkthroughs/food-log-guide
operations: [recipeSearch, addFoodLog, macrosLogAggregation, foodLogEntries]
---

# Log food and read nutrition totals

Use the Suggestic GraphQL API to record what a user eats and read their nutrition totals.

## Auth (server-side)
POST to `https://production.suggestic.com/graphql` with:
- `Authorization: Token <your_api_token>`
- `sg-user: <user_uuid>` — required (food logs are per-user).
- `Content-Type: application/json`

## Steps
1. **Find the item** — search for a food or recipe:
   - `recipeSearch` / `recipes` for recipes (by name, ingredient, or filters), or
   - the food search queries (branded/common foods) for the food-log database.
2. **addFoodLog** — add the food-log entry (food or recipe, meal type, servings, date). For photo-based logging use the AI food-log flow (`processAiFood` then `saveAiFoodLog`).
3. **foodLogEntries** — read back the user's entries for a date range.
4. **macrosLogAggregation** — read aggregated macronutrient sums for the date range (use the micronutrients aggregation query for micros).

## Conventions
- Pagination: Relay `edges { node }` with `first`/`after`.
- Prefer the current mutations over legacy `createOwnRecipe`/`removeOwnMeal` (deprecated — use `createMyRecipe`/`deleteUserRecipe`).
- Errors surface in the GraphQL `errors[]` array (HTTP 200).
