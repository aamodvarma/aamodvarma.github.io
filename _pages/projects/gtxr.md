---
title: Rocket Neural Network
author_profile: true
permalink: /projects/gtxr
---
{% include base_path %}

I am a part of the Georgia Tech Experimental Rocketry Club, Simulations Team to develop a new simulations framework using machine learning to generate aerodynamic coefficient and center of pressure values which would be used to optimize the rocket dimensions..

1. Generating training rocket data on 17 different parameters
2. Simulating training rocket using aerodynamic analysis and flight simulation software
3. Training neural network to predict 6 aerodynamic coefficients and center of pressure values
4. Implementing neural network 

My contributions focus on generating rocket training data, training the neural network and integrating the neural network within our work flow.

## Generating training samples
In order to train our neural network we need to generate training samples within given bounds of our rocket dimensions and parameters. This would then be used as an input to our aerodynamics analysis software where we'll generate prediction of aerodynamic drag, coefficients and center of pressure values. 

To generate these samples I used latin hypercube sampling method which is a statistical method for generating a well distributed set of random samples across our 17 dimensional distribution. As each of our parameters lie in different bounds I first normalize each of ours samples to be between 0 and 1. 

Through this method I was able to reduce the discrepancy of our set of values from 0.0008 for a set of 20k values to 0.0005. As well as down to 0.0002 for a set of 50k values. The discrepancy gives us an idea of the spacial filling aspects of our generated samples. Minimizing our this ensure that we cover the entire possible set of values within our set ensuring our neural network is properly able to learn from these values.

## Neural Network
In this case we want a neural network to output 6 aerodynamic coefficients and center of pressure values from 16 values of rocket dimensions. For this purpose I used a feed forward network with 2 linear layers. 

![Model Architecture](/images/rnn_model.png)

