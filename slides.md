
<!-- .slide: class="deck-slide" -->

## About the Speaker

<div class="deck-concept-grid">
  <div class="deck-card highlight-card" style="flex: 0 0 260px; text-align: center; align-items: center; justify-content: center;">
    <img src="me.jfif" alt="Morteza Kasravi" style="width: 130px; height: 140px; border-radius: 50%; object-fit: cover; border: 3px solid rgba(125, 249, 255, 0.5); box-shadow: 0 8px 24px rgba(0, 0, 0, 0.3); margin-bottom: 12px;" />
    <h3 style="font-size: 1.3rem !important; margin: 0 0 4px 0 !important;">Morteza Kasravi</h3>
    <p class="card-sub" style="margin-bottom: 12px !important;">Danfoss</p>
    
  </div>

  <div class="deck-card" style="flex: 1 1 0;">
    <div class="card-header">
      <h3>Tech Lead & Ontologist</h3>
    </div>
    <p class="card-sub">Danfoss • AI Architecture & Knowledge Engineering</p>
    <ul>
      <li><strong>Role at Danfoss:</strong> Tech Lead & Ontologist, shaping domain knowledge graphs, data models & AI architecture.</li>
      <li><strong>Core Focus:</strong> Practical LLMs, System Prompting, RAG (Retrieval-Augmented Generation) & Knowledge Representation.</li>
      <li><strong>Workshop Goal:</strong> Demystifying LLMs and building clear mental models for engineering & technical teams.</li>
    </ul>
  </div>
</div>

Note:
- Introduction to speaker background and role at Danfoss
- Transition into the core workshop topics

---

<!-- .slide: class="topic-title-only" -->

## What are LLMs and how do they work at a high level?

<div class="llm-diagram" aria-hidden="true">
	<div class="llm-node llm-node-soft">Input tokens</div>
	<div class="llm-connector"></div>
	<div class="llm-node llm-node-core">Model</div>
	<div class="llm-connector"></div>
	<div class="llm-node llm-node-soft">Output tokens</div>
</div>

Note:
- Factual vs. statistical systems
- Lowest level of abstraction: get tokens, output tokens
- Training and fitting: what is overfit, underfit
- Evaluations: underlying models change
- Guardrails: bring determinism into a probabilistic flow
- Automated evaluations and the difference between cross-validation and test corpora

---

<!-- .slide: class="compare-slide" -->

## Factual vs. Statistical Systems

<div class="compare-grid">
  <div class="compare-card">
    <div class="card-header">
      <span class="card-icon">⚡</span>
      <h3>Factual / Deterministic</h3>
    </div>
    <p class="card-sub">Rules, Logic & Absolute Truth</p>
    <ul>
      <li>Calculates exact, reproducible outputs</li>
      <li>Strict logic & database lookups</li>
      <li>Fails on ambiguity or missing data</li>
    </ul>
  </div>

  <div class="compare-vs">VS</div>

  <div class="compare-card highlight-card">
    <div class="card-header">
      <span class="card-icon">🎲</span>
      <h3>Statistical / Probabilistic</h3>
    </div>
    <p class="card-sub">Patterns, Weights & Prediction</p>
    <ul>
      <li>Predicts most likely next token</li>
      <li>Learns patterns & context from vast data</li>
      <li>Flexible & creative, but can hallucinate</li>
    </ul>
  </div>
</div>

Note:
- Traditional code vs. ML models
- Deterministic = same input always yields exact same output
- Probabilistic = outputs are sampled from probability distributions
- LLMs don't "know" facts; they predict statistically plausible text

---

<!-- .slide: class="deck-slide" -->

## Lowest Level of Abstraction

<div class="deck-concept-grid">
  <div class="deck-card">
    <div class="card-header"><span class="card-icon">🔤</span><h3>Tokens In</h3></div>
    <p class="card-sub">Input Pipeline</p>
    <ul>
      <li>Text converted into numerical sub-word chunks (tokens)</li>
      <li>Context window defines memory limit (e.g. 128k tokens)</li>
      <li>Prompt & system instructions form single input sequence</li>
    </ul>
  </div>

  <div class="deck-card highlight-card">
    <div class="card-header"><span class="card-icon">🧠</span><h3>Auto-Regression</h3></div>
    <p class="card-sub">Core Mechanism</p>
    <ul>
      <li>Calculates probability distribution for next token</li>
      <li>Appends generated token to input & repeats loop</li>
      <li>No internal state between API calls (stateless)</li>
    </ul>
  </div>

  <div class="deck-card">
    <div class="card-header"><span class="card-icon">🔠</span><h3>Tokens Out</h3></div>
    <p class="card-sub">Output Stream</p>
    <ul>
      <li>Streamed token by token to user interface</li>
      <li>Temperature & Top-P control sampling randomness</li>
      <li>Stops at end-of-sequence token or max limit</li>
    </ul>
  </div>
</div>

Note:
- Everything in LLMs is token-in, token-out
- Tokens ~ 4 characters / 0.75 words
- Show how temperature affects next-token probability distribution

---

<!-- .slide: class="deck-slide" -->

## Guardrails: Bringing Determinism to Probability

