# Support Ticket Triage & Resolution Agent

**Pattern:** Retrieval-Augmented Generation (RAG) · **Stack:** n8n, pgvector on Neon PostgreSQL, OpenAI · **Status:** Complete (tested with sample tickets and policy documents)

Reads incoming requests, classifies them by type and urgency, answers routine ones from the business's own policy documents, and sends anything complex, sensitive, or uncertain to a person.

**Property management application:** the foundation of maintenance request triage (sorting by urgency, escalating emergencies) and a tenant FAQ assistant (answering from lease terms and house rules).

---

## The problem

Staff read and answer every incoming request by hand, including questions the business has already answered in a policy document a hundred times. That triage work takes time away from the requests that actually need judgment.

## How it works

Incoming ticket → triage agent classifies type and urgency → if policy-answerable: policy search sub-workflow retrieves relevant passages → agent drafts a grounded response; otherwise → route to a person

- **Knowledge base:** policy documents are stored as embeddings in pgvector on Neon PostgreSQL, searchable by meaning rather than exact keywords.
- **Triage:** an OpenAI-powered agent classifies each ticket's type and urgency.
- **Retrieval:** for policy-answerable tickets, a sub-workflow searches the document store and returns the relevant passages.
- **Grounded response:** the agent drafts its answer from those passages, not from the model's general knowledge.

## Workflow structure

- **Main workflow:** Request Triage Agent
- **Sub-workflow 1:** Policy Document Search
- **Sub-workflow 2:** Seed Knowledge Base

## Reliability

| Rung | How it shows up |
|---|---|
| Output validation | Responses are grounded in retrieved policy text, preventing confidently wrong answers |
| Human fallback | Anything the agent isn't confident is policy-answerable goes to a person |

## Repository contents

- n8n workflow exports (JSON): main workflow and policy search sub-workflow
- Workflow screenshots

## Running it yourself

1. Import both workflow JSON files into n8n and link the sub-workflow in the main workflow.
2. Create n8n credentials for PostgreSQL (Neon, with the pgvector extension enabled) and OpenAI. Credentials are **not** included in the exports.
3. Embed your policy documents into the vector store.
4. Send test tickets, including some that should be escalated, to confirm routing.

## Limitations

- Answer quality depends on the completeness of the policy documents.
- Urgency classification is LLM-based; clearly defined urgency rules in the prompt improve consistency.

---

Built by [Alex Idachaba](https://alexidachaba.com) — AI automation for property management operations.
