+++ 
date = "2020-12-13"
title = "Git LFS vs Others"
slug = "git-lfs-vs-others" 
tags = []
categories = []
+++

## Git LFS

Git Large File Storage is a CLI and server made by Github to store large files. It integrates very nicely into the git workflow, making it easy for git users to adopt. Git LFS stores your repoistory's large files in the Git LFS server and adds a link to your repository that points there. Which files in your repository are managed by Git LFS is specified in `.gitattributes` and manages by `git lfs track` and `git lfs untrack` commands.

Git LFS is enabled and disabled per repository. In the repository you run:

```sh
git lfs install
```

Then this to track a file:

```sh
git lfs track my_big_file.txt
```

Then `git add` and `git push` as usual and thats it.

#### Pros

- Very easy to setup, manage which files are tracked, and remove if need be
- Naturally integrated with git so you can manage your code and data storage on one platform

#### Cons

- Maximum of 2GiB file size
  - This is not encouraging when thinking about genomic data
- Its more expensive than AWS S3 and Google Cloud

#### Pricing

- $5 / month per data pack, where a data pack is 50GB storage with 50GB bandwidth
- If you have 3 data packs, you would pay $15 / month and have 150GB of storage with 150GB bandwidth

## Big Players

### Amazon S3

#### Pricing

- $0.023 / month per GB for the first 50TB. This is equal to $1.15 / month for 50GB storage
- Much cheaper than Git LFS
- https://aws.amazon.com/s3/pricing/

### Google Cloud

#### Pricing

- About $0.20 / month per GB which is about $1 / month for 50GB
- But adding in costs from interacting with the storage it is $1.19 / month per 50GB.
- About the same as Amazon S3
- Cost per GB goes down as amount of data stored increaes.

## Smaller Players

### git-annex

git-annex was a bigger player in the early days of Git LFS, but has since been beat out. It is praised among its users but there is a consensus that Git LFS wins for the following reasons:

- When a team member or outsider clones a repo with Git LFS enabled, they dont need to know that before hand. They dont need to add anything to the clone command to tell github to pull down the GIT LFS data which is part of that repository. With git-annex however, that is the case.
- Because Git LFS is made by Github, git-annex cannot compete when it comes to improvement speed, support, and overall integration with Github.

### DVC

https://dvc.org

Stands for "Data Version Control". I found it in this article which pitches DVC: https://towardsdatascience.com/why-git-and-git-lfs-is-not-enough-to-solve-the-machine-learning-reproducibility-crisis-f733b49e96e8

Looks promising but its not fun to integrate an entirely new tool into your stack.

### BackBlaze B2

https://www.backblaze.com/b2/cloud-storage.html
