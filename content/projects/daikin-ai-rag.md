---
title: "AI for customer-quotation automation"
date: 2026-05-26
draft: false
summary: "An ongoing project at Daikin Germany using retrieval-augmented generation to automate customer quotation workflows."
role: "Pre-Sales Consultant, lead on the AI integration thread"
period: "2025 – present"
stack: ["Python", "LLMs (Anthropic / OpenAI / Ollama)", "Retrieval-Augmented Generation", "LangChain", "Power BI"]
links: []
featured: true
---

At Daikin Airconditioning Germany, customer quotations in the HVAC pre-sales process involve a mix of structured product data, semi-structured engineering specifications, and unstructured customer requirements. Generating a quotation is a careful translation exercise — pulling the right products, applying the right configurations, and writing the result in language a customer will recognise.

I am leading the AI-integration thread of an effort to automate parts of that workflow with retrieval-augmented generation. The interesting design question is not the model — there is no shortage of capable models — but the retrieval surface: how to make the product catalogue, the configuration rules, and the prior-quotation corpus addressable in a way that lets a model produce something a pre-sales engineer would actually approve.

A side benefit of the work has been clarifying which parts of the existing process genuinely deserve automation, versus which parts only feel manual because the tooling never caught up. Some of the most valuable changes have been removing steps entirely rather than automating them.

Concrete numbers from the broader digital-process redesign (which the AI work is one part of): the Pre-Sales team's annual workload dropped by roughly ten percent after the first round of changes. The AI thread aims to compound that, not replace it.
