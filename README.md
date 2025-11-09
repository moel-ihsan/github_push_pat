# Dokumentasi: Push ke GitHub via Personal Access Token (PAT)

Panduan ini membantu kamu mengatur autentikasi Git di macOS / VSCode agar bisa melakukan `git push` ke GitHub menggunakan **Fine-grained Personal Access Token (PAT)**.

---

## 🔧 Persiapan Awal

1. **Masuk ke GitHub** → buka [Settings → Developer settings → Personal Access Tokens → Fine-grained tokens](https://github.com/settings/tokens?type=beta)
2. Klik **Generate new token**.
3. Pilih repository atau organisasi yang ingin diakses.
4. Pilih izin minimum berikut:
   - **Repository permissions** → Contents → `Read and write`
   - (Opsional) Metadata → `Read-only`
5. Klik **Generate token** dan **salin token** (hanya tampil sekali).

> 💡 Simpan token di tempat aman (misal, 1Password / Notes terenkripsi).

---

## ⚙️ Konfigurasi di macOS / VSCode

### 1. Pastikan Remote Menggunakan HTTPS

Buka terminal di root folder repo kamu:

```bash
git remote -v
```
Jika tampil seperti ini:
```
origin	git@github.com:moelihsan/myrepo.git (fetch)
origin	git@github.com:moelihsan/myrepo.git (push)
```
Ubah ke HTTPS:
```bash
git remote set-url origin https://github.com/<USERNAME>/<REPO>.git
```
Contoh:
```bash
git remote set-url origin https://github.com/moelihsan/myrepo.git
```

---

### 2. Hapus Credential Lama (Jika Ada)

Kadang Keychain masih menyimpan password/token lama.

```bash
printf "protocol=https\nhost=github.com\n\n" | git credential-osxkeychain erase
```
Atau manual:
> Buka **Keychain Access → cari `github.com` → hapus semua Internet Password** terkait.

---

### 3. Set Credential Helper (Agar VSCode Bisa Ingat Token)

```bash
git config --global credential.helper osxkeychain
```

---

### 4. Push Pertama Kali

```bash
git push -u origin main
```
Akan muncul prompt username dan password:

- **Username:** GitHub username kamu (misal: `moelihsan`)
- **Password:** paste Fine-grained PAT kamu

> ✅ macOS akan menyimpan token di Keychain, jadi push berikutnya otomatis tanpa login ulang.

---

### 5. Tes di VSCode

Buka repo di VSCode → buka panel **Source Control (Ctrl+Shift+G)** → commit dan tekan **Sync Changes**.
Jika sudah login via token, perubahan akan otomatis ter-push.

---

## 🔁 Jika Pindah ke Repo Baru

Setiap kali membuat repo baru:

1. `git init`
2. `git branch -M main`
3. `git remote add origin https://github.com/<USERNAME>/<REPO>.git`
4. `git add . && git commit -m "first commit"`
5. `git push -u origin main`
6. Masukkan username & token (hanya pertama kali)

---

## 💡 Tips

- Gunakan 1 token untuk semua repo pribadi jika izin-nya mencakup semua repo kamu.
- Jika token kedaluwarsa, buat ulang token baru dan hapus credential lama di Keychain.
- Gunakan `gh auth login` (GitHub CLI) jika ingin login lewat terminal interaktif tanpa token manual.

---

🧭 **Selesai!**
Sekarang kamu bisa `git push` dari VSCode maupun terminal tanpa error *Invalid username or token* lagi.
