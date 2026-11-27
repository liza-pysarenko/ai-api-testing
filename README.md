# Swagger Petstore API Regression

Automated API regression for the public Swagger Petstore API using a Postman Collection, Newman, and GitHub Actions.

## API

- Base URL: `https://petstore3.swagger.io/api/v3`
- OpenAPI: `https://petstore3.swagger.io/api/v3/openapi.json`

## CRUD regression flow

The collection `collections/petstore-collection.json` runs the following sequence:

1. Create pet
2. Get created pet
3. Update pet
4. Get updated pet
5. Delete pet
6. Verify deletion (expected HTTP 404)

The collection uses the `baseUrl` collection variable and generates a unique pet ID and test data for each run. It validates documented response statuses, the Pet response structure, and data consistency. The created pet is deleted as part of the run.

## Run locally in Postman

1. Import `collections/petstore-collection.json` into Postman.
2. Open the collection and select **Run**.
3. Run all six requests in order with one iteration.
4. Review the assertions in Collection Runner.

## Run locally with Newman

Requirements: Node.js 20 or later and npm.

```bash
npm install --global newman@6 newman-reporter-htmlextra
mkdir -p results reports
newman run collections/petstore-collection.json --reporters cli,json,htmlextra --reporter-json-export results/newman-report.json --reporter-htmlextra-export reports/index.html
```

## GitHub Actions

The workflow `.github/workflows/newman.yml`:

- Runs on pushes to `main` when the collection or workflow changes.
- Supports manual execution through **Actions → API regression (Newman) → Run workflow**.
- Uses Node.js 20, Newman 6, and `newman-reporter-htmlextra`.
- Uploads machine-readable JSON and HTML reports as separate workflow artifacts.
- Publishes the HTML report to GitHub Pages whenever the Newman HTML report was generated, including when test assertions fail.

### Enable GitHub Pages

Before the first Pages deployment, open **Settings → Pages** in this repository and set **Build and deployment → Source** to **GitHub Actions**. The publishing job uses `actions/configure-pages`, `actions/upload-pages-artifact`, and `actions/deploy-pages`.

After a successful deployment, the report will be available at:

`https://liza-pysarenko.github.io/ai-api-testing/`

## Repository structure

- `collections/` — Postman collections
- `results/` — generated machine-readable run results
- `reports/` — generated HTML reports
- `.github/workflows/` — GitHub Actions workflows
