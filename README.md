# MLOps: Monitoring Model Drift and Bias

This repository contains my personal rework of the notebooks from the LinkedIn Learning course **[MLOps Essentials: Monitoring Model Drift and Bias](https://www.linkedin.com/learning/mlops-essentials-monitoring-model-drift-and-bias)** (2023), instructed by **Kumaran Ponnambalam**.

The original notebooks are part of the course material. I have rebuilt them from scratch, adjusting the visual style and significantly extending the inline comments to reinforce my own understanding. All comments are written in **Spanish**.

**Course certificate:** [View on LinkedIn](https://www.linkedin.com/learning/certificates/a258462ee55bbd8fc518baf4c6d94597609537448bb3472bf29f4c17d01d72cf?trk=share_certificate)

`#mlops` `#artificialintelligence`

---

## About the course

> As more and more ML models are developed and deployed, the need arises to ensure that they are effective, safe, and performing as desired. Model monitoring, a core function of MLOps, helps data scientists and MLOps engineers meet this need. Kumaran Ponnambalam covers the types of monitoring needed for ML models, goes deep into drift monitoring and bias, explains different detection techniques, and demonstrates how to execute them in Python using open-source libraries.

**Instructor:** Kumaran Ponnambalam — data professional with 20+ years of experience.

---

## Notebooks in this repository

| File | Topic |
|------|-------|
| `code_03_XX Drift Detection Example.ipynb` | Feature drift and concept drift detection using statistical tests |
| `code_06_03 Equal Opportunity Score with sklego.ipynb` | Bias measurement: Equal Opportunity Score with the `sklego` library |

Both notebooks use a credit-approval dataset (`credit-approval-training-data.csv`, `credit-approval-prod-data.csv`, `credit-approval-fair-data.csv`) to illustrate real-world drift and bias scenarios.

---

## Real-world reflection: drift and bias in a CNN for biofouling recognition

The concepts covered in this course are not abstract. Here is a case that illustrates all of them — one I find particularly compelling given my professional background in maritime services.

### Use case: automated biofouling recognition on vessel hulls

Biofouling matters through two distinct lenses: the **operational** one (hydrodynamic drag, fuel consumption, emissions under CII/EEXI) and the **regulatory/biosecurity** one (introduction of non-indigenous species, governed by standards such as New Zealand's clean hull standard and Australia's equivalent proposals). The case analyzed here sits on the regulatory side, but the concepts apply to both.

Deep learning with CNNs is already being applied to classify hull biofouling levels from in-water inspection images. A landmark study is **Mannix et al. (2021, *Nature Scientific Reports*)**, which trained a CNN on more than 10,000 expert-annotated images, achieving agreement on par with a human expert reviewer.

This is where it gets interesting from an MLOps perspective.

**Feature drift** — The paper's images come from three organisations (DAWE in Australia, MPI in New Zealand, CSLC in California), captured mostly on international vessels arriving at those ports. Deploying that model in the Strait of Gibraltar — where I spent years coordinating hull inspection and cleaning services for major shipping lines such as MAERSK, CMA CGM, and SVITZER — would expose it to a shifted input distribution: different Mediterranean biological communities, niche areas (sea chests, propellers, sea boxes) with specific local biota, different turbidity and underwater lighting conditions. The pixel distributions shift, even though the rule *"more biological coverage = higher fouling level"* still holds. The authors themselves acknowledge that local fine-tuning would improve performance.

**Concept drift** — The model's ground truth is not a physical law; it is human labels, and experts only agree with each other 89% of the time. If regulators tighten the clean hull standard tomorrow — and New Zealand and Australia are already moving in that direction — the same image goes from *acceptable* to *non-compliant* without anything in it changing. The input distribution is identical, but the input → label relationship has shifted. That is pure concept drift driven by governance.

**Training data bias** — ~10,000 images from three jurisdictions, heavily imbalanced: most are clean hulls (SLoF 0), with only ~10% corresponding to severe fouling (SLoF 2). A model trained on this distribution can learn to "play it safe" and fail precisely on the cases most critical for biosecurity enforcement.

**Label bias** — Ground truth is provided by human experts who only agree 89% of the time. The judgement of a specific annotator becomes the model's "truth", embedding that individual's threshold into every future prediction.

**Operational representation bias** — Fleets historically receiving more inspections (transoceanic vessels arriving at strict-biosecurity ports) are overrepresented. Fishing vessels, regional coastal traffic, and recreational craft are underrepresented. Model performance can degrade precisely on the segments with the least data. The decision on what to do about it is not purely technical — it is ethical and regulatory, and must be explicit in the system design.

**Takeaway:** Applying ML to maritime services is not just about model accuracy. It requires understanding where the model came from, where it will be deployed, and what shifts in the world — biological, operational, or regulatory — will eventually break it.

> Mannix, E.J. et al. (2021). Automating the assessment of biofouling in images using expert agreement as a gold standard. *Scientific Reports* 11, 2739. https://doi.org/10.1038/s41598-021-82024-x
