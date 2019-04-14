---
layout: default
---

# Introduction

*SDP* is a theorem disprover for separation logic entailments. It can
disprove an entailment F |- G by proving that the corresponding
non-entailment F |/- G is satisfable, i.e., there exists a witness-model
for this non-entailment. For this purpose, *SDP* is equipped with a
proof system for non-entailment, which contains various inference rules
designed to prove the existence of the witness-model.

# Why need to disprove entailments?

During the verification of a program, a verifier often needs to handle
verification conditions appearing in the form of entailments. However,
proving separation logic entailments is known to be undecidable and most
of the existing techniques mainly focus on proving valid entailment.
Such proof is needed to confirm that a program satisfies its
specification.

However, these techniques often return an answer *unknown* when they
cannot prove an entailment. In this case, it is desired to examine
further if this *unknown* answer means the entailment is *invalid*. This
is important for the verifier to conclude whether a program violates its
specifications, i.e., there is a bug in the implementation of the
program. Also, when an entailment can be concluded invalid, its
counter-model can be used to generate failed test cases, which enable
programmers to understand and to fix the bug more effectively.

# How good SDP is?

We first experimented SDP with the task of disproving invalid
entailments collected from the separation logic competition
SL-COMP 2019. SDP can disprove all of these entailments with the average
proving time of 0.36s per entailment. It outperformed all
state-of-the-art separation logic provers, which can prove at most
71.4\% (137/192) of the benchmarks. The detailed results is presented in
the below table.

|---------------------------------|-------|-------|---------|---------|---------|---------|
| Category                        | #Ents | Slide | Asterix | ComSPEN | Cyclist | SDP     |
|---------------------------------|-------|-------|---------|---------|---------|---------|
| singly linked-list              |   134 | 0     | 126     |     126 |      24 | **134** |
| nested linked-list              |    16 | x     | x       |       x |       0 | **16**  |
| skip list                       |    12 | x     | x       |       x |       0 | **12**  |
| doubly linked-list              |    18 | 0     | x       |       0 |      14 | **18**  |
| tree                            |     1 | 0     | x       |       x |       1 | **1**   |
|---------------------------------|-------|-------|---------|---------|---------|---------|
| singly linked-list (arithmetic) |     3 | x     | x       |       3 |       x | **3**   |
| doubly linked-list (arithmetic) |     8 | x     | x       |       8 |       x | **8**   |
|---------------------------------|-------|-------|---------|---------|---------|---------|
| Total                           |   192 | 0     | 126     |     137 |      39 | **192** |
|---------------------------------|-------|-------|---------|---------|---------|---------|


We also experimented SDP with invalid entailments collected from the
verification of buggy programs. In particular, we collect the
entailments that an existing verifier Hip/Sleek could not prove (it
returns the answer *unknown*). Then, we manually inspect to select only
invalid entailments. These entailments contain not only arithmetic
constraints but also complicated data structures. Hence, most of them
cannot be disproved by existing separation logic provers. In contrast,
our prover SDP can efficiently disprove all of them in averagely 0.08
seconds per entailment. The detailed experiment of our prover SDP is
shown in the below table.


|--------------------|---------------------------------------|--------|-------|-------|---------------|----------------|
| Data Structures    | Algorithms                            | #Procs | #Bugs | #Ents | Proved by SDB | Time (seconds) |
|--------------------|---------------------------------------|--------|-------|-------|---------------|----------------|
| singly linked-list | length, insert, append, reverse, copy |     13 |    17 |    17 | **17**        |           0.07 |
| doubly linked-list | length, insert, append, reverse, copy |     15 |    19 |    23 | **23**        |           0.05 |
| sorted linked-list | insertion, merge, quicksort, bubble   |     12 |    13 |    26 | **26**        |           0.03 |
| tree               | size, height, flatten                 |      7 |     9 |    10 | **10**        |           0.22 |
| binary search tree | size, height, flatten, insert, delete |     13 |    15 |    21 | **21**        |           0.05 |
| avl tree           | size, height, flatten, merge          |     10 |    12 |    16 | **16**        |           0.14 |
|--------------------|---------------------------------------|--------|-------|-------|---------------|----------------|
| Total              | 26                                    |     70 |    85 |   113 | **113**       |           0.08 |
|--------------------|---------------------------------------|--------|-------|-------|---------------|----------------|

# Download

- Our prover SDB and the experimented entaiment benchmarks can be
  downloaded here: [SDB and its
  benchmarks](https://www.dropbox.com/s/bzsh70pm6n50oaf/prover-benchmarks.zip)

- The details of our experiment are reported in this PDF file:
  [experiment details](https://www.dropbox.com/s/i2n1jgswu6o9h3f/FM19-experiment.pdf)
