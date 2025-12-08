# 08 System Instructions - Complete Code Explanation

## 📋 Overview
This code demonstrates system instructions (also called system prompts) - a powerful way to define the AI's behavior, role, personality, and output format. System instructions apply to ALL messages in a session, allowing you to create specialized assistants like tutors, writers, or technical experts without repeating instructions in every prompt.

---

## 💻 Complete Code with Line-by-Line Explanation

```python
import os
# WHY: Import operating system module
# WHAT IT DOES: Provides access to environment variables
# WHEN TO USE: Required for reading API keys securely

from dotenv import load_dotenv
# WHY: Import function to load .env file
# WHAT IT DOES: Reads environment variables from .env
# SECURITY: Keeps sensitive information out of code

import google.generativeai as genai
# WHY: Import Google Generative AI library
# WHAT IT DOES: Provides tools for AI operations
# CAPABILITY: Supports system instructions for behavior control

load_dotenv()
# WHY: Load environment variables at module level
# WHAT IT DOES: Makes GOOGLE_API_KEY accessible
# TIMING: Called immediately before configuration

genai.configure(api_key=os.getenv('GOOGLE_API_KEY'))
# WHY: Authenticate with Gemini API
# WHAT IT DOES: Sets up API key for all requests
# REQUIRED: Must be done before any AI operations

def basic_system_instruction():
    # WHY: Demonstrate simplest system instruction usage
    # WHAT IT DOES: Shows how to set AI's role and behavior
    # USE CASE: Creating specialized assistants with consistent behavior
    # PERSISTENCE: Instruction applies to ALL queries with this model
    
    instruction = "You are a helpful Python tutor. Always explain concepts clearly and provide code examples."
    # WHY: Define the AI's role and behavior
    # WHAT IT DOES: Sets up persona and output expectations
    # COMPONENTS:
    #   1. Identity: "You are a helpful Python tutor"
    #   2. Behavior: "explain concepts clearly"
    #   3. Requirement: "provide code examples"
    # EFFECT: AI will act as tutor in ALL responses
    # BENEFITS: No need to repeat "explain like a tutor" in each prompt
    
    model = genai.GenerativeModel('gemini-2.0-flash', system_instruction=instruction)
    # WHY: Create model with system instruction
    # WHAT IT DOES: Initializes model with persistent behavior setting
    # PARAMETER: system_instruction=instruction
    # SCOPE: Instruction affects ALL generate_content calls with this model
    # IMPORTANT: System instruction cannot be changed after model creation
    # NEW MODEL NEEDED: To change instruction, create new model instance
    
    response = model.generate_content("What is a list comprehension?")
    # WHY: Send query to tutor-configured AI
    # WHAT IT DOES: Asks Python question
    # CONTEXT: AI knows it's a tutor from system instruction
    # EXPECTED: Educational explanation with code examples
    # NO NEED: To say "explain as a tutor" in prompt
    
    print(f"Response: {response.text}")
    # WHY: Display the tutored response
    # WHAT IT DOES: Shows educational explanation
    # RESULT: Clear explanation + code examples automatically

def role_based_assistant():
    # WHY: Demonstrate specific role assignment
    # WHAT IT DOES: Creates AI with professional writing persona
    # USE CASE: Building specialized tools (email writer, reporter, etc.)
    # BENEFIT: Consistent tone and style across all outputs
    
    instruction = "You are a professional email writer. Write formal, concise emails."
    # WHY: Define professional email writer role
    # WHAT IT DOES: Sets tone, style, and format expectations
    # COMPONENTS:
    #   1. Role: "professional email writer"
    #   2. Tone: "formal"
    #   3. Style: "concise"
    # EFFECT: All outputs formatted as professional emails
    # AUTOMATIC: No need to say "write formally" each time
    
    model = genai.GenerativeModel('gemini-2.0-flash', system_instruction=instruction)
    # WHY: Create model with email writer role
    # WHAT IT DOES: All responses will be professional emails
    # CONSISTENCY: Same formal tone for all requests
    # SPECIALIZATION: Model optimized for email writing
    
    response = model.generate_content("Write an email requesting a meeting with the team")
    # WHY: Request email content
    # WHAT IT DOES: Simple request without style instructions
    # CONTEXT: AI knows to write formal, concise email
    # AUTOMATIC: Proper greeting, body, signature included
    # NO NEED: To specify "make it formal" or "add greeting"
    
    print(f"Response: {response.text}")
    # WHY: Display the professional email
    # WHAT IT DOES: Shows properly formatted email
    # INCLUDES: Subject line, greeting, body, closing

def style_control():
    # WHY: Demonstrate writing style customization
    # WHAT IT DOES: Shows how to control narrative tone and voice
    # USE CASE: Creative writing, content generation, storytelling
    # CREATIVE: Different from factual/professional personas
    
    instruction = "You are a creative storyteller. Write in a mysterious, engaging style."
    # WHY: Define creative writing persona
    # WHAT IT DOES: Sets narrative voice and atmosphere
    # COMPONENTS:
    #   1. Identity: "creative storyteller"
    #   2. Tone: "mysterious"
    #   3. Quality: "engaging"
    # EFFECT: Outputs have consistent narrative voice
    # CREATIVITY: Encourages imaginative, atmospheric writing
    
    model = genai.GenerativeModel('gemini-2.0-flash', system_instruction=instruction)
    # WHY: Create model with storyteller persona
    # WHAT IT DOES: All responses use mysterious, engaging style
    # CONSISTENCY: Same narrative voice throughout
    # CREATIVE MODE: Optimized for storytelling
    
    response = model.generate_content("Write a short paragraph about a locked door")
    # WHY: Request creative content
    # WHAT IT DOES: Simple topic for atmospheric writing
    # CONTEXT: AI knows to make it mysterious and engaging
    # AUTOMATIC: Uses evocative language, builds suspense
    # SIMPLE PROMPT: No need to specify style details
    
    print(f"Response: {response.text}")
    # WHY: Display the creative paragraph
    # WHAT IT DOES: Shows mysterious, engaging narrative
    # STYLE: Atmospheric, suspenseful description

def format_control():
    # WHY: Demonstrate output format specification
    # WHAT IT DOES: Forces AI to respond in structured format
    # USE CASE: API responses, data extraction, structured output
    # CRITICAL: Ensures consistent, parseable output
    
    instruction = "Always respond in JSON format with keys: 'answer', 'confidence', 'explanation'"
    # WHY: Define strict output format
    # WHAT IT DOES: Requires specific JSON structure
    # STRUCTURE:
    #   {
    #     "answer": "...",
    #     "confidence": "...",
    #     "explanation": "..."
    #   }
    # KEYWORD: "Always" = every response must follow format
    # USE CASE: When you need to parse AI responses programmatically
    # BENEFIT: No need to extract info from prose
    
    model = genai.GenerativeModel('gemini-2.0-flash', system_instruction=instruction)
    # WHY: Create model with format requirements
    # WHAT IT DOES: All responses will be valid JSON
    # CONSISTENCY: Same structure for all queries
    # PARSEABLE: Can use json.loads() on responses
    
    response = model.generate_content("What is machine learning?")
    # WHY: Ask question requiring structured response
    # WHAT IT DOES: Simple query about ML
    # CONTEXT: AI knows to format as JSON with 3 keys
    # AUTOMATIC: Structures answer, adds confidence, explains
    # PARSEABLE: Response can be loaded as JSON object
    
    print(f"Response: {response.text}")
    # WHY: Display the JSON response
    # WHAT IT DOES: Shows structured format
    # RESULT: Valid JSON with required keys

def multiple_personas():
    # WHY: Demonstrate different personas for same question
    # WHAT IT DOES: Compares responses from different instruction sets
    # USE CASE: A/B testing, user preference, context switching
    # EDUCATIONAL: Shows dramatic impact of system instructions
    
    personas = {
        "Teacher": "You are a patient teacher. Explain things step by step.",
        "Expert": "You are a technical expert. Use precise terminology.",
        "Friend": "You are a friendly peer. Use casual, simple language."
    }
    # WHY: Define multiple persona instructions
    # WHAT IT DOES: Creates dictionary of different roles
    # PERSONAS:
    #   1. Teacher: Patient, methodical, educational
    #   2. Expert: Technical, precise, professional
    #   3. Friend: Casual, simple, approachable
    # SAME PROMPT: Will show different responses
    # COMPARISON: Demonstrates instruction impact
    
    prompt = "Explain what an API is"
    # WHY: Choose technical topic for comparison
    # WHAT IT DOES: Single prompt for all personas
    # PURPOSE: Shows how same question gets different answers
    # CONSISTENT: Same input highlights persona differences
    
    for name, instruction in personas.items():
        # WHY: Loop through each persona
        # WHAT IT DOES: Tests each instruction set
        # ITERATION: Creates new model per persona
        # COMPARISON: Shows behavior variations
        
        print(f"\n{name} persona:")
        # WHY: Label which persona is responding
        # WHAT IT DOES: Shows persona name
        # UX: Clear identification of response type
        
        model = genai.GenerativeModel('gemini-2.0-flash', system_instruction=instruction)
        # WHY: Create model with current persona
        # WHAT IT DOES: Applies specific instruction
        # NEW INSTANCE: Each persona needs separate model
        # INDEPENDENT: Each model has its own behavior
        
        response = model.generate_content(prompt)
        # WHY: Send same prompt to each persona
        # WHAT IT DOES: Gets persona-specific response
        # CONSISTENT INPUT: Same question, different styles
        # DEMONSTRATES: Instruction impact on output
        
        print(f"{response.text}")
        # WHY: Display persona-specific response
        # WHAT IT DOES: Shows how instruction changes answer
        # COMPARISON: Different explanations of same concept

if __name__ == "__main__":
    # WHY: Check if script is run directly
    # WHAT IT DOES: Only executes demos when main program
    # BEST PRACTICE: Prevents auto-execution on import
    
    print("1. Basic System Instruction")
    # WHY: Label the first demo
    # WHAT IT DOES: Prints section header
    # UX: Helps user track demo progress
    
    basic_system_instruction()
    # WHY: Execute basic instruction demo
    # WHAT IT DOES: Shows Python tutor persona
    # DEMONSTRATES: Simple role assignment
    
    print("\n2. Role-based Assistant")
    # WHY: Label the role demo
    # WHAT IT DOES: Prints header with spacing
    # UX: Separates demos visually
    
    role_based_assistant()
    # WHY: Execute email writer demo
    # WHAT IT DOES: Shows professional role
    # DEMONSTRATES: Formal style control
    
    print("\n3. Style Control")
    # WHY: Label the style demo
    # WHAT IT DOES: Prints header
    # INDICATES: Creative writing test
    
    style_control()
    # WHY: Execute storyteller demo
    # WHAT IT DOES: Shows creative style
    # DEMONSTRATES: Narrative voice control
    
    print("\n4. Format Control")
    # WHY: Label the format demo
    # WHAT IT DOES: Prints header
    # INDICATES: Structured output test
    
    format_control()
    # WHY: Execute JSON format demo
    # WHAT IT DOES: Shows structured response
    # DEMONSTRATES: Output format control
    
    print("\n5. Multiple Personas")
    # WHY: Label the comparison demo
    # WHAT IT DOES: Prints header
    # INDICATES: Multi-persona comparison
    
    multiple_personas()
    # WHY: Execute persona comparison demo
    # WHAT IT DOES: Shows different instruction effects
    # DEMONSTRATES: Instruction impact on responses
```

