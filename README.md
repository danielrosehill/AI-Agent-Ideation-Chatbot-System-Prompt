# AI Agent Ideation Chatbot System Prompt

<img src="avatar.webp" alt="AI Agent Ideation Chatbot" align="left" width="300" style="margin-right: 20px; margin-bottom: 10px;" />

A specialized system prompt collection designed to create conversational chatbots for ideating AI assistant and agent concepts. This repository contains multiple prompt variants that transform a general-purpose LLM into a focused brainstorming partner for AI solution ideation.

## Overview

These system prompts guide an AI to generate creative, practical ideas for AI assistants and agents based on user requirements. They're particularly useful for:

- Brainstorming new AI assistant concepts
- Exploring potential AI solutions for specific domains
- Generating batches of AI agent ideas with varying capabilities
- Discovering innovative applications for AI technology
- Developing detailed concepts through collaborative ideation

## System Prompt Variants

This repository includes multiple variants of the AI Assistant Idea Generator, each with a unique approach to the ideation process:

1. **Original**: The classic approach with randomized ideation workflows
2. **Domain Expert**: Focuses on creating assistants tailored to specific fields or industries
3. **Problem Solver**: Takes a methodical approach focused on identifying pain points first
4. **Future Technology**: Explores forward-looking assistants that anticipate emerging technologies
5. **Collaborative Ideation**: Emphasizes iterative refinement and co-creation of assistant concepts
6. **User Persona**: Focuses on understanding end-user needs, behaviors, and contexts

For detailed information about each variant and when to use them, see [Prompt Variants Documentation](docs/prompt_variants.md).

## Repository Structure

```
.
├── README.md                 # This file
├── avatar.webp               # Project avatar image
├── system-prompts/           # System prompt files
│   ├── v1.md                 # Original system prompt
│   ├── templates/            # Base templates for creating new variants
│   └── variants/             # Different variants of the system prompt
│       ├── original.md       # Copy of the original prompt
│       ├── domain_expert.md  # Domain-specific approach
│       ├── problem_solver.md # Problem-first approach
│       ├── future_tech.md    # Forward-looking approach
│       ├── collaborative_ideation.md # Interactive co-creation approach
│       └── user_persona.md   # User-centered design approach
├── examples/                 # Example conversations using each variant
├── docs/                     # Documentation
│   ├── prompt_variants.md    # Detailed explanation of each variant
│   └── integration_guide.md  # Guide for integrating with different frameworks
```

## Usage with LangChain and Prompt Templates

While the system prompts work excellently in conversational interfaces, they can also be adapted for programmatic use with LangChain and prompt templating. Here's a basic example:

```python
from langchain.prompts import PromptTemplate
from langchain.llms import OpenAI
from langchain.chains import LLMChain

# Load the system prompt variant of your choice
with open("system-prompts/variants/domain_expert.md", "r") as file:
    system_prompt = file.read()

# Create a template with variables
template = f"""
{system_prompt}

DOMAIN: {{domain}}
EXPERTISE_LEVEL: {{expertise_level}}
NUMBER_OF_SUGGESTIONS: {{num_suggestions}}
"""

prompt = PromptTemplate(
    input_variables=["domain", "expertise_level", "num_suggestions"],
    template=template
)

# Initialize the LLM and chain
llm = OpenAI(temperature=0.8)  # Higher temperature for creativity
chain = LLMChain(llm=llm, prompt=prompt)

# Generate ideas
response = chain.run(domain="healthcare", expertise_level="professional", num_suggestions=5)
print(response)
```

For more integration examples with different frameworks and platforms, see the [Integration Guide](docs/integration_guide.md).

## Example Use Cases

The `/examples` directory contains sample conversations for each variant:

- [Domain Expert Example](examples/domain_expert_example.md): Healthcare AI assistants for clinical settings
- [Problem Solver Example](examples/problem_solver_example.md): Addressing remote work challenges
- [Future Technology Example](examples/future_tech_example.md): Next-generation education assistants
- [Collaborative Ideation Example](examples/collaborative_ideation_example.md): Developing a personal finance assistant
- [User Persona Example](examples/user_persona_example.md): Assistants for creative professionals

## Contributing

Feel free to fork this repository and adapt the system prompts for your specific needs. If you develop interesting variations or improvements, pull requests are welcome!

## License

This project is open source and available for use and modification.

---

*Last updated: March 26th, 2025*
