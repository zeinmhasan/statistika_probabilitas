<h1>penjelasan dan laporan modu_1</h1>
<hr />
<h2>soal 1</h2>
<p>
  pada soal satu kita diminta membuat sebuah program untuk mengolah suatu
  database dari csv dengan bantuan awk
</p>
<p>
  1.a) pada soal ini kita diminta untuk mencari berapa banyak buku yang dibaca
  seseorang, dalam soal diminta Chris Hemsworth, dari sini kita bisa
  memanfaatkan `awk` untuk mencari jumlah tersebut:
</p>

```bash
read nama
awk -F',' -v name="$nama" '$2 == name {count++} END {print name " membaca " count " buku."}' reading_data.csv
```

