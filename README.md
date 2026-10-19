# Fourier reconstruction course project

Recover images from incomplete, simulated Fourier measurements. Stage 1 compares a principal-subspace/PCA prior with zero-filling on MNIST. Later stages will use OrganAMNIST or PneumoniaMNIST and evaluate artificial-lesion preservation and invention; those later models and experiments are not implemented yet.

Start with [the study guide (PDF)](Paper_1_and_Stage_1_Guide.pdf), the runnable [main notebook](stage1_mnist.ipynb), or the [partner notebook](stage1_partner.ipynb), which includes guided response cells. An [editable Word guide](Paper_1_and_Stage_1_Guide.docx) is also available.

## Work together with GitHub

Both people keep a local copy of this repository and create their own environment from `environment.yml`. GitHub shares saved files and notebook outputs; it does not share a running Python session or automatically install packages.

For a first checkout:

```bash
git clone https://github.com/Sahil0hi/generative-fourier-reconstruction.git
cd generative-fourier-reconstruction
```

Then follow **Run Stage 1** below. In GitHub Desktop, **File > Clone repository** is the equivalent starting point.

Sahil normally edits `stage1_mnist.ipynb`; Partner edits `stage1_partner.ipynb`. Coordinate changes to shared code and settings. Avoid editing the same notebook concurrently; notebook files can be difficult to merge.

Partner's workflow:

1. Save and close the notebook. With no uncommitted changes, run `git pull --ff-only` to fetch the latest work. If local work is unfinished, commit it before pulling.
2. Open `stage1_partner.ipynb` from the project folder, select **GenAI reconstruction**, and run or annotate it. Complete the six **need to do** response cells using the actual results.
3. Save the notebook with the plots and printed outputs still included, then review the changed files in GitHub Desktop or with `git status`.
4. Share the saved notebook:

   ```bash
   git add stage1_partner.ipynb
   git commit -m "Record Stage 1 analysis"
   git push
   ```

5. Tell Sahil which results or explanations changed. Done means the pushed notebook contains the intended responses and saved outputs, and Sahil can pull and view them.

In GitHub Desktop, use **Fetch origin / Pull origin**, review and select the changed files, **Commit to main**, then **Push origin**. Sahil follows the same workflow with `stage1_mnist.ipynb`. If both people changed the same notebook, keep both versions and resolve the differences together before continuing.

The repository owner must add Partner under **Settings > Collaborators > Add people**, and Partner must accept the invitation to push directly. Public visibility allows reading and cloning; it does not grant write access. See [GitHub's collaborator instructions](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/repository-access-and-collaboration/inviting-collaborators-to-a-personal-repository).

The repository includes reviewed notebooks, documentation, the environment specification, and a saved Stage 1 figure and numerical summary. Downloaded datasets, local environments, raw experiment arrays, and model checkpoints stay local. The `.gitignore` allows only the reviewed deliverables listed in it; review a new file before adding it to that list. Keep complete local run folders, and exchange a specific raw run separately when needed.

## Run Stage 1

From this project folder, create the shared environment:

```bash
conda env create -f environment.yml
conda activate genai-reconstruction
python -m ipykernel install --user --name genai-reconstruction --display-name "GenAI reconstruction"
jupyter lab stage1_mnist.ipynb
```

Select **GenAI reconstruction**, then restart the kernel and run all cells. The first run downloads MNIST into `data/` if necessary. It uses CPU and needs no MRI data agreement or GPU. The dependencies use compatible version ranges for portability; the exact versions used in each run are saved with its outputs. Official references: [Conda environment files](https://docs.conda.io/projects/conda/en/stable/user-guide/tasks/manage-environments.html), [MNIST downloader](https://docs.pytorch.org/vision/stable/generated/torchvision.datasets.MNIST.html).

Partner can open [stage1_partner.ipynb](stage1_partner.ipynb) for the same runnable code with six **need to do** sections. Fill in the written notes using the run's outputs; all numerical code is supplied.

For a command-line run in the activated environment:

```bash
jupyter nbconvert --to notebook --execute --inplace stage1_mnist.ipynb --ExecutePreprocessor.timeout=600
```

The notebook fits PCA on training digits, chooses its rank/regularization on validation digits, and compares principal-subspace recovery with zero-filling on held-out digits. It includes the required 8-image training curve, automatic-versus-finite-difference gradient, measured training time, and additional Fourier checks. The output is a baseline demonstration on digits with simulated Fourier measurements.

The notebooks contain saved outputs from a Python 3.11 CPU run, including two plots and the printed numerical checks. A fresh installation from `environment.yml` has not been verified; its YAML syntax and the notebook's used dependency versions were checked.

The [saved numerical summary](outputs/stage1/20261017T194144_806900Z/summary.json) and [submission figure](outputs/stage1/20261017T194144_806900Z/stage1_figure.png) document one completed CPU run. Mean test PSNR was 17.374 dB for PCA and 15.817 dB for zero-filling over 200 test images. Use your own run's timing when reporting performance on your machine.

Each run creates a separate folder in `outputs/stage1/` with:

- `stage1_figure.png`: the main submission figure, including a fixed test case, metrics, and checks.
- `summary.json`: measured results, timing, hardware, versions, and settings.
- `run_data.npz`: split indices, raw measurements/reconstructions, per-image PSNR, validation scores, and the eight-image loss curve.
- `pca_checkpoint.npz` and `eight_image_model.pt`: fitted models. Draft difficulty sentences remain in the notebook for the team to review.

`outputs/stage1/latest_run.txt` identifies the most recent completed run. Keep failed-run folders when diagnosing problems.

## Partner's first run

1. Create the environment above and run all cells without changing the settings.
2. Inspect the sample digit grid and confirm intensities range from 0 to 1.
3. Check that the Fourier, gradient, and 8-image checks pass; record both gradient numbers and the actual training times.
4. Open the main figure and compare the two error maps. Explain a visible success or failure using the matching PSNR values.
5. Review the two difficulty sentences with Sahil and record your own contribution and local machine timing. Use the study guide's Stage 1 submission checklist for the complete handoff.

## Later medical-image data

Use the official [MedMNIST package and download instructions](https://github.com/MedMNIST/MedMNIST). Choose OrganAMNIST or PneumoniaMNIST, keep its official splits, and generate measurements from each processed reference image. The future lesion experiment inserts the spot before the FFT and includes no-insertion controls. These datasets contain CT and X-ray images respectively, so call the measurements simulated rather than acquired MRI. [Official dataset descriptions](https://github.com/MedMNIST/MedMNIST/blob/main/medmnist/info.py)
