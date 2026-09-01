# Why prefer a cross-model opposite side

The default of one model producing work and another challenging it aims to reduce correlated blind
spots. It is a heuristic, not a correctness guarantee, and deterministic verification remains
primary.

The original policy drew on work showing limits of model self-correction and benefits from external
verification, including Huang et al. (ICLR 2024), Tyen et al. (ACL 2024), and Stechly and Kambhampati
(2024), plus later work on disagreement across model families. Aider's architect/editor split is a
practical example of separating reasoning and editing roles.

This evidence supports the cross-model allocation as a default. It does not justify overriding an
explicit user request, treating either model as an oracle, or replacing tests and other sound checks.
