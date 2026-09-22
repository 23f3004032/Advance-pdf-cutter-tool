# Advance PDF Cutter Tool

A small Streamlit app that splits a big combined PDF into pieces by finding start and stop keywords in the text, instead of making you count pages by hand.

## Why this exists

I built this for myself because IITM term papers and notes usually come as one giant combined PDF with every subject stacked one after another. Digging through fifty-plus pages to pull out just one subject's questions got old fast, so I wrote a script to do it by scanning for where a subject starts and where the next one begins. It worked well enough that a bunch of other students started using it too, so it's stuck around and grown a bit past its original "just for me" scope.

## How it works

You upload a PDF and give it a start keyword (say, `MLF`) and, optionally, a stop keyword (say, `MLT`). The app then:

1. Reads the PDF page by page with `pypdf` and pulls the text out of each page.
2. Uppercases everything and searches for your start keyword using a regex with word boundaries (`\bMLF\b`), so it won't false-positive on something like "EXAMPLE" just because it contains a similar substring.
3. Once it finds the page where the start keyword shows up, it keeps scanning forward looking for a stop trigger. If you gave it a stop keyword, that's the only thing it watches for. If you didn't, it falls back to a built-in list of known IITM subject codes (MLF, MLT, MLP, DBMS, PDSA, MAD, MAD1, MAD2, BDM, SC, MATHS, STATS, PYTHON, ENGLISH, CT, TDS, BA) and stops as soon as any subject other than your target one appears.
4. Whichever page triggers the stop becomes the cutoff. If nothing ever triggers a stop, it just takes everything to the end of the document.
5. All the pages between the start and end are copied into a new PDF with `pypdf`'s `PdfWriter`, and you get a download button for the result.

No OCR, no external binaries — it works purely off whatever text `pypdf` can extract, so it depends on the source PDF having a real, selectable text layer rather than being a scanned image.

## Tech stack

- **Python**
- **Streamlit** — the web UI (file upload, keyword inputs, progress bar, download button)
- **pypdf** — reading pages, extracting text, and writing out the cut PDF
- **re** (standard library) — word-boundary keyword matching

That's the whole stack. It's intentionally a single-file app (`app.py`).

## Running it locally

```bash
git clone https://github.com/23f3004032/Advance-pdf-cutter-tool.git
cd Advance-pdf-cutter-tool
pip install -r requirements.txt
streamlit run app.py
```

Then open the local URL Streamlit prints, upload your PDF, type in a start keyword (and a stop keyword if you want to override the auto-detection), and hit "Extract Paper."

---

**Ankit Singh**
[LinkedIn](https://www.linkedin.com/in/ankit-singh-117925249/)