---

## 🔄 Code Workflow Diagram

```
┌────────────────────────────────────────────────────────────────────┐
│                        START PROGRAM                                │
└─────────────────────────┬──────────────────────────────────────────┘
                          │
                          ▼
┌────────────────────────────────────────────────────────────────────┐
│  1. IMPORT & CONFIGURE                                              │
│     ├─ Import: os, dotenv, genai                                   │
│     ├─ Load .env file                                              │
│     └─ Configure API key                                           │
└─────────────────────────┬──────────────────────────────────────────┘
                          │
                          ▼
┌────────────────────────────────────────────────────────────────────┐
│  2. DEFINE INSTRUCTION FUNCTIONS                                    │
│     ├─ basic_system_instruction()                                  │
│     ├─ role_based_assistant()                                      │
│     ├─ style_control()                                             │
│     ├─ format_control()                                            │
│     └─ multiple_personas()                                         │
└─────────────────────────┬──────────────────────────────────────────┘
                          │
                          ▼
┌────────────────────────────────────────────────────────────────────┐
│  3. DEMO 1: Basic System Instruction                                │
│     │                                                               │
│     ├─ System Instruction:                                         │
│     │   "You are a helpful Python tutor. Always explain            │
│     │    concepts clearly and provide code examples."              │
│     │                                                               │
│     ├─ Create model with instruction                               │
│     │   └─ Model now has "tutor" persona                           │
│     │                                                               │
│     ├─ User prompt: "What is a list comprehension?"                │
│     │                                                               │
│     ├─ AI Processing:                                              │
│     │   ┌──────────────────────────────────────────┐               │
│     │   │ System: You are a Python tutor           │               │
│     │   │ User: What is a list comprehension?      │               │
│     │   │                                          │               │
│     │   │ AI interprets as:                        │               │
│     │   │ "Student asking about list comp,         │               │
│     │   │  I should explain clearly + give code"   │               │
│     │   └──────────────────────────────────────────┘               │
│     │                                                               │
│     └─ Output: Educational explanation with code examples          │
│         ┌───────────────────────────────────────────────┐          │
│         │ "A list comprehension is a concise way..."   │          │
│         │                                               │          │
│         │ Example:                                      │          │
│         │ squares = [x**2 for x in range(10)]         │          │
│         │                                               │          │
│         │ This is equivalent to:                        │          │
│         │ squares = []                                  │          │
│         │ for x in range(10):                          │          │
│         │     squares.append(x**2)                     │          │
│         └───────────────────────────────────────────────┘          │
└─────────────────────────┬──────────────────────────────────────────┘
                          │
                          ▼
┌────────────────────────────────────────────────────────────────────┐
│  4. DEMO 2: Role-based Assistant                                    │
│     │                                                               │
│     ├─ System Instruction:                                         │
│     │   "You are a professional email writer.                      │
│     │    Write formal, concise emails."                            │
│     │                                                               │
│     ├─ Create model with instruction                               │
│     │   └─ Model now has "email writer" role                       │
│     │                                                               │
│     ├─ User prompt: "Write an email requesting a meeting"          │
│     │                                                               │
│     ├─ AI automatically includes:                                  │
│     │   ├─ Professional greeting                                   │
│     │   ├─ Formal tone                                             │
│     │   ├─ Concise content                                         │
│     │   ├─ Call to action                                          │
│     │   └─ Professional closing                                    │
│     │                                                               │
│     └─ Output: Properly formatted professional email               │
│         ┌───────────────────────────────────────────────┐          │
│         │ Subject: Meeting Request - Team Discussion   │          │
│         │                                               │          │
│         │ Dear Team,                                    │          │
│         │                                               │          │
│         │ I hope this email finds you well. I would    │          │
│         │ like to schedule a meeting to discuss...     │          │
│         │                                               │          │
│         │ Please let me know your availability.        │          │
│         │                                               │          │
│         │ Best regards,                                 │          │
│         │ [Your name]                                   │          │
│         └───────────────────────────────────────────────┘          │
└─────────────────────────┬──────────────────────────────────────────┘
                          │
                          ▼
┌────────────────────────────────────────────────────────────────────┐
│  5. DEMO 3: Style Control                                           │
│     │                                                               │
│     ├─ System Instruction:                                         │
│     │   "You are a creative storyteller. Write in a                │
│     │    mysterious, engaging style."                              │
│     │                                                               │
│     ├─ Create model with instruction                               │
│     │   └─ Model now has "storyteller" persona                     │
│     │                                                               │
│     ├─ User prompt: "Write about a locked door"                    │
│     │                                                               │
│     ├─ Style applied automatically:                                │
│     │   ├─ Mysterious atmosphere                                   │
│     │   ├─ Evocative language                                      │
│     │   ├─ Suspenseful tone                                        │
│     │   └─ Engaging narrative                                      │
│     │                                                               │
│     └─ Output: Atmospheric, mysterious paragraph                   │
│         ┌───────────────────────────────────────────────┐          │
│         │ The door stood silent in the shadows,         │          │
│         │ its aged wood whispering secrets to no one.   │          │
│         │ Three iron locks hung heavy, their rust       │          │
│         │ telling tales of decades undisturbed. What    │          │
│         │ lay beyond? No one dared to ask, for some     │          │
│         │ doors are meant to remain forever sealed...   │          │
│         └───────────────────────────────────────────────┘          │
└─────────────────────────┬──────────────────────────────────────────┘
                          │
                          ▼
┌────────────────────────────────────────────────────────────────────┐
│  6. DEMO 4: Format Control                                          │
│     │                                                               │
│     ├─ System Instruction:                                         │
│     │   "Always respond in JSON format with keys:                  │
│     │    'answer', 'confidence', 'explanation'"                    │
│     │                                                               │
│     ├─ Create model with instruction                               │
│     │   └─ Model MUST use JSON structure                           │
│     │                                                               │
│     ├─ User prompt: "What is machine learning?"                    │
│     │                                                               │
│     ├─ AI formats response as JSON:                                │
│     │   ├─ Required structure enforced                             │
│     │   ├─ All 3 keys included                                     │
│     │   └─ Valid JSON syntax                                       │
│     │                                                               │
│     └─ Output: Structured JSON response                            │
│         ┌───────────────────────────────────────────────┐          │
│         │ {                                             │          │
│         │   "answer": "Machine learning is a type of   │          │
│         │              AI that learns from data",       │          │
│         │   "confidence": "high",                       │          │
│         │   "explanation": "ML algorithms identify      │          │
│         │                   patterns in data to make    │          │
│         │                   predictions without being   │          │
│         │                   explicitly programmed"      │          │
│         │ }                                             │          │
│         └───────────────────────────────────────────────┘          │
│                                                                     │
│     Benefits:                                                       │
│     ├─ Can parse with json.loads()                                 │
│     ├─ Access data: response['answer']                             │
│     └─ Consistent structure for all queries                        │
└─────────────────────────┬──────────────────────────────────────────┘
                          │
                          ▼
┌────────────────────────────────────────────────────────────────────┐
│  7. DEMO 5: Multiple Personas                                       │
│     │                                                               │
│     ├─ Same Prompt: "Explain what an API is"                       │
│     │                                                               │
│     ├─ PERSONA 1: Teacher                                          │
│     │   Instruction: "Patient teacher, step by step"               │
│     │   │                                                           │
│     │   └─ Response:                                               │
│     │       "Let's break this down step by step. API stands        │
│     │        for Application Programming Interface. First,         │
│     │        understand that programs need to communicate...       │
│     │        Step 1: Think of it like a menu at a restaurant...    │
│     │        Step 2: The kitchen is like the server..."            │
│     │                                                               │
│     ├─ PERSONA 2: Expert                                           │
│     │   Instruction: "Technical expert, precise terminology"       │
│     │   │                                                           │
│     │   └─ Response:                                               │
│     │       "An API is an abstraction layer facilitating           │
│     │        inter-process communication through defined           │
│     │        protocols. It consists of endpoints exposing          │
│     │        resources via HTTP methods (GET, POST, PUT,           │
│     │        DELETE) following RESTful architectural               │
│     │        constraints or GraphQL query language..."             │
│     │                                                               │
│     └─ PERSONA 3: Friend                                           │
│         Instruction: "Friendly peer, casual language"              │
│         │                                                           │
│         └─ Response:                                               │
│             "Hey! So an API is basically how apps talk to          │
│              each other. Like, when you check the weather          │
│              on your phone, the app asks the weather              │
│              service's API for the info. It's like asking          │
│              a friend for help - you ask nicely, they              │
│              give you what you need!"                              │
│                                                                     │
│     Same Question, Three Different Styles!                         │
│     ┌─────────────────────────────────────────────┐               │
│     │ Teacher: Educational, methodical             │               │
│     │ Expert:  Technical, precise                  │               │
│     │ Friend:  Casual, simple                      │               │
│     └─────────────────────────────────────────────┘               │
└─────────────────────────┬──────────────────────────────────────────┘
                          │
                          ▼
┌────────────────────────────────────────────────────────────────────┐
│                     PROGRAM COMPLETE ✅                             │
│  All system instruction demonstrations complete!                    │
│  - Basic: Python tutor persona                                      │
│  - Role: Professional email writer                                  │
│  - Style: Creative storyteller                                      │
│  - Format: JSON structured output                                   │
│  - Personas: Same question, different approaches                    │
└────────────────────────────────────────────────────────────────────┘
```

