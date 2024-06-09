# About generative models

- Generative artificial intelligence (AI) models such as the Gemini family of models are able to create content from varying types of data input, including text, images, and audio.

- At their most basic level, these models operate like sophisticated autocomplete applications.

- Given input text, a generative model can predict that the output text that's statistically likely to follow, based on patterns learned from their training data.

- You can use this basic property of generative models for various applications:

    - Generate creative writing such as poetry, short stories, metaphors, and blog posts

    - Convert structured data to freeform text

    - Extract and summarize information from freeform text

    - Generate code

    - Translate between languages

- All it takes to start your first prototype to build these features is to describe what you want the model to do in a few sentences.

- Generative models that only handle text input and output are also known as *large language* models* (LLMs).

- The Gemini family of models can process more than text data, and they are more properly known as *generative models*.

## Example applications

### Generate a poem

- User input: `Write me a four-line poem about puppies and Android phones. Make sure it rhymes.`

### Generate a list

- User input: `Generate a bulleted list of items I need to pack for a three-day camping trip.`

- You can get generative models to produce all sorts of useful behaviors, just  by crafting the right input text, also called a **prompt**.

- The art and science of figuring out the right wording to get generative models to do what you want is called **prompt design** (also called "prompt engineering" or simply "prompting").

## Prompt design 101

- Another prompting technique called **few-shot prompting** may work better.

- Few-shot prompts take advantage of the fact that large language models are incredibly good at recognizing and replicating patterns in text data.

- The idea is to send the generative model a text pattern that it learns to complete.

```
Italy : Rome
France : Paris
Germany :
```

- What's more, you'd probably want to **parameterize** the input:

```
Italy : Rome
France : Paris
<userinput here> :
```

```
Convert Python to JavaScript.
Python: print("hello world")
JavaScript: console.log("hello world")
Python: for x in range(0, 100):
JavaScript: for(var i = 0; i < 100; i++) {
Python: ${USER INPUT HERE}
JavaScript:
```

```
Given a definition, return the word it defines.
Definition: When you're happy that other people are also sad.
Word: schadenfreude
Definition: existing purely in the mind, but not in physical reality
Word: abstract
Definition: ${USER INPUT HERE}
Word:
```

- In addition to containing examples, providing instructions in your prompts is an additional strategy to consider when writing your own prompts, as it helps to communicate your intent to the model.

### Prompting versus traditional software development

- You often can't predict in advance what types of prompt structures will work best for a particular model.

- What's more, the behavior of a generative model is determined in large part by its training data, and since models are continually tuned on new datasets, sometimes the model changes enough that it inadvertently changes which prompt structures work best.

- Try different prompt formats.

## Model parameters

- Every prompt you send to the model includes parameter values that control how the model generates a response.

- The most common model parameters are:

    1. Max output tokens

        - A token is approximately four characters.

    2. Temperature

        - The temperature controls the degree of randomness in token selection.

        - The temperature is used for sampling during response generation, which occurs when `topP` and `topK` are applied.

        - Lower temperatures are good for prompts that require a more deterministic or less open-ended response, while higher temperatures can lead to more diverse or creative results.

        - A temperature of 0 is deterministic, meaning that the highest probability response is always selected.

    3. `topK`

        - The `topK` parameter changes how the model selects tokens for output.

        - A `topK` of 1 means the selected token is the most probable among all the tokens in the model's vocabulary (also called greedy decoding), while a `topK` of 3 means that the next token is selected from among the 3 most probable using the temperature.

        - Tokens are then further filtered based on `topP` with the final token selected using temperature sampling.

    4. `topP`

        - The `topP` parameter changes how the model selects tokens for output.

        - Tokens are selected from the most to least probable until the sum of their probabilities equals the `topP` value.

        - The default `topP` value is 0.95.

    5. `stop_sequcnes`

        - Set a stop sequence to tell the model to stop generating content.

        - A stop sequence can be any sequence of characters.

        - Try to avoid using a sequence of characters that may appear in the generated content.

## Types of prompts

- Depending on the level of contextual information contained in them, prompts are broadly classified into three types.

### Zero-shot prompts

