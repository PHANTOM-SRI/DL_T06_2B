# DL_T06_2B – Romanized Telugu–English Film Review Sentiment (Task 2B, Team 6)

MGIT · Department of Emerging Technologies · Deep Learning

## Honest status
- **Data is synthetic and rule-labelled.** No human-labelled Romanized Telugu corpus was used; no inter-annotator agreement was measured. Scores describe this corpus only.
- **Strategy B (English IMDB transfer): EXECUTED.** Source used: Pang & Lee polarity corpus v2.0 (IMDb-sourced; 1,600 train / 400 test). The Keras IMDB download was unreachable in the execution environment, so this smaller IMDb-sourced English corpus substituted for it.
- Student names, contributions and academic year in the report are placeholders for the team to fill in.
- **Unseen-data test:** a separate synthetic challenge set (900 reviews, every review has tokens never seen in training). Novel slang/English words were written without native-speaker review. Selection used a separate unseen-validation set, never the test sets.
- The sequence-length sweep (L = 128) was run on the baseline models before robustness techniques were added and was reused for them.

## Contents
| File | Purpose |
|---|---|
| `DL_T06_2B.ipynb` | Executed notebook (data generation → preprocessing → length sweep → models → evaluation → CPU benchmark → checklist) |
| `telugu_romanized_labelled_SYNTHETIC.csv` | Labelled synthetic corpus (6000 reviews) |
| `cpu_benchmark_pool_10000_SYNTHETIC.csv` | 10,000 distinct reviews for CPU timing |
| `ANNOTATION_GUIDE.md`, `DATA_CARD.md` | Label rules; dataset documentation |
| `sequence_length_sweep.csv`, `configuration_comparison.csv`, `test_all_runs.csv` | Experiment tables |
| `cpu_benchmark_10000.csv`, `verification_checklist.csv` | Benchmark and requirement check |
| `unseen_validation_SYNTHETIC.csv`, `unseen_test_SYNTHETIC.csv` | Unseen-data challenge sets (600 / 900 reviews) |
| `selection_scores_validation.csv`, `unseen_error_analysis.csv`, `unseen_confusion_matrix.csv`, `unseen_test_errors.csv` | Model-selection scores; unseen-data error analysis |
| `error_analysis_categories.csv`, `test_errors_selected_model.csv`, `confusion_matrix_selected_test.csv` | Error analysis |
| `best_sentiment_model.keras`, `tokenizer.pkl`, `label_encoder.pkl`, `config.json` | Selected model and preprocessing |
| `DL_T06_2B_Report.pdf` / `.docx` | Project report |

## How to run
Open the notebook in Colab or Jupyter and run all cells (TensorFlow 2.x). With internet access the IMDB source models also train. Seeds are fixed. A CPU-only run takes roughly one hour (single core).

## Method summary
Max length chosen on validation Macro F1 from {64, 128, 192, 256} → **128**. SimpleRNN/LSTM/GRU × 3 seeds for two variants: **A** baseline scratch and **A2** scratch + noise augmentation + inference-time spelling normalisation. Selection score = mean of in-distribution validation Macro F1 and unseen-data validation Macro F1 (no test data); both test sets are evaluated once.

