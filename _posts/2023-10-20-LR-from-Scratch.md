---
layout: single
title: "Two Ways to Do Linear Regression from Scratch"
category: Projects
tags: [ML from Scratch, Regression]
toc: true
toc_label: "Table of Contents"
toc_icon: "cog"
#classes: wide
tagline: "Normal Equation vs Gradient Descent"
header:
  overlay_image: /assets/images/lr_scratch/overlay.jpg
  overlay_filter: 0.5
  caption: "Photo credit: [**Unsplash**](https://www.unsplash.com/)"
  teaser: /assets/images/lr_scratch/overlay.jpg
  og_image: /assets/images/lr_scratch/overlay.jpg
---

## Abstract
Linear regression is a fundamental technique in data science. In this post, I'll break down its core mechanics, focusing on the Normal Equation and Gradient Descent methods. This post offers a straightforward look into two common approaches for implementing this classic algorithm.

## Normal equation method

### Derivation of the normal equation

#### 1. Model and cost function
Suppose a linear model:

$$h_\theta(x) = \theta^T x,$$

Which expands to:

$$
h_\theta(x) = \theta_0 + \theta_1 x_1 + \theta_2 x_2 + ... + \theta_n x_n,
$$

Loss function (mean squared error):

$$
J(\theta) = \frac{1}{2m} \sum_{i=1}^{m} (h_\theta(x^{(i)}) - y^{(i)})^2,
$$

In matrix notation, this becomes:

$$
J(\theta) = \frac{1}{2} (X\theta - y)^T (X\theta - y)
$$

Where:
- $$X$$ is the design matrix (m samples x n features).
- $$y$$ is the column vector of output values.
- $$\theta$$ is the column vector of model parameters.

#### 2. Minimize the Cost Function
To find the value of $\theta$ that minimizes $$J(\theta)$$, we'll take the derivative with respect to $\theta$ and set it to zero.

Differentiating $J(\theta)$ with respect to $$\theta$$:

$$\frac{\partial J(\theta)}{\partial \theta} = X^T (X\theta - y)$$

For minimum, we'll set this to zero:

$$X^T (X\theta - y) = 0$$

Expanding:

$$X^T X \theta = X^T y$$

Now, to solve for $$\theta$$:

$$\theta = (X^T X)^{-1} X^T y$$

This is the **normal equation**!

### Implementation
~~~
def multiple_linear_regression(X, y):
    """
    Fit multiple linear regression.
    Parameters:
    - X: 2D numpy array where each row is a data point and each column is a predictor variable.
         It doesn't include the bias (intercept) term.
    - y: 1D numpy array, the target variable.
    
    Returns:
    - beta: Estimated coefficients including intercept.
    """
    
    # Add a bias column with all 1s to X
    ones = np.ones(X.shape[0])
    X = np.column_stack([ones, X])
    
    # Calculate coefficients using the normal equation
    beta = np.linalg.inv(X.T.dot(X)).dot(X.T).dot(y)
    
    return beta

def predict(X, beta):
    """
    Predict using the fitted multiple linear regression model.
    
    Parameters:
    - X: 2D numpy array where each row is a data point and each column is a predictor variable.
         It doesn't include the bias (intercept) term.
    - beta: 1D numpy array, the coefficients from the multiple_linear_regression.
    
    Returns:
    - Predictions as a 1D numpy array.
    """
    
    ones = np.ones(X.shape[0])
    X = np.column_stack([ones, X])
    
    return X.dot(beta)
	
# Example
X = np.array([[0, 2], [2, 3], [3, 4], [4, 5]])
y = np.array([3, 4, 6, 8])
beta = multiple_linear_regression(X, y)
print("Final parameters:", beta)

X_new = np.array([[1, 2], [2, 5]])
predictions = predict(X_new, beta)
print("Predictions:", predictions)
~~~
Final parameters: [-3. -1.  3.]
Predictions: [ 2. 10.]

## Gradient descent method

When **the derivatives are not directly solve-able**, we can use gradient descent to approximate the global minimum of the linear regression.

~~~
def compute_cost(X, y, theta):
    """
    Compute the cost (mean squared error) of using theta as the parameter for linear regression 
    to fit the data in X and y.
    
    Parameters:
    - X : numpy.ndarray
        The feature matrix. It includes the bias term (a column of ones).
    - y : numpy.ndarray
        The target variable vector.
    - theta : numpy.ndarray
        The parameter vector for linear regression.
        
    Returns:
    - float
        The cost computed using the provided theta.
    """
    m = len(y)
    predictions = X.dot(theta)
    cost = (1/(2*m)) * np.sum(np.square(predictions - y))
    return cost

def gradient_descent(X, y, alpha, num_iterations):
    """
    Perform gradient descent to learn theta parameters for linear regression.
    
    Parameters:
    - X : numpy.ndarray
        The feature matrix. It doesn't include the bias term.
    - y : numpy.ndarray
        The target variable vector.
    - alpha : float
        The learning rate.
    - num_iterations : int
        The number of iterations for gradient descent.
        
    Returns:
    - tuple (theta, cost_history)
        - theta : numpy.ndarray
            The final parameter vector learned after gradient descent.
        - cost_history : numpy.ndarray
            The history of cost computed at each iteration.
    """
    m = len(y)
    cost_history = np.zeros(num_iterations)
    ones = np.ones(X.shape[0])
    X = np.column_stack([ones, X])
    theta = np.zeros(X.shape[1])
    for i in range(num_iterations):
        gradients = (1/m) * X.T.dot(X.dot(theta) - y)
        theta = theta - alpha * gradients
        cost_history[i] = compute_cost(X, y, theta)
        
    return theta, cost_history

def predict(X, theta):
    ones = np.ones(X.shape[0])
    X = np.column_stack([ones, X])
    return X.dot(theta)

# Example
X = np.array([[0, 2], [2, 3], [3, 4], [4, 5]])
y = np.array([3, 4, 6, 8])

# Initialize parameters
alpha = 0.03  # learning rate
num_iterations = 10000

theta_final, cost_history = gradient_descent(X, y, alpha, num_iterations)
print("Final parameters:", theta_final)
~~~
Final parameters: [-2.73837898 -0.8983058   2.86052068]
~~~
X_new = np.array([[1, 2], [2, 5]])
predictions = predict(X_new, theta_final)
print(predictions)
~~~
[2.08435657 9.76761281]

Thank you for reading. The code for my project can be found [here](https://github.com/JoshJingtianWang/ML_from_scratch). 