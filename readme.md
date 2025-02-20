# Git Subtrees

This is a sandbox area for learning and testing git subtrees.

Similar to submodule, it provides a similar mechanism for managing one git
repository within another git repository.

## Usage Example

In this example, `github-actions-sandbox` is the super-project, and `sandbox`
is the sub-project.

Creating a submodule creates a `.gitmodules` folder in the super-project and a
folder for the submodule.

```sh
git subtree add --prefix=<path> <git-url> <ref>

# Example
git subtree add --prefix=sandbox https://github.com/maddawik/sandbox.git main
```

> [!NOTE]
> This command cannot be run if the working directory as any modifications.
> This is because...

This command creates a new commit that merges in the tip of `main` in
`sandbox`. This brings the full history of `sandbox` into our repository.

```sh
❯ git t
*   aa8a80e (HEAD -> subtrees) Add 'sandbox/' from commit '274e7a73dac51a01e99faefbc68b02b75a140feb'
|\
| * 274e7a7 initial commit
* 236a466 (origin/main, origin/HEAD, main) update readme (#87)
* b225188 merge it (#86)
* 69fe607 (tag: feature-new-1) feature/pr test (#85)

# TWO ROOT COMMITS?? BLASPHEMY!!!!!11
```

This is effectively no different than `git remote add <repo> <url>; git merge
origin/main` and then moving things under the `sandbox` directory.

We can also squash the git history of `sandbox` when doing this:

```sh
git subtree add --prefix=<path> --squash <url> <ref>
```

This reduces the amount of history 2 commits:

1. Squashing the content of the `sandbox` content into the 1st
2. Merging the contents of `sandbox` with `github-actions-sandbox` into a 2nd

Now we have sandbox... how we would we update it to a newer version? Or more
interestingly - how could we rollBACK TO THE FUTURE?

```sh
# Rolling back to an older commit!
git subtree merge -P sandbox --squash 274e7a7
```

The thing to note (and the big difference with `submodule`) is that there is no
`add` or `commit` happening for these commands. The complexity of updating
dependencies is simplified so that developers working on the super-project
don't concern themselves with updating submodules, they just pull the latest
state of the super project - which is effectively tracking the state of changes
in the subtree.

> [!NOTE]
> The person who manipulates the subtree(s) will be able to see the history of
> the sub-project even after squashing. This is because when they run the
> command to squash/merge, they download all of the refs for the sub-project.
> Although the history is available to them, this is only local to their
> machine. Other users cannot `git log <sha-from-sub-project>` while the
> "subtree-smasher" can.

Final repo history from `236a466` forward:

```sh
❯ git t
*   d990e57 (HEAD -> subtrees) Merge commit '81c3aee48244fbc5c23df1946da077504fed0d35' into subtrees
|\
| * 81c3aee Squashed 'sandbox/' changes from 3660e92..274e7a7
* | f6e3839 Merge commit '85020b6c635109c00746cbe9a754ef9bc66b57d9' as 'sandbox'
|\|
| * 85020b6 Squashed 'sandbox/' content from commit 3660e92
* 236a466 (origin/main, origin/HEAD, main) update readme (#87)

```

## Pros and Cons

### Pros

- Dealing with dependencies is a lot more straightforward (team members don't
  have to run submodule/subtree commands - they just pull)
- Git commands are not context-based like submodules
- And more...

### Cons

- You need to be aware of making changes to things in the subtree since they
  are now tracked
- And more...
