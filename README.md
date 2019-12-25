**Tentang KTLINT**

Mengapa menggunakan [Ktlint](https://ktlint.github.io/) ?\
“An anti-bikeshedding Kotlin linter with built-in formatter.”

[Ktlint](https://ktlint.github.io/) adalah alat analisis kode statik bebas konfigurasi untuk Kotlin. Itu dapat menganalisis kode Anda dan memberi tahu Anda di mana dalam kode Anda, Anda melanggar pedoman. Itu juga dapat memperbaiki sebagian besar pelanggaran secara otomatis untuk Anda, karenanya formatter bawaan.

Orang mungkin mengatakan bahwa mereka tidak memerlukan linter ini karena mereka memiliki pedoman pengkodean dan mereka memastikan kode tetap sejalan melalui ulasan kode. Pendekatan ini rentan terhadap kesalahan manusia dan sulit dipertahankan. Menambahkan Ktlint ke proyek Anda memastikan semua orang berbicara bahasa yang sama saat menulis kode mereka. Ini juga akan menjadi alat yang hebat untuk menaiki anggota baru ke tim Anda.

Menggunakan linter memungkinkan tim Anda mengesampingkan preferensi pribadi dan mengikuti pedoman yang diterima oleh mayoritas.

**Integrasi**

Tambahkan kedalam build.gradle (Module : app) untuk tugas gradle kita :

```
repositories {
    jcenter()
}

configurations {
    ktlint
}

dependencies {
    ktlint "com.pinterest:ktlint:0.32.0"
    // additional 3rd party ruleset(s) can be specified here
    // just add them to the classpath (e.g. ktlint 'groupId:artifactId:version') and
    // ktlint will pick them up
}

task ktlint(type: JavaExec, group: "verification") {
    description = "Check Kotlin code style."
    main = "com.pinterest.ktlint.Main"
    classpath = configurations.ktlint
    args "src/**/*.kt"
    // to generate report in checkstyle format prepend following args:
    // "--reporter=plain", "--reporter=checkstyle,output=${buildDir}/ktlint.xml"
    // see https://github.com/pinterest/ktlint#usage for more
}

check.dependsOn ktlint

task ktlintFormat(type: JavaExec, group: "formatting") {
    description = "Fix Kotlin code style deviations."
    main = "com.pinterest.ktlint.Main"
    classpath = configurations.ktlint
    args "-F", "src/**/*.kt"
}

```

**Penggunaan**

1.	Mengecek kode style dari kode kotlin

![alt text](https://github.com/udindn/image/blob/master/ktlintcheck.png "Ktlint")

analisis check :

```
10.20.08: Executing task 'ktlint'...

Executing tasks: [ktlint] in project E:\github\KtlintCodeStyleExample

> Task :app:ktlint
E:\github\KtlintCodeStyleExample\app\src\androidTest\java\com\example\ktlintcodestyleexample\ExampleInstrumentedTest.kt:5:1: Wildcard import (cannot be auto-corrected)
E:\github\KtlintCodeStyleExample\app\src\main\java\com\example\ktlintcodestyleexample\MainActivity.kt:11:1: Needless blank line(s)
"checkstyle" report written to E:\github\KtlintCodeStyleExample\app\build\ktlint.xml

> Task :app:ktlint FAILED
```

2.	Melakukan format style pada kode kotlin menggunakan ktlint formatter

![alt text](https://github.com/udindn/image/blob/master/ktlintformat.png "tlint Format")

setelah diformat :

```
10.22.49: Executing task 'ktlintFormat'...

Executing tasks: [ktlintFormat] in project E:\github\KtlintCodeStyleExample

> Task :app:ktlintFormat

BUILD SUCCESSFUL in 2s
1 actionable task: 1 executed
10.22.52: Task execution finished 'ktlintFormat'.
```

**Penyesuain Aturan**

Untuk mengaktifkan aturan tertentu, Anda harus mengaktifkan mode verbose ( ktlint --verbose ...). Di akhir setiap baris Anda akan melihat kode kesalahan. Gunakan itu sebagai argumen untuk ktlint-disable arahan (dibahas di bawah). Contoh apabila akan menon-aktifkan aturan pelarangan import wildcard :

```
import package.* // ktlint-disable no-wildcard-imports

/* ktlint-disable no-wildcard-imports */
import package.a.*
import package.b.*
/* ktlint-enable no-wildcard-imports */
```
Menon-aktifkan semua pengecekan :

```
import package.* // ktlint-disable
```
Selain itu KTLINT juga menyediakan EditorConfig agar kita dapat melakukan penyesuain aturan. Lihat [EditorConfig](https://github.com/pinterest/ktlint) pada Github

**Laporan**

baris dibawah ini berfungsi membuat file laporan yang akan tersimpan di folder build
```
 "--reporter=plain", "--reporter=checkstyle,output=${buildDir}/ktlint.xml"
```

**Referensi :**
1. https://ktlint.github.io/ ktlint
2. https://github.com/pinterest/ktlint ktlint Github
3. https://medium.com/trendyol-tech/ktlint-for-android-89d73e7669a9 Ktlint for Android
4. https://dev.to/onmyway133/how-to-setup-android-projects-11pn How to setup Android projects

