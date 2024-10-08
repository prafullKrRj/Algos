```ipynb
{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "name": "Naive_Bayes_Classifier.ipynb",
      "provenance": [],
      "toc_visible": true
    },
    "kernelspec": {
      "name": "python3",
      "display_name": "Python 3"
    },
    "language_info": {
      "name": "python"
    }
  },
  "cells": [
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "j6s9d_yfmiGn"
      },
      "source": [
        "# Naive Bayes Classifier in Python"
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "StR2rQ6wmx86"
      },
      "source": [
        "## Import Libraries"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "cdCJCtxz-W6L"
      },
      "source": [
        "# import the libraries\n",
        "import numpy as np\n",
        "import pandas as pd\n",
        "import matplotlib.pyplot as plt\n",
        "%matplotlib inline\n",
        "import seaborn as sns\n",
        "from sklearn import datasets"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "98oRaAjFm5i6"
      },
      "source": [
        "## Import the dataset"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 195
        },
        "id": "Dga0bN4X-sNZ",
        "outputId": "a977574f-7312-4269-f887-adf66c31ab74"
      },
      "source": [
        "# import the dataset\n",
        "iris = datasets .load_iris()\n",
        "df = pd.DataFrame(iris.data)\n",
        "df[4]=iris.target\n",
        "df.columns=['sepal_len', 'sepal_wid', 'petal_len', 'petal_wid', 'class']\n",
        "df.head()"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>sepal_len</th>\n",
              "      <th>sepal_wid</th>\n",
              "      <th>petal_len</th>\n",
              "      <th>petal_wid</th>\n",
              "      <th>class</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>5.1</td>\n",
              "      <td>3.5</td>\n",
              "      <td>1.4</td>\n",
              "      <td>0.2</td>\n",
              "      <td>0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>4.9</td>\n",
              "      <td>3.0</td>\n",
              "      <td>1.4</td>\n",
              "      <td>0.2</td>\n",
              "      <td>0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>4.7</td>\n",
              "      <td>3.2</td>\n",
              "      <td>1.3</td>\n",
              "      <td>0.2</td>\n",
              "      <td>0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>4.6</td>\n",
              "      <td>3.1</td>\n",
              "      <td>1.5</td>\n",
              "      <td>0.2</td>\n",
              "      <td>0</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>5.0</td>\n",
              "      <td>3.6</td>\n",
              "      <td>1.4</td>\n",
              "      <td>0.2</td>\n",
              "      <td>0</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "   sepal_len  sepal_wid  petal_len  petal_wid  class\n",
              "0        5.1        3.5        1.4        0.2      0\n",
              "1        4.9        3.0        1.4        0.2      0\n",
              "2        4.7        3.2        1.3        0.2      0\n",
              "3        4.6        3.1        1.5        0.2      0\n",
              "4        5.0        3.6        1.4        0.2      0"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 15
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "A8qzm15SnCPt"
      },
      "source": [
        "## Exploratory data analysis"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "2TvuOgJLAXcH",
        "outputId": "f81acff7-8af7-48fa-db51-3c7ff9f7838a"
      },
      "source": [
        "# view dimensions of dataset\n",
        "df.shape"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "(150, 5)"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 17
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "eRyU2AKVAZ5x"
      },
      "source": [
        "The dataset has 150 entries/rows and 5 columns."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "MLVwbJ8g--ZT",
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "outputId": "7f6cabff-c63d-4e6f-9890-5bb0801636c3"
      },
      "source": [
        "# view summary of dataset\n",
        "df.info()"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "<class 'pandas.core.frame.DataFrame'>\n",
            "RangeIndex: 150 entries, 0 to 149\n",
            "Data columns (total 5 columns):\n",
            " #   Column     Non-Null Count  Dtype  \n",
            "---  ------     --------------  -----  \n",
            " 0   sepal_len  150 non-null    float64\n",
            " 1   sepal_wid  150 non-null    float64\n",
            " 2   petal_len  150 non-null    float64\n",
            " 3   petal_wid  150 non-null    float64\n",
            " 4   class      150 non-null    int64  \n",
            "dtypes: float64(4), int64(1)\n",
            "memory usage: 6.0 KB\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "S81FkvGDAVLS",
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "outputId": "5d64a342-7312-4064-dd06-bf708dece551"
      },
      "source": [
        "# check missing values in dataset\n",
        "df.isnull().sum()"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "sepal_len    0\n",
              "sepal_wid    0\n",
              "petal_len    0\n",
              "petal_wid    0\n",
              "class        0\n",
              "dtype: int64"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 19
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "LfjT_eQidcbC"
      },
      "source": [
        "There are no missing values in the categorical variables."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 195
        },
        "id": "qDNrymrdhOD1",
        "outputId": "c824db3a-a696-4137-cf08-b49ed5345793"
      },
      "source": [
        "x = df.drop(['class'], axis=1)\n",
        "x.head()"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/html": [
              "<div>\n",
              "<style scoped>\n",
              "    .dataframe tbody tr th:only-of-type {\n",
              "        vertical-align: middle;\n",
              "    }\n",
              "\n",
              "    .dataframe tbody tr th {\n",
              "        vertical-align: top;\n",
              "    }\n",
              "\n",
              "    .dataframe thead th {\n",
              "        text-align: right;\n",
              "    }\n",
              "</style>\n",
              "<table border=\"1\" class=\"dataframe\">\n",
              "  <thead>\n",
              "    <tr style=\"text-align: right;\">\n",
              "      <th></th>\n",
              "      <th>sepal_len</th>\n",
              "      <th>sepal_wid</th>\n",
              "      <th>petal_len</th>\n",
              "      <th>petal_wid</th>\n",
              "    </tr>\n",
              "  </thead>\n",
              "  <tbody>\n",
              "    <tr>\n",
              "      <th>0</th>\n",
              "      <td>5.1</td>\n",
              "      <td>3.5</td>\n",
              "      <td>1.4</td>\n",
              "      <td>0.2</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>1</th>\n",
              "      <td>4.9</td>\n",
              "      <td>3.0</td>\n",
              "      <td>1.4</td>\n",
              "      <td>0.2</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>2</th>\n",
              "      <td>4.7</td>\n",
              "      <td>3.2</td>\n",
              "      <td>1.3</td>\n",
              "      <td>0.2</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>3</th>\n",
              "      <td>4.6</td>\n",
              "      <td>3.1</td>\n",
              "      <td>1.5</td>\n",
              "      <td>0.2</td>\n",
              "    </tr>\n",
              "    <tr>\n",
              "      <th>4</th>\n",
              "      <td>5.0</td>\n",
              "      <td>3.6</td>\n",
              "      <td>1.4</td>\n",
              "      <td>0.2</td>\n",
              "    </tr>\n",
              "  </tbody>\n",
              "</table>\n",
              "</div>"
            ],
            "text/plain": [
              "   sepal_len  sepal_wid  petal_len  petal_wid\n",
              "0        5.1        3.5        1.4        0.2\n",
              "1        4.9        3.0        1.4        0.2\n",
              "2        4.7        3.2        1.3        0.2\n",
              "3        4.6        3.1        1.5        0.2\n",
              "4        5.0        3.6        1.4        0.2"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 26
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "7gTUhVoohW5a",
        "outputId": "3179cd10-5b68-40c1-cede-9a511766e14c"
      },
      "source": [
        "y = df['class']\n",
        "y"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "0      0\n",
              "1      0\n",
              "2      0\n",
              "3      0\n",
              "4      0\n",
              "      ..\n",
              "145    2\n",
              "146    2\n",
              "147    2\n",
              "148    2\n",
              "149    2\n",
              "Name: class, Length: 150, dtype: int64"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 27
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "6auPnfOfdD1X",
        "outputId": "d19b8f19-ad06-4e1d-d380-5e22713c4d34"
      },
      "source": [
        "# check labels in target variable\n",
        "y.unique()"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "array([0, 1, 2])"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 30
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "JsaKK7TmgxeX",
        "outputId": "042fea1d-52cf-4ea1-f65b-3174458bc87e"
      },
      "source": [
        "# value counts for target variable\n",
        "y.value_counts()"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "2    50\n",
              "1    50\n",
              "0    50\n",
              "Name: class, dtype: int64"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 29
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "mLhXNNlihsV1"
      },
      "source": [
        "The target variabke has 3 labels and there are 50 entries for each label."
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "Co60mTGgnKrC"
      },
      "source": [
        "## Splitting dataset into Training and Testing Set"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "7hixQJ-ddnPM"
      },
      "source": [
        "# split X and y into training and testing sets\n",
        "\n",
        "from sklearn.model_selection import train_test_split\n",
        "x_train, x_test, y_train, y_test = train_test_split(x, y, test_size = 0.2, random_state = 7)"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "GGT3GD4iiWso",
        "outputId": "decd4365-609d-4d32-a93c-8254dd61ab56"
      },
      "source": [
        "x_train.shape[0], x_test.shape[0]"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "(120, 30)"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 33
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "Kz2CfNFeie7m"
      },
      "source": [
        "There are 120 entries in our training dataset and 30 entries in our testing dataset."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "yzPd2rAviM7Y",
        "outputId": "41bf3040-152e-4261-ebbe-d32aa06f1fec"
      },
      "source": [
        "y_train.value_counts()"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "0    43\n",
              "2    39\n",
              "1    38\n",
              "Name: class, dtype: int64"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 34
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "PCxIoGQEi2h-"
      },
      "source": [
        "# # To encode categorical variables with one-hot encoding\n",
        "# import category_encoders as ce\n",
        "\n",
        "# encoder = ce.OneHotEncoder(cols=['columns', 'to', 'encode'])\n",
        "# x_train = encoder.fit_transform(X_train)\n",
        "# x_test = encoder.transform(X_test)"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "8MlWks5unXsJ"
      },
      "source": [
        "## Feature Scaling"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "5wkpJAdejhVU"
      },
      "source": [
        "# Normalise this dataset\n",
        "from sklearn.preprocessing import StandardScaler\n",
        "\n",
        "scaler = StandardScaler()\n",
        "x_train = scaler.fit_transform(x_train)\n",
        "x_test = scaler.transform(x_test)"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "8ThL_zILnbmZ"
      },
      "source": [
        "## Model Training"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "A3rhISqDj9k_",
        "outputId": "e14632d1-535e-4f50-9603-f80494b05616"
      },
      "source": [
        "# train a Gaussian Naive Bayes classifier on the training set\n",
        "from sklearn.naive_bayes import GaussianNB\n",
        "\n",
        "# instantiate the model\n",
        "gaussianNaiveBayes = GaussianNB()\n",
        "\n",
        "# fit the model\n",
        "gaussianNaiveBayes.fit(x_train, y_train)"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "GaussianNB(priors=None, var_smoothing=1e-09)"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 38
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "jp6prM-NnfbD"
      },
      "source": [
        "## Prediction"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "KE5hHtLOkP4v",
        "outputId": "b41499a4-c17e-4ddd-d503-c57af351ef86"
      },
      "source": [
        "# Predict the results\n",
        "y_pred = gaussianNaiveBayes.predict(x_test)\n",
        "y_pred"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "array([2, 1, 0, 1, 1, 0, 2, 1, 0, 1, 2, 1, 0, 2, 0, 2, 2, 2, 0, 0, 1, 2,\n",
              "       1, 1, 2, 2, 1, 1, 2, 2])"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 39
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "HxfR9Ihvni0z"
      },
      "source": [
        "## Accuracy of Model"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "CX1FbM3tkYLl",
        "outputId": "b077b161-c7bb-4407-dbd5-7f1d92b132b5"
      },
      "source": [
        "# accuracy of model on testing dataset\n",
        "from sklearn.metrics import accuracy_score\n",
        "acc = accuracy_score(y_test, y_pred)\n",
        "acc"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "execute_result",
          "data": {
            "text/plain": [
              "0.8333333333333334"
            ]
          },
          "metadata": {
            "tags": []
          },
          "execution_count": 40
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "S_BkYIuCksHz"
      },
      "source": [
        "Here, y_test are the true class labels and y_pred are the predicted class labels in the test-set."
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "h46WMx1ynnlY"
      },
      "source": [
        "## Confusion Matrix"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "E1IGCjqlknom",
        "outputId": "d3f869a2-06bd-4eff-9f64-04ecdd78e483"
      },
      "source": [
        "# The Confusion Matrix and slice it into four pieces\n",
        "from sklearn.metrics import confusion_matrix\n",
        "\n",
        "confusionMatrix = confusion_matrix(y_test, y_pred)\n",
        "print('Confusion matrix\\n', confusionMatrix)"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "Confusion matrix\n",
            " [[7 0 0]\n",
            " [0 9 3]\n",
            " [0 2 9]]\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "-2LoJDZZnqsG"
      },
      "source": [
        "## Classification Report"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "colab": {
          "base_uri": "https://localhost:8080/"
        },
        "id": "gQ8UC0UGl6VL",
        "outputId": "3612037c-1e99-442a-a4af-99101f8785ff"
      },
      "source": [
        "from sklearn.metrics import classification_report\n",
        "\n",
        "print(classification_report(y_test, y_pred))"
      ],
      "execution_count": null,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "              precision    recall  f1-score   support\n",
            "\n",
            "           0       1.00      1.00      1.00         7\n",
            "           1       0.82      0.75      0.78        12\n",
            "           2       0.75      0.82      0.78        11\n",
            "\n",
            "    accuracy                           0.83        30\n",
            "   macro avg       0.86      0.86      0.86        30\n",
            "weighted avg       0.84      0.83      0.83        30\n",
            "\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "f8OsmbLXmSsi"
      },
      "source": [
        ""
      ],
      "execution_count": null,
      "outputs": []
    }
  ]
}
```