<div class="deck-concept-grid">
  <div class="deck-card">
    <div class="card-header"><span class="card-icon">🛡️</span><h3>Input Validation</h3></div>
    <p class="card-sub">Pre-Processing</p>
    <ul>
      <li>Filter toxic or unsafe prompts before LLM call</li>
      <li>Detect prompt injections & jailbreak attempts</li>
      <li>Enforce schema & domain boundaries</li>
    </ul>
  </div>

  <div class="deck-card highlight-card">
    <div class="card-header"><span class="card-icon">⚙️</span><h3>Structured Output</h3></div>
    <p class="card-sub">In-Flight Control</p>
    <ul>
      <li>Constrain decoding logits to force valid JSON/YAML</li>
      <li>JSON Schema enforcement at token level</li>
      <li>Guarantees machine-readable responses</li>
    </ul>
  </div>

  <div class="deck-card">
    <div class="card-header"><span class="card-icon">🔍</span><h3>Output Verification</h3></div>
    <p class="card-sub">Post-Processing</p>
    <ul>
      <li>Fact-check against grounded sources (RAG)</li>
      <li>Validate business logic with traditional code</li>
      <li>Fallback or retry loops on validation failure</li>
    </ul>
  </div>
</div>

Note:
- Guardrails bridge the gap between statistical LLMs and enterprise reliability
- Logit bias and constrained decoding guarantees 100% JSON valid syntax
- Python/Pydantic validation layer after LLM output


---

<!-- .slide: class="deck-slide" -->

## Continuous Model Evaluations

<div class="deck-concept-grid">
  <div class="deck-card">
    <div class="card-header"><span class="card-icon">🔄</span><h3>Model Evolution</h3></div>
    <p class="card-sub">Provider Updates</p>
    <ul>
      <li>Providers continuously update underlying weights and safety filters</li>
      <li>Same prompt can produce different answers over time</li>
      <li>Requires explicit snapshot pinning in production (e.g. gpt-4o-2024-08-06)</li>
    </ul>
  </div>

  <div class="deck-card">
    <div class="card-header"><span class="card-icon">⚠️</span><h3>Model Drift</h3></div>
    <p class="card-sub">Silent Failures</p>
    <ul>
      <li>System prompt changes silently alter reasoning & output format</li>
      <li>Format adherence can break across subtle API version bumps</li>
      <li>Hard to catch without automated assertion suites</li>
    </ul>
  </div>

  <div class="deck-card highlight-card">
    <div class="card-header"><span class="card-icon">📊</span><h3>Regression Testing</h3></div>
    <p class="card-sub">Continuous Eval</p>
    <ul>
      <li>Automated benchmarks run on every model version update</li>
      <li>Assertions on structured output format, tone, and accuracy</li>
      <li>Prevents regressions before shipping code to production</li>
    </ul>
  </div>
</div>

Note:
- Non-deterministic software requires continuous regression testing
- Discuss provider model deprecation schedules (e.g. gpt-4o updates)
- How to lock model snapshots (e.g. gpt-4o-2024-08-06)
---

<!-- .slide: class="deck-slide" -->

## Training & Fitting

<div class="deck-concept-grid">
  <div class="deck-card">
    <div class="card-header"><span class="card-icon">📉</span><h3>Underfitting</h3></div>
    <p class="card-sub">Too Simple</p>
    <ul>
      <li>Model fails to capture complex underlying patterns</li>
      <li>High error on both training and real data</li>
      <li>Result: Vague, generic, or inaccurate answers</li>
    </ul>
  </div>

  <div class="deck-card highlight-card">
    <div class="card-header"><span class="card-icon">🎯</span><h3>Optimal Fit</h3></div>
    <p class="card-sub">Generalization</p>
    <ul>
      <li>Captures true signal without memorizing noise</li>
      <li>Generalizes smoothly to unseen real-world prompts</li>
      <li>Balances accuracy, reasoning, and adaptability</li>
    </ul>
  </div>

  <div class="deck-card">
    <div class="card-header"><span class="card-icon">📈</span><h3>Overfitting</h3></div>
    <p class="card-sub">Memorized Noise</p>
    <ul>
      <li>Memorizes training data verbatim instead of learning concepts</li>
      <li>Performs perfectly on training set, fails on new prompts</li>
      <li>Common risk in small domain fine-tuning</li>
    </ul>
  </div>
</div>

Note:
- Explain pre-training vs. fine-tuning
- Why fine-tuning on small datasets often leads to overfitting
- Why RAG is often preferred over fine-tuning for factual knowledge

---

<!-- .slide: class="deck-slide" -->

## Automated Evaluations & Dataset Design

<div class="compare-grid">
  <div class="compare-card">
    <div class="card-header">
      <span class="card-icon">🔁</span>
      <h3>Cross-Validation</h3>
    </div>
    <p class="card-sub">Algorithm Tuning</p>
    <ul>
      <li>Splits data into multiple folds for iterative training</li>
      <li>Used to tune hyperparameters & model choice</li>
      <li>Measures statistical stability across data splits</li>
    </ul>
  </div>

  <div class="compare-vs">VS</div>

  <div class="compare-card highlight-card">
    <div class="card-header">
      <span class="card-icon">🔒</span>
      <h3>Golden Test Corpora</h3>
    </div>
    <p class="card-sub">LLM Production Benchmark</p>
    <ul>
      <li>Curated, held-out set of real-world representative prompts</li>
      <li>Never exposed to model prompts or developer tuning</li>
      <li>Evaluated via LLM-as-a-Judge or semantic similarity metrics</li>
    </ul>
  </div>
</div>

Note:
- Why classical k-fold cross-validation differs from LLM eval pipelines
- Creating a golden dataset (100-500 edge-case prompts)
- Using LLM-as-a-Judge (e.g. GPT-4 evaluating smaller model outputs)

---

