# Discovering Complex Behaviours in Bees Using CNN Autoencoders

**Final-Year Computer Science Dissertation | University of Sussex | 2025**

An exploratory machine-learning project investigating whether recurring patterns of bee movement can be identified from three-dimensional trajectory data without manually labelled behaviours.

The project combines time-series preprocessing, a convolutional autoencoder built in PyTorch, and unsupervised clustering to investigate potential behavioural patterns.

## Project Overview

Understanding animal behaviour is important for research in ecology, neuroscience and environmental monitoring. However, manually identifying behaviours from large quantities of movement data can be time-consuming and subjective.

This project explores a self-supervised approach to behavioural analysis, using a convolutional autoencoder to learn representations of bee movement before applying clustering algorithms to identify recurring patterns.

The aim was to investigate whether meaningful behavioural structure could be extracted directly from movement trajectories, rather than relying on predefined behavioural categories.

## Dataset

The dataset originated from a controlled behavioural experiment conducted at Queen Mary University of London.

It consisted of:

- Three-dimensional (X, Y, Z) movement trajectories from 50 bees.
- Position measurements recorded at 30 Hz.
- Recordings containing irregular timestamps, missing observations and discontinuities.
- Unlabelled movement data, requiring an unsupervised approach to pattern discovery.

The original dataset was supplied for academic research and is not included in this repository.

## Methodology

### 1. Data Preprocessing

A preprocessing pipeline was developed to prepare the raw movement trajectories for machine learning.

The pipeline included:

- Identifying timestamp resets and discontinuities.
- Splitting trajectories into continuous segments to prevent interpolation across large gaps.
- Removing segments that were too short for meaningful analysis.
- Applying cubic spline interpolation to address missing observations within suitable segments.
- Resampling trajectories to a consistent frequency of 30 Hz.
- Dividing trajectories into two-second windows containing 60 frames.
- Standardising coordinate values using training-set statistics.

Preprocessing was particularly important because errors introduced at this stage could significantly affect reconstruction performance and the quality of the learned representations.

### 2. Convolutional Autoencoder

A one-dimensional convolutional autoencoder was developed using PyTorch.

The model consisted of an encoder that compressed movement sequences into latent representations and a decoder that reconstructed the original trajectories.

**Model characteristics:**

- Input: 60-frame windows of 3D coordinates.
- Architecture: 1D convolutional encoder-decoder.
- Learning objective: minimise reconstruction error.
- Loss function: Mean Squared Error (MSE).
- Training: self-supervised, without behavioural labels.

Several architectural and training configurations were investigated, including changes to the learning rate and latent representation.

### 3. Clustering and Evaluation

Following training, latent representations were extracted from the encoder and grouped using MiniBatch K-means clustering.

Different cluster counts were investigated, with performance assessed using:

- Silhouette scores.
- Calinski–Harabasz index.
- Davies–Bouldin index.
- Cluster inertia.
- Visual inspection and dimensionality-reduction techniques.

These metrics were used to assess how clearly the learned representations could be separated into groups.

## Results

The final autoencoder achieved a reported validation reconstruction MSE of approximately **0.0412**.

Clustering experiments produced a reported silhouette score of **0.822** on the full dataset, suggesting that the learned representations contained distinguishable groups.

An important finding was the impact of preprocessing quality. Identifying and correcting a repeated interpolation issue improved model performance without requiring changes to the underlying architecture.

However, well-separated clusters do not necessarily correspond to distinct biological behaviours. Without independently labelled behavioural observations, the clusters could not be conclusively assigned behavioural meanings.

The project therefore demonstrates the potential of representation learning for exploratory animal movement analysis, rather than a validated system for identifying specific bee behaviours.

## Technologies Used

- **Python** — data processing and machine-learning development
- **Pandas and NumPy** — dataset manipulation and numerical operations
- **SciPy** — interpolation and signal preprocessing
- **PyTorch** — convolutional autoencoder implementation and training
- **Scikit-learn** — clustering and evaluation
- **Matplotlib** — visualisation of trajectories, training results and clustering

## Challenges and Lessons Learned

**Working with irregular time-series data:** Raw trajectories contained missing observations, timestamp resets and discontinuities. Developing a reliable preprocessing pipeline was essential to avoid introducing misleading movement patterns.

**Investigating model performance:** Experiments with different learning rates and architectures highlighted the importance of systematic evaluation. Correcting preprocessing problems proved particularly valuable.

**Interpreting unsupervised learning results:** Strong clustering metrics indicated structure within the learned representations, but did not establish the biological meaning of those clusters. This highlighted the importance of distinguishing mathematical separation from real-world interpretation.

## Limitations and Future Work

The project has several limitations:

- Clusters were not independently validated against observed or manually labelled behaviours.
- The dataset was collected under controlled experimental conditions, limiting conclusions about behaviour in natural environments.
- K-means clustering assumes relatively simple cluster structures and may not capture more complex behavioural relationships.

Potential future improvements include comparing alternative clustering algorithms, investigating temporal relationships between movement patterns, and validating clusters against expert behavioural annotations.

## About This Project

This project was completed as my final-year dissertation for a BSc (Hons) in Computer Science at the University of Sussex.

It brought together my interests in machine learning, data science and ecology, while providing practical experience in processing complex datasets, developing neural networks and critically evaluating model results.

**Author:** Alys Williams  
**Institution:** University of Sussex  
**Project year:** 2025
