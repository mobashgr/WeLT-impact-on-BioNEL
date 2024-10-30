## Hyperparameters
The table shows the used hyper-parameters for fine-tuning BERT models.

| Dataset | Epochs | Batch Size | Maximum Length |
| :------------------ | :----- | :----- | :----- |
| NCBI-Disease | 10| 8 | 320 |
| BC5CDR-Disease |10 | 8 |320|
| BioRED-Disease | 10 | 8 | 384 |
| BC5CDR-Chemical | 10 | 16 | 256 |
| BioRED-Chem | 10 | 32| 128 |

## Fine-tuned HF 🤗
The table provides links to the fine-tuned models as an output of our experimental work. 
As I mentioned previously, I couldn't change my username on Hugging Face, therefore I can't share the links because they contain one of the co-authors' names. However, they are available publicly on HF.
| Dataset | Orignal| WeLT 
| :------------------ | :----- | :----- 
| NCBI-Disease | [model_name_or_path]()| [model_name_or_path] ()|
| BC5CDR-Disease |[model_name_or_path]() | [model_name_or_path]() |
| BioRED-Disease |[model_name_or_path]()| [model_name_or_path]()|
| BC5CDR-Chemical |[model_name_or_path]() | [model_name_or_path]()|
| BioRED-Chem |[model_name_or_path]()| [model_name_or_path]() |

## Usage example for WeLT finetuning
This is an example of fine-tuning `NCBI-Disease` over `BioBERT` using `WeLT` weight scheme
```bash

cd named-entity-recognition
./preprocess.sh

export SAVE_DIR=./output
export DATA_DIR=../datasets/NER

export MAX_LENGTH=320
export BATCH_SIZE=8
export NUM_EPOCHS=10
export SAVE_STEPS=1000
export ENTITY=NCBI-disease
export SEED=1

python run_weight_scheme.py \
    --data_dir ${DATA_DIR}/${ENTITY}/ \
    --labels ${DATA_DIR}/${ENTITY}/labels.txt \
    --model_name_or_path dmis-lab/biobert-v1.1 \
   --output_dir ${ENTITY}-WELT-${MAX_LENGTH}-BioBERT \
    --max_seq_length ${MAX_LENGTH} \
    --num_train_epochs ${NUM_EPOCHS} \
    --weight_scheme WELT \
    --per_device_train_batch_size ${BATCH_SIZE} \
    --save_steps ${SAVE_STEPS} \
    --seed ${SEED} \
    --do_train \
    --do_eval \
    --do_predict \
    --overwrite_output_dir
  ```
## Usage example for predicting disease entities using WeLT
This is an example of fine-tuning `NCBI-Disease` over `BioBERT` using an `WELT` fine-tuned model on HF
```bash
cd named-entity-recognition

python3 ner.py \
--xmlfilepath path/unannotatedxmls/NCBItestset_corpus_noannotations.xml \
--model_name_or_path [path] \
--NERType Disease \
--outputfilepath Re-scaling-class-distribution-for-fine-tuning-BERT-based-models/predictedpath/NCBI-WELT-BioBERT-example.xml
  ```
## Usage example for strict evaluation of `NCBI-Disease` predicted file using WeLT

Assuming that `download.sh` has been already executed. Adjust the paths to the corresponding working directories.

```bash
cd BC7T2-evaluation_v3
python3 evaluate.py --reference_path path/referencepath/NCBItestset_corpus.xml \
 --prediction_path path/predictedpath/NCBI-WELT-BioBERT.xml \
 --evaluation_type span \
 --evaluation_method strict \
 --annotation_type Disease

 ```

