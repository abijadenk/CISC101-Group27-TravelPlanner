### **Module 1 — Intake & Setup**

Collect and normalize essential trip details into a structured JSON format for downstream modules. Ensure clarity, correctness, and robustness by handling edge cases gracefully.

Required Inputs
Destination(s)

Dates or trip length

Number of travelers

Budget style (affordable, mid-range, luxury)

Interests (food, culture, nature, etc.)

Preferred pace (relaxed, balanced, fast)

Key constraints (mobility, weather, diet)

Normalization Rules
Dates:

Parse exact dates into ISO 8601.

If only length_days provided → store length, set season = "unknown".

If vague (e.g., “early June”), infer placeholder and prompt for confirmation.

Destinations:

Store canonical city/region/country.

Preserve order for multi-stop trips.

If ambiguous (e.g., “Paris”), prompt for disambiguation (France vs. Texas).

Budget:

Map style to enum: affordable / mid-range / luxury.

If numeric budget ≤ 0 → convert to "affordable", set numeric fields = null, and prompt user to confirm constraints.

Interests:

Map to controlled tags (food, culture, nature, etc.).

Allow custom tags.

Pace:

Enum: relaxed / balanced / fast.

Attach heuristic for activities per day (2/3/4).

Constraints:

Structured flags: mobility, diet, accessibility, weather tolerance.

Arrival times stored if provided.

Edge Case Handling
Negative or zero budget:

Convert to "affordable".

Set numeric budget = null.

Add follow-up prompt: “Your budget was zero/negative. Should we plan in affordable style with constraints noted?”

Missing dates:

Store length_days if available.

Set season = "unknown".

Prompt: “Could you share at least the month or climate preference?”

Multi-stop with overlaps:

Preserve order of destinations.

Note potential travel time conflicts in data_quality.conflicts.

Defer resolution to planning module.

Extreme weather seasons:

If trip falls in monsoon/heatwave/winter storm season, tag season_risk for downstream adjustments.

Ambiguous destinations:

Prompt for clarification: “Which country or nearest major city is your destination? (e.g., Paris, France vs. Paris, Texas)”
