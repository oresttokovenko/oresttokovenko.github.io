---
layout: post
title: How to Use Python and ChatGPT to Generate Custom Anki Decks
description:
date:   2023-04-01
image:  '/images/anki_python.jpg'
tags:   [Python]
---

I am no stranger to the ever-evolving world of technology as a user of the Data tools/services ecosystem. Recently, I've been diving into the intricacies of Java for its high-performance capabilities, scalability, and seamless integration with popular technologies like Apache Flink and Kafka. To help me retain all the knowledge I've been acquiring, I rely on my trusty Anki app. With its spaced repetition magic and flashcard-style recall, Anki has been my go-to learning companion.

But, as you might expect, creating Anki cards manually for every concept in the "Learning Java, 5th Edition" by Marc Loy can be quite a chore. So, like any good engineer, I decided to automate this process using Python and ChatGPT. In this blog post, I will show you how I used these tools to create custom Anki decks that help me learn Java more efficiently.

### Step 1: Leverage ChatGPT for Flashcard Creation

First things first, I used ChatGPT to generate questions and answers based on the contents of my Java book. By sending a prompt to ChatGPT with the text I wanted to learn (this took several passes as GPT does have a limit of tokens it can input at a time), it analyzed the material and generated a CSV-like output with relevant questions and answers which can be easily copied and pasted into the csv to be used by our next step.

![Gif]({{site.baseurl}}/images/gptpy_demo.gif) 

### Step 2: Write a Python Script to Generate Anki Decks

Next, I created a Python script that took the CSV output from ChatGPT and converted it into Anki flashcards. To do this, I used a fantastic Python library called `genanki`. `genanki` helps create Anki decks programmatically, so all I had to do was write a script that mapped the questions and answers in the CSV to the format required.

![Gif]({{site.baseurl}}/images/gptpy_demo_2.gif)

{% highlight zsh %}

python csv_to_anki.py <input_csv> <output_anki>

{% endhighlight %}

### Step 3: Import the Generated Deck into Anki

Once I had the `.apkg` file, I imported it into the Anki app, and voilà! I had a shiny new custom Java Anki deck at my fingertips. Now, I can efficiently review all the Java concepts I'm learning, and the process is repeatable for any new material I encounter.

![Screenshot]({{site.baseurl}}/images/import_screenshot.jpg)

The beauty of this approach is that it's not just limited to Java. With a little tweaking, you can apply this technique to virtually any subject matter you're studying. So, whether you're learning a new programming language or brushing up on your 18th-century French literature, Python and ChatGPT can help you create custom Anki decks tailored to your learning needs. 

You can find the prompt, and script that I used in my [Github repo here](https://github.com/oresttokovenko/gpt-anki){:target="_blank"}{:rel="noopener noreferrer"}. To set up the environment, follow the instructions in the link. 

<!-- [Github repo here](https://github.com/oresttokovenko/gpt-anki) -->

As I continue to learn Java and explore new technologies, I am grateful for tools like Python, ChatGPT, and Anki. Not only do they help me learn more efficiently, but they also make the process more enjoyable. I can't wait to see what other automation tricks I can come up with as I delve deeper into the world of Data Engineering!

