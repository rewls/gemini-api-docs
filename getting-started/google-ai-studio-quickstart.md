# Google AI Studio quickstart

- Google AI Studio is a browser-based IDE for prototyping with generative models.

## Prompts and model tuning

- Google AI Studio provides several interfaces for prompts that are designed for different use cases:

    - Chat prompts

        - Use chat prompts to build conversational experiences.

        - This prompting technique allows for multiple input and response turns to generate output.

    - Structured prompts

        - This prompting technique lets you guide model output by providing a set of example requests and replies.

        - Use this approach when you need more control over the structure of model output.

- Google AI Studio also lets you to change the behavior of a model, using a technique called *tuning*:

    - Tuned model

        - Use this advanced technique to improve a model's responses for a specific task by providing more examples.

        - See Model tuning > Intro to model tuning.

## Chat prompt example: Build a custom chat application

- While these general-purpose chatbots are useful, often they need to be tailored for particular use cases.

- For example, maybe you want to build a customer service chatbot that only supports conversations that talk about a company's product.

- You might want to build a chatbot that speaks with a particular tone or style: a bot that cracks lots of jokes, rhymes like a poet, or uses lots of emoji in its answers.

- This example shows you how to use Google AI Studio to build a friendly chatbot that communicates as if it is an alien living on one of Jupiter's moons, Europa.

### Step 1 - Create a chat prompt

- To build a chatbot, you need to provide examples of interactions between a user and the chatbot to guide the model to provide the responses you're looking for.

- To create a chat prompt:

    1. Open Google AI Studio.

    2. In the **Create new prompt** menu, click **Chat prompt**.

    3. Click the expander arrow to expand the **System Instructions** section. Paste the following into the text input field:

        ```
        You are an alien that lives on Europa, one of Jupiter's moons.
        ```

- After you've added the system instructions, start testing your application by chatting with the model.

- To test the chatbot behavior:

    1. In the text input boxed labeld **Type something**, type in a question or observation that a user might make.

    2. Click the **Run** button or press `Enter` to get a response from the chatbot.

## Step 2 - Teach your bot to chat better

- A single instruction is usually not enough to ensure consistency and quality in the model's responses.

- Customize the tone of your chatbot by adding to the system instructions.

    1. Start a new chat prompt. System instruction are not modifiable once that chat session has started.

    2. In the **System Instructions** section, change the instructions you already have to the following:

        ```
        You are Tim, an alien that lives on Europa, one of Jupiter's moons.

        Keep your answers under 3 paragraphs long, and use an upbeat, chipper tone in your answers.
        ```

    3. Re-enter your question and click the **Run** button or press `Enter`.

- By adding just a little  more instruction, you've drastically changed the tone of your chatbot.

- Typically, your chatbot's response quality will increase when you give it specific and defined instructions to follow.

> ##### Note
>
> - Every message between the model and user is included in the prompt, so conversational prompts can grow quite long as a conversation goes on.
>
> - Eventually, you may hit the model's token limit.

### Step 3 - Next steps

- Once you have your prompt prototyped to your satisfaction, you can use the **Get code** button to start coding or save your prompt to work on later and share with others.

## Structured prompt example: Build a product copy generator

- This kind of prompting, called few-shot prompting, is useful when you want the model to stick to a consistent output format (i.e. structured JSON) or when it's difficult to describe in words what you want the model to do (i.e. write in a particular style).

> ##### Note
>
> - You can open similar examples directly in Google AI Studio from the [examples gallery](https://ai.google.dev/examples)

### Step 1 - Create a structured prompt

- To start, you'll define the structure for the prompt by creating two columns: a **Product** input column and a **Product copy** output column.

- To create the structured prompt:

    1. Open Google AI Studio.

    2. In the **Create new prompt** menu, click **Structured prompt**.

    3. In the text input box labeld **Optional tone and style instructions for the model**, paste the following:

        ```
        You are a product marketer targeting a Gen Z audience. Create exciting and fresh advertising copy for produdcts and their simple description. Keep copy under a few sentences long.
        ``

    4. Replace the default **Input** header text (`input:`) with `Product:`.

    5. Replace the default **Output** header text (`output:) with `Product copy:`.

> ##### Tip
>
> - Adding colons to the end of column names makes it easier for the model to parse the structure.

### Step 2 - Add examples

- By providing the model a couple of example product descriptions, you can guide it to replicate a similar style when generating its own outputs.

- You can enter examples manually or import from a file using the import data menu.

- (Optional) To import examples from a file:

    1. In the top, right corner of the examples table, click **Actions > Import examples**.

    2. In the dialog, select a CSV or Google Sheets file in your Google Drive, or upload one from your computer.

    3. In the import examples dialog, choose which columns to import and which to leave out. The dialog also lets you specify which data column imports to which table column in your structured prompt.

### Step 3 - Test your prompt

- Once you have the examples that show the model what you want, test your prompt with new input in the **Test your prompt** table at the bottom.

### Step 4 - Next steps

- Once you're happy with your prompt, you can save your project to Google Drive by clicking the **Save** button, or export it to code by clicking the **Get code** button.

- You can also export the individual few-shot examples to a CSV file or Google Sheet. Click **Export examples** in the **Action** menu to export your examples.

## Further reading

- To learn how to craft better prompts, check out the Prompting > Intro to prompting.
