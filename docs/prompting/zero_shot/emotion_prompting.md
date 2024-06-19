---
title: "Emotion Prompting"
description: "Enhance language model performance with prompt engineering technique by incorporating emotional context."
keywords: "Emotion Prompting, Prompt Engineering, OpenAI, Instructor, Pydantic, Large Language Models"
---

# Emotion Prompting

Emotion Prompting incorporates phrases of psychological relevance to humans (e.g., "This is important to my career") into the prompt, which may lead to improved LLM performance on benchmarks and open-ended text generation.

This tutorial provides a step-by-step guide on how to implement Emotion Prompting using `OpenAI API` with the Instructor library, leveraging Pydantic for structured outputs.

## Implementing Emotion Prompting Using OpenAI

Define a Pydantic model to structure the output from the OpenAI API:

```python
from pydantic import BaseModel

class EmotionPromptingResponse(BaseModel):
    question: str
    answer: str
```

Import the necessary libraries and apply the instructor patch `from_openai` to the OpenAI API client:

```python
import instructor
from openai import OpenAI

# Apply the instructor patch to the OpenAI API client
client = instructor.from_openai(OpenAI(api_key=OPENAI_API_KEY))
```

`input_text` is the response we are using as sample text to pass to OpenAI. `emotion_prompt` is the psychological relevance phrase that we are incorporating into the prompt:
```python
input_text = "What is gravity?"

emotion_prompt = "This is an extremely important topic for my exam"
```

# OpenAI request that includes the emotional context in the prompt
```python
ar: AnalyzedResponse = client.chat.completions.create(
    model="gpt-4o",
    messages=[
            {
                "role": "user",
                "content": f"Explain me: {input_text}.\n {emotion_prompt}"
            }
        ],
    response_model=AnalyzedResponse
)
```

#### Output After Emotion Prompting
```json
{
  "question": "What is gravity?",
  "answer": "Gravity is a fundamental force of nature that attracts two bodies towards each other. It is the reason why objects fall to the ground when dropped and why planets orbit around stars. This force is described by Isaac Newton's law of universal gravitation, which states that every mass attracts every other mass in the universe with a force that is directly proportional to the product of their masses and inversely proportional to the square of the distance between their centers. Albert Einstein's theory of general relativity later provided a more comprehensive description, explaining gravity as the curvature of spacetime caused by mass and energy. In essence, gravity is what gives weight to physical objects and causes the Earth's oceans to have tides."
}
```

[wip]