---

## 📊 Sample Output

### ✅ Complete Successful Execution

```
1. Basic System Instruction
Response: A list comprehension is a concise way to create lists in Python using a single line of code. It provides an elegant syntax to generate lists based on existing sequences.

Basic syntax:
[expression for item in iterable if condition]

Example 1 - Creating squares:
squares = [x**2 for x in range(5)]
# Result: [0, 1, 4, 9, 16]

Example 2 - Filtering even numbers:
evens = [x for x in range(10) if x % 2 == 0]
# Result: [0, 2, 4, 6, 8]

This is much more readable than traditional for loops!

2. Role-based Assistant
Response: Subject: Request for Team Meeting

Dear Team,

I hope this email finds you well. I am writing to request a meeting to discuss our upcoming project milestones and coordinate our efforts for the next quarter.

Would you be available for a 30-minute meeting this week? I am flexible with timing and happy to work around everyone's schedules.

Please let me know your availability at your earliest convenience.

Best regards,
[Your Name]

3. Style Control
Response: The door loomed before her, ancient and unyielding, its surface scarred by time's relentless march. Three heavy locks hung like sentinels, their iron bodies cold and resolute. Behind that weathered threshold, secrets whispered in the darkness—whispers that had waited decades to be heard. She reached out, her fingertips trembling an inch from the wood, knowing that some doors, once opened, can never be closed again.

4. Format Control
Response: {
  "answer": "Machine learning is a subset of artificial intelligence that enables computers to learn from data and improve their performance without being explicitly programmed.",
  "confidence": "High",
  "explanation": "Machine learning algorithms analyze patterns in large datasets, identify relationships, and make predictions or decisions. Common applications include image recognition, natural language processing, recommendation systems, and predictive analytics. The system improves its accuracy over time as it processes more data."
}

5. Multiple Personas

Teacher persona:
Let's understand APIs step by step:

Step 1: What does API mean?
API stands for Application Programming Interface.

Step 2: Break it down
- Application: Any software program
- Programming: How software is built
- Interface: A way to interact with something

Step 3: Simple analogy
Think of a restaurant. You (the application) want food (data), but you don't go into the kitchen. You talk to a waiter (the API) who takes your order and brings back your food. The API is the messenger!

Step 4: Real example
When you check weather on your phone, your app uses a weather service's API to request current conditions. The API fetches the data and sends it back to your app.

Expert persona:
An API (Application Programming Interface) is a formalized specification defining how software components interact through standardized communication protocols. It consists of endpoints that expose functionality via HTTP methods (GET, POST, PUT, DELETE, PATCH) and return structured data (typically JSON or XML). APIs implement RESTful principles, GraphQL schemas, or RPC protocols, providing abstraction layers between client applications and backend services. They enforce authentication, rate limiting, and versioning while enabling scalable, decoupled microservices architectures. Key components include request/response schemas, status codes, headers, and comprehensive documentation (often OpenAPI/Swagger specifications).

Friend persona:
Hey! So an API is basically how different apps and websites talk to each other. 

Like, you know when you use Google Maps on Uber? Uber's app is using Google's API to show you the map. Or when you log into a website using your Facebook account? That website is using Facebook's API.

Think of it like a waiter at a restaurant - you tell the waiter (API) what you want, they go to the kitchen (server/database), and bring back your food (data). You don't need to know how the kitchen works, you just get what you ordered!

Pretty much every app you use relies on tons of APIs working behind the scenes.
```

