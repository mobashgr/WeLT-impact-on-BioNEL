# BioNER
BioNER Code is adapted from [WeLT: Improving Biomedical Fine-tuned Pre-trained Language Models with Cost-sensitive Learning](https://github.com/mobashgr/WeLT)
## Installation 
**Dependencies**
-	Python (>=3.6)
-	Pytorch (>=1.2.0) 
1.	Clone this GitHub repository
2.	Navigate to the BioNER folder and install all necessary dependencies: `python3 -m pip install -r requirements.txt` \
Note: To install the appropriate torch, follow the [download instructions](https://pytorch.org/) based on your development environment.
## Data Preparation
**Data & Evaluation code Download** \
To directly download NER datasets for fine-tuning models from scratch, use `download.sh` or manually download them via this [link](https://drive.google.com/file/d/1nHH3UYpQImQhBTei5HiTcAAFBvsfaBw0/view) in main directory, `unzip datasets.zip` and `rm -r datasets.zip`
The same instructions are used for the evaluation code.

**Data Pre-processing** \
Execute `preprocessing.sh`.

## Reproducing Paper's results
We  conducted the experiments on two different BERT models using the WeLT weighting scheme. We have compared WeLT against the corresponding traditional fine-tuning approaches(i.e. BioBERT fine-tuning). 
We provide all the [fine-tuned models on Huggingface, an example of fine-tuning from scratch using WELT, and an example of predicting and evaluating disease entities](#Quick-Links).

### 1. Fine-tuning BERT Models 
Our experimental work focused on BioBERT(mixed/continual pre-trained language model) & PubMedBERT(domain-specific/trained from scratch pre-trained language model), however, WELT can be adapted to other transformers like ELECTRA.
| Model 	| Used version in HF :hugs: |
|---	|---	|
|BioBERT| [model_name_or_path](https://huggingface.co/dmis-lab/biobert-v1.1)|
|PubMedBERT| [model_name_or_path](https://huggingface.co/microsoft/BiomedNLP-PubMedBERT-base-uncased-abstract)|

### WELT fine-tuning

We have adapted the WeLT code and we use `named-entity-recognition/run_weight_scheme.py`
### 2. Building XML files
After fine-tuning BERT models, we recognize chemical & disease entities via `named-entity-recognition/ner.py`. The output files are in the `predicted path directory`

**Evaluation** \
We have used the strict and approximate evaluation of [BioCreative VII
Track 2 - NLM-CHEM track Full-text Chemical Identification and Indexing in PubMed articles](https://ftp.ncbi.nlm.nih.gov/pub/lu/BC7-NLM-Chem-track/BC7T2-evaluation_v3.zip)


## Quick Links
- [Fine-tuned models available on HF ](I couldn't change my username on Hugging Face, therefore I can't share the links because they contain one of the co-author's names. However, they are available publicly on HF.)
- [Fine-tuning from scratch example](*Please follow the instruction in README.md in named-entity-recognition folder*) 
- [Predicting disease entities using WELT example](*Please follow the instruction in README.md in named-entity-recognition folder*)
- [Evaluating predicted WELT disease example](*Please follow the instruction in README.md in named-entity-recognition folder*)
- [Instructions to reproduce BioNEL results](*Please navigate to BioNEL folder*)
 ## References 
-Ghadeer Mobasher, Wolfgang Müller, Olga Krebs, and Michael Gertz. 2023. WeLT: Improving Biomedical Fine-tuned Pre-trained Language Models with Cost-sensitive Learning. In The 22nd Workshop on Biomedical Natural Language Processing and BioNLP Shared Tasks, pages 427–438, Toronto, Canada. Association for Computational Linguistics.
