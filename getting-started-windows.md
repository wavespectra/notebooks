# Getting started with wavespectra on Windows

This guide takes you from a blank Windows machine to a running Jupyter notebook
with wavespectra installed, in about 15 minutes. You do **not** need anything
pre-installed — not even Python — and you do not need administrator rights.

Things have improved a lot since earlier versions: wavespectra no longer needs
a Fortran compiler, and ready-made packages are published for Windows, so
installation is now a single command.

---

## 1. Install Miniforge (this gives you Python + conda)

We will use **conda**, a package manager that installs Python itself along with
all scientific libraries. It is the closest thing Python has to the MATLAB
"everything included" experience, and it avoids all the problems with system
Python versions, PATH settings, and compilers.

> You may already have some Python on your system, but it is best to ignore it.
> Miniforge installs its own private Python and never touches anything else on
> your machine, so there is nothing to check or clean up first.

1. Download the installer: <https://conda-forge.org/download/>
   (choose **Windows x86_64**).
2. Run the installer. Accept the defaults — in particular:
   - Install for: **Just Me** (no admin rights needed)
   - Leave "Add Miniforge3 to my PATH" **unticked** (the default)
3. When it finishes, open the Start menu and look for **Miniforge Prompt**.
   This is a terminal window where conda is available. All commands below are
   typed into this window.

---

## 2. Create an environment for this project

An *environment* is a self-contained folder with its own Python and libraries —
think of it like a project-specific MATLAB installation. If anything ever goes
wrong, you delete the environment and start again in two minutes; nothing else
is affected.

In the Miniforge Prompt, type:

```
conda create -n waves python=3.12
```

Answer `y` when asked. Then activate it:

```
conda activate waves
```

The prompt changes from `(base)` to `(waves)` — that tells you which
environment is active. **You need to run `conda activate waves` every time you
open a new Miniforge Prompt** — this is the one step people forget.

---

## 3. Install wavespectra and Jupyter

With the `(waves)` environment active:

```
conda install wavespectra netcdf4 jupyterlab
```

This one command installs wavespectra (the latest release, with the new HP01
code), NetCDF file support, plotting libraries, and Jupyter. It may take a few
minutes. Answer `y` when asked.

To check it worked:

```
python -c "import wavespectra; print(wavespectra.__version__)"
```

This should print `4.7.0` or newer. (The new HP01 partitioning shipped in
4.6.0, so anything from there up is fine.)

---

## 4. Start Jupyter

First, move to the folder where you want to keep your notebooks, e.g.:

```
cd C:\Users\<your-username>\Documents\wavespectra-work
```

(create the folder in Windows Explorer first if it doesn't exist). Then:

```
jupyter lab
```

Your web browser opens with the Jupyter interface. JupyterLab is very similar
in spirit to the MATLAB desktop: a file browser on the left, and notebooks
(like MATLAB live scripts) as tabs. To open a starter notebook, download one
from this repository (e.g.
[hp01_quickstart.ipynb](https://github.com/wavespectra/notebooks/blob/master/hp01_quickstart.ipynb)),
save it into this folder and double-click it in the file browser.

To run a notebook cell, click on it and press **Shift+Enter** — same as
evaluating a section in a MATLAB live script.

When you are done, close the browser tab and press **Ctrl+C** in the Miniforge
Prompt to stop Jupyter.

---

## 5. Day-to-day: the only three commands you need

Once set up, a working session is just:

```
conda activate waves
cd C:\Users\<your-username>\Documents\wavespectra-work
jupyter lab
```

---

## Quick MATLAB → Python survival notes

| MATLAB | Python / wavespectra |
|---|---|
| Indexing starts at 1: `x(1)` | Starts at 0: `x[0]` |
| `struct` with named fields | `xarray.Dataset` — named variables with labelled dimensions (time, freq, dir) |
| `x(3,:)` — select by position | `dset.isel(time=2)` — select by position; `dset.sel(time="2014-12-01")` — select by label |
| Functions on data: `hs = calc_hs(spec)` | Methods attached to the data: `hs = dset.spec.hs()` |
| `%` comment | `#` comment |
| Code blocks by `end` | Code blocks by **indentation** — spacing is part of the syntax |
| `doc function` | `help(dset.spec.partition.hp01)` in a cell, or press **Shift+Tab** inside the parentheses |

Two more tips:

- Typing a variable name alone at the end of a cell prints it nicely — just
  like leaving off the semicolon in MATLAB.
- Full wavespectra documentation: <https://wavespectra.readthedocs.io/>

---

## If something goes wrong

- *"conda is not recognized"* — you are in a normal Command Prompt; open the
  **Miniforge Prompt** from the Start menu instead.
- *"No module named wavespectra"* — the environment is not active; run
  `conda activate waves` first.
- Environment beyond repair — delete and redo (takes two minutes):
  `conda deactivate`, `conda env remove -n waves`, then back to step 2.

---

## Appendix: pip alternative (not recommended here)

If for some reason conda cannot be used, the pure-Python route is: install
Python from <https://www.python.org/downloads/> (tick "Add python.exe to
PATH"), then in a Command Prompt:

```
python -m venv C:\Users\<your-username>\waves-env
C:\Users\<your-username>\waves-env\Scripts\activate
pip install "wavespectra[extra]" jupyterlab
```

Note the `[extra]` — with pip it is required to get NetCDF file support and
plotting. The conda route above includes these automatically, which is one of
the reasons we recommend it.
