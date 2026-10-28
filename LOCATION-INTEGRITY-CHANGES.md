# Price Truth location-integrity patch

## Changes

- Price results are matched against country, region/state, market, and normalized item.
- A location with no exact community reports now displays **Price data unavailable** instead of generating a plausible-looking price from seed values.
- Result currency is formatted from the currently selected country; switching country/state/market reruns the active item result through `applyCountry()`.
- The persisted active-query context is invalidated when its location no longer matches the active selection.
- The chatbot now explicitly says it has no report-backed price rather than quoting a synthetic estimate.

## Regression acceptance checks

1. Search rice in Nigeria / Abia / Ariaria International Market. Only reports matching Nigeria + Abia + that market + rice may be displayed.
2. Switch to Nigeria / Lagos / Mile 12 Market while keeping the rice query. The result must rerender for Lagos and must not show Abia reports.
3. Switch to United Kingdom / England / Borough Market while keeping the rice query. The result must rerender in GBP and must not show NGN reports.
4. If the selected location has no matching community reports, the result says `Price data unavailable`, report count is zero, and no generated price range is displayed.
5. Switch back to a location with matching reports. Only that location's eligible reports appear.

## Important limitation

This patch makes result pages fail closed when exact community reports are absent. The home/browse boards still contain prototype seed figures and should be labelled as illustrative or replaced with real sourced data in the next data-layer pass. Country/region/market lists in this MVP are curated sample lists, not a complete authoritative geography dataset.
