# Context or Culture? Explaining Sentiment in Annual CEO Communication through Leadership Contexts and National Environments

[![Python](https://img.shields.io/badge/python-3.11%2B-blue.svg)](#requirements)
[![Reproducibility](<https://img.shields.io/badge/reproducibility-output%20checks-green.svg>)](#output-checks-and-reproducibility)
[![Status](<https://img.shields.io/badge/status-replication%20workflow-informational.svg>)](#project-overview)

👤 **Author** : Saowalak De Rossi

## 🎯 Project overview

Examines whether the sentiment expressed in official CEO communications is associated more strongly with the leadership context being communicated or with the company's national environment.

## 🗂️ Repository structure

```
CEOCommunication/
├── data/                                       # Report, dataset 
├── docs/                                       # Litterature
├── notebooks/                                  # Jupyter notebooks files
│   ├── 01_data_exploration.ipynb           
│   ├── 02_sentiment_analysis.ipynb                     
│   ├── 03_sentiment_exploration.ipynb     
│   ├── 04_regression_analysis.ipynb        
│   ├── 05_regression_exploration.ipynb     
│   ├── 06_extension.ipynb                         
├── results/                                    
│   ├── regression/                          
│   ├── sentiment/                           
├── main.py                                     # Execute the full project extensions automatically 
├── requirements.txt                            # Project libraries required 
└── README.md                                   # General project & execution description
```

1. Install Python 3.11 or newer
2. In VS Code's terminal, create the environment:

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
   pip install -r requirements.txt
   pip install ipykernel
   ```
