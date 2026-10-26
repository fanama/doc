# Groff Quick-Start & Cheat Sheet

## 📋 Getting Started To-Do List

- [ ] **Install Groff**
  - **Linux (Ubuntu/Debian):** `sudo apt install groff`
  - **macOS:** `brew install groff`

- [ ] **Choose a Macro Package**
  - Use `-ms` (manuscripts/documents) or `-man` (man pages) for formatted documents.

- [ ] **Create Your First Document**
  - Save a file named `document.ms`.

- [ ] **Compile Your File**
  - **PDF Output:** `pdfroff -ms document.ms > output.pdf`
  - **PostScript Output:** `groff -ms -Tps document.ms > output.ps`
  - **Plain Text (Terminal Preview):** `groff -ms -Tascii document.ms | less`

- [ ] **View the Generated PDF**
  - Open `output.pdf` in your preferred PDF reader.

---

## 📑 Groff Cheat Sheet (ms Macro Package)

### 1. Document Structure & Headers

```groff
.TL
Title of the Document
.AU
Author Name
.AI
Author Institution / Affiliation
.AB
Abstract text goes here...
.AE
.NH 1
First-Level Section Heading
.NH 2
Second-Level Subsection Heading
.SH
Unnumbered Heading
```

---

### 2. Basic Text & Paragraphs

```groff
.PP
Standard indented paragraph starts here.

.LP
Left-justified block paragraph (no first-line indent).

.IP \(bu 4
Bullet point item (using standard bullet character \(bu).

.IP 1. 4
Numbered list item (4 represents the left indent space).
```

---

### 3. Font Formatting & Styles

```groff
.B "Bold Text"
.I "Italic Text"
.BI "Bold Italic Text"
.CW "Constant Width (Code)"

\fBInline bold text\fP and \fIinline italic text\fP
```

---

### 4. Low-Level Primitive Commands (Requests)

| Command | Action |
| :--- | :--- |
| `.br` | Break line (force new line without a new paragraph). |
| `.sp [n]` | Insert blank vertical space (e.g., `.sp 1i`, `.sp 2`). |
| `.ce [n]` | Center the next *n* lines. |
| `.ll [n]` | Change line length (e.g., `.ll 6.5i`). |
| `.ps [n]` | Change font point size (e.g., `.ps 12`). |
| `.vs [n]` | Change vertical spacing/leading (e.g., `.vs 14p`). |
| `.\"` | Comment line (ignored during rendering). |

---

### 5. Tables (`tbl` Preprocessor)

Compile using: `groff -t -ms document.ms -Tpdf > output.pdf`

```groff
.TS
allbox center;
cb cb
l r.
Header 1	Header 2
Item A	100
Item B	250
.TE
```

---

### 6. Useful Escape Sequences

* `\(bu` — Bullet symbol (`•`)
* `\(em` — Em-dash (`—`)
* `\(de` — Degree symbol (`°`)
* `\fR` — Switch back to Regular font
