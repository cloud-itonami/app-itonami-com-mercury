# Security boundary

- Secrets enter commands through vault references or standard input.
- Logs may contain account IDs and webhook endpoint IDs, but never secret keys,
  signing secrets, bank account/routing numbers, identity data, or statements.
- CI uses test-mode Stripe accounts only.
- Production rotation requires two-person review and a recorded rollback point.
- Reconciliation exports are encrypted outside Git; commits contain only
  aggregate totals and opaque event IDs needed for audit.