<!-- .slide: class="deck-slide topic-title-only" -->

## AI Hype vs. Actual Capabilities & Limits

<div class="llm-diagram">
  <div class="llm-node">Expectations</div>
  <div class="llm-connector"></div>
  <div class="llm-node llm-node-core">Gartner Trough</div>
  <div class="llm-connector"></div>
  <div class="llm-node">Measurable ROI</div>
</div>

Note:
- Transition to Section 2: Moving past marketing noise to real business value
- Dissecting why initial GenAI excitement often hits execution hurdles
- Understanding how different company sizes navigate the transition

---

<!-- .slide: class="deck-slide" -->

## The Adoption Divide: Enterprise vs. Mid-Sized Firms

<div class="deck-concept-grid">
  <div class="deck-card">
    <div class="card-header"><span class="card-icon">🏢</span><h3>Dedicated AI Teams</h3></div>
    <p class="card-sub">Big Tech & Enterprise</p>
    <ul>
      <li>Custom MLOps, dedicated prompt engineers & fine-tuning labs</li>
      <li>In-house evaluation platforms & custom RAG infrastructure</li>
      <li>High capital expenditure to absorb experimental failures</li>
    </ul>
  </div>

  <div class="deck-card highlight-card">
    <div class="card-header"><span class="card-icon">🌱</span><h3>Agile Adaptation</h3></div>
    <p class="card-sub">SMEs & Specialized Firms</p>
    <ul>
      <li>Leverage turnkey SaaS, foundational APIs & MCP tools</li>
      <li>Focus on high-impact domain workflows rather than training models</li>
      <li>Higher ROI per dollar by embedding AI into existing tools</li>
    </ul>
  </div>

  <div class="deck-card">
    <div class="card-header"><span class="card-icon">⚡</span><h3>The Real Bottleneck</h3></div>
    <p class="card-sub">Domain Expertise</p>
    <ul>
      <li>Technical talent is expensive; domain context is irreplaceable</li>
      <li>SMEs win by pairing domain experts directly with AI tools</li>
      <li>Focus on process clarity over custom model development</li>
    </ul>
  </div>
</div>

Note:
- Large companies spend millions building bespoke AI platforms that often underdeliver
- Mid-sized firms can move faster using off-the-shelf APIs and strong domain knowledge
- AI capability is democratized; process integration is the real competitive moat

---

<!-- .slide: class="deck-slide" -->

## The Productivity Paradox: Why 95%+ of AI Pilots Stall

<div class="deck-concept-grid">
  <div class="deck-card">
    <div class="card-header"><span class="card-icon">📉</span><h3>The 95-97% Gap</h3></div>
    <p class="card-sub">MIT & Industry Studies</p>
    <ul>
      <li>MIT / NBER studies show ~95%+ of corporate AI initiatives fail to show measurable P&L gain</li>
      <li>Gartner: Generative AI entered the "Trough of Disillusionment" in 2025/2026</li>
      <li>Pilots succeed in demo mode, fail in messy production edge cases</li>
    </ul>
  </div>

  <div class="deck-card">
    <div class="card-header"><span class="card-icon">⚠️</span><h3>Root Causes</h3></div>
    <p class="card-sub">Where Deployments Break</p>
    <ul>
      <li><strong>"Chatbot Syndrome":</strong> Freeform text boxes without structured domain workflows</li>
      <li><strong>Dirty Grounding Data:</strong> Fragmented, outdated internal documents</li>
      <li><strong>Lack of Evals:</strong> No continuous testing suite to catch silent regressions</li>
    </ul>
  </div>

  <div class="deck-card highlight-card">
    <div class="card-header"><span class="card-icon">🚀</span><h3>The 5% Winners</h3></div>
    <p class="card-sub">What Works</p>
    <ul>
      <li>Narrow, highly scoped tasks (e.g. document extraction, drafting support)</li>
      <li>Deterministic guardrails + human expert review loops</li>
      <li>Deep integration into existing business software and workflows</li>
    </ul>
  </div>
</div>

Note:
- Reference MIT Sloan / NBER study on GenAI adoption and minimal macro productivity gains
- Reference Gartner's Hype Cycle placing GenAI in the Trough of Disillusionment
- Explain why "slapping a chatbot on top of sharepoint" produces zero ROI

---

<!-- .slide: class="deck-slide" -->

## 4 Practical Modes of AI Utilization

<div class="deck-concept-grid" style="grid-template-columns: repeat(2, 1fr); gap: 20px;">
  <div class="deck-card">
    <div class="card-header"><span class="card-icon">📦</span><h3>Data Products</h3></div>
    <p class="card-sub">Structured Information</p>
    <ul>
      <li>Converting unstructured PDFs/reports into validated database records</li>
      <li>Automated summarizing, entity extraction, and tagging</li>
    </ul>
  </div>

  <div class="deck-card highlight-card">
    <div class="card-header"><span class="card-icon">🤖</span><h3>Agentic Automation</h3></div>
    <p class="card-sub">Task Execution</p>
    <ul>
      <li>LLMs calling tools, APIs, and databases via MCP/skills</li>
      <li>Multi-step reasoning and automated error-correction loops</li>
    </ul>
  </div>

  <div class="deck-card highlight-card">
    <div class="card-header"><span class="card-icon">⚡</span><h3>"Vibe Coding" & Prototyping</h3></div>
    <p class="card-sub">Accelerated Creation</p>
    <ul>
      <li>Natural language to working code, scripts, or building simulation models</li>
      <li>Rapid 10x iteration for non-traditional developers</li>
    </ul>
  </div>

  <div class="deck-card">
    <div class="card-header"><span class="card-icon">🔎</span><h3>Human Reviewers</h3></div>
    <p class="card-sub">Quality Assurance</p>
    <ul>
      <li>AI generates 80% draft; domain experts verify and sign off</li>
      <li>Critical for high-stakes engineering, legal, or climate calculations</li>
    </ul>
  </div>
