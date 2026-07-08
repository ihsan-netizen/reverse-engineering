# Daftar konten:
 📖 TOPIK 1: Pola Kode
- # Pengenalan Singkat tentang CPU
- # Fungsi Sederhana
- # Hello World!





## Bagian 1: Pengenalan Singkat tentang CPU

> Catatan belajar pribadi *refference: reverse-engineering-for-beginner*.

---

## Apa itu CPU?

Hari ini saya mulai mempelajari dasar mengenai **CPU (Central Processing Unit)** dan bagaimana sebuah program sebenarnya dijalankan oleh komputer. Selama ini saya hanya mengenal bahasa pemrograman tingkat tinggi seperti **C, C++, Java,** atau **Python**, akan tetapi ternyata pada akhirnya semua program harus diterjemahkan menjadi bahasa yang benar-benar dipahami oleh CPU, yaitu **machine code**.

CPU adalah komponen yang bertugas mengeksekusi setiap instruksi yang terdapat di dalam machine code. Dengan kata lain, CPU tidak memahami langsung kode yang kita tulis menggunakan bahasa pemrograman tingkat tinggi. CPU hanya memahami kumpulan instruksi sederhana yang sudah dikonversi ke bentuk yang sesuai dengan arsitekturnya.

---

## Istilah Penting

Selama mempelajari materi ini, ada beberapa istilah dasar yang harus dipahami.

### Instruction

**Instruction** adalah perintah paling dasar yang dapat dijalankan oleh CPU. Contohnya meliputi memindahkan data antar register, mengakses memori, atau melakukan operasi aritmetika sederhana. Setiap jenis CPU memiliki kumpulan instruction yang berbeda-beda, yang disebut **Instruction Set Architecture (ISA)**.

### Machine Code

**Machine code** merupakan kumpulan instruksi yang dapat diproses secara langsung oleh CPU. Setiap instruction biasanya direpresentasikan dalam beberapa byte sehingga dapat dikenali oleh perangkat keras.

### Assembly Language

**Assembly language** adalah representasi machine code yang lebih mudah dibaca manusia karena menggunakan mnemonic (nama instruksi) serta fitur tambahan seperti macro. Walaupun lebih mudah dipahami dibandingkan machine code, assembly tetap merupakan bahasa pemrograman tingkat rendah.

### CPU Register

**CPU Register** adalah tempat penyimpanan data sementara yang berada langsung di dalam CPU sehingga proses aksesnya jauh lebih cepat dibandingkan memori utama.

Register yang digunakan secara umum disebut **General Purpose Register (GPR)**.

Beberapa contoh jumlah register pada berbagai arsitektur:

| Arsitektur | Jumlah GPR |
|------------|-----------:|
| x86 | ±8 |
| x86-64 | ±16 |
| ARM | ±16 |

Cara paling mudah memahami register adalah dengan menganggapnya sebagai **variabel sementara tanpa tipe data tertentu**. Meskipun jumlahnya sedikit, ternyata CPU mampu melakukan sangat banyak operasi hanya dengan memanfaatkan register-register tersebut.

---

## Mengapa Bahasa Tingkat Tinggi Masih Perlu Diterjemahkan?

Materi ini juga menjelaskan alasan mengapa bahasa pemrograman tingkat tinggi dan machine code harus dipisahkan.

Manusia lebih mudah menulis program menggunakan bahasa seperti **C, C++, Java,** atau **Python** karena memiliki abstraksi yang tinggi dan sintaks yang mudah dipahami.

Sebaliknya, CPU bekerja jauh lebih efisien jika menerima instruksi dalam bentuk yang sangat sederhana.

Secara teori memang mungkin membuat CPU yang dapat langsung menjalankan bahasa tingkat tinggi, tetapi desain CPU seperti itu akan menjadi jauh lebih kompleks dibandingkan CPU modern.

Karena itulah dibutuhkan sebuah program bernama **compiler**, yang bertugas menerjemahkan kode dari bahasa tingkat tinggi menjadi assembly, kemudian akhirnya menjadi machine code yang siap dieksekusi oleh CPU.

---

# 1.1 A Couple of Words About Different ISAs

Bagian berikutnya membahas mengenai beberapa jenis **Instruction Set Architecture (ISA)** yang digunakan oleh berbagai prosesor.

## x86

Arsitektur **x86** sejak awal menggunakan **variable-length opcode**, yaitu ukuran instruction yang tidak tetap.

Karena karakteristik tersebut, ketika era 64-bit hadir dan lahirlah **x86-64 (x64)**, perubahan terhadap ISA tidak terlalu besar.

Menariknya, sampai saat ini masih terdapat banyak instruction yang sudah ada sejak prosesor **Intel 8086** berarsitektur 16-bit dan tetap dipertahankan pada prosesor modern.

---

## ARM

Berbeda dengan x86, **ARM** dirancang menggunakan konsep **RISC (Reduced Instruction Set Computer)** dengan instruction yang memiliki panjang tetap.

Pada awal pengembangannya, seluruh instruction ARM memiliki ukuran **4 byte**. Mode ini dikenal sebagai **ARM Mode**.

Pendekatan tersebut memang memberikan beberapa keuntungan, tetapi kemudian dianggap kurang efisien karena sebagian besar instruction yang sering digunakan sebenarnya tidak membutuhkan ruang sebesar 4 byte.

---

## Thumb Mode

Untuk mengatasi masalah tersebut, ARM memperkenalkan ISA baru bernama **Thumb**.

Pada **Thumb Mode**, setiap instruction hanya berukuran **2 byte** sehingga ukuran program menjadi lebih kecil dan penggunaan memori lebih hemat.

Namun, karena ruang yang tersedia lebih sedikit, tidak semua instruction ARM dapat direpresentasikan dalam ukuran tersebut. Akibatnya, kemampuan Thumb pada awalnya lebih terbatas dibandingkan ARM Mode.

Walaupun demikian, kode yang dikompilasi menggunakan ARM Mode maupun Thumb Mode tetap dapat berada dalam satu program yang sama.

---

## Thumb-2

ARM kemudian mengembangkan **Thumb-2**, yang mulai diperkenalkan pada **ARMv7**.

Thumb-2 tetap menggunakan instruction berukuran **2 byte**, tetapi juga menambahkan instruction baru yang memiliki ukuran **4 byte** sehingga seluruh fitur prosesor dapat didukung.

Banyak orang menganggap Thumb-2 merupakan gabungan antara ARM Mode dan Thumb Mode, tetapi anggapan tersebut sebenarnya tidak benar.

Thumb-2 adalah pengembangan dari Thumb agar mampu mendukung seluruh kemampuan prosesor dan dapat menggantikan ARM Mode.

Tujuan tersebut berhasil dicapai. Sebagian besar aplikasi pada **iPod**, **iPhone**, dan **iPad** dikompilasi menggunakan Thumb-2, terutama karena **Xcode** menjadikannya sebagai konfigurasi bawaan.

---

## ARM64

Ketika ARM memasuki era 64-bit, diperkenalkan ISA baru bernama **ARM64**.

ARM64 kembali menggunakan instruction dengan ukuran tetap, yaitu **4 byte**, sehingga tidak lagi memerlukan Thumb Mode.

Namun, perubahan menuju arsitektur 64-bit menyebabkan ARM kini memiliki tiga kelompok ISA yang berbeda:

- ARM Mode
- Thumb (termasuk Thumb-2)
- ARM64

Ketiganya memang memiliki beberapa bagian yang saling beririsan, tetapi tetap dianggap sebagai ISA yang berbeda, bukan hanya variasi dari ISA yang sama.

---

## Arsitektur RISC Lain

Selain ARM, masih terdapat beberapa arsitektur **RISC** lain yang menggunakan instruction tetap berukuran 32-bit, di antaranya:

- MIPS
- PowerPC
- Alpha AXP

---

# Catatan Pribadi

Dari materi ini saya memahami bahwa CPU hanya mampu menjalankan **machine code**, sedangkan manusia lebih nyaman menulis program menggunakan bahasa tingkat tinggi.

Agar kedua dunia tersebut dapat saling berkomunikasi, diperlukan **compiler** yang menerjemahkan kode menjadi machine code yang dapat dipahami CPU.

saya juga mulai memahami bahwa setiap prosesor memiliki **Instruction Set Architecture (ISA)** yang berbeda sehingga kumpulan instruction yang dimiliki pun tidak selalu sama.

Perkembangan ARM dari **ARM Mode**, **Thumb**, **Thumb-2**, hingga **ARM64** menunjukkan bagaimana sebuah ISA dapat terus berkembang untuk mendapatkan keseimbangan antara efisiensi ukuran kode, performa, dan kompatibilitas perangkat keras.



## Bagian 2: Fungsi yang Paling Sederhana

> Catatan belajar pribadi berdasarkan materi *Code Patterns Chapter 2*.

---

# Fungsi Paling Sederhana

Pada chapter ini saya mempelajari bagaimana sebuah fungsi yang paling sederhana diterjemahkan menjadi assembly pada beberapa arsitektur prosesor. Contoh fungsi yang digunakan sangat sederhana, yaitu sebuah fungsi yang hanya mengembalikan sebuah nilai konstan.

