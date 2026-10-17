# Beyond Accuracy@k: How Localization Quality Shapes Repository-Level LLM Program Repair

**Lab 5 — Scoping a Grounded Research Gap in LLM4APR** (Week 05)<br>
Deliverable: a 2-page Research Gap Statement and 3 formulated Research Questions.

**Full statement (PDF):** [`lab5-gap-statement.pdf`](lab5-gap-statement.pdf)<br>
**Author:** Juan Javier Samuel Z (东万) · L25020002

---

## Selected Bottleneck

**Localization**, meaning finding *where* to edit before a patch is written.

- MAGIS [2] finds that "locating lines is a key factor for GitHub issue resolution", and that changes touching more files and functions are significantly less likely to be resolved.
- The LLM4APR SLR [1] states that "fault localization is a precondition of patch generation" (§9.3.2).
- SHERLOC [7] reports that repair agents spend about half of their budget on localization.

## The Gap

Repository-level localizers (Agentless [10], LocAgent [4], CoSIL [5], SweRank [6], SHERLOC [7]) are evaluated mainly with retrieval metrics (Acc@k, Recall@k). Their value for repair is reported only as end-to-end gains. Loc2Repair [8] compares just three fixed conditions: no localization, predicted, and gold. Function-level studies (RepairAgent [11], D4C [12], Campos et al. [13]) compare only perfect against automated localization on single-function Java bugs.

For **repository-level issues**, three questions remain open:

1. **Precision vs. recall.** In SWE-bench [3], as analyzed by MAGIS [2], raising file recall from 29.58% to 51.06% *lowered* Claude-2's resolved ratio from 1.96% to 1.22%. No study varies localization precision and recall systematically.
2. **Interaction with change complexity.** Multi-file changes are harder [2], and some issues remain unresolved even with gold localization [8]. It is unknown whether localization quality matters more for multi-file fixes.
3. **Genuine correctness.** Strengthened tests reject 19.71% of previously passing patches from the top-30 SWE-bench Verified agents [9]. It is unknown whether better localization yields correct patches or merely *plausible* ones.

## Research Questions

| RQ | Independent variable(s) | Dependent variable(s) |
|---|---|---|
| **RQ1.** How do the precision and recall of the file-level localization given to an LLM repair agent affect its resolved rate and cost on repository-level issues? | Localization precision {1.0, 0.5, 0.25} (via added distractor files) × recall {1.0, ≈0.5} (keeping half of the gold files, rounded up; exact value logged per instance). Recall is varied only on multi-file instances, since a single-file instance allows recall of only 0 or 1. *Controlled:* backbone LLM, scaffold, prompt, instance set. | Resolved rate (%); input/output tokens and USD cost per instance. |
| **RQ2.** Does the effect of localization quality on repair success depend on the complexity of the required change? | Localization condition {none, predicted (e.g., CoSIL), gold}. *Moderator:* gold-patch complexity (single- vs. multi-file; number of hunks). | Resolved rate, analyzed by logistic regression with an IV × complexity interaction term. |
| **RQ3.** Does diagnostic localization (location plus a natural-language fault explanation) produce more genuinely correct patches than location-only output? | Localization output form {file path; file + function; diagnostic finding}. | Resolved rate on original tests; correctness rate under strengthened tests; overfitting rate (pass original tests, fail strengthened ones). |

## Proposed Study (Sketch)

- **Benchmark:** SWE-bench Verified, using a stratified subset of instances to bound cost.
- **Held fixed:** backbone LLM (open-weight, e.g., Qwen2.5-Coder-32B), agent scaffold, prompt template.
- **Manipulated:** the localized context the agent receives (distractor files added to the gold file set, or gold files removed from it) and the output form of localization.
- **Analysis:** resolved rate and cost per instance; logistic regression with a complexity interaction term, following MAGIS; overfitting rate under strengthened tests (SWE-ABS) or manual validation of a sample.

## References

1. Q. Zhang et al. *A Systematic Literature Review on Large Language Models for Automated Program Repair.* ACM TOSEM, 2026. [arXiv:2405.01466](https://arxiv.org/abs/2405.01466)
2. W. Tao et al. *MAGIS: LLM-Based Multi-Agent Framework for GitHub Issue Resolution.* NeurIPS, 2024. [arXiv:2403.17927](https://arxiv.org/abs/2403.17927)
3. C. E. Jimenez et al. *SWE-bench: Can Language Models Resolve Real-World GitHub Issues?* ICLR, 2024. [arXiv:2310.06770](https://arxiv.org/abs/2310.06770)
4. Z. Chen et al. *LocAgent: Graph-Guided LLM Agents for Code Localization.* ACL, 2025. [arXiv:2503.09089](https://arxiv.org/abs/2503.09089)
5. Z. Jiang et al. *Issue Localization via LLM-Driven Iterative Code Graph Searching* (CoSIL). ASE, 2025. [arXiv:2503.22424](https://arxiv.org/abs/2503.22424)
6. R. G. Reddy et al. *SweRank: Software Issue Localization with Code Ranking.* ICLR, 2026. [arXiv:2505.07849](https://arxiv.org/abs/2505.07849)
7. H. Tamoyan et al. *SHERLOC: Structured Diagnostic Localization for Code Repair Agents.* 2026. [arXiv:2606.24820](https://arxiv.org/abs/2606.24820)
8. M. N. Al Awad, S. Ivanov. *Loc2Repair: A Framework for Evaluating the Impact of File-Level Issue Localization in Repo-Level LLM Repair.* GeCoIn @ IJCAI-ECAI, 2026. [arXiv:2606.30963](https://arxiv.org/abs/2606.30963)
9. B. Yu et al. *SWE-ABS: Adversarial Benchmark Strengthening Exposes Inflated Success Rates on Test-based Benchmark.* 2026. [arXiv:2603.00520](https://arxiv.org/abs/2603.00520)
10. C. S. Xia et al. *Agentless: Demystifying LLM-based Software Engineering Agents.* 2024. [arXiv:2407.01489](https://arxiv.org/abs/2407.01489)
11. I. Bouzenia, P. Devanbu, M. Pradel. *RepairAgent: An Autonomous, LLM-Based Agent for Program Repair.* 2024. [arXiv:2403.17134](https://arxiv.org/abs/2403.17134)
12. J. Xu et al. *Aligning the Objective of LLM-based Program Repair* (D4C). ICSE, 2025. [arXiv:2404.08877](https://arxiv.org/abs/2404.08877)
13. V. Campos et al. *Exploring Generalizable Automated Program Repair with Large Language Models.* 2025. [arXiv:2506.03283](https://arxiv.org/abs/2506.03283)
14. B. Yang et al. *A Survey of LLM-based Automated Program Repair: Taxonomies, Design Paradigms, and Applications.* 2025. [arXiv:2506.23749](https://arxiv.org/abs/2506.23749)

## Repository Contents

```
.
├── README.md                 # this overview: gap summary and the 3 RQs
└── lab5-gap-statement.pdf    # 2-page Research Gap Statement (deliverable)
```
