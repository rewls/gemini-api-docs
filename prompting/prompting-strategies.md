# Prompt design strategies

- This page introduces you to some general prompt design strategies that you can employ when designing prompts.

- While there's no right or wrong way to desing a prompt, there are common strategies that you can use to affect the model's responses.

- Rigorous testing and evaluation remain crucial for optimizing model performance.

## Give clear and specific instructions

- Giving the model instructions on what to do is an effective and efficient way to customize model behavior.

- Ensure that the instructions you give are clear and specific.

- Instructions can be as simple as a list of step-by-step instructions or as complex as mapping out a user's experience and mindset.

### Define the task to perform

- Describe in detail the task that you want the model to perform.

```
Summarize this text.
Text: A quantum computer exploits quantum mechanical phenomena to perform calculations exponentially faster than any modern traditional computer. At very tiny scales, physical matter acts as both particles and as waves, and quantum computing uses specialized hardware to leverage this behavior.  The operating principles of quantum devices is beyond the scope of classical physics. When deployed at scale, quantum computers could be used in a wide variety of applications such as: in cybersecurity to break existing encryption methods while helping researchers create new ones, in meteorology to develop better weather forecasting etc. However, the current state of the art quantum computers are still largely experimental and impractical.
```

### Specify any constraints

- Specify any constraints on reading the prompt or generating a response.

- You can tell the model what to do and not to do.

```
Summarize this text in two sentences.
Text: A quantum computer exploits quantum mechanical phenomena to perform calculations exponentially faster than any modern traditional computer. At very tiny scales, physical matter acts as both particles and as waves, and quantum computing uses specialized hardware to leverage this behavior.  The operating principles of quantum devices is beyond the scope of classical physics. When deployed at scale, quantum computers could be used in a wide variety of applications such as: in cybersecurity to break existing encryption methods while helping researchers create new ones, in meteorology to develop better weather forecasting etc. However, the current state of the art quantum computers are still largely experimental and impractical.
```

### Define the format of the response

- For example, you can ask for the response to be formatted as a table, bulleted list, elevator pitch, keywords, sentence, or paragraph.

```
Summarize this text as bullets points of key information.
Text: A quantum computer exploits quantum mechanical phenomena to perform calculations exponentially
faster than any modern traditional computer. At very tiny scales, physical matter acts as both
particles and as waves, and quantum computing uses specialized hardware to leverage this behavior.
The operating principles of quantum devices is beyond the scope of classical physics. When deployed
at scale, quantum computers could be used in a wide variety of applications such as: in
cybersecurity to break existing encryption methods while helping researchers create new ones, in
meteorology to develop better weather forecasting etc. However, the current state of the art quantum
computers are still largely experimental and impractical.
```

## Include few-shot examples.

- The model attempts to identify patterns and relationships from the examples and applies them when generating a response.

- Prompts that contain a few examples are called *few-shot prompts*, while prompts that provide no examples are called *zero-shot prompts*.

- Few-shot prompts are often used to regulate the formatting phrasing, scoping, or general patterning of model responses.

- Use specific and varied examples to help the model narrow its focus and generate more accurate results.

- We recommend to always include few-shot examples in your prompts.

- In fact, you can remove instructions from your prompt if your examples are clear enough in showing the task at hand.

### Zero-shot vs few-shot prompts

```
Please choose the best explanation to the question:

Question: How is snow formed?
Explanation1: Snow is formed when water vapor in the air freezes into ice crystals in the
atmosphere, which can combine and grow into snowflakes as they fall through the atmosphere and
accumulate on the ground.
Explanation2: Water vapor freezes into ice crystals forming snow.
Answer:
```

- If your use case requires the model to produce concise responses, you can include examples in the prompt that give preference to concise responses.

