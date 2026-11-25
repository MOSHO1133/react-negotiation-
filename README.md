# ReAct Negotiation Demo + LLM-as-a-Judge Evaluation

Group 15 (Gulzareen, Muhammad Shees)
Implemented paper: P8 ReAct (Yao et al., ICLR 2023).
Judging protocol from the presented paper: P16 LLM-as-a-Judge (Zheng et al., NeurIPS 2023).

## What this does
A buyer negotiates with a seller who has a hidden price floor, over 20 synthetic cases
(14 where a deal is possible, 6 where it is not).
- **Fixed rule buyer:** offers 60% of budget, +8% of budget per round, max 8 rounds.
- **ReAct buyer:** small LLM (Qwen2.5-3B-Instruct) with Thought / Action / Observation steps
  and tools: calculator, offer, accept, walk_away.
- **Judging:** the same outcome pairs are rated by humans (blind, 2 raters) and by an LLM judge
  (Qwen2.5-7B-Instruct), in both A/B orders.

## Results
| | deals | deal rate (feasible) | avg surplus | format errors | constraint violations |
|---|---|---|---|---|---|
| fixed rule | 14 | 1.00 | 108.8 | 0 | 0 |
| ReAct v1 (first prompt) | 16 | 0.86 | 74.1 | 1 | 7 |
| ReAct v2 (fixed prompt) | 13 | 0.86 | 64.0 | 15 | 2 |

Judge: stability after swapping order = 1.00, agreement with humans = 0.80, kappa = 0.665.

## Findings
- The first ReAct prompt contained example numbers that the model copied. v2 fixed this.
- Small prompted models can break a hard constraint (budget) in their reasoning.
- The first judge prompt gave many unjustified ties; asking for the prices before the verdict fixed it.

## Limitations
20 cases, scripted seller, small models, prompts refined on the same 20 cases, judge sees
outcome summaries and not full traces.

## Files
- `notebook.ipynb` - full code and outputs (run in Google Colab with a T4 GPU)
- `cases.json` - the 20 cases
- `results/` - outcomes, judge verdicts, human labels
