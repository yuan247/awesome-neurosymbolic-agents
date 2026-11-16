# Contributing

An entry needs a symbolic object: a logical formula, a program, a proof, a PDDL operator, a path on a knowledge graph, a skill library, or a lemma library. A neural model has to propose it, translate it, imitate it, or update it.

Place it in one of the three sections already in the README:

1. Symbolic to model: a symbolic system supplies data or a teacher signal, and the knowledge goes into the parameters.
2. Model to symbolic: the model writes the symbolic object, and an external system runs it.
3. Joint training: the symbolic layer stays in the computation graph, or solver feedback enters training.

Systems from before 2024 are limited to ones that later work still calls. The link should open the paper.

## Format

```markdown
- [Paper title](https://arxiv.org/abs/XXXX.XXXXX) — One sentence on where the symbolic object sits and where learning or execution happens. Author et al., Year. [Code](https://github.com/org/repo)
```

- Use the paper's title.
- Prefer an arXiv or DOI link. Add a code link only when the repository exists.
- One sentence on where the symbolic object does the work.
- English prose. Keep the paper title in its original language.
- Use an existing section.

## Pull requests

1. One pull request covers one group of related papers.
2. Name the section, and say whether the solver is still present at inference.
3. Submit list text only. Do not commit paper PDFs or data dumps.
