# COCOMO-81 Dataset — Column Reference

The COCOMO-81 dataset contains 63 real software projects used by Barry Boehm (1981)
to calibrate the Constructive Cost Model. The goal is to predict `actual` effort from
project size and 15 cost-driver multipliers.

---

## Identifier & Target

| Column   | Description |
|----------|-------------|
| `num`    | Project number (1–63) |
| `actual` | **Target variable** — actual development effort in **person-months** |

---

## Project Type

| Column     | Values | Description |
|------------|--------|-------------|
| `dev_mode` | `organic` / `semidetached` / `embedded` | The COCOMO development mode, which controls the baseline effort formula coefficients |

| Mode | Typical project |
|------|----------------|
| `organic` | Small team, familiar problem, relaxed constraints (e.g. scientific programs, simple data processing) |
| `semidetached` | Mixed experience, moderate constraints (e.g. transaction processing, OS utilities) |
| `embedded` | Tight hardware/software/operational constraints (e.g. flight control, real-time systems) |

---

## Size

| Column | Description |
|--------|-------------|
| `loc`  | Project size in **KLOC** (thousands of lines of code) — the primary size driver |

---

## Cost Driver Multipliers

Each of the 15 columns below is a **multiplier** applied to the baseline effort estimate.
A value of `1.0` is nominal (no adjustment). Values below `1.0` reduce effort; values
above `1.0` increase it. The typical range is **0.70 – 1.66**.

### Product Factors

| Column | Full Name | What it measures |
|--------|-----------|-----------------|
| `rely` | Required software reliability | How serious a software failure would be (low = easily recoverable, high = risk to life) |
| `data` | Database size | Ratio of database bytes to program KLOC — large databases increase complexity |
| `cplx` | Product complexity | Complexity of control flow, algorithms, device management, data management |

### Computer (Platform) Factors

| Column | Full Name | What it measures |
|--------|-----------|-----------------|
| `time` | Execution time constraint | % of available CPU time used at runtime — tight time budgets increase effort |
| `stor` | Main storage constraint | % of available memory used — tight memory constraints increase effort |
| `virt` | Virtual machine volatility | How frequently the underlying OS/hardware platform changes during development |
| `turn` | Computer turnaround time | How long developers wait for compile/test cycles — slow turnaround hurts productivity |

### Personnel Factors

| Column | Full Name | What it measures |
|--------|-----------|-----------------|
| `acap` | Analyst capability | Skill and efficiency of analysts (requirements, design) — **one of the strongest drivers** |
| `aexp` | Applications experience | Team's prior experience with this type of application |
| `pcap` | Programmer capability | Skill and efficiency of programmers — **one of the strongest drivers** |
| `vexp` | Virtual machine experience | Team's experience with the OS and hardware platform |
| `lexp` | Programming language experience | Team's experience with the implementation language |

### Project Factors

| Column | Full Name | What it measures |
|--------|-----------|-----------------|
| `modp` | Modern programming practices | Use of structured programming, design reviews, code walkthroughs |
| `tool` | Use of software tools | Maturity and capability of tools used (editors, debuggers, CASE tools) |
| `sced` | Required development schedule | How compressed or stretched the schedule is relative to nominal — both extremes increase cost |

---

## Multiplier Rating Scale

| Rating     | Multiplier (approx.) |
|------------|---------------------|
| Very Low   | 0.70 – 0.87         |
| Low        | 0.86 – 0.94         |
| Nominal    | 1.00                |
| High       | 1.06 – 1.15         |
| Very High  | 1.21 – 1.40         |
| Extra High | 1.56 – 1.66         |

> Personnel factors (`acap`, `pcap`) have the widest range (0.71–1.42), meaning team
> capability can cut effort nearly in half or increase it by 40% relative to an average team.

---

## The COCOMO Formula (for reference)

```
effort = a × (loc ^ b) × ∏(all 15 multipliers)
```

Where `a` and `b` are constants that depend on `dev_mode`:

| dev_mode     | a    | b    |
|--------------|------|------|
| organic      | 2.4  | 1.05 |
| semidetached | 3.0  | 1.12 |
| embedded     | 3.6  | 1.20 |
