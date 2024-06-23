*NOTE*: This article has been translated from Farsi using `llama3-70b-8192` and `groq`

It was 1994 when I bought my first personal computer. To be honest, it was bought for me. Back then, the computer's RAM was around 1 megabyte, and the smartest thing this lovely invention could do was to type and make Aladdin jump off the screen.

From then on, computers have made tremendous progress. They've become smaller, just like humans. Now, you can talk to your mobile phone and ask it something. Or show it a road sign image from a foreign country and ask it to read it out loud in your language.

But how is this possible? Can a bunch of Flip-Flop's and a few grams of semiconductor learn how to see and hear?

## Mathematical Functions from Another Perspective
The first type of mathematical function we learn - if I'm not mistaken - is the straight line. The function f(x) = ax + b. At least, this is the first operational function we become familiar with. A function with one variable representing a straight line on a two-dimensional plane, like a piece of paper. Depending on the values of a and b, the position and slope of the line change.

Although a straight line alone can't do much, with its limited capabilities, it can do great things. Especially if we use multiple lines. For instance, with one line, we can divide a two-dimensional plane into two separate parts. One side of the line is the "good" side, and the other is the "bad" side! Now, if we have enough lines, we can divide the plane in any way we want.

![Sample Classification using a Straight Line](img/straight-line-classification-sample.svg)

Computers also look at functions from the same perspective - especially during learning. We have a general function with many parameters. When we teach it, it learns to consider the values of these parameters. The values that can provide our desired outcome. Of course, what we want to divide into "good" and "bad" (or more labels) is not just a simple two-dimensional plane, but the mechanism used is the same as dividing a two-dimensional plane.

Researchers have designed and experimented with various types of these general functions. ANN[>Artificial Neural Network], SVM[>Support Vector Machine]s, Fuzzy functions, PGM[>Probabilistic Graphical Model]s, and more. Again, I emphasize that all these, from a machine learning perspective, are functions with many parameters. These models work on high-dimensional spaces - tens and hundreds of thousands of dimensions - and have many more parameters.

## Training Our Function
To explore this topic better, let's go back to the simple example of a straight line. The function f(x; a, b) = ax + b. This time, I divided the inputs into two parts. The input part, or x, and the parameter part, a and b. From a mathematical and computational perspective, these two categories are no different. They're just helpful for designing and sending the correct inputs. If you're a programmer, you've definitely encountered naming standards! Intelligent use of this tip can solve the problem of training our function.

Before diving into the pool of training, it's essential to say that teaching a computer is similar to teaching a human. We need a teacher and a sufficient amount of training data for the computer to learn from. For instance, we need a collection of labeled images where the written areas are marked. We show these to the computer so that it can learn to find written areas in new images - maybe even read the text in our language.

Assuming our training set is D = {(x1, f1), (x2, f2), ..., (xN, fN)}. To train, we define a new function that only takes a and b as inputs. Let's call it the error function and denote it by J.

$$$
J(a, b) = \sum_{i=1}^N \|f(x_i; a, b) - f_i\|^2
$$$

Now, with the help of our teacher, we find the values of a and b that minimize J. This means finding the values that better provide our desired outcome. If you look closely at the form of J, you'll see that it calculates the difference between the learned function and our desired output, squares it, and sums it up for all the samples we've shown to the computer. The smaller the value of J, the smaller the individual terms inside the sigma are - since they're all positive.

And our noble teacher is none other than the "optimization toolbox". A toolbox that researchers in optimization, mathematics, and engineering have provided for us. If you're interested in this topic, I recommend reading [Stephen Boyd's](http://stanford.edu/~boyd/cvxdbook/bv_cvxdbook.pdf) book - one of the best scientists I admire. But as a user, algorithms exist and toolboxes are written. For simple issues like linear programming - as dear Boyd says - the tools have become technology and can be used without getting too deep.

I've gone on a bit of a tangent, and patience is limited. So, may your life be full of energy.