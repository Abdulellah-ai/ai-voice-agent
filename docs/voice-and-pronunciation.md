# Voice and pronunciation

## The problem
The first voice was judged by its owner as unpleasant, slow, and unmistakably synthetic. In a phone order, a mispronounced dish, number or price is a real error.

## What we tried and learned
- **Compared voice models by ear** on phone-quality audio across several providers. Voices from two other providers were clearly clearer and more natural. The current voice is one of them; a preferred voice from the second provider is waiting on its usage limits; a plain built-in voice is the safety net.
- **Speeding the voice up made it sound more robotic.** The better fix was shorter sentences, not faster speech.
- **Asking the model to add full diacritics made things worse** (measured), and the model also made diacritic mistakes of its own.
- **A chat-style audio model sometimes answered a line instead of reading it** (for example it replied to "where are you located?" instead of saying it). We found this by transcribing what it actually said, and fixed it by framing the request as "read this script exactly". Word error on fixed lines fell from 28% to 6.8%.
- **A voice request can stall silently.** A stall guard replaces a late voice with the fallback instead of leaving the caller in silence.

## Pronunciation, word by word
1. List the few words the voice says badly (dish names, numbers, area names) and the words it must always say.
2. Try respellings until a human listener confirms one sounds right. An automatic recognition check only *suggests* candidates; the human ear decides, because on most of the first words we tried the two disagreed.
3. Save the confirmed respelling. From then on the saved version is used every time that word is spoken.

There is a shared list for words every restaurant uses and a separate list in each restaurant's own file for its dishes and areas.

## Caveat
These results come from test audio and listening sessions. They have **not** been verified on a real phone call.
