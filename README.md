# Tapfiliate Client-Side GTM Tag Template

## Overview

The Tapfiliate Client-Side GTM Tag Template enables seamless integration of Tapfiliate affiliate tracking directly through Google Tag Manager. This template handles click detection, conversion tracking, and customer lifecycle events using Tapfiliate's JavaScript SDK.

## Features

- **Page View Tracking**: Automatic affiliate click detection
- **Conversion Tracking**: Track purchases with detailed conversion data
- **Customer Lifecycle**: Track customer sign-ups, trials, and leads
- **Advanced Configuration**: Support for cross-domain tracking, custom currencies, and program groups
- **Debug Mode**: Optional console logging for troubleshooting

## Installation

### 1. Import the Template

1. In Google Tag Manager, go to **Templates** → **Tag Templates**
2. Click **New** and select **Import**
3. Upload the Tapfiliate Client-Side template file
4. Save the template

### 2. Create Template Variables

Create the following variables in GTM:

| Variable Name | Type | Description | Required |
|---------------|------|-------------|----------|
| `account_id` | Text | Your Tapfiliate account ID | ✅ |
| `conversion_id` | Text | Unique conversion identifier | ⚠️ |
| `conversion_value` | Number | Conversion amount | ⚠️ |
| `customer_id` | Text | Customer identifier | ⚠️ |
| `cookie_domain` | Text | Domain for cookie placement | ❌ |
| `referral_code` | Text | Explicit referral code | ❌ |
| `currency` | Text | 3-letter ISO currency code | ❌ |
| `program_group` | Text | Program group ID | ❌ |


**Legend**: ✅ Required | ⚠️ Required for specific events | ❌ Optional

### 3. Configure Event Type Dropdown

Set up the `event_type` variable with these options:
- `page_view`
- `conversion`
- `customer`
- `trial`
- `lead`

## Configuration Guide

### Page View Tracking

**Purpose**: Detects affiliate clicks and sets tracking cookies.

**Required Variables**:
- `account_id`
- `event_type` = `page_view`

**Optional Variables**:
- `referral_code` - For explicit referral code tracking
- `cookie_domain` - For cross-subdomain tracking
- `program_group` - For multi-domain setups

**Example Setup**:
```
Account ID: 12345-abcde
Event Type: page_view
Cookie Domain: .yourdomain.com
```

### Conversion Tracking

**Purpose**: Tracks completed purchases or conversions.

**Required Variables**:
- `account_id`
- `event_type` = `conversion`

**Recommended Variables**:
- `conversion_id` - Unique order/transaction ID
- `conversion_value` - Purchase amount
- `customer_id` - For lifetime commission tracking
- `currency` - For multi-currency setups

**Example Setup**:
```
Account ID: 12345-abcde
Event Type: conversion
Conversion ID: {{Transaction ID}}
Conversion Value: {{Purchase Amount}}
Customer ID: {{User ID}}
Currency: USD
```

### Customer Lifecycle Tracking

**Purpose**: Tracks customer sign-ups, trials, and leads.

**Required Variables**:
- `account_id`
- `event_type` = `customer`, `trial`, or `lead`
- `customer_id`

**Example Setup**:
```
Account ID: 12345-abcde
Event Type: trial
Customer ID: {{User ID}}
```

## Event Types Explained

### page_view
- Scans URL for affiliate parameters
- Sets tracking cookie when affiliate traffic detected
- Should be placed on all pages
- No external calls made for non-affiliate traffic

### conversion
- Records completed purchases/conversions
- Calculates affiliate commissions
- Supports percentage and fixed commissions
- Enables deduplication via conversion_id

### customer
- Tracks general customer sign-ups
- Sets customer status to "new"
- Enables future lifetime commission tracking

### trial
- Tracks trial sign-ups
- Sets customer status to "trial"
- Ideal for SaaS and subscription businesses

### lead
- Tracks lead generation
- Sets customer status to "lead"
- Perfect for lead-gen and offline sales funnels

## Advanced Features

### Cross-Domain Tracking

When visitors navigate between subdomains:

```
Cookie Domain: .yourdomain.com
```

This ensures the affiliate cookie works across:
- shop.yourdomain.com
- checkout.yourdomain.com
- account.yourdomain.com

### Multi-Currency Support

For international businesses:

```
Currency: EUR
Currency: GBP
Currency: JPY
```

Overrides your program's default currency per conversion.

### Program Groups

For businesses with multiple brands/domains:

```
Program Group: brand-a
Program Group: brand-b
```

Enables separate tracking for different business units.

### Explicit Referral Codes

Force specific affiliate attribution:

```
Referral Code: partner123
```

Useful for:
- Direct partnerships
- Campaign-specific tracking
- Manual attribution

## Triggers and Setup

### Recommended Trigger Setup

| Event Type | Trigger | Notes |
|------------|---------|-------|
| page_view | All Pages | Essential for click detection |
| conversion | Purchase Complete | Thank you/confirmation page |
| customer | Form Submission | Sign-up completion |
| trial | Trial Started | After trial activation |
| lead | Lead Form Submit | Lead capture completion |

### Data Layer Integration

The template works with standard e-commerce data layers:

```javascript
// Page view - automatic

// Conversion
dataLayer.push({
  'event': 'purchase',
  'transaction_id': 'ORD-12345',
  'value': 99.99,
  'currency': 'USD',
  'user_id': 'USER-67890'
});

// Customer sign-up
dataLayer.push({
  'event': 'sign_up',
  'user_id': 'USER-67890'
});
```

## Troubleshooting

### Enable Debug Mode

1. Check the `debug` checkbox in your tag configuration
2. Open browser console (F12)
3. Look for messages starting with "Tapfiliate GTM -"

### Common Debug Messages

```
✅ "Tapfiliate GTM - Script loaded successfully"
✅ "Tapfiliate GTM - Tap conversion call completed"
❌ "Tapfiliate GTM - Script failed to load"
```

### Validation with Tapfiliate Debugger

Use Tapfiliate's built-in debugger to verify tracking:

1. Add `?tapfiliate_debug=1` to your URL
2. Check for tracking confirmation messages
3. Verify click and conversion data

### Common Issues

**Issue**: "Script loaded but no actions performed"
**Solution**: Ensure both `tap('create')` and event method are called

**Issue**: Conversions not attributing correctly
**Solution**: Verify page view tracking is active on landing pages

**Issue**: Cross-domain tracking not working
**Solution**: Set `cookie_domain` to your main domain (e.g., `.yourdomain.com`)

## Support

For technical issues:
1. Enable debug mode and check console logs
2. Verify all required variables are populated
3. Test with Tapfiliate's debugger tool
4. Contact your implementation team with specific error messages

For Tapfiliate-specific questions, consult the [official documentation](https://tapfiliate.com/docs/) or contact Tapfiliate support.
