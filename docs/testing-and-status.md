# Testing and status

## What is measured
| Check | Result | How |
|---|---|---|
| Automated test suite | **991 tests, passing** at the last full run | logic, safety rules, restaurant-file validation, delivery and retry, privacy deletion |
| Conversation evaluation | **52 of 54** (18 scenarios x 3 runs) | a simulated caller talks to the agent; the order is checked by the server |
| Same evaluation, smaller model | 43 of 54 | used to choose the model |
| Independent judge calibration | **31 of 32** hard cases | the only miss fell on the safe side |
| Four calls at once | 4 of 4 | simulated load |
| Confirmation turn | about 1.3 seconds on average | "yes" to saved order and spoken reply, in the simulator |
| Speech-recognition comparison | lowest error and latency among four | **synthetic** phone-quality clips, not real callers |

## What is not tested
- **Any real phone call**, including live hand-off to staff and recovery when the server restarts mid-call
- Heavy background noise with other people talking (such orders go to staff as "unconfirmed", which is safe but not automatic)
- Silence before the greeting on a real line: the simulator measured 3 to 7 seconds on the first call
- A real restaurant's menu, hours and staff workflow
- Behaviour over days, or under real load on a real server

## Known limitations
Priced options and extras, a one-order-per-call limit, end-of-day "sold out" without a deployment, payment and requested time are listed as future work.

## Next steps
1. Finish the telephony verification and connect a test number
2. Deploy to a server and run a first real call
3. Listen to the final voice on a real phone line
4. One real pilot restaurant with its own data, pronunciation check and staff training
5. Legal advice, then the privacy notice
