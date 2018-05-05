### Parallelism and Communication

A parallel system consists of many transition systems connected by a communicative and associative operator $||$.

$TS=TS_1\;||\;TS_2\;||\;...\;||\;TS_n$

#####1. Concurrency and Interleaving 

Interleaving refers to the model that the effect of $\alpha$ and $\beta$ executing concurrently is equivalent to either execute in the order of $\alpha;\beta$ or in the order of $\beta;\alpha$

$Effect(\alpha\;||\;\beta, \eta)=Effect((\alpha;\beta)+(\beta;\alpha),\eta)$, $+$ stands for nondeterminism

