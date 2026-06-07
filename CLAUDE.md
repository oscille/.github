# Oscille

Org-wide guidance for every Oscille repository. Clone repositories into the `repositories/` folder, where this guidance is inherited from the parent. A repo may add its own `CLAUDE.md` for anything specific to it.

## What Oscille is

Oscille builds the seed, a business that runs itself. The founding document is [CONSTITUTION.md](CONSTITUTION.md). It sets what the company is for, what it must never do, who decides what, and how those rules change. Everything else answers to it.

## Conventions

Read these before writing code or prose, and follow them. They live in [`conventions/`](conventions/).

- [CODE.md](conventions/CODE.md) covers how we design APIs and systems.
- [COMMENT.md](conventions/COMMENT.md) covers when and how we comment code.
- [WRITING.md](conventions/WRITING.md) covers how we write prose, docs, and messages.

Where a convention does not cover a case, follow the patterns already in the repo you are working in.

@conventions/CODE.md
@conventions/COMMENT.md
@conventions/WRITING.md

In case you are in a repository, the conventions are not automatically inherited. So please read them in the parent `repositories/` folder, and follow them in your work.

## Repository rules

- **Default branch** is `main`.
- **Branches** are named `feature/<desc>`, `fix/<desc>`, or `chore/<desc>`.
- **Commits** follow [Conventional Commits](https://www.conventionalcommits.org/), with a type of `feat`, `fix`, `docs`, `chore`, `refactor`, or `test`.
- **Pull requests** stay small and focused, pass CI before review, and squash-merge.
- **Git that touches a shared or human-owned repository** (commit, push, merge, branch or tag changes) runs only with explicit human approval each time; never on your own initiative.
- **Every repository** ships a `README`, a `LICENSE`, and CI that runs lint, typecheck, and tests.
- **Authorship** is human; never add an agent as co-author on a commit or pull request.