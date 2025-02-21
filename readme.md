# Git Submodules

This is a sandbox area for learning and testing git submodules.

It's a git command for managing one git repository within another git
repository.

## Usage Example

In this example, `github-actions-sandbox` is the super-project, and `sandbox`
is the sub-project.

Creating a submodule creates a `.gitmodules` folder in the super-project and a
folder for the submodule.

```sh
git submodule add <git-url> <optional-path>

# Example
git submodule add https://github.com/maddawik/sandbox.git sandbox
```

The submodule is a reference to a commit hash that `github-actions-sandbox` is
now using. The only thing that the super-project knows about `sandbox` is that
it should be on a specific commit, it does not track the content of that
submodule in any way.

In that respect - when working with a submodule, git commands are relative to
the directory you're running them from. Here's an example of running git status
right after running the commands to add a submodule above.

First from the root of this repo, we can see that there is a new submodule to
be added:

```sh
❯ cd github-actions-sanbox/
❯ git status
On branch submodules
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   .gitmodules
        new file:   sandbox
```

Now if we run `git status` from the subproject (changing the context for `git`
commands), we can see there's no changes in the working directory for the
submodule:

```sh
❯ cd sandbox/
❯ git status
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

This is an important distinction - you can make updates to the subproject,
create new branches, push them, etc. - but the super-project will only ever
care about the reference to a commit, nothing more. Respect them as separate
repos.

You can commit this submodule in the super-project like any other change. When
you or someone new clones the project, you'll also want to initialize the
modules (by default, submodules are not cloned)

TODO: Add a bit about updating the reference of the submodule

```sh
# Set up the submodule config in the super-project
git submodule init
# Clone any submodules & check out the correct commit
git submodule update

# Just DOO ITT
git submodule update --init
```

## Pros and Cons

### Pros

- You can commit changes to the sub-project(s) while working on the super-project
- The git history of the super-project(s) tracks its dependency history on the sub-project(s)
- And more...

### Cons

- Adds a layer of abstraction to version control
- Git commands are relative to the context of your working directory (you gotta
  think about that now)
- And more...
