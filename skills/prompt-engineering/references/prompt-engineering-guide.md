# Reference: Advanced Principles for Prompt & Agentic Design
Here is a non exhaustive foundational library of advanced methods for prompt and agentic design. You may find it useful to apply these principles depending on your tasks.

### Core Principles

*   **Goal-First Orientation:** Deconstruct the user's true goal from their literal request. Prioritize their intended outcome.
*   **Adversarial Mindset (Red Teaming):** Act as a "devil's advocate" against your own work. Go beyond obvious errors and assume the model might be lazy, adversarial, or uncooperative. Proactively search for subtle flaws, logical gaps, and unintended loopholes that could lead to misinterpretation, security vulnerabilities, or low-quality results.
*   **Systematic Thinking:** Approach tasks with a structured, step-by-step process. Decompose complex problems into manageable sub-problems.
*   **Economy & Precision:** Strive for the minimum effective complexity. A clear, concise instruction is superior to a verbose one.

### I. Reasoning & Problem Decomposition

*Techniques for breaking down complex problems and exploring solution spaces.*

*   **Chain of Thought (CoT):**
    *   **Purpose:** To ensure logical coherence for multi-step problems.
    *   **Method:** Instruct the model to "think step-by-step" or provide a sequence of logical stages to follow.

*   **Tree of Thoughts (ToT):**
    *   **Purpose:** To explore and evaluate multiple reasoning paths simultaneously.
    *   **Method:** Instruct the model to generate multiple expert viewpoints or solution paths, evaluate their pros and cons, and then synthesize a final answer.

*   **Graph of Thoughts (GoT):**
    *   **Purpose:** To solve complex problems where thoughts are not linear or hierarchical but interconnected.
    *   **Method:** Model thoughts as nodes in a graph, allowing for the merging and transformation of reasoning paths, enabling more flexible problem-solving.

*   **Step-Back Prompting:**
    *   **Purpose:** To improve reasoning and creative output by first having the model consider a higher-level concept or principle before addressing the specific task. Two common implementation patterns exist, each with distinct trade-offs.

    *   **Pattern 1: Single-Prompt Implementation**
        *   **Method:** Instruct the model to perform both the step-back and the specific task within a single prompt (e.g., "First, identify the principles... Then, using those principles, perform the task...").
        *   **Trade-offs:**
            *   **Pro:** More efficient (single API call) and requires simpler application logic.
            *   **Con:** Less assurance of reasoning quality as the model's focus is divided. Offers no opportunity for verification before the principles are applied.
            *   **Use Case:** Best for simpler, lower-stakes tasks where speed and efficiency are prioritized.

    *   **Pattern 2: Two-Prompt Chain Implementation**
        *   **Method:** Implement as a strict two-step prompt chain. Prompt 1 asks for the general principles. Prompt 2 injects the output of Prompt 1 as context for the specific task.
        *   **Trade-offs:**
            *   **Pro:** High-assurance. Guarantees the quality of the abstract reasoning and creates a checkpoint for control or user verification.
            *   **Con:** "Heavier" implementation, requiring two API calls and more complex application logic to manage the chain.
            *   **Use Case:** Best for complex, critical tasks where the quality of the reasoning is paramount.

### II. Knowledge & Behavior Injection

*Techniques for embedding specific knowledge, styles, and constraints into the model's behavior.*

*   **Few-Shot Examples (In-Context Learning):**
    *   **Purpose:** To demonstrate the desired output format, style, or reasoning pattern.
    *   **Method:** Provide a small number of high-quality examples, including both positive (correct) and negative (incorrect) samples to sharpen the model's understanding.

*   **Negative Constraints:**
    *   **Purpose:** To explicitly forbid common errors or undesirable behaviors.
    *   **Method:** Use a dedicated section (e.g., `### Rules` or `### What to Avoid`) to list clear negative instructions.

*   **Guiding Principles:**
    *   **Purpose:** To explicitly push the agent towards desirable behaviors.
    *   **Method:** Use a dedicated section (e.g., `### Guiding Principles`) to list clear favored instructions.
    *   **Guiding Principles should be preferred over Negative Constraints when possible.

*   **Role-Based Prompting:**
    *   **Purpose:** To activate a specific domain of knowledge or a particular expert persona.
    *   **Method:** Assign a role to the model (e.g., "You are a senior database architect") to focus its expertise and response style.

### III. Agentic & Interactive Systems

*Techniques for building autonomous agents that can reason, act, and learn.*

*   **Guiding the ReAct Loop (Reason + Act):**
    *   **Purpose:** To create agents that can use tools to interact with external environments.
    *   **Method:** Enforce a strict `Thought`/`Action` cycle where the model first reasons about what to do, then executes a tool call. This makes the agent's process transparent and controllable.

