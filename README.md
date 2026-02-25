# dotfiles

My Claude Code commands and configuration, managed via symlinks.

## Structure

```
~/dotfiles/                     ← git repo (source of truth)
  .claude/
    commands/
      systems-architect.md      ← real file, edit this one

~/.claude/
  commands/
    systems-architect.md        ← symlink (just a pointer)
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

## Restoring on a new machine

```bash
git clone https://github.com/Momo123xx/dotfiles.git ~/dotfiles
mkdir -p ~/.claude/commands
ln -s ~/dotfiles/.claude/commands/systems-architect.md ~/.claude/commands/systems-architect.md
```

Repeat the `ln -s` line for each command file in the repo.

## Rule

Always edit files inside `~/dotfiles/`. Never touch the ones in `~/.claude/` directly — they're just pointers.
