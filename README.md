# Unknown_Neural_Lyapunov
This repository contains the code for the paper: Neural Lyapunov control of unknown nonlinear systems with stability guarantees

## Requirements
- [dReal4: v4.19.02.1](https://github.com/dreal/dreal4)
- [PyTorch: 1.2.0](https://pytorch.org/get-started/locally/)

### To calculate the Lipschitz constant of neural networks
- [LipSDP](https://github.com/arobey1/LipSDP)

## How it works
This algorithm is made up of two neural netowrks and an SMT solver. The first neural network is responsible for learning the unknown dynamics. The second neural network aims to identify a valid Lyapunov function and a provably stabilizing nonlinear controller. The SMT solver then verifies that the candidate Lyapunov function indeed satisfies the Lyapunov conditions.

## A typical procedure is as follows:
- Learn the unknown dynamics with a Neural network (FNN)
- Define the neural network with random parameters for Lyapunov function and initialize controller’s parameters with a LQR controller (VNN)
- Define the learned dynamics with a nonlinear controller
- Set checking conditions for falsifier with dReal
- Start training and verifying for the neural Lyapunov function
- Procedure stops when no counterexample is found

FNN approximates the unknown dynamics by minimizing the Mean Square Loss bettween the outputs and the targets, and the training process stops when the max of the 2-norm over all the samples reaches the desired value. In VNN, the learning process updates the parameters by iteratively minimizing the Lyapunov risk, a cost function measures the degree of violation of the Lyapunov conditions and the norm of partial derivatives, while the verifying part (SMT solver) periodically searches counterexample state vectors and adds them back to the training set for the next iteration.

## Example
- [Van der Pol oscillator](https://github.com/RuikunZhou/Unknown_Neural_Lyapunov/blob/main/Van_der_Pol.ipynb)