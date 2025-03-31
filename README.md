# Langflow-RAG-Project
Langflow Cloud URL: https://astra.datastax.com/langflow/fc549d79-7d60-41d0-babd-676114e512d5/flow/adb88fe5-899c-4da1-97c6-6d8fd7ca40fc
# Demo Video Link



# Pipeline Overview: Two Parallel Flows
1. Ingestion Flow (Executed Once)
2. Query + Retrieval + Generation Flow
   

# Ingestion Flow (Executed Once)
Components and Connections:
File Upload Node
Loads the file combined_knowledge_base.txt (includes chat logs + company policy). → connected to Split Text

Split Text Node
Breaks the text into smaller chunks using a chunk size of 1000 characters.
→ connected to OpenAI Embeddings

OpenAI Embeddings Node
Generates vector representations using OpenAI's text-embedding-3-small model.
→ connected to Astra DB (Ingest)

Astra DB Node (Vector Store)
Stores the embedded vector chunks into a specific collection (chatlogs_companyfaq) inside the database (genai_pipeline) in AstraDB.



# Query + Retrieval + Generation Flow
Components and Connections:
Chat Input Node
Accepts a user's natural language question from the Playground.
→ connected to OpenAI Embeddings (Query)

OpenAI Embeddings Node
Embeds the query to generate a vector.
→ connected to Astra DB (Search)

Astra DB Node (Vector Search)
Performs a similarity search using the query embedding and returns the most relevant chunks from the previously ingested data.
→ connected to Parse DataFrame

Parse DataFrame Node
Extracts just the {text} field from the returned vector search results.
→ connected to Prompt Template

Prompt Node
Constructs a complete prompt for GPT using:

context: the retrieved document chunks

question: the original user input
→ connected to OpenAI Chat

OpenAI Chat Node
Uses the GPT model (gpt-4 or gpt-3.5) to generate a contextual, human-readable answer based on the prompt.
→ connected to Chat Output

Chat Output Node
Displays the final answer inside Langflow’s Playground.

# Sample User Questions for Playground Testing

# From the Chat Logs Section

1. “How do I reset my password?”
2. “What should I do if I see error code 403?”
3. “Is there a mobile app available?”
4. “Can I update my email address?”

# From the Company Policy/FAQ Section

1. “What is the company’s work from home policy?”
2. “How long is parental leave?”
3. “When do health benefits start?”
4. “How many vacation days do employees get?”
5. “Who should I contact for IT support?”



