# Document search with embeddings

## Overview

- You will use the Python client library to build a word embedding that allows you to compare search strings, or questions, to document contents.

- In this tutorial, you'll use embeddings to perform document search over a set of documents to ask questions related to the Google Car.

## Prerequisites

- Python 3.9+

- An installation of `jupyter` to run the notebook.

## Setup

```shell
$ pip install -U -q google.generativeai
```

```python
import textwrap
import numpy as np
import pandas as pd

import google.generativeai as genai

from IPython.display import Markdown
```

### Grab an API Key

- You must obtain an API key.

- If you don't already have one, create a key with one click in Google AI Studio.

- Once you have the API key, pass it to the SDK.

- You can do this in two ways:

    - Put the key in the `GOOGLE_API_KEY` environment variable (the SDK will automatically pick it up from there).

    - Pass the key to genai.configure(api_key=...)`

> ##### Key Point
>
> - Any embedding model will work for this tutorial, but for real applications it's important to choose a specific model and stick with it.

> ##### Note
>
> - At this time, the Gemini API is only available in certain regions

```python
for m in genai.list_models():
    if 'embedContent' in m.supported_generation_methods:
        print(m.name)
```

## Embedding generation

### API changes to Embeddings with model embedding-001

- For the new embeddings model, embedding-001, there is a new task type parameter and the optional title (only valid with task_type=`RETRIEVAL_DOCUMENT`).

- The task types are:

|Task Type|Description|
|-|-|
|RETRIEVAL_QUERY|Specifies the given text is a query in a search/retrieval setting.|
|RETRIEVAL_DOCUMENT|Specifies the given text is a document in a search/retrieval setting.|

> ##### Note
>
> - Specifying a `title` for `RETRIEVAL_DOCUMENT` provides better quality embeddings for retrieval.

```python
title = "The next generation of AI for developers and Google Workspace"
sample_text = ("Title: The next generation of AI for developers and Google Worspace"
               "\n"
               "Full article:\n"
               "\n"
               "Gemini API & Google AI Studio: An approachable way to explore and prototype with generative AI applications")

model = 'models/embedding-001'
embedding = genai.embed_content(model=model,
                                content=sample_text,
                                task_type="retrieval_document",
                                title=title)

print(embedding)
```

## Building an embeddings database

- Turn them into a dataframe for better visualization.

```python
DOCUMENT1 = {
        "title": "Operating the Climate Control System",
        "content": "Your Googlecar has a climate control system that allows you to adjust the temperature and airflow in the car. To operate the climate control system, use the buttons and knobs located on the center console.  Temperature: The temperature knob controls the temperature inside the car. Turn the knob clockwise to increase the temperature or counterclockwise to decrease the temperature. Airflow: The airflow knob controls the amount of airflow inside the car. Turn the knob clockwise to increase the airflow or counterclockwise to decrease the airflow. Fan speed: The fan speed knob controls the speed of the fan. Turn the knob clockwise to increase the fan speed or counterclockwise to decrease the fan speed. Mode: The mode button allows you to select the desired mode. The available modes are: Auto: The car will automatically adjust the temperature and airflow to maintain a comfortable level. Cool: The car will blow cool air into the car. Heat: The car will blow warm air into the car. Defrost: The car will blow warm air onto the windshield to defrost it."}
DOCUMENT2 = {
        "title": "Touchscreen",
        "content": "Your Googlecar has a large touchscreen display that provides access to a variety of features, including navigation, entertainment, and climate control. To use the touchscreen display, simply touch the desired icon.  For example, you can touch the \"Navigation\" icon to get directions to your destination or touch the \"Music\" icon to play your favorite songs."}
DOCUMENT3 = {
        "title": "Shifting Gears",
        "content": "Your Googlecar has an automatic transmission. To shift gears, simply move the shift lever to the desired position.  Park: This position is used when you are parked. The wheels are locked and the car cannot move. Reverse: This position is used to back up. Neutral: This position is used when you are stopped at a light or in traffic. The car is not in gear and will not move unless you press the gas pedal. Drive: This position is used to drive forward. Low: This position is used for driving in snow or other slippery conditions."}

documents = [DOCUMENT1, DOCUMENT2, DOCUMENT3]
```

```python
df = pd.DataFrame(documents)
df.columns = ['Title', 'Text']
df
```

```python
# Get the embeddings of each text and add to an embeddings column in the dataframe
def embed_fn(title, text):
    return genai.embed_content(model=model,
                               content=text,
                               task_type="retrieval_document",
                               title=title)["embedding"]

df['Embeddings'] = df.apply(lambda row: embed_fn(row['Title'], row['Text']), axis=1)
df
```

### Document search with Q&A

- You will ask a question about hyperparameter tuning, create an embedding of the question, and compare it against the collection of embeddings in the dataframe.

- The embedding of the question will be a vector, which will be compared against the vector of the documents using the dot product.

- This vector returned from the API is already normalized.

- The dot product represents the similarity in direction between two vectors.

- The values of the dot product can range between -1 and 1, inclusive.

- If the dot product between two vectors is 1, then the vectors in the same direction.

- If the dot product value is 0, then these vectors are orthogonal, or unrelated, to each other.

- Lastly, if the dot product is -1, then the vectors point in the opposite direction and are not similar to each other.

- Note, with the new embeddings model (`embedding-001`), specify the task type as `QUERY` for userquery and `DOCUMENT` when embedding a document text.

```python
query = "How do you shift gears in the Google car?"
model = 'models/embedding-001'

request = genai.embed_content(model=model,
                              content=query,
                              task_type="retrieval_query")
``

- Use the `find_best_passage` function to calculate the dot products, and then sort the dataframe from the largest to smallest dot product value to retrieve the relevant passage out of the database.

```python
def find_best_passage(query, dataframe):
    """
    Compute the distances between the query and each document in the dataframe
    using the dot product.
    """
    query_embedding = genai.embed_content(model=model,
                                          content=query,
                                          task_type="retrieval_query")
    dot_products = np.dot(np.stack(dataframe['Embeddings']),
                          query_embedding["embedding"])
    idx = np.argmax(dot_products)
    return dataframe.iloc[idx]['Text'] # Return text from index with max value
```

- Vew the most relevant document from the database:

```python
passage = find_best_passage(query, df)
passage
```

- Input your own custom data below to create a simple question and answering example.

- You will still use the dot product as a metric of similarity.

```python
def make_prompt(query, relevant_passage):
    escaped = relevant_passage.replace("'", "").replace('"', "").replace("\n", " ")
    prompt = textwrap.dedent("""You are a helpful and informative bot that answers questions using text from the reference passage included below. \
    Be sure to respond in a complete sentence, being comprehensive, including all relevant background information. \
    However, you are talking to a non-technical audience, so be sure to break down complicated concepts and \
    strike a friendly and converstional tone. \
    If the passage is irrelevant to the answer, you may ignore it.
    QUESTION: '{query}'
    PASSAGE: '{relevant_passage}'
      ANSWER:
    """).format(query=query, relevant_passage=escaped)

    return prompt
```

```python
prompt = make_prompt(query, passage)
print(prompt)
```

- Choose one of the Gemini content generation models in order to find the answer to your query.

```python
model = genai.GenerativeModel('gemini-1.5-pro-latest')
answer = model.generate_content(prompt)
```

```python
Markdown(answer.text)
```

- The provided passage does not contain information about how to shift gears in a Google car, so I cannot answer your question from this source.
