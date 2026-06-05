# Catatan Integrasi Adcuan (CTA-WA Tracking) — HANDIA Landing Page

> Catatan internal tentang cara kerja Adcuan & cara setup-nya di landing page ini.
> Sumber info: arahan Mas Farhan Sismed (4 Juni 2026).

---

## Apa itu Adcuan?

Adcuan = layanan **CTA-WhatsApp tracking & rotator**. Fungsinya:

1. **Tracking konversi iklan** — tiap klik tombol WA tercatat. Berguna buat ngukur
   iklan (Facebook/TikTok/Google) mana yang menghasilkan chat WA. Biasanya nembak
   pixel/event saat tombol diklik.
2. **Rotasi nomor CS** — kalau punya banyak admin/CS, Adcuan bisa otomatis
   muter-muter nomor tujuan biar beban chat merata.
3. **Link pendek redirect** — tombol ga lagi nunjuk langsung ke `wa.me/...`,
   tapi ke link Adcuan yang me-redirect ke WhatsApp sambil mencatat klik.

---

## 2 Komponen yang Harus Dipasang

### 1. Script di `<head>` (CUKUP 1, BUKAN 12)

```html
<script src="https://plus.adcuan.com/code.js" nomor="1"></script>
```

- Dipasang **sekali aja** di dalam `<head>`. (Sudah terpasang di `index.html`.)
- `nomor="1"` = ID konfigurasi akun Adcuan. Jangan diubah kecuali Mas Farhan kasih
  nilai lain.
- Script ini yang nge-handle tracking/pixel di sisi browser.

### 2. Link Adcuan di tiap tombol CTA

Contoh yang dikasih:
```
https://plus.adcuan.com/ctawa/854-1
```

Format: `https://plus.adcuan.com/ctawa/<AKUN>-<CTA>`
- `854` = ID akun/campaign HANDIA.
- `1` = ID CTA (tombol). Tiap CTA beda bisa dibikin `854-1`, `854-2`, dst.

---

## PENTING: Prefill text (?text=) HILANG

Saat ini tiap tombol di landing page punya pesan otomatis beda-beda, contoh:
```
wa.me/6285111399936?text=Halo HANDIA, saya mau pesan Paket Harmonis...
```

**Kalau diganti ke link Adcuan (`ctawa/854-x`), parameter `?text=` ga kebawa.**
Pesan pembuka di-set di DASHBOARD ADCUAN per-CTA, bukan di URL landing page.

Artinya:
- Mau tiap paket punya pesan pembuka beda → bikin **CTA terpisah di Adcuan** +
  set pesannya di sana.
- Pakai 1 link buat semua → pesan pembuka generik (CS harus nanya mau paket apa).

---

## Berapa Link yang Perlu Dibuat?

**BUKAN 12.** Banyak tombol intent-nya sama. Ada ~8 intent unik:

| # | Intent / Pesan | Tombol di LP | Saran ID Adcuan |
|---|----------------|--------------|-----------------|
| 1 | General chat (tanya umum) | Hero, Final CTA, Sticky bar, Footer, Float WA (5 tombol) | `854-1` |
| 2 | Pesan Paket Harmonis | Featured card | `854-2` |
| 3 | Konsul Dokter dulu | Featured card | `854-3` |
| 4 | Pesan Paket Mood Stabil | Kartu | `854-4` |
| 5 | Pesan Paket Coba Lengkap | Kartu | `854-5` |
| 6 | Pesan Paket Intimate Care | Kartu | `854-6` |
| 7 | Pesan Paket Pemula Menoherb | Kartu | `854-7` |
| 8 | Tanya Satuan | Satuan box | `854-8` |

> 5 tombol "general chat" cukup pakai **1 link yang sama** (`854-1`).
> Jadi total link unik = **8**, bukan 12.

**KEPUTUSAN (4 Juni 2026): pilih GRANULAR — 8 link per-paket.**
Biar bisa tau paket mana paling laku dari LP + pesan pembuka beda tiap paket.

---

