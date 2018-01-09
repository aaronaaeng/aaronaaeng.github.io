---
layout: category-post
title:  "Infinite Plinko"
date:   2018-01-09 10:39:00 -0700
categories: general
---
_This is a followup to my previous post about Finite Plinko.  If you did not read it yet, you can find it_ [here]({{ site.baseurl }}{% post_url 2018-01-08-finite-plinko %}).

Fite Plinko is fun and all but what if we went further?  The challenge problem listed at the bottom of the homework assignment asked at what height, if any, the distribution of the expected final location of the dropped disc would become uniform.  Solving this as a combination of increasingly longer bounded random variables was too difficult to do properly.  However, what we could do instead is treat it like a Markov chain.

A Markov chain is a stochastic model that represents a possible series of events where the probability of the next event is only dependent on the result of the previous event.  Now that sounds really fancy but what does that really mean?

Imagine you are sleepwalking.  Luckily for you, the doors connecting the rooms in your house are mostly closed.  You can only get to your bedroom (B), living room (L), and your kitchen (k). When you are sleepwalking, you randomly move along one of your available paths with varying probabilities.  We can represent this problem with the following chain:

![Simple Markov Chain]({{ "/assets/simple-markov-chain.png" }}){: .center-image } 

Here you can see all of the potential paths that you can take from any given room and the likelihood that you take that path.  From this, we can compute the likelihood that you are in any given room after any number of random walks.  We do this by create a transition matrix.

A transition matrix is a matrix that represents the likelihood that you move from any node to any other after a single random walk.  For this chain, we can create the following matrix:

![Simple Transition Matrix]({{ "/assets/simple-tran-matrix.png" }}){: .center-image } 

The row serves as the "input" to the system.  So the probability that you move from the kitchen to the living room can be seen to be .25.  Notice that each row sums to exactly 1.  This means that this matrix is a valid stochastic matrix.  That's a fancy way of saying that it represents valid probabilities.  This matrix allows us to calculate the probability of an end location from any start location after any number of random walks.

But what does that even mean?  Well, we can apply it for just one walk.  You start sleepwalking from your bedroom.  That means that after one random walk, there is a probability of .5 that you end in your living room and a probability of .5 that you end up in your kitchen.  We can even use matrix multiplication to get these end locations.  Say you want to determine the likelihood that you end in any location if you start in your kitchen.  We can calculate this by simply multiplying this transition matrix by a vector.  The vector has the value 1 in the location that corresponds to the starting room and has the value 0 for all other locations.

![Simple Matrix Multiplication]({{ "/assets/first-mat-mul.png" }}){: .center-image } 

As you can see, the resulting vector is simply the bottom row of the transition matrix.  We can now use this technique to determine the probability of ending in any location after any number of moves.  This can be done in one of two ways.  You could take this resulting vector and multiply it by the transition matrix to get a new resulting vector.  You can continue to do this however many times you would like until you have found your desired answer.  Unfortunately, if you want to determine the distribtution for your end locations after _n_ walks, you have to repeat this step _n_ times.  Alternatively, we can just raise the transition matrix to the power _n_ prior to multiplying it by the initial vector.  This provides the same result and requires far less steps.

So to calculate the likelihood that you are in any location after two walks, we can just repeat the last multiplication but use the square of the transition matrix instead.

![Second Matrix Multiplication]({{ "/assets/sec-mat-mul.png" }}){: .center-image } 