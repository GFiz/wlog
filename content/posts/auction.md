---
title: "A close look into 1st price / 2nd price auctions"
date: 2023-07-10
description: "We introduce some basic mechanism design concepts and analyse 1st/2nd price auctions in the symmetric case."
tags: ["mechanism design"]
categories: ["technical"]
searchHidden: false  
draft: true
math: true
---
## Introduction

At the heart of auction theory lie two fundamental mechanisms: first-price and second-price (Vickrey) auctions. Let us examine each in turn.

In a first-price auction, the rules are straightforward: the highest bidder claims the prize and pays precisely the amount they bid. Mathematically, we can express the payment rule as the supremum of all bids:
$$
\pi(b_1, b_2, \dots, b_n) = \max \left( \lbrace b_1, b_2, \dots, b_n \rbrace \right) = b^{(1)}.
$$ 

The second-price auction, named after William Vickrey who pioneered its study, operates with a subtle but crucial difference. While the highest bidder still wins, they pay not their own bid, but the second-highest bid offered. Formally, the payment rule becomes:
$$
\pi(b_1, b_2, \dots, b_n) = \max \left( \lbrace b_1, b_2, \dots, b_n \rbrace \setminus \lbrace b^{(1)} \rbrace \right) = b^{(2)}
$$
where $b^{(2)}$ naturally represents the second-highest bid.

To study how these auctions behave, we must first grasp the concept of valuation. Each participant $i$ possesses a private valuation $v_i \in \mathbb{R}^+$ for the item - a number known only to themselves which describes how valueable the inventory is for bidder $i$.  These valuations are drawn from probability distributions $F_i(v)$, where $F_i(v) = \mathbb{P}(v_i \leq v)$ denotes the cumulative probability and $f_i(v) = \frac{dF_i(v)}{dv}$ the corresponding probability density function.

The strategic behavior of bidders is captured by their *bidding strategy*. A strategy is formally defined as a function $\beta: \mathbb{R^+} \to \mathbb{R^+}$ that maps a bidder's valuation $v$ to their bid $b$. Under some mild assumptions, it can be show that a strategy  $\beta$  satisfies several natural properties:

1. **Monotonicity**: $\beta$ is increasing in $v$ i.e. higher valuations naturally lead to higher bids. 

2. **Continuity**: $\beta$ is continuous in $v$  i.e. small changes in valuation lead to small changes in the bid.

3. **Differentiability**: $\beta$ is often assumed to be differentiable, allowing us to use calculus techniques to find optimal bidding strategies.

In game-theoretic terms, we analyze these auctions using the framework of **Bayesian Nash Equilibrium**. In equilibrium, each bidder's strategy $\beta_i$ maximizes their expected utility, given the strategies of other bidders. The expected utility for bidder $i$, given a valuation $v_i$ and bid $b_i$ is given by 

$$
\begin{align}
 U_i(v_i,b_i,\beta_{-i}) &= \mathbb{P}(\text{Win} | b_i) \cdot (v_i - \mathbb{E}\left [\pi (b_i,\mathbf{b}_{-i})\right | \text{Win}])
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

{{< thm proposition "[Rationality and monotonicity of allocation imply monotonicity of $\beta$]" >}}
*Suppose that a bidder is rational. Then, their bidding strategy is monotone.*
**Proof.** Assume for contradiction that the bidder 1's  strategy is not monotone. Then, there exists some $v, v' \in \mathbb{R}$ such that $v < v'$ and $\beta(v)=b > \beta(v') = b'$ and consider the following utility values:
$$
\begin{align*}
U(v, b) &= \mathbb{P}(\text{Win} | b) \cdot \left(v - p(b)\right) \\\\
U(v', b') &= \mathbb{P}(\text{Win} | b') \cdot \left(v' - p(b')\right) \\\\
U(v, b') &= \mathbb{P}(\text{Win} | b') \cdot \left(v - p(b')\right) \\\\
U(v', b) &= \mathbb{P}(\text{Win} | b) \cdot \left(v' - p(b)\right) \\\\
\end{align*}
$$
where $p(b)=\mathbb{E}\left [\pi (b,\mathbf{b}_{-1})\right]$ and let us look at the following differences:
$$
\begin{align*}
\Delta_1 &= U(v, b) - U(v, b') \\\\
\Delta_2 &= U(v', b') - U(v', b)\\\\
\end{align*}
$$
Observe that 
$$
\Delta_1 + \Delta_2 = \left( \mathbb{P}(\text{Win} | b) - \mathbb{P}(\text{Win} | b') \right) \cdot (v_1 - v_2)
$$
Recall that $v < v'$ and $b > b'$. In particular, $\mathbb{P}(\text{Win} | b) \geq \mathbb{P}(\text{Win} | b')$. This implies that $\Delta_1 + \Delta_2 < 0$ and therefore swapping the values $b$ and $b'$ would increase the bidder's utility. This contradicts the assumption that the bidder is rational and thus the bidder's strategy must be monotone $\qed$
>Note that in the proof we are using the fact that the probability of winning is a monotone function of the bid. This property of the allocation rule a.k.a the social choice function is known as *the weakly monotone property*.
{{< /thm >}}


A particularly important case in auction theory is the symmetric scenario, where all bidders have valuations drawn from the same distribution $F(v)$, and consequently use the same strategy function $\beta(v)$ to map their valuation $v$ to a bid $b$. This symmetry simplifies the analysis while still bringing to light some key prroperties. For this post, we will assume that bidder valuations are independent and identically distributed (i.i.d.), meaning that each bidder's valuation is drawn independently from the same distribution. Therefore the joint distribution of valuations is $\mathbf{F}(v_1, v_2, \dots, v_n) = \prod_{i=1}^n F(v_i)$.

## Optimal Strategies

Let's know try to derive the optimal strategies in the symmetric setting for both 1st and 2nd price auctions.

{{< thm proposition "[Truthfulness in second price auctions]">}}
*In a second price auction, bidding truthfully, i.e. $\beta(v)=v$,  is a weakly dominant strategy.*
**Proof.**
Assume for some bidder 1, there exist some $v\in \mathbb{R}^+$ such that $v<b=\beta(b)$. Let $Y = \max_{i\neq j}\mathbf{b}$ and observe that the utility for placing a bid $b$ at value $v$ is given by:

$$
U(v,b, Y) = \begin{cases}
v - Y > 0 & \text{if } Y < v < b \\\\
v - Y \leq 0 & \text{if } v \leq Y \leq b \\\\
0 & \text{if } Y > b
\end{cases}
$$
and observe that, when taking the expectation over $Y$
$$
\mathbb{E}[U(v,b,Y)]\leq\mathbb{E}[U(v,v,Y)]
$$
A similar argument shows that the above also holds with $b<v$.
>Note That that the only property we have used here is the fact that we have private values and so the bidding strategy may only depend on a bidders own valuation.
{{< /thm >}}

{{< thm proposition "[Optimal strategy in first price]">}}
*In a first price auction with i.i.d valuations drawn from a distrubution $F$,  the following strategy is a symmetric equilibrium*:
$$
\begin{align}
    \beta(v) = \int_0^v(1-F(x)^{n-1})= \int_0^v x\frac{d}{dx}(F(x)^{n-1}) =\mathbb{E}\left[Y | Y< v \right],
\end{align}
$$
*where $Y= \max \lbrace X_1,\ldots, X_{n-1} \rbrace$ and $X_i \sim F$.*
**Proof.**


{{< /thm >}}
