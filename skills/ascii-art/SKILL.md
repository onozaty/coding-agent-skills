---
name: ascii-art
description: Skill for drawing screen layouts, diagrams, and tables using ASCII art. Use this skill whenever drawing wireframes, ASCII diagrams, text diagrams, box diagrams, or flow diagrams — especially when full-width characters are mixed in.
---

# ASCII Art Drawing Skill (full-width character support)

## Prerequisites & Rules

### Output Format
- **Always wrap in a code block (` ``` `)**
- Do not use ASCII art outside code blocks
- Do not mix Markdown decorations (`**`, `_`, etc.) inside ASCII art

### Monospace Font Assumption
- Half-width characters (ASCII letters, numbers, symbols) = display width **1**
- Full-width characters (CJK, full-width symbols) = display width **2**
- All lines must have the same total display width

---

## Character Sets (pick one and never mix)

### Simple (recommended, high compatibility)
```
Corners: +
Horizontal: -
Vertical: |
Space: half-width space
```

### Extended (looks nicer)
```
Corners: ┌ ┐ └ ┘ ├ ┤ ┬ ┴ ┼
Horizontal: ─
Vertical: │
```

---

## Character Width Calculation Rules

### Basic Rule
- Half-width character (ASCII letters/numbers/symbols) = width **1**
- Full-width character (CJK, full-width symbols) = width **2**

The display width of a line is the sum of each character's width, counted one by one.

### How to Determine Space Count (Important)

**Never count spaces by eye. Always calculate.**

```
trailing_spaces = target_inner_width - content_display_width - leading_spaces
```

#### Example: Center "フッター" (Footer) in a box with inner width 27

1. Calculate each character: フ(2)+ッ(2)+タ(2)+ー(2) = **8**
2. Choose leading spaces by design: **8**
3. Trailing spaces = 27 - 8 - 8 = **11**
4. Result: `|        フッター           |` (8 + 8 + 11 = 27) ✓

### Example: Column Width Calculation with Full-Width Text

```
// NG (misaligned)
+----------+------+
| 名前      | 年齢  |
| Alice    | 25   |

// OK (aligned)
+----------+------+
| 名前     | 年齢 |
| Alice    | 25   |
```

"名前" is 2 full-width chars → display width 4. Trailing spaces = cell inner width(8) - leading(1) - 4 = **3**.

---

## Drawing Steps

1. **Fix the outer frame first** (decide width and height)
2. **Decide column widths** (find max display width per column, account for full-width chars)
3. **Calculate space counts before drawing each line**
   - Sum the width of each character in the content one by one
   - Use `trailing_spaces = target_inner_width - content_width - leading_spaces`
   - Place exactly that many spaces (never adjust by eye)
4. **Review the checklist** (see below)

---

## Common Misalignment Patterns

| Pattern | Cause | Fix |
|---|---|---|
| Full-width column shifts right | Full-width chars counted as width 1 | Recount with full-width = 2 |
| Header and data rows misalign | Different padding amounts | Align display width across all rows |
| Broken after copy-paste | Invisible chars or full-width spaces mixed in | Use only half-width spaces |
| Nested structure misaligns | Drew inner frame before outer | Draw outer frame first, then inner |
| Space count is wrong | Adjusted spaces by eye | Calculate: `trailing = target - content - leading` |

---

## Pre-output Checklist

- [ ] Wrapped in a code block?
- [ ] Calculated space counts with `target_width - content_width - leading_spaces` (no eyeballing)?
- [ ] Summed each full-width character individually as width 2?
- [ ] Character set is consistent (no mixing `+` and `┌`)?
- [ ] No full-width spaces mixed in (use only half-width spaces)?

---

## Correct Examples

### Table with full-width text

```
+----------+--------+----------+
| 項目     | 値     | 備考     |
+----------+--------+----------+
| 名前     | Alice  | 英語名   |
| 年齢     | 25     |          |
| 住所     | 東京   | 都内     |
+----------+--------+----------+
```

### Screen layout

```
+---------------------------+
|    ヘッダー (Header)      |
+-----------+---------------+
| メニュー  | コンテンツ    |
| ・ホーム  |               |
| ・設定    |               |
+-----------+---------------+
|        フッター           |
+---------------------------+
```

### Flow diagram

```
+----------+     +----------+     +----------+
| 入力画面 | --> | 確認画面 | --> | 完了画面 |
+----------+     +----------+     +----------+
                      |
                      v
               +------------+
               | エラー表示 |
               +------------+
```
