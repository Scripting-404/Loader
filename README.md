# raw

Pemendek link raw GitHub dengan landing page papan chip beranimasi (tema dark biru).

## Pasang

1. Buat repo bernama `raw`, lalu unggah `index.html`, `404.html`, dan `links.json` ke root repo.
2. Buka Settings → Pages, pilih branch `main` dan folder `/ (root)`, lalu simpan.
3. Situs aktif di `https://USERNAME.github.io/raw/`.

`404.html` adalah salinan persis `index.html`. GitHub Pages memakainya untuk alamat seperti `/raw/app`, yang tidak punya berkas sendiri. Setiap kali `index.html` diubah, salin ulang ke `404.html`.

## Tambah tautan

Edit `links.json`:

```json
{ "slug": "app", "raw": "https://raw.githubusercontent.com/USERNAME/REPO/main/app.js" }
```

Tautan pendeknya: `https://USERNAME.github.io/raw/app`

Hanya URL yang diawali `https://raw.githubusercontent.com/` yang diterima.

## Status mesin

Papan chip di beranda punya empat status. Lampu di atas chip menunjukkan status yang aktif.

| Status | Warna | Chip, kipas, dan mekanisme lain |
|---|---|---|
| Siaga | biru, lampu hijau berdenyut | Kipas berputar pelan, jalur diam, memori rendah, roda gigi berhenti |
| Memproses | kuning | Chip menyala dan dipindai, data mengalir di jalur, kipas kencang, roda gigi berputar, memori dan bus berdenyut |
| Selesai | hijau | Chip hijau, memori penuh, bus menyala, lalu peramban dibuka ke berkas raw |
| Galat | merah, lampu merah berkedip | Kipas berhenti, jalur dan bus merah |

Galat muncul kalau `links.json` gagal dimuat atau slug tidak ada.

## Animasi baris tautan

Setiap baris punya mekanisme sendiri yang berjalan saat tautannya diminta. Jenisnya dipilih dari ekstensi berkas. Ubah dengan field `"mech"`.

| Mekanisme | Ekstensi bawaan | Nilai `mech` |
|---|---|---|
| Roda gigi | js, mjs, ts, jsx | `gear` |
| Sabuk | json, yaml, yml, toml, csv | `belt` |
| Piston | py, sh, rb, go | `piston` |
| Rol cetak | md, txt, html, css | `press` |
| Cam | lainnya | `cam` |

## Uji lokal

Ubah `<meta name="base" content="/raw/">` menjadi `content="/"`, jalankan `python3 -m http.server`, lalu buka `http://localhost:8000/?r=app`.
