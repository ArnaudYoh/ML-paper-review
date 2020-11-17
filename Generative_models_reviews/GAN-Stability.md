[On Convergence and Stability of GANs](https://arxiv.org/pdf/1705.07215.pdf)
=====

Naveen Kodali, Jacob Abernethy, James Hays & Zsolt Kira, 2017, (opt.) conference/journal
------------

Main Idea
---------

The authors take a regret minimization approach to explain the dynamics of GAN training. Throught this approach, the authors explore the causes of mode collapse. In a second part, the authors present DRAGAN, a model that uses regularization to avoid mode collapse. 

Details
------

The authors start by defining the regret based framework of the GAN game using the regret as:
![ ](https://latex.codecogs.com/gif.latex?R%28T%29%20%3A%3D%20%5Csum_%7Bt%3D1%7D%5ET%20L_t%28k_t%29%20-%20%5Cmin_%7Bk%20%5Cin%20K%7D%20%5Csum_%7Bt%3D1%7D%5ET%20L_t%28k%29)

Here the authors are esssentially comparing the accumulation of the loss we obtained over the time steps by choosing $k_t$ with the the loss obtain with the optimal $k$. 

Now we define a regret using the usual GAN loss for both the generator and the discriminator. In a concave-convex case, there is a unique solution that minimized both regrets, and it is attainable through online stochastic gradient. 

In practice, the solution is not unique and the model could be stuck in a local equilibrium that leads to poor results. To counteract this issue the authors propose a new penalisation: 
![ ](https://latex.codecogs.com/gif.latex?%5Clambda%20%5Ccdot%20%5Cmathbb%7BE%7D_%7Bx%20%5Csim%20P_%7Breal%7D%2C%20%5Cdelta%20%5Csim%20N%280%2C%20cI%29%7D%5B%7C%7C%5Cnabla_%7B%5Cmathrm%7Bx%7D%7DD_%5Ctheta%28x%20&plus;%20%5Cdelta%29%29%20-k%20%7C%7C%5D%5E2)

This gives us the DRAGAN training procedure for GANs. This penalty allows the gradient to be smaller around real points, thus making the model less prone to mode collapse. 

The authors compare their approach to WGAN-LP and LS-GAN, two other training procedure that regularize the gradient. They claim that their approach gives more freedom to the user since it does not rely on a coupling of real and generated data like WGAN and LS-GAN. 

Final Thoughts
-------------

Quite interesting read. Though, the critics for WGAN seem a bit weird since there isn't any experimental evidence that DRAGAN beats WGAN other than the very low manyfold example shown in the paper. 