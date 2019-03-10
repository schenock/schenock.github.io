
Very often when I am reading something I take disorganized, messy and fuzzy notes which sometimes I re-read and re-think. I decided to start putting them in my blog, as I add my favourite readings. Let me start with this one:  [It’s Only Natural: An Excessively Deep Dive Into Natural Gradient Optimization
](https://towardsdatascience.com/its-only-natural-an-excessively-deep-dive-into-natural-gradient-optimization-75d464b89dbb)


This is a very interesting reading. Something that you won't find in your ordinary machine/deep learning course. No wonder why there are so many "deep learning practitioners", students and people who actually find it interesting but get somehow stuck in understanding deeply the underlying "machine" which is mostly theoretical and it comes from fields such as mathematical optimization, information theory etc. In my humble opinion, students should be encouraged to try to understand the underlying principles. This reading just reminded me of the importance of such a way of thinking.


> *"It’s providing a way to directly control movement of your model in predicted distribution space, as separate from movement of your model in loss space."*


It is a question itself if controlling the movement in predicted distribution space is of any importance at all. **The initial predicted distribution might be very far from the true distribution in KL-divergence or any other sense.** Controlling movement in loss space actually matters (although no doubt is a difficult task), but IMHO, controlling in predicted distribution space seems of no use. Maybe I am missing some key points here, but to the best of my knowledge this doesn't bring additional value in the optimization process. 

Although correct, looks like I jumped to some conclusions before reading the next paragraph. 

> *"But, other than being elegant, it’s not clear to me that it’s providing value that couldn’t be provided via more heuristic means."*

Completely agreed regarding the elegance of it.


I have to admit, I feel very happy when I find very well-compiled sentence expressing a statement that is somehow true, in a nice framework defining the boundaries that determine exactly how and when it holds. This is one example: 

> *"A (admittedly more heuristic than it is rigorous) way I like to think about this is that if you’re in a region where gradients from point to point are very variable (that is to say: high variance), then your minibatch estimate of the gradient is in some sense more uncertain."*




**Plot twist**

> *"So by capturing information about the curvature of the log-likelihood-derivative space at a given point, Natural Gradient is also giving us a good signal of the curvature in our true, underlying loss space."*


It turns out that Natural Gradient is computing the Fisher Information Matrix, that corresponds to the gradient changes. **But not the real gradient** -  the gradient of the empirical likelihood for the points in your batches. To put it simply, the marginal value Natural Gradient is just giving a **(supposedly and hopefully fair)** estimate of the real gradient of the loss function. 

But of course, this comes at a price, and the price is calculating the Fisher Information Matrix required to compute the natural gradients. And there is **no evidence that this is in any case faster than
simple second-order optimization** i.e. calculating second derivatives with respect to underlying loss.   


Most importantly, this reading made me realize one very intereting thing, that when we *"scale"* the gradients we actually multiply the gradient vector by a **scalar** and the impact of this update to each parameter is not obvious (at least not immediately). 

>*"..parameters can exist on different scales, and can have different degrees of impact on your learned conditional distribution."*

Especially the latter can be very true and therefore we need to revisit our approach for updating the gradient vector by simply multiplying by a scalar.

However, in any case, in the end it all boils down to a fair approximation of the second derivatives of the underlying loss function. And we might not need to go deeper as no notable speed up in the process
we are going to get (when talking about speed of convergence, after RMSProp, Adam etc.). I guess the more important topic within optimization is avoiding "falling" into local extrema.
