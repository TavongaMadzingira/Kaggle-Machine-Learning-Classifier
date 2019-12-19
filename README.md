Introduction
This is my report for my implementation of my Kaggle competition submission of ‘Bring back the Sun’.
1.1. Implementation
I produced my submission by using an implementation using Keras in order to train and test based on the datasets provided. In my code I relied upon logistic regression in this report I shall outline my implementation of this model as well as the results and conclusions reached thereof:

1.2. Preparing data for training
The datasets provided for the Kaggle competition were divided into 2 files: training_data and additional_training_data; each of which contained a 4609 dimension features extracted matrix from a convolutional neural network alongside binary predictions for each class. There are also several missing data points held as “NaN” values in the csv; this is a problem as it will cause our fitting to produce a zero accuracy metric for each epoch. In order to deal with this problem I used pandas to produce a concatenated csv file in which all NaN values are replaced with zeros.

1.3. Train test split
In order for fitting to take place; the data must be separated into training and testing data classes as well as training and testing validation classes, the split, refers to the amount of our data and validation set that we use for training and testing; which in my case is training on 2331 samples and validating on 259 samples. During my experimentation I was using a specified “random_state”. This means that there was not a random division of data into subsets and thus our results do not vary each time that the program is run. This is necessary because it is important for us to be able to see exactly which changes are beneficial to the performance metric of our model rather than simple “luck”.


1.4. Constructing the model
I implemented a neural network with 3 hidden layers as well as an input and output layer. The activation function of my output layer was sigmoid (logistic activation function). But, why did I choose to use this over hyperbolic Tangent or Rectified Linear Unit? The answer to this lies in the type of data that we are operating upon and outputting. It is scaled between 0 and 1; remember, we have various probabilities upon which we are making predictions, and this means that all values involved can and will only fall between 0 and 1. Due to the monotonic nature of the sigmoid function; this is ideal for the output layer [1]. I then compiled the model using binary cross entropy as a metric, once again for the same reason that I have used the sigmoid function; the predictions do not extend beyond the space of 0 and 1. 

For my optimization algorithm, I have chosen to use Adaptive moment estimation (adam). This is because it appeared to produce good results on deep networks using default hyper parameters; as opposed to other optimization algorithms such as stochastic gradient descent (sgd). Finally, for my metric I chose accuracy as I wanted to be able to observe whether the accuracy would increase against each epoch.

1.5. Fitting 
In this section I will discuss how I trained the model using the train and test subsets. Model fitting requires the input of various parameters; the first of which is the number of epochs. When the entire dataset has been passed through a neural network; this is known as an epoch and by specifying the number of times that the dataset passes through the network we can ensure that the model is not “Over-fitting”. This simply means that the model is too specific to the training data, that it mimics the training data too well; thus reducing its accuracy when operating on Data that it has not encountered before, as it is unable to generalize [2]. We must also specify the batch size meaning the number of elements that we are passing through the network at one given time. I also set verbose to 1 so I could monitor the value accuracy for each epoch.

1.6. Realizing that I was overfitting my data 
 
Figure 1: A graph depicting over fit data.

   Above we see a graph showing model accuracy over a span of 1000 epochs; showing both the training and testing accuracy we can see a few signs of overfitting. Firstly the testing accuracy reaches a point that it begins to decline and more importantly there is an extremely big difference between the accuracy of the training data and that of the testing data. The first step I took to attempt to remedy this was to apply an l2 kernel regularizer to the initialization of my model layers.  This is also known as weight regularization, essentially we want to have control over the weight parameter during fitting; this prevents the model from being unstable [3]. Creating penalties for large weights can encourage the model to operate mainly on smaller weights. In this case we are operating on the squared weight values (L2) rather than the absolute sum of values (L1). In this experiment I decided to apply the maximum penalty this is because overfitting is characterized by low bias and high variance and hence we are trying to ensure opposing behavior by enforcing low weights. The effect of which can be seen below:

 
The first thing we can observe is how closely our training data matches our testing data and furthermore that the model accuracy does not reach a point at which it begins to decline. In order to ensure we can also observe our binary cross entropy loss as a graph; ideally we would want a universally declining graph (as the prediction should no longer differ from the true label).

 
Internal testing of this against the rounded accuracy matrix indicated a model accuracy of 0.79, resulting in a Kaggle score of 0.68.
2. Discussion/Possible improvements
Although I did not implement these methods I believe that our performance could have been improved through the following means:

Since accuracy generally increases proportionally with the amount of training data provided, we could create generated training data through randomized entries to our training data.

Applying early stopping; this is when we stop training when performance appears to degrade.

Use of Stacked Generalization; we could use multiple models and stack the predictions based on accuracy of individual epochs.


[1]	Sharma, S. (2019). Activation Functions in Neural Networks. [online] Towards Data Science. Available at: https://towardsdatascience.com/activation-functions-neural-networks-1cbd9f8d91d6 [Accessed 16 May 2019].
[2]	D. and Sonia, S. (2018). A Comparative Study of Supervised Machine Learning Algorithm. International Journal of Computer Sciences and Engineering, 6(12), pp.875-878.
[3]	Williams, C. (2003). Learning With Kernels: Support Vector Machines, Regularization, Optimization, and Beyond. Journal of the American Statistical Association, 98(462), pp.489-489.

