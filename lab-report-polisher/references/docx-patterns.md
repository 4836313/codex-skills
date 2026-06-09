# DOCX Patterns For Lab Reports

## Minimal Editing Pattern

Use `python-docx` to load the source template and save a new target document. Locate the main content cell or question paragraphs by exact prompt text. Insert content immediately after the relevant paragraph.

```python
from docx import Document
from docx.oxml import OxmlElement
from docx.oxml.ns import qn
from docx.shared import Pt, Inches

doc = Document(source_docx)
cell = doc.tables[0].rows[2].cells[0]
anchor = next(p for p in cell.paragraphs if p.text.strip().startswith("2.1"))
```

## Font And Spacing

Set East Asian and ASCII fonts explicitly. Do not rely on Word defaults.

```python
def set_run_font(run, east_asia="宋体", ascii_font="Times New Roman", size=10.5):
    run.font.name = ascii_font
    run.font.size = Pt(size)
    rpr = run._element.get_or_add_rPr()
    rfonts = rpr.rFonts or OxmlElement("w:rFonts")
    if rfonts.getparent() is None:
        rpr.append(rfonts)
    rfonts.set(qn("w:eastAsia"), east_asia)
    rfonts.set(qn("w:ascii"), ascii_font)
    rfonts.set(qn("w:hAnsi"), ascii_font)
```

Use `w:line=240` for single spacing. A practical body rhythm is `before=0 pt`, `after=4 pt`.

## Code Box OOXML

For code paragraphs, use paragraph shading and borders rather than nested tables when possible.

```python
def apply_code_box(paragraph):
    ppr = paragraph._p.get_or_add_pPr()
    shd = ppr.find(qn("w:shd")) or OxmlElement("w:shd")
    if shd.getparent() is None:
        ppr.append(shd)
    shd.set(qn("w:val"), "clear")
    shd.set(qn("w:fill"), "F7F7F7")

    borders = ppr.find(qn("w:pBdr")) or OxmlElement("w:pBdr")
    if borders.getparent() is None:
        ppr.append(borders)
    for side in ("top", "left", "bottom", "right"):
        el = borders.find(qn(f"w:{side}")) or OxmlElement(f"w:{side}")
        if el.getparent() is None:
            borders.append(el)
        el.set(qn("w:val"), "single")
        el.set(qn("w:sz"), "6")
        el.set(qn("w:space"), "4")
        el.set(qn("w:color"), "BFBFBF")
```

## Safe Blank Page Removal

Only remove the standalone paragraph whose text is exactly `深圳大学学生实验报告用纸`, plus one immediate empty spacer paragraph. Never remove rows from the final table.

```python
def element_text(element):
    return "".join(t.text or "" for t in element.iter(qn("w:t"))).strip()

def remove_stray_report_paper_page(doc):
    body = doc._body._body
    for child in list(body):
        if child.tag == qn("w:p") and element_text(child) == "深圳大学学生实验报告用纸":
            idx = list(body).index(child)
            body.remove(child)
            following = list(body)
            if idx < len(following) and following[idx].tag == qn("w:p") and element_text(following[idx]) == "":
                body.remove(following[idx])
            return
```

## Structural Checks

Before delivery:

- Confirm `指导教师批阅意见`, `成绩评定`, and `备注` are present.
- Confirm the final table still has the expected rows.
- Confirm the standalone `深圳大学学生实验报告用纸` paragraph count is zero if it was removed.
- Run `unzip -t final.docx`.
- Render pages using the Documents skill and inspect the contact sheet.

