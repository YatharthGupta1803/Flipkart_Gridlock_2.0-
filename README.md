# 🏁 Flipkart Gridlock 2.0 — Team Command Center

This workspace is structurally optimized to support parallel model development across 4 data scientists while guaranteeing zero file overwrites, clean Git history, and automated local data shielding.

---

## 1. Repository Architecture 📁

To prevent merge conflicts, the repository is split into isolated sandbox environments. Never dump files directly into the root folder.

* **GRIDLOCK/** (Root Project Folder)
    * ├── 📄 `.gitignore` ── Controls global tracking restrictions (keeps repo lightweight)
    * ├── 📄 `README.md` ── Team command center, Git instructions & submission log
    * │
    * ├── 📁 **data/** ── [LOCAL ONLY] Hidden entirely from GitHub
    * │   ├── └── 📁 `raw/` ── Holds original train.csv, test.csv, sample_submission.csv
    * │   └── └── 📁 `processed/` ── Holds your engineered features & scaled matrices
    * │
    * ├── 📁 **notebooks/** ── [GLOBAL] Tracked and pushed to GitHub
    * │   ├── ├── 📁 `shared/` ── Master repository for production-ready shared code
    * │   ├── ├── 📁 `avyukt/` ── Avyukt's private sandbox workspace
    * │   ├── ├── 📁 `yatharth/` ── Yatharth's private sandbox workspace
    * │   ├── ├── 📁 `sarthak/` ── Sarthak's private sandbox workspace
    * │   └── └── 📁 `ashish/` ── Ashish's private sandbox workspace
    * │
    * └── 📁 **submissions/** ── [LOCAL ONLY] Hidden archive tracking your local prediction outputs

---

## 2. Data Governance 🔒: Local vs. Global

Our tracking profile divides your workspace assets into two strict buckets. Review these rules before using Git:

### 🌐 GLOBAL DATA (Pushed to GitHub)
* └── 📝 **Your Notebook Code (`notebooks/your_name/*`)**
    * Must be pushed regularly so the team can verify architectures and combine insights.

### 🔒 LOCAL DATA (Stays on Your Mac)
* ├── 💾 **The Datasets (`data/`)** ── Blocked. Prevents sluggish multi-gigabyte sync cycles.
* ├── 📊 **Prediction Files (`submissions/`)** ── Blocked. Prevents teammates from overwriting outputs.
* └── 🛠️ **System Metadata (`.venv/` , `__pycache__/` , `.DS_Store`)** ── Blocked. Environmental junk.

---

## 3. Execution Mechanics 🛠️: Working in Subfolders

Because your active files live two layers deep inside `notebooks/your_name/`, your code paths must explicitly step backward into the root directory using standard parent routing (`../../`).

### 🔹 Step-Out Pathing: Loading Data
```python
import pandas as pd

# The "../../" drops Python back to the root folder to access the data directory safely
train = pd.read_csv("../../data/raw/train.csv")
test = pd.read_csv("../../data/raw/test.csv")
```

---

## 4. Git Rules of Engagement & Branching Workflow 🔄

> 🛑 **THE GOLDEN RULE:** Never push code directly to the `main` branch. Work exclusively on your personal development branch to guarantee a zero-conflict workspace.

### 🗺️ Operational Pipeline Overview
```text
 🟩 1. Sync Main  ──►  🟨 2. Catch Up Branch  ──►  🟦 3. Code in Sandbox  ──►  🟪 4. Push Branch

🟩 STEP 1: Sync Your Local Machine (Start of Session)
Goal: Download any fresh updates your teammates shipped to the remote repository while you were away.

# Switch to the team's shared master reference
git checkout main

# Pull down the newest changes safely
git pull origin main

🟨 STEP 2: Move to Your Dev Branch & Merge Updates
Goal: Switch to your private workstation and inject the team's new changes directly into your environment.
(Branch Registry: dev-avyukt | dev-yatharth | dev-sarthak | dev-ashish)

# Switch to your branch (or auto-create it if this is your first time)
git checkout dev-yourname 2>/dev/null || git checkout -b dev-yourname

# Merge the fresh main code updates you just pulled into your dev workspace
git merge main

🟦 STEP 3: Work, Code, and Save Inside Your Sandbox
Goal: Run your feature engineering loops and model training sessions safely.

Rule: Do your work exclusively inside your assigned directory space: notebooks/yourname/.


🟪 STEP 4: Ship Your Progress Globally (End of Session)
Goal: Upload your new architectures and code files to GitHub so the team can review your progress.

# Stage ONLY your personal sandbox folder (prevents accidental data uploads)
git add notebooks/yourname/

# Lock in your progress with a clear feature comment
git commit -m "feat(yourname): added advanced historical lag interaction features"

# Push your changes safely to your isolated cloud branch
git push origin dev-yourname

