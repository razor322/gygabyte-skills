# 🛠️ Gygabyte Skills

Agent Skills untuk AI coding agent, oleh [@gygabyte.lab](https://www.instagram.com/gygabyte.lab) ([razor322](https://github.com/razor322) di GitHub).

[![npm version](https://img.shields.io/npm/v/gygabyte-skills?color=cb3837&logo=npm)](https://www.npmjs.com/package/gygabyte-skills)
[![license](https://img.shields.io/npm/l/gygabyte-skills)](https://github.com/razor322/gygabyte-skills/blob/main/LICENSE)
[![skills.sh](https://skills.sh/b/razor322/gygabyte-skills)](https://skills.sh/razor322/gygabyte-skills)

[English](README.md) · **[Bahasa Indonesia](README.id.md)**

| Skill | Kegunaan |
| --- | --- |
| `ai-assisted-coding` | 🤖 Bangun dan debug aplikasi dengan AI coding agent memakai disiplin GrokBot dari Lauren Tan — workaround bersih, tanpa komentar penopang, kesalahan berulang jadi aturan lint/CI, satu pola standar, satu CLI verifikasi yang bisa dipakai ulang, dan Feature Map |
| `core-engineering-standards` | 🏗️ Tulis, review, dan refactor kode siap produksi — SRP, fungsi pendek dengan early return, typing eksplisit, tanpa hardcoded value, arsitektur 3 lapis, error handling eksplisit, schema validation, dan diff minimal |

## 📊 Apa bedanya kalau skill ini dipakai?

Bayangkan dua orang diberi tugas yang sama persis. Satu pakai skill ini, satu tidak. Kode keduanya **sama-sama jalan** — tapi yang pakai skill menghasilkan kode yang lebih rapi, lebih aman, dan lebih mudah dilanjutkan orang lain (atau AI) berikutnya.

### 🔍 Contoh perubahan yang langsung terlihat

- 🌀 **Kode tidak lagi bertingkat-rumit** — dari piramida `if` bersarang jadi alur yang datar dan mudah dibaca.
- 🏷️ **Semua fungsi bertipe jelas** — parameter dan nilai kembaliannya dinyatakan eksplisit.
- 🔢 **Tidak ada angka atau teks "ajaib"** — semuanya diberi nama di satu tempat, gampang diubah.
- 💬 **Tidak ada komentar "jangan diubah" / `TODO`** — aturan ditulis lewat lint dan test, bukan tempelan.
- 🧾 **Ada cara buktikan kode jalan** — satu perintah verifikasi, plus peta fitur supaya AI tidak menebak-nebak.

> Ini adalah contoh ilustratif dari tugas kecil yang kami coba (registrasi user, endpoint API, dan satu file proyek nyata). Bukan jaminan angka pasti di proyek Anda — hasilnya tetap bergantung pada basis kode dan cara AI dipakai. Yang kami tunjukkan di sini adalah **bentuk dampaknya**, bukan angka yang dijanjikan.

## 📦 Instalasi

### 🧰 Dengan skills CLI (disarankan — jalan untuk 40+ agent)

```bash
# semua skill di repo ini
npx skills add razor322/gygabyte-skills

# satu skill saja
npx skills add razor322/gygabyte-skills --skill ai-assisted-coding
npx skills add razor322/gygabyte-skills --skill core-engineering-standards

# global, untuk semua proyek
npx skills add razor322/gygabyte-skills -g
```

### 🎭 Claude Code (plugin)

```sh
claude plugin marketplace add razor322/gygabyte-skills
claude plugin install gygabyte@gygabyte-skills
```

Di dalam sesi Claude Code, perintahnya `/plugin marketplace add razor322/gygabyte-skills` dan `/plugin install gygabyte@gygabyte-skills`. Mulai sesi baru, lalu pakai `/skills` untuk memeriksa instalasi atau `/gygabyte:ai-assisted-coding` untuk memanggil skill-nya.

### ⚡ Codex (plugin)

```sh
codex plugin marketplace add razor322/gygabyte-skills
codex plugin install gygabyte@gygabyte-skills
```

Di dalam sesi Codex, perintahnya `/plugin marketplace add razor322/gygabyte-skills` dan `/plugin install gygabyte@gygabyte-skills`.

### 📥 Dari npm

```bash
npm install gygabyte-skills
# file skill ada di:
# node_modules/gygabyte-skills/skills/ai-assisted-coding/SKILL.md
# node_modules/gygabyte-skills/skills/core-engineering-standards/SKILL.md
```

Atau install pakai skills CLI:

```sh
npx skills add razor322/gygabyte-skills
```

Lalu salin folder itu ke direktori skill agent Anda, contohnya:

```bash
# Claude Code (proyek)
cp -r node_modules/gygabyte-skills/skills/. .claude/skills/
# Claude Code (global)
cp -r node_modules/gygabyte-skills/skills/. ~/.claude/skills/
# Codex
cp -r node_modules/gygabyte-skills/skills/. .agents/skills/
# opencode / OpenCode
cp -r node_modules/gygabyte-skills/skills/. .opencode/skills/
```

### 🖐️ Instalasi manual

Salin seluruh direktori leaf skill, termasuk `references` dan `assets`-nya, ke direktori skill agent Anda. Lihat bagian npm di atas untuk path tiap agent.

### 📄 Agent Anda tidak mendukung skills? Pakai file aturan proyek

Salin `templates/AGENTS.md` ke root proyek sebagai `AGENTS.md` (atau `CLAUDE.md`) lalu isi standar khusus proyek. Aturan dari kedua skill, tanpa perlu skill loader.

## 🧩 Skills

### 🏗️ core-engineering-standards

Menulis, review, dan refactor kode siap produksi di bahasa atau framework apa pun. Mencakup kode idiomatik yang bersih, SRP, fungsi di bawah 30-40 baris, early return dengan maksimal dua level nesting, typing eksplisit, tanpa hardcoded value, pemisahan lapisan presentation/application/infrastructure tanpa kontaminasi silang, error handling eksplisit dengan log dev terstruktur dan pesan user yang sudah disanitasi, schema validation di batas eksternal, dan diff minimal yang bedah. Bekerja bersama `ai-assisted-coding`: skill itu mengatur cara agent bekerja dengan AI, skill ini mengatur cara kodenya ditulis.

Instruksi lengkap: [`skills/core-engineering-standards/SKILL.md`](skills/core-engineering-standards/SKILL.md)

### 🤖 ai-assisted-coding

Berdasarkan Lauren Tan (@poteto), engineer GrokBot di SpaceXAI, lewat carousel TikTok @christina.atelier *"How A GrokBot Engineer Utilizes AI for Coding"*. Lauren mengirim 2.000 PR dalam sebulan (post X-nya menyebut 2.500) — angka itu bukan target, melainkan hasil dari tools yang bisa dipakai ulang, skill, basis kode yang dipahami AI, dan sistem verifikasi.

6 aturannya:

1. 🧹 **Bersihkan basis kode dulu** — AI meniru pola yang dilihatnya. Hapus workaround sementara, implementasi duplikat dan deprecated, dead code, dan hack yang hidup hanya karena komentar.
2. 🚫 **Tanpa komentar penopang** — batasan ditulis di API/arsitektur, aturan di lint, aturan bisnis di test.
3. 🔁 **Setiap kesalahan AI yang berulang jadi aturan** — lint rule, compiler check, atau validasi CI, dicatat di file aturan proyek.
4. 🎯 **Satu pola standar per pekerjaan** — satu cara membangun endpoint API, mengakses DB, menangani error, logging, dan menyusun fitur/modul.
5. ✅ **Satu CLI verifikasi yang bisa dipakai ulang** — jalankan, kontrol, debug, trace, dan buktikan perubahan lewat CLI yang sama setiap sesi, bukan script sekali pakai tiap kali.
6. 🗺️ **Simpan Feature Map** — nama, fungsi, cara mencapai, pintasan, selektor DOM, interaksi, hasil yang diharapkan — supaya AI berhenti menebak dari screenshot yang samar.

Instruksi lengkap: [`skills/ai-assisted-coding/SKILL.md`](skills/ai-assisted-coding/SKILL.md)

## 🗂️ Isi repo

```
gygabyte-skills/
├── .agents/plugins/marketplace.json
├── .claude-plugin/
│   ├── marketplace.json
│   └── plugin.json
├── .codex-plugin/plugin.json
├── AGENTS.md
├── README.md
├── README.id.md
├── LICENSE
├── package.json
├── skills/
│   ├── ai-assisted-coding/
│   │   ├── SKILL.md
│   │   ├── references/feature-map-template.md
│   │   └── assets/AGENTS.md
│   └── core-engineering-standards/
│       ├── SKILL.md
│       └── assets/AGENTS.md
└── templates/
    └── AGENTS.md
```

## 🌐 Listing di skills.sh

Skill muncul di [skills.sh](https://skills.sh) otomatis lewat telemetri anonim saat seseorang menjalankan `npx skills add razor322/gygabyte-skills`. Tidak ada pengajuan manual. Audit keamanan dibuat setelah instalasi pertama. Sampai instalasi pertama terjadi, halaman skills.sh masih kosong; badge di atas hanya tautan yang sudah siap.

## ➕ Menambah skill baru

1. Buat `skills/<nama>/SKILL.md` dengan frontmatter valid (`name` harus sama dengan nama folder).
2. Tambah baris ke tabel katalog di atas.
3. Naikkan `version` di `package.json`, `.claude-plugin/plugin.json`, dan `.codex-plugin/plugin.json`, plus `metadata.version` di setiap `SKILL.md`.
4. Publish: `npm publish --access public` (npm) dan push ke GitHub (untuk `npx skills add` dan plugin marketplace).

## 📜 Lisensi

MIT — lihat [LICENSE](LICENSE).
