# Leadership context and linguistic sentiment in annual corporate leadership communication: Evidence from German and Japanese sporting-goods companies

[![Python](https://img.shields.io/badge/python-3.11%2B-blue.svg)](#requirements)

**Author** : Saowalak De Rossi

**Research area:** Leaders communication, leadership contexts, national environments and natural language processing

## Project overview

This master's thesis examines whether sentiment in official annual CEO communications is associated more strongly with the leadership context being discussed or with the company's national environment.

### Study scope and sources

- **Industry:** sportswear and sporting goods
- **Target reporting years:** 2020–2025
- **Sources:** official annual CEO communications of  `adidas`, `puma`, `asics` and `mizuno`
- **Sentiment input:** english-language CEO communications, manually labelled for leadership context and information valence before BERT-based sentiment analysis

---

## Repository structure

```
CEOCommunication/
├── data/                                       # Source reports and coded dataset
├── docs/                                       # Thesis framework and literature
├── notebooks/                                  # Sequential analysis notebooks
│   ├── 01_data_exploration.ipynb   
│   ├── 02_sentiment.ipynb 
│   ├── 03_regression.ipynb  
│   ├── 04_extension_intercoder_reliability.ipynb          
│   ├── 05_extension_sentiment_analysis.ipynb       
├── results/                                       
│   ├── figures/
│   ├── tables/
project
├── requirements.txt                            # Python dependencies
└── README.md                                   # Project and execution documentation
```

---

## Dataset structure

| Column                  | Meaning and convention                                                                                                                                   |
| ----------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ID`                  | Unique identifier that identifies the segment's order and source ex. DEU25A100                                                                           |
| `Text`                | Full original data segment                                                                                                                               |
| `Text_type`           | body_text,  title, quote,  question, answer                                                                                                          |
| `Leadership_context`  | One dominant category:`PERF`, `STRAT`, `MARKET`, `CHALL`, `PEOPLE`, `STAKE` or `OTHER`                                                     |
| `Coding_ambiguity`    | True or False                                                                                                                                            |
| `Alternative_context` | If Coding_ambiguity is True, assign one dominant category:`PERF`, `STRAT`, `MARKET`, `CHALL`, `PEOPLE`, `STAKE` or `OTHER` otherwise empty |
| `Information_valence` | `positive`, `neutral` or `negative`                                                                                                                |
| `Communication_type`  | letter, message, interview                                                                                                                               |
| `Source_type`         | annual_report, sustainability_report, integrated_report                                                                                                  |
| `Company`             | `adidas`, `puma`, `asics` or `mizuno`                                                                                                            |
| `Country`             | `DEU` or `JPN`                                                                                                                                       |
| `Year`                | Numeric reporting year in YYYY format                                                                                                                    |
| `Covid`               | Specific period indicator for COVID-19 : 2024 and 2025 →`false` ; 2020, 2021, 2022 and 2023 → `true`                                               |
| `Communicator_role`   | `ceo` or `president`                                                                                                                                 |
| `Communicator_name`   | Name of the `Communicator_role`                                                                                                                        |
| `Comment`             | A brief explanation of leadership context labeling and/or the information valence                                                                        |
| `Coding_ambiguity`    | `true` if there is another Leadership context option otherwise `false`                                                                               |
| `Alternative_context` | The alternative leadership context if `Coding_ambiguity` is `True` otherwise empty                                                                   |

---

## Requirements

- Python 3.11 or newer, subject to the compatibility of the versions in `requirements.txt`
- VS Code with Python and Jupyter support, or another Jupyter environment
- Python packages for data handling, model inference, visualization and regression
- Internet access for the initial pretrained-model download

Core dependencies include `pandas`, `openpyxl`, `numpy`, `torch`, `transformers`, `scipy`, `statsmodels`, `matplotlib` and `seaborn`.

## Installation

Run these commands from the repository root.

   **macOS/Linux**

```bash
   python3 -m venv .venv
   source .venv/bin/activate
   python -m pip install --upgrade pip
   pip install -r requirements.txt
   pip install ipykernel
   python -m ipykernel install --user --name=ceocontext --display-name="CEOContext"
```

   **Windows PowerShell**

```powershell
   py -m venv .venv
   .venv\Scripts\Activate.ps1
   python -m pip install --upgrade pip
   python -m pip install -r requirements.txt
   python -m pip install ipykernel
   python -m ipykernel install --user --name=ceocontext --display-name="CEOContext"
```

Open a notebook and select the **CEOContext** kernel. Notebook relative paths depend on the working directory, verify it before loading data.

## Execution workflow

| Notebook                                      | Purpose                                                                                                                            |
| --------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------- |
| `01_data_exploration.ipynb`                 |                                                                                                                                    |
| `02_sentiment.ipynb`                        | Run the sentiment analysis                                                                                                         |
| `03_regression.ipynb`                       | Produce descriptive comparisons, figures and passages for qualitative review for the sentiment analysisRun the regression analysis |
| `04_extension_intercoder_reliability.ipynb` | Run the regression analysis                                                                                                        |
| `05_extension_sentimen_analysis.ipynb`      | Produce descriptive comparisons, figures and passages for qualitative review for the sentiment analysis                            |
| `06_extension_regression_analysis.ipynb`    | Interpret coefficients, check diagnostics and compare model specifications                                                         |

Run notebooks in order. After changes to source data or functions, restart the kernel and run the relevant notebooks from the beginning to avoid stale variables.

---

**Implementation references used during workflow development:**

- [Financial-RoBERTa model card](https://huggingface.co/soleimanian/financial-roberta-large-sentiment)
- [FinBERT model card](https://huggingface.co/ProsusAI/finbert)
- [Hugging Face tokenizer documentation](https://huggingface.co/docs/transformers/main_classes/tokenizer)
- [Hugging Face model loading and sequence classification](https://huggingface.co/docs/transformers/model_doc/auto)
- [pandas DataFrame documentation](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html)
- [scikit-learn Cohen&#39;s kappa documentation](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.cohen_kappa_score.html)
- [scikit-learn confusion matrix documentation](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.confusion_matrix.html)
- [statsmodels linear regression documentation](https://www.statsmodels.org/stable/regression)
- [statsmodels robust covariance estimation](https://www.statsmodels.org/dev/generated/statsmodels.regression.linear_model.RegressionResults.get_robustcov_results.html)
- [statsmodels weighted least squares documentation](https://www.statsmodels.org/stable/generated/statsmodels.regression.linear_model.WLS.html)
- [statsmodels joint F-test documentation](https://www.statsmodels.org/stable/generated/statsmodels.regression.linear_model.RegressionResults.f_test.html)
