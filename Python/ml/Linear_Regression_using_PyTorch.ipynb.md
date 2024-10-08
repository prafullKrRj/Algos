```ipynb
{
  "nbformat": 4,
  "nbformat_minor": 0,
  "metadata": {
    "colab": {
      "name": "Untitled23.ipynb",
      "provenance": [],
      "collapsed_sections": []
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
        "id": "q_lqqTn0DYBR",
        "colab_type": "text"
      },
      "source": [
        "# Using PyTorch to solve a linear regression problem from scratch"
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "FXp9Wo4gsleM",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "import numpy as np\n",
        "import torch"
      ],
      "execution_count": 2,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "iIM6-lIvtzbu",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "#Input values\n",
        "inputs = np.array([[73,67,43],\n",
        "                   [91,88,64],\n",
        "                   [87,134,58],\n",
        "                   [102,43,37],\n",
        "                   [69,96,70]], dtype='float32')"
      ],
      "execution_count": 3,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "JfBeB_EXunO0",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "#Actual target values\n",
        "targets = np.array([[56,70],\n",
        "                    [81,101],\n",
        "                    [119,133],\n",
        "                    [22,37],\n",
        "                    [103,119]], dtype='float32')"
      ],
      "execution_count": 4,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "63X-0P8NvA_X",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "#covert numpy array to tensors\n",
        "inputs = torch.from_numpy(inputs)\n",
        "targets = torch.from_numpy(targets)"
      ],
      "execution_count": 5,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "RKi9oDCNzCTO",
        "colab_type": "code",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 187
        },
        "outputId": "b9e9d92b-3be2-438c-888a-f90f050e90a3"
      },
      "source": [
        "print(inputs)\n",
        "print(targets)"
      ],
      "execution_count": 6,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "tensor([[ 73.,  67.,  43.],\n",
            "        [ 91.,  88.,  64.],\n",
            "        [ 87., 134.,  58.],\n",
            "        [102.,  43.,  37.],\n",
            "        [ 69.,  96.,  70.]])\n",
            "tensor([[ 56.,  70.],\n",
            "        [ 81., 101.],\n",
            "        [119., 133.],\n",
            "        [ 22.,  37.],\n",
            "        [103., 119.]])\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "ELGckCNBzI3q",
        "colab_type": "code",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 68
        },
        "outputId": "747eb29b-4942-4e48-f2e9-77ea4863e2fb"
      },
      "source": [
        "w = torch.randn(2,3,requires_grad=True)\n",
        "b = torch.randn(2,requires_grad=True)\n",
        "print(w)\n",
        "print(b)"
      ],
      "execution_count": 7,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "tensor([[-0.3658,  1.6638, -0.5992],\n",
            "        [-0.7918, -0.5693, -1.1982]], requires_grad=True)\n",
            "tensor([1.1520, 0.8294], requires_grad=True)\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "Mz6oeIjdz2dX",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "#Creating our model\n",
        "def model(x):\n",
        "  return x @ w.t() + b"
      ],
      "execution_count": 8,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "WKwH18D80usV",
        "colab_type": "code",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 102
        },
        "outputId": "e1132eaf-f552-49cc-c3eb-b38d0a529b7e"
      },
      "source": [
        "#Making predictions\n",
        "preds = model(inputs)\n",
        "print(preds)"
      ],
      "execution_count": 9,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "tensor([[  60.1563, -146.6406],\n",
            "        [  75.9281, -198.0117],\n",
            "        [ 157.5207, -213.8439],\n",
            "        [  13.2123, -148.7497],\n",
            "        [  93.6910, -192.3360]], grad_fn=<AddBackward0>)\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "_cPLvKX-1A7e",
        "colab_type": "code",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 102
        },
        "outputId": "4cae98c9-682d-48e4-e51b-23bf2d0b64e4"
      },
      "source": [
        "print(targets)"
      ],
      "execution_count": 10,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "tensor([[ 56.,  70.],\n",
            "        [ 81., 101.],\n",
            "        [119., 133.],\n",
            "        [ 22.,  37.],\n",
            "        [103., 119.]])\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "DcNWgbNX1CHj",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "#creating loss function MSE\n",
        "def mse(t1,t2):\n",
        "  diff = t1-t2\n",
        "  return torch.sum(diff * diff) / diff.numel()"
      ],
      "execution_count": 11,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "_ftCUOO51oUy",
        "colab_type": "code",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 34
        },
        "outputId": "b8b442a1-e01c-46ea-cec3-4bf037601c3e"
      },
      "source": [
        "#compute loss\n",
        "loss = mse(preds,targets)\n",
        "print(loss)"
      ],
      "execution_count": 12,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "tensor(38976.5625, grad_fn=<DivBackward0>)\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "401dxobHBvJ4",
        "colab_type": "text"
      },
      "source": [
        "##  The loss is higher , we'll use gradient descent to minimize the loss by adjusting the weights and biases."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "Xfrm94Jr19tx",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "#compute gradients\n",
        "loss.backward()"
      ],
      "execution_count": 14,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "3Mud6UFZ8QBs",
        "colab_type": "code",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 85
        },
        "outputId": "9136a779-aa3d-4051-c880-d863ec90b96f"
      },
      "source": [
        "print(w)\n",
        "print(w.grad)"
      ],
      "execution_count": 15,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "tensor([[-0.3658,  1.6638, -0.5992],\n",
            "        [-0.7918, -0.5693, -1.1982]], requires_grad=True)\n",
            "tensor([[   330.9023,    744.4784,    222.3100],\n",
            "        [-22725.7812, -25036.1055, -15447.1006]])\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "-62EpYZn8eBM",
        "colab_type": "code",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 51
        },
        "outputId": "3112cb7c-d32c-4199-cd03-c9e43610f37e"
      },
      "source": [
        "print(b)\n",
        "print(b.grad)"
      ],
      "execution_count": 16,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "tensor([1.1520, 0.8294], requires_grad=True)\n",
            "tensor([   3.9017, -271.9164])\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "HdqYmamw8ivs",
        "colab_type": "code",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 68
        },
        "outputId": "dc1ec2e0-77f1-469a-99e4-7b93677d6c98"
      },
      "source": [
        "#now we can reset the gradient back to zero\n",
        "w.grad.zero_()\n",
        "b.grad.zero_()\n",
        "print(w.grad)\n",
        "print(b.grad)"
      ],
      "execution_count": 17,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "tensor([[0., 0., 0.],\n",
            "        [0., 0., 0.]])\n",
            "tensor([0., 0.])\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "slQyKUnU84r6",
        "colab_type": "code",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 102
        },
        "outputId": "3c2e6ea2-39c6-474b-a565-256fdc7afd8f"
      },
      "source": [
        "# Now trying to optimize predictions and minimize the loss using gradients\n",
        "#Making predictions\n",
        "preds = model(inputs)\n",
        "print(preds)"
      ],
      "execution_count": 18,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "tensor([[  60.1563, -146.6406],\n",
            "        [  75.9281, -198.0117],\n",
            "        [ 157.5207, -213.8439],\n",
            "        [  13.2123, -148.7497],\n",
            "        [  93.6910, -192.3360]], grad_fn=<AddBackward0>)\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "2xqpwOm3CH5U",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "loss = mse(preds,targets)"
      ],
      "execution_count": null,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "Na_2MGiy9fFo",
        "colab_type": "code",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 68
        },
        "outputId": "620aa727-389e-436b-c4a9-f8fc5a2d73f7"
      },
      "source": [
        "#compute gradients\n",
        "loss.backward()\n",
        "print(w.grad)\n",
        "print(b.grad)"
      ],
      "execution_count": 20,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "tensor([[   330.9023,    744.4784,    222.3100],\n",
            "        [-22725.7812, -25036.1055, -15447.1006]])\n",
            "tensor([   3.9017, -271.9164])\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "XEWFhTUA9nrr",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "with torch.no_grad():\n",
        "  w -= w.grad * 1e-5\n",
        "  b -= b.grad * 1e-5\n",
        "  w.grad.zero_()\n",
        "  b.grad.zero_()"
      ],
      "execution_count": 21,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "ZoNvrPnt-4Ij",
        "colab_type": "code",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 68
        },
        "outputId": "c5de8657-daeb-43bd-f35f-f4106f81ca52"
      },
      "source": [
        "#checking the new weights and bias\n",
        "print(w)\n",
        "print(b)"
      ],
      "execution_count": 22,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "tensor([[-0.3691,  1.6563, -0.6014],\n",
            "        [-0.5645, -0.3190, -1.0438]], requires_grad=True)\n",
            "tensor([1.1520, 0.8321], requires_grad=True)\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "MG5LpmwS_Fmz",
        "colab_type": "code",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 34
        },
        "outputId": "112f3e3e-58a1-42e9-accd-71a0e2cb85b8"
      },
      "source": [
        "#Making predictions again\n",
        "preds = model(inputs)\n",
        "loss = mse(preds,targets)\n",
        "print(loss)"
      ],
      "execution_count": 24,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "tensor(26387.7773, grad_fn=<DivBackward0>)\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "Uik5vNZxCNLb",
        "colab_type": "text"
      },
      "source": [
        "## We can see that there is a little decrement in the loss and new predictions are more accurate than previous predictions. Now we'll be training our model for 100 epochs."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "LWBc9lLq_RDD",
        "colab_type": "code",
        "colab": {}
      },
      "source": [
        "# Training for multiple epochs\n",
        "for i in range(100):\n",
        "  preds = model(inputs)\n",
        "  loss = mse(preds,targets)\n",
        "  loss.backward()\n",
        "  with torch.no_grad():\n",
        "    w -= w.grad * 1e-5\n",
        "    b -= b.grad * 1e-5\n",
        "    w.grad.zero_()\n",
        "    b.grad.zero_()"
      ],
      "execution_count": 31,
      "outputs": []
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "5_FzF4gHAct3",
        "colab_type": "code",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 187
        },
        "outputId": "e6d3d16e-af41-42fc-cc69-26049567db3c"
      },
      "source": [
        "#Checking the new predictions and actual targets\n",
        "preds = model(inputs)\n",
        "print(preds)\n",
        "print(targets)"
      ],
      "execution_count": 32,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "tensor([[ 57.9286,  71.6112],\n",
            "        [ 75.0291,  94.7311],\n",
            "        [133.7938, 144.3751],\n",
            "        [ 23.6210,  43.0771],\n",
            "        [ 88.1792, 105.4346]], grad_fn=<AddBackward0>)\n",
            "tensor([[ 56.,  70.],\n",
            "        [ 81., 101.],\n",
            "        [119., 133.],\n",
            "        [ 22.,  37.],\n",
            "        [103., 119.]])\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "3e2kiQvnA4Y9",
        "colab_type": "code",
        "colab": {
          "base_uri": "https://localhost:8080/",
          "height": 34
        },
        "outputId": "f7fb88dd-038d-421c-e4a4-cbcfa1d2b727"
      },
      "source": [
        "# Checking new loss\n",
        "loss = mse(preds,targets)\n",
        "print(loss)"
      ],
      "execution_count": 33,
      "outputs": [
        {
          "output_type": "stream",
          "text": [
            "tensor(87.2747, grad_fn=<DivBackward0>)\n"
          ],
          "name": "stdout"
        }
      ]
    },
    {
      "cell_type": "markdown",
      "metadata": {
        "id": "rNNH9AvwCpKq",
        "colab_type": "text"
      },
      "source": [
        "Final predictions are much better than previous predictions and even the loss is lowered."
      ]
    },
    {
      "cell_type": "code",
      "metadata": {
        "id": "CW7vvWnuBkFr",
        "colab_type": "code",
        "colab": {}
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