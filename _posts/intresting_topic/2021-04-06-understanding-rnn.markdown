---
layout:     post
title:      "Understanding Recurrent Neural Networks"
subtitle:   " \"A Quick introduction to the building blocks of the algorithm powering sequence models\""
date:       2021-04-06 12:00:00
author:     "AD"
header-img: "img/post-bg-rnn.jpeg"
catalog: true
tags:
    - Machine Learning
    - Blog
---
Quick! complete the series A, B, C, D, E, F… That is easy right. Now try this Z, Y, X, W, V… Not that simple! How do we remember the same set of characters in a particular sequence and when the same set is to be recalled in a different way we struggle so hard. Take another example remember the lyrics of a song you know by heart and try singing it backwards, pretty tough isn’t it? It is easy to notice that the order in which the alphabets or the lyrics of a song occur is very important. Such problems where the **sequence or order** of the events is really important for predicting the next event are usually tackled by Recurrent Neural Networks or RNN. Speech recognition, language translation, stock market price prediction, music generation etc. all these tasks are performed by RNNs.

Lets see a comparison between RNN and a simple feed forward neural network and why RNNs are better suited for sequence modelling.
![img](/img/in_post/../in-post/post-rnn/rnn.png)
A simple feed forward neural network is really good at learning a pattern between a set of inputs and outputs. For example given an image the network can tell you whether the image contains a dog or not. But if you input another image of say an elephant followed by an image of a dog it has no context or memory of the dog’s image. It will classify the elephant’s image independently. This type of mechanism is not suited well to tasks which require previous context for making future predictions. For example lets say you have to predict the price of a stock —
![stock](/img/in-post/post-rnn/stock_prie.jpeg)
You can’t simply predict an arbitrary value for the stock price depending on the time. You will need to understand the trend of the stock, in other words you need to know how the stock price has been moving in the past to predict the future.
Recurrent Neural Networks

The recurrent neural network is a special type of neural network which not just looks at the current input being presented to it but also the previous input. So instead of

    Input → Hidden → Output
    it becomes
    Input + Previous Hidden → Hidden → Output

A Recurrent Neural Network

The schematic shows a representation of a recurrent neural network. The green block with the label A is a simple feed forward neural network we are familiar with. The right hand side of the equality sign in the image shows the network for each time step i.e. at t=0 the input X0 goes into the network to produce H0, the next time step the input is X1 but there is an additional input from the previous time step from the block A. This way the neural network not just looks at the current input but has the context from the previous inputs as well.

With a structure like this the RNN model starts to care about the past and what is coming next. As the recurrent units hold the past values, we can refer to this as memory. We are now able to consider a real meaning of context in data.

Lets look at one RNN unit and the functions governing the computation. This unit depicts the computation at any time t. The function below compute the output at a single time step.

A is the hidden layer output, g() is the activation function, W is the weight matrix, X is the input, H is the output at a time step and b is the bias. Note that the parameters are not different for different time steps. As you can see the current time step computation takes into account the previous hidden layer output as well to get the context. Nothing too difficult till now, just give inputs to your RNN one after the other and it should give out predictions keeping in mind the sequence or order in which the data is fed in. But how do you train such a network? Did you say backpropagation?
Backpropagating Through Time

This might sound like a time-travel science fiction movie title but backpropagation through time is the algorithm by which you train RNNs. Backpropagation through time is similar to usual backpropagation where you compute a forward pass for a data input then calculate the error at the output and backpropagate the error back to the neural network from the output to the input layer. But in backpropagation through time because the parameters (weights of the neural network) are shared by all time steps in the network, the gradient at each output depends not only on the calculations of the current time step, but also the previous time steps. For example, in order to calculate the gradient at time step t=4 we would need to backpropagate 3 steps and sum up the gradients. This is called Backpropagation Through Time (BPTT). Lets try to intuitively understand BPTT.

Suppose you have a sequential input series X1, X2, X3, X4 …. XN. You pass each of these input examples to the RNN to compute output at each timestep and at each timestep you compute the loss function as you do in regular backpropagation. In BPTT you accumulate all the losses for each time step and then pass the gradients backwards through the network again. Pictorially it looks like —

For example you have a sentence “What time is it?” So X0 = ‘what’, X1= ‘time’, X2=’is’, X3=’it’. You pass each of these inputs to the network and compute loss on each output and sum them together and compute the error. Now this error is backpropagated through the network for each time step —

As you can see the errors are propagated back to the network for each time step and that is where this gets its name from. Of course the vanilla BPTT is not bulletproof and has lot of issues and there are techniques to tackle them. One of the major issue with BPTT is the problem of vanishing gradients. But for now we keep it for a future article when we discuss about Gated Recurrent Units (GRUs) and LSTM as a solution to this and other problems. It will be easier to get an intuition about it with the solution in context.

## Closing Remarks

Now the next time you see google seamlessly recognizing your speech and converting into text or translating languages you know that there is some form of a RNN working behind the scenes. Honestly, a more concrete understanding of any concept can be gained by actually building those models from scratch. I will try to post a code walk-through of an RNN in Python soon. Meanwhile you can read up material from the internet. I found these posts really helpful and easy to get an understanding about RNNs.