</div>

Note:
- Data products: Moving from raw chat to API JSON payloads
- Agentic workflows: Models performing tasks using tools, not just answering questions
- Vibe coding: AI as a partner for quick scripts and simulation automation
- Human-in-the-loop: Why domain reviewers remain essential for precision output

---

<!-- .slide: class="deck-slide topic-title-only" -->

## Different Model Types & Why They Matter

<div class="llm-diagram">
  <div class="llm-node">Frontier LLMs</div>
  <div class="llm-connector"></div>
  <div class="llm-node llm-node-core">SLMs & Distillation</div>
  <div class="llm-connector"></div>
  <div class="llm-node">Local & Hybrid</div>
</div>

Note:
- Section Intro: Why "one huge model for everything" is inefficient and costly
- Strategic choice of model size, latency, cost, and privacy boundaries

---

<!-- .slide: class="deck-slide" -->

## Specialized Model Architectures

<div class="deck-concept-grid">
  <div class="deck-card">
    <div class="card-header"><span class="card-icon">🔹</span><h3>SLMs (Small Models)</h3></div>
    <p class="card-sub">Efficiency & Speed</p>
    <ul>
      <li>1B - 8B parameter models (e.g. Phi-4, Llama-3B)</li>
      <li>Low memory footprint & sub-100ms inference times</li>
      <li>Ideal for classification, simple extraction & local edge devices</li>
    </ul>
  </div>

  <div class="deck-card highlight-card">
    <div class="card-header"><span class="card-icon">🧪</span><h3>Distilled Models</h3></div>
    <p class="card-sub">Knowledge Transfer</p>
    <ul>
      <li>Trained using synthetic outputs generated by frontier models (GPT-4/Claude)</li>
      <li>Transfers deep reasoning capabilities into a fraction of the parameter size</li>
      <li>High performance on specialized tasks at 10x lower cost</li>
    </ul>
  </div>

  <div class="deck-card highlight-card">
    <div class="card-header"><span class="card-icon">⚡</span><h3>Flash / Mini Models</h3></div>
    <p class="card-sub">High-Throughput APIs</p>
    <ul>
      <li>Cost-optimized cloud endpoints (e.g. GPT-4o-mini, Gemini Flash)</li>
      <li>Extremely fast multi-modal token processing</li>
      <li>Workhorse for high-volume background tasks & agent steps</li>
    </ul>
  </div>
</div>

Note:
- SLMs fit on phone/laptop GPUs or lightweight servers
- Distillation allows smaller models to mime frontier reasoning for specific prompts
- Flash models replace heavy models for 90% of routine corporate API tasks

---

<!-- .slide: class="deck-slide" -->

## Privacy, Speed & Hybrid Architectures

<div class="compare-grid">
  <div class="compare-card">
    <div class="card-header">
      <span class="card-icon">🔒</span>
      <h3>Local Models</h3>
    </div>
    <p class="card-sub">Data Sovereignty & On-Prem</p>
    <ul>
      <li>Run on local workstation GPUs or private cloud infrastructure (e.g. Ollama)</li>
      <li>Zero data leakage: confidential IP and client data never leave the perimeter</li>
      <li>Zero API costs & works offline, but bounded by local hardware limits</li>
    </ul>
  </div>

  <div class="compare-vs">VS</div>

  <div class="compare-card highlight-card">
    <div class="card-header">
      <span class="card-icon">🔀</span>
      <h3>Hybrid Orchestration</h3>
    </div>
    <p class="card-sub">Intelligent Routing</p>
    <ul>
      <li>Local SLMs perform PII redaction, intent routing & simple processing</li>
      <li>Escalates complex, non-sensitive tasks to Cloud Frontier models</li>
      <li>Balances data security, latency, and cloud API spend automatically</li>
    </ul>
  </div>
</div>

Note:
- Why local models (Llama 3, Mistral) are crucial for sensitive climate/engineering calculations
- Hybrid routers allow companies to get the speed/privacy of local + power of cloud
- PII scrubbing locally before sending data to cloud APIs

---

<!-- .slide: class="deck-slide topic-title-only" -->

## Core Values & Impacts of AI Adoption

<div class="llm-diagram">
  <div class="llm-node">Workflow Shift</div>
  <div class="llm-connector"></div>
  <div class="llm-node llm-node-core">Cognitive Friction</div>
  <div class="llm-connector"></div>
  <div class="llm-node">New Paradigms</div>
</div>

Note:
- Section Intro: Moving from pure technical specs to the human and organizational reality
- AI adoption changes how people work, think, and verify information every day

---

<!-- .slide: class="deck-slide" -->

## Practical Organizational Effects & Cognitive Friction

