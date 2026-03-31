# Mesa Examples Exploration Notes

## Overview
I explored three examples from mesa-examples to understand the current state 
of the repository. Here are my findings.

---

## 1. conways_game_of_life_fast

**Status:** Partially broken

**Issue 1 - Visualization error:**
Running `python3 -m solara run app.py` throws:
```
ValueError: Unknown space type: NoneType
```
The PropertyLayer visualization is not yet implemented in SolaraViz.
This is a known issue: https://github.com/mesa/mesa/issues/2138
The README itself acknowledges this under "Future work".

**Issue 2 - Code does not match documentation:**
The README describes this as a PropertyLayer example, but the actual 
model.py uses a plain numpy array (`self.cell_layer_data = np.random.choice(...)`) 
instead of Mesa's PropertyLayer. The core claim of the example is misleading.

---

## 2. forest_fire (Jupyter Notebook)

**Status:** Broken

**Issue - Removed API:**
The Forest Fire Model notebook uses:
```python
from mesa.batchrunner import BatchRunner
```
But `BatchRunner` was removed in Mesa 3.x. Running the notebook throws:
```
ImportError: cannot import name 'BatchRunner' from 'mesa.batchrunner'
```
The notebook has not been updated to use the new `mesa.batch_run()` API.

---

## 3. bank_reserves

**Status:** Runs but has documentation issues

**Issue 1 - Outdated README:**
The README references files that no longer exist:
- Mentions `run.py` — file does not exist
- Mentions `server.py` — file does not exist
The model now uses `app.py` and Solara, but the README was never updated.

**Issue 2 - README describes old BatchRunner:**
The README says the model uses "BatchRunner" but the actual `batch_run.py` 
uses the new `mesa.batch_run()` function. Documentation is inconsistent 
with the code.

**Issue 3 - Poor default parameters:**
The default `init_people=2` means the model launches with only 2 agents, 
which makes it impossible to observe any meaningful behavior. 
The README suggests 25 people as a reasonable starting point.

---

## Summary of Problem Types Found

- **Broken examples** (crash on launch): conways_game_of_life_fast
- **Outdated API usage** (Mesa 3.x incompatibility): forest_fire notebook
- **Documentation mismatch** (README does not match code): bank_reserves, conways_game_of_life_fast
- **Poor defaults** (bad first-run experience): bank_reserves
