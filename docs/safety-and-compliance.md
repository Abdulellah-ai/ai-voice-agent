# Safety, privacy and compliance

## Enforced in code
- **AI disclosure:** the greeting must identify the assistant as automated; a greeting that denies it, or omits it, is rejected when the restaurant file is loaded
- **No order without an explicit yes** after a server-built read-back
- **No order saved twice:** locked against double saving, cancelling and closing
- **Unconfirmed or incomplete orders reach staff** when a call ends or the server crashes, instead of being lost
- **Staff hand-off** only on a request, complaint, accepted offer, repeated unclear answers, or an internal failure
- **Startup refuses to run** with a broken restaurant file (missing alert channel, bad number, invalid time, empty menu) and names the exact field in plain language
- **Call links are signed, single-use and tied to one call;** the telephony signature is verified; concurrent calls and call duration are capped
- **Logs contain no phone numbers and no caller speech**

## Privacy
- Caller audio is not stored
- Scheduled deletion: call transcripts and caller number after a short retention period, and name, address and phone inside orders after a longer one. The periods are one setting each, pending legal advice
- The recognition provider can be pinned to a European region

## What is not done
- No privacy notice or consent wording is published; legal review is pending
- A data-protection officer and breach-notification procedure are not set up
- No restaurant data has been loaded in a real deployment
- The outbound-calling controls of the first version (consent list, do-not-call, hours) do **not** exist in the current version, because it does not place calls
