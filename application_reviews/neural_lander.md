[Nerual Lander: Stable Drone Landing Control using Learned Dynamics](https://arxiv.org/abs/1811.08027)
=====

Guanya Shi et al., 2019, ICRA 
------------

Main Idea
---------

Neural-Lander is a DNN based controller that improves control performance during landing, take-off and maneuvres.

Details
-------

Neural lander uses a DNN to evaluate the environment perturbation that influence the behaviour of the drone. The networks takes the drone state and motor speeds as inputs and outputs the predicted pertubation forces $f_a$. 


The evaluated pertubation force is then used to compute the right control inputs using fixed-point iteration. The iteration is defined as:
![ ](https://latex.codecogs.com/gif.latex?u_k%20%3D%20B%5E%7B-1%7D_0%5Ceta_d%28u_%7Bk-1%7D%29%20%3D%20%5Cbegin%7Bbmatrix%7D%20%28f_d%20-%20%5Chat%7Bf%7D_a%28%5Czeta%2C%20u_%7Bk-1%7D%29%29%20%5Ccdot%20%5Chat%7Bk%7D%20%5C%5C%20%5Ctau_d%20%5Cend%7Bbmatrix%7D)
where $f_d$ is the desired force, ![ ](https://latex.codecogs.com/gif.latex?%5Chat%7Bk%7D) is the desired force direction. Finally, $\tau$ is the desired torque. Note that the authors mainly ignore the torque control perturbation, based on the idea that the aerodynamic disturbance is bounded and, as such, not critical. 


Because the DNN is using ReLU activation and is subject to spectral normalization, the authors prove the stability of their learned controller, both in terms of convergence to a small emough error and in practical usage.   


Final Thoughts
-------------

The paper shows a cool application of DNN to a real-world problem, which is even able to beat existing non-DNN-based solutions. 

