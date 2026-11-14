# Tony Study Notes

Static study notes for [wngtk/tech-study-notes](https://github.com/wngtk/tech-study-notes). The site title is **Tony Study Notes**. Pages are served from the `docs/` directory so the same relative links work on a local Python web server and on the GitHub project site at `/tech-study-notes/`.

The reader shell (HTML, CSS, and JavaScript) comes from [lecture-to-notes](https://github.com/ysyecust/lecture-to-notes). This repository publishes one course: Ssali Jonathan’s FastAPI Beyond CRUD series, 26 Chinese lecture PDFs, 777 pages. The published catalogue is `docs/data/catalog.json`. The course record used to build that catalogue is `content/courses/fastapi-beyond-crud/course.json`.

## Clone and read locally

After this pull request is merged, clone the default branch `main`:

```bash
git clone https://github.com/wngtk/tech-study-notes.git
cd tech-study-notes
python3 -m http.server 8000 --directory docs
```

Open [http://127.0.0.1:8000/](http://127.0.0.1:8000/).

Before merge, the site is on the pull request branch. Preview that branch instead of `main`:

```bash
git clone --branch cursor/tony-study-notes-site-ed47 https://github.com/wngtk/tech-study-notes.git
cd tech-study-notes
python3 -m http.server 8000 --directory docs
```

From whichever clone you just made, serve the same files under the project subpath GitHub Pages uses:

```bash
mkdir -p "$HOME/tony-notes-preview"
ln -sfn "$(pwd)/docs" "$HOME/tony-notes-preview/tech-study-notes"
python3 -m http.server 8001 --directory "$HOME/tony-notes-preview"
```

Open [http://127.0.0.1:8001/tech-study-notes/](http://127.0.0.1:8001/tech-study-notes/).

Keep the server rooted at `docs/` (or at a directory whose `tech-study-notes` entry is `docs/`). Asset URLs are relative to the page, including `data/catalog.json`, `pdfs/`, `thumbnails/`, and `assets/vendor/pdfjs/`.

## GitHub Pages

`docs/.nojekyll` is present so Pages serves the files directly. A repository admin turns Pages on after `main` contains `docs/` (after this pull request is merged):

1. Open [https://github.com/wngtk/tech-study-notes/settings/pages](https://github.com/wngtk/tech-study-notes/settings/pages).
2. Under **Build and deployment**, set **Source** to **Deploy from a branch**.
3. Set the branch to `main` and the folder to `/docs`.
4. Save.

The published URL is [https://wngtk.github.io/tech-study-notes/](https://wngtk.github.io/tech-study-notes/).

## Licenses and attribution

- The reader shell is GNU GPL v3. See [`docs/LICENSE`](docs/LICENSE) and [`docs/ATTRIBUTION.txt`](docs/ATTRIBUTION.txt).
- PDF.js and its bundled fonts, cmaps, and WASM components keep the license files already stored under `docs/assets/vendor/pdfjs/`.
- The FastAPI Beyond CRUD lecture PDFs are study notes included for reading. They are not relicensed by `docs/LICENSE`. Leave each PDF’s bytes, filename, and source citation unchanged.
- Course and lecture citations (instructor, YouTube URL, page count) live on the course record and in `docs/data/catalog.json`.

## Current course

| Field | Value |
| --- | --- |
| Catalogue id | `fastapi-beyond-crud` |
| Lectures | 26 |
| Pages | 777 |
| PDFs | `docs/pdfs/fastapi_beyond_crud_01_…_zh.pdf` through `…_26_…_zh.pdf` |
| Thumbnails | `docs/thumbnails/*.webp`, one per lecture, named from the PDF SHA-256 prefix |
| Source course record | `content/courses/fastapi-beyond-crud/course.json` |
| Playlist | https://www.youtube.com/playlist?list=PLEt8Tae2spYnHy378vMlPH--87cfeh33P |

## Adding a course

1. Create `content/courses/<course-id>/course.json`. Use the FastAPI course record as the field list: `id`, `title`, `institution`, `term`, `description`, `tags`, `source_url`, `featured`, and `items`.
2. Each item needs `file`, `title`, `order`, `expected_pages`, `duration_minutes`, `instructor`, `source_url`, `source_label`, and `meta`. `file` is the PDF filename only.
3. Copy the PDF into `docs/pdfs/` without rewriting it. Record the byte size and SHA-256.
4. Add a WebP thumbnail at `docs/thumbnails/<sha256-prefix>.webp` when the catalogue item has a `thumbnail`.
5. Add the course object and one item per lecture to `docs/data/catalog.json` (`schema_version` 1). A lecture item uses `kind: "lecture"`, plus `id`, `course_id`, `title`, `order`, `pages`, `bytes`, `sha256`, `pdf` (path relative to `docs/`, such as `pdfs/name.pdf`), `thumbnail`, `instructor`, `source_url`, `source_label`, and `meta`.
6. Point the course’s `item_ids` at those item ids, in lecture order. Update `item_count`, `lecture_count`, `page_count`, and the top-level `stats` (`course_count`, `lecture_count`, `page_count`, `paper_count`, `pdf_bytes`).
7. Keep the new PDF out of the GPL. Keep the instructor and source URL on both the course record and the catalogue item.

The reader opens a lecture at `reader.html?id=<item id>`.

## Adding an article

PDF lectures in this import do not include a web article. To add a reflowable article later, set the catalogue item’s `web` object to `article`, `document`, and `sha256`. `article` is the HTML path, `document` is the chapter JSON path, and `sha256` is the SHA-256 of the article HTML. The reader checks that hash before displaying the article. Put those files under `docs/` and keep their paths relative.

## Adding a paper

`docs/data/catalog.json` has `"papers": []`, so the papers page reports that none are listed. To list one, append an object with `title`, `meta`, `url`, and an optional `icon`. `url` is a relative path such as `papers/<name>__paper.html`. Place the article under `docs/papers/`. The papers page reads only the `papers` array.

The 提交 PDF page still links to the lecture-to-notes contribution guide. Material for this site is added in this repository with the steps above.

## Leave out

Do not add skills, authoring models, video, caches, virtual environments, credentials, or private filesystem paths. Do not replace unrelated files already in the repository when importing another course.
