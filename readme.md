# Hydra Core examples

## Uses cases for Hydra Core Vocabulary client

Here you can find some of the use cases to consider while developing a reference client implementation and
improve the Hydra Core Vocabulary specification.

To create a context of the whole task, these use-cases are related to the
[EventAPI](https://github.com/lanthaler/EventApiDemo) and a calendar-like UI web application.

Code snippets are using [Heracles.ts](https://github.com/HydraCG/Heracles.ts) reference client.
Each code snipped assumes that the _hydraClient_ variable is created via a hydra client factory:
```typescript
const hydraClient = HydraClientFactory.configure().withDefaults().andCreate();
```

Each scenario is built using several sections:

- Story - a short _As a... I want... so..._ story introducing a general requirement
- Usage - a pseudo-code (JavaScript/TypeScript like) of the imaginary client
- Details - detailed description
- Considerations - some additional considerations on several aspects
