# Context or Culture? Explaining Sentiment in Annual CEO Communication through Leadership Contexts and National Environments

[![Python](https://img.shields.io/badge/python-3.11%2B-blue.svg)](#requirements)

**Author** : Saowalak De Rossi
**Research area:** CEO communication, leadership contexts, national environments and natural language processing

## Project overview

This master's thesis examines whether sentiment in official annual CEO communications is associated more strongly with the leadership context being discussed or with the company's national environment.

### Study scope and sources

- **Industry:** sportswear and sporting goods
- **Target reporting years:** 2020–2025
- **Sources:** official annual CEO communications of  `adidas`, `puma`, `asics` and `mizuno`
- **Sentiment input:** english-language CEO communications, manually labelled for leadership context and information valence before BERT-based sentiment analysis

#### Research questions

1. How does sentiment vary across leadership contexts?
2. How does sentiment differ between the German and Japanese companies in the sample?
3. Do leadership contexts account for more variation in sentiment than country, after controlling for information valence, communication format and year?
4. Does the relationship between leadership context and sentiment vary by country?

---

## Repository structure

```
CEOCommunication/
├── data/                                       # Source reports and coded dataset
├── docs/                                       # Thesis framework and literature
├── notebooks/                                  # Sequential analysis notebooks
│   ├── 01_data_exploration.ipynb       
│   ├── 02_sentiment_analysis.ipynb                 
│   ├── 03_sentiment_exploration.ipynb   
│   ├── 04_regression_analysis.ipynb    
│   ├── 05_regression_exploration.ipynb   
│   ├── 06_extension.ipynb                     
├── results/                                
│   ├── extensions/                         
│   ├── figures/
│   ├── tables/
project
├── requirements.txt                            # Python dependencies
└── README.md                                   # Project and execution documentation
```

---

## Dataset structure

| Column                  | Meaning and convention                                                                                     |
| ----------------------- | ---------------------------------------------------------------------------------------------------------- |
| `ID`                  | Unique identifier that identifies the segment's order and source ex. DEU25A100                             |
| `Company`             | Lowercase company name                                                                                     |
| `Country`             | `DEU` or `JPN`                                                                                         |
| `Year`                | Numeric reporting year in YYYY format                                                                      |
| `Covid`               | Specific period indicator for COVID-19 : 2024 and 2025 →`False` ; 2020, 2021, 2022 and 2023 → `True` |
| `CEO`                 | CEO associated with the announcement                                                                       |
| `Communication_type`  | Communication format and, where applicable, subcategory                                                    |
| `Leadership_context`  | One dominant category:`PERF`, `STRAT`, `MARKET`, `CHALL`, `PEOPLE` or `STAKE`                  |
| `Information_valence` | `Positive`, `Neutral` or `Negative`                                                                  |
| `Comment`             | A brief explanation of leadership context labeling and the`Information_valence`                          |
| `Text`                | Full original data segment                                                                                 |

**Manual coding protocol**

1. Preserve the original paragraph as the default observation
2. Retain the complete text, removing only PDF layout artifacts such as line wrapping or word breaks introduced by pagination
3. Assign one primary leadership context according to:

   **Semantic meaning → Temporal orientation → Verb patterns → Keywords**
4. For ambiguous paragraphs, use the dominant communicative purpose rather than counting keywords
5. Split only when necessary and when independent contexts can be separated without destroying their meaning
6. Code information valence independently of sentiment and record a concise rationale

   **Leadership contexts**

   | Code       | Context                                | Primary focus                                                                                                        |
   | ---------- | -------------------------------------- | -------------------------------------------------------------------------------------------------------------------- |
   | `PERF`   | Business Performance & Achievements    | Realized financial or operational results, growth market-share achievements, records and completed milestones        |
   | `STRAT`  | Strategy, Growth & Future Direction    | Future objectives, priorities, investment plans, expansion, transformation and resource allocation                   |
   | `MARKET` | Product, Brand & Market                | Products, innovation, brands, consumers, athletes, retailers, marketing, distribution and commercial market dynamics |
   | `CHALL`  | Challenges, Crisis & Adaptation        | Problems, threats, uncertainty, disruptions and responses to adverse internal or external conditions                 |
   | `PEOPLE` | People & Organization                  | Employees, talent, management, organizational structure, workplace practices and corporate culture                   |
   | `STAKE`  | Stakeholders, Society & Sustainability | Societal and environmental impact, stakeholder relationships, human rights and broader responsibility                |

   Temporal orientation helps distinguish achieved growth (`PERF`) from growth ambitions (`STRAT`), but semantic meaning remains the first criterion.

   **Information valence**

   | Label             | Interpretation                                                                                                                  | Illustrative case                                                        |
   | ----------------- | ------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------ |
   | `Positive`      | Favorable realized development, improvement, achievement or outcome                                                             | Revenue increased; emissions fell; an operational problem was resolved   |
   | `Neutral/Mixed` | No clear directional outcome, primarily a plan or commitment, or positive and negative information without a dominant direction | A future target; a current situation; balanced improvements and setbacks |
   | `Negative`      | Unfavorable development, deterioration, failure or adverse outcome                                                              | Sales declined; a target was missed; jobs were cut                       |

---

## BERT-based sentiment analysis

The sentiment analysis is base on the BERT model. It performs inference with a pretrained model.
The basic workflow:

1. Load and validate the coded dataset
2. Tokenize with the model's tokenizer, retaining punctuation and sentence structure
3. Process each segment within the input limit as a single input
4. Return one sentiment observation per original segement
5. Save scores separately from the source dataset in a new dataset

The basic output structure:

| Output column        | Meaning                                              |
| -------------------- | ---------------------------------------------------- |
| `Sentiment_status` | Whether the row was scored or could not be processed |
| `P_positive`       | Positive-class model probability                     |
| `P_neutral`        | Neutral-class model probability                      |
| `P_negative`       | Negative-class model probability                     |
| `Sentiment_label`  | Highest-probability class                            |
| `Sentiment_score`  | `P_positive - P_negative`, ranging from −1 to +1  |
| `Token_count`      | Number of model content tokens                       |
| `Chunk_count`      | Number of internal model inputs                      |

---

## Regression framework

For observation *i*, the thesis specifies:

`Sentiment_i = α + β LeadershipContext_i + γ Country_i + ρ InformationValence_i + δ CommunicationType_i + λ Year_i + θ (LeadershipContext_i × Country_i) + ε_i`

- **Outcome:** continuous `Sentiment_score` base on the Sentiment analysis output
- **Main explanatory variables:** leadership context and country
- **Country coding:** Germany = 0; Japan = 1
- **Controls:** information valence, broad communication format and reporting year
- **Interaction:** whether the context–sentiment association differs by country

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

| Notebook                            | Purpose                                                                                                 |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------- |
| `01_data_exploration.ipynb`       | Check source coverage, IDs, coding categories, missing data and initial descriptive statistics          |
| `02_sentiment_analysis.ipynb`     | Run the sentiment analysis                                                                              |
| `03_sentiment_exploration.ipynb`  | Produce descriptive comparisons, figures and passages for qualitative review for the sentiment analysis |
| `04_regression_analysis.ipynb`    | Run the regression analysis                                                                             |
| `05_regression_exploration.ipynb` | Interpret coefficients, check diagnostics and compare model specifications                              |
| `06_extension.ipynb`              | Other explicitly documented extensions                                                                  |

Run notebooks in order. After changes to source data or functions, restart the kernel and run the relevant notebooks from the beginning to avoid stale variables.

---

## Literature

- Devlin et al. (2019), *BERT*.
- Kılınç & Arıcı (2020), *Corporate Narrative in Annual Reports: A Discourse Analysis of CEO Letters*
- Liu, Bilal & Komal (2022), *A Corpus-Based Comparison of the Chief Executive Officer Statements in Annual Reports and Corporate Social Responsibility Reports*
- Arvidsson (2023), *CEO talk of sustainability in CEO letters: towards the inclusion of a sustainability embeddedness and value-creation perspective*
- Arvidsson & Sabelfeld (2023), *Adaptive framing of sustainability in CEO letters*
- He, P., Gao, J., & Chen, W. (2023). *DeBERTaV3: Improving DeBERTa using ELECTRA-Style Pre-Training with Gradient-Disentangled Embedding Sharing*
- Altarawneh (2026), *Essays on the information content of CEO letters in annual reports*.
- Lu, Zhao & Hu (2026), *Building Corporate Identity Through Interactional Metadiscourse: A Corpus-based Study of the US and Chinese CEO Letters*
- Nguyen (2026), *Stance in CEO Statements from U.S. and Vietnamese Banks’ Annual Reports: A Corpus-Based Cross-Cultural Study*

Implementation references used during workflow development:

- [FinBERT model card](https://huggingface.co/ProsusAI/finbert)
- [Hugging Face tokenizer documentation](https://huggingface.co/docs/transformers/main_classes/tokenizer)
- [statsmodels linear regression documentation](https://www.statsmodels.org/stable/regression)
