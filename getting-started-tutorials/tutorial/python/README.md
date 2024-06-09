# Tutorial: Get started with the Gemini API

> Python

## Prerequisites

- You can run this quickstart in Google Colab, which runs this notebook directly in the browser and does not require additional environment configuration.

- Alternatively, to complete this quickstart locally, ensure that your development environment meets the following requirements:

    - Python 3.9+

    - An installation of `jupyter` to run the notebook.

## Setup

### Install the Python SDK

- The Python SDK for the Gemini API, is contained in the `google-generativeai` package.

- Install the dependency using pip:

    ```sh
    $ pip install -q -U google-generativeai
    ```

### Import packages

- Import the necessary packages.

```python
import pathlib
import textwrap

import google.generativeai as genai

from IPython.display import display
from IPython.display import Markdown


def to_markdown(text):
    text = text.replace('•', '  *')
    return Markdown(textwrap.indent(text, '> ', predicate = lambda _: True))
```

```python
import os
```

### Setup Your API Key

- If you don't already already have one, create a key with one click in Google AI Studio.

- Once you have the API key, pass it to the SDK.

- You can do this in two ways:

    - Put the key the `GOOGLE_API_KEY` environment variable (the 

    - Pass the key to `genai.configure(api_key = ...)`

```python
GOOGLE_API_KEY = os.getenv('GOOGLE_API_KEY')

genai.configure(api_key = GOOGLE_API_KEY)
```

## List models

- Use `list_models` to see the available Gemini models:

    - `gemini-pro`: optimized for text-only prompts.

    - `gemini-pro-vision`: optimized for text-and-images prompts.

```python
for m in genai.list_models():
    if 'generateContent' in m.supported_generation_methods:
        print(m.name)
```

> ##### +Error
>
> - Error message:
>
>   ```
>   RetryError: Timeout of 60.0s exceeded, last exception: 503 DNS resolution failed for generativelanguage.googleapis.com:443: C-ares status is not ARES_SUCCESS qtype=SRV name=_grpclb._tcp.generativelanguage.googleapis.com: DNS query cancelled
>   ```
>
> - Solution:
>
>   ```python
>   os.environ["GRPC_DNS_RESOLVER"] = "native"
>   ```

> ##### Note
>
> - For detailed information about the available models, including their capabilities and rate limits, see Gemini.
>
> - There are options for requesting rate limit increases, The rate limit for Gemini-Pro models is 60 requests per minute (RPM).

- The `genai` package also supports the PaLM family of models, but only the Gemini models support the generic, multimodal capabilities of the `generateContent` method.

## Generate text from text inputs.

- For text-only prompts, use the `gemini-pro` model:

```python
model = genai.GenerativeModel('gemini-pro')
```

- The `generate_content` method can handle a wide variety of use cases, including multi-turn chat and multimodal input, depending on what the underlying model supports.

- The available models only support text and images as input, and text as output.

- In the simplest case, you can pass a prompt string to the `GenerativeModel.generate_content` method:

```python
%%time
response = model.generate_content("What is the meaning of life?")
```

- Insimple cases, the `response.text` accessor is all you need.

- To display formatted Markdown text, use the `to_markdown` function:

```python
to_markdown(response.text)
```

- If the API failed to return a result, use `GenerateContentResponse.prompt_feedback` to see if it was blocked due to safety concerns regarding the prompt.

```python
response.prompt_feedback
```

- Gemini can generate multiple possible responses for a single prompt.

- These possible responses are called `candidates`, and you can review them to select the most suitable one as the response.

- View the response candidates with `GenerateContentResponse.candidates`:

    ```python
    response.candidates
    ```

- By default, the model returns a response after completing the entire generation process.

- You can also stream the response as it is being generated, and the model will return chunks of the response as soon as they are generated.

- To stream responses, use `GenerativeModel.generate_content(..., stream = True)`.

```python
%%time
response = model.generate_content("What is the meaning of life?", stream = True)
```

```python
for chunk in response:
    print(chunk.text)
    print("_" * 80)
```

- When streaming, some response attributes are not available until you've iterated through all the response chunks.

```python
response = model.generate_content("What is the meaning of life?", stream=True)
```

```python
response.prompt_feedback
```

- The `prompt_feedback` attribute works.

```python
try:
  response.text
except Exception as e:
  print(f'{type(e).__name__}: {e}')
```

- But attributes like `text` do not.

## Chat conversations

- Gemini enables you to have freeform conversations across multiple turns.

- The `ChatSession` class simplifies the process by managing the state of the conversation, so unlike with `generate_content`, you do not have to store the converstion history as a list.

- Initialize chat:

```python
model = genai.GenerativeModel('gemini-pro')
chat = model.start_chat(history=[])
chat
```

> ##### Note
>
> - The vision model `gemini-pro-vision` is not optimized for multi-turn chat.

- The `ChatSession.send_message` method returns the same `GenerateContentResponse` type as `GenerativeModel.generate_content`.

- It also appends your message and the response to the chat history:

    ```python
    response= chat.send_message(
            "In one sentence, explain how a computer works to a young child.")
    to_markdown(response.text)
    ```

    ```python
    chat.history
    ```

- Use the `stream = True` argument to stream the chat:

```python
response = chat.send_message(
        "Okey, how about a more detailed explanation to a high schooler?",
        stream = True)

for chunk in response:
    print(chunk.text)
    print("_" * 80)
```

- `glm.Content` objcets contain a list of `glm.Part` objects that each contain either a text (string) or inline_data (`glm.Blob`), where a blob contains binary data and a `mime_type`.

- The chat history is available as a list  of `glm.Content` objects in `ChatSession.history`:

    ```python
    for message in chat.history:
        display(to_markdown(f'**{message.role}**: {message.parts[0].text}'))
    ```

## Count tokens

- Large laguage models have a context window, and the context length is often measured in terms of the **number of tokens**.

- With the Gemini API, you can determine the number of tokens per any `glm.Content` object.

- In simplest case, you can pass a query string to the `GenerativeModel.coun_tokens` method as follows:

    ```python
    model.coun_tokens("What is the meaning of life?")
    ```

- Wimilarly, you can check `token_count` for your `ChatSession`:

    ```python
    model.count_tokens(chat.history)
    ```

## Use embeddings

- Embedding is a technique used to represent information as a list of floating point numbers in an array.

    - See https://developers.google.com/machine-learning/glossary#embedding-vector

- With Gemini, you can represent text in a vectorized form, making it easier to compare and contrast embeddings.

- For example, two texts that share a similar subject matter or sentiment should have similar embeddings, which can be identified through mathematical comparison techniques such as cosine similarity.

- For more on how and why you should use embeddings, refer to Capabilities > Embeddings.

- Use the `embed_content` method to generate embeddings.

- The method handles embedding for the following tasks (`task_type`):

    |Task Type|Description|
    |-|-|
    |RETRIEVAL_QUERY|Specifies the given text is a query in a search/retrieval setting.|
    |RETRIEVAL_DOCUMENT|Specifies the given text is a document in a search/retrieval setting. Using this task type requires a `title`.|
    |SEMANTIC_SIMILARITY|Specifies the given text will be used for Semantic Textual Similarity (STS).|
    |CLASSIFICATION|Specifies that the embeddings will be used for classification.|
    |CLUSTERING|Specifies that the embeddings will be used for clustering.|

- The following generates an embedding for a single string for document retrieval:

    ```python
    result = genai.embed_content(model = "models/embedding-001",
                                 content = "What is the meaning of life?",
                                 task_type = "retrieval_document",
                                 title = "Embedding of single string")

    # 1 input > 1 vector output
    print(str(result['embedding'])[:50], '... TRIMMED]')
    ```

> ##### Note
>
> - The `retrieval_document` task type is the only task that accepts a title.

- To handle batches of strings, pass a list of strings in `content`:

- While the `genai.embed_content` function accepts simple strings or lists of strings, it is actually built around the `glm.Content` type.

- While the `glm.Content` object is multimodal, the `embed_content` method only supports text embeddings.

## Advanced use cases

### Safety settings

- The `safety_settings` argument lets you configure what the model blocks and allows in both prompts and responses.

- By default, safety setting block content with medium and/or high probability of being unsafe content across all dimensions.

- Learn more about Safety > Safety settings.

- Enter a questionable prompt and run the model with the default safety settings, and it will not return any candidates.

- The `prompt_feedback` will tell you which safety filter blocked the prompt.

- Now provide the same prompt to the model with newly configured safety settings, and you may get a response.

```python
response = model.generate_content('[Questionable prompt here]',
                                  safety_settings = {'HARASSMENT':'block_none'})
```

- Each candidate has its own `safety_ratings`, in case the prompt passes but the individual responses fail the safety checks.

### Encode messages

- This section offers a fully-typed equivalent to the previous example, so you can better understand the lower-level details regarding how the SDK encodes messages.

- Underlying the Python SDK is the `google.ai.generativelanguage` client library:

    ```python
    import google.ai.generativelanguage as glm
    ```

- The SDK attempts to convert your message to a `glm.Content` object, which contains a list of `glm.Part` objects that each contain either:

    1. a `text` (string)

    2. `inline_data` (glm.Blob`), where a blob contains binary `data` and `mime_type`.

- You can also pass any of these lcasses as an equivalent dictionary.

> ##### Note
>
> - The only accepted mime types are some image types, `image/*`.

- So, the fully-typed equivalent to the previous example is:

    ```python
    model = genai.GenerativeModel('gemini-pro')
    response = model.generate_content(
            glm.Content(
                parts = [
                    glm.Part(text = "In one sentence, explain how a computer works to a young child."),
                    ],
                ),
            stream = True)
    ```

```python
response.resolve()

to_markdown(response.text[:100] + "... [TRIMEED] ...")
```

### Multi-turn conversations

- While the `genai.ChatSession` class shown earlier can handle many use cases, it does make some assumptions.

- If your use case doesn't fit into this chat implementation it's good to remember that `genai.ChatSession` is just a wrapper around `GenerativeModel.generate_content`.

- `genai.ChatSession` is just a wrapper around `GenerativeModel.generate_content`.

- The individual messages are `glm.Content` objects or compatible dictionaries.

- As a dictionary, the message requires `role` and `parts` keys.

- The `role` in a conversation can either be the `user`, which provide the prompts, or`model`, which provides the responses.

- Pass a list of `glm.Content` objects and it will be treated as multi-turn chat:

    ```python
    model = genai.GenerativeModel('gemini-pro')

    messages = [
            {'role':'user',
             'parts': ["Briefly explain how a computer works to a young child."]}]
    response = model.generate_content(messages)

    to_markdown(response.text)
    ```

- To continue the conversation, add the response and another message.

> ##### Note
>
> - For multi-turn conversations, you need to send the whole conversation history with each request.
>
> - The API is **stateless**.

```python
messages.append({'role':'model',
                'parts':[response.text]})

messages.append({'role':'user',
                'parts':["Okey, how about a more detailed explanation to a high school student?"]})

response = model.generate_content(messages)

to_markdown(response.text)
```

### Generation configuration

- Every prompt you send to the model includes parameter values that control how the model generates responses.

```python
model = genai.GenerativeModel('gemini-pro')
response = model.generate_content(
        'Tell me a story about a magic backpack.',
        generation_config = genai.types.GenerationConfig(
            # Only one candidate for now.
            candidate_count = 1,
            stop_sequences = ['x'],
            max_output_tokens = 20,
            temperature = 1.0)
        )
```

```python
text = response.text

if response.candidates[0].finish_reason.name == "MAX_TOKENS":
    text += '...'

to_markdown(text)
```

## What's next

- Prompting > Prompting strategies

- Gemini

- Request more quota
