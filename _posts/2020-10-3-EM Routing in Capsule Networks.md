EM routing was proposed by Hinton in his 2018 paper describing matrix capsules. He proposed matrix capsules networks as an alternative to standard convolutional neural nets which
according to him, do not consider some prior geometrical knowledge about the 3D world that images (or cameras) capture and consequently struggle to easily capture
spatial relationships. In his paper, he then proposed the so-called EM-routing as the learning procedure for the parameters of this new model.

To put it simple, each pair of adjacent layers in the matrix capsule network engage into EM iterations, defined as: each higher-layer capsule is defined by a Gaussian and 
the pose of each lower-layer *active* capsule corresponds to a data point in the mixture.

Hinton's main idea with capsule networks is to basically learn the hierarchy of features by doing some kind of per-layer clustering, expecting that at convergence
features would group themselves to form higher-level features in a (spatially) meaningful way. So, instead of leaving this feature derivation at higher levels to the data
and the learning process (as in CNNs), he proposes to control this grouping by: (1) using a capsule instead of a neuron, (2) using EM routing for maximizing the likelihood
of this grouping.

Now, the capsule itself is a container of: an activation neuron + 4x4 pose matrix (that is supposed to capture spatial pose information - rotation, translation or change
of the camera viewpoint). 

EM

In order to describe better the EM-routing, let's first recall the general form of expectation maximization algorithm.
So, EM is a bayesian method for finding MAP estimates of model parameters. EM performs maximum a posteriori estimation by considering a latent variable model and 
maximizing the joint log likelihood (of the data and the latent variable). Maximizing the joint log likelihood is equivalent to maximizing the marginal log likelihood
considering iid samples. 

From this point, EM:
- tries to maximize the marginal log-likelihood by introducing a family of lower bounds (lower bound is a concave function). 
- considering that the family of lower bounds is defined by two things: (1) the model parameters and (2) the variational parameter q (a function), it tries to
maximize the lower bound in a block coordinate fashion:

During E-step: it fixes parameters, optimizes for q.
During M-step: it fixes q, optimizes for model parameters


EM is often used for fitting a mixture of Gaussians (introducing a latent variable that `causes` the different gaussians). This is done by optimizing for
the mixture parameters (means and covariance matrices)

EM routing.

So, in the proposed EM-routing, Hinton proposes to iteratively means, variances and activation probabilities of the capsules by using EM for fitting a Gaussian mixture.
The E-step determines the assignment probability of each datapoint(a lower-layer capsule) to a parent capsule. The M-step re-calculates the mixture parameter based on assignment probability.
This iteration is repeated 3 times. The last activation value will be the parent capsule’s output. The 16 μ (means) from the last Gaussian model will be reshaped to form the 4×4 pose matrix of the 
parent capsule.

