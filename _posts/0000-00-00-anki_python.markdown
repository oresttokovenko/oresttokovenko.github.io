---
layout: post
title: Generating Custom Anki Decks with GPT-4 and LangChain
description:
date:   2023-04-01
image:  '/images/anki_python.jpg'
tags:   [Python]
---

I am no stranger to the ever-evolving world of technology as a user of the Data tools/services ecosystem. Recently, I've been diving into the intricacies of Java for its high-performance capabilities, scalability, and seamless integration with popular technologies like Apache Flink, Iceberg and Kafka (which all consist of a majority Java codebase). To help me retain all the knowledge I've been acquiring, I rely on my trusty Anki app. With its spaced repetition magic and flashcard-style recall, Anki has been my go-to learning companion.

But, as you might expect, creating Anki cards manually for every concept in the "Learning Java, 5th Edition" by Marc Loy can be quite a chore. So, like any good engineer, I decided to automate this process using LangChain and GPT-4. In this blog post, I will show you how I used these tools to create custom Anki decks that help me learn Java more efficiently.

### Leveraging GPT-4 and LangChain for Flashcard Creation

To begin, I utilized OpenAI's GPT-4 to generate Q&A pairs from my Java book's content. The process involved submitting a prompt to GPT-4 containing the text I aimed to understand, as well as my prompt. For this task, I utilized a few handy libraries and tools. The LangChain library, which hosts PydanticOutputParser and Pydantic BaseModel, helped facilitate data validation. This ensured that the output from GPT-4 conformed to my specified JSON array schema and was parsed into structured data.

```python
class FlashCard(BaseModel):
    question: str = Field(description="The question for the flashcard")
    answer: str = Field(description="The answer for the flashcard")

class FlashCardArray(BaseModel):
    flashcards: List[FlashCard]
```

<br>

 I defined the Pydantic classes above and once the data was parsed, I converted the FlashCardArray object into a Python list. I then transformed this list into a Pandas dataframe, which was subsequently appended to the CSV file. With these steps, I successfully created structured data from a GPT-4 output. If you want to append more text than GPT-4 can support in one API call, simply update the `input.txt` file with another piece of text and the program will append it to the existing csv. Once you're done creating flashcards, you can proceed to the next step. 

![Gif]({{site.baseurl}}/images/anki_gpt_pt1.gif)
*Converting the input text into structured data. Each API call and function run takes about 20 seconds*

### Generating the Anki Deck

Next, I created a Python script that took the CSV output from GPT-4 and converted it into Anki flashcards. To do this, I used a fantastic Python library called `genanki`. `genanki` helps create Anki decks programmatically, so all I had to do was write a script that mapped the questions and answers in the CSV to the format required. To make things simple, the `DECK_NAME` variable defined in the `.env` file will be the name for everyting - the csv and the deck. If you want to create more decks, simply update this variable and your old files will be preserved.

![Gif]({{site.baseurl}}/images/anki_gpt_pt2.gif)
*Generating an Anki Deck - `.apkg` file from the flashcard csv*

### Importing the Generated Deck into Anki

Once I had the `.apkg` file, I imported it into the Anki app, and voilà! I had a shiny new custom Anki deck at my fingertips. Now, I can efficiently review all the concepts I'm learning, and the process is repeatable for any new material I encounter.

![Gif]({{site.baseurl}}/images/anki_gpt_pt3.gif)
*Importing the deck into Anki and testing the first flashcard*

The beauty of this approach is that it's not just limited to Java. With a little tweaking, you can apply this technique to virtually any subject matter you're studying. So, whether you're learning a new programming language or brushing up on your 18th-century French literature, LangChain and GPT-4 can help you create custom Anki decks tailored to your learning needs. 

You can find the prompt, and script that I used in my [Github repo here](https://github.com/oresttokovenko/gpt-anki){:target="_blank"}{:rel="noopener noreferrer"}. To set up the environment, follow the instructions in the link. 

As I continue to learn Java and explore new technologies, I am grateful for tools like Python, LangChain, and GPT-4. Not only do they help me learn more efficiently, but they also make the process more enjoyable. I can't wait to see what other automation tricks I can come up with as I delve deeper into the world of Data Engineering!

