# Customer Support Ticket Triage and Resolution Agent

**Pattern:** Retrieval-Augmented Generation (RAG)

A support ticket agent that answers policy-based questions automatically from the business's own documentation, and routes anything more complex to a human.

## Problem

Support teams manually triage every incoming ticket, including the ones that are really just questions the business has already answered in a policy document somewhere. That triage work eats time that should go to tickets that actually need judgment.

## Build

Policy documents are stored as embeddings in pgvector on Neon Postgres, making them searchable by meaning rather than exact keyword match. An OpenAI-powered triage agent reads each incoming ticket and classifies its type and urgency. For tickets that look policy-answerable, a sub-workflow searches the policy document store and pulls back relevant passages, which the agent uses to draft a response grounded in the business's actual documented policy rather than the model's general knowledge.

## Outcome

Routine, policy-answerable tickets get an accurate first-pass response without a human reading them first, while genuinely complex or sensitive tickets are flagged for a person immediately instead of sitting in a queue.

## Reliability Notes

The RAG grounding step is itself a reliability mechanism — it constrains responses to the business's actual documented policy instead of letting the model answer from general knowledge, which is what prevents confidently wrong answers on policy questions.

## Stack

n8n, pgvector on Neon Postgres, OpenAI

## Status

Complete.
