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


    { "en": "Apa Itu 'WSN' (Wireless Sensor Network)?", "id": "Jaringan Sensor Nirkabel Terdistribusi Luas." },
    { "en": "Apa Itu 'Smart Sensor' (Sensor Cerdas)?", "id": "Sensor Dengan Pemrosesan Data Internal." },
    { "en": "Apa Itu 'Actuator' (Aktuator Listrik)?", "id": "Perangkat Pengubah Listrik Menjadi Gerak." },
    { "en": "Apa Itu 'Gateway' (Gerbang IoT)?", "id": "Penghubung Perangkat Lokal Ke Awan." },
    { "en": "Apa Itu 'MQTT' (Message Queuing Telemetry Transport)?", "id": "Protokol Pesan Ringan Untuk IoT." },
    { "en": "Apa Itu 'CoAP' (Constrained Application Protocol)?", "id": "Protokol Web Untuk Perangkat Terbatas." },
    { "en": "Apa Itu 'LoRa' (Long Range Radio)?", "id": "Teknologi Nirkabel Jarak Jauh Efisien." },
    { "en": "Apa Itu 'LoRaWAN' (Long Range Wide Area Network)?", "id": "Protokol Jaringan Radio Jarak Jauh." },
    { "en": "Apa Itu 'NB-IoT' (Narrowband Internet Of Things)?", "id": "Standar Seluler Untuk Perangkat IoT." },
    { "en": "Apa Itu 'Sigfox' (Teknologi Sigfox)?", "id": "Jaringan Nirkabel Global Daya Rendah." },
    { "en": "Apa Itu 'Zigbee' (Protokol Zigbee)?", "id": "Standar Jaringan Nirkabel Jarak Pendek." },
    { "en": "Apa Itu 'Z-Wave' (Protokol Z-Wave)?", "id": "Komunikasi Nirkabel Khusus Rumah Pintar." },
    { "en": "Apa Itu 'BLE' (Bluetooth Low Energy)?", "id": "Versi Bluetooth Dengan Konsumsi Hemat." },
    { "en": "Apa Itu 'Matter' (Standar Konektivitas Matter)?", "id": "Standar Universal Perangkat Rumah Pintar." },
    { "en": "Apa Itu 'Thread' (Protokol Thread)?", "id": "Jaringan Mesh Nirkabel Berbasis IP." },
    { "en": "Apa Itu 'NFC' (Near Field Communication)?", "id": "Komunikasi Nirkabel Jarak Sangat Dekat." },
    { "en": "Apa Itu 'RFID' (Radio Frequency Identification)?", "id": "Identifikasi Objek Melalui Gelombang Radio." },
    { "en": "Apa Itu 'ESP32' (Mikrokontroler ESP32)?", "id": "Chip Wi-Fi Dan Bluetooth Murah." },
    { "en": "Apa Itu 'Arduino' (Platform Arduino)?", "id": "Papan Pengembangan Elektronika Sumber Terbuka." },
    { "en": "Apa Itu 'Raspberry Pi' (Komputer Raspberry Pi)?", "id": "Komputer Kecil Ukuran Kartu Kredit." },
    { "en": "Apa Itu 'Edge Computing' (Komputasi Tepi)?", "id": "Pemrosesan Data Dekat Lokasi Sensor." },
    { "en": "Apa Itu 'Fog Computing' (Komputasi Kabut)?", "id": "Perluasan Layanan Awan Ke Jaringan." },
    { "en": "Apa Itu 'Digital Twin' (Kembaran Digital)?", "id": "Model Virtual Dari Aset Fisik." },
    { "en": "Apa Itu 'Smart City' (Kota Cerdas)?", "id": "Integrasi Teknologi Pengelola Infrastruktur Kota." },
    { "en": "Apa Itu 'Smart Home' (Rumah Pintar)?", "id": "Otomasi Perangkat Rumah Secara Terintegrasi." },
    { "en": "Apa Itu 'Smart Agriculture' (Pertanian Cerdas)?", "id": "Optimasi Pertanian Menggunakan Sensor IoT." },
    { "en": "Apa Itu 'Smart Grid' (Jaringan Listrik Cerdas)?", "id": "Sistem Listrik Dengan Aliran Digital." },
    { "en": "Apa Itu 'Industry 4.0' (Industri Empat Titik Nol)?", "id": "Transformasi Digital Sektor Manufaktur Modern." },
    { "en": "Apa Itu 'IIoT' (Industrial Internet Of Things)?", "id": "Penerapan IoT Dalam Operasi Industri." },
    { "en": "Apa Itu 'Telematics' (Telematika)?", "id": "Sistem Informasi Dan Komunikasi Kendaraan." },
    { "en": "Apa Itu 'Wearable Device' (Perangkat Pakai)?", "id": "Teknologi Elektronik Yang Dipakai Tubuh." },
    { "en": "Apa Itu 'Accelerometer' (Sensor Akselerometer)?", "id": "Sensor Pengukur Percepatan Dan Gerak." },
    { "en": "Apa Itu 'Gyroscope' (Sensor Giroskop)?", "id": "Sensor Pengukur Kecepatan Sudut Rotasi." },
    { "en": "Apa Itu 'Magnetometer' (Sensor Magnetometer)?", "id": "Sensor Pengukur Kekuatan Medan Magnet." },
    { "en": "Apa Itu 'Barometer' (Sensor Tekanan Udara)?", "id": "Sensor Pengukur Ketinggian Dan Cuaca." },
    { "en": "Apa Itu 'Hygrometer' (Sensor Kelembapan)?", "id": "Sensor Pengukur Kadar Air Udara." },
    { "en": "Apa Itu 'Thermistor' (Sensor Termistor)?", "id": "Resistor Sensitif Terhadap Perubahan Suhu." },
    { "en": "Apa Itu 'Ultrasonic Sensor' (Sensor Ultrasonik)?", "id": "Sensor Jarak Menggunakan Gelombang Suara." },
    { "en": "Apa Itu 'PIR' (Passive Infrared Sensor)?", "id": "Sensor Pendeteksi Gerakan Manusia Sesaat." },
    { "en": "Apa Itu 'ToF' (Time Of Flight Sensor)?", "id": "Sensor Jarak Presisi Berdasarkan Cahaya." },
    { "en": "Apa Itu 'Mesh Network' (Jaringan Mesh)?", "id": "Topologi Jaringan Setiap Simpul Terhubung." },
    { "en": "Apa Itu 'Star Topology' (Topologi Bintang)?", "id": "Jaringan Terpusat Pada Satu Hub." },
    { "en": "Apa Itu 'LPWAN' (Low Power Wide Area Network)?", "id": "Jaringan Luas Dengan Konsumsi Hemat." },
    { "en": "Apa Itu 'M2M' (Machine To Machine)?", "id": "Komunikasi Otomatis Antar Perangkat Mesin." },
    { "en": "Apa Itu 'Payload' (Beban Data IoT)?", "id": "Informasi Utama Yang Dikirim Sensor." },
    { "en": "Apa Itu 'Latency' (Latensi Data)?", "id": "Waktu Tunda Pengiriman Sinyal Digital." },
    { "en": "Apa Itu 'Throughput' (Throughput Jaringan)?", "id": "Kapasitas Transfer Data Sebenarnya, Nyata." },
    { "en": "Apa Itu 'Duty Cycle' (Siklus Kerja)?", "id": "Persentase Waktu Aktif Perangkat Radio." },
    { "en": "Apa Itu 'Firmware Over The Air' (Pembaruan Nirkabel)?", "id": "Pembaruan Perangkat Lunak Melalui Jaringan." },
    { "en": "Apa Itu 'Provisioning' (Penyediaan Perangkat)?", "id": "Proses Menghubungkan Perangkat Ke Jaringan." },
    { "en": "Apa Itu 'Onboarding' (Pengenalan Perangkat)?", "id": "Registrasi Perangkat Ke Platform Awan." },
    { "en": "Apa Itu 'Device Management' (Manajemen Perangkat)?", "id": "Pemantauan Dan Pengaturan Perangkat IoT." },
    { "en": "Apa Itu 'Cloud Platform' (Platform Awan)?", "id": "Infrastruktur Pusat Pengolah Data IoT." },
    { "en": "Apa Itu 'API' (Application Programming Interface)?", "id": "Antarmuka Penghubung Antar Layanan Digital." },
    { "en": "Apa Itu 'RESTful' (Arsitektur REST)?", "id": "Standar Komunikasi Data Berbasis HTTP." },
    { "en": "Apa Itu 'Websocket' (Protokol Websocket)?", "id": "Saluran Komunikasi Dua Arah Interaktif." },
    { "en": "Apa Itu 'Payload Compression' (Kompresi Data)?", "id": "Pengecilan Ukuran Data Sebelum Dikirim." },
    { "en": "Apa Itu 'Data Visualization' (Visualisasi Data)?", "id": "Representasi Grafis Informasi Hasil Sensor." },
    { "en": "Apa Itu 'Dashboard' (Dasbor IoT)?", "id": "Panel Kendali Dan Pemantauan Visual." },
    { "en": "Apa Itu 'Alerting' (Sistem Peringatan)?", "id": "Notifikasi Otomatis Saat Kondisi Kritis." },
    { "en": "Apa Itu 'Predictive Maintenance' (Pemeliharaan Prediktif)?", "id": "Perbaikan Sebelum Terjadi Kerusakan Mesin." },
    { "en": "Apa Itu 'Asset Tracking' (Pelacakan Aset)?", "id": "Pemantauan Lokasi Barang Secara Langsung." },
    { "en": "Apa Itu 'Geofencing' (Pagar Geografis)?", "id": "Batas Virtual Lokasi Berbasis Koordinat." },
    { "en": "Apa Itu 'Encryption' (Enkripsi Data)?", "id": "Pengamanan Informasi Melalui Kode Rahasia." },
    { "en": "Apa Itu 'TLS' (Transport Layer Security)?", "id": "Protokol Keamanan Jalur Komunikasi Internet." },
    { "en": "Apa Itu 'AES' (Advanced Encryption Standard)?", "id": "Algoritma Enkripsi Data Sangat Aman." },
    { "en": "Apa Itu 'Authentication' (Autentikasi Perangkat)?", "id": "Proses Verifikasi Identitas Perangkat Asli." },
    { "en": "Apa Itu 'Authorization' (Otorisasi Akses)?", "id": "Pemberian Hak Akses Kepada Pengguna." },
    { "en": "Apa Itu 'Cybersecurity' (Keamanan Siber)?", "id": "Perlindungan Sistem Dari Serangan Digital." },
    { "en": "Apa Itu 'Privacy' (Privasi Data)?", "id": "Perlindungan Informasi Pribadi Pengguna IoT." },
    { "en": "Apa Itu 'Blockchain' (Rantai Blok)?", "id": "Database Terdistribusi Aman Dan Transparan." },
    { "en": "Apa Itu 'OTA' (Over The Air)?", "id": "Metode Pengiriman Data Melalui Nirkabel." },
    { "en": "Apa Itu 'Li-Fi' (Light Fidelity)?", "id": "Transmisi Data Menggunakan Media Cahaya." },
    { "en": "Apa Itu 'Beacon' (Teknologi Beacon)?", "id": "Pemancar Sinyal Lokasi Jarak Pendek." },
    { "en": "Apa Itu 'Home Automation' (Otomasi Rumah)?", "id": "Kendali Otomatis Fasilitas Dalam Rumah." },
    { "en": "Apa Itu 'Smart Lighting' (Pencahayaan Cerdas)?", "id": "Sistem Lampu Terkendali Secara Otomatis." },
    { "en": "Apa Itu 'Smart Thermostat' (Termostat Cerdas)?", "id": "Pengatur Suhu Ruangan Secara Otomatis." },
    { "en": "Apa Itu 'Smart Lock' (Kunci Cerdas)?", "id": "Sistem Kunci Pintu Tanpa Kunci." },
    { "en": "Apa Itu 'Voice Assistant' (Asisten Suara)?", "id": "Kendali Perangkat Melalui Perintah Suara." },
    { "en": "Apa Itu 'Interoperability' (Interoperabilitas)?", "id": "Kemampuan Perangkat Berbeda Saling Berinteraksi." },
    { "en": "Apa Itu 'Scalability' (Skalabilitas)?", "id": "Kemampuan Sistem Menangani Banyak Perangkat." },
    { "en": "Apa Itu 'Reliability' (Keandalan)?", "id": "Kemampuan Sistem Beroperasi Tanpa Gagal." },
    { "en": "Apa Itu 'Energy Harvesting' (Pemanenan Energi)?", "id": "Pengumpulan Daya Dari Sumber Sekitar." },
    { "en": "Apa Itu 'Power Management' (Manajemen Daya)?", "id": "Optimasi Konsumsi Energi Perangkat Elektronik." },
    { "en": "Apa Itu 'Sleep Mode' (Mode Tidur)?", "id": "Status Hemat Energi Saat Tidak." },
    { "en": "Apa Itu 'Wake-Up Call' (Panggilan Bangun)?", "id": "Sinyal Pemicu Perangkat Keluar Tidur." },
    { "en": "Apa Itu 'ADC' (Analog To Digital Converter)?", "id": "Pengubah Sinyal Sensor Menjadi Digital." },
    { "en": "Apa Itu 'DAC' (Digital To Analog Converter)?", "id": "Pengubah Data Digital Menjadi Fisik." },
    { "en": "Apa Itu 'GPIO' (General Purpose Input Output)?", "id": "Pin Digital Untuk Berbagai Fungsi." },
    { "en": "Apa Itu 'PWM' (Pulse Width Modulation)?", "id": "Kendali Kecepatan Motor Atau Kecerahan." },
    { "en": "Apa Itu 'I2C' (Inter-Integrated Circuit)?", "id": "Protokol Komunikasi Antar Chip Sederhana." },
    { "en": "Apa Itu 'SPI' (Serial Peripheral Interface)?", "id": "Komunikasi Data Serial Kecepatan Tinggi." },
    { "en": "Apa Itu 'UART' (Universal Asynchronous Receiver Transmitter)?", "id": "Protokol Komunikasi Serial Paling Dasar." },
    { "en": "Apa Itu 'SoC' (System On Chip)?", "id": "Seluruh Sistem Elektronik Dalam Chip." },
    { "en": "Apa Itu 'SiP' (System In Package)?", "id": "Kumpulan Chip Dalam Satu Kemasan." },
    { "en": "Apa Itu 'MEMS' (Micro-Electro-Mechanical Systems)?", "id": "Sistem Mekanik Elektronik Ukuran Mikroskopis." },
    { "en": "Apa Itu 'Actuators' (Aktuator)?", "id": "Komponen Eksekutor Perintah Fisik Robot." },
    { "en": "Apa Itu 'Feedback Loop' (Loop Umpan Balik)?", "id": "Proses Perbaikan Berdasarkan Hasil Sensor." },
    { "en": "Apa Itu 'Closed Loop' (Sistem Loop Tertutup)?", "id": "Sistem Kendali Dengan Umpan Balik." },
    { "en": "Apa Itu 'Uptime' (Waktu Aktif)?", "id": "Durasi Sistem Beroperasi Tanpa Gangguan." },
    { "en": "Apa Itu '6G' (Sixth Generation)?", "id": "Teknologi Komunikasi Seluler Masa Depan." },
    { "en": "Apa Itu 'THz' (Terahertz Communication)?", "id": "Frekuensi Sangat Tinggi Transmisi Data." },
    { "en": "Apa Itu 'Quantum Computing' (Komputasi Kuantum)?", "id": "Sistem Komputer Berbasis Mekanika Kuantum." },
    { "en": "Apa Itu 'Qubit' (Quantum Bit)?", "id": "Unit Informasi Dasar Komputasi Kuantum." },
    { "en": "Apa Itu 'Superposition' (Superposisi)?", "id": "Kondisi Partikel Dalam Banyak Status." },
    { "en": "Apa Itu 'Entanglement' (Keterikatan Kuantum)?", "id": "Hubungan Instan Antar Dua Partikel." },
    { "en": "Apa Itu 'Quantum Supremacy' (Supremasi Kuantum)?", "id": "Kemampuan Kuantum Melebihi Komputer Klasik." },
    { "en": "Apa Itu 'PQC' (Post-Quantum Cryptography)?", "id": "Keamanan Data Dari Serangan Kuantum." },
    { "en": "Apa Itu 'Generative AI' (Kecerdasan Buatan Generatif)?", "id": "Sistem Kecerdasan Penghasil Konten Baru." },
    { "en": "Apa Itu 'LLM' (Large Language Model)?", "id": "Model Kecerdasan Buatan Pemroses Bahasa." },
    { "en": "Apa Itu 'NLP' (Natural Language Processing)?", "id": "Pemrosesan Bahasa Manusia Oleh Mesin." },
    { "en": "Apa Itu 'Explainable AI' (Kecerdasan Buatan Terjelaskan)?", "id": "Sistem Kecerdasan Yang Dapat Dipahami." },
    { "en": "Apa Itu 'AI Bias' (Bias Kecerdasan Buatan)?", "id": "Ketidakadilan Hasil Algoritma Terhadap Kelompok." },
    { "en": "Apa Itu 'AGI' (Artificial General Intelligence)?", "id": "Kecerdasan Buatan Setingkat Kemampuan Manusia." },
    { "en": "Apa Itu 'Machine Ethics' (Etika Mesin)?", "id": "Prinsip Moral Dalam Perilaku Robot." },
    { "en": "Apa Itu 'Turing Test' (Uji Turing)?", "id": "Uji Kemampuan Mesin Meniru Manusia." },
    { "en": "Apa Itu 'Singularity' (Singularitas Teknologi)?", "id": "Titik Pertumbuhan Teknologi Tak Terkendali." },
    { "en": "Apa Itu 'Web3' (Web Versi Tiga)?", "id": "Generasi Internet Terdesentralisasi Berbasis Blockchain." },
    { "en": "Apa Itu 'DAO' (Decentralized Autonomous Organization)?", "id": "Organisasi Digital Berjalan Melalui Kode." },
    { "en": "Apa Itu 'DeFi' (Decentralized Finance)?", "id": "Layanan Keuangan Tanpa Perantara Bank." },
    { "en": "Apa Itu 'Metaverse' (Metaverse)?", "id": "Dunia Virtual Kolektif Sangat Imersif." },
    { "en": "Apa Itu 'NFT' (Non-Fungible Token)?", "id": "Aset Digital Unik Terverifikasi Blockchain." },
    { "en": "Apa Itu 'Digital Twin' (Kembaran Digital)?", "id": "Representasi Virtual Dari Objek Fisik." },
    { "en": "Apa Itu 'Smart Contract' (Kontrak Pintar)?", "id": "Perjanjian Otomatis Berbasis Kode Blockchain." },
    { "en": "Apa Itu 'BCI' (Brain-Computer Interface)?", "id": "Antarmuka Penghubung Otak Dan Komputer." },
    { "en": "Apa Itu 'Neuralink' (Teknologi Neuralink)?", "id": "Perangkat Implan Otak Manusia Digital." },
    { "en": "Apa Itu 'Augmented Reality' (Realitas Tertambah)?", "id": "Integrasi Informasi Digital Dunia Nyata." },
    { "en": "Apa Itu 'Virtual Reality' (Realitas Virtual)?", "id": "Lingkungan Digital Buatan Total Imersif." },
    { "en": "Apa Itu 'Mixed Reality' (Realitas Campuran)?", "id": "Gabungan Unsur Nyata Dan Virtual." },
    { "en": "Apa Itu 'Edge AI' (Kecerdasan Buatan Tepi)?", "id": "Pemrosesan Kecerdasan Pada Perangkat Lokal." },
    { "en": "Apa Itu 'Computing Continuum' (Kontinum Komputasi)?", "id": "Integrasi Data Dari Tepi Hingga." },
    { "en": "Apa Itu 'Holography' (Holografi)?", "id": "Teknik Pembuatan Gambar Tiga Dimensi." },
    { "en": "Apa Itu 'Nanotechnology' (Nanoteknologi)?", "id": "Manipulasi Materi Pada Skala Atom." },
    { "en": "Apa Itu 'Biotechnology' (Bioteknologi)?", "id": "Pemanfaatan Organisme Untuk Produk Teknologi." },
    { "en": "Apa Itu 'Crispr' (Clustered Regularly Interspaced Short Palindromic Repeats)?", "id": "Teknologi Pengeditan Genetik Sangat Presisi." },
    { "en": "Apa Itu 'Synthetic Biology' (Biologi Sintetis)?", "id": "Desain Dan Konstruksi Organisme Baru." },
    { "en": "Apa Itu 'Green Tech' (Teknologi Hijau)?", "id": "Teknologi Ramah Lingkungan Berkelanjutan Tinggi." },
    { "en": "Apa Itu 'Net Zero' (Emisi Nol)?", "id": "Keseimbangan Emisi Gas Rumah Kaca." },
    { "en": "Apa Itu 'Carbon Capture' (Penangkapan Karbon)?", "id": "Teknologi Pengambilan Karbon Di Atmosfer." },
    { "en": "Apa Itu 'Circular Economy' (Ekonomi Sirkular)?", "id": "Sistem Produksi Minimal Sampah, Berkelanjutan." },
    { "en": "Apa Itu 'Agrivoltaics' (Agrivoltaik)?", "id": "Gabungan Pertanian Dan Panel Surya." },
    { "en": "Apa Itu 'Solid State Battery' (Baterai Solid)?", "id": "Teknologi Baterai Dengan Elektrolit Padat." },
    { "en": "Apa Itu 'Hydrogen Fuel Cell' (Sel Bahan Bakar)?", "id": "Pembangkit Listrik Dari Reaksi Hidrogen." },
    { "en": "Apa Itu 'Fusion Energy' (Energi Fusi)?", "id": "Energi Bersih Dari Reaksi Matahari." },
    { "en": "Apa Itu 'Space Mining' (Penambangan Ruang Angkasa)?", "id": "Pengambilan Sumber Daya Dari Asteroit." },
    { "en": "Apa Itu 'Starlink' (Satelit Starlink)?", "id": "Jaringan Internet Global Melalui Satelit." },
    { "en": "Apa Itu 'SpaceX' (Eksplorasi Ruang Angkasa)?", "id": "Perusahaan Transportasi Luar Angkasa Swasta." },
    { "en": "Apa Itu 'CubeSat' (Satelit Kubus)?", "id": "Satelit Mikro Berukuran Sangat Kecil." },
    { "en": "Apa Itu 'Autonomous Vehicle' (Kendaraan Otonom)?", "id": "Mobil Yang Dapat Berjalan Mandiri." },
    { "en": "Apa Itu 'Flying Taxi' (Taksi Terbang)?", "id": "Kendaraan Listrik Transportasi Udara Perkotaan." },
    { "en": "Apa Itu 'Hyperloop' (Sistem Hyperloop)?", "id": "Transportasi Tabung Vakum Kecepatan Tinggi." },
    { "en": "Apa Itu 'Digital Sovereignty' (Kedaulatan Digital)?", "id": "Kendali Negara Atas Data Digital." },
    { "en": "Apa Itu 'Data Privacy' (Privasi Data)?", "id": "Hak Individu Melindungi Informasi Pribadi." },
    { "en": "Apa Itu 'GDPR' (General Data Protection Regulation)?", "id": "Standar Perlindungan Data Pribadi Global." },
    { "en": "Apa Itu 'Right To Be Forgotten' (Hak Dilupakan)?", "id": "Hak Menghapus Informasi Pribadi Internet." },
    { "en": "Apa Itu 'Algorithm Transparency' (Transparansi Algoritma)?", "id": "Keterbukaan Cara Kerja Sistem Digital." },
    { "en": "Apa Itu 'Deepfake' (Deepfake)?", "id": "Manipulasi Video Menggunakan Kecerdasan Buatan." },
    { "en": "Apa Itu 'Fake News' (Berita Palsu)?", "id": "Informasi Bohong Yang Disebarkan Digital." },
    { "en": "Apa Itu 'Social Credit System' (Sistem Kredit Sosial)?", "id": "Penilaian Perilaku Warga Berbasis Teknologi." },
    { "en": "Apa Itu 'Surveillance Capitalism' (Kapitalisme Pengawasan)?", "id": "Eksploitasi Data Perilaku Untuk Keuntungan." },
    { "en": "Apa Itu 'Dark Patterns' (Pola Gelap)?", "id": "Desain Antarmuka Penipu Pengguna Digital." },
    { "en": "Apa Itu 'Filter Bubble' (Gelembung Filter)?", "id": "Isolasi Informasi Akibat Algoritma Personalisasi." },
    { "en": "Apa Itu 'Echo Chamber' (Ruang Gema)?", "id": "Penguatan Keyakinan Akibat Informasi Homogen." },
    { "en": "Apa Itu 'Cyber Bullying' (Perundungan Siber)?", "id": "Tindakan Kekerasan Melalui Media Digital." },
    { "en": "Apa Itu 'Digital Detox' (Detoks Digital)?", "id": "Pengurangan Penggunaan Perangkat Elektronik Sesaat." },
    { "en": "Apa Itu 'Nomophobia' (Nomofobia)?", "id": "Ketakutan Berlebih Tanpa Ponsel Pintar." },
    { "en": "Apa Itu 'Internet Addiction' (Kecanduan Internet)?", "id": "Penggunaan Internet Yang Tidak Terkendali." },
    { "en": "Apa Itu 'Digital Divide' (Kesenjangan Digital)?", "id": "Ketimpangan Akses Terhadap Teknologi Informasi." },
    { "en": "Apa Itu 'E-Waste' (Limbah Elektronik)?", "id": "Sampah Berasal Dari Perangkat Elektronika." },
    { "en": "Apa Itu 'Planned Obsolescence' (Keusangan Terencana)?", "id": "Desain Produk Dengan Masa Pakai." },
    { "en": "Apa Itu 'Repairability' (Kemampuan Perbaikan)?", "id": "Tingkat Kemudahan Memperbaiki Perangkat Elektronika." },
    { "en": "Apa Itu 'Open Access' (Akses Terbuka)?", "id": "Kebebasan Mengakses Informasi Ilmiah Digital." },
    { "en": "Apa Itu 'Creative Commons' (Lisensi Creative Commons)?", "id": "Standar Lisensi Hak Cipta Digital." },
    { "en": "Apa Itu 'Copyleft' (Konsep Copyleft)?", "id": "Kebebasan Menyebarkan Karya Dengan Syarat." },
    { "en": "Apa Itu 'Patent Troll' (Troll Paten)?", "id": "Penyalahgunaan Hak Paten Untuk Keuntungan." },
    { "en": "Apa Itu 'Cryptojacking' (Pembajakan Kripto)?", "id": "Pencurian Daya Komputasi Untuk Menambang." },
    { "en": "Apa Itu 'RPA' (Robotic Process Automation)?", "id": "Otomasi Tugas Rutin Melalui Software." },
    { "en": "Apa Itu 'Cobot' (Collaborative Robot)?", "id": "Robot Bekerja Berdampingan Dengan Manusia." },
    { "en": "Apa Itu 'Humanoid' (Robot Humanoid)?", "id": "Robot Berbentuk Menyerupai Tubuh Manusia." },
    { "en": "Apa Itu 'Swarm Robotics' (Robotika Kawanan)?", "id": "Sekelompok Robot Bekerja Secara Terkoordinasi." },
    { "en": "Apa Itu 'Exoskeleton' (Eksoskeleton)?", "id": "Rangka Luar Pembantu Kekuatan Manusia." },
    { "en": "Apa Itu '3D Printing' (Pencetakan Tiga Dimensi)?", "id": "Pembuatan Objek Fisik Dari Model." },
    { "en": "Apa Itu 'Additive Manufacturing' (Manufaktur Aditif)?", "id": "Produksi Barang Dengan Menambah Lapisan." },
    { "en": "Apa Itu 'Smart Materials' (Material Cerdas)?", "id": "Bahan Yang Berubah Sesuai Lingkungan." },
    { "en": "Apa Itu 'Graphene' (Grafena)?", "id": "Material Karbon Tipis Sangat Kuat." },
    { "en": "Apa Itu 'Photonics' (Fotonika)?", "id": "Teknologi Pengontrol Partikel Cahaya Foton." },
    { "en": "Apa Itu 'Quantum Sensing' (Penginderaan Kuantum)?", "id": "Pengukuran Sangat Akurat Berbasis Kuantum." },
    { "en": "Apa Itu 'Terahertz Imaging' (Pencitraan Terahertz)?", "id": "Pemindaian Objek Menggunakan Gelombang THz." },
    { "en": "Apa Itu 'LiFi' (Light Fidelity)?", "id": "Transmisi Data Menggunakan Cahaya Lampu." },
    { "en": "Apa Itu 'Invisible Web' (Web Tak Terlihat)?", "id": "Data Web Tidak Terindeks Umum." },
    { "en": "Apa Itu 'Zero Knowledge Proof' (Bukti Tanpa Pengetahuan)?", "id": "Verifikasi Rahasia Tanpa Mengungkap Data." },
    { "en": "Apa Itu 'Homomorphic Encryption' (Enkripsi Homomorfik)?", "id": "Pengolahan Data Dalam Kondisi Terenkripsi." },
    { "en": "Apa Itu 'Federated Learning' (Pembelajaran Terfederasi)?", "id": "Pelatihan Model AI Tanpa Bagi." },
    { "en": "Apa Itu 'Synthetic Data' (Data Sintetis)?", "id": "Data Buatan Untuk Melatih Algoritma." },
    { "en": "Apa Itu 'Digital Inclusion' (Inklusi Digital)?", "id": "Pemerataan Akses Teknologi Bagi Semua." },
    { "en": "Apa Itu 'Technological Unemployment' (Pengangguran Teknologi)?", "id": "Hilangnya Pekerjaan Akibat Otomasi Mesin." },
    { "en": "Apa Itu 'Universal Basic Income' (Pendapatan Dasar Universal)?", "id": "Jaminan Pendapatan Akibat Dampak Otomasi." },
    { "en": "Apa Itu 'Post-Humanism' (Pasca-Humanisme)?", "id": "Evolusi Manusia Melalui Integrasi Teknologi." },
    { "en": "Apa Itu 'Transhumanism' (Transhumanisme)?", "id": "Peningkatan Kapasitas Manusia Melalui Teknologi." },
    { "en": "Apa Itu 'APK' (Android Package Kit)?", "id": "Format Berkas Instalasi Aplikasi Android." },
    { "en": "Apa Itu 'AAB' (Android App Bundle)?", "id": "Format Publikasi Aplikasi Android Modern." },
    { "en": "Apa Itu 'SDK' (Software Development Kit)?", "id": "Kumpulan Alat Pengembangan Perangkat Lunak." },
    { "en": "Apa Itu 'NDK' (Native Development Kit)?", "id": "Alat Pengembangan Android Bahasa C." },
    { "en": "Apa Itu 'JVM' (Java Virtual Machine)?", "id": "Mesin Virtual Eksekusi Kode Java." },
    { "en": "Apa Itu 'ART' (Android Runtime)?", "id": "Lingkungan Eksekusi Aplikasi Android Modern." },
    { "en": "Apa Itu 'DEX' (Dalvik Executable)?", "id": "Format Berkas Kode Kompilasi Android." },
    { "en": "Apa Itu 'ADB' (Android Debug Bridge)?", "id": "Alat Komunikasi Perangkat Dan Komputer." },
    { "en": "Apa Itu 'Activity' (Aktivitas Android)?", "id": "Satu Layar Antarmuka Pengguna Aplikasi." },
    { "en": "Apa Itu 'Fragment' (Fragmen Android)?", "id": "Bagian Antarmuka Dalam Sebuah Aktivitas." },
    { "en": "Apa Itu 'Intent' (Intensi Android)?", "id": "Objek Pesan Antar Komponen Aplikasi." },
    { "en": "Apa Itu 'Manifest' (Android Manifest)?", "id": "Berkas Informasi Utama Konfigurasi Aplikasi." },
    { "en": "Apa Itu 'Gradle' (Sistem Bangun Gradle)?", "id": "Alat Otomasi Pembangunan Proyek Aplikasi." },
    { "en": "Apa Itu 'Jetpack Compose' (Jetpack Compose)?", "id": "Perangkat Membangun Antarmuka Android Modern." },
    { "en": "Apa Itu 'ANR' (Application Not Responding)?", "id": "Kondisi Aplikasi Berhenti Merespon Pengguna." },
    { "en": "Apa Itu 'View' (Tampilan Android)?", "id": "Komponen Dasar Antarmuka Pengguna Grafis." },
    { "en": "Apa Itu 'Layout' (Tata Letak)?", "id": "Struktur Penempatan Elemen Visual Aplikasi." },
    { "en": "Apa Itu 'Widget' (Widget Aplikasi)?", "id": "Elemen Kontrol Kecil Antarmuka Pengguna." },
    { "en": "Apa Itu 'Recycler View' (Tampilan Pendaur-Ulang)?", "id": "Elemen Penampil Daftar Data Efisien." },
    { "en": "Apa Itu 'Shared Preferences' (Preferensi Bersama)?", "id": "Penyimpanan Data Sederhana Kunci Nilai." },
    { "en": "Apa Itu 'Room Database' (Basis Data Room)?", "id": "Abstraksi Penyimpanan Data Lokal SQLite." },
    { "en": "Apa Itu 'API Level' (Tingkat API)?", "id": "Nomor Versi Kerangka Kerja Android." },
    { "en": "Apa Itu 'Flutter' (Kerangka Kerja Flutter)?", "id": "Alat Pembuat Aplikasi Multi-Platform Google." },
    { "en": "Apa Itu 'Dart' (Bahasa Pemrograman Dart)?", "id": "Bahasa Utama Pengembangan Aplikasi Flutter." },
    { "en": "Apa Itu 'Widget' (Widget Flutter)?", "id": "Blok Dasar Pembangun Antarmuka Flutter." },
    { "en": "Apa Itu 'State Management' (Manajemen Status)?", "id": "Pengaturan Aliran Data Dalam Aplikasi." },
    { "en": "Apa Itu 'Hot Reload' (Muat Ulang Cepat)?", "id": "Penerapan Perubahan Kode Secara Instan." },
    { "en": "Apa Itu 'React Native' (React Native)?", "id": "Kerangka Kerja Mobile Berbasis JavaScript." },
    { "en": "Apa Itu 'Expo' (Ekosistem Expo)?", "id": "Alat Percepat Pengembangan React Native." },
    { "en": "Apa Itu 'PWA' (Progressive Web App)?", "id": "Aplikasi Web Dengan Pengalaman Mobile." },
    { "en": "Apa Itu 'Unity' (Mesin Game Unity)?", "id": "Platform Pengembangan Game Multi-Platform Populer." },
    { "en": "Apa Itu 'Unreal Engine' (Mesin Unreal)?", "id": "Platform Pengembangan Game Grafis Tinggi." },
    { "en": "Apa Itu 'Asset' (Aset Game)?", "id": "Komponen Visual Maupun Suara Game." },
    { "en": "Apa Itu 'Sprite' (Gambar Sprite)?", "id": "Gambar Dua Dimensi Dalam Game." },
    { "en": "Apa Itu 'Prefab' (Prefabrikasi Unity)?", "id": "Template Objek Game Siap Pakai." },
    { "en": "Apa Itu 'Rigid Body' (Badan Kaku)?", "id": "Komponen Simulasi Fisika Pada Objek." },
    { "en": "Apa Itu 'Collider' (Penabrak Fisika)?", "id": "Batas Deteksi Tabrakan Antar Objek." },
    { "en": "Apa Itu 'Raycasting' (Pancaran Sinar)?", "id": "Teknik Deteksi Objek Melalui Garis." },
    { "en": "Apa Itu 'Game Loop' (Siklus Game)?", "id": "Proses Berulang Pembaruan Logika Game." },
    { "en": "Apa Itu 'FPS' (Frames Per Second)?", "id": "Jumlah Bingkai Gambar Per Detik." },
    { "en": "Apa Itu 'Shaders' (Program Shader)?", "id": "Kode Pemroses Efek Visual Grafis." },
    { "en": "Apa Itu 'Mesh' (Jaring Poligon)?", "id": "Kumpulan Titik Pembentuk Objek 3D." },
    { "en": "Apa Itu 'Texture' (Tekstur Gambar)?", "id": "Lapisan Gambar Pada Permukaan Objek." },
    { "en": "Apa Itu 'Material' (Material Objek)?", "id": "Pengaturan Sifat Cahaya Permukaan Objek." },
    { "en": "Apa Itu 'NavMesh' (Jaring Navigasi)?", "id": "Area Tempat Karakter Bisa Berjalan." },
    { "en": "Apa Itu 'AI' (Artificial Intelligence) Game?", "id": "Logika Kendali Karakter Bukan Pemain." },
    { "en": "Apa Itu 'NPC' (Non-Player Character)?", "id": "Karakter Yang Tidak Dikendalikan Pemain." },
    { "en": "Apa Itu 'GUI' (Graphical User Interface)?", "id": "Elemen Visual Interaksi Antarmuka Pengguna." },
    { "en": "Apa Itu 'HUD' (Heads-Up Display)?", "id": "Informasi Status Game Di Layar." },
    { "en": "Apa Itu 'Particles' (Sistem Partikel)?", "id": "Efek Visual Ledakan, Asap, Api." },
    { "en": "Apa Itu 'Skybox' (Kotak Langit)?", "id": "Latar Belakang Lingkungan Dunia Game." },
    { "en": "Apa Itu 'Baking' (Pemanggangan Cahaya)?", "id": "Proses Perhitungan Cahaya Statis Game." },
    { "en": "Apa Itu 'LOD' (Level Of Detail)?", "id": "Penyesuaian Kompleksitas Objek Berdasarkan Jarak." },
    { "en": "Apa Itu 'Occlusion Culling' (Penyisihan Oklusi)?", "id": "Menyembunyikan Objek Yang Tidak Terlihat." },
    { "en": "Apa Itu 'V-Sync' (Vertical Synchronization)?", "id": "Sinkronisasi Frame Rate Dan Monitor." },
    { "en": "Apa Itu 'Input Manager' (Manajer Masukan)?", "id": "Pengaturan Kontrol Keyboard, Mouse, Gamepad." },
    { "en": "Apa Itu 'Scripting' (Penulisan Skrip)?", "id": "Penulisan Kode Logika Perilaku Game." },
    { "en": "Apa Itu 'Build' (Versi Bangun)?", "id": "Proses Pengubahan Proyek Menjadi Aplikasi." },
    { "en": "Apa Itu 'Alpha Version' (Versi Alfa)?", "id": "Tahap Awal Pengembangan Aplikasi Internal." },
    { "en": "Apa Itu 'Beta Version' (Versi Beta)?", "id": "Tahap Pengujian Aplikasi Oleh Pengguna." },
    { "en": "Apa Itu 'Release Candidate' (Kandidat Rilis)?", "id": "Versi Aplikasi Hampir Siap Publikasi." },
    { "en": "Apa Itu 'Store Listing' (Daftar Toko)?", "id": "Halaman Informasi Aplikasi Di Playstore." },
    { "en": "Apa Itu 'ASO' (App Store Optimization)?", "id": "Optimasi Peringkat Aplikasi Di Toko." },
    { "en": "Apa Itu 'In-App Purchase' (Pembelian Dalam)?", "id": "Fitur Transaksi Di Dalam Aplikasi." },
    { "en": "Apa Itu 'AdMob' (Google AdMob)?", "id": "Platform Iklan Google Untuk Mobile." },
    { "en": "Apa Itu 'Firebase' (Platform Firebase)?", "id": "Layanan Backend Awan Milik Google." },
    { "en": "Apa Itu 'Push Notification' (Notifikasi Push)?", "id": "Pesan Langsung Ke Perangkat Pengguna." },
    { "en": "Apa Itu 'Crashlytics' (Firebase Crashlytics)?", "id": "Alat Pelapor Kesalahan Fatal Aplikasi." },
    { "en": "Apa Itu 'Analytics' (Analitik Data)?", "id": "Pemantauan Perilaku Pengguna Dalam Aplikasi." },
    { "en": "Apa Itu 'A/B Testing' (Uji A/B)?", "id": "Eksperimen Perbandingan Dua Fitur Berbeda." },
    { "en": "Apa Itu 'Deep Linking' (Tautan Dalam)?", "id": "Tautan Langsung Ke Konten Aplikasi." },
    { "en": "Apa Itu 'Dark Mode' (Mode Gelap)?", "id": "Tampilan Antarmuka Berbasis Warna Gelap." },
    { "en": "Apa Itu 'Responsive Design' (Desain Responsif)?", "id": "Antarmuka Adaptif Untuk Berbagai Layar." },
    { "en": "Apa Itu 'Latency' (Latensi Game)?", "id": "Waktu Tunda Respon Perintah Pemain." },
    { "en": "Apa Itu 'Packet Loss' (Kehilangan Paket)?", "id": "Data Hilang Saat Pengiriman Jaringan." },
    { "en": "Apa Itu 'Ping' (Uji Ping)?", "id": "Kecepatan Respon Komunikasi Server Game." },
    { "en": "Apa Itu 'Matchmaking' (Pencarian Lawan)?", "id": "Sistem Penghubung Pemain Dalam Multiplayer." },
    { "en": "Apa Itu 'Server-Side' (Sisi Server)?", "id": "Proses Komputasi Di Pusat Data." },
    { "en": "Apa Itu 'Client-Side' (Sisi Klien)?", "id": "Proses Komputasi Di Perangkat Pengguna." },
    { "en": "Apa Itu 'Caching' (Penyimpanan Tembolok)?", "id": "Penyimpanan Data Sementara Akses Cepat." },
    { "en": "Apa Itu 'Minification' (Minifikasi Kode)?", "id": "Pengecilan Ukuran Berkas Kode Sumber." },
    { "en": "Apa Itu 'Compression' (Kompresi Gambar)?", "id": "Pengurangan Ukuran Berkas Tanpa Rusak." },
    { "en": "Apa Itu 'Lazy Loading' (Pemuatan Malas)?", "id": "Pemuatan Konten Saat Hanya Dibutuhkan." },
    { "en": "Apa Itu 'SEO' (Search Engine Optimization)?", "id": "Optimasi Peringkat Website Mesin Pencari." },
    { "en": "Apa Itu 'UI' (User Interface)?", "id": "Elemen Visual Interaksi Pengguna Aplikasi." },
    { "en": "Apa Itu 'UX' (User Experience)?", "id": "Pengalaman Keseluruhan Pengguna Menggunakan Aplikasi." },
    { "en": "Apa Itu 'Wireframe' (Kerangka Gambar)?", "id": "Sketsa Kasar Struktur Antarmuka Aplikasi." },
    { "en": "Apa Itu 'Prototype' (Prototipe)?", "id": "Model Simulasi Awal Fungsi Aplikasi." },
    { "en": "Apa Itu 'Accessibility' (Aksesibilitas)?", "id": "Kemudahan Akses Bagi Semua Pengguna." },
    { "en": "Apa Itu 'Micro-Interaction' (Interaksi Mikro)?", "id": "Efek Kecil Penambah Pengalaman Pengguna." },
    { "en": "Apa Itu 'Skeleton Screen' (Layar Rangka)?", "id": "Tampilan Sementara Saat Memuat Konten." },
    { "en": "Apa Itu 'Splash Screen' (Layar Pembuka)?", "id": "Tampilan Awal Saat Menjalankan Aplikasi." },
    { "en": "Apa Itu 'Onboarding' (Pengenalan Pengguna)?", "id": "Panduan Singkat Penggunaan Aplikasi Baru." },
    { "en": "Apa Itu 'Feedback' (Umpan Balik Pengguna)?", "id": "Respon Sistem Atas Aksi Pengguna." },
    { "en": "Apa Itu 'Localization' (Lokalisasi)?", "id": "Penyesuaian Aplikasi Ke Bahasa Lokal." },
    { "en": "Apa Itu 'Encryption' (Enkripsi)?", "id": "Pengamanan Data Melalui Kode Rahasia." },
    { "en": "Apa Itu 'Hashing' (Hashing Kata Sandi)?", "id": "Pengubahan Data Menjadi Kode Unik." },
    { "en": "Apa Itu 'Token' (Token Akses)?", "id": "Identitas Digital Untuk Akses Keamanan." },
    { "en": "Apa Itu 'Versioning' (Penomoran Versi)?", "id": "Sistem Pelacakan Perubahan Perangkat Lunak." },
    { "en": "Apa Itu 'KCL' (Kirchhoff's Current Law)?", "id": "Jumlah Arus Masuk Titik Nol." },
    { "en": "Apa Itu 'KVL' (Kirchhoff's Voltage Law)?", "id": "Jumlah Tegangan Dalam Loop Nol." },
    { "en": "Apa Itu 'Ohm's Law' (Hukum Ohm)?", "id": "Hubungan Tegangan, Arus, Dan Hambatan." },
    { "en": "Apa Itu 'Faraday's Law' (Hukum Faraday)?", "id": "Induksi Magnetik Menghasilkan Tegangan Listrik." },
    { "en": "Apa Itu 'Lenz's Law' (Hukum Lenz)?", "id": "Arah Arus Induksi Melawan Perubahan." },
    { "en": "Apa Itu 'Thevenin's Theorem' (Teorema Thevenin)?", "id": "Penyederhanaan Rangkaian Menjadi Sumber Tunggal." },
    { "en": "Apa Itu 'Norton's Theorem' (Teorema Norton)?", "id": "Penyederhanaan Rangkaian Menjadi Arus Paralel." },
    { "en": "Apa Itu 'Superposition' (Teorema Superposisi)?", "id": "Analisis Rangkaian Dengan Banyak Sumber." },
    { "en": "Apa Itu 'Maximum Power Transfer' (Transfer Daya Maksimal)?", "id": "Beban Harus Sama Dengan Impedansi." },
    { "en": "Apa Itu 'Millman's Theorem' (Teorema Millman)?", "id": "Metode Mencari Tegangan Banyak Cabang." },
    { "en": "Apa Itu 'Active Power' (Daya Aktif)?", "id": "Daya Nyata Yang Menjadi Kerja." },
    { "en": "Apa Itu 'Reactive Power' (Daya Reaktif)?", "id": "Daya Pembentuk Medan Magnetik Sistem." },
    { "en": "Apa Itu 'Apparent Power' (Daya Semu)?", "id": "Total Daya Dari Hasil Perkalian." },
    { "en": "Apa Itu 'Power Factor' (Faktor Daya)?", "id": "Rasio Daya Aktif Terhadap Semu." },
    { "en": "Apa Itu 'RMS' (Root Mean Square)?", "id": "Nilai Efektif Arus Bolak-Balik Listrik." },
    { "en": "Apa Itu 'Frequency' (Frekuensi Listrik)?", "id": "Jumlah Siklus Gelombang Per Detik." },
    { "en": "Apa Itu 'Period' (Periode Gelombang)?", "id": "Waktu Untuk Satu Siklus Lengkap." },
    { "en": "Apa Itu 'Phase Angle' (Sudut Fase)?", "id": "Perbedaan Waktu Antara Dua Gelombang." },
    { "en": "Apa Itu 'Impedance' (Impedansi)?", "id": "Hambatan Total Aliran Arus Bolak-Balik." },
    { "en": "Apa Itu 'Admittance' (Admitansi)?", "id": "Kebalikan Dari Nilai Impedansi Listrik." },
    { "en": "Apa Itu 'Conductance' (Konduktansi)?", "id": "Kemudahan Aliran Arus Listrik Searah." },
    { "en": "Apa Itu 'Susceptance' (Suseptansi)?", "id": "Bagian Imajiner Dari Nilai Admitansi." },
    { "en": "Apa Itu 'Reactance' (Reaktansi)?", "id": "Hambatan Akibat Induktor Atau Kapasitor." },
    { "en": "Apa Itu 'Inductive Reactance' (Reaktansi Induktif)?", "id": "Hambatan Akibat Medan Magnet Kumparan." },
    { "en": "Apa Itu 'Capacitive Reactance' (Reaktansi Kapasitif)?", "id": "Hambatan Akibat Medan Listrik Kapasitor." },
    { "en": "Apa Itu 'Resonance' (Resonansi Rangkaian)?", "id": "Kondisi Reaktansi Induktif Kapasitif Sama." },
    { "en": "Apa Itu 'Q-Factor' (Quality Factor)?", "id": "Ukuran Kualitas Rangkaian Resonansi Listrik." },
    { "en": "Apa Itu 'Bandwidth' (Lebar Pita)?", "id": "Rentang Frekuensi Operasi Suatu Sistem." },
    { "en": "Apa Itu 'Low Pass Filter' (Filter Lolos Rendah)?", "id": "Penyaring Frekuensi Di Bawah Batas." },
    { "en": "Apa Itu 'High Pass Filter' (Filter Lolos Tinggi)?", "id": "Penyaring Frekuensi Di Atas Batas." },
    { "en": "Apa Itu 'Band Pass Filter' (Filter Lolos Pita)?", "id": "Penyaring Frekuensi Dalam Rentang Tertentu." },
    { "en": "Apa Itu 'Band Stop Filter' (Filter Tolak Pita)?", "id": "Penghambat Frekuensi Dalam Rentang Tertentu." },
    { "en": "Apa Itu 'Transfer Function' (Fungsi Transfer)?", "id": "Rasio Output Terhadap Input Sistem." },
    { "en": "Apa Itu 'Impulse Response' (Respon Impuls)?", "id": "Output Sistem Terhadap Input Singkat." },
    { "en": "Apa Itu 'Step Response' (Respon Langkah)?", "id": "Output Sistem Terhadap Input Konstan." },
    { "en": "Apa Itu 'Transient' (Kondisi Transien)?", "id": "Respon Sistem Sebelum Mencapai Stabil." },
    { "en": "Apa Itu 'Steady State' (Kondisi Tunak)?", "id": "Kondisi Sistem Setelah Menjadi Stabil." },
    { "en": "Apa Itu 'Laplace Transform' (Transformasi Laplace)?", "id": "Metode Analisis Rangkaian Domain Frekuensi." },
    { "en": "Apa Itu 'Z-Transform' (Transformasi Z)?", "id": "Metode Analisis Sinyal Digital Diskrit." },
    { "en": "Apa Itu 'Fourier Series' (Deret Fourier)?", "id": "Representasi Sinyal Periodik Menjadi Sinus." },
    { "en": "Apa Itu 'Op-Amp' (Operational Amplifier)?", "id": "Penguat Tegangan Dengan Penguatan Tinggi." },
    { "en": "Apa Itu 'Inverting Amplifier' (Penguat Membalik)?", "id": "Penguat Dengan Fase Output Terbalik." },
    { "en": "Apa Itu 'Non-Inverting Amplifier' (Penguat Tak-Membalik)?", "id": "Penguat Dengan Fase Output Sama." },
    { "en": "Apa Itu 'Differential Amplifier' (Penguat Diferensial)?", "id": "Penguat Selisih Antara Dua Input." },
    { "en": "Apa Itu 'CMRR' (Common Mode Rejection Ratio)?", "id": "Kemampuan Menolak Gangguan Sinyal Bersama." },
    { "en": "Apa Itu 'Slew Rate' (Laju Slew)?", "id": "Kecepatan Perubahan Tegangan Output Maksimal." },
    { "en": "Apa Itu 'GBWP' (Gain-Bandwidth Product)?", "id": "Konstanta Performa Penguatan Dan Frekuensi." },
    { "en": "Apa Itu 'Input Impedance' (Impedansi Masukan)?", "id": "Hambatan Terlihat Pada Terminal Masukan." },
    { "en": "Apa Itu 'Output Impedance' (Impedansi Keluaran)?", "id": "Hambatan Internal Pada Terminal Keluaran." },
    { "en": "Apa Itu 'Feedback' (Umpan Balik)?", "id": "Pengembalian Sebagian Output Ke Input." },
    { "en": "Apa Itu 'BJT' (Bipolar Junction Transistor)?", "id": "Transistor Pengendali Arus Dua Kutub." },
    { "en": "Apa Itu 'Common Emitter' (Emitor Bersama)?", "id": "Konfigurasi Transistor Sebagai Penguat Tegangan." },
    { "en": "Apa Itu 'Common Collector' (Kolektor Bersama)?", "id": "Konfigurasi Transistor Sebagai Penyangga Arus." },
    { "en": "Apa Itu 'Common Base' (Basis Bersama)?", "id": "Konfigurasi Transistor Frekuensi Sangat Tinggi." },
    { "en": "Apa Itu 'FET' (Field Effect Transistor)?", "id": "Transistor Pengendali Tegangan Medan Listrik." },
    { "en": "Apa Itu 'JFET' (Junction Field Effect Transistor)?", "id": "Transistor Efek Medan Persambungan Sederhana." },
    { "en": "Apa Itu 'MOSFET' (Metal Oxide Semiconductor)?", "id": "Transistor Efek Medan Gerbang Terisolasi." },
    { "en": "Apa Itu 'Enhancement Mode' (Mode Pengayaan)?", "id": "Transistor Aktif Saat Diberi Tegangan." },
    { "en": "Apa Itu 'Depletion Mode' (Mode Pengosongan)?", "id": "Transistor Mati Saat Diberi Tegangan." },
    { "en": "Apa Itu 'Threshold Voltage' (Tegangan Ambang)?", "id": "Tegangan Minimum Untuk Mengaktifkan MOSFET." },
    { "en": "Apa Itu 'Cut-Off Region' (Daerah Cut-Off)?", "id": "Kondisi Transistor Tidak Menghantarkan Arus." },
    { "en": "Apa Itu 'Saturation Region' (Daerah Saturasi)?", "id": "Kondisi Transistor Menghantarkan Arus Maksimal." },
    { "en": "Apa Itu 'Active Region' (Daerah Aktif)?", "id": "Kondisi Transistor Bekerja Sebagai Penguat." },
    { "en": "Apa Itu 'Load Line' (Garis Beban)?", "id": "Garis Batas Operasi Rangkaian Transistor." },
    { "en": "Apa Itu 'Q-Point' (Quiescent Point)?", "id": "Titik Kerja Stabil Sebuah Transistor." },
    { "en": "Apa Itu 'Half Wave Rectifier' (Penyearah Setengah)?", "id": "Penyearah Menggunakan Satu Gelombang Saja." },
    { "en": "Apa Itu 'Full Wave Rectifier' (Penyearah Penuh)?", "id": "Penyearah Menggunakan Seluruh Gelombang AC." },
    { "en": "Apa Itu 'Bridge Rectifier' (Penyearah Jembatan)?", "id": "Penyearah Penuh Menggunakan Empat Dioda." },
    { "en": "Apa Itu 'Ripple Factor' (Faktor Riak)?", "id": "Ukuran Kebersihan Output Tegangan Searah." },
    { "en": "Apa Itu 'Filter Capacitor' (Kapasitor Filter)?", "id": "Penghilang Riak Pada Output Penyearah." },
    { "en": "Apa Itu 'Zener Diode' (Dioda Zener)?", "id": "Dioda Khusus Pembatas Tegangan Tetap." },
    { "en": "Apa Itu 'Varactor Diode' (Dioda Varaktor)?", "id": "Dioda Dengan Kapasitansi Variabel Elektronik." },
    { "en": "Apa Itu 'Tunnel Diode' (Dioda Terowongan)?", "id": "Dioda Dengan Resistansi Negatif Cepat." },
    { "en": "Apa Itu 'LED' (Light Emitting Diode)?", "id": "Dioda Pemancar Cahaya Arus Listrik." },
    { "en": "Apa Itu 'Photodiode' (Fotodioda)?", "id": "Sensor Cahaya Pengubah Menjadi Arus." },
    { "en": "Apa Itu 'Logic Gate' (Gerbang Logika)?", "id": "Blok Dasar Pembentuk Sirkuit Digital." },
    { "en": "Apa Itu 'AND Gate' (Gerbang AND)?", "id": "Output Aktif Jika Semua Input." },
    { "en": "Apa Itu 'OR Gate' (Gerbang OR)?", "id": "Output Aktif Jika Salah Satu." },
    { "en": "Apa Itu 'NOT Gate' (Gerbang NOT)?", "id": "Gerbang Pembalik Nilai Logika Digital." },
    { "en": "Apa Itu 'NAND Gate' (Gerbang NAND)?", "id": "Kebalikan Dari Hasil Gerbang AND." },
    { "en": "Apa Itu 'NOR Gate' (Gerbang NOR)?", "id": "Kebalikan Dari Hasil Gerbang OR." },
    { "en": "Apa Itu 'XOR Gate' (Gerbang XOR)?", "id": "Aktif Jika Input Berbeda Nilai." },
    { "en": "Apa Itu 'XNOR Gate' (Gerbang XNOR)?", "id": "Aktif Jika Input Bernilai Sama." },
    { "en": "Apa Itu 'Flip-Flop' (Flip-Flop)?", "id": "Elemen Memori Satu Bit Digital." },
    { "en": "Apa Itu 'SR Flip-Flop' (Set-Reset)?", "id": "Flip-Flop Paling Dasar Penyimpan Status." },
    { "en": "Apa Itu 'JK Flip-Flop' (Jack-Kilby)?", "id": "Flip-Flop Universal Tanpa Kondisi Terlarang." },
    { "en": "Apa Itu 'D Flip-Flop' (Data)?", "id": "Flip-Flop Penunda Data Satu Detak." },
    { "en": "Apa Itu 'T Flip-Flop' (Toggle)?", "id": "Flip-Flop Pembalik Status Setiap Detak." },
    { "en": "Apa Itu 'MUX' (Multiplexer)?", "id": "Pemilih Satu Jalur Dari Banyak." },
    { "en": "Apa Itu 'DEMUX' (Demultiplexer)?", "id": "Penyalur Satu Jalur Ke Banyak." },
    { "en": "Apa Itu 'Encoder' (Enkoder Digital)?", "id": "Pengubah Jalur Menjadi Kode Biner." },
    { "en": "Apa Itu 'Decoder' (Dekoder Digital)?", "id": "Pengubah Kode Biner Menjadi Jalur." },
    { "en": "Apa Itu 'Shift Register' (Register Geser)?", "id": "Kumpulan Flip-Flop Pemindah Data Seri." },
    { "en": "Apa Itu 'Counter' (Pencacah Digital)?", "id": "Sirkuit Penghitung Jumlah Pulsa Masuk." },
    { "en": "Apa Itu 'ADC' (Analog To Digital Converter)?", "id": "Pengubah Sinyal Analog Menjadi Biner." },
    { "en": "Apa Itu 'DAC' (Digital To Analog Converter)?", "id": "Pengubah Sinyal Biner Menjadi Analog." },
    { "en": "Apa Itu 'Multiplexing' (Multipleksing)?", "id": "Penggabungan Banyak Sinyal Satu Media." },
    { "en": "Apa Itu 'Resolution' (Resolusi Digital)?", "id": "Tingkat Ketelitian Pengubahan Nilai Sinyal." },
    { "en": "Apa Itu 'Duty Cycle' (Siklus Kerja)?", "id": "Rasio Waktu Aktif Terhadap Periode." },
    { "en": "Apa Itu 'HVDC' (High Voltage Direct Current)?", "id": "Transmisi Arus Searah Tegangan Tinggi." },
    { "en": "Apa Itu 'FACTS' (Flexible AC Transmission Systems)?", "id": "Sistem Transmisi Arus Bolak-Balik Fleksibel." },
    { "en": "Apa Itu 'SVC' (Static Var Compensator)?", "id": "Kompensator Daya Reaktif Statis Jaringan." },
    { "en": "Apa Itu 'STATCOM' (Static Synchronous Compensator)?", "id": "Penstabil Tegangan Berbasis Inverter Daya." },
    { "en": "Apa Itu 'TCSC' (Thyristor Controlled Series Capacitor)?", "id": "Kapasitor Seri Terkendali Tiristor Daya." },
    { "en": "Apa Itu 'UPFC' (Unified Power Flow Controller)?", "id": "Pengatur Aliran Daya Sistem Terpadu." },
    { "en": "Apa Itu 'PST' (Phase Shifting Transformer)?", "id": "Transformator Penggeser Fase Aliran Daya." },
    { "en": "Apa Itu 'LCC' (Line Commutated Converter)?", "id": "Konverter Berbasis Komutasi Jaringan Listrik." },
    { "en": "Apa Itu 'VSC' (Voltage Source Converter)?", "id": "Konverter Berbasis Sumber Tegangan Searah." },
    { "en": "Apa Itu 'Bipolar Link' (Sambungan Bipolar)?", "id": "Sistem Transmisi Dengan Dua Penghantar." },
    { "en": "Apa Itu 'Monopolar Link' (Sambungan Monopolar)?", "id": "Sistem Transmisi Dengan Satu Penghantar." },
    { "en": "Apa Itu 'Grid Code' (Aturan Jaringan)?", "id": "Standar Teknis Pengoperasian Sistem Listrik." },
    { "en": "Apa Itu 'Load Shedding' (Pelepasan Beban)?", "id": "Pemutusan Beban Untuk Menjaga Stabilitas." },
    { "en": "Apa Itu 'Blackout' (Padam Total)?", "id": "Kondisi Kegagalan Total Sistem Tenaga." },
    { "en": "Apa Itu 'Brownout' (Penurunan Tegangan)?", "id": "Penurunan Tegangan Listrik Secara Sengaja." },
    { "en": "Apa Itu 'Spinning Reserve' (Cadangan Berputar)?", "id": "Kapasitas Pembangkit Siap Pakai Segera." },
    { "en": "Apa Itu 'Island Mode' (Mode Pulau)?", "id": "Pembangkit Beroperasi Terpisah Dari Jaringan." },
    { "en": "Apa Itu 'Black Start' (Mulai Gelap)?", "id": "Proses Menyalakan Pembangkit Tanpa Grid." },
    { "en": "Apa Itu 'Smart Grid' (Jaringan Cerdas)?", "id": "Jaringan Listrik Digital Dua Arah." },
    { "en": "Apa Itu 'Microgrid' (Jaringan Mikro)?", "id": "Sistem Energi Lokal Mandiri Terpadu." },
    { "en": "Apa Itu 'DER' (Distributed Energy Resources)?", "id": "Sumber Energi Tersebar Kapasitas Kecil." },
    { "en": "Apa Itu 'VPP' (Virtual Power Plant)?", "id": "Pusat Kendali Sumber Energi Terdistribusi." },
    { "en": "Apa Itu 'BESS' (Battery Energy Storage System)?", "id": "Sistem Penyimpanan Energi Baterai Besar." },
    { "en": "Apa Itu 'ESS' (Energy Storage System)?", "id": "Sistem Penyimpanan Cadangan Energi Listrik." },
    { "en": "Apa Itu 'MPPT' (Maximum Power Point Tracking)?", "id": "Optimasi Output Daya Panel Surya." },
    { "en": "Apa Itu 'PV' (Photovoltaic)?", "id": "Sel Pengubah Cahaya Menjadi Listrik." },
    { "en": "Apa Itu 'String Inverter' (Inverter String)?", "id": "Pengubah Daya Untuk Rangkaian Panel." },
    { "en": "Apa Itu 'Micro Inverter' (Inverter Mikro)?", "id": "Inverter Untuk Satu Panel Surya." },
    { "en": "Apa Itu 'Net Metering' (Meteran Netto)?", "id": "Sistem Ekspor Impor Energi Surya." },
    { "en": "Apa Itu 'BOS' (Balance Of System)?", "id": "Komponen Pendukung Instalasi Sistem Surya." },
    { "en": "Apa Itu 'Wind Turbine' (Turbin Angin)?", "id": "Mesin Pengubah Angin Menjadi Listrik." },
    { "en": "Apa Itu 'DFIG' (Doubly Fed Induction Generator)?", "id": "Generator Induksi Untuk Turbin Angin." },
    { "en": "Apa Itu 'PMSG' (Permanent Magnet Synchronous Generator)?", "id": "Generator Magnet Permanen Efisiensi Tinggi." },
    { "en": "Apa Itu 'Nacelle' (Nasel Turbin)?", "id": "Rumah Komponen Utama Turbin Angin." },
    { "en": "Apa Itu 'Yaw Control' (Kendali Yaw)?", "id": "Pengaturan Arah Hadap Turbin Angin." },
    { "en": "Apa Itu 'Pitch Control' (Kendali Pitch)?", "id": "Pengaturan Sudut Bilah Turbin Angin." },
    { "en": "Apa Itu 'SCADA' (Supervisory Control And Data Acquisition)?", "id": "Sistem Pengawasan Dan Akuisisi Data." },
    { "en": "Apa Itu 'PLC' (Programmable Logic Controller)?", "id": "Kontroler Logika Industri Terprogram Digital." },
    { "en": "Apa Itu 'HMI' (Human Machine Interface)?", "id": "Antarmuka Visual Manusia Dan Mesin." },
    { "en": "Apa Itu 'RTU' (Remote Terminal Unit)?", "id": "Unit Pengumpul Data Lapangan Industri." },
    { "en": "Apa Itu 'IED' (Intelligent Electronic Device)?", "id": "Perangkat Elektronik Pintar Sistem Proteksi." },
    { "en": "Apa Itu 'DCS' (Distributed Control System)?", "id": "Sistem Kendali Proses Industri Terdistribusi." },
    { "en": "Apa Itu 'Modbus' (Protokol Modbus)?", "id": "Standar Komunikasi Data Perangkat Industri." },
    { "en": "Apa Itu 'Profibus' (Process Field Bus)?", "id": "Standar Jaringan Komunikasi Lapangan Industri." },
    { "en": "Apa Itu 'CAN Bus' (Controller Area Network)?", "id": "Protokol Komunikasi Data Sistem Otomotif." },
    { "en": "Apa Itu 'BACnet' (Building Automation And Control Network)?", "id": "Protokol Komunikasi Otomasi Gedung Pintar." },
    { "en": "Apa Itu 'VFD' (Variable Frequency Drive)?", "id": "Pengatur Kecepatan Motor Melalui Frekuensi." },
    { "en": "Apa Itu 'Soft Starter' (Starter Lunak)?", "id": "Alat Pengurang Lonjakan Arus Motor." },
    { "en": "Apa Itu 'DOL Starter' (Direct On Line)?", "id": "Starter Motor Dengan Tegangan Penuh." },
    { "en": "Apa Itu 'Star-Delta Starter' (Starter Bintang-Segitiga)?", "id": "Starter Motor Pengurang Arus Start." },
    { "en": "Apa Itu 'Servo Motor' (Motor Servo)?", "id": "Motor Dengan Kendali Posisi Presisi." },
    { "en": "Apa Itu 'Stepper Motor' (Motor Langkah)?", "id": "Motor Bergerak Dalam Sudut Bertahap." },
    { "en": "Apa Itu 'BLDC' (Brushless DC Motor)?", "id": "Motor Searah Tanpa Sikat Karbon." },
    { "en": "Apa Itu 'PMSM' (Permanent Magnet Synchronous Motor)?", "id": "Motor Sinkron Berbasis Magnet Permanen." },
    { "en": "Apa Itu 'Reluctance Motor' (Motor Reluktansi)?", "id": "Motor Berdasarkan Prinsip Reluktansi Magnetik." },
    { "en": "Apa Itu 'Universal Motor' (Motor Universal)?", "id": "Motor Untuk Arus Searah, Bolak-Balik." },
    { "en": "Apa Itu 'Slip' (Slip Motor Induksi)?", "id": "Selisih Kecepatan Medan Dan Rotor." },
    { "en": "Apa Itu 'Synchronous Speed' (Kecepatan Sinkron)?", "id": "Kecepatan Putar Medan Magnet Statis." },
    { "en": "Apa Itu 'Torque' (Torsi Motor)?", "id": "Gaya Putar Yang Dihasilkan Motor." },
    { "en": "Apa Itu 'Power Factor Correction' (Perbaikan Faktor Daya)?", "id": "Optimasi Penggunaan Daya Reaktif Sistem." },
    { "en": "Apa Itu 'Capacitor Bank' (Bank Kapasitor)?", "id": "Kumpulan Kapasitor Penyeimbang Daya Reaktif." },
    { "en": "Apa Itu 'Harmonic Filter' (Filter Harmonisa)?", "id": "Penyaring Gangguan Gelombang Listrik Kotor." },
    { "en": "Apa Itu 'THD' (Total Harmonic Distortion)?", "id": "Persentase Total Distorsi Gelombang Listrik." },
    { "en": "Apa Itu 'K-Factor' (Faktor-K)?", "id": "Ukuran Kemampuan Transformator Menahan Harmonisa." },
    { "en": "Apa Itu 'UPS' (Uninterruptible Power Supply)?", "id": "Penyuplai Daya Cadangan Tanpa Putus." },
    { "en": "Apa Itu 'AVR' (Automatic Voltage Regulator)?", "id": "Sistem Penstabil Tegangan Keluaran Otomatis." },
    { "en": "Apa Itu 'Surge Arrester' (Penangkap Surja)?", "id": "Pelindung Peralatan Dari Lonjakan Tegangan." },
    { "en": "Apa Itu 'MCC' (Motor Control Center)?", "id": "Panel Pusat Kendali Motor Listrik." },
    { "en": "Apa Itu 'LVMDP' (Low Voltage Main Distribution Panel)?", "id": "Panel Utama Distribusi Tegangan Rendah." },
    { "en": "Apa Itu 'Busbar' (Rel Tembaga)?", "id": "Penghantar Utama Arus Besar Panel." },
    { "en": "Apa Itu 'Arc Flash' (Busur Api)?", "id": "Ledakan Cahaya Panas Akibat Arus." },
    { "en": "Apa Itu 'BIL' (Basic Insulation Level)?", "id": "Batas Ketahanan Isolasi Terhadap Surja." },
    { "en": "Apa Itu 'Creepage Distance' (Jarak Rambat)?", "id": "Jarak Terpendek Sepanjang Permukaan Isolasi." },
    { "en": "Apa Itu 'Clearance' (Jarak Bebas)?", "id": "Jarak Terpendek Melalui Udara Terbuka." },
    { "en": "Apa Itu 'Corona Discharge' (Pelepasan Korona)?", "id": "Ionisasi Udara Sekitar Kabel Tegangan." },
    { "en": "Apa Itu 'Insulator' (Isolator)?", "id": "Bahan Penghambat Aliran Arus Listrik." },
    { "en": "Apa Itu 'Dielectric' (Dielektrik)?", "id": "Bahan Isolasi Penyimpan Medan Listrik." },
    { "en": "Apa Itu 'Breakdown Voltage' (Tegangan Tembus)?", "id": "Tegangan Penyebab Kegagalan Bahan Isolasi." },
    { "en": "Apa Itu 'Earthing' (Pembumian)?", "id": "Sistem Penghubung Bagian Logam Bumi." },
    { "en": "Apa Itu 'Grounding' (Pentanahan)?", "id": "Penghubung Bagian Aktif Listrik Bumi." },
    { "en": "Apa Itu 'Soil Resistivity' (Resistivitas Tanah)?", "id": "Ukuran Hambatan Jenis Tanah Lokal." },
    { "en": "Apa Itu 'Step Potential' (Potensial Langkah)?", "id": "Beda Tegangan Antara Dua Kaki." },
    { "en": "Apa Itu 'Touch Potential' (Potensial Sentuh)?", "id": "Beda Tegangan Antara Tangan, Kaki." },
    { "en": "Apa Itu 'Exothermic Welding' (Las Eksotermik)?", "id": "Penyambungan Kabel Arde Melalui Reaksi." },
    { "en": "Apa Itu 'Lightning Rod' (Batang Petir)?", "id": "Batang Logam Penangkap Sambaran Petir." },
    { "en": "Apa Itu 'Faraday Cage' (Sangkar Faraday)?", "id": "Ruang Terlindung Dari Medan Listrik." },
    { "en": "Apa Itu 'EMC' (Electromagnetic Compatibility)?", "id": "Kemampuan Perangkat Beroperasi Tanpa Gangguan." },
    { "en": "Apa Itu 'EMI' (Electromagnetic Interference)?", "id": "Gangguan Gelombang Elektromagnetik Tidak Diinginkan." },
    { "en": "Apa Itu 'RFI' (Radio Frequency Interference)?", "id": "Gangguan Sinyal Pada Frekuensi Radio." },
    { "en": "Apa Itu 'Shielding' (Pelindung Sinyal)?", "id": "Lapisan Logam Penahan Gangguan Luar." },
    { "en": "Apa Itu 'Skin Effect' (Efek Kulit)?", "id": "Arus Mengalir Di Permukaan Kabel." },
    { "en": "Apa Itu 'Ferranti Effect' (Efek Ferranti)?", "id": "Kenaikan Tegangan Di Ujung Transmisi." },
    { "en": "Apa Itu 'Proximity Effect' (Efek Kedekatan)?", "id": "Distorsi Arus Antar Kabel Berdekatan." },
    { "en": "Apa Itu 'Load Flow' (Aliran Beban)?", "id": "Analisis Distribusi Daya Jaringan Listrik." },
    { "en": "Apa Itu 'Short Circuit' (Hubung Singkat)?", "id": "Kondisi Aliran Arus Tanpa Hambatan." },
    { "en": "Apa Itu 'Fault' (Gangguan Sistem)?", "id": "Kondisi Operasi Tidak Normal Listrik." },
    { "en": "Apa Itu 'Availability' (Ketersediaan)?", "id": "Rasio Waktu Sistem Siap Digunakan." },
    { "en": "Apa Itu 'Efficiency' (Efisiensi)?", "id": "Rasio Daya Keluaran Terhadap Masukan." },
    { "en": "Apa Itu 'AM' (Amplitude Modulation)?", "id": "Modulasi Informasi Melalui Perubahan Amplitudo." },
    { "en": "Apa Itu 'FM' (Frequency Modulation)?", "id": "Modulasi Informasi Melalui Perubahan Frekuensi." },
    { "en": "Apa Itu 'PM' (Phase Modulation)?", "id": "Modulasi Informasi Melalui Perubahan Fase." },
    { "en": "Apa Itu 'ASK' (Amplitude Shift Keying)?", "id": "Modulasi Digital Berdasarkan Amplitudo Sinyal." },
    { "en": "Apa Itu 'FSK' (Frequency Shift Keying)?", "id": "Modulasi Digital Berdasarkan Frekuensi Sinyal." },
    { "en": "Apa Itu 'PSK' (Phase Shift Keying)?", "id": "Modulasi Digital Berdasarkan Fase Sinyal." },
    { "en": "Apa Itu 'QAM' (Quadrature Amplitude Modulation)?", "id": "Kombinasi Modulasi Amplitudo Dan Fase." },
    { "en": "Apa Itu 'BPSK' (Binary Phase Shift Keying)?", "id": "Modulasi Fase Dengan Dua Kondisi." },
    { "en": "Apa Itu 'QPSK' (Quadrature Phase Shift Keying)?", "id": "Modulasi Fase Dengan Empat Kondisi." },
    { "en": "Apa Itu 'OFDM' (Orthogonal Frequency Division Multiplexing)?", "id": "Teknik Transmisi Banyak Sub-Pembawa Orto-Gonal." },
    { "en": "Apa Itu 'TDM' (Time Division Multiplexing)?", "id": "Pembagian Kapasitas Berdasarkan Slot Waktu." },
    { "en": "Apa Itu 'FDM' (Frequency Division Multiplexing)?", "id": "Pembagian Kapasitas Berdasarkan Pita Frekuensi." },
    { "en": "Apa Itu 'CDM' (Code Division Multiplexing)?", "id": "Pembagian Kapasitas Berdasarkan Kode Unik." },
    { "en": "Apa Itu 'WDM' (Wavelength Division Multiplexing)?", "id": "Multiplexing Berdasarkan Panjang Gelombang Cahaya." },
    { "en": "Apa Itu 'Bandwidth' (Lebar Pita Frekuensi)?", "id": "Rentang Frekuensi Operasi Saluran Komunikasi." },
    { "en": "Apa Itu 'Baseband' (Sinyal Pita Dasar)?", "id": "Sinyal Informasi Sebelum Proses Modulasi." },
    { "en": "Apa Itu 'Passband' (Sinyal Pita Lolos)?", "id": "Sinyal Hasil Modulasi Siap Transmisi." },
    { "en": "Apa Itu 'SNR' (Signal To Noise Ratio)?", "id": "Rasio Kekuatan Sinyal Terhadap Derau." },
    { "en": "Apa Itu 'BER' (Bit Error Rate)?", "id": "Rasio Kesalahan Bit Selama Transmisi." },
    { "en": "Apa Itu 'Gain' (Penguatan Antena)?", "id": "Kemampuan Antena Mengarahkan Energi Radiasi." },
    { "en": "Apa Itu 'Directivity' (Direktivitas Antena)?", "id": "Ukuran Pemusatan Radiasi Arah Tertentu." },
    { "en": "Apa Itu 'VSWR' (Voltage Standing Wave Ratio)?", "id": "Rasio Gelombang Berdiri Akibat Pantulan." },
    { "en": "Apa Itu 'Return Loss' (Rugi-Rugi Balik)?", "id": "Daya Hilang Akibat Ketidaksesuaian Impedansi." },
    { "en": "Apa Itu 'Impedance Matching' (Penyesuaian Impedansi)?", "id": "Penyamaan Hambatan Maksimal Transfer Daya." },
    { "en": "Apa Itu 'Balun' (Balanced Unbalanced)?", "id": "Penghubung Saluran Seimbang Dan Tak-Seimbang." },
    { "en": "Apa Itu 'Dipole Antenna' (Antena Dipol)?", "id": "Antena Dengan Dua Lengan Penghantar." },
    { "en": "Apa Itu 'Yagi Antenna' (Antena Yagi)?", "id": "Antena Pengarah Dengan Elemen Pasif." },
    { "en": "Apa Itu 'Parabolic Antenna' (Antena Parabola)?", "id": "Antena Reflektor Untuk Komunikasi Satelit." },
    { "en": "Apa Itu 'Waveguide' (Pemandu Gelombang)?", "id": "Penyalur Gelombang Mikro Melalui Tabung." },
    { "en": "Apa Itu 'Coaxial Cable' (Kabel Koaksial)?", "id": "Kabel Dengan Dua Penghantar Seperpusat." },
    { "en": "Apa Itu 'Fiber Optic' (Serat Optik)?", "id": "Media Transmisi Data Melalui Cahaya." },
    { "en": "Apa Itu 'Total Internal Reflection' (Pemantulan Internal)?", "id": "Prinsip Perambatan Cahaya Dalam Serat." },
    { "en": "Apa Itu 'Attenuation' (Redaman Sinyal)?", "id": "Pengurangan Kekuatan Sinyal Sepanjang Jalur." },
    { "en": "Apa Itu 'Dispersion' (Dispersi Sinyal)?", "id": "Pelebaran Pulsa Sinyal Dalam Serat." },
    { "en": "Apa Itu 'Oscilloscope' (Oskiloskop)?", "id": "Alat Visualisasi Bentuk Gelombang Listrik." },
    { "en": "Apa Itu 'Spectrum Analyzer' (Penganalisis Spektrum)?", "id": "Alat Analisis Amplitudo Domain Frekuensi." },
    { "en": "Apa Itu 'Function Generator' (Generator Fungsi)?", "id": "Pembangkit Berbagai Bentuk Gelombang Listrik." },
    { "en": "Apa Itu 'Multimeter' (Multimeter Digital)?", "id": "Alat Ukur Tegangan, Arus, Hambatan." },
    { "en": "Apa Itu 'Wattmeter' (Alat Ukur Daya)?", "id": "Alat Ukur Konsumsi Daya Aktif." },
    { "en": "Apa Itu 'Wheatstone Bridge' (Jembatan Wheatstone)?", "id": "Rangkaian Pengukur Hambatan Sangat Presisi." },
    { "en": "Apa Itu 'Kelvin Bridge' (Jembatan Kelvin)?", "id": "Rangkaian Pengukur Hambatan Sangat Rendah." },
    { "en": "Apa Itu 'Maxwell Bridge' (Jembatan Maxwell)?", "id": "Rangkaian Pengukur Nilai Induktansi Kumparan." },
    { "en": "Apa Itu 'Schering Bridge' (Jembatan Schering)?", "id": "Rangkaian Pengukur Nilai Kapasitansi Kapasitor." },
    { "en": "Apa Itu 'Transducer' (Transduser)?", "id": "Pengubah Besaran Fisik Menjadi Listrik." },
    { "en": "Apa Itu 'Strain Gauge' (Pengukur Regangan)?", "id": "Sensor Perubahan Hambatan Akibat Tekanan." },
    { "en": "Apa Itu 'LVDT' (Linear Variable Differential Transformer)?", "id": "Sensor Pengukur Pergeseran Posisi Linear." },
    { "en": "Apa Itu 'Thermocouple' (Termokopel)?", "id": "Sensor Suhu Berbasis Potensial Logam." },
    { "en": "Apa Itu 'RTD' (Resistance Temperature Detector)?", "id": "Sensor Suhu Berdasarkan Perubahan Hambatan." },
    { "en": "Apa Itu 'Hall Effect Sensor' (Sensor Efek Hall)?", "id": "Sensor Pendeteksi Kekuatan Medan Magnet." },
    { "en": "Apa Itu 'Piezoelectric Sensor' (Sensor Piezoelektrik)?", "id": "Sensor Pembangkit Listrik Akibat Tekanan." },
    { "en": "Apa Itu 'ADC' (Analog To Digital Converter)?", "id": "Pengubah Tegangan Analog Menjadi Digital." },
    { "en": "Apa Itu 'DAC' (Digital To Analog Converter)?", "id": "Pengubah Data Digital Menjadi Tegangan." },
    { "en": "Apa Itu 'Resolution' (Resolusi Alat Ukur)?", "id": "Perubahan Terkecil Yang Dapat Dideteksi." },
    { "en": "Apa Itu 'Accuracy' (Akurasi Alat Ukur)?", "id": "Kedekatan Hasil Ukur Nilai Sebenarnya." },
    { "en": "Apa Itu 'Precision' (Presisi Alat Ukur)?", "id": "Konsistensi Hasil Pengukuran Berulang Kali." },
    { "en": "Apa Itu 'Sensitivity' (Sensitivitas)?", "id": "Rasio Perubahan Output Terhadap Input." },
    { "en": "Apa Itu 'Nyquist Rate' (Laju Nyquist)?", "id": "Frekuensi Sampling Minimal Dua Kali." },
    { "en": "Apa Itu 'Aliasing' (Aliasing Sinyal)?", "id": "Distorsi Akibat Laju Sampling Rendah." },
    { "en": "Apa Itu 'Quantization' (Kuantisasi)?", "id": "Pembulatan Nilai Sinyal Ke Diskrit." },
    { "en": "Apa Itu 'Digital Signal Processing' (Pemrosesan Sinyal Digital)?", "id": "Manipulasi Sinyal Melalui Algoritma Komputer." },
    { "en": "Apa Itu 'Fast Fourier Transform' (Transformasi Fourier Cepat)?", "id": "Algoritma Analisis Frekuensi Sangat Cepat." },
    { "en": "Apa Itu 'FIR Filter' (Finite Impulse Response)?", "id": "Filter Digital Dengan Respon Terbatas." },
    { "en": "Apa Itu 'IIR Filter' (Infinite Impulse Response)?", "id": "Filter Digital Dengan Respon Tak-Terbatas." },
    { "en": "Apa Itu 'Sampling' (Pencuplikan Sinyal)?", "id": "Proses Pengambilan Sampel Sinyal Analog." },
    { "en": "Apa Itu 'PCM' (Pulse Code Modulation)?", "id": "Representasi Digital Dari Sinyal Analog." },
    { "en": "Apa Itu 'PWM' (Pulse Width Modulation)?", "id": "Modulasi Berdasarkan Lebar Pulsa Tegangan." },
    { "en": "Apa Itu 'PPM' (Pulse Position Modulation)?", "id": "Modulasi Berdasarkan Posisi Pulsa Waktu." },
    { "en": "Apa Itu 'PAM' (Pulse Amplitude Modulation)?", "id": "Modulasi Berdasarkan Amplitudo Pulsa Listrik." },
    { "en": "Apa Itu 'Demultiplexing' (Demultipleksing)?", "id": "Pemisahan Kembali Sinyal Dari Media." },
    { "en": "Apa Itu 'Isolator' (Isolator Sinyal)?", "id": "Perangkat Pencegah Aliran Arus Balik." },
    { "en": "Apa Itu 'Circulator' (Sirkulator)?", "id": "Perangkat Penyalur Sinyal Port Berurutan." },
    { "en": "Apa Itu 'Attenuator' (Peredam)?", "id": "Komponen Penurun Kekuatan Sinyal Listrik." },
    { "en": "Apa Itu 'Coupler' (Kopler Pengarah)?", "id": "Alat Pencuplik Sebagian Daya Sinyal." },
    { "en": "Apa Itu 'Mixer' (Pencampur Frekuensi)?", "id": "Alat Pengubah Frekuensi Sinyal Radio." },
    { "en": "Apa Itu 'Local Oscillator' (Osilator Lokal)?", "id": "Pembangkit Sinyal Referensi Dalam Mixer." },
    { "en": "Apa Itu 'PLL' (Phase Locked Loop)?", "id": "Sirkuit Penjaga Keselarasan Fase Sinyal." },
    { "en": "Apa Itu 'VCO' (Voltage Controlled Oscillator)?", "id": "Osilator Frekuensi Terkendali Tegangan Masuk." },
    { "en": "Apa Itu 'Synthesizer' (Sintesis Frekuensi)?", "id": "Pembangkit Frekuensi Radio Akurat Digital." },
    { "en": "Apa Itu 'Noise Figure' (Angka Derau)?", "id": "Ukuran Degradasi Sinyal Oleh Perangkat." },
    { "en": "Apa Itu 'Low Noise Amplifier' (Penguat Derau Rendah)?", "id": "Penguat Sinyal Lemah Dengan Derau." },
    { "en": "Apa Itu 'Power Amplifier' (Penguat Daya Radio)?", "id": "Penguat Sinyal Sebelum Dikirim Antena." },
    { "en": "Apa Itu 'Duplexer' (Duplekser)?", "id": "Pemisah Jalur Kirim Dan Terima." },
    { "en": "Apa Itu 'Diplexer' (Diplekser)?", "id": "Pemisah Dua Frekuensi Berbeda Antena." },
    { "en": "Apa Itu 'Antenna Array' (Larik Antena)?", "id": "Kumpulan Antena Untuk Meningkatkan Penguatan." },
    { "en": "Apa Itu 'Beamforming' (Pembentukan Berkas)?", "id": "Teknik Pengarahan Sinyal Secara Elektronik." },
    { "en": "Apa Itu 'MIMO' (Multiple Input Multiple Output)?", "id": "Teknologi Banyak Antena Pengirim Penerima." },
    { "en": "Apa Itu 'SISO' (Single Input Single Output)?", "id": "Sistem Komunikasi Dengan Antena Tunggal." },
    { "en": "Apa Itu 'Spread Spectrum' (Spektrum Tersebar)?", "id": "Teknik Perluasan Pita Frekuensi Sinyal." },
    { "en": "Apa Itu 'FHSS' (Frequency Hopping Spread Spectrum)?", "id": "Loncatan Frekuensi Untuk Keamanan Transmisi." },
    { "en": "Apa Itu 'DSSS' (Direct Sequence Spread Spectrum)?", "id": "Penyebaran Sinyal Melalui Kode Pseudo-Noise." },
    { "en": "Apa Itu 'Channel Capacity' (Kapasitas Saluran)?", "id": "Laju Informasi Maksimal Suatu Saluran." },
    { "en": "Apa Itu 'Shannon's Theorem' (Teorema Shannon)?", "id": "Batas Teoretis Kapasitas Saluran Komunikasi." },
    { "en": "Apa Itu 'Intersymbol Interference' (Interferensi Antar Simbol)?", "id": "Tumpang Tindih Sinyal Akibat Distorsi." },
    { "en": "Apa Itu 'Eye Diagram' (Diagram Mata)?", "id": "Alat Analisis Kualitas Sinyal Digital." },
    { "en": "Apa Itu 'Equalization' (Ekualisasi)?", "id": "Kompensasi Distorsi Sinyal Dalam Saluran." },
    { "en": "Apa Itu 'Error Correction Code' (Kode Koreksi)?", "id": "Teknik Perbaikan Data Transmisi Rusak." },
    { "en": "Apa Itu 'Parity Bit' (Bit Paritas)?", "id": "Bit Tambahan Pendeteksi Kesalahan Data." },
    { "en": "Apa Itu 'Cyclic Redundancy Check' (Cek Redundansi Siklik)?", "id": "Metode Verifikasi Integritas Data Digital." },
    { "en": "Apa Itu 'K3 Listrik' (Keselamatan Dan Kesehatan Kerja)?", "id": "Standar Keamanan Bekerja Dengan Listrik." },
    { "en": "Apa Itu 'PUIL 2011' (Persyaratan Umum Instalasi Listrik)?", "id": "Pedoman Standar Instalasi Listrik Indonesia." },
    { "en": "Apa Itu 'LOTO' (Lockout Tagout)?", "id": "Prosedur Penguncian Isolasi Energi Bahaya." },
    { "en": "Apa Itu 'PPE' (Personal Protective Equipment)?", "id": "Alat Pelindung Diri Saat Bekerja." },
    { "en": "Apa Itu 'Arc Flash' (Busur Api Listrik)?", "id": "Pelepasan Energi Panas Ledakan Listrik." },
    { "en": "Apa Itu 'GFCI' (Ground Fault Circuit Interrupter)?", "id": "Pemutus Arus Deteksi Kebocoran Tanah." },
    { "en": "Apa Itu 'AFCI' (Arc Fault Circuit Interrupter)?", "id": "Pemutus Arus Deteksi Loncatan Api." },
    { "en": "Apa Itu 'ELCB' (Earth Leakage Circuit Breaker)?", "id": "Pemutus Daya Saat Terjadi Kebocoran." },
    { "en": "Apa Itu 'RCCB' (Residual Current Circuit Breaker)?", "id": "Proteksi Arus Sisa Tanpa Overload." },
    { "en": "Apa Itu 'RCBO' (Residual Current Breaker Overload)?", "id": "Proteksi Arus Sisa Dan Beban." },
    { "en": "Apa Itu 'MCB' (Miniature Circuit Breaker)?", "id": "Pemutus Arus Beban Lebih Kecil." },
    { "en": "Apa Itu 'MCCB' (Molded Case Circuit Breaker)?", "id": "Pemutus Arus Kapasitas Industri Menengah." },
    { "en": "Apa Itu 'ACB' (Air Circuit Breaker)?", "id": "Pemutus Arus Besar Media Udara." },
    { "en": "Apa Itu 'VCB' (Vacuum Circuit Breaker)?", "id": "Pemutus Arus Besar Media Vakum." },
    { "en": "Apa Itu 'SF6' (Sulfur Hexafluoride Breaker)?", "id": "Pemutus Arus Media Gas Isolasi." },
    { "en": "Apa Itu 'Disconnect Switch' (Saklar Pemisah)?", "id": "Saklar Isolasi Rangkaian Tanpa Beban." },
    { "en": "Apa Itu 'Safety Switch' (Saklar Pengaman)?", "id": "Pemutus Daya Darurat Manual Cepat." },
    { "en": "Apa Itu 'Contactor' (Kontaktor Magnetik)?", "id": "Saklar Elektromagnetik Kendali Arus Besar." },
    { "en": "Apa Itu 'Overload Relay' (Relay Beban Lebih)?", "id": "Proteksi Panas Berlebih Pada Motor." },
    { "en": "Apa Itu 'Soft Starter' (Starter Lunak)?", "id": "Pengatur Arus Start Motor Bertahap." },
    { "en": "Apa Itu 'Limit Switch' (Saklar Pembatas)?", "id": "Sensor Batas Gerak Mekanik Objek." },
    { "en": "Apa Itu 'Proximity Sensor' (Sensor Jarak)?", "id": "Pendeteksi Keberadaan Objek Tanpa Sentuhan." },
    { "en": "Apa Itu 'Push Button' (Tombol Tekan)?", "id": "Alat Kendali Manual Sirkuit Kontrol." },
    { "en": "Apa Itu 'Emergency Stop' (Berhenti Darurat)?", "id": "Tombol Penghenti Sistem Saat Bahaya." },
    { "en": "Apa Itu 'Pilot Light' (Lampu Indikator)?", "id": "Lampu Penanda Status Operasi Panel." },
    { "en": "Apa Itu 'Busbar' (Rel Tembaga)?", "id": "Penghantar Utama Distribusi Arus Besar." },
    { "en": "Apa Itu 'Terminal Block' (Blok Terminal)?", "id": "Titik Sambungan Kabel Dalam Panel." },
    { "en": "Apa Itu 'DIN Rail' (Rel DIN)?", "id": "Rel Logam Dudukan Komponen Panel." },
    { "en": "Apa Itu 'Enclosure' (Kotak Panel)?", "id": "Wadah Pelindung Komponen Listrik Industri." },
    { "en": "Apa Itu 'IP Rating' (Ingress Protection)?", "id": "Tingkat Perlindungan Terhadap Debu, Air." },
    { "en": "Apa Itu 'IK Rating' (Impact Protection)?", "id": "Tingkat Perlindungan Terhadap Benturan Mekanik." },
    { "en": "Apa Itu 'Explosion Proof' (Tahan Ledakan)?", "id": "Peralatan Aman Untuk Area Berbahaya." },
    { "en": "Apa Itu 'ATEX' (Atmospheres Explosibles)?", "id": "Sertifikasi Peralatan Area Potensi Ledakan." },
    { "en": "Apa Itu 'Intrinsically Safe' (Aman Intrinsik)?", "id": "Sistem Energi Rendah Pencegah Ledakan." },
    { "en": "Apa Itu 'Earthing' (Pembumian Peralatan)?", "id": "Penghantar Bagian Logam Ke Bumi." },
    { "en": "Apa Itu 'Grounding' (Pentanahan Sistem)?", "id": "Penghantar Titik Netral Ke Bumi." },
    { "en": "Apa Itu 'Earth Rod' (Batang Arde)?", "id": "Logam Elektroda Penyalur Arus Bumi." },
    { "en": "Apa Itu 'Bonding' (Ikatan Penyamaratakan)?", "id": "Penyambungan Logam Untuk Menyamakan Potensial." },
    { "en": "Apa Itu 'Ground Loop' (Loop Tanah)?", "id": "Arus Gangguan Akibat Perbedaan Potensial." },
    { "en": "Apa Itu 'Surge Arrester' (Penangkap Surja)?", "id": "Pelindung Peralatan Dari Tegangan Lonjakan." },
    { "en": "Apa Itu 'Lightning Protection' (Perlindungan Petir)?", "id": "Sistem Penyalur Sambaran Petir Bumi." },
    { "en": "Apa Itu 'Insulation Resistance' (Tahanan Isolasi)?", "id": "Nilai Hambatan Lapangan Pelindung Kabel." },
    { "en": "Apa Itu 'Megger Test' (Uji Megger)?", "id": "Prosedur Pengukuran Tahanan Isolasi Listrik." },
    { "en": "Apa Itu 'Hipot Test' (High Potential)?", "id": "Uji Kekuatan Isolasi Tegangan Tinggi." },
    { "en": "Apa Itu 'Continuity Test' (Uji Kontinuitas)?", "id": "Pengecekan Keutuhan Jalur Penghantar Listrik." },
    { "en": "Apa Itu 'Load Bank' (Bank Beban)?", "id": "Alat Simulasi Beban Uji Pembangkit." },
    { "en": "Apa Itu 'Transformer Oil' (Minyak Transformator)?", "id": "Cairan Isolasi Dan Pendingin Trafo." },
    { "en": "Apa Itu 'Dielectric Strength' (Kekuatan Dielektrik)?", "id": "Kemampuan Bahan Menahan Tegangan Tembus." },
    { "en": "Apa Itu 'Flashover' (Loncatan Api)?", "id": "Loncatan Listrik Melalui Udara Isolator." },
    { "en": "Apa Itu 'Creepage' (Jarak Rambat)?", "id": "Arus Bocor Sepanjang Permukaan Isolator." },
    { "en": "Apa Itu 'Arc Chute' (Pemadam Busur)?", "id": "Komponen Pemutus Percikan Api Breaker." },
    { "en": "Apa Itu 'Interlock' (Saling Kunci)?", "id": "Sistem Keamanan Pencegah Operasi Salah." },
    { "en": "Apa Itu 'Dead Front' (Depan Mati)?", "id": "Panel Tanpa Bagian Bertegangan Terbuka." },
    { "en": "Apa Itu 'Live Working' (Kerja Bertegangan)?", "id": "Prosedur Perbaikan Saat Listrik Mengalir." },
    { "en": "Apa Itu 'Hot Stick' (Tongkat Isolasi)?", "id": "Alat Isolasi Operasi Tegangan Tinggi." },
    { "en": "Apa Itu 'Rubber Mat' (Karpet Karet)?", "id": "Alas Isolasi Keamanan Operator Panel." },
    { "en": "Apa Itu 'Voltage Detector' (Pendeteksi Tegangan)?", "id": "Alat Cek Keberadaan Aliran Listrik." },
    { "en": "Apa Itu 'Non-Contact Tester' (Tes Tanpa Sentuh)?", "id": "Pendeteksi Listrik Melalui Medan Elektromagnetik." },
    { "en": "Apa Itu 'Phase Sequence' (Urutan Fase)?", "id": "Urutan Tegangan Mencapai Nilai Puncak." },
    { "en": "Apa Itu 'Phase Rotation Meter' (Meter Rotasi)?", "id": "Alat Cek Arah Putaran Fase." },
    { "en": "Apa Itu 'Synchroscope' (Sinkroskop)?", "id": "Alat Bantu Paralel Dua Pembangkit." },
    { "en": "Apa Itu 'Power Quality' (Kualitas Daya)?", "id": "Ukuran Kesesuaian Tegangan Dan Arus." },
    { "en": "Apa Itu 'Swell' (Tegangan Naik Sesaat)?", "id": "Kenaikan Tegangan Listrik Durasi Singkat." },
    { "en": "Apa Itu 'Sag' (Tegangan Turun Sesaat)?", "id": "Penurunan Tegangan Listrik Durasi Singkat." },
    { "en": "Apa Itu 'Flicker' (Kedipan Tegangan)?", "id": "Variasi Tegangan Berulang Cepat Mengganggu." },
    { "en": "Apa Itu 'Harmonics' (Harmonisa Listrik)?", "id": "Gangguan Gelombang Kelipatan Frekuensi Dasar." },
    { "en": "Apa Itu 'Inrush Current' (Arus Inrush)?", "id": "Lonjakan Arus Saat Menyalakan Beban." },
    { "en": "Apa Itu 'Nominal Voltage' (Tegangan Nominal)?", "id": "Nilai Tegangan Operasi Standar Sistem." },
    { "en": "Apa Itu 'Rated Current' (Arus Rating)?", "id": "Batas Arus Operasi Aman Peralatan." },
    { "en": "Apa Itu 'Duty Cycle' (Siklus Kerja)?", "id": "Rasio Waktu Operasi Terhadap Istirahat." },
    { "en": "Apa Itu 'Ambient Temperature' (Suhu Sekitar)?", "id": "Suhu Lingkungan Tempat Peralatan Berada." },
    { "en": "Apa Itu 'Derating' (Penurunan Rating)?", "id": "Pengurangan Kapasitas Akibat Kondisi Ekstrem." },
    { "en": "Apa Itu 'Heat Sink' (Sirip Pendingin)?", "id": "Komponen Pembuang Panas Perangkat Elektronika." },
    { "en": "Apa Itu 'Thermal Runaway' (Kegagalan Termal)?", "id": "Kenaikan Panas Tak Terkendali Baterai." },
    { "en": "Apa Itu 'Current Limiting' (Pembatasan Arus)?", "id": "Fitur Proteksi Mencegah Arus Berlebih." },
    { "en": "Apa Itu 'Crowbar Circuit' (Sirkuit Linggis)?", "id": "Proteksi Tegangan Lebih Hubung Singkat." },
    { "en": "Apa Itu 'Snubber Circuit' (Sirkuit Snubber)?", "id": "Peredam Lonjakan Tegangan Saat Transien." },
    { "en": "Apa Itu 'Flyback Diode' (Dioda Flyback)?", "id": "Pelindung Dari Tegangan Kejut Induktif." },
    { "en": "Apa Itu 'Optical Isolator' (Isolator Optik)?", "id": "Pemisah Sinyal Menggunakan Media Cahaya." },
    { "en": "Apa Itu 'Galvanic Isolation' (Isolasi Galvanik)?", "id": "Pemisahan Sirkuit Tanpa Kontak Elektrik." },
    { "en": "Apa Itu 'Bus Tie' (Pengikat Busbar)?", "id": "Penghubung Antar Dua Bagian Panel." },
    { "en": "Apa Itu 'Feeder' (Penyulang)?", "id": "Saluran Distribusi Dari Gardu Induk." },
    { "en": "Apa Itu 'Main Lug' (Lug Utama)?", "id": "Titik Masuk Kabel Utama Panel." },
    { "en": "Apa Itu 'Subpanel' (Panel Cabang)?", "id": "Panel Distribusi Kecil Dibawah Utama." },
    { "en": "Apa Itu 'Branch Circuit' (Sirkuit Cabang)?", "id": "Jalur Listrik Terakhir Menuju Beban." },
    { "en": "Apa Itu 'Wire Gauge' (Ukuran Kawat)?", "id": "Standar Ketebalan Penghantar Kabel Listrik." },
    { "en": "Apa Itu 'Ampacity' (Kapasitas Arus Kabel)?", "id": "Arus Maksimal Yang Dapat Dialirkan." },
    { "en": "Apa Itu 'Voltage Drop' (Jatuh Tegangan)?", "id": "Kehilangan Tegangan Akibat Hambatan Kabel." },
    { "en": "Apa Itu 'Conduit' (Pipa Pelindung)?", "id": "Saluran Pelindung Kabel Instalasi Listrik." },
    { "en": "Apa Itu 'Cable Tray' (Baki Kabel)?", "id": "Struktur Penyangga Jalur Kabel Besar." },
    { "en": "Apa Itu 'Junction Box' (Kotak Sambung)?", "id": "Wadah Tempat Penyambungan Kabel Listrik." },
    { "en": "Apa Itu 'Strain Relief' (Peredam Tarikan)?", "id": "Pelindung Kabel Dari Beban Tarik." },
    { "en": "Apa Itu 'Splice' (Sambungan Kabel)?", "id": "Penyatuan Dua Ujung Penghantar Listrik." },
    { "en": "Apa Itu 'Lug' (Sepatu Kabel)?", "id": "Terminal Penghubung Ujung Kabel Panel." },
    { "en": "Apa Itu 'Crimping' (Pengerutan)?", "id": "Metode Pemasangan Sepatu Kabel Tekan." },
    { "en": "Apa Itu 'GTO' (Gate Turn-Off Thyristor)?", "id": "Thyristor Yang Bisa Dimatikan Gerbang." },
    { "en": "Apa Itu 'IGCT' (Integrated Gate-Commutated Thyristor)?", "id": "Saklar Daya Tinggi Frekuensi Cepat." },
    { "en": "Apa Itu 'MCT' (MOS Controlled Thyristor)?", "id": "Thyristor Terkendali Gerbang Efek Medan." },
    { "en": "Apa Itu 'SIT' (Static Induction Transistor)?", "id": "Transistor Daya Tinggi Frekuensi Tinggi." },
    { "en": "Apa Itu 'SITH' (Static Induction Thyristor)?", "id": "Thyristor Kecepatan Saklar Sangat Tinggi." },
    { "en": "Apa Itu 'LASCR' (Light Activated SCR)?", "id": "Penyearah Terkendali Berbasis Cahaya Optik." },
    { "en": "Apa Itu 'Converter' (Konverter Elektronika Daya)?", "id": "Pengubah Parameter Energi Listrik Digital." },
    { "en": "Apa Itu 'Cycloconverter' (Siklokonverter)?", "id": "Pengubah Frekuensi AC Langsung Rendah." },
    { "en": "Apa Itu 'Matrix Converter' (Konverter Matriks)?", "id": "Konverter AC Ke AC Tanpa." },
    { "en": "Apa Itu 'Multilevel Inverter' (Inverter Bertingkat)?", "id": "Inverter Penghasil Gelombang Sinus Halus." },
    { "en": "Apa Itu 'NPC' (Neutral Point Clamped)?", "id": "Topologi Inverter Bertingkat Titik Netral." },
    { "en": "Apa Itu 'Cascaded H-Bridge' (Jembatan-H Bertingkat)?", "id": "Inverter Susunan Seri Jembatan Daya." },
    { "en": "Apa Itu 'Buck Converter' (Konverter Buck)?", "id": "Rangkaian Penurun Tegangan Searah Efisien." },
    { "en": "Apa Itu 'Boost Converter' (Konverter Boost)?", "id": "Rangkaian Penaik Tegangan Searah Efisien." },
    { "en": "Apa Itu 'Buck-Boost Converter' (Konverter Buck-Boost)?", "id": "Rangkaian Penaik Penurun Tegangan Searah." },
    { "en": "Apa Itu 'Cuk Converter' (Konverter Cuk)?", "id": "Konverter DC Dengan Polaritas Terbalik." },
    { "en": "Apa Itu 'Sepic Converter' (Single-Ended Primary-Inductor)?", "id": "Konverter DC Tanpa Pembalikan Polaritas." },
    { "en": "Apa Itu 'Flyback Converter' (Konverter Flyback)?", "id": "Konverter DC Dengan Isolasi Trafo." },
    { "en": "Apa Itu 'Forward Converter' (Konverter Forward)?", "id": "Konverter DC Isolasi Transfer Langsung." },
    { "en": "Apa Itu 'Push-Pull Converter' (Konverter Dorong-Tarik)?", "id": "Konverter DC Dengan Dua Saklar." },
    { "en": "Apa Itu 'Half-Bridge Converter' (Konverter Setengah Jembatan)?", "id": "Konverter DC Menggunakan Dua Kapasitor." },
    { "en": "Apa Itu 'Full-Bridge Converter' (Konverter Jembatan Penuh)?", "id": "Konverter DC Dengan Empat Saklar." },
    { "en": "Apa Itu 'ZVS' (Zero Voltage Switching)?", "id": "Pensakelaran Saat Tegangan Bernilai Nol." },
    { "en": "Apa Itu 'ZCS' (Zero Current Switching)?", "id": "Pensakelaran Saat Arus Bernilai Nol." },
    { "en": "Apa Itu 'Resonant Converter' (Konverter Resonansi)?", "id": "Konverter Dengan Kerugian Saklar Kecil." },
    { "en": "Apa Itu 'Soft Switching' (Pensakelaran Lunak)?", "id": "Teknik Pengurangan Rugi-Rugi Daya Saklar." },
    { "en": "Apa Itu 'Hard Switching' (Pensakelaran Keras)?", "id": "Pensakelaran Dengan Tegangan Arus Tinggi." },
    { "en": "Apa Itu 'Snubber Circuit' (Sirkuit Snubber)?", "id": "Peredam Lonjakan Tegangan Transien Saklar." },
    { "en": "Apa Itu 'Commutation' (Komutasi Elektronika Daya)?", "id": "Proses Mematikan Komponen Semikonduktor Daya." },
    { "en": "Apa Itu 'Duty Ratio' (Rasio Tugas)?", "id": "Rasio Waktu Hidup Terhadap Periode." },
    { "en": "Apa Itu 'Modulation Index' (Indeks Modulasi)?", "id": "Parameter Pengatur Amplitudo Output Inverter." },
    { "en": "Apa Itu 'Space Vector PWM' (Pulse Width Modulation)?", "id": "Teknik Modulasi Vektor Ruang Digital." },
    { "en": "Apa Itu 'Total Harmonic Distortion' (Distorsi Harmonisa)?", "id": "Ukuran Kualitas Gelombang Listrik Bersih." },
    { "en": "Apa Itu 'Active Power Filter' (Filter Daya Aktif)?", "id": "Kompensator Harmonisa Berbasis Elektronika Daya." },
    { "en": "Apa Itu 'PFC' (Power Factor Correction)?", "id": "Teknik Perbaikan Nilai Faktor Daya." },
    { "en": "Apa Itu 'Interleaved Converter' (Konverter Interleaved)?", "id": "Konverter Paralel Pengurang Riak Arus." },
    { "en": "Apa Itu 'Bidirectional Converter' (Konverter Dua Arah)?", "id": "Konverter Dengan Aliran Daya Bolak-Balik." },
    { "en": "Apa Itu 'Power Density' (Kerapatan Daya)?", "id": "Jumlah Daya Per Satuan Volume." },
    { "en": "Apa Itu 'SOA' (Safe Operating Area)?", "id": "Batas Operasi Aman Komponen Semikonduktor." },
    { "en": "Apa Itu 'Thermal Resistance' (Resistansi Termal)?", "id": "Hambatan Terhadap Aliran Panas Komponen." },
    { "en": "Apa Itu 'Heat Sink' (Pendingin)?", "id": "Media Pembuang Panas Ke Lingkungan." },
    { "en": "Apa Itu 'Phase Controlled Rectifier' (Penyearah Terkendali)?", "id": "Penyearah Menggunakan Tiristor Sudut Picu." },
    { "en": "Apa Itu 'Firing Angle' (Sudut Picu)?", "id": "Waktu Mulai Menghantarkan Arus Tiristor." },
    { "en": "Apa Itu 'Extinction Angle' (Sudut Pemadaman)?", "id": "Waktu Saat Arus Berhenti Mengalir." },
    { "en": "Apa Itu 'Overlap Angle' (Sudut Tumpang Tindih)?", "id": "Durasi Perpindahan Arus Antar Komponen." },
    { "en": "Apa Itu 'Free-Wheeling Diode' (Dioda Free-Wheeling)?", "id": "Dioda Pelepas Energi Beban Induktif." },
    { "en": "Apa Itu 'Continuous Conduction Mode' (Mode Kontinu)?", "id": "Arus Induktor Tidak Pernah Nol." },
    { "en": "Apa Itu 'Discontinuous Conduction Mode' (Mode Diskontinu)?", "id": "Arus Induktor Mencapai Nilai Nol." },
    { "en": "Apa Itu 'Load Flow Analysis' (Analisis Aliran Daya)?", "id": "Studi Distribusi Daya Sistem Tenaga." },
    { "en": "Apa Itu 'Swing Bus' (Bus Berayun)?", "id": "Bus Referensi Tegangan Sistem Listrik." },
    { "en": "Apa Itu 'PV Bus' (Generator Bus)?", "id": "Bus Dengan Tegangan Daya Tetap." },
    { "en": "Apa Itu 'PQ Bus' (Load Bus)?", "id": "Bus Dengan Beban Daya Tetap." },
    { "en": "Apa Itu 'Newton-Raphson Method' (Metode Newton-Raphson)?", "id": "Algoritma Penyelesaian Aliran Daya Cepat." },
    { "en": "Apa Itu 'Gauss-Seidel Method' (Metode Gauss-Seidel)?", "id": "Metode Iterasi Aliran Daya Sederhana." },
    { "en": "Apa Itu 'Fault Analysis' (Analisis Gangguan)?", "id": "Perhitungan Arus Saat Hubung Singkat." },
    { "en": "Apa Itu 'Symmetrical Fault' (Gangguan Simetris)?", "id": "Hubung Singkat Tiga Fase Seimbang." },
    { "en": "Apa Itu 'Unsymmetrical Fault' (Gangguan Tak Simetris)?", "id": "Gangguan Fase Tanah Atau Antar." },
    { "en": "Apa Itu 'Stability' (Stabilitas Sistem Tenaga)?", "id": "Kemampuan Sistem Kembali Ke Normal." },
    { "en": "Apa Itu 'Steady State Stability' (Stabilitas Tunak)?", "id": "Stabilitas Terhadap Perubahan Beban Kecil." },
    { "en": "Apa Itu 'Transient Stability' (Stabilitas Transien)?", "id": "Stabilitas Terhadap Gangguan Besar Mendadak." },
    { "en": "Apa Itu 'Dynamic Stability' (Stabilitas Dinamis)?", "id": "Stabilitas Terhadap Osilasi Frekuensi Rendah." },
    { "en": "Apa Itu 'Critical Clearing Angle' (Sudut Pemutusan Kritis)?", "id": "Batas Sudut Pemutusan Gangguan Stabil." },
    { "en": "Apa Itu 'Equal Area Criterion' (Kriteria Luas Sama)?", "id": "Metode Analisis Stabilitas Mesin Tunggal." },
    { "en": "Apa Itu 'Swing Equation' (Persamaan Ayunan)?", "id": "Dinamika Rotor Generator Terhadap Gangguan." },
    { "en": "Apa Itu 'Governor' (Gubernur Pembangkit)?", "id": "Pengatur Kecepatan Putar Turbin Otomatis." },
    { "en": "Apa Itu 'LFC' (Load Frequency Control)?", "id": "Kendali Frekuensi Akibat Perubahan Beban." },
    { "en": "Apa Itu 'AGC' (Automatic Generation Control)?", "id": "Kendali Otomatis Output Daya Generator." },
    { "en": "Apa Itu 'Economic Dispatch' (Penyaluran Ekonomis)?", "id": "Optimasi Biaya Operasi Antar Pembangkit." },
    { "en": "Apa Itu 'Unit Commitment' (Komitmen Unit)?", "id": "Jadwal Operasi Unit Pembangkit Listrik." },
    { "en": "Apa Itu 'Reliability' (Keandalan Sistem Tenaga)?", "id": "Kemampuan Melayani Beban Tanpa Gangguan." },
    { "en": "Apa Itu 'SAIDI' (System Average Interruption Duration Index)?", "id": "Indeks Durasi Rata-Rata Pemadaman Listrik." },
    { "en": "Apa Itu 'SAIFI' (System Average Interruption Frequency Index)?", "id": "Indeks Frekuensi Rata-Rata Pemadaman Listrik." },
    { "en": "Apa Itu 'Contingency Analysis' (Analisis Kontinjensi)?", "id": "Evaluasi Sistem Terhadap Kegagalan Komponen." },
    { "en": "Apa Itu 'State Estimation' (Estimasi Keadaan)?", "id": "Perkiraan Kondisi Real-Time Jaringan Listrik." },
    { "en": "Apa Itu 'WAMS' (Wide Area Monitoring System)?", "id": "Sistem Pemantauan Jaringan Skala Luas." },
    { "en": "Apa Itu 'PMU' (Phasor Measurement Unit)?", "id": "Alat Ukur Fasor Sinkron Cepat." },
    { "en": "Apa Itu 'Harmonic Analysis' (Analisis Harmonisa)?", "id": "Studi Dampak Distorsi Gelombang Listrik." },
    { "en": "Apa Itu 'Optimal Power Flow' (Aliran Daya Optimal)?", "id": "Aliran Daya Dengan Batasan Optimasi." },
    { "en": "Apa Itu 'Voltage Collapse' (Kolaps Tegangan)?", "id": "Penurunan Tegangan Drastis Tak Terkendali." },
    { "en": "Apa Itu 'P-V Curve' (Kurva P-V)?", "id": "Hubungan Daya Aktif Terhadap Tegangan." },
    { "en": "Apa Itu 'Q-V Curve' (Kurva Q-V)?", "id": "Hubungan Daya Reaktif Terhadap Tegangan." },
    { "en": "Apa Itu 'Spinning Reserve' (Cadangan Berputar)?", "id": "Generator Aktif Penyeimbang Beban Mendadak." },
    { "en": "Apa Itu 'Cold Reserve' (Cadangan Dingin)?", "id": "Generator Mati Siap Dioperasikan Kembali." },
    { "en": "Apa Itu 'Hot Reserve' (Cadangan Panas)?", "id": "Generator Siap Sinkron Dalam Waktu." },
    { "en": "Apa Itu 'Hydro-Thermal Coordination' (Koordinasi Hidro-Termal)?", "id": "Optimasi Penggunaan Air Dan Bahan." },
    { "en": "Apa Itu 'Cascading Failure' (Kegagalan Berantai)?", "id": "Kegagalan Satu Komponen Memicu Lainnya." },
    { "en": "Apa Itu 'Transmission Loss' (Rugi-Rugi Transmisi)?", "id": "Daya Hilang Saat Pengiriman Energi." },
    { "en": "Apa Itu 'Wheeling Charges' (Biaya Wheeling)?", "id": "Biaya Penggunaan Jalur Transmisi Listrik." },
    { "en": "Apa Itu 'Deregulation' (Deregulasi Pasar Listrik)?", "id": "Persaingan Terbuka Dalam Industri Listrik." },
    { "en": "Apa Itu 'ISO' (Independent System Operator)?", "id": "Pengelola Jaringan Listrik Independen Adil." },
    { "en": "Apa Itu 'ANC' (Ancillary Services)?", "id": "Layanan Tambahan Pendukung Stabilitas Jaringan." },
    { "en": "Apa Itu 'Load Profiling' (Pembuatan Profil Beban)?", "id": "Pola Konsumsi Listrik Terhadap Waktu." },
    { "en": "Apa Itu 'Demand Response' (Respon Permintaan)?", "id": "Penyesuaian Beban Pelanggan Terhadap Harga." },
    { "en": "Apa Itu 'Time Of Use Pricing' (Harga Berdasarkan Waktu)?", "id": "Tarif Listrik Berbeda Setiap Jam." },
    { "en": "Apa Itu 'Interconnection' (Interkoneksi Jaringan)?", "id": "Penggabungan Dua Sistem Tenaga Listrik." },
    { "en": "Apa Itu 'Sub-Synchronous Resonance' (Resonansi Sub-Sinkron)?", "id": "Interaksi Getaran Poros Dan Listrik." },
    { "en": "Apa Itu 'Power Quality Monitor' (Pemantau Kualitas Daya)?", "id": "Alat Analisis Gangguan Tegangan Arus." },
    { "en": "Apa Itu 'Vector Control' (Kendali Vektor)?", "id": "Metode Kendali Motor Performa Tinggi." },
    { "en": "Apa Itu 'DTC' (Direct Torque Control)?", "id": "Kendali Torsi Motor Secara Langsung." },
    { "en": "Apa Itu 'STC' (Standard Test Conditions)?", "id": "Kondisi Standar Pengujian Panel Surya." },
    { "en": "Apa Itu 'NOCT' (Nominal Operating Cell Temperature)?", "id": "Suhu Operasi Normal Sel Surya." },
    { "en": "Apa Itu 'MPPT' (Maximum Power Point Tracking)?", "id": "Optimasi Penyerapan Daya Panel Surya." },
    { "en": "Apa Itu 'BIPV' (Building Integrated Photovoltaics)?", "id": "Panel Surya Terintegrasi Struktur Bangunan." },
    { "en": "Apa Itu 'Mono-Crystalline' (Mono-Kristalin)?", "id": "Sel Surya Dari Kristal Tunggal." },
    { "en": "Apa Itu 'Poly-Crystalline' (Poli-Kristalin)?", "id": "Sel Surya Dari Banyak Kristal." },
    { "en": "Apa Itu 'Thin Film' (Film Tipis)?", "id": "Sel Surya Lapisan Sangat Tipis." },
    { "en": "Apa Itu 'Inverter' (Pembalik Arus)?", "id": "Pengubah Arus Searah Menjadi Bolak-Balik." },
    { "en": "Apa Itu 'String Inverter' (Inverter String)?", "id": "Inverter Untuk Rangkaian Seri Panel." },
    { "en": "Apa Itu 'Micro Inverter' (Inverter Mikro)?", "id": "Inverter Khusus Untuk Satu Panel." },
    { "en": "Apa Itu 'Solar Tracker' (Pelacak Surya)?", "id": "Sistem Pengikut Arah Gerak Matahari." },
    { "en": "Apa Itu 'Irradiance' (Iradiansi Matahari)?", "id": "Kekuatan Radiasi Cahaya Per Meter." },
    { "en": "Apa Itu 'Insolation' (Insolasi Matahari)?", "id": "Jumlah Energi Matahari Diterima Permukaan." },
    { "en": "Apa Itu 'Performance Ratio' (Rasio Performa)?", "id": "Ukuran Efisiensi Riil Sistem Surya." },
    { "en": "Apa Itu 'BOS' (Balance Of System)?", "id": "Komponen Pendukung Selain Panel Surya." },
    { "en": "Apa Itu 'Charge Controller' (Pengontrol Isian)?", "id": "Pengatur Aliran Listrik Ke Baterai." },
    { "en": "Apa Itu 'Concentrated Solar' (Surya Terkonsentrasi)?", "id": "Pemanfaatan Cermin Pemusat Panas Matahari." },
    { "en": "Apa Itu 'Solar Thermal' (Termal Surya)?", "id": "Pengubahan Cahaya Menjadi Energi Panas." },
    { "en": "Apa Itu 'Floating PV' (Surya Terapung)?", "id": "Instalasi Panel Surya Di Perairan." },
    { "en": "Apa Itu 'Solar Farm' (Kebun Surya)?", "id": "Instalasi Panel Surya Skala Besar." },
    { "en": "Apa Itu 'HAWT' (Horizontal Axis Wind Turbine)?", "id": "Turbin Angin Poros Horisontal Umum." },
    { "en": "Apa Itu 'VAWT' (Vertical Axis Wind Turbine)?", "id": "Turbin Angin Poros Vertikal Khusus." },
    { "en": "Apa Itu 'Nacelle' (Nasel Turbin)?", "id": "Rumah Komponen Utama Di Atas Menara." },
    { "en": "Apa Itu 'Anemometer' (Alat Ukur Angin)?", "id": "Sensor Pengukur Kecepatan Angin Lokasi." },
    { "en": "Apa Itu 'Wind Vane' (Penunjuk Angin)?", "id": "Alat Penentu Arah Datangnya Angin." },
    { "en": "Apa Itu 'Cut-In Speed' (Kecepatan Masuk)?", "id": "Angin Minimal Untuk Memutar Turbin." },
    { "en": "Apa Itu 'Cut-Out Speed' (Kecepatan Keluar)?", "id": "Angin Maksimal Untuk Keamanan Turbin." },
    { "en": "Apa Itu 'Rated Speed' (Kecepatan Rating)?", "id": "Angin Untuk Daya Maksimal Turbin." },
    { "en": "Apa Itu 'Yaw System' (Sistem Yaw)?", "id": "Mekanisme Pengatur Arah Hadap Turbin." },
    { "en": "Apa Itu 'Pitch System' (Sistem Pitch)?", "id": "Mekanisme Pengatur Sudut Bilah Turbin." },
    { "en": "Apa Itu 'DFIG' (Doubly Fed Induction Generator)?", "id": "Generator Induksi Putaran Variabel Angin." },
    { "en": "Apa Itu 'Offshore Wind' (Angin Lepas Pantai)?", "id": "Pembangkit Angin Di Tengah Laut." },
    { "en": "Apa Itu 'Betz Limit' (Batas Betz)?", "id": "Efisiensi Teoritis Maksimal Turbin Angin." },
    { "en": "Apa Itu 'Geothermal' (Panas Bumi)?", "id": "Energi Panas Dari Dalam Bumi." },
    { "en": "Apa Itu 'Dry Steam' (Uap Kering)?", "id": "Pembangkit Panas Bumi Uap Langsung." },
    { "en": "Apa Itu 'Flash Steam' (Uap Flash)?", "id": "Pembangkit Air Panas Menjadi Uap." },
    { "en": "Apa Itu 'Binary Cycle' (Siklus Biner)?", "id": "Pembangkit Panas Bumi Cairan Menengah." },
    { "en": "Apa Itu 'Geothermal Reservoir' (Reservoir Panas)?", "id": "Sumber Panas Bawah Tanah Alami." },
    { "en": "Apa Itu 'Re-Injection Well' (Sumur Injeksi)?", "id": "Sumur Pengembalian Cairan Ke Bumi." },
    { "en": "Apa Itu 'Micro-Hydro' (Mikro Hidro)?", "id": "Pembangkit Listrik Air Skala Kecil." },
    { "en": "Apa Itu 'Head' (Tinggi Jatuh Air)?", "id": "Perbedaan Ketinggian Sumber Air Pembangkit." },
    { "en": "Apa Itu 'Penstock' (Pipa Pesat)?", "id": "Saluran Air Tekanan Tinggi Turbin." },
    { "en": "Apa Itu 'Kaplan Turbine' (Turbin Kaplan)?", "id": "Turbin Air Untuk Head Rendah." },
    { "en": "Apa Itu 'Pelton Turbine' (Turbin Pelton)?", "id": "Turbin Air Untuk Head Tinggi." },
    { "en": "Apa Itu 'Francis Turbine' (Turbin Francis)?", "id": "Turbin Air Untuk Head Menengah." },
    { "en": "Apa Itu 'Pumped Storage' (Penyimpanan Pompa)?", "id": "Penyimpanan Energi Melalui Cadangan Air." },
    { "en": "Apa Itu 'Run-Of-River' (Aliran Sungai)?", "id": "Pembangkit Tanpa Bendungan Penampung Besar." },
    { "en": "Apa Itu 'Biomass' (Biomassa)?", "id": "Bahan Organik Sumber Energi Terbarukan." },
    { "en": "Apa Itu 'Biogas' (Gas Bio)?", "id": "Gas Metana Dari Limbah Organik." },
    { "en": "Apa Itu 'Biofuel' (Bahan Bakar Bio)?", "id": "Bahan Bakar Cair Dari Tanaman." },
    { "en": "Apa Itu 'Gasification' (Gasifikasi)?", "id": "Proses Pengubahan Padatan Menjadi Gas." },
    { "en": "Apa Itu 'Pyrolysis' (Pirolisis)?", "id": "Dekomposisi Kimia Organik Melalui Panas." },
    { "en": "Apa Itu 'Anaerobic Digestion' (Pencernaan Anaerob)?", "id": "Penguraian Organik Tanpa Oksigen Bakteri." },
    { "en": "Apa Itu 'Tidal Energy' (Energi Pasang)?", "id": "Energi Dari Pasang Surut Laut." },
    { "en": "Apa Itu 'Wave Energy' (Energi Gelombang)?", "id": "Energi Dari Pergerakan Ombak Laut." },
    { "en": "Apa Itu 'OTEC' (Ocean Thermal Energy Conversion)?", "id": "Energi Dari Perbedaan Suhu Laut." },
    { "en": "Apa Itu 'Electrolyzer' (Elektroliser)?", "id": "Alat Pemisah Air Menjadi Hidrogen." },
    { "en": "Apa Itu 'Green Hydrogen' (Hidrogen Hijau)?", "id": "Hidrogen Dari Sumber Energi Terbarukan." },
    { "en": "Apa Itu 'Blue Hydrogen' (Hidrogen Biru)?", "id": "Hidrogen Gas Alam Dengan Karbon." },
    { "en": "Apa Itu 'Grey Hydrogen' (Hidrogen Abu)?", "id": "Hidrogen Dari Bahan Bakar Fosil." },
    { "en": "Apa Itu 'PEM' (Proton Exchange Membrane)?", "id": "Teknologi Sel Bahan Bakar Efisien." },
    { "en": "Apa Itu 'VPP' (Virtual Power Plant)?", "id": "Kumpulan Sumber Energi Terdistribusi Digital." },
    { "en": "Apa Itu 'Net Zero' (Emisi Nol)?", "id": "Keseimbangan Antara Emisi Dan Penyerapan." },
    { "en": "Apa Itu 'Carbon Footprint' (Jejak Karbon)?", "id": "Total Emisi Gas Rumah Kaca." },
    { "en": "Apa Itu 'Energy Audit' (Audit Energi)?", "id": "Evaluasi Konsumsi Energi Suatu Bangunan." },
    { "en": "Apa Itu 'LCOE' (Levelized Cost Of Energy)?", "id": "Biaya Rata-Rata Produksi Energi Listrik." },
    { "en": "Apa Itu 'Curtailment' (Pembatasan Produksi)?", "id": "Pengurangan Output Energi Akibat Berlebih." },
    { "en": "Apa Itu 'Grid Parity' (Paritas Jaringan)?", "id": "Harga Energi Terbarukan Sama Konvensional." },
    { "en": "Apa Itu 'Smart Meter' (Meteran Pintar)?", "id": "Alat Ukur Listrik Digital Komunikasi." },
    { "en": "Apa Itu 'Li-ion' (Lithium Ion Battery)?", "id": "Teknologi Baterai Isi Ulang Populer." },
    { "en": "Apa Itu 'Cycle Life' (Siklus Hidup Baterai)?", "id": "Jumlah Pengisian Maksimal Sebelum Rusak." },
    { "en": "Apa Itu 'SOC' (State Of Charge)?", "id": "Persentase Sisa Energi Dalam Baterai." },
    { "en": "Apa Itu 'DOD' (Depth Of Discharge)?", "id": "Persentase Kapasitas Baterai Yang Terpakai." },
    { "en": "Apa Itu 'Supercapacitor' (Superkapasitor)?", "id": "Penyimpan Energi Kapasitas Daya Tinggi." },
    { "en": "Apa Itu 'Flywheel' (Roda Gila)?", "id": "Penyimpan Energi Dalam Bentuk Putaran." },
    { "en": "Apa Itu 'CAES' (Compressed Air Energy Storage)?", "id": "Penyimpanan Energi Melalui Udara Bertekanan." },
    { "en": "Apa Itu 'Solar Cell' (Sel Surya)?", "id": "Perangkat Semikonduktor Pengumpul Foton Cahaya." },
    { "en": "Apa Itu 'Wafer' (Wafer Silikon)?", "id": "Bahan Dasar Pembuatan Sel Surya." },
    { "en": "Apa Itu 'Ingot' (Batangan Silikon)?", "id": "Bentuk Dasar Silikon Sebelum Diiris." },
    { "en": "Apa Itu 'EVA' (Ethylene Vinyl Acetate)?", "id": "Lapisan Perekat Pelindung Sel Surya." },
    { "en": "Apa Itu 'Backsheet' (Lembar Belakang)?", "id": "Lapisan Perlindungan Belakang Panel Surya." },
    { "en": "Apa Itu 'Junction Box' (Kotak Sambung Surya)?", "id": "Wadah Koneksi Listrik Belakang Panel." },
    { "en": "Apa Itu 'Busbar' (Rel Konduktor Sel)?", "id": "Jalur Pengumpul Arus Antar Sel." },
    { "en": "Apa Itu 'Efficiency' (Efisiensi Panel)?", "id": "Rasio Cahaya Menjadi Energi Listrik." },
    { "en": "Apa Itu 'Degradation' (Degradasi Panel)?", "id": "Penurunan Performa Panel Seiring Waktu." },
    { "en": "Apa Itu 'Azimuth' (Sudut Azimut)?", "id": "Arah Horizontal Orientasi Panel Surya." },
    { "en": "Apa Itu 'Tilt Angle' (Sudut Kemiringan)?", "id": "Sudut Tegak Lurus Panel Matahari." },
    { "en": "Apa Itu 'Solar Irradiance' (Iradiansi)?", "id": "Kerapatan Daya Cahaya Matahari Datang." },
    { "en": "Apa Itu 'VMPP' (Voltage Maximum Power Point)?", "id": "Tegangan Saat Daya Panel Maksimal." },
    { "en": "Apa Itu 'IMPP' (Current Maximum Power Point)?", "id": "Arus Saat Daya Panel Maksimal." },
    { "en": "Apa Itu 'VOC' (Open Circuit Voltage)?", "id": "Tegangan Maksimal Tanpa Ada Beban." },
    { "en": "Apa Itu 'ISC' (Short Circuit Current)?", "id": "Arus Maksimal Saat Hubung Singkat." },
    { "en": "Apa Itu 'Fill Factor' (Faktor Isi)?", "id": "Ukuran Kualitas Kinerja Sel Surya." },
    { "en": "Apa Itu 'BMS' (Building Management System)?", "id": "Sistem Kendali Fasilitas Gedung Terpusat." },
    { "en": "Apa Itu 'BAS' (Building Automation System)?", "id": "Otomasi Sistem Mekanikal Elektrikal Gedung." },
    { "en": "Apa Itu 'KNX' (Standar Protokol KNX)?", "id": "Protokol Otomasi Gedung Standar Global." },
    { "en": "Apa Itu 'DALI' (Digital Addressable Lighting Interface)?", "id": "Antarmuka Digital Khusus Kendali Lampu." },
    { "en": "Apa Itu 'BACnet' (Building Automation And Control Network)?", "id": "Protokol Komunikasi Standar Otomasi Gedung." },
    { "en": "Apa Itu 'LonWorks' (Local Operating Network)?", "id": "Platform Jaringan Kendali Perangkat Gedung." },
    { "en": "Apa Itu 'HVAC' (Heating Ventilation Air Conditioning)?", "id": "Sistem Pengatur Suhu Udara Gedung." },
    { "en": "Apa Itu 'Chiller' (Mesin Pendingin)?", "id": "Unit Pendingin Air Skala Besar." },
    { "en": "Apa Itu 'AHU' (Air Handling Unit)?", "id": "Unit Pengolah Aliran Udara Ruangan." },
    { "en": "Apa Itu 'VAV' (Variable Air Volume)?", "id": "Pengatur Volume Aliran Udara Variabel." },
    { "en": "Apa Itu 'FCU' (Fan Coil Unit)?", "id": "Unit Penukar Panas Lokal Ruangan." },
    { "en": "Apa Itu 'Damper' (Katup Udara)?", "id": "Alat Pengatur Laju Aliran Udara." },
    { "en": "Apa Itu 'VFD' (Variable Frequency Drive)?", "id": "Pengatur Kecepatan Motor Kipas Gedung." },
    { "en": "Apa Itu 'Smart Thermostat' (Termostat Cerdas)?", "id": "Pengatur Suhu Ruangan Otomatis Digital." },
    { "en": "Apa Itu 'Occupancy Sensor' (Sensor Keberadaan)?", "id": "Pendeteksi Kehadiran Manusia Dalam Ruang." },
    { "en": "Apa Itu 'Daylight Harvesting' (Pemanenan Cahaya)?", "id": "Optimasi Lampu Dengan Cahaya Alami." },
    { "en": "Apa Itu 'Lux Sensor' (Sensor Intensitas Cahaya)?", "id": "Pengukur Tingkat Kecerahan Cahaya Ruangan." },
    { "en": "Apa Itu 'CO2 Sensor' (Sensor Karbondioksida)?", "id": "Pengukur Kualitas Udara Dalam Gedung." },
    { "en": "Apa Itu 'Fire Alarm' (Sistem Alarm Kebakaran)?", "id": "Pendeteksi Dan Peringatan Bahaya Kebakaran." },
    { "en": "Apa Itu 'Smoke Detector' (Pendeteksi Asap)?", "id": "Sensor Pemicu Alarm Akibat Asap." },
    { "en": "Apa Itu 'Heat Detector' (Pendeteksi Panas)?", "id": "Sensor Pemicu Alarm Akibat Panas." },
    { "en": "Apa Itu 'Access Control' (Kontrol Akses)?", "id": "Sistem Pembatas Pintu Masuk Orang." },
    { "en": "Apa Itu 'CCTV' (Closed Circuit Television)?", "id": "Sistem Pemantauan Video Keamanan Gedung." },
    { "en": "Apa Itu 'DVR' (Digital Video Recorder)?", "id": "Alat Perekam Video Kamera Analog." },
    { "en": "Apa Itu 'NVR' (Network Video Recorder)?", "id": "Alat Perekam Video Kamera Digital." },
    { "en": "Apa Itu 'IP Camera' (Kamera Protokol Internet)?", "id": "Kamera Digital Pengirim Data Jaringan." },
    { "en": "Apa Itu 'Video Intercom' (Interkom Video)?", "id": "Sistem Komunikasi Suara Visual Pintu." },
    { "en": "Apa Itu 'Power Meter' (Meteran Daya Digital)?", "id": "Alat Pantau Konsumsi Energi Gedung." },
    { "en": "Apa Itu 'Load Profiling' (Pembuatan Profil Beban)?", "id": "Analisis Pola Penggunaan Listrik Gedung." },
    { "en": "Apa Itu 'Demand Response' (Respon Permintaan)?", "id": "Penyesuaian Beban Saat Beban Puncak." },
    { "en": "Apa Itu 'Energy Dashboard' (Dasbor Energi)?", "id": "Tampilan Visual Konsumsi Energi Real-Time." },
    { "en": "Apa Itu 'Smart Glass' (Kaca Cerdas)?", "id": "Kaca Berubah Warna Sesuai Cahaya." },
    { "en": "Apa Itu 'Elevator Controller' (Kontroler Lift)?", "id": "Sistem Kendali Pergerakan Kabin Lift." },
    { "en": "Apa Itu 'Escalator' (Tangga Berjalan)?", "id": "Sistem Transportasi Vertikal Lantai Gedung." },
    { "en": "Apa Itu 'Building Envelope' (Selubung Gedung)?", "id": "Struktur Fisik Pemisah Dalam Luar." },
    { "en": "Apa Itu 'Sub-Metering' (Meteran Cabang)?", "id": "Pengukuran Listrik Tiap Lantai Tenant." },
    { "en": "Apa Itu 'Integrated Security' (Keamanan Terpadu)?", "id": "Gabungan Akses, Alarm, Dan Kamera." },
    { "en": "Apa Itu 'Remote Monitoring' (Pemantauan Jarak Jauh)?", "id": "Pengawasan Sistem Dari Luar Gedung." },
    { "en": "Apa Itu 'Maintenance Management' (Manajemen Pemeliharaan)?", "id": "Jadwal Perawatan Fasilitas Gedung Rutin." },
    { "en": "Apa Itu 'Net Zero Building' (Gedung Emisi Nol)?", "id": "Gedung Dengan Produksi Energi Mandiri." },
    { "en": "Apa Itu 'NYA Cable' (Kabel Tembaga Tunggal)?", "id": "Kabel Isolasi PVC Inti Tunggal." },
    { "en": "Apa Itu 'NYM Cable' (Kabel Tembaga Ganda)?", "id": "Kabel Isolasi PVC Inti Banyak." },
    { "en": "Apa Itu 'NYY Cable' (Kabel Tanah)?", "id": "Kabel Kuat Untuk Pemasangan Tanam." },
    { "en": "Apa Itu 'NYAF Cable' (Kabel Serabut)?", "id": "Kabel Fleksibel Untuk Pemasangan Panel." },
    { "en": "Apa Itu 'XLPE' (Cross-Linked Polyethylene)?", "id": "Bahan Isolasi Kabel Tegangan Tinggi." },
    { "en": "Apa Itu 'PVC' (Polyvinyl Chloride)?", "id": "Bahan Isolasi Kabel Paling Umum." },
    { "en": "Apa Itu 'EPR' (Ethylene Propylene Rubber)?", "id": "Karet Isolasi Tahan Panas Tinggi." },
    { "en": "Apa Itu 'Conductor' (Konduktor)?", "id": "Logam Penghantar Aliran Arus Listrik." },
    { "en": "Apa Itu 'Insulation' (Isolasi)?", "id": "Lapisan Pelindung Kebocoran Arus Listrik." },
    { "en": "Apa Itu 'Sheath' (Selubung Luar)?", "id": "Pelindung Mekanik Paling Luar Kabel." },
    { "en": "Apa Itu 'Armouring' (Perisai Baja)?", "id": "Lapisan Logam Pelindung Benturan Fisik." },
    { "en": "Apa Itu 'SWA' (Steel Wire Armour)?", "id": "Kabel Berperisai Kawat Baja Kuat." },
    { "en": "Apa Itu 'AWA' (Aluminium Wire Armour)?", "id": "Kabel Berperisai Kawat Aluminium Ringan." },
    { "en": "Apa Itu 'Screening' (Tabir Logam)?", "id": "Pelindung Kabel Dari Gangguan Elektromagnetik." },
    { "en": "Apa Itu 'Twisted Pair' (Pasangan Berpilin)?", "id": "Dua Kabel Berpilin Pengurang Derau." },
    { "en": "Apa Itu 'Coaxial Cable' (Kabel Koaksial)?", "id": "Kabel Transmisi Sinyal Frekuensi Tinggi." },
    { "en": "Apa Itu 'Fiber Optic' (Kabel Serat Optik)?", "id": "Media Transmisi Melalui Pulsa Cahaya." },
    { "en": "Apa Itu 'Cat 6' (Category 6 Cable)?", "id": "Kabel Jaringan Data Kecepatan Tinggi." },
    { "en": "Apa Itu 'Bus Cable' (Kabel Bus)?", "id": "Kabel Khusus Protokol Komunikasi Data." },
    { "en": "Apa Itu 'Fire Resistant Cable' (Kabel Tahan Api)?", "id": "Kabel Tetap Berfungsi Saat Terbakar." },
    { "en": "Apa Itu 'Flame Retardant Cable' (Kabel Penghambat Api)?", "id": "Kabel Tidak Menyebarkan Rambatan Api." },
    { "en": "Apa Itu 'LSZH' (Low Smoke Zero Halogen)?", "id": "Kabel Rendah Asap Tanpa Halogen." },
    { "en": "Apa Itu 'LSHF' (Low Smoke Halogen Free)?", "id": "Kabel Aman Saat Terjadi Kebakaran." },
    { "en": "Apa Itu 'Control Cable' (Kabel Kontrol)?", "id": "Kabel Untuk Sinyal Perintah Peralatan." },
    { "en": "Apa Itu 'Instrumentation Cable' (Kabel Instrumentasi)?", "id": "Kabel Sinyal Analog Presisi Tinggi." },
    { "en": "Apa Itu 'Thermocouple Extension' (Kabel Kompensasi)?", "id": "Kabel Penghubung Sensor Suhu Akurat." },
    { "en": "Apa Itu 'Ribbon Cable' (Kabel Pita)?", "id": "Kabel Datar Untuk Koneksi Internal." },
    { "en": "Apa Itu 'Jumper Wire' (Kawat Jumper)?", "id": "Kabel Pendek Penghubung Titik Rangkaian." },
    { "en": "Apa Itu 'Enamelled Wire' (Kawat Email)?", "id": "Kawat Tembaga Tipis Lilitan Motor." },
    { "en": "Apa Itu 'Magnet Wire' (Kawat Magnet)?", "id": "Kawat Untuk Pembuatan Kumparan Elektromagnetik." },
    { "en": "Apa Itu 'Bus Duct' (Saluran Bus)?", "id": "Sistem Distribusi Arus Besar Batangan." },
    { "en": "Apa Itu 'Cable Tray' (Baki Kabel)?", "id": "Struktur Penyangga Jalur Kabel Terbuka." },
    { "en": "Apa Itu 'Cable Ladder' (Tangga Kabel)?", "id": "Penyangga Kabel Berat Bentuk Tangga." },
    { "en": "Apa Itu 'Conduit' (Pipa Kabel)?", "id": "Pelindung Kabel Dalam Dinding Lantai." },
    { "en": "Apa Itu 'Cable Gland' (Kelen Kabel)?", "id": "Alat Pengikat Kabel Pada Panel." },
    { "en": "Apa Itu 'Cable Tie' (Pengikat Kabel)?", "id": "Tali Plastik Pengatur Kerapian Kabel." },
    { "en": "Apa Itu 'Heat Shrink' (Selongsong Bakar)?", "id": "Isolasi Sambungan Kabel Melalui Pemanasan." },
    { "en": "Apa Itu 'Terminal Lug' (Sepatu Kabel)?", "id": "Penyambung Ujung Kabel Ke Terminal." },
    { "en": "Apa Itu 'Wire Nut' (Mur Sambung)?", "id": "Konektor Sambungan Putar Kabel Rumah." },
    { "en": "Apa Itu 'Ferrules' (Ferul Ujung)?", "id": "Pelindung Ujung Kabel Serabut Halus." },
    { "en": "Apa Itu 'Voltage Rating' (Rating Tegangan)?", "id": "Batas Tegangan Operasi Aman Kabel." },
    { "en": "Apa Itu 'Current Carrying Capacity' (KHA)?", "id": "Arus Maksimal Yang Dapat Dialirkan." },
    { "en": "Apa Itu 'Voltage Drop' (Jatuh Tegangan)?", "id": "Penurunan Tegangan Sepanjang Jalur Kabel." },
    { "en": "Apa Itu 'Short Circuit Rating' (Rating Hubung Singkat)?", "id": "Ketahanan Kabel Terhadap Arus Gangguan." },
    { "en": "Apa Itu 'Bending Radius' (Radius Tekuk)?", "id": "Batas Kelengkungan Maksimal Tekukan Kabel." },
    { "en": "Apa Itu 'Pulling Tension' (Tegangan Tarik)?", "id": "Batas Kekuatan Tarik Saat Instalasi." },
    { "en": "Apa Itu 'Dielectric Constant' (Konstanta Dielektrik)?", "id": "Sifat Penyimpanan Medan Listrik Isolasi." },
    { "en": "Apa Itu 'Resistivity' (Hambatan Jenis)?", "id": "Kemampuan Bahan Menahan Arus Listrik." },
    { "en": "Apa Itu 'Conductivity' (Konduktivitas)?", "id": "Kemampuan Bahan Menghantarkan Arus Listrik." },
    { "en": "Apa Itu 'Skin Effect' (Efek Kulit)?", "id": "Arus Mengalir Hanya Permukaan Konduktor." },
    { "en": "Apa Itu 'Proximity Effect' (Efek Kedekatan)?", "id": "Interferensi Arus Antar Kabel Berdekatan." },
    { "en": "Apa Itu 'Harmonics Derating' (Penurunan Rating Harmonisa)?", "id": "Penyesuaian Kapasitas Akibat Gelombang Kotor." },
    { "en": "Apa Itu 'Fill Ratio' (Rasio Isi)?", "id": "Kepadatan Kabel Dalam Satu Pipa." },
    { "en": "Apa Itu 'AWG' (American Wire Gauge)?", "id": "Standar Ukuran Diameter Kawat Amerika." },
    { "en": "Apa Itu 'SWG' (Standard Wire Gauge)?", "id": "Standar Ukuran Diameter Kawat Inggris." },
    { "en": "Apa Itu 'Cross Section Area' (Luas Penampang)?", "id": "Ukuran Besar Konduktor Dalam Milimeter." },
    { "en": "Apa Itu 'Standard IEC 60502' (Standar Kabel)?", "id": "Aturan Internasional Konstruksi Kabel Daya." },
    { "en": "Apa Itu 'Copper' (Tembaga)?", "id": "Konduktor Listrik Dengan Konduktivitas Tinggi." },
    { "en": "Apa Itu 'Aluminium' (Aluminium)?", "id": "Konduktor Listrik Ringan Dan Ekonomis." },
    { "en": "Apa Itu 'Luminous Flux' (Fluks Cahaya)?", "id": "Total Cahaya Keluar Dari Sumber." },
    { "en": "Apa Itu 'Lumen' (Satuan Fluks Cahaya)?", "id": "Satuan Ukur Total Output Cahaya." },
    { "en": "Apa Itu 'Luminous Intensity' (Intensitas Cahaya)?", "id": "Kekuatan Cahaya Ke Arah Tertentu." },
    { "en": "Apa Itu 'Candela' (Satuan Intensitas Cahaya)?", "id": "Satuan Dasar Kekuatan Cahaya Internasional." },
    { "en": "Apa Itu 'Illuminance' (Iluminansi)?", "id": "Kerapatan Fluks Cahaya Pada Permukaan." },
    { "en": "Apa Itu 'Lux' (Satuan Iluminansi)?", "id": "Satuan Kecerahan Cahaya Per Meter." },
    { "en": "Apa Itu 'Luminance' (Luminansi)?", "id": "Kecerahan Cahaya Dari Suatu Luasan." },
    { "en": "Apa Itu 'Nit' (Satuan Luminansi)?", "id": "Satuan Kecerahan Permukaan Per Meter." },
    { "en": "Apa Itu 'Efficacy' (Efikasi Lampu)?", "id": "Rasio Output Lumen Terhadap Watt." },
    { "en": "Apa Itu 'CRI' (Color Rendering Index)?", "id": "Indeks Akurasi Penampilan Warna Alami." },
    { "en": "Apa Itu 'CCT' (Correlated Color Temperature)?", "id": "Warna Cahaya Dalam Skala Kelvin." },
    { "en": "Apa Itu 'Warm White' (Putih Hangat)?", "id": "Cahaya Warna Kuning Suhu Rendah." },
    { "en": "Apa Itu 'Cool White' (Putih Dingin)?", "id": "Cahaya Warna Putih Sedikit Kebiruan." },
    { "en": "Apa Itu 'Daylight' (Cahaya Siang)?", "id": "Cahaya Terang Menyerupai Matahari Siang." },
    { "en": "Apa Itu 'Glare' (Kesilauan)?", "id": "Gangguan Penglihatan Akibat Cahaya Berlebih." },
    { "en": "Apa Itu 'UGR' (Unified Glare Rating)?", "id": "Indeks Penilaian Tingkat Kesilauan Ruangan." },
    { "en": "Apa Itu 'Ballast' (Balas Lampu)?", "id": "Alat Pengatur Arus Lampu Tabung." },
    { "en": "Apa Itu 'Electronic Ballast' (Balas Elektronik)?", "id": "Pengatur Arus Efisien Frekuensi Tinggi." },
    { "en": "Apa Itu 'LED Driver' (Driver LED)?", "id": "Penyuplai Arus Konstan Chip LED." },
    { "en": "Apa Itu 'HID' (High Intensity Discharge)?", "id": "Lampu Pelepasan Gas Intensitas Tinggi." },
    { "en": "Apa Itu 'HPS' (High Pressure Sodium)?", "id": "Lampu Uap Natrium Tekanan Tinggi." },
    { "en": "Apa Itu 'LPS' (Low Pressure Sodium)?", "id": "Lampu Uap Natrium Tekanan Rendah." },
    { "en": "Apa Itu 'Metal Halide' (Halida Logam)?", "id": "Lampu Pelepasan Gas Spektrum Putih." },
    { "en": "Apa Itu 'Mercury Vapor' (Uap Merkuri)?", "id": "Lampu Pelepasan Gas Berisi Merkuri." },
    { "en": "Apa Itu 'Fluorescent Lamp' (Lampu Fluoresen)?", "id": "Lampu Tabung Berisi Gas Merkuri." },
    { "en": "Apa Itu 'CFL' (Compact Fluorescent Lamp)?", "id": "Lampu Fluoresen Bentuk Kecil Kompak." },
    { "en": "Apa Itu 'Halogen Lamp' (Lampu Halogen)?", "id": "Lampu Pijar Berisi Gas Halogen." },
    { "en": "Apa Itu 'Incandescent' (Lampu Pijar)?", "id": "Lampu Dengan Filamen Tungsten Panas." },
    { "en": "Apa Itu 'OLED' (Organic Light Emitting Diode)?", "id": "Dioda Pemancar Cahaya Lapisan Organik." },
    { "en": "Apa Itu 'Luminous Efficiency' (Efisiensi Cahaya)?", "id": "Rasio Daya Tampak Terhadap Total." },
    { "en": "Apa Itu 'Maintenance Factor' (Faktor Pemeliharaan)?", "id": "Rasio Penurunan Cahaya Akibat Usia." },
    { "en": "Apa Itu 'Utilization Factor' (Faktor Utilisasi)?", "id": "Rasio Cahaya Mencapai Bidang Kerja." },
    { "en": "Apa Itu 'Reflection' (Pemantulan)?", "id": "Proses Pembalikan Cahaya Oleh Permukaan." },
    { "en": "Apa Itu 'Refraction' (Pembiasan)?", "id": "Perubahan Arah Cahaya Melalui Medium." },
    { "en": "Apa Itu 'Diffuser' (Difuser)?", "id": "Alat Penyebar Cahaya Menjadi Lembut." },
    { "en": "Apa Itu 'Reflector' (Reflektor)?", "id": "Alat Pemantul Cahaya Ke Arah." },
    { "en": "Apa Itu 'Louver' (Kisi-Kisi)?", "id": "Penghalang Cahaya Pencegah Silau Langsung." },
    { "en": "Apa Itu 'Beam Angle' (Sudut Berkas)?", "id": "Sudut Penyebaran Cahaya Utama Lampu." },
    { "en": "Apa Itu 'Candlepower' (Daya Lilin)?", "id": "Ukuran Intensitas Cahaya Satuan Lama." },
    { "en": "Apa Itu 'Lux Meter' (Alat Ukur Lux)?", "id": "Alat Ukur Tingkat Pencahayaan Ruang." },
    { "en": "Apa Itu 'Goniophotometer' (Goniofotometer)?", "id": "Alat Ukur Distribusi Cahaya Sumber." },
    { "en": "Apa Itu 'Integrating Sphere' (Bola Integrasi)?", "id": "Alat Ukur Total Lumen Lampu." },
    { "en": "Apa Itu 'Photometry' (Fotometri)?", "id": "Ilmu Pengukuran Cahaya Tampak Mata." },
    { "en": "Apa Itu 'Radiometry' (Radiometri)?", "id": "Ilmu Pengukuran Energi Radiasi Elektromagnetik." },
    { "en": "Apa Itu 'Inverse Square Law' (Hukum Kuadrat Terbalik)?", "id": "Cahaya Berkurang Terhadap Kuadrat Jarak." },
    { "en": "Apa Itu 'Lambert's Cosine Law' (Hukum Kosinus)?", "id": "Iluminansi Tergantung Sudut Datang Cahaya." },
    { "en": "Apa Itu 'Uniformity Ratio' (Rasio Keseragaman)?", "id": "Perbandingan Iluminansi Minimum Dan Rata-Rata." },
    { "en": "Apa Itu 'Point Method' (Metode Titik)?", "id": "Perhitungan Iluminansi Pada Titik Spesifik." },
    { "en": "Apa Itu 'Lumen Method' (Metode Lumen)?", "id": "Perhitungan Jumlah Lampu Rata-Rata Ruangan." },
    { "en": "Apa Itu 'Ambient Lighting' (Pencahayaan Ambien)?", "id": "Pencahayaan Umum Seluruh Area Ruangan." },
    { "en": "Apa Itu 'Task Lighting' (Pencahayaan Tugas)?", "id": "Pencahayaan Khusus Untuk Pekerjaan Tertentu." },
    { "en": "Apa Itu 'Accent Lighting' (Pencahayaan Aksen)?", "id": "Cahaya Penonjol Objek Estetika Tertentu." },
    { "en": "Apa Itu 'Direct Lighting' (Pencahayaan Langsung)?", "id": "Cahaya Langsung Menuju Bidang Kerja." },
    { "en": "Apa Itu 'Indirect Lighting' (Pencahayaan Tidak Langsung)?", "id": "Cahaya Memantul Dinding Sebelum Objek." },
    { "en": "Apa Itu 'Emergency Lighting' (Pencahayaan Darurat)?", "id": "Lampu Aktif Saat Listrik Utama." },
    { "en": "Apa Itu 'Escaping Route Lighting' (Lampu Evakuasi)?", "id": "Pencahayaan Penunjuk Jalan Keluar Darurat." },
    { "en": "Apa Itu 'Stroboscopic Effect' (Efek Stroboskopik)?", "id": "Ilusi Gerak Akibat Kedipan Cahaya." },
    { "en": "Apa Itu 'Flicker' (Kedipan)?", "id": "Variasi Cepat Intensitas Cahaya Lampu." },
    { "en": "Apa Itu 'Ignitor' (Pematik)?", "id": "Pemicu Tegangan Tinggi Lampu Gas." },
    { "en": "Apa Itu 'Capacitor' (Kapasitor Lampu)?", "id": "Komponen Perbaikan Faktor Daya Lampu." },
    { "en": "Apa Itu 'Starter' (Starter Fluoresen)?", "id": "Alat Bantu Pemanasan Elektroda Lampu." },
    { "en": "Apa Itu 'T5 Tube' (Tabung T5)?", "id": "Lampu Fluoresen Diameter Lima Per-Delapan." },
    { "en": "Apa Itu 'T8 Tube' (Tabung T8)?", "id": "Lampu Fluoresen Diameter Delapan Per-Delapan." },
    { "en": "Apa Itu 'Heat Sink' (Pendingin LED)?", "id": "Media Pembuang Panas Chip LED." },
    { "en": "Apa Itu 'Binning' (Klasifikasi LED)?", "id": "Pengelompokan LED Berdasarkan Performa Warna." },
    { "en": "Apa Itu 'L70 Rating' (Rating L70)?", "id": "Waktu Cahaya Turun Tiga Puluh." },
    { "en": "Apa Itu 'MacAdam Ellipse' (Elips MacAdam)?", "id": "Ukuran Konsistensi Warna Cahaya LED." },
    { "en": "Apa Itu 'Smart Lighting' (Pencahayaan Cerdas)?", "id": "Sistem Lampu Terkendali Otomatis Digital." },
    { "en": "Apa Itu 'Occupancy Sensor' (Sensor Keberadaan)?", "id": "Otomasi Lampu Berdasarkan Gerakan Manusia." },
    { "en": "Apa Itu 'Photocell' (Sel Foto)?", "id": "Otomasi Lampu Berdasarkan Cahaya Sekitar." },
    { "en": "Apa Itu 'Dimmable' (Bisa Redup)?", "id": "Lampu Dengan Fitur Pengaturan Intensitas." },
    { "en": "Apa Itu 'DALI' (Digital Addressable Lighting Interface)?", "id": "Protokol Komunikasi Kendali Lampu Digital." },
    { "en": "Apa Itu 'DMX512' (Digital Multiplex 512)?", "id": "Standar Kendali Lampu Panggung Hiburan." },
    { "en": "Apa Itu 'IP Rating' (Ingress Protection)?", "id": "Tingkat Perlindungan Lampu Terhadap Lingkungan." },
    { "en": "Apa Itu 'Floodlight' (Lampu Sorot)?", "id": "Lampu Pencahayaan Luar Area Luas." },
    { "en": "Apa Itu 'High Bay' (Teluk Tinggi)?", "id": "Lampu Untuk Langit-Langit Sangat Tinggi." },
    { "en": "Apa Itu 'Low Bay' (Teluk Rendah)?", "id": "Lampu Untuk Langit-Langit Ketinggian Sedang." },
    { "en": "Apa Itu 'Troffer' (Lampu Troffer)?", "id": "Rumah Lampu Untuk Plafon Kotak." },
    { "en": "Apa Itu 'Downlight' (Lampu Bawah)?", "id": "Lampu Terpasang Masuk Ke Plafon." },
    { "en": "Apa Itu 'Street Lighting' (Lampu Jalan)?", "id": "Pencahayaan Khusus Jalur Kendaraan Bermotor." },
    { "en": "Apa Itu 'Wall Washer' (Pencuci Dinding)?", "id": "Teknik Pencahayaan Merata Pada Dinding." },
    { "en": "Apa Itu 'Color Spectrum' (Spektrum Warna)?", "id": "Kumpulan Panjang Gelombang Cahaya Tampak." },
    { "en": "Apa Itu 'Ultraviolet' (Sinar Ultraungu)?", "id": "Radiasi Gelombang Pendek Diatas Cahaya." },
    { "en": "Apa Itu 'Infrared' (Sinar Inframerah)?", "id": "Radiasi Gelombang Panjang Dibawah Cahaya." },
    { "en": "Apa Itu 'Photon' (Foton)?", "id": "Partikel Dasar Pembawa Radiasi Elektromagnetik." },
    { "en": "Apa Itu 'Visible Light' (Cahaya Tampak)?", "id": "Radiasi Yang Dapat Dilihat Mata." },
    { "en": "Apa Itu 'Optical Fiber' (Serat Optik)?", "id": "Saluran Transmisi Data Melalui Cahaya." },
    { "en": "Apa Itu 'Refractive Index' (Indeks Bias)?", "id": "Ukuran Kecepatan Cahaya Dalam Medium." },
    { "en": "Apa Itu 'Photodiode' (Fotodioda)?", "id": "Dioda Pengubah Cahaya Menjadi Arus." },
    { "en": "Apa Itu 'Photoresistor' (Fotoresistor)?", "id": "Resistor Hambatan Berubah Sesuai Cahaya." },
    { "en": "Apa Itu 'LDR' (Light Dependent Resistor)?", "id": "Resistor Peka Terhadap Intensitas Cahaya." },
    { "en": "Apa Itu 'Solar Cell' (Sel Surya)?", "id": "Pengubah Cahaya Matahari Menjadi Listrik." },
    { "en": "Apa Itu 'Photovoltaic Effect' (Efek Fotovoltaik)?", "id": "Pembangkitan Tegangan Akibat Paparan Cahaya." },
    { "en": "Apa Itu 'Spectral Sensitivity' (Sensitivitas Spektral)?", "id": "Respon Alat Ukur Terhadap Warna." },
    { "en": "Apa Itu 'Standard Observer' (Pengamat Standar)?", "id": "Model Sensitivitas Mata Manusia Rata-Rata." },
    { "en": "Apa Itu 'Foveal Vision' (Penglihatan Foveal)?", "id": "Penglihatan Tajam Pusat Fokus Mata." },
    { "en": "Apa Itu 'Peripheral Vision' (Penglihatan Tepi)?", "id": "Penglihatan Area Sekitar Pusat Fokus." },
    { "en": "Apa Itu 'EV' (Electric Vehicle)?", "id": "Kendaraan Dengan Penggerak Motor Listrik." },
    { "en": "Apa Itu 'BEV' (Battery Electric Vehicle)?", "id": "Kendaraan Listrik Bertenaga Baterai Murni." },
    { "en": "Apa Itu 'PHEV' (Plug-in Hybrid Electric Vehicle)?", "id": "Kendaraan Hibrida Yang Bisa Diisi." },
    { "en": "Apa Itu 'HEV' (Hybrid Electric Vehicle)?", "id": "Kendaraan Gabungan Bensin Dan Listrik." },
    { "en": "Apa Itu 'FCEV' (Fuel Cell Electric Vehicle)?", "id": "Kendaraan Listrik Berbahan Bakar Hidrogen." },
    { "en": "Apa Itu 'BMS' (Battery Management System)?", "id": "Pengendali Dan Pelindung Sel Baterai." },
    { "en": "Apa Itu 'SOC' (State Of Charge)?", "id": "Persentase Sisa Kapasitas Energi Baterai." },
    { "en": "Apa Itu 'SOH' (State Of Health)?", "id": "Kondisi Kesehatan Dan Umur Baterai." },
    { "en": "Apa Itu 'Regenerative Braking' (Pengereman Regeneratif)?", "id": "Pengisian Baterai Saat Melakukan Pengereman." },
    { "en": "Apa Itu 'Inverter' (Pembalik Arus Kendaraan)?", "id": "Pengubah Arus Searah Menjadi Bolak-Balik." },
    { "en": "Apa Itu 'Converter' (Konverter Tegangan Searah)?", "id": "Penyesuai Level Tegangan Searah Kendaraan." },
    { "en": "Apa Itu 'OBC' (On-Board Charger)?", "id": "Alat Pengisi Baterai Internal Kendaraan." },
    { "en": "Apa Itu 'EVSE' (Electric Vehicle Supply Equipment)?", "id": "Infrastruktur Pengisian Daya Listrik Eksternal." },
    { "en": "Apa Itu 'Level 1 Charging' (Pengisian Level 1)?", "id": "Pengisian Lambat Melalui Stopkontak Rumah." },
    { "en": "Apa Itu 'Level 2 Charging' (Pengisian Level 2)?", "id": "Pengisian Cepat Melalui Arus Bolak-Balik." },
    { "en": "Apa Itu 'DC Fast Charging' (Pengisian Cepat DC)?", "id": "Pengisian Sangat Cepat Arus Searah." },
    { "en": "Apa Itu 'CHAdeMO' (Standar CHAdeMO)?", "id": "Protokol Pengisian Cepat Asal Jepang." },
    { "en": "Apa Itu 'CCS' (Combined Charging System)?", "id": "Standar Pengisian Gabungan AC, DC." },
    { "en": "Apa Itu 'Type 1 Connector' (Konektor Tipe 1)?", "id": "Soket Pengisian Standar Amerika Utara." },
    { "en": "Apa Itu 'Type 2 Connector' (Konektor Tipe 2)?", "id": "Soket Pengisian Standar Wilayah Eropa." },
    { "en": "Apa Itu 'Tesla NACS' (North American Charging Standard)?", "id": "Standar Konektor Pengisian Milik Tesla." },
    { "en": "Apa Itu 'V2G' (Vehicle To Grid)?", "id": "Penyaluran Energi Mobil Ke Jaringan." },
    { "en": "Apa Itu 'V2H' (Vehicle To Home)?", "id": "Penyaluran Energi Mobil Ke Rumah." },
    { "en": "Apa Itu 'V2L' (Vehicle To Load)?", "id": "Penggunaan Baterai Mobil Untuk Peralatan." },
    { "en": "Apa Itu 'Range Anxiety' (Kecemasan Jarak)?", "id": "Ketakutan Baterai Habis Sebelum Sampai." },
    { "en": "Apa Itu 'Energy Density' (Kerapatan Energi)?", "id": "Jumlah Energi Per Berat Baterai." },
    { "en": "Apa Itu 'Power Density' (Kerapatan Daya)?", "id": "Kecepatan Pengeluaran Energi Per Berat." },
    { "en": "Apa Itu 'LiFePO4' (Lithium Iron Phosphate)?", "id": "Baterai Litium Dengan Keamanan Tinggi." },
    { "en": "Apa Itu 'NMC' (Nickel Manganese Cobalt)?", "id": "Baterai Litium Dengan Kerapatan Tinggi." },
    { "en": "Apa Itu 'Solid State Battery' (Baterai Padat)?", "id": "Teknologi Baterai Elektrolit Padat Aman." },
    { "en": "Apa Itu 'Thermal Management' (Manajemen Termal)?", "id": "Pengaturan Suhu Baterai Dan Motor." },
    { "en": "Apa Itu 'Liquid Cooling' (Pendinginan Cairan)?", "id": "Sistem Pembuang Panas Menggunakan Cairan." },
    { "en": "Apa Itu 'Air Cooling' (Pendinginan Udara)?", "id": "Sistem Pembuang Panas Menggunakan Udara." },
    { "en": "Apa Itu 'Traction Motor' (Motor Traksi)?", "id": "Motor Utama Penggerak Roda Kendaraan." },
    { "en": "Apa Itu 'Induction Motor' (Motor Induksi)?", "id": "Motor Arus Bolak-Balik Tanpa Magnet." },
    { "en": "Apa Itu 'PMSM' (Permanent Magnet Synchronous Motor)?", "id": "Motor Sinkron Magnet Efisiensi Tinggi." },
    { "en": "Apa Itu 'Axial Flux Motor' (Motor Fluks Aksial)?", "id": "Motor Bentuk Cakram Torsi Tinggi." },
    { "en": "Apa Itu 'Radial Flux Motor' (Motor Fluks Radial)?", "id": "Motor Bentuk Silinder Paling Umum." },
    { "en": "Apa Itu 'Single Speed Transmission' (Transmisi Gigi Tunggal)?", "id": "Sistem Penyalur Daya Tanpa Perpindahan." },
    { "en": "Apa Itu 'Torque Vectoring' (Vektorisasi Torsi)?", "id": "Pengaturan Daya Tiap Roda Mandiri." },
    { "en": "Apa Itu 'Drive By Wire' (Kendali Melalui Kabel)?", "id": "Sistem Kendali Elektronik Tanpa Mekanik." },
    { "en": "Apa Itu 'ADAS' (Advanced Driver Assistance Systems)?", "id": "Sistem Elektronik Pembantu Keamanan Berkendara." },
    { "en": "Apa Itu 'Autonomous Driving' (Mengemudi Otonom)?", "id": "Kemampuan Kendaraan Berjalan Tanpa Supir." },
    { "en": "Apa Itu 'Telematics' (Telematika Kendaraan)?", "id": "Pengiriman Data Jarak Jauh Kendaraan." },
    { "en": "Apa Itu 'OTA' (Over The Air Update)?", "id": "Pembaruan Perangkat Lunak Secara Nirkabel." },
    { "en": "Apa Itu 'C-Rate' (Laju-C Baterai)?", "id": "Ukuran Kecepatan Pengisian Atau Pengosongan." },
    { "en": "Apa Itu 'Cell Balancing' (Penyeimbangan Sel)?", "id": "Pemerataan Tegangan Antar Sel Baterai." },
    { "en": "Apa Itu 'Prismatic Cell' (Sel Prismatik)?", "id": "Sel Baterai Berbentuk Kotak Persegi." },
    { "en": "Apa Itu 'Cylindrical Cell' (Sel Silinder)?", "id": "Sel Baterai Berbentuk Tabung Kecil." },
    { "en": "Apa Itu 'Pouch Cell' (Sel Kantong)?", "id": "Sel Baterai Berbentuk Kantong Fleksibel." },
    { "en": "Apa Itu 'Battery Pack' (Paket Baterai)?", "id": "Kumpulan Modul Baterai Terintegrasi Kendaraan." },
    { "en": "Apa Itu 'Thermal Runaway' (Kegagalan Termal)?", "id": "Reaksi Berantai Panas Berbahaya Baterai." },
    { "en": "Apa Itu 'Fire Suppression' (Pemadam Api Baterai)?", "id": "Sistem Pemadam Kebakaran Khusus Litium." },
    { "en": "Apa Itu 'Electric Traction' (Traksi Listrik)?", "id": "Sistem Penggerak Kendaraan Tenaga Listrik." },
    { "en": "Apa Itu 'Pantograph' (Pantograf)?", "id": "Alat Pengambil Listrik Kereta Api." },
    { "en": "Apa Itu 'Catenary' (Katenari)?", "id": "Kabel Listrik Di Atas Jalur." },
    { "en": "Apa Itu 'Third Rail' (Rel Ketiga)?", "id": "Rel Pengalir Arus Listrik Kereta." },
    { "en": "Apa Itu 'Substation' (Gardu Traksi)?", "id": "Gardu Pengubah Tegangan Jalur Kereta." },
    { "en": "Apa Itu 'AC Traction' (Traksi Bolak-Balik)?", "id": "Sistem Penggerak Kereta Arus Bolak-Balik." },
    { "en": "Apa Itu 'DC Traction' (Traksi Searah)?", "id": "Sistem Penggerak Kereta Arus Searah." },
    { "en": "Apa Itu 'Maglev' (Magnetic Levitation)?", "id": "Kereta Mengambang Menggunakan Medan Magnet." },
    { "en": "Apa Itu 'Deadman Switch' (Saklar Mati)?", "id": "Pengaman Penghenti Kereta Secara Otomatis." },
    { "en": "Apa Itu 'ATO' (Automatic Train Operation)?", "id": "Sistem Pengoperasian Kereta Api Otomatis." },
    { "en": "Apa Itu 'ATP' (Automatic Train Protection)?", "id": "Sistem Proteksi Kecepatan Kereta Api." },
    { "en": "Apa Itu 'CBTC' (Communication Based Train Control)?", "id": "Sistem Kendali Kereta Berbasis Komunikasi." },
    { "en": "Apa Itu 'Dynamic Braking' (Pengereman Dinamis)?", "id": "Pembuangan Energi Listrik Menjadi Panas." },
    { "en": "Apa Itu 'Inverter VVVF' (Variable Voltage Variable Frequency)?", "id": "Kendali Kecepatan Motor Traksi AC." },
    { "en": "Apa Itu 'Chopper Control' (Kendali Chopper)?", "id": "Kendali Tegangan Motor Traksi DC." },
    { "en": "Apa Itu 'Main Circuit Breaker' (Pemutus Utama)?", "id": "Pengaman Utama Rangkaian Listrik Kereta." },
    { "en": "Apa Itu 'Auxiliary Power' (Daya Bantu)?", "id": "Penyuplai Listrik Lampu Dan AC." },
    { "en": "Apa Itu 'Static Inverter' (Inverter Statis)?", "id": "Pengubah Daya Untuk Beban Bantu." },
    { "en": "Apa Itu 'Wheel Slip' (Selip Roda)?", "id": "Kehilangan Traksi Roda Pada Rel." },
    { "en": "Apa Itu 'Sand Box' (Kotak Pasir Kereta)?", "id": "Alat Penabur Pasir Penambah Traksi." },
    { "en": "Apa Itu 'Traction Transformer' (Transformator Traksi)?", "id": "Penurun Tegangan Listrik Katenari Kereta." },
    { "en": "Apa Itu 'Neutral Section' (Bagian Netral)?", "id": "Pemisah Antar Dua Sumber Listrik." },
    { "en": "Apa Itu 'Insulated Rail Joint' (Sambungan Rel)?", "id": "Pemisah Sirkuit Listrik Antar Blok." },
    { "en": "Apa Itu 'Track Circuit' (Sirkuit Jalur)?", "id": "Sistem Pendeteksi Keberadaan Kereta Api." },
    { "en": "Apa Itu 'Axle Counter' (Penghitung Gandar)?", "id": "Alat Hitung Roda Masuk Keluar." },
    { "en": "Apa Itu 'Wireless Charging' (Pengisian Nirkabel)?", "id": "Pengisian Daya Melalui Induksi Magnetik." },
    { "en": "Apa Itu 'Bidirectional Charging' (Pengisian Dua Arah)?", "id": "Aliran Daya Masuk Keluar Baterai." },
    { "en": "Apa Itu 'Smart Charging' (Pengisian Cerdas)?", "id": "Pengaturan Waktu Pengisian Daya Optimal." },
    { "en": "Apa Itu 'Plug-In' (Colok)?", "id": "Koneksi Fisik Kabel Ke Kendaraan." },
    { "en": "Apa Itu 'Charging Station' (Stasiun Pengisian)?", "id": "Tempat Pengisian Energi Listrik Umum." },
    { "en": "Apa Itu 'Fast Charging' (Pengisian Cepat)?", "id": "Pengisian Daya Tinggi Waktu Singkat." },
    { "en": "Apa Itu 'Ultra Fast Charging' (Sangat Cepat)?", "id": "Pengisian Daya Diatas Ratusan Kilowatt." },
    { "en": "Apa Itu 'Battery Swap' (Tukar Baterai)?", "id": "Penggantian Baterai Kosong Dengan Penuh." },
    { "en": "Apa Itu 'Micro-Mobility' (Mobilitas Mikro)?", "id": "Kendaraan Listrik Kecil Jarak Pendek." },
    { "en": "Apa Itu 'E-Scooter' (Skuter Listrik)?", "id": "Alat Transportasi Roda Dua Listrik." },
    { "en": "Apa Itu 'E-Bike' (Sepeda Listrik)?", "id": "Sepeda Dengan Bantuan Motor Listrik." },
    { "en": "Apa Itu 'Electric Bus' (Bus Listrik)?", "id": "Kendaraan Penumpang Besar Tenaga Listrik." },
    { "en": "Apa Itu 'Zero Emission' (Emisi Nol)?", "id": "Kendaraan Tanpa Gas Buang Berbahaya." },
    { "en": "Apa Itu 'Green Energy' (Energi Hijau)?", "id": "Energi Berasal Dari Sumber Terbarukan." },
    { "en": "Apa Itu 'Decarbonization' (Dekarbonisasi)?", "id": "Pengurangan Emisi Karbon Secara Global." },
    { "en": "Apa Itu 'Sustainability' (Keberlanjutan)?", "id": "Pemenuhan Kebutuhan Tanpa Merusak Alam." },
    { "en": "Apa Itu 'Carbon Credit' (Kredit Karbon)?", "id": "Izin Mengeluarkan Jumlah Emisi Tertentu." },
    { "en": "Apa Itu 'Life Cycle Assessment' (Penilaian Siklus)?", "id": "Evaluasi Dampak Lingkungan Produk Total." },
    { "en": "Apa Itu 'Recycling' (Daur Ulang Baterai)?", "id": "Pengolahan Kembali Material Baterai Bekas." },
    { "en": "Apa Itu 'VLSI' (Very Large Scale Integration)?", "id": "Integrasi Jutaan Transistor Dalam Chip." },
    { "en": "Apa Itu 'FPGA' (Field Programmable Gate Array)?", "id": "Sirkuit Terintegrasi Yang Dapat Diprogram." },
    { "en": "Apa Itu 'ASIC' (Application Specific Integrated Circuit)?", "id": "Chip Khusus Untuk Fungsi Tertentu." },
    { "en": "Apa Itu 'CPLD' (Complex Programmable Logic Device)?", "id": "Perangkat Logika Terprogram Arsitektur Sederhana." },
    { "en": "Apa Itu 'VHDL' (VHSIC Hardware Description Language)?", "id": "Bahasa Pemodelan Perangkat Keras Digital." },
    { "en": "Apa Itu 'Verilog' (Bahasa Verilog)?", "id": "Bahasa Deskripsi Standar Perangkat Keras." },
    { "en": "Apa Itu 'RTL' (Register Transfer Level)?", "id": "Abstraksi Aliran Sinyal Antar Register." },
    { "en": "Apa Itu 'Logic Synthesis' (Sintesis Logika)?", "id": "Konversi Kode Menjadi Jaringan Gerbang." },
    { "en": "Apa Itu 'Place And Route' (Penempatan Jalur)?", "id": "Pengaturan Fisik Komponen Dalam Chip." },
    { "en": "Apa Itu 'Timing Analysis' (Analisis Waktu)?", "id": "Verifikasi Kecepatan Sinyal Sirkuit Digital." },
    { "en": "Apa Itu 'Clock Skew' (Penyimpangan Detak)?", "id": "Perbedaan Waktu Kedatangan Sinyal Detak." },
    { "en": "Apa Itu 'Jitter' (Guncangan Sinyal)?", "id": "Variasi Periodik Waktu Sinyal Digital." },
    { "en": "Apa Itu 'Propagation Delay' (Tunda Rambat)?", "id": "Waktu Tempuh Sinyal Melalui Gerbang." },
    { "en": "Apa Itu 'Setup Time' (Waktu Persiapan)?", "id": "Waktu Minimal Data Stabil Sebelum." },
    { "en": "Apa Itu 'Hold Time' (Waktu Tahan)?", "id": "Waktu Minimal Data Stabil Sesudah." },
    { "en": "Apa Itu 'Metastability' (Metastabilitas)?", "id": "Kondisi Logika Tidak Stabil Sesaat." },
    { "en": "Apa Itu 'FSM' (Finite State Machine)?", "id": "Model Matematika Urutan Logika Digital." },
    { "en": "Apa Itu 'Moore Machine' (Mesin Moore)?", "id": "Output Tergantung Hanya Pada Status." },
    { "en": "Apa Itu 'Mealy Machine' (Mesin Mealy)?", "id": "Output Tergantung Status Dan Input." },
    { "en": "Apa Itu 'ALU' (Arithmetic Logic Unit)?", "id": "Unit Pemroses Operasi Matematika Logika." },
    { "en": "Apa Itu 'Register File' (Kumpulan Register)?", "id": "Array Lokasi Penyimpanan Cepat Prosesor." },
    { "en": "Apa Itu 'Pipelining' (Pemrosesan Jalur Pipa)?", "id": "Eksekusi Instruksi Secara Simultan Bertahap." },
    { "en": "Apa Itu 'Superscalar' (Arsitektur Superskalar)?", "id": "Eksekusi Banyak Instruksi Per Detak." },
    { "en": "Apa Itu 'RISC' (Reduced Instruction Set Computer)?", "id": "Prosesor Dengan Instruksi Sederhana Cepat." },
    { "en": "Apa Itu 'CISC' (Complex Instruction Set Computer)?", "id": "Prosesor Dengan Instruksi Kompleks Banyak." },
    { "en": "Apa Itu 'Von Neumann Architecture' (Arsitektur Von Neumann)?", "id": "Memori Tunggal Untuk Data Instruksi." },
    { "en": "Apa Itu 'Harvard Architecture' (Arsitektur Harvard)?", "id": "Memori Terpisah Untuk Data Instruksi." },
    { "en": "Apa Itu 'Cache Miss' (Gagal Cache)?", "id": "Data Tidak Ditemukan Dalam Cache." },
    { "en": "Apa Itu 'Cache Hit' (Berhasil Cache)?", "id": "Data Berhasil Ditemukan Dalam Cache." },
    { "en": "Apa Itu 'Virtual Memory' (Memori Virtual)?", "id": "Manajemen Memori Melampaui Kapasitas Fisik." },
    { "en": "Apa Itu 'MMU' (Memory Management Unit)?", "id": "Perangkat Pengelola Alamat Memori Prosesor." },
    { "en": "Apa Itu 'Interrupt Latency' (Latensi Interupsi)?", "id": "Waktu Respon Terhadap Sinyal Interupsi." },
    { "en": "Apa Itu 'RTOS' (Real-Time Operating System)?", "id": "Sistem Operasi Respon Waktu Cepat." },
    { "en": "Apa Itu 'Watchdog Timer' (Timer Penjaga)?", "id": "Sistem Pencegah Kegagalan Program Macet." },
    { "en": "Apa Itu 'DMA' (Direct Memory Access)?", "id": "Transfer Data Memori Tanpa CPU." },
    { "en": "Apa Itu 'I2C' (Inter-Integrated Circuit)?", "id": "Protokol Komunikasi Dua Kabel Serial." },
    { "en": "Apa Itu 'SPI' (Serial Peripheral Interface)?", "id": "Protokol Komunikasi Empat Kabel Cepat." },
    { "en": "Apa Itu 'UART' (Universal Asynchronous Receiver Transmitter)?", "id": "Komunikasi Serial Asinkron Sederhana Universal." },
    { "en": "Apa Itu 'CAN Bus' (Controller Area Network)?", "id": "Protokol Jaringan Komunikasi Data Otomotif." },
    { "en": "Apa Itu 'USB' (Universal Serial Bus)?", "id": "Standar Koneksi Perangkat Luar Universal." },
    { "en": "Apa Itu 'PCIe' (Peripheral Component Interconnect Express)?", "id": "Bus Komunikasi Kecepatan Sangat Tinggi." },
    { "en": "Apa Itu 'DDR RAM' (Double Data Rate)?", "id": "Memori Akses Acak Transfer Ganda." },
    { "en": "Apa Itu 'SRAM' (Static Random Access Memory)?", "id": "Memori Cepat Tanpa Perlu Penyegaran." },
    { "en": "Apa Itu 'DRAM' (Dynamic Random Access Memory)?", "id": "Memori Kapasitas Besar Perlu Penyegaran." },
    { "en": "Apa Itu 'Flash Memory' (Memori Flash)?", "id": "Penyimpanan Non-Volatil Yang Dapat Dihapus." },
    { "en": "Apa Itu 'EEPROM' (Electrically Erasable Programmable Memory)?", "id": "Memori Permanen Hapus Melalui Listrik." },
    { "en": "Apa Itu 'BIOS' (Basic Input Output System)?", "id": "Program Awal Inisialisasi Perangkat Keras." },
    { "en": "Apa Itu 'Bootloader' (Pemuat Boot)?", "id": "Kode Pemanggil Sistem Operasi Utama." },
    { "en": "Apa Itu 'JTAG' (Joint Test Action Group)?", "id": "Antarmuka Pengujian Dan Pemrograman Chip." },
    { "en": "Apa Itu 'Boundary Scan' (Pemindaian Batas)?", "id": "Metode Pengujian Jalur Internal Chip." },
    { "en": "Apa Itu 'PCB' (Printed Circuit Board)?", "id": "Papan Jalur Penghubung Komponen Elektronika." },
    { "en": "Apa Itu 'Gerber File' (Berkas Gerber)?", "id": "Format Standar Industri Manufaktur PCB." },
    { "en": "Apa Itu 'BOM' (Bill Of Materials)?", "id": "Daftar Lengkap Komponen Rakitan Elektronika." },
    { "en": "Apa Itu 'SMT' (Surface Mount Technology)?", "id": "Teknologi Pemasangan Komponen Permukaan Papan." },
    { "en": "Apa Itu 'THT' (Through Hole Technology)?", "id": "Teknologi Pemasangan Komponen Melalui Lubang." },
    { "en": "Apa Itu 'Solder Mask' (Masker Solder)?", "id": "Lapisan Pelindung Tembaga Dari Solder." },
    { "en": "Apa Itu 'Silkscreen' (Sablon Simbol)?", "id": "Cetakan Informasi Komponen Pada PCB." },
    { "en": "Apa Itu 'Via' (Lubang Jalur)?", "id": "Penghubung Jalur Antar Lapisan PCB." },
    { "en": "Apa Itu 'Blind Via' (Via Buta)?", "id": "Via Penghubung Lapisan Luar Dalam." },
    { "en": "Apa Itu 'Buried Via' (Via Terkubur)?", "id": "Via Penghubung Antar Lapisan Dalam." },
    { "en": "Apa Itu 'Signal Integrity' (Integritas Sinyal)?", "id": "Kualitas Sinyal Listrik Tanpa Distorsi." },
    { "en": "Apa Itu 'Power Integrity' (Integritas Daya)?", "id": "Kestabilan Distribusi Tegangan Dalam Sirkuit." },
    { "en": "Apa Itu 'Crosstalk' (Interferensi Jalur)?", "id": "Gangguan Sinyal Antar Jalur Berdekatan." },
    { "en": "Apa Itu 'Impedance Matching' (Penyesuaian Impedansi)?", "id": "Penyamaan Hambatan Jalur Transmisi Sinyal." },
    { "en": "Apa Itu 'Differential Signaling' (Sinyal Diferensial)?", "id": "Transmisi Data Melalui Dua Jalur." },
    { "en": "Apa Itu 'Decoupling Capacitor' (Kapasitor Kopling)?", "id": "Penyaring Derau Tegangan Dekat Komponen." },
    { "en": "Apa Itu 'Ferrite Bead' (Manik Ferit)?", "id": "Komponen Pasif Penekan Gangguan Frekuensi." },
    { "en": "Apa Itu 'Ground Plane' (Bidang Tanah)?", "id": "Lapisan Tembaga Luas Referensi Nol." },
    { "en": "Apa Itu 'Star Grounding' (Pentanahan Bintang)?", "id": "Teknik Penghubung Ground Satu Titik." },
    { "en": "Apa Itu 'EMI Shielding' (Pelindung Elektromagnetik)?", "id": "Penahan Gangguan Gelombang Luar Panel." },
    { "en": "Apa Itu 'ESD Protection' (Perlindungan Statis)?", "id": "Proteksi Terhadap Pelepasan Listrik Statis." },
    { "en": "Apa Itu 'Thermal Relief' (Peredam Panas)?", "id": "Desain Pad Memudahkan Proses Penyolderan." },
    { "en": "Apa Itu 'Reflow Soldering' (Solder Aliran)?", "id": "Metode Penyolderan Massal Komponen SMT." },
    { "en": "Apa Itu 'Wave Soldering' (Solder Gelombang)?", "id": "Metode Penyolderan Massal Komponen THT." },
    { "en": "Apa Itu 'BGA' (Ball Grid Array)?", "id": "Kemasan Chip Dengan Bola Solder." },
    { "en": "Apa Itu 'QFP' (Quad Flat Package)?", "id": "Kemasan Chip Kotak Kaki Empat." },
    { "en": "Apa Itu 'SOIC' (Small Outline Integrated Circuit)?", "id": "Kemasan Chip Ukuran Kecil Kompak." },
    { "en": "Apa Itu 'DIP' (Dual In-line Package)?", "id": "Kemasan Chip Dua Baris Kaki." },
    { "en": "Apa Itu 'Logic Analyzer' (Penganalisis Logika)?", "id": "Alat Pemantau Banyak Sinyal Digital." },
    { "en": "Apa Itu 'Protocol Analyzer' (Penganalisis Protokol)?", "id": "Alat Dekode Data Komunikasi Digital." },
    { "en": "Apa Itu 'MSO' (Mixed Signal Oscilloscope)?", "id": "Oskiloskop Pemantau Sinyal Analog Digital." },
    { "en": "Apa Itu 'Boundary Condition' (Kondisi Batas)?", "id": "Parameter Fisik Tepi Medan Elektromagnetik." },
    { "en": "Apa Itu 'Characteristic Impedance' (Impedansi Karakteristik)?", "id": "Hambatan Alami Jalur Transmisi Sinyal." },
    { "en": "Apa Itu 'Propagation Constant' (Konstanta Rambat)?", "id": "Ukuran Perubahan Sinyal Dalam Jalur." },
    { "en": "Apa Itu 'Smith Chart' (Diagram Smith)?", "id": "Grafik Kalkulasi Impedansi Frekuensi Radio." },
    { "en": "Apa Itu 'S-Parameters' (Parameter Hamburan)?", "id": "Karakteristik Jaringan Frekuensi Sangat Tinggi." },
    { "en": "Apa Itu 'Network Analyzer' (Penganalisis Jaringan)?", "id": "Alat Ukur Respon Frekuensi Radio." },
    { "en": "Apa Itu 'Anechoic Chamber' (Ruang Tanpa Gema)?", "id": "Ruang Pengujian Gangguan Elektromagnetik Steril." },
    { "en": "Apa Itu 'Zigbee' (Protokol Zigbee)?", "id": "Komunikasi Nirkabel Hemat Energi Jarak." },
    { "en": "Apa Itu 'LoRa' (Long Range Radio)?", "id": "Transmisi Data Jarak Jauh Daya." },
    { "en": "Apa Itu 'Wi-Fi 6' (Generasi Enam)?", "id": "Standar Jaringan Nirkabel Kecepatan Tinggi." },
    { "en": "Apa Itu '5G NR' (New Radio)?", "id": "Standar Teknologi Seluler Generasi Kelima." },
    { "en": "Apa Itu 'Beamforming' (Pembentukan Berkas)?", "id": "Pengarahan Sinyal Nirkabel Secara Fokus." },
    { "en": "Apa Itu 'Massive MIMO' (Multiple Input Multiple Output)?", "id": "Penggunaan Banyak Antena Secara Bersamaan." },
    { "en": "Apa Itu 'SDR' (Software Defined Radio)?", "id": "Sistem Komunikasi Radio Berbasis Perangkat." },
    { "en": "Apa Itu 'Cognitive Radio' (Radio Kognitif)?", "id": "Sistem Radio Adaptif Terhadap Spektrum." },
    { "en": "Apa Itu 'PID' (Proportional Integral Derivative)?", "id": "Kontroler Umpan Balik Linier Stabil." },
    { "en": "Apa Itu 'Setpoint' (Titik Setel)?", "id": "Nilai Target Yang Diinginkan Sistem." },
    { "en": "Apa Itu 'Process Variable' (Variabel Proses)?", "id": "Nilai Aktual Hasil Pengukuran Sensor." },
    { "en": "Apa Itu 'Error Signal' (Sinyal Kesalahan)?", "id": "Selisih Antara Setpoint Dan Aktual." },
    { "en": "Apa Itu 'Closed Loop' (Loop Tertutup)?", "id": "Sistem Kendali Dengan Umpan Balik." },
    { "en": "Apa Itu 'Open Loop' (Loop Terbuka)?", "id": "Sistem Kendali Tanpa Umpan Balik." },
    { "en": "Apa Itu 'Overshoot' (Loncatan Melampaui)?", "id": "Nilai Output Melebihi Target Setpoint." },
    { "en": "Apa Itu 'Settling Time' (Waktu Menetap)?", "id": "Waktu Sinyal Mencapai Kondisi Stabil." },
    { "en": "Apa Itu 'Rise Time' (Waktu Naik)?", "id": "Waktu Sinyal Mencapai Nilai Akhir." },
    { "en": "Apa Itu 'Steady State Error' (Kesalahan Tunak)?", "id": "Selisih Nilai Akhir Dan Target." },
    { "en": "Apa Itu 'Damping' (Peredaman)?", "id": "Pengurangan Amplitudo Osilasi Sistem Kendali." },
    { "en": "Apa Itu 'Transfer Function' (Fungsi Transfer)?", "id": "Model Matematika Input Terhadap Output." },
    { "en": "Apa Itu 'Bode Plot' (Diagram Bode)?", "id": "Grafik Respon Frekuensi Magnitudo, Fase." },
    { "en": "Apa Itu 'Root Locus' (Tempat Kedudukan Akar)?", "id": "Metode Analisis Stabilitas Akar Sistem." },
    { "en": "Apa Itu 'Nyquist Criterion' (Kriteria Nyquist)?", "id": "Penentu Stabilitas Berdasarkan Respon Frekuensi." },
    { "en": "Apa Itu 'Deadband' (Pita Mati)?", "id": "Rentang Input Tanpa Respon Output." },
    { "en": "Apa Itu 'Feedforward' (Umpan Maju)?", "id": "Kompensasi Gangguan Sebelum Mempengaruhi Sistem." },
    { "en": "Apa Itu 'Cascade Control' (Kendali Bertingkat)?", "id": "Sistem Dengan Loop Kendali Ganda." },
    { "en": "Apa Itu 'PLC' (Programmable Logic Controller)?", "id": "Komputer Khusus Pengendali Mesin Industri." },
    { "en": "Apa Itu 'Ladder Diagram' (Diagram Tangga)?", "id": "Bahasa Pemrograman Grafis Standar PLC." },
    { "en": "Apa Itu 'FBD' (Function Block Diagram)?", "id": "Pemrograman PLC Berbasis Blok Fungsi." },
    { "en": "Apa Itu 'SFC' (Sequential Function Chart)?", "id": "Bahasa Pemrograman PLC Berbasis Urutan." },
    { "en": "Apa Itu 'STL' (Statement List)?", "id": "Bahasa Pemrograman PLC Berbasis Teks." },
    { "en": "Apa Itu 'Structured Text' (Teks Terstruktur)?", "id": "Bahasa PLC Menyerupai Bahasa Pascal." },
    { "en": "Apa Itu 'CPU' (Central Processing Unit) PLC?", "id": "Otak Pemroses Logika Dalam PLC." },
    { "en": "Apa Itu 'Digital Input' (Input Digital)?", "id": "Sinyal Masukan Kondisi Nyala, Mati." },
    { "en": "Apa Itu 'Analog Input' (Input Analog)?", "id": "Sinyal Masukan Rentang Nilai Kontinu." },
    { "en": "Apa Itu 'Digital Output' (Output Digital)?", "id": "Sinyal Keluaran Penggerak Beban On-Off." },
    { "en": "Apa Itu 'Analog Output' (Output Analog)?", "id": "Sinyal Keluaran Pengendali Nilai Variabel." },
    { "en": "Apa Itu 'HMI' (Human Machine Interface)?", "id": "Layar Antarmuka Interaksi Manusia, Mesin." },
    { "en": "Apa Itu 'SCADA' (Supervisory Control And Data Acquisition)?", "id": "Sistem Pengawasan Dan Pengambilan Data." },
    { "en": "Apa Itu 'OPC' (Open Platform Communications)?", "id": "Standar Pertukaran Data Industri Universal." },
    { "en": "Apa Itu 'OPC UA' (Unified Architecture)?", "id": "Evolusi Protokol Komunikasi Industri Aman." },
    { "en": "Apa Itu 'RTU' (Remote Terminal Unit)?", "id": "Unit Kendali Data Lapangan Terpisah." },
    { "en": "Apa Itu 'Modbus' (Protokol Modbus)?", "id": "Standar Komunikasi Serial Perangkat Industri." },
    { "en": "Apa Itu 'Profibus' (Process Field Bus)?", "id": "Jaringan Komunikasi Lapangan Industri Cepat." },
    { "en": "Apa Itu 'Profinet' (Process Field Net)?", "id": "Standar Ethernet Untuk Otomasi Industri." },
    { "en": "Apa Itu 'EtherNet/IP' (Industrial Protocol)?", "id": "Protokol Komunikasi Industri Berbasis Ethernet." },
    { "en": "Apa Itu 'Foundation Fieldbus' (Bus Lapangan)?", "id": "Sistem Komunikasi Digital Instrumen Lapangan." },
    { "en": "Apa Itu 'HART' (Highway Addressable Remote Transducer)?", "id": "Protokol Sinyal Digital Diatas Analog." },
    { "en": "Apa Itu 'Distributed Control System' (DCS)?", "id": "Sistem Kendali Proses Terdistribusi Luas." },
    { "en": "Apa Itu 'Distributed I/O' (I/O Terdistribusi)?", "id": "Modul Input Output Lokasi Berbeda." },
    { "en": "Apa Itu 'Safety PLC' (PLC Keamanan)?", "id": "PLC Khusus Fungsi Perlindungan Darurat." },
    { "en": "Apa Itu 'SIL' (Safety Integrity Level)?", "id": "Tingkat Keandalan Sistem Keselamatan Instrumen." },
    { "en": "Apa Itu 'Actuator' (Aktuator)?", "id": "Perangkat Pengubah Sinyal Menjadi Gerak." },
    { "en": "Apa Itu 'Control Valve' (Katup Kendali)?", "id": "Katup Pengatur Laju Aliran Fluida." },
    { "en": "Apa Itu 'Solenoid Valve' (Katup Solenoid)?", "id": "Katup Terkendali Medan Magnet Listrik." },
    { "en": "Apa Itu 'Positioner' (Pemosisian Katup)?", "id": "Alat Penjamin Posisi Bukaan Katup." },
    { "en": "Apa Itu 'Encoder' (Enkoder Rotasi)?", "id": "Sensor Pengukur Posisi Putaran Poros." },
    { "en": "Apa Itu 'Resolver' (Resolver)?", "id": "Sensor Posisi Sudut Induktif Presisi." },
    { "en": "Apa Itu 'Vortex Flowmeter' (Meter Vortex)?", "id": "Pengukur Aliran Berdasarkan Pusaran Fluida." },
    { "en": "Apa Itu 'Coriolis Flowmeter' (Meter Coriolis)?", "id": "Pengukur Massa Aliran Fluida Akurat." },
    { "en": "Apa Itu 'Ultrasonic Flowmeter' (Meter Ultrasonik)?", "id": "Pengukur Aliran Berbasis Gelombang Suara." },
    { "en": "Apa Itu 'Magnetic Flowmeter' (Meter Magnet)?", "id": "Pengukur Aliran Cairan Konduktif Listrik." },
    { "en": "Apa Itu 'Radar Level' (Level Radar)?", "id": "Pengukur Ketinggian Menggunakan Gelombang Mikro." },
    { "en": "Apa Itu 'Hydrostatic Level' (Level Hidrostatis)?", "id": "Pengukur Ketinggian Berdasarkan Tekanan Cairan." },
    { "en": "Apa Itu 'Load Cell' (Sel Beban)?", "id": "Sensor Pengukur Berat Melalui Tekanan." },
    { "en": "Apa Itu 'Strain Gauge' (Pengukur Regangan)?", "id": "Sensor Perubahan Hambatan Akibat Deformasi." },
    { "en": "Apa Itu 'Thermowell' (Sumur Termal)?", "id": "Tabung Pelindung Sensor Suhu Industri." },
    { "en": "Apa Itu 'Pyrometer' (Pirometer)?", "id": "Sensor Suhu Tinggi Tanpa Kontak." },
    { "en": "Apa Itu 'Signal Conditioner' (Pengkondisi Sinyal)?", "id": "Pengubah Sinyal Sensor Menjadi Standar." },
    { "en": "Apa Itu 'Current Loop' (Loop Arus)?", "id": "Standar Transmisi Sinyal 4-20 Milliampere." },
    { "en": "Apa Itu 'Zener Barrier' (Pembatas Zener)?", "id": "Alat Pelindung Arus Area Berbahaya." },
    { "en": "Apa Itu 'Explosion Proof' (Tahan Ledakan)?", "id": "Rumah Peralatan Penahan Ledakan Internal." },
    { "en": "Apa Itu 'Calibration' (Kalibrasi)?", "id": "Proses Verifikasi Akurasi Alat Ukur." },
    { "en": "Apa Itu 'Traceability' (Ketertelusuran)?", "id": "Kesesuaian Kalibrasi Dengan Standar Nasional." },
    { "en": "Apa Itu 'Accuracy' (Akurasi)?", "id": "Kedekatan Hasil Ukur Nilai Sebenarnya." },
    { "en": "Apa Itu 'Precision' (Presisi)?", "id": "Konsistensi Hasil Pengukuran Berulang Kali." },
    { "en": "Apa Itu 'Resolution' (Resolusi)?", "id": "Perubahan Terkecil Yang Dapat Dideteksi." },
    { "en": "Apa Itu 'Linearity' (Linearitas)?", "id": "Kesebandingan Output Terhadap Perubahan Input." },
    { "en": "Apa Itu 'Hysteresis' (Histeresis)?", "id": "Perbedaan Pembacaan Saat Naik Turun." },
    { "en": "Apa Itu 'Drift' (Hanyutan)?", "id": "Perubahan Karakteristik Alat Seiring Waktu." },
    { "en": "Apa Itu 'Range' (Rentang Ukur)?", "id": "Batas Nilai Minimum Hingga Maksimum." },
    { "en": "Apa Itu 'Span' (Span Ukur)?", "id": "Selisih Antara Nilai Maksimum Minimum." },
    { "en": "Apa Itu 'Turndown Ratio' (Rasio Turun)?", "id": "Rasio Batas Maksimal Terhadap Minimal." },
    { "en": "Apa Itu 'Repeatability' (Repeatabilitas)?", "id": "Kemampuan Menghasilkan Nilai Sama Berulang." },
    { "en": "Apa Itu 'Zero Adjustment' (Penyetelan Nol)?", "id": "Kalibrasi Titik Awal Pengukuran Alat." },
    { "en": "Apa Itu 'Smart Transmitter' (Transmiter Cerdas)?", "id": "Alat Ukur Dengan Kemampuan Diagnosa." },
    { "en": "Apa Itu 'Robotic Arm' (Lengan Robot)?", "id": "Manipulator Mekanik Terkendali Secara Komputer." },
    { "en": "Apa Itu 'End Effector' (Alat Ujung)?", "id": "Perangkat Kerja Pada Ujung Robot." },
    { "en": "Apa Itu 'Degrees Of Freedom' (Derajat Kebebasan)?", "id": "Jumlah Arah Gerakan Independen Robot." },
    { "en": "Apa Itu 'Inverse Kinematics' (Kinematika Terbalik)?", "id": "Perhitungan Sudut Sendi Posisi Robot." },
    { "en": "Apa Itu 'Forward Kinematics' (Kinematika Maju)?", "id": "Perhitungan Posisi Ujung Sudut Sendi." },
    { "en": "Apa Itu 'Servo Drive' (Penggerak Servo)?", "id": "Sirkuit Elektronik Pengontrol Motor Servo." },
    { "en": "Apa Itu 'Machine Vision' (Visi Mesin)?", "id": "Sistem Inspeksi Berbasis Kamera Industri." },
    { "en": "Apa Itu 'AGV' (Automated Guided Vehicle)?", "id": "Kendaraan Pengangkut Otomatis Jalur Tetap." },
    { "en": "Apa Itu 'AMR' (Autonomous Mobile Robot)?", "id": "Robot Bergerak Mandiri Tanpa Jalur." },
    { "en": "Apa Itu 'Cobot' (Collaborative Robot)?", "id": "Robot Bekerja Aman Bersama Manusia." },
    { "en": "Apa Itu 'IIoT' (Industrial Internet Of Things)?", "id": "Integrasi Perangkat Industri Ke Internet." },
    { "en": "Apa Itu 'Digital Twin' (Kembaran Digital)?", "id": "Representasi Virtual Dari Aset Fisik." },
    { "en": "Apa Itu 'Edge Computing' (Komputasi Tepi)?", "id": "Pemrosesan Data Dekat Sumber Informasi." },
    { "en": "Apa Itu 'Cloud Computing' (Komputasi Awan)?", "id": "Layanan Penyimpanan Dan Pemrosesan Daring." },
    { "en": "Apa Itu 'Big Data' (Data Besar)?", "id": "Kumpulan Data Volume Sangat Besar." },
    { "en": "Apa Itu 'OEE' (Overall Equipment Effectiveness)?", "id": "Ukuran Produktivitas Dan Efisiensi Mesin." },
    { "en": "Apa Itu 'Industry 4.0' (Industri 4.0)?", "id": "Transformasi Digital Sektor Manufaktur Modern." },
    { "en": "Apa Itu 'Power Quality' (Kualitas Daya Listrik)?", "id": "Kesesuaian Parameter Tegangan, Arus, Frekuensi." },
    { "en": "Apa Itu 'Voltage Sag' (Kedipan Tegangan)?", "id": "Penurunan Tegangan Singkat Jaringan Listrik." },
    { "en": "Apa Itu 'Voltage Swell' (Lonjakan Tegangan)?", "id": "Kenaikan Tegangan Singkat Jaringan Listrik." },
    { "en": "Apa Itu 'Voltage Flicker' (Kedipan Cahaya)?", "id": "Variasi Tegangan Berulang Mengganggu Lampu." },
    { "en": "Apa Itu 'Transient' (Tegangan Transien)?", "id": "Lonjakan Tegangan Ekstrem Durasi Sangat." },
    { "en": "Apa Itu 'Harmonics' (Harmonisa Arus)?", "id": "Gangguan Gelombang Kelipatan Frekuensi Dasar." },
    { "en": "Apa Itu 'Interharmonics' (Inter-Harmonisa)?", "id": "Gangguan Frekuensi Bukan Kelipatan Bulat." },
    { "en": "Apa Itu 'Voltage Unbalance' (Ketidakseimbangan Tegangan)?", "id": "Perbedaan Magnitudo Fase Sistem Tiga." },
    { "en": "Apa Itu 'Frequency Variation' (Variasi Frekuensi)?", "id": "Penyimpangan Nilai Frekuensi Dari Standar." },
    { "en": "Apa Itu 'Notching' (Takikan Tegangan)?", "id": "Gangguan Tegangan Akibat Komutasi Elektronika." },
    { "en": "Apa Itu 'Noise' (Derau Elektromagnetik)?", "id": "Sinyal Listrik Liar Pengganggu Peralatan." },
    { "en": "Apa Itu 'PFC' (Power Factor Correction)?", "id": "Metode Perbaikan Nilai Faktor Daya." },
    { "en": "Apa Itu 'Active Power Filter' (Filter Aktif)?", "id": "Kompensator Harmonisa Berbasis Inverter Daya." },
    { "en": "Apa Itu 'Passive Power Filter' (Filter Pasif)?", "id": "Kombinasi L, C Penyaring Harmonisa." },
    { "en": "Apa Itu 'Detuned Reactor' (Reaktor Detuned)?", "id": "Induktor Pelindung Kapasitor Dari Harmonisa." },
    { "en": "Apa Itu 'Isolation Transformer' (Transformator Isolasi)?", "id": "Pemisah Gangguan Antar Dua Sirkuit." },
    { "en": "Apa Itu 'Surge Arrester' (Penangkap Surja)?", "id": "Alat Pembuang Tegangan Lebih Bumi." },
    { "en": "Apa Itu 'SPD' (Surge Protective Device)?", "id": "Perangkat Pelindung Elektronika Dari Surja." },
    { "en": "Apa Itu 'Grounding' (Pentanahan Sistem)?", "id": "Hubungan Titik Netral Menuju Bumi." },
    { "en": "Apa Itu 'Earthing' (Pembumian Peralatan)?", "id": "Hubungan Bagian Logam Menuju Bumi." },
    { "en": "Apa Itu 'Neutral Grounding Resistor' (NGR)?", "id": "Resistor Pembatas Arus Gangguan Tanah." },
    { "en": "Apa Itu 'Solid Grounding' (Pentanahan Langsung)?", "id": "Hubungan Netral Ke Bumi Tanpa." },
    { "en": "Apa Itu 'Peterson Coil' (Kumparan Peterson)?", "id": "Reaktor Penyeimbang Arus Kapasitif Tanah." },
    { "en": "Apa Itu 'Fault Current' (Arus Gangguan)?", "id": "Aliran Listrik Saat Terjadi Kegagalan." },
    { "en": "Apa Itu 'Overcurrent' (Arus Lebih)?", "id": "Kondisi Arus Melebihi Kapasitas Penghantar." },
    { "en": "Apa Itu 'Overload' (Beban Lebih)?", "id": "Penggunaan Daya Melebihi Rating Peralatan." },
    { "en": "Apa Itu 'Under Voltage' (Tegangan Kurang)?", "id": "Tegangan Jatuh Di Bawah Batas." },
    { "en": "Apa Itu 'Over Voltage' (Tegangan Lebih)?", "id": "Tegangan Naik Di Atas Batas." },
    { "en": "Apa Itu 'Phase Failure' (Kegagalan Fase)?", "id": "Hilangnya Tegangan Pada Salah Satu." },
    { "en": "Apa Itu 'Arc Flash' (Busur Api)?", "id": "Ledakan Cahaya Panas Akibat Hubung." },
    { "en": "Apa Itu 'Protective Relay' (Relay Proteksi)?", "id": "Alat Deteksi Gangguan Pemutus Sirkuit." },
    { "en": "Apa Itu 'Differential Relay' (Relay Diferensial)?", "id": "Proteksi Berdasarkan Selisih Arus Masuk." },
    { "en": "Apa Itu 'Distance Relay' (Relay Jarak)?", "id": "Proteksi Berdasarkan Nilai Impedansi Jalur." },
    { "en": "Apa Itu 'Directional Relay' (Relay Pengarah)?", "id": "Proteksi Berdasarkan Arah Aliran Arus." },
    { "en": "Apa Itu 'Buchholz Relay' (Relay Buchholz)?", "id": "Pengaman Gas Tekanan Internal Transformator." },
    { "en": "Apa Itu 'Circuit Breaker' (Pemutus Sirkuit)?", "id": "Saklar Pemutus Arus Gangguan Otomatis." },
    { "en": "Apa Itu 'MCB' (Miniature Circuit Breaker)?", "id": "Pemutus Arus Skala Kecil Domestik." },
    { "en": "Apa Itu 'Recloser' (Rekloser)?", "id": "Pemutus Jaringan Penutup Kembali Otomatis." },
    { "en": "Apa Itu 'Sectionalizer' (Seksonaliser)?", "id": "Alat Pemutus Jaringan Distribusi Terkoordinasi." },
    { "en": "Apa Itu 'Load Break Switch' (LBS)?", "id": "Saklar Pemutus Beban Secara Manual." },
    { "en": "Apa Itu 'Disconnecting Switch' (DS)?", "id": "Saklar Pemisah Rangkaian Tanpa Beban." },
    { "en": "Apa Itu 'RTU' (Remote Terminal Unit)?", "id": "Unit Pengumpul Data Lapangan Terpisah." },
    { "en": "Apa Itu 'AMI' (Advanced Metering Infrastructure)?", "id": "Infrastruktur Meteran Listrik Pintar Digital." },
    { "en": "Apa Itu 'Demand Response' (Respon Permintaan)?", "id": "Penyesuaian Konsumsi Beban Pelanggan Fleksibel." },
    { "en": "Apa Itu 'Load Profile' (Profil Beban)?", "id": "Pola Penggunaan Listrik Terhadap Waktu." },
    { "en": "Apa Itu 'Peak Shaving' (Pemangkasan Puncak)?", "id": "Pengurangan Beban Saat Konsumsi Maksimal." },
    { "en": "Apa Itu 'Load Shifting' (Pergeseran Beban)?", "id": "Pemindahan Waktu Konsumsi Listrik Pelanggan." },
    { "en": "Apa Itu 'Energy Storage' (Penyimpanan Energi)?", "id": "Sistem Cadangan Energi Listrik Baterai." },
    { "en": "Apa Itu 'Distributed Generation' (Pembangkit Tersebar)?", "id": "Sumber Energi Kapasitas Kecil Terdistribusi." },
    { "en": "Apa Itu 'Islanding' (Kondisi Berpulau)?", "id": "Pembangkit Beroperasi Terpisah Dari Grid." },
    { "en": "Apa Itu 'Anti-Islanding' (Anti-Berpulau)?", "id": "Proteksi Pencegah Pembangkit Beroperasi Terpisah." },
    { "en": "Apa Itu 'Power Quality Analyzer' (PQA)?", "id": "Alat Analisis Gangguan Kualitas Daya." },
    { "en": "Apa Itu 'Scope Meter' (Skop Meter)?", "id": "Kombinasi Oskiloskop Dan Multimeter Digital." },
    { "en": "Apa Itu 'Syncroscope' (Sinkroskop)?", "id": "Alat Bantu Paralel Dua Pembangkit." },
    { "en": "Apa Itu 'CT' (Current Transformer)?", "id": "Transformator Pengubah Arus Skala Besar." },
    { "en": "Apa Itu 'PT' (Potential Transformer)?", "id": "Transformator Pengubah Tegangan Skala Besar." },
    { "en": "Apa Itu 'Burden' (Beban Sekunder)?", "id": "Impedansi Eksternal Pada Sekunder Transformator." },
    { "en": "Apa Itu 'Knee Point Voltage' (Tegangan Lutut)?", "id": "Batas Saturasi Magnetik Inti CT." },
    { "en": "Apa Itu 'Accuracy Class' (Kelas Akurasi)?", "id": "Tingkat Ketelitian Pengukuran Alat Ukur." },
    { "en": "Apa Itu 'Instrument Transformer' (Transformator Instrumen)?", "id": "Transformator Khusus Untuk Alat Ukur." },
    { "en": "Apa Itu 'Shielded Cable' (Kabel Perisai)?", "id": "Kabel Pelindung Gangguan Luar Elektromagnetik." },
    { "en": "Apa Itu 'Voltage Drop' (Jatuh Tegangan)?", "id": "Kehilangan Tegangan Sepanjang Jalur Kabel." },
    { "en": "Apa Itu 'Ampacity' (Kuat Hantar Arus)?", "id": "Arus Maksimal Yang Dapat Dialirkan." },
    { "en": "Apa Itu 'Real Power' (Daya Aktif)?", "id": "Daya Nyata Yang Menjadi Kerja." },
    { "en": "Apa Itu 'Grid Stability' (Stabilitas Jaringan)?", "id": "Kemampuan Jaringan Menjaga Frekuensi Tegangan." },
    { "en": "Apa Itu 'Inertia' (Inersia Sistem Tenaga)?", "id": "Energi Simpanan Mesin Berputar Jaringan." },
    { "en": "Apa Itu 'Primary Frequency Response' (Respon Frekuensi)?", "id": "Tindakan Cepat Pembangkit Menstabilkan Frekuensi." },
    { "en": "Apa Itu 'Regulation' (Regulasi Tegangan)?", "id": "Kemampuan Menjaga Tegangan Tetap Stabil." },
    { "en": "Apa Itu 'Efficiency' (Efisiensi Sistem)?", "id": "Rasio Daya Keluaran Terhadap Masukan." },
    { "en": "Apa Itu 'Reliability' (Keandalan Jaringan)?", "id": "Kemampuan Melayani Beban Tanpa Gangguan." },
    { "en": "Apa Itu 'SoC' (System On Chip)?", "id": "Integrasi Seluruh Sistem Dalam Chip." },
    { "en": "Apa Itu 'MCU' (Microcontroller Unit)?", "id": "Komputer Kecil Untuk Kendali Perangkat." },
    { "en": "Apa Itu 'MPU' (Microprocessor Unit)?", "id": "Unit Pemroses Data Komputer Utama." },
    { "en": "Apa Itu 'DSP' (Digital Signal Processor)?", "id": "Prosesor Khusus Pengolah Sinyal Digital." },
    { "en": "Apa Itu 'GPP' (General Purpose Processor)?", "id": "Prosesor Untuk Berbagai Tugas Umum." },
    { "en": "Apa Itu 'ASIC' (Application Specific Integrated Circuit)?", "id": "Chip Khusus Satu Fungsi Tertentu." },
    { "en": "Apa Itu 'FPGA' (Field Programmable Gate Array)?", "id": "Sirkuit Digital Yang Dapat Diprogram." },
    { "en": "Apa Itu 'Kernel' (Inti Sistem Operasi)?", "id": "Bagian Utama Pengelola Sumber Daya." },
    { "en": "Apa Itu 'Task' (Tugas Dalam RTOS)?", "id": "Unit Terkecil Eksekusi Program Mandiri." },
    { "en": "Apa Itu 'Scheduler' (Penjadwal Sistem)?", "id": "Pengatur Urutan Eksekusi Tugas Prosesor." },
    { "en": "Apa Itu 'Context Switching' (Perpindahan Konteks)?", "id": "Proses Pergantian Tugas Oleh CPU." },
    { "en": "Apa Itu 'Preemption' (Pengambilalihan Tugas)?", "id": "Penghentian Tugas Prioritas Rendah Mendadak." },
    { "en": "Apa Itu 'Priority Inversion' (Inversi Prioritas)?", "id": "Tugas Tinggi Menunggu Tugas Rendah." },
    { "en": "Apa Itu 'Semaphore' (Semafor Sistem)?", "id": "Mekanisme Sinkronisasi Antar Tugas Program." },
    { "en": "Apa Itu 'Mutex' (Mutual Exclusion)?", "id": "Pengunci Akses Sumber Daya Tunggal." },
    { "en": "Apa Itu 'Deadlock' (Kebuntuan Sistem)?", "id": "Kondisi Tugas Saling Menunggu Selamanya." },
    { "en": "Apa Itu 'Interrupt' (Interupsi Perangkat Keras)?", "id": "Sinyal Penghenti Sementara Alur CPU." },
    { "en": "Apa Itu 'ISR' (Interrupt Service Routine)?", "id": "Fungsi Khusus Penanganan Sinyal Interupsi." },
    { "en": "Apa Itu 'NVIC' (Nested Vectored Interrupt Controller)?", "id": "Pengelola Prioritas Interupsi Mikroprosesor ARM." },
    { "en": "Apa Itu 'Polling' (Pengambilan Sampel)?", "id": "Pengecekan Status Perangkat Secara Rutin." },
    { "en": "Apa Itu 'Watchdog Timer' (Timer Penjaga)?", "id": "Sistem Pendeteksi Kegagalan Program Macet." },
    { "en": "Apa Itu 'RTC' (Real-Time Clock)?", "id": "Chip Penghitung Waktu Dan Tanggal." },
    { "en": "Apa Itu 'DMA' (Direct Memory Access)?", "id": "Transfer Data Tanpa Melibatkan CPU." },
    { "en": "Apa Itu 'PWM' (Pulse Width Modulation)?", "id": "Modulasi Lebar Pulsa Kendali Tegangan." },
    { "en": "Apa Itu 'ADC' (Analog To Digital Converter)?", "id": "Pengubah Tegangan Menjadi Data Digital." },
    { "en": "Apa Itu 'UART' (Universal Asynchronous Receiver Transmitter)?", "id": "Protokol Komunikasi Serial Tanpa Detak." },
    { "en": "Apa Itu 'SPI' (Serial Peripheral Interface)?", "id": "Komunikasi Serial Sinkron Kecepatan Tinggi." },
    { "en": "Apa Itu 'I2C' (Inter-Integrated Circuit)?", "id": "Komunikasi Serial Dua Kabel Sederhana." },
    { "en": "Apa Itu 'CAN Bus' (Controller Area Network)?", "id": "Protokol Komunikasi Handal Sistem Otomotif." },
    { "en": "Apa Itu 'JTAG' (Joint Test Action Group)?", "id": "Antarmuka Debugging Dan Pemrograman Chip." },
    { "en": "Apa Itu 'SWD' (Serial Wire Debug)?", "id": "Antarmuka Debugging Minimalis Dua Kabel." },
    { "en": "Apa Itu 'In-Circuit Emulator' (ICE)?", "id": "Alat Peniru Fungsi Perangkat Keras." },
    { "en": "Apa Itu 'Bootloader' (Pemuat Bot)?", "id": "Kode Awal Pemanggil Firmware Utama." },
    { "en": "Apa Itu 'Firmware' (Perangkat Tegar)?", "id": "Perangkat Lunak Permanen Dalam Hardware." },
    { "en": "Apa Itu 'EEPROM' (Electrically Erasable Programmable Memory)?", "id": "Memori Permanen Dapat Dihapus Listrik." },
    { "en": "Apa Itu 'Flash Memory' (Memori Flash)?", "id": "Penyimpanan Non-Volatil Kecepatan Tinggi Chip." },
    { "en": "Apa Itu 'SRAM' (Static Random Access Memory)?", "id": "Memori RAM Cepat Tanpa Refresh." },
    { "en": "Apa Itu 'DRAM' (Dynamic Random Access Memory)?", "id": "Memori RAM Murah Dengan Refresh." },
    { "en": "Apa Itu 'Von Neumann' (Arsitektur Von Neumann)?", "id": "Memori Tunggal Data Dan Instruksi." },
    { "en": "Apa Itu 'Harvard Architecture' (Arsitektur Harvard)?", "id": "Memori Terpisah Data Dan Instruksi." },
    { "en": "Apa Itu 'Pipelining' (Jalur Pipa Instruksi)?", "id": "Teknik Eksekusi Banyak Instruksi Simultan." },
    { "en": "Apa Itu 'Endianness' (Urutan Byte)?", "id": "Cara Penyimpanan Urutan Byte Memori." },
    { "en": "Apa Itu 'Little Endian' (Endian Kecil)?", "id": "Byte Terendah Disimpan Alamat Awal." },
    { "en": "Apa Itu 'Big Endian' (Endian Besar)?", "id": "Byte Tertinggi Disimpan Alamat Awal." },
    { "en": "Apa Itu 'Assembly' (Bahasa Rakitan)?", "id": "Bahasa Pemrograman Tingkat Rendah Mesin." },
    { "en": "Apa Itu 'Compiler' (Kompilator)?", "id": "Penerjemah Kode Ke Bahasa Mesin." },
    { "en": "Apa Itu 'Linker' (Penghubung Kode)?", "id": "Penggabung Berkas Objek Menjadi Eksekusi." },
    { "en": "Apa Itu 'Instruction Set' (Kumpulan Instruksi)?", "id": "Daftar Perintah Dasar Sebuah Prosesor." },
    { "en": "Apa Itu 'RISC' (Reduced Instruction Set Computer)?", "id": "Prosesor Dengan Instruksi Sederhana Efisien." },
    { "en": "Apa Itu 'ARM' (Advanced RISC Machine)?", "id": "Arsitektur Prosesor Hemat Energi Populer." },
    { "en": "Apa Itu 'FPV' (Floating Point Unit)?", "id": "Bagian Pemroses Angka Desimal Cepat." },
    { "en": "Apa Itu 'GPIO Pull-Up' (Tarik Ke Atas)?", "id": "Resistor Penjaga Status Logika Tinggi." },
    { "en": "Apa Itu 'GPIO Pull-Down' (Tarik Ke Bawah)?", "id": "Resistor Penjaga Status Logika Rendah." },
    { "en": "Apa Itu 'Debouncing' (Penghilang Pantulan)?", "id": "Teknik Pembersihan Sinyal Tombol Mekanik." },
    { "en": "Apa Itu 'Sampling' (Pencuplikan Sinyal)?", "id": "Proses Pengambilan Nilai Sinyal Analog." },
    { "en": "Apa Itu 'Sampling Rate' (Laju Cuplikan)?", "id": "Jumlah Sampel Diambil Per Detik." },
    { "en": "Apa Itu 'Nyquist Theorem' (Teorema Nyquist)?", "id": "Syarat Minimal Kecepatan Cuplikan Sinyal." },
    { "en": "Apa Itu 'Aliasing' (Aliasing Sinyal)?", "id": "Distorsi Sinyal Akibat Sampling Rendah." },
    { "en": "Apa Itu 'Quantization' (Kuantisasi)?", "id": "Pembulatan Nilai Sinyal Ke Digital." },
    { "en": "Apa Itu 'Quantization Error' (Kesalahan Kuantisasi)?", "id": "Selisih Antara Analog Dan Digital." },
    { "en": "Apa Itu 'SNR' (Signal To Noise Ratio)?", "id": "Perbandingan Kekuatan Sinyal Dan Derau." },
    { "en": "Apa Itu 'FFT' (Fast Fourier Transform)?", "id": "Algoritma Analisis Frekuensi Sinyal Cepat." },
    { "en": "Apa Itu 'DFT' (Discrete Fourier Transform)?", "id": "Transformasi Sinyal Waktu Ke Frekuensi." },
    { "en": "Apa Itu 'Digital Filter' (Penyaring Digital)?", "id": "Algoritma Pemroses Sinyal Secara Digital." },
    { "en": "Apa Itu 'FIR Filter' (Finite Impulse Response)?", "id": "Penyaring Dengan Respon Impuls Terbatas." },
    { "en": "Apa Itu 'IIR Filter' (Infinite Impulse Response)?", "id": "Penyaring Dengan Respon Impuls Tak-Terbatas." },
    { "en": "Apa Itu 'Low Pass Filter' (Lolos Rendah)?", "id": "Penyaring Frekuensi Di Bawah Batas." },
    { "en": "Apa Itu 'High Pass Filter' (Lolos Tinggi)?", "id": "Penyaring Frekuensi Di Atas Batas." },
    { "en": "Apa Itu 'Band Pass Filter' (Lolos Pita)?", "id": "Penyaring Frekuensi Dalam Rentang Tertentu." },
    { "en": "Apa Itu 'Convolution' (Konvolusi)?", "id": "Operasi Matematika Penggabungan Dua Sinyal." },
    { "en": "Apa Itu 'Z-Transform' (Transformasi Z)?", "id": "Analisis Sistem Linier Waktu Diskrit." },
    { "en": "Apa Itu 'Transfer Function' (Fungsi Transfer)?", "id": "Hubungan Input Dan Output Sistem." },
    { "en": "Apa Itu 'Stable System' (Sistem Stabil)?", "id": "Output Terbatas Untuk Input Terbatas." },
    { "en": "Apa Itu 'Open Source Hardware' (Perangkat Terbuka)?", "id": "Desain Perangkat Keras Bebas Diakses." },
    { "en": "Apa Itu 'Arduino' (Platform Arduino)?", "id": "Papan Mikrokontroler Mudah Untuk Pemula." },
    { "en": "Apa Itu 'Raspberry Pi' (Komputer Papan Tunggal)?", "id": "Komputer Kecil Berbasis Sistem Linux." },
    { "en": "Apa Itu 'Shield' (Perisai Arduino)?", "id": "Papan Ekspansi Fungsi Tambahan Arduino." },
    { "en": "Apa Itu 'Baud Rate' (Laju Baud)?", "id": "Kecepatan Transmisi Data Serial Bit." },
    { "en": "Apa Itu 'Start Bit' (Bit Mulai)?", "id": "Bit Penanda Awal Data Serial." },
    { "en": "Apa Itu 'Stop Bit' (Bit Berhenti)?", "id": "Bit Penanda Akhir Data Serial." },
    { "en": "Apa Itu 'Flow Control' (Kendali Aliran)?", "id": "Pengatur Kecepatan Pengiriman Data Komunikasi." },
    { "en": "Apa Itu 'Full Duplex' (Dupleks Penuh)?", "id": "Komunikasi Dua Arah Secara Bersamaan." },
    { "en": "Apa Itu 'Half Duplex' (Setengah Dupleks)?", "id": "Komunikasi Dua Arah Bergantian Waktu." },
    { "en": "Apa Itu 'Simplex' (Simpleks)?", "id": "Komunikasi Satu Arah Secara Permanen." },
    { "en": "Apa Itu 'Breadboard' (Papan Roti)?", "id": "Papan Percobaan Rangkaian Tanpa Solder." },
    { "en": "Apa Itu 'Jumper Wire' (Kabel Jumper)?", "id": "Kabel Penghubung Titik Rangkaian Percobaan." },
    { "en": "Apa Itu 'Multimeter' (Alat Ukur Listrik)?", "id": "Alat Ukur Arus, Tegangan, Hambatan." },
    { "en": "Apa Itu 'Oscilloscope' (Oskiloskop)?", "id": "Alat Visualisasi Grafik Sinyal Listrik." },
    { "en": "Apa Itu 'Logic Analyzer' (Penganalisis Logika)?", "id": "Alat Pantau Banyak Sinyal Digital." },
    { "en": "Apa Itu 'Through Hole' (Lubang Tembus)?", "id": "Komponen Dengan Kaki Menembus Papan." },
    { "en": "Apa Itu 'SMD' (Surface Mount Device)?", "id": "Komponen Menempel Di Permukaan Papan." },
    { "en": "Apa Itu 'Soldering' (Penyolderan)?", "id": "Proses Penyambungan Logam Menggunakan Timah." },
    { "en": "Apa Itu 'Flux' (Fluks Solder)?", "id": "Cairan Pembersih Permukaan Saat Solder." },
    { "en": "Apa Itu 'RF' (Radio Frequency)?", "id": "Spektrum Frekuensi Gelombang Radio Komunikasi." },
    { "en": "Apa Itu 'Electromagnetic Wave' (Gelombang Elektromagnetik)?", "id": "Rambatan Energi Medan Listrik Magnet." },
    { "en": "Apa Itu 'Wavelength' (Panjang Gelombang)?", "id": "Jarak Antara Dua Puncak Gelombang." },
    { "en": "Apa Itu 'Frequency' (Frekuensi)?", "id": "Jumlah Getaran Gelombang Per Detik." },
    { "en": "Apa Itu 'Amplitude' (Amplitudo)?", "id": "Tinggi Maksimal Gelombang Sinyal Listrik." },
    { "en": "Apa Itu 'Phase' (Fase)?", "id": "Posisi Sudut Gelombang Pada Waktu." },
    { "en": "Apa Itu 'Modulation' (Modulasi)?", "id": "Penumpangan Informasi Ke Sinyal Pembawa." },
    { "en": "Apa Itu 'Demodulation' (Demodulasi)?", "id": "Pemisahan Informasi Dari Sinyal Pembawa." },
    { "en": "Apa Itu 'AM' (Amplitude Modulation)?", "id": "Modulasi Melalui Perubahan Amplitudo Sinyal." },
    { "en": "Apa Itu 'FM' (Frequency Modulation)?", "id": "Modulasi Melalui Perubahan Frekuensi Sinyal." },
    { "en": "Apa Itu 'PM' (Phase Modulation)?", "id": "Modulasi Melalui Perubahan Fase Sinyal." },
    { "en": "Apa Itu 'Baseband' (Pita Dasar)?", "id": "Sinyal Informasi Sebelum Proses Modulasi." },
    { "en": "Apa Itu 'Passband' (Pita Lolos)?", "id": "Sinyal Hasil Modulasi Siap Transmisi." },
    { "en": "Apa Itu 'Transmitter' (Pemancar)?", "id": "Perangkat Pengirim Sinyal Informasi Nirkabel." },
    { "en": "Apa Itu 'Receiver' (Penerima)?", "id": "Perangkat Penerima Sinyal Informasi Nirkabel." },
    { "en": "Apa Itu 'Transceiver' (Transiver)?", "id": "Gabungan Perangkat Pemancar Dan Penerima." },
    { "en": "Apa Itu 'Antenna' (Antena)?", "id": "Alat Pengubah Listrik Menjadi Radio." },
    { "en": "Apa Itu 'Monopole Antenna' (Antena Monopol)?", "id": "Antena Dengan Satu Lengan Penghantar." },
    { "en": "Apa Itu 'Parabolic Antenna' (Antena Parabola)?", "id": "Antena Reflektor Fokus Sinyal Kuat." },
    { "en": "Apa Itu 'Omnidirectional' (Segala Arah)?", "id": "Radiasi Antena Ke Seluruh Penjuru." },
    { "en": "Apa Itu 'Directional' (Pengarah)?", "id": "Radiasi Antena Fokus Arah Tertentu." },
    { "en": "Apa Itu 'Polarization' (Polarisasi)?", "id": "Orientasi Medan Listrik Gelombang Radio." },
    { "en": "Apa Itu 'Return Loss' (Rugi Balik)?", "id": "Daya Hilang Akibat Ketidaksesuaian Impedansi." },
    { "en": "Apa Itu 'Impedance Matching' (Penyesuaian Impedansi)?", "id": "Penyamaan Hambatan Transfer Daya Maksimal." },
    { "en": "Apa Itu 'Smith Chart' (Diagram Smith)?", "id": "Alat Kalkulasi Impedansi Frekuensi Radio." },
    { "en": "Apa Itu 'LNA' (Low Noise Amplifier)?", "id": "Penguat Sinyal Lemah Derau Rendah." },
    { "en": "Apa Itu 'PA' (Power Amplifier)?", "id": "Penguat Sinyal Sebelum Dikirim Antena." },
    { "en": "Apa Itu 'Mixer' (Pencampur)?", "id": "Alat Pengubah Frekuensi Sinyal Radio." },
    { "en": "Apa Itu 'Oscillator' (Osilator)?", "id": "Pembangkit Sinyal Gelombang Periodik Listrik." },
    { "en": "Apa Itu 'Filter' (Penyaring)?", "id": "Komponen Pemilih Rentang Frekuensi Tertentu." },
    { "en": "Apa Itu 'LPF' (Low Pass Filter)?", "id": "Penyaring Frekuensi Di Bawah Batas." },
    { "en": "Apa Itu 'HPF' (High Pass Filter)?", "id": "Penyaring Frekuensi Di Atas Batas." },
    { "en": "Apa Itu 'BPF' (Band Pass Filter)?", "id": "Penyaring Frekuensi Dalam Rentang Tertentu." },
    { "en": "Apa Itu 'Attenuator' (Peredam)?", "id": "Komponen Penurun Kekuatan Sinyal Radio." },
    { "en": "Apa Itu 'Coupler' (Kopler)?", "id": "Alat Pencuplik Sebagian Daya Sinyal." },
    { "en": "Apa Itu 'Sensitivity' (Sensitivitas)?", "id": "Sinyal Minimum Yang Dapat Dideteksi." },
    { "en": "Apa Itu 'Selectivity' (Selektivitas)?", "id": "Kemampuan Menolak Sinyal Frekuensi Lain." },
    { "en": "Apa Itu 'Propagation' (Propagasi)?", "id": "Proses Perambatan Gelombang Melalui Medium." },
    { "en": "Apa Itu 'LOS' (Line Of Sight)?", "id": "Komunikasi Tanpa Penghalang Antar Antena." },
    { "en": "Apa Itu 'Reflection' (Pemantulan)?", "id": "Pembalikan Sinyal Oleh Permukaan Benda." },
    { "en": "Apa Itu 'Diffraction' (Difraksi)?", "id": "Pembelokan Sinyal Di Sekitar Penghalang." },
    { "en": "Apa Itu 'Refraction' (Pembiasan)?", "id": "Perubahan Arah Sinyal Antar Medium." },
    { "en": "Apa Itu 'Scattering' (Hamburan)?", "id": "Penyebaran Sinyal Oleh Objek Kecil." },
    { "en": "Apa Itu 'Multipath' (Banyak Jalur)?", "id": "Sinyal Sampai Melalui Berbagai Rute." },
    { "en": "Apa Itu 'Fading' (Pudar Sinyal)?", "id": "Fluktuasi Kekuatan Sinyal Di Penerima." },
    { "en": "Apa Itu 'Doppler Effect' (Efek Doppler)?", "id": "Perubahan Frekuensi Akibat Pergerakan Objek." },
    { "en": "Apa Itu 'Link Budget' (Anggaran Tautan)?", "id": "Perhitungan Total Penguatan Dan Redaman." },
    { "en": "Apa Itu 'Friis Equation' (Persamaan Friis)?", "id": "Rumus Perhitungan Daya Terima Sinyal." },
    { "en": "Apa Itu 'Path Loss' (Redaman Jalur)?", "id": "Kehilangan Daya Sinyal Selama Merambat." },
    { "en": "Apa Itu 'Free Space Path Loss' (FSPL)?", "id": "Redaman Sinyal Di Ruang Hampa." },
    { "en": "Apa Itu 'Fresnel Zone' (Zona Fresnel)?", "id": "Area Elips Fokus Rambatan Gelombang." },
    { "en": "Apa Itu 'Beamforming' (Pembentukan Berkas)?", "id": "Pengarahan Sinyal Secara Elektronik Fokus." },
    { "en": "Apa Itu 'Diversity' (Diversitas)?", "id": "Teknik Penggunaan Banyak Jalur Sinyal." },
    { "en": "Apa Itu 'CDMA' (Code Division Multiple Access)?", "id": "Akses Banyak Pengguna Berbasis Kode." },
    { "en": "Apa Itu 'TDMA' (Time Division Multiple Access)?", "id": "Akses Banyak Pengguna Berbasis Waktu." },
    { "en": "Apa Itu 'FDMA' (Frequency Division Multiple Access)?", "id": "Akses Banyak Pengguna Berbasis Frekuensi." },
    { "en": "Apa Itu 'OFDMA' (Orthogonal FDMA)?", "id": "Teknik Akses Banyak Sub-Pembawa Modern." },
    { "en": "Apa Itu 'SDMA' (Space Division Multiple Access)?", "id": "Akses Banyak Pengguna Berbasis Ruang." },
    { "en": "Apa Itu 'Bluetooth' (Teknologi Bluetooth)?", "id": "Komunikasi Nirkabel Jarak Pendek Rendah." },
    { "en": "Apa Itu 'Wi-Fi' (Wireless Fidelity)?", "id": "Teknologi Jaringan Lokal Tanpa Kabel." },
    { "en": "Apa Itu 'Zigbee' (Protokol Zigbee)?", "id": "Jaringan Nirkabel Hemat Energi Industri." },
    { "en": "Apa Itu 'Signal Generator' (Generator Sinyal)?", "id": "Pembangkit Sinyal Radio Untuk Pengujian." },
    { "en": "Apa Itu 'Wattmeter RF' (Alat Ukur Daya)?", "id": "Alat Ukur Daya Sinyal Radio." },
    { "en": "Apa Itu 'Shielding' (Pelindung)?", "id": "Lapisan Logam Penahan Gangguan Luar." },
    { "en": "Apa Itu 'Skin Effect' (Efek Kulit)?", "id": "Arus Mengalir Di Permukaan Konduktor." },
    { "en": "Apa Itu 'ISM Band' (Industrial Scientific Medical)?", "id": "Pita Frekuensi Bebas Lisensi Umum." },
    { "en": "Apa Itu 'SDR' (Software Defined Radio)?", "id": "Sistem Komunikasi Berbasis Perangkat Lunak." },
    { "en": "Apa Itu 'Latency' (Latensi)?", "id": "Waktu Tunda Pengiriman Paket Data." },
    { "en": "Apa Itu 'Throughput' (Throughput)?", "id": "Laju Data Berhasil Dikirim Sebenarnya." },
    { "en": "Apa Itu 'Bit Rate' (Laju Bit)?", "id": "Jumlah Bit Dikirim Per Detik." },
    { "en": "Apa Itu 'Baud Rate' (Laju Baud)?", "id": "Jumlah Simbol Dikirim Per Detik." },
    { "en": "Apa Itu 'Cortex-M' (Seri Cortex-M)?", "id": "Prosesor ARM Khusus Mikrokontroler Industri." },
    { "en": "Apa Itu 'CMSIS' (Cortex Microcontroller Software Interface Standard)?", "id": "Standar Abstraksi Perangkat Lunak ARM." },
    { "en": "Apa Itu 'NVIC' (Nested Vectored Interrupt Controller)?", "id": "Pengelola Prioritas Interupsi Perangkat Keras." },
    { "en": "Apa Itu 'SysTick' (System Tick Timer)?", "id": "Timer Standar Untuk Sistem Operasi." },
    { "en": "Apa Itu 'Thumb' (Instruksi Thumb)?", "id": "Set Instruksi ARM Kompresi 16-Bit." },
    { "en": "Apa Itu 'Pipeline' (Jalur Pipa Prosesor)?", "id": "Teknik Eksekusi Banyak Instruksi Simultan." },
    { "en": "Apa Itu 'HardFault' (Kesalahan Fatal)?", "id": "Interupsi Akibat Kegagalan Sistem Serius." },
    { "en": "Apa Itu 'Stack Pointer' (Penunjuk Tumpukan)?", "id": "Register Alamat Memori Tumpukan Aktif." },
    { "en": "Apa Itu 'Link Register' (Register Tautan)?", "id": "Penyimpan Alamat Kembali Setelah Fungsi." },
    { "en": "Apa Itu 'Program Counter' (Penghitung Program)?", "id": "Alamat Instruksi Yang Sedang Berjalan." },
    { "en": "Apa Itu 'FPU' (Floating Point Unit)?", "id": "Unit Pemroses Angka Desimal Cepat." },
    { "en": "Apa Itu 'MPU' (Memory Protection Unit)?", "id": "Unit Perlindungan Akses Memori Ilegal." },
    { "en": "Apa Itu 'WFI' (Wait For Interrupt)?", "id": "Instruksi Mode Hemat Daya Prosesor." },
    { "en": "Apa Itu 'Bit-Banding' (Pemetaan Bit)?", "id": "Akses Cepat Modifikasi Bit Tunggal." },
    { "en": "Apa Itu 'Embedded C' (Bahasa C Tertanam)?", "id": "Bahasa C Untuk Perangkat Keras." },
    { "en": "Apa Itu 'Volatile' (Kata Kunci Volatile)?", "id": "Mencegah Optimasi Kompilator Pada Variabel." },
    { "en": "Apa Itu 'Static' (Kata Kunci Statis)?", "id": "Menjaga Nilai Variabel Selama Program." },
    { "en": "Apa Itu 'Extern' (Kata Kunci Ekstern)?", "id": "Deklarasi Variabel Dari Berkas Lain." },
    { "en": "Apa Itu 'Const' (Kata Kunci Konstanta)?", "id": "Menandai Data Yang Tidak Berubah." },
    { "en": "Apa Itu 'Pointer' (Penunjuk Memori)?", "id": "Variabel Penyimpan Alamat Memori Lain." },
    { "en": "Apa Itu 'Dereferencing' (Dereferensi Pointer)?", "id": "Mengambil Nilai Dari Alamat Penunjuk." },
    { "en": "Apa Itu 'Struct' (Struktur Data)?", "id": "Kumpulan Variabel Dengan Tipe Berbeda." },
    { "en": "Apa Itu 'Union' (Perserikatan Memori)?", "id": "Variabel Berbagi Lokasi Memori Sama." },
    { "en": "Apa Itu 'Typedef' (Pendefinisian Tipe)?", "id": "Pemberian Nama Baru Tipe Data." },
    { "en": "Apa Itu 'Enum' (Enumerasi)?", "id": "Tipe Data Berupa Daftar Konstanta." },
    { "en": "Apa Itu 'Bitwise AND' (Operator AND)?", "id": "Operasi Logika Dan Per Bit." },
    { "en": "Apa Itu 'Bitwise OR' (Operator OR)?", "id": "Operasi Logika Atau Per Bit." },
    { "en": "Apa Itu 'Bitwise XOR' (Operator XOR)?", "id": "Operasi Logika Eksklusif Per Bit." },
    { "en": "Apa Itu 'Bit Shifting' (Pergeseran Bit)?", "id": "Menggeser Posisi Bit Kiri Kanan." },
    { "en": "Apa Itu 'Masking' (Penopengan Bit)?", "id": "Isolasi Bit Tertentu Dalam Register." },
    { "en": "Apa Itu 'Interrupt Driven' (Berbasis Interupsi)?", "id": "Program Berjalan Berdasarkan Sinyal Luar." },
    { "en": "Apa Itu 'Polling' (Pengambilan Suara)?", "id": "Pengecekan Status Perangkat Secara Berulang." },
    { "en": "Apa Itu 'Race Condition' (Kondisi Balapan)?", "id": "Konflik Akses Data Dua Tugas." },
    { "en": "Apa Itu 'Critical Section' (Bagian Kritis)?", "id": "Kode Yang Tidak Boleh Terinterupsi." },
    { "en": "Apa Itu 'Atomic Operation' (Operasi Atomik)?", "id": "Instruksi Yang Tidak Bisa Terbagi." },
    { "en": "Apa Itu 'Mutex' (Mutual Exclusion)?", "id": "Pengunci Sumber Daya Antar Tugas." },
    { "en": "Apa Itu 'Semaphore' (Semafor)?", "id": "Penanda Ketersediaan Sumber Daya Sistem." },
    { "en": "Apa Itu 'Heap' (Memori Heap)?", "id": "Area Alokasi Memori Dinamis Program." },
    { "en": "Apa Itu 'Stack' (Memori Tumpukan)?", "id": "Area Alokasi Variabel Lokal Fungsi." },
    { "en": "Apa Itu 'Memory Leak' (Kebocoran Memori)?", "id": "Kegagalan Melepas Alokasi Memori Dinamis." },
    { "en": "Apa Itu 'Buffer Overflow' (Luberan Penyangga)?", "id": "Data Melebihi Batas Alokasi Memori." },
    { "en": "Apa Itu 'Callback Function' (Fungsi Panggil Balik)?", "id": "Fungsi Yang Dipanggil Oleh Fungsi." },
    { "en": "Apa Itu 'Inline Function' (Fungsi Inlain)?", "id": "Penyalinan Kode Fungsi Secara Langsung." },
    { "en": "Apa Itu 'Macro' (Makro Praprosesor)?", "id": "Substitusi Teks Sebelum Proses Kompilasi." },
    { "en": "Apa Itu 'Include Guard' (Pelindung Inklusi)?", "id": "Mencegah Pendefinisian Ganda Berkas Header." },
    { "en": "Apa Itu 'Linker Script' (Skrip Penaut)?", "id": "Pengatur Pemetaan Kode Dalam Memori." },
    { "en": "Apa Itu 'Map File' (Berkas Peta)?", "id": "Laporan Penggunaan Alamat Memori Program." },
    { "en": "Apa Itu 'Standard Peripheral Library' (SPL)?", "id": "Kumpulan Fungsi Pengendali Perangkat Keras." },
    { "en": "Apa Itu 'HAL' (Hardware Abstraction Layer)?", "id": "Lapisan Antarmuka Kode Dan Hardware." },
    { "en": "Apa Itu 'LL Driver' (Low Layer Driver)?", "id": "Driver Akses Register Secara Langsung." },
    { "en": "Apa Itu 'Flash Latency' (Latensi Flash)?", "id": "Waktu Tunggu Baca Memori Flash." },
    { "en": "Apa Itu 'Wait State' (Status Tunggu)?", "id": "Detak Kosong Sinkronisasi Memori Cepat." },
    { "en": "Apa Itu 'Brown-Out Reset' (BOR)?", "id": "Reset Otomatis Saat Tegangan Turun." },
    { "en": "Apa Itu 'Watchdog' (Anjing Penjaga)?", "id": "Timer Pencegah Sistem Berhenti Bekerja." },
    { "en": "Apa Itu 'DMA' (Direct Memory Access)?", "id": "Penyalinan Data Tanpa Beban CPU." },
    { "en": "Apa Itu 'UART' (Universal Asynchronous Receiver Transmitter)?", "id": "Komunikasi Serial Data Tanpa Detak." },
    { "en": "Apa Itu 'Baud Rate' (Laju Baud)?", "id": "Kecepatan Bit Per Detik Serial." },
    { "en": "Apa Itu 'I2C' (Inter-Integrated Circuit)?", "id": "Protokol Komunikasi Dua Kabel Sederhana." },
    { "en": "Apa Itu 'Pull-Up Resistor' (Resistor Tarik Atas)?", "id": "Penjaga Status Logika Tinggi Jalur." },
    { "en": "Apa Itu 'Open Drain' (Saluran Terbuka)?", "id": "Konfigurasi Output Membutuhkan Resistor Eksternal." },
    { "en": "Apa Itu 'Push-Pull' (Dorong Tarik)?", "id": "Konfigurasi Output Penggerak Arus Dua." },
    { "en": "Apa Itu 'ADC' (Analog To Digital Converter)?", "id": "Pengubah Sinyal Fisik Menjadi Angka." },
    { "en": "Apa Itu 'Sampling Time' (Waktu Cuplikan)?", "id": "Durasi Pengambilan Data Sinyal Analog." },
    { "en": "Apa Itu 'Resolution' (Resolusi ADC)?", "id": "Tingkat Ketelitian Angka Hasil Konversi." },
    { "en": "Apa Itu 'DAC' (Digital To Analog Converter)?", "id": "Pengubah Angka Menjadi Sinyal Listrik." },
    { "en": "Apa Itu 'PWM' (Pulse Width Modulation)?", "id": "Sinyal Kotak Kendali Daya Rata-Rata." },
    { "en": "Apa Itu 'Duty Cycle' (Siklus Kerja)?", "id": "Persentase Waktu Aktif Sinyal PWM." },
    { "en": "Apa Itu 'ICSP' (In-Circuit Serial Programming)?", "id": "Metode Pemrograman Chip Dalam Rangkaian." },
    { "en": "Apa Itu 'JTAG' (Joint Test Action Group)?", "id": "Antarmuka Pengujian Dan Debugging Industri." },
    { "en": "Apa Itu 'SWD' (Serial Wire Debug)?", "id": "Antarmuka Debugging ARM Dua Pin." },
    { "en": "Apa Itu 'Semihosting' (Semihosting)?", "id": "Input Output Konsol Lewat Debugger." },
    { "en": "Apa Itu 'Breakpoint' (Titik Henti)?", "id": "Penanda Penghenti Eksekusi Kode Debug." },
    { "en": "Apa Itu 'Watchpoint' (Titik Pantau)?", "id": "Pemicu Berhenti Saat Variabel Berubah." },
    { "en": "Apa Itu 'Variable Shadowing' (Pembayangan Variabel)?", "id": "Nama Variabel Sama Beda Cakupan." },
    { "en": "Apa Itu 'Scope' (Cakupan Variabel)?", "id": "Area Keberlakuan Variabel Dalam Kode." },
    { "en": "Apa Itu 'Global Variable' (Variabel Global)?", "id": "Variabel Akses Seluruh Bagian Program." },
    { "en": "Apa Itu 'Local Variable' (Variabel Lokal)?", "id": "Variabel Hanya Berlaku Dalam Fungsi." },
    { "en": "Apa Itu 'Register Variable' (Variabel Register)?", "id": "Saran Penyimpanan Variabel Dalam CPU." },
    { "en": "Apa Itu 'Alignment' (Penyelarasan Data)?", "id": "Penyimpanan Data Berdasarkan Batas Alamat." },
    { "en": "Apa Itu 'Padding' (Bantalan Memori)?", "id": "Byte Kosong Untuk Penyelarasan Memori." },
    { "en": "Apa Itu 'Endianness' (Endianitas)?", "id": "Urutan Penyimpanan Byte Dalam Memori." },
    { "en": "Apa Itu 'Optimization' (Optimasi Kode)?", "id": "Proses Kompilator Mengefisiensikan Program Mesin." },
    { "en": "Apa Itu 'Dead Code' (Kode Mati)?", "id": "Bagian Kode Yang Tidak Dieksekusi." },
    { "en": "Apa Itu 'Library' (Pustaka Program)?", "id": "Kumpulan Fungsi Siap Pakai Pengembang." },
    { "en": "Apa Itu 'API' (Application Programming Interface)?", "id": "Antarmuka Penghubung Antar Modul Software." },
    { "en": "Apa Itu 'Reentrancy' (Reentransi)?", "id": "Fungsi Aman Dipanggil Secara Bersamaan." },
    { "en": "Apa Itu 'Recursion' (Rekursi)?", "id": "Fungsi Yang Memanggil Dirinya Sendiri." },
    { "en": "Apa Itu 'Lookup Table' (Tabel Cari)?", "id": "Data Pre-Kalkulasi Untuk Kecepatan Proses." },
    { "en": "Apa Itu 'Firmware' (Perangkat Tegar)?", "id": "Software Permanen Di Dalam Hardware." },
    { "en": "Apa Itu 'Bootloader' (Pemuat Bot)?", "id": "Program Awal Pemanggil Aplikasi Utama." },
    { "en": "Apa Itu 'Vector Table' (Tabel Vektor)?", "id": "Daftar Alamat Penanganan Interupsi Sistem." },
    { "en": "Apa Itu 'Reset Vector' (Vektor Reset)?", "id": "Alamat Awal Eksekusi Setelah Reset." },
    { "en": "Apa Itu 'Main Loop' (Loop Utama)?", "id": "Perulangan Tak Terbatas Program Utama." },
    { "en": "Apa Itu 'Middleware' (Perangkat Menengah)?", "id": "Software Penghubung Aplikasi Dan OS." },
    { "en": "Apa Itu 'Abstraction' (Abstraksi)?", "id": "Penyederhanaan Kompleksitas Sistem Perangkat Keras." },
    { "en": "Apa Itu 'Substation' (Gardu Induk)?", "id": "Pusat Pengatur Tegangan Jaringan Listrik." },
    { "en": "Apa Itu 'Step-Up Substation' (Gardu Penaik Tegangan)?", "id": "Peningkat Tegangan Generator Menuju Transmisi." },
    { "en": "Apa Itu 'Step-Down Substation' (Gardu Penurun Tegangan)?", "id": "Penurun Tegangan Transmisi Menuju Distribusi." },
    { "en": "Apa Itu 'Switchyard' (Pelataran Hubung)?", "id": "Area Terbuka Penempatan Komponen Utama." },
    { "en": "Apa Itu 'Busbar' (Rel Daya)?", "id": "Penghantar Utama Arus Besar Gardu." },
    { "en": "Apa Itu 'Circuit Breaker' (Pemutus Tenaga)?", "id": "Saklar Pemutus Arus Beban Gangguan." },
    { "en": "Apa Itu 'Disconnecting Switch' (Saklar Pemisah)?", "id": "Pemisah Rangkaian Listrik Kondisi Mati." },
    { "en": "Apa Itu 'Lightning Arrester' (Penangkap Petir)?", "id": "Pelindung Peralatan Dari Surja Petir." },
    { "en": "Apa Itu 'Current Transformer' (Transformator Arus)?", "id": "Alat Penurun Arus Untuk Pengukuran." },
    { "en": "Apa Itu 'Potential Transformer' (Transformator Tegangan)?", "id": "Alat Penurun Tegangan Untuk Pengukuran." },
    { "en": "Apa Itu 'Power Transformer' (Transformator Daya)?", "id": "Alat Pengubah Level Tegangan Utama." },
    { "en": "Apa Itu 'Autotransformer' (Transformator Otomatis)?", "id": "Transformator Dengan Satu Lilitan Bersama." },
    { "en": "Apa Itu 'Tap Changer' (Pengubah Sadapan)?", "id": "Pengatur Rasio Tegangan Keluaran Transformator." },
    { "en": "Apa Itu 'OLTC' (On-Load Tap Changer)?", "id": "Pengubah Sadapan Saat Kondisi Berbeban." },
    { "en": "Apa Itu 'Conservator Tank' (Tangki Konservator)?", "id": "Ruang Ekspansi Minyak Isolasi Transformator." },
    { "en": "Apa Itu 'Buchholz Relay' (Relay Buchholz)?", "id": "Proteksi Internal Transformator Gangguan Gas." },
    { "en": "Apa Itu 'Silica Gel' (Gel Silika)?", "id": "Penyerap Kelembapan Udara Masuk Transformator." },
    { "en": "Apa Itu 'Bushing' (Bosing)?", "id": "Isolator Penghubung Kumparan Luar Tangki." },
    { "en": "Apa Itu 'Secondary Circuit' (Sirkuit Sekunder)?", "id": "Jalur Listrik Alat Ukur Proteksi." },
    { "en": "Apa Itu 'Control Panel' (Panel Kendali)?", "id": "Pusat Pengaturan Operasi Gardu Induk." },
    { "en": "Apa Itu 'Transmission Line' (Saluran Transmisi)?", "id": "Penyalur Energi Listrik Jarak Jauh." },
    { "en": "Apa Itu 'SUTET' (Saluran Udara Tegangan Ekstra Tinggi)?", "id": "Transmisi Listrik Diatas Lima Ratus." },
    { "en": "Apa Itu 'SUTT' (Saluran Udara Tegangan Tinggi)?", "id": "Transmisi Listrik Tegangan Tinggi Udara." },
    { "en": "Apa Itu 'SKTT' (Saluran Kabel Tegangan Tinggi)?", "id": "Transmisi Listrik Melalui Kabel Tanah." },
    { "en": "Apa Itu 'Insulator Stack' (Susunan Isolator)?", "id": "Penyangga Kabel Penghambat Arus Tanah." },
    { "en": "Apa Itu 'Suspension Insulator' (Isolator Gantung)?", "id": "Isolator Penahan Beban Berat Kabel." },
    { "en": "Apa Itu 'Tension Insulator' (Isolator Tarik)?", "id": "Isolator Penahan Beban Sudut Transmisi." },
    { "en": "Apa Itu 'Corona Ring' (Cincin Korona)?", "id": "Alat Pendistribusi Medan Listrik Isolator." },
    { "en": "Apa Itu 'Shield Wire' (Kawat Pelindung)?", "id": "Kawat Penangkap Petir Jalur Transmisi." },
    { "en": "Apa Itu 'Counterpoise' (Kawat Pentanahan)?", "id": "Sistem Pentanahan Tambahan Kaki Menara." },
    { "en": "Apa Itu 'Sag' (Lendutan)?", "id": "Jarak Kabel Terendah Terhadap Tanah." },
    { "en": "Apa Itu 'Span' (Rentang)?", "id": "Jarak Horizontal Antara Dua Menara." },
    { "en": "Apa Itu 'ACSR' (Aluminum Conductor Steel Reinforced)?", "id": "Kabel Aluminium Dengan Inti Baja." },
    { "en": "Apa Itu 'Bundle Conductor' (Konduktor Berkas)?", "id": "Penggunaan Banyak Kabel Per Fase." },
    { "en": "Apa Itu 'Proximity Effect' (Efek Kedekatan)?", "id": "Interferensi Arus Antar Kabel Dekat." },
    { "en": "Apa Itu 'Surge Impedance' (Impedansi Surja)?", "id": "Karakteristik Hambatan Alami Jalur Transmisi." },
    { "en": "Apa Itu 'SIL' (Surge Impedance Loading)?", "id": "Daya Transmisi Beban Tanpa Reaktif." },
    { "en": "Apa Itu 'Reactive Compensation' (Kompensasi Reaktif)?", "id": "Penyeimbangan Daya Reaktif Jalur Transmisi." },
    { "en": "Apa Itu 'Shunt Reactor' (Reaktor Shunt)?", "id": "Penyerap Daya Reaktif Kapasitif Berlebih." },
    { "en": "Apa Itu 'Series Capacitor' (Kapasitor Seri)?", "id": "Kapasitor Penurun Reaktansi Jalur Transmisi." },
    { "en": "Apa Itu 'SVC' (Static Var Compensator)?", "id": "Kompensator Reaktif Cepat Berbasis Tiristor." },
    { "en": "Apa Itu 'STATCOM' (Static Synchronous Compensator)?", "id": "Penstabil Tegangan Berbasis Inverter Cerdas." },
    { "en": "Apa Itu 'Short Circuit Analysis' (Analisis Hubung Singkat)?", "id": "Perhitungan Arus Gangguan Sistem Tenaga." },
    { "en": "Apa Itu 'Stability Analysis' (Analisis Stabilitas)?", "id": "Evaluasi Kemampuan Sistem Menahan Gangguan." },
    { "en": "Apa Itu 'Voltage Stability' (Stabilitas Tegangan)?", "id": "Kemampuan Jaringan Mempertahankan Tegangan Nominal." },
    { "en": "Apa Itu 'Frequency Stability' (Stabilitas Frekuensi)?", "id": "Kemampuan Menjaga Frekuensi Tetap Stabil." },
    { "en": "Apa Itu 'Automatic Generation Control' (AGC)?", "id": "Pengatur Output Pembangkit Secara Otomatis." },
    { "en": "Apa Itu 'Remote Terminal Unit' (RTU)?", "id": "Unit Data Lapangan Pengirim Sinyal." },
    { "en": "Apa Itu 'Main Control Center' (Pusat Kendali)?", "id": "Tempat Pengaturan Seluruh Sistem Tenaga." },
    { "en": "Apa Itu 'Energy Management System' (EMS)?", "id": "Sistem Optimalisasi Operasi Jaringan Listrik." },
    { "en": "Apa Itu 'Black Start' (Mulai Gelap)?", "id": "Pemulihan Sistem Tanpa Daya Luar." },
    { "en": "Apa Itu 'Load Shedding' (Pelepasan Beban)?", "id": "Pemutusan Beban Darurat Jaga Stabilitas." },
    { "en": "Apa Itu 'Islanding' (Operasi Berpulau)?", "id": "Pembangkit Beroperasi Terpisah Dari Jaringan." },
    { "en": "Apa Itu 'Synchrophasor' (Sinkrofasor)?", "id": "Alat Ukur Fasor Terintegrasi Waktu." },
    { "en": "Apa Itu 'Wide Area Monitoring' (WAMS)?", "id": "Pemantauan Jaringan Listrik Skala Luas." },
    { "en": "Apa Itu 'Harmonic Distortion' (Distorsi Harmonisa)?", "id": "Cacat Gelombang Akibat Beban Non-Linier." },
    { "en": "Apa Itu 'Voltage Dip' (Tegangan Kedip)?", "id": "Penurunan Tegangan Singkat Jaringan Listrik." },
    { "en": "Apa Itu 'Voltage Swell' (Tegangan Lonjak)?", "id": "Kenaikan Tegangan Singkat Jaringan Listrik." },
    { "en": "Apa Itu 'Vector Group' (Grup Vektor)?", "id": "Hubungan Fase Antar Lilitan Transformator." },
    { "en": "Apa Itu 'Grounding System' (Sistem Pentanahan)?", "id": "Penyalur Arus Gangguan Menuju Bumi." },
    { "en": "Apa Itu 'Earth Resistance' (Hambatan Tanah)?", "id": "Nilai Resistansi Elektroda Terhadap Bumi." },
    { "en": "Apa Itu 'Mesh Grounding' (Pentanahan Jala)?", "id": "Konfigurasi Tanah Luas Bawah Gardu." },
    { "en": "Apa Itu 'Relay Proteksi' (Protective Relay)?", "id": "Alat Pendeteksi Gangguan Jaringan Listrik." },
    { "en": "Apa Itu 'Overcurrent Relay' (Relay Arus Lebih)?", "id": "Proteksi Saat Arus Melebihi Batas." },
    { "en": "Apa Itu 'Earth Fault Relay' (Relay Gangguan Tanah)?", "id": "Proteksi Kebocoran Arus Menuju Bumi." },
    { "en": "Apa Itu 'Lockout Relay' (Relay Pengunci)?", "id": "Relay Pencegah Penutupan Breaker Otomatis." },
    { "en": "Apa Itu 'Circuit Breaker Failure' (Kegagalan CB)?", "id": "Proteksi Saat Breaker Gagal Memutuskan." },
    { "en": "Apa Itu 'Carrier Communication' (Komunikasi Pembawa)?", "id": "Transmisi Sinyal Melalui Kabel Listrik." },
    { "en": "Apa Itu 'Wave Trap' (Perangkap Gelombang)?", "id": "Penyaring Sinyal Komunikasi Dari Jaringan." },
    { "en": "Apa Itu 'OPGW' (Optical Ground Wire)?", "id": "Kawat Tanah Berisi Serat Optik." },
    { "en": "Apa Itu 'Substation Automation' (Otomasi Gardu)?", "id": "Sistem Kendali Gardu Berbasis Digital." },
    { "en": "Apa Itu 'IEC 61850' (Standar Komunikasi Gardu)?", "id": "Protokol Standar Otomasi Gardu Global." },
    { "en": "Apa Itu 'GOOSE' (Generic Object Oriented Substation Event)?", "id": "Pesan Cepat Antar Perangkat Proteksi." },
    { "en": "Apa Itu 'Merging Unit' (Unit Penggabung)?", "id": "Pengubah Sinyal Analog Menjadi Digital." },
    { "en": "Apa Itu 'LOTO' (Lockout Tagout)?", "id": "Prosedur Penguncian Keamanan Perbaikan Listrik." },
    { "en": "Apa Itu 'Safety Distance' (Jarak Aman)?", "id": "Batas Jarak Minimum Terhadap Tegangan." },
    { "en": "Apa Itu 'Insulation Coordination' (Koordinasi Isolasi)?", "id": "Penentuan Level Ketahanan Isolasi Sistem." },
    { "en": "Apa Itu 'Basic Insulation Level' (BIL)?", "id": "Batas Ketahanan Isolasi Terhadap Surja." },
    { "en": "Apa Itu 'Transformer Oil Test' (Uji Minyak)?", "id": "Evaluasi Karakteristik Isolasi Cair Transformator." },
    { "en": "Apa Itu 'Tan Delta Test' (Uji Tan Delta)?", "id": "Pengukuran Kualitas Isolasi Peralatan Listrik." },
    { "en": "Apa Itu 'Partial Discharge' (Pelepasan Parsial)?", "id": "Kegagalan Isolasi Kecil Internal Peralatan." }



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