- 
```
Please choose the best explanation to the question:

Question: Why is sky blue?
Explanation1: The sky appears blue because of Rayleigh scattering, which causes shorter blue
wavelengths of light to be scattered more easily than longer red wavelengths, making the sky look
blue.
Explanation2: Due to Rayleigh scattering effect.
Answer: Explanation2

Question: What is the cause of earthquakes?
Explanation1: Sudden release of energy in the Earth's crust.
Explanation2: Earthquakes happen when tectonic plates suddenly slip or break apart, causing a
release of energy that creates seismic waves that can shake the ground and cause damage.
Answer: Explanation1

Question: How is snow formed?
Explanation1: Snow is formed when water vapor in the air freezes into ice crystals in the
atmosphere, which can combine and grow into snowflakes as they fall through the atmosphere and
accumulate on the ground.
Explanation2: Water vapor freezes into ice crystals forming snow.
Answer:
```

### Find the optimal number of examples

- Models like PaLM and Gemini can often pick up on patterns using a few examples, though you may need to experiment with what number of examples lead to the desired results.

- For simpler models like BERT, you may need more examples.

- At the same time, if you include too many examples, the model may start to overfit the response to the examples.

### Use examples to show patterns instead of antipatterns

- Using examples to show the model a pattern to follow is more effective than using examples to show the model an antipattern to avoid

- Negative pattern:

    ```
    Don't end haikus with a question:
    Haiku are fun
    A short and simple poem
    Don't you enjoy them?
    ```

- Positive pattern:

    ```
    Always end haikus with an assertion:
    Haiku are fun
    A short and simple poem
    A joy to write
    ```

### Use consistent formatting across examples

- Make sure that the structure and formatting of few-shot examples are the same to avoid responses with undesired formats, especially paying attention to XML tags, white spaces, newlines, and example splitters.

## Add contextual information

- You can include in the prompt instructions and information that the model needs to solve a problem instead of assuming that the model has all of the required information.

```
What should I do to fix my disconnected wifi? The light on my Google Wifi router is yellow and
blinking slowly.
```

- To customize the response for the specific router, you can add to the prompt the router's troubleshooting guide as context for it to refer to when providing a response.

```
Answer the question using the text below. Respond with only the text provided.
Question: What should I do to fix my disconnected wifi? The light on my Google Wifi router is yellow and blinking slowly.

Text:
Color: Slowly pulsing yellow
What it means: There is a network error.
What to do:
Check that the Ethernet cable is connected to both your router and your modem and both devices are turned on. You might need to unplug and plug in each device again.

Color: Fast blinking yellow
What it means: You are holding down the reset button and are factory resetting this device.
What to do:
If you keep holding down the reset button, after about 12 seconds, the light will turn solid yellow. Once it is solid yellow, let go of the factory reset button.

Color: Solid yellow
What it means: Router is factory resetting.
What to do:
This can take up to 10 minutes. When it's done, the device will reset itself and start pulsing white, letting you know it's ready for setup.

Color: Solid red
What it means: Something is wrong.
What to do:
Critical failure. Factory reset the router. If the light stays red, contact Wifi customer support.
```

## Add prefixes

- A prefix is a word or phrase that you add to the prompt content that can serve several purposes, depending on where you put the prefix:

    - Input prefix: Adding a prefix to the input signals semantically meaningful parts of the input to the model.

        - For example, the prefixes "English:" and "French:" demarcate two different languages.

    - Output prefix: The output prefix gives the model information about what's expected as a response

        - For example, the output prefix "JSON:" signals to the model that the output should be in JSON format.

    - Example prefix: In few-shot prompts, adding prefixes to the examples provides labels that the model can use when generating the output, which makes it easier to parse output content.

- In the following example, "Text:" is the input prefix and "The answer is:" is the output prefix.

```
Classify the text as one of the following categories.
- large
- small
Text: Rhino
The answer is: large
Text: Mouse
The answer is: small
Text: Snail
The answer is: small
Text: Elephant
The answer is:
```

## Let the model complete partial input

- Generative language models work like an advanced autocompletion tool.

