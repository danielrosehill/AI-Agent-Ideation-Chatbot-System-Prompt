# Integration Guide

This guide explains how to integrate the AI Assistant Idea Generator system prompts with various frameworks and platforms.

## LangChain Integration

### Basic Integration

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

### Advanced Integration with Conversation Memory

```python
from langchain.prompts import PromptTemplate
from langchain.llms import OpenAI
from langchain.chains import ConversationChain
from langchain.memory import ConversationBufferMemory

# Load the system prompt variant
with open("system-prompts/variants/collaborative_ideation.md", "r") as file:
    system_prompt = file.read()

# Create a template with conversation history
template = f"""
{system_prompt}

Current conversation:
{{history}}
Human: {{input}}
AI: 
"""

prompt = PromptTemplate(
    input_variables=["history", "input"],
    template=template
)

# Initialize with conversation memory
memory = ConversationBufferMemory()
llm = OpenAI(temperature=0.7)
conversation = ConversationChain(
    llm=llm, 
    prompt=prompt,
    memory=memory,
    verbose=True
)

# Interactive session
response = conversation.predict(input="I want to create an AI assistant for personal finance")
print(response)
```

## LlamaIndex Integration

```python
from llama_index import PromptTemplate, LLMPredictor, ServiceContext
from llama_index.llms import OpenAI

# Load the system prompt variant
with open("system-prompts/variants/problem_solver.md", "r") as file:
    system_prompt = file.read()

# Create template
template = f"""
{system_prompt}

PROBLEM_AREA: {{problem_area}}
NUMBER_OF_SUGGESTIONS: {{num_suggestions}}

"""

prompt_template = PromptTemplate(template)

# Set up LLM with the prompt
llm = OpenAI(temperature=0.8)
llm_predictor = LLMPredictor(llm=llm)
service_context = ServiceContext.from_defaults(llm_predictor=llm_predictor)

# Generate response
response = llm.complete(
    prompt_template.format(problem_area="remote work challenges", num_suggestions=3)
)
print(response)
```

## ChatGPT / OpenAI API Integration

```python
import openai

# Load the system prompt variant
with open("system-prompts/variants/user_persona.md", "r") as file:
    system_prompt = file.read()

# Set up the conversation
response = openai.ChatCompletion.create(
    model="gpt-4",
    temperature=0.8,
    messages=[
        {"role": "system", "content": system_prompt},
        {"role": "user", "content": "I'd like to explore AI assistants for creative professionals like graphic designers and video editors."}
    ]
)

print(response.choices[0].message.content)
```

## Claude / Anthropic API Integration

```python
import anthropic

# Load the system prompt variant
with open("system-prompts/variants/future_tech.md", "r") as file:
    system_prompt = file.read()

# Initialize client
client = anthropic.Anthropic(api_key="your_api_key")

# Create a message
message = client.messages.create(
    model="claude-3-opus-20240229",
    max_tokens=1000,
    temperature=0.8,
    system=system_prompt,
    messages=[
        {"role": "user", "content": "What AI assistants might be possible in 5-10 years for education?"}
    ]
)

print(message.content)
```

## Streamlit Web App Integration

```python
import streamlit as st
import openai
import os

# Load all available prompt variants
prompt_variants = {}
variants_dir = "system-prompts/variants"
for filename in os.listdir(variants_dir):
    if filename.endswith(".md"):
        variant_name = filename.replace(".md", "")
        with open(os.path.join(variants_dir, filename), "r") as file:
            prompt_variants[variant_name] = file.read()

# Streamlit UI
st.title("AI Assistant Idea Generator")

# Variant selection
selected_variant = st.selectbox(
    "Select Ideation Approach",
    list(prompt_variants.keys()),
    format_func=lambda x: x.replace("_", " ").title()
)

# User input based on variant
if selected_variant == "domain_expert":
    domain = st.text_input("Domain", "Healthcare")
    expertise_level = st.selectbox("Expertise Level", ["Beginner", "Professional", "Expert"])
    num_suggestions = st.slider("Number of Suggestions", 1, 10, 3)
    user_input = f"Generate {num_suggestions} AI assistant ideas for {expertise_level.lower()}s in the {domain} domain."
elif selected_variant == "problem_solver":
    problem = st.text_area("Describe your problem", "I'm struggling with...")
    num_suggestions = st.slider("Number of Suggestions", 1, 10, 3)
    user_input = f"I have this problem: {problem}. Can you suggest {num_suggestions} AI assistants to help?"
# Add conditions for other variants...
else:
    user_input = st.text_area("Your Request", "Generate some AI assistant ideas for...")

# Generate button
if st.button("Generate Ideas"):
    with st.spinner("Generating AI assistant ideas..."):
        response = openai.ChatCompletion.create(
            model="gpt-4",
            temperature=0.8,
            messages=[
                {"role": "system", "content": prompt_variants[selected_variant]},
                {"role": "user", "content": user_input}
            ]
        )
        
        st.markdown(response.choices[0].message.content)
```

## Batch Processing with Different Variants

```python
import openai
import json
import os

# Load all prompt variants
prompt_variants = {}
variants_dir = "system-prompts/variants"
for filename in os.listdir(variants_dir):
    if filename.endswith(".md"):
        variant_name = filename.replace(".md", "")
        with open(os.path.join(variants_dir, filename), "r") as file:
            prompt_variants[variant_name] = file.read()

# Topics to explore
topics = [
    "healthcare", 
    "education", 
    "personal productivity", 
    "creative work", 
    "finance"
]

# Generate ideas using each variant for each topic
results = {}

for topic in topics:
    results[topic] = {}
    
    for variant_name, system_prompt in prompt_variants.items():
        print(f"Generating ideas for {topic} using {variant_name}...")
        
        response = openai.ChatCompletion.create(
            model="gpt-4",
            temperature=0.8,
            messages=[
                {"role": "system", "content": system_prompt},
                {"role": "user", "content": f"Generate 3 AI assistant ideas related to {topic}."}
            ]
        )
        
        results[topic][variant_name] = response.choices[0].message.content

# Save results
with open("batch_results.json", "w") as f:
    json.dump(results, f, indent=2)
```

These integration examples demonstrate how to use the AI Assistant Idea Generator system prompts with various frameworks and platforms. You can adapt these examples to fit your specific use case and preferred technology stack.