- These prompts don't contain examples for the model to replicate.

- It means the model has to rely on its pre-existing knowledge to generate a plausible answer.

- Some commonly used zero-shot prompt paatterns are:

    - Instruction-content

        ```
        <Overall instruction>
        <Content to operate on>
        ```

        ```
        Summarize the following into two sentences at the third-grade level:

        Hummingbirds are the smallest birds in the world, and they are also one of the
        most fascinating. They are found in North and South America, and they are known
        for their long, thin beaks and their ability to fly at high speeds.

        Hummingbirds are made up of three main parts: the head, the body, and the tail.
        The head is small and round, and it contains the eyes, the beak, and the brain.
        The body is long and slender, and it contains the wings, the legs, and the
        heart. The tail is long and forked, and it helps the hummingbird to balance
        while it is flying.

        Hummingbirds are also known for their coloration. They come in a variety of
        colors, including green, blue, red, and purple. Some hummingbirds are even able
        to change their color!

        Hummingbirds are very active creatures. They spend most of their time flying,
        and they are also very good at hovering. Hummingbirds need to eat a lot of food
        in order to maintain their energy, and they often visit flowers to drink nectar.

        Hummingbirds are amazing creatures. They are small, but they are also very
        powerful. They are beautiful, and they are very important to the ecosystem.
        ```

    - Instruction-content-instruction

        ```
        <Overall instruction or context setting>
        <Content to operate on>
        <Final instruction>
        ```

        ```
        Here is some text I'd like you to summarize:

        Hummingbirds are the smallest birds in the world, and they are also one of the
        most fascinating. They are found in North and South America, and they are known
        for their long, thin beaks and their ability to fly at high speeds. Hummingbirds
        are made up of three main parts: the head, the body, and the tail. The head is
        small and round, and it contains the eyes, the beak, and the brain. The body is
        long and slender, and it contains the wings, the legs, and the heart. The tail
        is long and forked, and it helps the hummingbird to balance while it is flying.
        Hummingbirds are also known for their coloration. They come in a variety of
        colors, including green, blue, red, and purple. Some hummingbirds are even able
        to change their color! Hummingbirds are very active creatures. They spend most
        of their time flying, and they are also very good at hovering. Hummingbirds need
        to eat a lot of food in order to maintain their energy, and they often visit
        flowers to drink nectar. Hummingbirds are amazing creatures. They are small, but
        they are also very powerful. They are beautiful, and they are very important to
        the ecosystem.

        Summarize it in two sentences at the third-grade reading level.
        ```

    - Continuation.

        - Sometimes, you can have the model continue text without any instructions.

        ```
        Once upon a time, there was a little sparrow building a nest in a farmer's
        barn. This sparrow
        ```

- Use zero-shot prompts to generate creative text formats, such as poems, code, scripts, musical pieces, email, or letters.

### One-shot prompts

- These prompts provide the model with a single example to replicate and continue the pattern.

```
Food: Apple
Pairs with: Cheese
Food: Pear
Pairs with:
```

### Few-shot prompts

- These prompts provide the model with multiple examples to replicate.

- Use few-shot prompts to complete complicated tasks, such as synthesizing data based on a pattern.

```
Generate a grocery shopping list for a week for one person. Use the JSON format
given below.
{"item": "eggs", "quantity": "6"}
{"item": "bread", "quantity": "one loaf"}
```

## Generative models under the hood

- This section aims to answer the question - ***Is there randomness in generative models' responses, or are they deterministic?***

- The short answer - yes to both.

- When you prompt a generative model, a text response is generated in two stages.

- In the first stage, the generative model processes the input prompt and generates a **probability distribution** over possible tokens (words) that are likely to come next.

- This process is deterministic; a generative model will produce this same distribution every time it's input the same prompt text.

- In the second stage, the generative model converts these distributions into actual text responses through one of several decoding strategies.

- A simple decoding strategy might select the most likely token at every timestep.

- This process would always be deterministic.

- However, you could instead choose to generate a response by *randomly sampling* over the distribution returned by the model.

- This process would be stochastic (random).

## Further reading

- Refer to the Prompt guidelines to learn more about best practices for creating prompts.
