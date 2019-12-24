# ktlint-code-style-example

## Tentang KTLINT
Mengapa menggunakan [Ktlint](https://ktlint.github.io/) ?\
“An anti-bikeshedding Kotlin linter with built-in formatter.”

[Ktlint](https://ktlint.github.io/) adalah alat analisis kode statik bebas konfigurasi untuk Kotlin. Itu dapat menganalisis kode Anda dan memberi tahu Anda di mana dalam kode Anda, Anda melanggar pedoman. Itu juga dapat memperbaiki sebagian besar pelanggaran secara otomatis untuk Anda, karenanya formatter bawaan.

Orang mungkin mengatakan bahwa mereka tidak memerlukan linter ini karena mereka memiliki pedoman pengkodean dan mereka memastikan kode tetap sejalan melalui ulasan kode. Pendekatan ini rentan terhadap kesalahan manusia dan sulit dipertahankan. Menambahkan Ktlint ke proyek Anda memastikan semua orang berbicara bahasa yang sama saat menulis kode mereka. Ini juga akan menjadi alat yang hebat untuk menaiki anggota baru ke tim Anda.

Menggunakan linter memungkinkan tim Anda mengesampingkan preferensi pribadi dan mengikuti pedoman yang diterima oleh mayoritas.

## Penggunaan
Tambahkan kedalam file.gradle untuk tugas gradle kita :

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
    args "src/**/*.kt", "--reporter=plain", "--reporter=checkstyle,output=${buildDir}/ktlint.xml"
    // to generate report in checkstyle format prepend following args:
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

## Pemanggilan fungsi

1.	Mengecek kode style dari kode kotlin

![alt text](https://github.com/udindn/image/blob/master/ktlintcheck.png "Ktlint")

2.	Melakukan format style pada kode kotlin menggunakan ktlint formatter

![alt text](https://github.com/udindn/image/blob/master/ktlintformat.png "tlint Format")


