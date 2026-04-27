# LinkedIn Jobs Scraper

Extract structured data from [linkedin.com](https://linkedin.com) — linkedin.com jobs with state-keyed delta pricing — recurring runs only charge for NEW or CHANGED listings; already-seen jobs are skipped automatically. Built-in Telegram/Discord/Slack/WhatsApp/webhook notifications.

**[LinkedIn Jobs Scraper on Apify →](https://apify.com/blackfalcondata/linkedin-jobs-scraper?fpr=1h3gvi)**


## 🚀 How to use this actor

> ### 💚 $5 free Apify credits — every month
> No credit card required. No commitment. Cancel anytime.

### 👉 [Sign up free on Apify →](https://console.apify.com/sign-up?fpr=1h3gvi)

1. **Click sign up** — pick GitHub, Google, or email; takes ~30 seconds
2. **Open this actor** — input is pre-filled with a working example
3. **Click Start** — export results as JSON, CSV, or Excel

Your **$5 monthly platform credit** is enough to run this actor right away — and again every month — scraping typically several hundred to several thousand results per run, depending on your input.


---

## Key features


**Incremental mode** — Only get new or changed listings since your last run. Content hash per listing — no duplicates, no re-processing.

**Change classification** — Track unchanged, expired, cross-run repost detection across runs. Build audit trails of how listings evolve over time.

**Compact output** — Emit core fields only (AI-agent / MCP-friendly). Keeps response size small for LLM workflows.

**Description truncation** — Cap description length per listing to control output size and cost.

**Result cap** — Stop after N listings. Set to 0 for the full catalog.

**Proxy support** — Route traffic through Apify Proxy or your own proxy group to avoid regional blocks and rate limits.

**Export anywhere** — Download as JSON, CSV, or Excel. Stream via Apify API, webhooks, or integrations with Make, Zapier, Airbyte, Keboola.

**Structured data** — Every listing returns the same schema with consistent field naming. All fields always present — `null` when unavailable, never omitted.

---

## Use cases


**Data pipeline automation**
Integrate with your ETL pipeline to collect structured listings from linkedin.com on a schedule. Export to CSV, JSON, or directly to your database. Use compact mode to control output size.

**Market research**
Monitor listings, track trends, and analyze market dynamics with structured, deduplicated data from linkedin.com.

**Change monitoring**
Run daily or hourly in incremental mode to capture only new, updated, or expired listings. Perfect for price-tracking, churn analysis, and alerting pipelines.

**Compensation benchmarking**
Aggregate salary ranges across roles, industries, and locations on linkedin.com to inform pricing decisions, hiring plans, or candidate negotiations.

**AI / LLM training data**
Structured JSON per listing is ready for RAG pipelines, embeddings, and agent workflows. Compact mode trims tokens for LLM context windows.

---

## Quick start

```json
{
  "keywords": "software engineer",
  "location": "United States",
  "datePosted": "last24h",
  "maxResults": 100
}
```

---

## Input parameters

| Parameter | Type | Default | Description |
|-----------|------|---------|-------------|
| `keywords` | string | — | Job search keywords (e.g. "software engineer", "nurse"). Leave blank to browse all jobs in the selected location. |
| `location` | string | — | Free-text location (e.g. "Copenhagen, Denmark", "United States"). Use geoIds for higher precision. |
| `geoIds` | array | `[]` | Numeric LinkedIn geoIds (e.g. "103644278" = United States). Each geoId becomes a separate query, deduped on jobId. |
| `regions` | array | `[]` | Two-letter country codes (e.g. "US", "GB", "DE"). Resolved to LinkedIn country geoIds. Use geoIds[] for unsupported markets. |
| `regionPresets` | enum | — | Pre-defined country grouping. Combined with regions[] if both are set. |
| `datePosted` | enum | `"anytime"` | Filter by posting recency. "lastHour" is unique to this scraper. |
| `jobType` | array | `[]` | Multi-select employment type filter. |
| `experienceLevel` | array | `[]` | Multi-select seniority filter. |
| `workType` | array | `[]` | Multi-select onsite/remote/hybrid filter. |
| `salaryMin` | integer | — | Minimum annual salary (USD). Mapped to LinkedIn's nearest f_SB2 bucket. Post-filtered exactly. |
| `salaryMax` | integer | — | Maximum annual salary. Post-filtered (LinkedIn has no native max filter). |
| `salaryIncludeUnknown` | boolean | `true` | When salaryMin/Max set, include jobs with no salary data. |
| `companies` | array | `[]` | Numeric LinkedIn companyIds (filter at API level). |
| `excludeCompanies` | array | `[]` | Company-name substrings to exclude (case-insensitive post-filter). |
| `excludeKeywords` | array | `[]` | Title substrings to exclude (case-insensitive post-filter). |
| `easyApply` | boolean | `false` | Restrict results to LinkedIn Easy Apply postings. |
| `removeAgency` | boolean | `false` | Heuristic post-filter on company name ("recruitment", "staffing", etc.). |
| `distance` | integer | — | Search radius around location. LinkedIn auto-detects unit (miles in US/UK/CA/AU/NZ; km elsewhere). |
| `sortBy` | enum | `"recent"` | Sort order. "recent" is recommended for incremental feeds. |
| `startUrls` | array | `[]` | Direct LinkedIn job-search URLs. Supersedes structured query when set. Deferred to v0.5 — currently logged but not crawled. |
| `proxyConfiguration` | object | `{"useApifyProxy":true}` | Network egress configuration. The default uses Apify Proxy with whatever group your plan has available — switch to residential if you observe blocks. |
| `linkedinHost` | string | `"www"` | Subdomain (e.g. "www", "de"). Affects default geo-routing + UI labels. |
| `outputLanguage` | string | `"en-US,en;q=0.9"` | HTTP Accept-Language header. Default en-US for parsing-cleanliness. |
| `incrementalMode` | boolean | `false` | Persist seen-jobIds across runs; emit only NEW + UPDATED. Requires stateKey. |
| `stateKey` | string | — | Unique key for cross-run state. Required when incrementalMode=true. |
| `outputMode` | enum | `"all"` | Which items to emit when in incrementalMode. |
| `emitUnchanged` | boolean | `false` | Include items that haven't changed since last run. |
| `emitExpired` | boolean | `false` | Include items that disappeared from search results. |
| `skipReposts` | boolean | `false` | Suppress jobs with the same content as a previously-seen but inactive listing. |
| `enrichDetails` | boolean | `false` | Fetch each job's detail page for full description, seniorityLevel, employmentType, jobFunction, industry, and applicantCount. Adds 1 HTTP request per job. |
| `scopePerQuery` | boolean | `false` | Reserved — currently no-op (state is per-stateKey). |
| `discoverRelated` | boolean | `false` | After the main SERP, fetch LinkedIn's relatedJobs feed for the first N results to discover jobs that don't surface on the keyword search. Useful for thin markets. |
| `relatedSeedCount` | integer | `5` | How many of the first SERP results are used as seeds for relatedJobs expansion. Each seed yields up to 10 candidates. |
| `telegramToken` | string | — | Bot token from @BotFather. Leave empty to disable Telegram notifications. |
| `telegramChatId` | string | — | Numeric chat or channel ID where notifications should be sent. |
| `discordWebhookUrl` | string | — | Channel webhook URL. Leave empty to disable Discord notifications. |
| `slackWebhookUrl` | string | — | Incoming webhook URL. Leave empty to disable Slack notifications. |
| `whatsappAccessToken` | string | — | Permanent System User token from Meta Business. |
| `whatsappPhoneNumberId` | string | — | Numeric phone-number ID from the Meta dashboard. |
| `whatsappTo` | string | — | Recipient phone number in E.164 format (e.g. "4561234567"). |
| `webhookUrl` | string | — | Receives a JSON POST of {metadata, items} for each batch. |
| `webhookHeaders` | object | — | Additional HTTP headers to include with the generic webhook (e.g. Authorization). |
| `notificationLimit` | integer | `5` | Cap on items included in each notification message. |
| `notifyOnlyChanges` | boolean | `false` | When in incrementalMode, suppress notifications for UNCHANGED items. |
| `compact` | boolean | `false` | Emit only core fields (keeps cost the same; reduces dataset size). |
| `descriptionMaxLength` | integer | `0` | Truncate description (0 = no truncation). Only relevant when enrichDetails=true. |
| `phoneExtractionMode` | enum | `"strict"` | "strict" requires Tel:/+CC prefix; "lenient" also picks up bare local-format numbers. |
| `maxResults` | integer | `100` | Cap on emitted items (0 = unlimited). |

---

## Output fields

Every listing returns the same 55-field schema. Missing values are `null` — never omitted.

- `jobId`
- `linkedinJobId`
- `jobUrl`
- `title`
- `company`
- `companyUrl`
- `companyId`
- `location`
- `country`
- `postedAt`
- `applyUrl`
- `applyType`
- `description`
- `descriptionHtml`
- `descriptionMarkdown`
- `seniorityLevel`
- `employmentType`
- `industry`
- `jobFunction`
- `workplaceType`
- `applicantCount`
- `easyApply`
- `salaryMin`
- `salaryMax`
- `salaryCurrency`
- `salaryPeriod`
- `salarySource`
- `salaryIsPredicted`
- `companyLogo`
- `companyDescription`
- `companyEmployeeCount`
- `companyWebsite`
- `companyAddress`
- `recruiterName`
- `recruiterUrl`
- `recruiterTitle`
- `extractedEmails`
- `extractedPhones`
- `extractedUrls`
- `socialProfiles`
- `changeType`
- `firstSeenAt`
- `lastSeenAt`
- `previousSeenAt`
- `expiredAt`
- `isRepost`
- `repostOfId`
- `repostDetectedAt`
- `scrapedAt`
- `source`
- `language`
- `contentHash`
- `isPromoted`
- `postingBenefits`
- `trackingId`

---

## Sample output

One object per listing. Here is a real example from a production run:

```json
{
  "jobId": "306a0079177ef855fbd37ecb5d1b5b77c675b372996c405289d943dcfdfd1167",
  "linkedinJobId": "4373435862",
  "jobUrl": "https://www.linkedin.com/jobs/view/software-engineer-media-encoding-pipelines-l4-at-netflix-4373435862",
  "title": "Software Engineer, Media Encoding Pipelines (L4)",
  "company": "Netflix",
  "companyUrl": "https://www.linkedin.com/company/netflix",
  "companyId": "netflix",
  "location": "United States",
  "country": "United States",
  "postedAt": "2026-04-26T00:00:00.000Z",
  "applyUrl": "https://www.linkedin.com/jobs/view/software-engineer-media-encoding-pipelines-l4-at-netflix-4373435862",
  "applyType": "unknown"
}
```

*Truncated — full records contain 55 fields. See Output fields for the complete schema.*

**[Try LinkedIn Jobs Scraper now — $5 free credit, no credit card →](https://apify.com/blackfalcondata/linkedin-jobs-scraper?fpr=1h3gvi)**

---

## Pricing

Pay only for what you extract. No subscription required — Apify's free $5 credit covers thousands of results.

| Event | Price (USD) |
| --- | --- |
| Actor Start | $0.0005 |
| Result | $0.001 |

See the [actor on Apify](https://apify.com/blackfalcondata/linkedin-jobs-scraper?fpr=1h3gvi) for current pricing.

---

## FAQ

**How do I scrape linkedin.com?**
Use this actor on Apify to extract structured data from linkedin.com. Configure your search query and filters in the input, then click Start — no coding required.

**How do I get linkedin.com data as JSON, CSV, or Excel?**
The actor writes each listing to Apify's dataset. Download as JSON, CSV, or Excel from the Console, stream via the API, or push to Make, Zapier, Airbyte, or Keboola.

**Is it legal to scrape linkedin.com?**
Web scraping of publicly available data is generally legal. This actor only accesses publicly visible information. Always check linkedin.com's terms of service for your specific use case.

**How much does it cost?**
Pay-per-event pricing — you only pay for listings extracted. Apify's free $5 credit is enough to run thousands of results before you pay anything.

**How does incremental mode work?**
Each listing gets a content hash. On subsequent runs, only new or changed listings are emitted — saving time, compute, and storage. Expired listings can be tracked separately.

**Do I need an API key or credentials?**
No. Just sign up for Apify, paste your input, and click Start. No credit card required.

---

## Related products by Black Falcon Data


- [StepStone Scraper](https://apify.com/blackfalcondata/stepstone-scraper?fpr=1h3gvi) — Job listings from 18 European portals
- [Indeed Job Scraper](https://apify.com/blackfalcondata/indeed-job-scraper?fpr=1h3gvi) — Indeed job listings with salary data
- [Glassdoor Job Scraper](https://apify.com/blackfalcondata/glassdoor-job-scraper?fpr=1h3gvi) — Glassdoor listings with company ratings
- [Arbeitsagentur Scraper](https://apify.com/blackfalcondata/arbeitsagentur-scraper?fpr=1h3gvi) — Germany's official job portal (1M+ listings)
- [SEEK Scraper](https://apify.com/blackfalcondata/seek-scraper?fpr=1h3gvi) — Australia & NZ's largest job board
- [Naukri Scraper](https://apify.com/blackfalcondata/naukri-scraper?fpr=1h3gvi) — India's largest job portal

---

## Getting started with Apify

New to Apify? [Create a free account with $5 credit](https://console.apify.com/sign-up?fpr=1h3gvi) — no credit card required.

1. Sign up — $5 platform credit included
2. Open [LinkedIn Jobs Scraper](https://apify.com/blackfalcondata/linkedin-jobs-scraper?fpr=1h3gvi) and configure your input
3. Click **Start** — export results as JSON, CSV, or Excel

Need more later? [See Apify pricing](https://apify.com/pricing?fpr=1h3gvi).

---

## About Black Falcon Data

Black Falcon Data builds production-grade web scrapers for job boards and marketplace data. Browse our full actor catalog at [www.blackfalcondata.com](https://www.blackfalcondata.com).

---

*Last updated: 2026 04*
