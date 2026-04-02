# Tapfiliate — GTM Tag Template (Client-Side)

A Google Tag Manager custom tag template for [Tapfiliate](https://tapfiliate.com) affiliate tracking. Handles click detection, conversions, and customer/trial/lead tracking through the official `tapfiliate.js` library.

## Features

- **Click Detection** — Automatically detect affiliate referral parameters on page views
- **Conversion Tracking** — Track purchases with order ID, amount, currency, and customer ID
- **Customer / Trial / Lead** — Register sign-ups for lifetime/recurring commissions
- **Coupon Tracking** — Attribute conversions via coupon codes (single or comma-separated)
- **Commission Types** — Specify different commission type identifiers per conversion
- **Program Groups** — Support multi-site setups with program group identifiers
- **Cross-Subdomain** — Override cookie domain for cross-subdomain tracking
- **Debug Mode** — Console logging for troubleshooting

## Installation

### Option A: Import from Community Gallery

1. In your **web** GTM container, go to **Templates** → **Tag Templates**
2. Click **Search Gallery** and search for "Tapfiliate"
3. Add the template to your container

### Option B: Manual Import

1. Download `template.tpl` from this repository
2. In your web GTM container, go to **Templates** → **Tag Templates** → **New**
3. Click the **⋮** menu → **Import** and select the `.tpl` file
4. Click **Save**

## Setup

### 1. Page View Tag (All Pages)

Create a tag for click detection — this must fire on **every page**.

| Setting | Value |
|---|---|
| Account ID | Your Tapfiliate account ID |
| Event Type | Page View (detect click) |
| Trigger | All Pages |

### 2. Conversion Tag (Thank You / Purchase)

Create a tag for conversion tracking — fires on your purchase confirmation.

| Setting | Value |
|---|---|
| Account ID | Your Tapfiliate account ID |
| Event Type | Conversion |
| Conversion ID | `{{Transaction ID}}` (Data Layer variable) |
| Conversion Amount | `{{Purchase Amount}}` (Data Layer variable) |
| Customer ID | `{{User ID}}` (optional, for lifetime commissions) |
| Currency | `USD` (optional, overrides program default) |
| Trigger | Purchase / Thank You page |

### 3. Customer / Trial / Lead Tag (Sign-Up)

Create a tag for customer registration — fires on sign-up or trial start.

| Setting | Value |
|---|---|
| Account ID | Your Tapfiliate account ID |
| Event Type | Customer / Trial / Lead |
| Customer ID | `{{User ID}}` (Data Layer variable) |
| Trigger | Sign-up / Registration event |

## Template Fields

### Required

| Field | Description |
|---|---|
| **Account ID** | Your Tapfiliate account ID (from Profile Settings) |
| **Event Type** | `page_view`, `conversion`, `customer`, `trial`, or `lead` |

### Conversion Settings (visible when Event Type = Conversion)

| Field | Description |
|---|---|
| Conversion ID | Unique order/transaction ID for cross-referencing and deduplication |
| Conversion Amount | Monetary value for percentage-based commissions |
| Customer ID | Unique customer ID for lifetime/recurring commissions |
| Currency | ISO 4217 code (USD, EUR, GBP, etc.) |
| Coupon Code(s) | Single code or comma-separated list |
| Commission Type | Commission type identifier from program settings |

### Customer Settings (visible when Event Type = Customer/Trial/Lead)

| Field | Description |
|---|---|
| Customer ID | Unique customer identifier (required) |
| Coupon Code(s) | Coupon code(s) for customer attribution (Customer only) |

### Page View Settings (visible when Event Type = Page View)

| Field | Description |
|---|---|
| Referral Code | Explicit referral code (leave empty for auto-detection from URL) |

### Advanced Settings

| Field | Description |
|---|---|
| Cookie Domain | Override cookie domain for cross-subdomain tracking |
| Program Group | Program group identifier for multi-domain setups |
| Debug Logging | Enable console logging for troubleshooting |

## How It Works

The template loads `tapfiliate.js` from Tapfiliate's CDN and then executes the appropriate `tap()` calls based on the selected event type:

1. **Page View**: `tap('create', ...)` → `tap('detect')` — Sets cookie if affiliate referral parameters are found in the URL
2. **Conversion**: `tap('create', ...)` → `tap('conversion', externalId, amount, options, commissionType)` — Records the conversion if a valid affiliate cookie exists
3. **Customer/Trial/Lead**: `tap('create', ...)` → `tap('customer|trial|lead', customerId)` — Registers the customer for future lifetime commissions

## Affiliate Attribution Priority

When a coupon code is provided, it takes precedence over cookie-based (click) attribution. If the coupon isn't associated with an affiliate, Tapfiliate falls back to cookies.

## Testing

1. Enable **Debug Logging** in the Advanced Settings
2. Use GTM **Preview Mode**
3. Check the browser console for `Tapfiliate GTM -` prefixed log messages
4. Verify in your Tapfiliate dashboard that test conversions appear

## Resources

- [Tapfiliate JavaScript Integration Guide](https://tapfiliate.com/docs/integrations/javascript/)
- [Tapfiliate.js Reference](https://tapfiliate.com/docs/javascript/)
- [Tapfiliate REST API](https://tapfiliate.com/docs/rest/)
- [How to Test Your Integration](https://support.tapfiliate.com/en/articles/1680892)

## Author

Created by [New North Digital](https://newnorth.digital?utm_source=github&utm_medium=gtm-template&utm_campaign=tapfiliate-web-tag)

## License

Apache 2.0 — see [LICENSE](LICENSE).
