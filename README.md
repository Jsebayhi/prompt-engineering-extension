# 🎯 Prompt Engineering Extension for Gemini CLI

[![GitHub Stars](https://img.shields.io/github/stars/Jsebayhi/prompt-engineering-extension?style=flat-square&color=gold)](https://github.com/Jsebayhi/prompt-engineering-extension/stargazers)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg?style=flat-square)](LICENSE)
[![Gemini CLI](https://img.shields.io/badge/Gemini%20CLI-Compatible%20Extension-4285F4?style=flat-square)](https://github.com/GoogleCloudPlatform/gemini-cli)

> [!NOTE]
> **Foundational DevEx & Prompt Architecture Project**
> 
> This extension was developed as an early exploration into LLM Developer Experience (DevEx) and structured prompt engineering methodologies for the Google Gemini CLI. It successfully established community traction (7+ stars) by providing automated prompt optimization workflows.


---

## 💡 Overview & Value Proposition

Crafting high-precision prompts for Large Language Models requires systematic rigor: establishing role definitions, scoping context, constraining output formats, and anticipating adversarial edge cases.

The **Prompt Engineering Extension** integrates directly into the [Gemini CLI](https://github.com/GoogleCloudPlatform/gemini-cli) to automate this process. It acts as an embedded prompt architect that transforms vague, natural-language requirements into structured, robust, and hallucination-resistant prompts.

---

## 🛠️ Core Engineering Frameworks

The extension encapsulates proven prompt design patterns into automated agent instructions:

### 1. The PCTR Framework
Ensures every prompt follows an unambiguous four-pillar structure:
* **Persona:** Precise role definition, domain expertise, and behavioral boundaries.
* **Context:** Necessary background data, constraints, and assumptions.
* **Task:** Granular, single-responsibility objective with clear success criteria.
* **Requirements:** Strict formatting, tone, output schema (e.g., JSON/Markdown), and negative constraints.

### 2. Multi-Step Reasoning Strategies
* **Chain of Thought (CoT):** Dynamically injects step-by-step reasoning scaffolds for complex deductive problems.
* **Tree of Thoughts (ToT):** Guides the model through branching exploration paths to evaluate multiple trade-offs before finalizing decisions.

### 3. Adversarial Hardening
* **Loophole Detection:** Scans user prompts for underspecified requirements, ambiguities, and potential leakage vectors.
* **Constraint Enforcement:** Injects explicit negative guardrails to prevent hallucination, prompt injection, and unwanted verbosity.

---

## 🚀 Quickstart & Installation

### Prerequisites
* [Gemini CLI](https://github.com/GoogleCloudPlatform/gemini-cli) installed and authenticated.

### Install Extension
Run the following command in your terminal:

```bash
gemini extension install https://github.com/Jsebayhi/prompt-engineering-extension.git
```

*Alternative (GitLab Mirror):*
```bash
gemini extension install https://gitlab.com/jeremy-sebayhi-public-work/gemini-cli/prompt-engineering-extension.git
```

---

## 📋 Example Workflows

Once installed, the Gemini CLI agent automatically activates the prompt engineering skill whenever prompt optimization, system instructions, or refinement requests are detected:

### Scenario A: Drafting Production System Prompts
```text
> "Help me build a production system prompt for a triage agent managing Kafka consumer group lag alerts."
```
* **Output:** Generates a complete PCTR-structured prompt with JSON schema constraints, escalation criteria, and token-saving rules.

### Scenario B: Prompt Hardening & Loophole Analysis
```text
> "Review this customer support prompt and harden it against user jailbreaks and out-of-scope queries."
```
* **Output:** Delivers a vulnerability report and a drop-in replacement prompt incorporating strict negative constraints and fallback behavior.

---

## 🏛️ Extension Architecture

This project implements the official Gemini CLI extension specification:
* `gemini-extension.json`: Defines the extension manifest, metadata, and skill entry points.
* `skills/prompt-engineering/`: Contains modular domain knowledge files and prompt templates injected during contextual activation.
* `commands/`: CLI command bindings and helper utilities.

---

## 📄 License

Distributed under the MIT License. See [LICENSE](LICENSE) for details.