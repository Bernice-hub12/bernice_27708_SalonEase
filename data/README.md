# Data folder

Raw data for SalonEase. Files here are kept **untouched**. Cleaned and normalized versions are produced in Phase II, in a separate folder, so the step "raw data → 3NF" can be shown.

## Main dataset

| Item | Detail |
|---|---|
| Name | Hair Salon No-Show Dataset (wrangled version) |
| Platform / author | Kaggle, frederickferguson |
| Link | [paste the Kaggle URL from your browser] |
| Description | Appointment data from a hair salon in Toronto, March to July 2018, shared with the owner's permission (salon name withheld), according to the dataset description. Each row is one booking with details of the client's previous visit and whether the client did not show up. |
| File in this folder | `hair_salon_no_show_wrangled_df.csv` |
| Rows | 1,952 (bookings) |
| Columns | 22 (the first column is an unnamed row index) |
| No-shows | 224 of 1,952 bookings (11.5%) |
| Licence | [read it on the Kaggle page and paste it here; it must allow educational use] |
| Date downloaded | [date] |

## Column list

| Column | Meaning | Values |
|---|---|---|
| (unnamed first column) | Row index | 0 to 1951 |
| `book_tod` | Time of day of the booking | morning, afternoon, evening (235 blank) |
| `book_dow` | Day of week of the booking | Monday to Sunday |
| `book_category` | Category of the booked service | STYLE, COLOR, MISC |
| `book_staff` | Stylist booked | 7 values (6 stylists and "HOUSE") |
| `last_category` | Service category of the client's previous visit | STYLE, COLOR, MISC (949 blank = no previous visit) |
| `last_staff` | Stylist at the previous visit | as above (949 blank) |
| `last_day_services` | Number of services at the previous visit | 0 to 3 |
| `last_receipt_tot` | Amount paid at the previous visit | 5 to 382.8 (0 = none) |
| `last_dow` | Day of week of the previous visit | blank when no previous visit |
| `last_tod` | Time of day of the previous visit | blank when unknown |
| `last_noshow` | Previous booking was a no-show | 0 or 1 |
| `last_prod_flag` | Client bought a product at the previous visit | 0 or 1 |
| `last_cumrev` | Total revenue from the client so far | number |
| `last_cumbook` | Total bookings by the client so far | number |
| `last_cumstyle` | Total style services so far | number |
| `last_cumcolor` | Total color services so far | number |
| `last_cumprod` | Total product purchases so far | number |
| `last_cumcancel` | Total cancellations so far | number |
| `last_cumnoshow` | Total no-shows so far | number |
| `noshow` | **This booking was a no-show** | 0 or 1 |
| `recency` | Days since the client's previous visit | 0 when no previous visit |

(Column meanings are inferred from the column names and the data values. Confirm them against the Kaggle description.)

## Privacy check (done on this file)

- [x] No client names, phone numbers or emails in the file
- [x] No client ID and no dates (only day of week and time of day)
- [ ] Stylist first names appear in `book_staff` and `last_staff` (JJ, JOANNE, KELLY, BECKY, SINEAD, TANYA, plus "HOUSE"). In Phase II these will be replaced with codes (Stylist 1, Stylist 2, ...) in the STYLIST table. Slides use the codes.
- [ ] Licence allows educational use: **you must confirm on Kaggle**

## What the data gives and what it lacks (for the table design)

| Planned table | From the dataset | Not in the dataset (to be added) |
|---|---|---|
| `STYLIST` | The 7 stylist values (masked as codes) | Contact details, schedules |
| `SERVICE` | 3 categories (STYLE, COLOR, MISC) and price points from `last_receipt_tot` | Service names and prices: from the Kigali survey |
| `APPOINTMENT` | Day of week, time of day, category, stylist, no-show flag | Real dates, appointment ID, status values: generated |
| `CLIENT` | Previous-visit and history columns (recency, cumulative bookings and revenue) | There is no client ID, so clients are generated and linked to bookings |
| `PAYMENT` | `last_receipt_tot` gives realistic amounts | Payment method and date: generated |
| `STYLIST_SCHEDULE`, `AUDIT_LOG` | None | Generated or created by triggers |

## Supplementary data (clearly labelled, not from Kaggle)

| Data | How it is obtained | Used for |
|---|---|---|
| Services and prices in Kigali | A simple survey of one or more local salons (price list for hair, braiding, makeup and nails) | `SERVICE` table |
| Real dates, clients, payments | Generated sample records, labelled as generated | `CLIENT`, `APPOINTMENT`, `PAYMENT` |

## Limitations

- The data comes from a Toronto salon, not Rwanda, so prices, services and customer habits differ. The Kigali survey is used to localise services and prices.
- The file is a processed (wrangled) dataset: it holds engineered history columns for each booking and no raw appointment dates, so some tables must be built from generated data.
- Many rows (949 of 1,952) are first-time bookings with no previous visit, so the history columns are empty or zero for them.