<div class="deck-concept-grid">
  <div class="deck-card">
    <div class="card-header"><span class="card-icon">🧠</span><h3>Mental Fatigue</h3></div>
    <p class="card-sub">The Reviewer's Burden</p>
    <ul>
      <li>Shifting from <em>creating</em> from scratch to <em>auditing & reviewing</em> AI output</li>
      <li>Verification fatigue: Spotting subtle hallucinations requires constant vigilance</li>
      <li>Cognitive load shifts from execution to critical evaluation</li>
    </ul>
  </div>

  <div class="deck-card">
    <div class="card-header"><span class="card-icon">⚡</span><h3>Velocity vs. Depth</h3></div>
    <p class="card-sub">Process Acceleration</p>
    <ul>
      <li>10x faster first drafts and prototyping across departments</li>
      <li>Risk of "shallow completion": shipping polished-looking work with unverified math</li>
      <li>Need for deliberate pauses and structured review checkpoints</li>
    </ul>
  </div>

  <div class="deck-card highlight-card">
    <div class="card-header"><span class="card-icon">🎯</span><h3>The Value Shift</h3></div>
    <p class="card-sub">Core Competency</p>
    <ul>
      <li>Commoditizing routine synthesis & boilerplate drafting</li>
      <li>Elevating domain judgement, problem framing, and original thinking</li>
      <li>The most valuable employee is no longer the fastest writer, but the best curator</li>
    </ul>
  </div>
</div>

Note:
- Address verification fatigue: It's harder to spot a 2% error in a 10-page AI report than to write it yourself
- Emphasize that speed without domain judgment creates liability
- Re-framing employee value: from manual output to high-level orchestration & critical auditing

---

<!-- .slide: class="deck-slide" -->

## Un-neglectable Paradigms: New World, New Rules

<div class="deck-concept-grid">
  <div class="deck-card highlight-card">
    <div class="card-header"><span class="card-icon">🔄</span><h3>Creation ➔ Curation</h3></div>
    <p class="card-sub">Shift in Working Mode</p>
    <ul>
      <li>Old rule: Value comes from manual document or code production</li>
      <li>New rule: Value comes from asking sharp questions and rigorous verification</li>
      <li>Where to look: Design reports, simulation scripts & pitch proposals</li>
    </ul>
  </div>

  <div class="deck-card">
    <div class="card-header"><span class="card-icon">💬</span><h3>Chat ➔ Embedded System</h3></div>
    <p class="card-sub">Interface Evolution</p>
    <ul>
      <li>Old rule: Opening a chatbot window to type standalone prompts</li>
      <li>New rule: AI baked directly into IDEs, CAD/BIM tools, and email pipelines</li>
      <li>Where to look: VS Code Copilot, CAD automation scripts & automated inbox triage</li>
    </ul>
  </div>

  <div class="deck-card">
    <div class="card-header"><span class="card-icon">📜</span><h3>Static Knowledge ➔ Live Context</h3></div>
    <p class="card-sub">Information Retrieval</p>
    <ul>
      <li>Old rule: Relying on remembered model facts or static PDF search</li>
      <li>New rule: Connecting models to live APIs, tools (MCP) & dynamic RAG databases</li>
      <li>Where to look: Real-time climate data feeds, building codes & internal wikis</li>
    </ul>
  </div>
</div>

Note:
- Give concrete examples of changed contexts:
  1. Engineering / Climate consulting: AI drafts simulation setup scripts, engineer verifies inputs/physics.
  2. Software / Tooling: Moving away from standalone ChatGPT tabs into Copilot / IDE inline edits.
  3. Knowledge Management: Moving from static file folders to MCP-connected search tools.

---

<!-- .slide: class="deck-slide topic-title-only" -->

## Risks & Tradeoffs of Unguided AI

<div class="llm-diagram">
  <div class="llm-node">Unchecked Usage</div>
  <div class="llm-connector"></div>
  <div class="llm-node llm-node-core">Process Blindspots</div>
  <div class="llm-connector"></div>
  <div class="llm-node">Output Risk</div>
</div>

Note:
- Section Intro: Where AI is currently creeping into workflows without deliberate governance
- What happens when automation replaces critical thinking in design and calculations

---

<!-- .slide: class="deck-slide" -->

## Risks & Tradeoffs in Practice

<div class="deck-concept-grid">
  <div class="deck-card">
    <div class="card-header"><span class="card-icon">📍</span><h3>Where AI Creeps In</h3></div>
    <p class="card-sub">Active Use Cases</p>
    <ul>
      <li>Drafting technical client emails, summaries & proposals</li>
      <li>Writing simulation scripts (Python/EnergyPlus/Grasshopper)</li>
      <li>Synthesizing building codes & environmental standards</li>
    </ul>
  </div>

  <div class="deck-card highlight-card">
    <div class="card-header"><span class="card-icon">⚠️</span><h3>Unthinking Adoption</h3></div>
    <p class="card-sub">Process Blindspots</p>
    <ul>
      <li>"Plausible Nonsense": High linguistic fluency masking flawed physics</li>
      <li>Skipping sanity checks on units, climate assumptions, or boundary conditions</li>
      <li>Atrophy of foundational problem-solving skills in junior engineers</li>
    </ul>
  </div>

  <div class="deck-card">
    <div class="card-header"><span class="card-icon">🛡️</span><h3>Deliberate Guardrails</h3></div>
    <p class="card-sub">Mitigation Strategy</p>
    <ul>
      <li>Define mandatory human verification checkpoints for calculation scripts</li>
      <li>Separate text drafting (creative) from numerical reasoning (deterministic)</li>
      <li>Treat AI output as an unverified junior draft, never final authority</li>
    </ul>
  </div>
</div>

Note:
- Highlight the danger of "smooth prose" deceiving non-experts or tired reviewers
- Discuss the risk of junior engineers relying on AI before understanding underlying physics
- Establish clear rules: AI never signs off on client deliverables or engineering math without verification

