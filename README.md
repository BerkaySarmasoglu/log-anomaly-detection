# Log-Based Anomaly Detection on Distributed System Logs (HDFS)

## Overview
This project implements and evaluates a sequence-aware anomaly detection
pipeline on the HDFS_v1 dataset from Loghub. Two modeling approaches are
compared under an identical chronological, leakage-audited evaluation
protocol: a bag-of-events baseline and a template-sequence LSTM classifier.

## Problem Statement
[To be completed after Phase 0: precise formulation of the block-level
classification task, class balance characteristics, and evaluation
objective.]

## Dataset
- Source: HDFS_v1, Loghub collection.
- Attribution: Xu, W., Huang, L., Fox, A., Patterson, D., Jordan, M.
  "Detecting Large-Scale System Problems by Mining Console Logs", SOSP 2009.
  Zhu, J., He, S., He, P., Liu, J., Lyu, M. R. "Loghub: A Large Collection
  of System Log Datasets for AI-driven Log Analytics", ISSRE 2023.
- Scale: 11,175,629 log lines; 575,061 labeled blocks; 16,838 anomalous
  blocks (~2.93% positive rate).

## Methodology
1. Calibration — raw schema and structural integrity audit.
2. Split Boundary Audit — chronological cutoff determination with
   block-level boundary integrity verification, performed without label
   access.
3. Log Parsing — template extraction via Drain (logparser), fit on
   train+val only, applied as a frozen artifact to the test partition.
4. Exploratory Data Analysis — restricted to train+val.
5. Feature Engineering — bag-of-events and template-sequence
   representations.
6. Modeling — baseline classifier vs. sequence LSTM.
7. Sealed Evaluation — single-pass evaluation on the held-out test
   partition.

## Repository Structure
[To be completed once the notebook sequence stabilizes.]

## Setup
[To be completed: environment instructions, dependency installation.]

## Results
[Populated only after sealed evaluation is complete. No metric is
published prior to Phase 6.]

## License and Attribution
[Dataset usage terms and citation requirements per Loghub's stated
attribution policy.]