## Results (mean over 3 seeds; in-distribution test vs unseen-data test, both synthetic)
| Strategy             | Model     |   Selection score (mean) |   Test Macro F1 (mean) |   Test Mixed F1 (mean) |   OOD Test Macro F1 (mean) |   OOD Test Macro F1 (sd) |   OOD Test Mixed F1 (mean) |
|:---------------------|:----------|-------------------------:|-----------------------:|-----------------------:|---------------------------:|-------------------------:|---------------------------:|
| A2: Scratch + robust | LSTM      |                   0.8686 |                 0.9972 |                 0.9982 |                     0.8052 |                   0.0092 |                     0.847  |
| A2: Scratch + robust | SimpleRNN |                   0.8336 |                 0.9763 |                 0.9727 |                     0.7503 |                   0.0358 |                     0.7753 |
| A2: Scratch + robust | GRU       |                   0.8193 |                 0.9493 |                 0.9905 |                     0.731  |                   0.0437 |                     0.8091 |
| B: IMDB transfer     | GRU       |                   0.7945 |                 0.9996 |                 1      |                     0.6077 |                   0.017  |                     0.5779 |
| A: From scratch      | GRU       |                   0.7922 |                 0.9964 |                 1      |                     0.6208 |                   0.0166 |                     0.5811 |
| B: IMDB transfer     | SimpleRNN |                   0.7895 |                 0.9854 |                 0.9881 |                     0.61   |                   0.022  |                     0.5851 |
| A: From scratch      | LSTM      |                   0.7873 |                 0.9988 |                 0.9994 |                     0.6057 |                   0.0172 |                     0.5677 |
| B: IMDB transfer     | LSTM      |                   0.7847 |                 0.9996 |                 1      |                     0.5989 |                   0.0145 |                     0.58   |
| A: From scratch      | SimpleRNN |                   0.7766 |                 0.9855 |                 0.9916 |                     0.6071 |                   0.0193 |                     0.5936 |

Selected: **A2: Scratch + robust / LSTM** (seed 42). CPU, 10,000 distinct reviews incl. preprocessing: **3.2 s** (3124.6 reviews/s).

## Requirement check
| Requirement                                                  | Status                       | Evidence                                                                                               |
|:-------------------------------------------------------------|:-----------------------------|:-------------------------------------------------------------------------------------------------------|
| Romanized Telugu-English corpus (no Telugu script)           | ACHIEVED                     | Telugu-script reviews: 0                                                                               |
| Genuine human-labelled data                                  | NOT ACHIEVED                 | All labels are rule-generated; no human annotation performed                                           |
| Annotation guide and data card                               | ACHIEVED                     | Delivered as separate files (see ZIP)                                                                  |
| Duplicate and missing-value checks                           | ACHIEVED                     | Duplicates: 0, missing: 0                                                                              |
| Group-aware split without leakage                            | ACHIEVED                     | Group overlap asserted = empty                                                                         |
| Max sequence length selected by validation evidence          | ACHIEVED                     | Selected L = 128                                                                                       |
| SimpleRNN, LSTM, GRU compared (scratch)                      | ACHIEVED                     | 9 runs: 3 architectures x 3 seeds                                                                      |
| English IMDB transfer compared with scratch                  | ACHIEVED (substitute source) | Strategy B status: RUN; source: Pang & Lee polarity corpus v2.0 (IMDb-sourced; 1,600 train / 400 test) |
| Test Macro F1 >= 0.65                                        | ACHIEVED                     | Selected model test Macro F1 = 0.9976                                                                  |
| Mixed-class F1 >= 0.45                                       | ACHIEVED                     | Selected model test Mixed F1 = 0.9982                                                                  |
| CPU inference, 10,000 reviews < 60 s                         | ACHIEVED                     | Measured 3.200 s (CPU, incl. preprocessing)                                                            |
| Cohen's Kappa computed (model vs labels)                     | ACHIEVED                     | Kappa = 0.9964                                                                                         |
| Inter-annotator agreement                                    | NOT ACHIEVED                 | Requires >=2 human annotators; not performed                                                           |
| Error analysis by required categories                        | ACHIEVED                     | 2 test errors analysed                                                                                 |
| Saved model reload equivalence                               | ACHIEVED                     | Max diff = 0.00e+00                                                                                    |
| Unseen-data challenge set (every review has an unseen token) | ACHIEVED                     | n=900; selection used the separate unseen-validation set                                               |
| Robustness variant trained (augmentation + normalisation)    | ACHIEVED                     | 9 runs: 3 architectures x 3 seeds                                                                      |
| Unseen-data test Macro F1 >= 0.65 (selected model)           | ACHIEVED                     | Macro F1 = 0.8116; Mixed F1 = 0.8514                                                                   |
| Robust variant improves unseen-data Macro F1 over baseline   | ACHIEVED                     | best baseline 0.6208 vs best robust 0.8052 (means over 3 seeds)                                        |
