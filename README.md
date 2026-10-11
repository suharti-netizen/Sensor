# Sensor


1. ARcithek Komponen

Aplikasi Jetpack Compose dan Android Sensor Framework:
• MainActivity mengontrol lifecycle SensorManagerHelper.
• SensorManagerHelper membaca Akselerometer (Gerak) & Proximity (Posisi).
• Data dikirim ke UI (SensorDashboardScreen) untuk menggerakkan bola fisika dan memicu animasi (gerkan bola).

2. Sensor Gerak (Sensor.TYPE_ACCELEROMETER)

• Mekanisme: Membaca nilai Sumbu X (kiri/kanan) dan Sumbu Y (atas/bawah) lewat onSensorChanged.
• Fisik Bola: Nilai X & Y diubah menjadi percepatan (ax, ay) untuk memperbarui kecepatan bola (vx, vy).
• Efek: Menggunakan damping (0.92) sebagai gesekan udara dan membalikkan kecepatan (v = -v × 0.5) saat menabrak tepi Canvas agar bola memantul.

3. Sensor Posisi (Sensor.TYPE_PROXIMITY ) :[

• Mekanisme: Mendeteksi jarak objek di dekat earpiece HP (mengubah state isNear = true jika jarak = 0 cm).
• Efek UI Reaktif:
	• Tema Gelap: Transisi warna latar dari terang ke gelap via animateColorAsState.
	• Force Field: lingkaran neon animasi pulsa(sos) di sekitar bola pada Canvas.
	• Indikator: Mengubah teks Card menjadi "DEKAT (NEAR)" secara real-time.

4. Manajemen Lifecycle Sensor

Wajib dikelola agar sensor tidak boros baterai saat aplikasi di layar belakang:
• onResume(): Mendaftarkan listener sensor (SENSOR_DELAY_GAME & SENSOR_DELAY_UI). Sensor aktif.
• onPause(): Melepas listener via unregisterListener(). Sensor mati total untuk hemat baterai.
• Jetpack Compose: Menggunakan DisposableEffect + LocalLifecycleOwner untuk otomatis melakukan register/unregister sesuai siklus hidup aplikasi.
