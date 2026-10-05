## <u>Week 1: Random Experiments, Sample Spaces and Events</u>
The study of probability concerns itself with some event whose outcome is uncertain, called a **random experiment**. Instances of this random experiment are called **trials**.  

The set of all possible outcomes of a random experiment is called the **sample space**, denoted $\Omega$. Small sample spaces can be defined by listing all outcomes; larger sample spaces are usually defined by set builder notation on outcomes.
- A sample space is **discrete** if its cardinality is either **finite** or **countably infinite**
	- Otherwise (the cardinality is uncountably infinite), the sample space is **continuous**

An **event** is a **set of outcomes**, meaning any event $A$ is a subset of the sample space $A \subseteq\Omega$. An event is a **simple event** if it contains one outcome, or a compound event if it contains multiple outcomes. Events are often defined by set builder notation with outcomes; usually some restriction on the sample space.
- Events are **mutually exclusive** or **disjoint** if they cannot occur simultaneously, that is $A\cap B=\emptyset$
- Events are **independent** if the occurrence of one event does not alter the probability of the occurrence of the other; more on this when we introduce conditional probability
	- Mutual exclusivity is a specific type of very strong **dependence**!

Since sample spaces and events are sets, it is useful to remember all the specifics of working with sets, such as commutativity, associativity, and distributivity (of both $\cap$ and $\cup$), and De Morgan's laws (which apply the same to $>2$ sets as well):
$$
\begin{align}
(A\cup B)^c &= A^c\cap B^c \\
(A\cap B)^c &= A^c\cup B^c
\end{align}
$$
## <u>Week 1: Probability Functions</u>
The **frequency interpretation** of probability is that it is equal to the proportion of the event $A$ occurring as number of trials goes to infinity. The **Weak Law of Large Numbers** follows, stating that the sample average approaches the theoretical average as number of trials goes to infinity. However, we need a more formal, axiomatic mathematical basis.

Given a random experiment with **discrete** sample space $\Omega$, a **probability function** is defined as $P:\Omega\to[0,1]$ with the following properties:
- $\forall \omega\in\Omega,P(\omega)\geq0$
- $\sum_{\omega\in\Omega}P(\omega)=1$
- $\forall A\subseteq \Omega,P(A)=\sum_{\omega\in A}P(\omega)$
	- Ie. the probability of an event is the **sum of the probabilities of its contained outcomes**

Identifying a probability function requires identifying the sample space and the probabilities of the outcomes. Beyond satisfying the mathematically correct definition of a probability function, we also want our function to model reality. For example, modelling a coin toss as $P(H)=1,P(T)=0$ is mathematically valid, but perhaps not useful.

From the above definition alone we can make some intuitive conclusions; ie. a probability of $1$ implies certainty of an outcome occurring, a probability of $0$ implies impossibility, etc. We can also derive the **three probability axioms**:
- $\forall A\subseteq\Omega,P(A)\geq 0$
	- This follows from the first property of probability functions and from the definition of events
- $P(\Omega)=1$
	- This follows from the second property of probability functions
- $P(\bigcup_{i=1}^{\infty} A_i) = \sum_{i=1}^{\infty} P(A_i)$, if $A_i\cap A_j = \emptyset$ for $i\neq j$
	- Ie. for a set of pairwise **mutually exclusive** / **disjoint** events, the probability of their union is the sum of their probabilities
		- For the simplest two event case, this is $P(A\cup B)=P(A)+P(B)$
	- This follows from the third property of probability functions and from the definition of mutually exclusive events; we are essentially just summing the probabilities of all the contained outcomes (which are never repeatedly counted due to the events being disjoint)

Some additional properties of probability functions that we can deduce from the above properties:
- $P(A^c)=1-P(A)$
- If $A\subseteq B$, then $P(A)\leq P(B)$
- $P(A\cup B) = P(A) + P(B) - P(A\cap B)$, this is the principle of **inclusion-exclusion** and can be **very messily** extended to $n$ events
	- For three events $P(A\cup B\cup C) = P(A) + P(B) + P(C) - P(A\cap B) - P(A\cap C) - P(B\cap C) + P(A\cap B\cap C)$
	- Note, the third property above was a special case of this where all intersections were $\emptyset$, and $P(\emptyset)=0$
	- This follows from the definition of $P(A)$ for $A\subseteq\Omega$ and the definition of mutually exclusive events; when the events are not disjoint we have to ensure we are not repeatedly counting the outcomes that are within intersections of events

The simplest probability models are those where **all outcomes are equally likely**. Then determining the sample space and various probabilities is relatively simple:
- $\forall \omega\in\Omega,P(\omega)=\frac{1}{|\Omega|}$
- $\forall A\subseteq \Omega,P(A)=\frac{|A|}{|\Omega|}$
- Such models are **only possible with finite sample spaces**, since the sum of the probabilities of the outcomes would otherwise be $\infty\neq1$
## <u>Week 2: Counting</u>
Constructing sample spaces is often necessary before we can attempt to determine a probability function. For models where **all outcomes are equally likely**, we use **counting** methods (albeit there are also a few uses later on, in conditional probability and probability distribution topics).

When determining sample spaces, we must first decide whether we are doing it with replacement or without replacement:
- **Sampling without replacement:** consider a set of $n$ objects, choosing $k\ge 0$ objects from said set without replacement (ie. choosing an object removes it from the set) leads to $n(n-1)\cdots(n-k+1)$ possible orderings
	- Note that here we require $k\leq n$, as we would otherwise run out of objects to choose
- **Sampling with replacement:** consider a set of $n$ objects, choosing $k\ge 0$ objects from said set with replacement (ie. choosing an object does not remove it from the set) leads to $n^{k}$ possible orderings

**Fundamental Principle of Counting:** for an $n$ element sequence $(a_{1},a_{2},\dots ,a_{n})$; if there are $k_{i}$ possible values for each $i^{th}$ element where $1\leq i\leq n$, then there are $k_{1}k_{2}\dots k_{n}$ possible sequences
- This simple principle is very useful in determining many sample spaces; ex. for the outcomes of $n$ coin tosses $|\Omega|=2^{n}$, for the outcomes of $n$ die rolls $|\Omega|=6^{n}$, etc.
- This principle is commonly combined with the property $P(A^{c})=1-P(A)$ for questions involved **at least** counts; ex. for $4$ exams graded A-F with equal odds, the probability of getting at least one F is $1$ minus the probability of getting zero Fs, which is $1-\frac{4^{4}}{5^{4}}$
- A specific application of the FPC is **counting the number of subsets** of an $n$-element set; we describe the set as a sequence where each element is binary value representing the presence of the element or lack thereof, then by the FPC the number of sequences (subsets) is $2^{n}$

**Permutation:** a permutation is an **ordered** arrangement of $n$ different objects
- $Permutations(n,k)=\frac{n!}{(n-k)!}$ for $n$ objects and $k$ choices
- We define $0!=1$, thus for $k=n$ choices there are $n!$ permutations
- Since order matters, it is generally clear with permutations that **each object is distinguishable** even if they are categorized together
	- Ex. for $5$ math books and $10$ novels split across $3$ shelves of $5$ books each, the probability of the third shelf holding all math books is $\frac{5!10!}{15!}$ since there are $5!$ ways to permute the math books in the last shelf, and $10!$ ways to permute the novels on the remaining shelves
- Permutations are effectively specific cases of the fundamental principle of counting, but without replacement; note that the **order matters in both cases**, but this is not immediately intuitively clear for FPC

**Combination**: a combination is an **unordered** selection of $n$ different objects
- $Combinations(n,k)=\frac{n\cdot (n-1)\cdot (n-2)\cdots (n-k+1)}{k!}=\frac{n!}{(n-k)!k!}$ for $n$ objects and $k$ choices
	- The only difference from permutations is division by $k!$ which adjusts for the different ordered arrangements of our chosen $k$ objects
	- This is also known as the **binomial coefficient** where $\binom{n}{k}$ is the **number of subsets** of size $k$ for a set of size $n$
		- This interpretation is often useful when combined with our previous definition from FPC of the **total** number of subsets being $2^{n}$
- For $k=0$ and $k=n$ choices there is $1$ combination, and for $k=1$ and $k=n-1$ there are $n$ combinations; this **symmetry** can be generalized as $\binom{n}{k}=\binom{n}{n-k}$
- With combinations, since order does not matter, it is easy to forget that **each object is distinguishable** even if they are categorized together
	- Ex. a classroom has $6$ girls and $4$ boys; the number of ways to select any $5$ children is $\binom{10}{5}$, and the number of ways to select specifically $2$ girls and $3$ boys is $\binom{6}{2}\binom{4}{3}$; each girl and boy is a distinct object in their category!
	- Ex. a clinical trial with $25$ people will have $15$ receive the drug and $10$ receive the placebo; out of $6$ randomly chosen participants, the probability of $4$ having the drug and $2$ placebo is $\frac{\binom{15}{4}\binom{10}{2}}{\binom{25}{6}}$

