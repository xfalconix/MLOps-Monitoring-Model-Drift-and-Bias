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

This case study taught me that the difference between a data scientist and an engineer with operational experience is exactly what determines whether the model survives in production. After 10+ years in maritime operations and a Master's in Big Data & AI, I see it from both sides.

Case: CNN for automated biofouling recognition on vessel hulls.

Biofouling matters through two lenses: operational (hydrodynamic drag, fuel consumption, emissions under CII/EEXI) and regulatory/biosecurity (non-indigenous species, governed by standards like New Zealand's clean hull standard). The case below sits on the regulatory side, but the concepts apply to both — and it's the perfect lens to tell apart the two types of drift, plus bias.

A landmark study is Mannix et al. (2021, Nature Scientific Reports), which trained a CNN on 10,000+ expert-annotated images, reaching agreement on par with human expert reviewers.

Where it gets interesting:

🔹 Feature drift: the paper's images come from three organisations (DAWE-Australia, MPI-New Zealand, CSLC-California), mostly international vessels arriving at those ports. Deploy that model in the Strait of Gibraltar — where I spent years coordinating hull inspection and cleaning for shipping lines like MAERSK, CMA CGM and SVITZER — and the inputs shift: different Mediterranean biota, niche areas (sea chests, propellers, sea boxes), different turbidity and lighting. Pixel distributions move, even if the rule "more coverage = higher fouling" still holds. The authors themselves acknowledge local fine-tuning would improve performance.

🔹 Concept drift: ground truth isn't a physical law, it's human labels — and experts only agree 89% of the time. If regulators tighten the clean hull standard tomorrow (New Zealand and Australia are heading there), the same image goes from acceptable to non-compliant without anything in it changing. Pure concept drift driven by governance.

And then there's bias, which deserves the same scrutiny:

🔹 Training data bias: heavily imbalanced — most images are clean hulls (SLoF 0), only ~10% severe fouling (SLoF 2). The model can learn to "play it safe" and fail on the cases most critical for biosecurity.

🔹 Label bias: human experts only agree 89% of the time. One annotator's judgement becomes the model's "truth".

🔹 Operational representation bias: transoceanic vessels at strict-biosecurity ports are overrepresented; fishing fleets and coastal traffic, underrepresented. Performance degrades precisely where data is scarcest. The decision isn't technical — it's ethical and regulatory.

Applying ML to maritime services isn't just about model accuracy. It's about understanding where the model came from, where you're deploying it, and what shifts in the world will eventually break it.


> Mannix, E.J. et al. (2021). Automating the assessment of biofouling in images using expert agreement as a gold standard. *Scientific Reports* 11, 2739. https://doi.org/10.1038/s41598-021-82024-x