---

<!-- .slide: class="deck-slide topic-title-only" -->

## Data Sovereignty & Digital Dependency

<div class="llm-diagram">
  <div class="llm-node">IP Leakage</div>
  <div class="llm-connector"></div>
  <div class="llm-node llm-node-core">Vendor Lock-In</div>
  <div class="llm-connector"></div>
  <div class="llm-node">Sovereign Control</div>
</div>

Note:
- Section Intro: Who owns your data, where does it flow, and how vulnerable is your company to cloud provider lock-in?

---

<!-- .slide: class="deck-slide" -->

## Data Sovereignty & Digital Dependency

<div class="compare-grid">
  <div class="compare-card">
    <div class="card-header">
      <span class="card-icon">🔐</span>
      <h3>Data Sovereignty</h3>
    </div>
    <p class="card-sub">The Vulnerability</p>
    <ul>
      <li>Sending client IP, building schematics & NDA data to US cloud APIs</li>
      <li>Risk of training data contamination or accidental public leakage</li>
      <li>Regulatory compliance breaches (GDPR, EU AI Act, strict client NDAs)</li>
    </ul>
  </div>

  <div class="compare-vs">&</div>

  <div class="compare-card highlight-card">
    <div class="card-header">
      <span class="card-icon">🔗</span>
      <h3>Digital Dependency</h3>
    </div>
    <p class="card-sub">The Trap & Remediation</p>
    <ul>
      <li>Over-reliance on closed API ecosystems (OpenAI/Anthropic price & policy changes)</li>
      <li><strong>Actionable steps:</strong> Zero-retention enterprise agreements, open-weights (Llama/Mistral) fallback, and local models for sensitive IP</li>
    </ul>
  </div>
</div>

Note:
- Differentiate public ChatGPT (trains on data) vs Enterprise API (zero-retention guarantees)
- Explain digital dependency: what happens if an API doubles its price or changes safety filters overnight?
- Solution: Multi-model strategy, local open-source models, and explicit API contracts

---

<!-- .slide: class="deck-slide topic-title-only" -->

## Carbon Footprint & Sustainability Considerations

<div class="llm-diagram">
  <div class="llm-node">Data Center Water & Energy</div>
  <div class="llm-connector"></div>
  <div class="llm-node llm-node-core">Environmental Cost</div>
  <div class="llm-connector"></div>
  <div class="llm-node">Sustainable AI Practices</div>
</div>

Note:
- Section Intro: How a climate-focused engineering firm like Transsolar evaluates the carbon footprint of AI
- Balancing AI energy consumption against the efficiency gains in sustainable building design

---

<!-- .slide: class="deck-slide" -->

## Environmental Impact & Sustainable AI Usage

<div class="deck-concept-grid">
  <div class="deck-card">
    <div class="card-header"><span class="card-icon">📊</span><h3>The Scale of Impact</h3></div>
    <p class="card-sub">Current Estimates</p>
    <ul>
      <li>Single query: ~0.3 - 3 Wh (10x higher energy & water cooling than Google Search)</li>
      <li>Training a frontier model: 100s of MWh + tons of $CO_2$ equivalent</li>
      <li>Data center grid strain: Rapidly expanding footprint in energy grids worldwide</li>
    </ul>
  </div>

  <div class="deck-card highlight-card">
    <div class="card-header"><span class="card-icon">⚖️</span><h3>The Net Climate ROI</h3></div>
    <p class="card-sub">Transsolar Perspective</p>
    <ul>
      <li>High carbon cost for trivial uses (e.g. generating casual images/chat)</li>
      <li><strong>Net Positive ROI:</strong> Using AI to optimize building energy models, saving megawatts over a building's 50-year lifecycle</li>
      <li>Use intensity must match value created</li>
    </ul>
  </div>

  <div class="deck-card">
    <div class="card-header"><span class="card-icon">🌱</span><h3>What We Can Do</h3></div>
    <p class="card-sub">Practical Action</p>
    <ul>
      <li>Right-size models: Use SLMs / Flash models instead of GPT-4 for simple tasks</li>
      <li>Cache frequent queries & avoid repetitive prompt re-computations</li>
      <li>Be intentional: Don't burn kWh on queries easily solved with a 1-line script</li>
    </ul>
  </div>
</div>

Note:
- Put numbers into perspective: a ChatGPT query consumes roughly 10x the power of a standard web search
- Frame the paradox for Transsolar: AI consumes carbon, but if it helps design a zero-carbon building 5% faster, the net climate ROI is huge
- Actionable advice: Model right-sizing (SLMs consume 90% less power than massive frontier models)

---

<!-- .slide: class="deck-slide topic-title-only" -->

## Part 2: Practical Concepts

### AI is a Tool — Understand & Guide It Deliberately

<div class="llm-diagram">
  <div class="llm-node">Understand Mechanics</div>
  <div class="llm-connector"></div>
  <div class="llm-node llm-node-core">Master Prompting & Context</div>
  <div class="llm-connector"></div>
  <div class="llm-node">Unlock Real Impact</div>
</div>

Note:
- Transition from Part 1 (Theory, limits, risks) to Part 2 (Hands-on mechanics and practical usage)
- Takeaway message: AI is not magic or a substitute for thinking. It is a powerful tool that yields exponential returns when guided deliberately.

---

<!-- .slide: class="deck-slide" -->

## Prompt Length & Rich Context

