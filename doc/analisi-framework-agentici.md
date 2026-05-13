# Analisi Comparativa dei Principali Framework Agentici AI

**Data di riferimento:** Maggio 2026  
**Versione documento:** 1.0

---

## Indice

1. [Introduzione](#1-introduzione)
2. [Panoramica dei Framework](#2-panoramica-dei-framework)
   - 2.1 [LangChain / LangGraph](#21-langchain--langgraph)
   - 2.2 [LangChain DeepAgents & RUC DeepAgent](#22-langchain-deepagents--ruc-deepagent)
   - 2.3 [Microsoft Agent Framework (MAF)](#23-microsoft-agent-framework-maf)
   - 2.4 [Microsoft Semantic Kernel](#24-microsoft-semantic-kernel)
   - 2.5 [Microsoft Azure AI Foundry Agent Service](#25-microsoft-azure-ai-foundry-agent-service)
   - 2.6 [CrewAI](#26-crewai)
   - 2.7 [OpenAI Agents SDK](#27-openai-agents-sdk)
   - 2.8 [Google ADK (Agent Development Kit)](#28-google-adk-agent-development-kit)
   - 2.9 [Google Gemini Enterprise Agent Platform](#29-google-gemini-enterprise-agent-platform)
   - 2.10 [Google Genkit](#210-google-genkit)
   - 2.11 [Agno (ex Phidata)](#211-agno-ex-phidata)
   - 2.12 [Haystack (deepset)](#212-haystack-deepset)
   - 2.13 [Amazon Bedrock Agents](#213-amazon-bedrock-agents)
   - 2.14 [Amazon Bedrock AgentCore](#214-amazon-bedrock-agentcore)
   - 2.15 [Strands Agents SDK (AWS)](#215-strands-agents-sdk-aws)
   - 2.16 [PydanticAI](#216-pydanticai)
   - 2.17 [Smolagents (HuggingFace)](#217-smolagents-huggingface)
   - 2.18 [LlamaIndex Workflows](#218-llamaindex-workflows)
3. [Tabella Comparativa](#3-tabella-comparativa)
4. [Analisi per Dimensione](#4-analisi-per-dimensione)
   - 4.1 [Architettura e Modello di Esecuzione](#41-architettura-e-modello-di-esecuzione)
   - 4.2 [Supporto Multi-Agente](#42-supporto-multi-agente)
   - 4.3 [Gestione della Memoria e dello Stato](#43-gestione-della-memoria-e-dello-stato)
   - 4.4 [Human-in-the-Loop (HITL)](#44-human-in-the-loop-hitl)
   - 4.5 [Tool / Function Calling](#45-tool--function-calling)
   - 4.6 [Osservabilità e Debug](#46-osservabilità-e-debug)
   - 4.7 [Licenze e Modello di Business](#47-licenze-e-modello-di-business)
   - 4.8 [Ecosistema e Community](#48-ecosistema-e-community)
   - 4.9 [Supporto Linguaggi di Programmazione](#49-supporto-linguaggi-di-programmazione)
   - 4.10 [Maturità Produttiva](#410-maturità-produttiva)
5. [Trend Trasversali](#5-trend-trasversali)
6. [Scenari d'uso e Raccomandazioni](#6-scenari-duso-e-raccomandazioni)
7. [Conclusione](#7-conclusione)

---

## 1. Introduzione

Il 2025-2026 ha segnato un punto di svolta per i sistemi AI agentici: da esperimenti di laboratorio a infrastruttura produttiva in uso presso grandi organizzazioni globali. Un **agente AI** è un sistema autonomo che percepisce il contesto, pianifica azioni, invoca strumenti (tool) e itera verso un obiettivo senza richiedere istruzioni passo-per-passo dall'utente. I framework agentici sono le piattaforme software che permettono di costruire, orchestrare e gestire questi sistemi.

Questo documento analizza i diciotto principali framework e piattaforme agentiche disponibili a maggio 2026, confrontandoli su dieci dimensioni chiave: architettura, supporto multi-agente, gestione dello stato, HITL (Human-in-the-Loop), tool calling, osservabilità, licenze, community, linguaggi di programmazione e maturità produttiva.

### Criteri di Selezione

Sono stati inclusi framework che soddisfano almeno uno dei seguenti criteri:

- Uso documentato in ambienti di produzione enterprise
- Più di 8.000 stelle su GitHub o gestito da un hyperscaler
- Attivo sviluppo con release nel 2025-2026
- Rilevanza strategica per il mercato (nuovo player da monitorare)

---

## 2. Panoramica dei Framework

### 2.1 LangChain / LangGraph

**Descrizione**  
LangChain è la libreria di componenti LLM più diffusa al mondo (100.000+ stelle su GitHub). LangGraph, introdotto nel 2024 e giunto alla versione 1.0 GA nel 2025, ne è il layer di orchestrazione: modella l'esecuzione degli agenti come grafi diretti (anche ciclici) di nodi e archi, dove ogni nodo è una funzione Python o JS e lo stato condiviso (`TypedDict`) fluisce tra i nodi. I due prodotti vengono usati in tandem: LangChain per i componenti (retrievers, memory, tools, LLM wrappers), LangGraph per orchestrarli.

**Caratteristiche chiave**

- **Grafi stateful**: nodi = unità computazionali, archi = transizioni condizionali o dirette; supporto a loop, parallelismo e subgraph
- **Checkpointing durevole**: stato persistito su SQLite, PostgreSQL o store custom; riprende l'esecuzione dal punto esatto dopo un riavvio o un errore
- **HITL nativo**: primitiva `interrupt()` che sospende il grafo in attesa di approvazione umana in qualsiasi nodo
- **Time-travel debug**: riesecuzione di qualsiasi checkpoint salvato
- **Streaming v2**: tipo unificato `StreamPart`, streaming incrementale per ogni nodo
- **Subgraph come nodi**: interi grafi agentici possono essere embedded come nodi in un grafo padre, abilitando sistemi multi-agente gerarchici
- **Deep Agents**: harness batteries-included su LangGraph per planning, delegazione e accesso al filesystem
- **MCP**: supporto nativo a Model Context Protocol per server di tool esterni
- **LangGraph Platform**: deployment gestito cloud con LangSmith (osservabilità) integrato
- **200+ integrazioni**: LLM provider, vector store, database, API

**Architettura**  
Grafo diretto (DAG o ciclico). Ogni nodo riceve lo `State` e restituisce un `State` aggiornato. Il `Checkpointer` middleware persiste lo stato dopo ogni nodo. La `ToolNode` esegue automaticamente le chiamate strumento provenienti dall'LLM. Supporta pattern supervisor, fan-out/fan-in parallelo, map-reduce, reflection loop.

**Punti di forza**
- Ecosistema e community più grandi del settore
- Durabilità + HITL first-class, non bolted-on
- Osservabilità eccellente (LangSmith)
- Scalabilità produttiva comprovata (Klarna, JPMorgan, Uber, LinkedIn)
- API sia basso livello (grafo) sia alto livello (LangChain agent)

**Punti di debolezza**
- Overhead di boilerplate per workflow semplici
- Storica criticism sulla complessità di astrazione (migliorata nella 1.0)
- CVE di sicurezza scoperte nel marzo 2026 (path traversal, SQL injection nel checkpointer)
- LangSmith completo richiede piano a pagamento

**Licenza / Business model**: MIT open source + LangGraph Platform commerciale  
**Linguaggi**: Python (primario), JavaScript/TypeScript (LangGraph.js)  
**Versione**: LangChain 1.0, LangGraph 1.0 GA  
**Stars**: LangGraph 24.800+, LangChain 100.000+

---

### 2.2 LangChain DeepAgents & RUC DeepAgent

Due progetti distinti condividono il nome "DeepAgent":

**LangChain DeepAgents** (`langchain-ai/deepagents`): harness opinionato costruito su LangChain + LangGraph runtime. Offre planning, delegazione e accesso al filesystem out-of-the-box, eliminando il wiring manuale di prompt, tools e context management. È una libreria standalone a basso livello di configurazione che sfrutta le fondamenta mature di LangGraph.

**RUC DeepAgent** (Renmin University, accettato WWW 2026 – Oral): framework accademico per agenti di ragionamento generale. Capace di scoprire e usare autonomamente tool da un catalogo di 16.000+ API (RapidAPI) mediante deep reasoning. Supporta task di navigazione (web browsing, OS interaction, embodied AI) con moduli d'azione pluggabili. Non è un framework produttivo ma un contributo scientifico di riferimento.

---

### 2.3 Microsoft Agent Framework (MAF)

**Descrizione**  
Il Microsoft Agent Framework (MAF) v1.0, rilasciato il 2 aprile 2026, è il successore unificato e produttivo che sostituisce sia AutoGen sia Semantic Kernel. AutoGen v0.4 (gennaio 2025) è ora in **maintenance mode** — nessuna nuova feature. Semantic Kernel è il layer fondazionale su cui MAF è costruito, ma il suo roadmap è stato assorbito. MAF rappresenta la scelta ufficiale Microsoft per nuovi progetti, con impegno LTS.

**Caratteristiche chiave**

- **Workflow a grafo**: orchestrazione esplicita multi-agente in grafi diretti
- **Multi-runtime**: agenti Python e .NET interoperano nativamente via protocollo A2A
- **Supporto multi-provider**: OpenAI, Azure OpenAI, HuggingFace, modelli locali
- **A2A e MCP**: supporto nativo ai due protocolli open di interoperabilità
- **State management session-based** e integrazione con Foundry Agent Service per memoria long-term gestita
- **Sicurezza**: safety guard automatici contro loop infiniti; tool calling con esecuzione automatica
- **Osservabilità**: OpenTelemetry nativo + Azure Monitor + Foundry dashboards
- **Interop .NET/Python**: best-in-class supporto C# nel panorama agentico

**Architettura**  
Gli agenti sono oggetti di prima classe. I workflow sono grafi diretti. La comunicazione avviene via message bus condiviso o invocazione diretta. Supporta pattern sequenziali, supervisor/worker, publish-subscribe.

**Punti di forza**
- Unica soluzione di produzione LTS con backing Microsoft enterprise
- Miglior supporto .NET nell'ecosistema
- Unifica la confusione AutoGen/SK
- Forte interoperabilità A2A e MCP
- Sicurezza enterprise (Entra, RBAC, identity per agente)

**Punti di debolezza**
- Migrazione da AutoGen non banale (modello di programmazione cambiato)
- Community open-source più piccola di LangGraph/CrewAI (framework GA da aprile 2026)
- Track record limitato rispetto ai concorrenti

**Licenza**: MIT  
**Linguaggi**: Python, C#/.NET  
**Versione**: v1.0 GA (2 aprile 2026)  
**Stars AutoGen (legacy)**: 50.400; MAF: in crescita rapida

---

### 2.4 Microsoft Semantic Kernel

**Descrizione**  
SDK di orchestrazione AI enterprise-grade, originariamente il framework AI di riferimento Microsoft per .NET. Introduce concetti di "plugin" (skill), planner sequenziali/stepwise/Handlebars, memoria semantica basata su embedding, RAG nativo e integrazione con Azure AI Search, Pinecone, Weaviate, Chroma.

A maggio 2026, Semantic Kernel è il **layer fondazionale** su cui è costruito MAF. Il suo roadmap è stato assorbito da MAF; nuovi progetti devono usare MAF direttamente. SK rimane la base stabile per .NET e la libreria Process Framework per modellare processi aziendali complessi è ancora attiva.

**Punti di forza**: miglior developer experience .NET, deep Azure integration, plugin ecosystem maturo  
**Punti di debolezza**: Python support storicamente in ritardo; superato da MAF per nuovi progetti

**Licenza**: MIT | **Linguaggi**: C#/.NET (primario), Python, Java (sperimentale) | **Stars**: 27.865

---

### 2.5 Microsoft Azure AI Foundry Agent Service

**Descrizione**  
Servizio cloud managed (PaaS) per il deployment, hosting e operatività di agenti AI su scala enterprise. Non è un framework di sviluppo — è il runtime di produzione per agenti costruiti con qualsiasi framework (MAF, LangGraph, CrewAI, ecc.).

**Caratteristiche chiave**
- **Hosted agents**: autoscaling, monitoring e identity gestiti da Azure
- **Memoria long-term gestita**: estrazione, consolidamento e retrieval automatici cross-session (Public Preview)
- **Cloud-hosted MCP server** (`https://mcp.ai.azure.com`, GA dicembre 2025)
- **A2A Tool**: ogni agente può orchestrare o essere orchestrato da agenti di altri vendor tramite protocollo A2A
- **Per-agent Microsoft Entra identity** e Azure RBAC
- **Content safety**: filtri built-in per prompt injection mitigation
- **1.000+ tool** curati Microsoft e partner
- **Osservabilità**: Azure Monitor + OpenTelemetry

**Punti di forza**: zero infrastructure management; sicurezza Azure native; framework-agnostic  
**Punti di debolezza**: Azure lock-in; costo a scala; alcune feature ancora in Preview

**Licenza**: Commerciale (Azure)  
**Status**: GA (con alcune feature in Preview)

---

### 2.6 CrewAI

**Descrizione**  
Framework Python per orchestrare team di agenti AI con ruoli definiti. Ogni agente ha un `role`, un `goal` e una `backstory`; collaborano su `Task` all'interno di `Crew`. Costruito da zero, senza dipendenza da LangChain — tree delle dipendenze pulito e minimale. Uno dei framework con la crescita più rapida nel settore.

**Caratteristiche chiave**

- **Crew**: team role-based con esecuzione sequenziale, gerarchica o consensuale
- **Flows** (2025): modalità pipeline event-driven per workflow deterministici orientati alla produzione
- **Memoria a 4 livelli**: short-term (in-context), long-term (DB-backed), entity memory, contextual memory; cross-session persistence built-in
- **Adaptive-depth recall**: scoring composito per il recupero dei ricordi più rilevanti
- **Planning agent**: genera piani step-by-step iniettati nel contesto dei task
- **Reasoning mode**: gli agenti riflettono prima di agire
- **MCP e A2A** nativi (v1.10.1+)
- **CrewAI AMP** (enterprise): observability dashboard, team controls, SSO, audit trail

**Architettura**  
Oggetto `Crew` contiene lista di `Agent` e `Task`. Processo di esecuzione: sequenziale (A→B→C), gerarchico (manager LLM delega), consensuale (voting). Flows: DAG event-driven.

**Punti di forza**
- API intuitiva: time-to-working-agent più veloce per scenari role-based
- Dipendenze pulite (no LangChain)
- Community enorme e in rapida crescita
- Sia Crews autonome sia Flows deterministici nello stesso framework
- 2 miliardi di esecuzioni in produzione documentate

**Punti di debolezza**
- Processo gerarchico può essere imprevedibile ("creatività" dell'agente causa drift)
- Meno controllo granulare sul grafo di esecuzione rispetto a LangGraph
- Python only
- CrewAI AMP (osservabilità avanzata) a pagamento

**Licenza**: MIT + CrewAI AMP commerciale  
**Linguaggi**: Python  
**Versione**: v1.10.1+ (marzo 2026)  
**Stars**: 47.800+; 27M+ download

---

### 2.7 OpenAI Agents SDK

**Descrizione**  
Framework ufficiale OpenAI di produzione per workflow multi-agente, rilasciato a marzo 2025 come successore produttivo del framework sperimentale `Swarm` (ora archiviato). Strutturato intorno a quattro primitive: **Agents**, **Handoffs**, **Guardrails**, **Tracing**.

**Caratteristiche chiave**

- **Agents**: LLM con istruzioni e tool
- **Handoffs**: meccanismo formale di delegazione tra agenti specializzati
- **Guardrails**: validazione input/output in parallelo all'esecuzione; fail-fast su violazioni
- **Tracing**: cattura integrata di tutti gli eventi (LLM calls, tool calls, handoff, guardrail, custom events) verso OpenAI Traces Dashboard
- **Sandboxing** (aprile 2026): agenti in ambienti di calcolo controllati con accesso file/codice isolato
- **Model Harness** (aprile 2026): istruzioni, tool, approvazioni, tracing, handoff e resume bookkeeping in un harness managed unificato
- Output strutturato via modelli Pydantic
- Streaming nativo

**Architettura**  
Flat: gli agenti chiamano tool o cedono il controllo ad altri agenti. La topologia emerge dalle dichiarazioni di handoff, senza definizione esplicita di grafo. La semplicità facilita l'onboarding (ore, non giorni).

**Punti di forza**
- API più semplice nell'ecosistema
- Supporto first-party OpenAI (accesso anticipato a nuove feature dei modelli)
- Tracing integrato zero-config
- Apache 2.0

**Punti di debolezza**
- Nessun checkpointing / stato durevole built-in (gestione manuale con store esterni)
- Ottimizzato per modelli OpenAI (altri provider funzionano ma secondari)
- Guardrails non coprono i handoff call
- Python ahead di TypeScript nelle feature

**Licenza**: Apache 2.0  
**Linguaggi**: Python (primario), TypeScript/JavaScript  
**Status**: GA, sviluppo attivo

---

### 2.8 Google ADK (Agent Development Kit)

**Descrizione**  
Toolkit open-source code-first per costruire agenti AI sofisticati. Annunciato al Google Cloud NEXT 2025 e arrivato alla versione 1.0 GA (aprile 2026, Cloud Next 2026). Framework multi-linguaggio con design model-agnostic.

**Caratteristiche chiave**

- **Multi-linguaggio**: Python, Java, Go, TypeScript — tutti a GA 1.0; semantica identica cross-linguaggio (logica prototipata in Python porta a Java senza modifiche)
- **Model-agnostic**: ottimizzato per Gemini ma supporta qualsiasi modello via LiteLLM (Anthropic, OpenAI, Meta, Mistral, ecc.)
- **A2A first-class**: inter-agent communication via Agent-to-Agent Protocol
- **MCP**: integrazione nativa con tool server MCP
- **Event Compaction**: finestra scorrevole di eventi recenti + summarizzazione degli eventi più vecchi (−38% token, −18% latency)
- **HITL**: pausa/ripresa dell'esecuzione in qualsiasi punto; rewind a qualsiasi punto della conversazione
- **Evaluation framework** integrato
- **Self-healing tool plugin**: agenti che riparano i propri errori
- **Cloud API Registry**: tool managed nel cloud

**Architettura**  
Stack a 3 livelli: Tool Layer (MCP) → Agent Layer (A2A) → Orchestrator Layer (ADK). ADK gestisce intent, planning, memoria globale e HITL.

**Punti di forza**
- Unico framework major nativo su 4 linguaggi a GA
- Interoperabilità A2A open (150+ organizzazioni in produzione, governance Linux Foundation)
- Performance ottimizzata (Event Compaction)
- Backed da Google con deployment su Vertex AI Agent Engine

**Punti di debolezza**
- Community più piccola di LangGraph/CrewAI
- Ottimizzato per Gemini (altri modelli richiedono LiteLLM adapter)
- Documentazione meno matura dei competitor storici

**Licenza**: Apache 2.0  
**Linguaggi**: Python, Java, Go, TypeScript  
**Versione**: 1.0 GA (aprile 2026)  
**Stars**: 8.200+ (in rapida crescita)

---

### 2.9 Google Gemini Enterprise Agent Platform

**Descrizione**  
Piattaforma cloud managed Google per il deployment enterprise di agenti AI (annunciata a Google I/O, maggio 2026 — rebranding di Vertex AI Agent Builder). Bundla ADK + runtime managed + builder low-code + governance.

**Caratteristiche chiave**
- **Agent Studio**: builder drag-and-drop low-code
- **200+ modelli foundation** inclusi Gemini e Claude
- **Agent Engine Runtime**: serverless managed deployment
- **Memory Bank + Memory Profiles**: memoria cross-session persistente (mesi di recall)
- **Semantic layer**: agenti che ragionano sui dati enterprise
- **Simulation environment**: stress-test prima del deployment
- **Agent Identity, Registry, Gateway**: governance enterprise
- **Marketplace** di agenti terze parti
- **Secure Workspaces**: bash + file sandboxed

**Punti di forza**: full-stack da prototipo a produzione; infrastructure Google-scale; miglior accesso ai modelli Gemini  
**Punti di debolezza**: GCP lock-in; costi a scala; governance feature annunciate a maggio 2026, maturità da verificare

**Licenza**: Commerciale (GCP)  
**Status**: GA (PaaS)

---

### 2.10 Google Genkit

**Descrizione**  
Framework open-source full-stack per applicazioni AI in JavaScript/TypeScript (primario), Go, Python (preview) e Dart (preview). Costruito e usato da Google/Firebase in produzione. v1.0 rilasciata fine 2024.

**Caratteristiche chiave**
- Interfaccia unificata per 100+ modelli (Google, OpenAI, Anthropic, Ollama, ecc.)
- Output strutturato, tool calling agentico, I/O multimodale
- UI locale di dev e CLI per debug/osservabilità
- Deploy su Cloud Functions, Cloud Run o qualsiasi ambiente Node.js
- Integrazioni con Next.js, React, Angular, iOS, Android
- Flows: pipeline strongly-typed

**Best for**: sviluppatori JavaScript/TypeScript full-stack che vogliono aggiungere feature AI ad app web esistenti  
**Limitazioni**: non è progettato per orchestrazione multi-agente complessa; nessun HITL first-class; nessuna durabilità built-in

**Licenza**: Apache 2.0  
**Linguaggi**: JavaScript/TypeScript (full), Go (stabile), Python (preview), Dart (preview)

---

### 2.11 Agno (ex Phidata)

**Descrizione**  
Rebrandato da Phidata ad Agno nel gennaio 2025. Runtime Python ad alte prestazioni per costruire, deployare e gestire agenti AI a scala. Si posiziona come alternativa performance-first a LangGraph.

**Caratteristiche chiave**
- **Performance**: creazione agente ~2 microsecondi, 3,75 KiB/agente (claim: 5.000× più veloce di LangGraph nella creazione)
- **Teams**: collaborazione multi-agente role-defined
- **Workflows**: esecuzione sequenziale e parallela
- **Agentic RAG / Auto-RAG**: gli agenti cercano nella knowledge base on-demand invece di inserire sempre il contesto; pioneer del paradigma
- **Reasoning Agents**: risoluzione di problemi step-by-step con backtracking (sperimentale)
- **Multi-modal**: testo, immagine, audio, video nativamente
- **Monitoring** e gestione della memoria integrati

**Architettura**  
Agenti leggeri (footprint minimale). Teams coordinano agenti. Workflows orchestrano l'esecuzione. Memoria gestita come concern di servizio.

**Punti di forza**
- Performance eccezionale (latency/memoria a scala)
- Auto-RAG pioneer
- API minimale e pulita
- Multi-modal day-one

**Punti di debolezza**
- Community più piccola di LangGraph/CrewAI
- Monitoring parzialmente legato alla piattaforma proprietaria (AgentPhi)
- Claims di performance (5.000×) da verificare con benchmark indipendenti
- Ecosistema di integrazioni meno maturo

**Licenza**: Mozilla Public License 2.0  
**Linguaggi**: Python  
**Stars**: 39.100+

---

### 2.12 Haystack (deepset)

**Descrizione**  
Framework Python open-source per applicazioni LLM production-ready. Specializzato in pipeline RAG e workflow NLP. Haystack 2.x (2024) è una riscrittura completa con architettura pipeline-first. Haystack 1.x ha raggiunto end-of-life l'11 marzo 2025.

**Caratteristiche chiave**
- **Pipeline modulare**: grafi diretti di componenti (retriever, reader, generator, router) con loop e branch per comportamenti agent-like
- **Componente Agent**: integrazione LLM in loop con tool call e template Jinja2 per system prompt
- **LLMRanker**: reranking semantico powered by LLM nelle pipeline RAG
- **Hayhooks**: espone pipeline/agenti come endpoint HTTP o MCP server; endpoint OpenAI-compatible per chat completion
- **100+ integrazioni**: OpenAI, Mistral, Anthropic, Cohere, HuggingFace, Azure OpenAI, AWS Bedrock, modelli locali
- **Haystack Enterprise Platform**: osservabilità, collaboration, governance, access control (commerciale)

**Architettura**  
Pipeline-first: le pipeline sono l'unità di composizione. Componenti collegati via input/output tipizzati. Meno orientato al grafo di LangGraph — più chaining lineare con branching.

**Punti di forza**
- Best-in-class tooling per pipeline RAG/NLP
- Architettura esplicita e trasparente
- Heritage NLP/document processing
- Release bi-settimanali regolari

**Punti di debolezza**
- Orchestrazione multi-agente non è il primary use case
- HITL non è una primitiva first-class
- Community più piccola di LangChain/CrewAI

**Licenza**: Apache 2.0  
**Linguaggi**: Python  
**Versione**: 2.22.0 (8 gennaio 2026)  
**Stars**: 17.000+

---

### 2.13 Amazon Bedrock Agents

**Descrizione**  
Servizio AWS managed per costruire agenti autonomi senza scrivere codice di orchestrazione. Configurato tramite AWS Console o SDK: si dichiara modello, system prompt, tool (Lambda functions, Knowledge Bases) e AWS gestisce il reasoning loop.

**Caratteristiche chiave**
- Loop agente completamente managed (nessun codice di orchestrazione)
- Knowledge Base integration (RAG con Bedrock)
- Action Groups: Lambda functions come tool
- Episodic Memory cross-session
- Bedrock Guardrails per safety/reliability
- Inline agents: composizione dinamica a runtime
- Multi-agent collaboration: agente supervisor delega a sub-agenti
- Evaluations (preview): correttezza, fedeltà, accuratezza selezione tool

**Limitazioni**: funziona solo con modelli hostati su Amazon Bedrock  
**Punti di debolezza**: less developer control; AWS lock-in  
**Licenza**: Commerciale (AWS)

---

### 2.14 Amazon Bedrock AgentCore

**Descrizione**  
Piattaforma end-to-end (GA ottobre 2025) per costruire, deployare e operare agenti usando qualsiasi framework e modello. Framework-agnostic: funziona con LangGraph, CrewAI, LlamaIndex, Strands, ecc.

**Componenti**
- **Runtime**: sessioni serverless isolate, per-session microVM, persistenza filesystem (suspend/resume mid-task)
- **Memory**: memoria persistente con estrazione, consolidamento e retrieval intelligenti
- **Gateway**: integrazione tool sicura e controllata
- **Browser + Code Interpreter**: browser headless e sandbox per esecuzione codice
- **Identity**: access control per agente
- **Policy**: governance e compliance
- **Observability**: full OTEL-compatible su CloudWatch; metriche runtime a 1-secondo granularity
- **Payments** (preview, maggio 2026): agenti che accedono e pagano autonomamente API, MCP server e contenuti web (partnership Coinbase + Stripe)
- **Managed Agent Harness** (preview): dichiara model + prompt + tool, ottieni loop agente managed; supporta modelli OpenAI nativamente

**Punti di forza**: framework-agnostic; sicurezza enterprise AWS; capability pagamenti unica nell'ecosistema  
**Punti di debolezza**: diversi componenti ancora in Preview; pricing complesso; dipendenza AWS

**Licenza**: Commerciale (AWS)  
**Status**: GA (ottobre 2025), alcune feature in Preview

---

### 2.15 Strands Agents SDK (AWS)

**Descrizione**  
SDK open-source (Apache 2.0) di AWS rilasciato a maggio 2025. Framework code-first model-driven per costruire agenti in Python e TypeScript in poche righe. 14M+ download. Si integra nativamente con AgentCore.

**Caratteristiche chiave**
- **Model-driven**: sfrutta il ragionamento nativo dell'LLM per la selezione dei tool
- Multi-agent: pattern Swarm, Graph, Workflow
- MCP nativo
- Strands Agent SOPs: definizioni workflow in linguaggio naturale
- AgentCore harness integration: deploy in 3 API call

**Punti di forza**: Apache 2.0; API minimale; integrazioni AWS; contributi da Accenture, Anthropic, Meta, PwC  
**Punti di debolezza**: AWS-centric; TypeScript SDK meno maturo del Python; community più piccola

**Licenza**: Apache 2.0  
**Linguaggi**: Python, TypeScript

---

### 2.16 PydanticAI

**Descrizione**  
Framework per agenti AI type-safe sviluppato dal team Pydantic. Rilasciato a novembre 2024, v1 nel 2025 (versione 1.94.0 al 12 maggio 2026). Porta la filosofia di validazione e developer ergonomics di Pydantic allo sviluppo agentico.

**Caratteristiche chiave**
- Type safety rigorosa throughout (input tipizzati, firme tool, output)
- Instrumentazione OpenTelemetry built-in
- Durable Execution: preserva il progresso attraverso errori API e riavvii
- MCP, A2A, UI event stream standard
- Web search, thinking, capability harness integrati
- 100% test coverage

**Best for**: team che usano già Pydantic/FastAPI che vogliono aggiungere agenti type-safe  
**Limitazioni**: focus su singolo agente (non framework multi-agente)

**Licenza**: MIT | **Linguaggi**: Python | **Stars**: 17.000+

---

### 2.17 Smolagents (HuggingFace)

**Descrizione**  
Framework Python minimalista di HuggingFace. ~1.000 righe di logica core. Code-first: gli agenti scrivono ed eseguono codice Python per raggiungere i loro obiettivi, invece di chiamare schemi JSON di tool.

**Caratteristiche chiave**
- **CodeAgent**: scrive + esegue Python come azioni (paradigma code-as-action)
- **ToolCallingAgent**: tool calling JSON tradizionale
- Sandboxing sicuro (Blaxel, E2B, Modal, Docker, Pyodide+Deno WebAssembly)
- Model-agnostic (transformers locali, Ollama, OpenAI, Anthropic via LiteLLM)
- HuggingFace Hub: condivisione/importazione tool e agenti
- Multi-modal: testo, visione, video, audio
- Multi-agent via managed agents

**Punti di forza**
- Eccezionalmente minimale e auditabile
- Il paradigma code-as-action supera il JSON tool calling in benchmark di ragionamento
- Accesso all'enorme ecosistema HuggingFace
- Crescita stellare (da 3K a 26K+ stelle in ~12 mesi)

**Punti di debolezza**
- Dipendenza dal modello (richiede modelli capaci di scrivere codice corretto)
- Nessuna durabilità/checkpointing built-in
- Enterprise tooling meno maturo

**Licenza**: Apache 2.0 | **Linguaggi**: Python | **Stars**: 26.300+

---

### 2.18 LlamaIndex Workflows

**Descrizione**  
LlamaIndex si è evoluto da data framework per RAG a piattaforma agentica con Workflows (orchestrazione multi-step event-driven) e LlamaAgents (deployment one-click di agenti documentali). Focus su document AI e OCR con LlamaParse.

**Caratteristiche chiave**
- Workflows: async, event-driven, altamente controllato
- Workflow Debugger: visualizzazione, event log in tempo reale, confronto run
- LlamaAgents: agenti documentali deployati (invoice processing, contract review, claims)
- LlamaParse: parsing documenti avanzato (40+ skill, tabelle, grafici, immagini)
- AgentFS: filesystem virtuale per coding agent sicuri
- ACP integration, MCP servers, persistent memory
- TypeScript support completo

**Best for**: workflow agentici document-heavy; enterprise document AI  
**Licenza**: MIT | **Linguaggi**: Python, TypeScript | **Stars**: ~40.000

---

## 3. Tabella Comparativa

| Framework | Stars (mag 2026) | Linguaggi | Licenza | Multi-Agente | HITL | Stato Durevole | Maturità Produttiva |
|---|---|---|---|---|---|---|---|
| **LangGraph** | 24.800+ | Python, JS/TS | MIT | ★★★★★ | ★★★★★ | ★★★★★ | GA Stabile |
| **Microsoft Agent Framework** | Crescita rapida | Python, .NET | MIT | ★★★★★ | ★★★★☆ | ★★★★☆ | GA v1.0 (apr 2026) |
| **CrewAI** | 47.800+ | Python | MIT | ★★★★★ | ★★★☆☆ | ★★★☆☆ | GA, molto attivo |
| **OpenAI Agents SDK** | In crescita | Python, JS/TS | Apache 2.0 | ★★★☆☆ | ★★★☆☆ | ★★☆☆☆ | GA, sviluppo attivo |
| **Google ADK** | 8.200+ | Py/Java/Go/TS | Apache 2.0 | ★★★★★ | ★★★★☆ | ★★★★☆ | GA 1.0 (apr 2026) |
| **Agno (Phidata)** | 39.100+ | Python | MPL 2.0 | ★★★★☆ | ★★★☆☆ | ★★★☆☆ | GA, attivo |
| **Haystack** | 17.000+ | Python | Apache 2.0 | ★★☆☆☆ | ★★☆☆☆ | ★★☆☆☆ | GA, attivo (RAG) |
| **Smolagents** | 26.300+ | Python | Apache 2.0 | ★★★☆☆ | ★☆☆☆☆ | ★☆☆☆☆ | GA, attivo |
| **PydanticAI** | 17.000+ | Python | MIT | ★★☆☆☆ | ★★☆☆☆ | ★★★★☆ | GA, attivo |
| **LlamaIndex** | ~40.000 | Python, TS | MIT | ★★★☆☆ | ★★★☆☆ | ★★★☆☆ | GA, attivo |
| **Strands Agents** | In crescita | Python, TS | Apache 2.0 | ★★★☆☆ | ★★☆☆☆ | Via AgentCore | GA |
| **Bedrock Agents** | N/A (AWS) | Qualsiasi | Commerciale | ★★★★☆ | ★★★☆☆ | ★★★☆☆ | GA (AWS) |
| **Bedrock AgentCore** | N/A (AWS) | Qualsiasi | Commerciale | ★★★★★ | ★★★★☆ | ★★★★★ | GA (ott 2025) |
| **Azure Foundry Agent Service** | N/A (Azure) | Qualsiasi | Commerciale | ★★★★★ | ★★★★☆ | ★★★★☆ | GA (alcune Preview) |
| **Gemini Ent. Agent Platform** | N/A (GCP) | Qualsiasi | Commerciale | ★★★★★ | ★★★★☆ | ★★★★★ | GA (PaaS) |
| **Genkit** | In crescita | JS/TS/Go/Py | Apache 2.0 | ★★☆☆☆ | ★☆☆☆☆ | ★☆☆☆☆ | GA 1.0 |
| **AutoGen (legacy)** | 50.400 | Python | MIT | ★★★★☆ | ★★★☆☆ | ★★☆☆☆ | Maintenance mode |
| **Semantic Kernel (legacy)** | 27.865 | .NET/Py | MIT | Via MAF | Via MAF | Via MAF | Assorbito da MAF |

---

## 4. Analisi per Dimensione

### 4.1 Architettura e Modello di Esecuzione

I framework agentici adottano tre paradigmi architetturali distinti:

**Grafo esplicito** (LangGraph, MAF, Google ADK): l'esecuzione è un grafo diretto dove ogni nodo è un'unità computazionale e ogni arco è una transizione. Questo approccio offre il massimo controllo, osservabilità e prevedibilità, ma richiede più boilerplate. È il paradigma preferito per sistemi di produzione complessi.

**Role-based teams** (CrewAI): gli agenti sono entità con ruolo, obiettivo e backstory. Il comportamento emerge dalla collaborazione. Più intuitivo e naturale per workflow umano-like, ma meno controllabile nel dettaglio. I Flows di CrewAI aggiungono un layer deterministico ove necessario.

**Flat + Handoff** (OpenAI Agents SDK, Strands): topologia implicita — non si definisce un grafo, si dichiarano handoff tra agenti. Massima semplicità di onboarding; adatto per pipeline relativamente lineari con specializzazione per dominio.

**Pipeline** (Haystack): componenti collegati in pipeline directed multigraph. Ottimale per RAG/NLP, meno adatto per orchestrazione multi-agente complessa.

**Code-as-Action** (Smolagents): gli agenti scrivono ed eseguono codice Python invece di chiamare tool JSON. Paradigma potente ma dipendente dalla capacità di coding del modello.

### 4.2 Supporto Multi-Agente

| Livello | Framework |
|---|---|
| **Eccellente** (pattern multipli, supervisor/worker, parallelismo, cross-lang) | LangGraph, MAF, CrewAI, Google ADK, Bedrock AgentCore, Azure Foundry, Gemini Enterprise |
| **Buono** (supporto multi-agente ma pattern limitati) | OpenAI Agents SDK, Agno, Strands, LlamaIndex Workflows |
| **Base** (possibile ma non il primary use case) | Smolagents, Genkit |
| **Limitato / Single-agent** | Haystack, PydanticAI |

LangGraph è l'unico framework open-source che supporta nativamente subgraph composabili, parallelismo a livello di nodo, supervisor gerarchici e time-travel debug simultaneamente.

### 4.3 Gestione della Memoria e dello Stato

La gestione dello stato è la dimensione con la maggiore varianza tra i framework:

**Stato durevole first-class**: LangGraph (checkpointer su DB), MAF (session-based + Foundry memory), Google ADK (sessioni persistenti + Event Compaction), Bedrock AgentCore (per-session microVM + filesystem persistence), PydanticAI (Durable Execution), Gemini Enterprise (Memory Bank + Memory Profiles).

**Memoria multi-livello**: CrewAI (short-term, long-term, entity, contextual), Agno (4 livelli simili).

**Memoria manuale** (store esterno richiesto): OpenAI Agents SDK, Smolagents, Genkit, Haystack.

Il trend è chiaro: la memoria cross-session persistente è diventata un'aspettativa di base per le piattaforme enterprise, non più un differenziatore.

### 4.4 Human-in-the-Loop (HITL)

**First-class primitive**: LangGraph (`interrupt()` built-in su qualsiasi nodo), Google ADK (pausa/ripresa ovunque nel workflow).

**Built-in con workflow**: MAF, Bedrock AgentCore, Azure Foundry Agent Service, Gemini Enterprise.

**Parziale** (implementabile ma non primitiva): CrewAI (Flow interrupts, task callbacks), OpenAI Agents SDK (Model Harness + approval pattern), Agno, LlamaIndex.

**Manuale** (nessun supporto nativo): Haystack, Smolagents, Genkit, Strands.

Per workflow in ambienti regulated (finance, healthcare, legal) dove l'approvazione umana è obbligatoria, LangGraph e Google ADK offrono il supporto più solido.

### 4.5 Tool / Function Calling

Tutti i framework major supportano tool/function calling. Le differenze sono nella qualità dell'integrazione:

- **MCP nativo**: LangGraph, MAF, CrewAI, Google ADK, OpenAI Agents SDK, Strands, PydanticAI, Haystack (via Hayhooks)
- **A2A nativo**: MAF, Google ADK, CrewAI, Azure Foundry Agent Service, Gemini Enterprise, Bedrock AgentCore
- **Tooling ecosistema vasto**: LangChain (200+ integrazioni), Haystack (100+), CrewAI (200+)

### 4.6 Osservabilità e Debug

| Framework | Soluzione | Note |
|---|---|---|
| LangGraph | LangSmith + LangGraph Studio | Time-travel debug, tracing completo; tier avanzati a pagamento |
| MAF | OpenTelemetry + Azure Monitor + Foundry | Enterprise-grade, nativo cloud |
| CrewAI | CrewAI AMP + Langfuse/LangSmith | AMP a pagamento per dashboard avanzate |
| OpenAI Agents SDK | OpenAI Traces Dashboard | Zero-config, export a sistemi esterni |
| Google ADK | Vertex AI Agent Engine monitoring | Evaluation framework integrato |
| Bedrock AgentCore | CloudWatch OTEL + 1s granularity | Metriche runtime molto dettagliate |
| Haystack | Enterprise Platform + Langfuse/Datadog | Open-source richiede integrazione esterna |
| PydanticAI | OpenTelemetry built-in | Eccellente per team già su OTel |
| Smolagents | Nessuna (logging manuale) | Limitazione significativa |

### 4.7 Licenze e Modello di Business

| Modello | Framework |
|---|---|
| **MIT (open source)** | LangGraph, LangChain, MAF, AutoGen, Semantic Kernel, CrewAI, LlamaIndex, PydanticAI |
| **Apache 2.0** | Google ADK, Genkit, OpenAI Agents SDK, Strands, Haystack, Smolagents |
| **MPL 2.0** | Agno |
| **Commerciale (cloud)** | Azure Foundry, Gemini Enterprise, Bedrock Agents, Bedrock AgentCore |
| **Ibrido (open core + commerciale)** | LangGraph Platform, CrewAI AMP |

La maggioranza dei framework è open-source. Le piattaforme cloud sono commerciali ma spesso framework-agnostic (hosting di agenti scritti con tool open-source).

### 4.8 Ecosistema e Community

**Ecosistema dominante**: LangChain/LangGraph (~47M download mensili combinati, 100K+ stelle su GitHub totali). Repository di integrazioni più vasto.

**Crescita più rapida**: CrewAI (da 0 a 47.800+ stelle in meno di 2 anni; 2B+ esecuzioni); Smolagents (da 3K a 26K+ stelle in ~12 mesi).

**Backed da hyperscaler**: MAF (Microsoft), Google ADK (Google), Strands/Bedrock (AWS). Garantiscono stabilità a lungo termine ma con rischio lock-in sull'ecosistema.

**Niche ma solida**: PydanticAI (17K stelle, adottato da chi usa già Pydantic/FastAPI).

**Da evitare per nuovi progetti**: SuperAGI (sviluppo stalled), AutoGen legacy (maintenance mode), Semantic Kernel standalone (assorbito da MAF).

### 4.9 Supporto Linguaggi di Programmazione

| Linguaggio | Framework supportati |
|---|---|
| **Python** | Tutti (primario per la maggioranza) |
| **JavaScript/TypeScript** | LangGraph.js, OpenAI Agents SDK JS, Google ADK, Genkit (primario), Strands, LlamaIndex |
| **C#/.NET** | MAF (co-primario), Semantic Kernel (primario) |
| **Java** | Google ADK (GA 1.0), Semantic Kernel (sperimentale) |
| **Go** | Google ADK (GA 1.0), Genkit (stabile) |
| **Dart** | Genkit (preview) |

Google ADK è l'unico framework open-source con parità GA su 4 linguaggi. Per team .NET, MAF è l'unica scelta seria.

### 4.10 Maturità Produttiva

**Produzione enterprise comprovata**:
- LangGraph: Klarna ($60M/anno risparmiati), JPMorgan, Uber, LinkedIn, Elastic, AppFolio; 90M download mensili; 400+ aziende su LangGraph Platform
- CrewAI: 2B+ esecuzioni, 150+ clienti enterprise, Fortune 500
- Bedrock AgentCore: GA ottobre 2025, SLA enterprise AWS

**GA recente (stabilità da verificare nel tempo)**:
- MAF v1.0 (aprile 2026): promettente, engagement Microsoft forte; track record di produzione limitato
- Google ADK 1.0 (aprile 2026): 7M+ download Python; adozione enterprise in crescita

**Non adatto a nuovi progetti produttivi**:
- AutoGen (maintenance mode)
- SuperAGI (stalled)
- TaskWeaver (research)

---

## 5. Trend Trasversali

### 5.1 Standardizzazione dei Protocolli

Due protocolli open hanno raggiunto massa critica nel 2025-2026:

**MCP (Model Context Protocol)**: diventato lo standard de-facto per l'interfaccia tra agenti e tool esterni. Adottato da praticamente tutti i framework major — consente a qualsiasi agente di connettersi a qualsiasi MCP server.

**A2A (Agent-to-Agent Protocol)**: standard per la comunicazione inter-agente tra vendor diversi, co-promosso da Google, Microsoft e AWS. 150+ organizzazioni in produzione; ora sotto governance della Linux Foundation. Abilita federation di agenti cross-organizzazione.

Questi due protocolli stanno creando un ecosistema di "mattoncini" intercambiabili che riduce il lock-in su singoli framework.

### 5.2 Consolidamento Microsoft

La confusione AutoGen vs. Semantic Kernel è risolta: Microsoft Agent Framework (MAF v1.0, aprile 2026) è l'unico framework Microsoft per nuovi progetti. .NET shop devono standardizzarsi su MAF.

### 5.3 Full-Stack di Google

Google ha consegnato uno stack completo a tre livelli: ADK (open-source SDK) + A2A (protocollo) + Gemini Enterprise Agent Platform (PaaS managed), tutti GA ad aprile/maggio 2026. Il rebranding in "Gemini Enterprise Agent Platform" segnala un impegno strategico significativo.

### 5.4 Strategia a Tre Binari di AWS

AWS opera in parallelo: Bedrock Agents (no-code managed), AgentCore (infrastruttura framework-agnostic), Strands (open-source SDK). Il supporto nativo per modelli OpenAI in Bedrock e i pagamenti autonomi in AgentCore (maggio 2026) sono differenziatori notevoli.

### 5.5 Type Safety Come Standard

PydanticAI segnala un trend più ampio verso agenti tipizzati, validati e testabili. Ci si aspetta che questo pattern influenzi altri framework nell'arco del 2026.

### 5.6 Code-as-Action

Il paradigma CodeAgent di Smolagents — agenti che scrivono Python invece di chiamare tool JSON — ha dimostrato performance superiori nei benchmark di ragionamento. Questo approccio influenza l'evoluzione di altri framework.

### 5.7 Memoria Come Infrastruttura

La memoria cross-session persistente è passata da nice-to-have a feature attesa. Tutti i provider hyperscaler (Azure, GCP, AWS) offrono ora memoria managed con estrazione, consolidamento e retrieval automatici.

### 5.8 Agenti con Capacità di Pagamento

Amazon Bedrock AgentCore ha annunciato in preview (maggio 2026) la capacità per gli agenti di accedere e pagare autonomamente API, MCP server e contenuti web (partnership Coinbase + Stripe). È la prima piattaforma major a offrire questa capacità — potenzialmente trasformativa per sistemi agentici commerciali autonomi.

---

## 6. Scenari d'uso e Raccomandazioni

| Scenario | Framework Consigliato | Motivazione |
|---|---|---|
| **Agenti Python production-grade complessi** | LangGraph | Checkpointing, HITL, time-travel, community, LangSmith |
| **Team .NET enterprise** | Microsoft Agent Framework | Unica scelta matura .NET, LTS Microsoft |
| **Prototipazione rapida role-based** | CrewAI | API più intuitiva, time-to-working-agent minimo |
| **Pipeline OpenAI-centric semplici** | OpenAI Agents SDK | Semplicità, tracing zero-config, accesso anticipato modelli |
| **Multi-cloud / multi-linguaggio** | Google ADK | 4 linguaggi GA, A2A, model-agnostic |
| **Infrastruttura cloud AWS** | Bedrock AgentCore + Strands | Framework-agnostic, sicurezza enterprise, deploy rapido |
| **Infrastruttura cloud Azure** | Azure Foundry + MAF | Integrazione nativa Entra, 1.000+ tool managed |
| **Infrastruttura cloud GCP** | Gemini Enterprise + ADK | Gemini-native, memoria long-term, governance |
| **Pipeline RAG / NLP** | Haystack | Best-in-class RAG tooling, architettura trasparente |
| **Agenti document-heavy** | LlamaIndex Workflows | LlamaParse, AgentFS, document AI specializzato |
| **Developer JavaScript/web full-stack** | Genkit | JS/TS-first, deploy Firebase/Cloud Run |
| **Performance critica / larga scala** | Agno | Footprint minimale, Auto-RAG, latenza bassissima |
| **Type safety / team Pydantic/FastAPI** | PydanticAI | Type safety rigorosa, OTel, Durable Execution |
| **Research / prototipi minimali** | Smolagents | Code-as-action, ~1.000 righe core, auditabile |

---

## 7. Conclusione

Il panorama dei framework agentici nel 2026 è caratterizzato da **maturità crescente**, **standardizzazione dei protocolli** e una **polarizzazione** tra soluzioni open-source generaliste e piattaforme cloud managed verticali.

### Il quadro emergente

**LangGraph si è affermato come standard de facto** per lo sviluppo Python di agenti in produzione. Il suo modello a grafo stateful con checkpointing durevole, HITL nativo e osservabilità tramite LangSmith rappresenta il livello di riferimento che i nuovi competitor devono raggiungere. Con 90 milioni di download mensili e 400+ aziende sulla piattaforma managed, il network effect è significativo.

**CrewAI è l'alternativa più credibile**, con 2 miliardi di esecuzioni documentate e un'API intuitiva che ha abbassato drasticamente la barriera di accesso. L'aggiunta dei Flows nel 2025 ha risposto alla principale critica (mancanza di determinismo), rendendolo competitivo anche per workflow critici.

**Il 2026 è l'anno del consolidamento dei big player**: Microsoft ha unificato AutoGen e Semantic Kernel nel MAF v1.0; Google ha consegnato ADK 1.0 e Gemini Enterprise Agent Platform; AWS ha completato la triade Bedrock Agents / AgentCore / Strands. Tutti e tre hanno raggiunto la GA nelle stesse settimane (aprile-maggio 2026), segnalando che il mercato enterprise è ora considerato maturo.

**I protocolli MCP e A2A sono diventati infrastruttura**: non sono più differenziatori ma prerequisiti. Il loro successo riduce il lock-in su singoli framework, favorendo ecosistemi di componenti intercambiabili.

### Raccomandazioni strategiche

1. **Per nuovi progetti Python**: partire da LangGraph se si prevede complessità (stati durevoli, HITL, multi-agente gerarchico) o da CrewAI se si privilegia la velocità di prototipazione con ruoli ben definiti.

2. **Per organizzazioni .NET**: adottare Microsoft Agent Framework senza esitazioni — è l'unica scelta produttiva con LTS e il backing di un hyperscaler nel mondo .NET.

3. **Per team cloud-first**: scegliere la piattaforma managed dello stesso cloud provider già in uso (Azure Foundry, Bedrock AgentCore, Gemini Enterprise) per massimizzare sicurezza, integrazione e governance, usando framework open-source (LangGraph, CrewAI, ADK) per la logica degli agenti.

4. **Da evitare**: AutoGen legacy e SuperAGI per nuovi progetti; migrare da AutoGen v0.4 a MAF seguendo la migration guide Microsoft.

5. **Da monitorare**: PydanticAI (type safety trend), Smolagents (code-as-action paradigm), e le capability di pagamento autonomo di Bedrock AgentCore — indicatori dei prossimi sviluppi dell'intero settore.

In definitiva, il settore agentico ha raggiunto una maturità sufficiente per deployment enterprise su larga scala, ma rimane in rapida evoluzione. La scelta del framework giusto dipende criticamente dal contesto: linguaggi di programmazione del team, cloud provider, requisiti di durabilità e controllabilità, e velocità di iterazione richiesta. Non esiste un unico "vincitore" — esiste lo strumento giusto per ogni contesto.

---

*Documento redatto in data 13 maggio 2026 sulla base di informazioni pubblicamente disponibili, documentazione ufficiale, changelog e annunci dei vendor. I dati relativi a stelle GitHub, download e adozione sono indicativi e soggetti a variazione.*
