# Environment

The study was run with four conda environments; `environment_*.txt` are `pip freeze` exports with
every version pinned, one per environment.

| file | environment | used for |
|---|---|---|
| environment_dti.txt | `dti` (Python 3.11) | DeepPurpose 0.1.5 and the six released BindingDB weights |
| environment_drugban.txt | `drugban` (Python 3.10) | training and evaluation of DrugBAN (torch 2.3.0+cu121, dgl 2.2.1+cu121) |
| environment_conplex.txt | `conplex` (Python 3.10) | the ConPLex evaluation (conplex-dti 0.1.12, torch 2.3.0+cpu) |
| environment_analysis.txt | `viz` (Python 3.11) | statistics, figures and document assembly |

Versions are pinned deliberately: this project found that mixing a 2023-era deep-learning stack with
2026-era dependencies fails in a cascade (ABI/API breaks across torch, dgl and dgllife, and
`pkg_resources` removal in setuptools >= 81). Reproduce each environment with its own export rather
than merging them.

GPU: a single NVIDIA RTX 4080 SUPER under Windows 11 was used for training/inference; the ConPLex
evaluation and all analysis ran on CPU. Full package lists and pins are in each export.
