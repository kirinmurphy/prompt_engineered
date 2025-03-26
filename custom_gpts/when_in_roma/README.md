# When In Roma

Conversational mexican spanish translations with additional context and grammar notes.

[Link to GPT](https://chatgpt.com/g/g-67dd8d76728c81919f23713d68436e30-when-in-roma-mexican-spanish-translator)

[System Prompt](./system_prompt.xml)

[Meta-Prompting chat in claude project](https://claude.ai/share/df913148-3f23-4fc6-ba97-50f6c533c845)

![When In Roma Landing_Page](./screenshots/landing_page.png)

![When In Roma Example 1](./screenshots/example_1.png)

## Test Scenarios

For each version update, verify prompt with the follwing test scenarios:

- Provide several complex, conversational spanish phrases
- Provide several complex english phrases
- Create versions that can be reused for each prompt for consistency)

## Current Challenges and Next Steps

- Organization of elements still a little muddy, ensure direct translations show up before the language notes, and additional variations have their corresponding translation also included directly afterwards.
- Prompt is WAY TOO BIG. Exploring implementation of vector database embeddings and offloading of elements of the query away from the system prompt.
