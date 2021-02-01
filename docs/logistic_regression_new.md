---
layout: default
nav_order: 11
title: Binary Classification and Logistic Regression
---

# Binary Classification and Logistic Regression

This post is about Binary Classification in the context of Supervised Machine Learning Algorithms. In Binary classification, the algorithm needs to select between 2 hypothesis, for a given set of input features. For example: Will a customer will buy the product or not, based on a set of input features which consists of his income, address, occupaition, previous purchaces and so on. So the 2 classes here are: Will purcease and Will not purchace. In the formulas that follow, those 2 classes are denoted as 1 and 0. Which of the 2 classes should take the 1 and which the 0? Bassically, it doesn't matter, but the convention is to assign the 1 to the 'positive' hypothesis and the 0 to the negative hypothesis.

Just note that the focus now is on a Binary Classification, but based on that, we will extend to multi classes. In a post which follows.

So, Question: how will the algorithm work? Answer: It works according to the Supervised Learning algorithm. Let's recap briefly: Figure 1 presents the block diagram of  describe such a system - (re-posting the same diagram for convinience):

Figure 1: Supervise Learning Outlines

![Supervise Learning Outlines](../assets/images/supervised/outlines-of-machine-learning-system-model.svg)


Following Figure 1, we are now concentrating on setting a Prediction Model, which makes the classification decision for the inpuut features X. The model's coefficients are calculated during the Training phase, and later, during the normal data phase, do the classification.

Done with the introduction, the rest of the post will present the following:
1. Present the selection of Logistic Regression as the predictor for Binary Classification.
2. Present the usage of Gradient Decent to find the Logistic Regression coefficients


To start with, we will use a reduced features order Binary Classification sceanrio, i.e.: Predict if a customer will buy a new product based on his income. Such a prediction maybe does not make sense practicdally, but the reduced features dimensions to 1, makes it easier to present it graphically.
So now, Figure 2 presents the purchase perdiction based on income only.



Figure 2:  Binary Classification - Purchace prediction based on income


![Supervised  outlines](../assets/images/logistic-regression/binary-classification-points.png)


