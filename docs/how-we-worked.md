# How we worked: research, discoveries, and what we changed

This project did not start as a restaurant receptionist. It started as a general-purpose multilingual voice agent that could answer *and place* calls, and it was reshaped step by step, driven by what each round of testing and research uncovered. The honest version of that story, including the dead ends:

## 1. The first version: a general voice agent
A pipeline of speech recognition, a small language model and a voice, with one configuration file per language and call controls in code (consent, do-not-call list, permitted hours in the callee's time zone, day off, attempt limit, AI disclosure). It worked end to end on a laptop microphone and speaker.

**What we discovered:** it was slow, the voice was unpleasant, there was echo, and sometimes it heard the speaker but did not answer. Automated tests passed, but they only proved the logic was consistent, not that it sounded acceptable.
**What we changed:** we stopped treating "tests pass" as "it is good", and moved to measuring latency per stage and judging voices by ear.

## 2. Narrowing the use case
The first version's weaknesses (the next section) were serious enough that the project was rebuilt from the ground up, and the rebuild focused on one narrow, repeatable case: a restaurant's **incoming** calls (orders, bookings, opening hours).
**What we changed:** the current version only receives calls. The outbound dialer and its consent controls were left behind with the first version.

## 3. A basic defect: the order never reached the restaurant
The first design stored an order as free text and nothing guaranteed it reached the kitchen. There were no prices and no history, and the telephone path it assumed did not work technically.
**What we changed:** a structured order, server-computed prices, and a database write that stores the order and its kitchen ticket in one atomic step, with retries and a manual-handling path if delivery fails.

## 4. Who decides? We moved decisions out of the model
Early tests showed the model could save an order the caller had not agreed to, or close a call while a question was still open.
**What we changed:** the model only understands and talks. The server decides: an order is saved only after the server reads it back and the caller agrees.

## 5. Understanding "yes": from word lists to an independent judge
The first version recognised confirmation with word lists. Real phrasing broke it (a natural "yeah, send it" was missed; unrelated words matched).
**What we changed:** a separate, independent model call reads only the system's question and the caller's exact words and returns yes / no / unclear with the quoted evidence. The code checks the quote really appears in the caller's words, and the decision needs several agreeing attempts. In a hard calibration set it got 31 of 32 cases right, and the single miss fell on the safe side. Word lists were removed from every place that decides meaning.

## 6. Choosing each part by measurement
- **Speech recognition:** four engines compared on 160 synthetic phone-quality clips (clean and noisy). The chosen one had the lowest word error and the lowest latency and cost. Caveat: synthetic clips, not real callers.
- **Language model:** 18 conversation scenarios, three runs each, orders checked by the server. The chosen model passed 52 of 54 against 43 of 54 for a smaller one, and replied faster. A different model family is kept as an automatic fallback so one provider's outage does not silence a call.
- **Voice:** several voice models compared by ear on phone-quality audio.

## 7. Real conversations exposed gaps the tests did not
Testing with real conversation patterns found: guessing a quantity the caller never said, saving an order on an unrelated reply, closing before an open question, and a stale server still running old code. Each became a rule in code and a regression test.

## 8. Independent reviews before declaring anything ready
We ran several independent reviews (conversation logic, resilience, security and data, code clarity, readiness for a real restaurant) and fixed what they found, then ran security checks including live attacks against a running server (forged signatures, duplicate connections), all of which were rejected.

## 9. Where it stands
Strong on logic and safety in simulation, **unproven on a real phone line**. The honest next steps are in [testing and status](testing-and-status.md).
