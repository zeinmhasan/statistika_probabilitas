<h1>penjelasan dan laporan modul_1</h1>
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
<p>dan nanti outputnya akan seperti ini</p>
<img src="sorce/Screenshot 2025-03-15 120223.png">
<p>
  1.b) pada soal ini kita diminta untuk mencari berapa rata rata lama waktu untuk membaca pada berbagai 
  jenis device. dari sini kita dari sini kita bisa memanfaatkan `awk` untuk mencari jumlah tersebut:
</p>

```bash
read device
awk -F',' -v dev="$device" '$8 == dev {sum += $6; count++} END {if (count > 0) print "Rata-rata durasi membaca dengan", dev, "adalah", sum/count, "menit."; else print "Tidak ada data untuk perangkat", dev}' reading_data.csv
```

<p>nanti outputnya akan seperti ini</p>
<img src="sorce/Screenshot 2025-03-15 121621.png">

<p>
  1.c) pada soal ini kita diminta untuk mencari pembaca dengan ratting tertinggi disertai dengan nama, judul buku dan nilai ratting nya.
  dari sini kita dari sini kita bisa memanfaatkan `awk` untuk mencari informasi tersebut:
</p>

```bash
 awk -F',' 'NR>1 {if ($7 > max) {max=$7; name=$2; book=$3}} END {print "\nPembaca dengan rating tertinggi:", name, "-", book, "-", max}' reading_data.csv
```
<p>nanti outputnya akan seperti ini</p>
<img src="sorce/Screenshot 2025-03-15 122703.png">

<p>
  1.d) pada soal ini kita diminta untuk mencari genre buku yang paling populer setelah 2023 dan di regoin asia.
  dari sini kita dari sini kita bisa memanfaatkan `awk` untuk mencari informasi tersebut:
</p>

```bash
awk -F',' 'NR>1 && $5 > "2023-12-31" && $9 == "Asia" {
        count[$4,$9]++
    } 
    END {
        max_count = 0
        max_genre = ""
        max_region = ""
        for (key in count) {
            if (count[key] > max_count) {
                max_count = count[key]
                split(key, arr, SUBSEP)
                max_genre = arr[1]
                max_region = arr[2]
            }
        }
        print "Genre paling populer di", max_region, "setelah 2023 adalah", max_genre, "dengan", max_count, "buku."
    }' reading_data.csv
```
<p>nanti outputnya akan seperti ini</p>
<img src="sorce/Screenshot 2025-03-15 123131.png">
