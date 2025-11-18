# Contributing

This repository uses Git LFS to store large binary files (environment wheels,
model checkpoints, and generated `results/` artifacts). To keep the main Git
history lightweight and make pushes/pulls reliable, please follow the rules
below when adding large files.

## Tracked patterns (already configured)

- `environment/**`
- `environment/*.whl`
- `environment/wheels/**`
- `results/**`
- `*.whl`
- `*.ckpt`
- `*.npz`
- `*.data-*`
- `*.index`
- `*.pkl`

## How to add a new large file

1. Make sure Git LFS is installed:

```powershell
git lfs install
```

1. Add patterns for any new file types you expect to add (this updates
   `.gitattributes`):

```powershell
# Example: track model checkpoints and numpy archives
git lfs track "*.ckpt" "*.npz"
git add .gitattributes
git commit -m "chore(lfs): track new large file patterns"
```

1. Add and commit the large file as usual. Git LFS will store the file in LFS
   and keep a small pointer file in the Git history.

```powershell
git add path/to/large-file.ckpt
git commit -m "feat(model): add trained checkpoint for example"
```

1. Push to remote. LFS objects will be uploaded along with the Git refs:

```powershell
git push origin <branch>
# If you need to upload all LFS objects explicitly:
git lfs push --all origin
```

## Notes for maintainers

- Avoid committing large generated artifacts (e.g., intermediate `results/`)
  unless they are necessary for reproduction; prefer storing them externally
  when possible.
- If you encounter any issues with LFS uploads, run `git lfs env` and check
  the endpoint and auth settings.
