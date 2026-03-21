# ANN
## 1. Implementation (on methods)
### Feature Engineering
Text data is transformed into fixed-length numerical feature vectors using a sentence transformer (all-MiniLM-L6-v).

### Model Architecture
The ANN is a fully connect feedforward neural network that takes fixed-length feature vectors as input. The size of the input vectors is determined by the size of the feature representation.

The network has two fully connected hidden layers:
- The first layer maps the input to 256 dimensions, followed by a ReLU activation and a dropout layer (rate = 0.2) for regularization.
- The second layer reduces the representation to 128 dimensions using another ReLU activation.

The 128-dimensional vector serves as a shared representation. The network then branches into three output heads:
- Emotion regression
- Empathy regression
- Emotional polarity classification

### Training Procedure
The model is trained using the Adam optimizer with a learning rate of 1e-3. A multi-task loss function is used, combining mean squared error for regression tasks and cross-entropy loss for classification.

Training is performed over 80 epochs with a batch size of 64.

### Evaluation
Regression performance is measured using Mean Absolute Error (MAE). Classification performance is evaluated using precision, recall, and F1-score.

## 2. Results (on dev set)
```Bash
******** Regression Metrics ********
Emotion Mean Square Error:  0.5670906901359558
Empathy Mean Square Error:  0.7678810358047485

******** Classification Metrics ********
              precision    recall  f1-score   support

           0       0.69      0.24      0.36       170
           1       0.61      0.69      0.65       461
           2       0.63      0.71      0.67       359

    accuracy                           0.62       990
   macro avg       0.64      0.55      0.56       990
weighted avg       0.63      0.62      0.60       990
```

## 3. Notes (preprocessing/model choices)
Converted all text into lowercase and trimmed leading/trailing whitespace. 

## 4. How to Run
+ Import RNN.ipynb into Google Colab
+ Create a data/ directory
+ Upload trac2_CONVT_[dev | test | train].csv files and move into data/
+ Press Run all

# RNN
## 1. Implementation (on methods)
### Encoding / Feature Engineering
A vocabulary is constructed from the training dataset using token frequency counts. Each unique word is assigned a unique integer index, with special tokens for padding (`<PAD> = 0`) and unknown words (`<UNK> = 1`).

Input text is tokenized by splitting on whitespace and converted into sequences of integer IDs. All sequences are padded or truncated to a fixed length of 50 tokens to ensure consistent input size for the model.

### Model Architecture
The model is a multi-task bidirectional GRU-based neural network. Input sequences are first passed through an embedding layer (dimension 100), followed by a bidirectional GRU with a hidden size of 128.

The final hidden states from both directions are concatenated and passed through a shared fully connected layer. The network then branches into three output heads:
- Emotion regression
- Empathy regression
- Emotional polarity classification

### Training Procedure
The model is trained using the Adam optimizer with a learning rate of 1e-3. A multi-task loss function is used, combining mean squared error for regression tasks and cross-entropy loss for classification.

Training is performed over 80 epochs with a batch size of 64. The dataset is shuffled during training.

### Evaluation
Regression performance is measured using Mean Absolute Error (MAE). Classification performance is evaluated using precision, recall, and F1-score.

## 2. Results (on dev set)
```Bash
******** Regression Metrics ********
Emotion Mean Square Error:  0.6293336101971412
Empathy Mean Square Error:  0.6799753016453804

******** Classification Metrics ********
              precision    recall  f1-score   support

           0       0.51      0.34      0.41       143
           1       0.62      0.65      0.64       498
           2       0.66      0.68      0.67       468

    accuracy                           0.62      1109
   macro avg       0.59      0.56      0.57      1109
weighted avg       0.62      0.62      0.62      1109
```

## 3. Notes (preprocessing/model choices)
Converted all text into lowercase and trimmed leading/trailing whitespace. 

## 4. How to Run
+ Import RNN.ipynb into Google Colab
+ Create a data/ directory
+ Upload trac2_CONVT_[dev | test | train].csv files and move into data/
+ Press Run all
