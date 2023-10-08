### Towards Stable Backdoor Purification through Feature Shift Tuning (NeurIPS 2023)
This repository is the official implementation of [Towards Stable Backdoor Purification through Feature Shift Tuning](https://arxiv.org/abs/2310.01875).

Author: Rui Min*, Zeyu Qin*, Li Shen, Minhao Cheng

----
<div align=center><img src=pics/framework.png  width="80%" height="60%"></div>

## Abstract

It has been widely observed that deep neural networks (DNN) are vulnerable to backdoor attacks where attackers could manipulate the model behavior maliciously by tampering with a small set of training samples. Although a line of defense methods is proposed to mitigate this threat, they either require complicated modifications to the training process or heavily rely on the specific model architecture, which makes them hard to deploy into real-world applications. Therefore, in this paper, we instead start with fine-tuning, one of the most common and easy-to-deploy backdoor defenses, through comprehensive evaluations against diverse attack scenarios. Observations made through initial experiments show that in contrast to the promising defensive results on high poisoning rates, vanilla tuning methods completely fail at low poisoning rate scenarios. Our analysis shows that with the low poisoning rate, the entanglement between backdoor and clean features undermines the effect of tuning-based defenses. Therefore, it is necessary to disentangle the backdoor and clean features in order to improve backdoor purification. To address this, we introduce Feature Shift Tuning (FST), a method for tuning-based backdoor purification. Specifically, FST encourages feature shifts by actively deviating the classifier weights from the originally compromised weights. Extensive experiments demonstrate that our FST provides consistently stable performance under different attack settings. Additionally, it is also convenient to deploy in real-world scenarios with significantly reduced computation costs.


----
#### Our codes heavily depend on [BackdoorBench](https://github.com/SCLBD/BackdoorBench), *"BackdoorBench: A Comprehensive Benchmark of Backdoor Learning"*. It may be the best repo for backdoor research. Please consider leaving a :star: on their repository.
