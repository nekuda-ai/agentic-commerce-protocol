# Checkout Functionality Parity Analysis

**Date:** 2026-01-22  
**Status:** Draft  
**Purpose:** High-level analysis of checkout capabilities to achieve feature parity

---

## Executive Summary

This document identifies gaps in the Agentic Checkout Specification where additional functionality is needed to achieve comprehensive parity with industry-standard checkout protocols. The analysis is organized by functional area, with each area representing a potential SEP (Specification Enhancement Proposal).

---

## 1. Line Item Metadata & Product Details

**Current State:**
- Basic line item fields: `id`, `item`, `base_amount`, `discount`, `subtotal`, `tax`, `total`
- Optional fields: `name`, `description`, `images`, `unit_amount`
- Support for `disclosures`, `custom_attributes`, `marketplace_seller_details`

**Gaps Identified:**
- **Product identifiers**: No dedicated field for `product_id`, `sku`, or `variant_id`
- **Product variants**: No structured way to represent size, color, or other variant options
- **Quantity limits**: No fields for `max_quantity`, `min_quantity`, or quantity restrictions
- **Availability information**: No `in_stock`, `low_stock`, or availability status fields
- **Product categorization**: No category, tags, or taxonomy fields
- **Weight/dimensions**: No physical product attributes for shipping calculations

**Impact:** Medium - Affects inventory management, variant selection, and rich product displays

---

## 2. Fulfillment Sophistication

**Current State:**
- Support for `shipping` and `digital` fulfillment types
- Basic shipping fields: `carrier`, `earliest_delivery_time`, `latest_delivery_time`
- Simple item-to-option mapping

**Gaps Identified:**
- **Pickup options**: No support for in-store pickup, curbside pickup, or BOPIS (Buy Online Pickup In Store)
- **Local delivery**: No dedicated local/same-day delivery option type
- **Split shipments**: Limited support for complex multi-origin fulfillment
- **Fulfillment grouping**: No concept of "fulfillment expectations" (grouping items by destination/method)
- **Pre-orders**: No fields for `fulfillable_on` or backorder handling
- **Delivery windows**: Limited flexibility for time-slot based delivery
- **Store/warehouse selection**: No location picker for multi-location fulfillment

**Impact:** High - Critical for omnichannel retail and modern fulfillment models

---

## 3. Advanced Discounts & Promotions

**Current State:**
- Line-level `discount` field (integer amount)
- `items_discount` in totals
- Generic `discount` total type

**Gaps Identified:**
- **Coupon/promo codes**: No dedicated field to apply and validate promo codes
- **Discount metadata**: No description, code, or promotion details
- **Discount rules**: No indication of discount type (percentage, fixed, BOGO, etc.)
- **Stackability**: No way to represent multiple concurrent promotions
- **Automatic discounts**: No distinction between applied vs. available discounts
- **Loyalty points**: No integration with rewards or points systems
- **Volume discounts**: No tiered pricing based on quantity

**Impact:** High - Essential for marketing and promotional campaigns

---

## 4. Tax Handling & Compliance

**Current State:**
- Line-level `tax` field (integer amount)
- Total-level `tax` aggregation
- Integer amounts in minor units

**Gaps Identified:**
- **Tax exemptions**: No `tax_exempt` flag or exemption certificate fields
- **Tax identifiers**: No buyer tax ID, VAT number, or business registration fields
- **Tax breakdown**: No jurisdiction-level tax details (state, county, city)
- **Tax rates**: No percentage or rate information exposed
- **Reverse charge**: No VAT reverse charge mechanism for B2B
- **Tax calculation mode**: No indication of tax-included vs. tax-exclusive pricing
- **Nexus information**: No seller location or tax collection requirements

**Impact:** Medium-High - Critical for international and B2B transactions

---

## 5. Buyer Information & Accounts

**Current State:**
- Optional `buyer` object with `first_name`, `last_name`, `email`, `phone_number`
- Separate `fulfillment_details` with shipping information

**Gaps Identified:**
- **Buyer identification**: No `buyer_id`, `customer_id`, or account reference
- **Guest vs. authenticated**: No indicator of buyer authentication status
- **Buyer type**: No B2B vs. B2C distinction or business buyer fields
- **Company information**: No company name, department, or business details
- **Multiple contacts**: No separate billing contact vs. shipping contact
- **Buyer preferences**: No saved preferences, language, or communication settings
- **Account history**: No reference to previous orders or loyalty status

**Impact:** Medium - Important for returning customers and B2B flows

---

## 6. Payment Method Diversity

**Current State:**
- Single provider (`stripe`)
- Card-only support with network filtering (`amex`, `discover`, `mastercard`, `visa`)
- Delegated payment via token

**Gaps Identified:**
- **Digital wallets**: No explicit Google Pay, Apple Pay, PayPal, or Venmo handlers
- **Bank transfers**: No ACH, SEPA, wire transfer, or direct debit options
- **Buy now, pay later**: No Affirm, Klarna, Afterpay, or installment payment methods
- **Cryptocurrency**: No Bitcoin, stablecoins, or crypto payment options
- **Gift cards**: No gift card or store credit application
- **Multiple payment methods**: No split payment across multiple instruments
- **Saved payment methods**: No reference to stored payment instruments
- **Payment method selection**: Limited flexibility in payment handler configuration

