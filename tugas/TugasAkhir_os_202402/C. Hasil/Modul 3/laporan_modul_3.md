# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi
**Semester**: Genap / Tahun Ajaran 2024–2025
**Nama**: `Muhammad Nuur Fathan`
**NIM**: `240202840`
**Modul yang Dikerjakan**:
`Modul 3 Manajemen Memori Tingkat Lanjut (xv6-public x86)`

---

## 📌 Deskripsi Singkat Tugas

Tuliskan deskripsi singkat dari modul yang Anda kerjakan. Misalnya:

* **Modul 3 Manajemen Memori Tingkat Lanjut (xv6-public x86)**:
Pada modul ini, saya mengimplementasikan dua fitur manajemen memori tingkat lanjut pada kernel XV6:
* **Copy-on-Write (COW)**: Fork efisien dengan cara membagi halaman memori sampai ada proses yang mencoba menulis, barulah halaman disalin.
* **Shared Memory ala System V**: Proses dapat berbagi memori lewat sistem pemanggilan `shmget()` dan `shmrelease()`.

---

## 🛠️ Rincian Implementasi

### ✨ Copy-on-Write

* Modifikasi `fork()` di `proc.c` agar setiap halaman yang bisa ditulis dikloning sebagai **read-only** dan diberi flag `PTE_COW`.
* Tambahkan penanganan `trap` untuk `T_PGFLT` (page fault):

  * Periksa apakah fault berasal dari COW.
  * Alokasikan halaman baru, salin isi lama, perbarui PTE.
* Tambahkan fungsi `cowcopy()` untuk menyalin isi halaman pada page fault.

### ✨ Shared Memory System V

* Tambahkan struktur `shmentry` untuk menyimpan informasi halaman shared memory.
* Tambah array `shmtab[NSHM]` sebagai tabel global shared memory.
* Implementasi dua syscall baru:

  * `shmget(int key)`: Mengembalikan alamat virtual untuk halaman bersama.
  * `shmrelease(int key)`: Mengurangi reference count dan menghapus jika nol.
* Tambahkan flag `PTE_SHARE` untuk membedakan halaman bersama dari halaman biasa.
---

## ✅ Uji Fungsionalitas

Program uji yang dibuat:

* **`cowtest`**:

  * Fork proses, tulis di parent, baca di child, lalu tulis di child.
  * Hasil menunjukkan COW berhasil karena parent dan child tidak saling menimpa isi.

* **`shmtest`**:

  * Parent dan child mengakses halaman yang sama melalui `shmget()`.
  * Child menulis data, parent membaca hasil yang sama.


---

## 📷 Hasil Uji

Lampirkan hasil uji berupa screenshot atau output terminal. Contoh:

### 📍 Contoh Output `cowtest`:

```
Child sees: Y
Parent sees: X
```

### 📍 Contoh Output `shmtest`:

```
Child reads: A
Parent reads: B
```

Jika ada screenshot:

### Output `cowtest`

<img width="960" height="1032" alt="image" src="https://github.com/user-attachments/assets/a715563a-003c-4f8d-930d-a846e77ad0ce" />

### Output `shmtest`

<img width="960" height="1032" alt="image" src="https://github.com/user-attachments/assets/db1aa732-db7d-4d97-a559-11316ee17226" />


---

## ⚠️ Kendala yang Dihadapi

Tuliskan kendala (jika ada), :

1. **Variabel tidak terdeklarasi saat menangani page fault** – variabel `r` tidak dikenali karena tidak dideklarasikan di awal fungsi `trap()`; solusinya dengan menambahkan `struct trapframe *r = myproc()->tf;`.
2. **Error redeklarasi fungsi atau variabel global** – terjadi ketika `shmtab[]` atau fungsi `walkpgdir()` dan `mappages()` dideklarasikan ulang di `vm.c` secara `static`, padahal sudah ada deklarasi di `defs.h`.
3. **Fork gagal karena implementasi `cowuvm()` tidak memetakan semua halaman dengan benar** – menyebabkan child process crash.
4. **Refcount shared memory tidak sinkron** – jika `incref()` dan `decref()` tidak dipanggil secara konsisten, menyebabkan memory leak atau halaman dibebaskan terlalu cepat.
5. **Page fault gagal tertangani** – jika bit `PTE_COW` tidak dicek dengan benar di `trap.c`, maka penulisan ke halaman bersama menyebabkan panic.
6. **Data rusak saat copy-on-write** – penggunaan `memmove()` yang salah atau tidak memetakan halaman baru menyebabkan data menjadi tidak valid.
7. **Mapping shared memory bentrok dengan stack atau heap** – karena semua shared memory dialokasikan di `USERTOP - (i+1)*PGSIZE`, bentrokan bisa terjadi jika tidak ada kontrol ukuran.
8. **Fungsi `shmget()` tidak mengembalikan alamat yang benar** – akibat kesalahan indexing `shmtab` atau penghitungan alamat virtual user.
9. **Race condition pada `shmtab`** – tidak ada proteksi (misal lock) membuat akses bersama antar proses menyebabkan data shared memory korup.
10. **Shared memory tidak dibebaskan** – jika `shmrelease()` tidak menurunkan refcount atau tidak membebaskan frame saat refcount = 0.
11. **Fungsi `copyuvm()` masih terpakai** – jika `fork()` tidak diganti ke `cowuvm()`, maka optimisasi CoW tidak bekerja.
12. **Kesalahan `panic: trap` saat menjalankan cowtest atau shmtest** – seringkali karena akses halaman COW tidak ditangani dengan benar.
13. **Error kompilasi saat menambahkan header terpisah (`shm.h`)** – karena duplikasi definisi struct atau tidak disertakan `#ifndef` guard.
14. **Inisialisasi `shmtab[]` gagal dilakukan awal boot** – menyebabkan isi tabel tidak konsisten saat `shmget()` pertama dipanggil.

---

---

## 📚 Referensi
* Buku xv6 MIT: [https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
* Repositori xv6-public: [https://github.com/mit-pdos/xv6-public](https://github.com/mit-pdos/xv6-public)
* Stack Overflow, GitHub Issues, diskusi praktikum

---

