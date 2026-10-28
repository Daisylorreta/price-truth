# PriceTruth — Product Requirements Document

1. **Product:** PriceTruth is a global website that helps people find fair prices for everyday products and services.

2. **Problem:** Buyers may be overcharged because prices change between sellers, markets, regions, and countries.

3. **Solution:** PriceTruth uses reports from real people to show what items cost in a specific location.

4. **Target users:** Shoppers, newcomers, travelers, students, researchers, consumer groups, and honest sellers.

5. **Location selection:** Users choose a country, region or state, and market. Prices and currency update automatically.

6. **Price search:** Users search for products and services such as rice, fish, tomatoes, cement, fuel, and transport.

7. **Price information:** Results show the fair price range, typical price, recent reports, market, date, and price trends.

8. **Price reporting:** Users can submit an item, price, market, date, and optional receipt image.

9. **AI assistant:** PriceTruth AI answers questions such as “Is this price fair?” using available community price data.

10. **Technology and future:** The prototype uses HTML, CSS, JavaScript, LocalStorage, SVG images, and Visual Studio Code. Future versions will add a real database, user accounts, live prices, receipt scanning, moderation, and mobile apps.

## MP-6 / MP-7 location integrity acceptance criteria

- The active price context is always the tuple **country + region/state + market + item + quantity/unit**.
- A community report is eligible only when its country, region/state, market, item and measurement match the active query.
- Changing state or market must invalidate the previous location-specific result and recalculate the board.
- Changing country must invalidate the previous country-specific result, load the new country's region/market context, and display the new country's currency.
- A previous country's price must never be carried into a new country merely by changing the currency symbol.
- When no exact community reports exist, the UI may show an **Indicative local estimate**, but it must not present that estimate as a community report.
- The results page must contain no hard-coded country-specific price values before the location-aware application state is loaded.
- User-submitted reports must persist the canonical country key, state/region, market and currency so they can be matched to the correct location later.
