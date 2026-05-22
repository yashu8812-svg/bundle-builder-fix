## What I picked
Bundle Builder page at /pages/bundle-builder was showing an empty state with "No products found in the selected collections" despite the theme having all the necessary CSS, JS, and structure in place.

## Why it's the highest-impact thing here
1. **Conversion impact**: The Bundle Builder is a key conversion feature for bundling multiple knives with tiered discounts (10% at 3 items, 15% at 5 items). A dead page means lost revenue on a high-intent purchase path.
2. **Visibility**: It's linked from the main navigation and homepage slider, so visitors expect it to work.
3. **Low-hanging fruit**: The fix is a simple fallback logic change—no new features needed, just making the existing UI functional.

## What I did
The root cause: The Bundle Builder section iterates over series collections (configured in the theme JSON) to display products. However, in the seed data, the series collections (xinzuo-mo-series-knives, hezhen-supreme-series-knives, etc.) have no products assigned to them—product data exists but wasn't linked to collections during seeding.

**The fix**: Added fallback logic in `sections/bundle-builder.liquid`:
1. First check if series collections have products (original behavior)
2. If not, fall back to the `all-products` collection which contains all 237 products
3. When using the fallback, determine series membership by matching product handles against known series keywords (e.g., "mo-series", "supreme", "retro" in the product handle string)
4. Products are still deduplicated by ID to prevent duplicates

## What I'd do next
1. Fix the seed script to properly link products to their series collections during setup
2. Add a loading state/shimmer effect while products are being determined
3. Consider adding product filtering by knife type (Chef, Santoku, etc.) for better UX
4. Test the add-to-cart flow with actual discount code application