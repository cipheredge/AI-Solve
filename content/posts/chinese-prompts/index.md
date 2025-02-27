---
title: "New AI Development Discovery: Chinese Could Be the Preferred Language for Prompt Engineering"
date: 2025-02-13T23:20:13+04:00
slug: 'chinese-prompting-advantage'
draft: false
cover: "https://jiejue.obs.ap-southeast-1.myhuaweicloud.com/20250213231817670.webp"
tag:
  - AI
  - prompt-engineering
  - LLM-development
---

Recently, OpenAI's new AI model was found to automatically use Chinese for reasoning when processing English queries. This unexpected discovery has sparked widespread discussion in the tech community: in the AI era, Chinese might be more suitable than English for certain types of thinking processes. What does this mean for developers working on AI applications?

<!--more-->

## An Unexpected Discovery

According to a [report](https://gizmodo.com/why-does-chatgpts-algorithm-think-in-chinese-2000550311) by Gizmodo, users discovered that OpenAI's new model displays Chinese characters during reasoning, even when the conversation is entirely in English. More interestingly, this phenomenon isn't random but rather a "natural selection" by AI when processing certain problems.

AI expert Rohan Paul explains that certain languages might offer better tokenization efficiency or easier mapping for specific types of problems. This suggests that AI models find Chinese thinking more computationally optimal when handling certain problems.

## Implications for AI Development

This discovery has significant implications for AI application developers, especially those involved in prompt engineering. When developing applications based on Large Language Models (LLMs), we typically write prompts to guide AI in completing specific tasks. Traditionally, many developers habitually write prompts in English, believing it to be more "standard" or "professional."

But now we know:

1. LLMs suffer no computational efficiency loss when processing Chinese
2. The ideographic nature of Chinese might provide more efficient information encoding
3. Compared to English, Chinese might have advantages in certain logical reasoning scenarios

## Technical Principles Analysis

Why is this happening? It's related to how LLMs work. Modern LLMs use tokenizers to convert input text into numerical vectors. For a word like "artificial intelligence":
- Chinese: 人工智能 (4 characters, likely decomposed into fewer tokens)
- English: "Artificial Intelligence" (22 characters, usually decomposed into more tokens)

In some cases, Chinese's high information density might help AI process information more efficiently. This also explains why some AI models "spontaneously" choose to reason in Chinese.

More importantly, in today's world of highly unified encoding standards (UTF-8 has become the de facto standard), Chinese has shown unexpected advantages in text processing. In contrast, the English-speaking world has increasingly incorporated special Unicode characters like emojis in recent years, adding complexity and uncertainty to encoding.

This phenomenon somewhat confirms:

> In formal AI development scenarios, rigorous Chinese characters and words might be more conducive to maintaining code and prompt clarity than English text filled with various special symbols.

## Practical Application Recommendations

Based on this discovery, when developing LLM-based applications, we can consider:

1. Prioritizing Chinese when writing prompt templates
2. Not deliberately avoiding Chinese when designing AI reasoning processes
3. Trying Chinese expressions for complex logic to see if better results can be achieved

Note that this doesn't mean we should completely abandon English. In practical development:
- Code interacting with third-party libraries still primarily uses English
- System-level configurations and API calls still follow English conventions
- But at the prompt engineering level, Chinese might bring unexpected advantages

## Future Outlook

This discovery might indicate a new trend in the AI field: language choice is no longer purely a matter of habit but should be based on efficiency and effectiveness. This is undoubtedly good news for Chinese developers: we can more confidently use our native language in AI development without worrying about any technical disadvantages.

As AI technology continues to develop, we might see:
- More prompt engineering frameworks specifically designed for Chinese
- More AI application scenarios leveraging Chinese language characteristics
- More research exploring the relationship between language features and AI performance

In this new AI era, Chinese as a powerful expression tool is showing its unique value. This is not only a technical advancement but also provides Chinese developers with a unique advantage in global AI development.

Note: The article's image was generated by AI, showing the interweaving of Chinese and English in the digital world, suggesting that in the AI era, language choice depends more on its practical effectiveness in specific scenarios.