Contoh kode dalam bahasa C/C++ adalah sebagai berikut:

```c
int f()
{
    return 123;
}
```

Walaupun fungsi tersebut terlihat sangat sederhana, compiler tetap harus menerjemahkannya menjadi instruction yang dapat dijalankan oleh CPU. Menariknya, hasil assembly pada setiap arsitektur berbeda karena masing-masing memiliki **Instruction Set Architecture (ISA)** yang berbeda.

---

# 2.1 x86

Pada arsitektur **x86**, baik compiler **GCC** maupun **MSVC** menghasilkan assembly yang sangat sederhana.

```asm
f:
    mov eax, 123
    ret
```

Hanya terdapat dua instruction.

Instruction pertama, **MOV**, menyalin nilai **123** ke dalam register **EAX**. Pada arsitektur x86, register **EAX** secara konvensi digunakan sebagai tempat menyimpan nilai yang akan dikembalikan oleh sebuah fungsi (*return value*).

Instruction kedua adalah **RET**, yang berfungsi mengembalikan alur eksekusi ke fungsi yang memanggilnya (*caller*). Setelah fungsi selesai dijalankan, caller akan mengambil nilai hasil fungsi dari register **EAX**.

---

# 2.2 ARM

Pada arsitektur **ARM**, konsepnya tetap sama, tetapi implementasinya sedikit berbeda.

```asm
f PROC
    MOV r0, #123
    BX lr
ENDP
```

ARM menggunakan register **R0** sebagai tempat menyimpan nilai hasil dari sebuah fungsi. Oleh karena itu, nilai **123** disalin ke register **R0**.

Perbedaan lainnya terdapat pada proses kembali ke fungsi pemanggil. Pada ISA ARM, alamat untuk kembali (*return address*) tidak disimpan pada stack lokal, melainkan pada **Link Register (LR)**.

Instruction **BX LR** akan melompat ke alamat yang tersimpan di dalam **LR**, sehingga eksekusi program kembali ke fungsi yang sebelumnya memanggil fungsi tersebut.

Materi ini juga menjelaskan bahwa nama instruction **MOV** sebenarnya sedikit menyesatkan. Walaupun namanya berarti *move*, data yang diproses sebenarnya **tidak dipindahkan**, melainkan **disalin (copied)** sehingga nilai sumber tetap ada.

---

# 2.3 MIPS

Pada arsitektur **MIPS**, terdapat dua cara penamaan register.

Cara pertama menggunakan nomor register, yaitu **$0** sampai **$31**.

Cara kedua menggunakan nama yang lebih mudah dipahami (*pseudoname*), misalnya **$V0**, **$A0**, **$RA**, dan sebagainya.

Compiler GCC biasanya menampilkan register berdasarkan nomor.

```asm
j   $31
li  $2, 123
```

Sedangkan IDA menggunakan pseudoname.

```asm
jr  $ra
li  $v0, 0x7B
```

Walaupun penamaannya berbeda, register **$2** dan **$V0** sebenarnya merujuk pada register yang sama, yaitu register yang digunakan untuk menyimpan nilai hasil dari sebuah fungsi.

Instruction **LI (Load Immediate)** merupakan padanan dari instruction **MOV** pada x86 maupun ARM. Fungsinya adalah memasukkan nilai konstan secara langsung ke dalam register.

Instruction **J** atau **JR** digunakan untuk mengembalikan alur eksekusi ke fungsi pemanggil dengan melompat ke alamat yang tersimpan pada register **$31** atau **$RA (Return Address)**.

Register **$RA** memiliki fungsi yang serupa dengan **Link Register (LR)** pada ARM, yaitu menyimpan alamat untuk kembali setelah fungsi selesai dijalankan.

---

## Branch Delay Slot pada MIPS

Hal yang cukup menarik pada MIPS adalah urutan instruction terlihat sedikit berbeda dibandingkan arsitektur lainnya.

Sekilas terlihat bahwa instruction **jump (J/JR)** ditulis lebih dahulu dibandingkan **LI**, padahal nilai seharusnya dimasukkan ke register terlebih dahulu sebelum fungsi selesai.

Hal ini terjadi karena MIPS memiliki karakteristik arsitektur RISC yang disebut **Branch Delay Slot**.

Pada mekanisme ini, instruction yang berada tepat setelah instruction **jump** atau **branch** akan tetap dieksekusi terlebih dahulu sebelum proses perpindahan alur program benar-benar dilakukan.

Akibatnya, compiler biasanya menempatkan instruction yang harus dijalankan terakhir tepat setelah instruction **jump**, sehingga urutan assembly terlihat seolah-olah terbalik.

Materi ini juga menjelaskan bahwa alasan teknis di balik mekanisme tersebut tidak terlalu penting untuk dipahami pada tahap awal. Hal yang perlu diingat hanyalah bahwa pada MIPS, instruction setelah **jump** atau **branch** tetap akan dieksekusi sebelum perpindahan alur program benar-benar terjadi.

---

## Catatan Penamaan Register dan Instruction pada MIPS

Secara tradisional, nama register maupun instruction pada MIPS biasanya ditulis menggunakan huruf kecil (*lowercase*).

Namun, agar konsisten dengan arsitektur lain yang dibahas dalam buku ini, seluruh nama register dan instruction akan ditulis menggunakan huruf besar (*uppercase*).

---

# Catatan Pribadi

Dari chapter ini saya memahami bahwa walaupun sebuah fungsi hanya mengembalikan satu nilai konstan, compiler tetap harus menerjemahkannya menjadi instruction yang sesuai dengan arsitektur CPU yang digunakan.

Setiap ISA memiliki cara yang berbeda dalam menyimpan nilai hasil fungsi maupun mengembalikan alur eksekusi ke fungsi pemanggil. Pada **x86**, nilai hasil fungsi disimpan di register **EAX** dan dikembalikan menggunakan **RET**. Pada **ARM**, nilai hasil disimpan di **R0**, sedangkan alamat kembali disimpan di **Link Register (LR)** dan digunakan oleh instruction **BX LR**. Sementara itu, pada **MIPS**, nilai hasil disimpan pada **$V0/$2**, sedangkan alamat kembali berada pada **$RA/$31**, dengan karakteristik khusus berupa **Branch Delay Slot** yang menyebabkan urutan instruction tampak berbeda dibandingkan arsitektur lainnya.

Melalui contoh fungsi yang sangat sederhana ini, saya mulai memahami bahwa meskipun hasil akhirnya sama, yaitu mengembalikan nilai **123**, cara setiap arsitektur CPU menjalankannya bisa sangat berbeda sesuai dengan desain ISA masing-masing.


## Bagian 3: Hello, World!

> Catatan belajar pribadi berdasarkan materi *Code Patterns Chapter 3*.

---

# Hello, World!

Pada chapter ini, kita akan menggunakan contoh klasik yang sangat terkenal dari buku *“The C Programming Language”* [Ker88]:

```c
#include <stdio.h>
int main()
{
    printf("hello, world\n");
    return 0;
}

```

---

# 3.1 x86

Bagian ini membahas bagaimana kode di atas diterjemahkan ke dalam arsitektur x86 menggunakan beberapa compiler berbeda serta perbedaan gaya penulisan sintaksnya.

---

## 3.1.1 MSVC

Mari kita kompilasi kode tersebut menggunakan **MSVC 2010** dengan perintah berikut:

`cl 1.cpp /Fa1.asm`

*(Opsi `/Fa` digunakan untuk memerintahkan compiler agar menghasilkan file laporan atau assembly listing).*

Berikut adalah hasil *assembly listing* yang dihasilkan (menggunakan **Intel-syntax**):

```asm
CONST SEGMENT
$SG3830 DB 'hello, world', 0AH, 00H
CONST ENDS
PUBLIC _main
EXTRN _printf:PROC
; Function compile flags: /Odtp
_TEXT SEGMENT
_main PROC
push ebp
mov ebp, esp
push OFFSET $SG3830
call _printf
add esp, 4
xor eax, eax
pop ebp
ret 0
_main ENDP
_TEXT ENDS

```

Compiler menghasilkan file `1.obj`, yang nantinya akan dihubungkan (*linked*) menjadi `1.exe`. Dalam kasus ini, file tersebut berisi dua segmen utama: **CONST** (untuk konstanta data) dan **_TEXT** (untuk kode program).

String `"hello, world\n"` di dalam bahasa C/C++ memiliki tipe data `const char[]`, namun ia tidak memiliki nama variabel tersendiri di dalam kode. Oleh karena itu, compiler perlu menanganinya dengan mendefinisikan nama internal otomatis untuk string tersebut, yaitu **$SG3830**.

Sebab itu, contoh program di atas secara harfiah dapat ditulis ulang oleh compiler seperti ini:

```c
#include <stdio.h>
const char $SG3830[]="hello, world\n";
int main()
{
    printf($SG3830);
    return 0;
}

```

