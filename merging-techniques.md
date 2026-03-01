# Git Merge Strategies

## Starting Point (for all examples)

```
main:     A --- B --- C
                \
branchA:         D --- E
```

---

## 1. Merge Commit

Creates a merge commit (knot) that ties both histories together.

```bash
git checkout main
git merge branchA
```

```
main:     A --- B --- C ------- M
                \             /
branchA:         D --- E -----
```

**Pros:**
- Preserves full history of what happened and when
- Easy to see where branches diverged and merged

**Cons:**
- Creates a knot in history
- History becomes messy with many branches

---

## 2. Fast-Forward Merge

Only works when main has **no new commits** since branching. Git simply slides the main pointer forward — no merge commit created.

```bash
git checkout main
git merge --ff-only branchA
```

```
Before:
main:     A --- B
                \
branchA:         D --- E

After:
main:     A --- B --- D --- E
```

**Pros:**
- Cleanest possible merge
- Linear history, no merge commit

**Cons:**
- Only possible when histories haven't diverged
- Fails if main has new commits since branching

---

## 3. Squash and Merge

Squashes all commits from branchA into a single commit before merging into main.

```bash
git checkout main
git merge --squash branchA
git commit -m "Add feature from branchA"
```

```
main:     A --- B --- C --- DE
                \
branchA:         D --- E  (squashed into one)
```

**Pros:**
- Clean linear history
- One commit per feature — easy to revert an entire feature
- Hides messy WIP commits ("fix typo", "wip", "fix again")

**Cons:**
- Loses individual commit history of the branch
- Git doesn't recognize branch as truly merged — use `git branch -D` to delete

---

## 4. Rebase and Merge

Replays branchA commits on top of main, then fast-forwards main. Gives clean linear history without rewriting main.

```bash
# Step 1 - while on branchA
git checkout branchA
git rebase main

# Step 2 - bring main forward
git checkout main
git merge branchA
```

```
Step 1 - rebase branchA onto main:
main:     A --- B --- C
                          \
branchA:                   D' --- E'

Step 2 - fast-forward main:
main:     A --- B --- C --- D' --- E'
```

**Pros:**
- Clean linear history
- Keeps individual commits (unlike squash)
- Merge step is always a fast-forward (no knot)

**Cons:**
- Rewrites commit hashes (D → D', E → E')
- Never rebase shared/public branches

---

## 5. Interactive Rebase (Squash Specific Commits)

Lets you squash, reorder, reword, or drop specific commits before merging.

```bash
git rebase -i HEAD~4
```

Editor opens:
```
pick A1b2c3 commit D
squash B2c3d4 commit E
squash C3d4e5 commit F
pick D4e5f6 commit G
```

```
Before:  D --- E --- F --- G
After:   DEF --- G
```

**Interactive rebase commands:**

| Command | Shortcut | What it does |
|---|---|---|
| `pick` | `p` | Keep commit as is |
| `squash` | `s` | Squash into previous commit, combine messages |
| `fixup` | `f` | Squash into previous commit, discard this message |
| `reword` | `r` | Keep commit but edit message |
| `drop` | `d` | Delete the commit entirely |

> **Rule:** `squash` always folds **upward** into the nearest `pick` above it.

---

## Summary

```
merge commit:  A--B--C-------M     (knot, full history preserved)
fast-forward:  A--B--D--E          (linear, no rewrite, no merge commit)
squash:        A--B--C--DE         (linear, one commit per feature)
rebase:        A--B--C--D'--E'     (linear, individual commits, rewritten hashes)
```

| Strategy | Linear History | Preserves Commits | Rewrites History | Creates Merge Commit |
|---|---|---|---|---|
| Merge commit | ❌ | ✅ | ❌ | ✅ |
| Fast-forward | ✅ | ✅ | ❌ | ❌ |
| Squash & merge | ✅ | ❌ | ❌ | ❌ |
| Rebase & merge | ✅ | ✅ | ✅ | ❌ |

---

## Golden Rules

- ✅ Always rebase **feature branches onto main** — never the other way around
- ❌ Never rebase `main` or any shared/public branch
- ✅ Use `--ff-only` if you want Git to fail rather than create an unintended merge commit
- ✅ Use squash for messy WIP branches, rebase for clean feature branches
