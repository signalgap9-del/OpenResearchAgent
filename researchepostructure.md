# Scalable Research Repository Template for DeepCode + RPG-Encoder

> **Optimized repository structure** for LLM-based research agents combining DeepCode (initial generation) and RPG-Encoder (long-term maintenance).


<img width="1725" height="1210" alt="다운로드 (2)" src="https://github.com/user-attachments/assets/459caf16-ecd4-488d-a5a3-182d4176c4bb" />


## Directory Structure

```text
{research-project}/
│
├── src/                                    # [STABLE CORE] High cost of change
│   ├── domain_core/                        # Functional Area: Core Logic
│   │   ├── models/                         # Category: Model Architectures
│   │   │   ├── base.py                     # Abstract interface (immutable)
│   │   │   ├── {model_a}.py                # Specific implementations
│   │   │   └── {model_b}.py
│   │   ├── tasks/                          # Category: Problem Definitions
│   │   │   ├── prediction.py
│   │   │   └── optimization.py
│   │   └── metrics/                        # Category: Evaluation Criteria
│   │       ├── standard_metrics.py
│   │       └── domain_specific_metric.py   # e.g., custom loss functions
│   │
│   ├── methods/                            # Functional Area: Methodology
│   │   ├── training/                       # Category: Learning Algorithms
│   │   │   ├── loops.py                    # Training epoch control
│   │   │   └── optimizers.py               # Custom optimization strategies
│   │   ├── inference/                      # Category: Deployment/Testing
│   │   │   └── pipelines.py                # Inference pipelines
│   │   └── analysis/                       # Category: Post-hoc Analysis
│   │       └── visualizers.py              # Result visualization tools
│   │
│   └── infrastructure/                     # Functional Area: Technical Infra
│       ├── data/                           # Category: Data Engineering
│       │   ├── loaders.py                  # Data I/O abstraction
│       │   └── preprocessors.py            # Data transformations
│       ├── logging/                        # Category: Observability
│       └── distributed/                    # Category: Distributed Computing (optional)
│
├── experiments/                            # [ITERATIVE LAYER] RPG-managed iterations
│   ├── 00_baseline/                        # Foundation experiment (paper reproduction)
│   │   ├── main.py                         # Entry point
│   │   ├── config.yaml                     # Experiment-specific overrides
│   │   └── hypothesis.md                   # Research hypothesis and rationale
│   │
│   ├── 10_ablation_{component}/            # Component removal studies
│   │   ├── main.py                         # Reuses src/, modifies specific parts
│   │   ├── modified_{component}.py         # RPG tracks as "Modification"
│   │   └── diff.patch                      # Auto-generated dependency diff
│   │
│   ├── 20_extension_{capability}/          # New capability additions
│   │   ├── main.py                         # New functionality implementation
│   │   └── new_module.py                   # RPG tracks as "Addition"
│   │
│   ├── 30_application_{scenario}/          # Domain adaptation experiments
│   │   └── main.py                         # Transfer to new datasets/domains
│   │
│   └── 90_archive/                         # Deprecated experiments
│       ├── .rpg_ignore                     # Exclusion marker for RPG indexing
│       └── failed_experiment_01/           # Failed attempts (kept for records)
│
├── configs/                                # [COMPOSITION LAYER] Declarative configs
│   ├── model/                              # Architecture specifications
│   │   ├── small.yaml
│   │   └── large.yaml
│   ├── method/                             # Algorithmic specifications
│   │   ├── supervised.yaml
│   │   └── self_supervised.yaml
│   ├── data/                               # Dataset specifications
│   │   ├── train_config.yaml
│   │   └── val_config.yaml
│   └── experiments/                        # Composable experiment recipes
│       ├── 00_baseline.yaml                # model: small + method: supervised
│       ├── 10_ablation_attention.yaml      # inherits: 00_baseline + removes: attention
│       └── 20_extension_multimodal.yaml    # inherits: 00_baseline + adds: new_encoder
│
├── interfaces/                             # [CONTRACT LAYER] Stable APIs
│   ├── base_model.py                       # Abstract base class (RPG anchor node)
│   ├── base_trainer.py                     # Training interface
│   └── base_dataset.py                     # Data interface
│
└── docs/                                   # [KNOWLEDGE GRAPH] RPG-indexed documentation
    ├── 00_blueprint/                       # DeepCode initial outputs (static)
    │   ├── architecture.md                 # System architecture from paper
    │   └── algorithms.md                   # Algorithm specifications
    ├── 01_decisions/                       # Architecture Decision Records (ADRs)
    │   ├── 001_why_transformer.md
    │   └── 002_dataset_selection.md
    ├── 02_experiments/                     # Experiment results summaries
    │   └── 00_baseline_results.md
    └── 03_domain/                          # Domain terminology glossary
        └── glossary.md                     # Domain-specific terms for RPG search
```