Jika kita kembali melihat *assembly listing* pada segmen **CONST**, string tersebut diakhiri dengan byte nol (`00H`), yang merupakan standar penanda akhir string (*null-terminated*) dalam bahasa C/C++. String ini juga memuat kode `0AH` yang merepresentasikan karakter *newline* (`\n`).

Di dalam segmen kode (**_TEXT**), sejauh ini hanya terdapat satu fungsi, yaitu `main()`. Fungsi `main()` dimulai dengan kode **prologue** (pembuka) dan diakhiri dengan kode **epilogue** (penutup), sama seperti karakteristik fungsi pada umumnya.

Setelah fungsi *prologue* selesai, kita melihat pemanggilan ke fungsi `printf()` melalui instruksi `CALL _printf`. Sebelum pemanggilan dilakukan, alamat memori string yang berisi teks tersebut dimasukkan ke dalam *stack* dengan bantuan instruksi **PUSH**.

Ketika fungsi `printf()` selesai dieksekusi dan mengembalikan kontrol ke fungsi `main()`, alamat string tadi sebenarnya masih tertinggal di dalam *stack*. Karena data tersebut sudah tidak dibutuhkan lagi, penunjuk stack (**ESP register**) perlu disesuaikan kembali ke posisi semula.

Instruksi `ADD ESP, 4` berarti menambahkan nilai 4 ke dalam register ESP. Mengapa harus nilai 4? Karena ini adalah program arsitektur 32-bit, sehingga kita membutuhkan tepat 4 byte untuk melewatkan sebuah alamat memori melalui *stack*. Jika ini adalah kode x64 (64-bit), kita akan membutuhkan nilai 8 byte. Instruksi `ADD ESP, 4` ini secara fungsional setara dengan melakukan instruksi **POP**, namun dijalankan tanpa menggunakan register pembantu sehingga tidak ada isi register lain yang terbuang atau tertimpa.

Untuk tujuan yang sama, beberapa compiler (seperti *Intel C++ Compiler*) lebih memilih mengeluarkan instruksi `POP ECX` alih-alih menggunakan **ADD**. Pola seperti ini dapat diamati pada kode *Oracle RDBMS* yang dikompilasi dengan Intel C++ Compiler. Instruksi ini memiliki efek pembersihan *stack* yang sama, namun isi dari register **ECX** akan tertimpa dengan data sampah. Intel C++ compiler kemungkinan besar menggunakan `POP ECX` karena kode operasi (*opcode*) instruksi ini lebih pendek, yaitu hanya memakan ruang **1 byte** dibandingkan instruksi `ADD ESP, x` yang memakan ruang **3 byte**.

Berikut adalah contoh penggunaan POP alih-alih ADD dari potongan kode Oracle RDBMS Linux:

```asm
.text:0800029A push ebx
.text:0800029B call qksfroChild
.text:080002A0 pop ecx

```

Setelah memanggil `printf()`, kode asli C/C++ kita berisi pernyataan `return 0;` sebagai hasil akhir dari fungsi `main()`. Di dalam kode assembly yang dihasilkan, hal ini diimplementasikan dengan instruksi `XOR EAX, EAX`.

**XOR** sebenarnya merupakan operasi logika *“eXclusive OR”*, tetapi pembuat compiler sering memanfaatkannya alih-alih menggunakan `MOV EAX, 0`. Alasannya kembali pada efisiensi, karena ukuran *opcode*-nya sedikit lebih pendek (hanya **2 byte** untuk XOR dibandingkan **5 byte** untuk MOV).

Beberapa compiler lain terkadang mengeluarkan instruksi `SUB EAX, EAX`, yang berarti mengurangi nilai di dalam register EAX dengan nilai EAX itu sendiri. Operasi pengurangan ini tentu saja akan menghasilkan nilai nol.

Instruksi terakhir, **RET**, bertugas mengembalikan kontrol ke fungsi pemanggil (*caller*). Biasanya, kendali akan diterima oleh kode C/C++ CRT (*Runtime*), yang pada gilirannya akan menyerahkan kembali kontrol sepenuhnya ke Sistem Operasi.

---

## 3.1.2 GCC

Sekarang mari kita coba mengompilasi kode C/C++ yang sama menggunakan compiler **GCC 4.4.1** di sistem operasi Linux dengan perintah:

`gcc 1.c -o 1`

Selanjutnya, dengan bantuan *disassembler* **IDA**, mari kita lihat bagaimana fungsi `main()` dibuat. IDA, seperti halnya MSVC, menampilkan kode dalam bentuk **Intel-syntax**.

```asm
main proc near
var_10 = dword ptr -10h
push ebp
mov ebp, esp
and esp, 0FFFFFFF0h
sub esp, 10h
mov eax, offset aHelloWorld ; "hello, world\n"
mov [esp+10h+var_10], eax
call _printf
mov eax, 0
leave
retn
main endp

```

Hasil akhir yang didapatkan sebenarnya hampir sama. Alamat dari string *"hello, world\n"* (yang disimpan di segmen data) pertama-tama dimuat ke dalam register **EAX**, baru kemudian disimpan ke dalam *stack*.

Namun, ada perbedaan mendetail pada bagian fungsi *prologue*, di mana terdapat instruksi `AND ESP, 0FFFFFFF0h`. Instruksi ini berfungsi untuk meluruskan (*align*) nilai register ESP pada batas kelipatan 16-byte. Hal ini menyebabkan semua nilai di dalam *stack* dipaksa disejajarkan dengan cara yang sama. CPU dapat berkinerja jauh lebih baik dan cepat jika data yang ditanganinya berada di alamat memori yang selaras pada kelipatan 4-byte atau 16-byte.

Instruksi `SUB ESP, 10h` berfungsi mengalokasikan ruang sebesar 16 byte (`10h` dalam heksadesimal) pada *stack*. Meskipun jika kita perhatikan alur berikutnya, program ini sebenarnya hanya membutuhkan ruang 4 byte saja untuk melewatkan alamat string. Alokasi berlebih ini terjadi karena ukuran ruang *stack* yang disiapkan juga harus ikut diselaraskan pada batas kelipatan 16-byte.

Alamat string (atau penunjuk ke string) kemudian disimpan secara langsung ke posisi *stack* yang ditentukan tanpa menggunakan instruksi **PUSH**. Di sini, **var_10** diidentifikasi sebagai variabel lokal yang sekaligus bertindak sebagai wadah argumen untuk fungsi `printf()`. Setelah argumen siap di memori, barulah fungsi `printf()` dipanggil.

Berbeda dengan MSVC, ketika GCC mengompilasi kode tanpa mengaktifkan fitur optimasi, ia akan memuat nilai kembalian menggunakan instruksi standar `MOV EAX, 0` alih-alih menggunakan instruksi `XOR` yang memiliki *opcode* lebih pendek.

Instruksi terakhir sebelum *return*, yaitu **LEAVE**, merupakan instruksi tunggal yang setara dengan sepasang instruksi `MOV ESP, EBP` dan `POP EBP`. Dengan kata lain, instruksi ini bertugas mengatur penunjuk stack (ESP) kembali ke posisi semula serta memulihkan register **EBP** ke kondisi awalnya. Langkah ini wajib dilakukan karena kita telah memodifikasi nilai kedua register penunjuk tersebut di bagian awal fungsi.

---

## 3.1.3 GCC: AT&T Syntax

Mari kita lihat bagaimana kode yang sama direpresentasikan jika ditulis menggunakan **AT&T syntax**. Gaya penulisan sintaks ini jauh lebih populer dan menjadi standar di lingkungan sistem operasi keluarga UNIX.

Mari kita kompilasi di GCC 4.7.3 menggunakan perintah:

`gcc -S 1_1.c`

Kita akan mendapatkan hasil laporan kode seperti ini:

```asm
.file "1_1.c"
.section .rodata
.LC0:
.string "hello, world\n"
.text
.globl main
.type main, @function
main:
.LFB0:
.cfi_startproc
pushl %ebp
.cfi_def_cfa_offset 8
.cfi_offset 5, -8
movl %esp, %ebp
.cfi_def_cfa_register 5
andl $-16, %esp
subl $16, %esp
movl $.LC0, (%esp)
call printf
movl $0, %eax
leave
.cfi_restore 5
.cfi_def_cfa 4, 4
ret
.cfi_endproc
.LFE0:
.size main, .-main
.ident "GCC: (Ubuntu/Linaro 4.7.3-1ubuntu1) 4.7.3"
.section .note.GNU-stack,"",@progbits

```

Laporan di atas mengandung banyak sekali kode makro assembler (ditandai dengan awalan tanda titik seperti `.cfi_`). Bagian makro ini belum terlalu penting untuk kita pelajari pada tahap awal. Untuk menyederhanakan pengamatan, kita dapat mengabaikan makro-makro tersebut, kecuali makro `.string` yang bertugas mengodekan urutan karakter string berakhiran nol.

Jika dibersihkan dari makro pembantu, bentuk ringkasnya akan terlihat sebagai berikut:

