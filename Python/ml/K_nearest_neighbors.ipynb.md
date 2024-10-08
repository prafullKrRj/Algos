```ipynb
{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "name": "K_nearest_neighbors.ipynb",
      "provenance": []
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    },
    "accelerator": "GPU"
  },
  "cells": [
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "sO_IMbljLMNj",
        "colab_type": "text"
      },
      "source": [
        "# Building K Nearest Neighbor classifier from scratch."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "M6UfZmwc5tFr",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "import numpy as np\n",
        "from collections import Counter"
      ],
      "execution_count": 3,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "tGPHY7rl_u5i",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "# implementing formula for euclidean distance\n",
        "def euclidean_distance(x1,x2):\n",
        "  return np.sqrt(np.sum((x1-x2)**2))"
      ],
      "execution_count": 2,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "wpM1M503-p2_",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "#creating the KNN classifier class\n",
        "class KNN:\n",
        "  #setting k = 3 as default value\n",
        "  def __init__(self,k=3):\n",
        "    self.k = k\n",
        "  \n",
        "  def fit(self,X,y):\n",
        "    self.X_train = X\n",
        "    self.y_train = y\n",
        "  \n",
        "  def predict(self,X):\n",
        "    y_pred = [self._predict(x) for x in X]\n",
        "    return np.array(y_pred)\n",
        "\n",
        "  def _predict(self,x):\n",
        "    #distance\n",
        "    distances = [euclidean_distance(x,x_train) for x_train in self.X_train]\n",
        "    # k nearest samples\n",
        "    k_indices = np.argsort(distances)[:self.k]\n",
        "    k_nearest_labels = [self.y_train[i] for i in k_indices]\n",
        "    # most common class label\n",
        "    most_common = Counter(k_nearest_labels).most_common(1)\n",
        "    return most_common[0][0]"
      ],
      "execution_count": 36,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "86N7vyynLkRC",
        "colab_type": "text"
      },
      "source": [
        "## Now testing the performance of our KNN classifier on iris dataset."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "R7gjqIyKDVdT",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "from sklearn import datasets\n",
        "from sklearn.model_selection import train_test_split"
      ],
      "execution_count": 37,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "-r7Xyxh_Eswq",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "iris = datasets.load_iris()\n",
        "X,y = iris.data, iris.target"
      ],
      "execution_count": 38,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "cNEGtt-2E1XW",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "# train-test split\n",
        "X_train,X_test,y_train,y_test = train_test_split(X,y,test_size=0.2,random_state=1234)"
      ],
      "execution_count": 39,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "79X7K7dvE9ql",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "# instantiating our classifier\n",
        "clf = KNN(k=5)\n",
        "clf.fit(X_train,y_train)"
      ],
      "execution_count": 44,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "nsB_fUqoFPQ0",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "predictions = clf.predict(X_test)"
      ],
      "execution_count": 45,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "2SeXjx-PFaKj",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "# creating a function for checking accuracy\n",
        "def accuracy(y_true, y_pred):\n",
        "    accuracy = np.sum(y_true == y_pred) / len(y_true)\n",
        "    return accuracy"
      ],
      "execution_count": 46,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "v3JI-3QOO9x3",
        "colab_type": "text"
      },
      "source": [
        "## Checking Accuracy"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "ZDLjsJqxIuFt",
        "colab_type": "code",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 34
        },
        "outputId": "2d6977bf-c31d-4120-d4e6-77210054f9f1"
      },
      "source": [
        "print(\"Accuracy : {:.4f}\".format(accuracy(y_test, predictions)))"
      ],
      "execution_count": 50,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "Accuracy : 0.9667\n"
          ],
          "name": "stdout"
        }
      ]
    }
  ]
}
```