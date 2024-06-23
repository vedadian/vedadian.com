*NOTE*: This article has been translated from Farsi using `llama3-70b-8192` and `groq`

Now that we've reached the practical application of simple learnable models in the previous two parts ([first](/learnable-models-this-understanding-computer-how-does-it-learn) and [second](/learnable-models-straight-line-the-simple-righteous-function)), it's better to wrap up this topic quickly; firstly, lengthy series can be tedious, and secondly, even if I write a whole book about learnable models, the discussions won't be exhausted. Of course, I confess that I will revisit this topic frequently -- of course, in the future -- since artificial intelligence, which these models are based on, is one of my interests.

It's not bad to mention before we begin that I didn't choose the name "Support Vector Machine" for the separating lines in the previous post. Well, I didn't select that name; my friends had already translated it from [Support Vector Machine](https://fa.wikipedia.org/wiki/%D9%85%D8%A7%D8%B4%DB%8C%D9%86_%D8%A8%D8%B1%D8%AF%D8%A7%D8%B1_%D9%BE%D8%B4%D8%AA%D9%8A%D8%A8%D8%A7%D9%86%DB%8C), which is a type of learnable model and operates based on what we saw in the previous post with some details. I don't know a better name for these lines.

## Real learnable models
But the main point is that the functions used in practice are usually much more complex than a straight line. Functions with millions of parameters. For example, in deep neural networks used for machine translation, there are over 8 million parameters (see this [article](https://arxiv.org/pdf/1409.0473.pdf)).

This huge difference in the number of parameters, although it doesn't change the main concepts, significantly alters the technical execution details. For instance, when we have simple functions with few parameters, the local minima and saddle points in the function domain are much fewer, and optimization algorithms are more likely to reach the optimal point. On the other hand, optimization methods based on second-order derivatives, which have high convergence speed, require a much larger memory when dealing with functions having a large number of parameters. If the terms I used are unfamiliar, don't worry; to summarize, the discussion of optimization and finding the optimal parameters becomes much more difficult.

However, our world has made significant progress since the first time the primary-secondary method was used in practice. Before, in addition to requiring minimal knowledge to calculate derivatives and second-order derivatives of complex functions, we also needed inaccessible processing resources to make training real models possible. But now, with a suitable GPU, which costs around 1.5 million Tomans, you can use new software libraries that do all the calculations and optimizations for you and write exciting programs.

If you're interested in artificial intelligence, I strongly recommend getting familiar with [PyTorch](http://pytorch.org/). This rich library, written for the Python language, can greatly accelerate AI research. When you define a model using this library, both derivative calculation and optimization are easily performed using the tools provided. Moreover, executing training and testing the models you design will happen with a few GPU commands. Something that previously required more knowledge and much more work.

Another exciting thing is the practical examples provided by the development team using this library, which are publicly available on [GitHub](https://github.com/pytorch/examples). Before trying to run the examples, don't forget to install PyTorch! The installation commands are available on the first page of the [website](http://pytorch.org/) under "Run this command:".

To motivate you to visit the examples, I'll list some of the exciting titles among them:
1. Super Resolution (a kind of image quality improvement)
2. Language Model (a tool for generating text in a specific style and scoring sentence quality)
3. MNIST (handwritten digit recognition)

Another example with a twist is image style transfer:
![Image Style Transfer](/img/neuralstyle.png)
You can find the source code and explanations [here](http://pytorch.org/tutorials/advanced/neural_style_tutorial.html).

## Why the series on learnable models was created
During these years, while working and researching in AI-related fields, I often disagreed with my friends and colleagues' perspective on machine learning. I mean, when issues deviated from the common form -- for instance, when a new learning method was proposed or a new function was suggested, or the connection between manifold learning and artificial neural networks was mentioned -- that perspective hindered deep understanding (at least, I thought so!).

So, I tried to write something that would demonstrate the machine learning approach more clearly, away from technical details. That machine learning is equivalent to finding the minimum point of a mathematical function, which we also see in high school mathematics. That when we represent neural networks with beautiful graphs reminiscent of neural connections in the brain, we shouldn't forget that the mathematical relations governing the connections between two layers are simply $y=\sigma\left(Wx+b\right)$.