# Wrapper Vercel — PB SMS (Form Pengajuan Barang)

Vercel di sini hanya berfungsi sebagai **pintu depan / domain rapi**.
Seluruh tampilan dan logika tetap berada di Google Apps Script.

## Isi folder
- `index.html` — pembungkus (iframe) ke web app Apps Script
- `vercel.json` — konfigurasi header & cleanUrls
- `README.md` — catatan ini

## Langkah deploy

1. **Deploy Apps Script dulu**
   - Apps Script → *Deploy* → *New deployment* → type **Web app**
   - Execute as: **Me**
   - Who has access: **Anyone** (wajib, agar bisa dibuka dari domain Vercel tanpa login Google)
   - Salin URL yang berakhiran `/exec`

2. **Isi URL di `index.html`**
   ```js
   var URL_APPS_SCRIPT = "https://script.google.com/macros/s/AKfycb.../exec";
   ```

3. **Deploy ke Vercel**
   - Vercel → *Add New* → *Project* → import repo (atau drag & drop folder ini)
   - Framework Preset: **Other**
   - Build Command: kosongkan
   - Output Directory: kosongkan (root)
   - Deploy

4. **Selesai.** Semua perubahan tampilan selanjutnya:
   - Edit `Index.html` di Apps Script
   - *Deploy* → *Manage deployments* → ikon pensil (Edit) → Version: **New version** → *Deploy*
   - URL `/exec` tidak berubah, jadi Vercel **tidak perlu di-deploy ulang**

## Catatan penting

- `doGet()` di `Kode.gs` sudah memakai `XFrameOptionsMode.ALLOWALL`. **Jangan dihapus**, karena tanpa itu iframe akan diblokir browser.
- Jika deployment Apps Script diatur "Only myself", pengguna cabang akan melihat halaman login Google di dalam iframe. Pakai **Anyone**.
- Tombol *Cetak Form* tetap berfungsi (window.print dijalankan dari dalam iframe). Di iOS Safari, jika hasil cetak kosong, buka URL `/exec` langsung di tab baru lalu cetak.