```asm
.LC0:
.string "hello, world\n"
main:
pushl %ebp
movl %esp, %ebp
andl $-16, %esp
subl $16, %esp
movl $.LC0, (%esp)
call printf
movl $0, %eax
leave
ret

```

Beberapa perbedaan utama yang membedakan **Intel-syntax** dengan **AT&T-syntax** adalah:

* **Urutan operan sumber (*source*) dan tujuan (*destination*) ditulis terbalik.**
* Pada **Intel-syntax**: `<instruction> <destination> <source>`.
* Pada **AT&T-syntax**: `<instruction> <source> <destination>`.


Cara mudah untuk mengingat perbedaannya: ketika Anda membaca kode berbasis **Intel-syntax**, bayangkan ada tanda sama dengan di antara kedua operan (`Tujuan = Asal`). Sebaliknya, ketika berurusan dengan **AT&T-syntax**, bayangkan ada tanda panah proses ke arah kanan (`Asal ➔ Tujuan`).
* **Penggunaan Simbol Karakter Khusus:** Pada AT&T, setiap penulisan nama register wajib diawali dengan simbol persen (`%`) dan penulisan angka langsung (*literal value*) wajib diawali dengan simbol dolar (`$`). Selain itu, tanda kurung biasa `()` digunakan untuk mengakses alamat memori, menggantikan tanda kurung siku `[]` yang digunakan pada Intel.
* **Sufiks Ukuran Data pada Instruksi:** AT&T menambahkan satu huruf akhiran (sufiks) di tiap nama instruksinya untuk mempertegas ukuran data yang sedang diproses:
* **q** — quad (64 bit)
* **l** — long (32 bit)
* **w** — word (16 bit)
* **b** — byte (8 bit)
*(Contoh: `movl` berarti operasi pemindahan/penyalinan data berukuran long atau 32-bit).*



Mari kita amati kembali hasil kompilasi AT&T di atas: secara struktural kodenya identik dengan apa yang kita lihat di IDA, namun ada sedikit perbedaan visual yang samar. Nilai penyelarasan memori `0FFFFFFF0h` (pada format Intel) disajikan sebagai `$-16` (pada format AT&T). Kedua nilai ini sebenarnya bernilai **sama**. Angka 16 dalam desimal adalah `0x10` dalam heksadesimal, dan nilai `-0x10` pada tipe data 32-bit nilainya setara dengan `0xFFFFFFF0`.

Satu hal lagi yang perlu diperhatikan: nilai kembalian diatur ke angka 0 dengan menggunakan instruksi **MOV** standar (`movl $0, %eax`), bukan instruksi **XOR**. Instruksi `MOV` sendiri hanya bertugas memuat atau menyalin nilai ke dalam register. Penamaan komponen instruksi ini sebenarnya kurang tepat (*misnomer*) karena data tidak benar-benar dipindahkan, melainkan digandakan. Pada arsitektur komputer lain, instruksi sejenis ini biasanya langsung dinamai dengan istilah `LOAD` atau `STORE`.

---

# Catatan Pribadi

Dari chapter ini, saya memahami bahwa program sederhana seperti *"Hello, World!"* sekalipun melibatkan mekanisme manajemen memori dan penataan komponen yang cukup kompleks saat diterjemahkan ke tingkat assembly. Compiler memisahkan konstanta string ke dalam segmen khusus seperti **CONST** atau **.rodata** dengan memberikan pengenal nama internal otomatis, sementara kode instruksi yang dieksekusi diletakkan terpisah di segmen **_TEXT** atau **.text**.

Saya juga mempelajari perbedaan penanganan *stack* antar compiler di arsitektur x86: **MSVC** cenderung memanfaatkan instruksi `PUSH` untuk memasukkan argumen dan membersihkannya pasca-eksekusi menggunakan `ADD ESP, 4` (atau `POP ECX` pada compiler tertentu demi efisiensi ukuran kode), sementara **GCC** lebih suka melakukan penyelarasan alamat *stack* terlebih dahulu menggunakan instruksi pemotong seperti `AND ESP, 0FFFFFFF0h` guna mengoptimalkan kinerja pembacaan memori oleh perangkat keras CPU.

Terakhir, perbedaan gaya penulisan sintaks antara **Intel** dan **AT&T** sangat krusial dipahami saat melakukan *reverse engineering*. Struktur AT&T yang dominan di sistem UNIX membalik urutan operan (asal ke tujuan), serta menambahkan simbol penanda khusus (`%` untuk register, `$` untuk literal) dan sufiks ukuran data (`q`, `l`, `w`, `b`), yang membuat tampilan visualnya terlihat berbeda secara signifikan meskipun secara fungsional tetap menghasilkan instruksi mesin yang sama.



---


# 3.2 x86-64

Setelah sebelumnya melihat bagaimana compiler menghasilkan kode untuk arsitektur x86 32-bit, sekarang kita akan melihat bagaimana hasil kompilasi berubah ketika target arsitekturnya menggunakan **x86-64** atau sering disebut juga **AMD64**.

Perubahan terbesar pada mode 64-bit bukan hanya sekadar memperbesar ukuran register dari 32-bit menjadi 64-bit, tetapi juga membawa perubahan besar pada **Application Binary Interface (ABI)**, terutama dalam cara melewatkan argumen fungsi.

Pada arsitektur x86 32-bit, compiler biasanya menggunakan **stack** untuk mengirim parameter fungsi. Namun pada x86-64, karena jumlah register yang tersedia lebih banyak, compiler modern mulai memanfaatkan register untuk melewatkan argumen.

Tujuan utamanya adalah mengurangi akses ke memori eksternal (*stack* berada di memori), sehingga program dapat berjalan lebih cepat karena lebih sedikit melakukan operasi baca/tulis memori.

---

## 3.2.1 MSVC — x86-64

Mari kita mencoba mengompilasi program yang sama menggunakan **MSVC 2012 x64**.

Hasil assembly listing yang diperoleh:

```asm
$SG2989 DB 'hello, world', 0AH, 00H

main PROC
sub rsp, 40
lea rcx, OFFSET FLAT:$SG2989
call printf
xor eax, eax
add rsp, 40
ret 0

main ENDP
````

Jika dibandingkan dengan versi x86 32-bit sebelumnya, terdapat beberapa perubahan yang cukup jelas.

Pada arsitektur x86-64, seluruh register diperluas menjadi ukuran 64-bit. Nama register baru diberikan dengan tambahan awalan **R**.

Sebagai contoh:

| 32-bit | 64-bit |
| ------ | ------ |
| EAX    | RAX    |
| EBX    | RBX    |
| ECX    | RCX    |
| EDX    | RDX    |

Namun untuk menjaga kompatibilitas dengan program lama, bagian register yang lebih kecil tetap dapat digunakan.

Sebagai contoh, register **RAX** dapat diakses dalam beberapa ukuran:

```
RAX (64-bit)
 |
 +-- EAX (32-bit)
      |
      +-- AX (16-bit)
           |
           +-- AH / AL (8-bit)
```

Artinya, sebuah instruksi masih dapat bekerja pada bagian tertentu saja dari register tersebut.

---

Pada x86-64 Windows, mekanisme pemanggilan fungsi menggunakan aturan yang disebut **fastcall calling convention**.

Berbeda dengan x86 32-bit yang banyak menggunakan stack, pada Win64 empat argumen pertama fungsi dikirim melalui register:

| Urutan argumen  | Register |
| --------------- | -------- |
| Argumen pertama | RCX      |
| Argumen kedua   | RDX      |
| Argumen ketiga  | R8       |
| Argumen keempat | R9       |

Sedangkan argumen berikutnya baru akan menggunakan stack.

Karena fungsi `printf()` hanya menerima satu argumen, yaitu pointer menuju string `"hello, world\n"`, maka alamat string tersebut tidak lagi dimasukkan ke stack seperti pada x86 32-bit.

Sebaliknya, alamat string langsung dimasukkan ke register **RCX**:

```asm
lea rcx, OFFSET FLAT:$SG2989
call printf
```

Instruksi `LEA` (*Load Effective Address*) digunakan untuk mendapatkan alamat memori dari string tersebut.

Dengan kata lain, kode C:

```c
printf("hello, world\n");
```

secara konsep berubah menjadi:

```c
char *str = "hello, world\n";
printf(str);
```

kemudian pointer `str` dikirim melalui register RCX.

---

Hal menarik lainnya adalah bagian akhir fungsi:

```asm
xor eax, eax
```

Pada program C/C++, fungsi `main()` mengembalikan nilai bertipe `int`.

Walaupun sistem sudah menggunakan arsitektur 64-bit, tipe data `int` masih tetap berukuran 32-bit demi menjaga kompatibilitas dan portabilitas program lama.

Karena nilai yang dikembalikan adalah:

```c
return 0;
```

maka compiler hanya perlu membersihkan bagian **EAX**, bukan seluruh **RAX**.

Instruksi:

```asm
xor eax, eax
```

menghasilkan nilai:

```
EAX = 0
```

dan karena aturan x86-64 menyatakan bahwa penulisan pada register 32-bit otomatis membersihkan bagian atasnya, maka:

```
RAX = 0000000000000000
```

juga tercapai.

---

Pada awal fungsi terdapat instruksi:

```asm
sub rsp, 40
```

Instruksi ini mengalokasikan 40 byte ruang pada stack.

Bagi programmer yang terbiasa dengan x86 32-bit, angka ini terlihat cukup besar karena program sederhana ini hanya membutuhkan satu pointer string.

Namun pada Windows x64, ruang tersebut bukan hanya untuk variabel lokal.

Ruang ini disebut:

**Shadow Space**

atau sering juga disebut:

**Home Space**

Shadow space adalah area stack yang harus disediakan oleh fungsi pemanggil untuk menyimpan empat register argumen:

```
RCX
RDX
R8
R9
```

Walaupun fungsi yang dipanggil mungkin tidak menggunakannya, caller tetap wajib menyediakan ruang tersebut.

Ukuran setiap register adalah 8 byte:

```
4 register × 8 byte = 32 byte
```

Ditambah kebutuhan alignment stack:

```
32 byte + 8 byte alignment = 40 byte
```

Karena itu compiler melakukan:

```asm
sub rsp, 40
```

sebelum memanggil `printf()`.

Setelah fungsi selesai:

```asm
add rsp, 40
```

mengembalikan posisi stack seperti semula.

---

# 3.2.2 GCC — x86-64

Sekarang kita melihat bagaimana GCC menghasilkan kode pada sistem Linux 64-bit.

Contoh hasil kompilasi GCC 4.4.6 x64:

```asm
.string "hello, world\n"

