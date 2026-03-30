# Rencana Implementasi: Animated Cat Eyes

## Gambaran Umum

Implementasi komponen UI kucing animasi berbasis JavaScript murni dengan mata yang mengikuti kursor mouse, animasi idle otomatis, animasi kedipan, dan pergerakan pupil yang halus menggunakan `requestAnimationFrame`.

## Tugas

- [~] 1. Buat struktur file dan antarmuka inti
  - Buat file `animated-cat-eyes.js` sebagai modul utama komponen
  - Buat file `index.html` sebagai halaman demo
  - Definisikan konstanta konfigurasi default (ukuran, warna, idle timeout, lerp factor, blink interval)
  - _Persyaratan: 1.1, 6.1, 6.2, 6.3, 6.4_

- [~] 2. Implementasi rendering ilustrasi kucing (SVG)
  - [~] 2.1 Implementasi fungsi `createCatSVG(config)` yang merender kepala, telinga, hidung, dan mulut kucing
    - Gunakan elemen SVG dengan viewBox responsif
    - Terima parameter `width`, `height`, `bodyColor`
    - _Persyaratan: 1.1, 1.3, 6.1, 6.2_

  - [~] 2.2 Implementasi fungsi `createEyes(svgEl, config)` yang menambahkan dua Eye simetris ke SVG
    - Setiap Eye terdiri dari elemen lingkaran soket dan elemen Pupil di dalamnya
    - Terima parameter `pupilColor`
    - _Persyaratan: 1.2, 6.3_

  - [ ]* 2.3 Tulis unit test untuk `createCatSVG` dan `createEyes`
    - Verifikasi elemen SVG terbuat dengan atribut yang benar
    - Verifikasi dua Eye dirender di posisi simetris
    - _Persyaratan: 1.1, 1.2_

- [~] 3. Implementasi Mouse Tracker
  - [~] 3.1 Implementasi modul `MouseTracker` dengan method `start()`, `stop()`, dan `getAngle(eyeCenterX, eyeCenterY)`
    - Dengarkan event `mousemove` pada `document`
    - Hitung sudut arah dari pusat Eye ke posisi kursor menggunakan `Math.atan2`
    - Simpan posisi kursor terakhir yang valid
    - _Persyaratan: 2.1, 2.3_

  - [~] 3.2 Implementasi fungsi `calculatePupilPosition(angle, maxRadius)` yang mengembalikan `{x, y}`
    - Batasi perpindahan Pupil dalam radius maksimal 40% dari diameter Eye
    - _Persyaratan: 2.2, 2.3_

  - [ ]* 3.3 Tulis property test untuk `calculatePupilPosition`
    - **Property 1: Posisi pupil selalu dalam radius maksimal**
    - **Validates: Persyaratan 2.2, 3.4**

  - [ ]* 3.4 Tulis unit test untuk `MouseTracker`
    - Test perhitungan sudut untuk berbagai posisi kursor
    - Test posisi dipertahankan saat kursor keluar dari halaman
    - _Persyaratan: 2.1, 2.3_

- [~] 4. Implementasi Auto Animator
  - [~] 4.1 Implementasi modul `AutoAnimator` dengan method `start()`, `stop()`, dan `getTargetPosition(eyeCenter, maxRadius, timestamp)`
    - Hasilkan gerakan melingkar atau acak yang halus menggunakan fungsi sinusoidal
    - Pastikan posisi tidak melampaui radius maksimal 40% dari diameter Eye
    - _Persyaratan: 3.1, 3.2, 3.4_

  - [ ]* 4.2 Tulis property test untuk `AutoAnimator`
    - **Property 2: Posisi auto-animator selalu dalam radius maksimal**
    - **Validates: Persyaratan 3.4**

