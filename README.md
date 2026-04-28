# 🧠 ipykernel + Conda + PyCharm — Complete Guide

> A comprehensive reference for managing Jupyter kernels across conda environments and using them in PyCharm.
> Perfect for ML/DL practitioners who work with multiple conda environments.

---

## 📋 Table of Contents

- [Prerequisites](#prerequisites)
- [Installing ipykernel](#installing-ipykernel)
- [Creating Custom Kernels](#creating-custom-kernels)
- [Listing Kernels](#listing-kernels)
- [Removing Kernels](#removing-kernels)
- [Restoring Kernels](#restoring-kernels)
- [Using Kernels in PyCharm](#using-kernels-in-pycharm)
- [Using Kernels in Jupyter Notebook/Lab](#using-kernels-in-jupyter-notebooklab)
- [Managing Multiple Environments](#managing-multiple-environments)
- [Troubleshooting](#troubleshooting)
- [Quick Reference Cheatsheet](#quick-reference-cheatsheet)

---

## Prerequisites

- [Anaconda](https://www.anaconda.com/download) or [Miniconda](https://docs.conda.io/en/latest/miniconda.html) installed
- At least one conda environment created
- PyCharm (Professional or Community) installed

### Create a conda environment (if not done yet)
```bash
conda create -n my_env python=3.10
conda activate my_env
```

---

## Installing ipykernel

`ipykernel` must be installed **inside each conda environment** you want to use as a kernel.

### Install in current active environment
```bash
conda activate my_env
conda install ipykernel
```

### Install via pip (alternative)
```bash
conda activate my_env
pip install ipykernel
```

### Install in a specific environment without activating
```bash
conda install -n my_env ipykernel
```

---

## Creating Custom Kernels

After installing `ipykernel`, register the environment as a Jupyter kernel.

### Basic kernel registration
```bash
conda activate my_env
python -m ipykernel install --user --name my_env --display-name "My Environment"
```

### Parameters explained
| Parameter | Description |
|---|---|
| `--user` | Installs kernel for current user only (recommended) |
| `--name` | Internal kernel name (no spaces, used in commands) |
| `--display-name` | Human-readable name shown in PyCharm/Jupyter UI |

### Real-world examples
```bash
# ML environment
conda activate ML_Unsupervised
python -m ipykernel install --user --name ML_Unsupervised --display-name "Conda (ML Unsupervised)"

# Deep Learning environment
conda activate DL_Architecture_Generative
python -m ipykernel install --user --name DL_Generative --display-name "Conda (DL Generative)"

# Data Engineering environment
conda activate data_eng
python -m ipykernel install --user --name data_eng --display-name "Conda (Data Engineering)"
```

> ✅ Registered kernels are **permanent** — they survive terminal closes, system restarts, and PyCharm restarts.

---

## Listing Kernels

### List all registered kernels
```bash
jupyter kernelspec list
```

### Example output
```
Available kernels:
  ml_unsupervised    C:\Users\username\AppData\Roaming\jupyter\kernels\ml_unsupervised
  dl_generative      C:\Users\username\AppData\Roaming\jupyter\kernels\dl_generative
  python3            C:\Users\username\anaconda3\share\jupyter\kernels\python3
```

### Kernel storage locations
| OS | Default kernel path |
|---|---|
| Windows | `C:\Users\username\AppData\Roaming\jupyter\kernels\` |
| macOS | `~/Library/Jupyter/kernels/` |
| Linux | `~/.local/share/jupyter/kernels/` |

---

## Removing Kernels

### Remove a specific kernel
```bash
jupyter kernelspec remove kernel_name
```

### Remove multiple kernels at once
```bash
jupyter kernelspec remove kernel1 kernel2 kernel3
```

### Real-world examples
```bash
# Remove default python3 kernel
jupyter kernelspec remove python3

# Remove a custom kernel
jupyter kernelspec remove ml_unsupervised

# Remove multiple at once
jupyter kernelspec remove python3 dl_generative
```

> ⚠️ You will be prompted `[y/N]` — type `y` to confirm each removal.

> 💡 Removing a kernel does **not** delete the conda environment — only its Jupyter registration.

---

## Restoring Kernels

Removed kernels can always be re-registered with the same command used to create them.

### Restore python3 kernel
```bash
conda activate my_env
python -m ipykernel install --user --name python3 --display-name "Python 3"
```

### Restore any custom kernel
```bash
conda activate ML_Unsupervised
python -m ipykernel install --user --name ML_Unsupervised --display-name "Conda (ML Unsupervised)"
```

---

## Using Kernels in PyCharm

### Step 1 — Set the Python Interpreter

1. Go to **File → Settings** (`Ctrl+Alt+S`)
2. Navigate to **Project → Python Interpreter**
3. Click the dropdown → **Add Interpreter → Add Local Interpreter**
4. Choose **System Interpreter**
5. Browse to your conda env's `python.exe`:
   ```
   # Windows
   C:\Users\username\anaconda3\envs\my_env\python.exe

   # macOS/Linux
   /Users/username/anaconda3/envs/my_env/bin/python
   ```
6. Click **OK**

### Step 2 — Configure Jupyter Server

1. Go to **File → Settings → Languages & Frameworks → Jupyter → Jupyter Servers**
2. Select **"IDE-Managed Server"** (set to Auto)
3. Click **OK**

> ✅ PyCharm will auto-start/stop Jupyter — no manual commands needed.

### Step 3 — Select Kernel in Notebook

1. Open any `.ipynb` file in PyCharm
2. Look at the **top-right corner** of the notebook editor
3. Click the kernel dropdown
4. Select your registered kernel (e.g. `Conda (ML Unsupervised)`)

### Switching kernels mid-session

- Top-right dropdown → select a different kernel
- PyCharm will restart the kernel automatically

---

## Using Kernels in Jupyter Notebook/Lab

### Start Jupyter from any environment
```bash
conda activate base
jupyter notebook
# or
jupyter lab
```

### Change kernel in a notebook
- **Jupyter Notebook**: Kernel menu → Change Kernel → select your env
- **JupyterLab**: Click kernel name in top-right → select your env

### Serve all conda envs automatically (optional)
Install `nb_conda_kernels` in base to auto-detect all conda environments:
```bash
conda install -n base nb_conda_kernels
```
Then start Jupyter from base — all envs appear as kernels automatically.

---

## Managing Multiple Environments

### Recommended workflow for multiple projects

```bash
# Create environments
conda create -n project_ml python=3.10
conda create -n project_dl python=3.11

# Install ipykernel in each
conda activate project_ml
conda install ipykernel
python -m ipykernel install --user --name project_ml --display-name "Project ML"

conda activate project_dl
conda install ipykernel
python -m ipykernel install --user --name project_dl --display-name "Project DL"

# Verify
jupyter kernelspec list
```

### Check which kernel a notebook is using
The kernel name is stored in the notebook's metadata — visible in the top-right of PyCharm/Jupyter.

---

## Troubleshooting

### Kernel not showing in PyCharm
```bash
# Re-register the kernel
conda activate my_env
python -m ipykernel install --user --name my_env --display-name "My Env"
# Then restart PyCharm
```

### Jupyter connection refused (port 8888)
```bash
# Check if Jupyter is running
jupyter notebook list

# Kill all running Jupyter servers
jupyter notebook stop
```

### Wrong Python version in kernel
```bash
# Verify which Python the kernel uses
conda activate my_env
python --version
# Then re-register
python -m ipykernel install --user --name my_env --display-name "My Env (Python 3.10)"
```

### Kernel keeps dying/crashing
```bash
# Check if all required packages are installed in the env
conda activate my_env
conda list

# Reinstall ipykernel
conda install --force-reinstall ipykernel
```

### PyCharm shows "No running Jupyter servers found"
- Go to **Settings → Jupyter → Jupyter Servers**
- Switch to **"IDE-Managed Server"**
- This makes PyCharm handle the server automatically

### Duplicate kernels appearing
```bash
# List all kernels to find duplicates
jupyter kernelspec list

# Remove duplicates
jupyter kernelspec remove duplicate_kernel_name
```

---

## Quick Reference Cheatsheet

```bash
# Installing ipykernel
conda activate my_env
conda install ipykernel

# creating kernel servers
python -m ipykernel install --user --name env_name --display-name "UI_display_name"

# listing all kernels
jupyter kernelspec list

# removing kernels
jupyter kernelspec remove kernel_name            # remove one
jupyter kernelspec remove k1 k2 k3              # remove multiple

# restoring kernels
python -m ipykernel install --user --name env_name --display-name "UI_display_name"

# Jupyter Notebook commands
jupyter notebook list                            # list running servers
jupyter notebook stop                            # stop all servers
jupyter lab                                      # start JupyterLab
```

---

## 🤝 Contributing

Feel free to open issues or pull requests for additional tips, OS-specific instructions, or new troubleshooting entries.

---

## 📄 License

[MIT License](/LICENSE) — free to use, share, and modify.

---