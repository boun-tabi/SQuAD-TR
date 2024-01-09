## 📜 SQuAD-TR

SQuAD-TR is a machine translated version of the original [SQuAD2.0](https://rajpurkar.github.io/SQuAD-explorer/) dataset into Turkish using [Amazon Translate](https://aws.amazon.com/translate/).

### Dataset Description

- **Paper:** Building Efficient and Effective OpenQA Systems for Low-Resource Languages [[pdf](https://arxiv.org/abs/2401.03590)].
- **Point of Contact:** [Emrah Budur](mailto:emrah.budur@boun.edu.tr)


## Dataset Structure

### Data Instances

Our data instances follow that of the original SQuAD2.0 dataset.
Shared below is an example instance🍫 from the default train dataset.

Example from SQuAD2.0:
```
{
  "context": "Chocolate is New York City's leading specialty-food export, with up to US$234 million worth of exports each year. Entrepreneurs were forming a \"Chocolate District\" in Brooklyn as of 2014, while Godiva, one of the world's largest chocolatiers, continues to be headquartered in Manhattan.",
  "qas": [
    {
     "id": "56cff221234ae51400d9c140",
      "question": "Which one of the world's largest chocolate makers is stationed in Manhattan?",
      "is_impossible": false,
      "answers": [
        {
          "text": "Godiva",
          "answer_start": 194
        }
      ],
    }
  ]
}
```

Turkish translation:

```
{
    "context": "Çikolata, her yıl 234 milyon ABD dolarına varan ihracatı ile New York'un önde gelen özel gıda ihracatıdır. Girişimciler 2014 yılı itibariyle Brooklyn'de bir “Çikolata Bölgesi” kurarken, dünyanın en büyük çikolatacılarından biri olan Godiva merkezi Manhattan'da olmaya devam ediyor.",
    "qas": [
        {
            "id": "56cff221234ae51400d9c140",
            "question": "Dünyanın en büyük çikolata üreticilerinden hangisi Manhattan'da konuşlandırılmış?",
            "is_impossible": false,
            "answers": [
                {
                    "text": "Godiva",
                    "answer_start": 233
                }
            ]
        }
    ]
}

```

## Dataset Creation

We translated the titles, context paragraphs, questions and answer spans from the original SQuAD2.0 dataset using [Amazon Translate](https://aws.amazon.com/translate/) - requiring us to remap the starting positions of the answer spans, since their positions were changed due to the automatic translation.

We performed an automatic post-processing step to populate the start positions for the answer spans. To do so, we have first looked at whether there was an exact match for the translated answer span in the translated context paragraph and if so, we kept the answer text along with this start position found.
If no exact match was found, we looked for approximate matches using a character-level edit distance algorithm.

We have excluded the question-answer pairs from the original dataset where neither an exact nor an approximate match was found in the translated version. Our `default` configuration corresponds to this version. 

We have put the excluded examples in our `excluded` configuration. 

As a result, the datasets in these two configurations are mutually exclusive.  Below are the details for the corresponding dataset splits.

### Data Splits

The SQuAD2.0 TR dataset has 2 splits: _train_ and _validation_. Below are the statistics for the most recent version of the dataset in the default configuration.

| Split      | Articles | Paragraphs | Answerable Questions | Unanswerable Questions | Total   |
| ---------- | -------- | ---------- | -------------------- | ---------------------- | ------- |
| train      | 442      | 18776      | 61293                | 43498                  | 104,791 |
| validation | 35       | 1204       | 2346                 | 5945                   | 8291    |



| Split   | Articles | Paragraphs | Questions wo/ answers | Total   |
| ------- | -------- | ---------- | --------------------- | ------- |
| train-excluded   | 440        | 13490          | 25528                 | 25528   |
| dev-excluded     | 35        | 924          | 3582                     | 3582       |


In addition to the default configuration, we also a different view of train split can be obtained specifically for openqa setting by combining the `train` and `train-excluded` splits. In this view, we only have question-answer pairs (without `answer_start` field) along with their contexts.  

| Split      | Articles | Paragraphs | Questions w/ answers |  Total   |
| ---------- | -------- | ---------- | -------------------- |  ------- |
| openqa     | 442      | 18776      | 86821                |  86821   |

More information on our translation strategy can be found in our linked paper.

### Source Data

This dataset used the original SQuAD2.0 dataset as its source data.

### Licensing Information

The SQuAD-TR is released under [CC BY-NC-ND 4.0](https://creativecommons.org/licenses/by-nc-nd/4.0). 

## 📚 Resources 

### 📖 Download SQuAD-TR

#### 🔗 Raw files

* All SQuAD-TR files can be downloaded from the `data` folder of this repository.
* XQuAD-TR file can be downloaded [here](https://github.com/deepmind/xquad).
 
#### 🤗 HuggingFace datasets
```py
from datasets import load_dataset

squad_tr_standard_qa = load_dataset("boun-tabi/squad_tr", "default")
squad_tr_open_qa = load_dataset("boun-tabi/squad_tr", "openqa")
squad_tr_excluded = load_dataset("boun-tabi/squad_tr", "excluded")
xquad_tr = load_dataset("xquad", "xquad.tr") # External resource

```
* Demo application 👉 [Google Colab](https://colab.research.google.com/drive/1QVD0c1kFfOUc1sRGKDHWeF_HgNEineRt?usp=sharing). 

### 🔬 Reproducibility 

You can find all code, models and samples of the input data [here](https://drive.google.com/drive/folders/1Y3q1qvzpOSg3PWO18VQ0arp1hB-xcU1F?usp=sharing) and the instructions to reproduce the experiment results [here](https://docs.google.com/document/d/10oUYq22te0hwRaKEyM1Rkd2V9e1j8o_1GSPmnTOIBGI/edit?usp=sharing).  Please feel free to reach out to us if you have any specific questions. 


### ✍️ Citation

>[Emrah Budur](https://scholar.google.com/citations?user=zSNd03UAAAAJ), [Rıza Özçelik](https://rizaozcelik.github.io), [Dilara Soylu](https://scholar.google.com/citations?user=_NC2jJEAAAAJ), [Omar Khattab](https://omarkhattab.com), [Tunga Güngör](https://www.cmpe.boun.edu.tr/~gungort/)  and [Christopher Potts](https://web.stanford.edu/~cgpotts).  
Building Efficient and Effective OpenQA Systems for Low-Resource Languages. 2024. [[pdf]](https://arxiv.org/abs/2401.03590)

```
@misc{budur-etal-2024-squad-tr,
      title={Building Efficient and Effective OpenQA Systems for Low-Resource Languages}, 
      author={Emrah Budur and R{\i}za \"{O}z\c{c}elik and
              Dilara Soylu and Omar Khattab and
              Tunga G\"{u}ng\"{o}r and Christopher Potts},
      year={2024},
      eprint={2401.03590},
      archivePrefix={arXiv},
      primaryClass={cs.CL}
}
```

 
## ❤ Acknowledgment
This research was supported by the _[AWS Cloud Credits for Research Program](https://aws.amazon.com/government-education/research-and-technical-computing/cloud-credit-for-research/) (formerly AWS Research Grants)_.

We thank Alara Dirik, Almira Bağlar, Berfu Büyüköz, Berna Erden, Gökçe Uludoğan,  Havva Yüksel, Melih Barsbey, Murat Karademir, Selen Parlar, Tuğçe Ulutuğ, Utku Yavuz for their support on our application for AWS Cloud Credits for Research Program and Fatih Mehmet Güler for the valuable advice, discussion and insightful comments.
