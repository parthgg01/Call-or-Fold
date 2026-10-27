# Call-or-Fold
A No-Limit Texas Hold'em Engine Built from First Principles
Table of Contents
Overview
Key Features
System Architecture
Repository Structure
Getting Started
Mathematical Foundations
Verification and Correctness
Tech Stack
Team
Limitations and Notes
License
Overview

Call or Fold is a No-Limit Texas Hold'em poker engine implemented in Python, accompanied by a full technical reference that explains the rules, probability, game theory, and algorithms behind it.

The project is the final deliverable of a mentored learning program. The team first studied probability, combinatorics, expected value, and game theory, and then applied that knowledge to design, implement, and verify a complete poker engine. Every concept is introduced from first principles, followed by its formal mathematical derivation and an executable implementation.

Poker is a game of imperfect information: players act under both chance (which cards come next) and hidden information (which cards opponents hold). This project treats the game as a system of well-defined mathematical processes and shows how each one can be specified, implemented, and tested.

Key Features
Unbiased shuffling. Fisher-Yates permutation driven by a cryptographic random source, with rejection sampling to remove modulo bias.
Fast hand evaluation. Any 5, 6, or 7 card hand is reduced to a single integer score, so comparing two hands is one integer comparison. An exhaustive 21-subset evaluator serves as the reference, and a bitmask and lookup-table evaluator provides the fast path.
Equity estimation. Monte Carlo simulation with standard error reporting, rejection sampling against narrowed opponent ranges, and exact enumeration of turn and river outcomes.
Rules-accurate betting. Blind posting, the dead-button rule, turn order, minimum raise enforcement, and short all-in reopening logic.
Correct pot settlement. Uncalled bet refunds, horizontal main and side pot slicing, showdown evaluation, and odd-chip distribution.
Strategic decision policy. Chen hand scoring, push/fold decisions for short stacks, and postflop decisions based on range-conditioned equity and bet-pressure discounting. Includes an overview of counterfactual regret minimization (CFR).
Opponent modelling. Beta-Binomial tracking of opponent tendencies with a two-sigma significance gate, plus a post-hand self-review that never uses the cards of players who mucked.
System Architecture

The engine is divided into seven sequential processes. Each is documented in the report and implemented in the notebook.

#	Process	Responsibility
I	Card encoding and shuffling	Maps the 52 cards to integers 0 to 51 and produces a uniformly random deck order.
II	Hand evaluation	Converts any 5, 6, or 7 card hand into a strictly ordered integer score.
III	Equity and runouts	Estimates a hand's expected share of the pot by simulation or exact enumeration.
IV	Betting state machine	Advances blinds and the button, decides who acts next, and enforces raise rules.
V	Pot settlement	Refunds uncalled bets, builds main and side pots, and awards chips at showdown.
VI	Strategic decision policy	Selects actions using heuristics, equity, pot odds, and equilibrium tables.
VII	Opponent profiling and self-review	Learns opponent tendencies and reviews past decisions without hindsight bias.
Repository Structure
call-or-fold/
├── no_limit_holdem_engine_algorithms.ipynb   # Annotated notebook: theory, code, and checks
├── No-Limit-Hold_em.pdf                      # Technical reference report
└── README.md
File	Description
no_limit_holdem_engine_algorithms.ipynb	Executable reference that pairs each algorithm with its explanation, mathematics, and verification cells.
No-Limit-Hold_em.pdf	Written reference covering rules, hand rankings, derivations, algorithm specifications, and a worked end-to-end hand.
Getting Started
Requirements
Python 3.10 or newer
Jupyter Notebook, JupyterLab, or Google Colab

The code uses only the Python standard library, so no additional packages need to be installed.

Installation and Usage
bash
git clone https://github.com/YOUR-USERNAME/call-or-fold.git
cd call-or-fold
jupyter notebook no_limit_holdem_engine_algorithms.ipynb

Run the cells from top to bottom. Cells that use a seeded random generator are reproducible. Unseeded simulation cells may produce slightly different Monte Carlo estimates on each run, while exact enumeration results are deterministic.

To read the notebook without running it, open the .ipynb file directly on GitHub.

Mathematical Foundations

The project builds on the following ideas, each derived in the report.

Topic	Key idea
Combinatorics	Counting hands with binomial coefficients, for example C(52, 5) = 2,598,960 five-card hands.
Expected value	Probability-weighted payoff used to compare decisions.
Pot odds	A call is profitable when equity is at least C / (P + C).
Minimum defense frequency	The share of hands a defender must continue with so that bluffs cannot profit automatically.
Nash equilibrium	A strategy pair in which neither player gains by changing strategy alone.
Monte Carlo estimation	Averaging random trials, with standard error to measure uncertainty.
Bayesian updating	Beta-Binomial posteriors that adjust only when the evidence is statistically significant.
Regret minimization	Iteratively improving a strategy by tracking what each action would have earned.
Verification and Correctness

The report defines seven system invariants that a correct engine must always satisfy:

Chip conservation across stacks and the pot.
Non-negative stacks and consistent commitment bounds.
A bijective deck with no duplicate or missing cards.
Agreement between the fast and exhaustive evaluators.
Termination of every betting round in finite time.
Side-pot eligibility soundness.
Statistical gating of opponent adjustments and isolation of mucked cards.

The notebook includes assertion and test cells that check these properties, such as the partition of all five-card hands into their ten categories. Run the notebook to see the results for your own environment.

Tech Stack
Language: Python 3.10+ (standard library only)
Environment: Jupyter Notebook
Concepts: Monte Carlo simulation, bitmask and lookup-table optimization, combinatorics, game theory, Bayesian inference
Team

This project was developed under the guidance of a mentor by a team of five mentees.

Name	Role	Area of Work
Kshtij Jain	Mentor	Architecture, integration, system invariants, end-to-end walkthrough, and review
Parth Gupta	Mentee	Hand evaluation (Process II) and equity estimation (Process III)
Garvit Chaudhary	Mentee	Foundations, rules, and hand rankings (Sections 1 to 3), and card encoding and shuffling (Process I)
Divyanshi Dixit	Mentee	Betting state machine (Process IV) and pot settlement (Process V)
Aradhana Singh	Mentee	Strategic decision policy (Process VI)
Amogh Bansal	Mentee	Opponent profiling and self-review (Process VII), and invariant tests
Limitations and Notes
The notebook documents the reference implementation. Where the report describes more than the code actually does, the notebook marks the difference.
Performance figures quoted in the report depend on hardware and programming language. Measure them in your own environment before relying on them.
The strategy layer is a heuristic and equilibrium-table policy. It is not a full game-tree solver, and the CFR material is an introduction to how such solvers work.

This project is for learning and demonstration. It is not intended for real-money play.
License

Released under the MIT License.