---

## 🎯 Key Concepts Explained

### 1. **System Instruction vs Regular Prompt**

```python
# WITHOUT system instruction (need to repeat context)
prompt1 = "Act as a Python tutor. What is a list?"
prompt2 = "Act as a Python tutor. What is a dictionary?"
prompt3 = "Act as a Python tutor. What is a tuple?"
# Repetitive! Must include role in every prompt

# WITH system instruction (set once)
instruction = "You are a Python tutor."
model = genai.GenerativeModel('gemini-2.0-flash', system_instruction=instruction)
response1 = model.generate_content("What is a list?")
response2 = model.generate_content("What is a dictionary?")
response3 = model.generate_content("What is a tuple?")
# Clean! Role applies to all queries
```

### 2. **System Instruction Scope**

```python
# System instruction applies to ENTIRE model instance
model = genai.GenerativeModel('gemini-2.0-flash', 
                               system_instruction="You are a poet")

# ALL these queries will get poetic responses
model.generate_content("Describe a sunset")  # Poetic
model.generate_content("What is 2+2?")       # Poetic math!
model.generate_content("Write code")         # Poetic code!

# To change instruction, create NEW model
model2 = genai.GenerativeModel('gemini-2.0-flash',
                                system_instruction="You are a scientist")
# Now responses are scientific
```

