# DTI-EP experiment - open-box DTI models and within-target affinity ranking

Public code and data release accompanying the manuscript

> **Open-box drug-target interaction models do not reproduce within-target affinity ranking on a
> 57-target benchmark**

Repository: https://github.com/hankpotter1/DTI-EP_Experiment

This archive contains everything needed to inspect and re-run the analyses behind every table and
figure of the manuscript: the evaluation sets, the per-pair model predictions, the analysis script
chain, the figure sources, the reference implementation of the trivial-baseline protocol, the
provenance record, and the pinned environments.

## What the study does (one paragraph)

We assembled a 57-target benchmark (seven target classes, 25 ChEMBL-derived ligands per target,
1,425 target-ligand pairs, 1,409 unique molecules) and evaluated three codebases: six publicly
released DeepPurpose BindingDB weights, two DrugBAN checkpoints trained by us from the authors'
code, and the publicly released ConPLex (2023) weights - nine evaluation objects in total.
Within-target affinity ranking is uniformly weak (median per-target Spearman correlation 0.012-0.194;
the independent ConPLex architecture reaches 0.055), the benchmark lies largely inside the models'
BindingDB ChEMBL source corpus (>=76% of pairs), and heavy-atom count alone ranks as well as the best
model (0.191 versus 0.194). All nine evaluation objects fail a strict trivial-baseline criterion.

## Contents

| path | what it is |
|---|---|
| `data.zip` | Minimal data set: the three evaluation sets (`bench_eval_set_*.csv`) and the per-pair model predictions (`predictions_*.csv`, `drugban_predictions*.csv`), the ConPLex evaluation products (`minimal_data_set/conplex_eval*.json`, `conplex/conplex_results.tsv`), the analysis outputs behind every table and figure, and `conplex/README_conplex.md` (how ProtBert was obtained). |
| `scripts.zip` | The analysis script chain: benchmark construction and evaluation, overlap quantification (BindingDB/Davis/DrugBAN), statistical correction, the ConPLex evaluation, and figure generation. |
| `figures.zip` | Main figures 1-13 as PNG and vector PDF, named by the manuscript figure number. |
| `tools.zip` | Reference implementation of the trivial-baseline protocol proposed in the manuscript, with its README. |
| `provenance.zip` | Literature-verification trail for the references (including ConPLex) and the journal-selection survey. |
| `environment/` | `pip freeze` exports (pinned) for the four conda environments used, plus their README. |
| `LICENSE` | MIT for code, CC BY 4.0 for data (pending author confirmation). |

Each archive contains a `MANIFEST.txt` with the size and SHA-256 of every member.

## Data sources

All inputs are public; no new experimental data were generated.

- **ChEMBL** bioactivity data (target and ligand records from which the evaluation sets were built).
  References 7 and 8 of the manuscript (doi:10.1093/nar/gkad1004; doi:10.1093/nar/gkr777).
- **BindingDB** and its ChEMBL subset, used to quantify (target, ligand) overlap with the models'
  source corpus (doi:10.1093/nar/gkv1072; doi:10.1093/nar/gkl999).
- **Davis et al.** kinase selectivity panel, used as the in-domain positive control (doi:10.1038/nbt.1990).
- **DeepPurpose** toolkit and the six publicly released BindingDB pretrained weights
  (doi:10.1093/bioinformatics/btaa1005).
- **DrugBAN** source code, used to train the two checkpoints evaluated here
  (doi:10.1038/s42256-022-00605-1).
- **ConPLex** source code and the `ConPLex_v1_BindingDB` weights (doi:10.1073/pnas.2220778120), plus
  its publicly available ProtBert protein-language-model dependency (see `conplex/README_conplex.md`).

This repository redistributes none of these third-party assets except the ConPLex per-pair
predictions produced by us.

## How to reproduce

1. Create the environments from `environment/` (see `environment/README.md`). Do not merge them.
2. Obtain the public data and model weights from the sources listed above (and
   `conplex/README_conplex.md` for ProtBert).
3. Unpack `data.zip` and `scripts.zip` side by side. The scripts read `results/`-style paths; edit the
   `ROOT`/path constants at the top of each script to your checkout.
4. Run the numbered scripts in order (`96_build_bench_eval_set.py` ... `133_eval_conplex.py`), then the
   figure scripts. Each writes JSON/CSV into a local `results/` directory.

The trivial-baseline protocol is in `tools.zip`; `trivial_baseline.py` is self-testing.

## Citation

If you use this release, please cite the manuscript (title above) and this repository (https://github.com/hankpotter1/DTI-EP_Experiment).
No archival DOI has been minted for this repository.

## What is deliberately NOT here

On a whitelist basis the following are excluded: the download cache (`cache/`); all logs and
transient/intermediate dumps; `thirdparty/` (redistributed third-party model weights and extracted
third-party source trees - only the ConPLex evaluation products are included, under `data.zip`);
the journal submission packages (`submission_v*/`); and every file carrying credentials, keys or
private data. In particular the 1.6 GB ProtBert weights are not redistributed.

## Notes and limitations of this release

- Two author-input items that concern the manuscript remain open: the ChEMBL release number queried,
  and a repository identifier/commit for DeepPurpose, DrugBAN and ConPLex. They are flagged in the
  submission declarations.
- The analysis scripts are published as run, including the authors' local path constants; they are not
  rewritten to be path-portable.
