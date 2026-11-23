# Chess Overlay PoC - Modul 1: uji AccessibilityService (Chess.com Android)

Tujuan: mengetahui APAKAH papan Chess.com Android bisa dibaca dari accessibility tree (petak, bidak, daftar
langkah, koordinat layar petak). Tidak ada screenshot, tidak ada Stockfish, tidak ada overlay panah di modul ini.

## Cara kerja
- Service hanya aktif untuk paket `com.chess`. Event konten hanya dihitung; pohon node dibaca SEKALI saat tombol
  DUMP (mengambang di kanan layar) diketuk.
- `Analyzer` (logika murni, ada tes JVM) menilai isi dump: verdict `POSITION_READABLE`, `POSITION_PARTIAL`,
  `MOVES_ONLY`, `NOT_READABLE` atau `NO_DATA`, plus kandidat papan, kosakata label asli, dan grid layar.
- Hasil: JSON di penyimpanan privat aplikasi (3 terakhir). Tombol "Bagikan" mengirim JSON terakhir.

## Build
- Android Studio: Open folder ini, Sync, Run. (JDK 17)
- Tanpa PC: unggah folder ini ke repo GitHub, jalankan workflow "Build debug APK" (Actions), unduh artifact `app-debug.apk`.

## Pasang dan uji
1. Pasang APK. Android 13+ untuk APK di luar Play Store: Info aplikasi > menu titik tiga > "Izinkan setelan terbatas",
   baru service bisa diaktifkan.
2. Buka app > "Buka pengaturan Aksesibilitas" > aktifkan "Chess Overlay PoC".
3. Buka Chess.com pada replay/analisis permainan yang SUDAH SELESAI atau mode latihan. Ketuk DUMP pada:
   (a) posisi awal, (b) setelah beberapa langkah maju, (c) papan dibalik, (d) layar daftar langkah jika ada.
4. Buka app lagi, baca hasil, ketuk "Bagikan", kirim JSON-nya ke Claude.
Dump berisi semua teks di layar (misalnya nama pemain). Periksa dulu sebelum dibagikan.

## Dari ekstensi v3_1 ke sini
`parsePieceCode` / `parseSquareClass` -> `Analyzer.parsePiece` / `parseSquare`; `detectFlipped` -> `Analyzer.fitGrid`.
`matrixToFEN` / `validateFEN` (dan nanti `classify.js` dari v5) dipindahkan di modul berikutnya.

## Status verifikasi
Belum dikompilasi dan belum dijalankan di perangkat/emulator (lingkungan penulis tidak punya Android SDK).
Logika `Analyzer` sudah diperiksa lewat tiruan Python dengan skenario yang sama dengan `AnalyzerTest.kt`;
fixture tes sintetis, bukan format asli Chess.com.
