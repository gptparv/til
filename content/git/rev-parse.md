+++
date = 2025-06-02
description = "It is used for parsing and retrieving Git object identifiers in a user-friendly manner."
draft = false
title = "How to use the command 'git rev-parse'"

[extra]
keywords = "Git"
series = "Git"
toc = true

[taxonomies]
tags = [
    "Git",
]
+++

It is used for parsing and retrieving Git object identifiers in a user-friendly manner. Highly useful in scripting Git operations.

## Get commit hash of the branch

```bash
git rev-parse <branch_name>
```

Getting the commit hash of the branch is crucial to reference a specific state of the code. For example, deploying application changes, version tagging, or troubleshooting issues. 

Explanation:

- `branch_name`: specifies the name of the branch whose commit hash you want to retrieve. Returns the corresponding SHA-1 hash, a unique 40-character identifier that represents the precise state of the branch.

Example:

```bash
git rev-parse main
```

Output:

```bash
ce248f5ef7bb80f841f1bca3f3faf9d4d4c0b085
```

## Get the current branch name

```bash
git rev-parse --abbrev-ref HEAD
```

Determining the current branch name in environments where scripts or automated processes must adapt dynamically based on the active branch. 

Explanation:

- `--abbrev-ref`: outputs a human-readable abbreviation of a reference rather than its full SHA-1 hash. In this context, it produces a concise branch name.
- `HEAD`: Represents the pointer to the current branch. By applying `--abbrev-ref` to `HEAD`, the operation effectively translates the reference into the active branch name.

Output:

```bash
main
```

This result signifies that the user is working on the `main` branch.

## Get the absolute path to the root directory

```bash
git rev-parse --show-toplevel
```

Utilizing the absolute path prevents errors associated with path ambiguities and ensures that all path-dependent scripts and commands operate from the correct starting point.

Explanation:

- `--show-toplevel`: An option designed to reveal the absolute path of the top-level directory of the repository. This reveals the repository’s root directory independent of the current working directory or structure.

Output:

```bash
/Users/parv.gupta/repos/github.com/gptparv/til
```


## Verify given reference exists

```bash
git rev-parse --verify <branch_name>
```

Sometimes it useful in automation scripts to know if the branch exists.

Explanation:

- `--verify`: provided parameter returns standard output; otherwise, error out.

Example:

```bash
git rev-parse --verify main
```

Output:

```bash
ad90fcffc76aa50813b82b57f219a6df4e6a2224
```

Example:

```bash
git rev-parse --verify feature_branch
```

Output:

```bash
fatal: Needed a single revision
```

Script:

```bash
if git rev-parse --verify <branch_name> >/dev/null 2>&1; then
  echo "The branch exists."
else
  echo "The branch does not exist."
fi
```