main:
sub rsp, 8
mov edi, OFFSET FLAT:.LC0
xor eax, eax
call printf
xor eax, eax
add rsp, 8
ret
```

Sekilas terlihat mirip dengan versi Windows, tetapi terdapat beberapa perbedaan penting.

Linux, BSD, dan Mac OS X menggunakan aturan pemanggilan fungsi yang berbeda dengan Windows.

Pada sistem berbasis UNIX, enam argumen pertama dikirim melalui register:

| Urutan argumen | Register |
| -------------- | -------- |
| 1              | RDI      |
| 2              | RSI      |
| 3              | RDX      |
| 4              | RCX      |
| 5              | R8       |
| 6              | R9       |

Sedangkan argumen berikutnya menggunakan stack.

Karena `printf()` hanya memiliki satu argumen, pointer string dikirim melalui register pertama:

```
RDI
```

Namun yang digunakan GCC adalah:

```asm
mov edi, OFFSET FLAT:.LC0
```

bukan:

```asm
mov rdi, OFFSET FLAT:.LC0
```

Mengapa demikian?

---

Pada mode 64-bit, terdapat aturan penting:

Jika sebuah instruksi menulis nilai ke bagian bawah register 32-bit, maka bagian atas register tersebut otomatis dibersihkan.

Contoh:

```asm
mov eax, 11223344h
```

hasilnya:

```
RAX = 0000000011223344h
```

Bagian atas 32-bit otomatis menjadi nol.

Hal yang sama berlaku pada:

```asm
mov edi, OFFSET FLAT:.LC0
```

Instruksi tersebut sebenarnya mengisi:

```
EDI
```

tetapi efek akhirnya adalah:

```
RDI = 00000000xxxxxxxx
```

sehingga register 64-bit tetap memiliki alamat yang valid.

---

Jika kita melihat opcode hasil kompilasi:

```asm
.text:00000000004004D0 main proc near

48 83 EC 08
sub rsp, 8

BF E8 05 40 00
mov edi, offset format

31 C0
xor eax, eax

E8 D8 FE FF FF
call printf

31 C0
xor eax, eax

48 83 C4 08
add rsp, 8

C3
ret
```

Terlihat bahwa instruksi:

```asm
mov edi, offset format
```

hanya membutuhkan 5 byte.

Sedangkan jika menggunakan register 64-bit:

```asm
mov rdi, offset format
```

ukurannya menjadi 7 byte.

GCC memilih versi 32-bit karena lebih hemat ukuran kode.

Compiler dapat melakukan optimasi ini karena mengetahui bahwa segmen data yang berisi string tidak mungkin berada di alamat lebih besar dari 4GB pada program tersebut.

---

Hal menarik lainnya terdapat pada:

```asm
xor eax, eax
call printf
```

Instruksi pembersihan EAX sebelum memanggil `printf()` bukan hanya untuk mengatur nilai kembali.

Pada sistem UNIX x86-64, register **EAX digunakan untuk menyimpan jumlah register vector yang digunakan ketika melewatkan argumen fungsi**.

Register vector tersebut berkaitan dengan instruksi SIMD seperti:

* SSE
* AVX

Karena fungsi `printf()` tidak menerima argumen vector, maka compiler harus memastikan nilai EAX adalah:

```
0
```

sebelum melakukan pemanggilan fungsi.

---

# Perbandingan x86-64 Windows dan Linux

| Aspek        | Windows x64   | Linux x64 |
| ------------ | ------------- | --------- |
| Argumen 1    | RCX           | RDI       |
| Argumen 2    | RDX           | RSI       |
| Argumen 3    | R8            | RDX       |
| Argumen 4    | R9            | RCX       |
| Argumen 5    | Stack         | R8        |
| Argumen 6    | Stack         | R9        |
| Shadow space | Ada (32 byte) | Tidak ada |
| Return value | EAX/RAX       | EAX/RAX   |

Dari sini terlihat bahwa walaupun sama-sama menggunakan CPU x86-64, sistem operasi yang berbeda dapat memiliki aturan ABI yang berbeda pula.

Bagi seorang programmer biasa perbedaan ini tidak terlihat karena compiler menangani semuanya secara otomatis.

Namun dalam reverse engineering, debugging tingkat rendah, atau analisis malware, memahami aturan ini sangat penting karena kita harus mampu membaca bagaimana sebuah fungsi menerima dan mengirim data.



# 3.3 GCC — one more thing

Pada pembahasan sebelumnya kita sudah mengetahui bahwa string literal dalam bahasa C/C++ memiliki sifat **const**.

Sebagai contoh:

```c
printf("hello world\n");
````

String `"hello world\n"` bukanlah array karakter biasa yang dapat dimodifikasi. Compiler biasanya menyimpan string seperti ini di bagian khusus dari executable yang disebut **constant segment** atau **read-only data segment**.

Karena compiler sudah mengetahui bahwa string tersebut tidak akan berubah selama program berjalan, compiler memiliki kebebasan melakukan beberapa optimasi terhadap penyimpanannya.

Salah satu optimasi menarik yang dilakukan GCC adalah menggunakan **bagian tertentu dari sebuah string** untuk merepresentasikan string lainnya.

Mari kita lihat contoh berikut:

```c
#include <stdio.h>

int f1()
{
    printf("world\n");
}

int f2()
{
    printf("hello world\n");
}

int main()
{
    f1();
    f2();
}
```

Secara sederhana, kita mungkin mengira compiler akan menyimpan dua string terpisah:

```
world\n
hello world\n
```

Compiler umum seperti MSVC memang melakukan hal tersebut.

Namun GCC 4.8.1 memiliki pendekatan berbeda.

Hasil disassembly menggunakan IDA:

```asm
f1 proc near

s = dword ptr -1Ch

sub esp, 1Ch
mov [esp+1Ch+s], offset s ; "world\n"
call _puts
add esp, 1Ch

retn

f1 endp


f2 proc near

s = dword ptr -1Ch

sub esp, 1Ch
mov [esp+1Ch+s], offset aHello ; "hello "
call _puts
add esp, 1Ch

retn

f2 endp


aHello db 'hello '

s db 'world',0xa,0
```

Jika diperhatikan, GCC hanya menyimpan:

```
hello world\n
```

sebagai satu blok data:

```
alamat:
+-----------------------+
| h e l l o             |
+-----------------------+
| w o r l d \n \0       |
+-----------------------+
```

Kemudian GCC membuat dua referensi berbeda:

Untuk fungsi `f2()`:

```asm
mov [esp+1Ch+s], offset aHello
```

yang menunjuk ke awal string:

```
hello world\n
^
|
aHello
```

Sedangkan fungsi `f1()`:

```asm
mov [esp+1Ch+s], offset s
```

menunjuk langsung ke bagian:

```
hello world\n
      ^
      |
      s
```

yaitu karakter:

```
world\n
```

---

Hal yang menarik adalah fungsi `puts()` tidak mengetahui bahwa string tersebut merupakan bagian dari string lain.

Ketika `puts()` dipanggil dari `f1()`, ia hanya melihat:

```
world\n\0
```

dan berhenti ketika menemukan byte null (`00H`).

Ia tidak peduli bahwa beberapa byte sebelumnya terdapat:

```
hello 
```

Dengan kata lain, string tersebut sebenarnya **tidak terbagi secara fisik**.

Pembagian hanya terjadi secara logika melalui pointer yang berbeda.

---

Optimasi ini dapat menghemat penggunaan memori karena compiler tidak perlu menyimpan data yang sama berkali-kali.

