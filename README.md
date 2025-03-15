<h2>soal_2</h2>
<p>pada soal kali ini kita diminta untuk membuat beberapa script yang nanti akan saling berhubungan satu sama lain.
    soal ini membutuhkan pengetahuan tentang pemrograman pembuatan akun cara login hingga fitur fitur yang ada didalamnya.
</p>
<p>
    2.a) pada soal ini kita diminta untuk membuat sebuah script yang berfungsi sebagai wadah membuat user dan sebuah script untuk login user tersebut
    dan tidak lupa data akan disimpan di database basis csv.
</p>

```bash
DATABASE="./data/player.csv"
STATIC_SALT="Salt123"  # static salt untuk hashing password

# Meminta input dari pengguna
echo -n "Masukkan email: "
read EMAIL
echo -n "Masukkan nama: "
read USERNAME
echo -n "Masukkan password: "
read -s PASSWORD  # Input password secara tersembunyi
echo ""
```
<p>nanti hasil output akan seperti ini</p>
![Image](https://github.com/user-attachments/assets/1b79122a-958e-4b5c-9fe7-e43759d5dfd0)

<p>
    2.b) pada soal ini kita diminta untuk membuat validasi pada email agar harus memiliki karakter @ dan . , serta validasi password 
    harus memiliki setidaknya 8 karakter dengan wajib ada minimal 1 huruf kecil, huruf besar, dan nomor.
</p>

```bash
# Validasi email
if ! [[ "$EMAIL" =~ ^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$ ]]; then
    echo "ERROR: Format email tidak valid!"
    exit 1
fi


# Validasi password
if [[ "${#PASSWORD}" -lt 8 ]]; then
    echo "ERROR: Password harus memiliki minimal 8 karakter!" 
    exit 1
fi
if ! [[ "$PASSWORD" =~ [a-z] ]]; then
    echo "ERROR: Password setidaknya ada satu huruf kecil!"
    exit 1
fi
if ! [[ "$PASSWORD" =~ [A-Z] ]]; then
    echo "ERROR: Password setidaknya ada satu huruf kapital!"
    exit 1
fi
if ! [[ "$PASSWORD" =~ [0-9] ]]; then
    echo "ERROR: Password harus mengandung setidaknya satu angka!"
    exit 1
fi
```

<p>
    2.c) pada soal ini kita diminta untuk membuat fitur untuk mengecek ke unikan sebuah email, jadi setiap user tidak akan memiliki email yang sama.
</p>

```bash
if grep -q "^$EMAIL," "$DATABASE"; then
    echo "ERROR: Email sudah terdaftar!"
    exit 1
fi
```
<p>nanti hasil output akan seperti ini</p>
<img src="source/Screenshot 2025-03-15 140227">

<p>
    2.d) pada soal ini kita diminta untuk membuat password yang perlu disimpan dalam bentuk yang tidak mudah diakses.
     dan mengunakan algoritma hashing sha256sum yang memakai static salt. 
</p>

```bash
# Hashing password dengan SHA-256 dan static salt
HASHED_PASSWORD=$(echo -n "$PASSWORD$STATIC_SALT" | sha256sum | awk '{print $1}')
```

<p>
    2.e) pada soal ini kita diminta untuk membuat fitur pemantauan CPU dalam persen dan juga menampilkan jenis CPU yang digunkan.
</p>

```bash
CPU_USAGE=$(top -bn1 | grep "Cpu(s)" | awk '{print $2 + $4}')
echo "Penggunaan CPU: $CPU_USAGE%"
CPU_MODEL=$(lscpu | grep "Model name" | awk -F ':' '{print $2}' | sed 's/^ *//')
echo "Jenis CPU: $CPU_MODEL"
```
<p>nanti hasil output akan seperti ini</p>
<img src="source/Screenshot 2025-03-15 141349">

<p>
    2.f) pada soal ini kita diminta untuk membuat fitur pemantauan RAM dalam persen dan juga menampilkan jumlah RAM yang digunkan sekarang.
</p>

```bash
# Mengambil total dan penggunaan RAM saat ini
        TOTAL_RAM=$(free -m | awk '/Mem:/ {print $2}')
        USED_RAM=$(free -m | awk '/Mem:/ {print $3}')
        RAM_USAGE_PERCENT=$(awk "BEGIN {printf \"%.2f\", ($USED_RAM/$TOTAL_RAM)*100}")
        
        echo "Total RAM: ${TOTAL_RAM} MB"
        echo "Penggunaan RAM: ${USED_RAM} MB"
        echo "Persentase Penggunaan RAM: ${RAM_USAGE_PERCENT}%"
```
<p>nanti hasil output akan seperti ini</p>
<img src="source/Screenshot 2025-03-15 141730">

