<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuis Pengetahuan Umum Dasar</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            display: flex;
            justify-content: center;
            align-items: center;
            min-height: 100vh;
            background-color: #f0f0f0;
            margin: 0;
            padding: 20px;
            box-sizing: border-box;
        }

        .quiz-container {
            background-color: white;
            padding: 30px;
            border-radius: 10px;
            box-shadow: 0 0 15px rgba(0, 0, 0, 0.2);
            width: 100%;
            max-width: 600px;
            text-align: center;
        }

        h1 {
            color: #333;
            margin-bottom: 10px;
        }

        #completion-message {
            color: #28a745;
            font-size: 1.2em;
            font-weight: bold;
            margin-top: 5px;
            margin-bottom: 20px;
        }

        .question-counter-text {
            font-size: 0.9em;
            color: #666;
            margin-bottom: 20px;
        }

        #question-container {
            margin-bottom: 20px;
        }

        #question {
            font-size: 1.5em;
            font-weight: bold;
            margin-bottom: 25px;
            color: #444;
        }

        .btn-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 20px;
        }

        .btn {
            background-color: #007bff;
            color: white;
            border: none;
            padding: 12px 15px;
            border-radius: 5px;
            cursor: pointer;
            font-size: 1em;
            transition: background-color 0.2s ease, box-shadow 0.2s ease;
            word-wrap: break-word;
            min-height: 50px;
            display: flex;
            align-items: center;
            justify-content: center;
            outline: none;
            font-weight: bold;
        }

        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) { background-color: #007bff; }
        .btn:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):hover {}
        .btn:not([disabled]):not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q):focus:hover {
            background-color: #007bff;
            box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        }

        .btn.correct { background-color: #28a745 !important; box-shadow: none; }
        .btn.correct:hover { background-color: #218838 !important; }
        .btn.correct:focus {
            background-color: #28a745 !important;
            box-shadow: 0 0 0 3px rgba(40, 167, 69, 0.6) !important;
        }

        .btn.wrong { background-color: #dc3545 !important; box-shadow: none; }
        .btn.wrong:hover { background-color: #c82333 !important; }
        .btn.wrong:focus {
            background-color: #dc3545 !important;
            box-shadow: 0 0 0 3px rgba(220, 53, 69, 0.6) !important;
        }

        .btn:disabled {
            cursor: not-allowed;
            opacity: 0.65;
        }
        /* Adjusted to not conflict with new button's disabled state if it's not a skip-btn or answer btn */
        .btn:disabled:not(.correct):not(.wrong):not(.skip-btn):not(.btn-prev-q) {
            background-color: #6c757d !important;
            color: #ccc !important;
        }


        .controls {
            display: flex;
            justify-content: center;
            gap: 10px;
        }

        #skip-navigation-controls {
            justify-content: space-between; /* Adjusted to space-around or similar if needed for 3 buttons */
            margin-top: 40px;
            margin-bottom: 10px;
        }

        .skip-btn { /* This style is for prev-50 and next-50 */
            background-color: #28a745; /* Green */
            color: white;
            padding: 8px 12px;
            font-size: 0.9em;
            min-width: 80px; /* Ensures same width for all skip-type buttons */
        }
        .skip-btn:hover {
            background-color: #218838; /* Darker Green */
            color: white;
        }
        .skip-btn:disabled { /* Default disabled for green skip buttons */
            background-color: #a3d8b0 !important;
            color: #e9f5ec !important;
            /* cursor: not-allowed; is inherited from .btn:disabled */
            /* opacity: 0.65; is inherited from .btn:disabled */
        }

        /* New button style for "Previous Question" */
        .btn-prev-q {
            background-color: #5F9EA0; /* CadetBlue - "biru terang" */
            color: white; /* Text color */
            padding: 8px 12px; /* Same padding as skip-btn */
            font-size: 0.9em; /* Same font size as skip-btn */
            min-width: 80px; /* Same min-width as skip-btn */
        }
        .btn-prev-q:hover:not([disabled]) {
            background-color: #4682B4; /* SteelBlue - darker for hover */
            color: white;
        }
        .btn-prev-q:disabled {
            background-color: #B0C4DE !important; /* LightSteelBlue - for disabled state */
            color: #666666 !important; /* Darker text for readability on light blue */
            /* opacity will be applied by .btn:disabled */
        }


        .hide { display: none !important; }
    </style>
</head>
<body>
    <div class="quiz-container">
        <h1>Pengetahuan Umum Dasar</h1>
        <p id="completion-message" class="hide">Selamat Kuis Sudah Selesai 🎉</p>
        <div id="initial-controls" class="controls">
            <button id="start-btn" class="btn">Mulai</button>
            <button id="continue-btn" class="btn hide">Lanjutkan</button>
        </div>
        <div id="question-counter" class="question-counter-text hide">0/0</div>
        <div id="question-container" class="hide">
            <div id="question">Kata Bahasa Inggris</div>
            <div id="answer-buttons" class="btn-grid">
            </div>
            <div id="skip-navigation-controls" class="controls hide">
                <button id="prev-50-btn" class="btn skip-btn">&laquo; 50</button>
                <button id="prev-question-btn" class="btn btn-prev-q">&lt;</button> <button id="next-50-btn" class="btn skip-btn">50 &raquo;</button>
            </div>
        </div>
    </div>

    <script>
        const startButton = document.getElementById('start-btn');
        const continueButton = document.getElementById('continue-btn');
        const initialControls = document.getElementById('initial-controls');
        const completionMessageElement = document.getElementById('completion-message');
        const questionContainerElement = document.getElementById('question-container');
        const questionElement = document.getElementById('question');
        const answerButtonsElement = document.getElementById('answer-buttons');
        const questionCounterElement = document.getElementById('question-counter');

        const skipNavigationControls = document.getElementById('skip-navigation-controls');
        const prev50Button = document.getElementById('prev-50-btn');
        const prevQuestionButton = document.getElementById('prev-question-btn'); // Referensi untuk tombol baru
        const next50Button = document.getElementById('next-50-btn');
        const JUMP_AMOUNT = 50;

        let orderedQuestions, currentQuestionIndex;
        let score = 0;
        let questionTimeout;

        // Daftar kata mentah dari PDF (Inggris: Indonesia) - Total 1580 kata
        const rawVocabularyList = [


  { "en": "Apa Inti Dari Persamaan Maxwell?", "id": "Hukum Dasar Elektromagnetisme Klasik." },
  { "en": "Apa Itu Gelombang Elektromagnetik?", "id": "Getaran Medan Listrik Dan Magnet." },
  { "en": "Apa Itu Vektor Poynting?", "id": "Mewakili Arah Dan Besaran Aliran Energi." },
  { "en": "Perbedaan Sirkuit Listrik Dan Magnetik?", "id": "Arus Mengalir Di Sirkuit Listrik." },
  { "en": "Apa Itu Konstanta Dielektrik?", "id": "Kemampuan Bahan Menyimpan Energi Listrik." },
  { "en": "Apa Itu Faktor Disipasi (Tan Delta)?", "id": "Ukuran Ketidakefisienan Bahan Dielektrik." },
  { "en": "Apa Itu Bahan Piezoelektrik?", "id": "Menghasilkan Tegangan Saat Diberi Tekanan Mekanis." },
  { "en": "Apa Itu Bahan Termoelektrik?", "id": "Mengonversi Panas Menjadi Listrik Langsung." },
  { "en": "Apa Itu Stepped Leader Petir?", "id": "Saluran Awal Bercabang Turun Dari Awan." },
  { "en": "Apa Itu Dart Leader Petir?", "id": "Saluran Lanjutan Setelah Sambaran Pertama." },
  { "en": "Apa Itu Fenomena Ferroresonansi?", "id": "Osilasi Tegangan Tinggi Non-Linier." },
  { "en": "Apa Itu Otomatisasi Feeder (Penyulang)?", "id": "Penggunaan Kontrol Otomatis Pada Jaringan Distribusi." },
  { "en": "Apa Kepanjangan ACR (Automatic Circuit Recloser)?", "id": "Automatic Circuit Recloser." },
  { "en": "Apa Itu Faktor Pitch (Pitch Factor)?", "id": "Rasio Tegangan Induksi Kumparan Pendek." },
  { "en": "Apa Itu Faktor Distribusi (Distribution Factor)?", "id": "Rasio Tegangan Induksi Belitan Terdistribusi." },
  { "en": "Apa Itu Kurva-V Motor Sinkron?", "id": "Grafik Arus Jangkar Terhadap Arus Medan." },
  { "en": "Bagaimana Mencegah Hunting Motor Sinkron?", "id": "Menggunakan Belitan Peredam (Damper Winding)." },
  { "en": "Apa Itu Setting Time Dial Relay?", "id": "Pengaturan Waktu Tunda Pada Relay Elektromekanis." },
  { "en": "Apa Kepanjangan CTI (Coordination Time Interval)?", "id": "Coordination Time Interval." },
  { "en": "Apa Itu Nilai Pickup Relay?", "id": "Nilai Minimum Arus/Tegangan Untuk Operasi." },
  { "en": "Apa Itu Nilai Reset Relay?", "id": "Nilai Dimana Kontak Relay Kembali Normal." },
  { "en": "Apa Itu Konverter Back-to-Back?", "id": "Dua Konverter Terhubung Melalui DC (Direct Current) Link." },
  { "en": "Apa Itu Jembatan Wheatstone?", "id": "Sirkuit Untuk Mengukur Resistansi Tidak Dikenal." },
  { "en": "Apa Itu Jembatan Kelvin?", "id": "Modifikasi Jembatan Wheatstone Untuk Resistansi Rendah." },
  { "en": "Apa Kepanjangan DMM (Digital Multimeter)?", "id": "Digital Multimeter." },
  { "en": "Apa Itu Audit Energi?", "id": "Inspeksi Dan Analisis Penggunaan Energi." },
  { "en": "Apa Itu Diagram Sankey?", "id": "Diagram Visualisasi Aliran Energi." },
  { "en": "Apa Itu Profil Beban (Load Profile)?", "id": "Grafik Variasi Permintaan Listrik Terhadap Waktu." },
  { "en": "Apa Itu Teknik Peak Clipping?", "id": "Mengurangi Permintaan Listrik Selama Beban Puncak." },
  { "en": "Apa Itu Teknik Load Shifting?", "id": "Memindahkan Penggunaan Listrik Ke Waktu Off-Peak." },
  { "en": "Apa Itu Model Purdue?", "id": "Model Arsitektur Untuk Sistem Kontrol Industri." },
  { "en": "Apa Kepanjangan ICS (Industrial Control System)?", "id": "Industrial Control System." },
  { "en": "Apa Itu Segmentasi Jaringan?", "id": "Memisahkan Jaringan Menjadi Sub-jaringan Terisolasi." },
  { "en": "Apa Kepanjangan DNP3 (Distributed Network Protocol 3)?", "id": "Distributed Network Protocol 3." },
  { "en": "Apa Itu Autentikasi Aman DNP3 (Distributed Network Protocol 3)?", "id": "Mekanisme Keamanan Untuk Protokol SCADA (Supervisory Control And Data Acquisition)." },
  { "en": "Apa Itu Konduktansi Kulit (Skin Conductance)?", "id": "Ukuran Konduktivitas Listrik Kulit." },
  { "en": "Apa Itu Potensial Aksi?", "id": "Sinyal Listrik Singkat Di Sel Saraf." },
  { "en": "Apa Kepanjangan EEG (Electroencephalography)?", "id": "Electroencephalography." },
  { "en": "Apa Yang Diukur EEG (Electroencephalography)?", "id": "Aktivitas Listrik Di Otak." },
  { "en": "Apa Kepanjangan EKG (Electrocardiography)?", "id": "Electrocardiography." },
  { "en": "Apa Yang Diukur EKG (Electrocardiography)?", "id": "Aktivitas Listrik Jantung." },
  { "en": "Apa Itu Defibrilator?", "id": "Alat Yang Memberikan Kejutan Listrik Jantung." },
  { "en": "Apa Itu Electrosurgery?", "id": "Penggunaan Arus Frekuensi Tinggi Untuk Memotong Jaringan." },
  { "en": "Apa Itu Stimulasi Saraf Listrik Transkutan (TENS)?", "id": "Metode Terapi Nyeri Menggunakan Listrik." },
  { "en": "Apa Kepanjangan TENS (Transcutaneous Electrical Nerve Stimulation)?", "id": "Transcutaneous Electrical Nerve Stimulation." },
  { "en": "Apa Itu Impedansi Bioelektrik?", "id": "Hambatan Jaringan Biologis Terhadap Arus Listrik." },
  { "en": "Apa Itu Tomografi Impedansi Listrik (EIT)?", "id": "Teknik Pencitraan Medis Menggunakan Impedansi." },
  { "en": "Apa Kepanjangan EIT (Electrical Impedance Tomography)?", "id": "Electrical Impedance Tomography." },
  { "en": "Apa Itu Iontophoresis?", "id": "Memasukkan Obat Melalui Kulit Menggunakan Arus." },
  { "en": "Apa Itu Terapi Elektrokonvulsif (ECT)?", "id": "Prosedur Medis Menggunakan Kejang Listrik Terkontrol." },
  { "en": "Apa Itu Efek Magnetobiologis?", "id": "Efek Medan Magnet Pada Organisme Hidup." },
  { "en": "Apa Itu Terapi Medan Elektromagnetik Berdenyut (PEMF)?", "id": "Terapi Menggunakan Medan Magnet Berdenyut." },
  { "en": "Apa Kepanjangan PEMF (Pulsed Electromagnetic Field)?", "id": "Pulsed Electromagnetic Field." },
  { "en": "Apa Itu Stimulasi Magnetik Transkranial (TMS)?", "id": "Menstimulasi Otak Menggunakan Medan Magnet." },
  { "en": "Apa Kepanjangan TMS (Transcranial Magnetic Stimulation)?", "id": "Transcranial Magnetic Stimulation." },
  { "en": "Apa Itu Koil Helmholtz?", "id": "Perangkat Untuk Menghasilkan Medan Magnet Seragam." },
  { "en": "Apa Itu Efek Faraday?", "id": "Rotasi Polarisasi Cahaya Oleh Medan Magnet." },
  { "en": "Apa Itu Efek Kerr?", "id": "Perubahan Indeks Bias Akibat Medan Listrik." },
  { "en": "Apa Itu Efek Pockels?", "id": "Efek Kerr Linier Pada Material Tertentu." },
  { "en": "Apa Itu Superkapasitor?", "id": "Kapasitor Dengan Kapasitansi Sangat Tinggi." },
  { "en": "Apa Nama Lain Superkapasitor?", "id": "Ultracapacitor Atau EDLC (Electric Double-Layer Capacitor)." },
  { "en": "Apa Itu Memristor?", "id": "Komponen Pasif Keempat Setelah Resistor-Kapasitor-Induktor." },
  { "en": "Apa Karakteristik Memristor?", "id": "Resistansinya Tergantung Sejarah Arus." },
  { "en": "Apa Itu Spintronik?", "id": "Elektronik Yang Memanfaatkan Spin Elektron." },
  { "en": "Apa Itu Efek Tunnel Magnetoresistance (TMR)?", "id": "Efek Kuantum Untuk Sensor Magnetik." },
  { "en": "Apa Kepanjangan TMR (Tunnel Magnetoresistance)?", "id": "Tunnel Magnetoresistance." },
  { "en": "Apa Itu Graphene?", "id": "Lapisan Tunggal Atom Karbon." },
  { "en": "Apa Sifat Listrik Graphene?", "id": "Konduktivitas Sangat Tinggi." },
  { "en": "Apa Itu Tabung Nano Karbon (Carbon Nanotube)?", "id": "Struktur Silinder Dari Atom Karbon." },
  { "en": "Apa Itu Titik Kuantum (Quantum Dot)?", "id": "Nanokristal Semikonduktor Dengan Sifat Optik." },
  { "en": "Apa Itu Metamaterial?", "id": "Material Rekayasa Dengan Sifat Tidak Biasa." },
  { "en": "Apa Itu Indeks Bias Negatif?", "id": "Sifat Optik Unik Dari Metamaterial." },
  { "en": "Apa Itu Transfer Energi Nirkabel?", "id": "Mengirimkan Daya Listrik Tanpa Kabel." },
  { "en": "Apa Itu Kopling Induktif Resonan?", "id": "Metode Efisien Untuk Transfer Energi Nirkabel." },
  { "en": "Apa Itu Energi Harvesting?", "id": "Memanen Energi Dari Lingkungan Sekitar." },
  { "en": "Apa Contoh Energi Harvesting?", "id": "Piezoelektrik, Termoelektrik, Surya." },
  { "en": "Apa Itu Piezoelectric Nanogenerator (PENG)?", "id": "Memanen Energi Mekanis Skala Nano." },
  { "en": "Apa Itu Triboelectric Nanogenerator (TENG)?", "id": "Memanen Energi Listrik Statis." },
  { "en": "Apa Itu Komputasi Kuantum?", "id": "Komputasi Menggunakan Prinsip Mekanika Kuantum." },
  { "en": "Apa Itu Qubit?", "id": "Unit Dasar Informasi Kuantum." },
  { "en": "Apa Itu Superposisi Kuantum?", "id": "Kemampuan Qubit Berada Dalam Beberapa Keadaan." },
  { "en": "Apa Itu Keterkaitan Kuantum (Quantum Entanglement)?", "id": "Keterkaitan Nasib Dua Qubit Atau Lebih." },
  { "en": "Apa Itu Kriogenik?", "id": "Studi Tentang Suhu Sangat Rendah." },
  { "en": "Apa Itu Magnet Superkonduktor?", "id": "Elektromagnet Menggunakan Kumparan Superkonduktor." },
  { "en": "Apa Kepanjangan MRI (Magnetic Resonance Imaging)?", "id": "Magnetic Resonance Imaging." },
  { "en": "Dimana Magnet Superkonduktor Digunakan?", "id": "Mesin MRI (Magnetic Resonance Imaging), Akselerator Partikel." },
  { "en": "Apa Itu SQUID (Superconducting Quantum Interference Device)?", "id": "Sensor Medan Magnet Paling Sensitif." },
  { "en": "Apa Itu Efek Josephson?", "id": "Fenomena Arus Superkonduktor Melintasi Sambungan." },
  { "en": "Apa Itu Standar Tegangan Josephson?", "id": "Standar Definitif Untuk Satuan Volt." },
  { "en": "Apa Itu Efek Hall Kuantum?", "id": "Versi Mekanika Kuantum Dari Efek Hall." },
  { "en": "Apa Itu Standar Resistansi Von Klitzing?", "id": "Standar Definitif Untuk Satuan Ohm." },
  { "en": "Apa Itu Elektron Tunggal?", "id": "Komponen Elektronik Yang Mengontrol Elektron Individual." },
  { "en": "Apa Itu Blokade Coulomb?", "id": "Peningkatan Resistansi Pada Persimpangan Terowongan Kecil." },
  { "en": "Apa Itu Pompa Elektron?", "id": "Perangkat Yang Mentransfer Elektron Satu Per Satu." },
  { "en": "Apa Itu Standar Arus Kuantum?", "id": "Mendefinisikan Satuan Ampere Berdasarkan Muatan Elektron." },
  { "en": "Apa Itu Kontrol Berorientasi Medan (FOC)?", "id": "Kontrol Presisi Torsi Dan Fluks Motor." },
  { "en": "Apa Kepanjangan FOC (Field-Oriented Control)?", "id": "Field-Oriented Control." },
  { "en": "Apa Perbedaan DTC Dan FOC?", "id": "DTC (Direct Torque Control) Respon Torsi Lebih Cepat." },
  { "en": "Apa Mekanisme On-Load Tap Changer (OLTC)?", "id": "Mengubah Tap Tanpa Memutus Beban Trafo." },
  { "en": "Apa Keuntungan Trafo Tipe Kering?", "id": "Risiko Kebakaran Rendah Dan Bebas Perawatan." },
  { "en": "Apa Itu Transformator Cast Resin?", "id": "Belitan Dilapisi Resin Epoksi Padat." },
  { "en": "Apa Itu Tembaga Lapis Baja?", "id": "Baja Dengan Lapisan Luar Tembaga." },
  { "en": "Apa Kapasitas Termal Konduktor?", "id": "Kemampuan Menahan Panas Arus Gangguan." },
  { "en": "Apa Itu Perhitungan Tegangan Tarik Kabel?", "id": "Menghitung Gaya Maksimum Saat Menarik Kabel." },
  { "en": "Apa Itu Tekanan Dinding Samping?", "id": "Tekanan Kabel Pada Dinding Pipa Tikungan." },
  { "en": "Apa Aturan Radius Tekuk Minimum?", "id": "Radius Terkecil Kabel Boleh Dibengkokkan." },
  { "en": "Apa Itu Torsi Redaman (Damping Torque)?", "id": "Torsi Yang Meredam Osilasi Rotor." },
  { "en": "Apa Itu Torsi Sinkronisasi (Synchronizing Torque)?", "id": "Torsi Yang Menjaga Mesin Tetap Sinkron." },
  { "en": "Apa Itu Pole Slipping?", "id": "Fenomena Kehilangan Sinkronisasi Antar Generator." },
  { "en": "Apa Itu Fotometri?", "id": "Ilmu Pengukuran Cahaya." },
  { "en": "Apa Hukum Kuadrat Terbalik Dalam Iluminasi?", "id": "Iluminasi Berkurang Dengan Kuadrat Jarak." },
  { "en": "Apa Hukum Kosinus Iluminasi?", "id": "Iluminasi Tergantung Sudut Datang Cahaya." },
  { "en": "Apa Itu Depresiasi Lumen?", "id": "Penurunan Output Cahaya Lampu Seiring Waktu." },
  { "en": "Bagaimana Prinsip Operasi Buchholz Relay?", "id": "Mendeteksi Akumulasi Gas Atau Aliran Minyak." },
  { "en": "Apa Setting Proteksi Overfluxing (V/Hz)?", "id": "Melindungi Inti Dari Saturasi Fluks Berlebih." },
  { "en": "Apa Itu Proteksi Differensial Motor?", "id": "Mendeteksi Gangguan Internal Belitan Motor." },
  { "en": "Apa Kepanjangan RCM (Reliability-Centered Maintenance)?", "id": "Reliability-Centered Maintenance." },
  { "en": "Apa Tujuan RCM (Reliability-Centered Maintenance)?", "id": "Mengoptimalkan Strategi Perawatan Berbasis Keandalan." },
  { "en": "Apa Kepanjangan CBM (Condition-Based Maintenance)?", "id": "Condition-Based Maintenance." },
  { "en": "Bagaimana Prinsip CBM (Condition-Based Maintenance)?", "id": "Perawatan Dilakukan Berdasarkan Kondisi Aktual." },
  { "en": "Apa Kepanjangan MTTF (Mean Time To Failure)?", "id": "Mean Time To Failure." },
  { "en": "Untuk Aset Apa MTTF (Mean Time To Failure) Digunakan?", "id": "Untuk Aset Yang Tidak Dapat Diperbaiki." },
  { "en": "Apa Beda Modbus RTU Dan TCP?", "id": "RTU (Remote Terminal Unit) Serial, TCP (Transmission Control Protocol) Ethernet." },
  { "en": "Apa Itu Profibus DP (Decentralized Periphery)?", "id": "Komunikasi Cepat Untuk Perangkat Lapangan." },
  { "en": "Apa Itu Profibus PA (Process Automation)?", "id": "Untuk Instrumen Proses Di Area Berbahaya." },
  { "en": "Apa Itu Kedalaman Kulit (Skin Depth)?", "id": "Ukuran Seberapa Dalam Arus AC (Alternating Current) Menembus." },
  { "en": "Apa Itu Generator Homopolar?", "id": "Generator DC (Direct Current) Yang Menghasilkan Arus Searah." },
  { "en": "Apa Itu Generator Acyclic?", "id": "Sama Dengan Generator Homopolar." },
  { "en": "Apa Itu Motor reluctance Switched (SRM)?", "id": "Sama Dengan SRM (Switched Reluctance Motor)." },
  { "en": "Apa Itu Kompensator Sinkron?", "id": "Sama Dengan Synchronous Condenser." },
  { "en": "Apa Itu Uji Indeks Polarisasi (PI)?", "id": "Sama Dengan PI (Polarization Index) Test." },
  { "en": "Apa Itu Peringkat IP (Ingress Protection) 65?", "id": "Tahan Debu Total Dan Semprotan Air." },
  { "en": "Apa Itu Peringkat IP (Ingress Protection) 67?", "id": "Tahan Debu Total Dan Perendaman Sementara." },
  { "en": "Apa Itu Peringkat IP (Ingress Protection) 68?", "id": "Tahan Debu Total Dan Perendaman Kontinu." },
  { "en": "Apa Itu Selungkup NEMA (National Electrical Manufacturers Association) 1?", "id": "Untuk Penggunaan Dalam Ruangan Tujuan Umum." },
  { "en": "Apa Itu Selungkup NEMA (National Electrical Manufacturers Association) 3R?", "id": "Tahan Cuaca Untuk Penggunaan Luar Ruangan." },
  { "en": "Apa Itu Selungkup NEMA (National Electrical Manufacturers Association) 12?", "id": "Tahan Debu Dan Tetesan Cairan Non-Korosif." },
  { "en": "Apa Itu Siklus Histeresis?", "id": "Sama Dengan Kurva B-H atau Histeresis Loop." },
  { "en": "Apa Itu Rugi Arus Eddy?", "id": "Kerugian Daya Akibat Arus Induksi Liar." },
  { "en": "Apa Itu Teori Pita Energi?", "id": "Menjelaskan Perbedaan Konduktor, Semikonduktor, Isolator." },
  { "en": "Apa Itu Pita Valensi?", "id": "Pita Energi Terluar Yang Terisi Elektron." },
  { "en": "Apa Itu Pita Konduksi?", "id": "Pita Energi Di Atas Pita Valensi." },
  { "en": "Apa Itu Celah Pita (Energy Gap)?", "id": "Energi Yang Dibutuhkan Elektron Melompat." },
  { "en": "Apa Itu Doping Semikonduktor?", "id": "Menambahkan Ketidakmurnian Untuk Mengubah Sifat." },
  { "en": "Apa Itu Semikonduktor Tipe-N?", "id": "Semikonduktor Dengan Kelebihan Elektron." },
  { "en": "Apa Itu Semikonduktor Tipe-P?", "id": "Semikonduktor Dengan Kelebihan Lubang (Hole)." },
  { "en": "Apa Itu Persimpangan P-N (P-N Junction)?", "id": "Batas Antara Material Tipe-P Dan Tipe-N." },
  { "en": "Apa Itu Lapisan Deplesi?", "id": "Area Di Sekitar Persimpangan P-N." },
  { "en": "Apa Itu Bias Maju (Forward Bias)?", "id": "Tegangan Yang Memungkinkan Arus Mengalir." },
  { "en": "Apa Itu Bias Mundur (Reverse Bias)?", "id": "Tegangan Yang Menghalangi Aliran Arus." },
  { "en": "Apa Itu Dioda Zener?", "id": "Dioda Yang Beroperasi Stabil Di Daerah Breakdown." },
  { "en": "Apa Fungsi Dioda Zener?", "id": "Sebagai Regulator Tegangan." },
  { "en": "Apa Itu Transistor Bipolar (BJT)?", "id": "Transistor Yang Dikontrol Oleh Arus." },
  { "en": "Apa Kepanjangan BJT (Bipolar Junction Transistor)?", "id": "Bipolar Junction Transistor." },
  { "en": "Apa Itu Transistor Efek Medan (FET)?", "id": "Transistor Yang Dikontrol Oleh Tegangan." },
  { "en": "Apa Kepanjangan FET (Field-Effect Transistor)?", "id": "Field-Effect Transistor." },
  { "en": "Apa Kepanjangan MOSFET (Metal-Oxide-Semiconductor FET)?", "id": "Metal-Oxide-Semiconductor Field-Effect Transistor." },
  { "en": "Apa Itu Penguat Operasional (Op-Amp)?", "id": "Penguat Tegangan DC (Direct Current) Gain Tinggi." },
  { "en": "Apa Kepanjangan Op-Amp (Operational Amplifier)?", "id": "Operational Amplifier." },
  { "en": "Apa Karakteristik Op-Amp (Operational Amplifier) Ideal?", "id": "Gain Tak Terhingga, Impedansi Input Tak Terhingga." },
  { "en": "Apa Itu Penguat Inverting?", "id": "Output Berbeda Fasa 180 Derajat." },
  { "en": "Apa Itu Penguat Non-Inverting?", "id": "Output Sefasa Dengan Input." },
  { "en": "Apa Itu Voltage Follower?", "id": "Rangkaian Op-Amp (Operational Amplifier) Dengan Gain Satu." },
  { "en": "Apa Itu Komparator?", "id": "Membandingkan Dua Tegangan Dan Menghasilkan Output Digital." },
  { "en": "Apa Itu Rangkaian Schmitt Trigger?", "id": "Komparator Dengan Histeresis." },
  { "en": "Apa Itu Multivibrator Astabil?", "id": "Rangkaian Yang Menghasilkan Gelombang Kotak." },
  { "en": "Apa Itu Multivibrator Monostabil?", "id": "Menghasilkan Satu Pulsa Saat Dipicu." },
  { "en": "Apa Itu Multivibrator Bistabil?", "id": "Rangkaian Dengan Dua Keadaan Stabil (Flip-Flop)." },
  { "en": "Apa Itu Gerbang Logika AND?", "id": "Output Tinggi Jika Semua Input Tinggi." },
  { "en": "Apa Itu Gerbang Logika OR?", "id": "Output Tinggi Jika Salah Satu Input Tinggi." },
  { "en": "Apa Itu Gerbang Logika NOT?", "id": "Output Adalah Inversi Dari Input." },
  { "en": "Apa Itu Gerbang Logika NAND?", "id": "Inversi Dari Gerbang AND." },
  { "en": "Apa Itu Gerbang Logika NOR?", "id": "Inversi Dari Gerbang OR." },
  { "en": "Apa Itu Gerbang Logika XOR (Exclusive OR)?", "id": "Output Tinggi Jika Input Berbeda." },
  { "en": "Apa Itu Gerbang Logika XNOR (Exclusive NOR)?", "id": "Inversi Dari Gerbang XOR." },
  { "en": "Apa Itu Aljabar Boolean?", "id": "Matematika Untuk Menganalisis Sirkuit Logika." },
  { "en": "Apa Itu Peta Karnaugh?", "id": "Metode Grafis Untuk Menyederhanakan Ekspresi Boolean." },
  { "en": "Apa Itu Flip-Flop?", "id": "Elemen Penyimpanan Memori Satu Bit." },
  { "en": "Apa Itu Latch?", "id": "Jenis Flip-Flop Dasar." },
  { "en": "Apa Itu Register?", "id": "Kumpulan Flip-Flop Untuk Menyimpan Data." },
  { "en": "Apa Itu Counter (Penghitung)?", "id": "Rangkaian Sekuensial Untuk Menghitung Pulsa." },
  { "en": "Apa Itu Multiplexer (MUX)?", "id": "Memilih Satu Dari Banyak Input." },
  { "en": "Apa Itu Demultiplexer (DEMUX)?", "id": "Mengarahkan Satu Input Ke Banyak Output." },
  { "en": "Apa Itu Encoder?", "id": "Mengubah Data Menjadi Kode Biner." },
  { "en": "Apa Itu Decoder?", "id": "Mengubah Kode Biner Menjadi Data." },
  { "en": "Apa Kepanjangan RAM (Random-Access Memory)?", "id": "Random-Access Memory." },
  { "en": "Apa Kepanjangan ROM (Read-Only Memory)?", "id": "Read-Only Memory." },
  { "en": "Apa Itu Mikrokontroler?", "id": "Komputer Kecil Dalam Satu Sirkuit Terpadu." },
  { "en": "Apa Itu Mikroprosesor?", "id": "Unit Pemrosesan Pusat (CPU) Dalam Sirkuit Terpadu." },
  { "en": "Apa Kepanjangan CPU (Central Processing Unit)?", "id": "Central Processing Unit." },
  { "en": "Apa Itu Sistem Tertanam (Embedded System)?", "id": "Sistem Komputer Dengan Fungsi Khusus." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter) Tipe Flash?", "id": "ADC (Analog-to-Digital Converter) Paralel Tercepat." },
  { "en": "Apa Kepanjangan SAR (Successive Approximation Register) ADC?", "id": "Successive Approximation Register Analog-to-Digital Converter." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter) R-2R Ladder?", "id": "Struktur DAC (Digital-to-Analog Converter) Yang Presisi." },
  { "en": "Apa Kepanjangan FPGA (Field-Programmable Gate Array)?", "id": "Field-Programmable Gate Array." },
  { "en": "Apa Fungsi FPGA (Field-Programmable Gate Array)?", "id": "Sirkuit Terpadu Yang Dapat Diprogram." },
  { "en": "Apa Kepanjangan CPLD (Complex Programmable Logic Device)?", "id": "Complex Programmable Logic Device." },
  { "en": "Apa Perbedaan FPGA Dan CPLD?", "id": "FPGA (Field-Programmable Gate Array) Lebih Kompleks Dan Fleksibel." },
  { "en": "Apa Kepanjangan ASK (Amplitude Shift Keying)?", "id": "Amplitude Shift Keying." },
  { "en": "Apa Kepanjangan FSK (Frequency Shift Keying)?", "id": "Frequency Shift Keying." },
  { "en": "Apa Kepanjangan PSK (Phase Shift Keying)?", "id": "Phase Shift Keying." },
  { "en": "Apa Kepanjangan QAM (Quadrature Amplitude Modulation)?", "id": "Quadrature Amplitude Modulation." },
  { "en": "Apa Itu Kekuatan Pasar (Market Power)?", "id": "Kemampuan Mempengaruhi Harga Pasar Secara Signifikan." },
  { "en": "Apa Itu Hak Transmisi Finansial (FTR)?", "id": "Instrumen Lindung Nilai Terhadap Biaya Kongesti." },
  { "en": "Apa Itu Pasar Layanan Tambahan?", "id": "Pasar Untuk Regulasi Frekuensi Dan Cadangan." },
  { "en": "Apa Itu Segitiga Duval?", "id": "Metode Interpretasi Hasil DGA (Dissolved Gas Analysis)." },
  { "en": "Apa Itu Analisis Furan?", "id": "Mendeteksi Degradasi Kertas Isolasi Trafo." },
  { "en": "Apa Beda Regenerasi Dan Reklamasi Minyak?", "id": "Reklamasi Mengembalikan Sifat Minyak Total." },
  { "en": "Apa Itu Skema Zone Interlocking?", "id": "Koordinasi Cepat Antar Relay Proteksi Busbar." },
  { "en": "Apa Itu Skema Transfer Trip?", "id": "Sinyal Trip Dikirim Ke Pemutus Jarak Jauh." },
  { "en": "Apa Beda Skema Blocking Dan Permissive?", "id": "Blocking Mencegah Trip, Permissive Mengizinkan Trip." },
  { "en": "Apa Arsitektur DCS (Distributed Control System)?", "id": "Terdistribusi Dengan Kontroler-Kontroler Proses." },
  { "en": "Apa Prinsip Desain HMI (Human-Machine Interface)?", "id": "Jelas, Konsisten, Dan Intuitif." },
  { "en": "Apa Itu Rasionalisasi Alarm?", "id": "Proses Meninjau Dan Mengoptimalkan Sistem Alarm." },
  { "en": "Apa Beda Belitan Lap Dan Wave?", "id": "Belitan Lap Untuk Arus Tinggi, Wave Tegangan Tinggi." },
  { "en": "Apa Itu Belitan Konsentris?", "id": "Kumparan Dengan Pusat Yang Sama." },
  { "en": "Apa Itu Iluminansi?", "id": "Jumlah Cahaya Yang Jatuh Pada Permukaan." },
  { "en": "Apa Itu Luminansi?", "id": "Jumlah Cahaya Yang Dipantulkan Permukaan." },
  { "en": "Apa Itu Faktor Utilisasi (UF)?", "id": "Rasio Cahaya Yang Mencapai Bidang Kerja." },
  { "en": "Apa Itu Faktor Perawatan (MF)?", "id": "Rasio Iluminansi Seiring Waktu." },
  { "en": "Apa Itu Ukuran Konduktor Pembumian?", "id": "Ditentukan Berdasarkan Arus Gangguan Dan Waktu." },
  { "en": "Apa Beda Emisi Terkonduksi Dan Teradiasi?", "id": "Terkonduksi Melalui Kabel, Teradiasi Melalui Udara." },
  { "en": "Apa Beda Kerentanan (Susceptibility) Dan Imunitas?", "id": "Imunitas Adalah Kemampuan Menahan Gangguan." },
  { "en": "Apa Itu Kopling Kapasitif?", "id": "Transfer Gangguan Melalui Medan Listrik." },
  { "en": "Apa Itu Kopling Induktif?", "id": "Transfer Gangguan Melalui Medan Magnet." },
  { "en": "Apa Itu Ground Loop?", "id": "Jalur Arus Tak Diinginkan Melalui Pembumian." },
  { "en": "Apa Dampak Ground Loop?", "id": "Menimbulkan Noise Dan Kesalahan Pengukuran." },
  { "en": "Apa Itu Common-Mode Noise?", "id": "Noise Yang Muncul Sama Pada Kedua Konduktor." },
  { "en": "Apa Itu Differential-Mode Noise?", "id": "Noise Yang Muncul Berlawanan Pada Konduktor." },
  { "en": "Apa Fungsi Common-Mode Choke?", "id": "Meredam Common-Mode Noise." },
  { "en": "Apa Kepanjangan ESD (Electrostatic Discharge)?", "id": "Electrostatic Discharge." },
  { "en": "Bagaimana Mencegah Kerusakan ESD (Electrostatic Discharge)?", "id": "Menggunakan Gelang Antistatis Dan Meja Kerja Grounded." },
  { "en": "Apa Itu Latch-up?", "id": "Kondisi Hubung Singkat Internal Pada Sirkuit CMOS (Complementary Metal-Oxide-Semiconductor)." },
  { "en": "Apa Itu Watchdog Timer?", "id": "Timer Pengaman Untuk Mereset Sistem Macet." },
  { "en": "Apa Itu Brown-out Reset (BOR)?", "id": "Mereset Mikrokontroler Saat Tegangan Turun." },
  { "en": "Apa Kepanjangan BOR (Brown-out Reset)?", "id": "Brown-out Reset." },
  { "en": "Apa Itu In-Circuit Serial Programming (ICSP)?", "id": "Memprogram Mikrokontroler Saat Terpasang Di Rangkaian." },
  { "en": "Apa Kepanjangan ICSP (In-Circuit Serial Programming)?", "id": "In-Circuit Serial Programming." },
  { "en": "Apa Kepanjangan JTAG (Joint Test Action Group)?", "id": "Joint Test Action Group." },
  { "en": "Apa Fungsi Antarmuka JTAG (Joint Test Action Group)?", "id": "Untuk Debugging Dan Pengujian Perangkat Keras." },
  { "en": "Apa Itu Firmware?", "id": "Perangkat Lunak Yang Tertanam Dalam Perangkat Keras." },
  { "en": "Apa Itu Bootloader?", "id": "Program Kecil Untuk Memuat Sistem Operasi." },
  { "en": "Apa Itu Real-Time Operating System (RTOS)?", "id": "Sistem Operasi Dengan Batas Waktu Ketat." },
  { "en": "Apa Kepanjangan RTOS (Real-Time Operating System)?", "id": "Real-Time Operating System." },
  { "en": "Apa Itu Jitter?", "id": "Variasi Waktu Kedatangan Sinyal Digital." },
  { "en": "Apa Itu Skew?", "id": "Perbedaan Waktu Kedatangan Sinyal Paralel." },
  { "en": "Apa Itu Protokol I2C (Inter-Integrated Circuit)?", "id": "Protokol Komunikasi Serial Dua Kawat." },
  { "en": "Apa Itu Protokol SPI (Serial Peripheral Interface)?", "id": "Protokol Komunikasi Serial Sinkron." },
  { "en": "Apa Itu Protokol UART (Universal Asynchronous Receiver-Transmitter)?", "id": "Komunikasi Serial Asinkron." },
  { "en": "Apa Itu Parity Bit?", "id": "Bit Tambahan Untuk Deteksi Kesalahan Sederhana." },
  { "en": "Apa Itu CRC (Cyclic Redundancy Check)?", "id": "Metode Deteksi Kesalahan Yang Lebih Andal." },
  { "en": "Apa Itu Handshaking?", "id": "Proses Sinkronisasi Komunikasi Antar Perangkat." },
  { "en": "Apa Itu Baud Rate?", "id": "Jumlah Perubahan Sinyal Per Detik." },
  { "en": "Apa Perbedaan Bit Rate Dan Baud Rate?", "id": "Bit Rate Bisa Lebih Tinggi Dari Baud Rate." },
  { "en": "Apa Itu Level Logika TTL (Transistor-Transistor Logic)?", "id": "Standar Level Tegangan Untuk Sirkuit Logika." },
  { "en": "Apa Itu Level Logika CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Standar Level Tegangan Berbasis Tegangan Suplai." },
  { "en": "Apa Itu Open Collector Output?", "id": "Tipe Output Yang Memerlukan Resistor Pull-up." },
  { "en": "Apa Itu Push-Pull Output?", "id": "Tipe Output Yang Dapat Memberi-Menarik Arus." },
  { "en": "Apa Itu Tri-State Logic?", "id": "Logika Dengan Tiga Keadaan: Tinggi, Rendah, Impedansi Tinggi." },
  { "en": "Apa Itu Bus contention?", "id": "Saat Dua Output Mencoba Mengendalikan Bus Bersamaan." },
  { "en": "Apa Itu Resistor Pull-up?", "id": "Menarik Sinyal Ke Tegangan Tinggi Saat Mengambang." },
  { "en": "Apa Itu Resistor Pull-down?", "id": "Menarik Sinyal Ke Tegangan Rendah Saat Mengambang." },
  { "en": "Apa Itu Kapasitor Bypass?", "id": "Menyaring Noise Frekuensi Tinggi Dari Suplai." },
  { "en": "Apa Itu Kapasitor Decoupling?", "id": "Sama Dengan Kapasitor Bypass." },
  { "en": "Apa Itu Via Pada PCB (Printed Circuit Board)?", "id": "Lubang Konduktif Penghubung Antar Lapisan." },
  { "en": "Apa Itu Annular Ring?", "id": "Area Tembaga Di Sekitar Lubang Via." },
  { "en": "Apa Itu Solder Mask?", "id": "Lapisan Pelindung Pada PCB (Printed Circuit Board)." },
  { "en": "Apa Itu Silkscreen?", "id": "Lapisan Tinta Label Komponen Pada PCB (Printed Circuit Board)." },
  { "en": "Apa Itu Footprint Komponen?", "id": "Pola Pad Untuk Pemasangan Komponen." },
  { "en": "Apa Itu Surface-Mount Technology (SMT)?", "id": "Komponen Dipasang Langsung Di Permukaan PCB (Printed Circuit Board)." },
  { "en": "Apa Kepanjangan SMT (Surface-Mount Technology)?", "id": "Surface-Mount Technology." },
  { "en": "Apa Itu Through-Hole Technology?", "id": "Kaki Komponen Dimasukkan Melalui Lubang PCB (Printed Circuit Board)." },
  { "en": "Apa Itu Reflow Soldering?", "id": "Proses Pematrian Untuk Komponen SMT (Surface-Mount Technology)." },
  { "en": "Apa Itu Wave Soldering?", "id": "Proses Pematrian Untuk Komponen Through-Hole." },
  { "en": "Apa Itu Solder Paste?", "id": "Campuran Timah Solder Dan Fluks." },
  { "en": "Apa Itu Fluks (Flux)?", "id": "Bahan Kimia Pembersih Oksida Saat Pematrian." },
  { "en": "Apa Itu Tombstoning?", "id": "Cacat Pematrian Dimana Komponen SMT (Surface-Mount Technology) Berdiri." },
  { "en": "Apa Itu Solder Bridge?", "id": "Hubung Singkat Akibat Solder Berlebih." },
  { "en": "Apa Itu Cold Solder Joint?", "id": "Sambungan Solder Yang Tidak Matang Sempurna." },
  { "en": "Apa Kepanjangan RoHS (Restriction of Hazardous Substances)?", "id": "Restriction of Hazardous Substances." },
  { "en": "Apa Yang Diatur Direktif RoHS (Restriction of Hazardous Substances)?", "id": "Pembatasan Bahan Berbahaya Dalam Elektronik." },
  { "en": "Apa Itu Timah Bebas Timbal (Lead-Free)?", "id": "Solder Tanpa Kandungan Logam Timbal." },
  { "en": "Mengapa Timah Bebas Timbal Digunakan?", "id": "Untuk Mematuhi Regulasi Lingkungan." },
  { "en": "Apa Itu Thermal Pad?", "id": "Area Tembaga Untuk Disipasi Panas Komponen." },
  { "en": "Apa Itu Thermal Via?", "id": "Via Untuk Membantu Transfer Panas Antar Lapisan." },
  { "en": "Apa Itu Impedansi Terkontrol?", "id": "Desain Jejak PCB (Printed Circuit Board) Untuk Impedansi Tertentu." },
  { "en": "Apa Beda Stabilitas Sinyal Kecil Dan Transien?", "id": "Sinyal Kecil Gangguan Kecil, Transien Gangguan Besar." },
  { "en": "Apa Itu Koefisien Daya Sinkronisasi?", "id": "Torsi Yang Menjaga Sinkronisasi Per Sudut." },
  { "en": "Apa Itu Kegagalan Tembus Termal?", "id": "Kerusakan Isolasi Akibat Panas Berlebih." },
  { "en": "Apa Itu Kegagalan Tembus Intrinsik?", "id": "Kerusakan Isolasi Akibat Medan Listrik Murni." },
  { "en": "Apa Itu Jangkauan (Reach) Relay Jarak?", "id": "Batas Impedansi Maksimum Setting Relay." },
  { "en": "Apa Kepanjangan UFLS (Under-Frequency Load Shedding)?", "id": "Under-Frequency Load Shedding." },
  { "en": "Apa Tujuan Skema UFLS (Under-Frequency Load Shedding)?", "id": "Mencegah Keruntuhan Sistem Akibat Frekuensi Rendah." },
  { "en": "Apa Kepanjangan DCB (Disconnecting Circuit Breaker)?", "id": "Disconnecting Circuit Breaker." },
  { "en": "Apa Fungsi DCB (Disconnecting Circuit Breaker)?", "id": "Menggabungkan Fungsi Pemutus Dan Pemisah." },
  { "en": "Beda Desain Live Tank Dan Dead Tank?", "id": "Live Tank Bertegangan, Dead Tank Ditanahkan." },
  { "en": "Apa Kemampuan Penanganan Energi Arrester?", "id": "Energi Maksimum Yang Dapat Diserap Arrester." },
  { "en": "Apa Kepanjangan AFE (Active Front End) Rectifier?", "id": "Active Front End Rectifier." },
  { "en": "Apa Keuntungan AFE (Active Front End) Rectifier?", "id": "Aliran Daya Dua Arah, Harmonisa Rendah." },
  { "en": "Apa Kepanjangan MMC (Modular Multilevel Converter)?", "id": "Modular Multilevel Converter." },
  { "en": "Dimana MMC (Modular Multilevel Converter) Digunakan?", "id": "Aplikasi HVDC (High Voltage Direct Current) Dan STATCOM (Static Synchronous Compensator)." },
  { "en": "Apa Kepanjangan STS (Static Transfer Switch)?", "id": "Static Transfer Switch." },
  { "en": "Apa Fungsi STS (Static Transfer Switch)?", "id": "Memindahkan Beban Antar Sumber Daya Cepat." },
  { "en": "Apa Itu Kontrol Trapezoidal BLDC (Brushless DC) Motor?", "id": "Metode Kontrol Sederhana Berbasis Sensor Hall." },
  { "en": "Apa Itu Kontrol Sinusoidal BLDC (Brushless DC) Motor?", "id": "Metode Kontrol Halus Tanpa Torsi Riak." },
  { "en": "Apa Itu Saliency Mesin Sinkron?", "id": "Perbedaan Reluktansi Antara Sumbu d-q." },
  { "en": "Apa Kepanjangan UMP (Unbalanced Magnetic Pull)?", "id": "Unbalanced Magnetic Pull." },
  { "en": "Apa Penyebab UMP (Unbalanced Magnetic Pull) Pada Motor?", "id": "Eksentrisitas Celah Udara Antara Rotor-Stator." },
  { "en": "Apa Peran Electrical Safety Officer (ESO)?", "id": "Mengawasi Dan Menerapkan Program Keselamatan Listrik." },
  { "en": "Apa Pola Partial Discharge (PD) Korona?", "id": "Pulsa Terjadi Pada Puncak Tegangan AC (Alternating Current)." },
  { "en": "Apa Pola Partial Discharge (PD) Internal?", "id": "Pulsa Terjadi Di Sekitar Titik Nol Tegangan." },
  { "en": "Apa Kepanjangan FRA (Frequency Response Analysis)?", "id": "Frequency Response Analysis." },
  { "en": "Apa Interpretasi Tes FRA (Frequency Response Analysis)?", "id": "Membandingkan Hasil Ukur Dengan Referensi Awal." },
  { "en": "Apa Kepanjangan LCC (Life Cycle Cost)?", "id": "Life Cycle Cost." },
  { "en": "Apa Saja Komponen LCC (Life Cycle Cost)?", "id": "Biaya Awal, Operasi, Perawatan, Pembuangan." },
  { "en": "Apa Itu Standar IEC (International Electrotechnical Commission) 62443?", "id": "Standar Keamanan Siber Untuk Sistem Otomasi Industri." },
  { "en": "Apa Itu Dioda Data (Data Diode)?", "id": "Perangkat Keamanan Jaringan Satu Arah." },
  { "en": "Apa Perbedaan Router Dan Switch?", "id": "Router Menghubungkan Jaringan, Switch Menghubungkan Perangkat." },
  { "en": "Apa Kepanjangan HTS (High-Temperature Superconducting)?", "id": "High-Temperature Superconducting." },
  { "en": "Apa Keuntungan Kabel HTS (High-Temperature Superconducting)?", "id": "Kapasitas Arus Sangat Tinggi Tanpa Rugi." },
  { "en": "Apa Kepanjangan WBG (Wide-Bandgap) Semiconductor?", "id": "Wide-Bandgap Semiconductor." },
  { "en": "Apa Contoh Material WBG (Wide-Bandgap)?", "id": "SiC (Silicon Carbide) Dan GaN (Gallium Nitride)." },
  { "en": "Apa Itu Bus Diferensial?", "id": "Jalur Komunikasi Menggunakan Sepasang Sinyal Berlawanan." },
  { "en": "Apa Contoh Bus Diferensial?", "id": "RS-485, CAN (Controller Area Network) Bus, Ethernet." },
  { "en": "Apa Kepanjangan CAN (Controller Area Network)?", "id": "Controller Area Network." },
  { "en": "Dimana CAN (Controller Area Network) Bus Digunakan?", "id": "Otomotif, Otomasi Industri." },
  { "en": "Apa Itu Terminasi Bus?", "id": "Resistor Di Ujung Bus Untuk Mencegah Pantulan." },
  { "en": "Apa Itu FMEA (Failure Mode and Effects Analysis) Desain?", "id": "Analisis Kegagalan Selama Fase Desain." },
  { "en": "Apa Itu FMEA (Failure Mode and Effects Analysis) Proses?", "id": "Analisis Kegagalan Dalam Proses Manufaktur." },
  { "en": "Apa Itu Nomor Prioritas Risiko (RPN)?", "id": "Ukuran Kuantitatif Risiko Dalam FMEA (Failure Mode and Effects Analysis)." },
  { "en": "Apa Kepanjangan RPN (Risk Priority Number)?", "id": "Risk Priority Number." },
  { "en": "Apa Itu Faktor Crest?", "id": "Sama Dengan Crest Factor." },
  { "en": "Apa Itu Detektor Puncak (Peak Detector)?", "id": "Sirkuit Untuk Menangkap Nilai Tegangan Puncak." },
  { "en": "Apa Itu Detektor RMS (Root Mean Square) Sejati?", "id": "Mengukur Nilai RMS (Root Mean Square) Terlepas Bentuk Gelombang." },
  { "en": "Apa Itu Jembatan Wien?", "id": "Sirkuit Jembatan Untuk Osilator Gelombang Sinus." },
  { "en": "Apa Itu Osilator Relaksasi?", "id": "Osilator Yang Menghasilkan Gelombang Non-Sinusoidal." },
  { "en": "Apa Itu Phase-Locked Loop (PLL)?", "id": "Sistem Kontrol Yang Menghasilkan Sinyal Sinkron." },
  { "en": "Apa Kepanjangan PLL (Phase-Locked Loop)?", "id": "Phase-Locked Loop." },
  { "en": "Apa Komponen Utama PLL (Phase-Locked Loop)?", "id": "Phase Detector, Low-Pass Filter, VCO (Voltage-Controlled Oscillator)." },
  { "en": "Apa Kepanjangan VCO (Voltage-Controlled Oscillator)?", "id": "Voltage-Controlled Oscillator." },
  { "en": "Apa Itu Sintesis Frekuensi?", "id": "Menghasilkan Berbagai Frekuensi Dari Satu Referensi." },
  { "en": "Apa Itu Mixer Frekuensi?", "id": "Sirkuit Non-Linier Untuk Menggeser Frekuensi." },
  { "en": "Apa Itu Heterodyning?", "id": "Proses Mencampur Sinyal Untuk Menghasilkan Frekuensi Baru." },
  { "en": "Apa Itu Frekuensi Gambar (Image Frequency)?", "id": "Frekuensi Yang Tidak Diinginkan Dalam Penerima Superheterodyne." },
  { "en": "Apa Itu Penerima Superheterodyne?", "id": "Arsitektur Penerima Radio Paling Umum." },
  { "en": "Apa Itu Frekuensi Antara (IF)?", "id": "Frekuensi Tetap Hasil Proses Pencampuran." },
  { "en": "Apa Kepanjangan IF (Intermediate Frequency)?", "id": "Intermediate Frequency." },
  { "en": "Apa Itu Penguat Frekuensi Radio (RF Amplifier)?", "id": "Menguatkan Sinyal Lemah Dari Antena." },
  { "en": "Apa Kepanjangan RF (Radio Frequency)?", "id": "Radio Frequency." },
  { "en": "Apa Itu Automatic Gain Control (AGC)?", "id": "Menjaga Level Output Konstan." },
  { "en": "Apa Kepanjangan AGC (Automatic Gain Control)?", "id": "Automatic Gain Control." },
  { "en": "Apa Itu Squelch?", "id": "Sirkuit Untuk Membungkam Noise Saat Tidak Ada Sinyal." },
  { "en": "Apa Itu Duplexer?", "id": "Memungkinkan Antena Tunggal Untuk Mengirim-Menerima." },
  { "en": "Apa Itu Circulator?", "id": "Perangkat Tiga Port Yang Mengarahkan Sinyal." },
  { "en": "Apa Itu Isolator RF (Radio Frequency)?", "id": "Memungkinkan Sinyal Lewat Hanya Satu Arah." },
  { "en": "Apa Itu Directional Coupler?", "id": "Mencuplik Sebagian Kecil Daya Sinyal." },
  { "en": "Apa Itu Atenuator?", "id": "Perangkat Untuk Melemahkan Kekuatan Sinyal." },
  { "en": "Apa Itu Phase Shifter?", "id": "Perangkat Untuk Mengubah Fasa Sinyal." },
  { "en": "Apa Itu Balun?", "id": "Menghubungkan Saluran Seimbang Dan Tidak Seimbang." },
  { "en": "Apa Itu Saluran Transmisi Seimbang?", "id": "Contoh: Kabel Twin-Lead." },
  { "en": "Apa Itu Saluran Transmisi Tidak Seimbang?", "id": "Contoh: Kabel Koaksial." },
  { "en": "Apa Itu Radiasi Spurious?", "id": "Emisi Yang Tidak Diinginkan Di Luar Bandwidth." },
  { "en": "Apa Itu Spektrum Elektromagnetik?", "id": "Rentang Semua Kemungkinan Frekuensi Radiasi Elektromagnetik." },
  { "en": "Apa Itu Alokasi Spektrum?", "id": "Penetapan Pita Frekuensi Untuk Layanan Tertentu." },
  { "en": "Apa Itu Propagasi Gelombang Tanah?", "id": "Gelombang Radio Yang Mengikuti Kontur Bumi." },
  { "en": "Apa Itu Propagasi Gelombang Langit?", "id": "Gelombang Radio Dipantulkan Kembali Oleh Ionosfer." },
  { "en": "Apa Itu Propagasi Line-of-Sight (LOS)?", "id": "Propagasi Garis Lurus Antara Pemancar-Penerima." },
  { "en": "Apa Itu Fading?", "id": "Fluktuasi Kekuatan Sinyal Yang Diterima." },
  { "en": "Apa Itu Multipath Fading?", "id": "Interferensi Akibat Sinyal Tiba Dari Jalur Berbeda." },
  { "en": "Apa Itu Doppler Shift?", "id": "Perubahan Frekuensi Akibat Gerakan Relatif." },
  { "en": "Apa Itu Sistem Komunikasi Seluler?", "id": "Jaringan Radio Terdistribusi Melalui Area Sel." },
  { "en": "Apa Itu Handoff?", "id": "Proses Pemindahan Panggilan Antar Sel." },
  { "en": "Apa Itu Base Transceiver Station (BTS)?", "id": "Peralatan Radio Di Setiap Sel." },
  { "en": "Apa Itu Mobile Switching Center (MSC)?", "id": "Pusat Jaringan Untuk Komunikasi Seluler." },
  { "en": "Apa Kepanjangan GSM (Global System for Mobile Communications)?", "id": "Global System for Mobile Communications." },
  { "en": "Apa Kepanjangan CDMA (Code Division Multiple Access)?", "id": "Code Division Multiple Access." },
  { "en": "Apa Kepanjangan LTE (Long-Term Evolution)?", "id": "Long-Term Evolution." },
  { "en": "Apa Itu 5G (Generasi Kelima)?", "id": "Standar Teknologi Seluler Generasi Kelima." },
  { "en": "Apa Itu MIMO (Multiple-Input, Multiple-Output)?", "id": "Menggunakan Banyak Antena Untuk Meningkatkan Kinerja." },
  { "en": "Apa Itu Beamforming?", "id": "Memfokuskan Sinyal Nirkabel Ke Arah Tertentu." },
  { "en": "Apa Signifikansi Konstanta Inersia (H)?", "id": "Menentukan Respon Awal Generator Terhadap Gangguan." },
  { "en": "Apa Kepanjangan POD (Power Oscillation Damper)?", "id": "Power Oscillation Damper." },
  { "en": "Apa Fungsi POD (Power Oscillation Damper)?", "id": "Meredam Ayunan Daya (Power Swings) Sistem." },
  { "en": "Bagaimana Menghitung Jarak Rambat (Creepage Distance)?", "id": "Berdasarkan Tingkat Polusi Dan Tegangan." },
  { "en": "Beda Flashover Dan Puncture Pada Isolator?", "id": "Flashover Di Permukaan, Puncture Menembus Material." },
  { "en": "Apa Uji Utama Minyak Isolasi?", "id": "Tegangan Tembus, Kadar Air, Keasaman." },
  { "en": "Bagaimana Cara Kerja Proteksi Gelombang Berjalan?", "id": "Mendeteksi Gelombang Transien Dari Titik Gangguan." },
  { "en": "Apa Itu Skema Directional Comparison Unblocking?", "id": "Sinyal Carrier Mengizinkan Trip Jarak Jauh." },
  { "en": "Apa Itu Single-Pole Tripping?", "id": "Membuka Hanya Satu Fasa Saat Gangguan Satu Fasa." },
  { "en": "Apa Keuntungan Single-Pole Tripping?", "id": "Menjaga Kestabilan Sistem Selama Gangguan." },
  { "en": "Apa Itu Desain Busbar Split-Phase?", "id": "Memisahkan Fasa Untuk Mengurangi Arus Gangguan." },
  { "en": "Bagaimana Prosedur Penanganan Gas SF6 (Belerang Heksafluorida)?", "id": "Menggunakan Peralatan Khusus Untuk Mencegah Pelepasan." },
  { "en": "Apa Kepanjangan ZVS (Zero-Voltage Switching)?", "id": "Zero-Voltage Switching." },
  { "en": "Apa Kepanjangan ZCS (Zero-Current Switching)?", "id": "Zero-Current Switching." },
  { "en": "Apa Keuntungan Teknik Soft-Switching?", "id": "Mengurangi Stres Dan Rugi Pada Saklar." },
  { "en": "Apa Itu Belitan Fractional-Slot?", "id": "Jumlah Slot Per Kutub Per Fasa Pecahan." },
  { "en": "Apa Efek Tegangan Tidak Seimbang Pada Motor?", "id": "Arus Urutan Negatif, Pemanasan Berlebih." },
  { "en": "Apa Beda Klasifikasi Zona Dan Divisi?", "id": "Zona (IEC), Divisi (NEC) Untuk Area Berbahaya." },
  { "en": "Apa Kepanjangan NERC (North American Electric Reliability Corporation)?", "id": "North American Electric Reliability Corporation." },
  { "en": "Apa Peran NERC (North American Electric Reliability Corporation)?", "id": "Menjamin Keandalan Jaringan Listrik Amerika Utara." },
  { "en": "Apa Gas Kunci DGA (Dissolved Gas Analysis) Untuk Overheating?", "id": "Etilena (C2H4) Dan Metana (CH4)." },
  { "en": "Apa Gas Kunci DGA (Dissolved Gas Analysis) Untuk Arcing?", "id": "Asetilena (C2H2)." },
  { "en": "Apa Analisis Weibull?", "id": "Metode Statistik Untuk Menganalisis Data Kegagalan." },
  { "en": "Apa Aplikasi Data Synchrophasor?", "id": "Estimasi Keadaan, Deteksi Ayunan Daya." },
  { "en": "Beda Pesan GOOSE Dan MMS?", "id": "GOOSE (Generic Object Oriented Substation Event) Cepat, MMS (Manufacturing Message Specification) Lambat." },
  { "en": "Apa Keuntungan Inti Baja Amorf?", "id": "Rugi Tanpa Beban (No-Load Loss) Sangat Rendah." },
  { "en": "Beda Inti Ferrite Dan Serbuk Besi?", "id": "Ferrite Untuk Frekuensi Tinggi, Serbuk Besi Arus Tinggi." },
  { "en": "Apa Itu Magnet Neodymium?", "id": "Magnet Tanah Jarang Paling Kuat." },
  { "en": "Apa Kepanjangan SmCo (Samarium-Cobalt)?", "id": "Samarium-Cobalt." },
  { "en": "Apa Keuntungan Magnet SmCo (Samarium-Cobalt)?", "id": "Tahan Temperatur Tinggi." },
  { "en": "Apa Itu Diagram Fasor?", "id": "Representasi Grafis Dari Kuantitas AC (Alternating Current)." },
  { "en": "Apa Itu Impedansi Gelombang (Surge Impedance)?", "id": "Impedansi Karakteristik Saluran Transmisi." },
  { "en": "Apa Itu Matriks ABCD?", "id": "Parameter Jaringan Dua Gerbang (Two-Port Network)." },
  { "en": "Apa Itu Jaringan Dua Gerbang (Two-Port Network)?", "id": "Sirkuit Listrik Dengan Sepasang Terminal Input-Output." },
  { "en": "Apa Itu Jaringan Pi (π)?", "id": "Model Rangkaian Dengan Konfigurasi Seperti Huruf Pi." },
  { "en": "Apa Itu Jaringan T?", "id": "Model Rangkaian Dengan Konfigurasi Seperti Huruf T." },
  { "en": "Apa Itu Faktor Bentuk (Form Factor)?", "id": "Rasio Nilai RMS (Root Mean Square) Terhadap Rata-rata." },
  { "en": "Apa Itu Faktor Riak (Ripple Factor)?", "id": "Ukuran Riak Sisa Pada Output Penyearah." },
  { "en": "Apa Itu Efisiensi Penyearah?", "id": "Rasio Daya Output DC (Direct Current) Terhadap Input AC (Alternating Current)." },
  { "en": "Apa Itu Tegangan Puncak Terbalik (PIV)?", "id": "Tegangan Mundur Maksimum Yang Dapat Ditahan Dioda." },
  { "en": "Apa Kepanjangan PIV (Peak Inverse Voltage)?", "id": "Peak Inverse Voltage." },
  { "en": "Apa Itu Penyearah Setengah Gelombang?", "id": "Hanya Melewatkan Setengah Siklus Gelombang AC (Alternating Current)." },
  { "en": "Apa Itu Penyearah Gelombang Penuh?", "id": "Melewatkan Dan Membalik Setengah Siklus Negatif." },
  { "en": "Apa Itu Penyearah Jembatan?", "id": "Penyearah Gelombang Penuh Menggunakan Empat Dioda." },
  { "en": "Apa Itu Filter Kapasitor?", "id": "Kapasitor Untuk Meratakan Tegangan Output Penyearah." },
  { "en": "Apa Itu Rangkaian Clamper?", "id": "Menggeser Level DC (Direct Current) Sinyal Tanpa Mengubah Bentuk." },
  { "en": "Apa Itu Rangkaian Clipper?", "id": "Memotong Sebagian Sinyal Di Atas/Bawah Level Tertentu." },
  { "en": "Apa Itu Pengganda Tegangan?", "id": "Rangkaian Untuk Menghasilkan Tegangan DC (Direct Current) Kelipatan Puncak." },
  { "en": "Apa Itu Dioda Varactor?", "id": "Dioda Yang Kapasitansinya Berubah Dengan Tegangan." },
  { "en": "Apa Itu Dioda Terowongan (Tunnel Diode)?", "id": "Dioda Dengan Karakteristik Resistansi Negatif." },
  { "en": "Apa Itu Dioda Schottky?", "id": "Dioda Logam-Semikonduktor Dengan Kecepatan Tinggi." },
  { "en": "Apa Itu Dioda PIN?", "id": "Dioda Dengan Lapisan Intrinsik (I)." },
  { "en": "Apa Aplikasi Dioda PIN?", "id": "Saklar RF (Radio Frequency), Atenuator, Fotodetektor." },
  { "en": "Apa Itu Garis Beban DC (Direct Current)?", "id": "Grafik Titik Kerja Transistor." },
  { "en": "Apa Itu Titik-Q (Quiescent Point)?", "id": "Titik Operasi Transistor Tanpa Sinyal Input." },
  { "en": "Apa Itu Bias Transistor?", "id": "Memberi Tegangan DC (Direct Current) Untuk Menentukan Titik-Q." },
  { "en": "Apa Itu Rangkaian Pembagi Tegangan Bias?", "id": "Metode Pembiasan Transistor Paling Stabil." },
  { "en": "Apa Itu Penguat Kelas A?", "id": "Transistor Aktif Sepanjang Siklus Penuh." },
  { "en": "Apa Itu Penguat Kelas B?", "id": "Transistor Aktif Setengah Siklus (180 Derajat)." },
  { "en": "Apa Itu Penguat Kelas AB?", "id": "Kompromi Antara Kelas A Dan B." },
  { "en": "Apa Itu Penguat Kelas C?", "id": "Transistor Aktif Kurang Dari Setengah Siklus." },
  { "en": "Apa Itu Distorsi Crossover?", "id": "Distorsi Pada Penguat Kelas B." },
  { "en": "Apa Itu Penguat Diferensial?", "id": "Menguatkan Perbedaan Antara Dua Sinyal Input." },
  { "en": "Apa Kepanjangan CMRR (Common-Mode Rejection Ratio)?", "id": "Common-Mode Rejection Ratio." },
  { "en": "Apa Arti CMRR (Common-Mode Rejection Ratio) Tinggi?", "id": "Kemampuan Baik Menolak Sinyal Common-Mode." },
  { "en": "Apa Itu Cermin Arus (Current Mirror)?", "id": "Rangkaian Untuk Menyalin Arus." },
  { "en": "Apa Itu Beban Aktif?", "id": "Menggunakan Transistor Sebagai Beban Resistor." },
  { "en": "Apa Itu Rangkaian Darlington?", "id": "Sepasang Transistor Untuk Penguatan Arus Tinggi." },
  { "en": "Apa Itu Rangkaian Cascode?", "id": "Kombinasi Transistor Untuk Bandwidth Lebar." },
  { "en": "Apa Itu Umpan Balik (Feedback)?", "id": "Mengembalikan Sebagian Sinyal Output Ke Input." },
  { "en": "Apa Itu Umpan Balik Negatif?", "id": "Meningkatkan Stabilitas, Mengurangi Gain." },
  { "en": "Apa Itu Umpan Balik Positif?", "id": "Menyebabkan Osilasi, Digunakan Dalam Osilator." },
  { "en": "Apa Itu Kriteria Barkhausen?", "id": "Syarat Terjadinya Osilasi." },
  { "en": "Apa Itu Osilator Jembatan Wien?", "id": "Osilator Gelombang Sinus Menggunakan Jembatan Wien." },
  { "en": "Apa Itu Osilator Colpitts?", "id": "Osilator LC Menggunakan Pembagi Kapasitif." },
  { "en": "Apa Itu Osilator Hartley?", "id": "Osilator LC Menggunakan Pembagi Induktif." },
  { "en": "Apa Itu Osilator Kristal?", "id": "Osilator Dengan Stabilitas Frekuensi Sangat Tinggi." },
  { "en": "Apa Itu Efek Miller?", "id": "Peningkatan Kapasitansi Input Akibat Penguatan." },
  { "en": "Apa Itu Thyristor?", "id": "Keluarga Saklar Semikonduktor Empat Lapis." },
  { "en": "Apa Itu TRIAC (Triode for Alternating Current)?", "id": "Saklar Dua Arah Untuk Arus AC (Alternating Current)." },
  { "en": "Apa Itu DIAC (Diode for Alternating Current)?", "id": "Dioda Pemicu Dua Arah." },
  { "en": "Apa Itu Rangkaian Crowbar?", "id": "Rangkaian Proteksi Tegangan Lebih Cepat." },
  { "en": "Apa Itu Opto-isolator?", "id": "Mengisolasi Sirkuit Menggunakan Cahaya." },
  { "en": "Apa Nama Lain Opto-isolator?", "id": "Optocoupler." },
  { "en": "Apa Itu Heat Sink?", "id": "Perangkat Untuk Membuang Panas Dari Komponen." },
  { "en": "Apa Itu Tahanan Termal?", "id": "Hambatan Terhadap Aliran Panas." },
  { "en": "Apa Itu Power Derating?", "id": "Pengurangan Disipasi Daya Maksimum Pada Suhu Tinggi." },
  { "en": "Apa Itu Safe Operating Area (SOA)?", "id": "Grafik Batas Operasi Aman Transistor." },
  { "en": "Apa Kepanjangan SOA (Safe Operating Area)?", "id": "Safe Operating Area." },
  { "en": "Apa Itu Kegagalan Tembus Sekunder?", "id": "Kerusakan Transistor Akibat Hot Spot Lokal." },
  { "en": "Apa Itu Fotodioda?", "id": "Dioda Yang Mengubah Cahaya Menjadi Arus." },
  { "en": "Apa Itu Fototransistor?", "id": "Transistor Yang Dikontrol Oleh Cahaya." },
  { "en": "Apa Itu Solar Cell?", "id": "Dioda Area Besar Untuk Konversi Energi Surya." },
  { "en": "Apa Itu LED (Light-Emitting Diode) Inframerah?", "id": "LED (Light-Emitting Diode) Yang Memancarkan Cahaya Inframerah." },
  { "en": "Apa Itu Sensor Gambar CCD (Charge-Coupled Device)?", "id": "Sensor Semikonduktor Untuk Pencitraan Digital." },
  { "en": "Apa Itu Sensor Gambar CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Sensor Gambar Alternatif Selain CCD (Charge-Coupled Device)." },
  { "en": "Apa Itu Sistem Mikroelektromekanis (MEMS)?", "id": "Perangkat Mekanis Dan Elektrik Miniatur." },
  { "en": "Apa Kepanjangan MEMS (Microelectromechanical Systems)?", "id": "Microelectromechanical Systems." },
  { "en": "Apa Contoh Perangkat MEMS (Microelectromechanical Systems)?", "id": "Akselerometer, Giroskop, Mikrofon." },
  { "en": "Apa Itu Analisis Modal Sistem Tenaga?", "id": "Menganalisis Mode Osilasi Sistem." },
  { "en": "Apa Itu Faktor Partisipasi?", "id": "Menunjukkan Generator Mana Yang Berpartisipasi Dalam Osilasi." },
  { "en": "Apa Itu Margin Protektif (Protective Margin)?", "id": "Selisih Antara Withstand Voltage Dan Arrester." },
  { "en": "Apa Beda BIL Dan SIL?", "id": "BIL (Basic Insulation Level) Untuk Petir, SIL (Switching Impulse Level) Untuk Switching." },
  { "en": "Apa Itu Pelindung Medan Elektromagnetik (EMF Shielding)?", "id": "Menggunakan Material Konduktif Untuk Memblokir Medan." },
  { "en": "Apa Itu Bagan Operasi Mesin Sinkron?", "id": "Sama Dengan Kurva Kemampuan (Capability Curve)." },
  { "en": "Apa Karakteristik Relay Sangat Invers?", "id": "Waktu Trip Sangat Cepat Pada Arus Tinggi." },
  { "en": "Apa Karakteristik Relay Ekstrem Invers?", "id": "Lebih Cepat Dari Sangat Invers." },
  { "en": "Apa Itu Setting Slope Relay Differensial?", "id": "Mengkompensasi Kesalahan CT (Current Transformer) Dan Tap Trafo." },
  { "en": "Apa Kepanjangan Pst (Short-Term Flicker Severity)?", "id": "Short-Term Flicker Severity." },
  { "en": "Apa Kepanjangan Plt (Long-Term Flicker Severity)?", "id": "Long-Term Flicker Severity." },
  { "en": "Bagaimana Menghitung Faktor Ketidakseimbangan Tegangan?", "id": "Rasio Komponen Urutan Negatif Terhadap Positif." },
  { "en": "Bagaimana Tingkat Polusi Mempengaruhi Desain Isolator?", "id": "Polusi Tinggi Membutuhkan Jarak Rambat Lebih Panjang." },
  { "en": "Mengapa Pagar Gardu Induk Ditanahkan?", "id": "Untuk Mengontrol Tegangan Sentuh Di Sekitarnya." },
  { "en": "Apa Itu Kontrol Umpan Maju (Feedforward)?", "id": "Mengantisipasi Gangguan Sebelum Mempengaruhi Proses." },
  { "en": "Apa Itu Kontrol Decoupling?", "id": "Mengeliminasi Interaksi Antar Variabel Dalam Sistem." },
  { "en": "Apa Kepanjangan SMES (Superconducting Magnetic Energy Storage)?", "id": "Superconducting Magnetic Energy Storage." },
  { "en": "Bagaimana SMES (Superconducting Magnetic Energy Storage) Bekerja?", "id": "Menyimpan Energi Dalam Medan Magnet Superkonduktor." },
  { "en": "Apa Kepanjangan CAES (Compressed Air Energy Storage)?", "id": "Compressed Air Energy Storage." },
  { "en": "Bagaimana CAES (Compressed Air Energy Storage) Bekerja?", "id": "Menyimpan Energi Dalam Bentuk Udara Bertekanan." },
  { "en": "Apa Kepanjangan ETAP (Electrical Transient Analyzer Program)?", "id": "Electrical Transient Analyzer Program." },
  { "en": "Untuk Apa Perangkat Lunak ETAP (Electrical Transient Analyzer Program)?", "id": "Analisis Dan Simulasi Sistem Tenaga Listrik." },
  { "en": "Apa Kepanjangan PSCAD (Power Systems CAD)?", "id": "Power Systems Computer Aided Design." },
  { "en": "Untuk Apa Perangkat Lunak PSCAD (Power Systems CAD)?", "id": "Simulasi Transien Elektromagnetik Detail." },
  { "en": "Apa Itu MATLAB/Simulink?", "id": "Lingkungan Komputasi Numerik Dan Simulasi Grafis." },
  { "en": "Apa Itu Analisis Kontingensi N-1-1?", "id": "Analisis Dua Gangguan Berurutan." },
  { "en": "Apa Itu Pemulihan Sistem (System Restoration)?", "id": "Proses Memulihkan Jaringan Setelah Blackout." },
  { "en": "Apa Itu Jalur Pemulihan (Restoration Path)?", "id": "Urutan Pensaklaran Untuk Menghidupkan Kembali Beban." },
  { "en": "Apa Itu Cold Load Pickup?", "id": "Arus Tinggi Saat Menghidupkan Beban Dingin." },
  { "en": "Apa Itu Hot Load Pickup?", "id": "Arus Saat Menghidupkan Beban Yang Baru Padam." },
  { "en": "Apa Itu Respon Frekuensi Inersia?", "id": "Respon Alami Mesin Berputar Terhadap Perubahan Frekuensi." },
  { "en": "Apa Itu Penurunan Frekuensi (Frequency Droop)?", "id": "Sama Dengan Droop Speed Control." },
  { "en": "Apa Itu Batas Kestabilan Sudut?", "id": "Sudut Rotor Maksimum Sebelum Kehilangan Sinkronisasi." },
  { "en": "Apa Itu Redaman Kritis?", "id": "Sama Dengan Kondisi Critically Damped." },
  { "en": "Apa Itu Osilasi Elektromekanis?", "id": "Ayunan Daya Antar Generator Dalam Sistem." },
  { "en": "Apa Itu Mode Osilasi Lokal?", "id": "Osilasi Antar Satu Generator Dengan Sistem." },
  { "en": "Apa Itu Mode Osilasi Antar Area?", "id": "Osilasi Antar Kelompok Generator Di Area Berbeda." },
  { "en": "Apa Itu Pengujian Saturasi Inti Besi?", "id": "Mengukur Karakteristik Magnetisasi Inti CT (Current Transformer) Atau Trafo." },
  { "en": "Apa Itu Burden CT (Current Transformer)?", "id": "Total Beban (Impedansi) Terhubung Ke Sekunder CT (Current Transformer)." },
  { "en": "Apa Itu Kelas Akurasi CT (Current Transformer)?", "id": "Menunjukkan Tingkat Akurasi Pengukuran CT (Current Transformer)." },
  { "en": "Apa Itu Faktor Keamanan Instrumen (ISF)?", "id": "Melindungi Instrumen Ukur Dari Arus Gangguan." },
  { "en": "Apa Kepanjangan ISF (Instrument Security Factor)?", "id": "Instrument Security Factor." },
  { "en": "Apa Itu Kelas Akurasi Proteksi?", "id": "Contoh: 5P10, 10P20." },
  { "en": "Apa Arti Notasi 5P10?", "id": "Kesalahan 5% Pada 10 Kali Arus Nominal." },
  { "en": "Apa Itu Knee Point Voltage?", "id": "Titik Jenuh Pada Kurva Eksitasi CT (Current Transformer)." },
  { "en": "Apa Itu Uji Rasio PT (Potential Transformer)?", "id": "Memverifikasi Rasio Tegangan PT (Potential Transformer)." },
  { "en": "Apa Itu Faktor Koreksi Tegangan (VCF)?", "id": "Faktor Koreksi Untuk PT (Potential Transformer) Akibat Beban." },
  { "en": "Apa Itu Ferroresonansi Pada PT (Potential Transformer)?", "id": "Osilasi Berbahaya Akibat Interaksi Kapasitif-Induktif." },
  { "en": "Apa Itu Rangkaian Anti-Ferroresonansi?", "id": "Rangkaian Resistor Untuk Meredam Ferroresonansi." },
  { "en": "Apa Itu Pembumian Titik Bintang?", "id": "Menghubungkan Titik Netral Trafo Atau Generator Ke Tanah." },
  { "en": "Apa Itu Tegangan Netral Bergeser?", "id": "Tegangan Muncul Di Titik Netral." },
  { "en": "Apa Itu Arus Urutan Nol?", "id": "Jumlah Vektor Arus Tiga Fasa." },
  { "en": "Kapan Arus Urutan Nol Muncul?", "id": "Hanya Selama Gangguan Yang Melibatkan Tanah." },
  { "en": "Apa Itu Jaringan Urutan Nol?", "id": "Model Rangkaian Untuk Analisis Arus Urutan Nol." },
  { "en": "Apa Itu Kopling Urutan Nol?", "id": "Induksi Timbal Balik Antar Saluran Paralel." },
  { "en": "Apa Itu Proteksi Tanah Sensitif (SEF)?", "id": "Mendeteksi Arus Gangguan Tanah Sangat Kecil." },
  { "en": "Apa Kepanjangan SEF (Sensitive Earth Fault)?", "id": "Sensitive Earth Fault." },
  { "en": "Apa Itu Proteksi Kegagalan Kutub (Pole Discrepancy)?", "id": "Mendeteksi Jika Tidak Semua Kutub Pemutus Membuka." },
  { "en": "Apa Itu Skema Cincin Terbuka?", "id": "Jaringan Cincin Yang Normalnya Dioperasikan Terbuka." },
  { "en": "Apa Itu Titik Normally Open (NO)?", "id": "Titik Pemisah Pada Jaringan Cincin Terbuka." },
  { "en": "Apa Itu Transfer Beban Otomatis?", "id": "Memindahkan Pasokan Beban Ke Sumber Lain." },
  { "en": "Apa Itu Motor Sinkron Self-Controlled?", "id": "Kecepatan Dikontrol Oleh Frekuensi Inverter." },
  { "en": "Apa Itu Penggerak Cycloconverter?", "id": "VFD (Variable Frequency Drive) Untuk Motor Daya Sangat Besar." },
  { "en": "Apa Itu Penggerak Kramer?", "id": "Metode Kontrol Kecepatan Motor Induksi Slip-Ring." },
  { "en": "Apa Itu Penggerak Scherbius?", "id": "Metode Kontrol Kecepatan Motor Induksi Slip-Ring Lainnya." },
  { "en": "Apa Itu Motor Enggan (Reluctance Motor)?", "id": "Sama Dengan Motor Reluktansi." },
  { "en": "Apa Itu Torsi Reluktansi?", "id": "Torsi Yang Dihasilkan Akibat Perubahan Reluktansi." },
  { "en": "Apa Itu Motor Hibrida?", "id": "Menggabungkan Beberapa Teknologi Motor (Contoh: Stepper-PM)." },
  { "en": "Apa Itu Enkoder Absolut?", "id": "Memberikan Informasi Posisi Absolut." },
  { "en": "Apa Itu Enkoder Inkremental?", "id": "Memberikan Pulsa Relatif Terhadap Gerakan." },
  { "en": "Apa Itu Resolusi Enkoder?", "id": "Jumlah Pulsa Per Putaran." },
  { "en": "Apa Itu Saluran Serat Optik Di Atas Tanah?", "id": "Kabel Serat Optik Di Jaringan Udara." },
  { "en": "Apa Kepanjangan ADSS (All-Dielectric Self-Supporting)?", "id": "All-Dielectric Self-Supporting." },
  { "en": "Apa Itu Kabel ADSS (All-Dielectric Self-Supporting)?", "id": "Kabel Serat Optik Non-Logam Untuk Jaringan Udara." },
  { "en": "Apa Itu Fusi Serat Optik?", "id": "Sama Dengan Fusion Splicing." },
  { "en": "Apa Itu Atenuasi Sambungan (Splice Loss)?", "id": "Kehilangan Sinyal Pada Titik Sambungan Serat." },
  { "en": "Apa Itu Refleksi Balik (Backreflection)?", "id": "Cahaya Yang Dipantulkan Kembali Ke Sumber." },
  { "en": "Apa Itu Konektor LC?", "id": "Jenis Konektor Serat Optik Ukuran Kecil." },
  { "en": "Apa Itu Konektor SC?", "id": "Jenis Konektor Serat Optik Umum." },
  { "en": "Apa Itu Patch Panel Serat Optik?", "id": "Panel Terminasi Untuk Kabel Serat Optik." },
  { "en": "Apa Itu Pigtail Serat Optik?", "id": "Sepotong Kabel Dengan Konektor Di Satu Sisi." },
  { "en": "Apa Itu Multiplexer Pembagi Panjang Gelombang (WDM)?", "id": "Mengirimkan Beberapa Sinyal Cahaya Berbeda." },
  { "en": "Apa Kepanjangan WDM (Wavelength Division Multiplexing)?", "id": "Wavelength Division Multiplexing." },
  { "en": "Apa Itu DWDM (Dense WDM)?", "id": "WDM (Wavelength Division Multiplexing) Dengan Kapasitas Sangat Tinggi." },
  { "en": "Apa Itu Amplifier Serat Optik?", "id": "Menguatkan Sinyal Optik Tanpa Konversi Listrik." },
  { "en": "Apa Kepanjangan EDFA (Erbium-Doped Fiber Amplifier)?", "id": "Erbium-Doped Fiber Amplifier." },
  { "en": "Apa Itu Dispersi Kromatik?", "id": "Penyebaran Pulsa Optik Akibat Perbedaan Kecepatan." },
  { "en": "Apa Itu Dispersi Mode Polarisasi (PMD)?", "id": "Penyebaran Pulsa Akibat Perbedaan Kecepatan Polarisasi." },
  { "en": "Apa Kepanjangan PMD (Polarization Mode Dispersion)?", "id": "Polarization Mode Dispersion." },
  { "en": "Apa Itu Jaringan Optik Pasif (PON)?", "id": "Jaringan Serat Optik Tanpa Komponen Aktif." },
  { "en": "Apa Kepanjangan PON (Passive Optical Network)?", "id": "Passive Optical Network." },
  { "en": "Apa Itu Terminal Jalur Optik (OLT)?", "id": "Peralatan Pusat Dalam Jaringan PON (Passive Optical Network)." },
  { "en": "Apa Itu Unit Jaringan Optik (ONU)?", "id": "Peralatan Di Sisi Pelanggan Jaringan PON (Passive Optical Network)." },
  { "en": "Apa Itu Power Meter Optik?", "id": "Mengukur Kekuatan Sinyal Cahaya." },
  { "en": "Apa Itu Sumber Cahaya Optik?", "id": "Sumber Cahaya Stabil Untuk Pengujian." },
  { "en": "Apa Itu Visual Fault Locator (VFL)?", "id": "Menggunakan Laser Merah Mencari Kerusakan Serat." },
  { "en": "Apa Kepanjangan OPF (Optimal Power Flow)?", "id": "Optimal Power Flow." },
  { "en": "Apa Tujuan OPF (Optimal Power Flow)?", "id": "Menjadwalkan Pembangkitan Daya Paling Ekonomis." },
  { "en": "Apa Kepanjangan SCOPF (Security-Constrained OPF)?", "id": "Security-Constrained Optimal Power Flow." },
  { "en": "Apa Itu Estimasi Keadaan (State Estimation)?", "id": "Memperkirakan Kondisi Sistem Tenaga Real-Time." },
  { "en": "Apa Itu Weeping Pada Isolator?", "id": "Jejak Karbon Konduktif Pada Permukaan Isolator." },
  { "en": "Apa Itu Hidrofobisitas Isolator?", "id": "Kemampuan Permukaan Isolator Menolak Air." },
  { "en": "Apa Analisis E-Field Dan V-Field?", "id": "Analisis Distribusi Medan Listrik Dan Potensial." },
  { "en": "Apa Itu Metode Bola Bergulir?", "id": "Metode Desain Zona Proteksi Penangkal Petir." },
  { "en": "Apa Itu Metode Sudut Pelindung?", "id": "Metode Desain Penangkal Petir Sederhana." },
  { "en": "Apa Kepanjangan ESE (Early Streamer Emission)?", "id": "Early Streamer Emission." },
  { "en": "Apa Itu Penangkal Petir ESE (Early Streamer Emission)?", "id": "Terminal Udara Yang Aktif Mengeluarkan Streamer." },
  { "en": "Apa Kepanjangan MMS (Manufacturing Message Specification)?", "id": "Manufacturing Message Specification." },
  { "en": "Apa Fungsi Protokol MMS (Manufacturing Message Specification)?", "id": "Komunikasi Klien-Server Dalam Otomasi Gardu." },
  { "en": "Apa Itu Inverter Z-Source?", "id": "Topologi Inverter Yang Dapat Menaikkan Tegangan." },
  { "en": "Apa Kepanjangan LLC (Induktor-Induktor-Kapasitor) Resonant Converter?", "id": "Inductor-Inductor-Capacitor Resonant Converter." },
  { "en": "Apa Informasi Pada Label Arc Flash?", "id": "Batas Flash, Energi Insiden, Kategori PPE (Personal Protective Equipment)." },
  { "en": "Apa Kepanjangan EEWP (Energized Electrical Work Permit)?", "id": "Energized Electrical Work Permit." },
  { "en": "Kapan EEWP (Energized Electrical Work Permit) Diperlukan?", "id": "Saat Bekerja Pada Peralatan Listrik Bertegangan." },
  { "en": "Apa Beda Risiko Dan Bahaya?", "id": "Bahaya Adalah Sumber, Risiko Adalah Kemungkinan." },
  { "en": "Apa Itu Manajemen Armada Trafo?", "id": "Strategi Mengelola Sekelompok Besar Transformator." },
  { "en": "Apa Kepanjangan RCAM (Reliability-Centered Asset Management)?", "id": "Reliability-Centered Asset Management." },
  { "en": "Apa Itu Penilaian Akhir Umur (End-of-Life)?", "id": "Evaluasi Untuk Menentukan Waktu Penggantian Aset." },
  { "en": "Apa Respon Inersia Dari Turbin Angin?", "id": "Emulasi Inersia Menggunakan Kontrol Power Electronics." },
  { "en": "Apa Kepanjangan PPA (Power Purchase Agreement)?", "id": "Power Purchase Agreement." },
  { "en": "Apa Itu PPA (Power Purchase Agreement)?", "id": "Kontrak Jual Beli Listrik Jangka Panjang." },
  { "en": "Apa Kepanjangan LCOE (Levelized Cost of Energy)?", "id": "Levelized Cost of Energy." },
  { "en": "Apa Yang Diukur LCOE (Levelized Cost of Energy)?", "id": "Biaya Energi Rata-rata Seumur Hidup Pembangkit." },
  { "en": "Apa Itu Pengisian Daya EV (Electric Vehicle) Level 1?", "id": "Menggunakan Stop Kontak Rumah Tangga Standar." },
  { "en": "Apa Itu Pengisian Daya EV (Electric Vehicle) Level 2?", "id": "Menggunakan Suplai AC (Alternating Current) 240 Volt." },
  { "en": "Apa Itu Pengisian Daya Cepat DC (Direct Current)?", "id": "Mengisi Baterai EV (Electric Vehicle) Dengan Cepat." },
  { "en": "Apa Kepanjangan BMS (Battery Management System)?", "id": "Battery Management System." },
  { "en": "Apa Fungsi BMS (Battery Management System)?", "id": "Melindungi Dan Mengelola Sel Baterai." },
  { "en": "Apa Kepanjangan SOC (State of Charge)?", "id": "State of Charge." },
  { "en": "Apa Yang Diukur SOC (State of Charge)?", "id": "Tingkat Keterisian Baterai Saat Ini." },
  { "en": "Apa Kepanjangan SOH (State of Health)?", "id": "State of Health." },
  { "en": "Apa Yang Diukur SOH (State of Health)?", "id": "Kondisi Degradasi Baterai Seiring Waktu." },
  { "en": "Apa Kepanjangan RCFA (Root Cause Failure Analysis)?", "id": "Root Cause Failure Analysis." },
  { "en": "Apa Itu Metode 5 Whys?", "id": "Teknik Bertanya 'Mengapa' Untuk Menemukan Akar Masalah." },
  { "en": "Apa Itu Diagram Gantt?", "id": "Bagan Batang Untuk Visualisasi Jadwal Proyek." },
  { "en": "Apa Kepanjangan PERT (Program Evaluation and Review Technique)?", "id": "Program Evaluation and Review Technique." },
  { "en": "Apa Beda Diagram Gantt Dan PERT?", "id": "PERT (Program Evaluation and Review Technique) Fokus Pada Ketergantungan Tugas." },
  { "en": "Apa Itu Jaringan AON (Activity-on-Node)?", "id": "Representasi Jaringan Proyek Dimana Tugas Adalah Node." },
  { "en": "Apa Itu Jaringan AOA (Activity-on-Arrow)?", "id": "Representasi Jaringan Proyek Dimana Tugas Adalah Panah." },
  { "en": "Apa Itu Float Atau Slack Dalam Proyek?", "id": "Jumlah Waktu Tunda Tanpa Mempengaruhi Proyek." },
  { "en": "Apa Itu Fast Tracking Proyek?", "id": "Mengerjakan Tugas Paralel Untuk Mempercepat." },
  { "en": "Apa Itu Crashing Proyek?", "id": "Menambah Sumber Daya Untuk Mempercepat Proyek." },
  { "en": "Apa Kepanjangan WBS (Work Breakdown Structure)?", "id": "Work Breakdown Structure." },
  { "en": "Apa Fungsi WBS (Work Breakdown Structure)?", "id": "Memecah Proyek Menjadi Bagian Lebih Kecil." },
  { "en": "Apa Itu Lingkup Proyek (Project Scope)?", "id": "Totalitas Pekerjaan Yang Perlu Dilakukan." },
  { "en": "Apa Itu Scope Creep?", "id": "Penambahan Fitur Atau Fungsi Tak Terkendali." },
  { "en": "Apa Itu Analisis Stakeholder?", "id": "Mengidentifikasi Dan Menganalisis Pihak Berkepentingan." },
  { "en": "Apa Itu Matriks RACI?", "id": "Mendefinisikan Peran (Responsible, Accountable, Consulted, Informed)." },
  { "en": "Apa Kepanjangan RACI?", "id": "Responsible, Accountable, Consulted, and Informed." },
  { "en": "Apa Itu Earned Value Management (EVM)?", "id": "Metode Pengukuran Kinerja Proyek." },
  { "en": "Apa Kepanjangan EVM (Earned Value Management)?", "id": "Earned Value Management." },
  { "en": "Apa Kepanjangan SPI (Schedule Performance Index)?", "id": "Schedule Performance Index." },
  { "en": "Apa Kepanjangan CPI (Cost Performance Index)?", "id": "Cost Performance Index." },
  { "en": "Apa Itu Lessons Learned?", "id": "Pengetahuan Yang Diperoleh Dari Pengalaman Proyek." },
  { "en": "Apa Itu Studi Kelayakan (Feasibility Study)?", "id": "Evaluasi Praktikalitas Proyek Yang Diusulkan." },
  { "en": "Apa Itu Analisis SWOT?", "id": "Analisis Strengths, Weaknesses, Opportunities, Threats." },
  { "en": "Apa Kepanjangan SWOT?", "id": "Strengths, Weaknesses, Opportunities, and Threats." },
  { "en": "Apa Itu Indeks Profitabilitas (PI)?", "id": "Ukuran Daya Tarik Investasi Proyek." },
  { "en": "Apa Itu Tingkat Pengembalian Internal (IRR)?", "id": "Tingkat Diskonto Dimana NPV (Net Present Value) Sama Dengan Nol." },
  { "en": "Apa Kepanjangan IRR (Internal Rate of Return)?", "id": "Internal Rate of Return." },
  { "en": "Apa Itu Nilai Sekarang Bersih (NPV)?", "id": "Selisih Antara Arus Kas Masuk-Keluar." },
  { "en": "Apa Kepanjangan NPV (Net Present Value)?", "id": "Net Present Value." },
  { "en": "Apa Itu Periode Pengembalian (Payback Period)?", "id": "Waktu Yang Dibutuhkan Untuk Mengembalikan Investasi." },
  { "en": "Apa Itu Biaya Peluang (Opportunity Cost)?", "id": "Manfaat Yang Hilang Dari Alternatif Terbaik." },
  { "en": "Apa Itu Sunk Cost?", "id": "Biaya Yang Telah Terjadi Dan Tidak Dapat Dikembalikan." },
  { "en": "Apa Itu Komisioning Bertahap (Phased Commissioning)?", "id": "Mengaktifkan Dan Menguji Sistem Secara Bertahap." },
  { "en": "Apa Itu Handover Proyek?", "id": "Penyerahan Resmi Proyek Yang Selesai." },
  { "en": "Apa Itu Close-Out Proyek?", "id": "Proses Finalisasi Semua Aktivitas Proyek." },
  { "en": "Apa Itu Post-Mortem Proyek?", "id": "Tinjauan Setelah Proyek Selesai." },
  { "en": "Apa Itu Toleransi Gangguan (Fault Tolerance)?", "id": "Kemampuan Sistem Beroperasi Meski Ada Kegagalan." },
  { "en": "Apa Itu Graceful Degradation?", "id": "Penurunan Kinerja Sistem Secara Bertahap." },
  { "en": "Apa Itu Failover?", "id": "Proses Peralihan Otomatis Ke Sistem Cadangan." },
  { "en": "Apa Itu Switchover?", "id": "Proses Peralihan Manual Ke Sistem Cadangan." },
  { "en": "Apa Itu Titik Kegagalan Tunggal (SPOF)?", "id": "Komponen Yang Kegagalannya Menyebabkan Sistem Gagal." },
  { "en": "Apa Kepanjangan SPOF (Single Point of Failure)?", "id": "Single Point of Failure." },
  { "en": "Apa Itu Redundansi N+1?", "id": "Menyediakan Satu Komponen Cadangan Lebih." },
  { "en": "Apa Itu Redundansi 2N?", "id": "Sistem Cermin Penuh (Sepenuhnya Redundan)." },
  { "en": "Apa Itu Georedundansi?", "id": "Redundansi Sistem Di Lokasi Geografis Berbeda." },
  { "en": "Apa Itu Load Balancing?", "id": "Mendistribusikan Beban Kerja Secara Merata." },
  { "en": "Apa Itu Skalabilitas?", "id": "Kemampuan Sistem Menangani Peningkatan Beban." },
  { "en": "Apa Itu Skalabilitas Vertikal (Scale-Up)?", "id": "Menambah Kekuatan Pada Satu Server." },
  { "en": "Apa Itu Skalabilitas Horisontal (Scale-Out)?", "id": "Menambah Lebih Banyak Server Ke Sistem." },
  { "en": "Apa Itu Throughput?", "id": "Laju Data Yang Berhasil Diproses." },
  { "en": "Apa Itu Latensi?", "id": "Waktu Tunda Dalam Sistem." },
  { "en": "Apa Itu High-Availability Cluster?", "id": "Sekelompok Server Bekerja Sama Memberikan Ketersediaan Tinggi." },
  { "en": "Apa Itu Disaster Recovery Plan (DRP)?", "id": "Rencana Pemulihan Setelah Bencana." },
  { "en": "Apa Kepanjangan DRP (Disaster Recovery Plan)?", "id": "Disaster Recovery Plan." },
  { "en": "Apa Itu Business Continuity Plan (BCP)?", "id": "Rencana Menjaga Fungsi Bisnis Saat Gangguan." },
  { "en": "Apa Kepanjangan BCP (Business Continuity Plan)?", "id": "Business Continuity Plan." },
  { "en": "Apa Itu Recovery Time Objective (RTO)?", "id": "Waktu Maksimum Yang Ditargetkan Untuk Pemulihan." },
  { "en": "Apa Kepanjangan RTO (Recovery Time Objective)?", "id": "Recovery Time Objective." },
  { "en": "Apa Itu Recovery Point Objective (RPO)?", "id": "Toleransi Kehilangan Data Maksimum." },
  { "en": "Apa Kepanjangan RPO (Recovery Point Objective)?", "id": "Recovery Point Objective." },
  { "en": "Apa Itu Hot Site?", "id": "Situs Pemulihan Bencana Siap Pakai." },
  { "en": "Apa Itu Cold Site?", "id": "Situs Pemulihan Bencana Dengan Infrastruktur Dasar." },
  { "en": "Apa Itu Notching Dalam Kualitas Daya?", "id": "Distorsi Periodik Akibat Operasi Konverter." },
  { "en": "Apa Standar IEC (International Electrotechnical Commission) 61000?", "id": "Standar Terkait Kompatibilitas Elektromagnetik (EMC)." },
  { "en": "Apa Itu Zona Proteksi Petir (LPZ)?", "id": "Area Dengan Tingkat Proteksi Petir Tertentu." },
  { "en": "Apa Kepanjangan LPZ (Lightning Protection Zone)?", "id": "Lightning Protection Zone." },
  { "en": "Apa Metode Koordinasi Isolasi Statistik?", "id": "Berdasarkan Probabilitas Kegagalan Isolasi." },
  { "en": "Apa Model Penuaan Arrhenius?", "id": "Model Prediksi Umur Isolasi Berdasarkan Suhu." },
  { "en": "Apa Itu Relay Adaptif?", "id": "Relay Yang Settingnya Dapat Berubah Otomatis." },
  { "en": "Apa Kepanjangan VSC (Voltage Source Converter)?", "id": "Voltage Source Converter." },
  { "en": "Apa Kepanjangan LCC (Line-Commutated Converter)?", "id": "Line-Commutated Converter." },
  { "en": "Beda VSC Dan LCC HVDC?", "id": "VSC (Voltage Source Converter) Lebih Fleksibel Dan Terkontrol." },
  { "en": "Apa Kepanjangan TCSC (Thyristor Controlled Series Capacitor)?", "id": "Thyristor Controlled Series Capacitor." },
  { "en": "Apa Fungsi TCSC (Thyristor Controlled Series Capacitor)?", "id": "Mengatur Impedansi Saluran Transmisi Secara Dinamis." },
  { "en": "Bagaimana Memisahkan Rugi Histeresis Dan Eddy?", "id": "Melalui Pengujian Pada Frekuensi Berbeda." },
  { "en": "Apa Kelas Isolasi F Pada Motor?", "id": "Batas Suhu 155 Derajat Celsius." },
  { "en": "Apa Kelas Isolasi H Pada Motor?", "id": "Batas Suhu 180 Derajat Celsius." },
  { "en": "Apa Beda Uji On-line Dan Off-line?", "id": "On-line Saat Peralatan Beroperasi, Off-line Tidak." },
  { "en": "Apa Kepanjangan PDC (Polarization/Depolarization Current)?", "id": "Polarization and Depolarization Current." },
  { "en": "Apa Tujuan Analisis PDC (Polarization/Depolarization Current)?", "id": "Mengevaluasi Kandungan Kelembaban Dalam Isolasi." },
  { "en": "Apa Kepanjangan ADA (Advanced Distribution Automation)?", "id": "Advanced Distribution Automation." },
  { "en": "Apa Kepanjangan VVO (Volt/VAR Optimization)?", "id": "Volt/VAR Optimization." },
  { "en": "Apa Fungsi VVO (Volt/VAR Optimization)?", "id": "Mengoptimalkan Profil Tegangan Dan Daya Reaktif." },
  { "en": "Apa Itu Kontroler Microgrid?", "id": "Otak Yang Mengelola Operasi Microgrid." },
  { "en": "Apa Kepanjangan SOC (Security Operations Center)?", "id": "Security Operations Center." },
  { "en": "Apa Fungsi SOC (Security Operations Center)?", "id": "Memantau Dan Merespon Insiden Keamanan Siber." },
  { "en": "Apa Kepanjangan IDS (Intrusion Detection System)?", "id": "Intrusion Detection System." },
  { "en": "Apa Kepanjangan IPS (Intrusion Prevention System)?", "id": "Intrusion Prevention System." },
  { "en": "Beda IDS Dan IPS?", "id": "IDS (Intrusion Detection System) Mendeteksi, IPS (Intrusion Prevention System) Mencegah." },
  { "en": "Apa Kepanjangan NESC (National Electrical Safety Code)?", "id": "National Electrical Safety Code." },
  { "en": "Apa Yang Diatur NESC (National Electrical Safety Code)?", "id": "Keselamatan Instalasi Jaringan Utilitas Listrik." },
  { "en": "Apa Kepanjangan UL (Underwriters Laboratories)?", "id": "Underwriters Laboratories." },
  { "en": "Apa Peran UL (Underwriters Laboratories)?", "id": "Sertifikasi Keamanan Produk." },
  { "en": "Apa Itu SIMO (Single-Input, Multiple-Output)?", "id": "Sistem Dengan Satu Input, Banyak Output." },
  { "en": "Apa Itu MISO (Multiple-Input, Single-Output)?", "id": "Sistem Dengan Banyak Input, Satu Output." },
  { "en": "Apa Itu Interoperabilitas?", "id": "Kemampuan Sistem Berbeda Untuk Bekerja Sama." },
  { "en": "Apa Itu Plug-and-Play?", "id": "Kemampuan Menambah Perangkat Tanpa Konfigurasi Manual." },
  { "en": "Apa Itu Latency Jaringan?", "id": "Waktu Tunda Pengiriman Data." },
  { "en": "Apa Itu Jitter Jaringan?", "id": "Variasi Latensi Jaringan." },
  { "en": "Apa Itu Quality of Service (QoS)?", "id": "Manajemen Lalu Lintas Jaringan Untuk Kinerja." },
  { "en": "Apa Kepanjangan QoS (Quality of Service)?", "id": "Quality of Service." },
  { "en": "Apa Itu Packet Loss?", "id": "Kegagalan Paket Data Mencapai Tujuan." },
  { "en": "Apa Itu Checksum?", "id": "Nilai Untuk Memverifikasi Integritas Data." },
  { "en": "Apa Itu Alamat MAC (Media Access Control)?", "id": "Alamat Fisik Unik Perangkat Jaringan." },
  { "en": "Apa Itu Protokol ARP (Address Resolution Protocol)?", "id": "Memetakan Alamat IP (Internet Protocol) Ke Alamat MAC (Media Access Control)." },
  { "en": "Apa Itu Protokol DHCP (Dynamic Host Configuration Protocol)?", "id": "Memberikan Alamat IP (Internet Protocol) Secara Otomatis." },
  { "en": "Apa Itu DNS (Domain Name System)?", "id": "Menerjemahkan Nama Domain Menjadi Alamat IP (Internet Protocol)." },
  { "en": "Apa Itu Port Jaringan?", "id": "Titik Akhir Komunikasi Dalam Sistem Operasi." },
  { "en": "Apa Itu Socket?", "id": "Kombinasi Alamat IP (Internet Protocol) Dan Nomor Port." },
  { "en": "Apa Itu VPN (Virtual Private Network)?", "id": "Membuat Koneksi Aman Melalui Jaringan Publik." },
  { "en": "Apa Itu Enkripsi?", "id": "Mengubah Data Menjadi Kode Rahasia." },
  { "en": "Apa Itu Dekripsi?", "id": "Mengembalikan Data Terenkripsi Ke Bentuk Asli." },
  { "en": "Apa Itu Kriptografi Kunci Simetris?", "id": "Kunci Yang Sama Untuk Enkripsi-Dekripsi." },
  { "en": "Apa Itu Kriptografi Kunci Asimetris?", "id": "Menggunakan Pasangan Kunci Publik Dan Privat." },
  { "en": "Apa Itu Tanda Tangan Digital?", "id": "Memastikan Keaslian Dan Integritas Data." },
  { "en": "Apa Itu Sertifikat Digital?", "id": "Mengikat Identitas Dengan Kunci Publik." },
  { "en": "Apa Itu Otoritas Sertifikat (CA)?", "id": "Entitas Terpercaya Yang Menerbitkan Sertifikat Digital." },
  { "en": "Apa Itu Protokol SSL/TLS?", "id": "Protokol Keamanan Untuk Komunikasi Internet." },
  { "en": "Apa Kepanjangan SSL (Secure Sockets Layer)?", "id": "Secure Sockets Layer." },
  { "en": "Apa Kepanjangan TLS (Transport Layer Security)?", "id": "Transport Layer Security." },
  { "en": "Apa Itu HTTPS (Hypertext Transfer Protocol Secure)?", "id": "Versi Aman Dari Protokol HTTP (Hypertext Transfer Protocol)." },
  { "en": "Apa Itu Otentikasi?", "id": "Proses Verifikasi Identitas Pengguna." },
  { "en": "Apa Itu Otorisasi?", "id": "Proses Pemberian Hak Akses." },
  { "en": "Apa Itu Otentikasi Dua Faktor (2FA)?", "id": "Membutuhkan Dua Metode Verifikasi Berbeda." },
  { "en": "Apa Itu Biometrik?", "id": "Otentikasi Menggunakan Karakteristik Fisik." },
  { "en": "Apa Itu Honeypot?", "id": "Sistem Umpan Untuk Menjebak Penyerang." },
  { "en": "Apa Itu Penetration Testing?", "id": "Simulasi Serangan Siber Untuk Menguji Keamanan." },
  { "en": "Apa Itu Vulnerability Assessment?", "id": "Proses Identifikasi Kelemahan Keamanan." },
  { "en": "Apa Itu Zero-Day Exploit?", "id": "Serangan Yang Mengeksploitasi Kerentanan Baru." },
  { "en": "Apa Itu Patch Management?", "id": "Proses Mengelola Pembaruan Perangkat Lunak." },
  { "en": "Apa Itu Redundansi Geografis?", "id": "Sama Dengan Georedundansi." },
  { "en": "Apa Itu RPO (Recovery Point Objective) Nol?", "id": "Tidak Ada Toleransi Kehilangan Data Sama Sekali." },
  { "en": "Apa Itu RTO (Recovery Time Objective) Nol?", "id": "Pemulihan Sistem Harus Terjadi Seketika." },
  { "en": "Apa Itu Sinkronisasi Data?", "id": "Proses Menjaga Konsistensi Data Di Beberapa Lokasi." },
  { "en": "Apa Itu Replikasi Data Asinkron?", "id": "Data Disalin Dengan Sedikit Penundaan." },
  { "en": "Apa Itu Replikasi Data Sinkron?", "id": "Data Disalin Secara Real-Time." },
  { "en": "Apa Itu Load Shedding Adaptif?", "id": "Pelepasan Beban Yang Disesuaikan Kondisi Sistem." },
  { "en": "Apa Itu Wide Area Monitoring System (WAMS)?", "id": "Sama Dengan WAMS (Wide Area Measurement System)." },
  { "en": "Apa Itu Remedial Action Scheme (RAS)?", "id": "Skema Otomatis Untuk Mencegah Ketidakstabilan Sistem." },
  { "en": "Apa Kepanjangan RAS (Remedial Action Scheme)?", "id": "Remedial Action Scheme." },
  { "en": "Apa Itu Controlled Islanding?", "id": "Pemisahan Sistem Terencana Untuk Melindungi Area." },
  { "en": "Apa Itu Topologi Jaringan Terdistribusi?", "id": "Jaringan Tanpa Titik Pusat (Peer-to-Peer)." },
  { "en": "Apa Itu Jaringan Ad Hoc?", "id": "Jaringan Nirkabel Sementara Tanpa Infrastruktur." },
  { "en": "Apa Itu Sensor Nirkabel?", "id": "Sensor Yang Berkomunikasi Tanpa Kabel." },
  { "en": "Apa Itu Zigbee?", "id": "Protokol Komunikasi Nirkabel Daya Rendah." },
  { "en": "Apa Itu Bluetooth Low Energy (BLE)?", "id": "Versi Bluetooth Hemat Daya." },
  { "en": "Apa Kepanjangan BLE (Bluetooth Low Energy)?", "id": "Bluetooth Low Energy." },
  { "en": "Apa Itu LoRaWAN?", "id": "Protokol Jaringan Area Luas Daya Rendah." },
  { "en": "Apa Itu NB-IoT (Narrowband IoT)?", "id": "Teknologi Seluler Untuk Perangkat IoT (Internet of Things)." },
  { "en": "Apa Itu Message Queue Telemetry Transport (MQTT)?", "id": "Protokol Pesan Ringan Untuk IoT (Internet of Things)." },
  { "en": "Apa Kepanjangan MQTT?", "id": "Message Queue Telemetry Transport." },
  { "en": "Apa Itu Model Publisher-Subscriber?", "id": "Pola Komunikasi Pesan Yang Terdistribusi." },
  { "en": "Apa Itu Broker MQTT (Message Queue Telemetry Transport)?", "id": "Server Pusat Yang Mengelola Pesan." },
  { "en": "Apa Itu Topik Dalam MQTT (Message Queue Telemetry Transport)?", "id": "Label Untuk Mengkategorikan Pesan." },
  { "en": "Apa Itu Constrained Application Protocol (CoAP)?", "id": "Protokol Ringan Untuk Perangkat Terbatas." },
  { "en": "Apa Itu Digital Signal Processor (DSP)?", "id": "Mikroprosesor Khusus Untuk Pemrosesan Sinyal." },
  { "en": "Apa Kepanjangan DSP (Digital Signal Processor)?", "id": "Digital Signal Processor." },
  { "en": "Apa Itu Filter FIR (Finite Impulse Response)?", "id": "Jenis Filter Digital Tanpa Umpan Balik." },
  { "en": "Apa Itu Filter IIR (Infinite Impulse Response)?", "id": "Jenis Filter Digital Dengan Umpan Balik." },
  { "en": "Apa Itu Matriks Jacobian Dalam Aliran Daya?", "id": "Matriks Turunan Parsial Untuk Solusi Iteratif." },
  { "en": "Apa Itu Sparsity Matriks Jaringan?", "id": "Sebagian Besar Elemen Matriks Bernilai Nol." },
  { "en": "Apa Itu Kegagalan Komutasi Pada LCC (Line-Commutated Converter)?", "id": "Kegagalan Thyristor Mematikan Pada Waktu Tepat." },
  { "en": "Apa Fungsi Filter Pada Stasiun HVDC (High Voltage Direct Current)?", "id": "Mengurangi Harmonisa Yang Diinjeksikan Ke Jaringan AC (Alternating Current)." },
  { "en": "Apa Itu Kontrol Vektor VSC-HVDC?", "id": "Kontrol Independen Daya Aktif Dan Reaktif." },
  { "en": "Apa Kepanjangan LISN (Line Impedance Stabilization Network)?", "id": "Line Impedance Stabilization Network." },
  { "en": "Apa Fungsi LISN (Line Impedance Stabilization Network)?", "id": "Menyediakan Impedansi Terdefinisi Untuk Uji Emisi." },
  { "en": "Apa Itu Filter Aktif Shunt?", "id": "Menginjeksikan Arus Kompensasi Secara Paralel." },
  { "en": "Apa Itu Filter Aktif Seri?", "id": "Menginjeksikan Tegangan Kompensasi Secara Seri." },
  { "en": "Beda STATCOM Dan SVC?", "id": "STATCOM (Static Synchronous Compensator) Berbasis VSC (Voltage Source Converter), Respon Lebih Cepat." },
  { "en": "Apa Itu Motor Selsyn?", "id": "Motor Sinkron Untuk Transmisi Posisi Sudut." },
  { "en": "Apa Beda Stepper Magnet Permanen Dan VR?", "id": "PM (Permanent Magnet) Punya Torsi Penahan, VR (Variable Reluctance) Tidak." },
  { "en": "Apa Itu Sinkronisasi Waktu NTP (Network Time Protocol)?", "id": "Protokol Untuk Sinkronisasi Jam Di Jaringan Komputer." },
  { "en": "Apa Itu Sinkronisasi Waktu PTP (Precision Time Protocol)?", "id": "Protokol Sinkronisasi Waktu Sangat Akurat." },
  { "en": "Apa Itu Sinyal Waktu IRIG-B?", "id": "Format Kode Waktu Standar Untuk Sinkronisasi." },
  { "en": "Apa Akibat Reclosing Pada Gangguan Permanen?", "id": "Menimbulkan Stres Tambahan Pada Peralatan." },
  { "en": "Apa Kepanjangan SOE (Sequence of Events)?", "id": "Sequence of Events." },
  { "en": "Apa Fungsi Perekaman SOE (Sequence of Events)?", "id": "Mencatat Urutan Kejadian Dengan Resolusi Milidetik." },
  { "en": "Apa Kepanjangan DFR (Disturbance Fault Recorder)?", "id": "Disturbance Fault Recorder." },
  { "en": "Apa Itu Nanodielektrik?", "id": "Material Isolasi Yang Diisi Partikel Nano." },
  { "en": "Apa Itu Polimer Self-Healing?", "id": "Polimer Yang Dapat Memperbaiki Kerusakan Sendiri." },
  { "en": "Apa Itu Nanopartikel Magnetik?", "id": "Partikel Magnetik Berukuran Sangat Kecil." },
  { "en": "Apa Beda Faktor Kapasitas Dan Ketersediaan?", "id": "Ketersediaan Hanya Mempertimbangkan Waktu Siap Operasi." },
  { "en": "Apa Itu Pasar Cadangan Berputar (Spinning Reserve)?", "id": "Pasar Untuk Kapasitas Cadangan Tersinkronisasi." },
  { "en": "Apa Itu Fungsi Jendela (Windowing Function)?", "id": "Mengurangi Diskontinuitas Di Tepi Sinyal." },
  { "en": "Apa Itu Jendela Hamming?", "id": "Jenis Fungsi Jendela Untuk Analisis Spektral." },
  { "en": "Apa Itu Teorema Sampling?", "id": "Sama Dengan Teorema Nyquist." },
  { "en": "Apa Itu Ketidakpastian Tipe A?", "id": "Evaluasi Ketidakpastian Menggunakan Metode Statistik." },
  { "en": "Apa Itu Ketidakpastian Tipe B?", "id": "Evaluasi Ketidakpastian Dari Informasi Lain." },
  { "en": "Apa Itu Derau Putih (White Noise)?", "id": "Sinyal Acak Dengan Kepadatan Spektral Datar." },
  { "en": "Apa Itu Derau Merah Jambu (Pink Noise)?", "id": "Kepadatan Spektral Berkurang Seiring Frekuensi." },
  { "en": "Apa Itu Sinyal-ke-Derau (Signal-to-Noise Ratio)?", "id": "Rasio Kekuatan Sinyal Terhadap Kekuatan Derau." },
  { "en": "Apa Kepanjangan SNR (Signal-to-Noise Ratio)?", "id": "Signal-to-Noise Ratio." },
  { "en": "Apa Itu Angka Merit (Figure of Merit)?", "id": "Parameter Kuantitatif Untuk Kinerja Perangkat." },
  { "en": "Apa Itu Sistem Waktu Nyata (Real-Time System)?", "id": "Sistem Komputasi Dengan Batas Waktu Ketat." },
  { "en": "Apa Itu Sistem Waktu Nyata Keras?", "id": "Kegagalan Memenuhi Batas Waktu Menyebabkan Kegagalan Total." },
  { "en": "Apa Itu Sistem Waktu Nyata Lunak?", "id": "Toleransi Terhadap Keterlambatan Batas Waktu." },
  { "en": "Apa Itu Penjadwalan Preemptif?", "id": "Tugas Prioritas Lebih Tinggi Dapat Menginterupsi." },
  { "en": "Apa Itu Penjadwalan Non-Preemptif?", "id": "Tugas Berjalan Sampai Selesai Tanpa Interupsi." },
  { "en": "Apa Itu Inversi Prioritas?", "id": "Masalah Dimana Tugas Prioritas Rendah Menghalangi Tinggi." },
  { "en": "Apa Itu Mutex (Mutual Exclusion)?", "id": "Mekanisme Pencegahan Akses Simultan Ke Sumber Daya." },
  { "en": "Apa Itu Semaphore?", "id": "Variabel Untuk Mengontrol Akses Ke Sumber Daya." },
  { "en": "Apa Itu Deadlock?", "id": "Dua Atau Lebih Proses Saling Menunggu." },
  { "en": "Apa Itu Deteksi Tepi (Edge Detection)?", "id": "Mendeteksi Perubahan Sinyal Dari Rendah Ke Tinggi." },
  { "en": "Apa Itu Debouncing?", "id": "Menghilangkan Sinyal Palsu Dari Saklar Mekanis." },
  { "en": "Apa Itu Pengalamatan Memori?", "id": "Metode Penunjukan Lokasi Memori Spesifik." },
  { "en": "Apa Itu Peta Memori (Memory Map)?", "id": "Alokasi Alamat Untuk Perangkat Keras." },
  { "en": "Apa Itu DMA (Direct Memory Access)?", "id": "Transfer Data Tanpa Melibatkan CPU (Central Processing Unit)." },
  { "en": "Apa Itu Interupsi?", "id": "Sinyal Yang Menghentikan Sementara Eksekusi Program." },
  { "en": "Apa Itu Vektor Interupsi?", "id": "Alamat Rutin Layanan Interupsi." },
  { "en": "Apa Itu Interrupt Service Routine (ISR)?", "id": "Kode Yang Dieksekusi Saat Interupsi Terjadi." },
  { "en": "Apa Itu Latensi Interupsi?", "id": "Waktu Tunda Antara Sinyal Interupsi-Eksekusi ISR (Interrupt Service Routine)." },
  { "en": "Apa Itu Non-Maskable Interrupt (NMI)?", "id": "Interupsi Prioritas Tinggi Yang Tidak Bisa Diabaikan." },
  { "en": "Apa Itu Arsitektur Von Neumann?", "id": "Memori Tunggal Untuk Instruksi Dan Data." },
  { "en": "Apa Itu Arsitektur Harvard?", "id": "Memori Terpisah Untuk Instruksi Dan Data." },
  { "en": "Apa Itu Pipelining Instruksi?", "id": "Teknik Untuk Meningkatkan Throughput Instruksi." },
  { "en": "Apa Itu Cache Memori?", "id": "Memori Kecil Dan Cepat." },
  { "en": "Apa Itu Cache Hit?", "id": "Saat Data Yang Diminta Ditemukan Di Cache." },
  { "en": "Apa Itu Cache Miss?", "id": "Saat Data Yang Diminta Tidak Ditemukan Di Cache." },
  { "en": "Apa Itu Memori Virtual?", "id": "Teknik Manajemen Memori." },
  { "en": "Apa Itu Page Fault?", "id": "Interupsi Saat Data Tidak Ada Di Memori Fisik." },
  { "en": "Apa Itu Unit Manajemen Memori (MMU)?", "id": "Perangkat Keras Untuk Manajemen Memori Virtual." },
  { "en": "Apa Kepanjangan MMU (Memory Management Unit)?", "id": "Memory Management Unit." },
  { "en": "Apa Itu Endianness?", "id": "Urutan Byte Dalam Memori Komputer." },
  { "en": "Apa Itu Little-Endian?", "id": "Byte Paling Tidak Signifikan Disimpan Pertama." },
  { "en": "Apa Itu Big-Endian?", "id": "Byte Paling Signifikan Disimpan Pertama." },
  { "en": "Apa Itu Floating-Point Unit (FPU)?", "id": "Koprosesor Untuk Aritmatika Floating-Point." },
  { "en": "Apa Kepanjangan FPU (Floating-Point Unit)?", "id": "Floating-Point Unit." },
  { "en": "Apa Itu CISC (Complex Instruction Set Computer)?", "id": "Arsitektur Dengan Instruksi Kompleks." },
  { "en": "Apa Itu RISC (Reduced Instruction Set Computer)?", "id": "Arsitektur Dengan Instruksi Sederhana." },
  { "en": "Apa Itu Bahasa Assembly?", "id": "Bahasa Pemrograman Tingkat Rendah." },
  { "en": "Apa Itu Compiler?", "id": "Menerjemahkan Kode Sumber Ke Kode Mesin." },
  { "en": "Apa Itu Interpreter?", "id": "Mengeksekusi Kode Sumber Baris Per Baris." },
  { "en": "Apa Itu Linker?", "id": "Menggabungkan File Objek Menjadi File Eksekusi." },
  { "en": "Apa Itu Loader?", "id": "Memuat Program Ke Memori Untuk Eksekusi." },
  { "en": "Apa Itu Debugger?", "id": "Alat Untuk Menemukan Bug Dalam Program." },
  { "en": "Apa Itu Emulator?", "id": "Meniru Perangkat Keras Dalam Perangkat Lunak." },
  { "en": "Apa Itu Simulator?", "id": "Meniru Perilaku Sistem." },
  { "en": "Apa Itu Verifikasi Formal?", "id": "Membuktikan Kebenaran Sistem Menggunakan Matematika." },
  { "en": "Apa Kepanjangan VHDL (VHSIC Hardware Description Language)?", "id": "VHSIC Hardware Description Language." },
  { "en": "Apa Itu Verilog?", "id": "Bahasa Deskripsi Perangkat Keras Lainnya." },
  { "en": "Apa Itu Sintesis Logika?", "id": "Proses Mengubah HDL (Hardware Description Language) Menjadi Gerbang Logika." },
  { "en": "Apa Itu Analisis Waktu Statis (Static Timing Analysis)?", "id": "Menganalisis Waktu Sirkuit Tanpa Simulasi." },
  { "en": "Apa Itu Desain Untuk Uji (Design for Test)?", "id": "Metodologi Desain Untuk Memudahkan Pengujian." },
  { "en": "Apa Kepanjangan DFT (Design for Test)?", "id": "Design for Test." },
  { "en": "Apa Itu Boundary Scan?", "id": "Teknik Pengujian Untuk PCB (Printed Circuit Board)." },
  { "en": "Apa Itu Automatic Test Pattern Generation (ATPG)?", "id": "Pembuatan Pola Tes Secara Otomatis." },
  { "en": "Apa Kepanjangan ATPG?", "id": "Automatic Test Pattern Generation." },
  { "en": "Apa Itu Built-In Self-Test (BIST)?", "id": "Sirkuit Yang Mampu Menguji Dirinya Sendiri." },
  { "en": "Apa Kepanjangan BIST?", "id": "Built-In Self-Test." },
  { "en": "Apa Itu Yield Manufaktur?", "id": "Persentase Produk Fungsional Dari Total Produksi." },
  { "en": "Apa Itu Fab (Fabrication Plant)?", "id": "Pabrik Pembuatan Semikonduktor." },
  { "en": "Apa Itu Wafer Silikon?", "id": "Cakram Tipis Silikon Untuk Sirkuit Terpadu." },
  { "en": "Apa Itu Die?", "id": "Satu Sirkuit Terpadu Individual Di Wafer." },
  { "en": "Apa Itu Photolithography?", "id": "Proses Transfer Pola Ke Wafer." },
  { "en": "Apa Itu Doping?", "id": "Proses Menambahkan Impuritas Ke Semikonduktor." },
  { "en": "Apa Itu Derajat Kebebasan (DOF) Robot?", "id": "Jumlah Sumbu Gerak Independen Robot." },
  { "en": "Apa Kepanjangan DOF (Degrees of Freedom)?", "id": "Degrees of Freedom." },
  { "en": "Apa Beda Kinematika Maju Dan Mundur?", "id": "Maju Menghitung Posisi, Mundur Menghitung Sudut." },
  { "en": "Apa Itu End Effector?", "id": "Alat Di Ujung Lengan Robot." },
  { "en": "Kapan Servo Lebih Baik Dari Stepper?", "id": "Saat Dibutuhkan Kontrol Posisi Loop Tertutup." },
  { "en": "Apa Kepanjangan LOLP (Loss of Load Probability)?", "id": "Loss of Load Probability." },
  { "en": "Apa Yang Diukur LOLP (Loss of Load Probability)?", "id": "Probabilitas Beban Melebihi Kapasitas Pembangkitan." },
  { "en": "Apa Kepanjangan LOLE (Loss of Load Expectation)?", "id": "Loss of Load Expectation." },
  { "en": "Apa Satuan LOLE (Loss of Load Expectation)?", "id": "Jam Per Tahun Atau Hari Per Tahun." },
  { "en": "Apa Kepanjangan EENS (Expected Energy Not Supplied)?", "id": "Expected Energy Not Supplied." },
  { "en": "Apa Itu Polarisasi Gelombang Linier?", "id": "Medan Listrik Berosilasi Dalam Satu Garis." },
  { "en": "Apa Itu Polarisasi Gelombang Sirkular?", "id": "Ujung Vektor Medan Listrik Membentuk Lingkaran." },
  { "en": "Apa Itu Mode TE (Transverse Electric)?", "id": "Medan Listrik Tegak Lurus Arah Propagasi." },
  { "en": "Apa Itu Mode TM (Transverse Magnetic)?", "id": "Medan Magnet Tegak Lurus Arah Propagasi." },
  { "en": "Bagaimana Penampakan Korona Positif?", "id": "Cahaya Merata Kebiruan (Glow)." },
  { "en": "Bagaimana Penampakan Korona Negatif?", "id": "Bintik-Bintik Kemerahan Dekat Konduktor." },
  { "en": "Berapa Kekuatan Dielektrik Gas SF6?", "id": "Sekitar 2-3 Kali Lebih Kuat Dari Udara." },
  { "en": "Apa Kepanjangan OBC (On-Board Charger)?", "id": "On-Board Charger." },
  { "en": "Apa Fungsi OBC (On-Board Charger)?", "id": "Mengonversi Daya AC (Alternating Current) Jaringan Untuk Baterai." },
  { "en": "Apa Standar ISO (International Organization for Standardization) 15118?", "id": "Protokol Komunikasi Antara EV (Electric Vehicle) Dan Pengecas." },
  { "en": "Mengapa Manajemen Termal Baterai Penting?", "id": "Menjaga Kinerja, Umur, Dan Keamanan Baterai." },
  { "en": "Apa Itu Analisis Root Locus?", "id": "Metode Grafis Analisis Kestabilan Sistem Kontrol." },
  { "en": "Apa Itu Gain Margin?", "id": "Ukuran Kestabilan Relatif Dalam Domain Frekuensi." },
  { "en": "Apa Itu Phase Margin?", "id": "Ukuran Kestabilan Relatif Lainnya." },
  { "en": "Apa Itu Kriteria Kestabilan Nyquist?", "id": "Kriteria Grafis Untuk Kestabilan Sistem Umpan Balik." },
  { "en": "Apa Itu Transformasi-Z?", "id": "Analogi Transformasi Laplace Untuk Sinyal Diskret." },
  { "en": "Apa Beda Desain Filter FIR Dan IIR?", "id": "FIR (Finite Impulse Response) Selalu Stabil, IIR (Infinite Impulse Response) Tidak." },
  { "en": "Apa Kepanjangan FinFET (Fin Field-Effect Transistor)?", "id": "Fin Field-Effect Transistor." },
  { "en": "Apa Keuntungan FinFET (Fin Field-Effect Transistor)?", "id": "Mengurangi Arus Bocor Dan Meningkatkan Kinerja." },
  { "en": "Apa Kepanjangan GAA (Gate-All-Around) FET?", "id": "Gate-All-Around Field-Effect Transistor." },
  { "en": "Apa Beda Kunci IGBT Dan MOSFET?", "id": "IGBT (Insulated Gate Bipolar Transistor) Untuk Tegangan-Daya Tinggi." },
  { "en": "Apa Itu Lisensi Professional Engineer (PE)?", "id": "Lisensi Profesional Untuk Insinyur." },
  { "en": "Apa Itu Kode Etik Insinyur?", "id": "Panduan Perilaku Profesional Dan Etis." },
  { "en": "Apa Itu Hak Kekayaan Intelektual (IP)?", "id": "Melindungi Kreasi Intelektual." },
  { "en": "Apa Kepanjangan IP (Intellectual Property)?", "id": "Intellectual Property." },
  { "en": "Apa Beda Paten Dan Merek Dagang?", "id": "Paten Melindungi Penemuan, Merek Melindungi Identitas." },
  { "en": "Apa Topologi UPS (Uninterruptible Power Supply) Offline?", "id": "Beban Normalnya Diberi Daya Langsung Dari Jaringan." },
  { "en": "Apa Topologi UPS (Uninterruptible Power Supply) Line-Interactive?", "id": "Memiliki Trafo Untuk Mengatur Tegangan." },
  { "en": "Apa Topologi UPS (Uninterruptible Power Supply) Online?", "id": "Beban Selalu Diberi Daya Melalui Inverter." },
  { "en": "Apa Keuntungan UPS (Uninterruptible Power Supply) Online?", "id": "Perlindungan Tertinggi Dan Waktu Transfer Nol." },
  { "en": "Apa Kepanjangan SVG (Static Var Generator)?", "id": "Static Var Generator." },
  { "en": "Apa Fungsi SVG (Static Var Generator)?", "id": "Kompensasi Daya Reaktif Dinamis." },
  { "en": "Apa Itu Sistem Tenaga In-Phase?", "id": "Semua Tegangan Dan Arus Sefasa." },
  { "en": "Apa Itu Sistem Tenaga Quadrature-Phase?", "id": "Tegangan Dan Arus Berbeda Fasa 90 Derajat." },
  { "en": "Apa Itu Daya Harmonisa?", "id": "Daya Yang Terkait Dengan Komponen Harmonisa." },
  { "en": "Apa Itu Faktor-K Trafo?", "id": "Sama Dengan K-Factor Transformer." },
  { "en": "Apa Itu Transformator Isolasi?", "id": "Memberikan Isolasi Galvanik Antara Primer-Sekunder." },
  { "en": "Apa Itu Transformator Autotransformer?", "id": "Sama Dengan Autotransformator." },
  { "en": "Apa Itu Tap Changer Tanpa Beban (Off-Load)?", "id": "Tap Changer Yang Dioperasikan Saat Trafo Mati." },
  { "en": "Apa Itu Diverter Switch?", "id": "Komponen Kunci Dalam OLTC (On-Load Tap Changer)." },
  { "en": "Apa Itu Selector Switch?", "id": "Memilih Tap Berikutnya Dalam OLTC (On-Load Tap Changer)." },
  { "en": "Apa Itu Pengering Udara Pernapasan Trafo?", "id": "Sama Dengan Silika Gel." },
  { "en": "Apa Itu Tes Rasio Belitan (TTR)?", "id": "Sama Dengan TTR (Transformer Turns Ratio) Test." },
  { "en": "Apa Itu Tes Resistansi Isolasi?", "id": "Sama Dengan Insulation Resistance Test." },
  { "en": "Apa Itu Faktor Daya Disipasi?", "id": "Sama Dengan Tan Delta." },
  { "en": "Apa Itu Impedansi Hubung Singkat Trafo?", "id": "Impedansi Internal Trafo." },
  { "en": "Apa Satuan Impedansi Hubung Singkat?", "id": "Persen (%)." },
  { "en": "Apa Pengaruh Impedansi Hubung Singkat?", "id": "Membatasi Arus Gangguan." },
  { "en": "Apa Itu Pergeseran Fasa Trafo?", "id": "Ditentukan Oleh Grup Vektor." },
  { "en": "Apa Itu Uji Polaritas?", "id": "Menentukan Polaritas Relatif Belitan Trafo." },
  { "en": "Apa Itu Diagram Rangkaian Ekuivalen?", "id": "Model Sirkuit Listrik Suatu Perangkat." },
  { "en": "Apa Itu Cabang Magnetisasi?", "id": "Mewakili Arus Beban Nol Trafo." },
  { "en": "Apa Itu Impedansi Bocor?", "id": "Mewakili Fluks Yang Tidak Terkopel." },
  { "en": "Apa Itu Konduktor Berpilin (Stranded)?", "id": "Terdiri Dari Banyak Kawat Kecil." },
  { "en": "Apa Itu Konduktor Padat (Solid)?", "id": "Terdiri Dari Satu Kawat Tunggal." },
  { "en": "Apa Keuntungan Konduktor Berpilin?", "id": "Lebih Fleksibel Daripada Konduktor Padat." },
  { "en": "Apa Itu Kode Warna Resistor?", "id": "Sistem Pita Warna Untuk Menandai Nilai." },
  { "en": "Apa Itu Toleransi Resistor?", "id": "Penyimpangan Nilai Yang Diizinkan." },
  { "en": "Apa Itu Koefisien Suhu Resistor?", "id": "Perubahan Resistansi Per Derajat Suhu." },
  { "en": "Apa Itu Potensiometer?", "id": "Resistor Variabel Tiga Terminal." },
  { "en": "Apa Itu Rheostat?", "id": "Resistor Variabel Dua Terminal." },
  { "en": "Apa Itu Jaringan Pembagi Tegangan?", "id": "Menggunakan Resistor Seri Untuk Mendapatkan Tegangan." },
  { "en": "Apa Itu Jaringan Pembagi Arus?", "id": "Menggunakan Resistor Paralel Untuk Membagi Arus." },
  { "en": "Apa Itu Superposisi?", "id": "Metode Analisis Sirkuit Linier." },
  { "en": "Apa Itu Kapasitor Elektrolitik?", "id": "Kapasitor Terpolarisasi Dengan Kapasitansi Tinggi." },
  { "en": "Apa Itu Kapasitor Keramik?", "id": "Kapasitor Untuk Aplikasi Frekuensi Tinggi." },
  { "en": "Apa Itu Kapasitor Film?", "id": "Kapasitor Dengan Stabilitas Dan Akurasi Baik." },
  { "en": "Apa Itu Dielectric Withstanding Voltage?", "id": "Tegangan Maksimum Yang Dapat Ditahan Dielektrik." },
  { "en": "Apa Itu Arus Bocor Kapasitor?", "id": "Arus DC (Direct Current) Kecil Yang Mengalir Melalui Kapasitor." },
  { "en": "Apa Itu Equivalent Series Resistance (ESR)?", "id": "Resistansi Internal Efektif Kapasitor." },
  { "en": "Apa Kepanjangan ESR?", "id": "Equivalent Series Resistance." },
  { "en": "Apa Itu Induktor Inti Udara?", "id": "Induktor Tanpa Inti Magnetik." },
  { "en": "Apa Itu Induktor Inti Besi?", "id": "Induktor Dengan Inti Material Feromagnetik." },
  { "en": "Apa Itu Choke Frekuensi Radio (RF Choke)?", "id": "Induktor Untuk Memblokir Sinyal RF (Radio Frequency)." },
  { "en": "Apa Itu Mode Diferensial?", "id": "Sinyal Atau Derau Antara Dua Konduktor." },
  { "en": "Apa Itu Mode Bersama (Common Mode)?", "id": "Sinyal Atau Derau Pada Konduktor-Ground." },
  { "en": "Apa Itu Filter Common-Mode?", "id": "Filter Untuk Meredam Derau Mode Bersama." },
  { "en": "Apa Itu Grounding Satu Titik?", "id": "Semua Ground Terhubung Ke Satu Titik." },
  { "en": "Apa Itu Grounding Multipoint?", "id": "Sistem Ground Dengan Banyak Koneksi Ke Referensi." },
  { "en": "Apa Itu Grounding Hibrida?", "id": "Kombinasi Grounding Satu Titik Dan Multipoint." },
  { "en": "Apa Itu Twisted Pair?", "id": "Dua Kabel Dipilin Untuk Mengurangi Interferensi." },
  { "en": "Apa Itu Kabel Shielded Twisted Pair (STP)?", "id": "Twisted Pair Dengan Pelindung Tambahan." },
  { "en": "Apa Itu Kabel Unshielded Twisted Pair (UTP)?", "id": "Twisted Pair Tanpa Pelindung." },
  { "en": "Apa Itu Crosstalk?", "id": "Interferensi Antara Pasangan Kabel Berdekatan." },
  { "en": "Apa Itu Near-End Crosstalk (NEXT)?", "id": "Crosstalk Yang Diukur Di Ujung Pemancar." },
  { "en": "Apa Itu Far-End Crosstalk (FEXT)?", "id": "Crosstalk Yang Diukur Di Ujung Penerima." },
  { "en": "Apa Itu Kategori Kabel Ethernet (Contoh: Cat6)?", "id": "Standar Kinerja Untuk Kabel UTP (Unshielded Twisted Pair)." },
  { "en": "Apa Beda Driver LED (Light-Emitting Diode) Arus Dan Tegangan Konstan?", "id": "Arus Konstan Mengatur Arus, Tegangan Konstan Mengatur Tegangan." },
  { "en": "Apa Kepanjangan CQS (Color Quality Scale)?", "id": "Color Quality Scale." },
  { "en": "Apa Beda CRI Dan CQS?", "id": "CQS (Color Quality Scale) Metrik Kualitas Warna Lebih Modern." },
  { "en": "Apa Itu Visi Mesopik?", "id": "Penglihatan Manusia Dalam Kondisi Cahaya Rendah." },
  { "en": "Apa Mode AGC (Automatic Generation Control) Tie-Line Bias?", "id": "Mengontrol Frekuensi Dan Pertukaran Daya Antar Area." },
  { "en": "Beda Cadangan Berputar Dan Tidak Berputar?", "id": "Berputar Sinkron Dengan Jaringan, Tidak Berputar Offline." },
  { "en": "Apa Beda Isolator Komposit Dan Porselen?", "id": "Komposit Lebih Ringan Dan Hidrofobik." },
  { "en": "Apa Itu Pencucian Isolator Live-Line?", "id": "Membersihkan Isolator Dalam Kondisi Bertegangan." },
  { "en": "Apa Itu Resistivitas Termal Tanah?", "id": "Hambatan Tanah Terhadap Aliran Panas." },
  { "en": "Bagaimana Inisiasi Timer Breaker Failure?", "id": "Dimulai Jika Arus Gangguan Tidak Hilang." },
  { "en": "Beda Barrier Zener Dan Isolating?", "id": "Barrier Zener Membutuhkan Ground, Isolating Tidak." },
  { "en": "Beda Selungkup Ex d Dan Ex e?", "id": "Ex d Tahan Ledakan, Ex e Pencegahan Percikan." },
  { "en": "Mengapa Koreksi Suhu Pengukuran Tahanan Penting?", "id": "Resistansi Konduktor Berubah Dengan Suhu." },
  { "en": "Apa Arti Tes Tip-Up Tan Delta?", "id": "Kenaikan Tan Delta Menandakan Degradasi Isolasi." },
  { "en": "Apa Kepanjangan SOF (State of Function)?", "id": "State of Function." },
  { "en": "Apa Yang Diukur SOF (State of Function) Baterai?", "id": "Kemampuan Baterai Memberikan Daya Saat Ini." },
  { "en": "Beda V2G Dan V2H?", "id": "V2G (Vehicle-to-Grid) Ke Jaringan, V2H (Vehicle-to-Home) Ke Rumah." },
  { "en": "Apa Itu Smart Charging EV (Electric Vehicle)?", "id": "Mengatur Waktu Dan Laju Pengecasan Otomatis." },
  { "en": "Dimana Aplikasi Perangkat SiC (Silicon Carbide)?", "id": "Tegangan Sangat Tinggi (Di Atas 1200V)." },
  { "en": "Dimana Aplikasi Perangkat GaN (Gallium Nitride)?", "id": "Frekuensi Sangat Tinggi (MHz)." },
  { "en": "Apa Itu Metode Depresiasi Garis Lurus?", "id": "Penurunan Nilai Aset Yang Konstan Setiap Tahun." },
  { "en": "Apa Itu Nilai Sisa (Salvage Value)?", "id": "Estimasi Nilai Aset Di Akhir Umur." },
  { "en": "Apa Itu Nilai Buku (Book Value)?", "id": "Biaya Awal Aset Dikurangi Akumulasi Depresiasi." },
  { "en": "Apa Itu Konstanta Propagasi?", "id": "Menentukan Perubahan Amplitudo-Fasa Gelombang." },
  { "en": "Apa Itu Konstanta Atenuasi?", "id": "Bagian Riil Konstanta Propagasi." },
  { "en": "Apa Itu Konstanta Fasa?", "id": "Bagian Imajiner Konstanta Propagasi." },
  { "en": "Apa Itu Kecepatan Fasa?", "id": "Kecepatan Perambatan Fasa Gelombang." },
  { "en": "Apa Itu Kecepatan Grup?", "id": "Kecepatan Perambatan Paket Gelombang." },
  { "en": "Apa Itu Efek Doppler Relativistik?", "id": "Pergeseran Doppler Mempertimbangkan Efek Relativitas." },
  { "en": "Apa Itu Radiator Isotropik?", "id": "Antena Hipotetis Yang Memancar Merata." },
  { "en": "Apa Itu Pola Radiasi Antena?", "id": "Representasi Grafis Kekuatan Radiasi Antena." },
  { "en": "Apa Itu Main Lobe?", "id": "Arah Radiasi Terkuat Antena." },
  { "en": "Apa Itu Side Lobe?", "id": "Radiasi Yang Tidak Diinginkan Di Samping." },
  { "en": "Apa Itu Back Lobe?", "id": "Radiasi Yang Tidak Diinginkan Di Belakang." },
  { "en": "Apa Itu Front-to-Back Ratio?", "id": "Rasio Kekuatan Radiasi Depan-Belakang." },
  { "en": "Apa Itu Beamwidth Antena?", "id": "Lebar Sudut Pancaran Utama Antena." },
  { "en": "Apa Itu Antena Dipol Setengah Gelombang?", "id": "Antena Resonan Dasar." },
  { "en": "Apa Itu Antena Monopol Seperempat Gelombang?", "id": "Setengah Antena Dipol Dengan Bidang Tanah." },
  { "en": "Apa Itu Bidang Tanah (Ground Plane)?", "id": "Permukaan Konduktif Yang Berfungsi Sebagai Reflektor." },
  { "en": "Apa Itu Antena Yagi-Uda?", "id": "Antena Berarah Dengan Elemen Reflektor-Direktor." },
  { "en": "Apa Itu Antena Parabola?", "id": "Antena Reflektor Untuk Gain Sangat Tinggi." },
  { "en": "Apa Itu Antena Array?", "id": "Sekelompok Elemen Antena Bekerja Sama." },
  { "en": "Apa Itu Phased Array?", "id": "Array Yang Arah Pancarannya Dikontrol Elektronik." },
  { "en": "Apa Itu Aperture Antena?", "id": "Area Efektif Antena Menangkap Energi." },
  { "en": "Apa Itu Persamaan Friis?", "id": "Menghitung Daya Diterima Antara Dua Antena." },
  { "en": "Apa Itu Equivalent Isotropically Radiated Power (EIRP)?", "id": "Daya Efektif Yang Dipancarkan Dari Antena." },
  { "en": "Apa Kepanjangan EIRP?", "id": "Equivalent Isotropically Radiated Power." },
  { "en": "Apa Itu Suhu Derau (Noise Temperature)?", "id": "Ukuran Derau Yang Dihasilkan Komponen." },
  { "en": "Apa Itu Angka Derau (Noise Figure)?", "id": "Ukuran Peningkatan Derau Oleh Jaringan." },
  { "en": "Apa Itu Batas Shannon-Hartley?", "id": "Kapasitas Kanal Maksimum Teoritis." },
  { "en": "Apa Itu Bit Error Rate (BER)?", "id": "Rasio Bit Yang Rusak Terhadap Total Bit." },
  { "en": "Apa Kepanjangan BER (Bit Error Rate)?", "id": "Bit Error Rate." },
  { "en": "Apa Itu Diagram Konstelasi?", "id": "Representasi Skema Modulasi Digital." },
  { "en": "Apa Itu Orthogonal Frequency-Division Multiplexing (OFDM)?", "id": "Teknik Modulasi Untuk Komunikasi Digital Modern." },
  { "en": "Apa Kepanjangan OFDM?", "id": "Orthogonal Frequency-Division Multiplexing." },
  { "en": "Apa Itu Spread Spectrum?", "id": "Teknik Komunikasi Yang Menyebarkan Sinyal." },
  { "en": "Apa Itu Frequency-Hopping Spread Spectrum (FHSS)?", "id": "Mengubah Frekuensi Pembawa Secara Cepat." },
  { "en": "Apa Itu Direct-Sequence Spread Spectrum (DSSS)?", "id": "Mengalikan Sinyal Dengan Kode Penebar." },
  { "en": "Apa Itu Kode Pseudo-Noise (PN)?", "id": "Urutan Biner Acak Semu." },
  { "en": "Apa Itu GPS (Global Positioning System)?", "id": "Sistem Navigasi Satelit Global." },
  { "en": "Bagaimana GPS (Global Positioning System) Bekerja?", "id": "Menggunakan Sinyal Waktu Dari Beberapa Satelit." },
  { "en": "Apa Itu Trilaterasi?", "id": "Menentukan Posisi Berdasarkan Jarak Ke Titik-Titik." },
  { "en": "Apa Itu Sistem Tenaga Listrik Pesawat?", "id": "Menggunakan Frekuensi 400 Hz." },
  { "en": "Mengapa Pesawat Menggunakan Frekuensi 400 Hz?", "id": "Memungkinkan Transformator Dan Motor Lebih Kecil." },
  { "en": "Apa Itu Ground Power Unit (GPU)?", "id": "Menyediakan Daya Pesawat Saat Di Darat." },
  { "en": "Apa Kepanjangan GPU (Ground Power Unit)?", "id": "Ground Power Unit." },
  { "en": "Apa Itu Ram Air Turbine (RAT)?", "id": "Turbin Darurat Pembangkit Listrik Pesawat." },
  { "en": "Apa Kepanjangan RAT (Ram Air Turbine)?", "id": "Ram Air Turbine." },
  { "en": "Apa Itu Auxiliary Power Unit (APU)?", "id": "Unit Pembangkit Listrik Bantu Di Pesawat." },
  { "en": "Apa Kepanjangan APU (Auxiliary Power Unit)?", "id": "Auxiliary Power Unit." },
  { "en": "Apa Itu Sistem Penggerak Propulsi Listrik Kapal?", "id": "Menggunakan Motor Listrik Untuk Memutar Baling-Baling." },
  { "en": "Apa Keuntungan Propulsi Listrik?", "id": "Efisiensi, Fleksibilitas, Getaran Rendah." },
  { "en": "Apa Itu Shore Power?", "id": "Pasokan Listrik Dari Darat Untuk Kapal." },
  { "en": "Apa Itu Sistem Proteksi Katodik Impressed Current?", "id": "Menggunakan Sumber Arus DC (Direct Current) Eksternal." },
  { "en": "Apa Itu Sistem Railway Signalling?", "id": "Sistem Sinyal Untuk Mengatur Lalu Lintas Kereta." },
  { "en": "Apa Itu Track Circuit?", "id": "Sirkuit Listrik Untuk Mendeteksi Keberadaan Kereta." },
  { "en": "Apa Itu Axle Counter?", "id": "Menghitung Gandar Kereta Untuk Mendeteksi Keberadaan." },
  { "en": "Apa Itu Automatic Train Protection (ATP)?", "id": "Sistem Keamanan Yang Mencegah Kesalahan Masinis." },
  { "en": "Apa Itu Communication-Based Train Control (CBTC)?", "id": "Sistem Kontrol Kereta Berbasis Komunikasi Nirkabel." },
  { "en": "Apa Itu Penggerak Reluktansi Variabel?", "id": "Sama Dengan Motor Reluktansi." },
  { "en": "Apa Itu Motor Linier Sinkron (LSM)?", "id": "Motor Linier Dengan Kecepatan Sinkron." },
  { "en": "Dimana LSM (Linear Synchronous Motor) Digunakan?", "id": "Kereta Maglev, Roller Coaster." },
  { "en": "Apa Itu Kereta Maglev?", "id": "Kereta Yang Melayang Menggunakan Levitasi Magnetik." },
  { "en": "Apa Itu Sistem Levitasi Elektrodinamik (EDS)?", "id": "Menggunakan Magnet Superkonduktor Di Kereta." },
  { "en": "Apa Itu Sistem Levitasi Elektromagnetik (EMS)?", "id": "Menggunakan Elektromagnet Terkontrol Di Jalur." },
  { "en": "Apa Itu Akselerator Partikel?", "id": "Mesin Yang Mempercepat Partikel Bermuatan." },
  { "en": "Apa Itu Synchrotron?", "id": "Jenis Akselerator Partikel Sirkular." },
  { "en": "Apa Itu Cyclotron?", "id": "Jenis Akselerator Partikel Kompak." },
  { "en": "Apa Itu Reaktor Fusi Nuklir?", "id": "Menghasilkan Energi Dengan Menggabungkan Inti Atom." },
  { "en": "Apa Itu Tokamak?", "id": "Desain Reaktor Fusi Berbentuk Torus." },
  { "en": "Apa Tantangan Utama Fusi Nuklir?", "id": "Menahan Plasma Suhu Ekstrem." },
  { "en": "Apa Itu Pemanasan Plasma?", "id": "Memanaskan Gas Menjadi Keadaan Plasma." },
  { "en": "Apa Itu Injeksi Berkas Netral?", "id": "Metode Pemanasan Plasma Fusi." },
  { "en": "Apa Itu Pemanasan Frekuensi Radio?", "id": "Metode Lain Pemanasan Plasma Fusi." },
  { "en": "Apa Itu Kurva CBEMA (Computer Business Equipment Manufacturers Association)?", "id": "Kurva Toleransi Tegangan Untuk Peralatan IT (Information Technology)." },
  { "en": "Apa Kepanjangan ITIC (Information Technology Industry Council)?", "id": "Information Technology Industry Council." },
  { "en": "Apa Pertimbangan Desain Seismik Gardu Induk?", "id": "Memastikan Peralatan Tahan Terhadap Gempa Bumi." },
  { "en": "Apa Beda Sistem Deluge Dan Sprinkler?", "id": "Deluge Terbuka Semua, Sprinkler Individual." },
  { "en": "Apa Kepanjangan TOV (Temporary Overvoltage)?", "id": "Temporary Overvoltage." },
  { "en": "Apa Penyebab TOV (Temporary Overvoltage)?", "id": "Gangguan Fasa Tanah, Pelepasan Beban." },
  { "en": "Apa Itu Proteksi Gangguan Tanah Stator 100%?", "id": "Melindungi Seluruh Belitan Stator Dari Gangguan Tanah." },
  { "en": "Apa Kode ANSI (American National Standards Institute) Untuk Proteksi Loss-of-Field?", "id": "40." },
  { "en": "Apa Itu Proteksi Inadvertent Energization?", "id": "Mencegah Pemberian Energi Generator Saat Diam." },
  { "en": "Apa Kepanjangan UPFC (Unified Power Flow Controller)?", "id": "Unified Power Flow Controller." },
  { "en": "Apa Fungsi UPFC (Unified Power Flow Controller)?", "id": "Mengontrol Aliran Daya, Tegangan, Impedansi." },
  { "en": "Apa Kepanjangan IPFC (Interline Power Flow Controller)?", "id": "Interline Power Flow Controller." },
  { "en": "Apa Fungsi IPFC (Interline Power Flow Controller)?", "id": "Mengelola Aliran Daya Antar Beberapa Saluran." },
  { "en": "Apa Itu Diagram Belitan?", "id": "Representasi Grafis Koneksi Kumparan Mesin." },
  { "en": "Apa Proses Komutasi Pada Mesin DC (Direct Current)?", "id": "Pembalikan Arah Arus Di Kumparan Jangkar." },
  { "en": "Apa Kepanjangan HFCT (High-Frequency Current Transformer)?", "id": "High-Frequency Current Transformer." },
  { "en": "Untuk Apa HFCT (High-Frequency Current Transformer) Digunakan?", "id": "Mengukur Sinyal Partial Discharge (PD) Frekuensi Tinggi." },
  { "en": "Apa Itu Angka Netralisasi Minyak Trafo?", "id": "Ukuran Keasaman Minyak Transformator." },
  { "en": "Apa Kepanjangan D-PMU (Distribution Phasor Measurement Unit)?", "id": "Distribution Phasor Measurement Unit." },
  { "en": "Apa Fungsi D-PMU (Distribution Phasor Measurement Unit)?", "id": "Pemantauan Fasor Presisi Di Jaringan Distribusi." },
  { "en": "Apa Kepanjangan DLMS/COSEM?", "id": "Device Language Message Specification / Companion Specification for Energy Metering." },
  { "en": "Apa Itu Protokol DLMS/COSEM?", "id": "Standar Komunikasi Untuk Meteran Energi Cerdas." },
  { "en": "Apa Beda NIDS Dan HIDS?", "id": "NIDS (Network IDS) Memantau Jaringan, HIDS (Host IDS) Memantau Host." },
  { "en": "Apa Kepanjangan SIEM (Security Information and Event Management)?", "id": "Security Information and Event Management." },
  { "en": "Apa Fungsi Sistem SIEM (Security Information and Event Management)?", "id": "Mengumpulkan Dan Menganalisis Log Keamanan." },
  { "en": "Apa Kepanjangan EPSS (Emergency Power Supply System)?", "id": "Emergency Power Supply System." },
  { "en": "Beda Sistem Siaga Wajib Dan Opsional?", "id": "Wajib Untuk Keselamatan Jiwa (NEC 700)." },
  { "en": "Apa Itu Artikel NEC (National Electrical Code) 700?", "id": "Mengatur Sistem Darurat." },
  { "en": "Apa Itu Artikel NEC (National Electrical Code) 701?", "id": "Mengatur Sistem Siaga Wajib Secara Hukum." },
  { "en": "Apa Itu Artikel NEC (National Electrical Code) 702?", "id": "Mengatur Sistem Siaga Opsional." },
  { "en": "Apa Itu Magnetisasi Sisa?", "id": "Sama Dengan Sisa Fluks (Residual Flux)." },
  { "en": "Apa Itu Degaussing?", "id": "Proses Menghilangkan Medan Magnet Sisa." },
  { "en": "Apa Itu Efek Barkhausen?", "id": "Derau Yang Dihasilkan Saat Perubahan Magnetisasi." },
  { "en": "Apa Itu Domain Magnetik?", "id": "Area Kecil Dengan Arah Magnetisasi Seragam." },
  { "en": "Apa Itu Dinding Domain?", "id": "Batas Antara Domain Magnetik." },
  { "en": "Apa Itu Suhu Curie?", "id": "Suhu Dimana Material Kehilangan Sifat Feromagnetiknya." },
  { "en": "Apa Itu Material Paramagnetik?", "id": "Material Yang Sedikit Tertarik Medan Magnet." },
  { "en": "Apa Itu Material Diamagnetik?", "id": "Material Yang Sedikit Ditolak Medan Magnet." },
  { "en": "Apa Itu Susceptibilitas Magnetik?", "id": "Ukuran Seberapa Mudah Material Dimagnetisasi." },
  { "en": "Apa Itu Perisai Magnetik?", "id": "Sama Dengan Pelindung Magnetik (Magnetic Shielding)." },
  { "en": "Apa Itu Sirkuit Terkopel Magnetik?", "id": "Sirkuit Dengan Induktansi Timbal Balik." },
  { "en": "Apa Itu Faktor Kopling?", "id": "Sama Dengan Koefisien Kopling (k)." },
  { "en": "Apa Itu Generator Homopolar?", "id": "Generator DC (Direct Current) Tanpa Komutator." },
  { "en": "Apa Itu Pengereman Arus Eddy?", "id": "Sama Dengan Rem Arus Eddy." },
  { "en": "Apa Itu Pemanasan Dielektrik?", "id": "Pemanasan Bahan Isolator Menggunakan Medan Listrik." },
  { "en": "Apa Itu Rugi Dielektrik?", "id": "Energi Yang Hilang Sebagai Panas Dielektrik." },
  { "en": "Apa Itu Sudut Rugi (Loss Angle)?", "id": "Sudut Fasa Terkait Rugi Dielektrik." },
  { "en": "Apa Itu Teori Garis Transmisi?", "id": "Analisis Perambatan Gelombang Di Saluran." },
  { "en": "Apa Itu Pantulan Sinyal?", "id": "Terjadi Saat Impedansi Tidak Cocok." },
  { "en": "Apa Itu Koefisien Pantul?", "id": "Rasio Amplitudo Gelombang Pantul Terhadap Datang." },
  { "en": "Apa Itu Matching Stub?", "id": "Saluran Transmisi Pendek Untuk Mencocokkan Impedansi." },
  { "en": "Apa Itu Transformator Seperempat Gelombang?", "id": "Saluran Transmisi Untuk Mencocokkan Impedansi." },
  { "en": "Apa Itu Power Divider?", "id": "Membagi Sinyal Input Menjadi Beberapa Output." },
  { "en": "Apa Itu Power Combiner?", "id": "Menggabungkan Beberapa Sinyal Menjadi Satu Output." },
  { "en": "Apa Itu Filter Gelombang Mikro?", "id": "Filter Yang Beroperasi Pada Frekuensi Gelombang Mikro." },
  { "en": "Apa Itu Resonator Dielektrik?", "id": "Resonator Frekuensi Tinggi Menggunakan Material Dielektrik." },
  { "en": "Apa Itu Osilator Gunn?", "id": "Sumber Gelombang Mikro Menggunakan Dioda Gunn." },
  { "en": "Apa Itu Dioda IMPATT?", "id": "Dioda Pembangkit Gelombang Mikro Lainnya." },
  { "en": "Apa Itu Amplifier Kebisingan Rendah (LNA)?", "id": "Menguatkan Sinyal Sangat Lemah Tanpa Menambah Derau." },
  { "en": "Apa Kepanjangan LNA (Low-Noise Amplifier)?", "id": "Low-Noise Amplifier." },
  { "en": "Apa Itu Power Amplifier (PA)?", "id": "Menguatkan Sinyal Untuk Ditransmisikan." },
  { "en": "Apa Kepanjangan PA (Power Amplifier)?", "id": "Power Amplifier." },
  { "en": "Apa Itu Kelas Efisiensi Amplifier?", "id": "Ukuran Seberapa Efisien Amplifier Mengonversi Daya." },
  { "en": "Apa Itu Titik Kompresi 1-dB?", "id": "Titik Dimana Gain Amplifier Turun 1 dB." },
  { "en": "Apa Itu Titik Intersep Orde Ketiga (IP3)?", "id": "Ukuran Linearitas Amplifier." },
  { "en": "Apa Kepanjangan IP3 (Third-Order Intercept Point)?", "id": "Third-Order Intercept Point." },
  { "en": "Apa Itu Sirkuit Terpadu Gelombang Mikro Monolitik (MMIC)?", "id": "Sirkuit RF (Radio Frequency) Lengkap Dalam Satu Chip." },
  { "en": "Apa Kepanjangan MMIC?", "id": "Monolithic Microwave Integrated Circuit." },
  { "en": "Apa Itu Sirkuit Terpadu Hibrida?", "id": "Sirkuit Yang Menggabungkan Beberapa Teknologi Chip." },
  { "en": "Apa Itu Substrat RF (Radio Frequency)?", "id": "Material Papan Sirkuit Untuk Frekuensi Tinggi." },
  { "en": "Apa Itu Konstanta Dielektrik Substrat?", "id": "Parameter Kunci Untuk Desain Sirkuit RF (Radio Frequency)." },
  { "en": "Apa Itu Rugi Tangen (Loss Tangent)?", "id": "Ukuran Rugi-Rugi Sinyal Dalam Substrat." },
  { "en": "Apa Itu Microstrip?", "id": "Jenis Saluran Transmisi Pada PCB (Printed Circuit Board)." },
  { "en": "Apa Itu Stripline?", "id": "Jenis Saluran Transmisi Lainnya." },
  { "en": "Apa Itu Coplanar Waveguide (CPW)?", "id": "Saluran Transmisi Dengan Konduktor Di Bidang Sama." },
  { "en": "Apa Itu Mode Common-Mode?", "id": "Sama Dengan Mode Bersama." },
  { "en": "Apa Itu Mode Differential-Mode?", "id": "Sama Dengan Mode Diferensial." },
  { "en": "Apa Itu Parameter Y?", "id": "Parameter Admitansi Jaringan Dua Gerbang." },
  { "en": "Apa Itu Parameter Z?", "id": "Parameter Impedansi Jaringan Dua Gerbang." },
  { "en": "Apa Itu Parameter H?", "id": "Parameter Hibrida Jaringan Dua Gerbang." },
  { "en": "Apa Itu S-parameter?", "id": "Parameter Hamburan (Scattering) Untuk Jaringan RF (Radio Frequency)." },
  { "en": "Apa Itu Analisis Jaringan Vektor (VNA)?", "id": "Alat Untuk Mengukur S-parameter." },
  { "en": "Apa Kepanjangan VNA (Vector Network Analyzer)?", "id": "Vector Network Analyzer." },
  { "en": "Apa Itu Analisis Spektrum?", "id": "Alat Untuk Mengukur Sinyal Dalam Domain Frekuensi." },
  { "en": "Apa Itu Tracking Generator?", "id": "Sumber Sinyal Untuk Pengujian Respon Frekuensi." },
  { "en": "Apa Itu Near-Field Communication (NFC)?", "id": "Teknologi Komunikasi Nirkabel Jarak Sangat Dekat." },
  { "en": "Apa Kepanjangan NFC?", "id": "Near-Field Communication." },
  { "en": "Apa Itu Radio-Frequency Identification (RFID)?", "id": "Teknologi Identifikasi Menggunakan Gelombang Radio." },
  { "en": "Apa Kepanjangan RFID?", "id": "Radio-Frequency Identification." },
  { "en": "Apa Itu Tag RFID (Radio-Frequency Identification) Pasif?", "id": "Tag Tanpa Sumber Daya Internal." },
  { "en": "Apa Itu Tag RFID (Radio-Frequency Identification) Aktif?", "id": "Tag Dengan Baterai Internal." },
  { "en": "Apa Itu Backscatter?", "id": "Prinsip Komunikasi Tag RFID (Radio-Frequency Identification) Pasif." },
  { "en": "Apa Itu Software-Defined Radio (SDR)?", "id": "Sistem Komunikasi Radio Berbasis Perangkat Lunak." },
  { "en": "Apa Kepanjangan SDR (Software-Defined Radio)?", "id": "Software-Defined Radio." },
  { "en": "Apa Itu Radio Kognitif?", "id": "Radio Cerdas Yang Dapat Beradaptasi Lingkungan." },
  { "en": "Apa Itu Manajemen Spektrum Dinamis?", "id": "Alokasi Spektrum Frekuensi Secara Real-Time." },
  { "en": "Apa Itu Propagasi Gelombang Elektromekanis?", "id": "Perambatan Gangguan Melalui Sistem Tenaga." },
  { "en": "Apa Kepanjangan SSR (Subsynchronous Resonance)?", "id": "Subsynchronous Resonance." },
  { "en": "Apa Penyebab SSR (Subsynchronous Resonance)?", "id": "Interaksi Antara Kompensasi Seri Dan Generator." },
  { "en": "Apa Itu Sensitivitas Eigenvalue?", "id": "Mengukur Perubahan Mode Osilasi." },
  { "en": "Apa Itu Resistansi Jejak (Tracking Resistance)?", "id": "Kemampuan Isolasi Menahan Pembentukan Jejak Karbon." },
  { "en": "Apa Kepanjangan PDIV (Partial Discharge Inception Voltage)?", "id": "Partial Discharge Inception Voltage." },
  { "en": "Apa Itu PDIV (Partial Discharge Inception Voltage)?", "id": "Tegangan Minimum Timbulnya Partial Discharge." },
  { "en": "Beda Proteksi Busbar Impedansi Tinggi-Rendah?", "id": "Tinggi Lebih Aman, Rendah Lebih Cepat." },
  { "en": "Apa Itu Power Swing Blocking?", "id": "Fitur Relay Untuk Mencegah Trip Saat Ayunan Daya." },
  { "en": "Apa Kepanjangan TCBR (Thyristor Controlled Braking Resistor)?", "id": "Thyristor Controlled Braking Resistor." },
  { "en": "Apa Fungsi TCBR (Thyristor Controlled Braking Resistor)?", "id": "Menyerap Kelebihan Energi Untuk Kestabilan." },
  { "en": "Apa Kepanjangan SSSC (Static Synchronous Series Compensator)?", "id": "Static Synchronous Series Compensator." },
  { "en": "Apa Fungsi SSSC (Static Synchronous Series Compensator)?", "id": "Menginjeksikan Tegangan Seri Untuk Mengatur Daya." },
  { "en": "Apa Itu Aliran Daya Harmonisa?", "id": "Analisis Aliran Daya Memperhitungkan Harmonisa." },
  { "en": "Apa Kepanjangan GPR (Ground Potential Rise)?", "id": "Ground Potential Rise." },
  { "en": "Apa Itu GPR (Ground Potential Rise)?", "id": "Sama Dengan Earth Potential Rise (EPR)." },
  { "en": "Bagaimana Mitigasi Tegangan Sentuh Dan Langkah?", "id": "Dengan Cincin Grading Dan Lapisan Permukaan." },
  { "en": "Apa Kepanjangan RUL (Remaining Useful Life)?", "id": "Remaining Useful Life." },
  { "en": "Apa Itu Estimasi RUL (Remaining Useful Life)?", "id": "Memprediksi Sisa Umur Pakai Aset." },
  { "en": "Apa Itu Pasar Day-Ahead?", "id": "Pasar Listrik Untuk Transaksi Hari Berikutnya." },
  { "en": "Apa Itu Pasar Real-Time?", "id": "Pasar Listrik Untuk Keseimbangan Daya Sesaat." },
  { "en": "Apa Kepanjangan FTR (Financial Transmission Rights)?", "id": "Financial Transmission Rights." },
  { "en": "Apa Algoritma MPPT (Maximum Power Point Tracking) Perturb & Observe?", "id": "Mengubah Tegangan Dan Mengamati Perubahan Daya." },
  { "en": "Apa Algoritma MPPT (Maximum Power Point Tracking) Incremental Conductance?", "id": "Membandingkan Konduktansi Inkremental Dan Instan." },
  { "en": "Apa Kepanjangan PUE (Power Usage Effectiveness)?", "id": "Power Usage Effectiveness." },
  { "en": "Apa Yang Diukur PUE (Power Usage Effectiveness)?", "id": "Efisiensi Energi Pusat Data." },
  { "en": "Apa Level Redundansi UPS (Uninterruptible Power Supply) N+1?", "id": "Menyediakan Satu Modul Cadangan Lebih." },
  { "en": "Apa Level Redundansi UPS (Uninterruptible Power Supply) 2N?", "id": "Dua Sistem Independen Penuh (Cermin)." },
  { "en": "Apa Kepanjangan PDU (Power Distribution Unit)?", "id": "Power Distribution Unit." },
  { "en": "Apa Fungsi PDU (Power Distribution Unit)?", "id": "Mendistribusikan Daya Listrik Di Rak Server." },
  { "en": "Apa Itu PDU (Power Distribution Unit) Cerdas?", "id": "PDU (Power Distribution Unit) Dengan Kemampuan Monitoring Jarak Jauh." },
  { "en": "Apa Itu Hot-Swappable?", "id": "Komponen Dapat Diganti Saat Sistem Beroperasi." },
  { "en": "Apa Itu Cold Aisle Containment?", "id": "Mengisolasi Koridor Udara Dingin Di Pusat Data." },
  { "en": "Apa Itu Hot Aisle Containment?", "id": "Mengisolasi Koridor Udara Panas Di Pusat Data." },
  { "en": "Apa Tujuan Penahanan Koridor (Aisle Containment)?", "id": "Meningkatkan Efisiensi Pendinginan Pusat Data." },
  { "en": "Apa Itu Free Cooling?", "id": "Menggunakan Udara Luar Dingin Untuk Pendinginan." },
  { "en": "Apa Itu Economizer?", "id": "Sistem Yang Memungkinkan Free Cooling." },
  { "en": "Apa Itu Standar Uptime Institute Tier?", "id": "Standar Klasifikasi Keandalan Pusat Data." },
  { "en": "Apa Itu Tier I Data Center?", "id": "Kapasitas Dasar Tanpa Redundansi." },
  { "en": "Apa Itu Tier II Data Center?", "id": "Kapasitas Redundan Parsial." },
  { "en": "Apa Itu Tier III Data Center?", "id": "Dapat Dirawat Tanpa Downtime (Concurrently Maintainable)." },
  { "en": "Apa Itu Tier IV Data Center?", "id": "Toleran Terhadap Gangguan (Fault Tolerant)." },
  { "en": "Apa Itu Desain Point of Delivery (PoD)?", "id": "Desain Pusat Data Modular." },
  { "en": "Apa Itu Switchgear Berinsulasi Udara (AIS)?", "id": "Sama Dengan AIS (Air Insulated Substation)." },
  { "en": "Apa Itu Switchgear Berinsulasi Gas (GIS)?", "id": "Sama Dengan GIS (Gas Insulated Substation)." },
  { "en": "Apa Itu Metal-Enclosed Switchgear?", "id": "Switchgear Dalam Selungkup Logam." },
  { "en": "Apa Itu Metal-Clad Switchgear?", "id": "Tipe Metal-Enclosed Dengan Kompartemen Terpisah." },
  { "en": "Apa Itu Arc-Resistant Switchgear?", "id": "Switchgear Tahan Terhadap Ledakan Busur Api Internal." },
  { "en": "Apa Itu Rakitan Switchboard?", "id": "Panel Distribusi Daya Tegangan Rendah." },
  { "en": "Apa Itu Pemutus Sirkuit Drawout?", "id": "Dapat Ditarik Keluar Untuk Perawatan." },
  { "en": "Apa Itu Shutter Mekanis?", "id": "Penutup Pengaman Pada Kompartemen Switchgear." },
  { "en": "Apa Itu Pengujian Tipe (Type Test)?", "id": "Pengujian Desain Pada Prototipe Peralatan." },
  { "en": "Apa Itu Pengujian Rutin (Routine Test)?", "id": "Pengujian Yang Dilakukan Pada Setiap Unit Produksi." },
  { "en": "Apa Itu Uji Impuls (Impulse Test)?", "id": "Menguji Ketahanan Isolasi Terhadap Surja Tegangan." },
  { "en": "Apa Itu Uji Kenaikan Suhu?", "id": "Memastikan Peralatan Beroperasi Dalam Batas Suhu." },
  { "en": "Apa Itu Uji Ketahanan Sirkuit Pendek?", "id": "Menguji Kemampuan Peralatan Menahan Arus Gangguan." },
  { "en": "Apa Itu Faktor Puncak (Peak Factor)?", "id": "Sama Dengan Crest Factor." },
  { "en": "Apa Itu Faktor Ripple?", "id": "Sama Dengan Ripple Factor." },
  { "en": "Apa Itu Tegangan Breakdown?", "id": "Tegangan Yang Menyebabkan Kegagalan Dielektrik." },
  { "en": "Apa Itu Arus Avalanche?", "id": "Perkalian Pembawa Muatan Dalam Semikonduktor." },
  { "en": "Apa Itu Efek Zener?", "id": "Mekanisme Breakdown Pada Dioda Zener." },
  { "en": "Apa Itu Sirkuit Terintegrasi (IC)?", "id": "Sirkuit Elektronik Dalam Satu Chip Semikonduktor." },
  { "en": "Apa Kepanjangan IC (Integrated Circuit)?", "id": "Integrated Circuit." },
  { "en": "Apa Itu Small-Scale Integration (SSI)?", "id": "Hingga 100 Komponen Per Chip." },
  { "en": "Apa Itu Medium-Scale Integration (MSI)?", "id": "Hingga 3,000 Komponen Per Chip." },
  { "en": "Apa Itu Large-Scale Integration (LSI)?", "id": "Hingga 100,000 Komponen Per Chip." },
  { "en": "Apa Itu Very Large-Scale Integration (VLSI)?", "id": "Lebih Dari 100,000 Komponen Per Chip." },
  { "en": "Apa Hukum Moore?", "id": "Jumlah Transistor Berlipat Ganda Setiap Dua Tahun." },
  { "en": "Apa Itu Fabrikasi Semikonduktor?", "id": "Proses Pembuatan Sirkuit Terintegrasi." },
  { "en": "Apa Itu Cleanroom?", "id": "Ruangan Terkontrol Untuk Fabrikasi Semikonduktor." },
  { "en": "Apa Kelas Cleanroom?", "id": "Berdasarkan Jumlah Partikel Per Satuan Volume." },
  { "en": "Apa Itu Etching?", "id": "Proses Menghilangkan Material Secara Kimiawi." },
  { "en": "Apa Itu Deposition?", "id": "Proses Menambahkan Lapisan Tipis Material." },
  { "en": "Apa Itu Chemical Vapor Deposition (CVD)?", "id": "Metode Deposition Menggunakan Reaksi Kimia Gas." },
  { "en": "Apa Itu Physical Vapor Deposition (PVD)?", "id": "Metode Deposition Menggunakan Proses Fisik." },
  { "en": "Apa Itu Sputtering?", "id": "Jenis PVD (Physical Vapor Deposition)." },
  { "en": "Apa Itu Oksidasi Termal?", "id": "Proses Menumbuhkan Lapisan Silikon Dioksida." },
  { "en": "Apa Itu Ion Implantation?", "id": "Proses Doping Menggunakan Ion Berenergi." },
  { "en": "Apa Itu Annealing?", "id": "Proses Pemanasan Untuk Mengaktifkan Dopant." },
  { "en": "Apa Itu Chemical-Mechanical Planarization (CMP)?", "id": "Proses Meratakan Permukaan Wafer." },
  { "en": "Apa Kepanjangan CMP?", "id": "Chemical-Mechanical Planarization." },
  { "en": "Apa Itu Metrologi Wafer?", "id": "Ilmu Pengukuran Selama Proses Fabrikasi." },
  { "en": "Apa Itu Yield?", "id": "Persentase Chip Fungsional Per Wafer." },
  { "en": "Apa Itu Packaging Sirkuit Terpadu?", "id": "Proses Enkapsulasi Dan Penyambungan Die." },
  { "en": "Apa Itu Wire Bonding?", "id": "Menyambungkan Pad Die Ke Pin Paket." },
  { "en": "Apa Itu Flip-Chip?", "id": "Metode Pemasangan Die Terbalik." },
  { "en": "Apa Itu Ball Grid Array (BGA)?", "id": "Jenis Paket Dengan Bola Solder Di Bawah." },
  { "en": "Apa Itu Quad Flat Package (QFP)?", "id": "Jenis Paket Dengan Kaki Di Keempat Sisi." },
  { "en": "Apa Itu Dual In-Line Package (DIP)?", "id": "Paket Dengan Dua Baris Kaki Paralel." },
  { "en": "Apa Itu System on a Chip (SoC)?", "id": "Sistem Komputer Lengkap Dalam Satu Chip." },
  { "en": "Apa Kepanjangan SoC (System on a Chip)?", "id": "System on a Chip." },
  { "en": "Apa Itu System in Package (SiP)?", "id": "Beberapa Chip Dalam Satu Paket." },
  { "en": "Apa Kepanjangan SiP (System in Package)?", "id": "System in Package." },
  { "en": "Apa Itu Desain Termal Chip?", "id": "Manajemen Panas Yang Dihasilkan Sirkuit Terpadu." },
  { "en": "Apa Itu Power Gating?", "id": "Teknik Menghemat Daya Dengan Mematikan Blok." },
  { "en": "Apa Itu Clock Gating?", "id": "Teknik Menghemat Daya Dengan Mematikan Clock." },
  { "en": "Apa Itu Rekonfigurasi Penyulang?", "id": "Mengubah Konfigurasi Jaringan Untuk Optimasi." },
  { "en": "Apa Itu Kontrol Volt/VAR?", "id": "Manajemen Tegangan Dan Daya Reaktif Distribusi." },
  { "en": "Apa Kepanjangan SAIDI (System Average Interruption Duration Index)?", "id": "System Average Interruption Duration Index." },
  { "en": "Apa Yang Diukur SAIDI (System Average Interruption Duration Index)?", "id": "Rata-rata Durasi Pemadaman Per Pelanggan." },
  { "en": "Apa Kepanjangan SAIFI (System Average Interruption Frequency Index)?", "id": "System Average Interruption Frequency Index." },
  { "en": "Apa Yang Diukur SAIFI (System Average Interruption Frequency Index)?", "id": "Rata-rata Frekuensi Pemadaman Per Pelanggan." },
  { "en": "Apa Kepanjangan MAIFI (Momentary Average Interruption Frequency Index)?", "id": "Momentary Average Interruption Frequency Index." },
  { "en": "Beda Inverter Grid-Forming Dan Grid-Following?", "id": "Grid-Forming Menciptakan Jaringan, Grid-Following Mengikuti." },
  { "en": "Beda Deteksi Islanding Aktif Dan Pasif?", "id": "Aktif Menginjeksikan Sinyal, Pasif Memantau." },
  { "en": "Apa Itu Pelemahan Daya (Curtailment)?", "id": "Pembatasan Produksi Energi Terbarukan Disengaja." },
  { "en": "Beda Konverter ZVS Dan ZCS?", "id": "ZVS (Zero-Voltage Switching) Beralih Saat Tegangan Nol." },
  { "en": "Apa Itu Komutasi Motor BLDC (Brushless DC)?", "id": "Proses Pensaklaran Arus Belitan Stator." },
  { "en": "Apa Itu Field Weakening PMSM (Permanent Magnet Synchronous Motor)?", "id": "Operasi Motor Di Atas Kecepatan Nominal." },
  { "en": "Apa Itu Formula Peek?", "id": "Menghitung Tegangan Awal Korona." },
  { "en": "Apa Itu Kriteria Perambatan Streamer?", "id": "Kondisi Untuk Streamer Berkembang Menjadi Breakdown." },
  { "en": "Apa Fungsi Komponen Urutan Negatif?", "id": "Mendeteksi Ketidakseimbangan Sistem Tiga Fasa." },
  { "en": "Apa Arti Kelas 10 Relay Termal?", "id": "Trip Dalam 10 Detik Pada 600% Arus." },
  { "en": "Apa Arti Kelas 20 Relay Termal?", "id": "Trip Dalam 20 Detik Pada 600% Arus." },
  { "en": "Apa Itu Representasi Ruang Keadaan (State-Space)?", "id": "Model Matematis Sistem Dinamis." },
  { "en": "Apa Itu Keterkendalian (Controllability)?", "id": "Kemampuan Mengarahkan Keadaan Sistem." },
  { "en": "Apa Itu Keteramatan (Observability)?", "id": "Kemampuan Memperkirakan Keadaan Internal." },
  { "en": "Apa Itu Penempatan Kutub (Pole Placement)?", "id": "Teknik Desain Kontroler Umpan Balik Keadaan." },
  { "en": "Apa Fungsi Resistor Shunt?", "id": "Mengukur Arus Besar Dengan Voltmeter." },
  { "en": "Apa Itu Termokopel Tipe K?", "id": "Terbuat Dari Chromel-Alumel." },
  { "en": "Apa Itu Termokopel Tipe J?", "id": "Terbuat Dari Besi-Konstantan." },
  { "en": "Apa Itu Kompensasi Sambungan Dingin?", "id": "Mengoreksi Pembacaan Suhu Termokopel." },
  { "en": "Beda Elektrostriksi Dan Piezoelektrik?", "id": "Piezoelektrik Linier, Elektrostriksi Kuadratik." },
  { "en": "Apa Efek Magnetostriksi?", "id": "Perubahan Bentuk Material Akibat Medan Magnet." },
  { "en": "Beda Efikasi Dan Efisiensi Luminus?", "id": "Efikasi Terkait Mata Manusia." },
  { "en": "Apa Itu Pembangkit Listrik Darurat?", "id": "Sama Dengan Emergency Power Supply System (EPSS)." },
  { "en": "Apa Itu Motor Stepper Hibrida?", "id": "Menggabungkan Fitur Magnet Permanen Dan VR (Variable Reluctance)." },
  { "en": "Apa Itu Microstepping?", "id": "Meningkatkan Resolusi Gerakan Motor Stepper." },
  { "en": "Apa Itu Torsi Penahan (Detent Torque)?", "id": "Torsi Motor Stepper Tanpa Eksitasi." },
  { "en": "Apa Itu Torsi Tarik Masuk (Pull-in Torque)?", "id": "Torsi Maksimum Motor Stepper Untuk Start." },
  { "en": "Apa Itu Torsi Tarik Keluar (Pull-out Torque)?", "id": "Torsi Maksimum Motor Stepper Saat Berjalan." },
  { "en": "Apa Itu Kurva Torsi-Kecepatan?", "id": "Grafik Kinerja Torsi Motor Pada Berbagai Kecepatan." },
  { "en": "Apa Itu Backdrivability?", "id": "Kemampuan Memutar Output Shaft Secara Manual." },
  { "en": "Apa Itu Zero Backlash?", "id": "Tidak Ada Gerak Longgar Dalam Mekanisme." },
  { "en": "Apa Itu Aktuator Piezoelektrik?", "id": "Aktuator Yang Menggunakan Efek Piezoelektrik." },
  { "en": "Apa Itu Paduan Memori Bentuk (SMA)?", "id": "Material Yang Kembali Ke Bentuk Asli." },
  { "en": "Apa Kepanjangan SMA (Shape-Memory Alloy)?", "id": "Shape-Memory Alloy." },
  { "en": "Apa Itu Cairan Magnetoreologikal (MR Fluid)?", "id": "Cairan Yang Mengental Dengan Medan Magnet." },
  { "en": "Apa Itu Cairan Elektroreologikal (ER Fluid)?", "id": "Cairan Yang Mengental Dengan Medan Listrik." },
  { "en": "Apa Itu Motor Ultrasonik?", "id": "Motor Yang Menggunakan Getaran Piezoelektrik." },
  { "en": "Apa Itu Otomasi Proses Robotik (RPA)?", "id": "Otomatisasi Proses Bisnis Berbasis Perangkat Lunak." },
  { "en": "Apa Kepanjangan RPA (Robotic Process Automation)?", "id": "Robotic Process Automation." },
  { "en": "Apa Itu Visi Komputer?", "id": "Kemampuan Komputer Untuk 'Melihat'." },
  { "en": "Apa Itu Pengolahan Citra Digital?", "id": "Memanipulasi Gambar Menggunakan Algoritma Komputer." },
  { "en": "Apa Itu Segmentasi Gambar?", "id": "Membagi Gambar Menjadi Beberapa Segmen." },
  { "en": "Apa Itu Ekstraksi Fitur?", "id": "Mengidentifikasi Karakteristik Kunci Dalam Data." },
  { "en": "Apa Itu Jaringan Saraf Tiruan (ANN)?", "id": "Model Komputasi Terinspirasi Otak Biologis." },
  { "en": "Apa Kepanjangan ANN (Artificial Neural Network)?", "id": "Artificial Neural Network." },
  { "en": "Apa Itu Deep Learning?", "id": "Sub-bidang Machine Learning (ML) Dengan Jaringan Berlapis." },
  { "en": "Apa Itu Jaringan Saraf Konvolusional (CNN)?", "id": "Jenis Jaringan Saraf Untuk Analisis Gambar." },
  { "en": "Apa Kepanjangan CNN (Convolutional Neural Network)?", "id": "Convolutional Neural Network." },
  { "en": "Apa Itu Jaringan Saraf Berulang (RNN)?", "id": "Jenis Jaringan Saraf Untuk Data Urutan." },
  { "en": "Apa Kepanjangan RNN (Recurrent Neural Network)?", "id": "Recurrent Neural Network." },
  { "en": "Apa Itu Pembelajaran Terbimbing (Supervised Learning)?", "id": "Belajar Dari Data Yang Diberi Label." },
  { "en": "Apa Itu Pembelajaran Tak Terbimbing (Unsupervised Learning)?", "id": "Belajar Dari Data Yang Tidak Diberi Label." },
  { "en": "Apa Itu Pembelajaran Penguatan (Reinforcement Learning)?", "id": "Belajar Melalui Coba-Coba Dengan Imbalan." },
  { "en": "Apa Itu Natural Language Processing (NLP)?", "id": "Interaksi Komputer Dengan Bahasa Manusia." },
  { "en": "Apa Kepanjangan NLP?", "id": "Natural Language Processing." },
  { "en": "Apa Itu Big Data?", "id": "Kumpulan Data Sangat Besar Dan Kompleks." },
  { "en": "Apa Itu Tiga V Big Data?", "id": "Volume, Velocity, Variety." },
  { "en": "Apa Itu Data Mining?", "id": "Proses Menemukan Pola Dalam Data Besar." },
  { "en": "Apa Itu Analisis Prediktif?", "id": "Menggunakan Data Untuk Memprediksi Hasil Masa Depan." },
  { "en": "Apa Itu Blockchain?", "id": "Buku Besar Digital Terdistribusi Yang Aman." },
  { "en": "Apa Itu Smart Contract?", "id": "Kontrak Yang Mengeksekusi Diri Sendiri." },
  { "en": "Apa Itu Aset Digital?", "id": "Representasi Nilai Secara Digital." },
  { "en": "Apa Itu Cryptocurrency?", "id": "Mata Uang Digital Menggunakan Kriptografi." },
  { "en": "Apa Itu Penambangan (Mining) Cryptocurrency?", "id": "Proses Verifikasi Transaksi Blockchain." },
  { "en": "Apa Itu Bukti Kerja (Proof of Work)?", "id": "Mekanisme Konsensus Blockchain." },
  { "en": "Apa Itu Bukti Kepemilikan (Proof of Stake)?", "id": "Mekanisme Konsensus Blockchain Alternatif." },
  { "en": "Apa Itu Tokenisasi?", "id": "Proses Mengubah Aset Menjadi Token Digital." },
  { "en": "Apa Itu Initial Coin Offering (ICO)?", "id": "Metode Penggalangan Dana Menggunakan Cryptocurrency." },
  { "en": "Apa Itu Keuangan Terdesentralisasi (DeFi)?", "id": "Sistem Keuangan Berbasis Blockchain." },
  { "en": "Apa Itu Non-Fungible Token (NFT)?", "id": "Token Unik Yang Mewakili Kepemilikan Aset." },
  { "en": "Apa Itu Metaverse?", "id": "Dunia Virtual Tiga Dimensi." },
  { "en": "Apa Itu Virtual Reality (VR)?", "id": "Pengalaman Imersif Dalam Lingkungan Buatan." },
  { "en": "Apa Itu Augmented Reality (AR)?", "id": "Menambahkan Elemen Virtual Ke Dunia Nyata." },
  { "en": "Apa Itu Mixed Reality (MR)?", "id": "Interaksi Dunia Nyata Dan Virtual." },
  { "en": "Apa Itu Haptic Feedback?", "id": "Umpan Balik Sentuhan Atau Getaran." },
  { "en": "Apa Itu Internet of Bodies (IoB)?", "id": "Jaringan Perangkat Terhubung Dengan Tubuh Manusia." },
  { "en": "Apa Itu Antarmuka Otak-Komputer (BCI)?", "id": "Jalur Komunikasi Langsung Antara Otak-Komputer." },
  { "en": "Apa Kepanjangan BCI (Brain-Computer Interface)?", "id": "Brain-Computer Interface." },
  { "en": "Apa Itu Komputasi Tepi (Edge Computing)?", "id": "Pemrosesan Data Dekat Dengan Sumbernya." },
  { "en": "Apa Itu Fog Computing?", "id": "Lapisan Komputasi Antara Edge Dan Cloud." },
  { "en": "Apa Itu Serverless Computing?", "id": "Model Eksekusi Cloud Dimana Penyedia Mengelola Server." },
  { "en": "Apa Itu Microservices Architecture?", "id": "Membangun Aplikasi Sebagai Kumpulan Layanan Kecil." },
  { "en": "Apa Itu Kontainerisasi?", "id": "Mengemas Aplikasi Dan Dependensinya." },
  { "en": "Apa Itu Docker?", "id": "Platform Populer Untuk Kontainerisasi." },
  { "en": "Apa Itu Kubernetes?", "id": "Platform Orkestrasi Kontainer." },
  { "en": "Apa Itu DevOps?", "id": "Praktik Yang Menggabungkan Pengembangan Dan Operasi." },
  { "en": "Apa Itu Continuous Integration (CI)?", "id": "Mengintegrasikan Perubahan Kode Secara Teratur." },
  { "en": "Apa Itu Continuous Delivery (CD)?", "id": "Merilis Perubahan Kode Secara Otomatis." },
  { "en": "Apa Itu Infrastructure as Code (IaC)?", "id": "Mengelola Infrastruktur Menggunakan Kode." },
  { "en": "Apa Kepanjangan SSTI (Subsynchronous Torsional Interaction)?", "id": "Subsynchronous Torsional Interaction." },
  { "en": "Apa Itu SSTI (Subsynchronous Torsional Interaction)?", "id": "Interaksi Antara Sistem Listrik Dan Mekanis." },
  { "en": "Apa Itu Studi Interaksi Kontrol?", "id": "Menganalisis Interaksi Antar Kontroler Elektronik." },
  { "en": "Apa Itu Faktor Transien CT (Current Transformer)?", "id": "Kemampuan CT (Current Transformer) Menghindari Saturasi Saat Gangguan." },
  { "en": "Apa Itu Proteksi Line Current Differential (87L)?", "id": "Proteksi Differensial Untuk Saluran Transmisi." },
  { "en": "Apa Itu Pelindung Sambaran Langsung Petir?", "id": "Melindungi Peralatan Gardu Dari Sambaran Langsung." },
  { "en": "Bagaimana Ukuran Konduktor Grounding Ditentukan?", "id": "Berdasarkan Standar IEEE (Institute of Electrical and Electronics Engineers) 80." },
  { "en": "Bagaimana Cara Kerja Flicker Meter?", "id": "Mensimulasikan Respon Sistem Mata-Otak Manusia." },
  { "en": "Apa Itu Kurva Bebek (Duck Curve)?", "id": "Grafik Beban Jaringan Dengan Penetrasi Surya Tinggi." },
  { "en": "Apa Itu Inverter Clipping?", "id": "Pembatasan Daya Output Inverter Surya." },
  { "en": "Apa Itu Kurva Daya Turbin Angin?", "id": "Grafik Output Daya Terhadap Kecepatan Angin." },
  { "en": "Apa Itu Model Degradasi Baterai?", "id": "Model Matematis Untuk Memprediksi Penuaan Baterai." },
  { "en": "Apa Itu Thermal Runaway Baterai?", "id": "Reaksi Berantai Peningkatan Suhu Tak Terkendali." },
  { "en": "Apa Konsep V2B (Vehicle-to-Building)?", "id": "Mobil Listrik Menyuplai Daya Ke Gedung." },
  { "en": "Apa Konsep V2L (Vehicle-to-Load)?", "id": "Mobil Listrik Menyuplai Daya Langsung Ke Beban." },
  { "en": "Apa Itu Identifikasi Sistem?", "id": "Membangun Model Matematis Dari Data Pengukuran." },
  { "en": "Apa Itu Luenberger Observer?", "id": "Estimator Keadaan Untuk Sistem Linier." },
  { "en": "Apa Itu Filter Kalman?", "id": "Algoritma Untuk Mengestimasi Keadaan Sistem Dinamis." },
  { "en": "Apa Itu Analisis Tembaga Terlarut?", "id": "Mendeteksi Overheating Pada Sambungan Tembaga Trafo." },
  { "en": "Apa Itu Pemantauan Partial Discharge (PD) Online?", "id": "Mendeteksi PD (Partial Discharge) Saat Trafo Beroperasi." },
  { "en": "Apa Itu Uji Imunitas Radiasi?", "id": "Menguji Kemampuan Peralatan Menahan Gangguan Elektromagnetik." },
  { "en": "Beda Ruang Anechoic Dan Reverberation?", "id": "Anechoic Menyerap Gelombang, Reverberation Memantulkan." },
  { "en": "Apa Kepanjangan HAZOP (Hazard and Operability)?", "id": "Hazard and Operability Study." },
  { "en": "Apa Tujuan Studi HAZOP (Hazard and Operability)?", "id": "Mengidentifikasi Risiko Proses Dan Operasi." },
  { "en": "Apa Kepanjangan FMECA?", "id": "Failure Mode, Effects, and Criticality Analysis." },
  { "en": "Beda FMEA Dan FMECA?", "id": "FMECA (Failure Mode, Effects, and Criticality Analysis) Menambahkan Analisis Kritikalitas." },
  { "en": "Apa Itu Diagram Blok?", "id": "Representasi Grafis Sistem Menggunakan Blok." },
  { "en": "Apa Itu Diagram Aliran Sinyal?", "id": "Representasi Grafis Sistem Persamaan Aljabar." },
  { "en": "Apa Itu Gain?", "id": "Rasio Amplitudo Sinyal Output Terhadap Input." },
  { "en": "Apa Itu Pergeseran Fasa?", "id": "Perbedaan Fasa Antara Sinyal Output-Input." },
  { "en": "Apa Itu Respon Frekuensi?", "id": "Respon Sistem Terhadap Input Sinusoidal." },
  { "en": "Apa Itu Sistem Orde Pertama?", "id": "Sistem Yang Dideskripsikan Persamaan Diferensial Orde Satu." },
  { "en": "Apa Itu Sistem Orde Kedua?", "id": "Sistem Yang Dideskripsikan Persamaan Diferensial Orde Dua." },
  { "en": "Apa Itu Waktu Naik (Rise Time)?", "id": "Waktu Respon Dari 10% Hingga 90%." },
  { "en": "Apa Itu Waktu Tunda (Delay Time)?", "id": "Waktu Respon Mencapai 50% Nilai Akhir." },
  { "en": "Apa Itu Waktu Tepat (Settling Time)?", "id": "Waktu Respon Menetap Dalam Rentang Tertentu." },
  { "en": "Apa Itu Overshoot?", "id": "Respon Yang Melebihi Nilai Akhir." },
  { "en": "Apa Itu Undershoot?", "id": "Respon Yang Turun Di Bawah Nilai Akhir." },
  { "en": "Apa Itu Kesalahan Keadaan Tunak (Steady-State Error)?", "id": "Perbedaan Antara Output Dan Referensi." },
  { "en": "Apa Itu Tipe Sistem Kontrol?", "id": "Menentukan Kemampuan Sistem Mengikuti Input." },
  { "en": "Apa Itu Kontroler Proporsional (P)?", "id": "Output Kontroler Proporsional Terhadap Error." },
  { "en": "Apa Itu Kontroler Integral (I)?", "id": "Output Kontroler Proporsional Terhadap Integral Error." },
  { "en": "Apa Itu Kontroler Derivatif (D)?", "id": "Output Kontroler Proporsional Terhadap Laju Perubahan Error." },
  { "en": "Apa Itu Kontroler PI (Proportional-Integral)?", "id": "Menggabungkan Aksi Proporsional Dan Integral." },
  { "en": "Apa Itu Kontroler PD (Proportional-Derivative)?", "id": "Menggabungkan Aksi Proporsional Dan Derivatif." },
  { "en": "Apa Itu Anti-Windup?", "id": "Mencegah Penjenuhan Bagian Integral Kontroler." },
  { "en": "Apa Itu Sensor Cerdas?", "id": "Sensor Dengan Kemampuan Pemrosesan Internal." },
  { "en": "Apa Itu Jaringan Sensor Nirkabel (WSN)?", "id": "Jaringan Sensor Yang Berkomunikasi Nirkabel." },
  { "en": "Apa Kepanjangan WSN (Wireless Sensor Network)?", "id": "Wireless Sensor Network." },
  { "en": "Apa Itu Fusi Sensor?", "id": "Menggabungkan Data Dari Beberapa Sensor." },
  { "en": "Apa Itu Redundansi Sensor?", "id": "Menggunakan Beberapa Sensor Untuk Keandalan." },
  { "en": "Apa Itu Kalibrasi Lapangan?", "id": "Kalibrasi Instrumen Di Lokasi Pemasangan." },
  { "en": "Apa Itu Verifikasi Kalibrasi?", "id": "Memeriksa Apakah Kalibrasi Masih Valid." },
  { "en": "Apa Itu Rentang Dinamis?", "id": "Rasio Antara Nilai Terbesar-Terkecil." },
  { "en": "Apa Itu Sensitivitas?", "id": "Perubahan Output Per Perubahan Input." },
  { "en": "Apa Itu Resolusi?", "id": "Perubahan Input Terkecil Yang Dapat Dideteksi." },
  { "en": "Apa Itu Histeresis?", "id": "Perbedaan Output Saat Input Naik-Turun." },
  { "en": "Apa Itu Repeatability?", "id": "Kemampuan Memberikan Hasil Sama Berulang Kali." },
  { "en": "Apa Itu Reproducibility?", "id": "Kemampuan Direproduksi Dalam Kondisi Berbeda." },
  { "en": "Apa Itu Drift?", "id": "Perubahan Karakteristik Instrumen Seiring Waktu." },
  { "en": "Apa Itu Zero Drift?", "id": "Pergeseran Titik Nol Instrumen." },
  { "en": "Apa Itu Span Drift?", "id": "Perubahan Rentang Ukur Instrumen." },
  { "en": "Apa Itu Efek Pemuatan (Loading Effect)?", "id": "Pengaruh Instrumen Terhadap Sirkuit Yang Diukur." },
  { "en": "Apa Itu Impedansi Input Tinggi?", "id": "Mengurangi Efek Pemuatan Saat Mengukur Tegangan." },
  { "en": "Apa Itu Impedansi Output Rendah?", "id": "Meminimalkan Penurunan Tegangan Saat Memberi Beban." },
  { "en": "Apa Itu Galvanometer?", "id": "Alat Ukur Arus Sangat Kecil." },
  { "en": "Apa Itu Ammeter?", "id": "Sama Dengan Amperemeter." },
  { "en": "Apa Itu Voltmeter?", "id": "Alat Ukur Tegangan." },
  { "en": "Apa Itu Ohmmeter?", "id": "Alat Ukur Resistansi." },
  { "en": "Apa Itu Wattmeter?", "id": "Alat Ukur Daya Listrik Aktif." },
  { "en": "Apa Itu VARmeter?", "id": "Alat Ukur Daya Listrik Reaktif." },
  { "en": "Apa Itu Meter Faktor Daya?", "id": "Mengukur Faktor Daya (Cos φ)." },
  { "en": "Apa Itu Meter Energi?", "id": "Mengukur Konsumsi Energi Listrik (kWh)." },
  { "en": "Apa Itu Osiloskop?", "id": "Alat Visualisasi Bentuk Gelombang Sinyal." },
  { "en": "Apa Itu Penganalisis Logika (Logic Analyzer)?", "id": "Menganalisis Sinyal Digital." },
  { "en": "Apa Itu Generator Sinyal?", "id": "Menghasilkan Berbagai Bentuk Gelombang Uji." },
  { "en": "Apa Itu Penganalisis Spektrum?", "id": "Mengukur Sinyal Dalam Domain Frekuensi." },
  { "en": "Apa Itu LCR (Induktor, Kapasitor, Resistor) Meter?", "id": "Mengukur Induktansi, Kapasitansi, Resistansi." },
  { "en": "Apa Itu Jembatan Pengukuran?", "id": "Sirkuit Untuk Pengukuran Presisi." },
  { "en": "Apa Itu Pengkondisian Sinyal?", "id": "Memproses Sinyal Agar Sesuai Untuk Pengukuran." },
  { "en": "Apa Itu Amplifikasi?", "id": "Proses Penguatan Sinyal." },
  { "en": "Apa Itu Atenuasi?", "id": "Proses Pelemahan Sinyal." },
  { "en": "Apa Itu Filtrasi?", "id": "Proses Menghilangkan Komponen Frekuensi Tertentu." },
  { "en": "Apa Itu Linearisasi?", "id": "Membuat Hubungan Input-Output Menjadi Linier." },
  { "en": "Apa Itu Isolasi Sinyal?", "id": "Memisahkan Sirkuit Pengukuran Dari Sumber." },
  { "en": "Apa Itu Common Mode Rejection?", "id": "Kemampuan Menolak Derau Mode Bersama." },
  { "en": "Apa Itu Grounding Dan Shielding?", "id": "Teknik Untuk Mengurangi Derau Dan Interferensi." },
  { "en": "Apa Itu Error Sistematik?", "id": "Error Yang Konsisten Dan Dapat Diprediksi." },
  { "en": "Apa Itu Error Acak?", "id": "Error Yang Tidak Dapat Diprediksi." },
  { "en": "Apa Itu Akurasi Kelas?", "id": "Menunjukkan Batas Kesalahan Maksimum Instrumen." },
  { "en": "Apa Itu Kalibrator?", "id": "Sumber Referensi Akurat Untuk Kalibrasi." },
  { "en": "Apa Itu Standar Referensi?", "id": "Objek Atau Sistem Sebagai Dasar Pengukuran." },
  { "en": "Apa Itu Sistem Akuisisi Data (DAQ)?", "id": "Sistem Untuk Mengukur Dan Merekam Data." },
  { "en": "Apa Kepanjangan DAQ?", "id": "Data Acquisition." },
  { "en": "Apa Itu Laju Sampling?", "id": "Seberapa Sering Sinyal Analog Diukur." },
  { "en": "Apa Itu Multiplexer?", "id": "Memilih Satu Dari Banyak Saluran Input." },
  { "en": "Apa Itu Sample-and-Hold Circuit?", "id": "Mempertahankan Nilai Sampel Sinyal." },
  { "en": "Apa Itu Skema Autoreclosing Single-Shot?", "id": "Pemutus Sirkuit Mencoba Menutup Kembali Satu Kali." },
  { "en": "Apa Itu Logika Weak Infeed?", "id": "Proteksi Untuk Kondisi Sumber Gangguan Lemah." },
  { "en": "Apa Itu Logika Current Reversal?", "id": "Mengatasi Salah Operasi Relay Pada Saluran Paralel." },
  { "en": "Apa Beda Sag, Swell, Interruption?", "id": "Sag Turun, Swell Naik, Interruption Nol." },
  { "en": "Apa Itu Interupsi Sesaat (Momentary)?", "id": "Pemadaman Listrik Sangat Singkat (Kurang Dari 3 Detik)." },
  { "en": "Apa Itu Interupsi Sementara (Temporary)?", "id": "Pemadaman Listrik Durasi Menengah." },
  { "en": "Apa Itu Interupsi Berkelanjutan (Sustained)?", "id": "Pemadaman Listrik Durasi Panjang." },
  { "en": "Apa Itu Koefisien Ionisasi Townsend Pertama?", "id": "Jumlah Ionisasi Per Satuan Panjang." },
  { "en": "Apa Itu Logical Nodes Dalam IEC (International Electrotechnical Commission) 61850?", "id": "Representasi Fungsional Perangkat Gardu Induk." },
  { "en": "Apa Itu Merging Unit (MU)?", "id": "Mengonversi Sinyal Analog Menjadi Sampel Digital (SMV)." },
  { "en": "Apa Itu Reaktansi Subtransien Mesin Sinkron?", "id": "Reaktansi Awal Saat Terjadi Gangguan." },
  { "en": "Apa Itu Reaktansi Transien Mesin Sinkron?", "id": "Reaktansi Setelah Periode Subtransien." },
  { "en": "Apa Itu Reaktansi Sinkron?", "id": "Reaktansi Mesin Dalam Kondisi Tunak." },
  { "en": "Apa Itu Konverter Dua Arah?", "id": "Konverter Dimana Aliran Daya Bisa Dua Arah." },
  { "en": "Beda Kontrol Pitch Dan Stall?", "id": "Pitch Mengatur Sudut Bilah, Stall Mengandalkan Aerodinamika." },
  { "en": "Apa Kepanjangan FDS (Frequency Domain Spectroscopy)?", "id": "Frequency Domain Spectroscopy." },
  { "en": "Apa Fungsi Tes FDS (Frequency Domain Spectroscopy)?", "id": "Menganalisis Kondisi Isolasi Pada Berbagai Frekuensi." },
  { "en": "Apa Itu Hirarki Kontrol Bahaya?", "id": "Urutan Prioritas Pengendalian Risiko." },
  { "en": "Sebutkan Hirarki Kontrol Bahaya?", "id": "Eliminasi, Substitusi, Rekayasa, Administratif, PPE (Personal Protective Equipment)." },
  { "en": "Apa Itu Verifikasi LOTO (Lockout/Tagout)?", "id": "Memastikan Sistem Benar-benar Mati." },
  { "en": "Apa Bentuk Integral Hukum Gauss?", "id": "Fluks Listrik Total Sama Dengan Muatan Tertutup." },
  { "en": "Apa Teorema Poynting?", "id": "Menjelaskan Konservasi Energi Dalam Medan Elektromagnetik." },
  { "en": "Apa Itu Mode Gelombang TEM (Transverse Electromagnetic)?", "id": "Medan Listrik Dan Magnet Tegak Lurus Arah." },
  { "en": "Apa Itu Frekuensi Cutoff Waveguide?", "id": "Frekuensi Minimum Untuk Propagasi Gelombang." },
  { "en": "Apa Itu Rugi Return (Return Loss)?", "id": "Ukuran Daya Yang Dipantulkan Kembali." },
  { "en": "Apa Itu Rugi Penyisipan (Insertion Loss)?", "id": "Kehilangan Daya Akibat Penyisipan Perangkat." },
  { "en": "Apa Itu VSWR (Voltage Standing Wave Ratio)?", "id": "Rasio Tegangan Maksimum-Minimum Gelombang Berdiri." },
  { "en": "Apa Nilai VSWR (Voltage Standing Wave Ratio) Ideal?", "id": "1:1 (Tidak Ada Pantulan)." },
  { "en": "Apa Itu Penguat Terdistribusi?", "id": "Amplifier Untuk Bandwidth Sangat Lebar." },
  { "en": "Apa Itu Modulator I/Q?", "id": "Memodulasi Sinyal In-Phase Dan Quadrature." },
  { "en": "Apa Itu Error Vector Magnitude (EVM)?", "id": "Ukuran Akurasi Skema Modulasi Digital." },
  { "en": "Apa Kepanjangan EVM (Error Vector Magnitude)?", "id": "Error Vector Magnitude." },
  { "en": "Apa Itu Adjacent Channel Power Ratio (ACPR)?", "id": "Ukuran Interferensi Ke Kanal Sebelah." },
  { "en": "Apa Kepanjangan ACPR?", "id": "Adjacent Channel Power Ratio." },
  { "en": "Apa Itu Kode Gray?", "id": "Urutan Biner Dimana Hanya Satu Bit Berubah." },
  { "en": "Apa Itu Hamming Distance?", "id": "Jumlah Posisi Bit Yang Berbeda." },
  { "en": "Apa Itu Kode Hamming?", "id": "Jenis Kode Deteksi Dan Koreksi Error." },
  { "en": "Apa Itu Cyclic Code?", "id": "Jenis Kode Linier Blok." },
  { "en": "Apa Itu Reed-Solomon Code?", "id": "Kode Koreksi Error Yang Kuat." },
  { "en": "Apa Itu Turbo Code?", "id": "Kode Koreksi Error Kinerja Tinggi." },
  { "en": "Apa Itu Low-Density Parity-Check (LDPC) Code?", "id": "Kode Koreksi Error Mendekati Batas Shannon." },
  { "en": "Apa Itu Trellis Diagram?", "id": "Diagram Keadaan Untuk Menganalisis Kode Konvolusional." },
  { "en": "Apa Itu Algoritma Viterbi?", "id": "Algoritma Dekode Untuk Kode Konvolusional." },
  { "en": "Apa Itu Interleaving?", "id": "Mengubah Urutan Data Untuk Melawan Burst Error." },
  { "en": "Apa Itu Burst Error?", "id": "Error Yang Terjadi Secara Berurutan." },
  { "en": "Apa Itu Forward Error Correction (FEC)?", "id": "Mengirim Data Redundan Untuk Koreksi Error." },
  { "en": "Apa Itu Automatic Repeat Request (ARQ)?", "id": "Meminta Ulang Data Jika Terjadi Error." },
  { "en": "Apa Itu Hybrid ARQ (Automatic Repeat Request)?", "id": "Menggabungkan FEC (Forward Error Correction) Dan ARQ (Automatic Repeat Request)." },
  { "en": "Apa Itu Time-Division Multiplexing (TDM)?", "id": "Berbagi Kanal Dengan Slot Waktu." },
  { "en": "Apa Itu Frequency-Division Multiplexing (FDM)?", "id": "Berbagi Kanal Dengan Pita Frekuensi." },
  { "en": "Apa Itu Code-Division Multiple Access (CDMA)?", "id": "Berbagi Kanal Menggunakan Kode Unik." },
  { "en": "Apa Itu Orthogonal Variable Spreading Factor (OVSF)?", "id": "Teknik Kode Dalam Sistem WCDMA (Wideband CDMA)." },
  { "en": "Apa Itu SCADA (Supervisory Control And Data Acquisition) Master?", "id": "Unit Pusat Pengendali Sistem SCADA (Supervisory Control And Data Acquisition)." },
  { "en": "Apa Itu SCADA (Supervisory Control And Data Acquisition) RTU (Remote Terminal Unit)?", "id": "Unit Jarak Jauh Pengumpul Data." },
  { "en": "Apa Itu Protokol Modbus?", "id": "Protokol Komunikasi Populer Dalam Otomasi." },
  { "en": "Apa Itu Jaringan Foundation Fieldbus?", "id": "Jaringan Digital Dua Arah Untuk Instrumen." },
  { "en": "Apa Itu Diagram P&ID (Piping and Instrumentation Diagram)?", "id": "Diagram Teknik Detail Proses Industri." },
  { "en": "Apa Itu Sistem Interlock Keselamatan?", "id": "Sistem Yang Mencegah Kondisi Operasi Berbahaya." },
  { "en": "Apa Itu Cause and Effect Diagram?", "id": "Matriks Yang Mendefinisikan Logika Keselamatan." },
  { "en": "Apa Itu Manajemen Bypass?", "id": "Prosedur Mengelola Bypass Fungsi Keselamatan." },
  { "en": "Apa Itu Pengujian Fungsional?", "id": "Memverifikasi Bahwa Sistem Bekerja Sesuai Desain." },
  { "en": "Apa Itu Pengujian Kinerja?", "id": "Mengevaluasi Kinerja Sistem Di Bawah Beban." },
  { "en": "Apa Itu Pengujian Stres?", "id": "Menguji Batas Kemampuan Sistem." },
  { "en": "Apa Itu Pengujian Regresi?", "id": "Memastikan Perubahan Tidak Merusak Fungsi Yang Ada." },
  { "en": "Apa Itu Studi Keteroperasian?", "id": "Menguji Kemampuan Sistem Berbeda Bekerja Sama." },
  { "en": "Apa Itu Model Air Terjun (Waterfall)?", "id": "Model Pengembangan Perangkat Lunak Sekuensial." },
  { "en": "Apa Itu Model Agile?", "id": "Model Pengembangan Perangkat Lunak Iteratif." },
  { "en": "Apa Itu Scrum?", "id": "Kerangka Kerja Manajemen Proyek Agile." },
  { "en": "Apa Itu Sprint?", "id": "Siklus Pengembangan Waktu Tetap Dalam Scrum." },
  { "en": "Apa Itu Kanban?", "id": "Metode Visual Untuk Mengelola Alur Kerja." },
  { "en": "Apa Itu Lean Manufacturing?", "id": "Filosofi Produksi Untuk Mengurangi Pemborosan." },
  { "en": "Apa Itu Six Sigma?", "id": "Metodologi Untuk Peningkatan Kualitas Proses." },
  { "en": "Apa Itu DMAIC (Define, Measure, Analyze, Improve, Control)?", "id": "Siklus Peningkatan Dalam Six Sigma." },
  { "en": "Apa Itu Analisis Pareto?", "id": "Prinsip 80/20, Fokus Pada Masalah Vital." },
  { "en": "Apa Itu Poka-Yoke?", "id": "Mekanisme Pencegahan Kesalahan (Mistake-Proofing)." },
  { "en": "Apa Itu Kaizen?", "id": "Filosofi Peningkatan Berkelanjutan." },
  { "en": "Apa Itu 5S?", "id": "Metodologi Organisasi Tempat Kerja." },
  { "en": "Sebutkan Lima S?", "id": "Sort, Set In Order, Shine, Standardize, Sustain." },
  { "en": "Apa Itu Total Productive Maintenance (TPM)?", "id": "Strategi Perawatan Melibatkan Semua Karyawan." },
  { "en": "Apa Itu Overall Equipment Effectiveness (OEE)?", "id": "Ukuran Kinerja Manufaktur." },
  { "en": "Apa Kepanjangan OEE?", "id": "Overall Equipment Effectiveness." },
  { "en": "Apa Tiga Komponen OEE?", "id": "Availability, Performance, Quality." },
  { "en": "Apa Itu Just-In-Time (JIT)?", "id": "Strategi Manufaktur Untuk Mengurangi Inventaris." },
  { "en": "Apa Itu Root Cause Analysis (RCA)?", "id": "Metodologi Pencarian Akar Penyebab Masalah." },
  { "en": "Apa Itu Diagram Afinitas?", "id": "Alat Untuk Mengelompokkan Ide." },
  { "en": "Apa Itu Brainstorming?", "id": "Teknik Kreatif Untuk Menghasilkan Ide." },
  { "en": "Apa Itu Benchmarking?", "id": "Membandingkan Kinerja Dengan Yang Terbaik." },
  { "en": "Apa Itu Key Performance Indicator (KPI)?", "id": "Ukuran Kinerja Kunci." },
  { "en": "Apa Kepanjangan KPI?", "id": "Key Performance Indicator." },
  { "en": "Apa Itu Balanced Scorecard?", "id": "Kerangka Kerja Manajemen Kinerja Strategis." },
  { "en": "Apa Itu Manajemen Perubahan?", "id": "Pendekatan Mengelola Transisi Organisasi." },
  { "en": "Apa Itu Kurva Adopsi Inovasi?", "id": "Model Difusi Inovasi Dalam Masyarakat." },
  { "en": "Apa Itu Analisis PESTLE?", "id": "Analisis Faktor Political, Economic, Social, Technological, Legal, Environmental." },
  { "en": "Apa Itu Lima Kekuatan Porter?", "id": "Kerangka Kerja Analisis Kompetitif Industri." },
  { "en": "Apa Itu Matriks BCG?", "id": "Matriks Pertumbuhan-Pangsa Pasar." }



        ];

        let questions = [];

        rawVocabularyList.sort((a, b) => {
            const enA = a.en.toLowerCase();
            const enB = b.en.toLowerCase();
            if (enA < enB) return -1;
            if (enA > enB) return 1;
            return 0;
        });

        function generateQuestions() {
            const allIndonesianTranslations = rawVocabularyList.map(item => item.id);
            questions = [];
            rawVocabularyList.forEach(vocabItem => {
                const correctAnswer = vocabItem.id;
                const distractors = [];
                let attempts = 0;
                while (distractors.length < 3 && attempts < allIndonesianTranslations.length * 2) {
                    const randomIndex = Math.floor(Math.random() * allIndonesianTranslations.length);
                    const potentialDistractor = allIndonesianTranslations[randomIndex];
                    if (potentialDistractor !== correctAnswer && !distractors.includes(potentialDistractor)) {
                        distractors.push(potentialDistractor);
                    }
                    attempts++;
                }
                while (distractors.length < 3) {
                    const fallbackOptions = ["opsi lain A", "opsi lain B", "opsi lain C", "opsi lain D", "opsi lain E", "opsi lain F"];
                    let fallbackIndex = 0;
                    let safetyNet = 0;
                    while(distractors.length < 3 && safetyNet < fallbackOptions.length * 3) {
                        const fbOption = fallbackOptions[fallbackIndex % fallbackOptions.length] + `_${distractors.length}${Math.floor(Math.random()*100)}`;
                        if (fbOption !== correctAnswer && !distractors.includes(fbOption)) {
                             distractors.push(fbOption);
                        }
                        fallbackIndex++;
                        safetyNet++;
                    }
                     if(distractors.length < 3) {
                        for(let i=0; i < (3-distractors.length); i++){
                            distractors.push("pilihan default " + (i+1+distractors.length) + Math.random().toString(36).substring(7));
                        }
                     }
                }
                const answerOptions = [
                    { text: correctAnswer, correct: true },
                    { text: distractors[0], correct: false },
                    { text: distractors[1], correct: false },
                    { text: distractors[2], correct: false }
                ];
                questions.push({
                    question: vocabItem.en,
                    answers: answerOptions
                });
            });
        }

        generateQuestions();

        function saveProgress() {
            if (!questionContainerElement.classList.contains('hide') && orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                 const progress = {
                    currentQuestionIndex: currentQuestionIndex,
                    score: score,
                    orderedQuestions: orderedQuestions
                };
                localStorage.setItem('quizProgress', JSON.stringify(progress));
            }
        }

        function loadProgress() {
            const savedProgress = localStorage.getItem('quizProgress');
            if (savedProgress) {
                try {
                    const progressData = JSON.parse(savedProgress);
                    if (progressData && typeof progressData.currentQuestionIndex === 'number' &&
                        typeof progressData.score === 'number' && Array.isArray(progressData.orderedQuestions) &&
                        progressData.orderedQuestions.length > 0 &&
                        progressData.currentQuestionIndex < progressData.orderedQuestions.length &&
                        progressData.orderedQuestions.length === questions.length) { // Validasi tambahan: jumlah soal harus sama
                        return progressData;
                    } else {
                        clearProgress();
                        return null;
                    }
                } catch (e) {
                    console.error("Error parsing saved progress:", e);
                    clearProgress();
                    return null;
                }
            }
            return null;
        }

        function clearProgress() {
            localStorage.removeItem('quizProgress');
        }

        prev50Button.addEventListener('click', () => navigateQuestions(-JUMP_AMOUNT));
        prevQuestionButton.addEventListener('click', () => navigateQuestions(-1)); // Event listener untuk tombol baru
        next50Button.addEventListener('click', () => navigateQuestions(JUMP_AMOUNT));

        function navigateQuestions(amount) {
            clearTimeout(questionTimeout);
            if (!orderedQuestions || orderedQuestions.length === 0) return;

            let newIndex = currentQuestionIndex + amount;
            if (newIndex < 0) newIndex = 0;
            else if (newIndex >= orderedQuestions.length) newIndex = orderedQuestions.length - 1;

            if (newIndex !== currentQuestionIndex) {
                currentQuestionIndex = newIndex;
                setNextQuestion();
            } else {
                updateSkipButtonStates();
            }
        }

        function updateSkipButtonStates() {
            if (!orderedQuestions || orderedQuestions.length === 0 || questionContainerElement.classList.contains('hide')) {
                skipNavigationControls.classList.add('hide');
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Nonaktifkan tombol baru
                if(next50Button) next50Button.disabled = true;
                return;
            }
            skipNavigationControls.classList.remove('hide');
            const isFirstQuestion = currentQuestionIndex === 0;
            const isLastQuestion = currentQuestionIndex === (orderedQuestions.length - 1);

            if(prev50Button) prev50Button.disabled = isFirstQuestion;
            if(prevQuestionButton) prevQuestionButton.disabled = isFirstQuestion; // Atur status disabled tombol baru
            if(next50Button) next50Button.disabled = isLastQuestion;

            if (orderedQuestions.length <= 1) {
                if(prev50Button) prev50Button.disabled = true;
                if(prevQuestionButton) prevQuestionButton.disabled = true; // Atur status disabled tombol baru
                if(next50Button) next50Button.disabled = true;
            }
        }


        window.addEventListener('load', () => {
            const savedData = loadProgress();
            startButton.innerText = 'Mulai';
            completionMessageElement.classList.add('hide');
            if (savedData) {
                continueButton.classList.remove('hide');
            } else {
                continueButton.classList.add('hide');
            }
            if (questionContainerElement.classList.contains('hide')) {
                initialControls.classList.remove('hide');
                skipNavigationControls.classList.add('hide');
            } else {
                 initialControls.classList.add('hide');
                 // Mungkin juga perlu updateSkipButtonStates() di sini jika kuis dilanjutkan
                 // dan langsung menampilkan soal.
            }
        });

        startButton.addEventListener('click', () => startGame(false));
        continueButton.addEventListener('click', () => startGame(true));

        function startGame(isContinuing = false) {
            clearTimeout(questionTimeout);
            completionMessageElement.classList.add('hide');
            if (!isContinuing) {
                startButton.innerText = 'Mulai';
            }
            initialControls.classList.add('hide');
            questionContainerElement.classList.remove('hide');
            questionCounterElement.classList.remove('hide');

            const savedData = loadProgress();
            if (isContinuing && savedData && savedData.orderedQuestions && savedData.orderedQuestions.length === questions.length) {
                orderedQuestions = savedData.orderedQuestions;
                currentQuestionIndex = savedData.currentQuestionIndex;
                score = savedData.score;
            } else {
                clearProgress();
                orderedQuestions = [...questions];
                currentQuestionIndex = 0;
                score = 0;
            }

            if (!orderedQuestions || orderedQuestions.length === 0) {
                showResults();
                completionMessageElement.innerText = "Tidak ada soal untuk ditampilkan.";
                completionMessageElement.style.color = "#dc3545";
                completionMessageElement.classList.remove('hide');
                startButton.innerText = 'Mulai';
                return;
            }
            setNextQuestion();
        }

        function setNextQuestion() {
            resetState();
            if (orderedQuestions && currentQuestionIndex < orderedQuestions.length) {
                questionCounterElement.innerText = `${currentQuestionIndex + 1} / ${orderedQuestions.length}`;
                showQuestion(orderedQuestions[currentQuestionIndex]);
                saveProgress();
                if (document.activeElement && typeof document.activeElement.blur === 'function') {
                    document.activeElement.blur();
                }
            } else {
                showResults();
            }
            updateSkipButtonStates(); // Panggil di sini untuk memastikan state tombol selalu update
        }

        function showQuestion(questionData) {
            questionElement.innerText = questionData.question;
            answerButtonsElement.innerHTML = '';
            const shuffledAnswers = [...questionData.answers].sort(() => Math.random() - 0.5);
            shuffledAnswers.forEach(answer => {
                const button = document.createElement('button');
                button.innerText = answer.text;
                button.classList.add('btn');
                if (answer.correct) {
                    button.dataset.correct = answer.correct;
                }
                button.addEventListener('click', selectAnswer);
                answerButtonsElement.appendChild(button);
            });
        }

        function resetState() {
            clearTimeout(questionTimeout);
            while (answerButtonsElement.firstChild) {
                answerButtonsElement.removeChild(answerButtonsElement.firstChild);
            }
        }

        function selectAnswer(e) {
            const selectedButton = e.target;
            const correct = selectedButton.dataset.correct === 'true';
            if (correct) { score++; }
            Array.from(answerButtonsElement.children).forEach(button => {
                setStatusClass(button, button.dataset.correct === 'true');
                button.disabled = true;
            });
            saveProgress();
            questionTimeout = setTimeout(() => {
                if (orderedQuestions && currentQuestionIndex < orderedQuestions.length -1) {
                    currentQuestionIndex++;
                    setNextQuestion();
                } else if (orderedQuestions && currentQuestionIndex === orderedQuestions.length -1) {
                    showResults();
                }
            }, 7000);
        }

        function setStatusClass(element, correct) {
            clearStatusClass(element);
            if (correct) { element.classList.add('correct'); }
            else { element.classList.add('wrong'); }
        }

        function clearStatusClass(element) {
            element.classList.remove('correct');
            element.classList.remove('wrong');
        }

        function showResults() {
            clearTimeout(questionTimeout);
            questionContainerElement.classList.add('hide');
            questionCounterElement.classList.add('hide');
            skipNavigationControls.classList.add('hide');
            clearProgress();
            completionMessageElement.innerText = "Selamat Kuis Sudah Selesai 🎉";
            completionMessageElement.style.color = "#28a745";
            completionMessageElement.classList.remove('hide');
            startButton.innerText = 'Ulangi Kuis';
            initialControls.classList.remove('hide');
            continueButton.classList.add('hide');
        }
    </script>
</body>
</html>
