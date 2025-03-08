+++
date = '2025-03-08T12:24:58+01:00'
draft = true
title = 'Auction'
+++

# A close look into 1st price / 2nd price auctions

## The setup

In auction theory, we typically examine two fundamental types of auctions: first-price and second-price (Vickrey) auctions. In a first-price auction, the highest bidder wins the item and pays exactly what they bid. Formally, the payment rule can be expressed as the maximum of all bids: 
$$
\pi(b_1, b_2, \dots, b_n) = \max \left( \lbrace b_1, b_2, \dots, b_n \rbrace \right) = b^{(1)}.
$$ 

In contrast, a second-price auction, also known as a Vickrey auction, awards the item to the highest bidder but requires them to pay only the second-highest bid. The formal payment rule in this case is 

$$\pi(b_1, b_2, \dots, b_n) = \max \left( \lbrace b_1, b_2, \dots, b_n \rbrace \setminus \lbrace b^{(1)2} \rbrace \right) = b^{(2)}$$ 

where $b^{(2)}$ denotes  the second-highest bid (hence the name)

The foundation of auction analysis lies in the concept of valuation: each bidder $i$ possesses a private valuation (only known to himself) $v_i \in \mathbb{R}^+$ for the item being auctioned. These valuations are drawn from a probability distribution $F_i(v)$, where $F_i(v) = \mathbb{P}(v_i \leq v)$ denotes  the cumulative distribution function and $f_i(v) = \frac{dF_i(v)}{dv}$ is the corresponding probability density function.



The strategic behavior of bidders is captured by their *bidding strategy*. A strategy is formally defined as a function $\beta: \mathbb{R^+} \to \mathbb{R^+}$ that maps a bidder's valuation $v$ to their bid $b$. Under some mild assumptions, it can be show that a strategy  $\beta$  satisfies several natural properties:

1. **Monotonicity**: $\beta$ is increasing in $v$ i.e. higher valuations naturally lead to higher bids. 

2. **Continuity**: $\beta$ is continuous in $v$  i.e. small changes in valuation lead to small changes in the bid.

3. **Differentiability**: $\beta$ is often assumed to be differentiable, allowing us to use calculus techniques to find optimal bidding strategies.




In game-theoretic terms, we analyze these auctions using the framework of **Bayesian Nash Equilibrium**. In equilibrium, each bidder's strategy $\beta_i$ maximizes their expected utility, given the strategies of other bidders. The expected utility for bidder $i$, given a valuation $v_i$ and bid $b_i$ is given by 

$$
\begin{align}
 U_i(v_i,b_i,\beta_{-i}) &= \mathbb{P}(\text{Win} | b_i) \cdot (v_i - \mathbb{E}\left [\pi (b_i,\mathbf{b}_{-i})\right])
\end{align}
$$

where 
- $\beta_{-i}$ represents the strategies of all bidders except $i$.
- The expectation is taken over $\mathbf{b}_{-i}$, the tuple of bids for all bidders except $i$.


In equilibrium, the optimal strategy $\beta_i^{\ast}$ for each bidder $i$ satisfies:
$$
\beta_i^{\ast}(v_i) = \arg\max_{b_i} \mathbb{E}\left[U_i(v_i, b_i, \beta_{-i}^{\ast})\right]
$$

Given the equilibrium strategies $\beta_{-i}^{\ast}$ of all other bidders, bidder $i$ cannot increase their expected utility by deviating from $\beta_i^{\ast}$. We say that a bidder is *rational* if they follow such an optimal strategy.

{{< theorem proposition "[Rationality and monotonicity of allocation imply monotonicity of $\beta$]" >}}
*Suppose that a bidder is rational. Then, their bidding strategy is monotone.*
**Proof.**
Assume for contradiction that the bidder's strategy is not monotone. Then, there exists some $v_1, v_2 \in \mathbb{R}$ such that $v_1 < v_2$ and $\beta(v_1)=b_1 > \beta(v_2) = b_2$ and consider the following utility values:
$$
\begin{align*}
U(v_1, b_1) &= \mathbb{P}(\text{Win} | b_1) \cdot \left(v_1 - p(b_1)\right) \\\\
U(v_2, b_2) &= \mathbb{P}(\text{Win} | b_2) \cdot \left(v_2 - p(b_2)\right) \\\\
U(v_1, b_2) &= \mathbb{P}(\text{Win} | b_2) \cdot \left(v_1 - p(b_2)\right) \\\\
U(v_2, b_1) &= \mathbb{P}(\text{Win} | b_1) \cdot \left(v_2 - p(b_1)\right) \\\\
\end{align*}
$$
and let us look at the following differences:
$$
\begin{align*}
\Delta_1 = U(v_1, b_1) - U(v_1, b_2) &= \left( \mathbb{P}(\text{Win} | b_1) - \mathbb{P}(\text{Win} | b_2) \right) \cdot \left(v_1 - p(b_2)\right) \\\\
\Delta_2 = U(v_2, b_2) - U(v_2, b_1) &= \left( \mathbb{P}(\text{Win} | b_2) - \mathbb{P}(\text{Win} | b_1) \right) \cdot \left(v_2 - p(b_1)\right) \\\\
\end{align*}
$$
Observe that 
$$
\Delta_1 + \Delta_2 = \left( \mathbb{P}(\text{Win} | b_1) - \mathbb{P}(\text{Win} | b_2) \right) \cdot (v_1 - v_2)
$$
Recall that $v_1 < v_2$ and $b_1 > b_2$. In particular, $\mathbb{P}(\text{Win} | b_1) \geq \mathbb{P}(\text{Win} | b_2)$. This implies that $\Delta_1 + \Delta_2 < 0$ and therefore swapping the bids $b_1$ and $b_2$ would increase the bidder's utility. This contradicts the assumption that the bidder is rational and thus the bidder's strategy must be monotone.
>Note that in the proof we are using the fact that the probability of winning is a monotone function of the bid. This property of the allocation rule a.k.a the social choice function is known as *the weakly monotone property*.
{{< /theorem >}}




A particularly important case in auction theory is the symmetric scenario, where all bidders have valuations drawn from the same distribution $F(v)$, and consequently use the same strategy function $\beta(v)$ to map their valuation $v$ to a bid $b$. This symmetry simplifies the analysis while still capturing the essential strategic elements of auction behavior.



For this post, we will assume that bidder valuations are independent and identically distributed (i.i.d.), meaning that each bidder's valuation is drawn independently from the same distribution. Therefore the joint distribution of valuations is $\mathbf{F}(v_1, v_2, \dots, v_n) = \prod_{i=1}^n F(v_i)$.
