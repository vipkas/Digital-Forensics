---
description: Page HoraHore
---

# Spontan Uhuiii

## Benchmark AES dan Simpan Hasil ke File TXT

***

### Persiapan Awal

1.  **Python terinstall**\
    Cek versi Python di terminal:

    ```bash
    python --version
    ```
2.  **Install library Python yang diperlukan**

    ```bash
    pip install pycryptodome psutil
    ```

***

### Step 1: Simpan Script Benchmark AES

Buat file `aes_benchmark.py` conto isian gawe kode iki:

```python
import os
import time
import psutil
import statistics
from Crypto.Cipher import AES
from Crypto.Random import get_random_bytes
import sys

def pad(data, block_size):
    pad_len = block_size - (len(data) % block_size)
    return data + bytes([pad_len] * pad_len)

def unpad(data):
    return data[:-data[-1]]

def encrypt_decrypt_aes(data, keysize):
    mem_before = psutil.Process().memory_info().rss / (1024 * 1024)
    start = time.perf_counter()

    key = get_random_bytes(keysize // 8)
    cipher = AES.new(key, AES.MODE_CBC)
    ct = cipher.encrypt(pad(data, AES.block_size))
    cipher2 = AES.new(key, AES.MODE_CBC, cipher.iv)
    result = unpad(cipher2.decrypt(ct))

    end = time.perf_counter()
    mem_after = psutil.Process().memory_info().rss / (1024 * 1024)

    return {
        'duration': end - start,
        'cpu': psutil.cpu_percent(interval=0.1),
        'ram': mem_after - mem_before
    }

def run_aes_benchmark(sample_file, output_file):
    with open(sample_file, 'rb') as f:
        data = f.read()

    input_size_mb = len(data) / (1024 * 1024)
    AES_KEYSIZES = [128, 192, 256]
    REPEAT = 3

    results = []
    base_name = os.path.basename(sample_file)

    results.append(f"Benchmark AES untuk file: {base_name} ({input_size_mb:.2f} MB)\n")

    for keysize in AES_KEYSIZES:
        durations, cpus, rams = [], [], []

        for _ in range(REPEAT):
            metrics = encrypt_decrypt_aes(data, keysize)
            durations.append(metrics['duration'])
            cpus.append(metrics['cpu'])
            rams.append(metrics['ram'])

        avg_time = statistics.mean(durations)
        throughput = input_size_mb / avg_time
        avg_cpu = statistics.mean(cpus)
        avg_ram = statistics.mean(rams)

        results.append(f"AES-{keysize} Bit:")
        results.append(f"  Rata-rata Waktu : {avg_time:.4f} detik")
        results.append(f"  Throughput      : {throughput:.2f} MB/s")
        results.append(f"  CPU Rata-rata   : {avg_cpu:.2f} %")
        results.append(f"  RAM Digunakan   : {avg_ram:.2f} MB\n")

    with open(output_file, 'w') as out:
        out.write("\n".join(results))

    print(f"Hasil benchmark disimpan di {output_file}")

if __name__ == '__main__':
    if len(sys.argv) != 3:
        print("Usage: python aes_benchmark.py <file_input> <file_output.txt>")
        sys.exit(1)

    run_aes_benchmark(sys.argv[1], sys.argv[2])
```

***

### Step 2: Siapno File Input

File input yang digunakan di conto script tersebut bisa berupa **file binary apapun** yang dibuat uji enkripsi dan dekripsinya dengan AES.

Karena script membaca file dengan mode `'rb'` (read binary):

```python
with open(sample_file, 'rb') as f:
    data = f.read()
```

Maka file input ini bisa berupa:

* File arsip seperti `.tar.gz`, `.zip`, `.rar`
* File dokumen, misal `.pdf`, `.docx`
* File gambar, video, audio, dll
* File teks biasa pun bisa, tapi dibaca dalam mode binary

Cek file input (misal `sample.tar.gz`) sudah ada dan diketahui path-nya.

***

### Step 3: Jalankan Benchmark dari Terminal

Masuk ke folder tempat script `aes_benchmark.py`:

```bash
cd /path/to/script/
```

Lalu jalankan:

```bash
python aes_benchmark.py /path/to/sample.tar.gz hasil_benchmark.txt
```

***

### Step 4: Cek Hasil Benchmark

Buka file output:

```bash
cat hasil_benchmark.txt
```

***

### Contoh Output

```
Benchmark AES untuk file: sample.tar.gz (12.34 MB)

AES-128 Bit:
  Rata-rata Waktu : 0.1234 detik
  Throughput      : 100.00 MB/s
  CPU Rata-rata   : 15.00 %
  RAM Digunakan   : 20.00 MB

AES-192 Bit:
  Rata-rata Waktu : 0.1356 detik
  Throughput      : 90.99 MB/s
  CPU Rata-rata   : 14.00 %
  RAM Digunakan   : 21.00 MB

AES-256 Bit:
  Rata-rata Waktu : 0.1500 detik
  Throughput      : 82.26 MB/s
  CPU Rata-rata   : 16.00 %
  RAM Digunakan   : 22.00 MB
```

***

### Ringkasan

| Langkah            | Perintah / Aksi                                      |
| ------------------ | ---------------------------------------------------- |
| Install libraries  | `pip install pycryptodome psutil`                    |
| Simpan script      | Buat file `aes_benchmark.py` dan isi dengan kode     |
| Jalankan benchmark | `python aes_benchmark.py <file_input> <file_output>` |
| Lihat hasil        | `cat <file_output>`                                  |

***