- When you provide partial content, the model can provide the rest of the content or what it thinks is a continuation of that content as a response.

- When doing so, if you include any examples or context, the model can take those examples or context into account.

```
For the given order, return a JSON object that has the fields cheeseburger, hamburger, fries, or
drink, with the value being the quantity.

Order: A burger and a drink.
```

- Writing out the instructions in natural language can sometimes be challenging and it leaves a lot to the model's interpretation.

- You can give an example and a response prefix and let the model complete it:

```
Valid fields are cheeseburger, hamburger, fries, and drink.
Order: Give me a cheeseburger and fries
Output:
\`\`\`
{
"cheeseburger": 1,
"fries": 1
}
\`\`\`
Order: I want two burgers, a drink, and fries.
Output:
```

#### Prompt the model to format its response

- The completion strategy can also help format the response.

```
Create an outline for an essay about hummingbirds.
```

- To get the model to return an outline in a specific format, you can add text that represents the start of the outline and let the model complete it based on the pattern that you initiated.

```
Create an outline for an essay about hummingbirds.
I. Introduction
*
```

## Break down prompts into simple components

- For use cases that require complex prompts, you can help the model manage this complexity by breaking things down into simpler components.

### Break down instructions

- Instead of having many instructions in one prompt, create one prompt per instruction. 

- You can choose which prompt to process based on the user's input.

### Chain prompts

- For complex tasks that involve multiple sequential steps, make each step a prompt and chain the prompts together in a sequence.

- In this sequential chain of prompts, the output of one prompt in the sequence becomes the input of the next prompt.

- The output of the last prompt in the sequence is the final output.

### Aggregate responses

- Aggregation is when you want to perform different parallel tasks on different portions of the data and aggregate the results to produce the final output.

## Experiment with different parameter values

- Each call that you send to a model includes parameter values that control how the model generates a response.

- The model can generate different results for different parameter values.

- The most common parameters are the following:

    - Max output tokens

    - Temperature

    - Top-K

    - Top-P

### Max output tokens

- Specify a lower value for shorter responses and a higher value for longer responses.

### Temperature

- For most use cases, try starting with a temperature of `0.2`.

- If the model returns a response that's too generic, too short, or the model gives a fallback response, try increasing the temperature.

### Top-K

- Specify a lower value for less random responses and a higher value for more random responses.

- The default top-K is `40`.

### Top-P

- Specify a lower value for less random responses and a higher value for more random responses.

- The default top-P is `0.95`.

## Prompt iteration strategies

- Prompt design is an iterative process that often requires a few iterations before you get the desired response consistently.

- This section provides guidance on some things you can try when iterating on your prompts.

### Use different phrasing

- Using different words or phrasing in your prompts often yields different responses from the model even though they all mean the same thing.

- If you're not getting the expected results from your prompt, try rephrasing it.

```
Version 1:
How do I bake a pie?

Version 2:
Suggest a recipe for a pie.

Version 3:
What's a good pie recipe?
```

### Switch to an analogous task

- If you can't get the model to follow your instructions for a task, try giving it instructions for an analogous task that achieves the same result.

```
Which category does The Odyssey belong to:
thriller
sci-fi
mythology
biography
```

- You can rephrase the instructions as a multiple choice question and ask the model to choose an option.

```
Multiple choice problem: Which of the following options describes the book The Odyssey?
Options:
- thriller
- sci-fi
- mythology
- biography
```

## Change the order of prompt content

- The order of the content in the prompt can sometimes affect the response.

- Try changing the content order and see how that affects the response.

```
Version 1:
[examples]
[context]
[input]

Version 2:
[input]
[examples]
[context]

Version 3:
[examples]
[input]
[context]
```

## Fallback responses

- A fallback response is a response returned by the model when either the prompt or the response triggers a safety filter.

- If the model responds with a fallback response, try increasing the temperature.

## Things to avoid

- Avoid relying on models to generate factual information.

- Use with care on math and logic problems.
