# Learning Probability Density Functions using GAN

## Overview
This repository contains the solution for the assignment **“Learning Probability Density Functions using Data Only”**.  
The objective of this assignment is to learn an **unknown probability density function (PDF)** of a transformed random variable using a **Generative Adversarial Network (GAN)**, without assuming any parametric form of the distribution.

The GAN learns the distribution implicitly using only data samples.

---

## Dataset
- **Dataset:** India Air Quality Data  
- **Source:** Kaggle  
- **Feature Used:** NO₂ concentration  

### Data Preprocessing
The dataset is preprocessed by:
- Removing missing values
- Removing invalid negative values
- Normalizing the NO₂ data for stable GAN training

The cleaned and normalized NO₂ samples are used for further transformation and modeling.

---

## Transformation of Data
Each NO₂ sample \( x \) is transformed into a new variable \( z \) using the following function:

z = T_r(x) = x + a_r sin(b_r x)

where the parameters depend on the university roll number \( r \):

a_r = 0.5 (r mod 7)

b_r = 0.3 ((r mod 5) + 1)
### Computed Parameters
For my university roll number **102317212**, the computed parameters are:
- **\( a_r = 2.0 \)**
- **\( b_r = 0.89 \)**

These parameters are computed programmatically and applied to all input samples.

---

## Methodology

### GAN Architecture
A **one-dimensional Generative Adversarial Network (GAN)** is designed to learn the unknown distribution of the transformed variable \( z \).

#### Generator
- **Input:** Gaussian noise sampled from a standard normal distribution \( \mathcal{N}(0,1) \)
- **Architecture:**
  - Fully connected layer with 64 units and ReLU activation
  - Fully connected layer with 64 units and ReLU activation
  - Fully connected output layer with 1 unit
- **Output:** Generated samples \( z_f \)

The generator maps noise samples to synthetic transformed samples:
\[
z_f = G(\epsilon)
\]

---

#### Discriminator
- **Input:** Scalar value \( z \)
- **Architecture:**
  - Fully connected layer with 64 units and LeakyReLU activation
  - Fully connected layer with 64 units and LeakyReLU activation
  - Fully connected output layer with Sigmoid activation
- **Output:** Probability that the input sample is real

The discriminator distinguishes between real transformed samples and generated samples.

---

## Training Details
- **Loss Function:** Binary Cross Entropy (BCE)
- **Optimizer:** Adam
- **Learning Rate:** 0.0002
- **Batch Size:** 128
- **Number of Epochs:** 5000
- **Noise Distribution:** Standard normal distribution

The GAN is trained **only using samples of the transformed variable \( z \)**, without assuming any analytical form of the probability density function.

---

## PDF Plot Obtained from GAN Samples
After training, a large number of samples are generated from the trained generator.  
The probability density function \( p_h(z) \) is estimated using:
- Histogram-based density estimation
- Kernel Density Estimation (KDE)

The learned PDF obtained from GAN samples is shown below:

<p align="center">
<img width="912" height="610" alt="Screenshot 2026-03-02 131657" src="https://github.com/user-attachments/assets/0d22272c-8639-4d51-a46c-1e21704396bd" />
</p>

---

## Observations

### Mode Coverage
The generator is able to capture the dominant mode of the transformed data distribution effectively.  
The learned density exhibits a clear primary peak, indicating that the main structure of the distribution is preserved without severe mode collapse.

---

### Training Stability
During training, the generator and discriminator losses show oscillatory behavior, which is characteristic of adversarial learning.  
Over successive epochs, the losses stabilize, indicating balanced training between the two networks.

---

### Quality of Generated Distribution
The histogram and KDE obtained from the generated samples closely follow the empirical distribution of the transformed data.  
The learned PDF is smooth and continuous, demonstrating that the GAN can approximate a complex, non-linear density without assuming any parametric form.

---

## Repository Contents
- `Assignment4.ipynb` – Complete Google Colab notebook implementation  
- `plot.png` – PDF plot obtained from GAN samples  
- `README.md` – Project description, methodology, results, and observations  

---

## Conclusion
This assignment demonstrates the use of Generative Adversarial Networks for learning an unknown probability density function directly from data samples.  
Without assuming any parametric distribution, the GAN successfully approximates the transformed data distribution and produces a meaningful density estimate.
