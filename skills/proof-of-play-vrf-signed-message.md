---
name: Get a signed vRNG message
description: >-
  Retrieve an off-chain signature for a Proof of Play verified random number
  (vRNG) request so a third party can deliver the random number on-chain by
  calling deliverSignedRandomNumber.
api: openapi/proof-of-play-vrf-openapi.json
operations:
- PublicController_process
generated: '2026-07-20'
method: generated
source: openapi/proof-of-play-vrf-openapi.json
---

# Get a signed vRNG message

Proof of Play's vRNG (Verified Random Number Generator) produces fast, secure,
on-chain randomness for games and apps. When a request has been made on-chain,
this API returns a signed message an integrator can use to manually retry
delivery of the random number by calling `deliverSignedRandomNumber`.

## Prerequisites

- A bearer token for the vRNG API (`Authorization: Bearer <token>`). The
  OpenAPI declares a global `bearerAuth` requirement.
- The `chainId` of the chain the vRNG request was made on.
- The `txHash` of the transaction that created the vRNG request.

## Steps

1. **Call the signed-message endpoint.** Invoke operation
   `PublicController_process`:
   `GET https://staging.vrf.proofofplay.com/v1/vrf/{chainId}/{txHash}`
   with the `Authorization: Bearer <token>` header. Path params `chainId`
   (number) and `txHash` (string) identify the request.
2. **Read the response.** On `200` the body
   (`PublicProcessVrfTransactionResponseDto`) contains `requestId`,
   `roundNumber`, `randomNumber`, and `signature`.
3. **Deliver on-chain.** Use the returned `randomNumber` and `signature` to
   call `deliverSignedRandomNumber` on the target contract, completing the
   random-number delivery.

## Notes

- The published server host is `staging` only; confirm the production host with
  Proof of Play before relying on it.
- The operation is a `GET` and naturally idempotent; retrying is safe.
- Only a `200` response is documented; handle non-200 responses defensively.
