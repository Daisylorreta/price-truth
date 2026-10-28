# Price Truth — Location-aware price architecture

## Why this exists

The previous prototype could keep a result generated for one location after the user changed state or country. The price display was derived mostly from the item name, while country/state/market selection lived separately in local storage.

## Canonical location context

Every active price calculation now resolves a context with:

```text
countryKey
country
currency
currencySymbol
stateIndex
state
market
```

The market selection is persisted by **country + state**, not just country. This prevents a market selected in one Nigerian state from becoming the market for another state.

## Canonical price lookup

A price result is resolved from:

```text
country + state/region + market + item
```

Community reports must match all of those dimensions. A report from Abia cannot satisfy a Lagos query, and a Nigerian report cannot satisfy a United Kingdom query.

## Result types

### Community result

Used only when one or more matching reports exist. The range and typical price are calculated from those local reports.

### Indicative estimate

Used when there are no exact reports. It is derived from the prototype's seed benchmark and a deterministic location adjustment, then converted into the selected country's currency. The UI labels it as an estimate and does not call it a community report.

### No-data behaviour

The report list explicitly tells the user when there are no matching community reports and encourages the first report for that exact market.

## Location changes

Changing country, state, or market calls the same location-aware render path. On the results page this recalculates the active item immediately, so a search for `rice` remains the search for rice while its price context changes with the selected location.

## Persistence

The application keeps the selected country and state in local storage and now stores markets by a composite key such as:

```text
NG:2 -> Ariaria International Market
GB:0 -> Borough Market
```

This is intentionally separate from the community report store.

## QA scenarios

1. Search `rice` in Nigeria → Abia → Ariaria International Market.
2. Change to Nigeria → Lagos → Mile 12 Market.
   - The state/market context must change.
   - The price must be recalculated for Lagos.
3. Change to United Kingdom → England → Borough Market.
   - The result must switch to GBP.
   - Nigerian naira values and Nigerian reports must disappear from the result.
4. Add a report for UK/England/Borough Market/rice.
   - It must appear only for that exact context.
5. Switch back to Nigeria/Abia.
   - The UK report must not appear.
