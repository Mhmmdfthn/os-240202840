

---

# 📝 Laporan Tugas Akhir

**Mata Kuliah**: Sistem Operasi
**Semester**: Genap / Tahun Ajaran 2024–2025
**Nama**: `Muhammad Nuur Fathan`
**NIM**: `240202840`
**Modul yang Dikerjakan**:
**Modul 2 – Penjadwalan CPU Lanjutan (Priority Scheduling Non-Preemptive)**

---

## 📌 Deskripsi Singkat Tugas

Modul ini bertujuan untuk memodifikasi algoritma penjadwalan proses di kernel `xv6-public` dari **Round Robin** menjadi **Non-Preemptive Priority Scheduling**. Penjadwalan baru ini memilih proses RUNNABLE dengan prioritas **tertinggi** (nilai angka prioritas **terkecil**) dan menjalankannya sampai selesai atau berubah status. Sistem ini **tidak mendukung preemption**, sehingga setiap proses tetap dijalankan hingga selesai secara eksplisit.

---

## 🛠️ Rincian Implementasi

* Menambahkan field `priority` pada struktur `proc` di file `proc.h`
* Inisialisasi nilai default `priority` (60) di `allocproc()` pada `proc.c`
* Menambahkan syscall baru `set_priority(int)` untuk mengatur prioritas proses:

  * Mendefinisikan syscall di `syscall.h`, `user.h`, dan `usys.S`
  * Registrasi syscall di `syscall.c` dan implementasi di `sysproc.c`
* Memodifikasi fungsi `scheduler()` di `proc.c`:

  * Menyeleksi proses RUNNABLE dengan prioritas tertinggi
  * Menjalankan proses tersebut tanpa preemption hingga selesai atau blocking
* Menambahkan program uji `ptest.c` yang menguji eksekusi dua child dengan prioritas berbeda
* Memodifikasi `Makefile` untuk memasukkan `ptest` ke dalam daftar user program

---

## ✅ Uji Fungsionalitas

Pengujian dilakukan menggunakan program `ptest`:

* `ptest` membuat dua proses child:

  * `Child 1` dengan prioritas rendah (`90`)
  * `Child 2` dengan prioritas tinggi (`10`)
* Hasil pengujian menunjukkan bahwa `Child 2` selesai lebih dulu karena memiliki prioritas lebih tinggi

---

## 📷 Hasil Uji

### 📍 Contoh Output `ptest`:

```
Child 2 selesai
Child 1 selesai
Parent selesai
```

Hasil menunjukkan bahwa proses dengan prioritas lebih tinggi (nilai lebih kecil) dijalankan lebih dulu.



[<img width="951" height="1032" alt="Screenshot 2025-07-30 195337" src="https://github.com/user-attachments/assets/9d1dc8f1-f73e-4581-bd54-28cdd669b89d" />](https://github.com/Mhmmdfthn/os-240202840/blob/main/tugas/TugasAkhir_os_202402/C.%20Hasil/Modul%202/screenshot/Screenshot%202025-07-30%20195337.png?raw=true)




---

## ⚠️ Kendala yang Dihadapi

* Terjadi error `‘proc’ undeclared` dan `‘cpu’ undeclared` dalam fungsi `scheduler()` karena:

  * `proc` seharusnya dikelola melalui `cpu->proc`
  * `cpu` perlu diinisialisasi melalui `mycpu()`
* Solusi:

  * Menambahkan `struct cpu *cpu = mycpu();` di awal fungsi `scheduler`
  * Menggunakan `cpu->proc = p;` saat proses dipilih untuk dijalankan

---

## 📚 Referensi

* Buku xv6 MIT: [https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf](https://pdos.csail.mit.edu/6.828/2018/xv6/book-rev11.pdf)
* Repositori xv6-public: [https://github.com/mit-pdos/xv6-public](https://github.com/mit-pdos/xv6-public)
* Diskusi praktikum Sistem Operasi
* Stack Overflow & GitHub Issues terkait context switching dan priority scheduling di xv6
* Chat GPT untuk Problem Solving
---

