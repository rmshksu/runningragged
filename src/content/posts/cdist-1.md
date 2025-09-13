---
title: C(++)-Distributed I
published: 2025-09-13
description: Step aside, student. Mediocrity has arrived.
tags: ["Programming", "cpp", "Distributions", "Statistics"]
category: C-dist
draft: false
---

This is really the first part of an even larger side project: my decision science (or whatever the hell we're calling statistics on LinkedIn now) function repository, Alexandria. 

I like the idea of Alexandria because it leverages my love for good documentation (which is realistically anything that isn't the Arch Linux wiki, those doc maintainers should all be ashamed of themselves) and marries it with my passion for never opening stack overflow unless I'm being held at gun point. 

The idea I've had for a while is setting up a series of .cpp executables that I can string together in a frankenstein array to inevitably run custom Bayesian models. I have an aversion to using libraries that I can't assure will be alive and well by the end of my career so it'll be an exploration on my part to locate the ones I can trust. 

Eventually I'll lay out some education on object oriented programming but for now I want to stick to the messy process of building programs.

---

As with any project it's important to start off with some statements, observations, hypotheses, and literature review. 

I'll admit it, I'm not a programmer. I'm a scripter, I work exclusively with R, Python, and SAS. But I'm sick of the constant roadblocks I encounter with each language and I've always wanted to be a more competent programmer. My brief stint with network algorithms made me realize the value of optimal code and performant languages.

I've been in a fight with a close friend for almost a year as to what language I should jump to. I wanted to follow the footsteps of g++, who swapped from C++ to C so that he would stop getting away with bad programming principles. My friend said it's a wasted effort to learn C if I have no intention of doing operating system work. To be fair, my friend is a back-end developer who's solution to every problem is C#. The only operating system engineer I know would probably tell me to stick to python and get better at vectorization. 

My problem is that I can't fucking *stand* python. Probably because it's my first language and everyone (besides Java developers) hates their first language. But I also despise python because its barrier to entry is pathetically low while the skill ceiling is so high it's unreasonable. On top of that I really don't like interacting with a community that considers machine learning to be the entire definition of statistics, especially when they think multiple linear regression is machine learning.

After a bunch of digging I've resolved two things:

* Learning C would make me a much better programmer.

* I'd end up switching to C++ anyway once I'm deep in my career.

So we're doing C++ now.

---

I don't like R packages or Python libraries because I spend more time reading poorly written documentation than I do actually producing anything of value. In the era of vibe coders both of these things have become *extremely* common. I'm not better than anyone because I stray away from external libraries— I'd argue I'm objectively stupid for that. I just don't like reading more than I have to (thanks, crippling dyslexia). 

C++ won't entertain me on that preference though, I'm not going to beat a mature C++ library when it comes to defining vectors, matrices, arrays, and more. But I still want to minimize my library usage. I think, after a bit of digging, I've isolated two libraries I'll be sticking to for the majority of this project.

1. Rcpp: an R and C++ interface. As far as I can tell this is going to let me slowly integrate C++ into my research rather than make a full swap.

2. Eigen: Linear algebra hot pot. I did a bit where I attempted to perform simple linear regression in C. It was awful. I spent most of my time figuring out how to store a vector. Fuck that get in the van, Eigen.

--- 

I never realized that the majority of learning C++ was fighting with the compiler. Dear god.

I've spent about an hour just trying to get everything to function properly in VSCode. I'm not competent, I'll never claim that, but I'm useful levels of stubborn. Everything should be good now.

Eigen's syntax is fairly straight forward once you get past the weird obsession open source developments have with trying to be Wikipedia. You're not her, stop trying to be. 

All distributions are sourced from the uniform distribution. If I set up the standard uniform, a.k.a. $\text{Unif}(0,1)$, then I get a random number generator of continuous values between 0 and 1. This can be used with a series of fun little algorithms to make pretty much any distribution imaginable. 

So I'd like to start with that. Generating 10 random numbers is simple enough thanks to Eigen. When I did something similar in C I had to spend a lot of time defining a vector. Worst of all I hadn't even succeeded in setting it as a dynamic vector. Eigen let's me use the syntax `VectorXd` to create a dynamic vector that I can fill with arbitrary quantities of data. 

```cpp title="rng.cpp"
// start of the program
int main() { // output is an integer
    // set a dynamic vector and fill with 10 random values
    VectorXd x = VectorXd::Random(10);
    // sending x to output
    std::cout << x << std::endl;
    // exit
    return 0;
}
```

```ansi title="sample output"
[31m -0.999984 [0m // that's
[31m -0.736924 [0m // sub-optimal
[32m 0.511211 [0m
[31m -0.0826997 [0m 
[32m 0.0655345 [0m
[31m -0.562082 [0m
[31m -0.905911 [0m
[32m 0.357729 [0m
[32m 0.358593 [0m
[32m 0.869386 [0m
```

This gives me a simple program that generates those 10 numbers. The problem is that they're bound between -1 and 1 by default. Apparently this is a natural feature of the C `rand()` function, which Eigen piggybacks off of here. If I want them to be bound between 0 and 1, as with all quantitative programming, I have to choose between beating it with better code or using *math* (dear god no).

The math is pretty easy here actually. A quick transformation to the random variable gets us our ideal distribution:

$$
X \sim \text{Unif}(-1,1)
$$

$$
Y = (X + 1)/2 \sim \text{Unif}(0,1)
$$

The rules of linear algebra are upheld better by C++ than most professors, as we can't just add a constant to a vector freely. I'll have to make my constant a vector of constants, then divide the result.

```cpp title="standard_unif.cpp"
// start of the program
int main() { // output is an integer
    // set a dynamic vector and fill with 10 random values
    VectorXd x = VectorXd::Random(10);
    // add by an equal dimension vector
    VectorXd y = (VectorXd::Ones(10) + x) * 0.5;
    // sending y to output
    std::cout << y << std::endl;
    // exit
    return 0;
}
```
```ansi title="output"
// much better
[32m 7.82637e-06 [0m
[32m 0.131538 [0m
[32m 0.755605 [0m
[32m 0.45865 [0m
[32m 0.532767 [0m
[32m 0.218959 [0m
[32m 0.0470446 [0m
[32m 0.678865 [0m
[32m 0.679296 [0m
[32m 0.934693 [0m
```

I'm pretty sure I heard somewhere that computer scientists prefer to specify functions rather than generalize them, to say on computational efficiency. But I'm a mathematician (Great Value brand, at least) so I stick to the mantra of "Be wise, generalize!".

Really it's just that I don't want to have to edit this function every time I use it, and it's not going to get better from here in terms of complexity. So I'll edit this program to let me pass any number as $n$ to generate $n$ many random numbers. 

```cpp title="standard_unif.cpp"
// start of the program
int main() { // output is an integer
    int n; // set n as an integer
    // check for incorrect input
    if (!(std::cin >> n) || n <= 0) return 1;
    // vector of n unif(-1,1) values
    VectorXd x = VectorXd::Random(n);
    // transform to unif(0,1)
    VectorXd y = (VectorXd::Ones(n) + x) * 0.5;
    // output y
    std::cout << y << "\n";
    // exit
    return 0;
}
```

When I run the program and pass `4` as the value of n the output *looks* correct.

```ansi title="standard uniform sample"
[32m 7.82637e-06 [0m
[32m 0.131538 [0m
[32m 0.755605 [0m
[32m 0.45865 [0m
```

But these are just the first 4 values of the previous list. This is a seeding problem I'm sure, but a little bit of digging helped me realize that's a big problem to address with my limited knowledge of cpp. So for today, I'll call it quits.

<br>