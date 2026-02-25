# dotfiles

My Claude Code commands and configuration, managed via symlinks.

## Structure

```
~/dotfiles/                     ← git repo (source of truth)
  .claude/
    commands/
      systems-architect.md      ← real file, edit this one
  .zshrc                        ← boilerplate, sources .zshrc.local

~/.claude/
  commands/
    systems-architect.md        ← symlink (just a pointer)
~/.zshrc                        ← symlink (just a pointer)
~/.zshrc.local                  ← plain file, machine-specific, NOT in repo
```

## Editing a command

```bash
# Edit the file
nano ~/dotfiles/.claude/commands/systems-architect.md

# Save to git
cd ~/dotfiles
git add .claude/commands/systems-architect.md
git commit -m "update systems-architect: describe what changed"
git push
```

Changes are live in Claude the moment you save — no restart needed.

## .zshrc setup

`~/dotfiles/.zshrc` is a minimal boilerplate — it just loads the machine-specific config:

```bash
# Machine-specific config (not in repo)
[ -f ~/.zshrc.local ] && source ~/.zshrc.local
```

**`~/.zshrc.local`** is a plain, untracked file on each machine. Put anything device-specific there (custom functions, SSH shortcuts, PATH additions). It is never committed.

As you identify config that belongs on every machine, promote it up into `dotfiles/.zshrc`.

| File | In repo? | Symlinked? | Purpose |
|------|----------|------------|---------|
| `~/dotfiles/.zshrc` | Yes | Source | Shared boilerplate |
| `~/.zshrc` | No | Yes (pointer) | Points to dotfiles |
| `~/.zshrc.local` | No | No (plain file) | Machine-specific, never tracked |

## Restoring on a new machine

```bash
git clone https://github.com/Momo123xx/dotfiles.git ~/dotfiles
mkdir -p ~/.claude/commands
ln -s ~/dotfiles/.claude/commands/systems-architect.md ~/.claude/commands/systems-architect.md
ln -s ~/dotfiles/.zshrc ~/.zshrc
# then create ~/.zshrc.local with this machine's specific config
```

Repeat the `ln -s` line for each file in the repo.

## Restoring files from a past commit

| Goal | Command |
|------|---------|
| Undo a whole commit (safe) | `git revert <hash>` |
| Undo a whole commit (nuclear) | `git reset --hard <hash>` + `git push --force` |
| Restore one or more files from a past commit | `git checkout <hash> -- <file>` |

### git revert (industry standard)
Creates a new commit that undoes a previous one. History stays intact. Use this when you want to roll back a bad commit.

```bash
git log --oneline                  # find the hash to undo
git revert d94ee8e                 # new commit that cancels it out
git push
```

### git reset --hard (nuclear)
Moves the branch back to a past commit, erasing everything after it. Only use this if you're the sole user and want to cleanly wipe a dead end.

```bash
git reset --hard d94ee8e
git push --force                   # required because history was rewritten
```

### git checkout -- file (surgical)
`git revert` only works on whole commits. To restore a specific file (or files) from a past commit without touching anything else:

```bash
git log --oneline                                              # find the hash
git checkout <hash> -- .claude/commands/systems-architect.md  # restore one file
git checkout <hash> -- file1.md file2.md                      # or multiple files
git commit -m "restore file from <hash>"
git push
```

The file is immediately staged after checkout — just commit and push.

## Rule

Always edit files inside `~/dotfiles/`. Never touch the ones in `~/.claude/` directly — they're just pointers.
