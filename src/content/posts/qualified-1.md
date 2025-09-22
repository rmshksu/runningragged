---
title: Qualified I
published: 2025-09-27
description: I crave nothing more than to be helpful.
tags: ["Math", "Statistics"]
category: Quals
draft: true
---

My department uses a qualifying examination to determine who gets to be a Ph.D. student in the department. It's a semi-standard practice, especially in math and stats programs, but we're lagging behind on some things that other departments and Universities have in place by default.

One is the fact that the majority of the grad students in my department can't decide what the hell the exam is. Some say the quals are a 6 hour exam stretched over 1 year of coursework with the intent on making newcomers jump through hoops, exclusively for the plot.

Some believe the quals to be easy and an assumption of knowledge that should be met prior to walking through the doors (at the risk of sounding like a hypocrite, that's an insane thing to say). 

The rest vaguely fall into the category of seeing the quals as a necessary evil. The exam is tough because it contains the full breadth of '2 AM knowledge' (as in I shake you awake at 2 AM and ask you a question you should be able to rapidly answer) that any statistician worth their salt should obtain. The process of preparing for the exam is meant to weed out the ones who don't wish to be there and 'boot camp' the ones who do.

I disagree with all three. 

1. The exam is not an elaborate hazing scheme (I hope) because if it was I guarantee a team of professional statisticians could come up with something less elaborate and more evil. 

2. The knowledge of the exam isn't a valid assumption because it's incoherently stupid to assume that a Ph.D. program is such a confirmatory exercise of knowledge that nobody ever gets a Doctorate that significantly differs from their previous degrees.

3. There's no obvious connection between students who do well with the quals and high quality Ph.D. students in the department. I believe, if we reported the data, most high school algebra students could prove this to be true with a line of best fit. More importantly *I took the exams*. If I ever have to use more than 30% of that information I'll be deeply concerned about how my career could have gone so wrong that I work for a Department of Mathematics as a lecturing professor for engineering maths. 

The good news is that the problem with the exam, I believe, is easy to say. The bad news is that it's awful to produce.

We need a god damn study guide. So that's what I intend to produce. Not because I care about passing those exams (see 3. above) but because I *love* educating. I imagine the primary reason my department lacks a study guide, unlike most departments with an analogous qualifying exam, is because we're a small group and spread razor thin. Luckily I have passion, questionable capability, and horrible boundaries with my free time.

---

Let's start from the beginning. There are 2 (two) exams that make up "The Quals":

1. Mathematical Statistics

2. Linear Theory

They typically refer to the second exam as the "Applied Exam" but I think that's a gross misrepresentation. To be fair, for a lot of my department a problem can be considered applied if we resolve to a natural number greater than 2. So they're not lying so much as out of touch. 

I'll be starting with mathematical statistics since that tends to be the big bad beast haunting every student. 

---

The department recommends for us to study chapter 1 - 10 in Statistical Inference by George Casella and Roger L. Berger (we usually just refer to the book as Casella & Berger). No offense to either Casella or Berger, this isn't a very good textbook. It's an *unbelievably good* encylopedia and reference manual for all of mathematical statistics. But as a textbook it holds about as much water as a pasta strainer. 

So to ask a set of students to study 10 chapters of an encylopedia is a pretty ridiculous ask, even for Ph.D. students. The worst part is *they aren't wrong to ask this*. The exam *is* covering those 10 chapters, but chapters 1-5 are the background knowledge for chapters 6-10 so the syllabus isn't wrong it's just abandoning the assumption of pre-requisite knowledge. 

This is an even weirder feature of the exam since the *vast* majority of our Ph.D. students are coming directly from an undergraduate program and taking two subsequent courses on chapters 1-5 (and a brief overview of topics in chapters 6-10) before they even touch the exam. 

What I'd like to (attempt to) do is start with the "real" topics the exam covers and work backwards to hit all of the requisite knowledge. We'll see how this goes...

---

I opened my (stolen and extremely worn) copy of Casella & Berger to the chapter 6 exercises and immediately saw a good one to kick this off. 

**Exercise 6.2:**

>Let $X_1, ... X_n$ be independent random variables with densities

$$
f_{X_i}(x|\theta) = \begin{cases}
e^{i\theta - x} & x \gt i\theta . \\
0 & x < i\theta \\
\end{cases}
$$

>Prove that $T= {min}_i (X_i / i) is a sufficient statistic for \theta .

<br>

I like this problem because it's unintuitive in the way it's written. You can tell the exact moment when the primary author of the book switches between Casella and Berger. I don't know which one it is but *one of the two* prefers convention and rigor over readability of accessibility. I can't do better right now so this is unjustifiable slander.

Let's start with a the lowest level of assumed knowledge here. $X_1, ... , X_n$ are independent random variables. Something taught in my introductory statistics course (and most for that matter) is that independent events have multiplicative intersections. That is, if $X_1$ and $X_2$ are independent then:

$$
P(X_1 \cap X_2) = P(X_1)P(X_2)
$$

This is a result from the definition of independence which is:

$$
P(X_1|X_2) = P(X_1)
$$

With some quick algebra we get:

$$
\begin{aligned}
\frac{P(X_1 \cap X_2)}{P(X_2)} = P(X_1) \\
\\
P(X_1 \cap X_2) = P(X_1)P(X_2) \\
\end{aligned}
$$

I went through this exercise simply because I get frustrated by the bad habit of mathematical statistics instructors deciding that showing work is an optional step for the teacher but required for the student. 

Now we can extend this idea to random variables, which are just functions displayed as variables. The joint density of a string of independent random variables is the product of their densities:

$$
f(x_1,...,x_n | \theta) = \prod_{i=1}^n f(x_i | \theta)
$$

>Side note: My failure to punctuate my equations will be corrected when statisticians learn to italicize species names. I crave nothing more than to be spiteful. 

We can't apply this concept directly yet because the random variables in the C&B exercise have a piecewise density function. I'm sure anyone with an undergraduate in math is laughing at me right now and saying that it's completely possible— I don't really care to double check when I know there's a better way. Indicator functions.

---

Indicator functions are functions that check for a binary condition and return a different outcome based off of the condition. We can think of them as a more sophisticated mathematical equivalent of a *bit* (True/False question). For the C&B exercise our indicator function is:

$$
f(x_i | \theta) = e^{i \theta - x_i} I_{\{x \ge i \theta\}}
$$

Everything's contained in one clean line. Independence, take the wheel!

$$
f(x_1, ... , x_n | \theta) = \prod_{i=1}^n e^{i \theta - x_i} I_{\{x \ge i \theta\}}
$$

The next step involves an important theorem for avoiding doing hard work (the bane of any good statisticians existence). 

---

*Theorem 6.2.6* (Facorization Theorem):

Let $f(\boldsymbol{x}|\theta)$ denote the joint pdf or pmf of a sample $\boldsymbol{X}$. A statistics $T(\boldsymbol{X})$ is a sufficient statistics for $\theta$ if and only if there exist functions $g(t|\theta)$ and $h(\boldsymbol{x})$ such that, for all sample points $\boldsymbol{x}$ and all parameter points $\theta$,

$$
f(\boldsymbol{x}|\theta) = g(T(\boldsymbol{x})|\theta)h(\boldsymbol{x}).
$$

What this mess of rigor and formal language means is that if we can sneak our joint density function into the form of $g(T(\boldsymbol{x})|\theta)h(\boldsymbol{x})$ we can throw out $h(\boldsymbol{x})$ and identify the sufficient statistics from the remaining piece. 

Ya so what the hell is a sufficient statistic?

---

I think the easiest way to explain a sufficient statistic is to call it *not* an ancillary statistic. An *ancillary* statistic is any function of a random variable that doesn't rely on the parameters of the random variable. This is an attractive feature in general because parameters of distributions and random variables tend to require *population* level data to calculate. As it's often impossible to sample the entire population it's pretty nice to avoid relying on these parameters.

Sufficient statistics for parameters are any statistics that contain all of the information for that parameter found within the sample. That is, the information in a sufficient statistic for $\theta$, $T(x)$, will provide us with all of the information that $x$ has related to $\theta$. 

The reason I say sufficient statistics are just *not* ancillary statistics is because of the factorization theorem. We can focus on pulling out the ancillary statistic to form $h(\boldsymbol{x})$ and whatever is leftover can be considered the sufficient statistic.

So let's do that.

$$
\begin{aligned}
f(x_1, ... , x_n | \theta)  = \prod_{i=1}^n e^{i \theta - x_i} I_{\{x \ge i \theta\}} \\
\\
 = (e^{i \theta - x_i})^n I_{\{x \ge i \theta\}} \\
\\
 = e^{n i \theta - \sum_{i=1}^n x_i} I_{\{x \ge i \theta\}} \\
\\
\end{aligned}
$$

The problem's almost solved, but I need to briefly correct some language before we can finish. 

---

We're not *proving* anything in the sense that we're conducting a proof. There's no proof by induction, contraposition, minimal counterexample, or any identifiable method. This is confusing because Casella and Berger are *highly* competent and rigorous mathematical minds, why would they improperly use this phrase? I have no idea. Convention? Whatever. The point is, this isn't a proof. We're not stating lemmas, providing deductive steps, or finishing with a Halmos tombstone (the only correct way to conclude a proof). We're going to section off this joint density, cite a theorem, and turn in our work. 

Since this sort of "proof" problem shows up a lot in C&B I think it's appropriate to define a different structure to appropriately detailing our work. From here on out I'll structure these problems as such:

$"Proof."$

$Theorem$

$Step \ 1$

$Step \ 2$

$Conlusion$

$$
G.A.
$$

Where G.A. stands for "Go away", something I tell my students after every lecture concludes. 


