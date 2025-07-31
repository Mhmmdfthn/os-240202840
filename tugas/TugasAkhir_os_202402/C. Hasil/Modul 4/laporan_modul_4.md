
---

# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi
**Semester**: Genap / Tahun Ajaran 2024–2025
**Nama**: `Muhammad nuur Fathan`
**NIM**: `240202840`
**Modul yang Dikerjakan**:
`Modul 4 – Subsistem Kernel Alternatif (xv6-public)`

---

## 📌 Deskripsi Singkat Tugas

* **Modul 4 – Subsistem Kernel Alternatif**:
  Modul ini berfokus pada pengembangan dua fitur kernel baru pada xv6, yaitu:

  * Menambahkan system call baru `chmod(path, mode)` untuk mengubah mode akses file menjadi read-only atau read-write.
  * Menambahkan driver pseudo-device `/dev/random` yang menghasilkan data acak saat dibaca.

---

## 🛠️ Rincian Implementasi

### Untuk System Call `chmod()`:

* Menambahkan field `short mode;` di `struct inode` (hanya in-memory).
* Mengedit `syscall.h`, `user.h`, `usys.S`, dan `syscall.c` untuk mendaftarkan syscall `chmod`.
* Implementasi fungsi `sys_chmod` di `sysfile.c` untuk mengatur `ip->mode`.
* Menambahkan pengecekan `f->ip->mode == 1` pada `filewrite()` di `file.c` untuk mencegah penulisan ke file read-only.
* Membuat program uji `chmodtest.c`.

### Untuk Driver `/dev/random`:

* Menambahkan file baru `random.c` berisi fungsi `randomread()` yang menghasilkan byte acak.
* Mendaftarkan `randomread` ke `devsw[3]` secara langsung di `file.c`.
* Menambahkan perintah `mknod("/dev/random", 1, 3);` di `init.c` agar device node dibuat saat booting.
* Membuat program uji `randomtest.c`.
* Menambahkan `random.o` ke `Makefile` agar ikut dikompilasi ke kernel.

---

## ✅ Uji Fungsionalitas

Program uji yang digunakan:

* `chmodtest`: menguji apakah file dengan mode read-only benar-benar tidak bisa ditulis.
* `randomtest`: menguji apakah pembacaan dari `/dev/random` menghasilkan byte acak.

---

## 📷 Hasil Uji

### 📍  Output `chmodtest`:

```
Write blocked as expected
```

### 📍  Output `randomtest`:

```
254 183 124 93 42 115 136 121
```

### 📍 Screenshot xv6 shell:

<img width="948" height="772" alt="image" src="https://github.com/user-attachments/assets/3d4badb9-be23-4294-bc5e-a5babd13e181" />


---

## ⚠️ Kendala yang Dihadapi

* Awalnya menambahkan `mode` ke `struct dinode` menyebabkan `mkfs` gagal (`assert (BSIZE % sizeof(dinode)) == 0`).
* Salah menulis inisialisasi `devsw[3]` menyebabkan `randomtest` gagal `cannot open /dev/random`.
* Fungsi `randomread` tidak dipanggil karena `devsw[3]` hanya diisi di `fileinit()`, padahal perlu diinisialisasi global.

---

## 📚 Referensi

* Buku xv6 MIT: [https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
* Repositori xv6-public: [https://github.com/mit-pdos/xv6-public](https://github.com/mit-pdos/xv6-public)
* Diskusi praktikum dan debugging bersama teman 
* ChatGPT & Stack Overflow untuk tracing error kernel dan struktur file system

---
