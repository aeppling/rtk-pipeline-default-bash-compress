# default-bash-compress — benchmark results

SkillsBench through Harbor, Claude Code on claude-opus-4-8, 13 software-engineering tasks, 4 trials per arm per task, both arms in the same batch (2026-08-10). OFF is stock Claude Code; the RTK arm is Claude Code with the RTK v0.45.0 PreToolUse hook, which is exactly what this pipeline installs. Per-trial rows are in `results.tsv`.

## Cost

Mean cost per trial, trials that ended in a runtime error excluded. data-to-d3 is left out of the aggregates (its verifier never produced a score and its arms are unbalanced, 3 vs 4 cost trials); its rows stay in `results.tsv`.

| task | OFF | default-bash-compress | change | pass rate OFF → RTK |
|---|---|---|---|---|
| parallel-tfidf-search | $1.37 | $0.92 | -32% | 1.00 → 1.00 |
| react-performance-debugging | $1.22 | $0.97 | -21% | 0.50 → 0.00 |
| python-scala-translation | $1.22 | $1.01 | -17% | 0.00 → 0.00 |
| dialogue-parser | $0.37 | $0.32 | -12% | 0.75 → 0.79 |
| fix-build-google-auto | $2.11 | $1.91 | -10% | 1.00 → 1.00 |
| jax-computing-basics | $0.28 | $0.27 | -5% | 1.00 → 1.00 |
| spring-boot-jakarta-migration | $0.91 | $0.86 | -5% | no verifier score |
| debug-trl-grpo | $0.60 | $0.63 | +6% | 0.42 → 0.25 |
| simpo-code-reproduction | $0.98 | $1.04 | +7% | 0.00 → 0.00 |
| llm-prefix-cache-replay | $0.63 | $0.68 | +7% | 0.00 → 0.00 |
| azure-bgp-oscillation-route-leak | $0.49 | $0.54 | +11% | 0.00 → 0.00 |
| fix-visual-stability | $1.92 | $2.19 | +14% | 0.75 → 1.00 |
| **total (12 tasks)** | **$12.10** | **$11.35** | **-6.2%** | **0.49 → 0.46** (mean over the 11 scored tasks) |

- Per-task change: median -4.8%, mean -4.8%. Pooled over all trials with a permutation test: **−4.8%, p = 0.305**. 7 tasks cheaper with RTK, 5 costlier.
- Read it as cost-neutral to slightly cheaper. With 4 trials per arm the run-to-run spread of a single task is about 20%, larger than the per-task effects above, so only the pooled figure means anything.

## Quality

Scored tasks: 11. RTK higher on 2 (dialogue-parser, fix-visual-stability), lower on 2 (react-performance-debugging, debug-trl-grpo), tied on 7. Two tasks (spring-boot-jakarta-migration, data-to-d3) have no verifier score in this sandbox.

## What RTK did in the run

154 shell commands were rewritten to their rtk form across the 48 RTK trials: grep 63, find 82, ls 53, tree 11, git 18 (cat is not wrapped). Measured compression on those outputs: about 99 KB removed out of 366 KB, roughly 25K tokens, 0.08% of the RTK arm's 30.2M input tokens. The cost differences above are therefore mostly agent-path variance, not bytes removed: this hook saves little by itself and costs nothing.

The same data is discussed at https://www.rtk-ai.app/blog/rtk-on-skillsbench/.
