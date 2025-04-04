# action-gh-release Reproduction Repo

This repository is a reproduction of the issue with the action-gh-release GitHub Action.

## Overview

For projects that use:
- draft release input (`draft: true`)
- multiple usages of this action

And have:
- Greater than 100 releases in GitHub

The action-gh-release action will create a new release entity for each usage of the action, rather than one draft release.

## This Repository

This repository contains the following:
```
.
├── .github
│   └── workflows
│       └── main.yml
├── hello.js
└── world.js
```
Where
- `.github/workflows/main.yml` is a GitHub Action workflow that uses the action-gh-release action.
This workflow that helps demonstrate the issue (more on that in a moment).
- `hello.js` and `world.js` are two simple JS files.
They're used solely to have a file to upload to the release.

## Steps to Reproduce

Prerequisites:
1. A repository with 100 GitHub releases. 
This repository has been 'primed' to have 101 releases to demonstrate the issue.
2. A workflow that uses the `action-gh-release` action multiple times with the `draft: true` input.
This is included in `.github/workflows/main.yml` in this repository.

Steps to Reproduce:
1. Run the `main.yml` workflow in this repository.
This is more difficult than it should be, as a fork of the repo won't have the releases that are in the original (if I understand correctly).
2. On the 101st run of the workflow, the action-gh-release action will create a new release entity for each usage of the action, rather than one draft release.

## Real world example:

The 212th run of the workflow in this repository created the 100th release entity in the repository:
- Logs from the workflow: https://github.com/rwaskiewicz/action-gh-release-repro/actions/runs/14272880833
- Release Entity: https://github.com/rwaskiewicz/action-gh-release-repro/releases/tag/212

The 213th run of the workflow in this repository created the 2 draft release entities in the repository:
- Logs from the workflow: https://github.com/rwaskiewicz/action-gh-release-repro/actions/runs/14272890579
- Release Entity 1: https://github.com/rwaskiewicz/action-gh-release-repro/releases/tag/untagged-ba4721d8b12f6bbd9e7c
- Release Entity 2: https://github.com/rwaskiewicz/action-gh-release-repro/releases/tag/untagged-534d93ef9215159ec0a3

Note how in the logs of the 212th run, the action-gh-release action created a single release entity.
It was able to find the existing release entity in the "Upload World File to Release step":
> Run softprops/action-gh-release@c95fe1489396fe8a9eb87c0abf8aa5b2ef267fda
> 
> Found release 212 (with id=210448811)

However, in the 213th run, the action-gh-release action created 2 release entities.
It was not able to find the existing release entity in the "Upload World File to Release step":
> Run softprops/action-gh-release@c95fe1489396fe8a9eb87c0abf8aa5b2ef267fda
> 
> 👩‍🏭 Creating new GitHub release for tag 213...


