[Efficient Lifelong Learning with A-GEM]([url//thepaper.io](https://arxiv.org/abs/1812.00420))
=====

Chaudhry, Arslan and Ranzato, Marc’Aurelio and Rohrbach, Marcus and Elhoseiny, Mohamed, 2019, ICLR
------------

Main Idea
---------

Intoduction of a learning protocol that allows multiple passes only on a small subset of the data to choose the hyperparameters. Later, the paper introduces A-GEM, an improvement over GEM both in performance and computational efficiency

Details
------

..*The Learning Protocol:
Find best hyper-parameters $h^*$ of model using $T_{CV}$ tasks (multi-pass allowed). Then, with $h^*$ train the model using $T - T_{CV}$ tasks (NO multi-pass). In the paper $T_{CV} = 3$ and $T - T_{CV} = 17$.

Average Accuracy (Accuracy on all tasks learned up to now), Forgetting Measure (max difference of accuracy between current task and previously learned task). The paper introduces the Learning Curve Area (LCA) parametrized by a $\beta$ parameter. This $\beta \in \mathbb{N}$ controls how many mini-batches the model sees per task. We define $a_{i,j}$ as accuracy on batch $i$ for task $j$

![ ](https://latex.codecogs.com/gif.latex?LCA_%5Cbeta%20%3D%20%5Cfrac%7B1%7D%7B1%20&plus;%20%5Cbeta%7D%20%5Csum_%7Bb%3D0%7D%5E%7B%5Cbeta%7D%20%5Cfrac%7B1%7D%7BT%7D%20%5Csum_%7Bk%3D1%7D%5ET%20a_%7Bb%2Ck%7D)

..*The A-GEM Model:
GEM and Scalability issues: Consider some model being trained on the $t^{th}$task. We denote previously seen tasks $k < t$. The update is not a simple gradient vector $g_t$. If the gradient vector $g_t$ has an angle with any of the previous gradient vectors $g_k$ that is greater than $90$ degrees, we use a projection ![ ](https://latex.codecogs.com/gif.latex?%5CTilde%7Bg%7D). This allows to avoid significant increases of the loss on any previous task $k$.

When computing the projection ![ ](https://latex.codecogs.com/gif.latex?%5CTilde%7Bg%7D) we are looking at a matrix $G = -(g_1,..., g_{t-1}) \in \mathbb{R}^{(t-1) \times P}$ ($P$ is the parameter space size). This matrix records all previously seen gradients. The we are trying to find the projection ![ ](https://latex.codecogs.com/gif.latex?%5CTilde%7Bg%7D%20%3D%20G%5ET%20v%5E*.%20&plus;%20g) where $v^*$ is the solution to:

![ ](https://latex.codecogs.com/gif.latex?%5Cbegin%7Balign%7D%20%26%5Ctext%7Bminimize%7D_%7B%5CTilde%7Bg%7D%7D%20%5Cfrac%7B1%7D%7B2%7D%20%7C%7Cg%20-%20%5CTilde%7Bg%7D%7C%7C_2%5E2%20%5Cquad%20%5Ctext%7B%20s.t.%20%7D%20%5CTilde%7Bg%7D%5ETg_k%20%5Cgeq%200%20%5Cquad%20%5Cforall%20k%20%5Cle%20t%20%5C%5C%20%26%5Ctext%7Bminimize%7D_v%20%5Cfrac%7B1%7D%7B2%7D%20v%5ETGG%5ETv%20&plus;%20g%5ETG%5ETv%20%5Cquad%20%5Ctext%7B%20s.t.%20%7D%20v%20%5Cgeq%200%20%5Cend%7Balign%7D)

A-GEM Solution:
The A-GEM approch relaxes the requirement that no previous task should see its loss increase. Instead, we define the projection ![ ](https://latex.codecogs.com/gif.latex?%5CTilde%7Bg%7D) to prevent the increase of the average loss over the previous tasks. To do so, we introduce $g_{ref}$ in the Quadratic program. This $g_ref$ is an approximate gradient based on a sample taken from all episodic memories of previous tasks. The minimization task becomes:

![ ](https://latex.codecogs.com/gif.latex?%5Ctext%7Bminimize%7D_%7B%5CTilde%7Bg%7D%7D%20%5Cfrac%7B1%7D%7B2%7D%20%7C%7Cg%20-%20%5CTilde%7Bg%7D%7C%7C_2%5E2%20%5Cquad%20%5Ctext%7B%20s.t.%20%7D%20%5CTilde%7Bg%7D%5ETg_%7Bref%7D%5Cgeq%200%20%5Cquad%20%5Cforall%20k%20%5Cle%20t)

The value of the projected gradient is thus ![ ](https://latex.codecogs.com/gif.latex?%5CTilde%7Bg%7D%20%3D%20g%20-%20%5Cfrac%7Bg%5ET%20g_%7Bref%7D%7D%7Bg_%7Bref%7D%5ETg_%7Bref%7D%7Dg_%7Bref%7D).

Final Thoughts
-------------

Interesting paper with two different contribution. The trick used to build A-GEM from GEM is simple aned drastically improves scalability without hindering the model's performance.