The data set presented in Figure 2, hold the labeled training data, i.e. the each income feature point is labeled with its related purchase decisoin taken. We would need a prediction model that can fir these points. According to our paradigm, after fitting the model to the training data, it should fir the normal unlabeled data that comes after. (Though after training, the model's performance should be tested before accepted to be used for real data).

So which model can we take? We previously reviewed the linear prediction model, used for predicting Regression data. Will it fit here too?  Let's see. Figure 3 

examines Linear Prediction for Binary Classification.


![Linear Prediction for Binary Classification](../assets/images/logistic-regression/linear-prediction-binary-classification.png)


Figure 3 presents a line which is assumed to model the data points. Examine the model: According to it, a income of 3000, which was labeld with a 0, will now be mapped to ~0.4. and the income of 3500 now maps to 0.5. Can this model create valid predictions? Figure 4 presnets the decision boundary - any point on the line, from 0.5 and up will be classified as 1, and the rest as 0. One might tend to think that this model is valid - but that is not correct. Justtake another set of training points, as presented by Figure 5, keep the thershold on 0.5, and now, only income from 5000 and up is mapped to 1. And if we had taken more points, obviously the treshhold would move more. 

Figure 4: Linear Prediction for Binary Classification with thresholds

![Linear Prediction for Binary Classification THresholds](../assets/images/logistic-regression/linear-prediction-binary-classification_thresholds.png)


Figure 5: Linear Prediction for Binary Classification with thresholds - Problem!

![Linear Prediction for Binary Classification Thresholds Problem](../assets/images/logistic-regression/linear-prediction-binary-classification_thresholds_problem.png)



Obviously, linear prediction doesn't work for Binary Classification. Another different prediction model is needed. Let me present the Logistic Regression Model.


## Logistic Regression Model

The Logistic Regression is a model which predicts the ####probability#### of hypothesis given the data input, in a binary classification. The model is based on the sigmoid function presented in Eq. 1. 

Eq. 1: Sigmoid Function

$$\sigma(z)=\frac{1}{1+e^{-z}}$$


The sigmoid function maps the z values to values the range [0,1]. $$\sigmaz(z)\rightarrow0$$ as $$z\rightarrow -\infty$$, $$\sigmaz(z)\rightarrow1$$ as $$z\rightarrow \infty$$. and  $$\sigmaz(z) = 0.5$$ for $$z=0$$ as shown in Figure 5. 


Figure 5: Sigmoid Function

![Sigmoid Function](../assets/images/logistic-regression/sigmoid-function.png)


For Logistic Regression predictor, the z argument is replaces by a linear function of the input dataset x: $$z=b+wx$$ in, where $$x=\begin{bmatrix}
x_1 \\ x_2 \\ x_3 \\ x_4 \\..\\x_n 
\end{bmatrix}$$, an n dimensional vector, is the input data set, AKA input features, and $${b,w}$$ are the predictor's coefficients, such that b is a scalar and $$w=\begin{bmatrix}
w_1 \\ w_2 \\ w_3 \\ w_4 \\..\\w_n 
\end{bmatrix}$$ is the vector of coeffcients.

So we now plug $$z=b+wx = b + w_1x_1+w_2x_2+....w_nx_n $$ into Eq 1, as shown in Eq. 2.

Eq. 2: Logistic Regression Formula

$$p(y=1| x, w,b) = h(b+w^Tx)=\sigma(b+w^Tx) = \frac{1}{1+e^{^{-(b+w^Tx)}}}$$


Looking at Figure 5, the interpretation Eq. 2 is: 
The predicted probabilty of y=1 for large negative $$b+w^Tx$$ values, i.e.  p(y=1| $$b+w^Tx\rightarrow -\infty$$), tends to 0.
The predicted probabilty of y=1 for large positive $$b+w^Tx$$ values, i.e.  p(y=1| $$b+w^Tx\rightarrow \infty$$), tends to 1.
The predicted probabilty for y=1 for $$b+w^Tx$$=0 is 0.5. That means, y=1 with probability 0.5.
 
Now, if we had the values of b and w, we could calulate the predicted probability of y=1 for a given features set x.
Obviously, the probability for y=0, for a given set of coefficients {b,w} and input set x, is the 1s complement of p(y=1), as expressed in Eq. 3


Eq. 3: Probability for y=0 

$$p(y=0| x, w,b) = 1- p(y=1| x, w,b)$$

And, obviously....:
The predicted probabilty of y=0 for large negative $$b+w^Tx$$ values, i.e.  p(y=0| $$b+w^Tx\rightarrow -\infty$$), tends to 1.
The predicted probabilty of y=0 for large positive $$b+w^Tx$$ values, i.e.  p(y=0| $$b+w^Tx\rightarrow \infty$$), tends to 0.
The predicted probabilty for y=0 for $$b+w^Tx$$=0 is 0.5. That means, y=0 with probability 0.5.
 
Next paragraphs we will show how to calculate the coefficients $${w,b}$$.



#### Logistic Regression Cost Function

Same as with all Suprevised Learning predictors, the Logistic Regression coefficients are calculated during the Training phase. 
Question: What would be the criteria for the selection of the coefficients' values? 
Answer: This is trivial - we search for the coefficients which lead to the best prediction results, unders a cost function condition we should define.
The cost function normally expresses the difference (sometimes called error), between the labeled points actual values $$y$$, and their values predicted by the model. The Logistic Regression cost function is determined by th Most LikeLihood predictin, as detailed next.

Rembember the Cost function assigned for tLinear Prediction? Remninder - It was based on minimizing the error between the real values and the pmodel's redicted values, where the error was taken as the Euacliden distance, as shown in Eq. 4.

Eq 4: Cost function - Euclidean Distance:

J(b,w) = \frac{1}{m}\sum_{i=1}^{m}\frac{1}{2}(h_{b,w}(x^i)-y^i)^2


Still ccontinueing with the reminder - in the effort of finding the best coefficients, we had to find the minima of the cost function, and calculate the coefficents at that point. 
Q: How should the cost function be found?
A: It's the point were the first derivative equals 0.

And one more reminder - With Linear Prediction 2 solutions were presented: the analytical solutiuon and the Gradient Descent solution. Actuallym the analytical solution, involved with inverting matrixes, fits well, unless number of input features is huge.

Solution for Logistic Regression is different, from some reasons, e.g. the analytical solution is way up complicated, and Eq. 4 would not be convex - see Figure 6 , so the solution may converge to a local minima instead of the global minima. We need a convex cost function. 



Figure 6 a: Non Convex Cost Function




![Non Convex Cost Function](../assets/images/logistic-regression/non-convex-function.png)


Figure 6 b: A Convex  Function


![Convex  Function](../assets/images/logistic-regression/convex-function.png)


Jumping to the end of this chapter, Eq. 5 presents the cost function used for Logistic Regression. Let's examine its charectaristics. Next chapter details the derivation ofEq. 5.


#### Eq. 5: Cost function used for Logistic Regression
5.a

$$Cost(h_{b,w}(x^i,y^i))=\left\{\begin{matrix}
-log (h_{b,w}(x^i,y^i)) \; if\; y=1\\
-log (1-h_{b,w}(x^i,y^i))\; if \;y=0
\end{matrix}\right.
$$

Or expressing it in a single equation:

5.b
$$Cost(h_{b,w}(x^i), y^i)=[y^ilog(h_{(b,w)}(x^i))+(1-y^i)log(1-h_{(b,w)}(x^i))]$$

The index $$i$$ relates to the $$i^{th}$$ example out of m training examples.
Let's examine the cost function optinal outputs, at the 4 'extreme' points:

If hypothesis is y=1 and also predicted probability p(y=1|x) is 1 (i.e. the probability of y=1 given x is 1), then the cost is 0. ($$Note that log(1)=0$$).
If hypothesis is y=0 and also predicted probability p(y=1|x) is 0,ithen the cost is 0. ($$log(0)=1$$).


If candidate hypothesis is y=1 but prediction probability p(y=1|x) is 0, then the cost is $$\infty $$. ($$-log(0)=\infty $$).
If candidate hypothesis is y=0 and prediction probability p(y=1|x) is 1, then the cost is $$\infty $$. ($$log(0)=1$$).


Figure 7: Logistic Regression Cost Function


![Convex  Function](../assets/images/logistic-regression/logistic-regression-cost-function.png)


The overall cost function is the sum the m training examples cost functions:

#### Eq. 6: Logistic Regression overall Cost Function

$$
J(b,w)=\frac{1}{m}\sum_{i=1}^{m}Cost(h_{b,w}(x^i), y^i)=\\
-\frac{1}{m}\sum_{i=1}^{m}[y^ilog(h_{(b,w)}(x^i))+(1-y^i)log(1-h_{(b,w)}(x^i))]
$$

Where

$$h_{(b,w)}=\frac{1}{1+e^{-(b+w^Tx)}}$$


Next chapter details the development of Eq. 5. Surely recommended, but can be skipped. Chapter which follow it, presents the Gradient Descent solution for Logistic Regression.


### Mathematical Development of Cost Function

Here we develope the Logistic Regression cost function - see listed above.

For convinience, let's re-write the Logistic Regression formulas for 

#### Eq. 6: Logistic Regression Formula
6. a Logistic Regression Formula for y=1

$$p(y=1| x, w,b) = h(b+w^Tx)=\sigma(b+w^Tx) = \frac{1}{1+e^{^{-(b+w^Tx)}}}$$

6. b Logistic Regression Formula for y=0


$$p(y=0| x, w,b) = 1 - p(y=1| x, w,b) = 1- h(b+w^Tx)=$$


Consequently, we can combine 6a and 6b to have an expression for $$y\varepsilon [0,1]:

#### Eq. 7: Combined Logistic Regression Formula

$$p(y|x.b,w) =  h(b+w^Tx)^y(1- h(b+w^Tx))^{y-1}$$


Now we take Eq 7, to find the likelihhod of, the output of m training example. It equeals to the multiplication of probabilities $$p(y_i|b,w,x_i) $$of all i, i=1:m. The Likelihhod is a function of the parameters b,w, for a given outcome y and the input variable x.

#### Eq. 8: Likelihood Function
$$L(b, w| y, x) = (p(Y| X, w,b) = 
\prod_{i=1}^{m}p(y_i|x_i, b,w)= h(b+w^Tx_i\zeta )^{y_i}(1- h(b+w^Tx_i))^{y_i-1}$$

Eq. 8 is not concave, i.e. not convex. Note that the non-concave charectaristic is common to the exponential family, which are only logarithmically concave. 
With that in mind, and considering that logarithms are strictly increasing functions, maximizing the log of the likelihood is equivalent to maximizing the likelihood. Not only that, but taking the log makes things much more covinient, as the multiplication are converted to a sum. So Here we take the natural log of Eq. 8 likelihood equation.

#### Eq. 9: Log Likelihood Function

logL(b,w|y,x)=\sum_{i=1}^{m}logp(y_i|x_i, b,w)

Pluging Eq. 7 into Eq 9 + following the common convention of denoting the log-likelihood with a lowercase l, we get: 

#### Eq. 10: Log Likelihood Function

l(b,w|y,x)=\sum_{i=1}^{m}logp(y_i|x_i, b,w)=\sum_{i=1}^{m}log( h_{b,w}(x_i)^y_i(1- h_{b,w}(x_i))^{y_i-1})


Consider the Logarithm power rule:
#### Eq. 11: Logarithm power rule:

$$log(x^ y) = y log(x)$$


Plug Eq. 11 into Eq 10 and get:
#### Eq. 12: Log Likelihood Function - Simplified


l(b,w|y,x)=\sum_{i=1}^{m}log( h_{b,w}(x_i)^y_i(1- h_{b,w}(x_i))^{y_i-1})=\sum_{i=1}^{m}y_ilogh_{b,w}(x_i)+(y_i-1)log(1-h_{b,w}(x_i))


Eq. 12 is the Likelihhod Function, according wich we can find the maximun likelihood, in the effort to find optimal set of coefficients. BUT - instead of maximizing the likelihhod, to we can speak of minimozing the cost, where the cost is the likelihhod's negative:


#### Eq. 13: Cost Function

J(b,w) = -l(b,w)=\sum_{i=1}^{m}-y_ilogh_{b,w}(x_i)+(1-y_i)log(1-h_{b,w}(x_i))

Q.E.D.


### Gradient Descent


Eq. 14 shows Gradient Descent operator set on cost function J(b,w), solving for the free coeffcient {b} and the other linear coefficients {w_j}

#### Eq. 14: Gradient Descent For J(w,b)

Repeat till convergence:

#### Eq. 14a:

$$b:=b-\alpha \frac{\partial J(b,w)}{\partial b}$$

#### Eq. 14b:
For all {b}, {w_j} j=1...n calculate:

$$w_j:=w_j-\alpha \frac{\partial J(b,w)}{\partial w_j}$$

Explaination for the Gradient Decent Process :
The above equations should be repeated iteratively, calculating a new set of {b,w_j} at each iteration. This iterative process should contiue until all {b} and {w_j} converge. The convergence point, is the point where all derivatives are 0, i.e. the minima point. 


Let's calculate the partial derivative $$\frac{\partial Cost(b,w)}{\partial w_i}$$, relying on the derivatives chain rule, reconstructing the cost function into 3 equations:
#### Eq. 15: Decomposing Cost Function Before Chain Rule Derivation
##### Eq. 15 a

$$z=b+w^Tx$$

##### Eq. 15 b


$$h(z)=\sigma(z)=\frac{1}{1+e^{-z}}$$

##### Eq. 15 c

$$Cost(h(z))= $$-ylogh_{z}(x_i)+(1-y)log(1-h_{z}(x^i))$$


Accoringly:

#### Eq. 16: Cost Function Chain Derivatives

\frac{\partial }{\partial w_i}Cost(h(z))=\frac{\partial }{\partial h(z)}Cost(h(z))\cdot\frac{\partial }  {\partial z}h(z)\cdot\frac{\partial }  {\partial w_i}z


Now we can compute each of Eq 16's parts.

Use the natural log well known derivative:

#### Eq 16: Well Known Natural Log Derivative

##### Eq 17 a:
$$y  = log x$$

##### Eq 17 b:
$$\frac{\partial}{\partial x}log x=\frac{1}{x}$$


Plug that into the first partial derivative element of Eq 16:

##### Eq 18: First part of the derivative chain

$$\frac{\partial }{\partial h(z)}Cost(h(z))=\frac{\partial }{\partial h(z)}-y_ilogh_{z}(x_i)+(1-y_i)log(1-h_{z}(x_i))=-\frac{y_i}{h_z(x)}+\frac{1-y_i}{1-h_{z})$$


For the 2nd part of the derivative chain we'll use the reciprocal derivative rule:

##### Eq 19: The reciprocal derivative rule

$$h(x)=\frac{1}{f(x)}$$

$$h'(x)=-\frac{f'(x)}{f^2(x)}$$


Accordingly:



##### Eq 19: Second part of the derivative chain


$$\frac{\partial }  {\partial z}h(z)=\frac{\partial }  {\partial z}\frac{1}{1+e^{-z}}=
\
-\frac{-e^{-z}}{(1+e^{-z})^2}=-\frac{1-(1+e^{-z})}{(1+e^{-z})^2}=-h(z)^2+h(z)==h(z)(1-h(z))$$

##### Eq 20: Third part of the derivative chain

$$\frac{\partial }  {\partial w_i}=x_i$$



re-Combining the 3 parts of the chain we get:

##### Eq 21:  Recombining 3 chained derivatives:

$$-\frac{y_i}{h_z(x)}+\frac{1-y_i}{1-h_{z}} \cdot h(z)(1-h(z)) \cdot x_i=
\\
-\frac{y_i}{h_z(x)}+\frac{1-y_i}{1-h_{z}} \cdot h(z)^2-h(z) \cdot  x_i 
\\=(-y_i(1-h(z))+(1-y_i)h(z)x_i = (h(z)-y_i)x_i$$


##### Eq 22: Resultant Cost derivative:
$$\frac{\partial }  {\partial w_i}Cost(b,w)=(h(z)-y_i)x_i$$


Summing the cost for all m examples we haveL















RONEN TILL HERE!!!!!!!!!!

To simplify the derivation, we'll base on the derivatives chain rule. Before getting to that, let's prepare arrange the cost function:


We denote:
x=b+wx
and h

Eq. 8:

$$\frac{\partial f(y)}{\partial x}=\frac{\partial f(y)}{\partial y}\frac{\partial y}{\partial x}$$





$$


og the .
Cost(

Is is given in Eq. 5.


to find a minima with Gradient Descent, 



Having the prediction function, we need to calculate its coefficients. 

Similarily to Linear Regression, we will determine a cost function, whi



Logostic Regression is currently one of the most commom prediction model algorithm used by Machine Learning algorithms for binary classification. In case you're not familiar with prediction models, and how to solve for their coefficients, or even in case you have no clue about what prediction am I talking, I suggest you read my post on that before. Not mandatory though. If the term "Binary Classification" needs clarifications, I'd start with my Intro to Machine Learning. Not mandatory though.

In any case, to start with, I posted here again the Supervised Machine Learning blog diagram.

#### Figure 1: Supervised Machine Learning blog diagram

![Supervised learning outlines](../assets/images/supervised/outlines-of-machine-learning-system-model.svg)

As Figure 1 shows, the predictor sits in the heart of the system. The perdictor's coeficients are calculated during the Training phase, then ready to use in the Testing and Normal Data phases.

This post explains the Logistic Regression model, and the developemnt of the model's parameters' solution. We'll walk top to bottom - begin with an illustration of a classification problem, then present the Logistic Regression predictor model, and eventually show how to find its coefficients, with the good-old Descent Regression algorithm.


Here's the (commonly used) binary classification example: It is needed to predict whether a tumor is benign or maligent, based on its size. 




multi class



we need to predict if a customer will buy Iphone as his next phone, based on the next smartphone of a customer , which already owns a smatphone,  will buy another iphone after 2 years, based on whether he owned an iphone before or not. We'll see that normally a 
. This example is not realy realistic, but we'll use it to start with binary prediction based on a single feature. So, suppose we need to predict if a customer, which already owns a smatphone,  will buy an iphoe, based on whether he owned an iphone before or not. Suppose  that if he owned one 