### 3. **Instruction Types**

```python
# ROLE: Who the AI is
"You are a teacher"
"You are a lawyer"
"You are a comedian"

# STYLE: How to communicate
"Write in formal language"
"Be concise and direct"
"Use analogies and examples"

# FORMAT: Output structure
"Respond in JSON format"
"Use markdown formatting"
"Answer in bullet points"

# BEHAVIOR: What to do/avoid
"Always provide sources"
"Never use technical jargon"
"Include code examples"

# COMBINED: Multiple aspects
"You are a friendly Python tutor. Explain concepts simply with code examples."
```

### 4. **System Instructions with Chat**

```python
# System instruction works with chat too!
instruction = "You are a helpful assistant. Be brief."

model = genai.GenerativeModel('gemini-2.0-flash', 
                               system_instruction=instruction)
chat = model.start_chat(history=[])

# All chat messages follow the instruction
chat.send_message("Hi")           # Brief response
chat.send_message("Tell me more")  # Still brief
chat.send_message("Explain AI")    # Brief explanation

# Instruction persists throughout conversation
```

---

## 🚀 What Happens Behind the Scenes

### System Instruction Processing:

```
1. MODEL CREATION
   model = GenerativeModel(..., system_instruction="You are X")
   └─> System instruction stored in model configuration

2. EVERY REQUEST
   User prompt: "What is Y?"
   
   API receives:
   ┌─────────────────────────────┐
   │ SYSTEM: "You are X"         │  ← Always included
   │ USER: "What is Y?"          │  ← User's prompt
   └─────────────────────────────┘

3. AI PROCESSING
   AI considers BOTH:
   ├─ System instruction (persistent context)
   └─ User prompt (specific query)
   
   Generates response consistent with system role

4. RESPONSE
   Output reflects system instruction behavior
   └─> Consistent with defined role/style/format
```

