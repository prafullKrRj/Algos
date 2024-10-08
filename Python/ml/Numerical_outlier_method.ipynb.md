```ipynb
{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "name": "Numerical_outlier_method.ipynb",
      "provenance": []
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    }
  },
  "cells": [
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "qsCL6wPSvJwY",
        "colab_type": "text"
      },
      "source": [
        "## IMPORTS"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "zAy3SIjkvAXF",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "import numpy as np\n",
        "import random\n",
        "import matplotlib.pyplot as plt"
      ],
      "execution_count": 1,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "KPueZtWmvPFE",
        "colab_type": "text"
      },
      "source": [
        "###Creating a Pseudo-Dataset"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "vm5Vyj80vOsu",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "def dataset_developer(n, probability=[0.05, 0.05, 0.15, 0.5, 0.15]):\n",
        "\tX = []\n",
        "\twhile len(X)<n:\n",
        "\t\tt = random.random()\n",
        "\t\tif t<probability[0]:\n",
        "\t\t\tX.append(random.random()*0.2)\n",
        "\t\telif t>=probability[0] and t<probability[1]+probability[0]:\n",
        "\t\t\tX.append(random.random()*0.2+0.2)\n",
        "\t\telif t>= probability[1]+probability[0] and t<probability[0]+probability[1]+probability[2]:\n",
        "\t\t\tX.append(random.random()*0.2+0.2*2)\n",
        "\t\telif t>=probability[0]+probability[1]+probability[2] and t<probability[0]+probability[1]+probability[2]+probability[3]:\n",
        "\t\t\tX.append(random.random()*0.2+0.2*3)\n",
        "\t\telse:\n",
        "\t\t\tX.append(random.random()*0.2+0.2*4)\n",
        "\treturn X"
      ],
      "execution_count": 2,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "ub_0QO4yvX0m",
        "colab_type": "text"
      },
      "source": [
        "## Numeric Outlier Method"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "pZjhkSe1vVol",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "def numeric_outlier(X, k):\n",
        "\tQ1 = X[int((len(X)+1)/4)]\n",
        "\tQ3 = X[int(((len(X)+1)*3)/4)]\n",
        "\tIQR = Q3 - Q1\n",
        "\toutliers_index = [i for i in range(len(X)) if X[i]<Q1-k*IQR or X[i]>Q3+k*IQR]\n",
        "\treturn outliers_index"
      ],
      "execution_count": 3,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "fqcMJF9kvdF4",
        "colab_type": "text"
      },
      "source": [
        "### Visualising Results"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "SiUV9VncvahN",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "def imager(X, Y, outlier_X, outlier_Y, l):\n",
        "\tX = random.sample(X, len(X))\n",
        "\toutlier_X = random.sample(outlier_X, l)\n",
        "\tplt.scatter(X, Y, color='green', marker='.')\n",
        "\tplt.scatter(outlier_X, outlier_Y, color='red', marker='*')\n",
        "\tplt.plot()"
      ],
      "execution_count": 4,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "7vTK89yEvilL",
        "colab_type": "text"
      },
      "source": [
        "### Checking Results"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "SCvCWBGFvgFp",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "def accuracy(outliers_index, X):\n",
        "\taccuracy = 0\n",
        "\tfor j in range(len(X)):\n",
        "\t\tif j in outliers_index:\n",
        "\t\t\tif X[j]<0.5 or X[j]>0.9:\n",
        "\t\t\t\taccuracy += 1\n",
        "\t\telse:\n",
        "\t\t\tif X[j]>=0.5 and X[j]<=0.9:\n",
        "\t\t\t\taccuracy += 1\n",
        "\treturn accuracy/len(X)"
      ],
      "execution_count": 5,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "2EVHIXUKvnca",
        "colab_type": "text"
      },
      "source": [
        "### Creating MAIN"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "bHTRjouDvkXe",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "def main(X, k):\n",
        "\toutliers_index = numeric_outlier(X, k)\n",
        "\toutliers = [X[i] for i in outliers_index]\n",
        "\tX_c = [X[i] for i in range(len(X)) if i not in outliers_index]\n",
        "\timager(range(len(X_c)), X_c, range(len(X_c)), outliers, len(outliers_index))\n",
        "\treturn accuracy(outliers_index, X)"
      ],
      "execution_count": 6,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "niEZuZZ3vsAp",
        "colab_type": "code",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 282
        },
        "outputId": "f2f5f8f0-4b59-40a9-bd5a-c41b1bac73d8"
      },
      "source": [
        "k = 0.45\n",
        "random.seed(0)\n",
        "X = dataset_developer(1000)\n",
        "X.sort()\n",
        "acc = main(X, k)\n",
        "print(acc)"
      ],
      "execution_count": 7,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "0.985\n"
          ],
          "name": "stdout"
        },
        {
          "output_type": "display_data",
          "data": {
            "image/png": "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\n",
            "text/plain": [
              "<Figure size 432x288 with 1 Axes>"
            ]
          },
          "metadata": {
            "tags": [],
            "needs_background": "light"
          }
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "IR-5AH5zvxtV",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        ""
      ],
      "execution_count": 7,
      "outputs": []
    }
  ]
}
```