

# OpenResearchAgent: Design Proposal for Sustainable Research Code Management

[![DOI](https://zenodo.org/badge/1149225835.svg)](https://doi.org/10.5281/zenodo.18475586)

Contact:https://x.com/GapSigOfficial

## 1. Problem Analysis

### 1.1 Limitations of Current Approaches

DeepCode (Li et al., 2025) demonstrates state-of-the-art performance in paper-to-code generation (73.5% replication score on PaperBench). However, its architecture reveals fundamental constraints for long-term research projects:

- **CodeMem Limitations**: The stateful memory mechanism compresses code into natural language summaries. While this prevents context saturation during generation, it introduces irreversible information loss regarding fine-grained dependencies and implementation-specific constraints.
- **Static Blueprint**: DeepCode treats the implementation blueprint as a static artifact generated at initialization. Research code, however, requires continuous evolution where specifications emerge incrementally through experimentation.

### 1.2 Information Loss in Maintenance

As repository scale increases, DeepCode's text-based memory leads to:
- Semantic drift between original paper specifications and evolved code
- Inability to track cross-file dependencies during refactoring
- Repetition of implementation patterns due to lack of structural indexing

## 2. Proposed Solution

### 2.1 Architectural Integration

This proposal suggests integrating DeepCode with RPG-Encoder (Luo et al., 2026) to form a closed-loop system:

```
Phase 1 (Generation): DeepCode
Paper → Hierarchical Segmentation → Blueprint → Code

Phase 2 (Maintenance): RPG-Encoder  
Code → Repository Planning Graph → Incremental Updates → Maintained Code
```

RPG-Encoder provides the inverse mapping (Code → Intent) that DeepCode lacks, enabling bidirectional traceability between specifications and implementations.

### 2.2 Repository Planning Graph (RPG)

The RPG serves as the canonical intermediate representation, defined as a heterogeneous graph G = (V, E) where:

- **Node Structure**: Each node v ∈ V contains:
  - Semantic feature f: Atomic, implementation-agnostic behavior descriptions (e.g., "initialize optimizer with learning rate 1e-3")
  - Metadata m: Structural attributes (type, path, signatures)
  
- **Edge Structure**: Dual-view connectivity:
  - E_feature: Functional hierarchy (teleological relationships)
  - E_dep: Execution dependencies (imports, calls, inheritance)

This structure preserves both "what the code does" (semantic) and "how it executes" (structural) without compression loss.

### 2.3 Operational Mechanisms

**Initial Encoding**:
1. DeepCode generates initial repository from paper
2. RPG-Encoder extracts graph via semantic lifting:
   - Parse functions/classes into low-level nodes
   - Abstract functional centroids via hierarchical aggregation
   - Ground artifacts to directory scopes using LCA computation

**Evolution Protocol**:
When adding new experiments or refactoring:
1. Parse code diffs to identify affected entities
2. Execute atomic updates:
   - Deletions: Recursive pruning of orphaned nodes
   - Modifications: In-place update if semantic drift < τ; re-routing otherwise
   - Additions: Semantic routing to insert nodes at appropriate hierarchy levels
3. Localized dependency refresh (O(Δ) complexity vs O(N) full rebuild)

**Navigation Interface**:
- SearchNode: Intent-based retrieval using semantic features
- ExploreRPG: Graph traversal along functional or dependency edges
- FetchNode: Precise source retrieval with metadata


## 4. Implementation Considerations

### 4.1 Integration Points

- **Schema Alignment**: DeepCode's implementation blueprint must map to RPG's functional hierarchy (three-level path format: functional_area/category/subcategory)
- **Memory Synchronization**: Replace CodeMem's text summaries with RPG node references during generation phase
- **Update Triggers**: Commit-level differential updates when researcher modifies code

### 4.2 Technical Challenges

1. **Semantic Drift Threshold (τ)**: Determining optimal threshold for triggering node re-routing vs in-place updates in research contexts (vs. production software)
2. **Scale**: RPG-Encoder demonstrates linear complexity; verification required for repositories exceeding 100K LOC with rapid experimentation cycles
3. **Multi-modal Inputs**: Handling not only code but also figures, tables, and equations from papers within the graph structure


## 5. Architecutral design

<img width="1536" height="1024" alt="ChatGPT Image 2026년 2월 4일 오전 09_03_23" src="https://github.com/user-attachments/assets/014986c9-b20c-407d-af52-dd3eae079b8d" />


## 5. Conclusion

The proposed system addresses the asymmetry in current research agents: while DeepCode effectively maps Paper → Code (generation), it lacks the inverse Code → Paper mapping (comprehension) necessary for maintenance. RPG-Encoder provides this missing component through its dual-view graph representation.

The integration enables:
- Preservation of design intent through semantic features
- Maintenance of structural integrity through dependency edges  
- Sustainable evolution through incremental updates (95.7% cost reduction)

This closed-loop approach transforms research code generation from an episodic task into a continuously manageable process.

## References

- Li, Z., et al. (2025). DeepCode: Open Agentic Coding. arXiv:2512.07921v1 [cs.SE].
- Luo, J., et al. (2026). Closing the Loop: Universal Repository Representation with RPG-Encoder. arXiv:2602.02084v1 [cs.CL].
