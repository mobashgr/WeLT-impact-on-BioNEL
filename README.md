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
**NER Datasets**
| Dataset 	| Source 	|
|---	|---	|
| <ul><li>NCBI-disease</li> <li>BC5CDR-disease</li>  <li>BC5CDR-chem</li></ul> 	| NER datasets are directly retrieved from [BioBERT](https://github.com/dmis-lab/biobert) via this [link](http://nlp.dmis.korea.edu/projects/biobert-2020-checkpoints/datasets.tar.gz) 	|
| <ul><li>BioRED-Dis</li>  <li>BioRED-Chem</li></ul> 	| We have extended the aforementioned NER datasets to include [BioRED](https://ftp.ncbi.nlm.nih.gov/pub/lu/BioRED/). To convert from  `BioC XML / JSON` to `conll`, we used [bconv](https://github.com/lfurrer/bconv) and filtered the chemical and disease entities. 	|

**Data & Evaluation code Download** \
To directly download NER datasets for fine-tuning models from scratch, use [`download.sh`](https://github.com/mobashgr/Re-scaling-class-distribution-for-fine-tuning-BERT-based-models/blob/main/download.sh) or manually download them via this [link](https://drive.google.com/file/d/1nHH3UYpQImQhBTei5HiTcAAFBvsfaBw0/view) in main directory, `unzip datasets.zip` and `rm -r datasets.zip`
The same instructions are used for the evaluation code.

**Data Pre-processing** \
We adapted the [`preprocessing.sh`](https://github.com/mobashgr/Re-scaling-class-distribution-for-fine-tuning-BERT-based-models/blob/main/named-entity-recognition/preprocess.sh) from [BioBERT](https://github.com/dmis-lab/biobert) to include [BioRED](https://ftp.ncbi.nlm.nih.gov/pub/lu/BioRED/)

## Reproducing Paper's results
We  conducted the experiments on two different BERT models using the WELT weighting scheme. We have compared WELT against the corresponding traditional fine-tuning approaches(i.e. BioBERT fine-tuning). We explain the [WELT fine-tuning approach](#12-welt-fine-tuning).
We provide all the [fine-tuned models on Huggingface, an example of fine-tuning from scratch using WELT, and an example of predicting and evaluating disease entities](#Quick-Links).

### 1. Fine-tuning BERT Models 
Our experimental work focused on BioBERT(mixed/continual pre-trained language model) & PubMedBERT(domain-specific/trained from scratch pre-trained language model), however, WELT can be adapted to other transformers like ELECTRA.
| Model 	| Used version in HF :hugs: |
|---	|---	|
|BioBERT| [model_name_or_path](https://huggingface.co/dmis-lab/biobert-v1.1)|
|PubMedBERT| [model_name_or_path](https://huggingface.co/microsoft/BiomedNLP-PubMedBERT-base-uncased-abstract)|

### 1. WeLT fine-tuning

We have adopted [BioBERT-run_ner.py](https://github.com/dmis-lab/biobert-pytorch/blob/master/named-entity-recognition/run_ner.py) to develop a cost-sensitive trainer in [run_weight_scheme.py](https://github.com/mobashgr/Re-scaling-class-distribution-for-fine-tuning-BERT-based-models/blob/main/named-entity-recognition/run_weight_scheme.py#L94-103) that extends `Trainer` class to `WeightedLossTrainer` and override [`compute_loss`](https://github.com/mobashgr/Re-scaling-class-distribution-for-fine-tuning-BERT-based-models/blob/main/named-entity-recognition/run_weight_scheme.py#L96) function to include [`WELT`](https://github.com/mobashgr/Re-scaling-class-distribution-for-fine-tuning-BERT-based-models/blob/main/named-entity-recognition/run_weight_scheme.py#L129-142) in [`weighted Cross-Entropy loss function`](https://github.com/mobashgr/Re-scaling-class-distribution-for-fine-tuning-BERT-based-models/blob/main/named-entity-recognition/run_weight_scheme.py#L101)

### 2. Building XML files
After fine-tuning BERT models, we recognize chemical & disease entites via [`ner.py`](https://github.com/mobashgr/Re-scaling-class-distribution-for-fine-tuning-BERT-based-models/blob/main/named-entity-recognition/ner.py). The output files are in [predicted path directory](https://github.com/mobashgr/Re-scaling-class-distribution-for-fine-tuning-BERT-based-models/blob/main/predictedpath)

**Evaluation** \
We have used the strict and approximate evaluation of [BioCreative VII
Track 2 - NLM-CHEM track Full-text Chemical Identification and Indexing in PubMed articles](https://ftp.ncbi.nlm.nih.gov/pub/lu/BC7-NLM-Chem-track/BC7T2-evaluation_v3.zip)


## Quick Links
- [Fine-tuned models available on HF ](https://github.com/mobashgr/WeLT-impact-on-BioNEL/blob/main/named-entity-recognition/README.md#Fine-tuned-HF-:hugs:)
- [Fine-tuning from scratch example](https://github.com/mobashgr/WeLT-impact-on-BioNEL/blob/main/named-entity-recognition/README.md#Usage-example-for-WELT-finetuning) 
- [Predicting disease entities using WELT example](https://github.com/mobashgr/WeLT-impact-on-BioNEL/blob/main/named-entity-recognition/README.md#Usage-example-for-predicting-disease-entities-using-WELT)
- [Evaluating predicted WELT disease example](https://github.com/mobashgr/WeLT-impact-on-BioNEL/blob/main/named-entity-recognition/README.md#Usage-example-for-strict-evaluation-of-NCBI-Disease-predicted-file-using-WELT)

 ## Citation
 The manuscript is in preparation (TBD)
 ## Authors
 Authors: Ghadeer Mobasher*, Pedro Ruas, Francisco M. Couto, Olga Krebs, Michael Gertz and Wolfgang Müller
## Acknowledgment
Ghadeer Mobasher is part of the PoLiMeR-ITN (http://polimer-itn.eu/) and is supported by the European Union’s Horizon 2020 research and innovation program under the Marie Skłodowska-Curie grant agreement PoLiMeR, No 81261
