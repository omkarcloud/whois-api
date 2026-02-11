# Whois API

REST API for domain WHOIS lookups. Returns registrar info, registration dates, nameservers, and DNSSEC status in structured JSON.

## Features

- Single endpoint for all domain lookups
- Returns structured JSON (no raw WHOIS parsing needed)
- Supports 1,000+ TLDs including .com, .net, .org, .io, .co, .ai
- Real-time data from official WHOIS servers
- 850ms average response time
- 5,000 requests/month on free tier
- Example Response:

```json
{
  "domain": "github.com",
  "registration": {
    "registrar": "markmonitor, inc.",
    "whois_server": "whois.markmonitor.com",
    "created_at": "2007-10-09T18:20:50Z",
    "updated_at": "2024-09-07T09:16:33Z",
    "expires_at": "2026-10-09T18:20:50Z"
  },
  "dns": {
    "name_servers": [
      "ns-1283.awsdns-32.org",
      "ns-1707.awsdns-21.co.uk",
      "dns1.p08.nsone.net",
      "dns2.p08.nsone.net"
    ],
    "dnssec_status": "unsigned",
    "dnssec_enabled": false
  }
}
```

## Authentication

- Create account at [omkar.cloud](https://www.omkar.cloud/auth/sign-up)
- Get API key from [omkar.cloud/api-key](https://www.omkar.cloud/api-key)
- Include `API-Key` header in requests

Note: 5,000 requests/month free.

## Quick Start

```shell
curl -X GET "https://whois-api.omkar.cloud/whois?domain=github.com" \
  -H "API-Key: YOUR_API_KEY"
```

```json
{
  "domain": "github.com",
  "registration": {
    "registrar": "markmonitor, inc.",
    "whois_server": "whois.markmonitor.com",
    "created_at": "2007-10-09T18:20:50Z",
    "updated_at": "2024-09-07T09:16:33Z",
    "expires_at": "2026-10-09T18:20:50Z"
  },
  "dns": {
    "name_servers": ["ns-1283.awsdns-32.org", "ns-1707.awsdns-21.co.uk"],
    "dnssec_status": "unsigned",
    "dnssec_enabled": false
  }
}
```

## Installation

### Python

```shell
pip install requests
```

```python
import requests

response = requests.get(
    "https://whois-api.omkar.cloud/whois",
    params={"domain": "github.com"},
    headers={"API-Key": "YOUR_API_KEY"}
)

whois_data = response.json()
```

### Node.js

```shell
npm install axios
```

```js
import axios from "axios";

const response = await axios.get("https://whois-api.omkar.cloud/whois", {
  params: { domain: "github.com" },
  headers: { "API-Key": "YOUR_API_KEY" }
});

const whoisData = response.data;
```

## API Reference

### Endpoint

```
GET https://whois-api.omkar.cloud/whois
```

### Headers

| Header    | Required | Description                                              |
|-----------|----------|----------------------------------------------------------|
| `API-Key` | Yes      | API key from [omkar.cloud/api-key](https://www.omkar.cloud/api-key) |

### Parameters

| Parameter | Required | Description          |
|-----------|----------|----------------------|
| `domain`  | Yes      | Domain name to lookup |

### Response

```json
{
  "domain": "string",
  "registration": {
    "registrar": "string",
    "whois_server": "string",
    "created_at": "ISO 8601 datetime",
    "updated_at": "ISO 8601 datetime",
    "expires_at": "ISO 8601 datetime"
  },
  "dns": {
    "name_servers": ["string"],
    "dnssec_status": "string",
    "dnssec_enabled": "boolean"
  }
}
```

| Field                      | Type     | Description                        |
|----------------------------|----------|------------------------------------|
| `domain`                   | string   | Domain name                        |
| `registration.registrar`   | string   | Domain registrar                   |
| `registration.whois_server`| string   | WHOIS server used                  |
| `registration.created_at`  | string   | Domain registration date           |
| `registration.updated_at`  | string   | Last modification date             |
| `registration.expires_at`  | string   | Domain expiration date             |
| `dns.name_servers`         | array    | List of nameservers                |
| `dns.dnssec_status`        | string   | DNSSEC status (signed/unsigned)    |
| `dns.dnssec_enabled`       | boolean  | Whether DNSSEC is enabled          |

## Examples

### Check domain expiration

```python
response = requests.get(
    "https://whois-api.omkar.cloud/whois",
    params={"domain": "example.com"},
    headers={"API-Key": "YOUR_API_KEY"}
)

data = response.json()
print(f"Expires: {data['registration']['expires_at']}")
```

### Get registrar info

```python
response = requests.get(
    "https://whois-api.omkar.cloud/whois",
    params={"domain": "stripe.com"},
    headers={"API-Key": "YOUR_API_KEY"}
)

data = response.json()
print(f"Registrar: {data['registration']['registrar']}")
```

### Monitor multiple domains

```js
const domains = ["github.com", "stripe.com", "vercel.com"];

const results = await Promise.all(
  domains.map(domain =>
    axios.get("https://whois-api.omkar.cloud/whois", {
      params: { domain },
      headers: { "API-Key": "YOUR_API_KEY" }
    })
  )
);

results.forEach(r => {
  const { domain, registration } = r.data;
  console.log(`${domain} expires: ${registration.expires_at}`);
});
```

## Error Handling

```python
response = requests.get(
    "https://whois-api.omkar.cloud/whois",
    params={"domain": "invalid"},
    headers={"API-Key": "YOUR_API_KEY"}
)

if response.status_code == 200:
    data = response.json()
elif response.status_code == 401:
    # Invalid API key
    pass
elif response.status_code == 429:
    # Rate limit exceeded
    pass
```

## Rate Limits

| Plan    | Price | Requests/Month |
|---------|-------|----------------|
| Free    | $0    | 5,000          |
| Starter | $25   | 100,000        |
| Grow    | $75   | 1,000,000      |
| Scale   | $150  | 10,000,000     |

## Support

- Email: [happy.to.help@omkar.cloud](mailto:happy.to.help@omkar.cloud)
- WhatsApp: [Chat with us](https://api.whatsapp.com/send?phone=918178804274&text=I%20have%20a%20question%20about%20the%20Whois%20API.)

## Love It? Star It ⭐!

If this tool has been helpful, please give us a [star ⭐ on GitHub.](https://github.com/omkarcloud/whois-api/stargazers)
