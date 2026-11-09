# Project data

The project uses one file: GSA labor ceiling rates for the Multiple Award Schedule (MAS).

| | |
| --- | --- |
| Provider | U.S. General Services Administration (GSA) |
| Source URL | https://buy.gsa.gov/pricing/qr/mas?page=1&page_size=20&histogram=12&experience_range=0,45&price_range=15,500&ordering=current_price&acceptsDataDisclosure=true&sort=asc |
| Date accessed | 2026-11-02 |
| Method of access | Queried the GSA CALC+ ceiling rates API with the filters shown in the source URL: current price $15–$500 and minimum experience 0–45 years, sorted by current price (ascending). Results were saved as gzipped CSV. |
| File | `gsa_rates_2026-11-02.csv.gz` (gzipped CSV, 351,334 rows, 23 columns) |

## Known limits

- **Rates run from $20.10 to $500.00.** No current rate is above $500, but 92 next-year rates are, so the
  file was filtered on current rate. The cap comes from the query's price filter ($15–$500), not from
  the source.
- **These are ceiling rates.** They're the most a vendor may charge, not what is actually billed.
- **Every row is on the MAS schedule.**

## Recreating the analytic data

1. Download the GSA ceiling rates as described above and save the file as
   `data/gsa_rates_2026-11-02.csv.gz`. Don't unzip it.
2. Run `notebook.ipynb` from the `project_template/` folder, top to bottom. The "Prepare data" cell
   builds the analytic table `rates` (327,634 rows):
   - removes repeated listings, since the same rate appears once per SIN;
   - cleans titles and vendor names, groups education levels, and builds the features;
   - adds `log_price`, the model target.
