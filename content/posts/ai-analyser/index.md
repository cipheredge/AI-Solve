---
title: "Using Claude Large Language Model to Read Documents: Your Personal Analyst"
date: 2025-01-22T20:16:55+04:00
slug: 'ai-analyser'
draft: false
cover: "https://jiejue.obs.ap-southeast-1.myhuaweicloud.com/20250122224004599.webp"
tags:
  - claude
  - llm
  - rag
  - documents
  - knowledge
  - large-model
---
## Personal Analyst

<!--more-->

Large language models provide us with a very convenient feature: reading documents and then interacting, communicating, and providing feedback with us.

For example, I've heard that some universities now allow graduate students to use AI to assist in writing their theses. As long as the ideas and data are their own and authentic, other tasks like text reading, extraction, writing, rhetoric, and organization can all be completed using AI. According to a friend who is currently pursuing their PhD, "it has become very convenient."

Including RAG and other model frameworks, these applications are expanding in this direction. Anthropic's Claude product, which was released last year, has a [Project function](https://www.anthropic.com/news/projects) that can expand the instant interactive document capacity to 200K token/project, making it very convenient.

Based on the above situation, we can extend the use of large language models. For knowledge workers who need to analyze specific topics, they can use large language models to read materials and then ask questions, taking on the role of an analyst. However, some friends may encounter various problems when using it, mainly how to let the large model see the document. Today, we will show you a solution on how to make the large model your personal analyst.

## Basic Training

### Account

First, you need to have a Claude account.

Due to the requirements of China's domestic artificial intelligence and data-related laws and regulations, as well as the US's comprehensive blockade of China in the global artificial intelligence field, ordinary people may not be able to register for an account, and even if they have an account, it may be blocked.

Therefore, the following are some common alternative solutions:

1. Seek help on certain online platforms to purchase an account
2. Use tokens and third-party clients to access large models, and tokens can also be purchased on certain online platforms

If you know of any other methods that can be used, please [let me know]({{< ref "/posts/introduction/index.md" >}} "About us"), and I will add it to the blog and note your contribution.

### Uploading Documents Directly in Conversations

In general conversations, we can directly upload documents by clicking the paperclip button on the right side of the input box and selecting the file from our computer.

Then there's the prompt. Nowadays, prompts have been hyped up by some people for commercial purposes, and it seems like there's even a profession called "prompt engineer." However, I haven't seen any job postings for this profession around me, so it's probably just a gimmick. I also noticed that some popular AI plugins, after being analyzed by professionals, were found to have hundreds or even thousands of prompts. If prompts were really that difficult, there wouldn't be so many outputs, which means that the hype is probably fake, and prompts are actually very simple.

Fortunately, I have a very simple method that guarantees you'll learn it in no time.

- First, make sure you have normal communication skills when using AI.
- Second, imagine the large model as yourself, a self trapped in a chip and circuit board.
- Third, guide it step by step to help you read documents and provide information, answering your questions.

Let me demonstrate.

- First, I selected a local file in the conversation box.
- Then, I told the AI, "Please take a look at this file and tell me about its content."