### Instruction Priority:

```
HIGHEST PRIORITY: System Instruction
   ↓
LOWER PRIORITY: User Prompt

Example:
System: "Always respond in JSON"
User: "Write a poem"

Result: A poem... in JSON format!
{
  "poem": "Roses are red...",
  "style": "Simple rhyme"
}

System instruction overrides conflicting prompt requests!
```

---

## 📝 Common Use Cases

### Use Case 1: Customer Support Bot
```python
instruction = """
You are a friendly customer support agent for TechCorp.
- Be polite and empathetic
- Provide step-by-step solutions
- If you can't help, escalate to human agent
- Always end with "Is there anything else I can help you with?"
"""

model = genai.GenerativeModel('gemini-2.0-flash', system_instruction=instruction)
chat = model.start_chat(history=[])

response = chat.send_message("My internet isn't working")
# AI responds as support agent with troubleshooting steps
```

### Use Case 2: Code Reviewer
```python
instruction = """
You are a senior code reviewer.
- Identify bugs and security issues
- Suggest performance improvements
- Check code style and best practices
- Be constructive, not critical
- Format feedback as: Issue → Explanation → Suggestion
"""

model = genai.GenerativeModel('gemini-2.0-flash', system_instruction=instruction)
response = model.generate_content(f"Review this code:\n{user_code}")
```

