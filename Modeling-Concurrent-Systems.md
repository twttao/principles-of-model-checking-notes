### Modeling Concurrent Systems



#### 1. Transition Systems

* What is?

  A transition system is a seven tuple $(S, Act, \rightarrow, I, AP, L)$.

  * $S$ is the set of states
  * $Act​$ is the set of actions
  * $\rightarrow \subseteq S \times Act \times S$ is a transition relation (ex. $s\xrightarrow{\alpha} s' $ or $(s, \alpha, s') \in \rightarrow$)
  * $I \subseteq S$ is the set of initial states
  * $AP​$ is the set of atomic propositions; assertions about states
  * $L: S \rightarrow 2^{AP}$ is a "labeling function"; given a state $s$, $L(s)$ is a subset of $AP$ in which all propositions are satisfied

  ​

* Is it finite?

  If $S, Act, AP$ are finite



* Non-determinism
  * If a state has more than one outgoing transition, one is chosen non-deterministically
  * If there are more than one initial state, one is chosen non-deterministically



Insights 1

`Non-determinism is speaking from an internal perspective with regard to the system `

In *Example 2.2,*  the outgoing states from the state ***select*** is non-deterministic only because there is no way for the system *itself* to determine what drink will be selected. However, it does not mean the event is random at a fundamental level. For the user of the beverage vending machine, the process of selecting a drink is quite deterministic. This is referred to as "underspecification" in the book.



* Direct predecessors and successors
  * Direct successors for some action ($\alpha$-successor): $Post(s, a)=\{s' \in S\;|\;s \xrightarrow{a} s'\}$
  * All direct successors of a state: $Post(s)=\cup_{\alpha \in Act} Post(s, \alpha)$
  * Additionally, $Post(C, \alpha)=\cup_{s\in C} Post(s,\alpha)$, $Post(C)=\cup_{s\in C} Post(s)$
  * Similarly, $Pre(C,\alpha)=\cup_{s\in C} Pre(s, \alpha)$, $Pre(C)=\cup_{s\in C}Pre(s)$



* Terminal state

  $s$ is terminal if $Post(s)=\emptyset$



* Two kinds of determinism
  * action-deterministic: $|I| \leq 1 \wedge (\forall s\in S, \alpha \in Act, |Post(s,a) \leq 1|)$ (ie. at most one initial state; for any state and any action, there is at most one successor)
  * AP-deterministic: $|I|\leq 1 \wedge (\forall s\in S, A\in 2^{AP}|Post(s) \cap \{s' \in S\;|\;L(s')=A\}| \leq 1)$ (ie. at most one initial state; for any state and any property, there is at most one successor that satisfies that property)