**Impact:** High - Critical for conversion optimization and global reach

---

## 7. Order Notes & Special Instructions

**Current State:**
- Generic `messages[]` array for info/error communication
- `content` field in messages (plain or markdown)

**Gaps Identified:**
- **Gift messages**: No dedicated field for gift messages or gift wrapping
- **Order notes**: No buyer-to-seller communication field
- **Delivery instructions**: No specific field for delivery preferences
- **Special requests**: No structured special handling instructions
- **Gift wrapping**: No gift service selection or configuration
- **Personalization**: No product personalization or customization fields

**Impact:** Medium - Affects customer experience and fulfillment accuracy

---

## 8. Subscriptions & Recurring Billing

**Current State:**
- One-time checkout only
- No recurring payment support

**Gaps Identified:**
- **Subscription line items**: No indication of subscription vs. one-time purchase
- **Billing frequency**: No interval (weekly, monthly, yearly) specification
- **Trial periods**: No free trial or introductory pricing
- **Subscription management**: No pause, cancel, or modify subscription actions
- **Renewal dates**: No next billing date or renewal information
- **Usage-based billing**: No metered or consumption-based pricing
- **Contract terms**: No commitment period or contract length

**Impact:** High - Blocking for SaaS and subscription-based businesses

---

## 9. Multi-Currency & Localization

**Current State:**
- Single `currency` field (ISO 4217)
- No explicit locale support

**Gaps Identified:**
- **Currency conversion**: No foreign exchange rates or converted amounts
- **Locale preferences**: No `Accept-Language` handling or locale specification
- **Regional pricing**: No geo-based pricing variations
- **Currency display**: No formatting preferences or symbol positioning
- **Multi-currency selection**: No buyer currency selection mechanism
- **Exchange rate disclosure**: No rate transparency for converted amounts

**Impact:** Medium-High - Essential for international merchants

---

## 10. Order Confirmation & Post-Checkout

**Current State:**
- Simple `order` object with `id`, `checkout_session_id`, `permalink_url`
- Separate webhook spec for order updates

**Gaps Identified:**
- **Order status**: No status beyond `completed` at checkout completion
- **Confirmation details**: Limited order confirmation information
- **Estimated delivery**: No consolidated delivery promise at order level
- **Order timeline**: No key dates (processing, shipping, delivery)
- **Support information**: No customer service contact or help resources
- **Invoice/receipt**: No invoice number or receipt download link

**Impact:** Low-Medium - Affects post-purchase experience

---

## 11. Session Management & Expiry

**Current State:**
- Session lifecycle via `status` field
- Standard CRUD operations (create, update, retrieve, complete, cancel)

**Gaps Identified:**
- **Expiry timestamps**: No `expires_at` field for session TTL
- **Session extension**: No mechanism to extend expired sessions
- **Session recovery**: No `continue_url` for checkout handoff
- **Session locking**: No concurrency control or optimistic locking
- **Session metadata**: No created_at, updated_at, or audit timestamps
- **Abandoned cart**: No abandoned cart recovery or reminder mechanisms

**Impact:** Medium - Affects session reliability and cart recovery

---

## 12. Real-Time Inventory & Availability

**Current State:**
- No explicit inventory fields
- Item availability implied by successful session creation

**Gaps Identified:**
- **Stock quantities**: No `available_quantity` or `stock_status` fields
- **Low stock warnings**: No threshold-based messaging
- **Out of stock handling**: No backorder or waitlist options
- **Pre-order support**: No future availability date fields
- **Location-based inventory**: No store/warehouse inventory visibility
- **Real-time updates**: No inventory checks during session lifecycle
- **Quantity limits**: No max per customer or bulk purchase restrictions

**Impact:** Medium - Affects inventory accuracy and buyer expectations

---

## 13. B2B-Specific Features

**Current State:**
- Generic buyer and checkout models
- No B2B-specific fields

**Gaps Identified:**
- **Purchase orders**: No PO number field or PO-based payment
- **Net terms**: No invoice payment with payment terms (Net 30, Net 60)
- **Quote workflow**: No quote-to-order conversion
- **Approval workflows**: No multi-step approval process
- **Cost centers**: No department or budget allocation
- **Business tax IDs**: No VAT, EIN, or business registration numbers
- **Contract pricing**: No customer-specific or tiered pricing
- **Bulk ordering**: No minimum order quantities or bulk discounts

**Impact:** High - Blocking for B2B commerce use cases

---

## 14. Enhanced Error Handling & Validation

**Current State:**
- Flat error structure with `type`, `code`, `message`, `param`
- Error types: `invalid_request`, `request_not_idempotent`, `processing_error`, `service_unavailable`
- Message-level errors with codes: `missing`, `invalid`, `out_of_stock`, `payment_declined`, `requires_sign_in`, `requires_3ds`

