# Panel Attack

## /ref

This engine is based off V3PdP engine by Brandon Lockaby - gbrandon at gmail dot com . There is lots of commentary which is worth keeping the codebase around in the repo.

## How to run the source code

1.	Install love v0.9 (http://love2d.org/)
2.	`cd panel-attack`
3.  `love ./src`

## How to run the web-server

`python script/server.py`

## Gameplay Mechanics

### How garbage blocks works

When a player makes a combo or chain, garbage is sent to the opponent.
Garbage blocks will be named Kb for a 1xK rod, or KRb for a block of K rows.
Rb rods made of metal are denoted M.

The amount of garbage is as follows:

4-combo: 3b
5-combo: 4b
6-combo: 5b
7-combo: Rb
8-combo: 3b + 4b
9-combo: 2 * 4b
10-combo: 2 * 5b
11-combo: 5b + Rb
12-combo: 2 * Rb
13-combo: 3 * Rb
14-to-19-combo: 4 * Rb
20-to-24-combo: 6 * Rb
K-chain: (K-1)Rb
K metal blocks in combo: (K-2)M in addition to normal combo garbage

For the purposes of this document, assume P1 is dropping garbage on P2.

The garbage is not dropped instantly once it is awarded.
It is, however, displayed above P2's
stack N frames after it is awarded.     #TODO: find N.

First, the garbage will wait for P1's chain to end.

Then all the garbage that was created during P1's chain will wait until
the most recent combo or chain (in this set of garbage)
was at least M frames ago.              #TODO: find M.

Then, all the garbage will wait until P2's chain to end.

At this time, after Q frames, the       #TODO: find Q.
garbage will fall into P2's stack.

A few things worth noting about the above process:
- It is possible to skip the second stage altogether if P1's
    chain is of any respectable length.
- Multiple chains worth of garbage may accumulate in the third
    stage if P1 makes short chains or unchained combos
    while P2 is making a long chain.

To find precise garbage timings I will probably have to use a debugger.

### Difference from Tetris Attack

In Tetris Attack, it seems like a panel cannot match in the same frame
that it does some other state transition.  This is implemented in panel
attack by performing the matches check before the other state transitions.
The result is very dirty...

In Tetris Attack, the rising of the stack is not smooth around displacement=0.
This part of the animation takes just as long regardless of speed.
This is most noticeable if you use L or R, the stack will sort of jitter on
its way up.  I have no plans to implement this.

In Tetris Attack, only one pair of panels can be swaping at any time, and
the new swap can only be created in some frames of the old swap.  In
panel-attack, swaps cannot occur on consecutive frames, but I think
this is less strict than the rule enforced by Tetris Attack about the
timing of back-to-back swaps.

In Tetris Attack, you get nothing for chains past 13.  I will probably
implement things for chains past 13.

The procedures for generating initial configurations of panels and clusters
of [!] blocks bear only a superficial resemblance to the ones employed in
Tetris Attack.  Many fewer initial configurations are posible in Tetris Attack,
and Tetris Attack's procedure for [!] blocks is based on how many
panels the player clears, while panel-attack's is not.



In Tetris Attack, a stack of garbage that should all begin falling at the same
time will occasionally separate.  panel-attack will not implement this bug.

In Panel de Pon (but not in Tetris Attack), a manual rise that is interrupted
by a match will finish after the match and rise lock end.  panel-attack
will not implement this bug.

In Panel de Pon (but not in Tetris Attack), a 32 combo gives too many points.
panel-attack will not implement this bug.
