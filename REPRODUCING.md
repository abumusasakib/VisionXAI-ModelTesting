# Information

This [Code Ocean](https://codeocean.com) Compute Capsule will allow you to reproduce the results published by the author on your local machine<sup>1</sup>. Follow the instructions below, or consult [our knowledge base](https://help.codeocean.com/user-manual/sharing-and-finding-published-capsules/exporting-capsules-and-reproducing-results-on-your-local-machine) for more information. Don't hesitate to reach out to [Support](mailto:support@codeocean.com) if you have any questions.

<sup>1</sup> You may need access to additional hardware and/or software licenses.

## Prerequisites

- [Docker Community Edition (CE)](https://www.docker.com/community-edition)
- [nvidia-container-runtime](https://docs.docker.com/config/containers/resource_constraints/#gpu) for code that leverages the GPU
- MATLAB/MOSEK/Stata licenses where applicable

### Note: Git LFS migration

Large binary artifacts (environment wheels, prebuilt
packages, model checkpoints and generated `results/` outputs) were migrated to
Git LFS and the rewritten history was force-pushed to the `origin` remote.

- Why: the repository exceeded the HTTP push limits when uploading ~2.2 GiB
  of binary data over HTTPS; moving those blobs to Git LFS makes pushes and
  clones practical.
- What changed: `environment/` and `results/` (and several wheel/checkpoint
  file patterns) are now tracked by Git LFS. A backup branch
  `backup/autocommit-20251118-081733` was preserved on the remote with the
  original history.

If you have a local clone, update it by either re-cloning or resetting your
branches to the rewritten history.

## Instructions

Cleanest (recommended):

```powershell
git clone git@github.com:abumusasakib/VisionXAI-ModelTesting.git
cd VisionXAI-ModelTesting
git lfs pull
```

If you prefer to update an existing local clone (WARNING: will overwrite local
changes):

```powershell
git fetch origin --all
git reset --hard origin/master
git lfs pull
```

## The computational environment (Docker image)

This capsule is private and its environment cannot be downloaded at this time. You will need to rebuild the environment locally.

> If there's any software requiring a license that needs to be run during the build stage, you'll need to make your license available. See [our knowledge base](https://help.codeocean.com/user-manual/sharing-and-finding-published-capsules/exporting-capsules-and-reproducing-results-on-your-local-machine) for more information.

In your terminal, navigate to the folder where you've extracted the capsule and execute the following command:

```shell
cd environment && docker build . --tag 39bc7692-156c-4129-8fdf-5c202779e4d8; cd ..
```

> This step will recreate the environment (i.e., the Docker image) locally, fetching and installing any required dependencies in the process. If any external resources have become unavailable for any reason, the environment will fail to build.

## Running the capsule to reproduce the results

In your terminal, navigate to the folder where you've extracted the capsule and execute the following command, adjusting parameters as needed:

```shell
docker run --platform linux/amd64 --rm --gpus all \
  --workdir /code \
  --volume "$PWD/data":/data \
  --volume "$PWD/code":/code \
  --volume "$PWD/results":/results \
  39bc7692-156c-4129-8fdf-5c202779e4d8 bash run
```
