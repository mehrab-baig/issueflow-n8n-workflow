# Design Decisions
1. **Google Sheets over SQL:** Chosen for rapid prototyping and easy visibility of the ReviewQueue.
2. **Logic before AI:** Deterministic rules handle priority routing *before* the AI classifies the issue type, saving API costs and preventing AI from triggering emergency alerts.
3. **Structured Output Parser:** Used to force the LLM to output a strict JSON schema, eliminating hallucinated categories.
