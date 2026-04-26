# Point of Adversarial-Example Success (PAES)

**Point of Adversarial-Example Success (PAES)** is an experimental metric for evaluating adversarial robustness in security-focused machine-learning systems. It is designed around a simple operational question: **how many inference-time adversarial queries does it take before an attacker achieves a targeted misclassification?**

In many security contexts, average model performance is not enough. A system may be considered unsafe if an attacker can achieve even one successful targeted misclassification within a realistic query budget. This is especially relevant where a model acts as part of an access-control, identity-verification, triage, or decision-support system. In those settings, a single failure can be operationally serious, even if the model performs well on average.

PAES reframes adversarial robustness from the perspective of attacker effort. Rather than only asking whether an attack can succeed across a test set, it asks how much interaction with the model is required before the first successful adversarial example is found. This makes query count a useful operational signal: repeated failed attempts may be rate-limited, logged, blocked, or investigated, while fast success suggests that the system may be fragile in realistic deployment conditions.

PAES is closely related to **Adversarial Success Rate (ASR)**. ASR measures the proportion of attacks that succeed across a dataset or evaluation run. PAES instead focuses on the query count required for success. Two models may have the same ASR, but very different security properties if one can be compromised in tens of queries while the other requires thousands. PAES is therefore intended to complement ASR, not replace it.

Formally, PAES is defined as the number of inference-time adversarial queries required for an attacker to achieve a targeted misclassification under a black-box, decision-based attack model. In simple terms: PAES = number of attacker queries before first successful targeted misclassification.

A higher PAES value suggests that the model imposes a greater operational cost on the attacker under the specified threat model. A lower PAES value suggests that successful adversarial examples can be found quickly, making the system easier to compromise.

The current threat model assumes black-box access. The attacker cannot inspect model weights, gradients, training data, or internal confidence scores. The attacker can only submit inputs and observe the final hard-label decision, such as a class label or accept/reject result. The attack is targeted: the goal is not merely to cause any misclassification, but to force the model toward a chosen target class or identity. This makes the framing relevant to facial-recognition-style systems, identity checks, and other settings where an attacker wants to be accepted as a specific target.

The initial attack method considered for PAES is **HopSkipJumpAttack (HSJ)**, implemented through the IBM Adversarial Robustness Toolbox. HSJ is appropriate for this project because it is a decision-based black-box attack and can operate using hard-label outputs. The evaluation records whether the attack succeeds within a fixed query budget and, if so, the query number at which the first targeted success occurs.

The initial application area is image classification and facial-recognition-style models. Planned or initial baselines include ResNet-18 as a practical lower-cost baseline and ResNet-50 as a stronger comparison model. The planned demonstration dataset is **CelebA**, a publicly available facial-attribute dataset commonly used in computer vision research. CelebA is useful here because it provides a recognisable image-classification setting while making the robustness problem easier to communicate to both technical and non-technical audiences.

A related side metric is **FLOPs Until Success (FUSS)**. While PAES counts attacker queries, FUSS estimates the compute cost required before adversarial success. It can be approximated as: FUSS = PAES × estimated FLOPs per query.

FUSS is intended as a rough, hardware-agnostic indicator of attacker cost. PAES remains the primary operational metric, but FUSS may be useful for reasoning about the longer-term arms race between attacker compute, model complexity, and defensive overhead.

A typical PAES evaluation should record the model architecture, dataset, target class or identity, attack method, query budget, success status, query count at first success, and any relevant perturbation or detector behaviour. If FUSS is used, the evaluation should also record the estimated FLOPs per query.

Example result:

- model: resnet18
- attack: HopSkipJump
- targeted: true
- query_budget: 10000
- success: true
- paes: 742
- fuss: 134500000000
- dataset: CelebA

The project is intended as a research and implementation exercise, not as a finished production ML package. The aim is to formalise the PAES metric, implement baseline experiments, and test whether query-to-first-success provides a useful operational robustness signal for security-relevant ML systems.

## Tooling

The expected tooling includes PyTorch and torchvision for model training and evaluation, IBM Adversarial Robustness Toolbox for adversarial attacks, HopSkipJumpAttack for decision-based black-box testing, and ptflops, fvcore, or torch.profiler for FLOP estimation. The project may also be integrated into the A4S evaluation framework as a model metric.

## Dataset

The planned demonstration dataset is CelebA.

Dataset link: https://mmlab.ie.cuhk.edu.hk/projects/CelebA.html

## References

Dong, Y. et al. (2019). *Efficient Decision-Based Black-Box Adversarial Attacks on Face Recognition*. CVPR 2019.

Chen, J., Jordan, M., and Wainwright, M. (2020). *HopSkipJumpAttack: A Query-Efficient Decision-Based Attack*. IEEE Symposium on Security and Privacy 2020.

Sharif, M. et al. (2016). *Accessorize to a Crime: Real and Stealthy Attacks on State-of-the-Art Face Recognition*. CCS 2016.

Carlini, N. and Wagner, D. (2017). *Towards Evaluating the Robustness of Neural Networks*. IEEE Symposium on Security and Privacy 2017.

Madry, A. et al. (2018). *Towards Deep Learning Models Resistant to Adversarial Attacks*. ICLR 2018.

OECD.AI catalogue: *Time until adversary’s success* as a related conceptual framing.

## Status

This is an early-stage research and implementation project. The current focus is to define the metric clearly, build baseline experiments, and evaluate whether PAES offers a useful way to describe adversarial robustness in operational security terms.

## Disclaimer

This project is for defensive research, robustness evaluation, and educational purposes. It is intended to support safer ML system design by making adversarial effort more measurable and interpretable.
