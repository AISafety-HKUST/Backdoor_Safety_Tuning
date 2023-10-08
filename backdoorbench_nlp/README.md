# BackdoorBench - NLP 

![Python 3.6](https://img.shields.io/badge/python-3.7-DodgerBlue.svg?style=plastic)
![Pytorch 1.10.0](https://img.shields.io/badge/pytorch-1.10.0-DodgerBlue.svg?style=plastic)
![transformers: 4.1.1](https://img.shields.io/badge/transformers-4.1.1-brightgreen)

<!---

## [Overview](#overview)

<a href="#top">[Back to top]</a>
-->

BackdoorBench - NLP is a complementary material for the original [BackdoorBench](https://github.com/SCLBD/BackdoorBench) which mainly focuses on Computer Vision domain. It provides easy implementations of two mainstream backdoor attack methods and one defense method in Natural Language Processing(NLP) domain.

- **Methods**
  - 2 Backdoor attack methods: [HiddenKiller](https://arxiv.org/pdf/2105.12400.pdf), [BkdAtk-LWS](https://arxiv.org/pdf/2106.06361.pdf)
  - 1 Backdoor defense methods: [ONION](https://arxiv.org/pdf/2011.10369.pdf)
- **Datasets**: SST-2, AgNews, OLID
- **Models**: BERT-base-uncased

---
<font size=5><center><b> Table of Contents </b> </center></font>

<!-- * [Overview](#overview) -->

* [Usage](#usage)
  * [Attack](#attack)

  * [Defense](#defense)
* [Supported attacks](#supported-attacks)
* [Supported defenses](#supported-defsense)
* [Results](#results)

---

### [Usage](#usage)

<!--- <a href="#top">[Back to top]</a> -->

#### [Attack](#attack)

<a href="#top">[Back to top]</a>

This is a demo script of running HiddenKiller attack on SST-2. The first two commands are used to generate the poisoned training set with user-specified poison rate, target label, etc. The third command is to run HiddenKiller attack on BERT with the datasets generated by the first two commands.
```
python ./attack/HiddenKiller/generate_by_openattack.py --yaml_path ../../config/attack/hiddenkiller/generate_poison_data.yaml

python ./attack/HiddenKiller/generate_poison_train_data.py --yaml_path ../../config/attack/hiddenkiller/generate_poison_train_data.yaml

python ./attack/HiddenKiller/attack_hiddenkiller.py --yaml_path ../../config/attack/hiddenkiller/hiddenkiller_default.yaml
```
After attack, the poisoned model will be saved in ./models, which can be used for further defense.
If you want to change the attack methods, dataset, save folder location, you should specify both the attack method script in ./attack and the YAML config file to use different attack methods.

#### [Defense](#defense)

<a href="#top">[Back to top]</a>

This is a demo script of running ONION defense on SST-2 for HiddenKiller. Before defense you need to run the corresponding attack using the commands given above.

```
python ./defense/onion/test_defense_hiddenkiller.py --yaml_path ../../config/defense/onion/onion_hiddenkiller.yaml
```


If you want to change the defense methods and the setting for defense, you should specify both the attack method script in ../defense and the YAML config file to use different defense methods.

### [Supported attacks](#supported-attacks)

<a href="#top">[Back to top]</a>

|              | File name                                                    | Paper                                                        |
| :----------: | ------------------------------------------------------------ | ------------------------------------------------------------ |
| HiddenKiller | [generate_by_openattack.py](./attack/HiddenKiller/generate_by_openattack.py), [generate_poison_train_data.py](./attack/HiddenKiller/generate_poison_train_data.py), [attack_hiddenkiller.py](./attack/HiddenKiller/attack_hiddenkiller.py) | [Hidden Killer: Invisible Textual Backdoor Attacks with Syntactic Trigger](https://arxiv.org/pdf/2105.12400.pdf) ACL 2021 |
|     LWS      | [attack_lws.py](./attack/LWS/attack_lws.py)                  | [Turn the Combination Lock: Learnable Textual Backdoor Attacks via Word Substitution](https://arxiv.org/pdf/2106.06361.pdf)  ACL 2021 |
|              |                                                              |                                                              |

### [Supported defenses](#supported-defsense) 

<a href="#top">[Back to top]</a>

|       | File name                 | Paper                |
| :------------- |:-------------|:-----|
| ONION | [test_defense_hiddenkiller.py](./defense/onion/test_defense_hiddenkiller.py), [test_defense_lws.py](./defense/onion/test_defense_lws.py) | [ONION: A Simple and Effective Defense Against Textual Backdoor Attacks](https://arxiv.org/abs/2011.10369) EMNLP 2021 |

We did not merge the code of ONION into a single file for additional data pre-processing method is required for LWS attack before running the defense. Besides, the origianl implementation of the two codes differ a lot in inferfaces and abstractions. We will keep them in separate forms for now and provide a unified version later as the framework is established.

### [Results](#results)

<a href="#top">[Back to top]</a>

We present results on all darasets with poison ratio = 5%.

|                   |                   | BackdoorDefense→     | Nodefense    | Nodefense    | Nodefense    | ONION | ONION   | ONION   |
| ----------------- | -------------------- | ------------ | ------------ | ------------ | --------- | ------- | --------- | --------- |
| TargetedModel     | Dataset↓ | BackdoorAttack↓     | C-Acc (%)    | ASR (%)      | R-Acc (%)    | C-Acc (%) | ASR (%) | R-Acc (%) |
| BERT-base-uncased | SST-2 | BkdAtk-LWS | 89.017 | 94.079 | 4.276    | 86.200 | 90.417 | 9.583 |
| BERT-base-uncased | OLID | BkdAtk-LWS | 82.674 | 97.917 | 0.833    | 79.070 | 96.774 | 3.225 |
| BERT-base-uncased | AgNews | BkdAtk-LWS         | 93.105 | 99.193 | 0.614    | 92.100 | 68.030 | 10.967 |
| BERT-base-uncased | SST-2 | HiddenKiller  | 90.335 | 88.925 | 11.075   | 85.667 | 88.267  | 11.732 |
| BERT-base-uncased | OLID | HiddenKiller | 82.189 | 97.415 | 2.585    | 81.374 | 96.123 | 3.877 |
| BERT-base-uncased | AgNews | HiddenKiller   | 93.487 | 98.667 | 1.123    | 92.053 | 95.158 | 4.211 |