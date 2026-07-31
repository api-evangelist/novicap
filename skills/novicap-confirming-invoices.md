---
name: Submit and reconcile confirming (reverse factoring) invoices
description: Upload confirming (Confirming Standard) invoices and reconcile them against payment instructions via the Novicap API.
api: openapi/novicap-openapi.yml
operations:
  - addConfirmingInvoices
  - retrieveConfirmingInvoices
  - retrieveConfirmingPaymentInstructions
---

# Submit and reconcile confirming invoices

Use the Novicap API (`https://api.novicap.com/v1`) to submit invoices under the
**Confirming Standard** (reverse factoring) product and reconcile them against
payment instructions.

## Prerequisites
- An API key generated from your Novicap user profile.
- The `product_id` for your Confirming Standard product.
- Send `Authorization: Bearer {api_key}` and include `product_id` on every request.

## Steps
1. **Add confirming invoices** — `addConfirmingInvoices`
   (`POST /confirming_standard/invoices?product_id={product_id}`). Confirming
   invoice ids use the `I-` prefix (e.g. `I-ABCD`).
2. **List confirming invoices** — `retrieveConfirmingInvoices`
   (`GET /confirming_standard/invoices?product_id={product_id}`) to confirm what
   Novicap holds.
3. **Reconcile payment instructions** — `retrieveConfirmingPaymentInstructions`
   (`GET /confirming_standard/payment_instructions?product_id={product_id}`) to
   see how invoices were grouped and paid.
4. **Remove a mistaken invoice** — `deleteConfirmingInvoice`
   (`DELETE /confirming_standard/invoices/{transaction_id}?product_id={product_id}`)
   before it is committed to a payment instruction.

## Error handling
- `401` unauthorized, `403` endpoint not enabled (contact support@novicap.com),
  `404` unknown transaction id, `4xx` malformed request, `5xx` retry with backoff.
