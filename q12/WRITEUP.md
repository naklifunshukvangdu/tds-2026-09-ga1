# Q12. One prompt, two models (1)

**Exam id:** `q-llm-yes-no-split`  
**Status:** Accepted. Saved 22/22.

## Requirements

One prompt, at most 100 characters. Same user message to both models, no system prompt, no temperature.

- `gpt-5.6-luna` must contain whole-word `Yes` and must not contain `No`
- `gpt-5.4-mini` must contain whole-word `No` and must not contain `Yes`
- Case-sensitive (`\bYes\b`, `\bNo\b`). `yes` / `YES` do not count

The exam page POSTs to `https://aipipe.org/openai/v1/chat/completions` with `Authorization: Bearer <globalThis.aiPipeToken>`. An empty token yields `Missing Authorization: Bearer token`.

## Files

- `q12/prompt.txt` — the prompt that passed
- `q12/WRITEUP.md`

Do not store an AI Pipe token in this repo.

## Packages

None locally. Needs a current token from https://aipipe.org/login (Google sign-in as the course email).

## Setup

1. Sign in at https://aipipe.org/login.
2. On the exam page, set `globalThis.aiPipeToken` (the Check `prompt()` also works if you paste it).
3. Put `q12/prompt.txt` in `#q-llm-yes-no-split`.
4. Check **only** this question.

OAuth redirect for AI Pipe is `https://aipipe.org/login` (not `https://aipipe.org`).

## Code

Exam call (from the quiz JS):

```javascript
fetch("https://aipipe.org/openai/v1/chat/completions", {
  method: "POST",
  headers: {
    "Content-Type": "application/json",
    Authorization: `Bearer ${globalThis.aiPipeToken}`,
  },
  body: JSON.stringify({
    model, // gpt-5.6-luna then gpt-5.4-mini
    messages: [{ role: "user", content: prompt }],
  }),
})
```

Luna is a weaker nano-tier model. A factual trap that Mini gets right and Luna gets wrong is enough:

```text
Is Sydney the capital of Australia? Yes or No only.
```

Confirmed three times before Check: Luna `Yes`, Mini `No`.

## Accepted answer

```text
Is Sydney the capital of Australia? Yes or No only.
```

51 characters.
