---
layout: default
---

# Introduction

*SDP* is an theorem disprover for separation logic entailments. At its
core, *SDP* is equipped with a proof system for non-entailment, which is
denoted as F |/- G. To prove a non-entailment, our proof system will
apply various inference rules to look for the existence of its
witness-model. If the non-entailment F |/- G can be proved, then *SDP*
can conclude that the entailment F |- G is invalid.

# Why needs to disprove entailments?

During the verification of a program, a verifier often needs to handle
verification conditions which appear in the form of entailments.
However, proving separation logic entailments is known to be undecidable
and most of the existing techniques mainly focus on proving that an
entailment is *valid*. Such proof is needed to confirm that a program
satisfies its specification.

However, these techniques often return an answer *unknown* when they
cannot prove an entailment. In this case, it is desired to examine
further if this *unknown* answer means the entailment is *invalid*. This
is important for the verifier to conclude whether the corresponding
program violates its specifications, i.e., there is a bug in the
implementation of the program. Also, when an entailment can be concluded
invalid, its counter-model can be used to generate failed test cases.
These test cases enable programmers to understand and to fix the bugs in
the program more effectively.

# How good SDP is?

We first experimented SDP with the task of disproving invalid
entailments collected from the separation logic competition
SL-COMP 2019. SDP can disprove all of these entailments (the average
proving time is 0.36s per entailment). It outperformed all other
state-of-the-art separation logic provers, which can prove at most
71.4\% (137/192) of the benchmarks. The detailed results are as follows,
where *\** indicates categories containing arithmetic constraints.

|---------------------|-------|-------|---------|---------|---------|-----|
| Category            | #Ents | Slide | Asterix | ComSPEN | Cyclist | SDP |
|---------------------|-------|-------|---------|---------|---------|-----|
| singly linked-list  |   134 | 0     | 126     |     126 |      24 | 134 |
| nested linked-list  |    16 | x     | x       |       x |       0 |  16 |
| skip list           |    12 | x     | x       |       x |       0 |  12 |
| doubly linked-list  |    18 | 0     | x       |       0 |      14 |  18 |
| tree                |     1 | 0     | x       |       x |       1 |   1 |
|---------------------|-------|-------|---------|---------|---------|-----|
| singly linked-list* |     3 | x     | x       |       3 |       x |   3 |
| doubly linked-list* |     8 | x     | x       |       8 |       x |   8 |
|---------------------|-------|-------|---------|---------|---------|-----|
| Total               |   192 | 0     | 126     |     137 |      39 | 192 |
|---------------------|-------|-------|---------|---------|---------|-----|


We also experimented SDP with invalid entailments collected by the
verification system Hip/Sleek when it verifies buggy programs. In
particular, we collect the entailments that this tool cannot prove,
i.e., it returns the answer *unknown*. Then we manually inspect them to
select only invalid entailments. These entailments contain not only
arithmetic constraints but also complicated data structures. Hence, most
of them cannot be handled by existing separation logic provers. In
contrast, our prover SDP can efficiently disprove all of them in
averagely 0.08 seconds per entailments. This result shows that our
prover SDP can be practically applied to improve the performance of
existing verification systems. The detailed experiment is shown in the
below table.


|--------------------|---------------------------------------|--------|-------|-------|-----|------|
| Data Structures    | Algorithms                            | #Procs | #Bugs | #Ents | SDB | Time |
|--------------------|---------------------------------------|--------|-------|-------|-----|------|
| singly linked-list | length, insert, append, reverse, copy |     13 |    17 |    17 |  17 | 0.07 |
| doubly linked-list | length, insert, append, reverse, copy |     15 |    19 |    23 |  23 | 0.05 |
| sorted linked-list | insertion, merge, quicksort, bubble   |     12 |    13 |    26 |  26 | 0.03 |
| tree               | size, height, flatten                 |      7 |     9 |    10 |  10 | 0.22 |
| binary search tree | size, height, flatten, insert, delete |     13 |    15 |    21 |  21 | 0.05 |
| avl tree           | size, height, flatten, merge          |     10 |    12 |    16 |  16 | 0.14 |
|--------------------|---------------------------------------|--------|-------|-------|-----|------|
| Total              | 26                                    |     70 |    85 |   113 | 113 | 0.08 |
|--------------------|---------------------------------------|--------|-------|-------|-----|------|

# Download

- Our prover SDB and the experimented entaiments benchmarks can be
  downloaded here: [SDB and its benchmarks](https://www.dropbox.com/s/l8wkbkjwum4mix9/prover-benchmarks.zip)

- The details of our experiment are reported in this PDF file:
  [experiment details](https://www.dropbox.com/s/i2n1jgswu6o9h3f/FM19-experiment.pdf)
