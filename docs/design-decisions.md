# Design decisions and the evidence behind them

| Decision | Why |
|---|---|
| Server decides irreversible actions; the model only understands and speaks | A model can be persuaded or can slip. A rule in code cannot. Tests showed premature saves and closes before this was enforced |
| Order saved only after the server reads it back and the caller explicitly agrees | Prevents wrong or unwanted orders; the read-back sentence is built by the server so it cannot drift |
| Prices, totals, delivery fee and minimums computed by the server from the restaurant's file | The model never invents a price in an order. (A gap remains for free-text price questions; see below) |
| Independent judge instead of word lists for yes / no / unclear | Word lists lost real orders on natural phrasing. The judge sees only the question and the caller's words and must quote evidence the code verifies |
| Order and kitchen ticket written in one atomic database step | An order cannot be saved without its ticket or the reverse |
| Everything restaurant-specific lives in that restaurant's file | No dish, area or phrase of one restaurant is allowed in shared code or shared instructions |
| Safety rules in code, tone and menu policy in configuration | Safety must not depend on a configuration mistake |
| No diacritics requested from the model | Full diacritics measurably made the voice worse in our test (understanding of the greeting fell from 97% to 78%) |
| Fixed lines are pre-recorded once and replayed | Instant start, identical every time, and no voice request per call |
| Fallbacks at every layer (model, voice, recognition) | An outage must end in a polite message and a hand-off to staff, never silence |
| Server speaks the critical lines itself | Saved a full model call per turn and prevents wording drift on the sentences that matter most |

## Known gap
When a caller asks a price without ordering ("how much is the kebab?"), the model answers from the menu text it was given and there is no automated check that the spoken number matches. This is covered by instructions and examples only. The full fix is to check every number in a reply against allowed prices, or to build price answers in code.
