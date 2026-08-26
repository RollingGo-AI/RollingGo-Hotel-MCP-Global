# RollingGo Hotel MCP — Hotel Search & Booking

[![Version](https://img.shields.io/badge/Version-1.0.0-blue.svg)](https://github.com/DIDA-AI/dida_hotel_mcp_global/releases)
[![ModelScope](https://img.shields.io/badge/ModelScope-Rank%237-brightgreen.svg)](https://modelscope.cn/)
[![MCP Version](https://img.shields.io/badge/MCP-1.0.0-blue.svg)](https://modelcontextprotocol.io)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Python 3.10+](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)

🏠 [Apply Key](https://global.rollinggo.store/) · 🚀 [Quick Start](#-quick-start) · 📚 [Examples](#-usage-examples) · 💬 [Support](#-support) · 🔍 [Q&A](#-qa)  · 💰[Earn with RollingGo](https://global.rollinggo.store/docs/partnerdoc/partner1)

This is an official MCP server empowers AI Agents to search, compare, and book **over 2 Million hotels globally**. Powered by DIDA (14 years, world's #3 travel distribution platform), this server bridges the gap between AI travel recommendations and real-world bookings.

| Service | Endpoint | Available Tools | Authentication |
|---------|----------|-----------------|----------------|
| Hotel MCP | `https://mcp.rollinggo.ai/mcp` | searchHotels, getHotelDetail, getHotelSearchTags | `Authorization: Bearer <YOUR_API_KEY>` |

- **Transport Protocol**: `streamable-http`
- **Pricing**: Completely free, no usage limits
- **Access Method**: Self-service following this documentation; suitable for rapid prototyping and tool development.

>  RollingGo MCP also offers an OAuth 2.0 Authorization Code flow, providing 7 tools including getHotelSearchTags, searchHotels, getHotelDetail, hotelPriceConfirm, searchHotelOrders, and more. This mode is designed for deep integration with enterprise-grade production applications and requires a business contact via contact@rollinggo.ai.

>  **For Chinese users** or workflows primarily targeting the mainland China market and Alipay payment systems, please refer to this version: [**Dida-hotel-MCP-CN**](https://github.com/DIDA-AI/Dida-hotel-MCP-CN)



---
## 🌟 Why DIDA Hotel MCP?

Traditional AI agents can only recommend hotels based on static training datasets. The **DIDA Hotel MCP** equips your LLM agent with direct, real-time transactional capabilities:


✅ **Live Rates & Bookable Inventory** — Zero-latency price verification; every result is instantly bookable.

✅ **Supply Chain** — The world's Top **3** travel B2B platform, **14** years in the making, fully API-native end-to-end.

✅ **Global Hotel Network** — **2,000,000**+ properties covering **200+**
countries/regions. **500**+ suppliers covering every tier, from luxury chains to local boutiques.

✅ **Direct Contracts** — **110,000**+ directly connected hotels with live price and inventory sync.

✅ **Price Edge at Source** — Priced upstream of OTAs; sharp rates on hot spots.

✅ **Agent-Ready** — Works with **40+** leading agents: Cursor, Claude Code, Codex, Windsurf, Copilot, and more.

✅ **Earn your Revenue on Every MCP Call** — Set country-specific markups, earn commission on every completed booking, and track your orders, earnings, and payouts in real time. Flexible withdrawals for both businesses and individual developers.

---

## 🎯 Who is it for
• Companies or individual developers building AI Agents

• Developers looking to integrate hotel booking capabilities into MCP Clients

• Developers building travel planning, business travel management, OTA, and lifestyle service agents

• Product teams seeking to validate AI Agent commercial transaction loops

• Individuals with needs for hotel search, price comparison, and price drop alerts******

## 🎯 Use Cases
* **General-Purpose Agents**: Equip any AI agent with native hotel booking. Users can compare, filter, and book—all within a single natural-language conversation.
* **AI Travel Planners**: Integrate hotel searching directly into natural language itineraries.
* **Corporate Travel Assistants**: Allow employees to query, compare, and book business travel within Slack, Teams, or custom chat interfaces.
* **Transactional Demos**: Validate end-to-end AI agent commerce and payment flows without heavy backend integration.

---

## 🚀 Quick Start

Integrate global hotel search and booking into your AI assistant in under **5 minutes** with no coding required.

### Step 1: Get Your Developer API Key

1. Go to the [DIDA Partner Center](https://global.rollinggo.store/apply) and sign up for a free key.
2. You will receive an email containing: Credentials for your B2B Partner Center dashboard (to monitor orders, configure markup, and track earnings).

---

## Step 2: Connect to Your Agent

> Recommended clients: Claude CLI, Codex, and Cursor. Other MCP-compatible clients (such as Kiro, Doubao, etc.) can be configured similarly.

### Claude CLI

Create `.mcp.json` in your project root:

```json
{
  "mcpServers": {
    "Dida-Hotel": {
      "url": "https://mcp.rollinggo.ai/mcp",
      "type": "http",
      "headers": {
        "Authorization": "Bearer YOUR_API_KEY"
      }
    }
  }
}
```

Or add directly via the command line:

```bash
claude mcp add \
  --transport http \
  --header "Authorization: Bearer YOUR_API_KEY" \
  Dida-Hotel \
  https://mcp.rollinggo.ai/mcp
```

### Codex

Config file location: `.codex/config.json` in the project root, or globally at `~/.codex/config.json`

```json
{
  "mcpServers": {
    "Dida-Hotel": {
      "url": "https://mcp.rollinggo.ai/mcp",
      "type": "streamable-http",
      "headers": {
        "Authorization": "Bearer YOUR_API_KEY"
      }
    }
  }
}
```

### Cursor

Config file location: `.cursor/mcp.json` in the project root, or globally at `~/.cursor/mcp.json`

```json
{
  "mcpServers": {
    "Dida-Hotel": {
      "url": "https://mcp.rollinggo.ai/mcp",
      "type": "streamable-http",
      "headers": {
        "Authorization": "Bearer YOUR_API_KEY"
      }
    }
  }
}
```

> Replace `YOUR_API_KEY` with your actual API Key.

### Test Directly with cURL

> **Note**: cURL must include `-H "Accept: application/json, text/event-stream"`, otherwise the server will return a 400 error.

```bash
curl -X POST https://mcp.rollinggo.ai/mcp \
  -H "Content-Type: application/json" \
  -H "Accept: application/json, text/event-stream" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -d '{
    "jsonrpc": "2.0",
    "method": "tools/call",
    "params": {
      "name": "searchHotels",
      "arguments": {
        "originQuery": "Shanghai Bund five-star hotel",
        "place": "Shanghai Bund",
        "placeType": "Attraction",
        "checkInParam": {
          "checkInDate": "2026-06-01",
          "stayNights": 2
        },
        "filterOptions": {
          "starRatings": [5.0]
        },
        "size": 3
      }
    },
    "id": 1
  }'
```

---

## Step 3: Your First MCP Call

Once configured, simply tell your AI assistant:

> "Find me a five-star hotel near the Shanghai Bund for a stay starting the day after tomorrow."

The AI will automatically call the `searchHotels` Tool and return a list of hotels.

### Hotel Search Example

```json
{
  "jsonrpc": "2.0",
  "method": "tools/call",
  "params": {
    "name": "searchHotels",
    "arguments": {
      "originQuery": "Shanghai Bund five-star hotel",
      "place": "Shanghai Bund",
      "placeType": "Attraction",
      "checkInParam": {
        "checkInDate": "2026-06-01",
        "stayNights": 2
      },
      "filterOptions": {
        "starRatings": [5.0]
      },
      "size": 3
    }
  },
  "id": 1
}
```

---

## Step 4: View Results and Sample Data

### Hotel Search Results (Real Data)

> Searching for "Shanghai Bund five-star hotel", 2 nights, returns:

| Hotel | Stars | Lowest Price/Night | Distance to Bund |
|-------|-------|--------------------|------------------|
| Fairmont Peace Hotel Shanghai | ⭐⭐⭐⭐⭐ | $648 | 124m |
| The Peninsula Shanghai | ⭐⭐⭐⭐⭐ | $940 | 252m |

### Expected Response JSON Example

```json
{
  "message": "Hotel search successful",
  "hotelInformationList": [
    {
      "hotelId": 29529,
      "bookingUrl": "https://rollinggo.ai/pages/hotel/detail/index?...",
      "name": "Fairmont Peace Hotel on the Bund",
      "address": "No. 20 Nanjing East Road",
      "starRating": 5.0,
      "price": {
        "message": "Price query successful. Lowest price: 648, Currency: USD",
        "hasPrice": true,
        "currency": "USD",
        "lowestPrice": 648.0
      },
      "hotelAmenities": ["Bar", "Gym", "Pool", "SPA", "Parking", "WIFI"],
      "tags": ["SPA Service", "Resort Hotel", "Sports-Friendly Hotel"]
    }
  ]
}

```

## 🔧 Available Tools
[Official Docs | Dida MCP Tools](https://global.rollinggo.store/docs/mcp-docs/mcp-tool-reference)

The server registers 3 core tools to handle the complete search-to-book lifecycle:

### 1) searchHotels
Find hotels by location, date, price, star rating, and tags.

* **Input Parameters**:
  - `originQuery` (string, **required**): User's raw text request (e.g., "Find boutique hotels in Tokyo under $200").
  - `place` (string, **required**): Specific destination, attraction, or airport name.
  - `placeType` (string, **required**): Location type (`city`, `airport`, `point_of_interest`, `hotel`, etc.). Supported values: `city`, `airport`, `point_of_interest`, `train_station`, `subway_station`, `hotel`, `district/county`, `detailed address`.
  - `countryCode` (string, optional): ISO 3166-1 alpha-2 country code, e.g. `CN`, `US`.
  - `size` (number, optional, default: `5`): Number of hotels to return, max `20`.
  - `checkInParam` (object, optional): Check-in related parameters.
  - `filterOptions` (object, optional): Filter parameters.
  - `hotelTags` (object, optional): Tag / brand / budget filters.

<details>
<summary><b>🔎 View Detailed Input Parameters (checkInParam, filterOptions, hotelTags)</b></summary>

**`checkInParam` fields:**
- `adultCount` (number, optional, default: `2`): Adults per room.
- `checkInDate` (string, optional, format: `YYYY-MM-DD`): Check-in date. If omitted, in the past, or malformed, defaults to tomorrow.
- `stayNights` (number, optional, default: `1`): Number of nights (max 28).

**`filterOptions` fields:**
- `distanceInMeter` (number, optional): Straight-line distance from POI in meters. Defaults to `2000` when a POI is used.
- `starRatings` (number[], optional): Star rating range, defaults to `[0.0, 5.0]`, step `0.5`.

**`hotelTags` fields:**
- `requiredTags` (string[], optional): Required tags (hard constraint).
- `preferredBrands` (string[], optional): Preferred brands.
- `maxPricePerNight` (number, optional): Max budget per night (CNY).

</details>

<details>
<summary><b>📄 View Response JSON Schema Example</b></summary>

```json
{
  "message": "Hotel search succeeded",
  "hotelInformationList": [
    {
      "hotelId": 43615,
      "bookingUrl": "https://rollinggo.ai/pages/hotel/detail/index?...",
      "name": "Sunworld Dynasty Hotel Beijing",
      "brand": null,
      "address": "50 Wangfujing Street",
      "destinationId": "6140156",
      "latitude": 39.917748,
      "longitude": 116.412249,
      "distanceInMeters": 205,
      "starRating": 5.0,
      "price": {
        "message": "Price found, lowest: 626.0, currency: CNY",
        "hasPrice": true,
        "currency": "CNY",
        "lowestPrice": 626.0
      },
      "areaCode": "CN",
      "description": "...",
      "imageUrl": "https://image-cdn.RollingGo.com/...",
      "hotelAmenities": ["24h Front Desk", "WiFi"],
      "score": 1.0,
      "tags": ["Near Shopping Mall", "Free WiFi"]
    }
  ]
}
```

> **Note:** `price` is an object, not a number. Fields may be missing or `null` depending on city/supply source.

</details>

---

### 2) getHotelDetail
Fetch real-time room types, dynamic pricing, inventory, and cancellation policies for a selected hotel.

* **Input Parameters**:
  - `hotelId` (number, optional): Hotel ID. Mutually exclusive with `name`; if both are provided, `hotelId` takes priority.
  - `name` (string, optional): Hotel name (fuzzy match).
  - `dateParam` (object, optional): Check-in / check-out date parameters.
  - `occupancyParam` (object, optional): Guest count and room count parameters.
  - `localeParam` (object, optional): Country and currency parameters.

<details>
<summary><b>🔎 View Detailed Input Parameters (dateParam, occupancyParam, localeParam)</b></summary>

**`dateParam` fields:**
- `checkInDate` (string, optional, format: `YYYY-MM-DD`): Check-in date. Defaults to tomorrow if empty, malformed, or in the past.
- `checkOutDate` (string, optional, format: `YYYY-MM-DD`): Check-out date. Defaults to `checkInDate + 1` day if empty, malformed, or not after check-in.

**`occupancyParam` fields:**
- `adultCount` (number, optional, default: `2`): Adults per room.
- `childCount` (number, optional, default: `0`): Children per room.
- `childAgeDetails` (number[], optional): Child ages, e.g. `[3, 5]`.
- `roomCount` (number, optional, default: `1`): Number of rooms.

**`localeParam` fields:**
- `countryCode` (string, optional, default: `US`): ISO 3166-1 alpha-2 country code.
- `currency` (string, optional, default: `USD`): Currency code.

</details>

<details>
<summary><b>📄 View Response JSON Schema Example</b></summary>

```json
{
  "success": true,
  "errorMessage": null,
  "hotelId": 43615,
  "bookingUrl": "https://rollinggo.ai/pages/hotel/detail/index?...",
  "name": "Sunworld Dynasty Hotel Beijing",
  "checkIn": "2026-03-05",
  "checkOut": "2026-03-06",
  "roomRatePlans": [
    {
      "roomTypeId": 4984714,
      "roomName": "Superior Room",
      "roomNameCn": "高级客房",
      "ratePlanId": "7012072001634754626",
      "ratePlanName": "Superior Room King Bed, 1 King Bed",
      "bedType": 73,
      "bedTypeDescription": "Unknown",
      "currency": "CNY",
      "totalPrice": 0,
      "totalSalesRate": null,
      "inventoryCount": null,
      "isOnRequest": null,
      "recommendIndex": null,
      "cancellationPolicies": [
        {
          "fromDate": "2026-03-02T10:00:00+08:00",
          "toDate": null,
          "amount": 634,
          "percent": null,
          "type": null,
          "description": null
        }
      ],
      "includedFees": null,
      "excludedFees": null,
      "metadata": null
    }
  ]
}
```

> **Note:** On failure, the response may contain an error message (e.g. "Failed to fetch pricing, please retry later") or structured error fields. The `roomRatePlans` array can be long — consider paginating or limiting display on the client side.

</details>

---

### 3) getHotelSearchTags
Retrieve metadata containing all filterable tag names (e.g., "Free WiFi", "Gym", "Kid-Friendly") to refine search filtering. Suitable for local caching and client-side intent mapping.

<details>
<summary><b>📄 View Response JSON Schema Example</b></summary>

```json
{
  "tags": [
    {
      "name": "Free WiFi",
      "category": "Core Amenities",
      "description": "Provides free WiFi"
    }
  ],
  "usageGuide": {
    "tagUsage": "Place tag names into hotelTags.preferredTags (preference), requiredTags (hard requirement), or excludedTags (exclusion)",
    "exampleRequest": "{...}"
  }
}
```

**Common tag categories:**
- Brand & Ratings
- Specialty Highlights
- Core Amenities
- Family & Kids
- Service Details
- Service & Dining
- Transportation & Payment
- Views & Room Types
- Hotel Type
- Pricing

</details>

---

## 📚 Usage Examples

<details>
<summary><b>Example 1: City Search</b></summary>

```json
{
  "originQuery": "Find 4-star+ hotels in Beijing for 2 nights",
  "place": "Beijing",
  "placeType": "city",
  "checkInParam": {
    "checkInDate": "2026-03-01",
    "stayNights": 2
  },
  "filterOptions": {
    "starRatings": [4.0, 5.0]
  },
  "size": 5
}
```

</details>

<details>
<summary><b>Example 2: With Tags and Budget Constraints</b></summary>

```json
{
  "originQuery": "Find quality hotels in Beijing with free WiFi, budget under 1000 per night",
  "place": "Beijing",
  "placeType": "city",
  "hotelTags": {
    "requiredTags": ["Free WiFi"],
    "maxPricePerNight": 1000
  },
  "size": 5
}
```

</details>

<details>
<summary><b>Example 3: Query Hotel Room Types and Pricing</b></summary>

```json
{
  "hotelId": 43615,
  "dateParam": {
    "checkInDate": "2026-03-05",
    "checkOutDate": "2026-03-06"
  },
  "occupancyParam": {
    "adultCount": 2,
    "roomCount": 1
  },
  "localeParam": {
    "currency": "CNY",
    "countryCode": "CN"
  }
}
```

</details>

## 💬 Q&A 
🔍[Troubleshooting Guide](https://global.rollinggo.store/docs/mcp-docs/mcp-config)
<details>
<summary><b>Q&A</b></summary>

**Q1: The client doesn't show the Tool after configuration**
1. Check if the JSON configuration format is correct
2. Confirm that `url` and `type` are correct
3. Confirm that the API Key in the `Authorization` header is correct
4. Restart the client (changes only take effect after restart)

**Q2: Returns 401 Unauthorized**
Invalid or incorrectly formatted API Key:
1. The API Key should start with `mcp_`
2. In `Authorization: Bearer YOUR_API_KEY`, there must be a space after Bearer
3. Make sure there are no extra spaces or line breaks in the API Key

**Q3: Returns 400 Bad Request**
Common when calling directly with cURL. Check that the Accept header is included:
```bash
-H "Accept: application/json, text/event-stream"
```

**Q4: searchHotels returns empty results**
1. Check if `place` and `placeType` match (e.g., "Shanghai Bund" should be paired with "Attraction")
2. Relax filter criteria (remove star rating / tag restrictions)
3. Confirm that `checkInDate` is not in the past

**Q5: Prices don't match actual rates**
Search results show reference prices; real-time prices may vary. Currently, only queries are supported — online booking is not yet available.

</details>


## 💬 Support
* 📧 **Email**: [york.lu@dida.com](mailto:york.lu@dida.com)
* 🐛 **Issues**: Submit issues or feature requests on [GitHub Issues](https://github.com/DIDA-AI/dida_hotel_mcp_global/issues).
* 💬 **Discord Community**: Join our [Discord Server](https://discord.gg/DvKcz7YnH) or scan the QR code below to connect with other developers, discuss integrations, and get real-time support from the DIDA team.


<img src="discord-qr.png" width="300" alt="Discord Support" />

## 🌟 Dida Hotel MCP (OAuth) v2.3 Major Update
v2.3 optimizes the order query structure and adds multiple order detail fields to help Agents better handle check-in, payment, and cancellation scenarios. Note: This update applies to the OAuth integration version only, not the API Key version documented herein. The OAuth version requires business onboarding contact@rollinggo.ai.
<details>
<summary><b>What changed</b></summary>

### Unchanged tools (4)

- `getHotelSearchTags` — Get all enabled hotel filter tags
- `searchHotels` — Search global hotel list by conditions
- `getHotelDetail` — Get available room types and pricing for a hotel
- `hotelPriceConfirm` — Lock real-time final retail price for selected room

### Modified tools (2)

- `createHotelBookingWithPaymentURL` — Removed `alipayUrlScene` parameter; unified `bookingResult.paymentUrl` to generic checkout
- `searchHotelOrders` — Output simplified to 9 core fields (`orderNo`, `hotelName`, `roomName`, `orderStatus`, `totalPrice`, etc.) for list view; full detail moved to dedicated tool

### New tools (1)

- `getHotelOrderDetail` — Query full structured order details by `orderNo`, including `hotelConfirmationNo`, guest list, bed type, contact phones, coordinates, payment/cancellation deadlines, and policy flags

### New fields

- `hotelConfirmationNo` — Hotel-side real confirmation number for front-desk lookup
- `stayInfo.bedTypeStr` — Human-readable bed type description (e.g., `"1 King Bed (1.8m)"`)
- `stayInfo.guestNames` — Official pinyin/English guest name list for verification
- `priceInfo.paymentDeadline` — Payment deadline timestamp (`YYYY-MM-DD HH:mm:ss`) for countdown alerts
- `policyInfo.freeCancelDeadline` — Free cancellation deadline timestamp for refund window checks
- `policyInfo.isCancelable` — Whether free cancellation is still available at the current moment

---
**Tool count:** 7 total (up from 6 in v2.2) — 1 new, 2 modified, 4 unchanged.
</details>

---
# Appendix
## 🔣 If you want local deployment
#### Method A: Run via `uv` (Recommended - Zero Config Setup)
If you have [uv](https://github.com/astral-sh/uv) installed, run the server instantly:
```bash
uv run --with-requirements requirements.txt server.py
```

#### Method B: Standard Python Setup
```bash
# Clone the repository
git clone https://github.com/DIDA-AI/dida_hotel_mcp_global.git
cd dida_hotel_mcp_global

# Setup virtual environment
python -m venv venv
source venv/bin/activate  # On Windows: venv\Scripts\activate

# Install dependencies and run
pip install -r requirements.txt
python server.py
```
The local server will run on `http://localhost:8000/mcp`, automatically forwarding requests to the secure DIDA global API nodes.

---
## 🔑 Security & Headers
* The local server forwards requests to the secure DIDA global API.
* Always supply your API Key in the headers. Keys must start with `mcp_`.
* **Required header**: `Authorization: Bearer mcp_your_key_here`

---

## 📜 License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---
**Made with ❤️ by the DIDA Team**
