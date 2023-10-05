- Always accompany if or switch statements with else/default statements such that all paths are implemented and the circuit is combinatorial.

- If you have a loop waiting upon a condition you should add a delay such that it frees up other threads in the simulation so that they can perform computation s.t. the condition can be satisfied, rather than causing the simulation to hang.
- **Parallelism**: Use *fork* and *join* for a parallel block. *begin* and *end* are used for sequential blocks.

$display - used to print messages - useful for testing.