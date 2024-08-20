# Entity-Based Conversational Memory - Event Planner Scenario

## Overview

This project demonstrates the implementation of entity-based conversational memory using the `langchain` framework with the OpenAI GPT-3.5 model. The main goal is to create a dynamic conversation system that can remember details about entities (such as people and their preferences) across interactions, allowing for more personalized and context-aware responses.

## Project Structure

- **Environment Variables:**
  - The project uses a `.env` file to securely store the API key for OpenAI's GPT-3.5 model.
  
- **Key Components:**
  - **ChatOpenAI:** This is the core model used for generating responses. It is initialized with an API key and model name.
  - **ConversationEntityMemory:** This memory structure is designed to keep track of entities and their associated attributes throughout the conversation.
  - **ConversationChain:** This chain is responsible for managing the flow of conversation, integrating the LLM with the entity-based memory.

## Setup Instructions

### 1. Prerequisites
Ensure you have Python installed, along with the necessary libraries:
- `langchain`
- `langchain_openai`
- `dotenv`
- `os`

You can install the required Python packages using `pip`:

```bash
pip install langchain langchain_openai python-dotenv
```

### 2. Creating the `.env` File
This project requires a `.env` file to securely store the OpenAI API key.

1. Create a file named `.env` in the root directory of the project.
2. Inside the `.env` file, add your OpenAI API key as follows:

```bash
OPENAI_API_KEY=your_openai_api_key_here
```

Replace `your_openai_api_key_here` with your actual API key from OpenAI.

### 3. Running the Project
To start the conversation flow, simply run the Python script. The conversation will prompt you to input your name, your likes and dislikes, and details about other people who will be involved in an activity.

The final output will be a suggested activity that takes into account the preferences of all the mentioned entities.

### Example Usage
```python
# Import necessary modules
from dotenv import load_dotenv
import os
from langchain_openai import ChatOpenAI
from langchain.chains import ConversationChain
from langchain.memory import ConversationEntityMemory
from langchain.memory.prompt import ENTITY_MEMORY_CONVERSATION_TEMPLATE

# Load environment variables
load_dotenv()

# Initialize the model and memory
llm = ChatOpenAI(openai_api_key=os.getenv("OPENAI_API_KEY"), model_name="gpt-3.5-turbo", temperature=0.0)
memory = ConversationEntityMemory(llm=llm)
conversation = ConversationChain(llm=llm, memory=memory, prompt=ENTITY_MEMORY_CONVERSATION_TEMPLATE)

# Start the conversation
name = input("What is your name?")
description = input("Please describe your likes and dislikes.")
query = f"My name is {name} and I will be organizing an activity and attending. {description}"
conversation.invoke(input=query)

# Additional attendees
n_people = int(input("How many other people do you want to plan the activity for?"))
for i in range(n_people):
    name = input("What is this attendee's name?")
    description = input(f"Please describe {name}'s likes and dislikes.")
    query = f"{name} will be attending. {description}"
    conversation.run(input=query)

# Activity suggestion
query = "What would be a good activity to plan for all the people mentioned?"
result = conversation.predict(input=query)
print(result)
```

## Possible Improvements

- **User Interface:** Develop a user-friendly interface, possibly using a web framework like Flask or Django, to interact with the system more intuitively.
- **Expand Memory Types:** Integrate additional memory types (e.g., summary-based memory) to further enhance conversation context and long-term recall.
- **Activity Matching:** Implement a more sophisticated matching algorithm that could suggest more personalized activities based on a larger dataset of possible options.
- **Error Handling:** Improve error handling and provide more informative responses in cases where the input query might lead to ambiguous or overly large results.

## License

This project is licensed under the MIT License.
