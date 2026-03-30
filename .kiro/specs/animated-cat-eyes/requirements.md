# Dokumen Persyaratan

## Pendahuluan

Fitur ini menampilkan animasi kucing dengan mata yang dapat bergerak secara dinamis. Mata kucing akan mengikuti posisi kursor mouse pengguna di layar, atau bergerak secara otomatis menggunakan animasi jika tidak ada interaksi mouse. Fitur ini ditujukan sebagai komponen UI interaktif berbasis web yang dapat diintegrasikan ke dalam halaman HTML.

## Glosarium

- **Animated_Cat**: Komponen UI utama yang merender ilustrasi kucing animasi di halaman web.
- **Eye**: Salah satu dari dua bola mata kucing yang dapat bergerak di dalam area soket mata.
- **Pupil**: Bagian dalam Eye yang bergerak mengikuti arah target (kursor atau animasi otomatis).
- **Mouse_Tracker**: Modul yang mendeteksi posisi kursor mouse pengguna di layar.
- **Auto_Animator**: Modul yang menggerakkan Pupil secara otomatis ketika tidak ada interaksi mouse.
- **Idle_Timeout**: Durasi waktu tanpa interaksi mouse sebelum Auto_Animator aktif, ditetapkan 3 detik.

---

## Persyaratan

### Persyaratan 1: Tampilan Kucing Animasi

**User Story:** Sebagai pengguna, saya ingin melihat ilustrasi kucing yang menarik di halaman web, agar saya mendapatkan pengalaman visual yang menyenangkan.

#### Kriteria Penerimaan

1. THE Animated_Cat SHALL merender ilustrasi kucing yang terdiri dari kepala, telinga, mata, hidung, dan mulut menggunakan elemen SVG atau Canvas.
2. THE Animated_Cat SHALL menampilkan dua Eye yang simetris di posisi yang sesuai pada wajah kucing.
3. THE Animated_Cat SHALL dapat ditampilkan pada resolusi layar antara 320px hingga 1920px lebar tanpa kehilangan proporsi.

---

### Persyaratan 2: Mata Mengikuti Kursor Mouse

**User Story:** Sebagai pengguna, saya ingin mata kucing mengikuti kursor mouse saya, agar animasi terasa interaktif dan hidup.

#### Kriteria Penerimaan

1. WHEN posisi kursor mouse berubah, THE Mouse_Tracker SHALL menghitung sudut arah dari pusat Eye ke posisi kursor.
2. WHEN Mouse_Tracker menghasilkan sudut arah baru, THE Pupil SHALL berpindah ke posisi baru dalam radius maksimal 40% dari diameter Eye.
3. WHEN kursor mouse berada di luar batas halaman web, THE Eye SHALL mempertahankan posisi Pupil terakhir yang valid.
4. WHEN posisi kursor mouse berubah, THE Pupil SHALL memperbarui posisinya dalam waktu kurang dari 16ms (setara 60fps).

---

### Persyaratan 3: Animasi Otomatis Saat Idle

**User Story:** Sebagai pengguna, saya ingin mata kucing tetap bergerak meskipun saya tidak menggerakkan mouse, agar animasi tidak terlihat membeku.

#### Kriteria Penerimaan

1. WHEN tidak ada pergerakan mouse selama Idle_Timeout, THE Auto_Animator SHALL mengambil alih pergerakan Pupil.
2. WHILE Auto_Animator aktif, THE Pupil SHALL bergerak mengikuti pola gerakan melingkar atau acak yang halus dengan interval pembaruan 16ms.
3. WHEN kursor mouse bergerak kembali setelah Auto_Animator aktif, THE Auto_Animator SHALL berhenti dan THE Mouse_Tracker SHALL kembali mengontrol posisi Pupil.
4. WHILE Auto_Animator aktif, THE Pupil SHALL tidak berpindah melampaui radius maksimal 40% dari diameter Eye.

---

### Persyaratan 4: Animasi Berkedip

**User Story:** Sebagai pengguna, saya ingin mata kucing sesekali berkedip, agar animasi terlihat lebih natural dan hidup.

#### Kriteria Penerimaan

1. THE Animated_Cat SHALL menampilkan animasi kedipan mata dengan interval acak antara 2 detik hingga 6 detik.
2. WHEN animasi kedipan dimulai, THE Eye SHALL menutup penuh dalam durasi 150ms lalu membuka kembali dalam durasi 150ms.
3. WHILE animasi kedipan berlangsung, THE Pupil SHALL mempertahankan posisi terakhirnya dan tidak bergerak.

---

### Persyaratan 5: Pergerakan Pupil yang Halus

**User Story:** Sebagai pengguna, saya ingin pergerakan mata kucing terasa mulus dan tidak patah-patah, agar pengalaman visual lebih nyaman.

#### Kriteria Penerimaan

1. WHEN posisi target Pupil berubah, THE Pupil SHALL bergerak menuju posisi target menggunakan interpolasi linear (lerp) dengan faktor 0.1 hingga 0.3 per frame.
2. THE Animated_Cat SHALL menggunakan `requestAnimationFrame` untuk memperbarui posisi Pupil setiap frame render.
3. IF browser tidak mendukung `requestAnimationFrame`, THEN THE Animated_Cat SHALL menggunakan `setTimeout` dengan interval 16ms sebagai fallback.

---

### Persyaratan 6: Konfigurasi Komponen

**User Story:** Sebagai developer, saya ingin dapat mengonfigurasi tampilan dan perilaku komponen kucing, agar komponen dapat disesuaikan dengan kebutuhan proyek.

#### Kriteria Penerimaan

1. THE Animated_Cat SHALL menerima parameter ukuran (lebar dan tinggi dalam piksel) dengan nilai default 200x200 piksel.
2. THE Animated_Cat SHALL menerima parameter warna tubuh kucing dengan nilai default `#F4A460`.
3. THE Animated_Cat SHALL menerima parameter warna Pupil dengan nilai default `#1a1a1a`.
4. WHERE parameter Idle_Timeout dikonfigurasi, THE Animated_Cat SHALL menggunakan nilai tersebut sebagai pengganti nilai default 3 detik.
