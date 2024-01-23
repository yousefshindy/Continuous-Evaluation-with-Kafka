# **Continuous Evaluation with Kafka**

This project involves the development and comparison of two models - a Static Model and a Dynamic Model - for the classification of attacks. The Static Model focuses on a one-time analysis of the data and model training, while the Dynamic Model continuously adapts and improves its performance through incremental learning.

## **Static Model**
### **Statistical Analysis of Data**

Performed a statistical analysis of the data, identifying object data types, duplicated rows, and null values. Skewness analysis, word clouds, and KDE plots were used for further insights. After removing duplicated rows and null values, I transformed and modified specific columns based on statistical observations.

### **Data Imbalance and Feature Selection**
Observing a distribution of 43.47% to 56.53% between Attacks and Non-Attacks, I applied feature selection techniques, including Mutual Information, ANOVA, and Chi-squared methods. The top 8 features from each method were saved.

### **Model Training and Evaluation**
The data was split into training and testing datasets (80% to 20% ratio), and performance metrics, with a focus on F1-score, were established. Decision Tree and XGBoost models were selected and compared based on various feature sets. XGBoost emerged as the champion model, excelling with Mutual Information features.

```markdown
+------------------------------------------------------------------------------+
|                                      F1                                      |
+----------------+--------------+-------------+----------------+---------------+
|     Model      | All Features | MI Features | ANOVA Features | Chi2 Features |
+----------------+--------------+-------------+----------------+---------------+
| Decision Tree  |   0.49035    |   0.80547   |    0.80562     |    0.80557    |
|    XGBoost     |   0.80547    |   0.80559   |    0.80554     |    0.80554    |
+----------------+--------------+-------------+----------------+---------------+
```

### **Hyperparameter Tuning**
Fine-tuning was applied to both Decision Tree and XGBoost models using ANOVA and Mutual Information features, respectively. Results showed that XGBoost outperformed Decision Tree on the testing data, indicating better generalization.

## **Dynamic Model**
### **Data Processing and Cleaning**
For each iteration, the model reads 1000 rows from Kafka, cleans the data, and transforms it using the scaler previously fitted in the Static Model. The read_1000_rows and clean_df functions play a crucial role in this data processing phase.

### **Model Evaluation**
Both the Static Model and Dynamic Model are evaluated on the acquired data. The Static Model evaluation employs the features selected during its training phase, while the Dynamic Model evaluation uses the same features. Retraining is triggered if the F1-score falls below a predefined threshold (F1_threshold).

### **Model Retraining**
If the F1-score of the Dynamic Model drops below the threshold, the model undergoes retraining using the retrain_model function. The XGBoost's continuation training (xgb_model) allows the model to learn from new subsets of data, contributing to incremental performance improvement.

### **Results and Analysis**
The Dynamic Model consistently outperforms the Static Model, particularly when the F1-score falls below the predefined threshold. The retraining process, facilitated by the continuous learning capabilities of XGBoost, ensures that the model adapts to evolving patterns in the data.

### **Advantages and Limitations**
The Dynamic Model exhibits notable advantages, including continuous improvement through adaptation to new data, leveraging the incremental learning capabilities of XGBoost for streaming data, and consistently outperforming the Static Model across various metrics. However, there are associated limitations to consider. Continuous learning introduces the risk of overfitting to recent data, particularly if the model is overly flexible. The effectiveness of the retraining process is dependent on the quality of the initial model, and an inferior initial model may hinder significant improvements. Additionally, the retraining process increases the time taken by the dynamic model after each iteration, potentially posing challenges for long-running processes.





