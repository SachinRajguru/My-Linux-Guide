
# Section 5: Vi Editor Shortcuts

4. [Section 5: Vi Editor Shortcuts](#section-5-vi-editor-shortcuts)
   - [Introduction to Vi and Vim](#introduction-to-vi-and-vim)
   - [Understanding Vi Modes](#understanding-vi-modes)
   - [Opening and Closing Files](#opening-and-closing-files)
   - [Basic Editing and Writing](#basic-editing-and-writing)
   - [Navigation Shortcuts](#navigation-shortcuts)
   - [Advanced Editing Shortcuts](#advanced-editing-shortcuts)
   - [Search and Replace](#search-and-replace)
   - [Alternatives to Vi](#alternatives-to-vi)
   - [Common Pitfalls and Best Practices](#common-pitfalls-and-best-practices-section-5)
   - [Summary and Exercises](#summary-and-exercises-section-5)

## Introduction to Vi and Vim
Vi (Visual Editor) is a modal text editor for efficient file editing in terminals. Vim (Vi Improved) adds enhancements but shares core commands. Essential for Linux admins, as it's ubiquitous and powerful for large files.

## Understanding Vi Modes
- **Normal Mode**: Navigation and commands (default; press Esc).
- **Insert Mode**: Writing text (enter with 'i').
- **Command Mode**: File operations (enter with ':').

## Opening and Closing Files
- Open: `vi <filename>` or `vim <filename>`.
- Save/Quit: `:wq` (save and quit); `:q!` (quit without saving); `:q` (quit if no changes).

## Basic Editing and Writing
- Enter Insert: 'i' (insert at cursor).
- Write Text: Type normally.
- Exit Insert: Esc.

## Navigation Shortcuts
- Movement: h/j/k/l (left/down/up/right); 0 (line start); $ (line end).
- Lines: `:0` (first); `:500` (line 500); Shift+G (last).
- Scrolling: Arrow keys.

## Advanced Editing Shortcuts
- Delete Line: `dd`.
- Undo: `u`.
- Yank (Copy) Line: `yy`; Paste: `p`.
- Insert Line: Shift+O (above).

## Search and Replace
- Search: `/<text>` (forward); `?<text>` (backward); 'n' (next).
- Replace: `:%s/old/new/g` (all occurrences).

## Alternatives to Vi
- Nano: `nano <filename>` (simpler; Ctrl+O to save, Ctrl+X to quit).

## Common Pitfalls and Best Practices
- **Pitfalls**: Forgetting modes; accidental deletions.
- **Best Practices**: Practice in a test file; use `:wq` habitually.

## Summary and Exercises
Vi is modal for efficiency. Master modes and shortcuts.

**Exercises**:
1. Create "practice.txt" with Vi, write 5 lines, save.
2. Navigate to line 3, delete it, undo.
3. Search for a word and replace it.

---