# GitHub guide for this project

Repository: [generative-fourier-reconstruction](https://github.com/Sahil0hi/generative-fourier-reconstruction).

GitHub shares saved files between our computers. We each run Python locally using `environment.yml`. The shared notebook is `stage1_partner.ipynb`.

 repository.

With Git and Conda installed,  runs:

```bash
git clone https://github.com/Sahil0hi/generative-fourier-reconstruction.git
cd generative-fourier-reconstruction
conda env create -f environment.yml
conda activate genai-reconstruction
python -m ipykernel install --user --name genai-reconstruction --display-name "GenAI reconstruction"
jupyter lab stage1_partner.ipynb
```

Select **GenAI reconstruction** as the notebook kernel. MNIST downloads on the first run. Sahil's existing `final_project` folder is already connected; it does not need cloning again. See [GitHub's cloning instructions](https://docs.github.com/en/get-started/using-git/getting-changes-from-a-remote-repository#cloning-a-repository).

## Each work session

1. Save and close the notebook. In the project folder, check for local changes:

   ```bash
   git status
   ```

2. With a clean working tree, download the latest changes:

   ```bash
   git pull --ff-only
   ```

3. Open the notebook, run or edit it, and save it with its outputs.
4. Select the notebook for sharing and review what will be committed:

   ```bash
   git add -- stage1_partner.ipynb
   git diff --cached --stat
   ```

5. Save a local version, then upload it:

   ```bash
   git commit -m "Add Stage 1 results and notes"
   git push
   ```

Done means the new commit and saved notebook outputs appear on GitHub. Coordinate who edits the notebook at a time. If you have local changes before pulling, commit the work you want to share first. If a pull or push reports diverging branches or a notebook conflict, resolve it together before continuing. [GitHub's pull guidance](https://docs.github.com/en/get-started/using-git/getting-changes-from-a-remote-repository#pulling-changes-from-a-remote-repository)

## What the commands mean

| Command | Meaning |
| --- | --- |
| `git status` | Show changed, staged, and untracked files. |
| `git add FILE` | Stage that file's current changes for the next commit. |
| `git add .gitignore` | Stage changes to the ignore rules themselves. |
| `git restore --staged FILE` | Unstage a file while keeping local edits. |
| `git commit -m "message"` | Record the staged changes locally. |
| `git push` | Upload local commits to GitHub. |
| `git pull --ff-only` | Update from GitHub when no merge is needed. |

## Keep a file local

`README.md` and `stage1_mnist.ipynb` are already local-only. Our `.gitignore` ignores everything except explicitly allowed deliverables.

To stop sharing another tracked file, remove its `!/filename` allowlist entry, then replace `filename` below with its actual path:

```bash
git rm --cached -- filename
git add .gitignore
git commit -m "Keep file local"
git push
```

`--cached` preserves your local copy. Earlier commits still contain the file. Adding an ignore rule alone does not untrack an existing file. [Git removal](https://git-scm.com/docs/git-rm), [ignore rules](https://git-scm.com/docs/gitignore)

To share a new reviewed file, add an entry such as `!/my_result.pdf` to `.gitignore`, then stage **both** `.gitignore` and that file before committing and pushing.

## Notebook outputs and environments

Saved plots and printed results travel inside the committed `.ipynb`. Running variables, installed packages, datasets, and separate checkpoint files do not. A new environment must be created on each computer. After a shared dependency change, run `conda env update -f environment.yml` in the activated environment. GitHub shows a static notebook preview; use JupyterLab to execute it. [GitHub notebook support](https://docs.github.com/en/repositories/working-with-files/using-files/working-with-non-code-files#working-with-jupyter-notebook-files-on-github)