<div class="compare-grid">
  <div class="compare-card">
    <div class="card-header">
      <span class="card-icon">🔍</span>
      <h3>LLMs Are Not Search Engines</h3>
    </div>
    <p class="card-sub">The Keyword Trap</p>
    <ul>
      <li>Google prefers short keywords: "climate simulation thermal comfort"</li>
      <li>LLMs struggle with vague 3-word prompts because they sample from generic probability distributions</li>
      <li>Short prompts produce generic, average responses</li>
    </ul>
  </div>

  <div class="compare-vs">VS</div>

  <div class="compare-card highlight-card">
    <div class="card-header">
      <span class="card-icon">📄</span>
      <h3>Context-Rich Grounding</h3>
    </div>
    <p class="card-sub">The Rich Prompt</p>
    <ul>
      <li>Provide background, constraints, format preferences & goal: "Act as a building simulation engineer..."</li>
      <li>More context reduces search space & eliminates model assumptions</li>
      <li>Rule of thumb: Never assume model knowledge — explicitly feed relevant facts</li>
    </ul>
  </div>
</div>

Note:
- Contrast search engine habits vs LLM prompting habits
- Explain why "lazy prompts" lead to hallucination or generic advice
- Feed project constraints, climate parameters, and formatting expectations directly into the context window

---

<!-- .slide: class="deck-slide" -->

## Zero-Shot vs. Multi-Shot Prompting

<div class="compare-grid">
  <div class="compare-card">
    <div class="card-header">
      <span class="card-icon">🎯</span>
      <h3>Zero-Shot Prompting</h3>
    </div>
    <p class="card-sub">Direct Instruction</p>
    <ul>
      <li>Asking the model to complete a task without prior examples</li>
      <li>Relies entirely on pre-trained statistical weights</li>
      <li>Good for simple, common tasks; unreliable for strict custom formats</li>
    </ul>
  </div>

  <div class="compare-vs">VS</div>

  <div class="compare-card highlight-card">
    <div class="card-header">
      <span class="card-icon">💡</span>
      <h3>Few-Shot / Multi-Shot</h3>
    </div>
    <p class="card-sub">In-Context Examples</p>
    <ul>
      <li>Providing 2-3 concrete Input ➔ Output pairs directly in the prompt</li>
      <li>Dramatically anchors style, tone, schema, and edge-case handling</li>
      <li>One good example is worth 500 words of complex instruction!</li>
    </ul>
  </div>
</div>

Note:
- Show how examples act as strong statistical anchors
- If you want JSON, markdown tables, or specific engineering report formats, show the model 1-2 completed examples
- In-context learning vs fine-tuning

---

<!-- .slide: class="deck-slide" -->

## System Prompts & Core Instructions

<div class="deck-concept-grid">
  <div class="deck-card">
    <div class="card-header"><span class="card-icon">👑</span><h3>What Is a System Prompt?</h3></div>
    <p class="card-sub">Conceptual Foundation</p>
    <ul>
      <li>Persistent, high-priority instruction layer preceding user messages</li>
      <li>Establishes the AI's persona, role, boundaries, and default tone</li>
      <li>Invisible to end-users in packaged tools and custom chatbots</li>
    </ul>
  </div>

  <div class="deck-card highlight-card">
    <div class="card-header"><span class="card-icon">📋</span><h3>What Belongs There?</h3></div>
    <p class="card-sub">Essential Components</p>
    <ul>
      <li><strong>Role & Domain:</strong> "You are a senior climate consultant..."</li>
      <li><strong>Rules & Guardrails:</strong> "Never invent physics values. State missing data."</li>
      <li><strong>Output Schema:</strong> Enforce structured JSON, bullet points, or language</li>
    </ul>
  </div>

  <div class="deck-card">
    <div class="card-header"><span class="card-icon">⚡</span><h3>Why It Matters</h3></div>
    <p class="card-sub">Control & Quality</p>
    <ul>
      <li>Prevents repetitive instruction in every user turn</li>
      <li>Maintains consistent output quality across team members</li>
      <li>Forms the backbone of enterprise AI apps & agent systems</li>
    </ul>
  </div>
</div>

Note:
- Differentiate System Prompt (Developer / Config layer) vs User Prompt (Ad-hoc input)
- Explain how system prompts enforce safety, tone, and domain alignment
- In tools like Copilot or ChatGPT GPTs, system prompts define the agent behavior

---

<!-- .slide: class="deck-slide" -->

## Controlling Randomness: Temperature & Style

<div class="deck-concept-grid">
  <div class="deck-card">
    <div class="card-header"><span class="card-icon">🎯</span><h3>Temperature = 0.0</h3></div>
    <p class="card-sub">Deterministic & Focused</p>
    <ul>
      <li>Model always picks the highest-probability next token</li>
      <li>Consistent, reproducible outputs with minimal variance</li>
      <li><strong>Best for:</strong> Math, code generation, JSON extraction, factual summaries</li>
    </ul>
  </div>

  <div class="deck-card highlight-card">
    <div class="card-header"><span class="card-icon">🎛️</span><h3>Temperature = 0.7</h3></div>
    <p class="card-sub">Balanced Reasoning</p>
    <ul>
      <li>Samples from top plausible tokens with moderate variety</li>
      <li>Default setting for general conversation, writing & analysis</li>
      <li><strong>Best for:</strong> Technical drafting, brainstorming, email synthesis</li>
    </ul>
  </div>

  <div class="deck-card">
    <div class="card-header"><span class="card-icon">🎨</span><h3>Temperature = 1.0+</h3></div>
    <p class="card-sub">Creative & Random</p>
    <ul>
      <li>Flattens token probability curve, allowing low-probability tokens</li>
      <li>High diversity, novel phrasing, but increased risk of hallucinations</li>
      <li><strong>Best for:</strong> Creative ideation, design naming, alternative concepts</li>
    </ul>
  </div>