Sebagai contoh:

Tanpa optimasi:

```
hello world\n\0

world\n\0
```

Total:

```
14 + 7 = 21 byte
```

Dengan optimasi:

```
hello world\n\0
```

Total:

```
14 byte
```

Penghematan terlihat kecil pada contoh sederhana, tetapi pada program besar dengan banyak string konstan, penghematan ini dapat menjadi signifikan.

Teknik seperti ini adalah salah satu contoh bagaimana compiler dapat melakukan analisis tingkat tinggi terhadap kode sumber dan menghasilkan representasi mesin yang lebih efisien.

---

# 3.4 ARM

Setelah sebelumnya membahas arsitektur x86 dan x86-64, sekarang kita berpindah ke keluarga prosesor yang berbeda, yaitu **ARM**.

ARM memiliki desain yang cukup berbeda dibandingkan x86.

Jika x86 dikenal sebagai arsitektur **CISC (Complex Instruction Set Computer)** dengan instruksi yang memiliki ukuran bervariasi, ARM lebih dekat dengan konsep **RISC (Reduced Instruction Set Computer)**.

Karakteristik umum ARM:

* Instruksi lebih sederhana.
* Banyak operasi dilakukan melalui register.
* Instruksi biasanya memiliki ukuran tetap.
* Lebih mengutamakan efisiensi energi.

Karena karakteristik tersebut, ARM banyak digunakan pada:

* perangkat mobile,
* embedded system,
* IoT,
* perangkat elektronik kecil.

Dalam eksperimen ini digunakan beberapa compiler:

* **Keil Release 6/2013**, populer pada dunia embedded.
* **Apple Xcode 4.6.3** dengan LLVM-GCC 4.2.
* **GCC 4.9 Linaro** untuk ARM64.

Sebagian besar contoh menggunakan ARM 32-bit.

Jika membahas versi 64-bit ARM, biasanya disebut:

```
ARM64
```

---

# 3.4.1 Non-optimizing Keil 6/2013 (ARM mode)

Mari kita mulai dengan mengompilasi program Hello World menggunakan Keil:

```bash
armcc.exe --arm --c90 -O0 1.c
```

Compiler menghasilkan assembly listing dalam gaya Intel-syntax, tetapi menggunakan beberapa macro khusus ARM.

Agar lebih mudah dipahami, kita melihat hasil akhirnya melalui IDA.

Hasil disassembly:

```asm
.text:00000000 main

.text:00000000 10 40 2D E9
STMFD SP!, {R4,LR}

.text:00000004 1E 0E 8F E2
ADR R0, aHelloWorld ; "hello, world"

.text:00000008 15 19 00 EB
BL __2printf

.text:0000000C 00 00 A0 E3
MOV R0, #0

.text:00000010 10 80 BD E8
LDMFD SP!, {R4,PC}


.text:000001EC
DCB "hello, world",0
```

Karena program dikompilasi dalam **ARM mode**, setiap instruksi memiliki ukuran:

```
32 bit = 4 byte
```

Berbeda dengan Thumb mode yang menggunakan instruksi lebih kecil.

---

## STMFD — menyimpan register ke stack

Instruksi pertama:

```asm
STMFD SP!, {R4,LR}
```

memiliki fungsi yang mirip dengan:

```asm
PUSH {R4,LR}
```

pada x86.

Instruksi ini menyimpan dua register:

```
R4
LR
```

ke dalam stack.

Namun ada perbedaan penting.

Pada ARM mode, instruksi sebenarnya bukan `PUSH`.

`PUSH` hanya tersedia pada Thumb mode.

Bentuk asli ARM adalah:

```
STMFD
```

yang merupakan singkatan:

```
Store Multiple Full Descending
```

Artinya:

* Store → menyimpan data.
* Multiple → dapat menyimpan banyak register sekaligus.
* Full Descending → stack bergerak turun.

---

Sebelum menyimpan data, instruksi ini terlebih dahulu mengurangi nilai:

```
SP (Stack Pointer)
```

agar menunjuk ke area kosong baru.

Kemudian nilai register:

```
R4
LR
```

disimpan pada lokasi tersebut.

Keuntungan ARM adalah satu instruksi dapat menyimpan beberapa register sekaligus.

Hal seperti ini tidak memiliki padanan langsung pada x86.

Pada x86 biasanya harus dilakukan:

```asm
push eax
push ebx
push ecx
```

satu per satu.

---

## ADR dan konsep Position Independent Code

Instruksi berikut:

```asm
ADR R0, aHelloWorld
```

digunakan untuk mendapatkan alamat string:

```
"hello, world"
```

Keunikannya adalah ARM menggunakan konsep:

```
PC-relative addressing
```

atau sering disebut:

```
Position Independent Code (PIC)
```

Artinya kode program dapat ditempatkan di alamat memori mana saja tanpa perlu mengetahui alamat absolutnya.

Caranya adalah menggunakan nilai register:

```
PC (Program Counter)
```

dan menambahkan offset.

Misalnya:

```
Alamat string - alamat instruksi ADR
```

selalu memiliki nilai yang sama.

Karena jaraknya relatif tetap, program tetap berjalan walaupun sistem operasi meletakkannya pada lokasi berbeda di memori.

---

## BL — Branch with Link

Instruksi:

```asm
BL __2printf
```

digunakan untuk memanggil fungsi:

```c
printf()
```

Cara kerjanya berbeda dengan x86.

Pada x86:

```
return address
```

biasanya disimpan di stack.

Pada ARM:

```
return address
```

disimpan di register khusus:

```
LR (Link Register)
```

Ketika menjalankan:

```asm
BL printf
```

prosesornya melakukan:

1. Menyimpan alamat instruksi berikutnya ke LR.
2. Mengubah PC menuju fungsi printf.

Sehingga:

```
LR = alamat kembali
PC = alamat printf
```

Ketika printf selesai, fungsi cukup mengembalikan nilai LR ke PC.

---

Ini merupakan salah satu perbedaan utama antara ARM dan x86.

Pada ARM:

```
return address → LR register
```

Pada x86:

```
return address → stack
```

Karena ARM memiliki banyak register dan desain yang lebih sederhana, pendekatan ini dapat dilakukan dengan lebih efisien.

---

## MOV R0, #0

Instruksi:

```asm
MOV R0, #0
```

mengatur nilai return fungsi.

Dalam ARM calling convention:

```
R0
```

digunakan untuk menyimpan nilai kembali fungsi.

Karena fungsi C:

```c
return 0;
```

maka compiler menghasilkan:

```asm
MOV R0, #0
```

---

## LDMFD — mengembalikan register

Instruksi terakhir:

```asm
LDMFD SP!, {R4,PC}
```

merupakan kebalikan dari:

```asm
STMFD
```

Jika STMFD menyimpan register ke stack:

```
Register → Stack
```

maka LDMFD mengambil kembali:

```
Stack → Register
```

Instruksi ini mengembalikan:

```
R4
PC
```

Karena nilai PC diisi dengan alamat yang sebelumnya tersimpan, eksekusi otomatis kembali ke fungsi pemanggil.

Dengan teknik ini ARM tidak membutuhkan instruksi:

```asm
BX LR
```

pada akhir fungsi tertentu.

---

Directive terakhir:

```asm
DCB "hello, world",0
```

berfungsi mendefinisikan data byte.

Fungsinya sama seperti:

```asm
DB
```

pada assembly x86.

Artinya compiler membuat array byte:

```
68 65 6C 6C 6F ...
```

ditambah byte:

```
00
```

sebagai terminator string C.



## More about Thunk Functions

Konsep **thunk function** sering terasa membingungkan karena namanya sendiri kurang menggambarkan fungsinya.

Cara paling sederhana untuk memahami thunk adalah dengan menganggapnya sebagai:

> sebuah adaptor atau penghubung yang mengubah satu bentuk interface menjadi bentuk interface lainnya.

Analogi sederhananya adalah adaptor colokan listrik.

Misalnya, seseorang memiliki perangkat dengan colokan listrik Inggris, tetapi ingin menggunakannya pada stop kontak Amerika.

Perangkat tersebut sebenarnya tidak berubah.

Yang berubah hanyalah lapisan penghubung antara dua standar yang berbeda.

Thunk function bekerja dengan konsep yang hampir sama.

Thunk juga sering disebut sebagai:

- **wrapper function**
- **adapter function**

---

Secara historis, istilah thunk pertama kali diperkenalkan oleh:

**P. Z. Ingerman**

pada tahun 1961 ketika bekerja dengan bahasa pemrograman **Algol-60**.

Menurut definisinya, thunk adalah:

> "A piece of coding which provides an address."

Artinya, sebuah potongan kode yang menyediakan sebuah alamat.

Dalam konteks Algol-60, ketika sebuah prosedur dipanggil dengan ekspresi sebagai parameter, compiler dapat membuat thunk yang bertugas:

1. menghitung nilai ekspresi tersebut,
2. menyimpan hasilnya,
3. menyediakan alamat hasil tersebut pada lokasi standar.

