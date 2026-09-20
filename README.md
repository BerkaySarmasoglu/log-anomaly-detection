# Log Anomaly Detection — HDFS_v1

Block-level anomaly detection on the HDFS_v1 distributed-systems log dataset, comparing a
bag-of-events baseline against a sequence-preserving LSTM under a leakage-aware chronological
split and a one-time, pre-registered sealed test evaluation.

Full methodology, findings, and limitations: **[final_report.md](final_report.md)**

## Result

| | PR-AUC | Best-F1 | Precision | Recall |
|---|---|---|---|---|
| Validation (overall, blended) | 0.6386 | 0.7403 | 0.9557 | 0.6041 |
| Validation (non-truncated) | 1.0000 | 0.9990 | 1.0000 | 0.9979 |
| **Sealed test** (final, one-time) | **0.9960** | **0.9924** | **0.9946** | **0.9901** |

The sealed test result is evaluated against the correct baseline (validation's non-truncated
subset, not the blended overall figure — see `final_report.md` Section 8 for why the naive
comparison is misleading) and shows a small, expected generalization gap.

## Repository structure

```
log-anomaly-detection/
├── data/
│   ├── raw/           HDFS.log, anomaly_label.csv (gitignored)
│   ├── interim/        split boundaries, block lifespans, Drain dictionary, parsed events
│   ├── processed/       bag-of-events and sequence feature artifacts
│   └── reference/      quarantined publisher artifacts, used only for post-hoc sanity checks
├── notebooks/
│   ├── 00_calibration.ipynb            raw data integrity, schema, encoding
│   ├── 01_split_boundary_audit.ipynb    label-blind chronological split, censoring policy
│   ├── 02_log_parsing.ipynb            Drain template dictionary, frozen-artifact discipline
│   ├── 03_eda.ipynb                    train+val-only exploratory analysis
│   ├── 04_feature_engineering.ipynb    bag-of-events and sequence construction, all splits
│   ├── 05_modeling.ipynb               baseline vs. LSTM, validation-only comparison
│   └── 06_sealed_evaluation.ipynb      pre-registered, one-time sealed test evaluation
├── configs/
│   └── config.yaml                     all resolved hyperparameters, paths, split strategy
├── figures/                             LOG_-prefixed exported figures
├── outputs/
│   ├── models/                          persisted fitted models and frozen artifacts
│   └── metrics/                         persisted validation and sealed test metrics
├── requirements.txt
└── final_report.md
```

## Reproducing this project

```bash
git clone https://github.com/BerkaySarmasoglu/log-anomaly-detection.git
cd log-anomaly-detection
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
```

Download `HDFS_v1.zip` from [Zenodo record 8196385](https://zenodo.org/records/8196385) and
place `HDFS.log` and `anomaly_label.csv` under `data/raw/`. Run the notebooks in order,
`00` through `06`; each notebook's `Findings` sections document the exact evidence behind
every methodological decision, and each depends only on artifacts persisted by the notebooks
before it.

## Methodological discipline

- **Chronological, block-count split** (70/15/15), with an explicit, measured censoring
  policy for blocks whose lifespan crosses a split boundary — not assumed safe, checked.
- **Frozen-artifact discipline**: the Drain template dictionary is fit on train+validation
  only and applied unmodified to test, mirroring standard fit-on-train-only practice.
- **Sealed test evaluation**: the evaluation plan (model, decision threshold, metric set) is
  written down and locked *before* a single test label is loaded, and never revised
  afterward.
- Every notebook records what it got wrong along the way — an unpersisted artifact, an
  incorrect claim in an earlier notebook's findings, an initial diagnostic that itself needed
  correcting — rather than presenting only a cleaned-up final version.

## Dataset and citation

HDFS_v1, from the [Loghub](https://github.com/logpai/loghub) collection, obtained from
[Zenodo record 8196385](https://zenodo.org/records/8196385).

If referencing the underlying dataset:

- Wei Xu, Ling Huang, Armando Fox, David Patterson, Michael Jordan. *Detecting Large-Scale
  System Problems by Mining Console Logs*, SOSP 2009.
- Jieming Zhu, Shilin He, Pinjia He, Jinyang Liu, Michael R. Lyu. *Loghub: A Large Collection
  of System Log Datasets for AI-driven Log Analytics*, ISSRE 2023.

