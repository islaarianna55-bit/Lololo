# Hasil Pemeriksaan File JSON n8n
Repo: `D:/N8N-DEV/awesome-n8n-templates` (origin: hallobayi/awesome-n8n-templates)
Tanggal: 2026-10-14 | Commit awal audit: 8271103
Cakupan: 369 file `.json` (validasi JSON + struktur workflow + node + koneksi + kredensial)

## STATUS AKHIR (setelah perbaikan)
- JSON valid & struktur benar : **369 file (100%)**
- JSON RUSAK                  : **0 file**
- Koneksi menggantung         : 0
- Nama node duplikat          : 0
- Struktur field hilang       : 0
- Dugaan kredensial bocor     : 0
- Gate CI validasi JSON       : ditambahkan (`.github/workflows/validate-json.yml`)

## Ringkasan temuan awal (sebelum perbaikan)
- JSON valid & struktur benar : 361 file
- JSON RUSAK                  : 8 file
- Koneksi menggantung         : 0
- Nama node duplikat          : 0
- Struktur field hilang       : 0
- Dugaan kredensial bocor     : 0

---

## A. KRITIS — 8 file gagal di-parse

### Pola 1 — Judul ikut tersalin di akhir file (trailing text) — 6 file
Validitas: file berisi 1 workflow lengkap, tetapi ada teks sisa setelah `}` penutup.
n8n akan GAGAL mengimpor file ini.

| File | Sisa teks setelah JSON |
|---|---|
| `AI_Research_RAG_and_Data_Analysis/Recipe Recommendations with Qdrant and Mistral.json` | `sRecipe Recommendations with Qdrant and Mistral` |
| `OpenAI_and_LLMs/AI-Driven Lead Management and Inquiry Automation with ERPNext & n8n.json` | `AI-Driven Lead Management and Inquiry Automation with ERPNext & n8n` |
| `OpenAI_and_LLMs/lemlist __ GPT-3_ Supercharge your sales workflows.json` | `slemlist <> GPT-3: Supercharge your sales workflows` |
| `PDF_and_Document_Processing/Parse PDF with LlamaParse and save to Airtable.json` | `Parse PDF with LlamaParse and save to Airtable` |
| `Notion/Automate Competitor Research with Exa.ai, Notion and AI Agents.json` | judul diulang 3x |
| `AI_Research_RAG_and_Data_Analysis/Host Your Own AI Deep Research Agent with n8n, Apify and OpenAI o3.json` | seluruh workflow disalin 2x (duplikat penuh) |

### Pola 2 — Workflow terduplikasi bertingkat — 2 file
File berisi salinan identik yang di-*append* (dipisah `}{`), bukan satu dokumen.

| File | Jumlah salinan | Node (tiap salinan) | Ukuran di disk vs seharusnya |
|---|---|---|---|
| `AI_Research_RAG_and_Data_Analysis/Host Your Own AI Deep Research Agent with n8n, Apify and OpenAI o3.json` | **2x** | 87 | 144 KB vs ~72 KB |
| `AI_Research_RAG_and_Data_Analysis/🔍 Perplexity Research to HTML_ AI-Powered Content Creation.json` | **4x** | 47 | 136 KB vs ~34 KB |

Cara kerja: setelah `}` penutup yang valid, langsung diikuti `{"id"/"meta"...}` — dokumen yang sama berulang.
Untuk menentukan workflow mana yang benar, ambil **dokumen pertama**; sisanya buang.

### Pola 3 — File kosong — 1 file
| File | Ukuran | Catatan |
|---|---|---|
| `Other/ALL_unique_nodes.json` | **0 byte** | Sudah kosong sejak commit paling awal (`9f037c8`, dari `.txt` juga 0 byte). Tetap direferensikan di `README.md:582` sebagai "Complete node reference" — link RUSAK. |

---

## B. Penyebab akar
Semua berasal dari **satu commit**: `a1346dc` — "file format changed to .json" (enescingoz, 25 Jul 2025).
Commit itu **hanya me-rename 294 file** (semua `R100` = rename murni), bukan menulis ulang isi.
Artinya: teks judul & duplikasi sudah ada sejak file `.txt` aslinya, dan ikut terbawa saat di-rename ke `.json`.
Tidak ada perbaikan setelahnya hingga 198 commit terakhir.

---

## C. DINILAI AMAN — bukan masalah (verifikasi)
- 11 file memakai **community node** (`@apify/...`, `n8n-nodes-pdfmint`, `@cradl/...`, `n8n-nodes-fiwano`,
  `n8n-nodes-gcf`, `n8n-nodes-youtube-transcription`, `@gladiator1st/n8n-nodes-magichour`).
  Ini **sah dan memang didokumentasikan** di README (mis. baris 237, 390, 454-456 menyebut
  "Requires the ... community node"). Bukan bug.
- Tidak ada koneksi menggantung, node tanpa field wajib, atau kredensial bocor pada 361 file valid.

## D. Catatan CI
- `.github/workflows/secret-scan.yml` (gitleaks) — aktif, hanya memindai PR/push.
- `.github/workflows/pr-review.yml` (82 KB) — review pakai LLM, **tidak ada validator JSON**.
  Karena itu 8 file rusak bisa lolos ke repo tanpa terdeteksi. Rekomendasi: tambah langkah
  `python -c "import json,sys;json.load(open(f))"` untuk tiap file `.json` di PR.

## E. Tindakan yang sudah dilakukan (2026-10-14)
1. [x] 7 file diperbaiki: sisa teks dipotong & salinan duplikat dibuang → 1 dokumen JSON valid per file.
       Divalidasi ulang: 7/7 punya `nodes` non-kosong + `connections` lengkap.
2. [x] `Other/ALL_unique_nodes.json` diisi ulang dari data nyata repo: **210 tipe node unik**
       dari **368 workflow** (6.691 instance node). Bukan lagi 0 byte.
3. [x] Referensi usang `ALL_unique_nodes.txt` → `.json` diperbaiki di **13 README**
       (termasuk `README.md`, `README-tr.md`).
4. [x] Gate validasi JSON ditambahkan: `.github/workflows/validate-json.yml` —
       memeriksa tiap file `.json` dapat di-parse, tidak ada teks sisa, punya
       `nodes`+`connections`, dan semua koneksi menunjuk node yang benar-benar ada.
       Dijalankan lokal: **369/369 file lulus**.


Skrip audit & generator (temporary): `C:/Users/asus/AppData/Local/Temp/n8n_check/`
