
---

# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi
**Semester**: Genap / Tahun Ajaran 2024–2025
**Nama**: `Muhammad Nuur Fathan`
**NIM**: `240202840`
**Modul yang Dikerjakan**:
`Modul 5 – Audit dan Keamanan Sistem (xv6-public)`

---

## 📌 Deskripsi Singkat Tugas

* **Modul 5 – Audit dan Keamanan Sistem**:
  Menambahkan fitur audit log pada xv6 untuk mencatat semua system call yang dipanggil oleh proses, dan menyediakan syscall `get_audit_log()` yang hanya dapat diakses oleh proses dengan PID 1 (init) untuk membaca isi log.

---

## 🛠️ Rincian Implementasi

* Menambahkan struktur `audit_entry` dan array `audit_log[]` di `syscall.c`
* Menambahkan logika pencatatan system call di dalam fungsi `syscall()`
* Menambahkan syscall baru `get_audit_log()` di `sysproc.c`
* Menambahkan definisi syscall di `user.h`, `usys.S`, dan `syscall.h`
* Mendaftarkan syscall di tabel `syscalls[]` di `syscall.c`
* Membuat program uji `audit.c` untuk menampilkan log
* Menambahkan `_audit` ke dalam `Makefile` agar program uji dikompilasi

---

## ✅ Uji Fungsionalitas

* `audit`: untuk melihat isi audit log yang berisi catatan PID, nomor system call, dan waktu (tick) saat dipanggil

  > Catatan: hanya bisa dijalankan oleh proses `init` (PID 1), sehingga perlu mengganti `init.c` agar `audit` dijalankan sebelum `sh`

---

## 📷 Hasil Uji

### 📍 Contoh Output `audit` (saat dijalankan oleh PID 1):

```
=== Audit Log ===
[0] PID=1 SYSCALL=5 TICK=12
[1] PID=1 SYSCALL=6 TICK=13
[2] PID=1 SYSCALL=20 TICK=14
[3] PID=1 SYSCALL=22 TICK=15
```

Jika hasil uji dalam bentuk screenshot terminal:

```
<img width="960" height="1032" alt="image" src="https://github.com/user-attachments/assets/1c161b31-e988-429a-9c45-d23aac7fe342" />

```

---

## ⚠️ Kendala yang Dihadapi

* **Akses Struktur Antar File Kernel**
  Struktur `audit_entry` dan variabel `audit_log` awalnya didefinisikan di `syscall.c`, menyebabkan error saat diakses dari file lain. Diselesaikan dengan memindahkan definisinya ke `defs.h` dan menambahkan `extern`.

* **Duplikasi Struktur di User-Space**
  Terjadi redefinition error karena struktur `audit_entry` juga didefinisikan di `audit.c`. Solusi: hanya menggunakan definisi dari `user.h`.

* **Kesalahan Akses Proses Aktif**
  Error muncul karena akses langsung ke `proc->pid` dan `proc->tf->eax`. Diperbaiki dengan menggunakan `curproc` dari `myproc()`.

* **Pengujian Akses Terbatas PID 1**
  Untuk menguji agar hanya PID 1 yang bisa membaca log, `init.c` dimodifikasi agar menjalankan `audit` sebagai proses awal.

---

## 📚 Referensi

* Buku xv6 MIT: [https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
* Repositori xv6-public: [https://github.com/mit-pdos/xv6-public](https://github.com/mit-pdos/xv6-public)
* Diskusi dengan teman
* Chat GPT untuk Problem Solving

---

