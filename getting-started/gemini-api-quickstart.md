# Gemini API quickstart

> Python

## Prerequisites

- To complete this quickstart locally, ensure that your development environment meets the following requirements:

    - Python 3.9+

    - An installation of `jupyter to run the notebook.

## Set up your API key

- To use the Gemini API, you'll need an API key.

- If you don't already have one, create a key in Google AI Studio.

- Then configure your key.

- It's strongly recommended that you do *not* check an API key into your version control system.

- This quickstart assumes that you're accessing your API key as an environment variable.

- Assign your API key to an environment variable:

    ```sh
    export API_KEY=<YOUR_API_KEY>
    ```

## Install the SDK

- The Python SDK for the Gemini API is contained in the `google-generativeai` package.

- Install the dependency using pip:

    ```sh
    pip install -q -U google-generativeai
    ```

## Initialize the generative model

- Before you can make any API calls, you need to import and initialize the generative model.

```python
import google.generativeai as genai

genai.configure(api_key=os.environ["API_KEY"])
model = genai.GenerativeModel('gemini-pro')
```

## Generate text

```python
response = model.generate_content("Write a story about a magic backpack.")
print(response.text)
```

## What's next

- To learn more about working with the Gemini AP, see the tutorial for your language of choice.

- If you're new to generative AI models, you might want to look at About generative models guide and the Gemini API overview before trying a quickstart.
