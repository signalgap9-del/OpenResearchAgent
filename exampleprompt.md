you can use it in google antigravity or claude code


**🔥 OpenResearchAgent: Prompt Engineering Master Guide**


## 🧠 Core Principle: The 5C Framework

| Principle | Meaning | Research Application |
|-----------|---------|---------------------|
| **Context** | Context | "This project is Month 3, Attention Ablation in progress" |
| **Constraint** | Constraint | "Don't touch src/, only experiments/" |
| **Chain** | Chain | "1.Analyze → 2.Design → 3.Implement → 4.Verify" |
| **Concrete** | Concrete | "Implement Figure 3's left subplot" (ambiguity prohibited) |
| **Check** | Check | "Check if it violates interfaces/" |

---

## 🚀 Practical Prompt Templates

### Template 1: Initial Paper Implementation (Genesis Phase)

```markdown
[System]
You are a machine learning research engineer. Create a project following this structure:
- src/: Core algorithms (stable)
- experiments/: Experiments (iterative)
- interfaces/: Abstraction layer (do not modify)
- docs/: Decision records

[User]
Implement Figure 1 (Transformer Architecture) from the paper "Attention Is All You Need".

Requirements:
1. Implement in src/domain_core/models/transformer.py
2. Must inherit from interfaces/base_model.py (see attachments)
3. Include hyperparameters in configs/model/transformer_base.yaml
4. Record at least 3 design decisions in docs/00_blueprint/ (why implemented this way)

Prohibitions:
- Do not create experiments/ directory yet (Month 1 phase)
- Exclude data loader (modeling only)

Output Format:
1. Code blocks by file path
2. Explanation of how each file connects to interfaces/
3. Hints on how to extend from experiments/ in the future
```

**💡 Key Point**: 
- Providing **interface files as attachments** prevents AI from violating "contracts"
- **Phase specification** (Month 1) prevents premature experiment folder creation
- Requesting **extension hints** ensures future compatibility

---

### Template 2: Experiment Addition (Evolution Phase)

```markdown
[System]
Current project is Month 3. Existing src/ is stabilized, conducting Ablation Study in experiments/.

[Context Provided]
- Existing model location: src/domain_core/models/transformer.py (see attachment)
- Existing config: configs/model/transformer_base.yaml (see attachment)
- Previous experiments: experiments/00_baseline/ (see attachment)

[Task]
Create experiments/10_ablation_no_positional_encoding/.

Specific Requirements:
1. Do not modify src/transformer.py; override via modified_embedding.py
2. Implement version without positional encoding
3. Create configs/experiments/10_ablation_no_pos.yaml:
   - inherit: 00_baseline
   - override: model.embedding_layer = modified_embedding
4. Write hypothesis.md:
   - Hypothesis: "Can self-attention learn sequence order without positional encoding?"
   - Validation: Compare BLEU scores

[RPG-style Constraint]
- STOP and ask if attempting to modify src/
- modified_*.py must inherit from existing classes
- Use existing functions from evaluation/metrics/ for analysis
```

**💡 Key Point**:
- **Attachments** of existing code prevent hallucination
- **Inheritance enforcement** prevents interface violations
- **RPG Incremental Update simulation**: Explicit "STOP on src/ modification"

---

### Template 3: Debugging (Debug Phase)

```markdown
[Problem Context]
OOM (Out of Memory) occurring in experiments/20_extension_multimodal/.

[Relevant Files Attached]
- experiments/20_extension_multimodal/main.py (error at line 45)
- src/infrastructure/data/loader.py (data loading)
- configs/experiments/20_extension_multimodal.yaml (batch_size: 256)

[Task]
1. First, suggest gradual batch_size reduction strategy in configs/ (down to 32)
2. If src/ must be modified, check interfaces/ violation
3. Suggest adding memory profiling code (torch.cuda.memory_summary)

[Chain of Thought Required]
Think step by step:
1. Analyze OOM cause (data vs model)
2. experiments/ level solution (config adjustment)
3. src/ level solution (last resort)
4. Trade-off analysis for each solution

[Output Format]
- Immediately applicable code (within experiments/ only)
- If src/ modification needed, mark separately with interfaces/ checklist
```

**💡 Key Point**:
- **Layered debugging**: Force order experiments/ → src/
- **Chain of Thought enforcement** prevents impulsive src/ modifications
- **Trade-off analysis** documents research decisions

---

### Template 4: Refactoring (Refactoring Phase)

```markdown
[Refactoring Request]
Want to split src/domain_core/models/layers/attention.py into smaller modules.

[Safety Guards]
1. First check method signatures in interfaces/base_model.py
2. Analyze if following experiments use this module:
   - experiments/00_baseline/
   - experiments/10_ablation_attention/
   - experiments/20_extension_*/

[Step-by-Step Approach]
Phase 1 (Safe): Analyze if internal structure can change without modifying interfaces/
Phase 2 (Warning): If interfaces/ change needed, write migration guide
Phase 3 (Danger): If breaking change, generate list of experiments/ to modify

[Self-Consistency Check]
After writing, verify:
- [ ] Compatible with existing configs/?
- [ ] Does experiments/00_baseline/ still work?
- [ ] Is new structure recorded in docs/01_decisions/?
```

**💡 Key Point**:
- **3-level danger classification** makes AI approach carefully
- **Self-Consistency Checklist** for AI self-verification
- **Migration guide request** enforces documentation

---

### Template 5: Paper Writing/Review (Documentation Phase)

```markdown
[Knowledge Extraction]
Convert current project state into paper Method section.

[Sources]
- src/domain_core/models/ (final implementation)
- experiments/ (experimental history)
- docs/01_decisions/ (design decisions)

[Task]
1. Create docs/paper_draft/method_section.md
2. Summarize motivation for each experiment (00_, 10_, 20_) in one sentence
3. Create mapping table showing how implementation corresponds to paper Figures/Tables

[RPG-aware Query]
- Find feature description of "src/training/optimizers/adamw_custom.py" and include why custom optimizer was needed
- Generate ablation study table based on experiments/10_ablation/ results

[Format]
- LaTeX format (method.tex)
- Quote actual code snippets (3+ lines) with citations
```

**💡 Key Point**:
- **Code→Paper reverse transformation**: Utilizes RPG-Encoder's "Comprehension" approach
- **Feature Description citation**: Clearly describes implementation intent
- **Mapping table** ensures reproducibility

---

## 🎯 Advanced Technique: "RPG-Style Context Injection"

### Usage in AntiGravity/Cursor

```markdown
[Persistent Memory Simulation]
"In previous conversations, we designed src/ like this: [summary]
Current task is experiments/10_ablation/. 
Remember we're doing this without modifying src/?"

[Graph Traversal Command]
"Find where src/domain_core/models/transformer.py's forward method is called from in experiments/10_ablation/ files
(dependency graph traversal)"
```

### Self-Correction Prompt

```markdown
[Guardrail]
"Before writing, pause: Check if this modification violates abstract methods in interfaces/base_model.py.
If violation, STOP and notify me."
```

---

## 📋 Pre-Flight Checklist: Before Sending Prompt

- [ ] **Phase specified**: Month 1? Month 3? (AI determines folder creation location)
- [ ] **Attachments**: interfaces/, existing configs/ attached?
- [ ] **Forbidden zones**: Explicitly stated "Don't touch src/"?
- [ ] **Output format**: Code blocks? YAML? Markdown? Clear?
- [ ] **Verification step**: Includes self-correction like "Check interfaces/"?

---