<p>
    2.g) pada soal ini kita diminta untuk membuat fitur crontab, jadi nanti pemantauan CPU dan RAM nya bisa outomatis tergantung kita mau setiap berapa lama.
</p>

```bash
# Fungsi untuk menambahkan pencatatan penggunaan CPU ke crontab
function add_cpu_usage() {
    (crontab -l 2>/dev/null; echo "* * * * * $(realpath $0) cpu") | crontab - 
    echo "CPU usage logging added to crontab."
}

# Fungsi untuk menghapus pencatatan penggunaan CPU dari crontab
function remove_cpu_usage() {
    crontab -l | grep -v "$(realpath $0) cpu" | crontab -
    echo "CPU usage logging removed from crontab."
}

# Fungsi untuk menambahkan pencatatan penggunaan RAM ke crontab
function add_ram_usage() {
    (crontab -l 2>/dev/null; echo "* * * * * $(realpath $0) ram") | crontab -
    echo "RAM usage logging added to crontab."
}

# Fungsi untuk menghapus pencatatan penggunaan RAM dari crontab
function remove_ram_usage() {
    crontab -l | grep -v "$(realpath $0) ram" | crontab -
    echo "RAM usage logging removed from crontab."
}

# Fungsi untuk melihat cron jobs yang aktif
function view_cron_jobs() {
    echo "Active cron jobs:"
    crontab -l
}
```
<p>intinya pada kode ini kami mmebuat beberapa fitur yang ada pada crontab, yaitu menyalakan/add cronjob untuk cpu dan ram serta menonaktifkan/remove. 
   tidak lupa kita juga membuat fitur yang bisa melihat cronjob apa yang sedang berjalan.
</p>

<p>nanti hasil output akan seperti ini</p>
<img src="source/Screenshot 2025-03-15 142328">

<p>
    2.h) nah tadi kan kita udah buat crontab, sekarang kita akan membuat file untuk menampung data cronjobnya di core.log untuk pemantauan cpu dan fragment.log untuk pemantauan ram
</p>

<p>nanti hasil output akan seperti ini</p>
<img src="source/Screenshot 2025-03-15 142601">
<img src="source/Screenshot 2025-03-15 142610">

<p>
    2.i) pada soal ini kita diminta untuk membuat terminal yang menjadi jendela utama untuk user dan komputer(ui yang membungkus
     semua script tadi).
</p>

```bash
#!/bin/bash

while true; do
    clear
    echo "=============================="
    echo "      Terminal Dashboard      "
    echo "=============================="
    echo "1. Register"
    echo "2. Login"
    echo "3. Exit"
    echo "=============================="
    echo -n "Pilih menu: "
    read MENU

    case $MENU in
        1)
            ./register.sh
            read -n 1 -s -r -p "Tekan tombol apa saja untuk kembali ke menu..."
            ;;
        2)
            ./login.sh
            if [ $? -eq 0 ]; then
                while true; do
                    clear
                    echo "=============================="
                    echo "      User Dashboard      "
                    echo "=============================="
                    echo "1. Tampilkan Penggunaan CPU"
		    echo "2. Tampilkan Penggunaan RAM"
		    echo "3. Crontab manager"
                    echo "4. Logout"
                    echo "=============================="
                    echo -n "Pilih menu: "
                    read USER_MENU

                    case $USER_MENU in
                        1)
                            ./scripts/core_monitor.sh
                            ;;
			2)
			    ./scripts/frag_monitor.sh
                            ;;
			3)
			    ./scripts/manager.sh
			    ;;
			4)
                            break
                            ;;
                        *)
                            echo "Pilihan tidak valid!"
                            sleep 1
                            ;;
                    esac
                done
            fi
            ;;
        3)
            echo "Keluar..."
            exit 0
            ;;
        *)
            echo "Pilihan tidak valid!"
            sleep 1
            ;;
    esac

done
```

<p>nanti hasil output akan seperti ini</p>
<img src="source/Screenshot 2025-03-15 142907">
<img src="source/Screenshot 2025-03-15 142929">
