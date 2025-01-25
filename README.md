<!DOCTYPE html>
<html lang="id">
<head>
  <meta charset="UTF-8">
  <title>Absensi & Keterlambatan - Guru Piket + Pengisi</title>

  <!-- Chart.js CDN (Versi Stabil) -->
  <script src="https://cdn.jsdelivr.net/npm/chart.js@3.9.1/dist/chart.min.js"></script>
  <!-- SheetJS CDN -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>

  <style>
    /* ========== RESET & GLOBAL STYLES ========== */
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }
    body {
      font-family: "Segoe UI", Tahoma, sans-serif;
      background-color: #f0f2f5;
      color: #333;
    }
    a {
      text-decoration: none;
      color: inherit;
    }
    button {
      cursor: pointer;
      border: none;
      padding: 10px 20px;
      border-radius: 5px;
      font-size: 1rem;
      transition: background-color 0.3s ease, transform 0.2s ease;
    }
    button:hover {
      transform: translateY(-2px);
    }
    /* ========== HEADER STYLES ========== */
    header {
      background-color: #4a90e2;
      color: #fff;
      padding: 20px 0;
      text-align: center;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    header h1 {
      font-size: 2rem;
      margin: 0;
    }

    /* ========== NAVBAR STYLES ========== */
    nav {
      background-color: #fff;
      display: flex;
      flex-wrap: wrap;
      justify-content: center;
      padding: 10px 0;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
      position: sticky;
      top: 80px;
      z-index: 100;
    }
    nav .nav-item {
      margin: 5px 10px;
      padding: 10px 15px;
      background-color: #4a90e2;
      color: #fff;
      border-radius: 20px;
      transition: background-color 0.3s ease;
    }
    nav .nav-item:hover {
      background-color: #357ABD;
    }

    /* ========== CONTAINER & SCREEN STYLES ========== */
    .container {
      max-width: 1200px;
      margin: 30px auto;
      padding: 20px;
      background-color: #fff;
      border-radius: 10px;
      box-shadow: 0 4px 8px rgba(0,0,0,0.1);
    }
    .screen {
      display: none;
      animation: fadeIn 0.5s ease;
    }
    .active {
      display: block;
    }
    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(20px); }
      to { opacity: 1; transform: translateY(0); }
    }

    /* ========== FORM & INPUT STYLES ========== */
    label {
      display: block;
      margin: 15px 0 5px;
      font-weight: bold;
    }
    input[type="text"],
    input[type="date"],
    input[type="month"],
    select,
    input[type="file"] {
      width: 100%;
      padding: 10px;
      border: 1px solid #ccc;
      border-radius: 5px;
      font-size: 1rem;
      transition: border-color 0.3s ease;
    }
    input[type="text"]:focus,
    input[type="date"]:focus,
    input[type="month"]:focus,
    select:focus,
    input[type="file"]:focus {
      border-color: #4a90e2;
      outline: none;
    }
    .radio-group {
      display: flex;
      flex-wrap: wrap;
      gap: 15px;
      margin-bottom: 15px;
    }
    .radio-item {
      display: flex;
      align-items: center;
      gap: 5px;
    }

    /* ========== BUTTON STYLES ========== */
    .btn-primary {
      background-color: #4a90e2;
      color: #fff;
    }
    .btn-primary:hover {
      background-color: #357ABD;
    }
    .btn-danger {
      background-color: #e74c3c;
      color: #fff;
    }
    .btn-danger:hover {
      background-color: #c0392b;
    }
    .btn-secondary {
      background-color: #95a5a6;
      color: #fff;
    }
    .btn-secondary:hover {
      background-color: #7f8c8d;
    }

    /* ========== TABLE STYLES ========== */
    table {
      width: 100%;
      border-collapse: collapse;
      margin-top: 20px;
    }
    th, td {
      padding: 12px 15px;
      text-align: left;
      border-bottom: 1px solid #ddd;
    }
    th {
      background-color: #4a90e2;
      color: #fff;
    }
    tr:hover {
      background-color: #f1f1f1;
    }

    /* ========== STATUS BOX STYLES ========== */
    .status-box {
      margin-top: 15px;
      padding: 15px;
      background-color: #dff0d8;
      border: 1px solid #d0e9c6;
      border-radius: 5px;
      color: #3c763d;
      font-weight: bold;
    }

    /* ========== DASHBOARD STYLES ========== */
    .dashboard-section {
      margin-bottom: 40px;
    }
    .dashboard-card {
      padding: 20px;
      background-color: #ecf0f1;
      border-radius: 10px;
      box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    .dashboard-card h3 {
      margin-bottom: 20px;
      color: #34495e;
    }

    /* ========== MODAL STYLES ========== */
    .modal {
      display: none;
      position: fixed;
      z-index: 200;
      left: 0;
      top: 0;
      width: 100%;
      height: 100%;
      overflow: auto;
      background-color: rgba(0,0,0,0.5);
      align-items: center;
      justify-content: center;
    }
    .modal-content {
      background-color: #fff;
      padding: 30px;
      border-radius: 10px;
      width: 90%;
      max-width: 500px;
      position: relative;
      box-shadow: 0 5px 15px rgba(0,0,0,0.3);
      animation: slideIn 0.5s ease;
    }
    @keyframes slideIn {
      from { transform: translateY(-50px); opacity: 0; }
      to { transform: translateY(0); opacity: 1; }
    }
    .close {
      position: absolute;
      top: 15px;
      right: 20px;
      font-size: 1.5rem;
      font-weight: bold;
      color: #aaa;
      cursor: pointer;
      transition: color 0.3s ease;
    }
    .close:hover {
      color: #333;
    }

    /* ========== FOOTER STYLES ========== */
    footer {
      background-color: #fff;
      padding: 20px 0;
      text-align: center;
      box-shadow: 0 -2px 4px rgba(0,0,0,0.1);
      margin-top: 40px;
    }
    .footer-line {
      width: 50px;
      height: 4px;
      background-color: #4a90e2;
      margin: 0 auto 10px auto;
      border-radius: 2px;
    }
    footer p {
      color: #7f8c8d;
      font-size: 0.9rem;
    }

    /* ========== RESPONSIVE STYLES ========== */
    @media (max-width: 768px) {
      nav {
        flex-direction: column;
      }
      .nav-item {
        margin: 5px 0;
        width: 90%;
        text-align: center;
      }
      .modal-content {
        width: 95%;
      }
    }
  </style>
</head>
<body>

  <!-- HEADER -->
  <header>
    <h1>Absensi & Keterlambatan</h1>
  </header>

  <!-- NAVBAR -->
  <nav>
    <a href="#" class="nav-item" onclick="showScreen('homeScreen')">Home</a>
    <a href="#" class="nav-item" onclick="showScreen('absensiScreen')">Input Absensi</a>
    <a href="#" class="nav-item" onclick="showScreen('terlambatScreen')">Data Keterlambatan</a>
    <a href="#" class="nav-item" onclick="showScreen('historyScreen')">History</a>
    <a href="#" class="nav-item" onclick="showScreen('statistikScreen')">Statistik</a>
    <a href="#" class="nav-item" onclick="showScreen('settingsScreen')">Settings</a>
    <a href="#" class="nav-item" onclick="showScreen('profileScreen')">Profile</a>
    <a href="#" class="nav-item" onclick="showScreen('dataSiswaScreen')">Data Siswa</a>
  </nav>

  <!-- MAIN CONTAINER -->
  <div class="container">

    <!-- ========== SCREEN: HOME (DASHBOARD) ========== -->
    <div id="homeScreen" class="screen active">
      <h2>Dashboard Home</h2>

      <!-- Grafik Kehadiran Bulanan -->
      <div class="dashboard-section">
        <div class="dashboard-card">
          <h3>Grafik Kehadiran Siswa Selama Sebulan Terakhir</h3>
          <canvas id="chartAttendanceLine" style="max-width:800px;"></canvas>
        </div>
      </div>

      <!-- Top 5 Siswa dengan Keterlambatan Terbanyak -->
      <div class="dashboard-section">
        <div class="dashboard-card">
          <h3>Top 5 Siswa dengan Keterlambatan Terbanyak</h3>
          <table>
            <thead>
              <tr>
                <th>Rank</th>
                <th>Nama Siswa</th>
                <th>Total Keterlambatan</th>
              </tr>
            </thead>
            <tbody id="dashboardTop5TableBody"></tbody>
          </table>
        </div>
      </div>
    </div>

    <!-- ========== SCREEN: INPUT ABSENSI ========== -->
    <div id="absensiScreen" class="screen">
      <h2>Form Absensi</h2>
      <div id="absensiStatus" class="status-box" style="display:none;"></div>
      <form onsubmit="catatKehadiran(event)">
        <label for="namaSiswa">Nama Siswa:</label>
        <input type="text" id="namaSiswa" list="siswaList" required>
        <datalist id="siswaList">
          <!-- Options akan diisi oleh JavaScript -->
        </datalist>

        <label for="tanggalAbsen">Tanggal:</label>
        <input type="date" id="tanggalAbsen" required>

        <label for="statusKedatangan">Status Kedatangan:</label>
        <select id="statusKedatangan" required onchange="toggleAlasanCustom()">
          <option value="">-- Pilih Status --</option>
          <option value="late">Terlambat</option>
          <option value="izin">Izin</option>
          <option value="dipulangkan">Dipulangkan</option>
        </select>

        <label for="dropdownAlasan">Alasan (Jika diperlukan):</label>
        <select id="dropdownAlasan">
          <option value="">-- Pilih Alasan --</option>
          <option value="Bangun kesiangan">Bangun kesiangan</option>
          <option value="Macet/Transportasi bermasalah">Macet/Transportasi bermasalah</option>
          <option value="Alasan kesehatan (sakit ringan)">Alasan kesehatan (sakit ringan)</option>
          <option value="Menemani keluarga yang membutuhkan">Menemani keluarga yang membutuhkan</option>
          <option value="Lupa membawa peralatan sekolah">Lupa membawa peralatan sekolah</option>
          <option value="Cuaca ekstrem (hujan/banjir)">Cuaca ekstrem (hujan/banjir)</option>
          <option value="Urusan keluarga mendadak">Urusan keluarga mendadak</option>
        </select>

        <div id="alasanCustomContainer" style="display:none;">
          <label for="alasanCustom">Alasan Custom:</label>
          <input type="text" id="alasanCustom" placeholder="Tulis alasan kustom...">
        </div>

        <button type="submit" class="btn-primary">Catat Absensi</button>
      </form>
    </div>

    <!-- ========== SCREEN: DATA KETERLAMBATAN ========== -->
    <div id="terlambatScreen" class="screen">
      <h2>Data Keterlambatan</h2>
      <button class="btn-danger" onclick="resetSemuaKeterlambatan()">Reset Semua Keterlambatan</button>
      <table>
        <thead>
          <tr>
            <th>Nama</th>
            <th>Terlambat Total</th>
            <th>Aksi</th>
          </tr>
        </thead>
        <tbody id="dataKeterlambatanTable"></tbody>
      </table>
    </div>

    <!-- ========== SCREEN: HISTORY ========== -->
    <div id="historyScreen" class="screen">
      <h2>Riwayat Absensi</h2>
      <div class="filter-section">
        <h3>Filter / Analisis</h3>
        <label for="filterDate">Filter Harian:</label>
        <input type="date" id="filterDate">
        <button class="btn-secondary" onclick="filterHistoryByDate()">Filter</button>

        <label for="filterStartDate" style="margin-top:15px;">Filter Rentang Tanggal:</label>
        <input type="date" id="filterStartDate">
        <input type="date" id="filterEndDate">
        <button class="btn-secondary" onclick="filterHistoryByDateRange()">Filter</button>

        <button class="btn-secondary" style="margin-top:15px;" onclick="resetHistoryFilters()">Reset Filter</button>
        <div id="filterResult" style="display:none; margin-top:15px;" class="status-box"></div>
      </div>

      <table>
        <thead>
          <tr>
            <th>Tanggal</th>
            <th>Nama Siswa</th>
            <th>Status</th>
            <th>Alasan</th>
            <th>Pengisi (Guru)</th>
            <th>Aksi</th>
          </tr>
        </thead>
        <tbody id="historyTableBody"></tbody>
      </table>
    </div>

    <!-- ========== SCREEN: STATISTIK ========== -->
    <div id="statistikScreen" class="screen">
      <h2>Statistik</h2>
      <div style="display:flex; flex-wrap:wrap; gap:20px; align-items: center;">
        <div>
          <label for="chartOption">Pilih Grafik:</label>
          <select id="chartOption">
            <option value="siswa">Siswa Paling Sering Telat</option>
            <option value="alasan">Alasan Paling Sering</option>
            <option value="bulanan">Grafik Keterlambatan Bulanan</option>
          </select>
          <button class="btn-primary" onclick="renderStatisticChart()">Tampilkan Grafik</button>
        </div>

        <div>
          <label for="statistikBulan">Pilih Bulan untuk Statistik:</label>
          <input type="month" id="statistikBulan">
          <button class="btn-primary" onclick="renderStatisticChart()">Tampilkan Statistik Bulanan</button>
        </div>
      </div>

      <canvas id="chartStatistic" style="max-width:800px; margin-top:30px;"></canvas>

      <div id="downloadSection" style="margin-top:30px; display:none;">
        <button class="btn-primary" onclick="downloadMonthlyData()">Download Data Bulanan (Excel)</button>
      </div>
    </div>

    <!-- ========== SCREEN: SETTINGS ========== -->
    <div id="settingsScreen" class="screen">
      <h2>Settings</h2>

      <!-- Petugas Piket Guru -->
      <div style="margin-bottom:30px;">
        <h3>Input Petugas Piket Guru (Harian, Max 10)</h3>
        <div style="display:flex; flex-wrap:wrap; gap:15px; align-items: center; margin-bottom:15px;">
          <div style="flex:1;">
            <label for="guruNama">Nama Guru:</label>
            <input type="text" id="guruNama" placeholder="Contoh: Pak Andi">
          </div>
          <div style="flex:1;">
            <label for="guruHari">Hari Piket:</label>
            <select id="guruHari">
              <option value="">-- Pilih Hari --</option>
              <option value="Senin">Senin</option>
              <option value="Selasa">Selasa</option>
              <option value="Rabu">Rabu</option>
              <option value="Kamis">Kamis</option>
              <option value="Jumat">Jumat</option>
              <option value="Sabtu">Sabtu</option>
            </select>
          </div>
          <div style="align-self: flex-end;">
            <button class="btn-primary" onclick="tambahPetugasGuru()">Tambah Guru</button>
          </div>
        </div>
        <table>
          <thead>
            <tr>
              <th>Nama Guru</th>
              <th>Hari</th>
              <th>Aksi</th>
            </tr>
          </thead>
          <tbody id="petugasGuruTable"></tbody>
        </table>
      </div>

      <hr>

      <!-- Petugas Piket Siswa -->
      <div style="margin-bottom:30px;">
        <h3>Input Petugas Piket Siswa (2 Orang)</h3>
        <div style="display:flex; flex-wrap:wrap; gap:15px; align-items: center; margin-bottom:15px;">
          <div style="flex:1;">
            <label for="siswa1">Siswa 1:</label>
            <input type="text" id="siswa1" placeholder="Contoh: Budi">
          </div>
          <div style="flex:1;">
            <label for="siswa2">Siswa 2:</label>
            <input type="text" id="siswa2" placeholder="Contoh: Susi">
          </div>
          <div style="align-self: flex-end;">
            <button class="btn-primary" onclick="simpanPetugasSiswa()">Simpan Siswa</button>
          </div>
        </div>
      </div>

      <hr>

      <!-- Pengaturan Lainnya -->
      <div>
        <h3>Pengaturan Lainnya</h3>
        <div style="display:flex; flex-wrap:wrap; gap:15px; align-items: center; margin-bottom:15px;">
          <div style="flex:1;">
            <label for="languageSelect">Pilih Bahasa:</label>
            <select id="languageSelect">
              <option value="id">Bahasa Indonesia</option>
              <option value="en">English</option>
            </select>
          </div>
          <div style="align-self: flex-end;">
            <button class="btn-primary" onclick="alert('Pengaturan belum diimplementasikan.')">Simpan</button>
          </div>
        </div>
      </div>
    </div>

    <!-- ========== SCREEN: PROFILE ========== -->
    <div id="profileScreen" class="screen">
      <h2>Profil Petugas & Pengisi</h2>
      <p>Total Data Absensi: <strong id="profileAbsensiCount">0</strong></p>

      <hr>

      <!-- Daftar Petugas Guru -->
      <div style="margin-bottom:30px;">
        <h3>Daftar Petugas Guru (Urutan Senin–Sabtu)</h3>
        <table>
          <thead>
            <tr>
              <th>Hari</th>
              <th>Nama Guru</th>
              <th>Aksi</th>
            </tr>
          </thead>
          <tbody id="profileGuruTbody"></tbody>
        </table>
      </div>

      <!-- Daftar Petugas Siswa -->
      <div style="margin-bottom:30px;">
        <h3>Daftar Petugas Siswa (2 Orang)</h3>
        <ul id="profileSiswaList" style="list-style-type: disc; padding-left: 20px;"></ul>
      </div>

      <!-- Pengisi Hari Ini -->
      <div>
        <h3>Pengisi (Guru) Sesuai Hari Aktual</h3>
        <p>Hari ini: <strong id="hariAktualSpan">-</strong></p>
        <div style="display:flex; flex-wrap:wrap; gap:15px; align-items: center; margin-top:10px;">
          <div style="flex:1;">
            <label for="dropdownPengisi">Pilih Guru (Hari Ini):</label>
            <select id="dropdownPengisi">
              <!-- Options akan diisi oleh JavaScript -->
            </select>
          </div>
          <div style="align-self: flex-end;">
            <button class="btn-primary" onclick="setPengisi()">Set Pengisi</button>
          </div>
        </div>
        <p style="margin-top:15px;">Pengisi Saat Ini: <strong id="currentPengisiSpan">-</strong></p>
      </div>

      <!-- Modal untuk Mengedit Guru Piket -->
      <div id="editGuruModal" class="modal">
        <div class="modal-content">
          <span class="close" onclick="closeEditGuruModal()">&times;</span>
          <h3>Edit Guru Piket</h3>
          <form id="editGuruForm">
            <input type="hidden" id="editGuruIndex">
            <label for="editGuruNama">Nama Guru:</label>
            <input type="text" id="editGuruNama" required>
            <label for="editGuruHari">Hari Piket:</label>
            <select id="editGuruHari" required>
              <option value="">-- Pilih Hari --</option>
              <option value="Senin">Senin</option>
              <option value="Selasa">Selasa</option>
              <option value="Rabu">Rabu</option>
              <option value="Kamis">Kamis</option>
              <option value="Jumat">Jumat</option>
              <option value="Sabtu">Sabtu</option>
            </select>
            <button type="submit" class="btn-primary" style="margin-top:15px;">Simpan Perubahan</button>
          </form>
        </div>
      </div>
    </div>

    <!-- ========== SCREEN: DATA SISWA ========== -->
    <div id="dataSiswaScreen" class="screen">
      <h2>Data Siswa</h2>

      <!-- Form Tambah Siswa -->
      <div style="margin-bottom:30px;">
        <h3>Tambah Siswa</h3>
        <form onsubmit="tambahSiswa(event)">
          <div style="display:flex; flex-wrap:wrap; gap:15px;">
            <div style="flex:1;">
              <label for="dataSiswaNama">Nama Siswa:</label>
              <input type="text" id="dataSiswaNama" placeholder="Contoh: Ahmad" required>
            </div>
            <div style="flex:1;">
              <label for="dataSiswaKelas">Kelas:</label>
              <input type="text" id="dataSiswaKelas" placeholder="Contoh: 10 IPA 1" required>
            </div>
            <div style="flex:1;">
              <label for="dataSiswaNIS">NIS:</label>
              <input type="text" id="dataSiswaNIS" placeholder="Contoh: 123456" required>
            </div>
            <div style="flex:1;">
              <label for="dataSiswaWaliKelas">Wali Kelas:</label>
              <input type="text" id="dataSiswaWaliKelas" placeholder="Contoh: Ibu Sari" required>
            </div>
          </div>
          <button type="submit" class="btn-primary" style="margin-top:15px;">Tambah Siswa</button>
        </form>
      </div>

      <!-- Import Data Siswa -->
      <div style="margin-bottom:30px;">
        <h3>Import Data Siswa dari Excel</h3>
        <input type="file" id="importFileSiswa" accept=".xlsx, .xls">
        <button class="btn-primary" onclick="importDataSiswa()" style="margin-top:10px;">Import Data Siswa</button>
      </div>

      <!-- Tombol Hapus Semua & Hapus Pilih -->
      <div style="margin-bottom:30px;">
        <button class="btn-danger" onclick="hapusSemuaSiswa()">Hapus Semua</button>
        <button class="btn-danger" onclick="hapusPilihSiswa()" style="margin-left:10px;">Hapus Pilih</button>
      </div>

      <!-- Tabel Data Siswa -->
      <table>
        <thead>
          <tr>
            <th><input type="checkbox" id="selectAll" onclick="toggleSelectAll(this)"></th>
            <th>Nama Siswa</th>
            <th>Kelas</th>
            <th>NIS</th>
            <th>Wali Kelas</th>
            <th>Aksi</th>
          </tr>
        </thead>
        <tbody id="dataSiswaTable"></tbody>
      </table>
    </div>

  </div>

  <!-- FOOTER -->
  <footer>
    <div class="footer-line"></div>
    <p>
      Created by <br>
      Antonius Elga Kurnia, S.Pd.
    </p>
  </footer>

  <!-- JAVASCRIPT -->
  <script>
    /********************************************************
     *                VAR & STATE
     ********************************************************/
    let currentScreen = "homeScreen";

    // Guru array: {nama, hari} => max 10. 
    let petugasGuru = [];
    let petugasSiswa = []; // array of 2 strings
    let currentPengisi = ""; // guru yg input data

    // Data Absensi & Rekap
    // “pengisi” => guru yang input data
    let historyAbsensi = [];
    let archiveAbsensi = [];
    let dataSiswa = {}; // {nama: {terlambat_total, terlambat_mingguan, last_date}}
    let databaseSiswa = []; // Array of siswa {nama, kelas, nis, waliKelas}

    // Data Keterlambatan
    let dataKeterlambatan = []; // Array of {nama, terlambat_total}

    let chartObj = null;
    let attendanceLineChart = null; // Untuk Grafik Kehadiran Bulanan

    // Definisikan urutan hari
    const dayOrder = ["Senin","Selasa","Rabu","Kamis","Jumat","Sabtu"];

    // Definisikan localStorage keys
    const STORAGE_KEYS = {
      HISTORY: 'historyAbsensi',
      ARCHIVE: 'archiveAbsensi',
      GURU: 'petugasGuru',
      SISWA: 'petugasSiswa',
      DATA_SISWA: 'databaseSiswa',
      DATA_KETERLAMBATAN: 'dataKeterlambatan',
      LAST_ARCHIVE: 'lastArchiveDate',
      CURRENT_PENGISI: 'currentPengisi'
    };

    /********************************************************
     *          FUNGSI UNTUK LOCAL STORAGE
     ********************************************************/
    function saveData() {
      localStorage.setItem(STORAGE_KEYS.HISTORY, JSON.stringify(historyAbsensi));
      localStorage.setItem(STORAGE_KEYS.ARCHIVE, JSON.stringify(archiveAbsensi));
      localStorage.setItem(STORAGE_KEYS.GURU, JSON.stringify(petugasGuru));
      localStorage.setItem(STORAGE_KEYS.SISWA, JSON.stringify(petugasSiswa));
      localStorage.setItem(STORAGE_KEYS.DATA_SISWA, JSON.stringify(databaseSiswa));
      localStorage.setItem(STORAGE_KEYS.DATA_KETERLAMBATAN, JSON.stringify(dataKeterlambatan));
      localStorage.setItem(STORAGE_KEYS.LAST_ARCHIVE, JSON.stringify(new Date()));
      localStorage.setItem(STORAGE_KEYS.CURRENT_PENGISI, JSON.stringify(currentPengisi));
    }

    function loadData() {
      let history = localStorage.getItem(STORAGE_KEYS.HISTORY);
      if(history) historyAbsensi = JSON.parse(history);
      
      let archive = localStorage.getItem(STORAGE_KEYS.ARCHIVE);
      if(archive) archiveAbsensi = JSON.parse(archive);
      
      let guru = localStorage.getItem(STORAGE_KEYS.GURU);
      if(guru) petugasGuru = JSON.parse(guru);
      
      let siswa = localStorage.getItem(STORAGE_KEYS.SISWA);
      if(siswa) petugasSiswa = JSON.parse(siswa);
      
      let dataS = localStorage.getItem(STORAGE_KEYS.DATA_SISWA);
      if(dataS) {
        try {
          databaseSiswa = JSON.parse(dataS);
          // Pastikan databaseSiswa adalah array
          if(!Array.isArray(databaseSiswa)){
            databaseSiswa = [];
          }
        } catch(e) {
          console.error("Error parsing databaseSiswa from localStorage:", e);
          databaseSiswa = [];
        }
      }

      let dataKet = localStorage.getItem(STORAGE_KEYS.DATA_KETERLAMBATAN);
      if(dataKet){
        try{
          dataKeterlambatan = JSON.parse(dataKet);
          if(!Array.isArray(dataKeterlambatan)){
            dataKeterlambatan = [];
          }
        } catch(e){
          console.error("Error parsing dataKeterlambatan from localStorage:", e);
          dataKeterlambatan = [];
        }
      }
      
      let pengisi = localStorage.getItem(STORAGE_KEYS.CURRENT_PENGISI);
      if(pengisi) currentPengisi = JSON.parse(pengisi);
    }

    /********************************************************
     *          FUNGSI ARCHIVE DATA MINGGUAN
     ********************************************************/
    function archiveWeeklyData() {
      let lastArchive = localStorage.getItem(STORAGE_KEYS.LAST_ARCHIVE);
      let lastDate = lastArchive ? new Date(JSON.parse(lastArchive)) : null;
      let today = new Date();

      if(!lastDate) {
        // Jika belum pernah di-archive, set tanggal sekarang
        localStorage.setItem(STORAGE_KEYS.LAST_ARCHIVE, JSON.stringify(today));
        return;
      }

      // Hitung selisih minggu
      let diffTime = today - lastDate;
      let diffWeeks = Math.floor(diffTime / (1000 * 60 * 60 * 24 * 7));

      if(diffWeeks >=1){
        // Pindahkan historyAbsensi ke archiveAbsensi
        archiveAbsensi = archiveAbsensi.concat(historyAbsensi);
        historyAbsensi = [];

        // Reset jumlah keterlambatan mingguan
        dataKeterlambatan.forEach(siswa => {
          siswa.terlambat_mingguan = 0;
        });

        // Simpan tanggal archive terbaru
        localStorage.setItem(STORAGE_KEYS.LAST_ARCHIVE, JSON.stringify(today));

        // Simpan data
        saveData();

        alert("Data absensi minggu lalu telah diarsipkan dan keterlambatan mingguan direset.");
      }
    }

    /********************************************************
     *                NAVIGASI MENU
     ********************************************************/
    function showScreen(screenId) {
      document.getElementById(currentScreen)?.classList.remove('active');
      document.getElementById(screenId)?.classList.add('active');
      currentScreen = screenId;

      // Refresh tampilan
      if (screenId === 'terlambatScreen') {
        renderDataKeterlambatanTable();
      }
      if (screenId === 'historyScreen') {
        renderHistoryTable();
      }
      if (screenId === 'profileScreen') {
        updateProfileInfo();    
        renderProfileGuru();    
        renderProfileSiswa();   
        renderProfilePengisiToday(); 
      }
      if (screenId === 'settingsScreen') {
        renderPetugasGuruTable(); // Supaya tampilan table guru di Settings update
      }
      if (screenId === 'dataSiswaScreen') {
        renderDataSiswaTable();
        renderDataSiswaList(); // Update datalist untuk autocomplete
      }
      if (screenId === 'statistikScreen') {
        // Jika grafik sudah ada, destroy untuk update
        if(chartObj) chartObj.destroy();
        if(attendanceLineChart) attendanceLineChart.destroy();
        document.getElementById('downloadSection').style.display = "none";
      }
      if (screenId === 'homeScreen') {
        renderHome();
      }
    }

    /********************************************************
     *     PETUGAS GURU: input & list
     ********************************************************/
    function tambahPetugasGuru() {
      let nama = document.getElementById('guruNama').value.trim();
      let hari = document.getElementById('guruHari').value.trim();
      if (!nama || !hari) {
        alert("Nama & Hari piket guru tidak boleh kosong!");
        return;
      }
      if (petugasGuru.length >= 10) {
        alert("Max guru=10!");
        return;
      }
      // Cek duplikat
      let dupe = petugasGuru.some(g => g.nama.toLowerCase() === nama.toLowerCase() && g.hari === hari);
      if(dupe){
        alert("Guru dengan nama dan hari piket tersebut sudah ada!");
        return;
      }
      // Tambah
      petugasGuru.push({nama, hari});
      document.getElementById('guruNama').value = "";
      document.getElementById('guruHari').value = "";
      alert(`Ditambahkan: ${nama} (hari: ${hari})`);
      renderPetugasGuruTable();
      saveData();
      renderProfileGuru();
      renderProfilePengisiToday();
      renderHome(); // Update Home dashboard jika diperlukan
    }

    function renderPetugasGuruTable() {
      let tbody = document.getElementById('petugasGuruTable');
      tbody.innerHTML = "";

      petugasGuru.forEach((item, index) => {
        let tr = document.createElement('tr');
        let tdNama = document.createElement('td');
        tdNama.textContent = item.nama;
        let tdHari = document.createElement('td');
        tdHari.textContent = item.hari;
        let tdAksi = document.createElement('td');
        
        // Tombol Edit
        let btnEdit = document.createElement('button');
        btnEdit.textContent = "Edit";
        btnEdit.onclick = () => openEditGuruModal(index);
        btnEdit.classList.add('btn-secondary');

        // Tombol Hapus
        let btnHapus = document.createElement('button');
        btnHapus.textContent = "Hapus";
        btnHapus.classList.add('btn-danger');
        btnHapus.onclick = () => hapusPetugasGuru(index);

        tdAksi.appendChild(btnEdit);
        tdAksi.appendChild(btnHapus);

        tr.appendChild(tdNama);
        tr.appendChild(tdHari);
        tr.appendChild(tdAksi);
        tbody.appendChild(tr);
      });
    }

    function hapusPetugasGuru(index){
      if(!confirm(`Hapus guru piket: ${petugasGuru[index].nama} pada hari ${petugasGuru[index].hari}?`)) return;
      petugasGuru.splice(index,1);
      renderPetugasGuruTable();
      saveData();
      renderProfileGuru();
      renderProfilePengisiToday();
      renderHome(); // Update Home dashboard jika diperlukan
      alert("Guru piket dihapus!");
    }

    /********************************************************
     *     MANIPULASI MODAL EDIT GURU PIKET
     ********************************************************/
    // Modal Elements
    const editGuruModal = document.getElementById('editGuruModal');

    function openEditGuruModal(index){
      document.getElementById('editGuruIndex').value = index;
      document.getElementById('editGuruNama').value = petugasGuru[index].nama;
      document.getElementById('editGuruHari').value = petugasGuru[index].hari;
      editGuruModal.style.display = "flex";
    }

    function closeEditGuruModal(){
      editGuruModal.style.display = "none";
    }

    // Close modal when clicking outside
    window.onclick = function(event) {
      if (event.target == editGuruModal) {
        editGuruModal.style.display = "none";
      }
    }

    // Handle Edit Guru Form Submission
    document.getElementById('editGuruForm').addEventListener('submit', function(e){
      e.preventDefault();
      let index = parseInt(document.getElementById('editGuruIndex').value);
      let nama = document.getElementById('editGuruNama').value.trim();
      let hari = document.getElementById('editGuruHari').value.trim();
      if(!nama || !hari){
        alert("Nama & Hari piket guru tidak boleh kosong!");
        return;
      }
      // Cek duplikat
      let dupe = petugasGuru.some((g, i) => g.nama.toLowerCase() === nama.toLowerCase() && g.hari === hari && i !== index);
      if(dupe){
        alert("Guru dengan nama dan hari piket tersebut sudah ada!");
        return;
      }
      petugasGuru[index] = {nama, hari};
      closeEditGuruModal();
      renderPetugasGuruTable();
      saveData();
      renderProfileGuru();
      renderProfilePengisiToday();
      renderHome(); // Update Home dashboard jika diperlukan
      alert("Data guru piket diperbarui!");
    });

    /********************************************************
     *     MANAGEMEN SISWA: input & list
     ********************************************************/
    function tambahSiswa(event){
      event.preventDefault();
      let nama = document.getElementById('dataSiswaNama').value.trim();
      let kelas = document.getElementById('dataSiswaKelas').value.trim();
      let nis = document.getElementById('dataSiswaNIS').value.trim();
      let waliKelas = document.getElementById('dataSiswaWaliKelas').value.trim();
      if(!nama || !kelas || !nis || !waliKelas){
        alert("Nama, Kelas, NIS, dan Wali Kelas siswa tidak boleh kosong!");
        return;
      }
      // Cek duplikat
      let dupe = databaseSiswa.some(s => s.nama.toLowerCase() === nama.toLowerCase() || s.nis === nis);
      if(dupe){
        alert("Siswa dengan nama atau NIS tersebut sudah ada!");
        return;
      }
      // Tambah siswa
      databaseSiswa.push({nama, kelas, nis, waliKelas});
      document.getElementById('dataSiswaNama').value = "";
      document.getElementById('dataSiswaKelas').value = "";
      document.getElementById('dataSiswaNIS').value = "";
      document.getElementById('dataSiswaWaliKelas').value = "";
      renderDataSiswaTable();
      renderDataSiswaList(); // Update datalist untuk autocomplete
      saveData();
      alert("Siswa berhasil ditambahkan!");
    }

    /********************************************************
     *      PENCARIAN DAN PENYARINGAN DATA SISWA
     ********************************************************/
    function cariSiswa(){
      let query = document.getElementById('searchSiswa')?.value.trim().toLowerCase();
      if(!query){
        alert("Masukkan kata kunci pencarian.");
        return;
      }
      // Filter dataSiswa berdasarkan nama, kelas, NIS, atau waliKelas
      let filtered = databaseSiswa.filter(siswa => 
        siswa.nama.toLowerCase().includes(query) ||
        siswa.kelas.toLowerCase().includes(query) ||
        siswa.nis.toLowerCase().includes(query) ||
        siswa.waliKelas.toLowerCase().includes(query)
      );
      renderDataSiswaTable(filtered);
    }

    function resetCariSiswa(){
      if(document.getElementById('searchSiswa')){
        document.getElementById('searchSiswa').value = "";
      }
      renderDataSiswaTable();
    }

    /********************************************************
     *      MODIFIKASI FUNGSI RENDER DATA SISWA
     ********************************************************/
    function renderDataSiswaTable(filteredData = null){
      let tbody = document.getElementById('dataSiswaTable');
      tbody.innerHTML = "";

      let dataToRender = filteredData || databaseSiswa;

      dataToRender.forEach((siswa, index) => {
        let tr = document.createElement('tr');

        // Checkbox
        let tdCheckbox = document.createElement('td');
        let checkbox = document.createElement('input');
        checkbox.type = "checkbox";
        checkbox.classList.add('select-siswa');
        checkbox.value = index;
        tdCheckbox.appendChild(checkbox);
        tr.appendChild(tdCheckbox);

        // Nama Siswa
        let tdNama = document.createElement('td');
        tdNama.textContent = siswa.nama;
        tr.appendChild(tdNama);

        // Kelas
        let tdKelas = document.createElement('td');
        tdKelas.textContent = siswa.kelas;
        tr.appendChild(tdKelas);

        // NIS
        let tdNIS = document.createElement('td');
        tdNIS.textContent = siswa.nis;
        tr.appendChild(tdNIS);

        // Wali Kelas
        let tdWaliKelas = document.createElement('td');
        tdWaliKelas.textContent = siswa.waliKelas;
        tr.appendChild(tdWaliKelas);

        // Aksi
        let tdAksi = document.createElement('td');

        // Tombol Edit
        let btnEdit = document.createElement('button');
        btnEdit.textContent = "Ubah";
        btnEdit.onclick = () => editSiswa(index);
        btnEdit.classList.add('btn-secondary');

        // Tombol Hapus
        let btnHapus = document.createElement('button');
        btnHapus.textContent = "Hapus";
        btnHapus.classList.add('btn-danger');
        btnHapus.onclick = () => hapusSiswa(index);

        tdAksi.appendChild(btnEdit);
        tdAksi.appendChild(btnHapus);

        tr.appendChild(tdAksi);
        tbody.appendChild(tr);
      });
    }

    // Fungsi untuk memperbarui datalist untuk autocomplete
    function renderDataSiswaList() {
      let datalist = document.getElementById('siswaList');
      datalist.innerHTML = "";
      databaseSiswa.forEach(siswa => {
        let option = document.createElement('option');
        option.value = siswa.nama;
        datalist.appendChild(option);
      });
    }

    function editSiswa(index){
      let newNama = prompt("Masukkan nama baru siswa:", databaseSiswa[index].nama);
      if(newNama === null) return; // Cancel
      newNama = newNama.trim();
      if(!newNama){
        alert("Nama tidak boleh kosong!");
        return;
      }
      let newKelas = prompt("Masukkan kelas baru siswa:", databaseSiswa[index].kelas);
      if(newKelas === null) return; // Cancel
      newKelas = newKelas.trim();
      if(!newKelas){
        alert("Kelas tidak boleh kosong!");
        return;
      }
      let newNIS = prompt("Masukkan NIS baru siswa:", databaseSiswa[index].nis);
      if(newNIS === null) return; // Cancel
      newNIS = newNIS.trim();
      if(!newNIS){
        alert("NIS tidak boleh kosong!");
        return;
      }
      let newWaliKelas = prompt("Masukkan Wali Kelas baru siswa:", databaseSiswa[index].waliKelas);
      if(newWaliKelas === null) return; // Cancel
      newWaliKelas = newWaliKelas.trim();
      if(!newWaliKelas){
        alert("Wali Kelas tidak boleh kosong!");
        return;
      }
      // Cek duplikat
      let dupe = databaseSiswa.some((s, i) => 
        (s.nama.toLowerCase() === newNama.toLowerCase() || s.nis.toLowerCase() === newNIS.toLowerCase()) && i !== index
      );
      if(dupe){
        alert("Siswa dengan nama atau NIS tersebut sudah ada!");
        return;
      }
      databaseSiswa[index] = {nama: newNama, kelas: newKelas, nis: newNIS, waliKelas: newWaliKelas};
      renderDataSiswaTable();
      renderDataSiswaList(); // Update datalist untuk autocomplete
      saveData();
      alert("Data siswa diperbarui!");
    }

    function hapusSiswa(index){
      if(!confirm(`Hapus data siswa: ${databaseSiswa[index].nama} dari kelas ${databaseSiswa[index].kelas}?`)) return;
      // Hapus dari historyAbsensi dan archiveAbsensi
      let namaLower = databaseSiswa[index].nama.toLowerCase();
      historyAbsensi = historyAbsensi.filter(i => i.nama.toLowerCase() !== namaLower);
      archiveAbsensi = archiveAbsensi.filter(i => i.nama.toLowerCase() !== namaLower);
      // Hapus dari dataKeterlambatan
      dataKeterlambatan = dataKeterlambatan.filter(k => k.nama.toLowerCase() !== namaLower);
      // Hapus dari databaseSiswa
      let deletedNama = databaseSiswa[index].nama;
      databaseSiswa.splice(index,1);
      petugasSiswa = petugasSiswa.filter(s => s.toLowerCase() !== namaLower);
      renderDataSiswaList(); // Update datalist untuk autocomplete
      recalcDataSiswaFromHistory();
      renderHistoryTable();
      renderDataKeterlambatanTable();
      renderTerlambatTable();
      renderDataSiswaTable();
      renderProfileSiswa();
      saveData();
      renderHome(); // Update Home dashboard jika diperlukan
      alert(`Data siswa "${deletedNama}" dihapus!`);
    }

    /********************************************************
     *     SIMPAN PETUGAS SISWA
     ********************************************************/
    function simpanPetugasSiswa() {
      let s1 = document.getElementById('siswa1').value.trim();
      let s2 = document.getElementById('siswa2').value.trim();
      if(!s1||!s2) {
        alert("Isi kedua siswa!");
        return;
      }
      // Cek apakah siswa ada di databaseSiswa
      let siswa1Exists = databaseSiswa.some(s => s.nama.toLowerCase() === s1.toLowerCase());
      let siswa2Exists = databaseSiswa.some(s => s.nama.toLowerCase() === s2.toLowerCase());
      if(!siswa1Exists || !siswa2Exists){
        alert("Salah satu atau kedua siswa tidak ditemukan di database. Silakan tambahkan siswa di menu Data Siswa.");
        return;
      }
      petugasSiswa = [s1, s2];
      alert("Petugas Siswa => "+petugasSiswa.join(" & "));
      saveData();
      renderProfileSiswa();
      renderHome(); // Update Home dashboard jika diperlukan
      document.getElementById('siswa1').value = "";
      document.getElementById('siswa2').value = "";
    }

    /********************************************************
     *      REKALKULASI dataSiswa
     ********************************************************/
    function recalcDataSiswaFromHistory() {
      dataSiswa = {};
      let combinedData = historyAbsensi.concat(archiveAbsensi);
      combinedData.forEach(record => {
        let { tanggal, nama, status } = record;
        if (!dataSiswa[nama]) {
          dataSiswa[nama] = {
            terlambat_total: 0,
            terlambat_mingguan: 0,
            last_date: ""
          };
        }
        if (status === "late") {
          dataSiswa[nama].terlambat_total++;
          dataSiswa[nama].terlambat_mingguan++;
          if (!dataSiswa[nama].last_date || tanggal > dataSiswa[nama].last_date) {
            dataSiswa[nama].last_date = tanggal;
          }
        }
      });

      // Update dataKeterlambatan
      dataKeterlambatan = [];
      for(let nama in dataSiswa){
        let info = dataSiswa[nama];
        if(info.terlambat_total > 0){
          dataKeterlambatan.push({
            nama,
            terlambat_total: info.terlambat_total
          });
        }
      }
    }

    /********************************************************
     *            INPUT ABSENSI (CEK DUPLIKAT)
     ********************************************************/
    function catatKehadiran(event) {
      event.preventDefault();
      let rawNama = document.getElementById('namaSiswa').value.trim();
      let namaSiswa = rawNama.toLowerCase();
      let tanggal = document.getElementById('tanggalAbsen').value;

      let statusSelect = document.getElementById('statusKedatangan');
      let statusKedatangan = statusSelect.value;

      let alasan = document.getElementById('dropdownAlasan').value.trim();
      let alasanCustom = document.getElementById('alasanCustom').value.trim();
      let finalAlasan = "-";

      if(!rawNama || !tanggal || !statusKedatangan){
        alert("Mohon lengkapi data (nama, tanggal, status).");
        return;
      }

      // Cek apakah siswa ada di databaseSiswa
      let siswaExists = databaseSiswa.some(s => s.nama.toLowerCase() === namaSiswa);
      if(!siswaExists){
        alert("Siswa tidak ditemukan di database. Silakan tambahkan siswa di menu Data Siswa.");
        return;
      }

      // cek duplikat
      let dupe = historyAbsensi.some(x=>x.nama.toLowerCase() === namaSiswa && x.tanggal === tanggal);
      let dupeArchive = archiveAbsensi.some(x=>x.nama.toLowerCase() === namaSiswa && x.tanggal === tanggal);
      if(dupe || dupeArchive){
        alert(`Siswa ${rawNama} sudah tercatat di ${tanggal}`);
        return;
      }

      // Validasi tambahan: Jika status adalah 'late', pastikan alasan atau alasanCustom diisi
      if(statusKedatangan === "late"){
        if(!alasan && !alasanCustom){
          alert("Mohon isi alasan keterlambatan atau alasan kustom.");
          return;
        }
      }

      if(statusKedatangan === "late"){
        if(alasanCustom){
          finalAlasan = alasanCustom;
        } else if(alasan){
          finalAlasan = alasan;
        }
      } else if(statusKedatangan === "izin" || statusKedatangan === "dipulangkan") {
        finalAlasan = alasan || "-";
      }

      // catat
      historyAbsensi.push({
        nama: rawNama, // Simpan dengan format asli
        tanggal,
        status:statusKedatangan,
        alasan:finalAlasan,
        pengisi: currentPengisi || "-"
      });

      // Update dataKeterlambatan hanya jika status adalah 'late'
      if(statusKedatangan === "late"){
        let keterlambatan = dataKeterlambatan.find(k => k.nama.toLowerCase() === namaSiswa);
        if(keterlambatan){
          keterlambatan.terlambat_total += 1;
        } else {
          dataKeterlambatan.push({
            nama: rawNama,
            terlambat_total: 1
          });
        }

        // Cek jika terlambat >= 3
        if(keterlambatan && keterlambatan.terlambat_total === 3){
          alert(`Siswa ${rawNama} sudah terlambat 3x dan harus dipulangkan.`);
          // Implementasikan logika pemulangan sesuai kebutuhan
        }
      }

      recalcDataSiswaFromHistory();
      renderHistoryTable();
      renderDataKeterlambatanTable();
      renderTerlambatTable();
      renderDataSiswaTable();
      saveData();

      let box = document.getElementById('absensiStatus');
      box.style.display="block";
      box.textContent=`Data tersimpan: ${rawNama}, ${tanggal}, ${statusKedatangan}, Pengisi=${currentPengisi||"-"}`;

      // reset
      document.getElementById('namaSiswa').value="";
      document.getElementById('tanggalAbsen').value="";
      statusSelect.value = "";
      document.getElementById('dropdownAlasan').value = "";
      document.getElementById('alasanCustom').value = "";
      document.getElementById('alasanCustomContainer').style.display = "none";
    }

    /********************************************************
     *      TOGGLE ALASAN CUSTOM
     ********************************************************/
    function toggleAlasanCustom(){
      let status = document.getElementById('statusKedatangan').value;
      if(status === "late"){
        document.getElementById('alasanCustomContainer').style.display = "block";
      } else {
        document.getElementById('alasanCustomContainer').style.display = "none";
        document.getElementById('alasanCustom').value = "";
      }
    }

    /********************************************************
     *      RENDER TABEL HISTORY
     ********************************************************/
    function renderHistoryTable(filteredData=null) {
      let tbody = document.getElementById('historyTableBody');
      tbody.innerHTML="";
      let dataToRender = filteredData || historyAbsensi;

      // Sertakan juga archiveAbsensi jika filter tidak spesifik
      if(!filteredData){
        dataToRender = historyAbsensi.concat(archiveAbsensi);
      }

      dataToRender.forEach((item,index)=>{
        let tr=document.createElement('tr');

        // Tanggal
        let tdTgl=document.createElement('td');
        tdTgl.textContent=item.tanggal;
        tr.appendChild(tdTgl);

        // Nama
        let tdNama=document.createElement('td');
        tdNama.textContent=item.nama;
        tr.appendChild(tdNama);

        // Status (Dropdown)
        let tdStatus=document.createElement('td');
        let selectStatus = document.createElement('select');
        selectStatus.innerHTML = `
          <option value="late" ${item.status === "late" ? "selected" : ""}>Terlambat</option>
          <option value="izin" ${item.status === "izin" ? "selected" : ""}>Izin</option>
          <option value="dipulangkan" ${item.status === "dipulangkan" ? "selected" : ""}>Dipulangkan</option>
        `;
        selectStatus.onchange = () => updateStatusHistory(index, selectStatus.value);
        tdStatus.appendChild(selectStatus);
        tr.appendChild(tdStatus);

        // Alasan
        let tdAlasan=document.createElement('td');
        tdAlasan.textContent=item.alasan || "-";
        tr.appendChild(tdAlasan);

        // Pengisi
        let tdPengisi=document.createElement('td');
        tdPengisi.textContent=item.pengisi || "-";
        tr.appendChild(tdPengisi);

        // Aksi
        let tdAksi=document.createElement('td');
        let btnE=document.createElement('button');
        btnE.textContent="Edit Tanggal";
        btnE.classList.add('btn-secondary');
        btnE.onclick=()=>editRiwayatTanggal(index);

        let btnH=document.createElement('button');
        btnH.textContent="Hapus";
        btnH.classList.add('btn-danger');
        btnH.onclick=()=>deleteRiwayat(index);

        tdAksi.appendChild(btnE);
        tdAksi.appendChild(btnH);
        tr.appendChild(tdAksi);

        tbody.appendChild(tr);
      });
    }

    function updateStatusHistory(index, newStatus){
      // Tentukan apakah data tersebut berada di historyAbsensi atau archiveAbsensi
      if(index < historyAbsensi.length){
        historyAbsensi[index].status = newStatus;
      } else {
        archiveAbsensi[index - historyAbsensi.length].status = newStatus;
      }
      // Recalculate data
      recalcDataSiswaFromHistory();
      renderHistoryTable();
      renderDataKeterlambatanTable();
      renderTerlambatTable();
      saveData();
      alert("Status absensi berhasil diperbarui!");
      renderHome(); // Update Home dashboard jika diperlukan
    }

    function editRiwayatTanggal(index){
      let oldDate;
      // Tentukan apakah data berada di history atau archive
      if(index < historyAbsensi.length){
        oldDate = historyAbsensi[index].tanggal;
      } else {
        oldDate = archiveAbsensi[index - historyAbsensi.length].tanggal;
      }

      let newDate=prompt("Tanggal baru (YYYY-MM-DD):",oldDate);
      if(newDate&&newDate!==oldDate){
        // Validasi siswa masih ada
        let namaSiswa;
        if(index < historyAbsensi.length){
          namaSiswa = historyAbsensi[index].nama.toLowerCase();
        } else {
          namaSiswa = archiveAbsensi[index - historyAbsensi.length].nama.toLowerCase();
        }

        let siswaExists = databaseSiswa.some(s => s.nama.toLowerCase() === namaSiswa);
        if(!siswaExists){
          alert("Siswa tidak ditemukan di database. Silakan tambahkan siswa di menu Data Siswa.");
          return;
        }

        if(index < historyAbsensi.length){
          historyAbsensi[index].tanggal=newDate;
        } else {
          archiveAbsensi[index - historyAbsensi.length].tanggal=newDate;
        }

        recalcDataSiswaFromHistory();
        renderHistoryTable();
        renderDataKeterlambatanTable();
        renderTerlambatTable();
        saveData();
        alert("Tanggal berhasil diubah!");
        renderHome(); // Update Home dashboard jika diperlukan
      }
    }
    function deleteRiwayat(index){
      let rec;
      if(index < historyAbsensi.length){
        rec = historyAbsensi[index];
      } else {
        rec = archiveAbsensi[index - historyAbsensi.length];
      }

      if(!confirm(`Hapus absensi: ${rec.nama}, ${rec.tanggal}?`))return;
      if(index < historyAbsensi.length){
        historyAbsensi.splice(index,1);
      } else {
        archiveAbsensi.splice(index - historyAbsensi.length,1);
      }
      recalcDataSiswaFromHistory();
      renderHistoryTable();
      renderDataKeterlambatanTable();
      renderTerlambatTable();
      saveData();
      alert("Data absensi terhapus!");
      renderHome(); // Update Home dashboard jika diperlukan
    }

    /********************************************************
     *      RENDER TABEL DATA KETERLAMBATAN
     ********************************************************/
    function renderDataKeterlambatanTable(){
      let tbody = document.getElementById('dataKeterlambatanTable');
      tbody.innerHTML = "";

      dataKeterlambatan.forEach((siswa, index) => {
        let tr = document.createElement('tr');

        let tdNama = document.createElement('td');
        tdNama.textContent = siswa.nama;
        let tdTotal = document.createElement('td');
        tdTotal.textContent = siswa.terlambat_total;
        let tdAksi = document.createElement('td');

        // Tombol Reset
        let btnReset = document.createElement('button');
        btnReset.textContent = "Reset";
        btnReset.onclick = () => resetKeterlambatan(index);
        btnReset.classList.add('btn-danger');

        // Tombol Hapus
        let btnHapus = document.createElement('button');
        btnHapus.textContent = "Hapus";
        btnHapus.onclick = () => hapusKeterlambatan(index);
        btnHapus.classList.add('btn-danger');

        tdAksi.appendChild(btnReset);
        tdAksi.appendChild(btnHapus);

        tr.appendChild(tdNama);
        tr.appendChild(tdTotal);
        tr.appendChild(tdAksi);

        tbody.appendChild(tr);
      });
    }

    function resetKeterlambatan(index){
      if(!confirm(`Reset jumlah keterlambatan siswa: ${dataKeterlambatan[index].nama}?`)) return;
      let namaLower = dataKeterlambatan[index].nama.toLowerCase();
      // Reset keterlambatan_total di dataKeterlambatan
      dataKeterlambatan[index].terlambat_total = 0;
      // Reset keterlambatan_total di dataSiswa
      if(dataSiswa[dataKeterlambatan[index].nama]){
        dataSiswa[dataKeterlambatan[index].nama].terlambat_total = 0;
      }
      renderDataKeterlambatanTable();
      renderTerlambatTable();
      saveData();
      alert(`Jumlah keterlambatan siswa "${dataKeterlambatan[index].nama}" telah direset.`);
      renderHome(); // Update Home dashboard jika diperlukan
    }

    function hapusKeterlambatan(index){
      if(!confirm(`Hapus data keterlambatan siswa: ${dataKeterlambatan[index].nama}?`)) return;
      dataKeterlambatan.splice(index,1);
      renderDataKeterlambatanTable();
      renderTerlambatTable();
      saveData();
      alert("Data keterlambatan siswa dihapus dari Data Keterlambatan.");
      renderHome(); // Update Home dashboard jika diperlukan
    }

    function resetSemuaKeterlambatan(){
      if(!confirm("Yakin reset semua keterlambatan siswa?")) return;
      dataKeterlambatan.forEach(siswa => {
        siswa.terlambat_total = 0;
        if(dataSiswa[siswa.nama]){
          dataSiswa[siswa.nama].terlambat_total = 0;
        }
      });
      renderDataKeterlambatanTable();
      renderTerlambatTable();
      saveData();
      alert("Semua keterlambatan siswa telah direset.");
      renderHome(); // Update Home dashboard jika diperlukan
    }

    /********************************************************
     *      FILTER HISTORY
     ********************************************************/
    function filterHistoryByDate(){
      let date = document.getElementById('filterDate').value;
      if(!date){
        alert("Pilih tanggal terlebih dahulu!");
        return;
      }
      // Sertakan archiveAbsensi
      let combinedData = historyAbsensi.concat(archiveAbsensi);
      let filtered = combinedData.filter(item => item.tanggal === date);
      renderHistoryTable(filtered);
      displayFilterResult(`Tampilkan data absensi tanggal ${date}`, filtered.length);
    }

    function filterHistoryByDateRange(){
      let startDate = document.getElementById('filterStartDate').value;
      let endDate = document.getElementById('filterEndDate').value;
      if(!startDate || !endDate){
        alert("Pilih kedua tanggal (start & end)!");
        return;
      }
      if(startDate > endDate){
        alert("Tanggal mulai tidak boleh lebih besar dari tanggal akhir!");
        return;
      }
      let combinedData = historyAbsensi.concat(archiveAbsensi);
      let filtered = combinedData.filter(item => item.tanggal >= startDate && item.tanggal <= endDate);
      renderHistoryTable(filtered);
      displayFilterResult(`Tampilkan data absensi dari ${startDate} sampai ${endDate}`, filtered.length);
    }

    function resetHistoryFilters(){
      renderHistoryTable();
      if(document.getElementById('filterDate')){
        document.getElementById('filterDate').value = "";
      }
      if(document.getElementById('filterStartDate')){
        document.getElementById('filterStartDate').value = "";
      }
      if(document.getElementById('filterEndDate')){
        document.getElementById('filterEndDate').value = "";
      }
      if(document.getElementById('filterResult')){
        document.getElementById('filterResult').style.display = "none";
      }
    }

    function displayFilterResult(description, count){
      let d = document.getElementById('filterResult');
      d.style.display = "block";
      d.innerHTML = `<p>${description}: ${count} record ditemukan.</p>`;
    }

    /********************************************************
     *      STATISTIK: BAR CHART (SKALA 0–15)
     ********************************************************/
    function renderStatisticChart(){
      let option = document.getElementById('chartOption').value;
      let chartTitle = "";
      let labels = [];
      let values = [];
      let typeChart = 'bar'; // default

      // Mendapatkan bulan yang dipilih
      let statistikBulan = document.getElementById('statistikBulan').value;

      // Menggabungkan data dari historyAbsensi dan archiveAbsensi
      let combinedData = historyAbsensi.concat(archiveAbsensi);

      if(option === "siswa"){
        if(statistikBulan){
          chartTitle = `Siswa Paling Sering Telat untuk Bulan ${statistikBulan} (Maks 15)`;
          // Filter data berdasarkan bulan
          let filteredData = combinedData.filter(item => item.tanggal.startsWith(statistikBulan) && item.status === "late");
          // Hitung keterlambatan per siswa
          let siswaMap = {};
          filteredData.forEach(item => {
            let nama = item.nama;
            if(!siswaMap[nama]){
              siswaMap[nama] = 0;
            }
            siswaMap[nama]++;
          });
          // Konversi ke array dan sortir
          let arr = [];
          for(let nama in siswaMap){
            arr.push({label: nama, value: siswaMap[nama]});
          }
          arr.sort((a,b) => b.value - a.value);
          arr = arr.slice(0,15); // Maks 15
          labels = arr.map(x => x.label);
          values = arr.map(x => x.value);
        } else {
          chartTitle = "Siswa Paling Sering Telat (Maks 15)";
          // Hitung keterlambatan per siswa
          let siswaMap = {};
          combinedData.forEach(item => {
            if(item.status === "late"){
              let nama = item.nama;
              if(!siswaMap[nama]){
                siswaMap[nama] = 0;
              }
              siswaMap[nama]++;
            }
          });
          // Konversi ke array dan sortir
          let arr = [];
          for(let nama in siswaMap){
            arr.push({label: nama, value: siswaMap[nama]});
          }
          arr.sort((a,b) => b.value - a.value);
          arr = arr.slice(0,15); // Maks 15
          labels = arr.map(x => x.label);
          values = arr.map(x => x.value);
        }

      } else if(option === "alasan"){
        if(statistikBulan){
          chartTitle = `Alasan Keterlambatan Terbanyak untuk Bulan ${statistikBulan} (Maks 15)`;
          // Filter data berdasarkan bulan
          let filteredData = combinedData.filter(item => item.tanggal.startsWith(statistikBulan) && item.status === "late");
          // Hitung keterlambatan per alasan
          let alasanCount = {};
          filteredData.forEach(item => {
            let alasan = item.alasan;
            if(!alasanCount[alasan]){
              alasanCount[alasan] = 0;
            }
            alasanCount[alasan]++;
          });
          // Konversi ke array dan sortir
          let arr = [];
          for(let alasan in alasanCount){
            arr.push({label: alasan, value: alasanCount[alasan]});
          }
          arr.sort((a,b) => b.value - a.value);
          arr = arr.slice(0,15); // Maks 15
          labels = arr.map(x => x.label);
          values = arr.map(x => x.value);
        } else {
          chartTitle = "Alasan Keterlambatan Terbanyak (Maks 15)";
          // Hitung keterlambatan per alasan
          let alasanCount = {};
          combinedData.forEach(item => {
            if(item.status === "late"){
              let alasan = item.alasan;
              if(!alasanCount[alasan]){
                alasanCount[alasan] = 0;
              }
              alasanCount[alasan]++;
            }
          });
          // Konversi ke array dan sortir
          let arr = [];
          for(let alasan in alasanCount){
            arr.push({label: alasan, value: alasanCount[alasan]});
          }
          arr.sort((a,b) => b.value - a.value);
          arr = arr.slice(0,15); // Maks 15
          labels = arr.map(x => x.label);
          values = arr.map(x => x.value);
        }

      } else if(option === "bulanan"){
        chartTitle = "Grafik Keterlambatan Bulanan";
        typeChart = 'line';
        // Hitung keterlambatan per tanggal
        let latenessByDate = {};
        combinedData.forEach(item => {
          if(item.status === "late"){
            let tanggal = item.tanggal;
            if(statistikBulan && !tanggal.startsWith(statistikBulan)){
              return; // Lewati data yang tidak termasuk bulan yang dipilih
            }
            if(!latenessByDate[tanggal]){
              latenessByDate[tanggal] = 0;
            }
            latenessByDate[tanggal]++;
          }
        });
        // Sort tanggal
        labels = Object.keys(latenessByDate).sort();
        values = labels.map(date => latenessByDate[date]);
      }

      // Mendapatkan konteks canvas
      let ctx = document.getElementById('chartStatistic').getContext('2d');

      // Jika chart sudah ada, destroy untuk menghindari overlap
      if(chartObj) chartObj.destroy();
      if(attendanceLineChart) attendanceLineChart.destroy();

      // Menentukan jenis grafik
      if(option === "bulanan"){
        // Grafik Keterlambatan Bulanan
        attendanceLineChart = new Chart(ctx, {
          type: typeChart,
          data: {
            labels,
            datasets: [{
              label: chartTitle,
              data: values,
              backgroundColor: 'rgba(255,99,132,0.2)',
              borderColor: 'rgba(255,99,132,1)',
              borderWidth: 2,
              fill: true,
              tension: 0.3
            }]
          },
          options: {
            responsive:true,
            scales:{
              y:{
                beginAtZero:true,
                min:0,
                max:15,
                ticks:{
                  stepSize:1
                }
              }
            }
          }
        });
        // Tampilkan tombol download jika grafik bulanan
        document.getElementById('downloadSection').style.display = "block";
      } else {
        // Grafik Siswa atau Alasan
        chartObj = new Chart(ctx, {
          type: typeChart,
          data: {
            labels,
            datasets: [{
              label: chartTitle,
              data: values,
              backgroundColor: option === "siswa" ? 'rgba(54,162,235,0.6)' : 'rgba(255,206,86,0.6)',
              borderColor: option === "siswa" ? 'rgba(54,162,235,1)' : 'rgba(255,206,86,1)',
              borderWidth: 1
            }]
          },
          options: {
            responsive:true,
            scales:{
              y:{
                beginAtZero:true,
                min:0,
                max:15,
                ticks:{ stepSize:1 }
              }
            }
          }
        });
        document.getElementById('downloadSection').style.display = "none";
      }
    }

    /********************************************************
     *      IMPORT DATA SISWA (Excel)
     ********************************************************/
    function importDataSiswa(){
      let fileInput = document.getElementById('importFileSiswa');
      let file = fileInput.files[0];
      if(!file){
        alert("Silakan pilih file Excel terlebih dahulu.");
        return;
      }

      let reader = new FileReader();
      reader.onload = function(e){
        let data = new Uint8Array(e.target.result);
        let workbook = XLSX.read(data, {type: 'array'});

        // Asumsikan data siswa berada di sheet pertama
        let firstSheetName = workbook.SheetNames[0];
        let worksheet = workbook.Sheets[firstSheetName];
        let jsonData = XLSX.utils.sheet_to_json(worksheet, {header:1});

        // Parsing data
        // Asumsikan baris pertama adalah header: ["Nama", "Kelas", "NIS", "WaliKelas"]
        if(jsonData.length < 2){
          alert("File Excel tidak memiliki data yang cukup.");
          return;
        }

        let headers = jsonData[0];
        let requiredHeaders = ["Nama", "Kelas", "NIS", "WaliKelas"];
        let isValid = requiredHeaders.every((header, index) => header === jsonData[0][index]);
        if(!isValid){
          alert("Format header Excel tidak sesuai. Harus: " + requiredHeaders.join(", "));
          return;
        }

        // Mulai dari baris kedua
        let importedCount = 0;
        for(let i=1; i<jsonData.length; i++){
          let row = jsonData[i];
          let [nama, kelas, nis, waliKelas] = row;

          if(!nama || !kelas || !nis || !waliKelas){
            // Lewati baris yang tidak lengkap
            continue;
          }

          // Cek duplikat di databaseSiswa
          let dupe = databaseSiswa.some(s => s.nama.toLowerCase() === nama.toLowerCase() || s.nis === nis);
          if(dupe){
            // Lewati duplikat
            continue;
          }

          // Tambah siswa
          databaseSiswa.push({nama, kelas, nis, waliKelas});
          importedCount++;
        }

        if(importedCount > 0){
          renderDataSiswaTable();
          renderDataSiswaList(); // Update datalist untuk autocomplete
          saveData();
          alert(`${importedCount} data siswa berhasil diimpor.`);
        } else {
          alert("Tidak ada data siswa yang diimpor. Pastikan file tidak kosong atau data sudah ada di database.");
        }

        // Reset input file
        fileInput.value = "";
      };
      reader.onerror = function(){
        alert("Terjadi kesalahan saat membaca file.");
      };
      reader.readAsArrayBuffer(file);
    }

    /********************************************************
     *      DOWNLOAD DATA BULANAN (Excel)
     ********************************************************/
    function downloadMonthlyData(){
      let month = prompt("Masukkan bulan yang ingin di-download (YYYY-MM):", "2025-01");
      if(!month){
        alert("Bulan tidak boleh kosong!");
        return;
      }

      // Filter data untuk bulan tersebut dari archiveAbsensi dan historyAbsensi
      let filtered = archiveAbsensi.filter(item => item.tanggal.startsWith(month)) // pastikan format tanggal adalah YYYY-MM-DD
                    .concat(historyAbsensi.filter(item => item.tanggal.startsWith(month)));
      
      // Hanya ambil data yang statusnya 'late', 'izin', atau 'dipulangkan'
      if(filtered.length === 0){
        alert("Tidak ada data absensi untuk bulan tersebut.");
        return;
      }

      // Buat worksheet
      let ws_data = [["Nama Siswa", "Tanggal Absensi", "Status", "Alasan", "Kelas", "Wali Kelas"]];
      filtered.forEach(item => {
        // Cari kelas dan wali kelas dari databaseSiswa
        let siswa = databaseSiswa.find(s => s.nama.toLowerCase() === item.nama.toLowerCase());
        let kelas = siswa ? siswa.kelas : "-";
        let waliKelas = siswa ? siswa.waliKelas : "-";
        ws_data.push([item.nama, item.tanggal, capitalizeFirstLetter(item.status), item.alasan, kelas, waliKelas]);
      });

      let ws = XLSX.utils.aoa_to_sheet(ws_data);
      let wb = XLSX.utils.book_new();
      XLSX.utils.book_append_sheet(wb, ws, "Absensi");

      // Download
      XLSX.writeFile(wb, `Absensi_${month}.xlsx`);
    }

    // Helper function untuk mengkapitalisasi huruf pertama
    function capitalizeFirstLetter(string) {
      return string.charAt(0).toUpperCase() + string.slice(1);
    }

    /********************************************************
     *               PROFILE
     ********************************************************/
    function updateProfileInfo(){
      document.getElementById('profileAbsensiCount').textContent=historyAbsensi.length + archiveAbsensi.length;
    }

    // Tampilkan data guru di table, diurut hari senin-sabtu
    function renderProfileGuru(){
      let sorted = [...petugasGuru];
      // Sort by day index
      sorted.sort((a,b)=> dayOrder.indexOf(a.hari) - dayOrder.indexOf(b.hari));

      let tbody=document.getElementById('profileGuruTbody');
      tbody.innerHTML="";
      sorted.forEach((item, index)=>{
        let tr=document.createElement('tr');
        let tdHari=document.createElement('td');
        tdHari.textContent=item.hari;
        let tdNama=document.createElement('td');
        tdNama.textContent=item.nama;
        let tdAksi=document.createElement('td');

        // Tombol Edit di Profile
        let btnEdit = document.createElement('button');
        btnEdit.textContent = "Edit";
        btnEdit.onclick = () => openEditGuruModal(index);
        btnEdit.classList.add('btn-secondary');

        // Tombol Hapus di Profile
        let btnHapus = document.createElement('button');
        btnHapus.textContent = "Hapus";
        btnHapus.classList.add('btn-danger');
        btnHapus.onclick = () => hapusPetugasGuru(index);

        tdAksi.appendChild(btnEdit);
        tdAksi.appendChild(btnHapus);

        tr.appendChild(tdHari);
        tr.appendChild(tdNama);
        tr.appendChild(tdAksi);
        tbody.appendChild(tr);
      });
    }
    // Tampilkan petugasSiswa di <ul>
    function renderProfileSiswa(){
      let ul = document.getElementById('profileSiswaList');
      ul.innerHTML="";
      petugasSiswa.forEach(s=>{
        let li=document.createElement('li');
        li.textContent=s;
        ul.appendChild(li);
      });
    }

    // Menampilkan dropdown Pengisi => hanya guru sesuai hari aktual
    function renderProfilePengisiToday(){
      let dayName = getTodayDayName(); // ex "Selasa"
      document.getElementById('hariAktualSpan').textContent= dayName || "(Minggu)";

      let dropdown=document.getElementById('dropdownPengisi');
      dropdown.innerHTML="";

      if(!dayName){
        // Minggu atau hari lain yang tidak piket
        let opt=document.createElement('option');
        opt.value="";
        opt.textContent="(Hari ini libur/Minggu)";
        dropdown.appendChild(opt);
      } else {
        // filter petugasGuru => item.hari==dayName
        let todayGuru = petugasGuru.filter(g=>g.hari===dayName);
        if(todayGuru.length===0){
          let opt=document.createElement('option');
          opt.value="";
          opt.textContent="(Tidak ada guru piket hari ini)";
          dropdown.appendChild(opt);
        } else {
          todayGuru.forEach(g=>{
            let opt=document.createElement('option');
            opt.value=g.nama; 
            opt.textContent=g.nama + ` (${g.hari})`;
            dropdown.appendChild(opt);
          });
        }
      }
      // Set selected option to currentPengisi if sesuai hari
      if(currentPengisi){
        let options = dropdown.options;
        for(let i=0; i<options.length; i++){
          if(options[i].value === currentPengisi){
            dropdown.selectedIndex = i;
            break;
          }
        }
      }

      // Tampilkan currentPengisi di <span>
      document.getElementById('currentPengisiSpan').textContent = currentPengisi || "-";
    }

    function setPengisi(){
      let dd=document.getElementById('dropdownPengisi');
      let val=dd.value;
      currentPengisi=val;
      document.getElementById('currentPengisiSpan').textContent= currentPengisi||"-";
      saveData();
      alert("Pengisi saat ini => "+currentPengisi);
      renderHome(); // Update Home dashboard jika diperlukan
    }

    // Return dayName from system date => dayOrder atau ""
    function getTodayDayName(){
      let d=new Date();
      let dayNum=d.getDay(); // 0=Sun,1=Mon,2=Tue,...6=Sat
      if(dayNum===0||dayNum>6)return "";
      return dayOrder[dayNum-1]||"";
    }

    /********************************************************
     *      FUNGSI RENDER TERLAMBAT TABLE (PERBAIKI ERROR)
     ********************************************************/
    function renderTerlambatTable(){
      // Fungsi ini hanya alias untuk renderDataKeterlambatanTable
      renderDataKeterlambatanTable();
    }

    /********************************************************
     *      DASHBOARD: RENDER DATA (TERINTEGRASI KE HOME)
     ********************************************************/
    function renderHome(){
      // 1. Grafik Kehadiran Bulanan
      renderAttendanceLineChart();

      // 2. Top 5 Siswa dengan Keterlambatan Terbanyak
      renderTop5LateStudents();
    }

    function renderAttendanceLineChart(){
      // Ambil bulan terakhir dari data
      let combinedData = historyAbsensi.concat(archiveAbsensi);
      if(combinedData.length === 0){
        // Tidak ada data absensi untuk membuat grafik
        let ctx = document.getElementById('chartAttendanceLine').getContext('2d');
        ctx.clearRect(0, 0, ctx.canvas.width, ctx.canvas.height);
        return;
      }
      // Tentukan bulan terakhir berdasarkan data
      let latestDate = combinedData.reduce((max, item) => item.tanggal > max ? item.tanggal : max, combinedData[0].tanggal);
      let latestMonth = latestDate.slice(0,7); // YYYY-MM

      // Ambil data untuk bulan terakhir
      let monthlyData = combinedData.filter(item => item.tanggal.startsWith(latestMonth));

      // Hitung kehadiran per hari
      let attendanceByDate = {};
      monthlyData.forEach(item => {
        let date = item.tanggal;
        if(!attendanceByDate[date]){
          attendanceByDate[date] = new Set();
        }
        // Anggap setiap status selain 'absent' sebagai hadir
        if(item.status !== "absent"){
          attendanceByDate[date].add(item.nama);
        }
      });

      // Buat array tanggal dan jumlah hadir
      let dates = [];
      let counts = [];
      let daysInMonth = new Date(latestMonth.slice(0,4), latestMonth.slice(5,7), 0).getDate();
      for(let day=1; day<=daysInMonth; day++){
        let dateStr = `${latestMonth}-${day.toString().padStart(2,'0')}`;
        dates.push(dateStr);
        counts.push(attendanceByDate[dateStr] ? attendanceByDate[dateStr].size : 0);
      }

      // Render chart
      let ctx = document.getElementById('chartAttendanceLine').getContext('2d');

      // Destroy existing chart if any
      if(attendanceLineChart) attendanceLineChart.destroy();

      attendanceLineChart = new Chart(ctx, {
        type: 'line',
        data: {
          labels: dates,
          datasets: [{
            label: `Jumlah Kehadiran pada Bulan ${latestMonth}`,
            data: counts,
            backgroundColor: 'rgba(46, 204, 113, 0.2)',
            borderColor: 'rgba(46, 204, 113, 1)',
            borderWidth: 2,
            fill: true,
            tension: 0.3
          }]
        },
        options: {
          responsive: true,
          scales: {
            y: {
              beginAtZero: true,
              suggestedMax: 30,
              ticks: {
                stepSize: 5
              }
            }
          }
        }
      });
    }

    function renderTop5LateStudents(){
      let tbody = document.getElementById('dashboardTop5TableBody');
      tbody.innerHTML = "";

      // Sort dataKeterlambatan descending
      let sortedKeterlambatan = [...dataKeterlambatan].sort((a,b) => b.terlambat_total - a.terlambat_total);
      let top5 = sortedKeterlambatan.slice(0,5);

      top5.forEach((siswa, index) => {
        let tr = document.createElement('tr');

        let tdRank = document.createElement('td');
        tdRank.textContent = index + 1;
        tdRank.style.fontWeight = 'bold';

        let tdNama = document.createElement('td');
        tdNama.textContent = siswa.nama;

        let tdTotal = document.createElement('td');
        tdTotal.textContent = siswa.terlambat_total;

        tr.appendChild(tdRank);
        tr.appendChild(tdNama);
        tr.appendChild(tdTotal);

        tbody.appendChild(tr);
      });
    }

    /********************************************************
     *      INIT 
     ********************************************************/
    function init(){
      loadData();
      archiveWeeklyData();
      recalcDataSiswaFromHistory();
      renderHistoryTable();
      renderDataKeterlambatanTable();
      renderTerlambatTable();
      renderPetugasGuruTable();
      renderDataSiswaTable();
      renderDataSiswaList(); // Render datalist untuk autocomplete
      renderProfileGuru();
      renderProfileSiswa();
      renderProfilePengisiToday();
      updateProfileInfo();
      renderHome(); // Render Home dashboard saat inisialisasi
    }

    /********************************************************
     *       FUNGSI ARCHIVE DATA MINGGUAN SECARA OTOMATIS
     ********************************************************/
    // Memanggil init saat halaman dimuat
    window.onload = init;

    /********************************************************
     *      FUNGSI HAPUS SEMUA & HAPUS PILIH SISWA DI DATA SISWA SCREEN
     ********************************************************/
    function hapusSemuaSiswa(){
      if(!confirm("Apakah Anda yakin ingin menghapus semua data siswa?")) return;
      // Hapus semua siswa dari semua data
      databaseSiswa = [];
      petugasSiswa = [];
      historyAbsensi = [];
      archiveAbsensi = [];
      dataKeterlambatan = [];
      recalcDataSiswaFromHistory();
      renderHistoryTable();
      renderDataKeterlambatanTable();
      renderTerlambatTable();
      renderDataSiswaTable();
      renderProfileSiswa();
      renderDataSiswaList(); // Update datalist untuk autocomplete
      saveData();
      renderHome(); // Update Home dashboard jika diperlukan
      alert("Semua data siswa telah dihapus!");
    }

    function hapusPilihSiswa(){
      // Dapatkan semua checkbox yang dipilih
      let checkboxes = document.querySelectorAll('.select-siswa:checked');
      if(checkboxes.length === 0){
        alert("Pilih minimal satu siswa untuk dihapus.");
        return;
      }
      if(!confirm(`Apakah Anda yakin ingin menghapus ${checkboxes.length} siswa yang dipilih?`)) return;

      // Ambil indeks siswa yang dipilih dan urutkan dari yang terbesar untuk menghindari masalah saat splice
      let indices = Array.from(checkboxes).map(cb => parseInt(cb.value)).sort((a,b) => b - a);
      let deletedNames = [];

      indices.forEach(index => {
        let siswa = databaseSiswa[index];
        if(siswa){
          let namaLower = siswa.nama.toLowerCase();
          // Hapus dari historyAbsensi dan archiveAbsensi
          historyAbsensi = historyAbsensi.filter(i => i.nama.toLowerCase() !== namaLower);
          archiveAbsensi = archiveAbsensi.filter(i => i.nama.toLowerCase() !== namaLower);
          // Hapus dari dataKeterlambatan
          dataKeterlambatan = dataKeterlambatan.filter(k => k.nama.toLowerCase() !== namaLower);
          // Hapus dari petugasSiswa jika ada
          petugasSiswa = petugasSiswa.filter(s => s.toLowerCase() !== namaLower);
          // Hapus dari databaseSiswa
          deletedNames.push(siswa.nama);
          databaseSiswa.splice(index,1);
        }
      });

      renderDataSiswaList(); // Update datalist untuk autocomplete
      recalcDataSiswaFromHistory();
      renderHistoryTable();
      renderDataKeterlambatanTable();
      renderTerlambatTable();
      renderDataSiswaTable();
      renderProfileSiswa();
      saveData();
      renderHome(); // Update Home dashboard jika diperlukan
      alert(`Data siswa yang dihapus: ${deletedNames.join(", ")}`);
    }

    function toggleSelectAll(source){
      let checkboxes = document.querySelectorAll('.select-siswa');
      checkboxes.forEach(cb => cb.checked = source.checked);
    }
  </script>
</body>
</html>
