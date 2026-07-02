---
name: gtm-eshop-quickstart
description: Fast GTM-only setup for e-shop projects using the WooCommerce template container pattern. Use when user asks to set up GTM from scratch or onboard a new e-shop into GTM quickly and safely.
---

# gtm-eshop-quickstart

Purpose: Execute a repeatable, GTM-only onboarding flow for e-shops with minimal mistakes.

Scope: Google Tag Manager only. Ignore GA4/Ads account creation except for IDs/labels required inside GTM.

## Source baseline for this skill
- Playbook source: `/Users/luigigoulianos/Downloads/Πώς στήνω από το μηδέν tracking για eshop (2).pdf`
- Reference container source: `/Users/luigigoulianos/Downloads/GTM-NLN7F6HF_workspace2 (1).json`
- Reference container public ID: `GTM-NLN7F6HF`

## When to use
- New e-shop onboarding where GTM must be set up quickly.
- Existing site needs standard WooCommerce ecommerce tracking import.
- User explicitly says only GTM matters.

## Hard rules
1. Never create a second GTM container for the same domain.
2. Never import with `Overwrite` unless target container is truly fresh/empty.
3. Never publish before Preview/Tag Assistant validation.
4. Do not edit Data Layer Variables unless developer explicitly asks.
5. Do not leave template default conversion labels from another account.

## Fast workflow

### 1) Container targeting
- Confirm brand domain and intended container.
- In GTM, verify account/container naming:
- Account Name = brand/company name.
- Container Name = bare domain (no `https://`).
- Platform = `Web`.

### 2) Installation check on site
- Ensure GTM snippet is installed once (no duplicate containers).
- Validate in Tag Assistant (`https://tagassistant.google.com/`) that exactly one expected `GTM-XXXXXXX` is detected.

### 3) Import strategy
- Go to `Admin -> Import Container`.
- Choose template JSON.
- Workspace: default or task workspace.
- If container is empty: use `Overwrite`.
- If container has existing setup: stop and use merge strategy only after explicit approval.

### 4) Mandatory replacements after import
Update only these values first:
- `Google Analytics | GA4 | Tag` constant -> GA4 Measurement ID (format `G-XXXXXXXXXX`).
- `Google Ads Conversion ID` constant -> Ads Conversion ID.
- Every Google Ads conversion tag `conversionLabel` -> correct label per event.

Do not reuse the same label across different events.

### 5) Validate trigger/event mapping
Expected ecommerce events to exist and fire:
- `view_item`
- `add_to_cart`
- `begin_checkout`
- `purchase`

Regex trigger pattern should cover at least:
- `view_item|add_to_cart|view_cart|begin_checkout|purchase`

### 6) Preview QA (required)
In GTM Preview + Tag Assistant, verify:
- Product page -> `view_item` tags fire.
- Add to cart action -> `add_to_cart` tags fire.
- Checkout start -> `begin_checkout` tags fire.
- Order success/thank-you -> `purchase` tags fire.
- No unexpected duplicate firing for the same event.

### 7) Publish gate
Publish only when all are true:
- IDs updated.
- All conversion labels replaced.
- Preview passed for all key events.
- No duplicate GTM installation.

## Reference template map (from GTM-NLN7F6HF)

### Tags found
- `Google Analytics | GA4 | Tag` (`googtag`)
- `Google Analytics | Ecommerce Events` (`gaawe`)
- `GA4 Event | Add to Cart` (`gaawe`)
- `Google Ads | G Tag` (`googtag`)
- `Google Ads | Conversion Linker` (`gclidw`)
- `Google Ads | Remarketing` (`sp`)
- `Google Ads | View Item` (`awct`)
- `Google Ads | Add to Cart` (`awct`)
- `Google Ads | Checkout` (`awct`)
- `Google Ads | Purchase` (`awct`)

### Triggers found
- `Ecommerce | All events Regex Trigger` (custom event regex)
- `View Item Trigger`
- `Add to Cart Trigger`
- `Begin Checkout Trigger`
- `Purchase Trigger`
- `Click | Add to Cart Button`
- `Reusable Event Trigger (by Variable)`

### Variables found (key)
- `Google Analytics | GA4 | Tag` constant (reference value in template: `G-CV839B1Y7G`)
- `Google Ads Conversion ID` constant (reference value in template: `16731331971`)
- `Google Ads | G Tag` constant (reference value in template: `AW-16731331971`)
- `Ecommerce Value`
- `Ecommerce Transaction ID`
- `Ecommerce Items`
- `Ecommerce Currency`
- `User-Provided Data (Web-Auto)`

### Known conversion labels in reference template (must be replaced for new account)
- Purchase: `Mms9CPjwld0ZEIO7j6o-`
- Checkout: `RkihCK637toZEIO7j6o-`
- View Item: `oK9YCO-BvIIaEIO7j6o-`
- Add to Cart: `pbDGCKbtzeoZEIO7j6o-`

## Common failure checks
- Wrong container/domain selected.
- Imported into non-empty container with overwrite.
- Forgot one conversion label.
- Same conversion label used on multiple events.
- Publish done without purchase-path test.
- Duplicate GTM snippets/plugins on WordPress.

## Output format this skill should return
When executing this skill, always produce:
1. Container targeted (`account`, `container`, `workspace`).
2. What was imported and with which mode.
3. Exact fields changed (old -> new for IDs/labels).
4. Preview test result by event (`view_item`, `add_to_cart`, `begin_checkout`, `purchase`).
5. Final publish recommendation: `READY` or `BLOCKED` with reason.
