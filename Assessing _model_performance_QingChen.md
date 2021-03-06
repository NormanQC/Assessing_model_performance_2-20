
# Scriber: Qing Chen

# Assessing model accuracy

Suppose we have n features: f1,f2...fn

We want to learn a function from these features, but we don't know how many features should we use as our model. That's the question we want to work on. Sometime the more feature the model learn, the better. But sometime when we build the new dataset to the model, the error might be large, the prediction is not good.

How do we evaluate the model, how to get the best model is what we are going to talk about today.

# Generalization performance

- use data set train a model
- use **new data set** test model
- get the performance of model

# Measuring generalization performance: theory
- zero-one loss for classification:
$$l(Y,\hat{f}(X)) = \text{I}(Y \neq \hat{f}(X))$$
$\text{I}(Y \neq \hat{f}(X))$ means if the condition in the bracket is true: 1, false:0


<img src="https://raw.githubusercontent.com/NormanQC/Assessing_model_performance_2-20/master/prediction%20table.PNG" width=40%>

- How many not true
- How much lossing by learning
- How much need to be improved

# Cross validation
### We divide a dataset into three part: training split, validation split , test split
- training split：fit data in model
- validation split: to estimate how well model has been trained and to estimate model properties(mean error for numeric predictors, classification errors for classifiers)
- test split: to assess the performance of model when facing new data, we could use the test set to estimate the error rate after we have chosen the final model
- Typical data splits are 60%-30%-10% for training, validation, and testing
- in practice 80%-20% for training and testing splits

# $k$-fold cross validation
#### process
- we divide data into K fold(subsamples), (k-1) training fold and 1 test fold
- the cross-validation process in then repeated k times, with each of the k fold used exactly once as the validation data
- each k result has MES(mean squared error) 
- compute the average of MES 
- to estimate how accuately a predictive model will perform in practice

# Bias and Vairance decomposition of test error

- **Loss function:**
$$\begin{align}
E[(Y-\hat{Y})^2] &= E[(f(X)+\epsilon-\hat{f}(X))^2]\\
&=E[(f(X)+\epsilon-\hat{f}(X)+E[\hat{f}(X)]-E[\hat{f}(X)])^2]\\
&=E[f(X)-E[\hat{f}(X)]+E[\hat{f}(X)]-\hat{f}(X)+\epsilon)^2]\\
\end{align}$$

$$a=f(X)-E[\hat{f}(X)]$$

$$b=E[\hat{f}(X)]-\hat{f}(X)$$

$$\epsilon=error$$

$$(a+b+\epsilon)^2=a^2+b^2+\epsilon^2+2ab+ab\epsilon+2a\epsilon$$

$$\begin{align}
&=E[((f(X)-E[\hat{f}(X)])^2+(E[\hat{f}(X)]-\hat{f}(X))^2+\epsilon^2+2(f(X)-E[\hat{f}(X)])(E[\hat{f}(X)]-\hat{f}(X))\\
&+2(f(X)-E[\hat{f}(X)])\epsilon+2(E[\hat{f}(X)]-\hat{f}(X))\epsilon]\\
&=E[((f(X)-E[\hat{f}(X)])^2]+E[(E[\hat{f}(X)]-\hat{f}(X))^2]+E[\epsilon^2]+2E[(f(X)-E[\hat{f}(X)])(E[\hat{f}(X)]-\hat{f}(X))]\\
&+2E[(f(X)-E[\hat{f}(X)])\epsilon]+2E[(E[\hat{f}(X)]-\hat{f}(X))\epsilon]\\
\end{align}$$

- since $E(\epsilon)=0$
$$2E[(f(X)-E[\hat{f}(X)])\epsilon]=2E((f(X)-E[\hat{f}(X))E(\epsilon)=0$$
$$2E[(E[\hat{f}(X)]-\hat{f}(X))\epsilon]=2E((E[\hat{f}(X)]-\hat{f}(X))E(\epsilon)=0$$

- since $E[E[\hat{f}(X)]=E(\hat{f}(X))$
$$E[\hat{f}(X)]-\hat{f}(X)=E[E[\hat{f}(X)]-E(\hat{f}(X))=E(\hat{f}(X))-E(\hat{f}(X))=0$$

$$2E[(f(X)-E[\hat{f}(X)])(E[\hat{f}(X)]-\hat{f}(X))]=0$$

$$=\underbrace{E[((f(X)-E[\hat{f}(X)])^2]}_{\text{(bias)^2}}+\underbrace{E[(E[\hat{f}(X)]-\hat{f}(X))^2]}_{\text{variance}}+\underbrace{E[\epsilon^2]}_{\text{non reducable term}}$$

# Overfitting vs Underfitting
- **bias**: how far are the predicted values from the actual values. If the average predicted values are far off from the actual values then the bias is high
- **variance**: Variance occurs when the model performs good on the trained dataset but does not do well on a dataset that it is not trained on, like a test dataset or validation dataset. Variance tells us how scattered are the predicted value from the actual value.
<img src="https://raw.githubusercontent.com/NormanQC/Assessing_model_performance_2-20/master/overfitting.PNG" width=60%>

#### overfitting: low bias but high variance
#### underfitting: high bias but low variance

# The Bias-Variance decomposition: the learning curve

<img src="https://raw.githubusercontent.com/NormanQC/Assessing_model_performance_2-20/master/KNN.PNG" width=50%>

- **knn**: when K increase, bias increase, variance decrease

<img src="https://raw.githubusercontent.com/NormanQC/Assessing_model_performance_2-20/master/linear.PNG" width=50%>

- **linear regression**: when the number of features increase, bias decrease, variance increase

# Confusion matrix
- **True positive**: data points labeled as positive that are actually positive
- **False positives**: data points labeled as positive that are actually negative
- **True negatives**: data points labeled as negative that are actually negative
- **False negatives**: data points labeled as negative that are actually positive

<img src="https://raw.githubusercontent.com/NormanQC/Assessing_model_performance_2-20/master/confusion%20table.PNG" width=100%>

#### Precision = TP/(TP+FP)= 1/2
#### Sensitivity = TP/(TP+FN) = 1/2
#### Specificity = TN/(TN+FP) = 2/3

# Roc curve
- evaluation the performance of model
- if larger the area below the Roc curve the better the model is
- **AUC** is the area below the Roc curve
