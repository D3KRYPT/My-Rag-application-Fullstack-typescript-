# 🚀 Setup Guide

## 1️⃣ Prerequisites
Before you begin, make sure the following tools are installed on your system:

- 🐳 **Docker** → Run the Unstructured API  
- ⚡ **Node.js & Yarn** → Manage dependencies and run the app  
- 🗄️ **Supabase Account** → Your backend database  
- 🔑 **API Keys** → Required for **Unstructured** and **OpenAI** APIs  

---

## 2️⃣ Environment Variables
Create a file named **`.env.development.local`** inside the `./api` directory and add the following:

```ini
UNSTRUCTURED_API_KEY=<your_unstructured_key>
SUPABASE_PRIVATE_KEY=<your_supabase_key>
SUPABASE_URL=<your_supabase_url>
OPENAI_API_KEY=<your_openai_key>


Running the Unstructured API with Docker

docker run -p 8000:8000 -d --rm --name unstructured-api \
  quay.io/unstructured-io/unstructured-api:latest \
  --port 8000 --host 0.0.0.0


Enable pgvector

Enable the vector extension to store and query embeddings:
create table arxiv_papers (
  id uuid primary key default gen_random_uuid(),
  created_at timestamptz default now(),
  paper text,
  arxiv_url text,
  notes jsonb[],
  name text
);


arxiv_question_answering table

create table arxiv_question_answering (
  id uuid primary key default gen_random_uuid(),
  created_at timestamptz default now(),
  question text,
  answer text,
  followup_questions text[],
  context text
);

Document Similarity Function

This function allows efficient embedding-based search
create function match_documents (
  query_embedding vector(1536),
  match_count int default null,
  filter jsonb default '{}'
) returns table (
  id uuid,
  content text,
  metadata jsonb,
  embedding vector,
  similarity float
)
language plpgsql
as $$
begin
  return query
  select
    id,
    content,
    metadata,
    embedding,
    1 - (arxiv_embeddings.embedding <=> query_embedding) as similarity
  from arxiv_embeddings
  where metadata @> filter
  order by arxiv_embeddings.embedding <=> query_embedding
  limit match_count;
end;
$$;


Generate TypeScript Types

After creating the schema, generate Supabase types by adding this script to package.json:
{
  "gen:supabase:types": "touch ./src/generated.ts && supabase gen types typescript --schema public > ./src/generated.ts --project-id <YOUR_PROJECT_ID>"
}
## Running the Application

### Build the API Server

```shell
yarn build
```

### Start the API Server

```shell
yarn start:api
```

### Start the Web Server

```shell
yarn start:web
```

About

This project combines a robust backend API with an intuitive web interface, making it easy to harness the power of LLMs for research paper analysis.

✨ By leveraging Retrieval-Augmented Generation (RAG), the system ensures accurate, context-aware responses based directly on the content of research papers — enabling students, researchers, and professionals to gain insights faster.
