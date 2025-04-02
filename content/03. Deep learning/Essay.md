## Essay 1

# Learn From the Learnt: Source-Free Active Domain Adaptation via Contrastive Sampling and Visual Persistence, ECCV 2024

  

### Background
This research introduces a novel paradigm called "Source-Free Active Domain Adaptation (SFADA)." The approach tackles two realistic constraints simultaneously: the inability to access source domain data and the need to selectively label only a portion of target domain data. In practical scenarios, constraints such as privacy laws, data ownership issues, and labeling costs frequently emerge, necessitating effective domain adaptation methods that can operate under these limitations.
### Method

The SFADA methodology comprises the following key components:

1. **Learn from the Learnt (LFTL)**: An iterative framework that builds upon models pre-trained on the source domain, strategically identifying the most valuable data in the target domain and gradually enhancing the model's performance.
2. **Contrastive Active Sampling (CAS)**: This approach extends beyond simple uncertainty measurements by employing contrastive loss to prioritize information-rich samples that can improve diversity in model learning.
3. **Visual Persistence-guided Adaptation (VPA)**: A self-supervised learning technique that applies various transformations to target domain images and guides these transformed versions to form clusters around the original image in the feature space.

The entire SFADA process operates iteratively by selecting information-rich samples through CAS, labeling them, and then updating the model by combining this labeled data with self-supervised learning through VPA. Throughout this process, the balance between preserving source model knowledge and acquiring target domain characteristics is dynamically adjusted.
### Experimental Results and Significance
The researchers evaluated SFADA on three standard domain adaptation benchmarks (VisDA-C, Office-Home, Office-31) and achieved superior performance across various labeling budgets. On VisDA-C, it demonstrated exceptional performance compared to existing SFUDA and ADA approaches with merely 1% labeling, and exhibited optimal performance with a 5% budget. For Office-Home, it showed comparable results at 5% and the best performance at 10%. On Office-31, it achieved favorable results in certain domains with a 5% budget.

These findings demonstrate that substantial performance improvements can be achieved with very few labels, highlighting the high labeling efficiency of the proposed methodology. Therefore, unlike existing UDA (requiring source data), SFDA (no target labels), and traditional active learning (not considering domain differences), SFADA offers a practical paradigm that simultaneously addresses two realistic constraints: source data accessibility and labeling costs, providing a more appropriate solution for real-world applications.

### Evaluation of Presenter
Problem Statement (5), Survey (5), Proposed Method & Results (5), Presentation (4)
Total: 19

  

## Essay 2

# Style Adaptation and Uncertainty Estimation for Multi-Source Blended-Target Domain Adaptation

### Background

This research addresses a novel domain challenge called Multi-source Blended-target Domain Adaptation. The researchers propose this methodology for several important reasons. First, in real-world scenarios, target domain data frequently appears in mixed forms from various distributions. Second, there are numerous cases where data sources (domain labels) cannot be identified due to privacy protection requirements, such as encrypted data stored on cloud servers. Because of these challenges, existing Blended-target Domain Adaptation (BTDA) methods primarily utilized only single source domains, making it difficult to obtain comprehensive feature information. To address this limitation, the authors introduce a new paradigm called Multi-source Blended-target Domain Adaptation (MBDA) and propose the SAUE (Style Adaptation and Uncertainty Estimation) methodology.
### Method

The SAUE approach consists of three core technologies:

  

1. **Style Adaptation**: A similarity-based style adaptation strategy that enhances source features by leveraging style information from the target domain.

  

2. **Uncertainty Estimation and Elimination**: This component models prediction uncertainty using Dirichlet distribution, then reduces the influence of incorrectly classified source samples through KL divergence.

  

3. **Adversarial Alignment without Domain Labels**: The researchers construct a lightweight adversarial learning strategy that repurposes the category classifier to discriminate the source domains of features without requiring domain labels.

  

From a theoretical perspective, the paper presents a robust mathematical foundation for the proposed method through PAC-Bayesian theory and generalization bounds.

  

### Result

The researchers thoroughly tested the SAUE model through experiments on four benchmark datasets: ImageCLEF-DA, Office-Home, DomainNet, and VisDA 2017. It achieved average accuracy of 70.9% on DomainNet, 73.7% on Office-Home, 84.3% on ImageCLEF-DA, and 81.5% on VisDA-2017, outperforming all existing methods.

  

The sensitivity analysis investigated the influence of two key hyperparameters (λ_d, λ_e), revealing that the model is relatively less sensitive to the adversarial balance parameter (λ_d) but highly sensitive to the annealing parameter (λ_e).

  

### Conclusion

The SAUE approach demonstrates exceptional performance even without target domain labels, showing significant potential in real-world applications where privacy protection is a priority.

  

