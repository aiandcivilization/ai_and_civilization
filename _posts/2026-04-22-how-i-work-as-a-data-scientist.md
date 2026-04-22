---
layout: post
title: "How I Work as a Data Scientist: Skill Trees, CLI Tools, and Context Engineering"
date: 2026-04-22 15:00:00 +0200
description: "A practical breakdown of my data scientist workflow: skill trees, progressive disclosure, CLI-first automation, token efficiency, and continuous learning with LLM agents."
categories: [data-science, ai, engineering]
tags: [context-engineering, agent-skills, cli-tools, productivity, llm-workflows, knowledge-systems]
author: Yanis Labeyrie
image: /assets/img/data-scientist-skill-tree-architecture.png
mathjax: false
last_modified_at: 2026-04-22
---

For a long time, I made the same mistake most data scientists make when they start using LLMs seriously: I treated the model like a brighter interface, not like a system to engineer.

That approach does not scale.

You get bloated prompts, repeated explanations, fragile outputs, and constant rework. The model may be strong, but the workflow stays weak. The real bottleneck is not raw intelligence. It is information organization.

My job today is not just to analyze data. It is to build an environment where analysis, debugging, querying, documentation, and decision support become progressively cheaper over time. I think about my work as a data scientist the same way I think about a well-designed production system: reduce entropy, compress repetition, and turn local fixes into reusable infrastructure.

### 1. Do Not Solve Tasks, Solve Task Classes

The wrong way to work is to solve the same problem again and again with slightly different wording.

The right way is to identify the recurring class of problem and build a reusable path for it.

If I repeatedly need to inspect a database, compare schemas, clean inputs, generate documentation, or debug a pipeline, I do not want a model that improvises every time. I want a workflow that routes the request into the right abstraction with the least possible token waste.

That is why my setup is built around a tree of skills.

### 2. The Skill Tree

A skill is not just a prompt. It is a compact operational unit for a cluster of tasks.

But large monolithic skills become unreadable very quickly. They overload the agent with too many rules at once, which is the exact opposite of good context engineering. So I structure them as a tree:

- a parent skill defines the broad domain and routing logic
- subskills handle narrower workflows
- leaf skills contain the concrete procedures, commands, and edge-case behavior

This is progressive disclosure applied to agent systems.

The agent should not load the entire operating manual when it only needs one screwdriver.

![Skill tree architecture]({{ '/assets/img/data-scientist-skill-tree-architecture.png' | relative_url }})

That matters in practice. A data scientist does not only work on notebooks. In the same day, I may need to query production data, inspect a failing pipeline, compare reports, package a CLI utility, write documentation, and organize research. If all instructions are dumped into one giant file, the system becomes noisy and expensive. If the knowledge is layered properly, the agent only pulls the minimum viable context for the job.

The diagram above represents that architecture well: clusters at the top, finer tools underneath, and specialized subskills at the edges. It is less like a document tree and more like a retrieval graph for work.

When the structure matters, I want the agent to see the structure directly. That is also why code blocks matter in articles and in skills. Sometimes prose is too lossy. A formatting rule is clearer when it is shown as an artifact:

```md
---
name: sql-cli
description: "Use this skill when the user needs SQL discovery or SQL execution."
---

# sql-cli

## Instructions

### Step 1: Discover the database
sql-cli list-databases

### Step 2: Narrow the schema
sql-cli list-schemas --db_name prod_arc
```

This is not cosmetic. It reduces ambiguity. The agent sees the exact skeleton it is supposed to follow.

### 3. Why This Matters for Data Science

Data science is full of hidden repetition.

People talk about modeling, forecasting, experimentation, or machine learning, but a large share of the real work is operational:

- finding the right table
- understanding a schema
- validating a metric definition
- tracing a broken dependency
- cleaning inconsistent inputs
- rewriting the same explanatory note for the tenth time

If you handle those steps manually each time, your effective throughput collapses.

So my system is designed to eliminate redundant cognitive loops. The goal of a skill is simple: cut the time spent on recurrent tasks, and make the good path easier than the improvisational one.

### 4. CLI First, Not MCP First

One of the strongest design choices in my workflow is heavy reliance on CLI tools.

I generally prefer packaged command-line tools over more opaque server abstractions for repetitive operational work. The reason is straightforward: a good CLI tool exposes a stable interface, supports `--help`, documents its own methods, and gives the agent a discoverable surface for progressive disclosure.

If I want to solve a recurring problem, I would rather create or improve a CLI than force the model to remember a long natural-language procedure forever.

