# Introduction to prompt design

- *Prompt design* is the process of creating prompts that elicit the desired reponse from language models.

- This page introduces some basic concepts, strategies, and best practices to get you started in designing prompts.

## What is a prompt

- A prompt is a natural language request submitted to a language model to receive a response back.

### Prompt content types

- Prompts can include one or more of the following types of content:

    - Input (required)

    - Context (optional)

    - Examples (optional)

#### Input

- An input is the text in the prompt that you want the model to provide a response for, and it's a required content type.

#### Question input

- A question input is a question that you ask the model that the model provides an answer to.

```
What's a good name for a flower shop that specializes in selling bouquets of dried flowers?
```

#### Task input

- A task input is a task that you want the model to perform.

```
Give me a simple list of thigs that I must bring on a cample trip.
```

#### Entity input

- An entity input is what the model performs an action on, such as classify or summarize.

- This type of input can benefit from the inclusion of instructions.

```
Classify the following items as [large, small].
Elephant
Mouse
Snail
```

#### Completion input

- A completion input is text that the model is expected to complete or continue.

```
Some simple strategies for overcoming writer's block include
```

### Context

- Context can be one of the following:

    - Instructions that specify how the model should behave.

    - Information that the model uses or references to generate a response.

- Add contextual information in your prompt when you need to give information to the model, or restrict the boundaries of the responses to only what's within the prompt.

```
Marbles:
Color: red
Number: 12
Color: blue
Number: 28
Color: yellow
Number: 15
Color: green
Number: 17

How many green marbles are there?
```

### Examples

- Examples are input-output pairs that you include in the prompt to give the model an example of an ideal response.

- Including examples in the prompt is an effective strategy for customizing the response format.

```
Classify the following.
Options:
- red wine
- white wine

Text: Chardonnay
The answer is: white wine
Text: Cabernet
The answer is: red wine
Text: Moscato
The answer is: white wine

Text: Riesling
The answer is:
```

## Next steps

- For a deeper understanding of prompt design, see the prompt strategies topic.