Dengan kata lain, thunk menjadi perantara antara apa yang diminta program dan bagaimana data tersebut sebenarnya diproses.

---

Contoh modern yang lebih mudah ditemukan adalah kompatibilitas antar versi arsitektur.

Microsoft dan IBM pernah memiliki dua lingkungan berbeda:

- lingkungan 16-bit,
- lingkungan 32-bit.

Keduanya dapat berjalan pada komputer dan sistem operasi yang sama melalui teknologi:

```text
WOW
(Windows On Windows)
````

Ketika sistem harus berpindah dari:

```text
16-bit → 32-bit
```

atau sebaliknya:

```text
32-bit → 16-bit
```

proses konversi tersebut disebut:

```text
thunking
```

Microsoft bahkan menyediakan tool khusus:

```text
THUNK.EXE
```

yang disebut sebagai:

```text
thunk compiler
```

---

Dalam contoh ARM sebelumnya, thunk digunakan untuk menghubungkan kode Thumb dengan library eksternal yang menggunakan ARM mode.

Masalahnya adalah:

* kode utama menggunakan Thumb mode,
* fungsi eksternal berada dalam ARM mode,
* alamat fungsi dinamis belum diketahui saat proses kompilasi.

Solusinya adalah membuat fungsi kecil perantara.

Alurnya:

```text
Thumb code
    |
    v
Thunk function (ARM mode)
    |
    v
Dynamic library function
```

Thunk tidak melakukan pekerjaan utama.

Tugasnya hanya:

1. menerima kontrol,
2. mengambil alamat fungsi sebenarnya,
3. meneruskan eksekusi.

---

# 3.4.5 ARM64

Setelah melihat ARM 32-bit dengan berbagai mode seperti:

* ARM mode,
* Thumb mode,
* Thumb-2 mode,

sekarang kita melihat versi 64-bit ARM yang disebut:

```text
ARM64
```

Berbeda dengan ARM 32-bit, ARM64 tidak memiliki:

* Thumb,
* Thumb-2.

ARM64 hanya menggunakan instruksi ARM dengan ukuran tetap:

```text
32-bit = 4 byte
```

Pada ARM64 jumlah register juga bertambah dan ukuran register menjadi lebih besar.

Register 64-bit menggunakan awalan:

```text
X
```

sedangkan bagian 32-bit dari register tersebut menggunakan:

```text
W
```

Contoh:

```text
X0 = register 64-bit

W0 = 32-bit bagian bawah dari X0
```

---

## GCC ARM64

Mari kita compile contoh Hello World menggunakan:

```bash
GCC 4.8.1 ARM64
```

Hasil objdump:

```asm
0000000000400590 <main>:

