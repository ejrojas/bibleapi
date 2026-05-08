<p align="center">
  <img src="https://bible.riosav.com/riosav_logo.png" alt="Riosav Bible API Logo" width="180" />
</p>

<h1 align="center">Bible API</h1>

<p align="center">
  Public REST API for Bible verses in <strong>KJV (English)</strong> and <strong>RVR1960 (Spanish)</strong>.
</p>

<p align="center">
  <a href="https://bible.riosav.com">Live Site</a> |
  <a href="https://bible.riosav.com/docs.php">Docs</a> |
  <a href="https://bible.riosav.com/portal/register.php">Get API Key</a>
</p>

---

## Why This API

Bible API gives developers a clean, fast way to fetch scripture by version, book, chapter, and verse range.

- Easy API-key auth
- Predictable JSON responses
- Free tier with generous limits
- Built-in account portal for key management

---

## Base URL

```txt
https://bible.riosav.com
```

---

## Quick Start

### 1) Create an Account

Register here:

https://bible.riosav.com/portal/register.php

After email verification, your API key is issued.

### 2) Send API Key Header

```http
X-API-Key: your_api_key_here
```

### 3) Make Your First Request

```bash
curl -H "X-API-Key: your_api_key_here" \
  https://bible.riosav.com/api/kjv/genesis/1/1
```

---

## Endpoint Format

### Single Verse

```txt
GET /api/{version}/{book}/{chapter}/{verse}
```

### Verse Range

```txt
GET /api/{version}/{book}/{chapter}/{verse_start}-{verse_end}
```

### Supported Versions

| Version | Translation | Language |
|---|---|---|
| `kjv` | King James Version | English |
| `rvr1960` | Reina Valera 1960 | Spanish |

### Parameter Notes

| Parameter | Description | Example |
|---|---|---|
| `version` | `kjv` or `rvr1960` | `kjv` |
| `book` | Name (case-insensitive) or number | `genesis` or `1` |
| `chapter` | Chapter number | `1` |
| `verse` / `verse_start-verse_end` | Single verse or range (max 10) | `1` or `1-4` |

---

## Request Examples

### Single Verse

```bash
curl -H "X-API-Key: your_api_key_here" \
  https://bible.riosav.com/api/kjv/genesis/1/1
```

### Verse Range

```bash
curl -H "X-API-Key: your_api_key_here" \
  https://bible.riosav.com/api/rvr1960/juan/3/16-17
```

---

## Response Format

### Success

```json
{
  "status": "success",
  "version": "KJV",
  "language": "en",
  "book": "genesis",
  "book_number": 1,
  "chapter": 1,
  "verses": [
    { "verse": 1, "text": "In the beginning God created the heaven and the earth." }
  ],
  "credits_used": 1,
  "credits_remaining": 199
}
```

### Error

```json
{
  "status": "error",
  "code": "RATE_LIMIT_EXCEEDED",
  "message": "You have exceeded your daily limit of 200 credits.",
  "retry_after": "2026-05-08T00:00:00Z"
}
```

`retry_after` appears only on limit-related responses.

---

## Rate Limits

| Limit | Value |
|---|---|
| Daily credits | 200/day (reset at midnight UTC) |
| Burst limit | 25 requests per 5-minute window |
| Max verses/request | 10 |

Credit model: `1 verse = 1 credit`

---

## Error Codes

| Code | HTTP | Meaning |
|---|---:|---|
| `MISSING_API_KEY` | 401 | Missing `X-API-Key` header |
| `INVALID_API_KEY` | 401 | Key not found |
| `ACCOUNT_SUSPENDED` | 403 | Account suspended |
| `ACCOUNT_INACTIVE` | 403 | Unverified account or disabled key |
| `RATE_LIMIT_EXCEEDED` | 429 | Daily limit exhausted |
| `BURST_LIMIT_EXCEEDED` | 429 | Burst window exceeded |
| `INVALID_VERSION` | 400 | Invalid translation version |
| `INVALID_BOOK` | 400 | Book not found |
| `INVALID_CHAPTER` | 400 | Chapter not found |
| `INVALID_VERSE` | 400 | Verse not found |
| `RANGE_TOO_LARGE` | 400 | More than 10 verses requested |
| `INVALID_RANGE_FORMAT` | 400 | Malformed verse range |
| `SERVER_ERROR` | 500 | Internal server error |

---

## Book Names

Book names are case-insensitive.

- `Genesis`, `genesis`, `GENESIS` are all valid
- You can also use numeric book IDs (`1` through `66`)

For Spanish requests (`rvr1960`), use Spanish book names (for example `juan`) or numeric IDs.

---

## Developer Notes

- CORS enabled (`Access-Control-Allow-Origin: *`)
- JSON response envelope is consistent for success and error cases
- API keys are stored hashed server-side

---

## Project Links

- Live site: https://bible.riosav.com
- Docs page: https://bible.riosav.com/docs.php
- Registration: https://bible.riosav.com/portal/register.php

---

## License

This project is provided as-is for public API use and integration.
