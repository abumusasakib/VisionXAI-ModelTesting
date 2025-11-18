# VisionXAI-ModelTesting — Inference-only README

## Project Building and Running

### Build image

```cmd
cd environment && docker build . --tag 39bc7692-156c-4129-8fdf-5c202779e4d8
```

### Run image

```cmd
docker run --platform linux/amd64 --rm --gpus all --workdir /code --volume "%cd%/data":/data --volume "%cd%/code":/code --volume "%cd%/results":/results 39bc7692-156c-4129-8fdf-5c202779e4d8 bash run
```

## Running Jupyter for Development

1. **Start a Docker Container:**
   You can start a new container from the image you built using the following command in Command Line:

   ```cmd
   docker run -p 8888:8888 -it --platform linux/amd64 --rm --gpus all --workdir /code --volume "%cd%/data":/data --volume "%cd%/code":/code --volume "%cd%/results":/results 39bc7692-156c-4129-8fdf-5c202779e4d8 /bin/bash
   ```

   This command will start a new container based on the image tagged as `39bc7692-156c-4129-8fdf-5c202779e4d8` and open an interactive shell (`/bin/bash`) within the container.

2. **Activate Miniconda Environment:**
   Before launching the Jupyter Notebook server, ensure that you have activated your Miniconda environment. If you haven't activated it yet, you can do so by running:

   ```bash
   source /opt/conda/bin/activate
   ```

   This command activates the Miniconda environment.

3. **Launch Jupyter Notebook:**
   Once your Miniconda environment is activated, you can launch the Jupyter Notebook server in Command Line by running:

   ```bash
   jupyter notebook --ip=0.0.0.0 --port=8888 --no-browser --allow-root
   ```

   This command will start the Jupyter Notebook server and open a web browser with the Jupyter Dashboard, where you can navigate your files and create or open notebooks.

4. **Access Jupyter Notebook:**
   After running the `jupyter notebook` command, you should see output in your terminal with a URL that starts with `http://127.0.0.1:8888`. Open this URL in your web browser, and you should be directed to the Jupyter Dashboard, where you can create or open notebooks.

This repository contains an image-captioning notebook and related data. The default project state has been configured to run inference only (no training), to make it safe to run on machines without heavy compute or to produce evaluation outputs without modifying model weights.

## DO_TRAIN flag

- Location: `code/bangla_image_caption_testing.ipynb` in the configuration cell near the top.
- Purpose: Toggle whether the notebook should perform training or only run model inference.
- Default value: `DO_TRAIN = False` (inference-only)

## Behavior

- If `DO_TRAIN = False` (the default):
  - All training paths are disabled.
  - The training loop function immediately returns and does not perform optimization.
  - A safe no-op `train_step` is present so any accidental calls will not crash.
  - The orchestrator will attempt to restore an existing checkpoint for inference (from `best_model_metadata.json` or the `train` checkpoint directory) but will not start new training.

- If `DO_TRAIN = True`:
  - The original training code paths are restored and the notebook will attempt to run training as before. Only flip this to True if you understand resource requirements (GPU, disk space) and have appropriate checkpoints/backups.

## How to run inference (recommended)

1. Open the notebook `code/bangla_image_caption_testing.ipynb` in Jupyter or VS Code and run cells. The default configuration will skip training and proceed to load checkpoints and run the inference functions (e.g. `run_random_single()`, `run_random_batch()`, `run_first_n()`).

2. Headless execution example (must have Jupyter and required packages installed):

   jupyter nbconvert --to notebook --execute "code\bangla_image_caption_testing.ipynb" --output "code\bangla_image_caption_testing_executed.ipynb"

3. Check outputs in `/results/` (attention plots, HTML report, `captions.json`, BLEU scores CSV, etc.).

## Notes and safety

- The notebook still requires model checkpoints and a tokenizer for meaningful output. If no checkpoints are available, inference will likely fail because model weights are not initialized.
- To enable training, set `DO_TRAIN = True` in the configuration cell and ensure you have adequate compute & storage.
