**Integrations:**
- You do not have access to API keys or secrets for integrations
- However, you can use the integration gateway to access external APIs integrations
- You need to call the integration endpoint with the correct headers

---

**Best AI models:**
- openai: gpt-4o

**Chat agent tips:**
- Include a system message with a personality, goal, and the current date
- Include recent conversation history
- Add relevant tools for the agent to use (set the max steps to 2 or more)
- Format the results of tools as markdown and truncate or limit length as needed

**AI native apps:**
- Use LLMs to create structured data from natural language (e.g. filling a whole form with a text input or with a conversation)

---

```/example-project/app/actions.ts
'use server';
import { createIntegrationConfig } from '@/core/integrations/config';
import { createServerClient } from '@/core/supabase/server';
import { createOpenAI } from '@ai-sdk/openai';
import { CoreMessage, generateText, tool } from 'ai';
import { z } from 'zod';

export async function getUserId() {
  const client = await createServerClient();
  const user = await client.auth.getUser();
  return user.data.user?.id;
}

export async function getSearchResults(query: string) {
  const userId = await getUserId();
  const { baseUrl, headers } = createIntegrationConfig({ integration: 'serpapi', applicationUserId: userId });
  const response = await fetch(\`$\{baseUrl}/search?q=$\{query}\`, { headers });
  const data = await response.json();
  return data;
}

export async function initOpenAI() {
  const userId = await getUserId();
  const { baseUrl, headers } = createIntegrationConfig({ integration: 'openai', applicationUserId: userId });

  // openai expects the baseURL to point to the v1 endpoint
  // other integrations may have other expectations, be sure to meet them
  return createOpenAI({ baseURL: \`$\{baseUrl}/v1\`, headers: headers, apiKey: 'no-api-key' });
}

export async function answer(messages: CoreMessage[]) {
  const openai = await initOpenAI();
  const { text: answer } = await generateText({
    model: openai('gpt-4o'),
    tools: {
      search: tool({
        parameters: z.object({ query: z.string() }),
        execute: async ({ query }) => {
          return await getSearchResults(query);
        },
      }),
    },
    maxSteps: 10,
    messages: [
      { role: 'system', content: 'You are a helpful assistant that can search the web for information.' },
      ...messages,
    ],
  });

  return answer;
}

export async function generateFormDataFromInput(input: string) {
  const openai = await initOpenAI();
  const { object: formData } = await generateObject({
    model: openai('gpt-4o'),
    schema: z.object({
      name: z.string(),
      email: z.string(),
      phone: z.string(),
    }),
    system: "Convert the user's input into a structured data object",
    prompt: input,
  });
  return formData;
}
```
