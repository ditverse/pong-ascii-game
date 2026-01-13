# PROJECT REQUIREMENTS DOCUMENT (PRD)

## Project Name: Terminal Pong (CLI-Based Multiplayer Game)

**Version:** 1.1 (Final Edition)
**Source Document:** Rancangan Tugas Besar - Network Programming.pdf

---

### 1. Project Overview (Gambaran Proyek)

Membangun permainan **Pong Multiplayer Real-Time** yang berjalan sepenuhnya di antarmuka Terminal/Console (CLI). Sistem menggunakan arsitektur **Host-Client** berbasis protokol **TCP Socket** murni dalam satu skrip terintegrasi.

* **Tujuan:** Membuktikan sinkronisasi state permainan (bola, paddle, skor) secara real-time antar dua perangkat via LAN.
* **Target Platform:** Cross-platform (Windows/Linux/Termux) menggunakan Python Standard Library.

---

### 2. Tech Stack & Constraints (Batasan Teknis)

Instruksi ini wajib diikuti oleh AI untuk memastikan kode memenuhi syarat tugas kuliah:

* **Language:** Python 3.x.
* **Libraries:** Hanya **Standard Library** (hanya `socket`, `threading`, `time`, `os`, `sys`, `random`). **DILARANG** menggunakan library eksternal berat seperti `pygame` atau `curses` (kecuali `msvcrt` untuk Windows/`tty` untuk Linux input handling jika terpaksa, namun utamakan cross-platform logic sederhana).
* **Protocol:** TCP (Transmission Control Protocol) untuk semua komunikasi.
* **Interface:** ASCII / Text-Based UI (TUI). Tidak ada GUI window.
* **Architecture:** Single Script (`main.py`) yang berisi logika Server dan Client sekaligus.

---

### 3. System Architecture (Arsitektur Sistem)

Sistem menggunakan pendekatan **Single Script Integration**. Peran perangkat ditentukan saat *runtime* melalui menu:

#### A. Device 1: Host Mode (Server + Player 1)
* **Role:** Bertindak sebagai **Game Authority** DAN **Client 1** (Player Kiri).
* **Responsibilities:**
  1. Menjalankan Thread Server di background (Bind `0.0.0.0`).
  2. Auto-detect LAN IP Address dan menampilkannya.
  3. Menjalankan Thread Client lokal yang connect ke `localhost`.
  4. Menjalankan **Game Loop Physics**.
  5. Broadcast state ke Client 1 (Local) dan Client 2 (Remote).

#### B. Device 2: Join Mode (Player 2)
* **Role:** Bertindak murni sebagai **Client** (Player Kanan).
* **Responsibilities:**
  1. Input IP Address Host.
  2. Connect ke Server via TCP.
  3. Mengirim input user (Keystroke `W/S`) ke Server.
  4. Menerima broadcast state dari Server dan merender TUI.

---

### 4. Functional Requirements (Spesifikasi Fungsional)

#### 4.1. Network & Connection
* **FR-NET-01:** Server harus bisa menangani **2 koneksi simultan** (1 dari localhost, 1 dari remote IP).
* **FR-NET-02:** Client Guest harus memiliki timeout/error handling jika koneksi gagal.
* **FR-NET-03:** Protokol pertukaran data menggunakan format string ringan (CSV-like).

#### 4.2. Game Physics (Server Side)
* **FR-PHYS-01:** Bola bergerak otomatis (X/Y velocity).
* **FR-PHYS-02:** Bola memantul di dinding Atas/Bawah.
* **FR-PHYS-03:** Bola memantul di Paddle.
* **FR-PHYS-04:** Skor bertambah jika bola melewati Paddle.
* **FR-PHYS-05:** Game Over jika salah satu pemain mencapai Skor 5.

#### 4.3. Input Handling
* **FR-INP-01:** Input kontrol hanya tombol `W` (Naik) dan `S` (Turun).
* **FR-INP-02:** Input harus **Non-Blocking** (gunakan Threading).

#### 4.4. Rendering (ASCII TUI)
* **FR-REN-01:** Area permainan berukuran tetap (misal: 60x20 karakter).
* **FR-REN-02:** Refresh rate minimal 10-20 FPS.
* **FR-REN-03:** Karakter: Paddle `|`, Bola `O`, Net `:`, Kosong ` `.