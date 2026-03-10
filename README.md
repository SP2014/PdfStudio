# PDF Crop & Layout Studio

A professional, server-powered web application for precisely cropping PDF pages and arranging multiple copies in a custom grid layout on a single output page.

---

## Features

- **Interactive crop editor** — drag to draw a crop region; drag handles to resize; drag the box to reposition
- **Precise dimension input** — type exact values in mm, px, or inches
- **Preset dimensions** — A4, A5, Business Card, Label, Square, Half A4
- **Custom grid layouts** — any rows × columns combination (up to 8×8)
- **Quick layout chips** — 1×1, 2×1, 1×2, 2×2, 3×1, 1×3, 3×2, 2×3
- **Output page sizes** — A4, A3, US Letter, A5
- **Portrait / Landscape** output orientation
- **Margin & gutter control** in millimetres
- **Aspect ratio preservation** with centring
- **Cut / crop marks** as dashed borders
- **Multi-page PDF support** — select any page to crop
- **Server-side PDF rendering** — vector-quality export via ReportLab + pypdfium2
- **Auto session cleanup** — uploaded files deleted when session ends

---

## Setup

### 1. Install Python dependencies

```bash
pip install -r requirements.txt
```

### 2. Run the server

```bash
python app.py
```

### 3. Open in browser

```
http://localhost:5000
```

---

## Project Structure

```
pdf_studio/
├── app.py                  # Flask backend (API + routing)
├── requirements.txt        # Python dependencies
├── templates/
│   └── index.html          # Main HTML template
├── static/
│   ├── css/
│   │   └── app.css         # Stylesheet
│   └── js/
│       └── app.js          # Frontend logic (UI, Crop, Layout modules)
├── uploads/                # Temporary uploaded PDFs (auto-cleaned)
└── outputs/                # (Reserved for future batch export)
```

---

## API Reference

| Endpoint | Method | Description |
|---|---|---|
| `GET /` | GET | Serve the main application |
| `POST /api/upload` | POST | Upload a PDF; returns session_id, page_count, page dimensions |
| `GET /api/render/<sid>/<page>` | GET | Render a PDF page as PNG (`?dpi=150`) |
| `GET /api/crop-preview-img/<sid>/<page>` | GET | Serve the cropped region as PNG (`?x&y&w&h` in mm) |
| `POST /api/export` | POST | Generate and download the final layout PDF |
| `DELETE /api/session/<sid>` | DELETE | Clean up session and uploaded file |

---

## Keyboard Shortcuts

| Key | Action |
|---|---|
| `Esc` | Clear crop selection |
| `Ctrl+A` | Select entire page |
| `1` / `2` / `3` | Switch between steps |

---

## Production Deployment

For production use, replace the Flask dev server with **Gunicorn**:

```bash
pip install gunicorn
gunicorn -w 4 -b 0.0.0.0:5000 app:app
```

For high-volume deployments, consider:
- **Redis** for session storage instead of in-memory dict
- **Celery** for async PDF generation
- **S3 / GCS** for file storage instead of local disk
- **Nginx** as reverse proxy with `client_max_body_size 50m`

---

## License

MIT — free for commercial and personal use.
# PdfStudio
