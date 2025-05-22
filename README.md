# GSC Manual Backfill

This repository provides tools for manually backfilling Google Search Console (GSC) data when there is a delay or failure in data retrieval from Google's side.

## 📘 Overview

In cases where GSC data ingestion is delayed or incomplete, you can use our internal backfill endpoint to manually re-ingest the missing data for one or more sites.

## 📂 Included Files

- **`gsc_backfill_all_sites.ipynb`**  
  A Jupyter notebook that demonstrates how to use the GSC backfill API to trigger a backfill for all sites (or a selected subset). This is intended to be run locally.

## 🔐 Requirements

### 1. `AUTH_TOKEN`

An authorization token is required to access the backfill endpoint.

- You can obtain this token from any authenticated Raptive API request (e.g. via Swagger).
- ⚠️ Note: This token may expire or rotate periodically, so you may need to update it before running the notebook.

### 2. `SNOWFLAKE_CONNECTION_PARAM`

Snowflake connection parameters are required to fetch the full list of `site_id`s (or a filtered subset of them) for the backfill process.

- Ensure your local environment is configured to connect to Snowflake using the appropriate credentials and network access.

## 🛠️ Usage

1. Open `gsc_backfill_all_sites.ipynb` locally.
2. Set your `AUTH_TOKEN` and `SNOWFLAKE_CONNECTION_PARAM` at the top of the notebook.
3. Run the notebook cells to:
   - Fetch the list of sites from Snowflake,
   - Send POST requests to the backfill endpoint for each site or group,
   - Monitor status codes and logs for success/failure per site.

## 🔁 Endpoint Info

The GSC backfill is triggered via:

POST http://api.raptive.com/api/v2/gscBackfill/group

Each request creates a backfill task for one or more sites.

## ✅ Tips

- Monitor the response status: a `201 Created` response means the backfill task was successfully initiated.
- You can customize the backfill window (e.g., `periodStartDate`, `periodEndDate`) in the payload sent to the endpoint.
- If backfilling a large number of sites, consider rate limits and add a small delay between requests.