400590:
a9bf7bfd
stp x29, x30, [sp,#-16]!

400594:
910003fd
mov x29, sp

400598:
90000000
adrp x0, 400000 <_init-0x3b8>

40059c:
91192000
add x0, x0, #0x648

4005a0:
97ffffa0
bl 400420 <puts@plt>

4005a4:
52800000
mov w0, #0x0

4005a8:
a8c17bfd
ldp x29, x30, [sp],#16

4005ac:
d65f03c0
ret
```

---

## STP — Store Pair

Instruksi pertama:

```asm
stp x29, x30, [sp,#-16]!
```

adalah:

```text
Store Pair
```

yang berarti menyimpan dua register sekaligus.

Register yang disimpan:

```text
X29
X30
```

Karena register ARM64 berukuran 64-bit:

```text
1 register = 8 byte
```

maka:

```text
2 register × 8 byte = 16 byte
```

itulah alasan stack dikurangi sebesar 16 byte.

---

Tanda:

```asm
!
```

memiliki arti penting.

Instruksi ini menggunakan metode:

```text
pre-index
```

Artinya:

1. SP dikurangi terlebih dahulu.
2. Data baru ditulis ke stack.

Secara konsep:

```asm
sub sp, sp, #16
store x29
store x30
```

Jika dibandingkan dengan x86:

```asm
push x29
push x30
```

---

Register:

```text
X29
```

digunakan sebagai:

```text
Frame Pointer (FP)
```

Sedangkan:

```text
X30
```

adalah:

```text
Link Register (LR)
```

Karena keduanya diperlukan selama fungsi berjalan, keduanya disimpan saat function prologue dan dikembalikan saat function epilogue.

---

## Membuat Stack Frame

Instruksi berikut:

```asm
mov x29, sp
```

menyalin nilai stack pointer ke frame pointer.

Tujuannya adalah membuat referensi tetap terhadap stack frame fungsi.

Konsepnya:

```text
SP → berubah selama fungsi berjalan

FP → tetap menjadi titik referensi
```

Hal ini mempermudah compiler mengakses variabel lokal.

---

## ADRP dan ADD

Bagian menarik berikutnya:

```asm
adrp x0, 400000
add  x0, x0, #0x648
```

digunakan untuk mendapatkan alamat string:

```text
Hello!
```

Mengapa membutuhkan dua instruksi?

Karena ARM tidak memiliki instruksi yang dapat langsung memasukkan nilai 64-bit besar ke register.

Ukuran instruksi ARM hanya:

```text
32-bit
```

sehingga alamat besar harus dibuat secara bertahap.

---

Instruksi:

```asm
ADRP
```

berfungsi mengambil alamat halaman memori 4KB:

```text
4 KiB page
```

dan memasukkannya ke:

```text
X0
```

Kemudian:

```asm
ADD
```

menambahkan offset kecil.

Pada contoh:

```text
0x400000 + 0x648
=
0x400648
```

Alamat tersebut menunjuk ke:

```text
.rodata

Hello!\n
```

---

Teknik ini mirip dengan konsep:

```text
PC-relative addressing
```

yang sebelumnya sudah terlihat pada ARM 32-bit.

Tujuannya adalah menghasilkan:

```text
Position Independent Code
```

sehingga program dapat dimuat di alamat memori berbeda tanpa harus melakukan perubahan besar.

---

## BL — Branch with Link

Pemanggilan:

```asm
bl puts
```

bekerja seperti pada ARM sebelumnya.

Instruksi ini:

1. menyimpan alamat kembali ke LR,
2. berpindah ke fungsi tujuan.

Karena ARM menggunakan:

```text
Link Register
```

maka alamat kembali tidak langsung disimpan ke stack.

---

## Return Value pada ARM64

Bagian berikut:

```asm
mov w0, #0
```

digunakan untuk:

```c
return 0;
```

Pada ARM64 nilai return dikirim melalui:

```text
X0
```

Namun karena fungsi:

```c
main()
```

mengembalikan:

```c
int
```

maka hanya bagian 32-bit yang perlu diisi:

```text
W0
```

bukan seluruh:

```text
X0
```

---

Hal ini sama seperti x86-64.

Walaupun arsitektur sudah 64-bit, tipe:

```c
int
```

tetap 32-bit demi kompatibilitas.

---

Jika fungsi diubah menjadi:

```c
#include <stdint.h>

uint64_t main()
{
    printf("Hello!\n");
    return 0;
}
```

maka compiler akan menghasilkan:

```asm
mov x0, #0
```

karena nilai return sekarang benar-benar membutuhkan register 64-bit.

---

## LDP — Load Pair

Instruksi:

```asm
ldp x29, x30, [sp],#16
```

merupakan kebalikan dari:

```asm
stp
```

Jika STP:

```text
Register → Stack
```

maka LDP:

```text
Stack → Register
```

---

Perbedaan penting:

Pada LDP tidak ada tanda:

```asm
!
```

sehingga menggunakan:

```text
post-index
```

Artinya:

1. data diambil dari stack,
2. SP baru ditambah 16.

Secara konsep:

```asm
load x29
load x30
add sp, sp, #16
```

---

## RET pada ARM64

Instruksi terakhir:

```asm
ret
```

mengembalikan kontrol ke pemanggil.

Fungsinya mirip:

```asm
BX LR
```

pada ARM 32-bit.

Namun ARM64 menyediakan instruksi khusus:

```text
RET
```

yang memberi informasi tambahan kepada CPU bahwa instruksi tersebut adalah:

```text
function return
```

bukan sekadar jump biasa.

Dengan informasi ini processor dapat melakukan optimasi internal.

---


# 3.5 MIPS

Pada bagian ini saya mempelajari bagaimana compiler **GCC** menghasilkan assembly untuk arsitektur **MIPS**.

Berbeda dengan arsitektur seperti **x86** atau **ARM**, MIPS memiliki beberapa konsep unik, salah satunya adalah penggunaan **Global Pointer (GP)**.

Selain itu, pada bagian ini juga dibahas bagaimana compiler melakukan optimasi terhadap penggunaan register, stack frame, function call, serta bagaimana instruksi MIPS bekerja ketika program **Hello, World!** dijalankan.

---

# 3.5.1 Global Pointer

Salah satu konsep penting dalam MIPS adalah **Global Pointer** atau biasa disingkat **GP**.

Pada arsitektur MIPS, setiap instruction memiliki ukuran tetap yaitu:

```text
32 bit
````

Karena ukuran sebuah instruction hanya 32 bit, maka tidak memungkinkan untuk langsung menyimpan alamat memori 32-bit penuh ke dalam satu instruction.

Oleh karena itu, compiler biasanya membutuhkan beberapa instruction untuk membentuk sebuah alamat lengkap.

Contohnya seperti saat GCC memuat alamat string:

```asm
LUI
ADDIU
```

Kombinasi kedua instruction tersebut digunakan untuk membentuk alamat 32-bit.

---

## Konsep Area 64KiB

Walaupun alamat penuh tidak dapat dimasukkan dalam satu instruction, MIPS memiliki cara agar akses terhadap data tertentu dapat dilakukan dengan cepat.

Sebuah instruction MIPS dapat menggunakan:

```text
16-bit signed offset
```

Sehingga sebuah data masih dapat diakses langsung apabila berada pada range:

```text
register - 32768
hingga
register + 32767
```

Karena alasan tersebut, compiler menyediakan sebuah register khusus yang menunjuk ke tengah area memori sebesar:

```text
64 KiB
```

Register tersebut disebut:

```text
Global Pointer ($gp)
```

---

## Fungsi Global Pointer

Register **$gp** digunakan sebagai pointer menuju area data yang sering digunakan.

Area tersebut biasanya berisi:

* Global variable.
* Data kecil yang sering diakses.
* Alamat fungsi eksternal seperti:

```c
printf()
```

atau:

```c
puts()
```

Dengan menggunakan **$gp**, compiler dapat mengambil alamat data atau fungsi hanya dengan satu instruction.

Hal ini lebih cepat dibandingkan harus membuat alamat lengkap menggunakan dua instruction.

---

## Section .sdata dan .sbss

Dalam file ELF, area yang digunakan oleh **global pointer** biasanya terbagi menjadi dua section:

### .sdata

Digunakan untuk menyimpan:

* Data global yang sudah memiliki nilai awal.

Contoh:

```c
int value = 10;
```

---

### .sbss

Digunakan untuk menyimpan:

* Data global yang belum memiliki nilai awal.

Contoh:

```c
int value;
```

---

Dengan konsep ini, programmer dapat memilih data mana yang harus ditempatkan pada area tersebut agar dapat diakses lebih cepat.

Konsep seperti ini sebenarnya bukan hanya digunakan oleh MIPS. Arsitektur lain seperti **PowerPC** juga menggunakan teknik yang serupa.

---

# 3.5.2 Optimizing GCC

Pada contoh berikut, program dikompilasi menggunakan:

```text
GCC 4.4.5
```

dengan optimasi aktif.

Compiler menghasilkan assembly seperti berikut:

```asm
main:

lui   $28,%hi(__gnu_local_gp)
addiu $sp,$sp,-32
addiu $28,$28,%lo(__gnu_local_gp)

sw    $31,28($sp)

lw    $25,%call16(puts)($28)

lui   $4,%hi($LC0)

jalr  $25
addiu $4,$4,%lo($LC0)
```

---

## Mengatur Global Pointer

Pada awal fungsi **main()**, compiler terlebih dahulu mengatur register:

```asm
$gp
```

menggunakan instruction:

```asm
LUI
ADDIU
```

Tujuannya adalah membuat register **$gp** menunjuk ke area data global.

Dengan demikian, compiler dapat mengambil alamat fungsi seperti:

```c
puts()
```

dengan lebih cepat.

---

## Menyimpan Return Address

Instruction berikut:

```asm
sw $31,28($sp)
```

digunakan untuk menyimpan register:

```text
$31
```

ke stack.

Pada MIPS, register **$31** dikenal sebagai:

```text
RA (Return Address)
```

Register ini menyimpan alamat untuk kembali setelah sebuah fungsi selesai dijalankan.

Karena fungsi **main()** memanggil fungsi lain yaitu:

```c
puts()
```

maka nilai RA harus diamankan terlebih dahulu.

---

## Mengambil Alamat puts()

Alamat fungsi:

```c
puts()
```

diambil menggunakan:

```asm
lw $25,%call16(puts)($28)
```

Instruction:

```text
LW (Load Word)
```

digunakan untuk mengambil data berukuran satu word dari memori.

Alamat fungsi tersebut diambil berdasarkan nilai:

```text
$gp
```

yang sebelumnya telah disiapkan.

---

## Memuat Alamat String

String:

```text
Hello, world!
```

disimpan sebagai data konstan.

Untuk mendapatkan alamat string tersebut, MIPS menggunakan pasangan instruction:

```asm
LUI
ADDIU
```

Contoh:

```asm
lui  $4,%hi($LC0)
addiu $4,$4,%lo($LC0)
```

Instruction:

```asm
LUI
```

digunakan untuk mengisi 16 bit bagian atas dari alamat.

Sedangkan:

```asm
ADDIU
```

menambahkan 16 bit bagian bawah.

Hasil akhirnya adalah alamat lengkap dari string.

Register:

```asm
$4
```

juga memiliki nama lain:

```text
$a0
```

Register ini digunakan untuk menyimpan argument pertama ketika memanggil sebuah fungsi.

---

## JALR dan Branch Delay Slot

Pemanggilan fungsi dilakukan menggunakan:

```asm
jalr $25
```

JALR memiliki fungsi:

1. Melompat ke alamat yang tersimpan pada register.
2. Menyimpan alamat kembali ke register RA.

Namun MIPS memiliki konsep unik yaitu:

```text
branch delay slot
```

Instruction setelah branch atau jump tetap akan dieksekusi terlebih dahulu.

Contohnya:

```asm
jalr $25
addiu $4,$4,%lo($LC0)
```

Walaupun terlihat bahwa `ADDiu` berada setelah `JALR`, sebenarnya instruction tersebut dijalankan sebelum program berpindah ke fungsi `puts()`.

---

## Nilai yang Disimpan pada RA

Pada beberapa arsitektur lain, return address biasanya berisi alamat instruction berikutnya.

Namun pada MIPS berbeda.

Karena adanya:

```text
branch delay slot
```

nilai yang disimpan ke RA adalah:

```text
PC + 8
```

Bukan:

```text
PC + 4
```

Hal ini karena satu instruction setelah jump masih akan dieksekusi terlebih dahulu.

---

## Mengembalikan Nilai 0

Pada akhir fungsi terdapat:

```asm
move $2,$0
```

Instruction tersebut digunakan untuk:

```c
return 0;
```

Pada MIPS:

```asm
$0
```

adalah register khusus yang selalu bernilai:

```text
0
```

Kemudian nilainya disalin ke:

```asm
$v0
```

yang merupakan register untuk menyimpan nilai return.

---

## Register Zero pada MIPS

MIPS menyediakan register khusus:

```asm
$zero
```

yang selalu bernilai nol.

Hal ini dilakukan karena nilai:

```text
0
```

merupakan salah satu nilai yang paling sering digunakan dalam pemrograman.

Dengan menyediakan register khusus ini, banyak operasi dapat dilakukan tanpa perlu membuat instruction tambahan.

---

## Instruction MOVE pada MIPS

Menariknya, MIPS sebenarnya tidak memiliki instruction asli:

```asm
MOVE
```

Instruction:

```asm
move $dst,$src
```

hanyalah pseudoinstruction dari:

```asm
add $dst,$src,$zero
```

Artinya:

```text
DST = SRC + 0
```

Hasilnya sama seperti melakukan copy data.

Compiler dan assembler menggunakan pseudoinstruction agar kode lebih mudah dibaca.

---

## RET pada MIPS

Untuk kembali dari fungsi digunakan:

```asm
j $31
```

Register:

```asm
$31
```

berisi return address.

Instruction tersebut memiliki fungsi yang sama seperti:

```asm
RET
```

pada arsitektur lain.

Namun karena adanya branch delay slot, instruction setelah jump:

```asm
addiu $sp,$sp,32
```

tetap dijalankan terlebih dahulu.

Instruction tersebut digunakan untuk mengembalikan stack ke kondisi awal.

---

# Kesimpulan Sementara

Pada bagian awal MIPS ini saya memahami bahwa arsitektur MIPS memiliki konsep yang berbeda dibandingkan x86.

Konsep penting yang saya pelajari adalah penggunaan **Global Pointer ($gp)** untuk mempercepat akses terhadap data yang sering digunakan.

Saya juga memahami bahwa alamat 32-bit tidak dapat langsung dimasukkan ke satu instruction MIPS, sehingga compiler menggunakan kombinasi:

* **LUI**
* **ADDIU**

untuk membentuk alamat lengkap.

Selain itu, MIPS memiliki karakteristik unik seperti:

* Register **$zero** yang selalu bernilai 0.
* Penggunaan pseudoinstruction seperti **MOVE**.
* Konsep **branch delay slot**.
* Return address yang menggunakan nilai **PC + 8**.

Konsep-konsep tersebut menunjukkan bahwa desain MIPS sangat berbeda dibandingkan arsitektur seperti x86, tetapi tetap memiliki tujuan yang sama yaitu menghasilkan eksekusi program yang efisien.

```


