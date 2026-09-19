Data Vortex — Round 2
Sentiment & Topic Classification

A text classification system developed for Data Vortex Round 2 to classify user-generated text across two tasks:

Sentiment Classification — Negative, Neutral, Positive
Topic Classification — Account Security, Community Discussion, Feature Feedback, Technical Issues
📌 Project Overview

The project compares traditional TF-IDF-based machine learning approaches with a transformer-based DistilBERT model.

The complete pipeline includes:

Data preprocessing and cleaning
Stratified train/test splitting
Data leakage verification
Sentiment and topic distribution analysis
Word-level TF-IDF
Character-level TF-IDF
Combined word + character features
Class-weighted models for imbalanced topic classification
Hyperparameter tuning
Logistic Regression
Linear SVM
Complement Naive Bayes
DistilBERT fine-tuning
Confusion matrix analysis
Sentiment error analysis
📊 Dataset

Total samples: 9,000

Split	Samples	Percentage
Training	7,183	79.81%
Testing	1,817	20.19%
Sentiment Classes
Class	Test Samples
Negative	652
Neutral	580
Positive	585
Topic Classes
Class	Test Samples
Community Discussion	1,549
Technical Issues	178
Feature Feedback	65
Account Security	25

The topic classification task is highly imbalanced, so Macro F1 is used alongside accuracy to evaluate performance across minority classes.

🔍 Data Leakage Check

The dataset was explicitly checked for text overlap between training and testing sets.

Unique training texts: 6,320
Unique testing texts: 1,580
Text overlap: 0

This ensures that identical text samples were not shared between the training and testing sets.

🧹 Text Preprocessing

The text preprocessing pipeline includes cleaning and normalization of the raw text before feature extraction.

Example:

Original

some1 come wait in line with me Thursday at 3@target bc briana is being a bitch

Cleaned

some1 come wait in line with me thursday at 3 USERMENTION bc briana is being a bitch
🧠 Models Evaluated
1. TF-IDF Models

Both word-level and character-level TF-IDF features were evaluated.

Final feature configuration

The strongest topic-classification configuration explored was:

Word n-grams : (1, 1)
Character n-grams : (3, 5)
Features : 47,082

This configuration achieved:

Accuracy: 94.33%
Macro F1: 72.65%

Topic Classification Results
Class	Precision	Recall	F1
Account Security	0.8462	0.4400	0.5789
Community Discussion	0.9442	0.9948	0.9689
Feature Feedback	0.7059	0.3692	0.4848
Technical Issues	1.0000	0.7753	0.8734
Macro Avg	0.8741	0.6448	0.7265
2. Logistic Regression

Logistic Regression was evaluated with:

Standard class weights
Balanced class weights
Custom class weights
Different TF-IDF configurations
Hyperparameter tuning
3. Linear SVM

Linear SVM was also evaluated with balanced and unbalanced class weighting.

4. Complement Naive Bayes

Complement Naive Bayes was tested as an additional baseline, particularly for text classification.

🤖 DistilBERT

A pretrained:

distilbert-base-uncased

model was fine-tuned for both classification tasks.

Configuration
Maximum sequence length: 64
Training samples: 7,183
Test samples: 1,817
Epochs: 3
Learning rate: 2e-5
Training batch size: 16
Evaluation batch size: 32
FP16 training
GPU: NVIDIA Tesla T4
Sentiment — DistilBERT
Final Performance

Accuracy: 70.28%
Macro F1: 70.01%

Class	Precision	Recall	F1
Negative	0.7715	0.7561	0.7637
Neutral	0.6147	0.6052	0.6099
Positive	0.7133	0.7402	0.7265
Macro Avg	0.6999	0.7005	0.7001
Confusion Matrix
                 Predicted
              Neg  Neu  Pos
Actual Neg    493  114   45
       Neu    100  351  129
       Pos     46  106  433

The largest errors occur between Neutral and Positive/Negative, indicating that ambiguous sentiment remains the primary challenge.

Topic — DistilBERT
Final Performance

Accuracy: 92.96%
Macro F1: 59.54%

Class	Precision	Recall	F1
Account Security	1.0000	0.4000	0.5714
Community Discussion	0.9332	0.9916	0.9615
Feature Feedback	0.0000	0.0000	0.0000
Technical Issues	0.8994	0.8034	0.8487
Macro Avg	0.7081	0.5487	0.5954

The lower Macro F1 compared with accuracy is primarily caused by the severe class imbalance and poor recognition of the minority Feature Feedback class.

📈 Model Comparison
Sentiment

The traditional TF-IDF approaches achieved approximately 60–62% accuracy, while fine-tuned DistilBERT achieved:

Accuracy : 70.28%
Macro F1 : 70.01%

This demonstrates a substantial improvement in sentiment classification using contextual transformer representations.

Topic

The best explored TF-IDF configuration achieved:

Accuracy : 94.33%
Macro F1 : 72.65%

DistilBERT achieved:

Accuracy : 92.96%
Macro F1 : 59.54%

For topic classification, the TF-IDF approach performed better on minority-class-aware Macro F1 in the experiments conducted.

🔎 Error Analysis

A dedicated sentiment error analysis was performed.

Total sentiment errors: 544

Most frequent error patterns:

Actual	Predicted	Errors
Positive	Neutral	146
Neutral	Negative	113
Negative	Neutral	102
Neutral	Positive	98
Negative	Positive	49
Positive	Negative	36

This indicates that the main difficulty is distinguishing Neutral sentiment from Positive and Negative sentiment, particularly for short, ambiguous, or context-dependent posts.

🗂️ Project Structure
DATA_VORTEX_ROUND_2/
│
├── README.md
│
├── notebooks/
│   └── DATA_VORTEX_Round_2.ipynb
│
├── models/
│   ├── topic_model.pkl
│   ├── tfidf_word.pkl
│   ├── tfidf_char.pkl
│   ├── label_mappings.pkl
│   └── model_info.pkl
│
├── results/
│   ├── sentiment_confusion_matrix.png
│   ├── topic_confusion_matrix.png
│   └── error_analysis.csv
│
├── reports/
│   ├── Final_Report.pdf
│   └── Model_Evaluation_Report.pdf
│
├── requirements.txt
│
└── .gitignore
🛠️ Technologies
Python
Pandas
NumPy
Scikit-learn
PyTorch
Hugging Face Transformers
Hugging Face Datasets
Accelerate
Matplotlib
Seaborn
Joblib
🚀 Reproducibility

Install the required dependencies:

pip install -r requirements.txt

Then open:

notebooks/DATA_VORTEX_Round_2.ipynb

and execute the notebook sequentially.

Note: The raw dataset and large DistilBERT model.safetensors file are not included in this repository because of file-size/submission constraints. The trained lightweight TF-IDF models and associated metadata are included.

📌 Key Findings
The dataset contains 9,000 text samples.
No text overlap was found between train and test sets.
Sentiment classification is substantially more challenging than topic classification.
DistilBERT improved sentiment performance to approximately 70% Macro F1.
Topic classification benefits strongly from TF-IDF features.
Topic imbalance significantly affects Macro F1.
Feature Feedback remains the most difficult topic class to identify.
Error analysis shows that sentiment ambiguity, especially around the Neutral class, is the primary source of sentiment errors.
👥 Project

Data Vortex — Round 2

Text Classification & NLP Pipeline
Sentiment Analysis + Topic Classification