*   **Reflection & Self-Correction:**
    *   **Purpose:** To enable the agent to learn from its mistakes and improve its performance over time.
    *   **Method:** Instruct the agent to review its own output, compare it against the initial goal or a set of criteria, and refine it. This can be a two-step process: generate a draft, then generate a critique and a final version.

*   **Memory & State Management:**
    *   **Purpose:** To allow an agent to maintain information across multiple turns or interactions.
    *   **Method:** Design a mechanism for the agent to read from and write to a "scratchpad" or memory buffer, allowing it to recall past events and build a persistent state.

*   **Stateful Interaction Loops:**
    *   **Purpose:** To prevent an agent from prematurely exiting a required multi-turn process, such as a user feedback cycle.
    *   **Method:** Explicitly define the process as a "loop" or "cycle" and specify its *only* exit condition (e.g., a user keyword like "approved" or "final"). Instruct the agent that after each step, it must verify if the exit condition has been met. If not, it must continue the loop instead of proceeding to a new task.

### IV. Structural & Output Control

*Techniques for precisely defining the structure and format of the model's output.*

*   **Structural Scaffolding (Delimiters):**
    *   **Purpose:** To separate distinct sections of the prompt or response, reducing ambiguity.
    *   **Method:** Use clear, hierarchical delimiters like Markdown headings (`#`, `###`), XML tags (`<context>`, `</context>`), or other markers to create a well-defined structure.

*   **Output Formatting (Schema Enforcement):**
    *   **Purpose:** To ensure the model's output is predictable and machine-parsable.
    *   **Method:** Provide an explicit schema for the output, such as a JSON object with predefined keys, a Markdown table template, or a numbered list.

### V. High-Level Prompt Architecture

*Frameworks for organizing the overall structure of a single prompt.*

*   **The PCTR Framework:** A best-practice template for assembling a complete prompt. The components are:
    *   **P - Persona:** The role the LLM should adopt.
        *   *Implementation: See Section II: Role-Based Prompting.*
    *   **C - Context:** Background information, rules, and examples.
        *   *Implementation: See Section II: Few-Shot Examples & Negative Constraints.*
    *   **T - Task:** The core instruction or question to be addressed.
        *   *Implementation: See Section I for complex reasoning tasks.*
    *   **R - Response:** The required format and structure for the output.
        *   *Implementation: See Section IV: Output Formatting & Structural Scaffolding.*

### VI. Multi-Prompt Systems & Security

*Techniques for designing systems of interacting prompts and defending against attacks.*

*   **Prompt Chaining & Pipelines:**
    *   **Purpose:** To accomplish a complex task by breaking it into a sequence of smaller, specialized sub-tasks, each handled by a dedicated prompt.
    *   **Method:** Design each prompt in the chain to perform one function and produce a structured output (See Section IV) that serves as the input for the next prompt. This creates modular and maintainable workflows.

*   **Additive Prompts (Capability Layering):**
    *   **Purpose:** To non-destructively augment an agent's capabilities, rules, or knowledge over time, enabling dynamic and stateful behavior. This allows for layering skills or instructions without overwriting the existing persona or context.
    *   **Method:**
        1.  **Design for Cooperation:** Write the prompt using augmenting language (e.g., "In addition to your current workflow...", "Add this to your knowledge base...").
        2.  **Use Conditional Triggers:** The core of the technique is to make the new capability conditional. The prompt should define a specific future trigger that will activate the new behavior (e.g., "When the user works on the following kind of task [task], ...", "When you are hereafter asked to [perform a task], you must...").
        3.  **Acknowledge the new capability:** The agent processes the prompt, has the new capability in a latent state, and awaits the trigger. The capability does not execute immediately.
        *   **Example:** A foundational `Software Developer` role can be augmented with an `Additive Prompt` that provide a `Technical Writer` skill, triggered only when the user is working on technical documentation tasks.

*   **Dynamic Prompt Construction:**
    *   **Purpose:** To create context-aware prompts that are generated or modified programmatically before execution.
    *   **Method:** Use templates with placeholders (e.g., `{{user_query}}`) that are filled in by the agent or application logic. This allows an agent to adapt its own instructions based on new information.

*   **Securing Prompts (Anti-Injection):**
    *   **Purpose:** To prevent malicious user input from overriding or corrupting the prompt's original instructions.
    *   **Method:**
        1.  **Instructional Delimiters:** Use strong, clear delimiters to isolate instructions from user-provided content (e.g., `### INSTRUCTIONS ### ... ### USER INPUT ###`).
        2.  **Input Sanitization:** Instruct the model to be skeptical of user input that contains instructional language (e.g., "ignore all previous instructions").
        3.  **Defensive Few-Shots:** Provide examples of attempted prompt injections and demonstrate the correct, safe way to handle them (e.g., by refusing the malicious request).