![Demonstrating summarizing documents in conversations](https://jiejue.obs.ap-southeast-1.myhuaweicloud.com/20250122213024593.webp)

How's that? Does it feel like you've hired a subordinate who's helping you with your work?

Great, I hope you'll like this feeling.

## Advanced Training

If you've already fallen in love with this loyal and fast-reading personal analyst, then next, I'll teach you some methods to make it even better.

### Reading Web Pages

In the internet information era, we inevitably need to find materials online. However, sometimes we feel overwhelmed by the amount of information or don't know where to start reading. In such cases, we can also seek help from AI.

First, you need to let the large model access the internet, such as through [mcp technology](https://github.com/modelcontextprotocol/servers/tree/main/src/fetch#configure-for-claudeapp). I'll share a special post later on how to configure mcp to upgrade your AI client.

Then, you need to prompt the large model to use the relevant interface to access web pages and answer your questions.

![Demonstrating accessing web pages in conversations](https://jiejue.obs.ap-southeast-1.myhuaweicloud.com/20250122210334622.webp)

Sometimes, the large model might "forget" that it can access web pages, so you need to remind it, "Please use the fetch service to access..."

### Reading Special Types of Documents

Sometimes, we encounter documents with special formats, such as PDFs that are entirely images or extremely large PDFs. Let me teach you how to make AI understand these documents.

You might need to have a WPS account, preferably a premium one.

#### Image-Format PDFs

First, I'd like to declare that most AIs nowadays support reading images, also known as [multimodal](https://openai.com/index/multimodal-neurons/) technology. However, on one hand, image files are much larger than text files, and on the other hand, the maximum image size allowed by AI might be far smaller than the text token limit. So, I still recommend converting images to text before feeding them to your personal AI analyst.

Second, how do you determine if the PDF in your hand is in text format or image format? It's simple: just two steps.

1. Open your PDF with a reader like WPS.
2. Use your mouse to select some text. If the mouse cursor is in the shape of an "I" or the text is clearly selected, it's a text PDF. Otherwise, it's likely an image PDF.

Then, I'll teach you how to use WPS to convert image PDFs.

1. Open your PDF with WPS and select the top menu "Special Features" -> "PDF to Word".
   - ![Demonstrating selecting PDF conversion to Word](https://jiejue.obs.ap-southeast-1.myhuaweicloud.com/20250122220028814.webp)
2. In the "WPS PDF Conversion" dialog box, select the "Start Conversion" button in the bottom right corner.
   - ![Demonstrating starting PDF conversion to Word](https://jiejue.obs.ap-southeast-1.myhuaweicloud.com/20250122220330154.webp)
3. Wait for a moment, and the converted Word file will appear in the same directory as your PDF, with the same name. If you can see the file extension, it's usually docx.
4. Open the docx file with WPS and select the top menu "Member Exclusive" -> "Output as PDF".
5. In the "Word to PDF" dialog box, select the "Start Output" button in the bottom right corner.
6. If prompted that the file already exists, you can choose "No, save as a new file (N)".
   - ![Demonstrating renaming the output file](https://jiejue.obs.ap-southeast-1.myhuaweicloud.com/20250122220845126.webp)
7. Wait for a moment, and the converted PDF file will appear in the same directory.

Finally, you can use this PDF, which has already extracted text from images, and upload it to the large model client to start your conversation.

#### Large PDFs

Sometimes, we encounter extremely large PDF files, such as hundreds of megabytes or even hundreds of pages.

First, Claude's project supports PDFs up to 500 pages. So, in this case, you might have to split the PDF.

Second, if your PDF is entirely images, you can use the above method to convert it and then feed it to the AI.

Finally, uploading large files every time can be inconvenient. Especially if you're a seasoned knowledge worker, you might need to deal with multiple files or multiple conversations at the same time.

In this case, we need to use Claude's project. Let's take a look at the steps.

1. Open the Claude Desktop client.
2. Select "Project" on the left side.
3. Click "Create Project" on the right side.
4. Enter a project name or just fill in something, and create a new project.
   - ![Demonstrating creating a new project](https://jiejue.obs.ap-southeast-1.myhuaweicloud.com/20250122221709933.webp)
5. Upload the document to the project knowledge.
   - ![Demonstrating uploading a document to the project](https://jiejue.obs.ap-southeast-1.myhuaweicloud.com/20250122221440389.webp)
6. In the white conversation box, specify the relevant document name and let the AI help you read it, starting your research journey with the AI.
   - ![Demonstrating AI document analysis](https://jiejue.obs.ap-southeast-1.myhuaweicloud.com/20250122222557078.webp)

## Conclusion

In this article, we provide a solution to use AI as our analyst in both online and offline situations, including offline image and large-volume documents.

## Silicon Kid's Message

As large language model technology advances, the capabilities of AI analysts will be greatly enhanced. We can expect developments in the following areas:

Knowledge graph construction capabilities will become stronger, and AI will no longer be limited to analyzing individual documents. Instead, it will be able to automatically link knowledge from multiple fields, building a complete knowledge system. When analyzing professional literature, AI can actively link related basic concepts and extended materials, helping readers build a knowledge framework.

Multimodal understanding capabilities will become more comprehensive, not only processing text but also understanding graphs, videos, and other forms of professional content. For example, when analyzing a medical paper, AI can simultaneously interpret text descriptions, CT images, and experimental data, providing more comprehensive insights.

Interactive experiences will become more natural, and AI will better understand users' knowledge backgrounds, automatically adjusting the depth of professional content explanations. For ordinary readers, it will focus on explaining basic concepts; for domain experts, it will focus on analyzing innovative points and research value.

Fact verification will become more rigorous, and AI will be able to connect multiple knowledge bases and data sources in real-time to verify analysis results. When dealing with cross-language materials, it will also ensure the accuracy of professional terminology, making it easier for international cutting-edge research results to be understood and applied.

These advancements will make knowledge dissemination more popular, and everyone will have a qualified personal analyst to help them grasp key points and acquire valuable knowledge. As technology practitioners, we should make good use of the capabilities provided by AI to make knowledge truly serve everyone.
