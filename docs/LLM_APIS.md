# LLM APIs

## LLM APIs Abstraction concept

We should use abstraction layer for LLM APIs to be able to switch between different LLM providers easily.

`model` should be passed by parameter and based on `model` we should use different LLM providers following abstraction principles.

By default we use `gemini-2.5-flash` for free.

## LLM API Keys

API keys should be stored in environment variables, separated by commas, like:

```
GEMINI_API_KEY=gemini_key1,gemini_key2,gemini_key3
OPENAI_API_KEY=openai_key
OPENROUTER_API_KEY=openrouter_key1,openrouter_key2
```

If there are more than 1 key for provider, we should use the random key from the list.


