# Background

## Generative Adversarial Network (GAN)

A GAN consists of two networks: a _generator_ network _G_ and a discriminator
network _D_ [1]. The generator _G_ takes as input a random noise ___z___ sampled
from a prior distribution, _p<sub>z</sub>_ and output a fake sample _G(__z__)_.
The discriminator _D_ takes as input either a sample drawn from real data or
generated by the generator and try to tell the fake data from real data.

The adversarial setting goes like this:

- while _D_ try to tell the fake data from real data in each run
- _G_ also try to fool _D_ (to make _D_ misclassified the generated, fake data
  as real ones)

Theoretically, the model distribution _p(G(__z__))_ will become closer to the
data distribution as the training unfolds.

<img src="figs/gan.png" alt="system" style="max-width:400px;">

> Formally, _G_ is actually learning a projection from the prior distribution
_p(__z__)_ to the data distribution _p(__x__)_. To make it clear, it's a
distribution-to-distribution mapping rather than a sample-to-sample mapping.

## Conditional Generative Adversarial Networks (CGAN)

In a conditional GAN (CGAN) [4], both the generator _G_ and the discriminator
_D_ are now conditioned on some variable _y_. Typical (_x_, _y_) pairs include
(data, label), (data, tag vector), (image, image).

<img src="figs/cgan.png" alt="system" style="max-width:500px;">

## Wasserstein GAN (WGAN)

The objective functions for the generator and the discrimnator in a traditional
GAN is given as:

__(Discriminator)__ _max_ __E__<sub>__x__~_p<sub>d</sub>_</sub>
[log(_D_(__x__))] + __E__<sub>___z___~_p<sub>z</sub>_</sub>
[1 - log(_D_(_G_(___z___)))]

__(Generator)__ _min_ __E__<sub>___z___~_p<sub>z</sub>_</sub>
[1 - log(_D_(_G_(___z___)))]

An alternative form called WGAN was later proposed with the intuition to
estimate the Wasserstein distance between the real and the model distributions
by a deep neural network and use it as a critic for the generator [2]. The
objective functions for WGAN can be formulated as:

__(Discriminator)__ _max_ __E__<sub>__x__~_p<sub>d</sub>_</sub> [_D_(__x__)]
&ndash; __E__<sub>___z___~_p<sub>z</sub>_</sub> [_D_(_G_(___z___))]
[(▽<sub>__x̄__</sub>||__x̄__|| &ndash; 1)<sup>2</sup>]

__(Generator)__ _min_ __E__<sub>___z___~_p<sub>z</sub>_</sub>
[_D_(_G_(___z___))]

## Wasserstein GANs with gradient penalty (WGAN-GP)

In order to enforce Lipschitz constraints on the discriminator, which is
required in the training of WGAN, Gulrajani _et al._ proposed to add to the
objective function of $D$ a gradient penalty (GP) term that punish the
discriminator when and where it violates the Lipschitz constraints [3].

However, it is computationally impossible to impose gradient penalty anywhere in
the data space. With the observation that the optimal discriminator to a fixed
generator lie in middle of the data distribution (_p<sub>d</sub>_) and the model
distribution (_p<sub>g</sub>_), they proposed to enforce gradient penalties
along straight lines between pairs of points sampled from _p<sub>d</sub>_ and
_p<sub>g</sub>_. Now the objective function for the discriminator becomes

__(Discriminator)__ _max_ __E__<sub>__x__~_p<sub>d</sub>_</sub> [_D_(__x__)]
&ndash; __E__<sub>___z___~_p<sub>z</sub>_</sub> [_D_(_G_(___z___))] +
__E__<sub>__x̄__~_p_<sub>__x̄__</sub></sub>
[(▽<sub>__x̄__</sub>||__x̄__|| &ndash; 1)<sup>2</sup>] ,

where _p_<sub>__x̄__</sub> = _ε p<sub>d</sub>_ + (1 - _ε_) _p<sub>g</sub>_,
_ε_~_U_[0, 1].

## References

[1] Ian J. Goodfellow, Jean Pouget-Abadie, Mehdi Mirza, Bing Xu, David
    Warde-Farley, Sherjil Ozair, Aaron Courville, and Yoshua Bengio,
    "Generative Adversarial Networks",
    in _Proc. NIPS_, 2014.

[2] Martin Arjovsky, Soumith Chintala, and Léon Bottou,
    "Wasserstein Generative Adversarial Networks",
    in _Proc. ICML_, 2017.

[3] Ishaan Gulrajani, Faruk Ahmed, Martin Arjovsky, Vincent Dumoulin, and
    Aaron Courville,
    "Improved Training of Wasserstein GANs",
    in _Proc. NIPS_, 2017.

[4] Mehdi Mirza and Simon Osindero,
    "Conditional Generative Adversarial Nets",
    _arXiv preprint, arXiv:1411.1784_, 2014.