## Why This Structure?

### 1. Optimized for DeepCode (Initial Generation Phase)

**`docs/00_blueprint/`**: Stores DeepCode's Concept Agent and Algorithm Agent outputs. These structured documents serve as the persistent "source of truth" that survives code evolution, preventing the "specification drift" problem.

**`src/` vs `experiments/` separation**: DeepCode excels at generating the core `src/` directory from papers (one-time high-fidelity generation). However, it struggles with iterative experimental modifications. This separation isolates DeepCode's generated core from iterative changes.

**`configs/experiments/`**: Uses YAML-based declarative configuration instead of scattered command-line arguments. DeepCode's hierarchical content segmentation parses structured configs more accurately than imperative Python code.

### 2. Optimized for RPG-Encoder (Maintenance Phase)

**Prefix-Based Organization (`00_`, `10_`, `20_`, `90_`)**: Maps directly to RPG-Encoder's three atomic update protocols:
- **`00_`**: Stable nodes (rarely modified, foundation reference)
- **`10_/20_`**: Subject to RPG "Modification" and "Addition" updates
- **`90_`**: Target for RPG "Deletion" updates (pruning)

**Configuration Inheritance**: By using `inherit` + `override` patterns in configs, RPG-Encoder stores only the diff in node metadata rather than full configurations. This achieves the **95.7% maintenance cost reduction** demonstrated in the RPG-Encoder paper.

**`.rpg_ignore`**: Explicit marker for RPG-Encoder's "Node Deletion with Structural Hygiene" algorithm. When experiments move to `90_archive/`, this file prevents graph bloat by excluding them from the active RPG index while preserving historical records.

**`modified_{component}.py` Pattern**: Enables RPG-Encoder's **Semantic Drift Detection**. If modifications change the semantic `feature` description significantly (threshold τ), RPG triggers re-routing; otherwise, it performs efficient in-place updates.

## Scalability Principles

### 1. The Closed Loop (Paper → Code → Paper)

```
DeepCode (Forward):   Paper → Blueprint → src/ (Generation/Compression)
RPG-Encoder (Inverse): experiments/ → RPG Graph → Knowledge (Comprehension/Expansion)
```

- **`src/`**: Compressed intent (what the paper specifies)
- **`experiments/`**: Expanded implementation (empirical discoveries)
- **`docs/01_decisions/`**: The bridge (rationale for deviations)

### 2. Incremental Update Efficiency

The structure decouples maintenance cost from repository scale:

- **Initial Encoding**: Full RPG extraction from `src/` (one-time cost)
- **Experiment Addition**: O(Δ) updates (only new nodes and edges)
- **Configuration Changes**: Diff-based metadata updates (not full file re-indexing)
- **Archival**: O(1) pruning via `.rpg_ignore` (not O(N) deletion)

### 3. Semantic Navigation

RPG-Encoder's `SearchNode` and `ExploreRPG` tools operate efficiently:

- **`docs/03_domain/glossary.md`**: Provides domain vocabulary for intent-based search
- **`interfaces/`**: Creates stable anchor points for dependency traversal
- **`hypothesis.md`**: Stores semantic context for each experiment (RPG `feature` field source)

## Usage Workflow

**Month 1**: DeepCode generates `src/` and `docs/00_blueprint/` from the research paper. RPG-Encoder performs initial graph extraction.

**Month 3**: Add ablation study in `10_ablation_attention/`. RPG-Encoder:
1. Detects `modified_attention.py` as "Modification"
2. Checks semantic drift against `src/domain_core/models/attention.py`
3. Updates dependency edges if interfaces remain stable
4. Cost: ~633K tokens (vs. 14.7M for full rebuild)

**Month 6**: Add extension in `20_extension_multimodal/`. RPG-Encoder:
1. Detects new files as "Addition"
2. Performs semantic routing to place nodes in correct hierarchy
3. Links to existing `src/domain_core/` nodes via dependency analysis

**Month 12**: Archive failed experiments to `90_archive/`. RPG-Encoder:
1. Reads `.rpg_ignore`
2. Removes subtree from active graph (search space management)
3. Preserves files for reproducibility without indexing overhead
