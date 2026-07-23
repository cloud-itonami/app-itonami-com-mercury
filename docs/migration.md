# Stripe and Mercury migration

## Preconditions

- Mercury account approved in the company's exact legal name
- US Stripe Payments account activated for the same legal entity
- Mercury payout account verified in that Stripe account
- live secret and webhook signing secrets stored only in the operator vault
- application checkout paused during the cutover window

## Cutover

1. Record the predecessor Stripe account ID and final reconciliation cursor.
2. Create the production webhook in the target Stripe account.
3. Install `STRIPE_SECRET_KEY` and `STRIPE_WEBHOOK_SECRET` through the hosting
   platform's secret input without printing either value.
4. Confirm `/v1/account` reports country `US` and legal name
   equal to the configured expected entity before re-enabling checkout.
5. Create a Checkout Session without charging a card.
6. Execute one approved low-value live purchase.
7. Verify DID credit issuance, Stripe balance, Mercury payout and ledger totals.
8. Execute an approved exception refund and verify cumulative reversal.
9. Disable the old webhook only after its outstanding events are reconciled.

## Automated fail-closed assertions

- seller account country must be `US`
- seller legal name must normalize to `EXPECTED_LEGAL_NAME`
- charges and payouts must both be enabled
- no currently-due or past-due Stripe requirements
- webhook endpoint URL must equal the configured production callback URL
- payout destination identifiers are compared by non-secret fingerprint only
- any mismatch prevents deployment or checkout activation