## Pesan Pembuka per-CTA (COPAS ke Dashboard Adcuan)

Saat bikin tiap CTA di Adcuan, set "pesan pembuka / default text" pakai ini:

| ID | Nama CTA | Pesan pembuka (copas) |
|----|----------|------------------------|
| `854-1` | General Chat | `Halo HANDIA, saya mau tanya & pilih paket menopause (Rose V / Menoherb).` |
| `854-2` | Paket Harmonis | `Halo HANDIA, saya mau pesan Paket Harmonis (2 Rose V + Menoherb + 2 konsul).` |
| `854-3` | Konsul Dokter | `Halo HANDIA, saya mau konsul dokter dulu.` |
| `854-4` | Paket Mood Stabil | `Halo HANDIA, saya mau pesan Paket Mood Stabil (2 Menoherb + 2 konsul).` |
| `854-5` | Paket Coba Lengkap | `Halo HANDIA, saya mau pesan Paket Coba Lengkap (Rose V + Menoherb + 1 konsul).` |
| `854-6` | Paket Intimate Care | `Halo HANDIA, saya mau pesan Paket Intimate Care (2 Rose V + 1 konsul).` |
| `854-7` | Paket Pemula Menoherb | `Halo HANDIA, saya mau pesan Paket Pemula Menoherb (1 Menoherb + 1 konsul).` |
| `854-8` | Tanya Satuan | `Halo HANDIA, saya mau beli satuan (Rose V / Menoherb).` |

> CATATAN: nomor ID di atas (`854-1` s/d `854-8`) adalah USULAN urutan.
> Kalau dashboard Adcuan kasih ID yang beda (mis. auto-generate), ga masalah —
> tinggal kirim 8 URL final-nya ke yang ngedit `index.html`, nanti dipasang sesuai
> mapping nama CTA di tabel ini.

---

## Cara Kerja Saat User Klik (alur)

```
User klik tombol
   → buka https://plus.adcuan.com/ctawa/854-x
   → Adcuan catat klik + nembak pixel iklan
   → redirect ke WhatsApp (nomor di-set Adcuan, bisa rotasi)
   → chat kebuka dengan pesan pembuka (di-set di dashboard Adcuan)
```

---

## Status di `index.html` — ✅ TERPASANG (6 Juni 2026)

- [x] 8 script `code.js?nomor=1..8` di `<head>`.
- [x] Keputusan: **GRANULAR — 8 link per-paket**.
- [x] 12 tombol sudah di-wire ke link Adcuan (param `?text` dibuang, pesan pembuka
      di-handle Adcuan):
  - `854-1` (general) → Hero, Final CTA, Sticky bar, Footer, Float WA (5 tombol)
  - `854-2` → Pesan Paket Harmonis
  - `854-3` → Konsul Dokter Dulu
  - `854-4` → Pesan Paket Mood Stabil
  - `854-5` → Pesan Paket Coba Lengkap
  - `854-6` → Pesan Paket Intimate Care
  - `854-7` → Pesan Paket Pemula Menoherb
  - `854-8` → Tanya Satuan

> Tombol "Lihat Paket & Harga" di hero TIDAK diubah (itu anchor scroll ke #offer,
> bukan tombol WA).

## Yang Perlu Disiapkan Mas Farhan / Akmal di Dashboard Adcuan

1. Tentukan mau **1 link** atau **8 link per-intent**.
2. Kalau per-intent: bikin CTA `854-1` s/d `854-8` di Adcuan, set **nomor tujuan**
   + **pesan pembuka** masing-masing sesuai tabel di atas.
3. Kasih daftar link final ke yang ngedit `index.html` buat dipasang ke tombol.

## Hal yang Perlu Dicek Setelah Live

- Pastikan `code.js` ga inject widget WA mengambang sendiri yang nabrak tombol
  Float WA custom kita (kanan-bawah). Kalau dobel, salah satu dimatiin.
- Tes klik tiap tombol → harus nyampe ke WA nomor yang benar.
- Cek dashboard Adcuan: klik kebaca/ke-track.
