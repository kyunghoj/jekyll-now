## Recursive solution

The key to solving a problem recursively is to recognize that it can be broken down into a collection of smaller sub-problems, to each of which *that same general solving procedure that we are seeking applies*, and the total solution is then found in some *simple* way from those sub-problems' solutions. Each of thus created sub-problems being "smaller" guarantees that the base case(s) will eventually be reached. Thence, for the Towers of Hanoi: 

* label the pegs A, B, C,
* let *n* be the total number of disks,
* number the disks from 1 (smallest, topmost) to *n* (largest, bottom-most).

Assuming all *n* disks are distributed in valid arrangements among the pegs; assuming there are *m* top disks on a *source* peg, and all the rest of the disks are larger than *m*, so they can be safely ignored; to move *m* disks from a source peg to a *target* peg using a *spare* peg, without violating the rules: 

1. Move *(m - 1)* disks from the **source** to the **spare** peg, by *the same general solving procedure*. Rules are not violated, by assumption. This leaves the disk *m* as a top disk on the source peg. 
2. Move the disk *m* from the **source** to the **target** peg, which is guaranteed to be a valid move, by the assumptions -- *a simple step*.
3. Move the *(m - 1)* disks that we have just placed on the spare, from the **spare** to the **target** peg by *the same general solving procedure*, so they are placed on top of the disk *m* without violating the rules.
4. The base case being to move *0* disks (in steps 1 and 3), that is, do nothing -- which obviously doesn't violate the rules.

The full Tower of Hanoi solution then consists of moving *n* disks from the source peg A to the target peg C, using B as the spare peg.

