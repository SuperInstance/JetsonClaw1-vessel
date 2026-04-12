# JetsonClaw1 Knowledge Journal

## For Other Agents Reading This

This journal captures decisions, lessons, and context that future agents (vessels) need to be productive in this fleet. Think of it as the ship log.

## Session 2026-04-11: The Full Steam Build

### What Happened
Casey said "do it" and "keep good records" and "push often" and "pull out all the stops and use your full abilities" and "turn your academic work into actualized work" and "journal your knowledge for other agents."

### The Pipeline We Discovered Works
1. GLM-5 writes a paper identifying a gap in the FLUX architecture
2. GLM-5 builds a Rust crate implementing the solution
3. Tests verify the implementation
4. Push to GitHub with cross-pollination references
5. Other vessels can read the MAINTENANCE.md to understand context

This pipeline turns academic exploration into actualized code in ~5 minutes per crate.

### Architecture Lessons

#### Why Memory-Mapped Ports (Not New Opcodes)
The ISA is full (0x00-0xFF). Adding new opcodes would break backward compatibility. Solution: reserve the top 4KB of the 64KB address space for typed capability ports. OP_LOAD/OP_STORE become the universal interface. Zero new opcodes, unlimited extensibility.

#### Why Confidence is Optional (Not Default)
The Think Tank decided: default confidence propagation creates zombie values in dead code paths. An agent that never checks confidence still pays the CPU cost of tracking it. Solution: CONF_ variants at 0x70-0x79, StripConf wrapper for speed.

#### Why Trust Has 5 Dimensions (Not 1)
Single-value trust is too coarse. A vessel can be competent but selfish, or honest but unreliable. The fleet needs competence, integrity, benevolence, identity, and longevity as separate scores. See hermes-trust-ecology.md for the full design.

#### Why Ethics is Software (Not Opcodes)
We can't add opcodes for ethics — which opcodes would "do no harm" be? Ethics is a constraint LAYER that reads VM state and vetoes actions. The cuda-ethics crate sits above the VM, not inside it.

### Mathematical Constants Worth Knowing

#### Decay Half-Lives
- conf_decay rate 0.99: half-life = 69.3 cycles (confidence persists for ~70 decisions)
- conf_decay rate 0.95: half-life = 13.5 cycles (rapid forgetting)
- instinct_decay rate 0.95: half-life = 13.5 activations (habituation in ~14 exposures)
- trust_decay: variable, depends on interaction frequency

#### Critical Thresholds
- Confidence floor for action: 0.10 (below this, agent should defer/escalate)
- Reflex bypass: intensity > 0.80 (skips deliberation, acts immediately)
- Convergence rate: 25% toward group mean (balances individuality vs herd)
- Extinction: intensity < 10/255 (prune instinct)
- Energy critical: 92% RAM, 0% swap (brothers-keeper triggers)
- Energy warning: 85% RAM (brothers-keeper alerts)

#### Bayesian Fusion Formula
P(A|B) = P(B|A) * P(A) / P(B)
In fleet terms: confidence_fused = evidence_strength * prior_confidence / base_rate
This is CONF_FUSE (0x74). Use when you have EVIDENCE. Use CONF_MERGE (0x73) when you have RECENCY.

#### Energy Economics
- ATP market: double auction (buyers bid max, sellers ask min, clear at intersection)
- Circadian modulation: rate * (0.5 + 0.5 * cos(2pi * phase))
- Pool contribution incentive: ln(1 + amount) — diminishing returns prevents gaming
- Crisis rationing: cap withdrawals to sqrt(available / vessels) when pressure > 0.8

### File Organization Conventions

#### In Each Crate
- MAINTENANCE.md: Architecture decisions, math formulas, related crates, "why this exists"
- ARCHITECTURE.md: Detailed design with diagrams
- CHANGELOG.md: Version history with rationale
- src/lib.rs: Core types with inline math comments
- README.md: Build/test/run instructions, API docs, cross-pollination refs
- The Deeper Connection: Every README ends with a paragraph about fleet-as-organism

#### In FLUX VM
- flux_vm.c: Main VM (switch dispatch) — the canonical implementation
- flux_vm_cg.c: Computed goto variant — 4.3x faster on ARM64
- format_size(): CRITICAL function — determines encoding byte length for each opcode
- Do NOT add new sections to format_size() — replace the entire function to avoid shadowing

### Commit Message Convention

Good commit messages capture the THINKING, not just the change:
- "add: memory-mapped capability ports" — WHAT
- "The ISA is full (0x00-0xFF). Rather than break backward compatibility, we reserve the top 4KB of address space for typed I/O ports. OP_LOAD/OP_STORE become universal. Zero new opcodes needed." — WHY

### PR Comment Convention

PR comments are "inner thoughts frozen in time." They should answer:
1. What was I thinking when I wrote this?
2. What would confuse someone reading this for the first time?
3. What are the non-obvious constraints?
4. What breaks if this changes?

### API Status (2026-04-11)
- DeepInfra (jc1 exclusive): Hermes-405B, phi-4, Hermes-70B — all working
- DeepSeek: EXPIRED
- SiliconFlow: EXPIRED (shared with Oracle1)
- z.ai GLM-5: Working (via OpenClaw subagents)

### Key Repos for New Vessels
- Start here: flux-runtime-c (understand the VM)
- Then: cuda-instruction-set (understand the ISA)
- Then: cuda-confidence-math (understand confidence math)
- Then: cuda-atp-market (understand energy economy)
- Then: cuda-ethics (understand constraint layer)
- Explore: fleet-benchmarks/docs/creative-explorations/ (read the thinking)
- Explore: opcode-philosophy/docs/ (read the philosophy)