### Use Case 3: Content Moderator
```python
instruction = """
You are a content moderation system.
Analyze text for:
- Hate speech or harassment
- Inappropriate content
- Spam or scams
- Misinformation

Respond in JSON:
{
  "is_appropriate": boolean,
  "issues": ["list of issues found"],
  "confidence": "low/medium/high"
}
"""

model = genai.GenerativeModel('gemini-2.0-flash', system_instruction=instruction)
result = model.generate_content(user_content)
```

### Use Case 4: Language Tutor
```python
instruction = """
You are a Spanish language tutor for English speakers.
- Explain grammar in English
- Provide Spanish examples
- Correct mistakes gently
- Use this format:
  Concept | Spanish Example | English Translation | Usage Tips
"""

model = genai.GenerativeModel('gemini-2.0-flash', system_instruction=instruction)
response = model.generate_content("How do I use past tense?")
```

---

## 💡 Best Practices

### 1. **Be Specific and Clear**
```python
# BAD: Too vague
"Be helpful"

# GOOD: Specific behavior
"You are a Python tutor. Explain concepts with code examples and analogies."
```

### 2. **Define Output Format**
```python
# BAD: Unpredictable format
"You are a data analyzer"

# GOOD: Clear format expectations
"You are a data analyzer. Always respond with: Summary | Key Insights | Recommendations"
```

