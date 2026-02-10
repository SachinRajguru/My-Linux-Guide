
# VI Editor Shortcuts

## Introduction to Vi/Vim Editor
Vi (and its improved version, Vim) is a powerful, modal text editor pre-installed on most Linux systems. It operates in modes: **Normal** (navigation/commands), **Insert** (editing), and **Command-line** (advanced commands via `:`). Vi is efficient for quick edits but has a learning curve—practice with small files.

**Key Concepts**:
- **Modes**: Switch with keys (e.g., `i` for Insert, `Esc` for Normal).
- **Installation**: If not available, `sudo apt install vim` (Vim is preferred over basic Vi).
- **Opening**: `vi filename` or `vim filename` (creates file if it doesn't exist).
- **Safety Tip**: Use `:w` to save frequently. Exit with `:q!` to discard changes.

This section corrects and expands on basic shortcuts. For advanced use, see `man vim`.

---

## Modes in Vi Editor
Vi has three main modes for different tasks.

- **Normal Mode** (default) – For navigation, deletion, and commands. (Enter from Insert with `Esc`).
- **Insert Mode** – For typing text. (Enter with `i`, `a`, etc.; exit with `Esc`).
- **Command-Line Mode** (or Ex Mode) – For file operations, search/replace. (Enter from Normal with `:`).

---

## Basic Navigation (Normal Mode)
Move the cursor efficiently without editing.

- `h` – Move **left** (one character).
- `l` – Move **right** (one character).
- `j` – Move **down** (one line).
- `k` – Move **up** (one line).
- `0` – Move to the **beginning** of the line.
- `^` – Move to the **first non-blank** character of the line.
- `$` – Move to the **end** of the line.
- `w` – Move to the **start of the next word**.
- `b` – Move to the **start of the previous word**.
- `gg` – Move to the **start** of the file.
- `G` – Move to the **end** of the file.
- `:n` – Move to **line number `n`** (e.g., `:50` for line 50).

**Tip**: Use `Ctrl + U` (up half-page) and `Ctrl + D` (down half-page) for large files.

---

## Insert Mode Shortcuts
Enter Insert mode to add/edit text. Always return to Normal with `Esc`.

- `i` – Insert **before** the cursor.
- `I` – Insert at the **beginning** of the line.
- `a` – Append **after** the cursor.
- `A` – Append at the **end** of the line.
- `o` – Open a **new line below** the current line.
- `O` – Open a **new line above** the current line.
- `Esc` – Exit Insert mode (back to Normal).

**Example**: Press `i`, type "Hello", then `Esc` to stop editing.

---

## Editing Text (Normal Mode)
Perform deletions, copies, and pastes.

- `x` – Delete a **single character** under the cursor.
- `X` – Delete the **character before** the cursor.
- `dw` – Delete from cursor to **end of word**.
- `dd` – Delete the **entire line**.
- `d$` – Delete from cursor to **end of line** (same as `D`).
- `d0` – Delete from cursor to **beginning of line**.
- `D` – Delete from cursor to **end of line**.
- `u` – **Undo** the last action.
- `Ctrl + r` – **Redo** an undone change.
- `yy` – **Yank (copy)** the current line.
- `yw` – **Yank (copy)** from cursor to end of word.
- `p` – **Paste** after the cursor.
- `P` – **Paste** before the cursor.

**Tip**: Combine with numbers (e.g., `3dd` deletes 3 lines). Use visual mode (`v`) for selections.

---

## Search and Replace (Command-Line Mode)
Search patterns and replace text.

- `/pattern` – Search **forward** for "pattern" (e.g., `/error`).
- `?pattern` – Search **backward** for "pattern".
- `n` – Repeat the last search **forward**.
- `N` – Repeat the last search **backward**.
- `:%s/old/new/g` – Replace **all occurrences** of "old" with "new" in the file.
- `:s/old/new/g` – Replace **all occurrences** in the **current line**.

**Examples**:
- `:%s/foo/bar/g` → Replaces all "foo" with "bar".
- `:%s/old/new/gc` → Adds confirmation (`c` for confirm).

**Tip**: Case-sensitive by default; use `\c` for ignore case (e.g., `/\cpattern`).

---

## Working with Multiple Files (Command-Line Mode)
Handle multiple files or windows.

- `:e filename` – **Open/edit** a new file (switches to it).
- `:w` – **Save** the current file.
- `:wq` – **Save and quit**.
- `:q!` – **Quit without saving** (discards changes).
- `:split filename` – **Split screen horizontally** and open another file.
- `:vsplit filename` – **Split screen vertically**.
- `Ctrl + w + w` – **Switch** between split windows.

**Tip**: Use `:ls` to list open buffers. For tabs (Vim-specific), `:tabe filename`.

---

## Additional Notes and Best Practices
- **Saving/Quitting**: `:wq!` forces save+quit (overwrites read-only). `:x` is like `:wq`.
- **Visual Mode**: Press `v` for character selection, `V` for line selection—then edit (e.g., `d` to delete).
- **Customization**: Edit `~/.vimrc` for settings (e.g., `set number` for line numbers).
- **Common Pitfalls**: Don't edit in Normal mode—use Insert. If stuck, `Esc` returns to Normal.
- **Interview Questions**:
  - How to exit Vi? `:q!` or `:wq`.
  - Difference between Vi and Vim? Vim has more features (e.g., syntax highlighting).
- **Practice**: Create a file with `vi test.txt`, add text, search/replace, and save. Use `vimtutor` for interactive tutorial.

## Resources
- Official Docs: `man vi` or `man vim`.
- GitHub Repo: Full examples in `README.md`.
- Tutorials: `vimtutor` (built-in) or online Vim cheatsheets.

Master Vi for efficient Linux editing—start with basics and build up!

---