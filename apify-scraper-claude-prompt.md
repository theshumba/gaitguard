# Claude prompt — Apify Google Maps Scraper for UK equine leads

A detailed, copy-paste-ready prompt to give to any Claude-powered browser-control tool
(Browser Use, Anthropic Computer Use, Operator, browser-agent Chrome extensions, etc.)
that drives the Apify Google Maps Scraper actor at
https://console.apify.com/actors/nwua9Gu5YrADL7ZDj/input
(actor ID: `compass/crawler-google-places`).

---

## The prompt

```
You are an autonomous browser agent. Your job is to fully configure and launch the
Apify "Google Maps Scraper" actor (compass/crawler-google-places) to extract 2,000–3,000
UK equine business leads for GaitGuard, a UK equine monitoring startup.

CRITICAL PRE-FLIGHT
- Confirm the user is logged into Apify Console at https://console.apify.com.
  If not, stop and ask them to log in.
- Navigate to https://console.apify.com/actors/nwua9Gu5YrADL7ZDj/input
- Verify the page header reads "Google Maps Scraper" by compass/crawler-google-places.
  If a different actor is loaded, stop and ask the user.
- The actor charges $4 per 1,000 scraped places at base rate. Add-ons cost extra.
  This run is expected to cost roughly $50–120 USD depending on enrichment depth.
  Confirm the user is happy with that ballpark before clicking Save & Start. Do NOT
  start the run without an explicit "yes go" from the user.

CONTEXT
GaitGuard sells AI-powered equine monitoring (a camera that watches every stride at the
horse walker). The ICP, in priority order:
1. Mid-to-high-end livery yards (full / part / schooling) catering to non-horsey owners
2. DIY livery yards
3. Training yards (eventing, dressage, showjumping)
4. Stud farms and breeders
5. Riding schools and equestrian centres
6. Racing yards
We want broad UK coverage — England, Wales, Scotland, equal weight — not just South East.

THE FASTEST PATH (DO THIS FIRST)
The Apify input page has two tabs near the top: "Form" and "JSON". Click "JSON".
Replace the entire contents of the JSON editor with the configuration below, then
click "Save". This is faster and more reliable than form-filling each field.

CONFIGURATION JSON (paste exactly):

{
  "searchStringsArray": [
    "livery yard",
    "DIY livery",
    "full livery yard",
    "part livery",
    "horse stables",
    "equestrian centre",
    "riding school",
    "stud farm",
    "horse training yard",
    "dressage yard",
    "showjumping yard",
    "eventing yard",
    "racehorse trainer",
    "polo yard",
    "horse riding centre"
  ],
  "locationQuery": "United Kingdom",
  "maxCrawledPlacesPerSearch": 200,
  "language": "en",
  "countryCode": "gb",
  "skipClosedPlaces": true,
  "scrapePlaceDetailPage": true,
  "scrapeContacts": true,
  "scrapeDirectories": false,
  "scrapeReviewsCount": 5,
  "maxReviews": 5,
  "reviewsSort": "newest",
  "reviewsOrigin": "all",
  "reviewsStartDate": "",
  "scrapeReviewsPersonalData": true,
  "scrapeImages": false,
  "maxImages": 0,
  "exportPlaceUrls": false,
  "includeWebResults": false,
  "deeperCityScrape": true,
  "searchMatching": "all",
  "placeMinimumStars": "",
  "website": "allPlaces",
  "allPlacesNoSearchAction": ""
}

REASONING FOR EACH KEY FIELD (so you can adjust if any field name has changed):
- searchStringsArray (15 queries): each search term runs as a separate sub-job. With
  200 places per term × 15 terms = up to 3,000 leads. Terms span every yard type in
  the ICP so we don't miss any segment.
- locationQuery: "United Kingdom" with countryCode "gb" gives nationwide coverage.
- maxCrawledPlacesPerSearch: 200 keeps total under ~3,000 (cost-controlled). Bump to
  300 if the user wants more, or drop to 100 for a smaller test run first.
- language: "en" for English-language listings.
- countryCode: "gb" filters to UK results.
- skipClosedPlaces: true so we don't waste budget on permanently-closed yards.
- scrapePlaceDetailPage: true enables full details (opening hours, full address,
  category, etc.) — necessary for high-quality leads.
- scrapeContacts: true triggers the company contacts add-on, which surfaces emails
  and phone numbers from the yard's website. This is the highest-value add-on for B2B.
- scrapeDirectories: false (avoids scraping directory aggregator pages — we want
  individual yards).
- maxReviews: 5 with reviewsSort "newest" — pulls 5 recent reviews per place to
  give us social-proof signal (recent activity, complaints, decision-maker names
  sometimes appear in reviews). Drop to 0 if cost-sensitive.
- scrapeReviewsPersonalData: true — names of reviewers can sometimes be the yard
  owner / manager replying to reviews, which is a free decision-maker signal.
- scrapeImages: false — images cost extra and aren't useful for B2B prospecting.
- deeperCityScrape: true — encourages the actor to expand beyond initial result page
  to find more yards in each region.
- searchMatching: "all" — broadest match for each search term.

FORM-FILLING FALLBACK (only if JSON tab approach fails)

If the JSON tab is missing or shows a parse error, fall back to filling the Form tab.
Walk through each section in this exact order:

1. Search term(s)
   - Click the "+ Add" button.
   - Add each of these as a separate row, one per click:
     livery yard
     DIY livery
     full livery yard
     part livery
     horse stables
     equestrian centre
     riding school
     stud farm
     horse training yard
     dressage yard
     showjumping yard
     eventing yard
     racehorse trainer
     polo yard
     horse riding centre
   - If "Bulk edit" is available, click it and paste the 15 lines, one per line.

2. Location (use only one location per run)
   - Clear the "New York, USA" placeholder and type: United Kingdom

3. Number of places to extract (per each search term or URL)
   - Set to: 200

4. Language
   - Confirm "English" is selected (it should already be).

5. 🔍 Add-on: Search filters & categories
   - Expand the section.
   - Leave categoryFilterWords empty (we cover the categories with our search terms).
   - Leave placeMinimumStars empty.
   - Set "searchMatching" to "all" if a dropdown is present.
   - Set "skipClosedPlaces" toggle to ON.

6. 📌 Add-on: Additional place details scraping
   - Expand. Toggle ON "scrapePlaceDetailPage" / "Place detail page".
   - Toggle OFF "scrapeDirectories".
   - Toggle ON "deeperCityScrape" if present.

7. 🏢 Add-on: Company contacts enrichment
   - Expand. Toggle ON "scrapeContacts" / "Scrape contacts from website".
   - This is the most valuable add-on — it pulls emails, social links, sometimes
     decision-maker names from each yard's website. Worth the extra cost.

8. 👥 Add-on: Business leads enrichment
   - Expand. Read the cost note carefully — this typically searches LinkedIn for
     decision-makers and is the most expensive add-on. For a 3,000-lead run this
     can add $100+ to the bill. RECOMMENDATION: leave this OFF for the first run.
     If the user explicitly wants LinkedIn-sourced decision-maker enrichment, ask
     them to confirm the cost increase before toggling on.

9. ⭐ Add-on: Reviews
   - Expand. Set:
     - maxReviews: 5
     - reviewsSort: newest
     - reviewsOrigin: all
     - reviewsStartDate: leave blank
     - scrapeReviewsPersonalData: ON

10. 🖼️ Add-on: Images
    - Expand. Set maxImages: 0. Toggle scrapeImages OFF.
    - Images add cost without business value for B2B prospecting.

11. 🧭 Define the search area by other geolocation parameters*
    - Leave collapsed / blank. We're using locationQuery instead.

12. 🔗 Scrape with Google Maps URLs or place IDs*
    - Leave collapsed / blank.

13. 🧭 Scraping places without search terms or URLs*
    - Leave collapsed / blank.

14. Run options
    - Default memory (4 GB) and timeout (604,800s) are fine — no change needed.

15. Final review before launch
    - Scroll to the top. Confirm Search term(s) shows 15 entries.
    - Confirm Location reads "United Kingdom".
    - Confirm Number of places reads 200.
    - Confirm scrapeContacts is ON.
    - Confirm scrapeImages is OFF (cost saver).

16. Launch
    - Click "Save" first to persist the configuration as a saved task.
    - Show the user the final config one more time and ask: "Ready to launch? Expected
      cost is roughly $50–120 USD. Type 'go' to start."
    - Only on explicit "go", click "Save & Start".

POST-LAUNCH
- Note the run ID and dataset ID from the URL after launch.
- Tell the user: "Run started. ID: {runId}. Watch progress at the Runs tab. Expected
  duration: 30–90 minutes. Results will be available as CSV/JSON download from the
  Storage tab when finished."
- Suggest: "Once the run finishes, download as CSV and we can convert it to GaitGuard
  CRM legacy-import format using the same converter we used for the Origami CSV."

ERROR HANDLING
- If any field name in the JSON is rejected as "unknown property", do not invent a
  fallback name — leave the property out and warn the user. Apify rejects unknown
  fields silently otherwise.
- If "Save" fails with a validation error, surface the exact error text to the user
  before retrying.
- If the run cost preview shows above $200, STOP and warn the user before launching.

DO NOT
- Do not click any "Buy credits" or "Upgrade plan" button without explicit permission.
- Do not modify any other actor or saved task on the user's account.
- Do not enable the LinkedIn business-leads enrichment without explicit cost approval.
- Do not run multiple parallel jobs — one is enough for this volume.
```

---

## After the run finishes

When the Apify run completes, download the dataset as CSV from the Storage tab. Save it
locally and we can convert it to the GaitGuard CRM legacy-import format with the same
Python converter we used for the Origami CSV — just remap the column names:

| Apify CSV column            | Legacy GaitGuard field |
|-----------------------------|------------------------|
| `title`                     | `yard`                 |
| `categoryName`              | `category`             |
| `phone`                     | `phone`                |
| `email`                     | `email`                |
| `website`                   | `website`              |
| `address`                   | `location`             |
| `totalScore` + `reviewsCount` | `notes` (packed)     |
| `placeId`                   | `id`                   |

The decision-maker name often appears in `ownerUpdates` or in the most recent reply-to-review.
We can extract those automatically.
