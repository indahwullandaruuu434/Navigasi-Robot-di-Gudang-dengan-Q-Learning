
#  Navigasi Robot di Gudang dengan Q-Learning

Kode ini adalah tentang sebuah robot yang berada di dalam dunia kotak-kotak (GridWorld) berukuran 10x10. Robot ini punya misi untuk mencapai sebuah tujuan, ibarat mencari harta karun, yang terletak di pojok kanan bawah. Robot ini tidak sendirian, ada rintangan, tempat pengisian energi (charger), dan sebuah objek yang harus diambil sebelum mencapai tujuan.

Robot kita ini punya energi, dan setiap langkah yang diambil akan mengurangi energinya. Kalau kehabisan energi atau menabrak rintangan, ya game over! Tapi tenang, ada charger yang bisa digunakan untuk isi ulang energi.

Nah, di sinilah Q-Learning berperan. Q-Learning ini seperti buku panduan yang mengajari robot bagaimana cara terbaik untuk mencapai tujuan dan mendapatkan hadiah sebesar-besarnya. Hadiah ini ibarat skor, jadi semakin besar skornya, semakin jago robot kita!

## Deskripsi

Lingkungan GridWorld adalah grid 10x10 dengan robot, rintangan, stasiun pengisian daya, dan tujuan. Robot mulai dari sudut kiri atas (0, 0) dan bertujuan untuk mencapai tujuan di sudut kanan bawah (9, 9) sambil mengumpulkan objek. Robot memiliki tingkat energi terbatas, yang berkurang dengan setiap langkah. Robot dapat mengisi ulang energinya di stasiun pengisian daya.

Tujuan dari agen ini adalah untuk mempelajari kebijakan optimal yang memaksimalkan hadiah kumulatif. Agen diberi hadiah karena mencapai tujuan dan mengumpulkan objek, tetapi dihukum karena menabrak rintangan atau kehabisan energi.

## Kelas `GridWorld`

Kelas `GridWorld` mendefinisikan lingkungan, termasuk:

-   **Inisialisasi**: Mengatur ukuran grid, posisi awal, posisi tujuan, tingkat energi awal, posisi rintangan, posisi stasiun pengisian daya, dan posisi tujuan.
-   `reset()`: Mengatur ulang lingkungan ke keadaan awal.
-   `step(action)`: Mengambil langkah dalam lingkungan berdasarkan tindakan yang diberikan (0: atas, 1: bawah, 2: kiri, 3: kanan). Ini memperbarui posisi robot, tingkat energi, dan mengembalikan keadaan baru, hadiah, dan bendera `done` (yang menunjukkan apakah episode telah berakhir).
-   `render()`: Mencetak representasi tekstual dari keadaan grid saat ini.

## Algoritma Q-Learning

Kode ini menggunakan Q-Learning untuk melatih agen. Q-Learning adalah algoritma pembelajaran penguatan tanpa model yang mempelajari kebijakan optimal dengan memperbarui tabel Q. Tabel Q memetakan pasangan status-tindakan ke nilai Q, yang mewakili hadiah yang diharapkan untuk mengambil tindakan tersebut dalam keadaan itu dan mengikuti kebijakan optimal sesudahnya.

### Inisialisasi Tabel Q

`initialize_q_table()` membuat tabel Q. Setiap entri dalam tabel Q sesuai dengan pasangan status-tindakan. Statusnya adalah tupel `(posisi, tingkat energi, objek_dikumpulkan)`. Tingkat energi didiskretisasi menjadi 6 level (0-9, 10-19, ..., 50). Ada empat kemungkinan tindakan (0: atas, 1: bawah, 2: kiri, 3: kanan).

### Kebijakan Epsilon-Greedy

`epsilon_greedy_policy()` memilih tindakan berdasarkan tabel Q dan nilai epsilon. Dengan probabilitas epsilon, ia memilih tindakan acak (eksplorasi); jika tidak, ia memilih tindakan dengan nilai Q tertinggi untuk keadaan saat ini (eksploitasi).

### Pelatihan

Agen dilatih menggunakan algoritma Q-Learning. Selama setiap episode, agen memulai pada keadaan awal dan mengambil tindakan berdasarkan kebijakan epsilon-greedy. Tabel Q diperbarui menggunakan persamaan Bellman:

```
Q(s, a) = (1 - alpha) * Q(s, a) + alpha * (r + gamma * max(Q(s', a')))
```

di mana:

-   `Q(s, a)` adalah nilai Q saat ini untuk keadaan `s` dan tindakan `a`.
-   `alpha` adalah tingkat pembelajaran.
-   `r` adalah hadiah yang diterima setelah mengambil tindakan `a` dalam keadaan `s`.
-   `gamma` adalah faktor diskonto.
-   `max(Q(s', a'))` adalah nilai Q maksimum yang diharapkan untuk keadaan berikutnya `s'` di antara semua kemungkinan tindakan.

## Parameter

-   `epsilon`: Parameter eksplorasi-eksploitasi (nilai yang lebih tinggi mendorong lebih banyak eksplorasi).
-   `alpha`: Tingkat pembelajaran (seberapa banyak nilai Q baru menimpa nilai lama).
-   `gamma`: Faktor diskonto (menentukan pentingnya hadiah masa depan).
-   `num_episodes`: Jumlah episode pelatihan.

## Pengujian

Setelah pelatihan, agen diuji dengan membiarkannya menavigasi grid menggunakan kebijakan yang dipelajari (yaitu, dengan selalu memilih tindakan dengan nilai Q tertinggi dalam tabel Q). Hadiah total yang dikumpulkan selama pengujian dicetak.

## Menjalankan Kode

Untuk menjalankan kode:

1.  Pastikan Anda telah menginstal Python 3 dan pustaka NumPy.
2.  Salin kode ke file Python (misalnya, `gridworld.py`).
3.  Jalankan file dari terminal Anda: `python gridworld.py`

Ini akan melatih agen dan kemudian menguji agen yang terlatih, mencetak representasi grid di setiap langkah dan hadiah total yang dikumpulkan selama pengujian.

## Peningkatan

Berikut adalah beberapa perbaikan potensial yang dapat dipertimbangkan:

-   Visualisasi yang Lebih Baik: Alih-alih mencetak grid tekstual, Anda dapat menggunakan pustaka seperti `matplotlib` atau `pygame` untuk membuat visualisasi grafis yang lebih baik dari lingkungan dan pergerakan agen.
-   Teknik Q-Learning yang Lebih Canggih: Implementasikan varian Q-Learning seperti Deep Q-Learning (DQN) atau Double DQN untuk menangani ruang keadaan yang lebih besar atau lebih kompleks.
-   Penyetelan Hyperparameter: Bereksperimenlah dengan nilai yang berbeda untuk epsilon, alpha, dan gamma untuk menemukan pengaturan optimal untuk kinerja agen.
-   Desain Lingkungan yang Lebih Kompleks: Tambahkan lebih banyak rintangan, stasiun pengisian daya, atau jenis sel khusus lainnya ke grid untuk membuat masalah lebih menantang.
-   Pembusukan Epsilon: Kurangi nilai epsilon secara bertahap selama pelatihan untuk menggeser keseimbangan dari eksplorasi ke eksploitasi seiring berjalannya waktu.

