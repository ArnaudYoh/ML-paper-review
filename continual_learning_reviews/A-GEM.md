<style TYPE="text/css">
code.has-jax {font: inherit; font-size: 100%; background: inherit; border: inherit;}
</style>
<script type="text/x-mathjax-config">
MathJax.Hub.Config({
    tex2jax: {
        inlineMath: [['$','$'], ['\\(','\\)']],
        skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'] // removed 'code' entry
    }
});
MathJax.Hub.Queue(function() {
    var all = MathJax.Hub.getAllJax(), i;
    for(i = 0; i < all.length; i += 1) {
        all[i].SourceElement().parentNode.className += ' has-jax';
    }
});
</script>
<script type="text/javascript" src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.4/MathJax.js?config=TeX-AMS_HTML-full"></script>
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
$$
\begin{equation*}
    LCA_\beta = \frac{1}{1 + \beta} \sum_{b=0}^{\beta} \frac{1}{T} \sum_{k=1}^T a_{b,k}
\end{equation*}
$$

..*The A-GEM Model:
GEM and Scalability issues: Consider some model being trained on the $t^{th}$task. We denote previously seen tasks $k < t$. The update is not a simple gradient vector $g_t$. If the gradient vector $g_t$ has an angle with any of the previous gradient vectors $g_k$ that is greater than $90$ degrees, we use a projection $\Tilde{g}$. This allows to avoid significant increases of the loss on any previous task $k$.

When computing the projection $\Tilde{g}$ we are looking at a matrix $G = -(g_1,..., g_{t-1}) \in \mathbb{R}^{(t-1) \times P}$ ($P$ is the parameter space size). This matrix records all previously seen gradients. The we are trying to find the projection $\Tilde{g} = G^T v^*. + g$ where $v^*$ is the solution to:
$$
        \begin{align}
            &\text{minimize}_{\Tilde{g}} \frac{1}{2} ||g - \Tilde{g}||_2^2 \quad \text{ s.t. } \Tilde{g}^Tg_k \geq 0 \quad \forall k \le t \\
            &\text{minimize}_v \frac{1}{2} v^TGG^Tv + g^TG^Tv \quad \text{ s.t. } v \geq 0
        \end{align}
$$

A-GEM Solution:
The A-GEM approch relaxes the requirement that no previous task should see its loss increase. Instead, we define the projection $\Tilde{g}$ to prevent the increase of the average loss over the previous tasks. To do so, we introduce $g_{ref}$ in the Quadratic program. This $g_ref$ is an approximate gradient based on a sample taken from all episodic memories of previous tasks. The minimization task becomes:
$$
        \begin{equation}
            \text{minimize}_{\Tilde{g}} \frac{1}{2} ||g - \Tilde{g}||_2^2 \quad \text{ s.t. } \Tilde{g}^Tg_{ref} \geq 0 \quad \forall k \le t
        \end{equation}
$$
The value of the projected gradient is thus $\Tilde{g} = g - \frac{g^T g_{ref}}{g_{ref}^Tg_{ref}}g_{ref}$.

Final Thoughts
-------------

Interesting paper with two different contribution. The trick used to build A-GEM from GEM is simple aned drastically improves scalability without hindering the model's performance.
