# <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/layers.svg" width="24" height="24" alt=""> Gygabyte Skills

Agent Skills untuk AI coding agent, oleh [@gygabyte.lab](https://www.instagram.com/gygabyte.lab) ([razor322](https://github.com/razor322) di GitHub).

[![npm version](https://img.shields.io/npm/v/gygabyte-skills?color=cb3837&logo=npm)](https://www.npmjs.com/package/gygabyte-skills)
[![license](https://img.shields.io/npm/l/gygabyte-skills)](https://github.com/razor322/gygabyte-skills/blob/main/LICENSE)
[![skills.sh](https://skills.sh/b/razor322/gygabyte-skills)](https://skills.sh/razor322/gygabyte-skills)

[English](README.md) · **[Bahasa Indonesia](README.id.md)**

| Skill | Kegunaan |
| --- | --- |
| `ai-assisted-coding` | Bangun dan debug aplikasi dengan AI coding agent memakai disiplin GrokBot dari Lauren Tan — workaround bersih, tanpa komentar penopang, kesalahan berulang jadi aturan lint/CI, satu pola standar, satu CLI verifikasi yang bisa dipakai ulang, Feature Map, trust pyramid, dan token yang diutamakan untuk verifikasi |
| `core-engineering-standards` | Tulis, review, dan refactor kode siap produksi — SRP, fungsi pendek dengan early return, typing eksplisit, tanpa hardcoded value, arsitektur 3 lapis, error handling eksplisit, schema validation, dan diff minimal |
| `enforceable-architecture` | Ubah pelajaran yang didapat dengan susah payah menjadi constraint arsitektur yang bisa ditegakkan — larang API yang berat nuance di product code dengan wrapper aman, buat struktur yang benar menjadi yang paling jelas lewat layout direktori, tegakkan batas layer secara mekanis |
| `runtime-verification` | Beri agent cara menjalankan aplikasi sendiri dan membuktikan perubahan berhasil — satu CLI/driver yang bisa dipakai ulang, Feature Map sebagai kosakata bersama, dan bukti video, screenshot, dan state-chart, bukan sekadar review diff |
| `formal-verification` | Tambah lapisan formal verification di atas test — model-check alur stateful terpenting dan buktikan algoritma kunci — sebagai satu lapisan lagi di samping runtime verification, bukan silver bullet |
| `software-factory` | Bangun outer loop otomatisasi hanya setelah inner coding loop solid — fuzzing saat release, auto-fix PR, dan automerge — karena mengotomatisasi loop yang menghasilkan slop hanya menghasilkan lebih banyak slop |

## <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/trending-up.svg" width="18" height="18" alt=""> Apa bedanya kalau skill ini dipakai?

Bayangkan dua orang diberi tugas yang sama persis. Satu pakai skill ini, satu tidak. Kode keduanya **sama-sama jalan** — tapi yang pakai skill menghasilkan kode yang lebih rapi, lebih aman, dan lebih mudah dilanjutkan orang lain (atau AI) berikutnya.

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/search.svg" width="18" height="18" alt=""> Contoh perubahan yang langsung terlihat

- **Kode tidak lagi bertingkat-rumit** — dari piramida `if` bersarang jadi alur yang datar dan mudah dibaca.
- **Semua fungsi bertipe jelas** — parameter dan nilai kembaliannya dinyatakan eksplisit.
- **Tidak ada angka atau teks "ajaib"** — semuanya diberi nama di satu tempat, gampang diubah.
- **Tidak ada komentar "jangan diubah" / `TODO`** — aturan ditulis lewat lint dan test, bukan tempelan.
- **Ada cara buktikan kode jalan** — satu perintah verifikasi, plus peta fitur supaya AI tidak menebak-nebak.

> Ini adalah contoh ilustratif dari tugas kecil yang kami coba (registrasi user, endpoint API, dan satu file proyek nyata). Bukan jaminan angka pasti di proyek Anda — hasilnya tetap bergantung pada basis kode dan cara AI dipakai. Yang kami tunjukkan di sini adalah **bentuk dampaknya**, bukan angka yang dijanjikan.

## <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/package.svg" width="18" height="18" alt=""> Instalasi

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/wrench.svg" width="18" height="18" alt=""> Dengan skills CLI (disarankan — jalan untuk 40+ agent)

```bash
# semua skill di repo ini
npx skills add razor322/gygabyte-skills

# satu skill saja
npx skills add razor322/gygabyte-skills --skill ai-assisted-coding
npx skills add razor322/gygabyte-skills --skill core-engineering-standards
npx skills add razor322/gygabyte-skills --skill runtime-verification

# global, untuk semua proyek
npx skills add razor322/gygabyte-skills -g
```

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/puzzle.svg" width="18" height="18" alt=""> Claude Code (plugin)

```sh
claude plugin marketplace add razor322/gygabyte-skills
claude plugin install gygabyte@gygabyte-skills
```

Di dalam sesi Claude Code, perintahnya `/plugin marketplace add razor322/gygabyte-skills` dan `/plugin install gygabyte@gygabyte-skills`. Mulai sesi baru, lalu pakai `/skills` untuk memeriksa instalasi atau `/gygabyte:ai-assisted-coding` untuk memanggil skill-nya.

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/puzzle.svg" width="18" height="18" alt=""> Codex (plugin)

```sh
codex plugin marketplace add razor322/gygabyte-skills
codex plugin install gygabyte@gygabyte-skills
```

Di dalam sesi Codex, perintahnya `/plugin marketplace add razor322/gygabyte-skills` dan `/plugin install gygabyte@gygabyte-skills`.

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/package.svg" width="18" height="18" alt=""> Dari npm

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

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/hammer.svg" width="18" height="18" alt=""> Instalasi manual

Salin seluruh direktori leaf skill, termasuk `references` dan `assets`-nya, ke direktori skill agent Anda. Lihat bagian npm di atas untuk path tiap agent.

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/scroll-text.svg" width="18" height="18" alt=""> Agent Anda tidak mendukung skills? Pakai file aturan proyek

Salin `templates/AGENTS.md` ke root proyek sebagai `AGENTS.md` (atau `CLAUDE.md`) lalu isi standar khusus proyek. Aturan dari keenam skill, tanpa perlu skill loader.

## <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/puzzle.svg" width="18" height="18" alt=""> Skills

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/landmark.svg" width="18" height="18" alt=""> core-engineering-standards

Menulis, review, dan refactor kode siap produksi di bahasa atau framework apa pun. Mencakup kode idiomatik yang bersih, SRP, fungsi di bawah 30-40 baris, early return dengan maksimal dua level nesting, typing eksplisit, tanpa hardcoded value, pemisahan lapisan presentation/application/infrastructure tanpa kontaminasi silang, error handling eksplisit dengan log dev terstruktur dan pesan user yang sudah disanitasi, schema validation di batas eksternal, dan diff minimal yang bedah. Bekerja bersama `ai-assisted-coding`: skill itu mengatur cara agent bekerja dengan AI, skill ini mengatur cara kodenya ditulis.

Instruksi lengkap: [`skills/core-engineering-standards/SKILL.md`](skills/core-engineering-standards/SKILL.md), plus detail enforceable architecture di [`skills/core-engineering-standards/references/enforceable-architecture.md`](skills/core-engineering-standards/references/enforceable-architecture.md) dan checklist pre-submit di [`skills/core-engineering-standards/references/checklist.md`](skills/core-engineering-standards/references/checklist.md).

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/bot.svg" width="18" height="18" alt=""> ai-assisted-coding

Berdasarkan Lauren Tan (@poteto), engineer GrokBot di SpaceXAI, lewat carousel TikTok @christina.atelier *"How A GrokBot Engineer Utilizes AI for Coding"* dan talk-nya *"All Code Is Slop (Except Mine)"* (14 Nov 2026). Lauren mengirim 2.000 PR dalam sebulan (post X-nya menyebut 2.500; di talk-nya ia menyebut 3.000 secara pribadi dan 11.000 sebagai tim, dengan revert rate ~0,5%) — angka itu bukan target, melainkan hasil dari tools yang bisa dipakai ulang, skill, basis kode yang dipahami AI, dan sistem verifikasi.

Aturannya:

1. **Bersihkan basis kode dulu** — AI meniru pola yang dilihatnya. Hapus workaround sementara, implementasi duplikat dan deprecated, dead code, dan hack yang hidup hanya karena komentar.
2. **Tanpa komentar penopang** — batasan ditulis di API/arsitektur, aturan di lint, aturan bisnis di test.
3. **Setiap kesalahan AI yang berulang jadi aturan** — lint rule, compiler check, atau validasi CI, dicatat di file aturan proyek.
4. **Satu pola standar per pekerjaan** — satu cara membangun endpoint API, mengakses DB, menangani error, logging, dan menyusun fitur/modul.
5. **Satu CLI verifikasi yang bisa dipakai ulang** — jalankan, kontrol, debug, trace, dan buktikan perubahan lewat CLI yang sama setiap sesi, bukan script sekali pakai tiap kali.
6. **Simpan Feature Map** — nama, fungsi, cara mencapai, pintasan, selektor DOM, interaksi, hasil yang diharapkan — supaya AI berhenti menebak dari screenshot yang samar.
7. **Trust pyramid** — usaha terbesar di tempat yang paling deterministik: basis kode, lalu static analysis, lalu verifikasi, lalu prompt di paling atas. Saat mengoreksi agent, ubah koreksi itu jadi deterministic check dulu, bukan menambah baris di file aturan.
8. **Token terbanyak untuk verifikasi** — verifikasi adalah pengeluaran token utama, bukan afterthought; nilai sistem dari revert rate dan performa yang terlihat user, bukan dari jumlah PR. Review artefak (video, screenshot, state chart), bukan hanya diff.

Instruksi lengkap: [`skills/ai-assisted-coding/SKILL.md`](skills/ai-assisted-coding/SKILL.md)

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/shield-check.svg" width="18" height="18" alt=""> enforceable-architecture

Ubah pelajaran yang didapat dengan susah payah menjadi constraint arsitektur yang bisa ditegakkan, bukan sekadar nasihat. Larang API yang berat nuance di product code dan sediakan wrapper aman (API mentahnya hanya boleh di dalam framework layer — contohnya React `useEffect` di Dune untuk Grok Bot), buat struktur yang benar menjadi yang paling jelas lewat layout direktori (contohnya `electron/main` vs `electron/renderer`), dan tegakkan batas layer secara mekanis dengan lint dan dependency analysis. Dipakai saat sebuah pola terus disalahgunakan, agent terus melintasi batas layer, atau sebuah aturan terus diabaikan.

Instruksi lengkap: [`skills/enforceable-architecture/SKILL.md`](skills/enforceable-architecture/SKILL.md)

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/monitor-play.svg" width="18" height="18" alt=""> runtime-verification

Beri AI agent cara menjalankan aplikasi sendiri dan membuktikan perubahan berhasil. Satu CLI/driver yang bisa dipakai ulang alih-alih script baru tiap sesi, Feature Map sebagai kosakata bersama dari sudut pandang user, dan bukti video, screenshot, dan state-chart alih-alih sekadar review diff. Token terbanyak dipakai untuk verifikasi. Dipakai saat memverifikasi fitur, memperbaiki bug UI, atau me-review kerja agent.

Instruksi lengkap: [`skills/runtime-verification/SKILL.md`](skills/runtime-verification/SKILL.md)

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/flask-conical.svg" width="18" height="18" alt=""> formal-verification

Tambah lapisan formal verification di atas test: model-check alur stateful terpenting dengan TLA+ (contohnya notifikasi lintas device/platform), buktikan algoritma kunci dengan Lean (contohnya library virtualisasi sendiri, supaya tidak flicker/jitter), dan tangkap edge case UI yang stateful dengan property-based testing. Satu lapisan lagi di samping runtime verification — bukan silver bullet, dan paling layak dulu di tempat bug paling mahal.

Instruksi lengkap: [`skills/formal-verification/SKILL.md`](skills/formal-verification/SKILL.md)

### <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/factory.svg" width="18" height="18" alt=""> software-factory

Bangun outer loop otomatisasi hanya setelah inner coding loop solid. Urutannya: koreksi lewat basis kode dulu, lint rule dulu, lalu bersihkan pola lama, buat verifikasi kuat — baru otomatisasi. Outer loop (contohnya fuzzing saat release yang memakai aplikasi seperti user asli, membuka fix PR per bug, lalu automerge) hanya aman kalau PR cenderung bagus secara default dan revert rate rendah dan stabil. Mengotomatisasi loop yang menghasilkan slop hanya menghasilkan lebih banyak slop dan membakar token.

Instruksi lengkap: [`skills/software-factory/SKILL.md`](skills/software-factory/SKILL.md)

## <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/folder-tree.svg" width="18" height="18" alt=""> Isi repo

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
│   │   ├── references/trust-pyramid.md
│   │   └── assets/AGENTS.md
│   ├── core-engineering-standards/
│   │   ├── SKILL.md
│   │   ├── references/checklist.md
│   │   ├── references/enforceable-architecture.md
│   │   └── assets/AGENTS.md
│   ├── enforceable-architecture/
│   │   ├── SKILL.md
│   │   ├── references/examples.md
│   │   └── assets/AGENTS.md
│   ├── runtime-verification/
│   │   ├── SKILL.md
│   │   ├── references/evidence-checklist.md
│   │   └── assets/AGENTS.md
│   ├── formal-verification/
│   │   ├── SKILL.md
│   │   ├── references/when-to-use.md
│   │   └── assets/AGENTS.md
│   └── software-factory/
│       ├── SKILL.md
│       ├── references/factory-checklist.md
│       └── assets/AGENTS.md
└── templates/
    └── AGENTS.md
```

## <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/globe.svg" width="18" height="18" alt=""> Listing di skills.sh

Skill muncul di [skills.sh](https://skills.sh) otomatis lewat telemetri anonim saat seseorang menjalankan `npx skills add razor322/gygabyte-skills`. Tidak ada pengajuan manual. Audit keamanan dibuat setelah instalasi pertama. Sampai instalasi pertama terjadi, halaman skills.sh masih kosong; badge di atas hanya tautan yang sudah siap.

## <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/plus.svg" width="18" height="18" alt=""> Menambah skill baru

1. Buat `skills/<nama>/SKILL.md` dengan frontmatter valid (`name` harus sama dengan nama folder).
2. Tambah baris ke tabel katalog di atas.
3. Naikkan `version` di `package.json`, `.claude-plugin/plugin.json`, dan `.codex-plugin/plugin.json`, plus `metadata.version` di setiap `SKILL.md`.
4. Publish: `npm publish --access public` (npm) dan push ke GitHub (untuk `npx skills add` dan plugin marketplace).

## <img src="https://cdn.jsdelivr.net/npm/lucide-static@latest/icons/scroll-text.svg" width="18" height="18" alt=""> Lisensi

MIT — lihat [LICENSE](LICENSE).
