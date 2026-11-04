# Computrabajo Scraper: export to CSV or JSON with Python, Node.js or curl

Scrape job postings from Computrabajo in 18 Latin American countries: title, company with rating, city, region, salary with currency and period, contract, schedule, remote or hybrid, education, experience, skills, industry and dates. Search by keyword and city or paste URLs.

This repository has ready scripts for the [Computrabajo Scraper: LATAM Jobs and Salaries | $0.50 per 1,000](https://apify.com/v0iddo/computrabajo-scraper) Actor on Apify. The Actor does the
fetching, paging and cleaning; the scripts here run it with your input and save the results as a CSV or JSON file.

## Quick start

1. Create an Apify account and copy your API token from
   [Console > Settings > Integrations](https://console.apify.com/settings/integrations).
2. Install the client and run the script with the example input:

```bash
pip install "apify-client>=2"
export APIFY_TOKEN=your_token
python run.py input.json -o results.csv --max-items 50
```

Edit `input.json` for your own run. You can also run the Actor in the browser on its
[Apify page](https://apify.com/v0iddo/computrabajo-scraper), schedule it there, and connect its output to Google Sheets, Slack, Zapier or Make.

## Input

The example input (`input.json`):

```json
{
  "countries": [
    "mx"
  ],
  "keywords": [
    "contador"
  ],
  "location": "Monterrey",
  "includeDetails": true,
  "maxItems": 20
}
```

| Field | Type | What it does |
| --- | --- | --- |
| `countries` | array | Computrabajo sites to search; each search term runs on each of them. |
| `keywords` | array | Job titles or words such as contador, vendedor or desarrollador python. |
| `location` | string | Optional city, state or region such as Monterrey, Bogotá, Lima or Nuevo León. |
| `postedWithin` | string | Only postings published or updated in that period. |
| `salaryOnly` | boolean | Only postings that show a salary (not "A convenir"). |
| `remoteOnly` | boolean | Only postings marked remote or hybrid (presencial y remoto). |
| `searchUrls` | array | Search result pages copied from your browser, for example https://mx.computrabajo.com/trabajo-de-contador-en-monterrey . |
| `includeDetails` | boolean | Read each posting's page for the date first posted, contract type, schedule, industry, salary value, education, experience, languages, skills and the descrip... |
| `maxItems` | integer | The run stops after this many result rows. |

## Output

One row per result. A real row from a run of the example input (more in
[`examples/sample-output.json`](examples/sample-output.json)):

```json
{
  "jobId": "288F5BACD8FEDD6B61373E686DCF3405",
  "country": "Mexico",
  "countryCode": "mx",
  "title": "Auxiliar Contable",
  "company": "Importante empresa del sector",
  "companyId": null,
  "companyUrl": null,
  "companyLogo": null,
  "companyRating": null,
  "companyVerified": false,
  "confidentialEmployer": true,
  "location": "Monterrey, Nuevo León",
  "city": "Monterrey",
  "region": "Nuevo León",
  "workMode": "onsite",
  "salaryText": "$ 15,000.00 (Mensual)",
  "salaryMin": 15000,
  "salaryMax": 15000,
  "salaryCurrency": "MXN",
  "salaryInterval": "month",
  "salaryHidden": false,
  "contractType": "Contrato por tiempo indeterminado",
  "schedule": "Tiempo Completo",
  "employmentType": null,
  "industry": null,
  "education": "Educación media superior -Bachillerato General",
  "experienceYears": 2,
  "languages": [],
  "skills": [],
  "drivingLicenses": [],
  "travel": null,
  "relocation": null,
  "keywords": [
    "ayudante",
    "asistente",
    "auxiliar",
    "aux",
    "auxiliary"
  ],
  "directApply": null,
  "promoted": false,
  "urgent": false,
  "postedText": "Hace 13 horas",
  "postedDate": "2026-11-03",
  "firstPostedDate": null,
  "validThrough": null,
  "descriptionText": "Importante empresa dedicada a la transformación de aluminio solicita:\n\nAuxiliar Contable\nFunciones:\nRevisión y registro contable de entradas de almacén y cotejar con la orden de\ncompra. (se registran en 3 sistemas, ASPEL, COI e IQMS).\nRevisión y registro contable de fletes con...",
  "url": "https://mx.computrabajo.com/ofertas-de-trabajo/oferta-de-trabajo-de-auxiliar-contable-experiencia-en-impuestos-en-monterrey-288F5BACD8FEDD6B61373E686DCF3405",
  "scrapedAt": "2026-11-04T02:41:16Z"
}
```

Fields: `jobId`, `country`, `countryCode`, `title`, `company`, `companyId`, `companyUrl`, `companyLogo`, `companyRating`, `companyVerified`, `confidentialEmployer`, `location`, `city`, `region`, `workMode`, `salaryText`, `salaryMin`, `salaryMax`, `salaryCurrency`, `salaryInterval`, `salaryHidden`, `contractType`, `schedule`, `employmentType`, `industry`, `education`, `experienceYears`, `languages`, `skills`, `drivingLicenses`, `travel`, `relocation`, `keywords`, `directApply`, `promoted`, `urgent`, `postedText`, `postedDate`, `firstPostedDate`, `validThrough`, `descriptionText`, `url`, `scrapedAt`.

## Node.js

```javascript
import { ApifyClient } from 'apify-client';

const client = new ApifyClient({ token: process.env.APIFY_TOKEN });
const run = await client.actor('v0iddo/computrabajo-scraper').call({"countries": ["mx"], "keywords": ["contador"], "location": "Monterrey", "includeDetails": true, "maxItems": 20});
const { items } = await client.dataset(run.defaultDatasetId).listItems();
```

Full example: [`examples/node/run.mjs`](examples/node/run.mjs).

## Any language: one HTTP request

```bash
curl -X POST "https://api.apify.com/v2/acts/v0iddo~computrabajo-scraper/run-sync-get-dataset-items" \
  -H "Authorization: Bearer $APIFY_TOKEN" -H 'Content-Type: application/json' \
  -d '{"countries": ["mx"], "keywords": ["contador"], "location": "Monterrey", "includeDetails": true, "maxItems": 20}'
```

The response is the JSON array of results.

## Cost

The Actor is paid per result: **$0.50 per 1,000 results**, nothing per run. `maxItems` caps the rows, and so the cost, of one run. A new Apify account gets $5 of platform credit every month.

## License

MIT for the code in this repository. Made by [vøiddo](https://voiddo.com).
