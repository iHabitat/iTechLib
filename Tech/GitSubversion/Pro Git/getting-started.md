# Pro Git

## Git Has Integrity

The mechanism that Git uses for this checksumming is called a **SHA-1 hash**.

## The Three States

Git has three main states that your file can reside in:

- committed
- modified
- staged

This lead us to the three main sections of a Git project:

- the Git directory, stores the metadata and object database
- the working directory, a single checkout of one version of the project
- the staging area, is a simple file stores information about what will go into your next commit