**Gaps Identified:**
- **Error recovery suggestions**: No actionable remediation guidance
- **Field-level validation**: Limited granular validation feedback
- **Async validation**: No progressive validation during checkout flow
- **Warning vs. error**: No severity levels beyond error/info
- **Localized errors**: No multi-language error messages
- **Error tracking**: No error correlation IDs or debugging aids

**Impact:** Low-Medium - Affects developer experience and debugging

---

## 15. Analytics & Attribution

**Current State:**
- Optional `affiliate_attribution` support
- Basic attribution fields: `provider`, `token`, `publisher_id`, `campaign_id`, etc.
- First-touch and last-touch attribution

**Gaps Identified:**
- **Session attribution**: No traffic source, referrer, or campaign tracking
- **Multi-touch attribution**: Limited to first/last touch models
- **Conversion tracking**: No conversion pixels or analytics integration
- **UTM parameters**: No dedicated fields for marketing parameters
- **A/B testing**: No experiment or variant tracking
- **Funnel tracking**: No step-level analytics
- **Custom dimensions**: Limited custom metadata for analytics

**Impact:** Low-Medium - Affects marketing measurement and optimization

---

## 16. Payment Security & Compliance

**Current State:**
- 3D Secure support via `authentication_metadata` and `authentication_result`
- Request signing with `Signature` and `Timestamp` headers
- Bearer token authentication

**Gaps Identified:**
- **Strong Customer Authentication (SCA)**: 3DS is present, but no broader SCA framework
- **PSD2 compliance**: No explicit PSD2 regulatory fields
- **CVV handling**: No card verification requirements
- **Address verification**: No AVS (Address Verification Service) results
- **Risk scoring**: No fraud score or risk assessment fields
- **2FA/MFA**: No multi-factor authentication for high-value orders
- **Biometric auth**: No biometric authentication support

**Impact:** Medium - Important for security and regulatory compliance

---

## 17. Accessibility & Compliance Features

**Current State:**
- Links for `terms_of_use`, `privacy_policy`, `return_policy`
- Content in `plain` or `markdown` format

**Gaps Identified:**
- **WCAG compliance**: No accessibility metadata or ARIA labels
- **Screen reader support**: No semantic structure for assistive tech
- **Keyboard navigation**: No tab order or focus management hints
- **Legal disclosures**: Limited to three link types
- **Age verification**: No age gate or restricted product handling
- **Regional restrictions**: No geo-blocking or market restrictions
- **GDPR compliance**: No consent management or data processing agreements

**Impact:** Low-Medium - Important for legal compliance and inclusion

---

## 18. Performance & Optimization

**Current State:**
- Idempotency support via `Idempotency-Key`
- Request/response correlation via `Request-Id`
- API versioning via `API-Version` header

**Gaps Identified:**
- **Partial updates**: No PATCH support for incremental changes
- **Conditional requests**: No ETag or If-Modified-Since support
- **Response pagination**: Not applicable but could affect fulfillment options
- **Field filtering**: No sparse fieldsets or response filtering
- **Batch operations**: No bulk update or batch API
- **Webhooks**: Separate spec, but no webhook retry policies in main spec
- **Rate limiting**: No rate limit headers or throttling guidance

**Impact:** Low - Affects scalability and developer experience

---

## Priority Recommendations

### High Priority (Critical for Parity)
1. **Fulfillment Sophistication** - Pickup, local delivery, split shipments
2. **Advanced Discounts & Promotions** - Coupon codes, promotion engine
3. **Payment Method Diversity** - Digital wallets, BNPL, alternative payments
4. **Subscriptions & Recurring Billing** - Recurring payment support
5. **B2B-Specific Features** - PO numbers, net terms, approval workflows

### Medium Priority (Important for Completeness)
6. **Line Item Metadata** - Product IDs, SKUs, variants
7. **Tax Handling & Compliance** - Exemptions, breakdowns, VAT
8. **Buyer Information & Accounts** - Customer IDs, account references
9. **Multi-Currency & Localization** - Currency conversion, locale support
10. **Real-Time Inventory** - Stock levels, availability status
11. **Session Management** - Expiry, recovery URLs

### Low Priority (Nice-to-Have Enhancements)
12. **Order Notes & Special Instructions** - Gift messages, delivery instructions
13. **Order Confirmation** - Enhanced post-checkout details
14. **Enhanced Error Handling** - Recovery suggestions, localized errors
15. **Analytics & Attribution** - Enhanced tracking beyond affiliate attribution
16. **Accessibility & Compliance** - WCAG, GDPR enhancements
17. **Performance & Optimization** - PATCH, conditional requests

---

## Next Steps

For each high and medium priority area, create a detailed SEP that:
1. Analyzes the specific fields and structures needed
2. Proposes schema additions with field-level specifications
3. Defines validation rules and error handling
4. Provides implementation examples
5. Addresses backward compatibility

These SEPs should be created independently and can be implemented in any order based on merchant needs and business priorities.

