# Google Analytics

### URL
https://analytics.google.com/analytics/web/provision/?authuser=3#/provision/create


# Repository Setup with Sparse Checkout

This repository contains large image files in the `content/` folder and generated files in the `docs/` folder that you may not want to download locally.

## Clone Without Images and Docs

Use these commands to clone the repository without downloading JPG images or the docs folder:

```bash
# Clone without downloading any blobs initially
git clone --filter=blob:none --no-checkout <repository-url> portfolio
cd portfolio

# Configure sparse checkout to exclude images and docs
git sparse-checkout init --cone
git sparse-checkout set --no-cone '/*' '!content/**/*.jpg' '!content/**/*.JPG' '!content/**/*.png' '!content/**/*.PNG' '!docs/**'

# Checkout the main branch (won't download excluded files)
git checkout main
```

## What This Does

- `--filter=blob:none`: Clones the repository structure without downloading file contents initially
- `--no-checkout`: Doesn't checkout files yet (lets us configure sparse-checkout first)
- `sparse-checkout set`: Tells Git which files to include (`/*` = everything) and exclude (`!pattern` = exclusions)
- Images in `content/` subfolders and the entire `docs/` folder will not be downloaded

## Verify Setup

Check your sparse checkout configuration:
```bash
git sparse-checkout list
```

Check repository size:
```bash
du -sh .git
```

## Working with the Repository

You can work normally with all other files:
```bash
git add <files>
git commit -m "Your message"
git push
```

The excluded files remain on the remote and are still tracked by Git - they're just not downloaded to your local machine.

## Add More Exclusions

To exclude additional patterns:
```bash
git sparse-checkout set --no-cone '/*' '!content/**/*.jpg' '!content/**/*.JPG' '!docs/**' '!other-pattern/**'
```

## Disable Sparse Checkout

To download all files (including images and docs):
```bash
git sparse-checkout disable
```

## Notes

- Sparse checkout is stored locally - other collaborators aren't affected
- The excluded files will still show in GitHub/remote repository
- You can still view the files' history with `git log -- path/to/file`