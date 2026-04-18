# DOCX Report Generation Guide

Reference for generating professional DOCX reports with docx-js.

## Setup

```bash
npm install -g docx
```

## CJK Font Configuration

Always configure both ASCII and East Asian fonts:

```javascript
const font = { ascii: "Arial", hAnsi: "Arial", eastAsia: "Microsoft YaHei" };
```

## Document Structure

Use exactly ONE section. All content goes in `sections[0].children`.

```javascript
const doc = new Document({
  styles: { default: { document: { run: { font, size: 20 } } },
  sections: [{ properties: { page: { size: { width: 11906, height: 16838 } } }, children: [...] }]
});
```

## Color Palette

```javascript
const C = {
  primary: "1A3C6E",    // Deep navy (headings)
  accent: "2E75B6",     // Blue (sub-headings)
  success: "2D8B4E",    // Green (positive)
  warning: "D4760A",    // Orange (caution)
  danger: "C0392B",     // Red (negative)
  gray: "666666",
  lightGray: "F2F2F2",
  altRow: "F0F4FA",
  softBlue: "E3F2FD",   // Info box background
  softGreen: "E8F5E9",  // Success box background
  softYellow: "FFF8E1", // Warning box background
  softRed: "FDECEA",    // Danger box background
};
```

## Reusable Components

### Table Cell

```javascript
function cell(text, opts = {}) {
  const { width, bold, color, bg, align, fontSize, colspan } = opts;
  return new TableCell({
    borders: { top: border, bottom: border, left: border, right: border },
    width: width ? { size: width, type: WidthType.DXA } : undefined,
    margins: { top: 50, bottom: 50, left: 90, right: 90 },
    shading: bg ? { fill: bg, type: ShadingType.CLEAR } : undefined,
    columnSpan: colspan,
    children: [new Paragraph({
      alignment: align || AlignmentType.LEFT,
      spacing: { before: 0, after: 0, line: 260 },
      children: [new TextRun({ text, bold: bold || false, color: color || "333333", font, size: fontSize || 19 })]
    })]
  });
}
```

### Header Cell

```javascript
function hCell(text, width) {
  return cell(text, { width, bold: true, color: C.headerText, bg: C.headerBg, align: AlignmentType.CENTER });
}
```

### Info Box (Colored Callout)

```javascript
function infoBox(title, text, bgColor) {
  return new Table({
    width: { size: 100, type: WidthType.PERCENTAGE },
    columnWidths: [9400],
    rows: [new TableRow({ cantSplit: true, children: [
      new TableCell({
        borders: {
          top: { style: BorderStyle.SINGLE, size: 1, color: "E0E0E0" },
          bottom: { style: BorderStyle.SINGLE, size: 1, color: "E0E0E0" },
          left: { style: BorderStyle.SINGLE, size: 6, color: C.accent },
          right: { style: BorderStyle.SINGLE, size: 1, color: "E0E0E0" }
        },
        shading: { fill: bgColor || C.softBlue, type: ShadingType.CLEAR },
        width: { size: 9400, type: WidthType.DXA },
        margins: { top: 80, bottom: 80, left: 140, right: 140 },
        children: [
          new Paragraph({ spacing: { before: 0, after: 40 }, children: [new TextRun({ text: title, bold: true, color: C.primary, font, size: 20 })] }),
          new Paragraph({ spacing: { before: 0, after: 0, line: 270 }, children: [new TextRun({ text, color: "444444", font, size: 19 })] }),
        ]
      })
    ] })]
  });
}
```

### Skill Status Indicators

Use colored circles with text:
- Active: `"\uD83D\uDFE2 \u6D3B\u8DC3"` (green circle)
- Stagnant: `"\uD83D\uDFE1 \u505C\u6EDE"` (yellow circle)
- Outdated: `"\uD83D\uDD34 \u8FC7\u65F6"` (red circle)

### Star Ratings

```javascript
function stars(n) { return "\u2605".repeat(n) + "\u2606".repeat(5 - n); }
```

## Table Rules

- Always set `width: { size: 100, type: WidthType.PERCENTAGE }` on tables
- Always set `cantSplit: true` on TableRow
- Column widths must sum to table width
- Use alternating row colors with `bg: C.altRow`

## Post-Generation

```bash
python /mnt/appuserdata/builtin/work/hebe/skills/docx/scripts/sanitize.py output.docx
```

## Page Break

```javascript
new Paragraph({ children: [new PageBreak()] })
```
