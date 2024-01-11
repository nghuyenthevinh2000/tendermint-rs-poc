# Tendermint foundational knowledge

Note: https://timroughgarden.github.io/fob21/andy.pdf

## The setting
1. Communication is **partially synchronous**
2. Communication is point-to-point rather than "broadcast to all"
3. For the sake of simplicity, assuming that all processors have perfectly
synchronised clocks.
4. Time is divided into epochs. Each epochs will have a different **leader**. The leader for ith epoch is processor i mod n.
5. Quorum certificate (QC): n − f processors produce votes for a particular block. This certificate proves that a block has passed quorum. Majority needs to have QC before committing a block.


## The first simple model: one round of voting
1. A processor considers a block confirmed once they see a QC for that block, from block proposer
2. Epoch is of length $2\triangle$ with non-faulty leader i, broadcasting block at time t
3. At time $t + \triangle$, non - faulty processors each consider block from leader, produce vote for that block corresponding to epoch i. Block consideration and voting might > $\triangle$, leading to total time larger than epoch time $2\triangle$

Since we are in partially synchronous setting, consistency is not guaranteed. Block b might not yet be finalized, then block b' is proposed in new epoch. Block b' != b. Processors need to see QC in time before new epoch happens.

## The second model: two rounds of voting (Lock - Commit paradigm)
1. some processors produce QC for block B
2. some processors see QC on the block B produced in "stage 1" of epoch i, they lock on to that block B, waiting for majority to see QC, and start "stage 2".
3. processors will continue to broadcast QC to all. Received QC in "stage 2" will commit the block B.