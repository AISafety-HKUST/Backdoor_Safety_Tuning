### Towards Stable Backdoor Purification through Feature Shift Tuning (NeurIPS 2023)
This repository is the official implementation of [Towards Stable Backdoor Purification through Feature Shift Tuning](https://arxiv.org/abs/2310.01875).

Author: Rui Min*, Zeyu Qin*, Li Shen, Minhao Cheng

----
<div align=center><img src=pics/framework.png  width="80%" height="60%"></div>

### Abstract

It has been widely observed that deep neural networks (DNN) are vulnerable to backdoor attacks where attackers could manipulate the model behavior maliciously by tampering with a small set of training samples. Although a line of defense methods is proposed to mitigate this threat, they either require complicated modifications to the training process or heavily rely on the specific model architecture, which makes them hard to deploy into real-world applications. Therefore, in this paper, we instead start with fine-tuning, one of the most common and easy-to-deploy backdoor defenses, through comprehensive evaluations against diverse attack scenarios. Observations made through initial experiments show that in contrast to the promising defensive results on high poisoning rates, vanilla tuning methods completely fail at low poisoning rate scenarios. Our analysis shows that with the low poisoning rate, the entanglement between backdoor and clean features undermines the effect of tuning-based defenses. Therefore, it is necessary to disentangle the backdoor and clean features in order to improve backdoor purification. To address this, we introduce Feature Shift Tuning (FST), a method for tuning-based backdoor purification. Specifically, FST encourages feature shifts by actively deviating the classifier weights from the originally compromised weights. Extensive experiments demonstrate that our FST provides consistently stable performance under different attack settings. Additionally, it is also convenient to deploy in real-world scenarios with significantly reduced computation costs.

### Setup

Clone this repository and install all the required dependencies with the following commands.
```cmd
git clone https://github.com/AISafety-HKUST/stable_backdoor_purification.git
cd stable_backdoor_purification
conda create -n stable_backdoor_purification python=3.8
conda activate stable_backdoor_purification
sh ./sh/install.sh
sh ./sh/init_folders.sh
```

### Pipeline
#### Train backdoor models
Before conducting backdoor defense, you have to train a backdoor model with the poisoned training set. Here is an example of training a [BadNet](https://arxiv.org/abs/1708.06733) model on CIFAR-10.
```cmd
python ./attack/badnet.py --yaml_path ../config/attack/prototype/cifar10.yaml
```
You can customize the training process by modifying the configuration files. There are two important configuration files for training a backdoor model. The configuration files in the [prototype](config/attack/prototype/cifar10.yaml) directory contain some general configurations. For example, you could specify the architecture, learning rate, epoch numbers, etc by changing the corresponding field in the [config file](config/attack/prototype/cifar10.yaml). For specific attacks, the config file lies in individual [folders](config/attack/badnet), where you could specify hyperparameters dedicated to these attacks, such as the poisoning rate and trigger type.

#### Defense backdoor attacks
Here we list several fine-tuning methods in our paper.
To conduct FST:

```cmd
python fine_tune/ft.py --attack badnet --split_ratio 0.02 --pratio 0.1 --device cuda:2 --lr 0.01 --attack_target 0 --model resnet18 --dataset cifar10 --epochs 10 --initlr 0.1 --ft_mode fst --init --alpha 0.1
```


----
#### Our codes heavily depend on [BackdoorBench](https://github.com/SCLBD/BackdoorBench), *"BackdoorBench: A Comprehensive Benchmark of Backdoor Learning"*. It may be the best repo for backdoor research. Please consider leaving a :star: on their repository.
