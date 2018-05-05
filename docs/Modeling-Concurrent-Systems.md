### Modeling Concurrent Systems

##### 1. Transition Systems

* Definition

  A transition system is a seven tuple $(S, Act, \rightarrow, I, AP, L)$

  * $S$ is the set of states
  * $Act​$ is the set of actions
  * $\rightarrow \subseteq S \times Act \times S$ is a transition relation (ex. $s\xrightarrow{\alpha} s' $ or $(s, \alpha, s') \in \rightarrow$)
  * $I \subseteq S$ is the set of initial states
  * $AP​$ is the set of atomic propositions; assertions about states
  * $L: S \rightarrow 2^{AP}$ is a "labeling function"; given a state $s$, $L(s)$ is a subset of $AP$ in which all propositions are satisfied




* Is it finite?

  If $S, Act, AP$ are finite, ...





* Non-determinism
  * If a state has more than one outgoing transition, one is chosen non-deterministically
  * If there are more than one initial state, one is chosen non-deterministically




> Insights 1
>
> Non-determinism is speaking from an internal perspective with regard to the system 
>
> In *Example 2.2,*  the outgoing states from the state ***select*** is non-deterministic only because there is no way for the system *itself* to determine what drink will be selected. However, it does not mean the event is random at a fundamental level. For the user of the beverage vending machine, the process of selecting a drink is quite deterministic. This is referred to as "underspecification" in the book.



* Direct predecessors and successors
  * Direct successors for some action ($\alpha$-successor): $Post(s, a)=\{s' \in S\;|\;s \xrightarrow{a} s'\}$
  * All direct successors of a state: $Post(s)=\cup_{\alpha \in Act} Post(s, \alpha)$
  * Additionally,
     * $Post(C,\alpha)=\cup_{s\in C} Post(s,\alpha)$
     * $Post(C)=\cup_{s\in C} Post(s)$
  * Similarly,
     * $Pre(C,\alpha)=\cup_{s\in C} Pre(s, \alpha)$
     * $Pre(C)=\cup_{s\in C}Pre(s)$



* Terminal state

  $s$ is terminal if $Post(s)=\emptyset$

  ​



* Two kinds of determinism
  * action-deterministic: $|I| \leq 1 \wedge (\forall s\in S, \alpha \in Act, |Post(s,a) \leq 1|)$ (ie. at most one initial state; for any state and any action, there is at most one successor)
  * AP-deterministic: $|I|\leq 1 \wedge (\forall s\in S, A\in 2^{AP}|Post(s) \cap \{s' \in S\;|\;L(s')=A\}| \leq 1)$ (ie. at most one initial state; for any state and any property, there is at most one successor that satisfies that property)




* Executions (alternating sequence of states and actions)

  * Finite: $\varrho=s_0\alpha_1s_1\alpha_2...\alpha_ns_n$, for all $0 \leq i < n$ 
    * OR $\varrho=s_o \xrightarrow{\alpha_1}...\xrightarrow{a_n} s_n$

  * Infinite: $\rho = s_0\alpha_1s_1\alpha_2s_2\alpha_3...$,
    * $\rho = s_o\xrightarrow{\alpha_1}s_1\xrightarrow{\alpha_2}...$



* Reachable states (states that can be reached via some execution sequence)

  If $s\in Reach(TS)$, then $ s_0\xrightarrow{\alpha_1} s_1 \xrightarrow{\alpha_2}...\xrightarrow{a_n}s_n = s$.





##### 2. Modeling Sequential Hardware Circuits

* The book gives an example of "sequential hardware circuit", where $\lambda_y=\neg(x\oplus r)$ (output $y$ is the negation of the XOR of input $x$ and register $r$), and $\delta_r=x \vee r$ (register $r$ is determined by a disjunctive relationship between $x$ and itself).
* Generally, a sequential hardware circuit with $n$ <u>input</u> bits, $m$ <u>output</u> bits, and $k$ <u>registers</u> can be represented by $TS=(S, Act, \rightarrow, I, AP, L)$
  * $S=Eval(x_1,...,x_n, r_1,...,r_k)$ ($Eval$ refers to the set of all possible pairs of $x$ and $r$; its size is $2^{n+k}$)
  * $I$ refers to "the $k$ registers ... evaluated with their initial value"
  * $Act=\{\tau\}$ (action is irrelevant in our consideration)
  * $AP=\{x_1,...,x_n,y_1,...y_m,r_1,...,r_k\}$ (valid values of $x$, $y$, and $r$, a subset of all possible cases)




##### 3. Modeling Data-dependent Systems

* Basics
  * $Var$ is a set of typed variables
  * $dom(x)$ refers to the type of $x$
  * $Eval(Var)​$ is the set of evaluations that assigns to variables
  * $Cond(Var)​$ is the set of boolean conditions over $Var​$ (propositional logic formula)
  * $Effect: Act \times Eval(Var) \to Eval(Var)$: the first $Eval(Var)$ is current variable values; the second $Eval(Var)$ are future values given some specific $Act$




* Program Graph (PG)

    * $Loc$ is the set of locations: nodes of the graph; indicates all reachable states of variables
    * $Act$ is the set of actions
    * $Effect: Act \times Eval(Var) \to Eval(Var)$
    * $\hookrightarrow \subseteq Loc \times Cond(Var) \times Act \times Loc$: the conditional transition relation
  *  "given some location, when some condition satisfies $\eta \vDash g$, go to another location under some action, that produces new variable evaluations $Effect(\alpha, \cdot)$"
  * $Cond(Var) \times Act$ is $g: \alpha$;  $Loc \times Loc$ is $l \hookrightarrow l'$
    * $Loc_0 \subseteq Loc$: initial locations
    * $g_0\in Cond(Var)$ initial condition



> Beverage vending machine example
>
> $Var = \{nsoda, nbeer\}$ (variables keeping track of the number of beverages)
>
> $Act=\{bget, sget,coin,ret\_coin,refill\}$ (transition events)
>
> $Effect(coin, \eta) = \eta$ (state mutation formula)
>
> $Effect(ref\_coin, \eta)= \eta$
>
> $Effect(sget, \eta)=\eta[nsoda:=nsoda-1]$
>
> $Effect(bget, \eta)=\eta[nbeer:=nbeer-1]$
>
> $Effect(refill, \eta)=[nsoda:=max,nbeer:=max]$



* Transition System of Program Graphs
  * $PG=(Loc, Act, Effect,\hookrightarrow, Loc_0,g_0)$
  * $TS(PG)=(S,Act,\to,I,AP,L)$
    * $S=Loc\times Eval(Var)$ OR $\langle\;l,\eta \;\rangle$ are the locations
    * $\to \subseteq S\times Act \times S$ is $\dfrac{l\hookrightarrow^{g:\alpha} l'\;\wedge\; \eta \vDash g}{\langle\;l, \eta\;\rangle \xrightarrow{\alpha} \langle\;l', Effect(\alpha, \eta)\;\rangle}$ are the transition rules
    * $I=\{\langle\;l,\eta\;\rangle\;|\;l\in Loc_0, \eta \vDash g_0\}$ are initial states
    * $AP=Loc\cup Cond(Var)$ are propositions about locations and variable values
    * $L(\langle\;l,\eta\;\rangle)=\{l\}\cup \{g\in Cond(Var)\;|\;\eta\vDash g\}$ means given some location, it is both true that it is at $l$ and that some formulae $g$ is true about the variables

---
