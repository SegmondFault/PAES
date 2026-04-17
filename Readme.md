# **Point of Adversarial-Example Success (PAES) \-**

In security-focused applications, it only takes a single or small number of breaches for losses to become untenable, or for a system to be rendered unfit for purpose.

Conversely, in high-value defensive contexts, a threat actor’s goal and motivation may be satisfied by a single breach.

In tandem, input validation may easily catch and prevent repeated misclassification attempts, or slow them enough to make an attack inviable.

Respecting the heavy constraints on the attack surface — but also the potential impact of even a single failure — the author proposes the metric **PAES**, or **Point of Adversarial-Example Success,** a query-based operational metric.

---

## **Definition**

**Point of Adversarial-Example Success (PAES)** 

The number of inference-time adversarial queries q required for an attacker to achieve a targeted misclassification under a black-box, decision-based attack model.

**Built from:** *Adversarial Success Rate (ASR)*

The **Adversarial Success Rate (ASR)**, as defined in *Croce & Hein (2020)* and used in *Chen et al. (2020)*, measures the proportion of inputs for which an adversarial attack succeeds in changing the model’s prediction.

PAES re-expresses this concept for operational-security settings, focusing not on the overall fraction of successful attacks, but on the *effort or query count required* to achieve the first success.

**Side metric – FLOPs Until Success (FUSS)** 

A hardware-agnostic estimate of compute cost, intended as a long-term indicator of the arms race between attacker and defender. FUSS is derived directly from PAES.

---

## **1 Trustworthiness and Application**

**Aspect:** Security -> Robustness under targeted, black-box attacks.

**Goal:** Measure the operational robustness of image classifiers (e.g. facial-recognition systems) under targeted, decision-based black-box attacks.

**Threat model:**

A black-box adversary can query the system and observe only a hard label or accept/reject outcome, attempting to force misclassification toward a chosen target.

---

## **2 Class of Models the Metric Applies To**

* Image classifiers & facial-recognition models (CNNs / ResNets / ViTs)

**Primary experiments:**

* **ResNet-18** — practical baseline, initial testing, lower compute cost

* **ResNet-50** — stronger baseline

---

## **3 Working Assumptions**

* **Black-box query model:** the attacker can submit inputs and receive only the final label or decision.

* **Attack model used:** HopSkipJump (HSJ), implemented in the IBM Adversarial Robustness Toolbox.

* **Targeted objective:** attacker aims for a specific target identity/label; success \= target accepted, thereby modelling unauthorised access attempts.

* **Detection model:** detectors applied per query.

* **Budgeting:** the attack halts on success or when the query budget is reached.

---

## **4 Scientific References (Papers & Grounding)**

* Dong Y. et al. (2019). *Efficient Decision-Based Black-Box Adversarial Attacks on Face Recognition.* CVPR 2019\.

* Chen J., Jordan M., Madry A. (2020). *HopSkipJumpAttack: A Query-Efficient Decision-Based Attack.* CVPR 2020\.

* Sharif M. et al. (2016). *Accessorize to a Crime: Real and Stealthy Attacks on State-of-the-Art Face Recognition.* CCS 2016\.

* Carlini N., Wagner D. (2017). *Towards Evaluating the Robustness of Neural Networks.* IEEE S\&P 2017\.

* Madry A. et al. (2018). *Towards Deep Learning Models Resistant to Adversarial Attacks.* ICLR 2018\.

* OECD.AI catalogue — *Time until adversary’s success* **(conceptual ancestor)**

**Toolkits / implementations**

* **Adversarial Robustness Toolbox (ART)** — HopSkipJump, Square, Carlini & Wagner attacks

* **ptflops / fvcore / torch.profiler** — FLOP estimation

* **PyTorch / torchvision** — training & models

* **A4S evaluation framework** — metric integration via @model\_metric

---

## **5 Dataset**

**CelebA** — publicly available facial-attribute dataset suitable for research use.

A lively **demonstration** dataset (PAES ≈ “pays”) that can also communicate robustness concepts clearly to **non-technical stakeholders**.

 [https://mmlab.ie.cuhk.edu.hk/projects/CelebA.html](https://mmlab.ie.cuhk.edu.hk/projects/CelebA.html)