</div>

Note:
- Explain token sampling distributions and logit manipulation
- Practical rule: Set temp to 0 for code/data extraction; 0.7 for general writing; 1.0 for creative brainstorming
- Using statistical randomness intentionally rather than by accident

---

<!-- .slide: class="deck-slide" -->

## Extending Memory: Knowledge Bases & RAG

<div class="deck-concept-grid">
  <div class="deck-card">
    <div class="card-header"><span class="card-icon">🧱</span><h3>The Problem</h3></div>
    <p class="card-sub">Model Limits</p>
    <ul>
      <li>Models don't know your private files, recent building codes, or project specs</li>
      <li>Context windows are limited & expensive to fill with entire PDF archives</li>
      <li>Training cutoffs leave models blind to current data</li>
    </ul>
  </div>

  <div class="deck-card highlight-card">
    <div class="card-header"><span class="card-icon">🔎</span><h3>RAG Architecture</h3></div>
    <p class="card-sub">Retrieval-Augmented Generation</p>
    <ul>
      <li>1. Search internal document database for relevant text chunks</li>
      <li>2. Inject top matching chunks into the prompt context automatically</li>
      <li>3. Model generates answer <em>grounded</em> strictly in retrieved source text</li>
    </ul>
  </div>

  <div class="deck-card">
    <div class="card-header"><span class="card-icon">✨</span><h3>Key Benefits</h3></div>
    <p class="card-sub">Enterprise Factuality</p>
    <ul>
      <li>Zero fine-tuning required; instant updates when documents change</li>
      <li>Provides clickable citations back to source PDF pages</li>
      <li>Prevents hallucinations by enforcing grounded sources</li>
    </ul>
  </div>
</div>

Note:
- RAG = Search + In-context prompting
- Explain why RAG is the enterprise standard for document search (SharePoint, building standards, internal wikis)
- Citations and auditability for engineering work

---

<!-- .slide: class="deck-slide" -->

## Live Action: Skills, Tools, MCP & Memory

<div class="deck-concept-grid">
  <div class="deck-card">
    <div class="card-header"><span class="card-icon">🛠️</span><h3>Tools & Function Calling</h3></div>
    <p class="card-sub">Deterministic Action</p>
    <ul>
      <li>Model outputs structured tool calls (e.g., `run_simulation(city='Munich')`)</li>
      <li>External code executes calculation & returns exact numbers to LLM</li>
      <li>Bridges statistical LLMs with deterministic calculators & APIs</li>
    </ul>
  </div>

  <div class="deck-card highlight-card">
    <div class="card-header"><span class="card-icon">🔌</span><h3>MCP (Model Context Protocol)</h3></div>
    <p class="card-sub">Open Integration Standard</p>
    <ul>
      <li>Universal protocol connecting AI clients to local files, databases & APIs</li>
      <li>Allows Copilot/Chatbots to query live weather data, BIM models, or SQL</li>
      <li>Eliminates custom API glue code for every tool</li>
    </ul>
  </div>

  <div class="deck-card">
    <div class="card-header"><span class="card-icon">🧠</span><h3>Agent Memory & State</h3></div>
    <p class="card-sub">Long-Term Context</p>
    <ul>
      <li>Storing user preferences, project conventions, and past decisions across chats</li>
      <li>Enables human-in-the-loop workflows with continuous context</li>
      <li>Reduces setup overhead for recurring daily tasks</li>
    </ul>
  </div>
</div>

Note:
- Differentiate RAG (reading documents) vs Tools/MCP (doing actions & querying live systems)
- Model Context Protocol (MCP) as the open standard for tool integration
- Human-in-the-loop validation during agent execution

---

<!-- .slide: class="deck-slide" -->

## Mapping Concepts to Your Daily Tools

<div class="compare-grid">
  <div class="compare-card">
    <div class="card-header">
      <span class="card-icon">🖥️</span>
      <h3>Where These Show Up</h3>
    </div>
    <p class="card-sub">Everyday AI Applications</p>
    <ul>
      <li><strong>Copilot / IDEs:</strong> System prompts + live workspace RAG + MCP tools (terminal/git)</li>
      <li><strong>Chatbots (ChatGPT/Claude):</strong> System prompts + web search tools + file attachments</li>
      <li><strong>Studio Apps (Azure/OpenAI):</strong> Temperature control + custom system instructions + RAG indices</li>
    </ul>
  </div>

  <div class="compare-vs">💬</div>

  <div class="compare-card highlight-card">
    <div class="card-header">
      <span class="card-icon">🙋‍♂️</span>
      <h3>Audience Check-In</h3>
    </div>
    <p class="card-sub">What Tools Do You Use?</p>
    <ul>
      <li>Which AI tools do you use daily in your Transsolar workflow?</li>
      <li><em>(ChatGPT, GitHub Copilot, Claude, Rhino/Grasshopper plugins, custom Python scripts...)</em></li>
      <li>Let's map these exact mechanics to your specific environment!</li>
    </ul>
  </div>
</div>

Note:
- Interactive audience question to transition into Q&A or live demo
- Ask participants to name their daily tools so we can map system prompts, temperature, RAG, and tools directly to their work
- Bridge into section 3 (Q&A) and section 4 (Mini workshop demo)