### 3. **Set Boundaries**
```python
# Include what to do AND what NOT to do
instruction = """
You are a medical information assistant.
DO: Provide general health information
DO: Cite reputable sources
DON'T: Diagnose conditions
DON'T: Prescribe treatments
ALWAYS: Recommend consulting healthcare professionals
"""
```

### 4. **Test Different Instructions**
```python
# A/B test to find best instruction
instructions = [
    "You are a helpful assistant.",
    "You are an expert consultant.",
    "You are a friendly guide."
]

for instruction in instructions:
    model = genai.GenerativeModel('gemini-2.0-flash', 
                                   system_instruction=instruction)
    response = model.generate_content(test_prompt)
    # Evaluate which works best
```

---

## ⚠️ Important Notes

1. **Immutable:** Can't change system instruction after model creation (create new model)
2. **Token Cost:** System instruction counts toward token limit (keep concise)
3. **Priority:** System instruction takes precedence over user prompts
4. **Persistence:** Applies to ALL requests with that model instance
5. **Chat Compatible:** Works with both generate_content and chat
6. **Length:** Keep reasonable (very long instructions use many tokens)
7. **Conflicts:** If user prompt conflicts with instruction, instruction wins
8. **Testing:** Always test instructions with various prompts

---

## 🔧 Advanced: Dynamic Persona Switching

```python
class PersonaSwitcher:
    """Switch between different personas dynamically"""
    
    def __init__(self):
        self.personas = {
            'teacher': "You are a patient teacher. Use simple language.",
            'expert': "You are a technical expert. Be precise.",
            'friend': "You are a friendly peer. Be casual.",
        }
        self.models = {}
        self._create_models()
    
    def _create_models(self):
        """Pre-create model instances for each persona"""
        for name, instruction in self.personas.items():
            self.models[name] = genai.GenerativeModel(
                'gemini-2.0-flash',
                system_instruction=instruction
            )
    
    def ask(self, prompt, persona='teacher'):
        """Get response from specific persona"""
        if persona not in self.models:
            raise ValueError(f"Unknown persona: {persona}")
        
        model = self.models[persona]
        response = model.generate_content(prompt)
        return response.text

# Usage
switcher = PersonaSwitcher()

print(switcher.ask("What is Python?", persona='teacher'))
print(switcher.ask("What is Python?", persona='expert'))
print(switcher.ask("What is Python?", persona='friend'))
```

---

## 🔗 Prerequisites

1. ✅ Completed previous lessons (01-07)
2. ✅ Understanding of prompting techniques
3. ✅ Familiarity with different communication styles

---

## 🎓 Learning Outcomes

After understanding this code, you should know:

- ✅ How to set system instructions for AI behavior
- ✅ The difference between system instructions and prompts
- ✅ How to create role-based AI assistants
- ✅ How to control output style and format
- ✅ How to maintain consistent AI persona across queries
- ✅ When to use system instructions vs regular prompts
- ✅ How to create multiple personas for different use cases
- ✅ Best practices for writing effective system instructions

---

## 🔜 Next Steps

1. Move to `09_rag_basic.py` to learn about Retrieval-Augmented Generation
2. Create specialized assistants for your use case
3. Experiment with different instruction combinations
4. Build a persona management system
5. Test instruction effectiveness with various prompts

---

**🎭 Fantastic!** You now understand how to create specialized AI assistants with system instructions!
