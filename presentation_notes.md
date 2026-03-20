# Presentation Notes 

## Overview of Project

    - Approximately 25,000 people in the US are diagnosed with multiple sclerosis (MS) each year. This equates to roughly 8 new cases per 100,000 U.S. citizens.
    - MS commonly causes gait (walking style/pattern) dysfunction as it can damage a person's nerves, cause excessive fatigue, and lead to muscle weakening.
    - Wearable devices, like Fitbits, provide passive data collection on a person's walking habits.
    - Goal: Can we use hourly data from wearable devices to distinguish healthy patients from those with MS.
    - Long-term Goal: If our results show promise, can Fitbits be used as a MS screening tool?

## Dataset

    - Three public Fitbit datasets were used: Barka (44 MS), Fitbit Kaggle (32 Healthy), LifeSnaps (71 Healthy)
    - Each dataset was stripped down to three components: (1) Id, (2) datetime, (3) number of steps
    - Each participant was capped so that they could only contribute 720 hours (30 days) in order to standardize the observation window.
    - Limitation: Given that our dataset was comprised of either healthy individuals or patients with MS, confounding likely biases our data in some way. Unfortunately, that is a limitation that is largely unavoidable given the goal of our project and the decision for us to use only open-source data.

## Aims

    - Aim 1: Create day-level features from step count data and evaluate their performance.
    - Aim 2: Establish how many days of data are needed for reliable patient-level classification.
    - Aim 3: Establish under realistic MS prevalence (1:400) screening tradeoffs at high sensitivity.

## Aim 1

    - Aggregate hourly step count data to day level features: total steps, CV, active hour, time-of-day
    - Logistic regression w/ inverse frequency weights, RobustScaler() used to handle right-skewness
    - Mean AUC = 0.84, Prec = 0.54, Recall = 0.83, F1 = 0.61. Complications due to class imbalance + dataset confounding

## Aim 2

    - Averaged predicted probabilities across 'K' days per patient for patient-level score. 
    - K evaluated at 1-7, 14, 21, and 28 days. AUC stabilized around 0.93 ~ 5-7 days. Minimal gains 7+
    - Practical implication: 1 week likely sufficient for stable classification.

## Aim 3

    - 500 GroupShuffleSplit iterations, threshold derived training ROC curve, which was applied to test set
    - 85% Sensitivity: 7100 FP per 100k, 34 FP per TP; 95% Sensitivity: 28500 FP per 100k, 123 FP per TP
    - PPV drops from 3.4% to < 1% as sensitivity increases. Lots of false alarms as expected. 
    - Final takeaway: Implementing model with current training data limitations would generate a massive referral burden. Larger dataset is needed for better patient-level discrimination. 