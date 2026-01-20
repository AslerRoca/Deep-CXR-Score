# Deep-CXR-Score

This repository describes the methodology used to develop **Deep-CXR-Score**, a hybrid AI system that predicts visual chest MRI scores in cystic fibrosis (CF) using frontal chest x-ray and pulmonary function tests (PFT).

---

## Image acquisition and assessment

### MRI

![MRI scoring system](figures/mriscoresystem.png)

Standardized chest MRI was performed after diagnosis or referral starting at ~3 months of age and then annually. Imaging was performed using three similar 1.5 T scanners from the same manufacturer (Magnetom Symphony, Magnetom Avanto, Magnetom Aera; Siemens Healthineers, Erlangen, Germany). Protocols were kept constant over the study period apart from minor adaptations to software versions (thesis references: 31, 20, 28, 29, 42, 43, 44, 23, 32, 46, 47).

In brief, T1-weighted sequences before and after IV contrast and T2-weighted sequences before contrast were acquired. Four-dimensional lung perfusion imaging used macrocyclic gadolinium-based contrast agents (0.1 mmol/kg body weight of gadobutrol or gadoteric acid) injected at 3–5 ml/s. Children ≤ 5 years were routinely sedated with oral or rectal chloral hydrate (100 mg/kg, max 2 g) and monitored with MR-compatible pulse oximetry. Renal function was checked before contrast administration. Contrast required for 4D perfusion imaging was avoided in infancy due to prescription regulations.

All MRI examinations were assessed by the same validated reader (MOW) using the established scoring system (thesis references: 31, 20, 28, 29, 42, 43, 44, 23, 32, 46, 47, 25). Prior MRI results were available to the reader for comparison. Each lobe and the lingula were scored as 0 (absent), 1 (<50% involved), or 2 (≥50% involved) for bronchiectasis/wall thickening, mucus plugging, sacculation/abscess, consolidation, special finding/pleural lesion, and perfusion abnormalities. Morphology items sum to the MRI morphology score, perfusion abnormalities sum to the MRI perfusion score, and both sum to the MRI global score (0–72).

### Chest x-ray

All chest x-rays were acquired in either posterior–anterior or anterior–posterior projection. Chest x-rays used for ETI evaluation were scored using the modified Chrispin–Norman score (frontal view only) by the same reader (MOW) (thesis references: cn_1, cn_2).

The modified Chrispin–Norman score grades CF lung damage on a single frontal film. The image is divided into four lung quadrants. Four radiographic features are assessed—bronchial line (tram-track) shadows, ring shadows, mottled (nodular) shadows, and large soft shadows representing consolidation or atelectasis. Each feature is scored per quadrant on a 3-point scale (0 = absent, 1 = mild, 2 = marked), yielding a maximum of 32 points.

---

## Image preprocessing

Image preprocessing consisted of two main steps: chest x-ray size normalization and ROI selection. FFT-based resizing was used for normalization. ROI selection included lung region cropping and separation of left and right lung regions; both were performed using two trained nnU-Net models (thesis references: 33, 51).

![Workflow for chest x-ray preprocessing](figures/fluss_scoring.drawio.png)

### Chest x-ray ROI selection

![ROI selection for chest x-ray](figures/fluss_roiselection.drawio.png)

Chest x-rays contained non-patient regions (e.g., markers, black borders, window artifacts) that could negatively affect downstream lung-halves segmentation. An automatic ROI selection pipeline was implemented using nnU-Net. The model was trained with 12 manually selected ROI bounding boxes and then applied to automatically select ROIs for the remaining ~600 chest x-rays.

### Lung halves segmentation in chest x-ray

![Lung halves segmentation](figures/optimizing.drawio.png)

An initial nnU-Net was trained on the open-source Montgomery County chest x-ray dataset (138 frontal films; 17 from patients <18 years). Performance was evaluated on 24 internal films (6 each from age bands 0–1, 1–6, 6–12, and 12–19 years). Three preprocessing schemes were evaluated to improve segmentation: global histogram equalization, CLAHE, and simple intensity remapping. A radiologist (LW) manually delineated 30 pediatric chest x-rays from the internal archive, and a new nnU-Net was retrained using the combined pediatric + Montgomery images.

---

## Model for Deep-CXR-Score

![Hybrid AI scoring model](figures/newfluss_prediction.png)

The study comprised two components:

- A **ResNet-50** model predicting item-level visual MRI scores from a single frontal chest x-ray.
- A **centroid-based classifier** using PFT parameters transformed into a 13-level quantitative scale, providing an independent “backup” estimate for MRI perfusion-related severity.

