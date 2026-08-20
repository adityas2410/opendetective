# OpenDetective

OpenDetective is an open-source, evidence-grounded investigative environment for criminal, corporate, fraud, and OSINT investigations.

## What OpenDetective does

- Create and manage investigation cases.
- Ingest documents, messages, images, recordings, and structured data as evidence.
- Build a persistent investigation graph of entities, events, claims, and relationships.
- Ask investigative questions in a terminal workspace.
- Explore relationships, reconstruct timelines, and identify contradictions or missing evidence.
- Trace findings back to the evidence that supports them.

## Install

OpenDetective requires Python 3.11 or later, `uv`, and a reachable Neo4j instance.

Install the application:

```bash
uv tool install opendetective
```

To run Neo4j locally with Docker Compose:

```bash
docker compose up -d neo4j
```

Neo4j is then available at `bolt://localhost:7687`.

## Configure

Copy the environment template and set the values for your environment:

```bash
cp .env.example .env
```

On PowerShell:

```powershell
Copy-Item .env.example .env
```

```env
NEO4J_URI=bolt://localhost:7687
NEO4J_USERNAME=neo4j
NEO4J_PASSWORD=your-neo4j-password

OPENDETECTIVE_MODEL_PROVIDER=your-provider
OPENDETECTIVE_MODEL_NAME=your-model
OPENDETECTIVE_MODEL_API_KEY=your-api-key
```

`OPENDETECTIVE_MODEL_PROVIDER` and `OPENDETECTIVE_MODEL_NAME` select the reasoning model used by the investigation workflow. OpenDetective owns the workflow; the model is a configurable component, so choose a hosted or local provider that fits your environment. Keep `.env` private and never commit credentials or case data.

## Quick start

Start OpenDetective:

```bash
opendetective
```

Create and open a case, ingest its evidence, then ask an investigative question:

```text
OpenDetective

> /case create acme-fraud
Case "acme-fraud" created.

> /case open acme-fraud
Case "acme-fraud" opened.

> /ingest ./evidence
Processing evidence and updating the investigation graph...

> /investigate suspicious payments involving Robert Hale
```

OpenDetective returns an evidence-backed finding, the relevant people, accounts, events, or relationships, and citations to the underlying evidence. Follow the citations before treating a finding as established.

## Slash commands

| Command | Purpose | Example |
| --- | --- | --- |
| `/case create <name>` | Create a new investigation case. | `/case create acme-fraud` |
| `/case open <name>` | Open an existing case. | `/case open acme-fraud` |
| `/ingest <path>` | Add a file or directory of evidence to the open case. | `/ingest ./evidence` |
| `/investigate <question>` | Investigate an evidence-grounded question. | `/investigate who authorized the transfer?` |
| `/evidence` | List evidence available in the open case. | `/evidence` |
| `/help` | Show command help. | `/help` |
| `/exit` | Close OpenDetective. | `/exit` |

## How it works

```text
evidence → extraction → provenance-aware knowledge graph → LangGraph investigation workflow → evidence-backed answer
```

Evidence is processed into structured investigative knowledge while retaining a link to its original source. Neo4j stores the persistent investigation graph: entities, events, claims, relationships, and their evidence. LangGraph controls the investigation workflow—its state, steps, and reasoning—rather than storing case knowledge. Together, semantic retrieval and graph traversal provide relevant context for each investigation.

## Evidence and provenance

OpenDetective keeps distinct the following types of investigative information:

- **Raw evidence** — the original file, record, or material provided to a case.
- **Extracted fact** — information observed or parsed from evidence.
- **Inference** — a model-generated interpretation or derived relationship.
- **Hypothesis** — an unproven explanation that can be investigated.
- **Conclusion** — a finding presented to the investigator, with its supporting evidence.

An inference does not silently become a fact. Findings identify their supporting evidence where available, so investigators can inspect the source, assess confidence, and review the reasoning before acting.

## Privacy and local operation

OpenDetective is designed for local and self-hosted operation. Case data, evidence, Neo4j data, and model access remain in the environment you control. You can run Neo4j directly or through Docker Compose, and configure a model provider appropriate for your privacy, security, and deployment requirements.

## Responsible use

Protect sensitive evidence, follow the laws and policies that apply to your investigation, and independently verify AI-assisted findings before relying on them. OpenDetective supports investigative work; it does not replace professional judgment.

## Development

```bash
git clone https://github.com/adityas2410/opendetective.git
cd opendetective
uv sync
uv run opendetective
uv run pytest
```
