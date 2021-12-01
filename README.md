# BUILD: Benchmark for Understanding Indian Legal Documents


Indian Court Judgements have an inherent structure which is not explicitly mentioned in the judgement text. Identification of rhetorical roles of the sentences provides structure to the judgements. This is an important step which will act as building block for developing Legal AI solutions. We present benchmark for Rhetorical Role Prediction which include annotated data , evaluation methodology and baseline prediction model. This work is part of [OpenNyAI](https://opennyai.org/) mission which is funded by [EkStep Foundation](https://ekstep.org/).

## 1. What are Rhetorical Roles of Court Judgements
Typical structure of an Indian court judgement is as shown below. The flow is not linear and these roles can appear in any sequence.
![Typical Structure of Indian Court Judgements](Rhetorical_Roles_Structure.png)

## 2. Data
The data collection process was aimed at collecting sentence level rhetorical roles in Indian court judgements.
The data annotations were done voluntarily by Law students from multiple Indian law universities where each judgement sentence was 
classified  into one of the seven pre-defined rhetorical role.For a detailed overview of the process,please refer to the paper.

### 2.1 Data Download 


There are two  files:
- [Training Data](https://storage.googleapis.com/indianlegalbert/OPEN_SOURCED_FILES/Rhetorical_Role_Benchmark/Data/train.json)

- [Dev Data](https://storage.googleapis.com/indianlegalbert/OPEN_SOURCED_FILES/Rhetorical_Role_Benchmark/Data/dev.json)


### 2.2 Input Data Format

The top level structure of each JSON file is a list, where each entry represents a judgement-labels data point. Each data point is
a dict with the following keys:
- `id`: a unique id for this  data point. This is useful for evaluation.
- `annotations`:list of dict.The items in the dict are:
  - `result`a list of dictionaries containing sentence text and corresponding labels pair.The keys are:
    - `id`:unique id of each sentence
    - `value`:a dictionary with the following keys:
      - `start`:integer.starting index of the text
      - `end`:integer.end index of the text
      - `text`:string.The actual text of the sentence
      - `labels`:list.the labels that correspond to the text
- `data`: the actual text of the judgement.
- `meta`: a string.It tells about the category of the case(Criminal,Tax etc.)


## 3. Inference of Baseline Model on dev data
The baseline model was created using unified deep
learning architecture SciBERT-HSLN approach suggested by (Brack et al., 2021). SciBERT was replaced
with BERT BASE which are published by (Devlin et
al., 2018). Baseline model achieved micro f1 of 77.7 on hiddent test data. 

### 3.1 Run Baseline Model on dev data
#### 3.1.1 Install Dependencies
Python 3.8

To install the requirements,follow the instructions
```
pip install -r requirements.txt
```
#### 3.1.2 Download pretrained model
[Model File](https://storage.googleapis.com/indianlegalbert/OPEN_SOURCED_FILES/Rhetorical_Role_Benchmark/Model/model.pt)

#### 3.1.3 Run inference on dev data
 To run the inference on downloaded dev file , please run following command with appropriate paths
```
python infer_new.py dev_json_path output_json_path model_path

```

## 4. Submission & Evaluations
To preserve the integrity of test results, we do not release the test data to the public. Instead, we require you to submit your model so that we can run it on the test data for you.  We use Codalab for test data evaluation. Please refer to [Submission Guide](https://worksheets.codalab.org/worksheets/0xc791430db3c74588b87723255bd2d9db) for more details. 
The evaluation metric used here is micro f1.

## 5. Inference of Baseline Model on custom data

To train the model on data other than the one provided, we will need to split raw text into sentences. We use spacy transformer model for that. Install the spacy transformers model
by following the steps below:
To install the requirements,follow the instructions
```
pip install -r requirements.txt
```

To install en_core_web_trf, run:
```
 python -m spacy download en_core_web_trf
```


### 5.1 Data preparation for your own  data

If you want to train  the model on your own dataset and do the inference,you will need to preprocess the data to convert it 
into the required json format.To do so,follow the following steps:

The data prep file requires the data to be in the following format:
It should be a list of dict where each dict corresponds to a judgement.Each dict has the following keys:
- `id`: a unique id for this data point. This is useful for evaluation.
- `data`: dict with the key text which contains the actual text of the judgement.
For eg.
```
[  {"id": 1,
  "data": { "text": "input_text_1" } 
  },
  {"id": 2,
  "data": { "text": "input_text_2" } 
  },
   ....
]
```

To convert this into the format accepted by the model,run the data prep by:
```
python infer_data_prep.py
```

### 5.2 Run Inference

To run the inference,follow the following steps
```
python infer_new.py input_json_path output_json_path model_path

```
The output json will be written in the path provided as output_json_path.

The prediction file format will be same as the training json with `labels` filled with predicted labels.


## 6. Training Baseline Model on train data
The training data is in the Input Data Format specified above.To train the model on your custom data,
please convert it into the required format.
For training baseline model on train data, follow steps

### 6.1 Preprocessing
  Once the data is in the input data format,the data needs to be tokenized and written in the
  particular folder.This can be done by:
  ```
  python tokenize_files.py train_json_path
  ```
  
### 6.2 Run Training
  
  To train the HSLN model on the given data,we need to run the baseline_run.py.Model parameters like
max_epochs,num_batches etc. can be configured in the  baseline_run.py.To run the model on default parameters,
  ```
   python baseline_run.py 
  ```
  


## License
BUILD dataset is distribued under the [CC BY-SA 4.0](http://creativecommons.org/licenses/by-sa/4.0/legalcode) license.
The code is distribued under the Apache 2.0 license.