That creates a better loop:

1. I encounter a recurring task.
2. I package the solution as a CLI capability.
3. I expose clear commands and help text.
4. I route the agent to that tool through a skill.
5. When a failure appears, I improve the general method instead of patching one case by hand.

This is how you turn ad hoc work into infrastructure.

Take SQL as an example. Writing SQL by hand every time is a tax. So the objective is not merely to help the agent write better queries. The objective is to reduce or eliminate the need to write them manually at all. A good SQL CLI can guide database discovery, list schemas, inspect columns, run parameterized queries, and standardize common operations. Once that layer is solid enough, the model stops reinventing SQL and starts orchestrating a reliable interface.

For a data scientist, this is extremely powerful. You are no longer only analyzing data. You are designing the control plane around analysis.

### 5. Two Tools That Changed the Equation

Two tools illustrate the philosophy behind my stack particularly well.

#### Rust Token Killer

The first is RTK, short for Rust Token Killer. Its job is brutally practical: cut useless token consumption in shell interactions.

That may sound minor, but it is not. Agent systems leak efficiency through tiny repeated costs. If every shell command returns noisy output, every task becomes more expensive than necessary. Over dozens or hundreds of interactions, the waste compounds.

RTK is a reminder that productivity is often won at the interface layer, not only at the model layer.

Repository:

- [Rust Token Killer](https://github.com/rtk-ai/rtk)

#### code-review-graph

The second is `code-review-graph`, which gives the agent a higher-level understanding of a codebase by mapping structure, dependencies, and impact radius.

This matters because one of the biggest token sinks in coding and analytics work is brute-force code reading. If the system can navigate a graph of functions, files, and relationships instead of scanning everything line by line, you massively reduce context waste while gaining a more global view of the code.

That global view is crucial. As soon as a data scientist writes internal tools, ETL jobs, analytics services, or reporting pipelines, code comprehension becomes part of the job. A graph-based perspective lets the agent reason about the system rather than just stare at text.

Repository:

- [code-review-graph](https://github.com/tirth8205/code-review-graph)

### 6. Continuous Learning Is the Real Missing Layer

Most people still use LLMs as if each conversation should start from zero.

That is not how real expertise works.

To understand this, we need an analogy from neuroscience. When a neural pathway is used repeatedly, it becomes reinforced. Signal transmission gets more efficient. In biological terms, myelination improves conduction. In practical terms, repetition becomes capability.

I try to reproduce that dynamic artificially.

When the agent hits friction, I do not just fix the immediate problem. I ask: what general behavior was missing? Should this become a new skill, a sharper routing rule, a better CLI method, or a stronger formatting convention?

This is where meta-behavior improvement becomes important. I maintain skills whose purpose is to refine other skills, tighten instructions, and improve the system after failures. The point is not to create more documentation for its own sake. The point is to create plasticity inside the workflow.

That is my version of continuous learning.

And to be blunt, this is still an unsolved problem in the broader LLM ecosystem. Models are powerful, but durable operational learning remains weak unless the user engineers it explicitly.

### 7. Context Engineering Is Information Architecture

People often speak about prompt engineering as if the key problem were wording.

I disagree.

The deeper problem is information architecture.

Which instructions should always be loaded? Which ones should be deferred? Which tools should expose their own help system? Which workflows deserve a dedicated abstraction? Which recurring fixes should be promoted into system behavior?

That is context engineering in the practical sense.

A good workflow does not merely give the model more context. It gives the model the right context at the right level of granularity. That is why progressive disclosure is central to how I work. It keeps the system legible, modular, and cheap enough to scale.

### 8. My Actual Objective as a Data Scientist

I do not define my work only by dashboards, models, notebooks, or SQL queries.

I define it by the rate at which I can transform repeated friction into reusable capability.

That includes:

- organizing knowledge so the right method is easy to retrieve
- building skills that route work cleanly
- creating CLI tools for recurrent operations
- strengthening abstractions after each failure
- reducing token waste across the entire stack

In other words, I am not just producing analyses. I am building a system that gets better at producing them.

That is the real leverage.

And I suspect this is where a large part of high-performance data science is going: less time spent performing isolated tasks, more time spent engineering the environment that performs them repeatedly with lower cost, lower error, and higher clarity.

### References & Data Sources

- [Rust Token Killer (RTK)](https://github.com/rtk-ai/rtk)
- [code-review-graph](https://github.com/tirth8205/code-review-graph)