### Deep learning score chest x-ray with same-day MRI score as ground truth

Because the MRI scoring system is defined at lobar level, score items were treated independently by lobe. Due to limited lobar information in frontal x-rays, paired lung-half x-rays and corresponding MRI scores were generated and used to predict the target scoring system.

#### ResNet-50 training

A modified ResNet-50 architecture (thesis reference: 34) was trained on a single NVIDIA RTX 3060 GPU. The model was fitted on the training set, tuned on the validation set to select the best checkpoint, and evaluated on an independent test set. The model outputs lobar-level MRI scores for bronchiectasis/wall thickening, sacculation/abscess, mucus plugging, consolidation, special findings, and perfusion.

#### Modified architecture

The model begins with an 11×11 convolution followed by 3×3 max pooling, passes through the four standard ResNet stages of residual blocks, and ends with global average pooling and a fully connected regression head.

#### Input and output

Left and right lungs were analyzed separately. Each input is a 512×512 chest x-ray containing a single lung half. Instance-based min–max interval adjustment and z-score normalization were applied during training. The network outputs a 3×6 matrix (three lobes: upper, middle/lingula, lower; six pathological features), yielding lung-half lobar CF severity scores.

#### Loss function

A combined loss was used to balance pointwise accuracy and agreement with summed MRI scores:

\[
L_C = \alpha L_E + \beta L_P + \gamma L_{\rho}, \qquad
\alpha,\beta,\gamma>0,\ \alpha+\beta+\gamma=1.
\]

Where:

- Cross-entropy loss:

\[
L_E = -\frac{1}{N}\sum_{i=1}^{N}\left[
y_i \ln x_i + (1-y_i)\ln(1-x_i)
\right]
\]

- Pearson-correlation loss:

\[
L_P = 1-\mathrm{corr}_P(x,y)
= 1-\frac{\sum_{i=1}^{N}(x_i-\bar x)(y_i-\bar y)}
{\sqrt{\sum_{i=1}^{N}(x_i-\bar x)^2}\sqrt{\sum_{i=1}^{N}(y_i-\bar y)^2}}
\]

- Spearman-correlation loss (with ranks \(r_i=\mathrm{rank}(x_i)\), \(s_i=\mathrm{rank}(y_i)\)):

\[
L_{\rho} = 1-\mathrm{corr}_S(x,y)
= 1-\frac{\sum_{i=1}^{N}(r_i-\bar r)(s_i-\bar s)}
{\sqrt{\sum_{i=1}^{N}(r_i-\bar r)^2}\sqrt{\sum_{i=1}^{N}(s_i-\bar s)^2}}
\]

Weights were tuned on the validation set; the final model used \(\alpha=0.5\), \(\beta=0.3\), \(\gamma=0.2\), emphasizing classification error while enforcing linear and ordinal agreement with MRI reference scores.

#### Optimizer and learning-rate schedule

AdamW (thesis reference: adam) was used with step-decay learning-rate scheduling. The best initial learning rate in this study was 0.0003.

#### Visualization of activation maps (Grad-CAM)

Grad-CAM was used to visualize model attention (thesis reference: 52). The pretrained ResNet-50 was run on all test-set x-rays, attention maps were computed from the fourth convolutional layer (feeding lobar predictions), and maps were aligned, normalized, and averaged across the test cohort to create a composite activation map.

---

## Statistical analyses

A 3-class F1 score was used to evaluate lobar-level prediction quality:

\[
0.10-0.50=\text{not good},\quad
0.50-0.80=\text{ok},\quad
0.80-0.90=\text{good},\quad
0.90-1.00=\text{very good}.
\]

Agreement between predicted and reference lobar scores was assessed using **linear weighted Cohen’s kappa**:

\[
\le 0=\text{no agreement},\quad
0.01-0.20=\text{none to slight},\quad
0.21-0.40=\text{fair},\quad
0.41-0.60=\text{moderate},\quad
0.61-0.80=\text{substantial},\quad
0.81-1.00=\text{almost perfect agreement}.
\]

Spearman rank correlation \(\rho\) was used to compare predicted and reference summary scores, interpreted as:

\[
0.10-0.39=\text{weak},\quad
0.40-0.69=\text{moderate},\quad
0.70-0.89=\text{strong},\quad
0.90-1.00=\text{very strong}
\]
(thesis reference: 35).

Group comparisons used Mann–Whitney U tests; score changes were assessed via ANOVA on ranks and Wilcoxon signed-rank tests. Statistical significance was set at **P < 0.05**. Multiple-comparison correction (e.g., Bonferroni–Holm) was applied as appropriate.