Equipped the four definitions above, we can consider four different sampling scenarios:
1. **Sampling with replacement** where **order matters**; we use **fundamental principle of counting**; $|\Omega|=n^k$
2. **Sampling without replacement** where **order matters**; we use **permutations**; $|\Omega|=\frac{n!}{(n-k)!}$
3. **Sampling without replacement** where **order does not matter**; then we use **combinations**; $|\Omega|=\frac{n!}{(n-k)!k!}$
4. Sampling with replacement where order does not matter; then $|\Omega|=\frac{(n+k-1)!}{(n-1)!k!}$ (this is a complicated scenario which we won't cover)
Note, these are only the ways we calculate the cardinality of the entire sample space, and in the simplest of cases. More involved examples:
- Ex. the odds of being dealt a single-suit bridge hand ($13$ cards) is $\frac{4}{\binom{52}{13}}$; the odds of all $4$ players being dealt such hands at once is $\frac{4!}{\binom{52}{13}\binom{39}{13}\binom{26}{13}\binom{13}{13}}$ since there are $4!$ ways to permute the suits among the $4$ players
	- This uses permutations to determine the event size (numerator), and FPC and combinations for the sample size (denominator)!
- Ex. in $20$ coin tosses, the probability of getting exactly $10$ heads is $\frac{\binom{20}{10}}{2^{20}}$
	- This relies on the definition of combinations regarding subsets of size $k$ from a set of size $n$, where the elements of the subset are the positions in the sequence of tosses where we get heads
## <u>Week 3: Conditional Probability</u>
**Conditional Probability:** for events $A,B$ with $P(B)>0$, the conditional probability of $A$ given $B$ is:
$$P(A|B) = \frac{P(A\cap B)}{P(B)}$$
Here, $A$ is the event whose probability we want to update, and $B$ is evidence that we observe or treat as a given. $P(A)$ is the **prior** and $P(A|B)$ is the posterior probability. 

Intuitively, we can think of this as $P(A)$ being normalized to the restriction of the sample space to $B\subseteq\Omega$, since our condition necessarily implies $P(B) = 1$. From a mathematical perspective, conditional probability **is a probability function**:
- $\forall b\in B,P(b|B)\geq0$
- $\sum_{b\in B}P(b|B)=1$
- $\forall A\subseteq B,P(A|B)=\sum_{a\in A}P(a|B)$ 

From this definition, we can deduce these properties:
- $P(A|A)=1$
- If $A\subseteq B$, then $P(A|B)=\frac{P(A)}{P(B)}$, since $A\cap B=A$ and thus $P(A\cap B)=P(A)$
- $P(A\cap B)=P(A|B)\cdot P(B)$
	- This follows from simply rearranging terms, and is useful as $P(A\cap B)$ is often the desired unknown
		- In these cases we are either given $P(A|B)$ or can determine it by counting, we **cannot use the formula** at the beginning of the chapter as $P(A\cap B)$ is our unknown!
	- Recall that $\cap$ is commutative, so it follows that $P(A\cap B)=P(B\cap A)=P(B|A)\cdot P(A)$
		- Do not get confused; indeed in general $P(A|B)\neq P(B|A)$
	- For three events, $P(A\cap B\cap C)=P(C|A\cap B)\cdot P(B|A)\cdot P(A)$; this is called the **Multiplicative Law of Probability** and can be **very messily** extended to $n$ events

From a frequentist perspective with $n$ experiments, $P(A|B)=\frac{n_{AB}}{n_B}$.

Techniques like **complementing**, etc. still apply for problem solving; some common examples:
- For two die rolls, the probability of the first die roll being a $2$ given that the sum is $6$ is $\frac{P(\{(2,4)\})}{P(\{(1,5),(2,4),(3,3),(4,2),(5,1)\})}=\frac{\frac{1}{36}}{\frac{5}{36}}=\frac{1}{5}$
	- Note, the roll $(3,3)$ is not counted twice; recall that each die is a **distinguishable object**!
- For three coin tosses, the probability of all heads given the first is heads is $\frac{P(HHH\cap H_{1})}{P(H_{1})}=\frac{P(HHH)}{P(H_{1})}=\frac{\frac{1}{8}}{\frac{1}{2}}=\frac{1}{4}$
	- Note, by the second property above $P(HHH\cap H_{1})=P(HHH)$ since $HHH\subseteq H_{1}$
- For three card draws, the probability of three aces is $P(A_{1}\cap A_{2}\cap A_{3})=P(A_{1})\cdot P(A_{2}|A_{1})\cdot P(A_{3}|A_{1}\cap A_{2})=\frac{4}{52}\cdot \frac{3}{51} \cdot \frac{2}{50}$
- The probability of being dealt a blackjack is $P((A_{1}\cap T_{2})\cup (T_{1}\cap A_{2}))=P(A_{1}\cap T_{2})+P(T_{1}\cap A_{2})=P(A_{1})P(T_{2}|A_{1})+P(T_{1})P(A_{2}|T_{1})=\frac{4}{52}\frac{16}{51}+\frac{16}{52}\frac{4}{51}$
	- Note, we used the mutual exclusivity of the two intersection events
- The probability of at least one pair of $k$ people having the same birthday is $P(B)=1-P(B^{c})=1-(\frac{364}{365})(\frac{363}{365})\cdots (\frac{365-(k-1)}{365})$


In some cases, we are given conditional probabilities and asked to solve for unconditional probabilities. The approach to solve such problems is called **conditioning**.
- Suppose $B_{1},\dots,B_{k}$ is a partition of $\Omega$; recall, a partition is a set of subsets which are pairwise mutually exclusive and cover the entire set
	- Then, for an event $A\subseteq\Omega$, we can **decompose** the set $A$ as $(A\cap B_{1})\cup\dots \cup(A\cap B_{k})$
		- Thus, the probability can be expressed as $P(A)=\sum_{i=1}^{k}P(A\cap B_{i})$; now note that we can turn each term into its conditional equivalent, which gives us the following law

**Law of Total Probability:** suppose $B_{1},\dots,B_{k}$ is a partition of $\Omega$, then $\forall A\subseteq\Omega:$
$$P(A)=\sum_{i=1}^{k}{P(A\cap B_{i})}=\sum_{i=1}^{k}{P(A|B_{i})\cdot P(B_{i})}$$
For the simple partition of $B,B^{c}$, we have $P(A)=P(A|B)\cdot P(B)+P(A|B^{c})\cdot P(B^{c})$.


Recall that generally $P(A|B)\neq P(B|A)$, however using the properties of conditional probability and the Law of Total Probability, we can determine how to calculate one from the other.

**Bayes Formula:** for event $A$ and a partition of the sample space $B_{1},\dots,B_{k}$, for each $1\leq j\leq k$ we have:
$$P(B_{j}|A)=\frac{P(A\cap B_{j})}{P(A)}=\frac{P(A|B_{j})\cdot P(B_{j})}{\sum_{i=1}^{k}{P(A|B_{i})\cdot P(B_{i})}}$$
For the simple partition of $B,B^{c}$, we have: 
$$P(B|A)=\frac{P(A\cap B)}{P(A)}=\frac{P(A|B)\cdot P(B)}{P(A|B)\cdot P(B)+P(A|B^{c})\cdot P(B^{c})}$$
Note how the properties of conditional probability determine the numerator, and the Law of Total Probability the denominator.


Recall in the very first section we gave a vague definition of independence. Now armed with concepts of conditional probability, we can define independence formally.

**Independence:** two events $A,B$ are independent if:
$$
\begin{align}
P(A|B)&=P(A) \\
P(B|A)&=P(B)
\end{align}
$$
Otherwise, the events are **dependent**. It follows that this also implies that for independent events, $P(A\cap B)=P(A)\cdot P(B)$, known as the **Multiplication Rule for Independent Events**. For independence between more than two events, every finite subgroup of events must be independent (satisfy the above rule) as well. Intuitively, independence of events also implies independence with **complements** as well.

Independence is clearly associated with sampling with replacement, while dependence with sampling without replacement. However, with a large enough population, the numerical impact can become negligible of sampling without replacement can become negligible!

Finally, for **mutually exclusive** (and therefore dependent) events, we have:
$$
P(A|B)=P(B|A)=0
$$
## <u>Week 4: Discrete Random Variables</u>
**Random Variable:** for a sample space $\Omega$ of an experiment, a random variable (RV) is a rule that associates a real number to each $\omega\in\Omega$. In mathematical terms, an RV $X$ is a function $X:\Omega\to \mathbb{R}$. 
- Since RVs are **functions** they map each **outcome** to **exactly one** number; thus each RV value represents an event (one or more outcomes) that is by definition mutually exclusive / disjoint to all events represented by all other possible value of the RV
	- This is important to understand (later for CDFs among other concepts) as it is a powerful property when combined with the fact that we are in discrete sample spaces; ie. when working with unions we **don't have to worry about inclusion-exclusion principle** etc.

RVs are generally represented with late capital letters, whereas small letters are used to denote a specific value of the RV, ie: $X(\omega)=x$ means that the RV $X$ maps the value $x$ to the outcome $\omega$. Often word descriptions of RVs are more practical (and more possible) than listing out each outcome and value.

**Discrete Random Variable:** an RV whose possible **values** constitute either a **finite** or **countably infinite** set. This also implies that they can have finite and countably infinite domains; it follows that discrete RVs usually map data from **counting**, not measuring.


When probabilities are assigned to outcomes in $\Omega$ by a probability function, those probabilities are then by definition also assigned to values of any RV that maps them. For an RV $X$, the **probability distribution** of $X$ dictates how the total probability of $1$ is distributed among all possible values. Specifically for discrete RVs, the distribution is represented by a **probability mass function** (PMF) $p_{X}:S\to[0,1]$ where $S$ is the range of $X$:
$$
p_{X}(x)=P(X=x)
$$
This is often shortened to just $p(x)$. Some remarks about PMFs, which are simply extensions of probability function axioms from previous sections applied to RVs:
- The PMF only maps nonzero probabilities to a finite or countably infinite amount of values (that set of $x$ is known as the **support**), otherwise the total probability would be $\infty\neq 1$; $p(x)=0$ is therefore assumed by convention for all $x$ **not in the support**
- $\forall x \in S,0\leq p(x)\leq 1$
- $\sum_{x} p(x) =1$, where we consider only the $x$ in the support

**Cumulative Distribution Function:** for a discrete RV $X$ with PMF $p(x)$, the CDF $F_{X}(x)$ is the probability that an observed value of $X$ will be **at most** $x$. In mathematical terms, $F:S\to[0,1]$ such that:
$$F(x)=P(X\leq x)=\sum_{x'\leq x}p(x')$$
We get values of the CDF by simply **summing the probabilities** of each RV value less than or equal to the desired $x$; this follows from the properties of discrete sample spaces, probability functions and discrete RVs representing mutually exclusive events!

Key properties of CDFs for discrete RVs:
- $F(x)$ is always a non-decreasing **step function**
	- Each **step** reflects an $x$ where the PMF takes a nonzero value, while every **flat** interval reflects $x$ that are not in the support 
- This comes with all the expected properties of step functions, namely:
	- $F(x)$ is right-continuous, that is $\lim_{ x \to c^{+}} F(x)=F(c)$
	- $\lim_{ x \to \infty} F(x)=1$
	- $\lim_{ x \to -\infty} F(x)=0$

Our definition of CDF assumed a PMF was available, but we can also derive information about the underlying PMF with only a CDF available, for example if we assume a discrete RV such that $X \subseteq \mathbb{Z}$:
- $P(X=a)=F(a)-F(a-1)$
- $P(a\leq X\leq b)=F(b)-F(a-1)$
- $P(a< X\leq b)=F(b)-F(a)$
If we wanted more generalization with regards to $X$, we would replace $F(a-1)$ with $\lim_{ x \to a^{-} } F(x)$. Together, the PMF and CDF describe the exact distribution of a random discrete quantity, from where we can calculate various metrics.

**Percentile:** the $\alpha^{th}$ percentile of a distribution is the value $x_{\alpha}$ such that $F(x_{\alpha})=P(X\leq x_{\alpha})=\frac{\alpha}{100}$, where  $0\leq \alpha\leq 100$. So $x_{\alpha}$ is the cutoff under which $\alpha$ percent of the data falls. But recall that for a discrete RV the CDF will be a step function, so there may not exist an $x_{\alpha}$ such that $F(x_{\alpha})=\frac{\alpha}{100}$. In this case the percentile $x_{\alpha}$ is defined as the **smallest** $x$ such that $F(x)\geq \frac{\alpha}{100}$.


**Expectation:** for a discrete RV $X$ with range $S$, the expectation is: $$E(X)=\mu=\sum_{x \in S}x\cdot p(x)=\sum_{x \in S}x\cdot P(X=x)$$This definition assumes a finite $S$. For a countably infinite $S$, the existence of the mean depends on the infinite summation converging. The expectation essentially describes where the probability distribution is **centred / balanced**.

It is important to understand that $E$ is a weighted average summation. Several key algebraic properties of expectation follow:
- $E(c)=c$, for any constant $c \in \mathbb{R}$, intuitively as the weights simply add up to $1\cdot c$
- **Law of the Unconscious Statistician:** for a discrete RV $X$ with range $S$ and a PMF $p$, and for any **non-linear** function on this RV $g(X)$, the expectation $E(g(X))=\sum_{x \in S}g(x)\cdot p(x) \neq g(E(X))$; this is a weighted average of possible $g(x)$ values with weights from the PMF
- **Linearity of Expectation:** the specific cases where $E(g(X))=g(E(X))$, are when $g$ is a **linear transformation** because then the function can simply be distributed into the expectation since it is a summation:
	- $E(X+c)=E(X)+c$, meaning that shifting each value of $X$ by a constant changes the expectation by the same amount
	- $E(cX)=cE(X)$, meaning that multiplying each value of $X$ by a constant changes the expectation by the same amount
	- $E(X+Y)=E(X)+E(Y)$; this is a linear combination, and it applies **even if all the combined RVs have different distributions**
	- $E(a\cdot g(X)+b\cdot h(Y)+c)=aE(g(X))+bE(h(Y))+c$, bringing all of this together
	- $E(XY)=E(X) \cdot E(Y)$ only if $X, Y$ are **independent**, otherwise this requires the joint probability function of the joint distribution of $X,Y$, to be defined in Chapter 10
	
**Markov's Inequality:** for a **non-negative** random variable $X$ and a constant $c>0$, $P(X\geq c)\leq \frac{E(X)}{c}$. Note, this is **not a tight bound**! Furthermore, this is only useful for $c>E(X)$, otherwise it returns probabilities of $\geq 100\%$. 


**Variance:** for a discrete RV $X$ with PMF $p$ and expectation $\mu$, the variance of $X$ is: $$V(X)=\sigma^{2}=E((X-\mu)^{2})=\sum_{x \in S}((x-\mu)^{2}\cdot p(x))$$Note, the units here are squared; the **standard deviation** $SD(X)=\sigma=+\sqrt{V(X)}$ is the measure in the original units.

The quantity $(X-\mu)^{2}$ is the squared deviation from the mean of each RV value, and the variance is the expectation of those squared deviations. Recall expectation is a weighted summation, so standard deviation does not algebraically undo the effects of squaring each term, it only yields a measure in the original units. 

It can be shown (by Linearity of Expectation) that there is a **shortcut formula for variance** where $V(X)=E(X^{2})-E(X)^{2}$. Recall $\mu$ is just alternative notation for expectation.

Some key algebraic properties of variance and standard deviation:
- $V(c)=0$, for any constant $c \in \mathbb{R}$, intuitively since constants do not vary (each squared deviation is $0$ since $E(c)=c$ as shown earlier)
- $V(X+c)=V(X)$, meaning that adding a single constant to each value does not change overall variability (each squared deviation would remain the same, again this follows from our earlier observation that $E(X+c)=E(X)+c$
- $V(cX)=c^{2}V(X)$, with $|c|$ instead for standard deviation
- $V(X+Y)=V(X)+V(Y)+2\text{Cov}(X,Y)$
	- Note, covariance will be defined in Chapter 11
	- $V(X+Y)=V(X)+V(Y)$ only if $X,Y$ are **independent**
- $V(XY)$ is best calculated with the shortcut; ie. defined $Z+XY$ then $V(XY)=V(Z)=E(Z^2)-E(Z)^2=E(X^2Y^2)-E(XY)^2$
	- This would require the joint probability function of the joint distribution of $X,Y$ to be known; these will be defined in Chapter 10

**Chebyshev's Inequality:** for a random variable $X$ and a constant $k>0$, $P(|X-\mu|<k\sigma)\geq 1-\frac{1}{k^{2}}$. This is a **lower bound** on the probability of any given value of $X$ being **within** $k$ standard deviations from the expectation. 

Note, the use of absolute value means that this is the probability of deviations in **both directions**; to get the probability in one direction we have to divide the final result by $2$. Also, by the properties of probability functions, we can deduce that $P(|X-\mu|\geq k\sigma)<\frac{1}{k^{2}}$.

As a final note, **transformed** random variables **do not usually have the same distribution** as the original variable!
## <u>Week 5: Bernoulli, Geometric, Binomial and Poisson Distributions</u>
**Bernoulli Random Variable:** a discrete RV whose only possible values are $x=0,1$ where these values denote failure and success respectively. Note, this RV represents a **single trial**. 
- The RV is indicator of **success or failure**, denoted $X\sim Ber(p)$
- The parameter $p$ is the probability of success in this single trial
- $p(x)=p^{x}\cdot(1-p)^{1-x}$, then as expected intuitively for $X=0,p(0)=1-p$ and for $X=1,p(1)=p$
- $E(X\sim Ber(p))=0\cdot(1-p) + 1\cdot p=p$
- $V(X\sim Ber(p))=(0-p)^{2}\cdot(1-p) + (1-p)^{2}\cdot p=p-p^{2}$

**Geometric Random Variable:** a discrete RV wherein a Bernoulli trial is performed a **countably infinite** number of times. By definition, all of these Bernoulli trials are dichotomous, independent, and have heterogenous probability throughout.
- The RV is the **amount of repeated** Bernoulli trial **failures** **up until the first success**, denoted $X\sim Geo(p)$, with values $x \in \mathbb{N}$
- The parameter $p$ is the probability of success in the Bernoulli trial
- $p(x)=p\cdot(1-p)^{x}$, intuitively $(1-p)^{x}$ is the number of repeated failures before the first success, and $\sum_{x=0}^{\infty}{p(x)}=1$
- $E(X\sim Geo(p))=\frac{1-p}{p}$
- $V(X\sim Geo(p))=\frac{1-p}{p^{2}}$
- Another common way to define the RV is as the amount of repeated Bernoulli trials **up to and including** the first success, then $x \in \mathbb{N^{+}}$ and $p(x)=p\cdot(1-p)^{x-1}$; if we denote this RV as $Y$, then the original RV above is $X=Y-1$
	- Note the expectation and variance formulas might change slightly then, but produce the same values
	
**Binomial Random Variable:** a discrete RV wherein a Bernoulli trial is performed a **finite** number of times $n$. By definition, all of these Bernoulli trials are dichotomous, independent, and have heterogenous probability.
- The RV is the **amount of successes** within the $n$ trials, denoted $X\sim Bin(n,p)$ with values $x \in[0,n]$
- The parameter $n$ is the number of Bernoulli trials, and $p$ is the probability of success
- $b(x;n,p)=\binom{n}{x}\cdot p^{x}\cdot (1-p)^{n-x}$
	- Which reflects $(\text{number of sequences of length \textit{n} with \textit{x} successes})\cdot (\text{probability of any such sequence})$
	- This is the geometric PMF but for multiple successes, and accounting for all the ways the successes can be allocated in the sequence
- $E(X\sim Bin(n,p))=np$, which follows from the binomial RV simply being a sum of $n$ Bernoulli RVs
- $V(X\sim Bin(n,p))=np(1-p)$
- Note, if we are sampling **without replacement** between trials, then the experiment is technically not binomial as the trials are not independent and the probability would not be heterogenous
	- We will use a convention; when sampling from a dichotomous population of size $N$, we can treat the experiment as binomial if $n\leq 0.05N$, in which case the conditional probabilities are effectively unchanged between trials

**Poisson Random Variable:** a discrete RV which counts the number of occurrences in a **continuous** interval, with a constant rate at which events are expected to occur, independent occurrences between non-overlapping intervals, and no more than one occurrence happening simultaneously.
- The RV is the **amount of occurrences** in the interval, denoted $X\sim Poi(\lambda)$, with values $x \in \mathbb{N}$
- The parameter $\lambda$ is the **expected** amount of occurrences in the interval
- $p(x;\lambda)=\frac{\lambda^{x}e^{-\lambda}}{x!}$, and $\sum_{x=0}^{\infty}{p(x;\lambda)}=1$
- $E(X\sim Poi(\lambda))=\lambda$
- $V(X\sim Poi(\lambda))=\lambda$
- Poisson distributions can be used to approximate binomial distributions when $n\to \infty$ and $p=\frac{\lambda}{n}\to 0$:
	- Consider we split up the continuous time interval at hand into $n$ subintervals, then when each subinterval gets small enough the probability of more than one event is negligible, so the event happening in each subinterval becomes a Bernoulli trial with $p=\frac{\lambda}{n}$
	- Then we can model that as binomial distribution where $\lim_{ n \to \infty }b(x;n,p)=\lim_{ n \to \infty }{\binom{n}{x}(\frac{\lambda}{n})^{x}(1-\frac{\lambda}{n})^{n-x}}=\frac{\lambda^{x}e^{-\lambda}}{x!}$
- Poisson distributions tend to be **right-skewed**
## <u>Week 6: Negative Binomial and Hypergeometric Distributions</u>
**Negative Binomial Random Variable:** a discrete RV wherein a Bernoulli trial is performed **until a total of** $r>1$ **successes** have occurred. By definition, all of these Bernoulli trials are dichotomous, independent, and have heterogenous probability throughout.
- The RV is the **amount of failures before** we achieve $r>1$ **successes**, denoted $X\sim NegBin(r,p)$ with values $x \in \mathbb{N}$
	- Note, for $r=1$ this is just the **geometric distribution**
- The parameter $r$ is the number of successes, and $p$ is the probability of success
- $nb(x;r,p)=\binom{x+r-1}{r-1}\cdot p^{r}\cdot (1-p)^{x}$
	- Where $x+(r-1)$ is the **total amount of trials** before the $r^{th}$ success, and we determine the number of ways to arrange $r-1$ successes in those trials, and $p^{r}\cdot (1-p)^{x}$ is probability of one such sequence
	- Note the similarities with the binomial distribution here; the term negative binomial reflects that this distribution is in a sense an inverse of the binomial
- $E(X\sim NegBin(r,p))=\frac{r(1-p)}{p}$
- $V(X\sim NegBin(r,p))=\frac{r(1-p)}{p^{2}}$
- Note, we could also have defined the RV as the total number of trials before $r$ successes, then that RV $Y$ would be $Y=X+r$ where $X$ is our definition above
	- We could also have defined the RV as the total number of trials including the $r^{th}$ success, so $Y=X+1$
	- Note the expectation and variance formulas might change slightly in these cases, but produce the same values

**Hypergeometric Random Variable:** a discrete RV that arises when **sampling without replacement** from a **dichotomous**, finite population, where every sample of size $n$ is **equally likely**.
- The RV is the **amount of a desired type of object** in a draw of $n$ objects from the population $N$, denoted $X\sim HypGeo(D,N,n)$ where:
	- $x \in[0,n]$ if $D\geq n$
	- $x \in[0,D]$ if $D<n$
	- To be well defined we also require $x\leq D$ and $n-x\leq N-D$
- The parameter $D$ is the amount of the desired object in the population, the parameter $N$ is the total population size, and $n$ the draw size
- $h(x;D,N,n)=\frac{\binom{D}{x}\binom{N-D}{n-x}}{\binom{N}{n}}$
	- The PMF here is described purely by counting methods; the denominator $\binom{N}{n}$ is the space of all possible draws of size $n$, and the numerator is all possible draws of $x$ objects out of $D$, and all possible draws of the remaining $n-x$ slots from the undesired $N-D$ 
- $E(X\sim HypGeo(D,N,n))=n\frac{D}{N}$
- $V(X\sim HypGeo(D,N,n))=n(\frac{D}{N})(\frac{N-D}{N})(\frac{N-n}{N-1})$
- Note, taking the **same scenario** that would be appropriate for a hypergeometric distribution, but changing to sampling **with replacement**, would lead us to the binomial distribution!
## <u>Week 7: Continuous Random Variables</u>
The study of discrete random variables (in fact all the preceding chapters assumed discrete sample spaces, everything we discussed so far is dependent on that!) required only the tools of discrete mathematics; sums and differences. The study of continuous random variables requires the tools of calculus; derivatives and integrals. 

In practice the limitations of our measuring instruments restrict us to a discrete (though finely subdivided) world!
- Nevertheless, we use continuous models as they often approximate real-world situations very well, and continuous mathematics (calculus) is frequently easier to work with
- However, unlike discrete distributions, the distribution of any given continuous RV usually **cannot be derived using probabilistic and counting arguments** (with a few notable exceptions)
	- Instead one must make a judicious choice of PDF based on prior knowledge and available data of what distributions tend to fit

**Continuous Random Variable:** an RV where the **sample space is continuous**, and thus the following is true:
- The domain is an interval of $\mathbb{R}$, which is **uncountably infinite** by definition
- The set of possible values is an interval or union of intervals of $\mathbb{R}$, which is uncountably infinite
- Every **exact** value has a probability of $0$, meaning $P(X=x)=0$ for any possible value $x$
	- Intuitively, this is true because $P(X=x)$ is the probability of $X$ being one exact number (with infinite digits post decimal, not rounded to the nearest tenth or even hundredth) out of the uncountably infinite possible values of $X$
	- It follows that for continuous RVs, $P(a\leq X\leq b)=P(a\leq X<b)=P(a<X\leq b)=P(a< X< b)$; **the bounds** themselves can be included or not because the only difference would be $P(X=a)$ and / or $P(X=b)$, both of which are $0$

**Probability Density Function:** continuous RVs have PDFs rather than PMFs (which follows since $\forall x,P(X=x)=0$); which are defined as a function $f(x)$ such that for any possible values of the RV $a,b$ such that $a\leq b$:$$P(a\leq X\leq b)=\int_{a}^{b}{f(x)dx}$$This means the probability that $X$ takes on a value on the interval $[a,b]$ is the area under the graph of the density function on this interval. This graph is the continuous equivalent of a probability histogram for discrete RVs. The following conditions must be satisfied:
- $\forall x,f(x)\geq 0$, which together with the properties of continuous RVs means that $f(x)\neq P(X=x)$!
	- What $f(x)$ actually represents is how dense values of $X$ are around $x$; it is the probability of an infinitesimal interval around $x$ divided by the width of the interval
	- Note, $f(x)$ can take values $>1$ (unlike PMFs) since $f(x)\neq P(X=x)$; this intuitively clear for PDF graphs that are narrow and tall
- $\int_{-\infty}^{\infty}{f(x)dx}=1$, which means the entire area under the density curve must be $1$, which follows from the properties of probability
- Now, we can see how for any **specific** value $x$, we have $P(X=x)=P(x\leq X\leq x)=\int_{x}^{x}{f(x)dx}=0$
- Note, RVs can have **different PDFs** along subintervals of their possible values; in these cases the calculations of the CDF, $E(X)$, $V(X)$, etc. must be separated out into multiple integrals corresponding subintervals
	- The CDF function will then be **piecewise**, while expectation and variance will **sum** the multiple integrals

**Cumulative Distribution Function:** for a continuous RV $X$ with PDF $f(x)$, the CDF $F_{X}(x)$ is the probability that an observed value of $X$ will be **at most** $x$. In mathematical terms, $F:S\to[0,1]$ such that:
$$F(x)=P(X\leq x)=\int_{-\infty}^{x}{f(u)du}$$
The CDF is the **sum of the area under the density curve** left of $x$. In practice, we set the **lower bound** to the **minimum** of the possible $X$ values. This is essentially a continuous / Reimann sum version of what we did for discrete RVs, where we simply summed the probabilities. 

Key properties of CDFs for continuous RVs:
- $F(x)$ is now a continuous **function of** $x$, rather than a step-function which is better interpreted graphically
- The PDF is the rate of change of the CDF; it follows from the definition above that $F'(x)=f(x)$, so we can **derive the** PDF **from the** CDF, and **vice versa** (noting that the CDF is **not just the antiderivative** of the PDF, but rather a **function** of the RV)
- Recall, piecewise PDFs will lead to piecewise CDFs, and **each successive piece** of the CDF will be a function of the RV **plus** the total areas (static; calculated as **definite integrals** over those pieces) under **all preceding** pieces
- We have $\lim_{ x \to -\infty }{F(x)}=0$ and $\lim_{ x \to \infty }{F(x)}=1$, which simply indicates the range of values in the **support** 
- Recall our definition of the PDF; now with our definition of CDF we can simplify:
$$P(a\leq X\leq b)=\int_{a}^{b}{f(x)dx}=F(b)-F(a)$$

**Percentile:** the $\alpha^{th}$ percentile of a distribution is the value $x_{\alpha}$ such that $F(x_{\alpha})=P(X\leq x_{\alpha})=\frac{\alpha}{100}$, where  $0\leq \alpha\leq 100$. In the continuous case, $x_{\alpha}=F^{-1}(\frac{\alpha}{100})$. Then to find for ex. the **median** ($50^{th}$ percentile) we would **solve for** $x$ in:
$$0.5=F(x)=\int_{-\infty}^{x}{f(u)du}$$


**Expectation:** for a continuous RV $X$, the expectation is: $$E(X)=\mu=\int_{-\infty}^{\infty}{(x\cdot f(x))dx}$$**Variance:** for a continuous RV $X$ with PDF $f$ and expectation $\mu$, the variance of $X$ is: $$V(X)=\sigma^{2}=E((X-\mu)^{2})=\int_{-\infty}^{\infty}((x-\mu)^{2}\cdot f(x))dx$$Again, **in practice the lower bound and upper bounds** are set by the **support** of our RV. All the same properties, like the Law of the Unconscious Statistician, and the Linearity of Expectation, and the variance shortcut, still apply.

Note, Markov's and Chebyshev's inequalities also still apply!
## <u>Week 8: Continuous Distributions</u>
**Uniform Random Variable:** a continuous RV is said to have a uniform distribution on an interval $[a,b]$, denoted $X\sim Unif(a,b)$, if:
$$f(x;a,b)= \frac{1}{b-a}$$
Following from the properties of PDFs, the density curve of such a distribution is a **rectangle** with width $b-a$ and height $\frac{1}{b-a}$, meaning it has **constant density** along its support
- $F(x)=\frac{x-a}{b-a}$; note in the case of the interval $[0,1]$ we have $F(x)=x$
- $E(X\sim Unif(a,b))=\frac{a+b}{2}$
- $V(X\sim Unif(a,b))=\frac{(b-a)^{2}}{12}$


**Exponential Random Variable:** a continuous RV is said to have an exponential distribution with scale $\theta>0$, denoted $X\sim Exp(\theta)$, if:
$$f(x;\theta)= \frac{1}{\theta}e^{-\frac{x}{\theta}}, \; \text{for} \; x\geq 0$$
The exponential distribution models the **random interval until the first or next consecutive Poisson occurrence**. The **scale** parameter $\theta$ represents the expectation of the distribution. 
- $F(x)=1-e^{-\frac{x}{\theta}}$ (or $1-e^{-\lambda x}$ with $\lambda$ parameter)
- $E(X\sim Exp(\theta))=\theta$
- $V(X\sim Exp(\theta))=\theta^2$
This distribution can also be derived from the Poisson distribution, then $X\sim Exp(\lambda)$ with $f(x;\lambda)=\lambda e^{-\lambda x}$, $\theta=\frac{1}{\lambda}$, etc.
- The scale parameter represents the **mean subinterval between events** in the context of a Poisson process, whereas the rate parameter $\lambda$ represents the **average frequency of events per interval** and thus is the unit inverse of the scale
- Note, this distribution is **memoryless** (only the Exponential and Geometric distributions have this property); it can be shown with simple probability operations and algebra that $\forall s,t\geq 0, P(X>s+t|X>s)=P(X>t)$
- There is also a significant result where the **minimum** of $n$ **independent** and **identically distributed** exponential random variables, each with parameter $\lambda$, is itself an Exponential Distribution with parameter $n\lambda$


**Gamma Random Variable:** a continuous RV is said to have a gamma distribution with scale $\theta>0$ and shape $\alpha>0$, denoted $X\sim Gamma(\alpha,\theta)$, if:
$$f(x;\alpha,\theta)= \frac{1}{\Gamma(\alpha)\cdot\theta^\alpha}x^{\alpha-1}e^{-\frac{x}{\theta}}, \; \text{for} \; x>0$$
The gamma distribution models the **random interval until** $k$ **events in a Poisson process** for $k\geq 2$, ex. time interval between the $1$st and $4$th event. The **scale** parameter $\theta$ is the same as the exponential distribution, while the **shape** parameter $\alpha$ is the **count of consecutive Poisson events** that we are modelling. As with the exponential distribution, you can also substitute $\frac{1}{\theta}=\lambda$.
- $F(x)$  when $\alpha,\theta \in \mathbb{Z}^+$, otherwise no closed form; usually computed with numerical methods
- $E(X\sim Exp(\theta))=\alpha\theta$
- $V(X\sim Exp(\theta))=\alpha\theta^2$
- The **standard** gamma distribution sets $\theta=1$ and $\alpha>0$
Note the relations to the exponential distribution:
- When $\alpha=1$, we have $Gamma(1,\theta)\sim Exp(\theta)$
- The **sum** of $n$ **independent identical exponential** RVs (thus, same $\theta$) results in a **gamma** RV $X\sim Gamma(n,\theta)$
The $\Gamma$ function used within is defined as $\Gamma(\alpha)=(\alpha-1)!$


**Normal Random Variable:** a continuous RV is said to have a normal distribution with location $\mu$ and scale $\sigma^2>0$, denoted $X\sim N(\mu,\sigma^2)$, if:
$$f(x;\mu,\sigma^2)= \frac{1}{\sigma \sqrt{2\pi}}e^{-(x-\mu)^{2}/2\sigma^2}, \; \text{for} \; -\infty<x<\infty$$
This is of course the famous bell-curve distribution, also known as the **Gaussian** distribution, and it models many natural phenomena very well. The normal distribution has several key distinctive properties:
- The distribution is **symmetric** about the mean $\mu$ which is the **centre** of the distribution, which means $P(X\leq \mu)=P(X\geq \mu)=0.5$
- The variance is the spread of the distribution; larger $\sigma^2$ means a flatter and wider curve and vice versa
- $E(X\sim N(\mu,\sigma^2))=\mu$
- $V(X\sim N(\mu,\sigma^2))=\sigma^2$
- For two **independent** RVs $X\sim N(\mu_{X},\sigma^2_{X})$ and $Y\sim N(\mu_{Y},\sigma^2_{Y})$, the **linear combination** with some constants $a,b,c \in \mathbb{R}$ is **also normally distributed** $Z=aX+bY+c\sim N(a\mu_{X}+b\mu_{Y}+c,a^2\sigma^2_{X}+b^2\sigma^2_{Y})$, and this extends directly to linear combinations of $n$ normal RVs
	- Note, this applies **only to normal RVs**; generally RVs that are functions of other continuous RVs take on a different distribution!
- The **z-score** calculates how many standard deviations from the mean an observation $x$ of a normally distributed RV is, where $z=\frac{x-\mu}{\sigma}$, and we note the **Empirical Rule** for all normal distributions:
	- $68\%$ of the probability is within $\pm1\sigma$ of $\mu$
	- $95\%$ of the probability is within $\pm2\sigma$ of $\mu$
	- $99.7\%$ of the probability is within $\pm3\sigma$ of $\mu$
- $F(x)$ **has no closed form**; usually computed with numerical methods such as **standardization** to one special case of the normal distribution described below

**Standard Normal Distribution:** a random variable $Z$ is said to follow a standard normal distribution if $Z\sim N(\mu=0,\sigma^2=1)$
- By the linear property described above, we know we can **standardize any normal RV**, so $X\sim N(\mu,\sigma^2)\implies Z=\frac{X-\mu}{\sigma}\sim N(0,1)$ and this can be proven by expanding out $E(\frac{X-\mu}{\sigma})$ and $V(\frac{X-\mu}{\sigma})$
	- Note, this is the z-score transformation!
- Since all normal distributions have the same proportions of probability (recall the Empirical Rule above), once we have a standardized observation, we can use the **Z-Table** for the standard distribution as an **approximation of the CDF for any normal distribution**, ex:
	- $P(a\leq X\leq b)=P(\frac{a-\mu}{\sigma}\leq \frac{x-\mu}{\sigma}\leq \frac{b-\mu}{\sigma})=P(\frac{a-\mu}{\sigma}\leq Z\leq \frac{b-\mu}{\sigma})=P(Z\leq \frac{b-\mu}{\sigma})-P(Z\leq \frac{a-\mu}{\sigma})$ and then refer to Z-Table
	- $P(Z\geq a)=P(Z\leq-a)$ by symmetry, and then refer to Z-Table
## <u>Week 9: Moment Generating Functions</u>
**Moments:** for $k \in \mathbb{N}$, the $k^{th}$ **moment** of a random variable $X$ is $E(X^k)$ and the $k^{th}$ **central moment** is $E((X-E(X))^k)$
- Then, the **first moment** is the **expectation** of $X$, and the second moment is $E(X^2)$ which is useful in the variance calculation shortcut
- The first central moment is $E(X-E(X))=0$, and the **second central moment** is the **variance** of $X$

**Moment Generating Function:** for some $h>0$, the MGF of a random variable $X$ is defined as below, given that $E(e^{tX})$ **exists on some interval around** $0$:
$$
M(t)=E(e^{tX}), \; \text{for some }-h<t<h
$$
Note that $t$ is a variable in the MGF but becomes a **constant** in the expectation calculation. Then, for discrete and continuous RVs respectively:
$$
\begin{align}
M(t)&=E(e^{tX})=\sum e^{tx}\cdot p(x) \\
M(t)&=E(e^{tX})=\int_{-\infty}^{\infty} e^{tx}\cdot f(x) \: dx
\end{align}
$$
For example, given $X\sim Exp(\lambda)$ where $f(x)=\lambda e^{-\lambda x}$ for $x>0$, the MGF is:
$$
M(t)=E(e^{tX})=\int_{0}^{\infty} e^{tx}\cdot \lambda e^{-\lambda x} \: dx
$$

For an RV, if $M(t)$ exists then for any $k \in \mathbb{N}$, the $k^{th}$ **moment** is equal to the $k^{th}$ **derivative** of the MGF at $t=0$:
$$
\frac{d^k}{dt^k}M(0)=M^{(k)}(0)=E(X^k)
$$

For random variables $X_{1},X_{2},\dots,X_{n}$ and constants $a_{1},a_{2},\dots,a_{n}$, consider the **linear combination** $Y=a_{1}X_{1}+a_{2}X_{2}+\cdots+a_{n}X_{n}$, then:
$$
M_{Y}(t)=E(e^{tY})=E(e^{t(a_{1}X_{1}+a_{2}X_{2}+\cdots+a_{n}X_{n})})=E(\prod_{i=1}^{n}e^{ta_{i}X_{i}})
$$
Recall by properties of expectation, if $X_{1},X_{2},\dots,X_{n}$ are all **independent**, then:
$$
M_{Y}(t)=E(\prod_{i=1}^{n}e^{ta_{i}X_{i}})=\prod_{i=1}^{n}E(e^{ta_{i}X_{i}})=\prod_{i=1}^{n}M_{X_{i}}(a_{i}t)
$$

Thus, the MGF has **three** key applications:
1. If the MGF exists for a probability distribution, then it is unique; this means an MGF **uniquely identifies a probability distribution**
	- If we are given the MGF of a **discrete** distribution, it is often simple to map from $E(e^{tX})$ summation form to probabilities of each $x$; for example, given $M(t)=\frac{1}{6}e^t+\frac{2}{6}e^{2t}+\frac{3}{6}e^{3t}$ for a discrete $X$, we can discern $X={1,2,3}$ and $p(1)=\frac{1}{6},p(2)=\frac{2}{6},p(3)=\frac{3}{6}$
		- This is only viable for distributions with **small supports**; for discrete distributions that can have countably infinite supports we refer to the **MGF table** for common discrete distributions
	- If we are given the MGF of a **continuous** distribution, we refer to the **MGF table** for common continuous distributions
2. If we can find $E(e^{tX})$, then we can find $E(X^k)$; meaning we can determine moments using the MGF with the derivative theorem above
	- To find $E(e^{tX})$ given a PMF / PDF, we can setup the summation / integration of $E(X)$ as usual with the given probability function and evaluate to closed form (ie. the example above with the exponential RV)
		- The MGF is closed form with respect to $t$, and is often much simpler to differentiate relative to calculating moments directly from the definition of expectation which can involve complicated integrals
3. The MGF is exceptionally useful for finding the distribution of a **sum of independent random variables**, by the property above wherein the MGF of this sum is the product of MGFs of each RV, the result of which can be used to determine the distribution
## <u>Week 10: Transformations of Random Variables</u>
Consider a given **continuous** RV $X$ and let $Y=g(X)$. The algorithm for the **distribution method** of transformation of RVs follows:
1. **Determine the support** of $Y$ **corresponding to the given support** of $X$; ex. assume $X\in[0,2]$, then $Y\in[g(0),g(2)]$
2. **Determine the CDF** of $Y$ by **relating it to the given CDF** of $X$; ex.
$$
\begin{align}
F_{Y}(y_{0})&=P(Y\leq y_{0}) \\
&=P(g(X)\leq y_{0}) \\
&=P(g^{-1}(g(X))\leq g^{-1}(y_{0})) \\
&=P(X\leq g^{-1}(y_{0})) \\
&=F_{X}(g^{-1}(y_{0}))=\int_{-\infty}^{g^{-1}(y_{0})}f_{X}(x)\;dx
\end{align}
$$
3. **Differentiate the CDF** of $Y$ to get the PDF


Computers can only generate random observations from the standard Uniform Distribution; $U\sim Unif(0,1)$. However, there is an algorithm called the **inverse CDF method** that allows us to turn these standard uniform observations into **observations from any distribution**:
- Consider a non-uniform distribution for which we want to generate random observations, recall the CDF is a function where for the support $S$ of an RV, $F:S\to[0,1]$, from which it follows that $F^{-1}:[0,1]\to S$
- But the support of the standard uniform distribution is precisely $[0,1]$, thus generating a random standard uniform observation $u$ and using it as the value $u=F(x)=P(X\leq x)$ allows us to create a distribution $X=F_{X}^{-1}(U)$ which has CDF $F(x)$
	- Graphically, we are essentially using standard uniform observations to pick points on the $y$-axis of $F(X)$, and tracing them to $x$
	- For example; to generate Exponential observations we know our desired CDF is $F(x)=1-e^{-\lambda x}$, then we set $u=1-e^{-\lambda x}$ and solve for $x=-\frac{\ln(1-u)}{\lambda}$; now any $u$ we generate from $U\sim Unif(0,1)$ can be transformed into an observation $x$ from $X\sim Exp(\lambda)$

The formal statement of the idea above: if $U\sim Unif(0,1)$, then $X=F_{X}^{-1}(U)$ has the CDF $F_{X}(x)$
- Proof: $P(X\leq x)=P(F^{-1}(U)\leq x)=P(F(F^{-1}(U))\leq F(x))=P(U\leq F(x))=F(x)$
	- Note in the final equality we used the property of standard uniform distributions that $F(u)=u$
## <u>Week 10: Joint Probability Distributions</u>
**Joint Random Variables (Bivariate):** given a random experiment with sample space $\Omega$ and two random variables $X,Y$ which assign each element $\omega \in\Omega$ a number, the ordered pair $(X,Y)$ is a **bivariate joint random vector**
- The support of this bivariate vector is the cartesian product of the supports of $X$ and $Y$

**Joint Probability Distribution (Bivariate):** joint RVs are used to describe the probabilistic behaviour of two RVs **simultaneously**
- The graphs of such distributions are maps of $\mathbb{R}^2\to \mathbb{R}$; probability is now represented by the **volume** under the probability functions, corresponding to subsets of the $xy$-plane domain rather than area like in the univariate case


For **discrete** $X,Y$, the joint PMF is $p_{X,Y}(x,y)=P(X=x,Y=y)=P(X=x\cap Y=y)$, and captures the probability that events $X=x$ and $Y=y$ occur simultaneously
- Properties of joint discrete PMFs that are extended directly from the univariate case:
	- $p(x,y)\geq 0$ for all $(x,y)$ in support
	- $\sum_{Y}\sum_{X}p(x,y)=1$
- Properties of the joint discrete CDF $F_{X,Y}(x,y)=P(X\leq x,Y\leq y)$, **some of which differ from the univariate case**, follow:
	- $F(x_{1},y_{1})=\sum_{y\leq y_{1}}\sum_{x\leq x_{1}}p(x,y)$ which is the definition of CDF directly extended from the univariate case
	- $F(-\infty,-\infty)=F(x,-\infty)=F(-\infty,y)=0$
	- $F(\infty,\infty)=1\neq F(x,\infty)=P(X\leq x)\neq F(\infty,y)=P(Y\leq y)$
	- If $a\leq b$ and $c\leq d$, then $P(a\leq X\leq b,c\leq Y\leq d)=F(b,d)-F(b,c)-F(a,d)+F(a,c)$, note we have to **add back the interval from the lower bounds to** $0$ as it gets subtracted twice; this becomes clear when the probabilities are expressed as a grid


For **continuous** $X,Y$, the joint CDF with relation to the PDF is:
$$
F(x_{0},y_{0})=\int_{-\infty}^{y_{0}}\int_{-\infty}^{x_{0}}f(x,y)\;dxdy
$$
Recall, we are calculating the volume under a section of the probability graph here; the inner integral determines the area under one RV as a function of the other, and the outer integral projects it along the area of the other RV
- Properties of joint discrete PDFs that are extended directly from the univariate case:
	- $f(x,y)\geq 0$ for all $(x,y)$ in support
	- $\int_{-\infty}^{\infty}\int_{-\infty}^{\infty}f(x,y)\;dxdy=1$

Consider an example; if we have a joint PDF $f(x,y)=2x$ with support $0\leq x,y \leq 1$, then:
$$
P(X<0.5, Y>0.5)=\int_{0.5}^{1}\int_{0}^{0.5}2x\;dxdy
$$
Notice we **did not use the CDF** here; similar to the univariate continuous case, we can work directly with integrals and the PDF instead of subtracting CDFs from each other.

Note, there is a **property of double integrals** to think about when working with joint PDFs:
- So far we have assumed that the bounds of integration create a rectangular region of support; this is the simplest case, the region becomes a defined by a curve whenever **support of one of the RVs is a function of the other RV**
	- Consider the support $0\leq y\leq x\leq 1$, then the area for a given $x$ depends on $y$ and vice versa, so the **total probability** can be gotten as:
		- $\int_{0}^{1}\int_{0}^{x}f(x,y)\;dydx$, since each $x$'s area is dependent on $y$ and $0\leq y\leq x$
		- $\int_{0}^{1}\int_{y}^{1}f(x,y)\;dxdy$, since each $y$'s area is dependent on $x$ and $y\leq x\leq 1$


Probabilities that describe the likelihood of **one variable’s outcome regardless of the other** are called **marginal probabilities**. Following from our definitions of joint random variables above, the **marginal distribution** PMFs and PDFs respectively would be:
$$
\begin{align}
p_{X}(x)=\sum_{Y}p(x,y)&,\;p_{Y}(y)=\sum_{X}p(x,y)  \\
f_{X}(x)=\int_{-\infty}^{\infty}f(x,y)\;dy&,\;f_{Y}(y)=\int_{-\infty}^{\infty}f(x,y)\;dx
\end{align}
$$
In the continuous case, the variable being integrated with respect to will be eliminated and thus the **result of the integration is the marginal PDF, which will be a function of only the RV of interest**. This PDF is then used as in yet another integration to get marginal probabilities!
- Note if we have a **non-rectangular support**, these functions must reflect that as well; consider our earlier example where $0\leq y\leq x\leq 1$, then: $f_{X}(x)=\int_{0}^{x}f(x,y)\;dy$ and $f_{Y}(y)=\int_{y}^{1}f(x,y)\;dx$
	- Since $f_{Y}(y)=0$ outside of the support, you would still technically get the correct result; nevertheless


**Expectation** and **variance** extend to joint distributions directly; however both of these metrics are **only defined over a single random variable**, thus these measures only make sense for joint random variables if we define **another RV which is a function** of the two joint RVs:
- For joint RVs $(X,Y)$, define another RV $Z=g(X,Y)$ where we have some function $g:\mathbb{R}^2\to \mathbb{R}$, then expectation for discrete and continuous RVs respectively:
$$
\begin{align}
E(Z)&=\sum_{Y}\sum_{X}g(x,y)\cdot p(x,y) \\
E(Z)&=\int_{-\infty}^{\infty}\int_{-\infty}^{\infty}g(x,y)\cdot f(x,y)\;dxdy
\end{align}
$$
- **Linearity of expectation** still applies, wherein $E(a_{1}g_{1}(X,Y)+a_{2}g_{2}(X,Y)+b)=a_{1}E(g_{1}(X,Y))+a_{2}E(g_{1}(X,Y))+b$
	- Furthermore, recall if $X,Y$ are independent then $E(XY)=E(X)\cdot E(Y)$, and thus $E(g(X)h(Y))=E(g(X))\cdot E(h(Y))$
- For variance, **use the shortcut** $V(Z)=E(Z^2)-E(Z)^2$

If we are asked to find expectation or variance of **one** of the joint random variables, for example $E(X)$, we can use one of two methods:
- Define a function $g(X,Y)=X$ and an RV $Z=g(X,Y)$, then perform the $E(Z)$ as above, ie. summation or integration **over the entire support** of the joint RVs; intuitively this is simply multiplying the $X$ value by each probability!
- Determine the marginal distribution of $X$, ie. $p_{X}(x)=\sum_{Y}p(x,y)$ or $f_{X}(x)=\int_{-\infty}^{\infty}f(x,y)\;dy$, then use that in the **univariate expectation calculation** where $E(X)=\sum_{X}x\cdot p_{X}(x)$ or $E(X)=\int_{-\infty}^{\infty}x\cdot f_{X}(x)\;dx$
## <u>Week 11: Covariance and Correlation</u>
Covariance and correlation are metrics that describe how two variables change together **in the context of their bivariate joint probability distribution**.

**Covariance:** for RVs $X,Y$, covariance is the strength and direction of the **linear relationship** between $X$ and $Y$, and is defined as:
$$
\begin{align}
\text{Cov}(X,Y)&=E[(X-E(X))\cdot(Y-E(Y))]  \\
&=E(XY)-E(X)\cdot E(Y) \\
&=\int_{-\infty}^{\infty}\int_{-\infty}^{\infty}xy\cdot f(x,y)\;dxdy-\int_{-\infty}^{\infty}\int_{-\infty}^{\infty}x\cdot f(x,y)\;dxdy \cdot\int_{-\infty}^{\infty}\int_{-\infty}^{\infty}y\cdot f(x,y)\;dxdy
\end{align}
$$
The interpretation of what is a small or large covariance depends on the measurements of the RVs themselves, and the units are also relevant. Note, this calculation requires knowledge of the joint PMF or PDF of the two RVs, in order to determine the various expectations. Lastly:
- $\text{Cov}(X,Y)=\text{Cov}(Y,X)$
- $\text{Cov}(X,X)=V(X)$
- $\text{Cov}(aX+bY+c,Z)=a\text{Cov}(X,Z)+b\text{Cov}(Y,Z)$

**Correlation:** for RVs $X,Y$, with standard deviations $\sigma_{X},\sigma_{Y}$, correlation $\rho$ is defined as:
$$
\rho_{XY}=\frac{\text{Cov}(X,Y)}{\sigma_{X}\sigma_{Y}}
$$
Correlation is also a measure of linear association between two RVs, but is **unit-free** and **measurement invariant**; $-1\leq\rho\leq 1$. Then, $\text{Cov}=\rho=0$ implies two RVs are **uncorrelated**. Note again that the joint PMF or PDF will be required, to use the expectation shortcut of calculating variance (and then standard deviation).
- $\rho_{XY}=\rho_{YX}$
- $\rho_{XX}=1$
- $\rho_{aX+b,cY+d}=\rho_{XY}$
- If $Y$ is a linear combination of $X$ or vice versa, then $\rho_{XY}=\pm1$

Covariance and correlation are related to the concept of independence;  uncorrelated RVs are **linearly independent**, but may still be quadratically dependent, etc. **Independence is a stronger statement**; independent$\implies$uncorrelated, uncorrelated$\centernot\implies$independent.


**Conditional Distribution:** for bivariate joint RVs, the conditional distribution of a random variable given another one is the distribution of one of the random variables when the other has assumed a specific value; assume joint RVs $X,Y$ with joint PMF $p(x,y)$ or PDF $f(x,y)$, then:
- $P(X=x|Y=y_{0})=\frac{P(X=x\cap Y=y_{0})}{P(Y=y_{0})}=\frac{p(x,y_{0})}{p_{Y}(y_{0})}=p(x|y_{0})$
- $P(X=x|Y=y_{0})=\frac{f(x,y_{0})}{f_{Y}(y_{0})}=f(x|y_{0})$
	- Then $P(X\leq x|Y=y_{0})=F(x|y_{0})$
	- Note that $y_{0}$ is a **parameter (not a variable)** in all these calculations, thus the **conditional CDF is simply the single integral of the conditional PDF with regards to** $x$ above
We can also use CDFs in slightly more involved way in calculations like:
- $P(X\leq x|Y\leq y_{0})=\frac{P(X\leq x \cap Y\leq y_{0})}{P(Y\leq y_{0})}=\frac{F(x,y_{0})}{F_{Y}(y_{0})}$
	- In these cases, we would also need to do the single integral of the marginal PDF in the denominator
Note, we are often **given the joint** PMF or PDF, but the denominators require us to also determine the **marginal** PMF, PDF, or CDFs. It also follows that these conditional probability functions are **not defined** when $p_{Y}(y)=0$ or $f_{Y}(y)=0$ above.

**Conditional expectation** extends directly from the base definition of expectation:
$$
\begin{align}
E(X|Y=y_{0})&=\sum_{X}x\cdot p(x|y_{0})\;\text{or} \\
&=\int_{-\infty}^{\infty}x\cdot f(x|y_{0})\;dx
\end{align}
$$


Recall the idea of independence defined $P(A\cap B)=P(A)\cdot P(B)$, or $P(A|B)=P(A), P(B|A)=P(B)$. Then in conditional distribution terms, two joint RVs are **independent** if:
$$
p(x,y)=p_{X}(x)\cdot p_{Y}(y),\;\text{or} \; f(x,y)=f_{X}(x)\cdot f_{Y}(y)
$$
Meaning, each joint probability is simply the multiplicative of the corresponding marginal probabilities. It follows that independence implies that for $p(x|y_{0})=p_{X}(x)$ and $f(x|y_{0})=f_{X}(x)$.  Overall, $X,Y$ are **independent** if:
1. If for all $(x,y)\in S_{X}\times S_{Y}$ we have $f(x,y)=g(x)\cdot h(y)$, where $g$ is a nonnegative function of $x$ and $h$ a nonnegative function of $y$
2. If the support is rectangular and thus can be factorized; then, if **support is non-rectangular** then the RVs are **dependent** (ie. the value of $y$ gives us the domain of $X$, so if non-rectangular this domain differs for different $y$)
## <u>Week 12: Statistics, Sampling Distributions and Central Limit Theorem</u>
**Population:** the set of all elements of interest, which may be finite or infinite
- A probability distribution for the population is usually assumed, with **unknown parameters**

**Sample:** a subset of the population
- Samples are collected to make **inferences** about the population
- In a random sample, **randomness comes from the sampling procedure**
- Each sampled observation is assumed to be **independent** of all others and follow the **same probability distribution as the population**, thus each sampled observation is an **independent random variable**!
	- **iid**: a set of random variables $X_{1},X_{2},\dots ,X_{n}$ is said to be **independent and identically distributed** if all the RVs: 
		1. Are **independent** of each other
		2. Follow the **same distribution** (that of the population)
		- This is denoted $X_{1},X_{2},\dots ,X_{n}\overset{iid}{\sim} F_{X}$, where $F_{X}$ is the population distribution
		- These conditions are satisfied by definition if we are **sampling with replacement** or from an **infinite population**, or recall from binomial distributions the convention of sample size being less than $5\%$ of the population size if sampling without replacement

**Statistic:** a statistic is a function $T$ over a **collection of random variables** $\mathbf{X}$ that does not depend on any unknown parameters
- **Sample Statistic:** a sample statistic $T(\mathbf{X})$ is a quantity computed from the sample data $\mathbf{X}=(X_{1},X_{2},\dots,X_{n})$ which does not involve any unknown quantities
	- Note, sample statistics are **quantities computed from sample data** unlike expectation or variance of a probability distribution which are **theoretical values derived from models**
	- Common sample statistics are the **sample mean** $\bar{X}=\frac{1}{n}\sum_{i=1}^{n}X_{i}$, sample variance $S$, etc.

Since sample statistics are functions of random variables, **statistics themselves are random variables** denoted in uppercase, and observations of statistics are denoted in lowercase ex. $\bar{x},s$
- The probability distribution of a sample statistic is called the **sample distribution** to emphasize that it describes how the statistic varies in value across all samples that might be selected, and it depends on:
	1. The population distribution
	2. The sample size $n$
	3. The sampling methodology
The sample statistic $\bar{X}$ is usually of most interest. $\bar{X}$ based on a large $n$ tends to be closer to $\mu$ than does $\bar{X}$ based on a small $n$.


**Central Limit Theorem:** let $X_{1},X_{2},\dots ,X_{n}$ **iid** random variables with $E(X_{i})=\mu$ and $V(X_{i})=\sigma^2<\infty$, then for large enough $n$:
$$
\bar{X}_{n}=\frac{1}{n}\sum_{i=1}^{n}X_{i}\dot{\sim} N(\mu,\frac{\sigma^2}{n})
$$
This states that as the sample size $n$ increases, the sampling distribution of $\bar{X}$ becomes increasingly **normally distributed** with $E(\bar{X})=\mu$,  $V(\bar{X})=\frac{\sigma^2}{n}$ and $SD(\bar{X})=\frac{\sigma}{\sqrt{ n }}$, **irrespective of the population distribution** from which values were sampled
- If the distribution of the random variables $X_{1},X_{2},\dots ,X_{n}$ themselves was normal, then this result would follow directly from the property of **linear combinations of normally distributed RVs** (the sample mean is such a linear combination) also being normally distributed

If we have $X_{i}\sim Bernoulli(p)$ iid, where we have by definition $E(X_{i})=p$ and $V(X_{i})=p(1-p)$, then $\hat{p}$ is the **sample proportion** with:
$$
\hat{p}=\frac{1}{n}\sum_{i=1}^{n}X_{i}\dot{\sim}N(p,\frac{p(1-p)}{n})
$$