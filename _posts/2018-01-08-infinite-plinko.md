---
layout: category-post
title:  "Infinite Plinko"
date:   2018-01-08 20:01:00 -0700
categories: general
---

Infinite Plinko and I go way back... all the way to October of 2017.  

In my data science class, there was a question about a simple finite plinko game.  Plinko, of course, is the completely skill-based game from The Price is Right where a player lets go of a disc and watches it bounce between pegs.  Players can then win prizes depending on its final location.

Here's the basic image from the homework:

![Basic Plinko Board]({{ "/assets/basic-plinko-pyramid.png" }}){: .center-image } 

It can look a little weird at first but this pyramid of circles provides an easy way to investigate some exciting aspects of mathematics.  Each of the black circles represents a peg.  When the disc hits the peg, it can either move to the left or the right.  For this example, we assume that there is an equal chance of the disc moving in either direction.  Though it is possible to solve this problem with a different probability, it is far easier when probability of a rightward move is .5.  The buckets at the bottom represent possible end locations.  While in the show, you can win cash prizes, here you only win personal satisfaction.  One thing that you can start to see is that the number of times that the puck moves to the right corresponds to the number of its end bucket.

Try it out for yourself!  If you follow down from the top peg to any bucket, the number of right moves corresponds to the final bucket where it lands.  So if a path is _RLRLRL_, it ends up in bucket 3 as there were 3 right moves.  This also holds true no matter the order of the moves.  _RRRLLL_, _LLLRRR_, and _LRLRLRLR_ all end up in bucket 3.

We can even validate this with a simulation:

```python
def plinko_trial(num_rows=6, p=0.5):
    bucket = 0 
    for ii in range(num_rows):
        bucket += np.random.choice([0,1], p=[1-p, p])
    return bucket

def easy_plinko(num_samples=int(1e2), rows=6, p=0.5):
    final_bins = [plinko_trial(rows, p) for _ in range(num_samples)]
    return final_bins
    
def graph(final_bins):
    fig, ax = plt.subplots(nrows=1, ncols=1, figsize=(12,6))
    ax.hist(final_bins, bins=[0, 1, 2, 3, 4, 5, 6, 7], normed=True)

graph(easy_plinko(10000))
```

This creates the following histogram of distributions:

![Plinko Board with Boundaries]({{ "/assets/plinko-sim.png" }}){: .center-image } 

Now this looks exactly like the distribution for a binomial random variable.  This makes sense, too.  Each time the disc reaches a peg can be thought of as an individual trial.  For each trial, there is the probability $$p=.5$$ that the disc moves to the right.  The final location is the sum of successful moves to the right.  Now this sounds exactly like a Binomial random variable.  Every successful trial represents a move to the right.  Every failute is a move to the left.  The number of trials is simply the number of peg rows that the disc encounters.  Therefore, the given image can be represented as:

$$X \text{ ~ } Bin(6, 0.5)$$

From here, the problem only gets more interesting.  Solving for a single pyramid is fun and all but it lacks something that the actual plinko board has: boundaries.

Boundaries provide the exciting (and confusing) part of the problem.  The idea of a boundary can be seen below:

![Plinko Board with Boundaries]({{ "/assets/bounded-plinko-pyramid.png" }}){: .center-image } 

Here, the board has walls at either side.  This is represented by the red dots.  When the wall only separates off a single bucket, it is easy to see what happens.  Every disc that was going to land in bucket 0 ends up in bucket 1.  On the other side, every disc that was going to land in 6 lands in 5.  Now this is the important thing to recognize about a disc's interation with a wall:  _The end location of a blocked disc is the reflection of the original destination about the bounding wall._

Before we start to unpack that claim, let's take a detour to talk about another game: pool.

In pool, whenever you are trying to line up a shot that rebounds off a wall you use the Law of Reflection.   This law states that the angle of reflection is equal to the angle of incidence.  Therefore, you can line up your shot by imagining that there is another pool table on the other side of the wall.  

With this in mind you can imagine that instead of the disc being rebounded by the wall, it simply continues onto another plinko board mirrored on the other side.  In this mirrored plinko world, all directions are switched.  So a left in our world becomes a right in this one.

![Bounded Plinko Board with Mirror]({{ "/assets/bounded-plinko-mirror.png" }}){: .center-image } 

The idea of a mirrored plinko world becomes far more useful when multiple buckets are cut off by a boundary.  While only 1 potential path
