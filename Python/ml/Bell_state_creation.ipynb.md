```ipynb
{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {
    "id": "zNvhCnuxI7A4"
   },
   "outputs": [],
   "source": [
    "from qiskit import __qiskit_version__, QuantumCircuit, execute, Aer\n",
    "from qiskit.visualization import plot_histogram, plot_bloch_vector, plot_bloch_multivector\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "id": "p2528vcNQoaw"
   },
   "source": [
    "# Bell States\n",
    "Bell state - A specific quantum state of two qubits which is maximally entangled. They represent the simplest examples of entangled states.Bell states are used in Quantum Teleportation,Quantum cryptography and super dense coding.\n",
    "There are four known Bell states:\n",
    "\n",
    "\n",
    "$$|\\phi^+\\rangle = \\frac{1}{\\sqrt{2}}\\big(|00\\rangle+|11\\rangle\\big)$$\n",
    "$$|\\phi^-\\rangle = \\frac{1}{\\sqrt{2}}\\big(|00\\rangle-|11\\rangle\\big)$$\n",
    "$$|\\psi^+\\rangle = \\frac{1}{\\sqrt{2}}\\big(|01\\rangle+|10\\rangle\\big)$$\n",
    "$$|\\psi^-\\rangle = \\frac{1}{\\sqrt{2}}\\big(|01\\rangle-|10\\rangle\\big)$$\n"
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "id": "b-VIlaFOTDso"
   },
   "source": [
    "# Creating the phi-plus state\n",
    "To create the phi+ state we first take two qubits(both initalized to $|0\\rangle$). Then add the following gates\n",
    "- A Hadamard gate H on the first qubit, which puts it into the superposition state $(|0\\rangle+|1\\rangle)/\\sqrt{2}$. \n",
    "- A controlled-Not operation (CX) between the two qubits. "
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {
    "colab": {
     "base_uri": "https://localhost:8080/",
     "height": 94
    },
    "id": "o-ndWokIQu4z",
    "outputId": "1f73c5d0-0a2c-4282-ae6f-f134fd345053"
   },
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 206.852x144.48 with 1 Axes>"
      ]
     },
     "execution_count": 3,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#Creating the phi+ state\n",
    "#Create a quantum circuit with 2 qubits which are by default in |0> state\n",
    "phi_plus = QuantumCircuit(2)\n",
    "# add a Hadamard gate to the first qubit\n",
    "phi_plus.h(0)  \n",
    "# add a cnot gate to the circuit with first qubit as control qubit and second qubit as target qubit\n",
    "phi_plus.cx(0,1)            \n",
    "phi_plus.draw('mpl')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {
    "colab": {
     "base_uri": "https://localhost:8080/"
    },
    "id": "Bb1QjT-aSfJj",
    "outputId": "93f818ca-e37f-4c67-c6c3-45e3d548aa16"
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "[0.70710678+0.j 0.        +0.j 0.        +0.j 0.70710678+0.j]\n"
     ]
    }
   ],
   "source": [
    "backend = Aer.get_backend('statevector_simulator') # Tell Qiskit how to simulate our circuit\n",
    "# Create a Quantum Program for execution. This statement runs the circuit.\n",
    "# the circuit we want to run (phiplus), and the simulator we want to use (backend = statevector simulator). \n",
    "job = execute(phi_plus, backend)\n",
    "# Grab the results from the job.\n",
    "result = job.result()\n",
    "# Display the output state vector, output is an list of amplitudes of the states |00>,|01>,|10>,|11> respectively\n",
    "print(result.get_statevector(phi_plus))  "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "id": "xZ66ZOv8VVfr"
   },
   "source": [
    "# Creating the phi-minus state\n",
    "To create the phi-minus state we first take two qubits(both initalized to $|0\\rangle$ ). Then add the following gates\n",
    "\n",
    "- A Hadamard gate H on first qubit, which puts it into the superposition state $(|0\\rangle+|1\\rangle)/\\sqrt{2}$. \n",
    "- A controlled-Not operation (CX) between the two qubits. \n",
    "- A Z gate to the first qubit ."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {
    "colab": {
     "base_uri": "https://localhost:8080/",
     "height": 94
    },
    "id": "npW5mzv9VJ85",
    "outputId": "26b5aaa6-79b3-4e4a-a598-a3e1daf2a074"
   },
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 267.052x144.48 with 1 Axes>"
      ]
     },
     "execution_count": 4,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#Creating the phi-minus state\n",
    "#Create a quantum circuit with 2 qubits which are by default in 0 state\n",
    "phi_minus = QuantumCircuit(2)\n",
    "# add a Hadamard gate to the first qubit\n",
    "phi_minus.h(0) \n",
    "# add a cnot gate to the circuit with first qubit as control qubit and second qubit as target qubit\n",
    "phi_minus.cx(0,1) \n",
    " # add a Z gate to the first qubit\n",
    "phi_minus.z(0)              \n",
    "phi_minus.draw('mpl')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {
    "colab": {
     "base_uri": "https://localhost:8080/"
    },
    "id": "8Xfhb9w-XSY1",
    "outputId": "6c818d94-30dc-48ef-d96c-7d74db1e1d41"
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "[ 0.70710678+0.j -0.        +0.j  0.        +0.j -0.70710678+0.j]\n"
     ]
    }
   ],
   "source": [
    "backend = Aer.get_backend('statevector_simulator') # Tell Qiskit how to simulate our circuit\n",
    "# Create a Quantum Program for execution. This statement runs the circuit.\n",
    "# the circuit we want to run (phiplus), and the simulator we want to use (backend = statevector simulator). \n",
    "job = execute(phi_minus, backend)\n",
    "# Grab the results from the job.\n",
    "result = job.result()\n",
    "# Display the output state vector, output is an list of amplitudes of the states |00>,|01>,|10>,|11> respectively\n",
    "print(result.get_statevector(phi_minus))  "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "id": "HYzH3NISYT9C"
   },
   "source": [
    "# Creating the psi-plus state\n",
    "To create the psi state we first take two qubits(both initalized to $|0\\rangle$ ). Then add the following gates\n",
    "- A Hadamard gate H on first qubit, which puts it into the superposition state $(|0\\rangle+|1\\rangle)/\\sqrt{2}$. \n",
    "- A controlled-Not operation (CX) between the two qubits. \n",
    "- A x gate to second qubit."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {
    "colab": {
     "base_uri": "https://localhost:8080/",
     "height": 94
    },
    "id": "Clo_frQEXkGH",
    "outputId": "b5f5afb8-1fb1-4709-a174-a7c2ff779647"
   },
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 267.052x144.48 with 1 Axes>"
      ]
     },
     "execution_count": 7,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#Creating the phi-minus state\n",
    "#Create a quantum circuit with 2 qubits which are by default in 0 state\n",
    "psi_plus = QuantumCircuit(2)\n",
    "# add a Hadamard gate to the first qubit\n",
    "psi_plus.h(0) \n",
    "# add a cnot gate to the circuit with first qubit as control qubit and second qubit as target qubit\n",
    "psi_plus.cx(0,1)   \n",
    "# add a x gate to the second qubit\n",
    "psi_plus.x(1)               \n",
    "psi_plus.draw('mpl')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {
    "colab": {
     "base_uri": "https://localhost:8080/"
    },
    "id": "gHviYfUNYt7F",
    "outputId": "241c2ca2-8073-4fac-8117-3dda3cfe2cda"
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "[0.        +0.j 0.70710678+0.j 0.70710678+0.j 0.        +0.j]\n"
     ]
    }
   ],
   "source": [
    "backend = Aer.get_backend('statevector_simulator') # Tell Qiskit how to simulate our circuit\n",
    "job3 = execute(psi_plus, backend)\n",
    "# Grab the results from the job.\n",
    "result = job3.result()     \n",
    "# Display the output state vector,output is an list of amplitudes of the states |00>,|01>,|10>,|11> respectively\n",
    "print(result.get_statevector(psi_plus))  "
   ]
  },
  {
   "cell_type": "markdown",
   "metadata": {
    "id": "6dOE-3KVaKJ-"
   },
   "source": [
    "#Creating the psi-minus state\n",
    "To create the psi state we first take two qubits(both initalized to $|0\\rangle$ ). Then add the following gates\n",
    "\n",
    "- A Hadamard gate H on first qubit, which puts it into the superposition state $(|0\\rangle+|1\\rangle)/\\sqrt{2}$. \n",
    "- A controlled-Not operation (CX) between two qubits. \n",
    "- A x gate to the first qubit\n",
    "- A z gate to second qubit.\n"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {
    "colab": {
     "base_uri": "https://localhost:8080/",
     "height": 94
    },
    "id": "H5FiciMhZAgN",
    "outputId": "506f4cdb-cd02-47ad-bfd3-5736f1ddf4ce"
   },
   "outputs": [
    {
     "data": {
      "image/png": "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\n",
      "text/plain": [
       "<Figure size 267.052x144.48 with 1 Axes>"
      ]
     },
     "execution_count": 5,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "#Creating the phi-minus state\n",
    "#Create a quantum circuit with 2 qubits which are by default in 0 state\n",
    "psi_minus = QuantumCircuit(2)\n",
    "# add a Hadamard gate to the first qubit\n",
    "psi_minus.h(0)  \n",
    "# add a cnot gate to the circuit with first qubit as control qubit and second qubit as target qubit\n",
    "psi_minus.cx(0,1) \n",
    "# add a x gate to the first qubit\n",
    "psi_minus.x(0)\n",
    "# add a z gate to the second qubit\n",
    "psi_minus.z(1)               \n",
    "psi_minus.draw('mpl')"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {
    "colab": {
     "base_uri": "https://localhost:8080/"
    },
    "id": "PeVjOaJhanvW",
    "outputId": "ff624d04-e30a-4b02-ef37-aed1597d4907"
   },
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "[ 0.        +0.j  0.70710678+0.j -0.70710678+0.j -0.        +0.j]\n"
     ]
    }
   ],
   "source": [
    "# Tell Qiskit how to simulate our circuit \n",
    "backend = Aer.get_backend('statevector_simulator') \n",
    "job4 = execute(psi_minus, backend)\n",
    " # Grab the results from the job.\n",
    "result = job4.result()        \n",
    "# Display the output state vector,output is an list of amplitudes of the states |00>,|01>,|10>,|11> respectively\n",
    "print(result.get_statevector(psi_minus))  "
   ]
  }
 ],
 "metadata": {
  "colab": {
   "name": "Bell state creation",
   "provenance": []
  },
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
   "version": "3.8.8"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 1
}

```