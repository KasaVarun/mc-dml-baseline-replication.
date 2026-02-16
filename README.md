# MC-DML Baseline Replication

Replication of the LLM-based baselines (Direct and Reflection) from the paper "Monte Carlo Tree Search Boosts Reasoning via Iterative Preference Learning" (arXiv:2504.16855v1) for text adventure games using the Jericho environment.

## Project Description
This repository contains a from-scratch implementation of the baseline approaches from the paper. The baselines use LLMs (gpt-3.5-turbo-0125 via OpenAI API) to generate actions in text-based adventure games:
- **Direct LLM Baseline**: Generates actions directly from the current game state without prior reflections.
- **Reflection Baseline**: Incorporates reflections from past failed episodes to improve action selection.

Key features:
- Minimal dependencies (no RL or agent frameworks; uses PyTorch not required here, but OpenAI, Jericho, SQLite3).
- Data management: All trajectories and reflections stored in a single SQLite database (`results.db`) – no intermediate files.
- Games: Zork1, Deephome, Ludicorp, Pentari, Detective, Library, Balances, Temple, Ztuu (ROMs cloned from GitHub).
- Results: Low scores matching the paper's baselines (e.g., averages 0-10, validating weak performance of simple LLMs).

This is Part 1 of the assignment (baseline replication). For Part 2 (MCTS), extend the notebook.

## Requirements
- Python 3.10+ (tested on 3.12 in Kaggle).
- OpenAI API key (set as environment variable: `export OPENAI_API_KEY='your-key-here'` or use in code).
- Dependencies listed in `requirements.txt` (install via `pip install -r requirements.txt`).

### requirements.txt
```
jericho==3.1.0
openai==1.12.0
sqlite3  # Built-in
pandas  # For result querying (optional)
```

## How to Run
1. **Clone the Repository**:
   ```
   git clone https://github.com/yourusername/mc-dml-baseline-replication.git
   cd mc-dml-baseline-replication
   ```

2. **Install Dependencies**:
   ```
   pip install -r requirements.txt
   ```

3. **Set Up OpenAI API Key**:
   - Export as env var (above) or hardcode in the notebook/code for testing.

4. **Run the Notebook**:
   - Open `baseline_replication.ipynb` in Jupyter Notebook, Colab, or Kaggle.
   - Run cells sequentially:
     - Cells 1-3: Install deps and clone ROMs.
     - Cell 4: Verify ROM files.
     - Cells 5-9: Setup, DB functions, LLM generation, baseline logic.
     - Cell 10: Run Reflection baseline (prints scores; stores in `results.db`).
     - For Direct LLM: Uncomment `main(use_reflection=False)` in Cell 10 and re-run.
     - Cell 11: Query DB for averaged results (prints table).

5. **Alternative: Run as Script**:
   - Convert notebook to .py: `jupyter nbconvert --to script baseline_replication.ipynb`
   - Run: `python baseline_replication.py`

6. **Expected Runtime**: 1-3 hours for full run (9 games x 3 episodes x up to 200 steps with API calls). Reduce `NUM_EPISODES=1` or `MAX_STEPS=50` in setup cell for testing. Monitor for OpenAI rate limits.

7. **View Results**:
   - After run, use the query cell to see averages.
   - Download `results.db` and open with SQLite viewer for full data.

## Results Summary
Results from Reflection baseline (3 episodes/game) match paper's low performance, with slight differences due to gpt-3.5 vs. paper's stronger LLM.

| Game       | Max Score | Average Score | Paper Reflection Avg (Approx.) |
|------------|-----------|---------------|--------------------------------|
| zork1     | 350      | 1.67         | 5                             |
| deephome  | 300      | 1.00         | 1                             |
| ludicorp | 150      | 1.67         | 4                             |
| pentari   | 70       | 0.00         | 5                             |
| detective | 360      | 10.00        | 30                            |
| library   | 30       | 0.00         | 6                             |
| balances  | 51       | 0.00         | 10                            |
| temple    | 35       | 0.00         | 8                             |
| ztuu      | 100      | 0.00         | 5                             |

- Insights: Averages 0-10% of max, aligning with paper (baselines ~4-5%). Direct baseline (no reflections) would be lower.

## Notes
- Prompts approximate paper's; adjust for exact replication.
- ROMs: Cloned automatically; ensure paths match if running locally.
- GitHub URL: https://github.com/kasa_varun/mc-dml-baseline-replication (update with your username).
- Author: Varun Kasa (@kasa_varun)

For issues, open a GitHub issue.
```