### Evaluation of Presenter

Problem Statement (5), Survey (5), Proposed Method & Results (4), Presentation (4)

Total: 18

  

## Essay 3

# Universal Semi-Supervised Domain Adaptation by Mitigating Common-Class Bias

  

### Background

This research proposes Universal Semi-Supervised Domain Adaptation (UniSSDA), a novel setting that combines Universal Domain Adaptation (UniDA) and Semi-Supervised Domain Adaptation (SSDA). This framework accommodates target domains that are partially labeled, allows for source and target domain label spaces that may not completely align, and encompasses various adaptation scenarios.

  

### Common-Class Bias Problem

The researchers discovered that existing UniDA and SSDA methodologies are vulnerable to "Common-class Bias" in the UniSSDA environment. This bias manifests through the following mechanisms:

  

- Abundant labeled source data naturally skews model learning toward the source distribution

- This bias is particularly detrimental to learning target private classes that lack representation in the source domain

- The model prioritizes classes common to both domains while sacrificing performance on private classes

  

### Method

To address this challenge, the authors propose a "Prior-Guided Pseudo-Label Refinement" strategy:

  

1. **Supervised Classifier**: Introducing an additional classifier on top of the feature extractor that is trained exclusively on labeled samples

  

2. **Group Weight Readjustment**: Adjusting class group (common, source private, target private) distributions on a per-instance basis

  

3. **Classifier Decision Aggregation**: Combining decisions from both classifiers to derive final prediction probabilities

  

This approach effectively reduces the influence of source-induced bias and enhances recognition capability for target private classes. The researchers designed it specifically based on the observation that supervised classifiers demonstrate less susceptibility to common-class bias.

  

### Result

The researchers thoroughly evaluated the model's performance using the Office-Home, DomainNet, and VisDA datasets. In experiments with ResNet-34 backbone, the model achieved average accuracies of 70.7% and 68.6% on Office-Home and DomainNet respectively, representing substantial improvements of 3.1% and 5.7% over previous performance levels. Experiments with foundation models (DINOv2, CLIP) also achieved superior performance across all datasets, maintaining common class accuracy without compromising target private class accuracy.

  

### Conclusion

This research introduces UniSSDA as a novel domain adaptation paradigm and identifies the vulnerability of existing methods to common-class bias. Through the proposed methodology, the researchers effectively mitigated bias and demonstrated its superiority through comprehensive experiments.

  

### Evaluation of Presenter

Problem Statement (5), Survey (5), Proposed Method & Results (4), Presentation (5)

Total: 19

  

## Essay 4

# DA-Ada: Learning Domain-Aware Adapter for Domain Adaptive Object Detection

  

### Background

Domain Adaptive Object Detection (DADO) aims to generalize detectors trained on a labeled source domain to an unlabeled target domain. Visual-language models (VLMs) demonstrate considerable potential for DADO as they can provide essential general knowledge on previously unseen images. However, existing domain-invariant approaches suffer from bias toward the source domain. To address this limitation, this research proposes a novel Domain-Aware Adapter (DA-Ada) that leverages both domain-invariant knowledge and domain-specific knowledge.

  

### Method

The DA-Ada framework consists of two primary components:

  

1. **Domain-Invariant Adapter (DIA)**: This component learns domain-invariant knowledge by aligning feature distributions between the two domains

  

2. **Domain-Specific Adapter (DSA)**: This element recovers domain-specific knowledge from the difference between the input and output of the block, which is discarded by DIA

  

Additionally, the researchers propose a Visual-guided Textual Adapter (VTA) that embeds cross-domain information learned by DA-Ada into the textual encoder to enhance the discriminability of the detection head. This model optimizes an integrated multi-objective loss function that balances adversarial learning of domain-invariant knowledge, clear separation between domain knowledge types, and effective object detection in both source and target domains.

  

### Result

The DA-Ada approach demonstrated excellent performance across various domain adaptive object detection benchmarks. It achieved 58.5% mAP on the Cross-Weather adaptation task, 66.7% on Cross-FoV, 67.3% on Sim-to-Real, and 48.0% on Cross-Style adaptation. These results clearly indicate that DA-Ada provides consistent performance improvements across diverse environments. Notably, visualization analysis confirms that DA-Ada effectively combines domain-invariant and domain-specific knowledge to significantly enhance object detection capabilities in the target domain.

  

### Conclusion

The DA-Ada framework effectively transforms the generalized knowledge of VLMs into cross-domain information for DADO. This approach advances beyond existing methods that focus solely on learning domain-invariant knowledge by also recovering specific knowledge to improve the discriminative ability in the target domain.

  

### Evaluation of Presenter

Problem Statement (4), Survey (5), Proposed Method & Results (4), Presentation (5)

Total: 18