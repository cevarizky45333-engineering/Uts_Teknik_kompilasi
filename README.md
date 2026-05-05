# Uts_Teknik_kompilasi

Nama : CEVA RIZKY FADILLAH
Nim  : 231011402554
Kelas: 06TPLE006

# Tugas Mandiri: Implementasi Fase-Fase Kompilator
**Mata Kuliah:** Teknik Kompilasi  
**Topik:** Lexer, Parser (EBNF), AST, dan TAC  

## Deskripsi Tugas
Program ini adalah sebuah **Mini Compiler** yang telah dilengkapi untuk mendukung operator pangkat (`^`). Implementasi ini mencakup beberapa fase penting dalam kompilasi, yaitu:
1. **Lexical Analysis (Lexer):** Memecah kode sumber menjadi token.
2. **Syntax Analysis (Parser):** Membangun Abstract Syntax Tree (AST) berdasarkan aturan EBNF.
3. **Intermediate Code Generation:** Menghasilkan Three Address Code (TAC) dari AST yang telah dibuat.

## Tantangan Utama: Implementasi Operator Pangkat (`^`)
Sesuai instruksi, dukungan operator pangkat telah ditambahkan dengan prioritas (*precedence*) yang lebih tinggi daripada perkalian (`*`) dan pembagian (`/`).

### Perubahan Kode:
- **Tugas 1 (Lexer):** Memperbarui regex `[+*/()\-^]` agar mengenali simbol `^`.
- **Tugas 2 (Parser):** Mengimplementasikan fungsi `power()` untuk menangani operasi eksponensial.
- **Tugas 3 (Hierarki):** Menghubungkan fungsi `term()` ke `power()` untuk menjaga urutan hierarki operator yang benar.

## Jawaban Pertanyaan Refleksi

### 1. Mengapa fungsi `power()` harus dipanggil di dalam `term()`, bukan sebaliknya?
Dalam teknik kompilasi, hierarki pemanggilan fungsi menentukan **Operator Precedence**. Fungsi yang menangani operator dengan prioritas lebih tinggi (pangkat) harus berada lebih dalam di struktur pemanggilan dibandingkan operator dengan prioritas lebih rendah (perkalian/pembagian). Dengan memanggil `power()` di dalam `term()`, kita memastikan operasi pangkat diproses terlebih dahulu sebelum dikalikan atau dibagi.

### 2. Apa yang terjadi pada fase Analisis Semantik jika variabel `z` digunakan dalam kode sumber tetapi tidak ada di `symbol_table`?
Compiler akan melemparkan sebuah **ParserError (Semantic Error)**. Hal ini terjadi karena pada fase analisis semantik, compiler melakukan pengecekan keberadaan variabel dalam lingkungan (*env/symbol table*). Jika variabel `z` tidak terdefinisi, program tidak dapat melanjutkan proses kompilasi karena tidak memiliki nilai atau referensi untuk variabel tersebut.

### 3. Jelaskan mengapa dalam TAC, instruksi untuk `a ^ 2` harus muncul sebelum instruksi untuk `+`.
Three Address Code (TAC) mencerminkan urutan eksekusi logika. Karena pangkat memiliki prioritas lebih tinggi dari penjumlahan, nilai dari `a ^ 2` harus dihitung terlebih dahulu untuk menghasilkan nilai sementara (misal: `t1`). Nilai sementara ini kemudian digunakan sebagai salah satu operan untuk instruksi penjumlahan berikutnya. Tanpa menyelesaikan `a ^ 2` lebih dulu, operasi penjumlahan tidak memiliki nilai input yang valid.

## Uji Coba Program
**Input:** `a ^ 2 + b * c`  
**Symbol Table:** `{'a': 5, 'b': 10, 'c': 2}`

**Output TAC:**
```text
t1 = a ^ 2.0
t2 = b * c
t3 = t1 + t2
