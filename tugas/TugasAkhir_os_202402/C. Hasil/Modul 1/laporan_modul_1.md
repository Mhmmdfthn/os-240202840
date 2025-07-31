Berikut ini contoh **Laporan Tugas Akhir** yang udah kamu minta, dengan format sesuai template. Kamu tinggal ganti `<Nama Lengkap>` dan `<Nomor Induk Mahasiswa>` aja.

---

# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi
**Semester**: Genap / Tahun Ajaran 2024–2025

**Nama**   : `Muhammad Nuur Fathan`
**NIM**    : `240202840`

**Modul yang Dikerjakan**:
`Modul 1 – System Call dan Instrumentasi Kernel`

---

## 📌 Deskripsi Singkat Tugas

* **Modul 1 – System Call dan Instrumentasi Kernel**:
  Tugas ini bertujuan untuk menambahkan dua buah system call baru dalam kernel xv6-public, yaitu:

  * `getpinfo()`: Menampilkan informasi proses-proses aktif (PID, ukuran memori, nama proses)
  * `getReadCount()`: Mengembalikan jumlah pemanggilan system call `read()` sejak sistem boot

---

## 🛠️ Rincian Implementasi

* Menambahkan `struct pinfo` di `proc.h` sebagai wadah info proses
* Membuat variabel global `readcount` di `sysproc.c` untuk menghitung pemanggilan `read()`
* Menambahkan nomor syscall baru di `syscall.h`
* Menambahkan deklarasi syscall di `user.h` dan `usys.S`
* Menambahkan entri syscall baru ke tabel di `syscall.c`
* Implementasi syscall `sys_getpinfo()` dan `sys_getreadcount()` di `sysproc.c`
* Menambahkan `readcount++` di fungsi `sys_read()` dalam `sysfile.c`
* Membuat program uji `ptest.c` untuk `getpinfo()` dan `rtest.c` untuk `getreadcount()`
* Menambahkan `_ptest` dan `_rtest` ke dalam `Makefile`

---

## ✅ Uji Fungsionalitas

* `ptest`: untuk menguji hasil dari system call `getpinfo()`
* `rtest`: untuk menguji hasil dari system call `getReadCount()`

---

## 📷 Hasil Uji

### 📍 Output `rtest`:

```
Read Count Sebelum: 12
hello
Read Count Setelah: 13
```

### 📍 Output `ptest`:

```
PID     MEM     NAME
1       1228    init
2       1638    sh
3       1228    ptest
```

---
### Hasil uji dalam bentuk screenshot terminal:

<img width="960" height="1032" alt="image" src="https://github.com/user-attachments/assets/61353f4c-b05b-4c1f-8a40-9396c21605e2" />


## ⚠️ Kendala yang Dihadapi

* Variabel global `readcount` tidak dikenali di `sysfile.c` → Solusi: Menambahkan `extern int readcount;`
* Error redefinition pada `proc.h` karena file `proc.h` di-include dua kali → Solusi: Menambahkan header guard `#ifndef PROC_H` … `#endif`
* Kesalahan urutan pemanggilan `argint()` dan `argptr()` dalam `sys_read()`
* Salah memahami struktur `ptable` dalam `sys_getpinfo()` → harusnya mengakses `proc[NPROC]`, bukan `ptable.proc`

---

## 📚 Referensi

* Buku xv6 MIT: [https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
* Repositori xv6-public: [https://github.com/mit-pdos/xv6-public](https://github.com/mit-pdos/xv6-public)
* Diskusi praktikum dan dokumentasi kernel xv6
* Stack Overflow dan GitHub Issues saat debugging
* Chat GPT untuk Problem Solving
---

