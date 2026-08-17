# Writing a Computer Science Research Paper

A practical, end-to-end guide: from finding a contribution to surviving the rebuttal.

---

## Table of Contents

1. [How CS publishing actually works](#1-how-cs-publishing-actually-works)
2. [Finding a contribution worth writing up](#2-finding-a-contribution-worth-writing-up)
3. [The one-sentence claim](#3-the-one-sentence-claim)
4. [Paper archetypes and their expected structure](#4-paper-archetypes-and-their-expected-structure)
5. [Writing order: never front to back](#5-writing-order-never-front-to-back)
6. [Section-by-section guide](#6-section-by-section-guide)
7. [Designing an evaluation that survives review](#7-designing-an-evaluation-that-survives-review)
8. [Figures and tables](#8-figures-and-tables)
9. [Math, notation, algorithms](#9-math-notation-algorithms)
10. [Sentence-level writing](#10-sentence-level-writing)
11. [The LaTeX toolchain](#11-the-latex-toolchain)
12. [Reproducibility and artifacts](#12-reproducibility-and-artifacts)
13. [Anonymization for double-blind review](#13-anonymization-for-double-blind-review)
14. [Reviews and rebuttals](#16-reviews-and-rebuttals)
15. [Rejection and resubmission](#17-rejection-and-resubmission)
16. [Ethics, authorship, and disclosure](#18-ethics-authorship-and-disclosure)

---

## 1. How CS publishing actually works

Computer science is unusual among academic fields: **top-tier conferences are the primary venue, not journals.** A paper at NeurIPS or CHI carries more weight than most journal articles. This has practical consequences for how you work.

**Hard deadlines.** Conferences have fixed submission dates, typically once or twice a year. You are not submitting when the work is ready; you are making the work ready for a date. Plan accordingly.

**Full peer review of the full paper.** Unlike fields where conferences take abstracts, CS conferences review complete papers, usually 8–14 pages plus references and appendices. Acceptance rates at top venues run roughly 15–25%.


### Knowing your venue tier

Not everything belongs at a flagship venue. The landscape roughly:

| Tier | Examples | When to aim here |
|---|---|---|
| Flagship conferences | SOSP/OSDI, SIGCOMM, NeurIPS/ICML/ICLR, POPL/PLDI, CCS/S&P/USENIX Security, CHI, SIGMOD/VLDB, ICSE, STOC/FOCS | Substantial, surprising, well-evaluated contribution |
| Workshops | Co-located with the above (HotOS, HotNets, and many others) | Early-stage ideas, position papers, 4–6 pages, fast feedback |
| Journals | TOCS, TON, JMLR, TSE, TOPLAS, PACMPL | Extended versions, long-form theory, or fields where journals dominate |

**Advice for a first paper:** a workshop is an excellent, low-risk place to start. Present the idea, get feedback from people who work on it, then build toward the full conference submission. 

### Reading the call for papers

Before writing a word, read the CFP for your target venue and note:

- Exact deadline **with timezone** (many use AoE — Anywhere on Earth)
- Whether there is a separate, earlier **abstract registration** deadline (very common; miss it and you cannot submit the paper)
- Page limit, and what counts toward it (references? appendices?)
- Required template and format
- Double-blind or single-blind
- Topics of interest and any explicit exclusions
- Artifact evaluation track and its deadline
- Ethics / human-subjects requirements

---

## 2. Finding a contribution worth writing up

A CS paper answers: **what did you make or discover that nobody had, and how do we know it works?**

Common shapes a contribution takes:

- **A new system** that achieves something previously impossible or impractical
- **A new algorithm** with better complexity, accuracy, or constants that matter in practice
- **A new technique** applied to a problem where nobody had applied it
- **A measurement or empirical study** revealing something the community assumed wrongly
- **A negative or replication result** showing a widely-believed result does not hold
- **A theoretical result**: a bound, an impossibility proof, a characterization
- **A dataset or benchmark** that unblocks work others could not do
- **A tool** that developers or researchers can actually use

### The gap-finding process

1. Pick 5–10 recent papers at your target venue on your topic.
2. Read each one's **Limitations / Future Work** section. Authors tell you exactly where the holes are.
3. Read the **Related Work** sections to build a mental map of who is doing what.
4. Look for: assumptions everyone makes that might be false, settings nobody has tested, techniques from an adjacent subfield that have not crossed over, results reported only on toy benchmarks.

### The sanity test

Before you invest months, write down the answers:

- **What is the problem?** In one sentence, for a non-specialist.
- **Why does it matter?** Who is hurt by it not being solved?
- **Why is it hard?** If it were easy, someone would have done it. What is the obstacle?
- **What is your insight?** The key idea that makes progress possible. This is the heart of the paper.
- **How will you know it worked?** Name the metric and the baseline *before* you run anything.

If you cannot answer "why is it hard," you probably do not have a paper — you have an engineering task.

---

## 3. The one-sentence claim

Write this sentence and tape it above your desk:

> We show that **[technique/system]** achieves **[measurable outcome]** on **[problem]**, which prior work could not because **[obstacle]**.

Examples of the shape:

- "We show that speculative decoding with a learned draft model reduces autoregressive inference latency by 2.4× with no change in output distribution."
- "We show that the standard assumption of independent failures does not hold in production datacenters, and that correlated failures increase expected data loss by an order of magnitude under common replication schemes."

Every section of the paper exists to support this sentence. If a paragraph does not, cut it.

**Also useful: the elevator pitch.** Three sentences — problem, insight, result. If you cannot deliver this to a colleague and see them nod, the paper is not ready.

---

## 4. Paper archetypes and their expected structure

Reviewers pattern-match against the norms of your subfield. Deviating without reason costs you.

### Systems paper (OS, networking, distributed, architecture)

```
1. Introduction
2. Background & Motivation      ← often includes a motivating measurement
3. Design                       ← the intellectual core
4. Implementation               ← language, LOC, what you had to change
5. Evaluation                   ← the largest section
6. Related Work
7. Conclusion
```

Systems reviewers want: a clear problem, an insight that is not obvious, an honest evaluation on real hardware with real workloads, and reported overheads. LOC counts and "we modified the Linux kernel in N places" are expected.

### Machine learning paper

```
1. Introduction
2. Related Work                 ← often early in ML
3. Method / Approach
4. Experiments                  ← setup, main results, ablations, analysis
5. Limitations
6. Conclusion
+ Appendix: hyperparameters, additional results, proofs
```

ML reviewers want: fair baselines with tuned hyperparameters, multiple seeds with variance reported, ablations isolating each component's contribution, and honesty about where the method fails. Many venues now require a reproducibility checklist and a limitations section.

### Theory paper

```
1. Introduction (incl. informal statement of results)
2. Preliminaries & Notation
3. Model / Problem Statement
4. Main Result (theorem + proof, or proof sketch with full proof in appendix)
5. Extensions / Lower Bounds
6. Open Problems
```

Theory reviewers want: precise definitions, correct proofs, clear positioning against known bounds, and intuition before formalism. Give the reader the proof idea in prose before the formal argument.

### Empirical software engineering / measurement paper

```
1. Introduction
2. Background
3. Research Questions            ← stated explicitly as RQ1, RQ2, ...
4. Methodology                   ← data collection, sampling, coding process
5. Results (organized by RQ)
6. Discussion / Implications
7. Threats to Validity           ← required; internal, external, construct
8. Related Work
9. Conclusion
```

### HCI / user study paper

```
1. Introduction
2. Related Work
3. System / Design (if applicable)
4. Study Design                  ← participants, tasks, conditions, measures
5. Results                       ← quantitative + qualitative
6. Discussion
7. Limitations
8. Conclusion
```

Requires IRB/ethics approval documentation, participant demographics, and appropriate statistical treatment.

**How to decide:** open three recently accepted papers from your exact target venue and copy the section structure. This is not lazy; it is meeting the reader's expectations.

---

## 5. Writing order: never front to back

Recommended order:

1. **Figures and tables first.** Sketch the plots that will constitute your results, even before you have the data. This forces you to decide what claim each experiment supports. If you cannot say what a figure proves, do not run the experiment.
2. **Method / Design.** You know this best; it is mostly description.
3. **Evaluation.** Setup, then results, driven by the figures you already made.
4. **Related Work.** Mechanical once you have read the literature.
5. **Introduction.** Now that you know what the paper actually says.
6. **Conclusion.** Short.
7. **Abstract.** Last.
8. **Title.** Also last, or at least revisit it last.

The introduction is the hardest section and gets the most reader attention. Writing it first means writing it about a paper that does not exist yet, then rewriting it entirely.

---

## 6. Section-by-section guide

### Title

Specific and informative beats clever. Reviewers and search engines both reward specificity.

- Weak: "A Novel Approach to Caching"
- Better: "Segcache: A Memory-Efficient and Scalable In-Memory Key-Value Cache for Small Objects"

Systems papers often name the system in the title. If your system has a name, use it. Avoid puns that obscure content; avoid "Towards..." unless the work really is preliminary.

### Abstract

150–250 words, six moves, roughly one to two sentences each:

1. **Context** — the setting the reader should have in mind.
2. **Problem** — what goes wrong, specifically.
3. **Gap** — why existing approaches do not solve it.
4. **What you did** — the approach, named.
5. **Key result** — with numbers. "Reduces tail latency by 3.1×" not "significantly improves performance."
6. **Implication** — why it matters beyond your paper.

No citations. No abbreviations you have not expanded. No forward references to sections. Write it last, then cut 20%.

### Introduction

The single most important section. Many reviewers form an opinion here and spend the rest of the paper confirming it. A reliable structure — sometimes called the "five-paragraph intro":

**¶1 — The world and why it matters.** Establish the domain and stakes. Concrete, not grand. Avoid "In recent years, X has become increasingly important."

**¶2 — The specific problem.** Narrow to the exact difficulty. Ideally include a concrete number, example, or observation that makes it vivid.

**¶3 — Why prior work is insufficient.** Not a literature review — a short account of the *classes* of existing approaches and the fundamental reason each falls short. Be fair; the authors of that prior work are likely your reviewers.

**¶4 — Your insight and approach.** The key idea, stated plainly, then the system/method that embodies it. This paragraph should contain the sentence a reader would quote when explaining your paper to someone else.

**¶5 — Results and contributions.** Headline numbers, then an explicit bulleted contributions list.

A contributions list is conventional and worth including:

```markdown
This paper makes the following contributions:

- We characterize [phenomenon] across [scale/scope], showing that [finding] (§3).
- We design [system], which [key mechanism] to achieve [property] (§4).
- We implement [system] in [N lines of Rust] on top of [platform] (§5).
- We show that [system] achieves [number] on [workloads], outperforming
  [baseline] by [factor], while [important caveat/cost] (§6).
```

Each bullet should be a claim you can defend, with a section pointer. Do not list "we survey related work" as a contribution.

**A useful test:** a reviewer who reads only your abstract, introduction, and figures should be able to write a fair summary of your paper. Many effectively do exactly this.

### Background / Motivation

Include only what the reader needs to understand *your* contribution. This is not a textbook chapter.

A motivating measurement here is powerful in systems and measurement papers: show data demonstrating the problem is real and quantitatively serious, before you propose anything. This converts the reader from "why should I care" to "how would I fix this."

### Related Work: placement and treatment

**Placement.** Two conventions:

- **Early (§2)** — when readers need the landscape to understand your contribution. Standard in ML and theory.
- **Late (before conclusion)** — when your contribution must be understood first for comparisons to make sense. Standard in systems.

**Treatment.** The fatal mistake is the annotated bibliography: "Smith et al. did X. Jones et al. did Y. Lee et al. did Z." This tells the reader nothing.

Instead, organize by *theme or approach*, and in each cluster state the shared limitation and how you differ:

> **Weak:** "Chen et al. [12] propose a learned index. Wang et al. [31] extend this to updates. Kim et al. [19] add concurrency support."
>
> **Better:** "Learned indexes [12, 31, 19] replace tree traversal with model inference, trading index size for lookup cost. All assume a static or slowly-changing key distribution, and degrade sharply under adversarial insertion patterns (§6.4). Our approach retains the space benefit while bounding worst-case lookup cost."

**Be generous.** Reviewers are often the authors of the work you cite. Mischaracterizing prior work — or omitting an obvious competitor — is the fastest route to rejection. If a paper is close to yours, cite it prominently and explain the difference clearly rather than burying it.

**Concurrent work.** If something very similar appeared within roughly the last few months, cite it as concurrent and independent. Hiding it looks worse than the overlap.

### Method / Design

Structure: **overview → components → details.**

Start with a system diagram and a paragraph walking through it end-to-end. Then take each component in turn. The reader should always know where they are in the architecture.

Explain **why**, not just **what**. "We use a two-level index" is description. "We use a two-level index because a single level would require 40 GB of metadata, exceeding the memory budget of our target deployment" is design rationale — and it is what distinguishes a paper from documentation.

Discuss alternatives you rejected and why. This preempts the reviewer's "why didn't you just...?" and demonstrates you understand the design space.

State assumptions and the threat model / system model explicitly. Unstated assumptions are the most common source of "the evaluation doesn't support the claims" reviews.

### Implementation

Short but concrete. Language, lines of code, what you built on, what you had to modify, engineering obstacles that were genuinely nontrivial. This section builds credibility that the work is real.

### Evaluation

See §7 — this deserves its own treatment.

### Discussion / Limitations

Naming your limitations makes you more credible, not less. Reviewers will find them regardless; the question is whether you found them first.

Distinguish:

- **Limitations of the approach** — fundamental (your method assumes X; when X fails, so does the method).
- **Limitations of the evaluation** — contingent (you tested on 3 workloads; generalization to others is untested).

Do not use this section to smuggle in excuses. State the limitation cleanly and, where possible, say what would be needed to address it.

Empirical SE and HCI papers use a **Threats to Validity** section instead, conventionally split into internal (does your setup actually measure what you claim), external (does it generalize), and construct (are your metrics the right proxies for the concept) validity.

### Conclusion

Short — one paragraph is often enough. Restate the contribution and result, then point to what it opens up. Do not introduce new information. Do not merely copy the abstract.

### References

Use a citation manager (Zotero, JabRef, Mendeley) from day one. For CS specifically, pull BibTeX from **DBLP** — it is the most consistently correct source for CS venues.

Common problems:

- Citing the arXiv preprint when the paper was published at a peer-reviewed venue. Cite the published version.
- Inconsistent venue naming ("Proc. of the 30th SOSP" vs "SOSP '25").
- Lowercased proper nouns in titles. Protect with braces: `title = {A Study of {Byzantine} Fault Tolerance in {Rust}}`.
- Missing years, page numbers, DOIs.

### Appendix

Use for proofs, hyperparameter tables, additional experiments, survey instruments. Reviewers are generally not obligated to read it — so nothing load-bearing goes there. If a claim in the main text depends on it, the main text must summarize the essential content.

---

## 7. Designing an evaluation that survives review

More CS papers are rejected for weak evaluation than for weak ideas.

### Start from the claims

List every claim in your introduction. For each, name the experiment that proves it. Then structure the evaluation section around those questions, stated explicitly:

```markdown
Our evaluation answers the following questions:

- **Q1:** Does [system] reduce end-to-end latency on realistic workloads? (§6.2)
- **Q2:** How does it scale with core count and dataset size? (§6.3)
- **Q3:** What is the contribution of each design component? (§6.4)
- **Q4:** What is the overhead in memory and CPU? (§6.5)
- **Q5:** When does it fail? (§6.6)
```

Q5 matters more than people expect. A paper that characterizes its own failure regime reads as honest science; one that reports only wins reads as marketing.

### Baselines

The single most common reviewer complaint is an unfair or missing baseline.

- Compare against the **strongest** prior method, not the easiest to implement.
- **Tune the baseline** with the same effort you tuned your method. Reviewers can tell when you did not.
- Include a naive baseline too, so readers can calibrate the scale of improvement.
- If you cannot run a competitor (no code, different platform), say so explicitly and compare against numbers reported in their paper, noting the caveat.

### Workloads and datasets

- Real workloads and standard benchmarks beat synthetic ones. Use synthetic only to isolate a specific variable, and say so.
- Justify your choices. Why these datasets? Reviewers assume unstated selection means cherry-picking.
- Report dataset sizes, characteristics, preprocessing, and splits.

### Ablations

For every component you claim contributes, remove it and measure. A design with five mechanisms and no ablation invites "how do we know the gains don't come entirely from mechanism 2?"

### Statistical hygiene

- **Multiple runs.** A single run is not a result. Report mean with standard deviation or confidence intervals, and state the number of trials.
- **Multiple seeds** for anything stochastic (ML especially). Reporting the best seed is misconduct-adjacent; report the distribution.
- **Percentiles for latency**, not just means. p50, p99, p99.9. Tail latency is often the whole story.
- **Error bars on every plot** where variance exists. Say in the caption what they represent.
- If you run significance tests, name the test, check its assumptions, and correct for multiple comparisons if you ran many.

### Reporting your setup

Enough for someone else to reproduce it:

- Hardware: CPU model, core count, RAM, storage type, NIC, GPU model and count
- Software: OS and kernel version, compiler and flags, library versions, framework versions
- Configuration: all parameters, and how they were chosen
- For ML: hyperparameters, search procedure, compute budget, training time

### Honesty rules

- Report where you lose, not just where you win.
- Report overheads — memory, CPU, energy, storage, latency added.
- Do not use a log scale to hide small differences, or a truncated y-axis to exaggerate them.
- Do not report a geometric mean speedup that is driven entirely by one outlier benchmark without saying so.

---

## 8. Figures and tables

Reviewers look at figures before they read text. Treat them as first-class.

**Every figure needs a self-contained caption.** A reader flipping through should understand the figure without the body text. Captions go *below* figures, *above* tables, by convention.

**Make the takeaway explicit.** Either in the caption ("Throughput scales linearly to 32 cores, then plateaus due to lock contention") or as a bolded lead-in sentence in the text. Never leave the reader to infer what a plot is for.

**Practical rules:**

- Use vector formats (PDF/SVG), never bitmap screenshots of plots.
- Font size in figures should match or exceed body text size. Test by printing at 100%.
- Design for grayscale and for color-vision deficiency: use distinguishable line styles and markers in addition to color. Viridis-family colormaps are safe defaults.
- Label axes with units. Always.
- Do not use 3D bar charts, pie charts, or chartjunk.
- Keep the same color for the same system across all figures in the paper.
- Tables: use `booktabs` in LaTeX (`\toprule`, `\midrule`, `\bottomrule`); avoid vertical rules. Right-align numbers, fix decimal places, bold the best result per row or column.

**A useful discipline:** if a figure and a paragraph say the same thing, delete the paragraph, or cut the figure. Page limits are tight.

---

## 9. Math, notation, algorithms

- **Define every symbol at first use.** If you have more than ~10 symbols, add a notation table.
- **Be consistent.** If $n$ is the number of nodes, it is always the number of nodes.
- **Prose before formalism.** Give the intuition, then the theorem. A reader who understands the idea can follow the proof; the reverse rarely works.
- **Number equations you refer to**, and only those.
- **Theorem environments:** state the theorem formally, and if the proof is long, give a proof sketch in the body with the full proof in an appendix.
- **Algorithms:** use `algorithm2e` or `algorithmicx`. Include a line-by-line explanation in prose. State complexity explicitly.
- Punctuate equations as part of the sentence — they are grammatical objects, and displayed equations usually end in a comma or period.

---

## 10. Sentence-level writing

**Tense:**
- Prior work and established facts: present ("Raft provides a leader-based consensus protocol").
- What you did: past ("We implemented the prototype in 4,200 lines of Go").
- What the paper does: present ("Section 4 describes the design").
- Results as ongoing facts: present ("The system sustains 1.2M ops/s").

**Voice:** "We" is standard and preferred in CS. Passive voice for methods ("Experiments were conducted") is acceptable but usually flabbier. Never "the authors."

**Sentences:**

- Put the subject and verb near the front. Long preambles bury the point.
- Prefer verbs to nominalizations: "we evaluate" not "we perform an evaluation of."
- Cut intensifiers with no content: *very, quite, really, extremely, significantly* (unless statistically), *novel, interesting, obviously, clearly, simply, of course*.
- Cut "in order to" → "to". Cut "due to the fact that" → "because". Cut "it is worth noting that" entirely.
- One idea per paragraph. Start with a topic sentence stating that idea.

**Precision:**

- Every quantitative claim gets a number. "Much faster" means nothing; "2.4× faster" means something.
- Say "reduces p99 latency from 40 ms to 12 ms," not "improves performance."
- Avoid "novel," "state-of-the-art," and "significantly outperforms" as unsupported adjectives. Show it instead.

**Consistency:** pick one term per concept and never vary it for stylistic variety. If it is a "shard," it is never a "partition," a "segment," or a "chunk." Elegant variation is good in prose and poison in technical writing.

**Cross-references:** use `\cref` from the `cleveref` package so you write `\cref{fig:throughput}` and it renders "Figure 3" automatically and consistently.

**Read it aloud.** Or use text-to-speech. You will hear problems your eye skips over.

---

## 11. The LaTeX toolchain

CS papers are written in LaTeX. Word is accepted at some venues; you will fight it.

**Editors:** Overleaf is the standard for collaboration (real-time editing, version history, template gallery). Locally: TeX Live plus VS Code with the LaTeX Workshop extension, and `latexmk` for builds. If you work locally, use Git — and set `--word-diff` or a `.gitattributes` with a `tex` diff driver so diffs are readable.

**Templates:** download from the venue, never from a third party. `acmart` for ACM venues (with the right `\documentclass` option — `sigconf`, `acmsmall`, etc.), `IEEEtran` for IEEE, plus venue-specific styles for USENIX, NeurIPS, ICML, and others. Do not modify margins, font size, or spacing to fit — this is checked and is grounds for desk rejection.

**Packages worth using:**

| Package | Purpose |
|---|---|
| `booktabs` | Professional tables |
| `cleveref` | Consistent, automatic cross-references |
| `siunitx` | Correctly formatted numbers and units |
| `algorithm2e` | Algorithm pseudocode |
| `listings` or `minted` | Source code |
| `pgfplots` / `tikz` | Plots and diagrams generated from LaTeX |
| `microtype` | Subtle typographic improvements; genuinely helps fit |
| `subcaption` | Subfigures |
| `hyperref` | Clickable links and PDF metadata (load last, usually) |
| `xcolor` + `todonotes` | Draft comments and margin notes |

**Bibliography:** BibTeX or, preferably, BibLaTeX + Biber. Pull entries from DBLP. Keep one `.bib` file. Use consistent key naming (`author-year-shortword`).

**Workflow habits:**

- **One sentence per line** in your `.tex` source. This makes version-control diffs meaningful.
- Keep figures in a `figures/` directory, generated by scripts committed alongside — so a data change regenerates the plot.
- Use `\newcommand` for repeated terms and system names: `\newcommand{\sysname}{Segcache\xspace}`. Renaming your system later becomes one edit.
- Comment out rather than delete during revisions, until the deadline passes.

**Fitting the page limit:** first cut content, not spacing. Then: tighten prose, shrink figures, move material to appendix, use `\vspace` sparingly, enable `microtype`. Do not shrink the bibliography font or margins — reviewers and chairs notice.

---

## 12. Reproducibility and artifacts

Many venues now run **Artifact Evaluation** — a separate committee checks whether your code runs and reproduces your claims, awarding badges (Available, Functional, Reproduced). It is usually optional but increasingly expected, and it materially helps your paper's citation count and credibility.

**Prepare from the start, not after acceptance:**

- Keep code in version control with a tagged commit corresponding to the submitted results.
- Pin dependencies exactly (`requirements.txt` with versions, `Cargo.lock`, container image).
- Ship a container (Docker/Podman) or a Nix/Guix environment.
- Write a `README` with: system requirements, install steps, a smoke test that runs in minutes, and per-claim instructions mapping paper figures to commands.
- Include the scripts that generate every figure from raw data, and the raw data if licensing permits.
- Deposit for permanence: Zenodo or figshare gives you a DOI; a GitHub URL alone is not archival.
- Document hardware requirements honestly. If a result needs 8×A100 for a week, say so and provide a scaled-down version reviewers can actually run.

**Anonymity:** during double-blind review, host artifacts anonymously (anonymous GitHub proxies, or an anonymized Zenodo deposit).

---

## 13. Anonymization for double-blind review

Get this wrong and the paper is desk-rejected.

- Remove author names, affiliations, acknowledgments, and funding statements.
- Cite your own prior work in **third person**: "Prior work [7] showed..." — not "In our prior work [7], we showed..." Do **not** anonymize the citation itself to "[Anonymous]"; that removes information reviewers need and is usually not required.
- Strip identifying names from the system, dataset, and URLs, or use an anonymizing proxy.
- Check PDF metadata (`pdfinfo` will show author fields from your editor).
- Check figure files for embedded usernames or paths; check LaTeX comments; check code screenshots for directory paths like `/home/jsmith/`.
- Avoid "our company's production cluster" if that identifies the company — describe it generically and note the constraint.
- Check the venue's policy on preprints. Some allow arXiv posting before review; some restrict publicity during review. Read the CFP.

---


## 14. Reviews and rebuttals

### Reading reviews

Wait 24 hours before responding to anything. The first reaction to a harsh review is never the useful one.

Then sort each point into:

1. **Factual errors by the reviewer** — they missed something that is in the paper. Correct politely with a section pointer, and note that this means the paper failed to communicate it clearly.
2. **Fair criticism you can address in the rebuttal** — a missing experiment you can run in the window, a clarification you can give.
3. **Fair criticism you cannot address now** — acknowledge honestly and commit to the camera-ready or future work.
4. **Requests out of scope** — explain why, respectfully, in terms of the paper's stated goals.

If three reviewers independently misunderstood the same thing, the reviewers are not the problem. Your writing is.

### Writing the rebuttal

Space is tight (often 500–1000 words). Priorities:

- **Lead with the most consequential objection**, not the easiest one.
- **Address common concerns once**, in a shared section, then respond per-reviewer.
- **Give data.** "We ran the requested comparison against X; our method achieves 1.8× lower latency (numbers in table below)" is worth ten paragraphs of argument.
- **Concede gracefully where you should.** "R2 is right that our claim about generalization overreaches; we will restrict it to the tested regime." Reviewers respond well to authors who update.
- **Be specific about commitments.** "We will add §6.7 reporting memory overhead" beats "we will clarify this."
- **Never be sarcastic, defensive, or condescending.** It has never once helped.
- Do not promise experiments you cannot deliver by camera-ready.

### If accepted

- Address every reviewer request in the camera-ready, or explain in the response why not.
- De-anonymize, add acknowledgments and funding.
- Release the artifact.
- Prepare the talk and, increasingly, a short video and a poster.

---

## 15. Rejection and resubmission

Most papers at top venues are rejected. Rejection at a 20%-acceptance venue is the modal outcome, not a verdict on you.

**A working process:**

1. Wait a week.
2. Extract every substantive criticism into a list, stripped of tone.
3. Sort into: things you can fix by writing, things you can fix by experiments, things that are fundamental.
4. If the fundamental list is empty, revise and resubmit — often to the next venue in the same tier, or one tier down.
5. If it is not empty, decide whether to do the work or reframe the contribution.

**Do not** resubmit unchanged to the next deadline. Reviewer pools overlap, some venues share review data, and it wastes everyone's time including yours.

**Do** keep a document mapping each reviewer criticism to what you changed. If you resubmit to a venue with a revision-tracking process, you will need it; and it keeps you honest.

---

## 16. Ethics, authorship, and disclosure

- **Human subjects** require IRB or equivalent approval *before* data collection. Retroactive approval generally does not exist.
- **Security research** requires responsible disclosure: notify affected vendors, respect embargo periods, and describe your disclosure process in the paper. Many security venues require an ethics section.
- **Data** must be used within its license and terms of service. Scraping in violation of ToS is an ethics-committee issue at several venues now.
- **Dual use:** consider and discuss potential misuse where relevant. Some venues require a broader impacts statement.
- **Authorship** should reflect intellectual contribution. Discuss order early, in writing, before the crunch. CS conventions vary: many subfields use contribution order with the advisor last; some use alphabetical (common in theory).
- **Plagiarism and self-plagiarism.** Reusing your own text from a prior paper without citation is a real violation at most venues. Simultaneous submission to two venues is prohibited essentially everywhere.
- **LLM use** is now governed by explicit venue policies. Most allow AI assistance for writing and editing but require that authors take full responsibility for all content, and prohibit listing an AI as an author. Some require disclosure. Read your venue's policy — they change yearly.

---


## The shortest version

If you remember nothing else:

1. Have one clear claim, and know what evidence supports it before you run anything.
2. Write the figures first and the introduction last.
3. Tune your baselines as hard as you tuned your method.
4. Report where you lose.
5. Give the reader the insight in plain language before the formalism.
6. Have someone outside the project read it two weeks before the deadline.
7. Submit a day early.
