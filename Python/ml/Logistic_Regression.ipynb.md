```ipynb
{
 "cells": [
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "## LOGISTIC REGRESSION FROM SCRATCH"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "This problem implements logistic regression on university exams of students and predicts if a particular student will pass the exam or not. "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "The data is taken from a file named 'ex2data1.txt' which contains marks of different students in two exams in first two columns and admission verdict in third coloumn as 0 or 1 (0 == failure and 1 == success)."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [],
   "source": [
    "# importiing important libraries \n",
    "%matplotlib notebook\n",
    "import numpy as np\n",
    "import pandas as pd\n",
    "import matplotlib.pyplot as plt\n",
    "import scipy.optimize as op # used scipy to implement gradient descent"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [],
   "source": [
    "# function to add a column of ones to the matrix\n",
    "def add_ones(X):\n",
    "    m = len(X)\n",
    "    X = np.c_[np.ones(m), X]\n",
    "    return X"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
   "outputs": [],
   "source": [
    "# function to perform feature scaling\n",
    "def scaling(arr):\n",
    "    arr = arr.astype('float64')\n",
    "    n = len(arr[0])\n",
    "    mu = np.mean(arr, axis=0).reshape(1,n)\n",
    "    sigma = np.std(arr, axis=0).reshape(1,n)\n",
    "    for x in range(n):\n",
    "        arr[:,x]-= mu[0,x]\n",
    "        arr[:,x]/= sigma[0,x]\n",
    "    return (mu,sigma,arr)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {},
   "outputs": [],
   "source": [
    "# function to calculate sigmoid of a value\n",
    "def sigmoid(z):\n",
    "    sig = 1/(1+np.exp(-z))\n",
    "    return sig"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [],
   "source": [
    "# calculates the loss function for a given theta vector\n",
    "def cost_func(t, X, y):\n",
    "    m,n = X.shape\n",
    "    t = t.reshape((n,1));\n",
    "    y = y.reshape((m,1))\n",
    "    hypo = sigmoid(np.dot(X,t))  # calculating hypothesis\n",
    "    cost = np.multiply(y,np.log(hypo)) + np.multiply(1-y,np.log(1-hypo))  # element wise cost calculation\n",
    "    J = (-1/m) * np.sum(cost)  # total cost summation\n",
    "    return J"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [],
   "source": [
    "# calculating values of gradients for all theta values simultaneously\n",
    "def gradient(t,X,y):\n",
    "    m,n = X.shape\n",
    "    t = t.reshape((n,1));\n",
    "    y = y.reshape((m,1))\n",
    "    temp = sigmoid(np.dot(X,t)) - y\n",
    "    grad = np.dot(X.T, temp) / m\n",
    "    return grad"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "0.6931471805599453\n",
      "[[ -0.1       ]\n",
      " [-12.00921659]\n",
      " [-11.26284221]]\n"
     ]
    }
   ],
   "source": [
    "df = pd.read_csv('ex2data1.txt', names = ['exam1', 'exam2', 'status'])\n",
    "X = df[['exam1','exam2']].values\n",
    "y = df['status'].values.reshape(len(X),1)\n",
    "X = add_ones(X)\n",
    "theta = np.zeros((len(X[1]),1))\n",
    "print(cost_func(theta, X, y))\n",
    "print(gradient(theta,X,y))"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "array([-25.16131853,   0.20623159,   0.20147149])"
      ]
     },
     "execution_count": 11,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "initial_theta = np.zeros(len(theta));\n",
    "Result = op.minimize(fun = cost_func, \n",
    "                                 x0 = initial_theta, \n",
    "                                 args = (X, y),\n",
    "                                 method = 'TNC',\n",
    "                                 jac = gradient);\n",
    "theta = Result.x\n",
    "theta"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {},
   "source": [
    "Now lets see what is the probability of a student getting admission with scores 45 and 85 in the exams."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 15,
   "metadata": {},
   "outputs": [],
   "source": [
    "# vector contaning scores with added column of one\n",
    "test = [1,45,85]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 17,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "0.7762906220893772"
      ]
     },
     "execution_count": 17,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "probability = sigmoid(np.dot(test,theta))\n",
    "probability"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 21,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "So there is a 77.63 % chance of the student getting addmission\n"
     ]
    }
   ],
   "source": [
    "print(f\"So there is a {probability*100:1.2f} % chance of the student getting addmission.\")"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": null,
   "metadata": {},
   "outputs": [],
   "source": []
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.7.6"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 4
}

```