---
name: Submit dynamic-discounting invoices and create a payment instruction
description: Register suppliers, upload dynamic-discounting invoices, then create a payment instruction to fund them via the Novicap API.
api: openapi/novicap-openapi.yml
operations:
  - addDynamicDiscountingSuppliers
  - addDynamicDiscountingInvoices
  - retrieveDynamicDiscountingInvoices
  - createDynamicDiscountingPaymentInstruction
---

# Submit dynamic-discounting invoices and fund them

Use the Novicap API (`https://api.novicap.com/v1`) to onboard suppliers, submit
invoices, and create a payment instruction under the **Dynamic Discounting** product.

## Prerequisites
- An API key generated from your Novicap user profile.
- The `product_id` for your Dynamic Discounting product.
- Send the key on every call as `Authorization: Bearer {api_key}` and include
  `product_id` on every request (query parameter or JSON field).

## Steps
1. **Add suppliers** — `addDynamicDiscountingSuppliers`
   (`POST /dynamic_discounting/suppliers?product_id={product_id}`). Register the
   suppliers whose invoices you will submit.
2. **Add invoices** — `addDynamicDiscountingInvoices`
   (`POST /dynamic_discounting/invoices?product_id={product_id}`). Each invoice
   is keyed on `(supplier, reference)`; re-posting the same pair updates the
   invoice as long as it is not already part of a payment instruction (there is
   no `Idempotency-Key` header — this dedup-on-reference is the only safeguard).
3. **Verify** — `retrieveDynamicDiscountingInvoices`
   (`GET /dynamic_discounting/invoices?product_id={product_id}`) and confirm the
   invoices are present and correct before funding.
4. **Create the payment instruction** — `createDynamicDiscountingPaymentInstruction`
   (`POST /dynamic_discounting/payment_instructions?product_id={product_id}`).
   Once an invoice is included in a payment instruction it can no longer be
   updated by re-posting.

## Error handling
- `401` — missing/invalid API key.
- `403` — the endpoint is not enabled for your account; email support@novicap.com.
- `400` — malformed request. `404` — unknown transaction id on delete/lookup.
- `5xx` (500/502/503/504) — retry with backoff.