- [~] 5. Implementasi Idle Detection
  - [~] 5.1 Implementasi fungsi `createIdleDetector(idleTimeout, onIdle, onActive)` yang mengelola transisi antara mode mouse dan mode idle
    - Gunakan `setTimeout` untuk mendeteksi idle setelah `idleTimeout` ms
    - Reset timer setiap kali ada event `mousemove`
    - Panggil `onIdle` saat idle, `onActive` saat mouse aktif kembali
    - _Persyaratan: 3.1, 3.3_

  - [ ]* 5.2 Tulis unit test untuk `createIdleDetector`
    - Test transisi dari aktif ke idle setelah timeout
    - Test transisi dari idle kembali ke aktif saat mouse bergerak
    - _Persyaratan: 3.1, 3.3_

- [~] 6. Checkpoint - Pastikan semua test lulus
  - Pastikan semua test lulus, tanyakan kepada user jika ada pertanyaan.

- [~] 7. Implementasi animasi pupil halus (lerp)
  - [~] 7.1 Implementasi fungsi `lerp(current, target, factor)` untuk interpolasi linear
    - _Persyaratan: 5.1_

  - [~] 7.2 Implementasi loop animasi utama menggunakan `requestAnimationFrame`
    - Setiap frame: ambil posisi target (dari MouseTracker atau AutoAnimator), terapkan lerp, update posisi Pupil di DOM
    - Implementasi fallback ke `setTimeout` dengan interval 16ms jika `requestAnimationFrame` tidak tersedia
    - _Persyaratan: 2.4, 5.1, 5.2, 5.3_

  - [ ]* 7.3 Tulis property test untuk fungsi `lerp`
    - **Property 3: Lerp selalu menghasilkan nilai antara current dan target**
    - **Validates: Persyaratan 5.1**

- [~] 8. Implementasi animasi kedipan
  - [~] 8.1 Implementasi fungsi `createBlinker(eyeElements, onBlinkStart, onBlinkEnd)` yang mengelola siklus kedipan
    - Jadwalkan kedipan berikutnya dengan interval acak antara 2000ms hingga 6000ms
    - Animasikan Eye menutup dalam 150ms lalu membuka dalam 150ms menggunakan CSS transition atau manipulasi SVG
    - Pertahankan posisi Pupil selama kedipan berlangsung
    - _Persyaratan: 4.1, 4.2, 4.3_

  - [ ]* 8.2 Tulis property test untuk interval kedipan
    - **Property 4: Interval kedipan selalu antara 2000ms dan 6000ms**
    - **Validates: Persyaratan 4.1**

  - [ ]* 8.3 Tulis unit test untuk `createBlinker`
    - Test durasi animasi buka/tutup mata
    - Test pupil tidak bergerak selama kedipan
    - _Persyaratan: 4.2, 4.3_

- [~] 9. Implementasi komponen utama `AnimatedCatEyes`
  - [ ] 9.1 Implementasi fungsi factory `createAnimatedCatEyes(container, config)` yang menyatukan semua modul
    - Render SVG kucing ke dalam `container`
    - Inisialisasi MouseTracker, AutoAnimator, IdleDetector, Blinker, dan loop animasi
    - Kembalikan objek dengan method `destroy()` untuk membersihkan event listener
    - _Persyaratan: 1.1, 1.2, 1.3, 6.1, 6.2, 6.3, 6.4_

  - [ ] 9.2 Perbarui `index.html` untuk mendemonstrasikan komponen dengan konfigurasi default dan kustom
    - _Persyaratan: 6.1, 6.2, 6.3, 6.4_

  - [ ]* 9.3 Tulis integration test untuk `createAnimatedCatEyes`
    - Test komponen merender dengan konfigurasi default
    - Test komponen merender dengan konfigurasi kustom
    - Test method `destroy()` membersihkan semua listener
    - _Persyaratan: 1.1, 1.2, 1.3, 6.1, 6.2, 6.3, 6.4_

- [ ] 10. Checkpoint akhir - Pastikan semua test lulus
  - Pastikan semua test lulus, tanyakan kepada user jika ada pertanyaan.

## Catatan

- Tugas bertanda `*` bersifat opsional dan dapat dilewati untuk MVP yang lebih cepat
- Setiap tugas merujuk ke persyaratan spesifik untuk keterlacakan
- Property test memvalidasi properti universal (batas radius, interval waktu, nilai lerp)
- Unit test memvalidasi contoh spesifik dan kondisi tepi
