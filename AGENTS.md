# Repository Guidelines

## Project Structure & Module Organization

This workspace contains two independent Git repositories. 

- `mmore/src/mmore/`: The souce code of the open source tool my thesis is based on. Documentation is under `mmore/docs/source/`.
- `LiGHT-cluster-template/src/template_package_name/`. The most important thing to keep in mind is that all experiments I run through the lab's RCP cluster. 
Link to doc: `https://lighters-playground.github.io/LiGHT-doc/clusters/rcp/rcp/` and `https://epflight.github.io/mmore`
- Root PDFs are reference materials.

## Build, Test, and Development Commands

From `mmore/`, use Python 3.10+ and install system dependencies described in its README and doc.

## Coding Style & Naming Conventions

Use four-space Python indentation, `snake_case` functions/modules, and `PascalCase` classes. Follow surrounding type annotations and configuration patterns. MMORE uses Ruff linting and formatting; the cluster template uses Black, isort, and codespell through pre-commit. Run each project's own hooks.


## Commit & Pull Request Guidelines

I remain the coder. I want you to suggest modifications or novel ideas with clear sources and reasoning. When we are trying to fix the bug, I want you to help me step by step from reproducing the bug to fixing, writing test and final commit message.
