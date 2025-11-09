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


  { "en": "Apa Itu Resistor?", "id": "Komponen Penghambat Arus Listrik." },
  { "en": "Apa Fungsi Resistor?", "id": "Membatasi Dan Mengatur Arus Listrik." },
  { "en": "Apa Satuan Resistansi?", "id": "Satuan Resistansi Adalah Ohm (Ω)." },
  { "en": "Apa Simbol Resistor Tetap?", "id": "Garis Bergerigi Atau Persegi Panjang." },
  { "en": "Apa Jenis Resistor?", "id": "Tetap, Variabel, Dan Non-Linear." },
  { "en": "Apa Itu Resistor Variabel?", "id": "Resistor Yang Nilainya Dapat Diubah." },
  { "en": "Contoh Resistor Variabel?", "id": "Potensiometer Dan Trimpot." },
  { "en": "Apa Itu Potensiometer?", "id": "Resistor Variabel Tiga Terminal." },
  { "en": "Apa Itu LDR (Light Dependent Resistor)?", "id": "Resistor Yang Peka Terhadap Cahaya." },
  { "en": "Apa Itu NTC (Negative Temperature Coefficient)?", "id": "Resistor Peka Suhu Bernilai Negatif." },
  { "en": "Apa Itu PTC (Positive Temperature Coefficient)?", "id": "Resistor Peka Suhu Bernilai Positif." },
  { "en": "Bagaimana Cara Membaca Resistor Gelang Warna?", "id": "Menggunakan Kode Gelang Warna Standar." },
  { "en": "Apa Warna Gelang Toleransi 5 Persen?", "id": "Warna Emas." },
  { "en": "Apa Warna Gelang Toleransi 10 Persen?", "id": "Warna Perak." },
  { "en": "Apa Itu Kapasitor?", "id": "Komponen Penyimpan Muatan Listrik." },
  { "en": "Apa Fungsi Kapasitor?", "id": "Menyimpan Energi Listrik Sementara." },
  { "en": "Apa Satuan Kapasitansi?", "id": "Satuan Kapasitansi Adalah Farad (F)." },
  { "en": "Apa Simbol Kapasitor Non-Polar?", "id": "Dua Garis Vertikal Sejajar." },
  { "en": "Apa Simbol Kapasitor Polar?", "id": "Satu Garis Lurus Dan Satu Melengkung." },
  { "en": "Apa Dua Jenis Kapasitor Utama?", "id": "Kapasitor Polar Dan Non-Polar." },
  { "en": "Contoh Kapasitor Polar?", "id": "Kapasitor Elektrolit (Elco)." },
  { "en": "Apa Itu Elco (Electrolytic Capacitor)?", "id": "Kapasitor Yang Memiliki Polaritas." },
  { "en": "Apa Tanda Kaki Negatif Elco?", "id": "Tanda Garis Putih Di Badan." },
  { "en": "Contoh Kapasitor Non-Polar?", "id": "Keramik, Milar, Kertas." },
  { "en": "Apa Fungsi Kapasitor Sebagai Filter?", "id": "Meloloskan Frekuensi Tertentu." },
  { "en": "Apa Itu Induktor?", "id": "Komponen Penyimpan Energi Magnetik." },
  { "en": "Apa Fungsi Induktor?", "id": "Menyimpan Energi Dalam Medan Magnet." },
  { "en": "Apa Satuan Induktansi?", "id": "Satuan Induktansi Adalah Henry (H)." },
  { "en": "Apa Simbol Induktor?", "id": "Simbol Kumparan Kawat Melingkar." },
  { "en": "Induktor Terbuat Dari Apa?", "id": "Lilitan Kawat Tembaga." },
  { "en": "Apa Saja Jenis Inti Induktor?", "id": "Inti Udara, Inti Besi, Inti Ferit." },
  { "en": "Apa Fungsi Inti Pada Induktor?", "id": "Meningkatkan Nilai Induktansi." },
  { "en": "Apa Itu Dioda?", "id": "Komponen Semikonduktor Dua Terminal." },
  { "en": "Apa Fungsi Utama Dioda?", "id": "Penyearah Arus Listrik." },
  { "en": "Bagaimana Cara Kerja Dioda?", "id": "Melewatkan Arus Ke Satu Arah Saja." },
  { "en": "Apa Nama Terminal Dioda?", "id": "Anoda (Positif) Dan Katoda (Negatif)." },
  { "en": "Apa Tanda Terminal Katoda Dioda?", "id": "Tanda Garis Cincin Di Badan." },
  { "en": "Apa Itu Forward Bias (Panjar Maju)?", "id": "Dioda Menghantarkan Arus Listrik." },
  { "en": "Apa Itu Reverse Bias (Panjar Mundur)?", "id": "Dioda Menghambat Arus Listrik." },
  { "en": "Dioda Terbuat Dari Bahan Apa?", "id": "Silikon Atau Germanium." },
  { "en": "Apa Itu Dioda Zener?", "id": "Dioda Penstabil Tegangan." },
  { "en": "Apa Fungsi Dioda Zener?", "id": "Menjaga Tegangan Output Tetap Stabil." },
  { "en": "Apa Itu LED (Light Emitting Diode)?", "id": "Dioda Yang Memancarkan Cahaya." },
  { "en": "Kaki Panjang LED Terminal Apa?", "id": "Terminal Positif (Anoda)." },
  { "en": "Kaki Pendek LED Terminal Apa?", "id": "Terminal Negatif (Katoda)." },
  { "en": "Apa Itu Dioda Schottky?", "id": "Dioda Dengan Tegangan Maju Rendah." },
  { "en": "Apa Itu Dioda Bridge?", "id": "Empat Dioda Penyearah Gelombang Penuh." },
  { "en": "Apa Fungsi Dioda Bridge?", "id": "Mengubah Arus AC Menjadi DC." },
  { "en": "Apa Itu Photodiode (Dioda Foto)?", "id": "Dioda Yang Peka Terhadap Cahaya." },
  { "en": "Apa Itu Transistor?", "id": "Komponen Semikonduktor Tiga Terminal." },
  { "en": "Apa Fungsi Utama Transistor?", "id": "Sebagai Saklar Dan Penguat Sinyal." },
  { "en": "Apa Dua Jenis Utama Transistor?", "id": "BJT (Bipolar) Dan FET (Unipolar)." },
  { "en": "Apa Kepanjangan BJT (Bipolar Junction Transistor)?", "id": "Transistor Persimpangan Bipolar." },
  { "en": "Apa Saja Kaki Transistor BJT?", "id": "Basis (Base), Kolektor (Collector), Emitor (Emitter)." },
  { "en": "Apa Dua Tipe Transistor BJT?", "id": "Tipe NPN Dan Tipe PNP." },
  { "en": "Apa Fungsi Kaki Basis BJT?", "id": "Mengontrol Arus Antara Kolektor Emitor." },
  { "en": "Apa Kepanjangan FET (Field Effect Transistor)?", "id": "Transistor Efek Medan." },
  { "en": "Apa Saja Kaki Transistor FET?", "id": "Gate, Drain, Dan Source." },
  { "en": "Apa Fungsi Kaki Gate FET?", "id": "Mengontrol Arus Antara Drain Source." },
  { "en": "Apa Itu JFET (Junction Field Effect Transistor)?", "id": "FET Yang Menggunakan Persimpangan PN." },
  { "en": "Apa Itu MOSFET (Metal Oxide Semiconductor FET)?", "id": "FET Dengan Gerbang Oksida Logam." },
  { "en": "Kapan Transistor Berfungsi Sebagai Saklar?", "id": "Saat Kondisi Jenuh Dan Cut-Off." },
  { "en": "Kapan Transistor Berfungsi Sebagai Penguat?", "id": "Saat Berada Di Daerah Aktif." },
  { "en": "Apa Itu IC (Integrated Circuit)?", "id": "Rangkaian Elektronik Terintegrasi Dalam Chip." },
  { "en": "Apa Nama Lain IC (Integrated Circuit)?", "id": "Chip Atau Microchip." },
  { "en": "Contoh IC Digital?", "id": "Gerbang Logika, Mikrokontroler, Memori." },
  { "en": "Contoh IC Analog?", "id": "Op-Amp, Regulator Tegangan." },
  { "en": "Apa Kepanjangan Op-Amp (Operational Amplifier)?", "id": "Penguat Operasional." },
  { "en": "Apa Fungsi Utama Op-Amp?", "id": "Penguat Sinyal Listrik Diferensial." },
  { "en": "Apa Itu Saklar (Switch)?", "id": "Komponen Pemutus Dan Penyambung Arus." },
  { "en": "Apa Itu SPST (Single Pole Single Throw)?", "id": "Saklar On-Off Satu Kutub." },
  { "en": "Apa Itu SPDT (Single Pole Double Throw)?", "id": "Saklar Pemilih Satu Kutub Dua Arah." },
  { "en": "Apa Itu Relay?", "id": "Saklar Elektromekanis Berbasis Elektromagnet." },
  { "en": "Bagian Utama Relay?", "id": "Kumparan (Coil) Dan Kontak (Contact)." },
  { "en": "Apa Fungsi Kumparan Relay?", "id": "Menciptakan Medan Magnet." },
  { "en": "Apa Fungsi Kontak Relay?", "id": "Memutus Atau Menghubungkan Arus Beban." },
  { "en": "Apa Itu Transformator (Trafo)?", "id": "Komponen Penaik Atau Penurun Tegangan AC." },
  { "en": "Prinsip Kerja Transformator?", "id": "Induksi Elektromagnetik." },
  { "en": "Apa Itu Trafo Step-Up?", "id": "Transformator Penaik Tegangan." },
  { "en": "Apa Itu Trafo Step-Down?", "id": "Transformator Penurun Tegangan." },
  { "en": "Apa Itu PCB (Printed Circuit Board)?", "id": "Papan Sirkuit Cetak." },
  { "en": "Apa Fungsi PCB (Printed Circuit Board)?", "id": "Menghubungkan Komponen Secara Fisik." },
  { "en": "Apa Itu Arus Listrik?", "id": "Aliran Muatan Listrik (Elektron)." },
  { "en": "Apa Satuan Arus Listrik?", "id": "Ampere (A)." },
  { "en": "Apa Itu Tegangan Listrik?", "id": "Beda Potensial Antara Dua Titik." },
  { "en": "Apa Satuan Tegangan Listrik?", "id": "Volt (V)." },
  { "en": "Apa Itu Hambatan Listrik (Resistansi)?", "id": "Ukuran Penolakan Terhadap Arus." },
  { "en": "Apa Satuan Hambatan Listrik?", "id": "Ohm (Ω)." },
  { "en": "Apa Bunyi Hukum Ohm?", "id": "Tegangan Sebanding Arus Dan Hambatan." },
  { "en": "Apa Rumus Hukum Ohm?", "id": "V = I x R." },
  { "en": "Apa Itu Daya Listrik (Power)?", "id": "Laju Energi Listrik Yang Digunakan." },
  { "en": "Apa Satuan Daya Listrik?", "id": "Watt (W)." },
  { "en": "Apa Rumus Daya Listrik?", "id": "P = V x I." },
  { "en": "Apa Itu Rangkaian Seri?", "id": "Komponen Terhubung Berurutan Satu Jalur." },
  { "en": "Apa Itu Rangkaian Paralel?", "id": "Komponen Terhubung Berdampingan Bercabang." },
  { "en": "Apa Itu Arus DC (Direct Current)?", "id": "Arus Listrik Searah." },
  { "en": "Contoh Sumber Arus DC?", "id": "Baterai, Aki, Power Supply DC." },
  { "en": "Apa Itu Arus AC (Alternating Current)?", "id": "Arus Listrik Bolak-Balik." },
  { "en": "Contoh Sumber Arus AC?", "id": "Listrik PLN, Generator AC." },  
  { "en": "Apa Itu Resistor SMD (Surface Mount Device)?", "id": "Resistor Pemasangan Permukaan." },
  { "en": "Bagaimana Cara Membaca Resistor SMD Kode Angka?", "id": "Menggunakan Kode Angka Di Badan." },
  { "en": "Apa Arti Kode 103 Pada Resistor SMD?", "id": "10 Ditambah 3 Nol (10K Ohm)." },
  { "en": "Apa Arti Kode 4R7 Pada Resistor SMD?", "id": "Tanda R Adalah Koma (4,7 Ohm)." },
  { "en": "Apa Itu Power Rating (Daya) Resistor?", "id": "Batas Daya Maksimal Yang Diterima." },
  { "en": "Apa Itu Kapasitor SMD (Surface Mount Device)?", "id": "Kapasitor Pemasangan Permukaan." },
  { "en": "Apa Itu Kapasitor Tantalum?", "id": "Kapasitor Polar Dengan Efisiensi Tinggi." },
  { "en": "Apa Itu Varco (Variable Capacitor)?", "id": "Kapasitor Yang Nilainya Bisa Diatur." },
  { "en": "Apa Fungsi Varco (Variable Capacitor)?", "id": "Memilih Frekuensi Radio (Tuning)." },
  { "en": "Bagaimana Membaca Kode Kapasitor Keramik?", "id": "Menggunakan Kode Angka (PikoFarad)." },
  { "en": "Apa Arti Kode 104 Kapasitor Keramik?", "id": "10 Ditambah 4 Nol (100nF)." },
  { "en": "Apa Arti Kode 102 Kapasitor Keramik?", "id": "10 Ditambah 2 Nol (1nF)." },
  { "en": "Apa Itu Dioda Varactor (Varicap)?", "id": "Dioda Yang Bekerja Sebagai Kapasitor." },
  { "en": "Apa Fungsi Dioda Varactor?", "id": "Mengatur Frekuensi Osilator (VCO)." },
  { "en": "Apa Itu Dioda Laser?", "id": "Dioda Pemancar Cahaya Laser." },
  { "en": "Apa Itu Dioda Inframerah (Infrared)?", "id": "Dioda Pemancar Cahaya Inframerah." },
  { "en": "Di Mana Dioda Inframerah Sering Digunakan?", "id": "Remote Control Televisi." },
  { "en": "Apa Itu Transistor Darlington?", "id": "Gabungan Internal Dua Transistor BJT." },
  { "en": "Apa Keuntungan Transistor Darlington?", "id": "Penguatan Arus (Hfe) Sangat Tinggi." },
  { "en": "Apa Itu Phototransistor (Transistor Foto)?", "id": "Transistor Yang Dikontrol Oleh Cahaya." },
  { "en": "Apa Kepanjangan IGBT (Insulated Gate Bipolar Transistor)?", "id": "Transistor Bipolar Gerbang Terisolasi." },
  { "en": "Apa Fungsi IGBT (Insulated Gate Bipolar Transistor)?", "id": "Saklar Daya Tinggi Kecepatan Tinggi." },
  { "en": "Apa Itu IC Timer 555?", "id": "IC Pewaktu (Timer) Yang Sangat Populer." },
  { "en": "Apa Tiga Mode Operasi IC 555?", "id": "Astable, Monostable, Dan Bistable." },
  { "en": "Apa Itu Gerbang Logika (Logic Gate)?", "id": "Blok Dasar Pemrosesan Rangkaian Digital." },
  { "en": "Apa Itu Gerbang Logika AND?", "id": "Output 1 Jika Semua Input 1." },
  { "en": "Apa Itu Gerbang Logika OR?", "id": "Output 1 Jika Ada Input 1." },
  { "en": "Apa Itu Gerbang Logika NOT (Inverter)?", "id": "Membalik Sinyal Input (0 Jadi 1)." },
  { "en": "Apa Itu Gerbang Logika NAND?", "id": "Kebalikan Dari Gerbang Logika AND." },
  { "en": "Apa Itu Gerbang Logika NOR?", "id": "Kebalikan Dari Gerbang Logika OR." },
  { "en": "Apa Itu Gerbang Logika XOR (Exclusive OR)?", "id": "Output 1 Jika Input Berbeda." },
  { "en": "Apa Itu Gerbang Logika XNOR (Exclusive NOR)?", "id": "Output 1 Jika Input Sama." },
  { "en": "Apa Itu Kristal Osilator (Crystal Oscillator)?", "id": "Penghasil Sinyal Clock Frekuensi Stabil." },
  { "en": "Apa Fungsi Kristal Osilator?", "id": "Menstabilkan Frekuensi Rangkaian Digital." },
  { "en": "Apa Itu Sekring (Fuse)?", "id": "Komponen Pengaman Rangkaian Elektronik." },
  { "en": "Bagaimana Cara Kerja Sekring (Fuse)?", "id": "Putus Jika Terjadi Arus Berlebih." },
  { "en": "Apa Beda Sekring Cepat (Fast Blow)?", "id": "Langsung Putus Saat Arus Berlebih." },
  { "en": "Apa Beda Sekring Lambat (Slow Blow)?", "id": "Tahan Lonjakan Arus Sesaat." },
  { "en": "Apa Itu Buzzer?", "id": "Komponen Penghasil Sinyal Suara (Beep)." },
  { "en": "Apa Beda Buzzer Aktif Dan Pasif?", "id": "Aktif (Tegangan), Pasif (Frekuensi)." },
  { "en": "Apa Itu Speaker (Pengeras Suara)?", "id": "Mengubah Sinyal Listrik Menjadi Suara." },
  { "en": "Apa Itu Mikrofon (Microphone)?", "id": "Mengubah Sinyal Suara Menjadi Listrik." },
  { "en": "Apa Itu Trimpot (Trimmer Potentiometer)?", "id": "Potensiometer Kecil Untuk Penyetelan Internal." },
  { "en": "Apa Itu Multimeter (Avometer)?", "id": "Alat Ukur Arus, Tegangan, Hambatan." },
  { "en": "Apa Kepanjangan Avometer?", "id": "Ampere, Volt, Ohm Meter." },
  { "en": "Apa Itu Multimeter Analog?", "id": "Multimeter Yang Menggunakan Jarum Penunjuk." },
  { "en": "Apa Itu Multimeter Digital (DMM)?", "id": "Multimeter Yang Menampilkan Angka Digital." },
  { "en": "Apa Itu Osiloskop (Oscilloscope)?", "id": "Alat Ukur Bentuk Sinyal Listrik." },
  { "en": "Apa Yang Ditampilkan Osiloskop?", "id": "Grafik Tegangan Terhadap Waktu." },
  { "en": "Apa Itu Sumbu X Pada Osiloskop?", "id": "Sumbu Waktu (Time Division)." },
  { "en": "Apa Itu Sumbu Y Pada Osiloskop?", "id": "Sumbu Tegangan (Voltage Division)." },
  { "en": "Apa Itu Solder (Soldering Iron)?", "id": "Alat Pemanas Untuk Melelehkan Timah." },
  { "en": "Apa Itu Timah Solder (Tin)?", "id": "Logam Pengisi Untuk Menyambung Komponen." },
  { "en": "Apa Itu Flux (Fluks)?", "id": "Cairan Pembersih Permukaan Saat Menyolder." },
  { "en": "Apa Itu Frekuensi?", "id": "Jumlah Getaran Sinyal Per Detik." },
  { "en": "Apa Satuan Frekuensi?", "id": "Hertz (Hz)." },
  { "en": "Apa Itu Periode (Period)?", "id": "Waktu Yang Dibutuhkan Satu Siklus." },
  { "en": "Apa Itu Amplitudo (Amplitude)?", "id": "Simpangan Terjauh Gelombang Sinyal." },
  { "en": "Apa Itu SCR (Silicon Controlled Rectifier)?", "id": "Dioda Tiga Terminal Berfungsi Saklar." },
  { "en": "Apa Saja Kaki SCR (Silicon Controlled Rectifier)?", "id": "Anoda, Katoda, Dan Gerbang (Gate)." },
  { "en": "Apa Itu TRIAC (Triode for Alternating Current)?", "id": "Komponen Pengendali Arus AC Dua Arah." },
  { "en": "Apa Saja Kaki TRIAC?", "id": "Main Terminal 1, Main Terminal 2, Gate." },
  { "en": "Apa Itu DIAC (Diode for Alternating Current)?", "id": "Dioda Pemicu TRIAC (Trigger)." },
  { "en": "Apa Itu Termistor (Thermistor)?", "id": "Resistor Yang Nilainya Tergantung Suhu." },
  { "en": "Apa Beda NTC Dan PTC Thermistor?", "id": "NTC (Hambatan Turun), PTC (Hambatan Naik)." },
  { "en": "Apa Itu Varistor (VDR)?", "id": "Resistor Yang Nilainya Tergantung Tegangan." },
  { "en": "Apa Fungsi Varistor?", "id": "Melindungi Rangkaian Dari Lonjakan Tegangan." },
  { "en": "Apa Itu Breadboard (Project Board)?", "id": "Papan Uji Coba Rangkaian Sementara." },
  { "en": "Apa Keuntungan Menggunakan Breadboard?", "id": "Merakit Rangkaian Tanpa Solder." },
  { "en": "Apa Itu Regulator Tegangan (Voltage Regulator)?", "id": "IC Penstabil Tegangan Output." },
  { "en": "Contoh IC Regulator Tegangan Populer?", "id": "IC 7805 (Output 5 Volt)." },
  { "en": "Apa Arti 05 Pada IC 7805?", "id": "Menandakan Tegangan Output Positif 5V." },
  { "en": "Apa Itu IC Seri 79xx?", "id": "IC Regulator Tegangan Negatif." },
  { "en": "Apa Contoh IC Seri 79xx?", "id": "IC 7905 (Output Negatif 5V)." },
  { "en": "Apa Itu Heat Sink (Pendingin)?", "id": "Logam Penyerap Panas Komponen Aktif." },
  { "en": "Kenapa Transistor Daya Perlu Heat Sink?", "id": "Agar Tidak Rusak Karena Panas." },
  { "en": "Apa Itu Ground (GND)?", "id": "Titik Referensi Tegangan Nol Volt." },
  { "en": "Apa Simbol Ground (GND)?", "id": "Tiga Garis Horizontal Mengerucut." },
  { "en": "Apa Itu VCC?", "id": "Terminal Sumber Tegangan Positif (Umumnya BJT)." },
  { "en": "Apa Itu VEE?", "id": "Terminal Sumber Tegangan Negatif (Umumnya BJT)." },
  { "en": "Apa Itu VDD?", "id": "Terminal Sumber Tegangan Positif (Umunnya FET/CMOS)." },
  { "en": "Apa Itu VSS?", "id": "Terminal Sumber Tegangan Negatif (Umumnya FET/CMOS)." },
  { "en": "Apa Itu Kapasitor Bypass (Decoupling)?", "id": "Kapasitor Untuk Menghilangkan Noise Listrik." },
  { "en": "Di Mana Kapasitor Bypass Dipasang?", "id": "Sangat Dekat Kaki Power Supply IC." },
  { "en": "Apa Itu Rangkaian RC (Resistor-Capacitor)?", "id": "Rangkaian Filter Atau Pewaktu Sederhana." },
  { "en": "Apa Itu Rangkaian RL (Resistor-Inductor)?", "id": "Rangkaian Filter Menggunakan Induktor." },
  { "en": "Apa Itu Rangkaian LC (Inductor-Capacitor)?", "id": "Rangkaian Resonator Atau Filter Frekuensi." },
  { "en": "Apa Itu Frekuensi Resonansi?", "id": "Frekuensi Saat Reaktansi Kapasitif Dan Induktif Sama." },
  { "en": "Apa Itu Solder Wick (Wick Braid)?", "id": "Serabut Tembaga Pembersih Sisa Timah." },
  { "en": "Apa Itu Solder Sucker (Penyedot Timah)?", "id": "Alat Vakum Untuk Menghisap Timah." },
  { "en": "Apa Itu Multivibrator?", "id": "Rangkaian Elektronik Penghasil Osilasi." },
  { "en": "Apa Itu Multivibrator Astabil?", "id": "Rangkaian Osilator Tanpa Input Pemicu." },
  { "en": "Apa Itu Multivibrator Monostabil?", "id": "Rangkaian Penghasil Satu Pulsa (One-Shot)." },
  { "en": "Apa Itu Multivibrator Bistabil (Flip-Flop)?", "id": "Rangkaian Penyimpan Data 1-Bit." },
  { "en": "Apa Itu Optocoupler (Optoisolator)?", "id": "Isolator Rangkaian Menggunakan Cahaya." },
  { "en": "Apa Bagian Dalam Optocoupler?", "id": "LED Dan Phototransistor (Atau Photodiode)." },
  { "en": "Apa Fungsi Utama Optocoupler?", "id": "Memisahkan Dua Rangkaian Beda Tegangan." },
  { "en": "Apa Itu Seven Segment Display?", "id": "Penampil Angka Digital (0 Sampai 9)." },
  { "en": "Berapa Jumlah Segmen (LED) Pada Seven Segment?", "id": "Tujuh Segmen (A-G) Dan Titik (DP)." },
  { "en": "Apa Itu Tipe Common Anode?", "id": "Semua Anoda LED Terhubung Bersama (Ke Positif)." },
  { "en": "Apa Itu Tipe Common Cathode?", "id": "Semua Katoda LED Terhubung Bersama (Ke Negatif)." },
  { "en": "Apa Itu Mikrokontroler (Microcontroller)?", "id": "Komputer Kecil Dalam Satu IC." },
  { "en": "Apa Bagian Utama Mikrokontroler?", "id": "CPU, Memori, Dan Port I/O." },
  { "en": "Apa Kepanjangan CPU (Central Processing Unit)?", "id": "Unit Pemrosesan Pusat." },
  { "en": "Apa Kepanjangan I/O (Input/Output)?", "id": "Masukan Dan Keluaran Sinyal." },
  { "en": "Contoh Mikrokontroler Populer?", "id": "Arduino, ESP32, Raspberry Pi Pico." },
  { "en": "Apa Itu RAM (Random Access Memory)?", "id": "Memori Penyimpanan Data Sementara." },
  { "en": "Apa Sifat Data RAM (Random Access Memory)?", "id": "Volatile, Hilang Saat Listrik Mati." },
  { "en": "Apa Itu ROM (Read Only Memory)?", "id": "Memori Hanya Baca (Biasanya Firmware)." },
  { "en": "Apa Itu EEPROM (Electrically Erasable Programmable ROM)?", "id": "ROM Yang Bisa Dihapus Elektrik." },
  { "en": "Apa Itu Memori Flash (Flash Memory)?", "id": "Jenis EEPROM Cepat (Penyimpan Program)." },
  { "en": "Apa Itu Sensor?", "id": "Komponen Pendeteksi Perubahan Lingkungan Fisik." },
  { "en": "Apa Yang Dideteksi Sensor Suhu?", "id": "Perubahan Temperatur Atau Panas." },
  { "en": "Contoh Sensor Suhu Populer?", "id": "LM35, DS18B20, NTC." },
  { "en": "Apa Itu Sensor Ultrasonik?", "id": "Sensor Pengukur Jarak Gelombang Suara." },
  { "en": "Bagaimana Cara Kerja Sensor Ultrasonik?", "id": "Mengirim Dan Menerima Pantulan Suara." },
  { "en": "Contoh Sensor Ultrasonik Populer?", "id": "HC-SR04." },
  { "en": "Apa Itu Sensor PIR (Passive Infrared)?", "id": "Sensor Pendeteksi Gerakan (Panas Tubuh)." },
  { "en": "Apa Itu Sensor Kelembaban (Humidity)?", "id": "Sensor Pengukur Kadar Air Di Udara." },
  { "en": "Contoh Sensor Kelembaban Populer?", "id": "DHT11, DHT22." },
  { "en": "Apa Itu Aktuator (Actuator)?", "id": "Komponen Pengubah Sinyal Listrik Menjadi Gerakan." },
  { "en": "Apa Itu Motor DC (Direct Current)?", "id": "Motor Listrik Berarus Searah." },
  { "en": "Bagaimana Mengatur Kecepatan Motor DC?", "id": "Menggunakan Sinyal PWM." },
  { "en": "Apa Kepanjangan PWM (Pulse Width Modulation)?", "id": "Modulasi Lebar Pulsa." },
  { "en": "Apa Itu Motor Servo?", "id": "Motor DC Dengan Kontrol Posisi Sudut." },
  { "en": "Berapa Kaki Motor Servo?", "id": "Tiga Kaki (Power, Ground, Sinyal)." },
  { "en": "Apa Itu Motor Stepper (Stepper Motor)?", "id": "Motor Yang Bergerak Per Langkah (Step)." },
  { "en": "Apa Keuntungan Motor Stepper?", "id": "Presisi Tinggi Tanpa Umpan Balik." },
  { "en": "Apa Itu Solenoid?", "id": "Aktuator Elektromagnetik Gerakan Linear." },
  { "en": "Apa Itu Baterai (Battery)?", "id": "Sumber Penyimpan Energi Listrik Kimia (DC)." },
  { "en": "Apa Itu Baterai Primer?", "id": "Baterai Sekali Pakai (Tidak Bisa Diisi)." },
  { "en": "Apa Itu Baterai Sekunder?", "id": "Baterai Isi Ulang (Rechargeable)." },
  { "en": "Contoh Baterai Primer?", "id": "Alkaline, Zinc-Carbon." },
  { "en": "Contoh Baterai Sekunder?", "id": "Li-Ion, Ni-MH, Aki." },
  { "en": "Apa Itu Baterai Li-Ion (Lithium-Ion)?", "id": "Baterai Isi Ulang Kepadatan Tinggi." },
  { "en": "Apa Itu Power Supply (Catu Daya)?", "id": "Alat Penyedia Tenaga Listrik Rangkaian." },
  { "en": "Apa Itu Kabel Jumper (Jumper Wires)?", "id": "Kabel Penghubung Rangkaian Di Breadboard." },
  { "en": "Jenis Konektor Kabel Jumper?", "id": "Male-Male, Male-Female, Female-Female." },
  { "en": "Apa Itu Header Pin?", "id": "Konektor Pin Lurus Untuk PCB." },
  { "en": "Apa Itu Terminal Blok (Terminal Block)?", "id": "Konektor Kabel Menggunakan Sekrup Pengunci." },
  { "en": "Apa Itu Konektor DC (DC Jack/Plug)?", "id": "Konektor Khusus Sumber Tegangan DC." },
  { "en": "Apa Itu Konektor USB (Universal Serial Bus)?", "id": "Konektor Standar Data Dan Daya." },
  { "en": "Apa Itu Hubung Singkat (Short Circuit)?", "id": "Koneksi Hambatan Sangat Rendah (Berbahaya)." },
  { "en": "Apa Resiko Hubung Singkat?", "id": "Arus Berlebih, Panas, Kebakaran." },
  { "en": "Apa Itu Rangkaian Terbuka (Open Circuit)?", "id": "Koneksi Terputus, Arus Tidak Mengalir." },
  { "en": "Apa Itu Sinyal Analog?", "id": "Sinyal Kontinu Dengan Nilai Bervariasi." },
  { "en": "Contoh Sinyal Analog?", "id": "Sinyal Suara, Sinyal Sensor Suhu." },
  { "en": "Apa Itu Sinyal Digital?", "id": "Sinyal Diskrit (Hanya Nilai 0 Dan 1)." },
  { "en": "Contoh Sinyal Digital?", "id": "Data Komputer, Sinyal Clock." },
  { "en": "Apa Itu ADC (Analog to Digital Converter)?", "id": "Pengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Fungsi ADC (Analog to Digital Converter)?", "id": "Agar Mikrokontroler Membaca Sensor Analog." },
  { "en": "Apa Itu DAC (Digital to Analog Converter)?", "id": "Pengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Fungsi DAC (Digital to Analog Converter)?", "id": "Menghasilkan Tegangan Analog (Contoh: Audio)." },
  { "en": "Apa Itu Function Generator (Generator Fungsi)?", "id": "Alat Penghasil Sinyal Tes." },
  { "en": "Bentuk Sinyal Function Generator?", "id": "Sinus, Kotak, Segitiga." },
  { "en": "Apa Itu Logic Analyzer (Penganalisa Logika)?", "id": "Alat Penganalisa Banyak Sinyal Digital." },
  { "en": "Beda Osiloskop Dan Logic Analyzer?", "id": "Osiloskop (Analog), Logic Analyzer (Digital)." },
  { "en": "Apa Itu Jalur PCB (PCB Trace)?", "id": "Garis Tembaga Penghubung Komponen." },
  { "en": "Apa Itu Pad PCB?", "id": "Titik Tembaga Tempat Kaki Komponen Disolder." },
  { "en": "Apa Itu Via PCB?", "id": "Lubang Penghubung Jalur Antar Lapisan." },
  { "en": "Apa Itu Solder Mask (Masker Solder)?", "id": "Lapisan Pelindung PCB (Biasanya Hijau)." },
  { "en": "Apa Fungsi Solder Mask?", "id": "Mencegah Hubung Singkat Saat Solder." },
  { "en": "Apa Itu Silkscreen (Sablon Sutra)?", "id": "Teks Putih Penanda Komponen Di PCB." },
  { "en": "Apa Itu Komponen THT (Through-Hole Technology)?", "id": "Komponen Dengan Kaki Menembus PCB." },
  { "en": "Apa Keuntungan Komponen SMD (Surface Mount Device)?", "id": "Ukuran Lebih Kecil, Pemasangan Otomatis." },
  { "en": "Apa Kerugian Komponen SMD (Surface Mount Device)?", "id": "Sulit Disolder Secara Manual." },
  { "en": "Apa Itu Desoldering?", "id": "Proses Melepas Komponen Dari PCB." },
  { "en": "Kapan Solder Wick Digunakan?", "id": "Membersihkan Sisa Timah Di Pad." },
  { "en": "Apa Itu Solder Paste (Pasta Solder)?", "id": "Campuran Timah Dan Flux (Untuk SMD)." },
  { "en": "Apa Itu Hot Air Rework Station?", "id": "Alat Uap Panas Solder/Desolder SMD." },
  { "en": "Apa Itu ESD (Electrostatic Discharge)?", "id": "Pelepasan Listrik Statis (Merusak IC)." },
  { "en": "Bagaimana Mencegah ESD (Electrostatic Discharge)?", "id": "Menggunakan Gelang Anti-Statis." },
  { "en": "Apa Itu Gelang Anti-Statis (Anti-Static Wrist Strap)?", "id": "Gelang Untuk Membuang Muatan Statis." },
  { "en": "Apa Itu Impedansi (Impedance)?", "id": "Hambatan Total Rangkaian AC (Z)." },
  { "en": "Apa Satuan Impedansi?", "id": "Satuan Impedansi Adalah Ohm (Ω)." },
  { "en": "Apa Itu Reaktansi Induktif (XL)?", "id": "Hambatan Yang Ditimbulkan Induktor (AC)." },
  { "en": "Apa Itu Reaktansi Kapasitif (XC)?", "id": "Hambatan Yang Ditimbulkan Kapasitor (AC)." },
  { "en": "Bagaimana Sifat Kapasitor Terhadap DC?", "id": "Menghambat Total (Open Circuit Ideal)." },
  { "en": "Bagaimana Sifat Induktor Terhadap DC?", "id": "Melewatkan Total (Short Circuit Ideal)." },
  { "en": "Apa Itu Filter Lolos Bawah (Low Pass Filter)?", "id": "Meloloskan Frekuensi Rendah, Meredam Tinggi." },
  { "en": "Apa Itu Filter Lolos Atas (High Pass Filter)?", "id": "Meloloskan Frekuensi Tinggi, Meredam Rendah." },
  { "en": "Apa Itu Filter Lolos Pita (Band Pass Filter)?", "id": "Meloloskan Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Filter Tolak Pita (Band Stop Filter)?", "id": "Meredam Rentang Frekuensi Tertentu." },
  { "en": "Apa Itu Atenuator (Attenuator)?", "id": "Rangkaian Untuk Melemahkan Sinyal." },
  { "en": "Apa Itu Penguat (Amplifier)?", "id": "Rangkaian Untuk Menguatkan Amplitudo Sinyal." },
  { "en": "Apa Itu Gain (Penguatan)?", "id": "Ukuran Seberapa Besar Sinyal Dikuatkan." },
  { "en": "Apa Itu Feedback (Umpan Balik)?", "id": "Mengembalikan Sebagian Sinyal Output Ke Input." },
  { "en": "Apa Itu Umpan Balik Positif (Positive Feedback)?", "id": "Memperkuat Input (Bisa Osilasi)." },
  { "en": "Apa Itu Umpan Balik Negatif (Negative Feedback)?", "id": "Menstabilkan Penguatan (Umum Di Op-Amp)." },
  { "en": "Apa Itu Osilator (Oscillator)?", "id": "Rangkaian Penghasil Sinyal Periodik." },
  { "en": "Apa Syarat Terjadinya Osilasi?", "id": "Umpan Balik Positif Dan Penguatan." },
  { "en": "Apa Itu LCD (Liquid Crystal Display)?", "id": "Penampil Karakter Atau Grafis." },
  { "en": "Contoh LCD Populer?", "id": "LCD 16x2 (16 Kolom 2 Baris)." },
  { "en": "Apa Itu OLED (Organic Light Emitting Diode)?", "id": "Layar Display Dengan Kontras Tinggi." },
  { "en": "Apa Kelebihan OLED Dari LCD?", "id": "Lebih Tipis, Kontras Tinggi." },
  { "en": "Apa Itu Terminal?", "id": "Titik Sambungan Listrik Pada Komponen." },
  { "en": "Apa Itu Kaki Komponen (Lead)?", "id": "Kawat Logam Penghubung Komponen." },
  { "en": "Apa Itu Polaritas (Polarity)?", "id": "Adanya Kutub Positif Dan Negatif." },
  { "en": "Apa Itu Elektronika?", "id": "Ilmu Yang Mempelajari Aliran Elektron." },
  { "en": "Apa Beda Elektronika Analog Dan Digital?", "id": "Analog (Kontinu), Digital (Diskrit)." },
  { "en": "Apa Itu Sirkuit (Circuit)?", "id": "Jalur Tertutup Aliran Arus Listrik." },
  { "en": "Apa Syarat Arus Mengalir?", "id": "Harus Ada Jalur Tertutup (Closed Loop)." },
  { "en": "Apa Itu Skema Rangkaian (Schematic)?", "id": "Gambar Simbol Komponen Dan Koneksinya." },
  { "en": "Apa Itu Layout PCB?", "id": "Tata Letak Fisik Komponen Di PCB." },
  { "en": "Apa Itu Resistivitas (Resistivity)?", "id": "Kemampuan Bahan Menghambat Arus." },
  { "en": "Apa Itu Konduktor (Conductor)?", "id": "Bahan Penghantar Listrik Yang Baik." },
  { "en": "Contoh Konduktor Yang Baik?", "id": "Tembaga, Perak, Emas, Aluminium." },
  { "en": "Apa Itu Isolator (Insulator)?", "id": "Bahan Penghambat Listrik Yang Baik." },
  { "en": "Contoh Isolator Yang Baik?", "id": "Karet, Plastik, Kaca, Udara." },
  { "en": "Apa Itu Semikonduktor (Semiconductor)?", "id": "Bahan Antara Konduktor Dan Isolator." },
  { "en": "Contoh Bahan Semikonduktor?", "id": "Silikon (Si) Dan Germanium (Ge)." },
  { "en": "Apa Itu Doping Pada Semikonduktor?", "id": "Proses Menambah Ketidakmurnian Bahan." },
  { "en": "Apa Itu Semikonduktor Tipe-N?", "id": "Kelebihan Elektron (Muatan Negatif)." },
  { "en": "Apa Itu Semikonduktor Tipe-P?", "id": "Kekurangan Elektron (Muatan Positif, Hole)." },
  { "en": "Apa Itu Persimpangan P-N (P-N Junction)?", "id": "Gabungan Semikonduktor Tipe P Dan N." },
  { "en": "Apa Komponen Dasar Persimpangan P-N?", "id": "Dioda Adalah Persimpangan P-N." },
  { "en": "Apa Itu Elektron Valensi?", "id": "Elektron Di Kulit Atom Terluar." },
  { "en": "Apa Itu Pita Konduksi?", "id": "Pita Energi Tempat Elektron Bergerak Bebas." },
  { "en": "Apa Itu Celah Energi (Energy Gap)?", "id": "Energi Pemisah Pita Valensi Konduksi." },
  { "en": "Kenapa Isolator Tidak Menghantar Listrik?", "id": "Celah Energi Sangat Lebar." },
  { "en": "Apa Itu Arus Bocor (Leakage Current)?", "id": "Arus Kecil Saat Kondisi Terhambat." },
  { "en": "Apa Itu Tegangan Tembus (Breakdown Voltage)?", "id": "Tegangan Saat Isolator Mulai Menghantar." },
  { "en": "Apa Fungsi Tegangan Tembus Zener?", "id": "Menstabilkan Tegangan (Di Daerah Zener)." },
  { "en": "Apa Itu Kapasitor Kopling (Coupling Capacitor)?", "id": "Kapasitor Penghubung Sinyal AC Antar Rangkaian." },
  { "en": "Apa Fungsi Kapasitor Kopling?", "id": "Memblokir Komponen Arus DC." },
  { "en": "Apa Itu Dielektrik (Dielectric)?", "id": "Bahan Isolator Antara Pelat Kapasitor." },
  { "en": "Contoh Bahan Dielektrik?", "id": "Keramik, Udara, Mika, Elektrolit." },
  { "en": "Apa Yang Mempengaruhi Nilai Kapasitansi?", "id": "Luas Pelat, Jarak, Bahan Dielektrik." },
  { "en": "Apa Itu Reaktansi?", "id": "Hambatan Komponen Pasif Terhadap Arus AC." },
  { "en": "Kapan Reaktansi Kapasitif (XC) Kecil?", "id": "Saat Frekuensi Sinyal Sangat Tinggi." },
  { "en": "Kapan Reaktansi Kapasitif (XC) Besar?", "id": "Saat Frekuensi Sinyal Sangat Rendah." },
  { "en": "Kapan Reaktansi Induktif (XL) Kecil?", "id": "Saat Frekuensi Sinyal Sangat Rendah." },
  { "en": "Kapan Reaktansi Induktif (XL) Besar?", "id": "Saat Frekuensi Sinyal Sangat Tinggi." },
  { "en": "Apa Itu Jembatan Wheatstone (Wheatstone Bridge)?", "id": "Rangkaian Pengukur Nilai Resistor Presisi." },
  { "en": "Apa Itu LCD (Liquid Crystal Display) TFT?", "id": "LCD Dengan Transistor Tipis Aktif." },
  { "en": "Apa Itu LED (Light Emitting Diode) RGB?", "id": "Satu LED Dengan Tiga Warna (Merah, Hijau, Biru)." },
  { "en": "Bagaimana LED RGB Menghasilkan Warna Putih?", "id": "Mencampurkan Merah, Hijau, Biru." },
  { "en": "Apa Itu Resistor Pull-Up?", "id": "Resistor Penghubung Pin Input Ke VCC." },
  { "en": "Apa Fungsi Resistor Pull-Up?", "id": "Memberi Logika High Saat Pin Mengambang." },
  { "en": "Apa Itu Resistor Pull-Down?", "id": "Resistor Penghubung Pin Input Ke GND." },
  { "en": "Apa Fungsi Resistor Pull-Down?", "id": "Memberi Logika Low Saat Pin Mengambang." },
  { "en": "Apa Itu Kondisi Mengambang (Floating)?", "id": "Pin Input Tidak Terhubung (Logika Tidak Jelas)." },
  { "en": "Apa Itu Debouncing?", "id": "Menghilangkan Efek Pantulan Kontak Saklar." },
  { "en": "Kenapa Saklar Perlu Debouncing?", "id": "Mencegah Pembacaan Input Ganda." },
  { "en": "Apa Itu Dioda Flyback (Freewheeling Diode)?", "id": "Dioda Pelindung Beban Induktif (Relay/Motor)." },
  { "en": "Bagaimana Pemasangan Dioda Flyback?", "id": "Paralel Terbalik Dengan Beban Induktif." },
  { "en": "Apa Fungsi Dioda Flyback?", "id": "Menyerap GGL Induksi Balik (Back EMF)." },
  { "en": "Apa Itu GGL Induksi Balik (Back EMF)?", "id": "Lonjakan Tegangan Saat Induktor Mati." },
  { "en": "Apa Itu Snubber Circuit?", "id": "Rangkaian Peredam Lonjakan Tegangan." },
  { "en": "Apa Itu Rangkaian Diferensiator (Differentiator)?", "id": "Rangkaian Pengubah Sinyal (Slope)." },
  { "en": "Apa Itu Rangkaian Integrator (Integrator)?", "id": "Rangkaian Penjumlah Sinyal (Area)." },
  { "en": "Apa Itu Common Mode Rejection Ratio (CMRR)?", "id": "Kemampuan Op-Amp Menolak Sinyal Sama." },
  { "en": "Apa Itu Slew Rate Pada Op-Amp?", "id": "Kecepatan Perubahan Tegangan Output Maksimal." },
  { "en": "Apa Itu Gain Bandwidth Product (GBW)?", "id": "Hasil Kali Penguatan Dan Lebar Pita." },
  { "en": "Apa Itu Noise (Deras) Elektronik?", "id": "Sinyal Listrik Acak Yang Tidak Diinginkan." },
  { "en": "Apa Itu Signal-to-Noise Ratio (SNR)?", "id": "Perbandingan Kekuatan Sinyal Terhadap Noise." },
  { "en": "Kapan SNR (Signal-to-Noise Ratio) Dianggap Baik?", "id": "Saat Nilai SNR Sangat Tinggi." },
  { "en": "Apa Itu Distorsi (Distortion)?", "id": "Perubahan Bentuk Sinyal Asli." },
  { "en": "Apa Itu Distorsi Harmonik (Harmonic Distortion)?", "id": "Munculnya Frekuensi Kelipatan Sinyal Asli." },
  { "en": "Apa Itu Bandwidth (Lebar Pita)?", "id": "Rentang Frekuensi Yang Dapat Dilewatkan." },
  { "en": "Apa Itu Titik -3dB?", "id": "Frekuensi Saat Daya Sinyal Setengah." },
  { "en": "Apa Itu DC Offset?", "id": "Adanya Komponen DC Pada Sinyal AC." },
  { "en": "Apa Itu Ground Loop?", "id": "Masalah Noise Akibat Beda Potensial Ground." },
  { "en": "Bagaimana Mencegah Ground Loop?", "id": "Menggunakan Satu Titik Ground (Star Ground)." },
  { "en": "Apa Itu Ferit Bead (Ferrite Bead)?", "id": "Komponen Peredam Noise Frekuensi Tinggi." },
  { "en": "Apa Itu Konverter DC-DC?", "id": "Mengubah Satu Level Tegangan DC Ke Lainnya." },
  { "en": "Apa Itu Konverter Buck (Buck Converter)?", "id": "Konverter Penurun Tegangan DC (Step-Down)." },
  { "en": "Apa Itu Konverter Boost (Boost Converter)?", "id": "Konverter Penaik Tegangan DC (Step-Up)." },
  { "en": "Apa Itu Konverter Buck-Boost?", "id": "Konverter Penaik Atau Penurun Tegangan." },
  { "en": "Apa Itu LDO (Low-Dropout Regulator)?", "id": "Regulator Tegangan Dengan Beda Input-Output Kecil." },
  { "en": "Kelebihan LDO (Low-Dropout Regulator)?", "id": "Noise Rendah, Sederhana, Respon Cepat." },
  { "en": "Kekurangan LDO (Low-Dropout Regulator)?", "id": "Efisiensi Rendah Jika Beda Tegangan Besar." },
  { "en": "Apa Itu Switching Regulator?", "id": "Regulator Menggunakan Saklar (FET) Cepat." },
  { "en": "Kelebihan Switching Regulator?", "id": "Efisiensi Energi Sangat Tinggi." },
  { "en": "Kekurangan Switching Regulator?", "id": "Lebih Kompleks Dan Menghasilkan Noise." },
  { "en": "Apa Itu Ripple (Riak)?", "id": "Fluktuasi Kecil Tegangan DC Output." },
  { "en": "Apa Penyebab Ripple Pada Power Supply?", "id": "Sisa Sinyal AC Setelah Penyearahan." },
  { "en": "Bagaimana Mengurangi Ripple?", "id": "Menggunakan Kapasitor Filter Lebih Besar." },
  { "en": "Apa Itu Prototyping (Prototipe)?", "id": "Pembuatan Model Awal Rangkaian Uji." },
  { "en": "Apa Itu Veroboard (Stripboard)?", "id": "Papan PCB Berlubang Jalur Tembaga Lurus." },
  { "en": "Apa Itu Perfboard?", "id": "Papan PCB Berlubang Titik Tembaga Individual." },
  { "en": "Apa Itu Mikroskop USB?", "id": "Alat Bantu Melihat Komponen SMD Kecil." },
  { "en": "Apa Itu Helping Hands (Tangan Ketiga)?", "id": "Alat Penjepit PCB Saat Menyolder." },
  { "en": "Apa Itu Thermal Paste (Pasta Termal)?", "id": "Bahan Penghantar Panas Antara Komponen-Heatsink." },
  { "en": "Apa Fungsi Thermal Paste?", "id": "Memaksimalkan Transfer Panas Ke Pendingin." },
  { "en": "Apa Itu Standar IPC (Institute for Printed Circuits)?", "id": "Standar Industri Untuk Desain PCB." },
  { "en": "Apa Itu Datasheet (Lembar Data)?", "id": "Dokumen Spesifikasi Teknis Komponen." },
  { "en": "Informasi Apa Di Datasheet?", "id": "Spesifikasi, Pinout, Cara Penggunaan." },
  { "en": "Apa Itu Pinout (Konfigurasi Pin)?", "id": "Deskripsi Fungsi Setiap Kaki Komponen." },
  { "en": "Apa Itu Mouser Atau Digikey?", "id": "Distributor Global Komponen Elektronika." },
  { "en": "Apa Itu Simulasi Rangkaian?", "id": "Menguji Rangkaian Secara Virtual (Software)." },
  { "en": "Contoh Software Simulasi Rangkaian?", "id": "LTspice, Proteus, Multisim." },
  { "en": "Apa Itu CAD (Computer-Aided Design) Elektronik?", "id": "Software Desain Skema Dan Layout PCB." },
  { "en": "Contah Software CAD Elektronik?", "id": "Eagle, KiCad, Altium Designer." },
  { "en": "Apa Itu File Gerber?", "id": "File Standar Untuk Manufaktur PCB." },
  { "en": "Apa Itu Firmware?", "id": "Perangkat Lunak Tertanam Di Hardware." },
  { "en": "Di Mana Firmware Disimpan?", "id": "Biasanya Di ROM Atau Flash Memory." },
  { "en": "Apa Beda Software Dan Firmware?", "id": "Firmware Spesifik Hardware, Software Lebih Umum." },
  { "en": "Apa Itu GPIO (General Purpose Input Output)?", "id": "Pin Digital Serbaguna Mikrokontroler." },
  { "en": "Mode Apa Saja Pin GPIO?", "id": "Mode Input Atau Mode Output." },
  { "en": "Apa Itu Logika Digital?", "id": "Sistem Berbasis Nilai Biner (0 Dan 1)." },
  { "en": "Apa Arti Logika 1 (High)?", "id": "Menandakan Adanya Tegangan (Misal 5V)." },
  { "en": "Apa Arti Logika 0 (Low)?", "id": "Menandakan Tidak Ada Tegangan (0V/GND)." },
  { "en": "Apa Itu Transduser (Transducer)?", "id": "Pengubah Satu Bentuk Energi Ke Lainnya." },
  { "en": "Apakah Sensor Termasuk Transduser?", "id": "Ya, Sensor Adalah Tipe Transduser." },
  { "en": "Apakah Aktuator Termasuk Transduser?", "id": "Ya, Aktuator Juga Tipe Transduser." },
  { "en": "Apa Itu Piezoelektrik (Piezoelectric)?", "id": "Bahan Penghasil Listrik Saat Ditekan." },
  { "en": "Contoh Penggunaan Piezoelektrik?", "id": "Buzzer, Sensor Getar, Pemantik Api." },
  { "en": "Apa Itu Rotary Encoder?", "id": "Sensor Pengukur Posisi Putaran Poros." },
  { "en": "Beda Rotary Encoder Dan Potensiometer?", "id": "Encoder (Digital, Putaran Terus), Potensio (Analog, Terbatas)." },
  { "en": "Apa Itu Encoder Inkremental (Incremental)?", "id": "Encoder Penghitung Pulsa Putaran (Relatif)." },
  { "en": "Apa Itu Encoder Absolut (Absolute)?", "id": "Encoder Pemberi Nilai Posisi Pasti (Absolut)." },
  { "en": "Apa Itu Sensor Efek Hall (Hall Effect)?", "id": "Sensor Pendeteksi Keberadaan Medan Magnet." },
  { "en": "Contoh Penggunaan Sensor Efek Hall?", "id": "Sensor Kecepatan Roda, Sensor Posisi." },
  { "en": "Apa Itu Reed Switch?", "id": "Saklar Yang Diaktifkan Oleh Magnet." },
  { "en": "Contoh Penggunaan Reed Switch?", "id": "Sensor Alarm Pintu Atau Jendela." },
  { "en": "Apa Itu Strain Gauge?", "id": "Sensor Pengukur Regangan Atau Tekanan Benda." },
  { "en": "Di Mana Strain Gauge Digunakan?", "id": "Timbangan Digital (Load Cell)." },
  { "en": "Apa Itu Load Cell?", "id": "Transduser Pengubah Tekanan Menjadi Sinyal Listrik." },
  { "en": "Apa Itu Termokopel (Thermocouple)?", "id": "Sensor Suhu Tinggi Berbasis Tegangan." },
  { "en": "Bagaimana Prinsip Kerja Termokopel?", "id": "Efek Seebeck (Beda Logam Beda Suhu)." },
  { "en": "Apa Itu RTD (Resistance Temperature Detector)?", "id": "Sensor Suhu Berbasis Perubahan Resistansi Logam." },
  { "en": "Bahan Apa Untuk RTD (Resistance Temperature Detector)?", "id": "Umumnya Menggunakan Bahan Platina (Platinum)." },
  { "en": "Apa Itu Sensor Flex?", "id": "Sensor Yang Mengukur Tingkat Tekukan." },
  { "en": "Apa Itu Sensor Gas?", "id": "Sensor Pendeteksi Keberadaan Gas Tertentu." },
  { "en": "Contoh Sensor Gas?", "id": "MQ-2 (Asap/LPG), MQ-7 (Karbon Monoksida)." },
  { "en": "Apa Itu Sensor Warna?", "id": "Sensor Pendeteksi Nilai Warna (RGB)." },
  { "en": "Contoh Sensor Warna Populer?", "id": "TCS230 Atau TCS3200." },
  { "en": "Apa Itu Sensor Aliran Air (Water Flow)?", "id": "Sensor Pengukur Laju Aliran Cairan." },
  { "en": "Apa Itu Sensor Gyroscope?", "id": "Sensor Pengukur Kecepatan Rotasi Sudut." },
  { "en": "Apa Itu Sensor Akselerometer (Accelerometer)?", "id": "Sensor Pengukur Percepatan Linear (Gravitasi)." },
  { "en": "Apa Itu IMU (Inertial Measurement Unit)?", "id": "Gabungan Gyroscope Dan Akselerometer." },
  { "en": "Apa Itu Kompas Digital (Magnetometer)?", "id": "Sensor Pengukur Medan Magnet Bumi (Arah)." },
  { "en": "Apa Itu Sensor Barometer?", "id": "Sensor Pengukur Tekanan Udara Atmosfer." },
  { "en": "Apa Fungsi Sensor Barometer?", "id": "Mengukur Ketinggian (Altimeter) Atau Cuaca." },
  { "en": "Apa Itu Modul GPS (Global Positioning System)?", "id": "Modul Penerima Sinyal Satelit Lokasi." },
  { "en": "Apa Itu Modul RFID (Radio Frequency Identification)?", "id": "Sistem Identifikasi Menggunakan Gelombang Radio." },
  { "en": "Apa Dua Bagian Sistem RFID?", "id": "Tag (Kartu) Dan Reader (Pembaca)." },
  { "en": "Apa Itu NFC (Near Field Communication)?", "id": "Teknologi Komunikasi Nirkabel Jarak Dekat." },
  { "en": "Apa Beda RFID Dan NFC?", "id": "NFC Adalah Bagian RFID Frekuensi Tinggi." },
  { "en": "Apa Itu Modul Bluetooth?", "id": "Modul Komunikasi Nirkabel Jarak Pendek." },
  { "en": "Contoh Modul Bluetooth Populer?", "id": "HC-05 (Master/Slave), HC-06 (Slave)." },
  { "en": "Apa Itu Modul Wi-Fi?", "id": "Modul Komunikasi Nirkabel Jaringan Internet." },
  { "en": "Contoh Modul Wi-Fi Populer?", "id": "ESP8266 (ESP-01) Atau ESP32." },
  { "en": "Apa Kelebihan ESP32 Dari ESP8266?", "id": "Lebih Cepat, Dual-Core, Ada Bluetooth." },
  { "en": "Apa Itu Modul GSM (Global System for Mobile)?", "id": "Modul Komunikasi Jaringan Seluler (SMS/Panggilan)." },
  { "en": "Contoh Modul GSM Populer?", "id": "SIM800L Atau SIM900A." },
  { "en": "Apa Itu Modul LoRa (Long Range)?", "id": "Modul Komunikasi Nirkabel Jarak Jauh." },
  { "en": "Apa Itu Driver Motor?", "id": "IC Atau Modul Pengendali Motor." },
  { "en": "Kenapa Perlu Driver Motor?", "id": "Motor Butuh Arus Besar (Merusak GPIO)." },
  { "en": "Contoh IC Driver Motor?", "id": "L293D Atau L298N (H-Bridge)." },
  { "en": "Apa Itu H-Bridge?", "id": "Rangkaian Pengontrol Arah Putaran Motor DC." },
  { "en": "Apa Itu Driver Motor Stepper?", "id": "Driver Khusus Pengendali Motor Stepper." },
  { "en": "Contoh Driver Motor Stepper?", "id": "A4988 Atau DRV8825." },
  { "en": "Apa Itu Driver Relay?", "id": "Rangkaian Transistor Untuk Memicu Relay." },
  { "en": "Kenapa Relay Perlu Driver?", "id": "Kumparan Relay Butuh Arus (Merusak GPIO)." },
  { "en": "Apa Itu Solenoid Valve (Katup Solenoid)?", "id": "Keran Air Yang Dikontrol Listrik." },
  { "en": "Apa Itu Level Shifter (Penggeser Level)?", "id": "Konverter Level Tegangan Logika (3.3V ke 5V)." },
  { "en": "Kapan Level Shifter Dibutuhkan?", "id": "Saat Komunikasi Dua IC Beda Tegangan." },
  { "en": "Apa Itu I2C (Inter-Integrated Circuit)?", "id": "Protokol Komunikasi Dua Kabel (SDA, SCL)." },
  { "en": "Apa Itu SDA (Serial Data)?", "id": "Jalur Data Untuk Protokol I2C." },
  { "en": "Apa Itu SCL (Serial Clock)?", "id": "Jalur Clock Untuk Sinkronisasi I2C." },
  { "en": "Apa Keuntungan Protokol I2C?", "id": "Banyak Perangkat Dalam Dua Kabel." },
  { "en": "Apa Itu SPI (Serial Peripheral Interface)?", "id": "Protokol Komunikasi Serial Cepat Empat Kabel." },
  { "en": "Apa Empat Kabel SPI?", "id": "MISO, MOSI, SCLK, SS (CS)." },
  { "en": "Apa Itu MISO (Master In Slave Out)?", "id": "Data Dari Slave Ke Master (SPI)." },
  { "en": "Apa Itu MOSI (Master Out Slave In)?", "id": "Data Dari Master Ke Slave (SPI)." },
  { "en": "Apa Itu SCLK (Serial Clock)?", "id": "Jalur Clock Untuk Sinkronisasi SPI." },
  { "en": "Apa Itu SS/CS (Slave Select)?", "id": "Pin Pemilih Perangkat Slave (SPI)." },
  { "en": "Apa Itu UART (Universal Asynchronous Receiver-Transmitter)?", "id": "Komunikasi Serial Asinkron Dua Kabel." },
  { "en": "Apa Dua Kabel UART?", "id": "TX (Transmit) Dan RX (Receive)." },
  { "en": "Bagaimana Koneksi UART?", "id": "TXDihubungkan Ke RX Lawan." },
  { "en": "Apa Itu USB (Universal Serial Bus) to TTL?", "id": "Konverter Komunikasi USB Ke Serial UART." },
  { "en": "Contoh IC USB to TTL?", "id": "CH340, CP2102, FT232R." },
  { "en": "Apa Itu Baud Rate?", "id": "Kecepatan Komunikasi Serial (Bit Per Detik)." },
  { "en": "Apa Syarat Komunikasi Serial Berhasil?", "id": "Kedua Sisi Harus Punya Baud Rate Sama." },
  { "en": "Apa Itu Bit?", "id": "Satuan Data Digital Terkecil (0 Atau 1)." },
  { "en": "Apa Itu Byte?", "id": "Kumpulan Delapan Bit Data (8 Bit)." },
  { "en": "Apa Itu Heksadesimal (Hexadecimal)?", "id": "Sistem Bilangan Basis 16 (0-9, A-F)." },
  { "en": "Kenapa Heksadesimal Digunakan?", "id": "Menyederhanakan Penulisan Bilangan Biner Panjang." },
  { "en": "Apa Itu Biner (Binary)?", "id": "Sistem Bilangan Basis 2 (0 Dan 1)." },
  { "en": "Apa Itu Desimal (Decimal)?", "id": "Sistem Bilangan Basis 10 (0-9)." },
  { "en": "Apa Itu ASCII (American Standard Code)?", "id": "Standar Kode Angka Untuk Karakter." },
  { "en": "Apa Itu Dot Matrix Display?", "id": "Penampil Berbasis Matriks Titik LED." },
  { "en": "Contoh Modul Dot Matrix?", "id": "MAX7219 8x8 Dot Matrix." },
  { "en": "Apa Itu TFT (Thin Film Transistor) Display?", "id": "Layar LCD Warna Berbasis Transistor." },
  { "en": "Apa Itu Layar Sentuh Resistif (Resistive)?", "id": "Layar Sentuh Berbasis Tekanan Dua Lapisan." },
  { "en": "Apa Itu Layar Sentuh Kapasitif (Capacitive)?", "id": "Layar Sentuh Berbasis Muatan Listrik (Tubuh)." },
  { "en": "Mana Yang Lebih Baik, Resistif Atau Kapasitif?", "id": "Kapasitif Lebih Responsif (Multi-Sentuh)." },
  { "en": "Apa Itu Rangkaian Pembagi Tegangan (Voltage Divider)?", "id": "Dua Resistor Seri Pembagi Voltase." },
  { "en": "Apa Rumus Rangkaian Pembagi Tegangan?", "id": "Vout = Vin * (R2 / (R1+R2))." },
  { "en": "Apa Itu Rangkaian Pembagi Arus (Current Divider)?", "id": "Dua Resistor Paralel Pembagi Arus." },
  { "en": "Apa Itu Paket Komponen DIP (Dual In-Line Package)?", "id": "IC Dengan Dua Baris Kaki." },
  { "en": "Apa Itu Paket Komponen SOIC (Small Outline IC)?", "id": "Paket IC Tipe SMD (Surface Mount)." },
  { "en": "Apa Itu Paket TO-92?", "id": "Paket Transistor Plastik Kecil Tiga Kaki." },
  { "en": "Apa Itu Paket TO-220?", "id": "Paket Transistor Daya Dengan Pendingin Logam." },
  { "en": "Apa Itu Paket QFP (Quad Flat Package)?", "id": "IC SMD Dengan Kaki Di Empat Sisi." },
  { "en": "Apa Itu BGA (Ball Grid Array)?", "id": "IC SMD Dengan Bola Timah Dibawah." },
  { "en": "Apa Itu Keluarga Logika TTL (Transistor-Transistor Logic)?", "id": "Logika Digital Berbasis Transistor BJT." },
  { "en": "Berapa Tegangan Kerja Logika TTL Standar?", "id": "Tegangan Kerja 5 Volt." },
  { "en": "Apa Itu Keluarga Logika CMOS (Complementary MOS)?", "id": "Logika Digital Berbasis MOSFET." },
  { "en": "Apa Kelebihan Logika CMOS?", "id": "Konsumsi Daya Sangat Rendah (Statis)." },
  { "en": "Apa Itu Penyearah Setengah Gelombang (Half-Wave)?", "id": "Penyearah AC Ke DC Satu Dioda." },
  { "en": "Apa Itu Penyearah Gelombang Penuh (Full-Wave)?", "id": "Penyearah Menggunakan Dua Atau Empat Dioda." },
  { "en": "Apa Keuntungan Penyearah Gelombang Penuh?", "id": "Ripple Lebih Kecil, Efisiensi Tinggi." },
  { "en": "Apa Itu Rangkaian Clipper (Pemotong)?", "id": "Rangkaian Pemotong Bagian Sinyal." },
  { "en": "Apa Fungsi Rangkaian Clipper?", "id": "Membatasi Amplitudo Sinyal Maksimal." },
  { "en": "Apa Itu Rangkaian Clamper (Penggeser)?", "id": "Rangkaian Penggeser Level DC Sinyal." },
  { "en": "Apa Itu Filter Butterworth?", "id": "Filter Dengan Respon Paling Datar (Flat)." },
  { "en": "Apa Itu Filter Chebyshev?", "id": "Filter Dengan Transisi Tajam (Ripple)." },
  { "en": "Apa Itu Filter Bessel?", "id": "Filter Dengan Respon Fasa Linear." },
  { "en": "Apa Itu Orde Filter?", "id": "Menentukan Tingkat Ketajaman (Slope) Filter." },
  { "en": "Apa Itu Kelas Penguat (Amplifier) A?", "id": "Transistor Aktif Sepanjang Siklus (360°)." },
  { "en": "Apa Keuntungan Penguat Kelas A?", "id": "Linearitas Paling Baik (Distorsi Rendah)." },
  { "en": "Apa Kerugian Penguat Kelas A?", "id": "Sangat Tidak Efisien (Panas)." },
  { "en": "Apa Itu Kelas Penguat (Amplifier) B?", "id": "Transistor Aktif Setengah Siklus (180°)." },
  { "en": "Apa Itu Distorsi Crossover?", "id": "Distorsi Pada Penguat Kelas B." },
  { "en": "Apa Itu Kelas Penguat (Amplifier) AB?", "id": "Gabungan Kelas A Dan Kelas B." },
  { "en": "Apa Fungsi Penguat Kelas AB?", "id": "Mengurangi Distorsi Crossover Kelas B." },
  { "en": "Apa Itu Kelas Penguat (Amplifier) C?", "id": "Transistor Aktif Kurang Dari 180°." },
  { "en": "Di Mana Penguat Kelas C Digunakan?", "id": "Frekuensi Radio (RF) Efisiensi Tinggi." },
  { "en": "Apa Itu Kelas Penguat (Amplifier) D?", "id": "Penguat Berbasis PWM (Switching)." },
  { "en": "Apa Keuntungan Penguat Kelas D?", "id": "Efisiensi Energi Sangat Tinggi." },
  { "en": "Apa Itu Osilator Hartley?", "id": "Osilator LC Menggunakan Induktor Terbelah." },
  { "en": "Apa Itu Osilator Colpitts?", "id": "Osilator LC Menggunakan Kapasitor Terbelah." },
  { "en": "Apa Itu Osilator Wien Bridge?", "id": "Osilator RC Penghasil Sinyal Sinus." },
  { "en": "Apa Itu Osilator Pergeseran Fasa (Phase-Shift)?", "id": "Osilator Menggunakan Jaringan RC Phase-Shift." },
  { "en": "Apa Itu Faktor Q (Quality Factor)?", "id": "Ukuran Kualitas Rangkaian Resonansi." },
  { "en": "Apa Arti Faktor Q Tinggi?", "id": "Redaman Rendah, Selektivitas Frekuensi Baik." },
  { "en": "Apa Itu SRAM (Static RAM)?", "id": "RAM Berbasis Flip-Flop (Sangat Cepat)." },
  { "en": "Apa Sifat SRAM (Static RAM)?", "id": "Volatile, Tidak Perlu Refresh." },
  { "en": "Apa Itu DRAM (Dynamic RAM)?", "id": "RAM Berbasis Kapasitor (Perlu Refresh)." },
  { "en": "Apa Sifat DRAM (Dynamic RAM)?", "id": "Volatile, Murah, Kepadatan Tinggi." },
  { "en": "Apa Itu SDRAM (Synchronous DRAM)?", "id": "DRAM Yang Sinkron Dengan Clock Sistem." },
  { "en": "Apa Itu Inverter (Pembalik)?", "id": "Rangkaian Pengubah Tegangan DC Ke AC." },
  { "en": "Apa Itu Thyristor?", "id": "Keluarga Komponen Semikonduktor Saklar Daya." },
  { "en": "Apakah SCR Termasuk Thyristor?", "id": "Ya, SCR Adalah Jenis Thyristor." },
  { "en": "Apakah TRIAC Termasuk Thyristor?", "id": "Ya, TRIAC Juga Jenis Thyristor." },
  { "en": "Apa Itu Dioda TVS (Transient Voltage Suppressor)?", "id": "Dioda Pelindung Lonjakan Tegangan Cepat." },
  { "en": "Apa Beda TVS Dan Zener?", "id": "TVS Merespon Lonjakan Jauh Lebih Cepat." },
  { "en": "Apa Itu Polyfuse (Sekring Polimer)?", "id": "Sekring Yang Dapat Pulih (Resettable)." },
  { "en": "Bagaimana Cara Kerja Polyfuse?", "id": "Hambatan Naik Saat Panas (Arus Berlebih)." },
  { "en": "Apa Itu Rangkaian Crowbar (Linggis)?", "id": "Rangkaian Proteksi Hubung Singkat Catu Daya." },
  { "en": "Apa Itu EMI (Electromagnetic Interference)?", "id": "Gangguan Elektromagnetik Yang Tidak Diinginkan." },
  { "en": "Apa Itu EMC (Electromagnetic Compatibility)?", "id": "Kemampuan Perangkat Beroperasi Tanpa Gangguan." },
  { "en": "Apa Itu Desibel (dB)?", "id": "Satuan Logaritmik Pengukuran Rasio." },
  { "en": "Apa Itu Antena?", "id": "Komponen Pengubah Sinyal Listrik Ke Gelombang Radio." },
  { "en": "Apa Fungsi Antena?", "id": "Memancarkan Atau Menerima Gelombang Elektromagnetik." },
  { "en": "Apa Itu Modulasi (Modulation)?", "id": "Menumpangkan Sinyal Informasi Ke Pembawa." },
  { "en": "Apa Itu Demodulasi (Demodulation)?", "id": "Memisahkan Sinyal Informasi Dari Pembawa." },
  { "en": "Apa Itu Modulasi AM (Amplitude Modulation)?", "id": "Modulasi Yang Mengubah Amplitudo Pembawa." },
  { "en": "Apa Itu Modulasi FM (Frequency Modulation)?", "id": "Modulasi Yang Mengubah Frekuensi Pembawa." },
  { "en": "Apa Kelebihan Modulasi FM Dari AM?", "id": "Kualitas Suara Lebih Baik, Tahan Noise." },
  { "en": "Apa Itu Mixer (Pencampur Frekuensi)?", "id": "Rangkaian Pengali Dua Sinyal Frekuensi." },
  { "en": "Apa Itu LCR Meter?", "id": "Alat Ukur Induktansi, Kapasitansi, Resistansi." },
  { "en": "Apa Itu Penganalisa Spektrum (Spectrum Analyzer)?", "id": "Alat Ukur Sinyal Di Domain Frekuensi." },
  { "en": "Apa Itu Sumbu X Penganalisa Spektrum?", "id": "Sumbu Frekuensi." },
  { "en": "Apa Itu Sumbu Y Penganalisa Spektrum?", "id": "Sumbu Amplitudo (Daya)." },
  { "en": "Apa Itu Efek Kulit (Skin Effect)?", "id": "Arus AC Mengalir Di Permukaan Konduktor." },
  { "en": "Kapan Efek Kulit (Skin Effect) Terjadi?", "id": "Pada Frekuensi Sangat Tinggi." },
  { "en": "Apa Itu Kabel Koaksial (Coaxial Cable)?", "id": "Kabel Transmisi Sinyal Frekuensi Tinggi." },
  { "en": "Struktur Kabel Koaksial?", "id": "Inti, Dielektrik, Serabut, Jaket Luar." },
  { "en": "Apa Itu Impedansi Karakteristik?", "id": "Impedansi Khas Kabel Transmisi (Contoh 50 Ohm)." },
  { "en": "Apa Itu SWR (Standing Wave Ratio)?", "id": "Rasio Gelombang Berdiri (Indikator Mismatch)." },
  { "en": "Apa Arti SWR (Standing Wave Ratio) 1:1?", "id": "Pencocokan Impedansi Sempurna (Tidak Ada Pantulan)." },
  { "en": "Apa Itu Beban Semu (Dummy Load)?", "id": "Resistor Pengganti Antena Untuk Tes." },
  { "en": "Apa Itu Rangkaian PLL (Phase-Locked Loop)?", "id": "Rangkaian Pengunci Fasa Sinyal." },
  { "en": "Fungsi Rangkaian PLL (Phase-Locked Loop)?", "id": "Sintesis Frekuensi, Demodulasi FM." },
  { "en": "Apa Itu VCO (Voltage-Controlled Oscillator)?", "id": "Osilator Yang Frekuensinya Diatur Tegangan." },
  { "en": "Apa Itu Dioda PIN?", "id": "Dioda Untuk Saklar RF (Frekuensi Radio)." },
  { "en": "Apa Itu Dioda Gunn?", "id": "Dioda Penghasil Sinyal Microwave (Gelombang Mikro)." },
  { "en": "Apa Itu Magnetron?", "id": "Tabung Penghasil Gelombang Mikro Daya Tinggi." },
  { "en": "Di Mana Magnetron Digunakan?", "id": "Oven Microwave Dan Radar." },
  { "en": "Apa Itu Tabung Vakum (Vacuum Tube)?", "id": "Komponen Aktif Penguat Sinyal (Lama)." },
  { "en": "Apa Itu Cathode Ray Tube (CRT)?", "id": "Tabung Vakum Untuk Layar (TV Tabung)." },
  { "en": "Apa Itu FPGA (Field-Programmable Gate Array)?", "id": "IC Digital Yang Dapat Diprogram Ulang." },
  { "en": "Apa Beda FPGA Dan Mikrokontroler?", "id": "FPGA (Hardware Programabel), MCU (Software)." },
  { "en": "Apa Bahasa Pemrograman FPGA?", "id": "VHDL (Very High Speed IC Hardware Description Language) Atau Verilog." },
  { "en": "Apa Itu CPLD (Complex Programmable Logic Device)?", "id": "Perangkat Logika Terprogram (Lebih Sederhana)." },
  { "en": "Apa Itu ASIC (Application-Specific Integrated Circuit)?", "id": "IC Kustom Untuk Tugas Spesifik." },
  { "en": "Apa Beda FPGA Dan ASIC?", "id": "FPGA (Diprogram Ulang), ASIC (Permanen)." },
  { "en": "Apa Itu DSP (Digital Signal Processor)?", "id": "Mikroprosesor Khusus Pengolahan Sinyal Digital." },
  { "en": "Di Mana DSP (Digital Signal Processor) Digunakan?", "id": "Audio, Video, Komunikasi." },
  { "en": "Apa Itu ADC (Analog to Digital Converter) Flash?", "id": "Tipe ADC Paling Cepat." },
  { "en": "Apa Itu ADC (Analog to Digital Converter) SAR?", "id": "Successive Approximation Register (Umum Dipakai)." },
  { "en": "Apa Itu ADC (Analog to Digital Converter) Sigma-Delta?", "id": "Tipe ADC Resolusi Sangat Tinggi." },
  { "en": "Apa Itu Resolusi ADC?", "id": "Jumlah Bit Hasil Konversi Digital." },
  { "en": "Apa Arti Resolusi ADC 10-Bit?", "id": "Menghasilkan 1024 Level Digital." },
  { "en": "Apa Itu LSB (Least Significant Bit)?", "id": "Bit Dengan Nilai Paling Kecil (Kanan)." },
  { "en": "Apa Itu MSB (Most Significant Bit)?", "id": "Bit Dengan Nilai Paling Besar (Kiri)." },
  { "en": "Apa Itu DAC (Digital to Analog Converter) R-2R Ladder?", "id": "Jaringan Resistor R-2R Untuk DAC." },
  { "en": "Apa Itu Multiplexer (MUX)?", "id": "Pemilih Satu Input Dari Banyak Input." },
  { "en": "Apa Fungsi Pin Select MUX?", "id": "Memilih Jalur Input Mana Yang Aktif." },
  { "en": "Apa Itu Demultiplexer (DEMUX)?", "id": "Penyalur Satu Input Ke Banyak Output." },
  { "en": "Apa Fungsi Pin Select DEMUX?", "id": "Memilih Jalur Output Mana Yang Aktif." },
  { "en": "Apa Itu Encoder (Dalam Logika Digital)?", "id": "Pengubah Banyak Input Ke Kode Biner." },
  { "en": "Apa Itu Decoder (Dalam Logika Digital)?", "id": "Pengubah Kode Biner Ke Banyak Output." },
  { "en": "Contoh Decoder Populer?", "id": "Decoder BCD Ke Seven Segment." },
  { "en": "Apa Itu BCD (Binary Coded Decimal)?", "id": "Representasi Biner Untuk Angka Desimal." },
  { "en": "Apa Itu Flip-Flop?", "id": "Rangkaian Bistabil Penyimpan Satu Bit." },
  { "en": "Apa Itu Flip-Flop SR (Set-Reset)?", "id": "Flip-Flop Paling Dasar (Set Dan Reset)." },
  { "en": "Apa Itu Flip-Flop D (Data)?", "id": "Flip-Flop Penyimpan Data Saat Clock." },
  { "en": "Apa Itu Flip-Flop JK?", "id": "Flip-Flop Universal (JK, Set, Reset, Toggle)." },
  { "en": "Apa Itu Flip-Flop T (Toggle)?", "id": "Flip-Flop Yang Membalik Output (Toggle)." },
  { "en": "Apa Itu Latch?", "id": "Mirip Flip-Flop, Tapi Transparan Level." },
  { "en": "Beda Latch Dan Flip-Flop?", "id": "Latch (Level-Triggered), Flip-Flop (Edge-Triggered)." },
  { "en": "Apa Itu Edge Triggered (Pemicu Tepi)?", "id": "Perubahan Output Saat Tepi Naik/Turun Clock." },
  { "en": "Apa Itu Register (Dalam Digital)?", "id": "Kumpulan Flip-Flop Penyimpan Data (Word)." },
  { "en": "Apa Itu Shift Register (Register Geser)?", "id": "Register Yang Menggeser Data Bit." },
  { "en": "Apa Itu SISO (Serial In Serial Out)?", "id": "Register Geser Input Serial Output Serial." },
  { "en": "Apa Itu SIPO (Serial In Parallel Out)?", "id": "Register Geser Input Serial Output Paralel." },
  { "en": "Apa Itu PISO (Parallel In Serial Out)?", "id": "Register Geser Input Paralel Output Serial." },
  { "en": "Apa Itu PIPO (Parallel In Parallel Out)?", "id": "Register Geser Input Paralel Output Paralel." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Rangkaian Pensekuensial Penghitung Pulsa Clock." },
  { "en": "Apa Itu Counter Asinkron (Ripple Counter)?", "id": "Output Flip-Flop Memicu Clock Berikutnya." },
  { "en": "Apa Itu Counter Sinkron (Synchronous Counter)?", "id": "Semua Flip-Flop Diberi Clock Bersamaan." },
  { "en": "Apa Itu Decade Counter (Pencacah Dekade)?", "id": "Counter Yang Menghitung Sampai Sepuluh (0-9)." },
  { "en": "Apa Itu Ring Counter?", "id": "Register Geser Dengan Output Terhubung Input." },
  { "en": "Apa Itu Johnson Counter?", "id": "Modifikasi Ring Counter (Output Invert)." },
  { "en": "Apa Itu State Machine (Mesin Keadaan)?", "id": "Model Rangkaian Pensekuensial (Keadaan, Transisi)." },
  { "en": "Apa Itu Komparator (Comparator) Digital?", "id": "Rangkaian Pembanding Dua Angka Biner." },
  { "en": "Apa Itu Adder (Penjumlah)?", "id": "Rangkaian Logika Penjumlah Dua Bit." },
  { "en": "Apa Itu Half Adder (Penjumlah Setengah)?", "id": "Menjumlahkan Dua Bit (Hasil Sum, Carry)." },
  { "en": "Apa Itu Full Adder (Penjumlah Penuh)?", "id": "Menjumlahkan Tiga Bit (Dua Bit, Satu Carry In)." },
  { "en": "Apa Itu ALU (Arithmetic Logic Unit)?", "id": "Bagian CPU Operasi Aritmatika Logika." },
  { "en": "Apa Itu Tembaga (Copper)?", "id": "Logam Konduktor Utama Pada PCB." },
  { "en": "Apa Itu Timah (Tin)?", "id": "Logam Titik Lebur Rendah Untuk Solder." },
  { "en": "Apa Itu Timbal (Lead)?", "id": "Logam Campuran Timah (Kini Dihindari)." },
  { "en": "Apa Itu RoHS (Restriction of Hazardous Substances)?", "id": "Direktif Pembatasan Bahan Berbahaya (Timbal)." },
  { "en": "Apa Itu Solder Bebas Timbal (Lead-Free)?", "id": "Solder Tanpa Timbal (Sesuai RoHS)." },
  { "en": "Apa Itu Kawat Email (Enamel Wire)?", "id": "Kawat Tembaga Berisolasi Lapisan Tipis." },
  { "en": "Di Mana Kawat Email Digunakan?", "id": "Kumparan Induktor, Transformator, Motor." },
  { "en": "Apa Itu Pita Listrik (Electrical Tape)?", "id": "Pita Isolasi Sambungan Kabel Listrik." },
  { "en": "Apa Itu Selang Bakar (Heat Shrink Tube)?", "id": "Selang Isolasi Yang Mengerut Saat Panas." },
  { "en": "Apa Itu Obeng (Screwdriver)?", "id": "Alat Pembuka Atau Pemasang Sekrup." },
  { "en": "Apa Itu Tang Potong (Cutting Pliers)?", "id": "Tang Untuk Memotong Kawat Komponen." },
  { "en": "Apa Itu Tang Lancip (Needle Nose Pliers)?", "id": "Tang Penjepit Ujung Runcing (Menjepit)." },
  { "en": "Apa Itu Pinset (Tweezers)?", "id": "Alat Penjepit Komponen Kecil (SMD)." },
  { "en": "Apa Itu Kaca Pembesar (Magnifying Glass)?", "id": "Alat Bantu Visual Inspeksi Solderan." },
  { "en": "Apa Itu Jumper?", "id": "Penghubung Singkat Opsi Konfigurasi PCB." },
  { "en": "Apa Itu Test Point (Titik Uji)?", "id": "Titik Logam Di PCB Untuk Pengukuran." },
  { "en": "Apa Itu Konektor FPC (Flexible Printed Circuit)?", "id": "Konektor Untuk Kabel Pita Fleksibel." },
  { "en": "Apa Itu Konektor ZIF (Zero Insertion Force)?", "id": "Konektor Tanpa Gaya Pemasangan (Pengunci)." },
  { "en": "Apa Itu Konektor BNC (Bayonet Neill-Concelman)?", "id": "Konektor Koaksial RF (Osilator, Video)." },
  { "en": "Apa Itu Konektor SMA (SubMiniature version A)?", "id": "Konektor Koaksial RF (Antena Wi-Fi)." },
  { "en": "Apa Itu Konektor RCA (Radio Corporation of America)?", "id": "Konektor Sinyal Audio Video Analog." },
  { "en": "Apa Itu Konektor Jack Telepon (TRS)?", "id": "Konektor Audio (Headphone, Mikrofon)." },
  { "en": "Apa Arti TRS (Tip-Ring-Sleeve)?", "id": "Tiga Kontak Jack (Stereo Atau Balanced)." },
  { "en": "Apa Arti TS (Tip-Sleeve)?", "id": "Dua Kontak Jack (Mono Unbalanced)." },
  { "en": "Apa Itu Konektor XLR?", "id": "Konektor Audio Profesional Tiga Pin (Balanced)." },
  { "en": "Apa Itu Audio Balanced?", "id": "Sinyal Audio Tiga Kabel (Penolak Noise)." },
  { "en": "Apa Itu Audio Unbalanced?", "id": "Sinyal Audio Dua Kabel (Rentan Noise)." },
  { "en": "Apa Itu Konektor VGA (Video Graphics Array)?", "id": "Konektor Sinyal Video Analog 15-Pin." },
  { "en": "Apa Itu Konektor DVI (Digital Visual Interface)?", "id": "Konektor Sinyal Video (Digital/Analog)." },
  { "en": "Apa Itu Konektor HDMI (High-Definition Multimedia Interface)?", "id": "Konektor Sinyal Audio Video Digital." },
  { "en": "Apa Itu Konektor DisplayPort?", "id": "Konektor Sinyal Audio Video Digital (PC)." },
  { "en": "Apa Itu Konektor Ethernet (RJ45)?", "id": "Konektor Standar Jaringan Kabel Komputer." },
  { "en": "Apa Itu Konektor Telepon (RJ11)?", "id": "Konektor Jalur Telepon Rumah (Lebih Kecil)." },
  { "en": "Apa Itu Konektor Molex?", "id": "Konektor Daya Internal Komputer (Disk)." },
  { "en": "Apa Itu Konektor SATA (Serial ATA)?", "id": "Konektor Data Hard Drive Modern." },
  { "en": "Apa Itu Konektor JST (Japan Solderless Terminal)?", "id": "Konektor Kecil Putih (Baterai, Kipas)." },
  { "en": "Apa Itu Konektor DuPont?", "id": "Konektor Hitam Standar (Kabel Jumper)." },
  { "en": "Apa Itu Kawat AWG (American Wire Gauge)?", "id": "Standar Ukuran (Diameter) Kawat." },
  { "en": "Bagaimana Aturan Ukuran AWG?", "id": "Angka AWG Kecil Berarti Kawat Tebal." },
  { "en": "Apa Itu Kabel Serabut (Stranded Wire)?", "id": "Kabel Terdiri Banyak Serat Halus." },
  { "en": "Apa Keuntungan Kabel Serabut?", "id": "Lebih Fleksibel, Tahan Getaran." },
  { "en": "Apa Itu Kabel Solid (Solid Wire)?", "id": "Kabel Terdiri Satu Kawat Inti." },
  { "en": "Apa Keuntungan Kabel Solid?", "id": "Lebih Murah, Bagus Untuk Breadboard." },
  { "en": "Apa Itu PCB Single Layer (Satu Lapis)?", "id": "Jalur Tembaga Hanya Di Satu Sisi." },
  { "en": "Apa Itu PCB Double Layer (Dua Lapis)?", "id": "Jalur Tembaga Di Kedua Sisi Papan." },
  { "en": "Apa Itu PCB Multi Layer (Multi Lapis)?", "id": "PCB Dengan Jalur Di Lapisan Tengah." },
  { "en": "Apa Itu Firmware Development?", "id": "Proses Penulisan Kode Untuk Mikrokontroler." },
  { "en": "Apa Itu IDE (Integrated Development Environment)?", "id": "Perangkat Lunak Untuk Pemrograman (Editor, Compiler)." },
  { "en": "Contoh IDE (Integrated Development Environment) Populer?", "id": "Arduino IDE, VS Code, PlatformIO." },
  { "en": "Apa Itu Compiler (Kompilator)?", "id": "Penerjemah Kode Program Ke Bahasa Mesin." },
  { "en": "Apa Itu Debugger?", "id": "Alat Bantu Pencari Kesalahan (Bug) Program." },
  { "en": "Apa Itu JTAG (Joint Test Action Group)?", "id": "Standar Antarmuka Untuk Debugging Hardware." },
  { "en": "Apa Itu SWD (Serial Wire Debug)?", "id": "Antarmuka Debug Dua Kabel (Alternatif JTAG)." },
  { "en": "Apa Itu Bootloader?", "id": "Program Kecil Pemuat Program Utama." },
  { "en": "Fungsi Bootloader Pada Arduino?", "id": "Memuat Program Lewat USB (Serial)." },
  { "en": "Apa Itu ISP (In-System Programming)?", "id": "Metode Memprogram IC Langsung Di Rangkaian." },
  { "en": "Apa Itu Interrupt (Interupsi)?", "id": "Sinyal Yang Menghentikan Tugas Utama CPU." },
  { "en": "Apa Fungsi Interrupt (Interupsi)?", "id": "Merespon Kejadian Penting Secara Cepat." },
  { "en": "Apa Itu Watchdog Timer (WDT)?", "id": "Pewaktu Pengawas (Mencegah Hang)." },
  { "en": "Bagaimana Watchdog Timer Bekerja?", "id": "Mereset Mikrokontroler Jika Tidak Di-'tendang'." },
  { "en": "Apa Itu Sleep Mode (Mode Tidur) Mikrokontroler?", "id": "Mode Diam Untuk Menghemat Daya Baterai." },
  { "en": "Apa Itu Brown-Out Detector (BOD)?", "id": "Sirkuit Pendeteksi Tegangan Input Rendah." },
  { "en": "Apa Fungsi Brown-Out Detector (BOD)?", "id": "Mencegah Kerusakan Data Saat Voltase Turun." },
  { "en": "Apa Itu Real-Time Clock (RTC)?", "id": "IC Pewaktu (Jam Dan Kalender) Akurat." },
  { "en": "Contoh IC RTC (Real-Time Clock) Populer?", "id": "DS1307 Atau DS3231." },
  { "en": "Kenapa RTC (Real-Time Clock) Perlu Baterai Koin?", "id": "Agar Waktu Tetap Berjalan Saat Listrik Mati." },
  { "en": "Apa Itu SD Card Module?", "id": "Modul Pembaca Kartu Memori SD." },
  { "en": "Apa Itu Data Logging (Pencatatan Data)?", "id": "Proses Menyimpan Data Sensor Ke Memori." },
  { "en": "Apa Itu Sistem Embedded (Sistem Tertanam)?", "id": "Sistem Komputer Khusus Dalam Perangkat." },
  { "en": "Contoh Sistem Embedded (Sistem Tertanam)?", "id": "Microwave, Mesin Cuci, Router Wi-Fi." },
  { "en": "Apa Itu IoT (Internet of Things)?", "id": "Jaringan Perangkat Fisik Terhubung Internet." },
  { "en": "Apa Itu Shield (Perisai) Arduino?", "id": "Papan Tambahan Yang Dipasang Di Atas Arduino." },
  { "en": "Contoh Shield (Perisai) Arduino?", "id": "Motor Shield, Ethernet Shield, LCD Shield." },
  { "en": "Apa Itu Breadboard Power Supply?", "id": "Modul Catu Daya Khusus Untuk Breadboard." },
  { "en": "Apa Itu SMD (Surface Mount Device) Rework?", "id": "Proses Perbaikan (Solder Ulang) Komponen SMD." },
  { "en": "Apa Itu Reflow Soldering?", "id": "Metode Solder SMD Menggunakan Oven." },
  { "en": "Apa Itu Wave Soldering?", "id": "Metode Solder THT Gelombang Timah Cair." },
  { "en": "Apa Itu Solder Fume Extractor?", "id": "Alat Penghisap Asap Solder (Kesehatan)." },
  { "en": "Apa Itu Solder Tip Cleaner (Pembersih Mata Solder)?", "id": "Alat Pembersih Ujung Solder (Serabut Logam)." },
  { "en": "Apa Itu ESD (Electrostatic Discharge) Mat?", "id": "Alas Kerja Anti-Statis Pelindung Komponen." },
  { "en": "Apa Itu Voltase RMS (Root Mean Square)?", "id": "Nilai Efektif Tegangan AC." },
  { "en": "Berapa Voltase RMS Listrik PLN?", "id": "Sekitar 220 Volt AC." },
  { "en": "Apa Itu Voltase Puncak (Peak Voltage)?", "id": "Nilai Amplitudo Maksimum Sinyal AC." },
  { "en": "Apa Itu Voltase Puncak-Ke-Puncak (Peak-to-Peak)?", "id": "Nilai Antara Puncak Positif Dan Negatif." },
  { "en": "Apa Itu Faktor Daya (Power Factor)?", "id": "Rasio Daya Nyata Dan Daya Semu." },
  { "en": "Apa Itu Daya Nyata (Real Power)?", "id": "Daya Yang Benar-Benar Dipakai (Watt)." },
  { "en": "Apa Itu Daya Reaktif (Reactive Power)?", "id": "Daya Yang Dibutuhkan Beban Induktif/Kapasitif." },
  { "en": "Apa Itu Daya Semu (Apparent Power)?", "id": "Total Daya (Gabungan Nyata Reaktif)." },
  { "en": "Apa Satuan Daya Semu (Apparent Power)?", "id": "Volt-Ampere (VA)." },
  { "en": "Apa Itu Koreksi Faktor Daya (PFC)?", "id": "Memperbaiki Faktor Daya Mendekati Satu." },
  { "en": "Apa Itu Sistem Tiga Fasa (Three-Phase)?", "id": "Sistem Listrik Tiga Sinyal AC." },
  { "en": "Apa Itu Noise Common Mode?", "id": "Noise Yang Muncul Di Kedua Jalur Sinyal." },
  { "en": "Apa Itu Noise Differential Mode?", "id": "Noise Yang Muncul Di Antara Jalur Sinyal." },
  { "en": "Apa Itu Common Mode Choke?", "id": "Induktor Filter Peredam Noise Common Mode." },
  { "en": "Apa Itu Kapasitor X?", "id": "Kapasitor Peredam Noise Differential Mode (Antar Fasa)." },
  { "en": "Apa Itu Kapasitor Y?", "id": "Kapasitor Peredam Noise Common Mode (Fasa Ke Ground)." },
  { "en": "Apa Itu Rangkaian Clamping Zener?", "id": "Dioda Zener Pelindung Tegangan Berlebih." },
  { "en": "Apa Itu Galvanic Isolation (Isolasi Galvanik)?", "id": "Pemisahan Rangkaian Tanpa Koneksi Listrik Langsung." },
  { "en": "Bagaimana Cara Isolasi Galvanik?", "id": "Transformator, Optocoupler, Relay." },
  { "en": "Apa Keuntungan Isolasi Galvanik?", "id": "Keamanan (Mencegah Syok), Mengurangi Noise." },
  { "en": "Apa Itu Penguat Isolasi (Isolation Amplifier)?", "id": "Op-Amp Dengan Isolasi Galvanik." },
  { "en": "Apa Itu Penguat Diferensial (Differential Amplifier)?", "id": "Penguat Yang Menguatkan Selisih Dua Input." },
  { "en": "Fungsi Penguat Diferensial?", "id": "Menguatkan Sinyal Kecil Di Tengah Noise." },
  { "en": "Apa Itu Penguat Instrumentasi (Instrumentation Amplifier)?", "id": "Penguat Diferensial Presisi Tinggi." },
  { "en": "Apa Kelebihan Penguat Instrumentasi?", "id": "CMRR Tinggi, Impedansi Input Tinggi." },
  { "en": "Apa Itu Rangkaian Voltage Follower (Buffer)?", "id": "Op-Amp Dengan Penguatan Satu (Gain=1)." },
  { "en": "Apa Fungsi Rangkaian Voltage Follower?", "id": "Penyangga (Buffer), Mengisolasi Impedansi." },
  { "en": "Apa Itu Op-Amp Ideal?", "id": "Model Teoritis Op-Amp Sempurna." },
  { "en": "Sifat Op-Amp Ideal?", "id": "Gain Tak Terhingga, Impedansi Input Tak Terhingga." },
  { "en": "Apa Itu Op-Amp Inverting Amplifier?", "id": "Penguat Pembalik Fasa (180 Derajat)." },
  { "en": "Apa Itu Op-Amp Non-Inverting Amplifier?", "id": "Penguat Tanpa Membalik Fasa (0 Derajat)." },
  { "en": "Apa Itu Op-Amp Summing Amplifier?", "id": "Rangkaian Op-Amp Penjumlah Tegangan Input." },
  { "en": "Apa Itu Virtual Ground (Tanah Virtual)?", "id": "Titik Di Input Inverting Op-Amp (0V)." },
  { "en": "Apa Itu Input Bias Current Op-Amp?", "id": "Arus DC Kecil Di Pin Input." },
  { "en": "Apa Itu Input Offset Voltage Op-Amp?", "id": "Selisih Tegangan Input Agar Output Nol." },
  { "en": "Apa Itu Rail-to-Rail Op-Amp?", "id": "Op-Amp Yang Outputnya Mencapai Tegangan Supply." },
  { "en": "Apa Itu Komparator (Comparator) Analog?", "id": "Op-Amp Pembanding Dua Tegangan Analog." },
  { "en": "Apa Output Komparator (Comparator) Analog?", "id": "Logika High Atau Logika Low." },
  { "en": "Apa Beda Komparator Dan Op-Amp?", "id": "Komparator (Output Digital), Op-Amp (Feedback Analog)." },
  { "en": "Apa Itu Histeresis (Hysteresis)?", "id": "Perbedaan Level Pemicu (Mencegah Osilasi)." },
  { "en": "Apa Itu Schmitt Trigger?", "id": "Komparator Dengan Histeresis." },
  { "en": "Fungsi Rangkaian Schmitt Trigger?", "id": "Mengubah Sinyal Noise Menjadi Sinyal Bersih." },
  { "en": "Apa Itu Dioda Tunnel?", "id": "Dioda Dengan Efek Resistansi Negatif." },
  { "en": "Apa Itu Dioda IMPATT?", "id": "Dioda Penghasil Sinyal Frekuensi Sangat Tinggi." },
  { "en": "Apa Itu Dioda Solar (Solar Cell)?", "id": "Dioda Pengubah Cahaya Matahari Menjadi Listrik." },
  { "en": "Apa Itu Efek Fotovoltaik (Photovoltaic)?", "id": "Prinsip Kerja Dioda Solar Cell." },
  { "en": "Apa Itu Termopile (Thermopile)?", "id": "Sensor Pengukur Suhu (Radiasi Inframerah)." },
  { "en": "Apa Itu Sensor PIR (Passive Infrared) Pyroelectric?", "id": "Sensor Berbasis Bahan Piroelektrik." },
  { "en": "Apa Itu MEMS (Micro-Electro-Mechanical Systems)?", "id": "Sistem Mekanis Elektro Mikro (Sensor)." },
  { "en": "Contoh Sensor MEMS (Micro-Electro-Mechanical Systems)?", "id": "Akselerometer, Gyroscope, Mikrofon Di HP." },
  { "en": "Apa Itu Sinyal Diferensial?", "id": "Transmisi Sinyal Menggunakan Dua Kabel (Kebalikan)." },
  { "en": "Keuntungan Sinyal Diferensial?", "id": "Sangat Tahan Terhadap Noise Eksternal." },
  { "en": "Contoh Penggunaan Sinyal Diferensial?", "id": "Ethernet, USB, RS-485." },
  { "en": "Apa Itu RS-232?", "id": "Standar Komunikasi Serial (Tegangan Tinggi)." },
  { "en": "Apa Itu RS-485?", "id": "Standar Komunikasi Serial Diferensial (Multi-Drop)." },
  { "en": "Apa Itu CAN (Controller Area Network) Bus?", "id": "Protokol Jaringan Kuat (Otomotif, Industri)." },
  { "en": "Apa Itu K-Line?", "id": "Protokol Diagnostik Otomotif (OBD)." },
  { "en": "Apa Itu LIN (Local Interconnect Network) Bus?", "id": "Protokol Jaringan Otomotif Biaya Rendah." },
  { "en": "Apa Itu Modbus?", "id": "Protokol Komunikasi Industri (Master-Slave)." },
  { "en": "Apa Itu Fieldbus?", "id": "Jaringan Digital Industri (Pengganti 4-20mA)." },
  { "en": "Apa Itu Sinyal 4-20mA?", "id": "Standar Sinyal Analog Loop Arus Industri." },
  { "en": "Keuntungan Sinyal 4-20mA?", "id": "Tahan Noise Jarak Jauh." },
  { "en": "Apa Itu HART (Highway Addressable Remote Transducer)?", "id": "Protokol Hibrida (Analog 4-20mA Digital)." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Komputer Kontrol Industri Yang Tangguh." },
  { "en": "Apa Itu Troubleshooting (Pencarian Kesalahan)?", "id": "Proses Mencari Dan Memperbaiki Masalah." },
  { "en": "Langkah Pertama Troubleshooting?", "id": "Inspeksi Visual (Melihat Komponen Rusak)." },
  { "en": "Apa Tanda Komponen Terbakar?", "id": "Gosong, Retak, Menggembung, Bau Hangus." },
  { "en": "Apa Tanda Elco (Kapasitor Elektrolit) Rusak?", "id": "Bagian Atas Menggembung Atau Bocor." },
  { "en": "Bagaimana Mengukur Sekring (Fuse) Rusak?", "id": "Gunakan Multimeter Mode Kontinuitas (Buzzer)." },
  { "en": "Apa Arti Buzzer Berbunyi Saat Cek Sekring?", "id": "Sekring Masih Bagus (Tersambung)." },
  { "en": "Apa Arti Buzzer Diam Saat Cek Sekring?", "id": "Sekring Putus Atau Rusak." },
  { "en": "Bagaimana Mengukur Dioda Dengan Multimeter Analog?", "id": "Satu Arah Jarum Bergerak." },
  { "en": "Bagaimana Mengukur Dioda Dengan Multimeter Digital?", "id": "Mode Dioda Menampilkan Tegangan Maju." },
  { "en": "Apa Tanda Dioda Rusak (Short)?", "id": "Dua Arah Terhubung (Buzzer Bunyi)." },
  { "en": "Apa Tanda Dioda Rusak (Open)?", "id": "Dua Arah Terputus (Tidak Bergerak)." },
  { "en": "Bagaimana Mengukur Transistor BJT (NPN) Normal?", "id": "Basis Ke Kolektor/Emitor Seperti Dioda." },
  { "en": "Apa Itu Solder Dingin (Cold Solder Joint)?", "id": "Solderan Buruk, Retak, Tidak Menempel Sempurna." },
  { "en": "Bagaimana Memperbaiki Solder Dingin?", "id": "Memanaskan Ulang Solderan Dengan Timah Baru." },
  { "en": "Apa Itu Jumper Kabel?", "id": "Kabel Untuk Menghubungkan Titik Rangkaian." },
  { "en": "Apa Itu Pemotong Kawat (Wire Stripper)?", "id": "Alat Untuk Mengupas Isolasi Kabel." },
  { "en": "Apa Itu Catu Daya Laboratorium (Bench Power Supply)?", "id": "Sumber Listrik DC Yang Bisa Diatur." },
  { "en": "Apa Keuntungan Catu Daya Laboratorium?", "id": "Tegangan Dan Arus Bisa Diatur." },
  { "en": "Apa Itu Fitur CC (Constant Current)?", "id": "Fitur Pembatas Arus Maksimal Output." },
  { "en": "Apa Itu Fitur CV (Constant Voltage)?", "id": "Fitur Menjaga Tegangan Output Stabil." },
  { "en": "Apa Itu Isolator Listrik?", "id": "Bahan Yang Tidak Menghantarkan Listrik." },
  { "en": "Kenapa Pegangan Obeng Terbuat Dari Karet?", "id": "Karet Adalah Isolator Yang Baik." },
  { "en": "Apa Bahaya Listrik Statis (ESD)?", "id": "Merusak Komponen Sensitif Seperti IC CMOS." },
  { "en": "Apa Itu Arus Kejut (Electric Shock)?", "id": "Aliran Listrik Yang Melewati Tubuh." },
  { "en": "Kenapa Grounding (Pentanahan) Penting?", "id": "Melindungi Manusia Dan Alat Dari Listrik." },
  { "en": "Apa Itu MCB (Miniature Circuit Breaker)?", "id": "Pemutus Sirkuit Otomatis (Pengganti Sekring)." },
  { "en": "Apa Fungsi MCB (Miniature Circuit Breaker)?", "id": "Melindungi Jaringan Listrik Dari Beban Berlebih." },
  { "en": "Apa Itu ELCB (Earth Leakage Circuit Breaker)?", "id": "Pemutus Sirkuit Jika Ada Arus Bocor." },
  { "en": "Apa Fungsi ELCB (Earth Leakage Circuit Breaker)?", "id": "Melindungi Manusia Dari Arus Kejut." },
  { "en": "Apa Itu Voltmeter?", "id": "Alat Ukur Tegangan Listrik (Volt)." },
  { "en": "Bagaimana Cara Memasang Voltmeter?", "id": "Dipasang Paralel Dengan Beban." },
  { "en": "Apa Itu Amperemeter?", "id": "Alat Ukur Arus Listrik (Ampere)." },
  { "en": "Bagaimana Cara Memasang Amperemeter?", "id": "Dipasang Seri Dengan Beban." },
  { "en": "Apa Itu Ohm-meter?", "id": "Alat Ukur Hambatan Listrik (Ohm)." },
  { "en": "Syarat Mengukur Hambatan?", "id": "Rangkaian Harus Mati (Tidak Ada Tegangan)." },
  { "en": "Apa Itu Wattmeter?", "id": "Alat Ukur Daya Listrik (Watt)." },
  { "en": "Apa Itu KWh Meter?", "id": "Alat Ukur Energi Listrik (PLN)." },
  { "en": "Apa Itu Probe Osiloskop?", "id": "Kabel Input Penghubung Osiloskop Ke Rangkaian." },
  { "en": "Apa Itu Mode 1X Dan 10X Probe?", "id": "Pengaturan Atenuasi (Pelemahan) Sinyal Probe." },
  { "en": "Apa Itu Kalibrasi Probe Osiloskop?", "id": "Menyesuaikan Probe Menggunakan Sinyal Kotak." },
  { "en": "Apa Itu Rangkaian Astable Multivibrator?", "id": "Rangkaian Osilator Penghasil Sinyal Kotak." },
  { "en": "Apa Itu Rangkaian LED Berkedip (Blinker)?", "id": "Contoh Rangkaian Astable Multivibrator." },
  { "en": "Apa Itu Rangkaian Flip-Flop (Bistable)?", "id": "Rangkaian Penyimpan Memori Satu Bit." },
  { "en": "Apa Itu Rangkaian Saklar Sentuh (Touch Switch)?", "id": "Saklar Yang Aktif Karena Sentuhan." },
  { "en": "Apa Itu Rangkaian Sensor Cahaya Sederhana?", "id": "Rangkaian Menggunakan LDR Mengontrol LED." },
  { "en": "Apa Itu Rangkaian Sensor Gelap?", "id": "Lampu Otomatis Menyala Saat Gelap." },
  { "en": "Apa Itu Rangkaian Dimmer (Peredup Lampu)?", "id": "Rangkaian Pengatur Kecerahan Lampu AC." },
  { "en": "Komponen Utama Rangkaian Dimmer?", "id": "TRIAC Dan DIAC." },
  { "en": "Apa Itu Rangkaian Pengatur Kecepatan Kipas?", "id": "Rangkaian Serupa Dengan Dimmer Lampu." },
  { "en": "Apa Itu Joule Thief Circuit?", "id": "Rangkaian Penaik Tegangan DC Efisiensi Tinggi." },
  { "en": "Fungsi Joule Thief Circuit?", "id": "Menyalakan LED Dari Baterai Hampir Habis." },
  { "en": "Apa Itu Sinyal Sinus (Sine Wave)?", "id": "Bentuk Gelombang Murni (Listrik AC PLN)." },
  { "en": "Apa Itu Sinyal Kotak (Square Wave)?", "id": "Bentuk Gelombang Sinyal Digital (Clock)." },
  { "en": "Apa Itu Sinyal Segitiga (Triangle Wave)?", "id": "Bentuk Gelombang Naik Turun Linear." },
  { "en": "Apa Itu Sinyal Gigi Gergaji (Sawtooth Wave)?", "id": "Bentuk Gelombang Naik Linear, Turun Cepat." },
  { "en": "Apa Itu Duty Cycle (Siklus Kerja)?", "id": "Persentase Waktu Sinyal Dalam Kondisi HIGH." },
  { "en": "Berapa Duty Cycle Sinyal Kotak Simetris?", "id": "Lima Puluh Persen (50%)." },
  { "en": "Apa Itu Sinyal PWM (Pulse Width Modulation)?", "id": "Sinyal Dengan Duty Cycle Yang Berubah-ubah." },
  { "en": "Apa Fungsi Sinyal PWM (Pulse Width Modulation)?", "id": "Mengontrol Kecerahan LED, Kecepatan Motor." },
  { "en": "Apa Itu Audio Amplifier (Penguat Audio)?", "id": "Rangkaian Penguat Sinyal Suara." },
  { "en": "Apa Itu Pre-Amp (Penguat Awal)?", "id": "Penguat Sinyal Lemah (Mikrofon) Sebelum Power Amp." },
  { "en": "Apa Itu Power Amplifier (Penguat Daya)?", "id": "Penguat Sinyal Untuk Menggerakkan Speaker." },
  { "en": "Apa Itu Tone Control (Kontrol Nada)?", "id": "Rangkaian Pengatur Bass Dan Treble Audio." },
  { "en": "Apa Itu Equalizer (Grafik)?", "id": "Rangkaian Pengatur Frekuensi Audio Spesifik." },
  { "en": "Apa Itu Crossover (Audio)?", "id": "Rangkaian Pemisah Frekuensi Audio." },
  { "en": "Apa Fungsi Crossover (Audio)?", "id": "Membagi Sinyal Ke Woofer Dan Tweeter." },
  { "en": "Apa Itu Woofer (Speaker)?", "id": "Speaker Khusus Suara Frekuensi Rendah (Bass)." },
  { "en": "Apa Itu Tweeter (Speaker)?", "id": "Speaker Khusus Suara Frekuensi Tinggi (Treble)." },
  { "en": "Apa Itu Speaker Mid-Range?", "id": "Speaker Khusus Suara Frekuensi Tengah (Vokal)." },
  { "en": "Apa Itu Subwoofer?", "id": "Speaker Khusus Suara Frekuensi Sangat Rendah." },
  { "en": "Apa Itu Impedansi Speaker?", "id": "Hambatan Total Speaker (Contoh 8 Ohm)." },
  { "en": "Apa Itu Konversi Energi?", "id": "Perubahan Satu Bentuk Energi Ke Lainnya." },
  { "en": "Contoh Konversi Energi Pada LED?", "id": "Energi Listrik Menjadi Energi Cahaya." },
  { "en": "Contoh Konversi Energi Pada Motor?", "id": "Energi Listrik Menjadi Energi Gerak." },
  { "en": "Contoh Konversi Energi Pada Mikrofon?", "id": "Energi Suara Menjadi Energi Listrik." },
  { "en": "Apa Itu Efisiensi Energi?", "id": "Persentase Energi Yang Diubah Menjadi Bermanfaat." },
  { "en": "Kenapa Komponen Elektronik Panas?", "id": "Sisa Energi Yang Terbuang Menjadi Panas." },
  { "en": "Apa Itu Satuan Piko (Pico)?", "id": "Faktor Sepersetriliun (10 Pangkat -12)." },
  { "en": "Contoh Satuan Piko (Pico)?", "id": "PikoFarad (pF)." },
  { "en": "Apa Itu Satuan Nano (Nano)?", "id": "Faktor Sepersemiliar (10 Pangkat -9)." },
  { "en": "Contoh Satuan Nano (Nano)?", "id": "NanoFarad (nF), NanoHenry (nH)." },
  { "en": "Apa Itu Satuan Mikro (Micro)?", "id": "Faktor Sepersejuta (10 Pangkat -6)." },
  { "en": "Contoh Satuan Mikro (Micro)?", "id": "MikroFarad (uF), MikroAmpere (uA)." },
  { "en": "Apa Itu Satuan Mili (Milli)?", "id": "Faktor Seperseribu (10 Pangkat -3)." },
  { "en": "Contoh Satuan Mili (Milli)?", "id": "MiliVolt (mV), MiliAmpere (mA)." },
  { "en": "Apa Itu Satuan Kilo (Kilo)?", "id": "Faktor Seribu (10 Pangkat 3)." },
  { "en": "Contoh Satuan Kilo (Kilo)?", "id": "KiloOhm (KΩ), KiloHertz (KHz)." },
  { "en": "Apa Itu Satuan Mega (Mega)?", "id": "Faktor Sejuta (10 Pangkat 6)." },
  { "en": "Contoh Satuan Mega (Mega)?", "id": "MegaOhm (MΩ), MegaHertz (MHz)." },
  { "en": "Apa Itu Satuan Giga (Giga)?", "id": "Faktor Semiliar (10 Pangkat 9)." },
  { "en": "Contoh Satuan Giga (Giga)?", "id": "GigaHertz (GHz)." },
  { "en": "Apa Itu Open Source Hardware (Perangkat Keras Terbuka)?", "id": "Desain Hardware Yang Dibagikan Gratis." },
  { "en": "Contoh Open Source Hardware?", "id": "Arduino, Raspberry Pi." },
  { "en": "Apa Itu Komunitas Maker (Pembuat)?", "id": "Komunitas Hobi Elektronika Dan Proyek DIY." },
  { "en": "Apa Itu DIY (Do It Yourself)?", "id": "Proyek Yang Dibuat Sendiri (Swakarya)." },
  { "en": "Apa Itu Datasheet Pin Configuration?", "id": "Gambar Denah Kaki-Kaki Komponen IC." },
  { "en": "Apa Itu Absolute Maximum Ratings?", "id": "Batas Maksimal (Tegangan, Arus) Komponen." },
  { "en": "Apa Itu Recommended Operating Conditions?", "id": "Kondisi Operasi Normal Rekomendasi Pabrik." },
  { "en": "Apa Itu Electrical Characteristics?", "id": "Tabel Spesifikasi Kelistrikan Komponen Detail." },
  { "en": "Apa Itu Package Outline Drawing?", "id": "Gambar Dimensi Fisik Komponen (Ukuran)." },
  { "en": "Apa Itu Footprint (Jejak) Komponen?", "id": "Pola Pad Tembaga Di PCB." },
  { "en": "Kenapa Footprint (Jejak) Penting?", "id": "Agar Kaki Komponen Pas Saat Disolder." },
  { "en": "Apa Itu Aplikasi Khas (Typical Application)?", "id": "Contoh Rangkaian Penggunaan Komponen." },
  { "en": "Apa Itu Revision History (Sejarah Revisi)?", "id": "Catatan Perubahan Pada Datasheet." },
  { "en": "Apa Itu Indeks Proteksi IP (Ingress Protection)?", "id": "Standar Perlindungan Terhadap Debu Air." },
  { "en": "Apa Arti IP67?", "id": "Tahan Debu Total, Tahan Air (Tenggelam 1m)." },
  { "en": "Apa Arti IP68?", "id": "Tahan Debu Total, Tahan Air (Tenggelam >1m)." },
  { "en": "Apa Itu Standar NEMA (National Electrical Manufacturers Association)?", "id": "Standar Boks Panel Listrik (Amerika)." },
  { "en": "Apa Itu UL (Underwriters Laboratories) Rating?", "id": "Sertifikasi Keamanan Produk Listrik." },
  { "en": "Apa Itu CE (Conformité Européenne) Marking?", "id": "Tanda Kesesuaian Standar Produk Eropa." },
  { "en": "Apa Itu FCC (Federal Communications Commission) Warning?", "id": "Standar Gangguan Frekuensi Radio (Amerika)." },
  { "en": "Apa Itu Komponen Pasif?", "id": "Komponen Yang Tidak Memerlukan Catu Daya." },
  { "en": "Contoh Komponen Pasif?", "id": "Resistor, Kapasitor, Induktor." },
  { "en": "Apa Itu Komponen Aktif?", "id": "Komponen Yang Memerlukan Catu Daya." },
  { "en": "Contoh Komponen Aktif?", "id": "Transistor, Dioda, IC." },
  { "en": "Apakah Dioda Komponen Aktif?", "id": "Ya, Dioda Memerlukan Bias Tegangan." },
  { "en": "Apa Itu Rangkaian Linear?", "id": "Output Berbanding Lurus Dengan Input." },
  { "en": "Apa Itu Rangkaian Non-Linear?", "id": "Output Tidak Berbanding Lurus Input." },
  { "en": "Apa Itu Linearitas (Linearity)?", "id": "Ukuran Kesesuaian Output Terhadap Input." },
  { "en": "Apa Itu Kalibrasi (Calibration)?", "id": "Proses Penyesuaian Alat Ukur Standar." },
  { "en": "Kenapa Kalibrasi Alat Ukur Penting?", "id": "Untuk Menjamin Keakuratan Hasil Pengukuran." },
  { "en": "Apa Itu Akurasi (Accuracy)?", "id": "Seberapa Dekat Pengukuran Dengan Nilai Sebenarnya." },
  { "en": "Apa Itu Presisi (Precision)?", "id": "Seberapa Konsisten Hasil Pengukuran Berulang." },
  { "en": "Apakah Akurat Pasti Presisi?", "id": "Biasanya Ya, Tapi Belum Tentu." },
  { "en": "Apakah Presisi Pasti Akurat?", "id": "Belum Tentu, Bisa Presisi Tapi Salah." },
  { "en": "Apa Itu Error (Kesalahan) Pengukuran?", "id": "Selisih Antara Nilai Ukur Dan Sebenarnya." },
  { "en": "Apa Itu Error Sistematis?", "id": "Kesalahan Tetap (Contoh: Salah Kalibrasi)." },
  { "en": "Apa Itu Error Acak (Random)?", "id": "Kesalahan Tak Terduga (Contoh: Noise)." },
  { "en": "Apa Itu Parallax Error?", "id": "Kesalahan Baca Karena Sudut Pandang (Analog)." },
  { "en": "Apa Itu Multimeter True RMS?", "id": "Multimeter Pengukur Nilai RMS Akurat (Non-Sinus)." },
  { "en": "Kapan Perlu Multimeter True RMS?", "id": "Saat Mengukur Sinyal AC Non-Sinusoidal." },
  { "en": "Apa Itu Auto-Ranging Multimeter?", "id": "Multimeter Yang Otomatis Memilih Rentang Ukur." },
  { "en": "Apa Itu Manual-Ranging Multimeter?", "id": "Multimeter Manual Memilih Rentang (Selector)." },
  { "en": "Apa Itu Mode Kontinuitas (Continuity)?", "id": "Mode Cek Sambungan (Buzzer Berbunyi)." },
  { "en": "Apa Itu Mode Tahan Data (Data Hold)?", "id": "Fitur Menahan Angka Pengukuran Di Layar." },
  { "en": "Apa Itu CAT (Category) Rating Multimeter?", "id": "Peringkat Keamanan Multimeter." },
  { "en": "Rating CAT Mana Untuk Listrik Rumah?", "id": "CAT III Atau CAT IV." },
  { "en": "Apa Itu Sensor Suhu Inframerah (IR)?", "id": "Pengukur Suhu Tanpa Sentuhan (Termometer Tembak)." },
  { "en": "Apa Itu Emisivitas (Emissivity)?", "id": "Kemampuan Benda Memancarkan Radiasi Termal." },
  { "en": "Apa Itu Kamera Termal (Thermal Camera)?", "id": "Kamera Penglihat Radiasi Panas (Inframerah)." },
  { "en": "Fungsi Kamera Termal Di Elektronika?", "id": "Mencari Komponen Yang Panas Berlebih (Overheat)." },
  { "en": "Apa Itu Sinyal Baseband?", "id": "Sinyal Informasi Asli Sebelum Modulasi." },
  { "en": "Apa Itu Sinyal Broadband (Pita Lebar)?", "id": "Transmisi Data Rentang Frekuensi Lebar." },
  { "en": "Apa Itu Duplex (Komunikasi)?", "id": "Komunikasi Dua Arah." },
  { "en": "Apa Itu Full-Duplex?", "id": "Komunikasi Dua Arah Bersamaan (Telepon)." },
  { "en": "Apa Itu Half-Duplex?", "id": "Komunikasi Dua Arah Bergantian (Walkie-Talkie)." },
  { "en": "Apa Itu Simplex (Komunikasi)?", "id": "Komunikasi Satu Arah Saja (Radio Siaran)." },
  { "en": "Apa Itu Atenuasi (Attenuation)?", "id": "Pelemahan Kekuatan Sinyal Seiring Jarak." },
  { "en": "Kenapa Sinyal Perlu Diperkuat (Amplified)?", "id": "Untuk Melawan Efek Atenuasi." },
  { "en": "Apa Itu Repeater (Pengulang)?", "id": "Perangkat Penerima Dan Pemancar Ulang Sinyal." },
  { "en": "Apa Itu Impedance Matching (Pencocokan Impedansi)?", "id": "Menyamakan Impedansi Sumber Dan Beban." },
  { "en": "Apa Tujuan Impedance Matching?", "id": "Transfer Daya Maksimal, Mengurangi Pantulan." },
  { "en": "Apa Akibat Impedansi Tidak Cocok (Mismatch)?", "id": "Sinyal Memantul, Rugi Daya, SWR Tinggi." },
  { "en": "Apa Itu Balun (Balanced to Unbalanced)?", "id": "Transformator Impedansi (Balanced Ke Unbalanced)." },
  { "en": "Contoh Penggunaan Balun?", "id": "Menghubungkan Antena Dipole (Balanced) Ke Koaksial (Unbalanced)." },
  { "en": "Apa Itu Konektor DB9 (D-Sub 9)?", "id": "Konektor 9-Pin (Umumnya Serial RS-232)." },
  { "en": "Apa Itu Konektor DB25 (D-Sub 25)?", "id": "Konektor 25-Pin (Port Paralel Printer Lama)." },
  { "en": "Apa Itu Konektor IDC (Insulation Displacement Connector)?", "id": "Konektor Kabel Pita (Gepeng) Komputer." },
  { "en": "Apa Itu Kabel Serat Optik (Fiber Optic)?", "id": "Kabel Transmisi Data Menggunakan Cahaya." },
  { "en": "Apa Kelebihan Serat Optik?", "id": "Kecepatan Sangat Tinggi, Tahan Interferensi." },
  { "en": "Apa Bahan Inti Serat Optik?", "id": "Kaca (Silika) Atau Plastik." },
  { "en": "Apa Itu Transceiver Optik?", "id": "Perangkat Pengirim Penerima Sinyal Cahaya." },
  { "en": "Apa Itu LED (Light Emitting Diode) Surface Mount?", "id": "LED Ukuran Sangat Kecil (SMD)." },
  { "en": "Contoh Ukuran Paket LED SMD?", "id": "0805, 0603, 1206." },
  { "en": "Apa Itu LED (Light Emitting Diode) Addressable?", "id": "LED Yang Bisa Diatur Warna Individual." },
  { "en": "Contoh LED (Light Emitting Diode) Addressable?", "id": "WS2812B Atau NeoPixel." },
  { "en": "Apa Itu Neopixel?", "id": "Merek Dagang Adafruit Untuk LED WS2812." },
  { "en": "Apa Itu Resistor Array (Jaringan Resistor)?", "id": "Beberapa Resistor Dalam Satu Paket." },
  { "en": "Apa Itu Kapasitor Array (Jaringan Kapasitor)?", "id": "Beberapa Kapasitor Dalam Satu Paket." },
  { "en": "Apa Itu Dioda Array (Jaringan Dioda)?", "id": "Beberapa Dioda Dalam Satu Paket." },
  { "en": "Apa Itu Penguat Operasional (Op-Amp) Ganda?", "id": "Dua Op-Amp Dalam Satu IC (Contoh: LM358)." },
  { "en": "Apa Itu Penguat Operasional (Op-Amp) Quad?", "id": "Empat Op-Amp Dalam Satu IC (Contoh: LM324)." },
  { "en": "Apa Itu Rangkaian Monostable Multivibrator?", "id": "Rangkaian Penghasil Satu Pulsa (One-Shot)." },
  { "en": "Apa Fungsi Rangkaian Monostable?", "id": "Timer Sederhana, Debouncing." },
  { "en": "Apa Itu Rangkaian Voltage Doubler?", "id": "Rangkaian Pengganda Tegangan DC." },
  { "en": "Apa Itu Rangkaian Voltage Tripler?", "id": "Rangkaian Pengali Tiga Tegangan DC." },
  { "en": "Apa Itu Rangkaian Pompa Muatan (Charge Pump)?", "id": "Rangkaian Penaik Tegangan DC Tanpa Induktor." },
  { "en": "Contoh IC Pompa Muatan (Charge Pump)?", "id": "MAX232 (Driver RS-232)." },
  { "en": "Apa Itu Regulator Tegangan Negatif?", "id": "IC Penstabil Tegangan Output Negatif." },
  { "en": "Contoh Regulator Tegangan Negatif?", "id": "Seri 79xx (Contoh: 7905)." },
  { "en": "Apa Itu Catu Daya Ganda (Dual Supply)?", "id": "Sumber Listrik Tegangan Positif Dan Negatif." },
  { "en": "Kapan Catu Daya Ganda Dibutuhkan?", "id": "Sering Digunakan Untuk Rangkaian Op-Amp." },
  { "en": "Apa Itu Rangkaian Zero Crossing Detector?", "id": "Rangkaian Pendeteksi Sinyal AC Melewati Nol." },
  { "en": "Apa Fungsi Rangkaian Zero Crossing?", "id": "Sinkronisasi Kontrol TRIAC (Dimmer)." },
  { "en": "Apa Itu Rangkaian Sample and Hold?", "id": "Rangkaian Pengambil Dan Penahan Sinyal Analog." },
  { "en": "Di Mana Rangkaian Sample and Hold Digunakan?", "id": "Di Dalam ADC (Analog to Digital Converter)." },
  { "en": "Apa Itu Efek Miller?", "id": "Peningkatan Kapasitansi Input Pada Penguat." },
  { "en": "Apa Itu Decibel-Milliwatts (dBm)?", "id": "Satuan Pengukuran Daya (Level Sinyal RF)." },
  { "en": "Apa Itu Open-Collector (Kolektor Terbuka)?", "id": "Output Transistor Tanpa Resistor Pull-Up Internal." },
  { "en": "Apa Keuntungan Output Open-Collector?", "id": "Dapat Dihubungkan Ke Tegangan Berbeda." },
  { "en": "Apa Itu Open-Drain (Drain Terbuka)?", "id": "Sama Seperti Open-Collector, Tapi Untuk MOSFET." },
  { "en": "Apa Itu Logika Wired-AND?", "id": "Menggabungkan Beberapa Output Open-Collector." },
  { "en": "Apa Itu Output Totem-Pole?", "id": "Output Transistor Push-Pull Standar TTL." },
  { "en": "Apa Itu Logika Tri-State (Tiga Keadaan)?", "id": "Logika Output High, Low, Atau Impedansi Tinggi." },
  { "en": "Apa Keadaan Impedansi Tinggi (High-Z)?", "id": "Output Terputus (Seperti Tidak Tersambung)." },
  { "en": "Apa Fungsi Logika Tri-State?", "id": "Memungkinkan Banyak Perangkat Berbagi Bus." },
  { "en": "Apa Itu Bus (Dalam Digital)?", "id": "Jalur Komunikasi Data Paralel Bersama." },
  { "en": "Apa Itu Fan-Out (Keluarga Logika)?", "id": "Jumlah Maksimal Input Yang Bisa Digerakkan." },
  { "en": "Apa Itu Fan-In (Keluarga Logika)?", "id": "Jumlah Input Pada Satu Gerbang Logika." },
  { "en": "Apa Itu Noise Margin (Batas Kebisingan)?", "id": "Toleransi Level Tegangan Terhadap Noise." },
  { "en": "Apa Itu Propagation Delay (Tunda Propagasi)?", "id": "Waktu Tunda Sinyal Melewati Gerbang." },
  { "en": "Apa Itu DisipASI Daya (Power Dissipation)?", "id": "Jumlah Daya Yang Digunakan Komponen." },
  { "en": "Apa Itu Keluarga Logika ECL (Emitter-Coupled Logic)?", "id": "Logika Digital Sangat Cepat (Boros Daya)." },
  { "en": "Apa Itu Keluarga Logika IIL (Integrated Injection Logic)?", "id": "Logika Digital Kepadatan Tinggi." },
  { "en": "Apa Itu Kapasitor Tantalum SMD?", "id": "Kapasitor Polar SMD Dengan Efisiensi Tinggi." },
  { "en": "Bagaimana Tanda Polaritas Kapasitor Tantalum SMD?", "id": "Garis Menandakan Kutub Positif (Anoda)." },
  { "en": "Apa Beda Polaritas Tantalum Dan Elco?", "id": "Tantalum (Garis=Positif), Elco (Garis=Negatif)." },
  { "en": "Apa Itu Induktor SMD (Surface Mount Device)?", "id": "Induktor Kecil Pemasangan Permukaan." },
  { "en": "Apa Itu Resistor Film Tipis (Thin Film)?", "id": "Resistor Presisi Tinggi (Akurasi Baik)." },
  { "en": "Apa Itu Resistor Film Tebal (Thick Film)?", "id": "Resistor Umum SMD (Toleransi Standar)." },
  { "en": "Apa Itu Resistor MELF (Metal Electrode Leadless Face)?", "id": "Resistor SMD Berbentuk Silinder." },
  { "en": "Apa Itu Resistor Shunt (Shunt Resistor)?", "id": "Resistor Hambatan Sangat Kecil (Pengukur Arus)." },
  { "en": "Bagaimana Resistor Shunt Mengukur Arus?", "id": "Mengukur Penurunan Tegangan Di Resistor." },
  { "en": "Apa Itu Rangkaian RC Snubber?", "id": "Rangkaian Peredam Lonjakan Tegangan (Noise)." },
  { "en": "Apa Itu Rangkaian Pi Filter?", "id": "Filter Pasif Bentuk Huruf Pi (CLC/LCL)." },
  { "en": "Apa Itu Rangkaian T Filter?", "id": "Filter Pasif Bentuk Huruf T (RCR/LCL)." },
  { "en": "Apa Itu Filter Aktif?", "id": "Filter Yang Menggunakan Komponen Aktif (Op-Amp)." },
  { "en": "Apa Keuntungan Filter Aktif?", "id": "Dapat Memberi Penguatan (Gain)." },
  { "en": "Apa Itu Filter Sallen-Key?", "id": "Topologi Filter Aktif Populer." },
  { "en": "Apa Itu Power MOSFET?", "id": "MOSFET Khusus Aplikasi Saklar Daya Tinggi." },
  { "en": "Apa Itu RDS(on) MOSFET?", "id": "Hambatan Drain-Source Saat Kondisi Menyala." },
  { "en": "Kenapa RDS(on) Penting?", "id": "RDS(on) Rendah Berarti Efisiensi Tinggi." },
  { "en": "Apa Itu Gerbang (Gate) Kapasitansi MOSFET?", "id": "Kapasitansi Yang Perlu Diisi Untuk Menyala." },
  { "en": "Apa Itu Gate Driver (Driver Gerbang)?", "id": "IC Khusus Untuk Memicu Gerbang MOSFET." },
  { "en": "Kenapa MOSFET Perlu Gate Driver?", "id": "Mengisi Gerbang Kapasitansi Dengan Cepat." },
  { "en": "Apa Itu Dioda Bodi (Body Diode) MOSFET?", "id": "Dioda Parasitik Internal Pada MOSFET." },
  { "en": "Apa Itu Avalanche Breakdown?", "id": "Tegangan Tembus Dioda (Efek Zener)." },
  { "en": "Apa Itu Penguat Common Emitter (BJT)?", "id": "Konfigurasi Penguat BJT Penguatan Tinggi." },
  { "en": "Apa Sifat Penguat Common Emitter?", "id": "Membalik Fasa (180 Derajat)." },
  { "en": "Apa Itu Penguat Common Collector (BJT)?", "id": "Konfigurasi Penguat Buffer (Emitter Follower)." },
  { "en": "Apa Sifat Penguat Common Collector?", "id": "Penguatan Tegangan Satu (Tidak Membalik)." },
  { "en": "Apa Itu Penguat Common Base (BJT)?", "id": "Konfigurasi Penguat Impedansi Input Rendah." },
  { "en": "Apa Itu Penguat Common Source (FET)?", "id": "Konfigurasi Penguat FET (Mirip Common Emitter)." },
  { "en": "Apa Itu Penguat Common Drain (FET)?", "id": "Konfigurasi Penguat FET (Source Follower)." },
  { "en": "Apa Itu Penguat Common Gate (FET)?", "id": "Konfigurasi Penguat FET (Mirip Common Base)." },
  { "en": "Apa Itu Garis Beban DC (DC Load Line)?", "id": "Grafik Analisis Titik Kerja Transistor." },
  { "en": "Apa Itu Titik Q (Quiescent Point)?", "id": "Titik Operasi Transistor Saat Diam (Tanpa Sinyal)." },
  { "en": "Apa Itu Biasing Transistor?", "id": "Memberi Tegangan DC Agar Beroperasi." },
  { "en": "Kenapa Biasing Transistor Penting?", "id": "Menempatkan Titik Q Di Daerah Aktif." },
  { "en": "Apa Itu Biasing Pembagi Tegangan?", "id": "Metode Biasing BJT Paling Stabil." },
  { "en": "Apa Itu Thermal Runaway (Transistor)?", "id": "Kondisi Transistor Panas (Arus Naik Merusak)." },
  { "en": "Bagaimana Mencegah Thermal Runaway?", "id": "Desain Biasing Stabil, Pendingin (Heatsink)." },
  { "en": "Apa Itu Penguat Diferensial Pasangan Ekor Panjang?", "id": "Dasar Rangkaian Internal Op-Amp." },
  { "en": "Apa Itu Current Mirror (Cermin Arus)?", "id": "Rangkaian Penyalin Arus Referensi." },
  { "en": "Apa Itu Sumber Arus Konstan?", "id": "Rangkaian Penghasil Arus Output Stabil." },
  { "en": "Apa Itu Penguat Cascode?", "id": "Konfigurasi Penguat Bandwidth Tinggi." },
  { "en": "Apa Itu Penguat Push-Pull?", "id": "Dua Transistor Bekerja Bergantian (Kelas B/AB)." },
  { "en": "Apa Itu Tabung Hampa (Vacuum Tube) Triode?", "id": "Tabung Tiga Elemen (Katoda, Anoda, Grid)." },
  { "en": "Apa Itu Tabung Hampa (Vacuum Tube) Pentode?", "id": "Tabung Lima Elemen." },
  { "en": "Kenapa Penguat Tabung (Tube Amp) Masih Digunakan?", "id": "Karakter Suara Unik (Audio Gitar)." },
  { "en": "Apa Itu Sinyal RF (Radio Frequency)?", "id": "Sinyal Elektromagnetik Frekuensi Tinggi." },
  { "en": "Apa Itu Rangkaian Tank (Tank Circuit)?", "id": "Rangkaian Resonansi LC Paralel." },
  { "en": "Apa Fungsi Rangkaian Tank?", "id": "Filter Band-Pass, Osilator RF." },
  { "en": "Apa Itu Modulasi PPM (Pulse Position Modulation)?", "id": "Modulasi Menggeser Posisi Pulsa." },
  { "en": "Apa Itu Modulasi PAM (Pulse Amplitude Modulation)?", "id": "Modulasi Mengubah Amplitudo Pulsa." },
  { "en": "Apa Itu Multiplexing (Multipleksi)?", "id": "Mengirim Banyak Sinyal Lewat Satu Saluran." },
  { "en": "Apa Itu FDM (Frequency Division Multiplexing)?", "id": "Multipleksi Berbasis Pembagian Frekuensi." },
  { "en": "Apa Itu TDM (Time Division Multiplexing)?", "id": "Multipleksi Berbasis Pembagian Waktu." },
  { "en": "Apa Itu CDM (Code Division Multiplexing)?", "id": "Multipleksi Berbasis Pembagian Kode Unik." },
  { "en": "Apa Itu Bit Rate?", "id": "Kecepatan Transfer Data (Bit Per Detik)." },
  { "en": "Apa Itu Baud Rate?", "id": "Kecepatan Perubahan Simbol Per Detik." },
  { "en": "Apakah Bit Rate Selalu Sama Baud Rate?", "id": "Tidak Selalu, Tergantung Modulasi." },
  { "en": "Apa Itu Paritas Bit (Parity Bit)?", "id": "Bit Tambahan Untuk Pengecekan Error." },
  { "en": "Apa Itu Paritas Ganjil (Odd Parity)?", "id": "Jumlah Bit 1 Selalu Ganjil." },
  { "en": "Apa Itu Paritas Genap (Even Parity)?", "id": "Jumlah Bit 1 Selalu Genap." },
  { "en": "Apa Itu Checksum?", "id": "Metode Pengecekan Error Blok Data." },
  { "en": "Apa Itu CRC (Cyclic Redundancy Check)?", "id": "Metode Pengecekan Error Lebih Kuat." },
  { "en": "Apa Itu Handshaking (Jabat Tangan)?", "id": "Proses Negosiasi Protokol Komunikasi." },
  { "en": "Apa Itu Kontrol Aliran (Flow Control)?", "id": "Mengatur Kecepatan Pengiriman Data." },
  { "en": "Apa Itu Kontrol Aliran Perangkat Keras (Hardware)?", "id": "Menggunakan Pin Khusus (RTS/CTS)." },
  { "en": "Apa Itu Kontrol Aliran Perangkat Lunak (Software)?", "id": "Menggunakan Karakter Khusus (XON/XOFF)." },
  { "en": "Apa Itu RTS (Request To Send)?", "id": "Sinyal Siap Mengirim Data." },
  { "en": "Apa Itu CTS (Clear To Send)?", "id": "Sinyal Balasan (Boleh Mengirim Data)." },
  { "en": "Apa Itu Loopback Test (Tes Loopback)?", "id": "Tes Menghubungkan TX Ke RX Sendiri." },
  { "en": "Apa Itu Bit Order (Urutan Bit)?", "id": "Urutan Pengiriman (LSB Dulu Atau MSB Dulu)." },
  { "en": "Apa Itu Endianness?", "id": "Urutan Penyimpanan Byte Dalam Memori." },
  { "en": "Apa Itu Big-Endian?", "id": "Byte Paling Signifikan Disimpan Dulu." },
  { "en": "Apa Itu Little-Endian?", "id": "Byte Paling Tidak Signifikan Disimpan Dulu." },
  { "en": "Prosesor Intel Menggunakan Endian Apa?", "id": "Little-Endian." },
  { "en": "Apa Itu Bilangan Floating Point?", "id": "Representasi Bilangan Desimal (Koma)." },
  { "en": "Apa Itu Bilangan Fixed Point?", "id": "Representasi Bilangan Desimal (Posisi Koma Tetap)." },
  { "en": "Apa Itu Two's Complement (Komplemen Dua)?", "id": "Metode Representasi Bilangan Biner Negatif." },
  { "en": "Apa Itu ALU (Arithmetic Logic Unit) Overflow?", "id": "Hasil Perhitungan Melebihi Kapasitas Bit." },
  { "en": "Apa Itu Gray Code (Kode Gray)?", "id": "Kode Biner (Hanya Satu Bit Berubah)." },
  { "en": "Kenapa Menggunakan Gray Code?", "id": "Mencegah Error Posisi (Encoder Putar)." },
  { "en": "Apa Itu Papan Pengembangan (Development Board)?", "id": "Papan Siap Pakai (Mikrokontroler Aksesori)." },
  { "en": "Contoh Papan Pengembangan?", "id": "Arduino Uno, NodeMCU, Raspberry Pi." },
  { "en": "Apa Itu Single-Board Computer (SBC)?", "id": "Komputer Lengkap Dalam Satu Papan." },
  { "en": "Contoh Single-Board Computer (SBC)?", "id": "Raspberry Pi." },
  { "en": "Beda Mikrokontroler Dan SBC?", "id": "Mikrokontroler (Tugas Spesifik), SBC (Komputer Umum)." },
  { "en": "Apa Itu Sistem Operasi (Operating System) Real-Time?", "id": "RTOS (Real-Time Operating System)." },
  { "en": "Apa Fungsi RTOS (Real-Time Operating System)?", "id": "Menjamin Respon Tugas Dalam Waktu Pasti." },
  { "en": "Apa Itu Determinisme (Waktu Nyata)?", "id": "Kemampuan Memprediksi Waktu Eksekusi Tugas." },
  { "en": "Apa Itu Jitter (Waktu Nyata)?", "id": "Variasi Waktu Tunda Eksekusi Tugas." },
  { "en": "Apa Itu Arduino Uno?", "id": "Papan Mikrokontroler Berbasis ATmega328P." },
  { "en": "Apa Itu Arduino Nano?", "id": "Versi Kompak Arduino Uno (Breadboard-Friendly)." },
  { "en": "Apa Itu Arduino Mega?", "id": "Papan Arduino Dengan Lebih Banyak Pin I/O." },
  { "en": "Apa Mikrokontroler Arduino Mega?", "id": "Berbasis ATmega2560." },
  { "en": "Apa Itu Raspberry Pi?", "id": "Single-Board Computer (SBC) Berbasis Linux." },
  { "en": "Apakah Raspberry Pi Punya Pin GPIO?", "id": "Ya, Memiliki 40 Pin GPIO." },
  { "en": "Apa Itu Raspberry Pi Pico?", "id": "Papan Mikrokontroler Buatan Raspberry Pi." },
  { "en": "Apa Chip Mikrokontroler Pi Pico?", "id": "Berbasis RP2040." },
  { "en": "Apa Itu NodeMCU?", "id": "Papan Pengembangan Berbasis ESP8266 (Wi-Fi)." },
  { "en": "Apa Itu ESP32?", "id": "Penerus ESP8266 (Wi-Fi Dan Bluetooth)." },
  { "en": "Bahasa Pemrograman Arduino?", "id": "C++ (Dengan Kerangka Arduino)." },
  { "en": "Bahasa Pemrograman Raspberry Pi?", "id": "Python (Umum), C++, Lainnya." },
  { "en": "Bahasa Pemrograman ESP8266/ESP32?", "id": "Arduino (C++) Atau MicroPython." },
  { "en": "Apa Itu MicroPython?", "id": "Implementasi Python Untuk Mikrokontroler." },
  { "en": "Apa Itu CircuitPython?", "id": "Cabang MicroPython Dari Adafruit." },
  { "en": "Apa Itu Pin Analog (Arduino)?", "id": "Pin Pembaca Tegangan Analog (Menggunakan ADC)." },
  { "en": "Apa Itu Pin Digital (Arduino)?", "id": "Pin Pembaca/Penulis Logika (High/Low)." },
  { "en": "Apa Itu Pin PWM (Arduino)?", "id": "Pin Digital Penghasil Sinyal PWM (Tanda ~)." },
  { "en": "Apa Itu Pin AREF (Analog Reference)?", "id": "Pin Referensi Tegangan Maksimal ADC." },
  { "en": "Apa Itu Pin Vin (Arduino)?", "id": "Pin Input Tegangan (Catu Daya Eksternal)." },
  { "en": "Apa Itu Pin 5V (Arduino)?", "id": "Pin Output Tegangan 5V Teregulasi." },
  { "en": "Apa Itu Pin 3.3V (Arduino)?", "id": "Pin Output Tegangan 3.3V Teregulasi." },
  { "en": "Apa Itu Pin Reset (Arduino)?", "id": "Pin Untuk Mereset Mikrokontroler (Aktif Low)." },
  { "en": "Apa Itu Pin IOREF (Arduino)?", "id": "Pin Referensi Tegangan Operasi I/O." },
  { "en": "Apa Itu Sketch (Arduino)?", "id": "Sebutan Untuk Program Arduino." },
  { "en": "Apa Fungsi setup() Di Arduino?", "id": "Kode Inisialisasi (Berjalan Sekali)." },
  { "en": "Apa Fungsi loop() Di Arduino?", "id": "Kode Utama (Berjalan Berulang-ulang)." },
  { "en": "Apa Perintah pinMode()?", "id": "Mengatur Mode Pin (Input Atau Output)." },
  { "en": "Apa Perintah digitalWrite()?", "id": "Menulis Logika High Atau Low (Digital)." },
  { "en": "Apa Perintah digitalRead()?", "id": "Membaca Logika High Atau Low (Digital)." },
  { "en": "Apa Perintah analogRead()?", "id": "Membaca Nilai Analog (0-1023)." },
  { "en": "Apa Perintah analogWrite()?", "id": "Menulis Sinyal PWM (0-255)." },
  { "en": "Apa Perintah delay()?", "id": "Menjeda Eksekusi Program (Milidetik)." },
  { "en": "Apa Kelemahan Perintah delay()?", "id": "Memblokir Total Eksekusi Kode Lain." },
  { "en": "Alternatif Selain Perintah delay()?", "id": "Menggunakan Fungsi millis()." },
  { "en": "Apa Fungsi millis()?", "id": "Mengembalikan Waktu (Milidetik) Sejak Dinyalakan." },
  { "en": "Apa Itu Serial Monitor?", "id": "Jendela Komunikasi Teks Arduino-Komputer." },
  { "en": "Apa Perintah Serial.begin()?", "id": "Memulai Komunikasi Serial (Set Baud Rate)." },
  { "en": "Apa Perintah Serial.println()?", "id": "Mengirim Teks Ke Serial (Baris Baru)." },
  { "en": "Apa Perintah Serial.read()?", "id": "Membaca Data Masuk Dari Serial." },
  { "en": "Apa Itu Library (Pustaka) Arduino?", "id": "Kumpulan Kode Tambahan (Fungsi Khusus)." },
  { "en": "Contoh Library (Pustaka) Arduino?", "id": "Wire.h (I2C), SPI.h (SPI)." },
  { "en": "Apa Itu Proses Verifikasi (Verify)?", "id": "Proses Kompilasi Sketch (Cek Error)." },
  { "en": "Apa Itu Proses Unggah (Upload)?", "id": "Mengirim Kode Terkompilasi Ke Arduino." },
  { "en": "Apa Itu KomentAr (Comment) Kode?", "id": "Teks Penjelasan Yang Diabaikan Kompiler." },
  { "en": "Bagaimana Membuat Komentar Satu Baris?", "id": "Menggunakan Tanda Garis Miring Dua (//)." },
  { "en": "Bagaimana Membuat Komentar Multi-Baris?", "id": "Diawali (/*) Dan Diakhiri (*/)." },
  { "en": "Apa Itu Variabel?", "id": "Nama Tempat Penyimpanan Data Di Memori." },
  { "en": "Apa Tipe Data Integer (int)?", "id": "Tipe Data Bilangan Bulat." },
  { "en": "Apa Tipe Data Float (float)?", "id": "Tipe Data Bilangan Desimal (Koma)." },
  { "en": "Apa Tipe Data Boolean (bool)?", "id": "Tipe Data Logika (True Atau False)." },
  { "en": "Apa Tipe Data Char (char)?", "id": "Tipe Data Satu Karakter Huruf." },
  { "en": "Apa Tipe Data String (String)?", "id": "Tipe Data Kumpulan Karakter (Teks)." },
  { "en": "Apa Itu Array?", "id": "Kumpulan Variabel Tipe Sama." },
  { "en": "Apa Itu Operator Aritmatika?", "id": "Operator Matematika (+, -, *, /)." },
  { "en": "Apa Itu Operator Perbandingan?", "id": "Operator Pembanding (==, !=, <, >)." },
  { "en": "Apa Arti Operator == ?", "id": "Perbandingan Sama Dengan." },
  { "en": "Apa Arti Operator != ?", "id": "Perbandingan Tidak Sama Dengan." },
  { "en": "Apa Itu Operator Logika?", "id": "Operator (AND, OR, NOT)." },
  { "en": "Apa Arti Operator && (AND)?", "id": "Logika DAN (Kedua Sisi Harus True)." },
  { "en": "Apa Arti Operator || (OR)?", "id": "Logika ATAU (Salah Satu Sisi True)." },
  { "en": "Apa Arti Operator ! (NOT)?", "id": "Logika BUKAN (Membalik Nilai Boolean)." },
  { "en": "Apa Itu Pernyataan If (If Statement)?", "id": "Struktur Kontrol Percabangan Kondisional." },
  { "en": "Apa Itu Pernyataan Else?", "id": "Blok Kode Jika Kondisi If Salah." },
  { "en": "Apa Itu Pernyataan Else If?", "id": "Kondisi Tambahan Setelah If Gagal." },
  { "en": "Apa Itu Perulangan For (For Loop)?", "id": "Perulangan Terhitung (Inisialisasi, Kondisi, Iterasi)." },
  { "en": "Apa Itu Perulangan While (While Loop)?", "id": "Perulangan Selama Kondisi Bernilai True." },
  { "en": "Apa Itu Fungsi (Function)?", "id": "Blok Kode Bernama Yang Bisa Dipanggil." },
  { "en": "Apa Keuntungan Fungsi (Function)?", "id": "Kode Terstruktur, Dapat Digunakan Ulang." },
  { "en": "Apa Itu Parameter Fungsi?", "id": "Nilai Input Untuk Fungsi." },
  { "en": "Apa Itu Nilai Kembali (Return Value)?", "id": "Nilai Output Dari Fungsi." },
  { "en": "Apa Itu Void (Dalam Fungsi)?", "id": "Menandakan Fungsi Tidak Mengembalikan Nilai." },
  { "en": "Apa Itu Skup (Scope) Variabel?", "id": "Area Di Mana Variabel Dikenali." },
  { "en": "Apa Itu Variabel Global?", "id": "Variabel Yang Dikenali Di Seluruh Kode." },
  { "en": "Apa Itu Variabel Lokal?", "id": "Variabel Yang Dikenali Di Dalam Fungsi." },
  { "en": "Apa Itu Konstanta (Constant)?", "id": "Variabel Yang Nilainya Tidak Bisa Diubah." },
  { "en": "Bagaimana Mendefinisikan Konstanta?", "id": "Menggunakan Kata Kunci 'const'." },
  { "en": "Apa Itu #define?", "id": "Arahan Preprocessor (Pengganti Teks)." },
  { "en": "Apa Itu #include?", "id": "Arahan Preprocessor (Memasukkan File/Library)." },
  { "en": "Apa Itu Pointer?", "id": "Variabel Penyimpan Alamat Memori." },
  { "en": "Apa Itu Struct (Dalam Pemrograman C)?", "id": "Kumpulan Variabel Berbagai Tipe." },
  { "en": "Apa Itu Operator Aritmatika Bitwise?", "id": "Operator Logika Pada Level Bit (&, |, ^)." },
  { "en": "Apa Itu Operator Bitwise AND (&)?", "id": "Operasi Bit AND (1 Jika Keduanya 1)." },
  { "en": "Apa Itu Operator Bitwise OR (|)?", "id": "Operasi Bit OR (1 Jika Salah Satu 1)." },
  { "en": "Apa Itu Operator Bitwise XOR (^)?", "id": "Operasi Bit XOR (1 Jika Berbeda)." },
  { "en": "Apa Itu Operator Bitwise NOT (~)?", "id": "Operasi Membalik Semua Bit (Invert)." },
  { "en": "Apa Itu Operator Bit Shift Kiri (<<)?", "id": "Menggeser Bit Ke Arah Kiri (Kali 2)." },
  { "en": "Apa Itu Operator Bit Shift Kanan (>>)?", "id": "Menggeser Bit Ke Arah Kanan (Bagi 2)." },
  { "en": "Apa Itu Masking Bit (Bit Masking)?", "id": "Mengisolasi Bit Tertentu Menggunakan Bitwise." },
  { "en": "Apa Itu Pengaturan Bit (Bit Setting)?", "id": "Mengubah Nilai Bit Menjadi 1 (OR)." },
  { "en": "Apa Itu Pembersihan Bit (Bit Clearing)?", "id": "Mengubah Nilai Bit Menjadi 0 (AND NOT)." },
  { "en": "Apa Itu Toggle Bit?", "id": "Membalik Nilai Bit (Menggunakan XOR)." },
  { "en": "Apa Itu MROM (Masked Read Only Memory)?", "id": "ROM Yang Diprogram Saat Produksi." },
  { "en": "Apa Itu PROM (Programmable Read Only Memory)?", "id": "ROM Yang Bisa Diprogram Sekali." },
  { "en": "Apa Itu EPROM (Erasable Programmable ROM)?", "id": "PROM Yang Dihapus Sinar Ultraviolet." },
  { "en": "Bagaimana Ciri IC EPROM?", "id": "Ada Jendela Kaca Kuarsa Di Atasnya." },
  { "en": "Apa Itu UV Eraser (Penghapus UV)?", "id": "Alat Penghapus Data IC EPROM." },
  { "en": "Apa Itu Memori Non-Volatile (Tidak Menguap)?", "id": "Data Tersimpan Meski Listrik Mati." },
  { "en": "Contoh Memori Non-Volatile?", "id": "ROM, Flash Memory, EEPROM." },
  { "en": "Apa Itu Memori Volatile (Menguap)?", "id": "Data Hilang Saat Listrik Mati." },
  { "en": "Contoh Memori Volatile?", "id": "SRAM Dan DRAM (RAM)." },
  { "en": "Apa Itu Arsitektur Harvard?", "id": "Memisahkan Memori Program Dan Data." },
  { "en": "Apa Itu Arsitektur Von Neumann?", "id": "Menyatukan Memori Program Dan Data." },
  { "en": "Mikrokontroler Arduino Menggunakan Arsitektur Apa?", "id": "Arsitektur Harvard (Modifikasi)." },
  { "en": "Komputer PC Menggunakan Arsitektur Apa?", "id": "Arsitektur Von Neumann." },
  { "en": "Apa Itu CISC (Complex Instruction Set Computer)?", "id": "Komputer Set Instruksi Kompleks." },
  { "en": "Apa Itu RISC (Reduced Instruction Set Computer)?", "id": "Komputer Set Instruksi Sederhana." },
  { "en": "Apa Itu Pipeline (Dalam CPU)?", "id": "Teknik Eksekusi Instruksi Paralel Bertahap." },
  { "en": "Apa Itu Cache (Memori)?", "id": "Memori SRAM Super Cepat Dekat CPU." },
  { "en": "Apa Fungsi Cache (Memori)?", "id": "Mempercepat Akses Data Yang Sering Digunakan." },
  { "en": "Apa Itu DMA (Direct Memory Access)?", "id": "Fitur Akses Memori Tanpa Melalui CPU." },
  { "en": "Apa Keuntungan DMA (Direct Memory Access)?", "id": "Mengurangi Beban Kerja CPU (Transfer Data)." },
  { "en": "Apa Itu Register CPU?", "id": "Lokasi Penyimpanan Internal CPU Tercepat." },
  { "en": "Apa Itu Program Counter (PC)?", "id": "Register Penyimpan Alamat Instruksi Berikutnya." },
  { "en": "Apa Itu Stack (Tumpukan)?", "id": "Area Memori LIFO (Last In First Out)." },
  { "en": "Apa Itu Stack Pointer?", "id": "Register Penunjuk Alamat Puncak Stack." },
  { "en": "Apa Itu Heap (Memori)?", "id": "Area Memori Untuk Alokasi Dinamis." },
  { "en": "Apa Itu Overflow (Stack)?", "id": "Kondisi Stack Melebihi Kapasitas Memori." },
  { "en": "Apa Itu Sensor Level Air?", "id": "Sensor Pengukur Ketinggian Permukaan Cairan." },
  { "en": "Apa Itu Sensor pH?", "id": "Sensor Pengukur Tingkat Keasaman Basa Cairan." },
  { "en": "Apa Itu Sensor ORP (Oxidation Reduction Potential)?", "id": "Sensor Pengukur Potensi Oksidasi Reduksi." },
  { "en": "Apa Itu Sensor Konduktivitas (Conductivity)?", "id": "Sensor Pengukur Kemampuan Cairan Menghantar Listrik." },
  { "en": "Apa Itu Sensor Kekeruhan (Turbidity)?", "id": "Sensor Pengukur Kekeruhan Cairan (Cahaya)." },
  { "en": "Apa Itu Sensor Tekanan (Pressure Sensor)?", "id": "Sensor Pengukur Kekuatan Tekanan (Gas/Cairan)." },
  { "en": "Contoh Sensor Tekanan?", "id": "BMP180, BMP280 (Tekanan Udara)." },
  { "en": "Apa Itu Sensor Kecepatan Angin (Anemometer)?", "id": "Sensor Pengukur Laju Aliran Udara." },
  { "en": "Apa Itu Sensor Arah Angin (Wind Vane)?", "id": "Sensor Penentu Arah Datangnya Angin." },
  { "en": "Apa Itu Sensor Curah Hujan (Rain Gauge)?", "id": "Sensor Pengukur Jumlah Presipitasi Hujan." },
  { "en": "Apa Itu Sensor Getar (Vibration Sensor)?", "id": "Sensor Pendeteksi Guncangan Atau Getaran." },
  { "en": "Contoh Sensor Getar Sederhana?", "id": "Sensor Piezoelektrik Atau Switch Pegas." },
  { "en": "Apa Itu Sensor Suara (Sound Sensor)?", "id": "Modul Sensor Berbasis Mikrofon." },
  { "en": "Fungsi Modul Sensor Suara?", "id": "Mendeteksi Intensitas Suara (Keras/Pelan)." },
  { "en": "Apa Itu Sensor Api (Flame Sensor)?", "id": "Sensor Pendeteksi Spektrum Cahaya Api." },
  { "en": "Apa Itu Sensor UV (Ultraviolet)?", "id": "Sensor Pengukur Intensitas Radiasi Sinar UV." },
  { "en": "Apa Itu DIP (Dual In-line Package) Switch?", "id": "Saklar Kecil Berkelompok (Konfigurasi)." },
  { "en": "Apa Itu Potensiometer Logaritmik (Log)?", "id": "Potensiometer Resistansi Berubah Secara Logaritmik." },
  { "en": "Di Mana Potensiometer Logaritmik Digunakan?", "id": "Kontrol Volume Audio." },
  { "en": "Apa Itu Potensiometer Linear (Lin)?", "id": "Potensiometer Resistansi Berubah Secara Linear." },
  { "en": "Di Mana Potensiometer Linear Digunakan?", "id": "Kontrol Sensor, Kontrol Tegangan." },
  { "en": "Apa Itu Rheostat (Reostat)?", "id": "Resistor Variabel Dua Terminal (Kontrol Arus)." },
  { "en": "Apa Beda Rheostat Dan Potensiometer?", "id": "Rheostat (Kontrol Arus), Potensio (Kontrol Tegangan)." },
  { "en": "Apa Itu Voltage Follower (Buffer Op-Amp)?", "id": "Rangkaian Op-Amp Penguatan Satu." },
  { "en": "Apa Fungsi Voltage Follower?", "id": "Penyangga Impedansi (Impedansi Input Tinggi)." },
  { "en": "Apa Itu Op-Amp Komparator?", "id": "Rangkaian Pembanding Dua Tegangan Input." },
  { "en": "Apa Itu Histeresis (Hysteresis) Komparator?", "id": "Jarak Antara Level Pemicu Atas Bawah." },
  { "en": "Apa Itu Schmitt Trigger?", "id": "Komparator Dengan Histeresis (Umpan Balik Positif)." },
  { "en": "Fungsi Rangkaian Schmitt Trigger?", "id": "Mengubah Sinyal Analog Menjadi Sinyal Digital." },
  { "en": "Apa Itu Rangkaian Clamping Dioda?", "id": "Rangkaian Pembatas Tegangan (Melindungi Input)." },
  { "en": "Apa Itu Rangkaian Crowbar (Linggis)?", "id": "Rangkaian Proteksi Tegangan Berlebih (Konslet)." },
  { "en": "Komponen Utama Rangkaian Crowbar?", "id": "SCR (Silicon Controlled Rectifier) Dan Zener." },
  { "en": "Apa Itu MOV (Metal Oxide Varistor)?", "id": "Resistor Pelindung Lonjakan Tegangan Tinggi." },
  { "en": "Di Mana MOV (Metal Oxide Varistor) Biasa Dipasang?", "id": "Jalur Listrik AC (Stop Kontak)." },
  { "en": "Apa Itu GDT (Gas Discharge Tube)?", "id": "Tabung Gas Pelindung Lonjakan Tegangan." },
  { "en": "Apa Itu Surge Arrester (Penangkal Petir)?", "id": "Pelindung Utama Jaringan Listrik." },
  { "en": "Apa Itu Logic Probe (Probe Logika)?", "id": "Alat Tes Sederhana Logika Digital (High/Low)." },
  { "en": "Apa Itu Current Probe (Probe Arus)?", "id": "Probe Khusus Pengukur Arus (Osiloskop)." },
  { "en": "Apa Itu Frekuensi Counter (Penghitung Frekuensi)?", "id": "Alat Ukur Frekuensi Sinyal Presisi." },
  { "en": "Apa Itu Impedansi Input Multimeter?", "id": "Hambatan Internal Alat Ukur (Sangat Tinggi)." },
  { "en": "Kenapa Impedansi Input Multimeter Tinggi?", "id": "Agar Tidak Membebani Rangkaian Diukur." },
  { "en": "Apa Itu Efek Pembebanan (Loading Effect)?", "id": "Alat Ukur Mengganggu Rangkaian Diukur." },
  { "en": "Apa Itu Pengukuran Empat Titik (Four-Wire)?", "id": "Metode Pengukuran Resistansi Sangat Rendah." },
  { "en": "Apa Itu Fiducial Marker (PCB)?", "id": "Tanda Optik Bantuan Mesin Perakitan SMD." },
  { "en": "Apa Itu Annular Ring (PCB)?", "id": "Area Tembaga Sekeliling Lubang Bor." },
  { "en": "Kenapa Annular Ring (PCB) Penting?", "id": "Menjamin Koneksi Solderan Yang Baik." },
  { "en": "Apa Itu DRC (Design Rule Check)?", "id": "Pengecekan Otomatis Desain PCB." },
  { "en": "Apa Itu Electrical Rules Check (ERC)?", "id": "Pengecekan Koneksi Elektrikal Skematik." },
  { "en": "Apa Itu Netlist (Daftar Jaring)?", "id": "Daftar Koneksi Antar Komponen." },
  { "en": "Apa Itu Bill Of Materials (BOM)?", "id": "Daftar Semua Komponen Rangkaian." },
  { "en": "Apa Itu Pick And Place Machine?", "id": "Mesin Pemasang Komponen SMD Otomatis." },
  { "en": "Apa Itu Stensil PCB (PCB Stencil)?", "id": "Cetakan Aplikasi Pasta Solder SMD." },
  { "en": "Apa Itu Komunikasi Simplex?", "id": "Komunikasi Satu Arah (Contoh: Radio FM)." },
  { "en": "Apa Itu Komunikasi Half-Duplex?", "id": "Komunikasi Dua Arah Bergantian (Walkie-Talkie)." },
  { "en": "Apa Itu Komunikasi Full-Duplex?", "id": "Komunikasi Dua Arah Bersamaan (Telepon)." },
  { "en": "Apa Itu Protokol (Protokol Komunikasi)?", "id": "Aturan Tata Cara Komunikasi Data." },
  { "en": "Apa Itu Topologi Jaringan?", "id": "Struktur Tata Letak Fisik Jaringan." },
  { "en": "Apa Itu Topologi Bus?", "id": "Semua Perangkat Terhubung Ke Satu Kabel." },
  { "en": "Apa Itu Topologi Star (Bintang)?", "id": "Semua Perangkat Terhubung Ke Hub Pusat." },
  { "en": "Apa Itu Topologi Ring (Cincin)?", "id": "Perangkat Terhubung Membentuk Lingkaran." },
  { "en": "Apa Itu Topologi Mesh?", "id": "Semua Perangkat Saling Terhubung (Redundan)." },
  { "en": "Apa Itu Ethernet?", "id": "Standar Jaringan Kabel Lokal (LAN)." },
  { "en": "Apa Itu MAC (Media Access Control) Address?", "id": "Alamat Fisik Unik Perangkat Jaringan." },
  { "en": "Apa Itu IP (Internet Protocol) Address?", "id": "Alamat Logika Perangkat Di Jaringan." },
  { "en": "Apa Itu TCP (Transmission Control Protocol)?", "id": "Protokol Komunikasi Jaringan Handal (Koneksi)." },
  { "en": "Apa Itu UDP (User Datagram Protocol)?", "id": "Protokol Komunikasi Jaringan Cepat (Tanpa Koneksi)." },
  { "en": "Apa Itu HTTP (Hypertext Transfer Protocol)?", "id": "Protokol Transfer Halaman Web." },
  { "en": "Apa Itu HTTPS (Hypertext Transfer Protocol Secure)?", "id": "Protokol HTTP Dengan Enkripsi SSL/TLS." },
  { "en": "Apa Itu FTP (File Transfer Protocol)?", "id": "Protokol Transfer File Di Jaringan." },
  { "en": "Apa Itu SMTP (Simple Mail Transfer Protocol)?", "id": "Protokol Pengiriman Email." },
  { "en": "Apa Itu POP3 (Post Office Protocol 3)?", "id": "Protokol Pengambilan Email (Mengunduh)." },
  { "en": "Apa Itu IMAP (Internet Message Access Protocol)?", "id": "Protokol Pengambilan Email (Sinkronisasi)." },
  { "en": "Apa Itu DNS (Domain Name System)?", "id": "Sistem Penerjemah Nama Domain Ke IP." },
  { "en": "Apa Itu DHCP (Dynamic Host Configuration Protocol)?", "id": "Protokol Pemberi Alamat IP Otomatis." },
  { "en": "Apa Itu MQTT (Message Queuing Telemetry Transport)?", "id": "Protokol Komunikasi Ringan Untuk IoT." },
  { "en": "Apa Itu Model OSI (Open Systems Interconnection)?", "id": "Model Konseptual Jaringan Tujuh Lapis." },
  { "en": "Apa Itu Lapis Fisik (Physical Layer) OSI?", "id": "Lapis 1 (Kabel, Sinyal Listrik)." },
  { "en": "Apa Itu Lapis Tautan Data (Data Link) OSI?", "id": "Lapis 2 (MAC Address, Frame)." },
  { "en": "Apa Itu Lapis Jaringan (Network Layer) OSI?", "id": "Lapis 3 (IP Address, Routing)." },
  { "en": "Apa Itu Lapis Transport (Transport Layer) OSI?", "id": "Lapis 4 (TCP, UDP, Port)." },
  { "en": "Apa Itu Lapis Sesi (Session Layer) OSI?", "id": "Lapis 5 (Manajemen Koneksi Sesi)." },
  { "en": "Apa Itu Lapis Presentasi (Presentation) OSI?", "id": "Lapis 6 (Enkripsi, Format Data)." },
  { "en": "Apa Itu Lapis Aplikasi (Application) OSI?", "id": "Lapis 7 (HTTP, FTP, DNS)." },
  { "en": "Apa Itu Router?", "id": "Perangkat Jaringan Penghubung Antar Jaringan." },
  { "en": "Apa Itu Switch (Jaringan)?", "id": "Perangkat Jaringan Penghubung Perangkat LAN." },
  { "en": "Apa Itu Hub (Jaringan)?", "id": "Perangkat Jaringan (Lama, Kurang Cerdas)." },
  { "en": "Apa Itu Modem (Modulator-Demodulator)?", "id": "Perangkat Pengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu Access Point (Titik Akses)?", "id": "Perangkat Jaringan Pemancar Sinyal Wi-Fi." },
  { "en": "Apa Itu Firewall (Tembok Api)?", "id": "Sistem Keamanan Penyaring Lalu Lintas Jaringan." },
  { "en": "Apa Itu LED (Light Emitting Diode) Bicolor?", "id": "Satu LED Dua Warna (Dua Kaki)." },
  { "en": "Apa Itu LED (Light Emitting Diode) Tricolor?", "id": "Satu LED Dua Warna (Tiga Kaki)." },
  { "en": "Apa Itu LED (Light Emitting Diode) Inframerah (IR)?", "id": "LED Pemancar Cahaya Inframerah (Tak Terlihat)." },
  { "en": "Apa Itu Penerima Inframerah (IR Receiver)?", "id": "Sensor Penerima Sinyal Inframerah (Remote)." },
  { "en": "Contoh Penerima Inframerah (IR Receiver)?", "id": "TSOP4838." },
  { "en": "Apa Itu LED (Light Emitting Diode) Ultraviolet (UV)?", "id": "LED Pemancar Cahaya Ultraviolet." },
  { "en": "Apa Itu Laser Diode Module?", "id": "Modul Penghasil Sinar Laser Merah." },
  { "en": "Apa Itu Keyboard Matriks (Matrix Keypad)?", "id": "Papan Tombol (4x4) Hemat Pin." },
  { "en": "Bagaimana Cara Kerja Keyboard Matriks?", "id": "Scanning Kolom Dan Baris." },
  { "en": "Apa Itu Joystick Module?", "id": "Modul Sensor Stik Analog (Dua Potensio)." },
  { "en": "Apa Itu Thumbwheel Switch?", "id": "Saklar Putar Pemilih Angka Desimal." },
  { "en": "Apa Itu Rotary Switch?", "id": "Saklar Putar Pemilih Banyak Posisi." },
  { "en": "Apa Itu Slide Switch (Saklar Geser)?", "id": "Saklar On-Off Berbasis Geser." },
  { "en": "Apa Itu Toggle Switch (Saklar Togol)?", "id": "Saklar Tuas (Naik-Turun Atau Kiri-Kanan)." },
  { "en": "Apa Itu Push Button (Tombol Tekan)?", "id": "Saklar Aktif Saat Ditekan." },
  { "en": "Apa Itu Push Button Momentary?", "id": "Aktif Hanya Saat Ditekan (Umum)." },
  { "en": "Apa Itu Push Button Latching (Mengunci)?", "id": "Ditekan (On), Ditekan Lagi (Off)." },
  { "en": "Apa Itu Limit Switch (Saklar Batas)?", "id": "Saklar Mekanis Pendeteksi Batas Gerakan." },
  { "en": "Di Mana Limit Switch Digunakan?", "id": "Mesin CNC, Printer 3D, Robotika." },
  { "en": "Apa Itu Sensor Kemiringan (Tilt Sensor)?", "id": "Sensor Pendeteksi Perubahan Sudut Kemiringan." },
  { "en": "Apa Itu Mercury Switch (Saklar Raksa)?", "id": "Saklar Kemiringan Berbasis Air Raksa (Berbahaya)." },
  { "en": "Apa Itu Ball Switch (Saklar Bola)?", "id": "Saklar Kemiringan Berbasis Bola Logam." },
  { "en": "Apa Itu Sensor Suhu LM35?", "id": "Sensor Suhu Analog Output Tegangan Linear." },
  { "en": "Apa Kelebihan Sensor Suhu LM35?", "id": "Output Linear (10mV Per Derajat Celcius)." },
  { "en": "Apa Itu Sensor Suhu DS18B20?", "id": "Sensor Suhu Digital (Protokol One-Wire)." },
  { "en": "Apa Itu Protokol One-Wire (1-Wire)?", "id": "Protokol Komunikasi Satu Kabel Data." },
  { "en": "Apa Itu Sensor DHT11?", "id": "Sensor Suhu Dan Kelembaban Digital (Murah)." },
  { "en": "Apa Itu Sensor DHT22 (AM2302)?", "id": "Sensor Suhu Kelembaban (Lebih Akurat)." },
  { "en": "Apa Itu Sensor BME280?", "id": "Sensor Suhu, Kelembaban, Dan Tekanan Udara." },
  { "en": "Apa Itu Sensor BMP280?", "id": "Sensor Suhu Dan Tekanan Udara." },
  { "en": "Apa Itu Kapasitor Variabel (Varco)?", "id": "Kapasitor Yang Nilainya Dapat Diatur (Tuner)." },
  { "en": "Apa Itu Dioda Varactor (Varicap)?", "id": "Dioda Berfungsi Kapasitor Terkendali Tegangan." },
  { "en": "Apa Itu Resistor Wirewound (Kawat Gulung)?", "id": "Resistor Berdaya Tinggi (Disipasi Panas)." },
  { "en": "Apa Itu Resistor Komposisi Karbon?", "id": "Tipe Resistor Lama (Kurang Presisi)." },
  { "en": "Apa Itu Resistor Film Karbon?", "id": "Tipe Resistor Umum Murah (Toleransi 5%)." },
  { "en": "Apa Itu Resistor Film Logam (Metal Film)?", "id": "Resistor Presisi (Toleransi 1% Atau Kurang)." },
  { "en": "Apa Itu Toleransi Resistor?", "id": "Batas Penyimpangan Nilai Resistansi (Akurasi)." },
  { "en": "Apa Itu Koefisien Suhu Resistor?", "id": "Seberapa Besar Nilai Berubah Karena Suhu." },
  { "en": "Apa Itu Noise Resistor (Johnson-Nyquist)?", "id": "Noise Termal Yang Dihasilkan Resistor." },
  { "en": "Apa Itu Kapasitor Kertas (Paper Capacitor)?", "id": "Tipe Kapasitor Lama (Dielektrik Kertas)." },
  { "en": "Apa Itu Kapasitor Mika (Mica Capacitor)?", "id": "Kapasitor Frekuensi Tinggi Stabil." },
  { "en": "Apa Itu Kapasitor Polystyrene (Styroflex)?", "id": "Kapasitor Presisi Untuk Filter." },
  { "en": "Apa Itu Kapasitor Polipropilena (MKP/MKT)?", "id": "Kapasitor Audio, Filter AC." },
  { "en": "Apa Itu Kapasitor Keramik Multilayer (MLCC)?", "id": "Kapasitor SMD Paling Umum (Kecil)." },
  { "en": "Apa Itu Kapasitor Super (Supercapacitor)?", "id": "Kapasitor Nilai Sangat Besar (Penyimpan Energi)." },
  { "en": "Apa Beda Kapasitor Super Dan Baterai?", "id": "Supercap (Isi Cepat), Baterai (Energi Padat)." },
  { "en": "Apa Itu ESR (Equivalent Series Resistance) Kapasitor?", "id": "Hambatan Seri Internal Kapasitor." },
  { "en": "Kenapa ESR (Equivalent Series Resistance) Penting?", "id": "ESR Rendah Baik Untuk Power Supply." },
  { "en": "Apa Itu ESL (Equivalent Series Inductance) Kapasitor?", "id": "Induktansi Seri Internal Kapasitor." },
  { "en": "Apa Itu Arus Bocor (Leakage) Kapasitor?", "id": "Arus DC Kecil Melewati Dielektrik." },
  { "en": "Apa Itu Tegangan Kerja Kapasitor?", "id": "Batas Tegangan Maksimal Yang Diizinkan." },
  { "en": "Apa Itu Induktor Inti Udara (Air Core)?", "id": "Induktor Tanpa Inti Material (RF)." },
  { "en": "Apa Itu Induktor Inti Besi (Iron Core)?", "id": "Induktor Frekuensi Rendah (Daya)." },
  { "en": "Apa Itu Induktor Inti Ferit (Ferrite Core)?", "id": "Induktor Frekuensi Tinggi (Filter Noise)." },
  { "en": "Apa Itu Toroid?", "id": "Induktor Atau Trafo Berbentuk Donat." },
  { "en": "Apa Keuntungan Induktor Toroid?", "id": "Medan Magnet Terkandung, Efisiensi Tinggi." },
  { "en": "Apa Itu Choke (Induktor)?", "id": "Induktor Pencekik Sinyal AC Frekuensi Tinggi." },
  { "en": "Apa Itu RF Choke?", "id": "Choke Untuk Memblokir Sinyal Frekuensi Radio." },
  { "en": "Apa Itu DC Choke?", "id": "Choke Untuk Menghaluskan Arus DC." },
  { "en": "Apa Itu Faktor Q Induktor?", "id": "Ukuran Kualitas Induktor (Resistansi Rendah)." },
  { "en": "Apa Itu Frekuensi Resonansi Diri (SRF) Induktor?", "id": "Frekuensi Saat Induktor Menjadi Kapasitor." },
  { "en": "Apa Itu Trafo CT (Center Tap)?", "id": "Trafo Dengan Sadapan Tengah Di Sekunder." },
  { "en": "Fungsi Trafo CT (Center Tap)?", "id": "Membuat Catu Daya Simetris (Positif/Negatif)." },
  { "en": "Apa Itu Trafo Isolasi (Isolation Transformer)?", "id": "Trafo Dengan Rasio Lilitan 1:1." },
  { "en": "Fungsi Trafo Isolasi?", "id": "Keamanan (Memutus Koneksi Ground Galvanik)." },
  { "en": "Apa Itu Autotransformer (Autotrafo)?", "id": "Trafo Dengan Satu Lilitan (Primer Sekunder)." },
  { "en": "Apa Itu Variac?", "id": "Autotrafo Yang Tegangan Outputnya Bisa Diatur." },
  { "en": "Apa Itu Arus Inrush (Inrush Current)?", "id": "Lonjakan Arus Awal Saat Perangkat Dinyalakan." },
  { "en": "Apa Itu Soft Starter?", "id": "Rangkaian Pembatas Arus Inrush." },
  { "en": "Apa Itu Crowding (Transistor)?", "id": "Efek Penumpukan Arus Di Emitor." },
  { "en": "Apa Itu Saturasi (Transistor)?", "id": "Kondisi Transistor BJT Menyala Penuh (Saklar)." },
  { "en": "Apa Itu Cut-Off (Transistor)?", "id": "Kondisi Transistor BJT Mati Penuh (Saklar)." },
  { "en": "Apa Itu Daerah Aktif (Transistor)?", "id": "Kondisi Transistor BJT Sebagai Penguat." },
  { "en": "Apa Itu Daerah Triode (FET)?", "id": "Kondisi FET Berfungsi Seperti Resistor." },
  { "en": "Apa Itu Daerah Saturasi (FET)?", "id": "Kondisi FET Berfungsi Sebagai Penguat." },
  { "en": "Apa Itu Cut-Off (FET)?", "id": "Kondisi FET Mati (Tidak Ada Arus)." },
  { "en": "Apa Itu Tegangan Pinch-Off (VP)?", "id": "Tegangan Yang Memulai Daerah Saturasi FET." },
  { "en": "Apa Itu Tegangan Threshold (VT) MOSFET?", "id": "Tegangan Gerbang Minimal Untuk Menyala." },
  { "en": "Apa Itu MOSFET Tipe Enhancement (Peningkatan)?", "id": "MOSFET Yang Normalnya Mati (Perlu VT)." },
  { "en": "Apa Itu MOSFET Tipe Depletion (Penyusutan)?", "id": "MOSFET Yang Normalnya Menyala (Tanpa VT)." },
  { "en": "Apa Itu Saluran-N (N-Channel) MOSFET?", "id": "MOSFET Menggunakan Elektron Sebagai Pembawa." },
  { "en": "Apa Itu Saluran-P (P-Channel) MOSFET?", "id": "MOSFET Menggunakan Hole Sebagai Pembawa." },
  { "en": "Bagaimana Mengaktifkan N-Channel MOSFET?", "id": "Memberi Tegangan Gerbang Positif (VGS+)." },
  { "en": "Bagaimana Mengaktifkan P-Channel MOSFET?", "id": "Memberi Tegangan Gerbang Negatif (VGS-)." },
  { "en": "Apa Itu CMOS (Complementary Metal-Oxide-Semiconductor)?", "id": "Gabungan N-Channel Dan P-Channel MOSFET." },
  { "en": "Apa Keuntungan Logika CMOS?", "id": "Konsumsi Daya Statis Sangat Rendah." },
  { "en": "Apa Itu Latch-Up (Pada CMOS)?", "id": "Kondisi Hubung Singkat Internal CMOS." },
  { "en": "Apa Penyebab Latch-Up?", "id": "Tegangan Berlebih Atau ESD." },
  { "en": "Apa Itu IC Logika Seri 4000?", "id": "Keluarga IC Logika Berbasis CMOS." },
  { "en": "Apa Itu IC Logika Seri 7400?", "id": "Keluarga IC Logika Berbasis TTL." },
  { "en": "Apa Itu Varian 74HC (High-speed CMOS)?", "id": "Seri 7400 Cepat Berbasis CMOS." },
  { "en": "Apa Itu Varian 74HCT (High-speed CMOS TTL)?", "id": "Seri 74HC Kompatibel Input TTL." },
  { "en": "Apa Itu IC 7400?", "id": "IC Berisi Empat Gerbang NAND 2-Input." },
  { "en": "Apa Itu IC 7402?", "id": "IC Berisi Empat Gerbang NOR 2-Input." },
  { "en": "Apa Itu IC 7404?", "id": "IC Berisi Enam Gerbang NOT (Inverter)." },
  { "en": "Apa Itu IC 7408?", "id": "IC Berisi Empat Gerbang AND 2-Input." },
  { "en": "Apa Itu IC 7432?", "id": "IC Berisi Empat Gerbang OR 2-Input." },
  { "en": "Apa Itu IC 7486?", "id": "IC Berisi Empat Gerbang XOR 2-Input." },
  { "en": "Apa Itu IC 7447?", "id": "IC Decoder BCD Ke Seven Segment." },
  { "en": "Apa Itu IC 7474?", "id": "IC Berisi Dua Flip-Flop Tipe D." },
  { "en": "Apa Itu IC 7476?", "id": "IC Berisi Dua Flip-Flop Tipe JK." },
  { "en": "Apa Itu IC 7490?", "id": "IC Pencacah Dekade (Decade Counter)." },
  { "en": "Apa Itu IC 74138?", "id": "IC Decoder 3-Ke-8 Baris." },
  { "en": "Apa Itu IC 74151?", "id": "IC Multiplexer (MUX) 8-Ke-1." },
  { "en": "Apa Itu IC 74165?", "id": "IC Register Geser PISO 8-Bit." },
  { "en": "Apa Itu IC 74595?", "id": "IC Register Geser SIPO 8-Bit." },
  { "en": "Di Mana IC 74595 Sering Digunakan?", "id": "Menambah Pin Output Mikrokontroler." },
  { "en": "Apa Itu IC 555 Mode Astable?", "id": "Mode Osilator Penghasil Gelombang Kotak." },
  { "en": "Apa Itu IC 555 Mode Monostable?", "id": "Mode Penghasil Satu Pulsa (Timer)." },
  { "en": "Apa Fungsi Pin 2 (Trigger) IC 555?", "id": "Memulai Siklus Pewaktu (Input Pemicu)." },
  { "en": "Apa Fungsi Pin 3 (Output) IC 555?", "id": "Pin Keluaran Sinyal (Kotak/Pulsa)." },
  { "en": "Apa Fungsi Pin 4 (Reset) IC 555?", "id": "Mereset IC (Aktif Low)." },
  { "en": "Apa Fungsi Pin 5 (Control Voltage) IC 555?", "id": "Mengatur Tegangan Ambang Referensi." },
  { "en": "Apa Fungsi Pin 6 (Threshold) IC 555?", "id": "Input Ambang (Mengakhiri Siklus)." },
  { "en": "Apa Fungsi Pin 7 (Discharge) IC 555?", "id": "Membuang Muatan Kapasitor Eksternal." },
  { "en": "Apa Fungsi Pin 8 (VCC) IC 555?", "id": "Pin Catu Daya Positif." },
  { "en": "Apa Fungsi Pin 1 (GND) IC 555?", "id": "Pin Catu Daya Negatif (Ground)." },
  { "en": "Apa Itu IC 556?", "id": "Versi Ganda (Dual) IC 555." },
  { "en": "Apa Itu IC LM386?", "id": "IC Penguat Audio Daya Rendah." },
  { "en": "Apa Itu IC LM358?", "id": "IC Op-Amp Ganda (Dual)." },
  { "en": "Apa Itu IC LM324?", "id": "IC Op-Amp Quad (Empat)." },
  { "en": "Apa Itu IC LM741?", "id": "IC Op-Amp Legendaris (Tunggal)." },
  { "en": "Apa Itu IC PC817?", "id": "IC Optocoupler (Isolator) Umum." },
  { "en": "Apa Itu IC ULN2003?", "id": "IC Array Transistor Darlington." },
  { "en": "Fungsi IC ULN2003?", "id": "Driver Beban Arus Tinggi (Motor, Relay)." },
  { "en": "Apa Itu IC TP4056?", "id": "IC Charger Baterai Lithium-Ion." },
  { "en": "Apa Itu IC MAX232?", "id": "IC Konverter Level RS-232 Ke TTL." },
  { "en": "Apa Itu IC ATmega328P?", "id": "Mikrokontroler Utama Arduino Uno." },
  { "en": "Apa Itu IC ESP8266EX?", "id": "Mikrokontroler Dengan Wi-Fi Terintegrasi." },
  { "en": "Apa Itu IC ESP32-WROOM-32?", "id": "Modul Mikrokontroler Wi-Fi Dan Bluetooth." },
  { "en": "Apa Itu IC CH340G?", "id": "IC USB Ke Serial (Murah)." },
  { "en": "Apa Itu IC CP2102?", "id": "IC USB Ke Serial (Stabil)." },
  { "en": "Apa Itu IC FT232RL?", "id": "IC USB Ke Serial (Kualitas Tinggi)." },
  { "en": "Apa Itu Dioda 1N4001?", "id": "Dioda Penyearah 1 Ampere 50 Volt." },
  { "en": "Apa Itu Dioda 1N4007?", "id": "Dioda Penyearah 1 Ampere 1000 Volt." },
  { "en": "Apa Itu Dioda 1N4148?", "id": "Dioda Sinyal Kecil Kecepatan Tinggi." },
  { "en": "Apa Itu Dioda 1N5408?", "id": "Dioda Penyearah 3 Ampere 1000 Volt." },
  { "en": "Apa Itu Dioda 1N5819?", "id": "Dioda Schottky 1 Ampere 40 Volt." },
  { "en": "Apa Itu Dioda Zener 1N4733A?", "id": "Dioda Zener 5.1 Volt 1 Watt." },
  { "en": "Apa Itu Transistor BC547?", "id": "Transistor NPN Sinyal Kecil Umum." },
  { "en": "Apa Itu Transistor BC557?", "id": "Transistor PNP Sinyal Kecil Umum." },
  { "en": "Apa Itu Transistor 2N2222?", "id": "Transistor NPN Sinyal Kecil (Logam)." },
  { "en": "Apa Itu Transistor 2N3904?", "id": "Transistor NPN Sinyal Kecil (Plastik)." },
  { "en": "Apa Itu Transistor 2N3906?", "id": "Transistor PNP Sinyal Kecil (Plastik)." },
  { "en": "Apa Itu Transistor TIP31C?", "id": "Transistor NPN Daya Menengah." },
  { "en": "Apa Itu Transistor TIP32C?", "id": "Transistor PNP Daya Menengah." },
  { "en": "Apa Itu Transistor TIP120?", "id": "Transistor NPN Darlington Daya." },
  { "en": "Apa Itu Transistor TIP122?", "id": "Transistor NPN Darlington (Rating Lebih Tinggi)." },
  { "en": "Apa Itu Transistor 2N3055?", "id": "Transistor NPN Daya Tinggi (Paket TO-3)." },
  { "en": "Apa Itu MOSFET IRF540N?", "id": "N-Channel Power MOSFET Umum." },
  { "en": "Apa Itu MOSFET IRF9540N?", "id": "P-Channel Power MOSFET Umum." },
  { "en": "Apa Itu MOSFET IRFZ44N?", "id": "N-Channel MOSFET (Tegangan Gate Rendah)." },
  { "en": "Apa Itu MOSFET IRLZ44N?", "id": "N-Channel MOSFET (Logic Level Gate)." },
  { "en": "Apa Itu Logic Level Gate?", "id": "MOSFET Yang Aktif Penuh Di 5V." },
  { "en": "Apa Itu IC Regulator 7805?", "id": "Regulator Tegangan Positif 5 Volt." },
  { "en": "Apa Arti '05' Pada 7805?", "id": "Menandakan Output Positif 5 Volt." },
  { "en": "Apa Itu IC Regulator 7809?", "id": "Regulator Tegangan Positif 9 Volt." },
  { "en": "Apa Itu IC Regulator 7812?", "id": "Regulator Tegangan Positif 12 Volt." },
  { "en": "Apa Itu IC Regulator 7905?", "id": "Regulator Tegangan Negatif 5 Volt." },
  { "en": "Apa Arti '79' Pada 7905?", "id": "Menandakan Regulator Tegangan Negatif." },
  { "en": "Apa Itu IC Regulator LM317?", "id": "Regulator Tegangan Positif Yang Dapat Diatur." },
  { "en": "Bagaimana Cara Mengatur Tegangan LM317?", "id": "Menggunakan Dua Resistor Eksternal." },
  { "en": "Apa Itu IC Regulator LM337?", "id": "Regulator Tegangan Negatif Yang Dapat Diatur." },
  { "en": "Apa Itu IC Regulator LM1117-3.3?", "id": "Regulator LDO Tegangan 3.3 Volt." },
  { "en": "Apa Itu IC Regulator AMS1117-3.3?", "id": "Regulator LDO 3.3V (Versi Umum)." },
  { "en": "Apa Itu Nomor Seri Resistor?", "id": "Standar Nilai (Contoh: E12, E24)." },
  { "en": "Apa Itu Seri Resistor E12?", "id": "Memiliki 12 Nilai Dasar Per Dekade." },
  { "en": "Apa Itu Seri Resistor E24?", "id": "Memiliki 24 Nilai Dasar Per Dekade." },
  { "en": "Apa Itu Nilai Preferensi (Preferred Value)?", "id": "Nilai Standar Komponen Yang Diproduksi." },
  { "en": "Apa Itu Kawat Pelangi (Ribbon Cable)?", "id": "Kabel Gepeng Warna-Warni." },
  { "en": "Apa Itu Kabel UTP (Unshielded Twisted Pair)?", "id": "Kabel Jaringan Ethernet Tanpa Pelindung." },
  { "en": "Apa Itu Kabel STP (Shielded Twisted Pair)?", "id": "Kabel Jaringan Ethernet Dengan Pelindung." },
  { "en": "Apa Itu Ferit Bead (Ferrite Clamp)?", "id": "Bonggolan Di Ujung Kabel (Peredam Noise)." },
  { "en": "Apa Itu Tespen (Test Pen)?", "id": "Obeng Penguji Adanya Tegangan Listrik AC." },
  { "en": "Bagaimana Tespen (Test Pen) Menyala?", "id": "Neon Indikator Menyala Jika Ada Fasa." },
  { "en": "Apa Itu Solder Paste (Pasta Solder)?", "id": "Campuran Timah Bubuk Dan Flux (SMD)." },
  { "en": "Apa Itu Stensil Solder (Solder Stencil)?", "id": "Cetakan Logam Untuk Aplikasi Pasta Solder." },
  { "en": "Apa Itu Oven Reflow (Reflow Oven)?", "id": "Oven Khusus Pemasangan Komponen SMD." },
  { "en": "Apa Itu Preheating (Pemanasan Awal) Solder?", "id": "Proses Menghangatkan PCB Sebelum Solder." },
  { "en": "Apa Itu Thermal Shock (Kejut Termal)?", "id": "Kerusakan Komponen Akibat Perubahan Suhu Cepat." },
  { "en": "Apa Itu Land Pattern (Pola Jejak)?", "id": "Nama Lain Footprint Komponen Di PCB." },
  { "en": "Apa Itu Panelisasi PCB (PCB Panelization)?", "id": "Menggabungkan Banyak Desain PCB Kecil." },
  { "en": "Apa Itu V-Cut (V-Groove) PCB?", "id": "Potongan Alur V Pemisah Papan PCB." },
  { "en": "Apa Itu Tenteng Via (Tented Via)?", "id": "Lubang Via Yang Ditutup Solder Mask." },
  { "en": "Apa Itu Via Plugged (Tersumbat)?", "id": "Lubang Via Yang Diisi Resin Epoksi." },
  { "en": "Apa Itu Kapasitansi Parasitik?", "id": "Kapasitansi Yang Tidak Diinginkan (Antar Jalur)." },
  { "en": "Apa Itu Induktansi Parasitik?", "id": "Induktansi Yang Tidak Diinginkan (Pada Jalur)." },
  { "en": "Apa Itu Crosstalk (Sinyal Silang)?", "id": "Gangguan Sinyal Antar Jalur PCB Dekat." },
  { "en": "Bagaimana Mengurangi Crosstalk?", "id": "Menjauhkan Jalur, Menggunakan Jalur Ground." },
  { "en": "Apa Itu Jalur Ground Plane (Bidang Ground)?", "id": "Lapisan Tembaga Luas Sebagai Ground." },
  { "en": "Apa Keuntungan Ground Plane?", "id": "Mengurangi Noise, Referensi Ground Stabil." },
  { "en": "Apa Itu Jalur Power Plane (Bidang Daya)?", "id": "Lapisan Tembaga Luas Untuk VCC." },
  { "en": "Apa Itu Thermal Relief (Pelepas Termal)?", "id": "Koneksi Jaring Ke Ground Plane." },
  { "en": "Fungsi Thermal Relief?", "id": "Memudahkan Penyolderan Komponen Kaki Ground." },
  { "en": "Apa Itu Jalur Diferensial (Differential Pair)?", "id": "Dua Jalur Sinyal (Positif, Negatif)." },
  { "en": "Kenapa Jalur Diferensial Harus Sama Panjang?", "id": "Menjaga Integritas Sinyal Kecepatan Tinggi." },
  { "en": "Contoh Jalur Diferensial?", "id": "Jalur USB, Ethernet, HDMI." },
  { "en": "Apa Itu Kontrol Impedansi (Impedance Control)?", "id": "Menjaga Impedansi Jalur PCB Tetap." },
  { "en": "Kenapa Kontrol Impedansi Penting?", "id": "Untuk Sinyal Frekuensi Sangat Tinggi (RF)." },
  { "en": "Apa Itu Jalur Microstrip?", "id": "Jalur Di Lapisan Luar Di Atas Ground." },
  { "en": "Apa Itu Jalur Stripline?", "id": "Jalur Di Lapisan Dalam (Di Antara Ground)." },
  { "en": "Apa Itu Via Buta (Blind Via)?", "id": "Via Penghubung Luar Ke Dalam (Sebagian)." },
  { "en": "Apa Itu Via Terkubur (Buried Via)?", "id": "Via Penghubung Antar Lapisan Dalam." },
  { "en": "Apa Itu Tes Jarum Terbang (Flying Probe)?", "id": "Metode Tes PCB Tanpa Perlengkapan Khusus." },
  { "en": "Apa Itu Tes In-Circuit (ICT)?", "id": "Metode Tes PCB Menggunakan Ranjang Paku." },
  { "en": "Apa Itu JTAG (Joint Test Action Group) Boundary Scan?", "id": "Metode Tes IC Di Papan PCB." },
  { "en": "Apa Itu Conformal Coating (Pelapis Konformal)?", "id": "Lapisan Pelindung Tipis Untuk PCB." },
  { "en": "Fungsi Conformal Coating?", "id": "Melindungi PCB Dari Kelembaban Dan Debu." },
  { "en": "Apa Itu Potting (Enkapsulasi)?", "id": "Melapisi Total Rangkaian Dengan Resin." },
  { "en": "Fungsi Potting (Enkapsulasi)?", "id": "Perlindungan Total (Air, Getaran, Rahasia)." },
  { "en": "Apa Itu LED (Light Emitting Diode) Tembus Lubang (THT)?", "id": "LED Biasa Dengan Kaki Panjang." },
  { "en": "Apa Itu LED (Light Emitting Diode) Chip On Board (COB)?", "id": "Banyak Chip LED Dalam Satu Modul." },
  { "en": "Apa Keuntungan LED (Light Emitting Diode) COB?", "id": "Cahaya Sangat Terang Dan Merata." },
  { "en": "Apa Itu Lumen (Satuan)?", "id": "Satuan Pengukuran Kecerahan Cahaya." },
  { "en": "Apa Itu Candela (Satuan)?", "id": "Satuan Intensitas Cahaya (Arah Tertentu)." },
  { "en": "Apa Itu Lux (Satuan)?", "id": "Satuan Iluminasi (Lumen Per Meter Persegi)." },
  { "en": "Apa Itu Suhu Warna (Color Temperature)?", "id": "Tampilan Warna Cahaya (Warm/Cool White)." },
  { "en": "Apa Satuan Suhu Warna?", "id": "Kelvin (K)." },
  { "en": "Apa Itu Cahaya Warm White?", "id": "Cahaya Putih Kekuningan (Hangat)." },
  { "en": "Apa Itu Cahaya Cool White?", "id": "Cahaya Putih Kebiruan (Dingin)." },
  { "en": "Apa Itu CRI (Color Rendering Index)?", "id": "Kemampuan Cahaya Menampilkan Warna Asli." },
  { "en": "Apa Itu Efisiensi Luminus (Luminous Efficacy)?", "id": "Efisiensi (Lumen Per Watt)." },
  { "en": "Apa Itu Ballast (Lampu Neon)?", "id": "Komponen Pembatas Arus Lampu Neon." },
  { "en": "Apa Itu Starter (Lampu Neon)?", "id": "Komponen Pemicu Awal Lampu Neon." },
  { "en": "Apa Itu Lampu Pijar (Incandescent)?", "id": "Lampu Berbasis Pemanasan Filamen." },
  { "en": "Apa Itu Lampu Halogen?", "id": "Lampu Pijar Dengan Gas Halogen." },
  { "en": "Apa Itu Lampu CFL (Compact Fluorescent Lamp)?", "id": "Lampu Neon Kompak (Hemat Energi)." },
  { "en": "Apa Itu Lampu LED (Light Emitting Diode)?", "id": "Lampu Berbasis Dioda Pemancar Cahaya." },
  { "en": "Mana Lampu Paling Efisien?", "id": "Lampu LED." },
  { "en": "Apa Itu Driver LED (Light Emitting Diode)?", "id": "Catu Daya Khusus Untuk Lampu LED." },
  { "en": "Kenapa LED (Light Emitting Diode) Perlu Driver?", "id": "LED Membutuhkan Arus Konstan (CC)." },
  { "en": "Apa Itu Dioda HV (High Voltage)?", "id": "Dioda Tegangan Tinggi (Contoh: Microwave)." },
  { "en": "Apa Itu Dioda Fast Recovery (Pemulihan Cepat)?", "id": "Dioda Penyearah Untuk Frekuensi Tinggi." },
  { "en": "Kenapa Perlu Dioda Fast Recovery?", "id": "Dioda Biasa Lambat Di Frekuensi Tinggi." },
  { "en": "Apa Itu Dioda Soft Recovery?", "id": "Dioda Pemulihan Halus (Mengurangi Noise)." },
  { "en": "Apa Itu Dioda Step Recovery?", "id": "Dioda Penghasil Pulsa Harmonik." },
  { "en": "Apa Itu Dioda Backward?", "id": "Mirip Dioda Tunnel (Detektor Lemah)." },
  { "en": "Apa Itu Dioda Peltier (Termoelektrik)?", "id": "Komponen Pemindah Panas (Pendingin/Pemanas)." },
  { "en": "Bagaimana Cara Kerja Dioda Peltier?", "id": "Satu Sisi Dingin, Satu Sisi Panas." },
  { "en": "Apa Itu Efek Peltier?", "id": "Prinsip Kerja Modul Termoelektrik." },
  { "en": "Apa Itu Efek Seebeck?", "id": "Kebalikan Efek Peltier (Beda Panas Jadi Listrik)." },
  { "en": "Apa Itu Generator Termoelektrik (TEG)?", "id": "Pembangkit Listrik Dari Beda Panas." },
  { "en": "Apa Itu Sistem Tenaga Surya Fotovoltaik?", "id": "Pembangkit Listrik Tenaga Matahari." },
  { "en": "Apa Itu Panel Surya (Solar Panel)?", "id": "Komponen Utama Pembangkit Tenaga Surya." },
  { "en": "Apa Itu Sel Surya (Solar Cell)?", "id": "Unit Dasar Pembuat Panel Surya." },
  { "en": "Bahan Utama Sel Surya?", "id": "Silikon (Monokristalin, Polikristalin)." },
  { "en": "Apa Itu Monokristalin (Monocrystalline)?", "id": "Sel Surya Efisiensi Tinggi (Satu Kristal)." },
  { "en": "Apa Itu Polikristalin (Polycrystalline)?", "id": "Sel Surya Efisiensi Sedang (Banyak Kristal)." },
  { "en": "Apa Itu Thin Film (Lapisan Tipis) Solar Cell?", "id": "Sel Surya Fleksibel (Efisiensi Rendah)." },
  { "en": "Apa Itu MPPT (Maximum Power Point Tracking)?", "id": "Teknik Pemanenan Daya Surya Maksimal." },
  { "en": "Apa Itu Solar Charge Controller (SCC)?", "id": "Pengatur Pengisian Baterai Panel Surya." },
  { "en": "Fungsi Solar Charge Controller?", "id": "Mencegah Overcharge Dan Over-Discharge Baterai." },
  { "en": "Apa Itu SCC (Solar Charge Controller) Tipe PWM?", "id": "Tipe Kontroler Sederhana (Murah)." },
  { "en": "Apa Itu SCC (Solar Charge Controller) Tipe MPPT?", "id": "Tipe Kontroler Canggih (Efisiensi Tinggi)." },
  { "en": "Apa Itu Inverter Tenaga Surya (Solar Inverter)?", "id": "Pengubah Arus DC Panel Ke AC." },
  { "en": "Apa Itu Sistem On-Grid (Terhubung Jaringan)?", "id": "Sistem Surya Terhubung Jaringan PLN." },
  { "en": "Apa Itu Sistem Off-Grid (Lepas Jaringan)?", "id": "Sistem Surya Mandiri (Menggunakan Baterai)." },
  { "en": "Apa Itu Sistem Hibrida (Hybrid)?", "id": "Gabungan On-Grid Dan Off-Grid (PLN Baterai)." },
  { "en": "Apa Itu Net Metering (Impor Ekspor)?", "id": "Skema Menjual Kelebihan Listrik Surya Ke PLN." },
  { "en": "Apa Itu Baterai Siklus Dalam (Deep Cycle)?", "id": "Baterai Khusus Sistem Tenaga Surya." },
  { "en": "Kenapa Perlu Baterai Deep Cycle?", "id": "Tahan Pengosongan Muatan Berulang Kali." },
  { "en": "Apa Itu Aki (Baterai) VRLA (Valve Regulated Lead Acid)?", "id": "Aki Kering (Tipe AGM Dan Gel)." },
  { "en": "Apa Itu Baterai AGM (Absorbent Glass Mat)?", "id": "Aki Kering (Elektrolit Diserap Alas Kaca)." },
  { "en": "Apa Itu Baterai Gel (Gel Battery)?", "id": "Aki Kering (Elektrolit Berbentuk Gel)." },
  { "en": "Apa Itu Baterai Lithium Besi Fosfat (LiFePO4)?", "id": "Baterai Lithium (Siklus Hidup Panjang)." },
  { "en": "Apa Itu BMS (Battery Management System)?", "id": "Sistem Manajemen Perlindungan Baterai Lithium." },
  { "en": "Fungsi BMS (Battery Management System)?", "id": "Melindungi Baterai Dari Overcharge Undercharge." },
  { "en": "Apa Itu Overcharge (Baterai)?", "id": "Pengisian Daya Berlebih (Merusak)." },
  { "en": "Apa Itu Over-Discharge (Baterai)?", "id": "Pengosongan Daya Berlebih (Merusak)." },
  { "en": "Apa Itu Cell Balancing (BMS)?", "id": "Fitur Penyeimbang Tegangan Sel Baterai." },
  { "en": "Kenapa Cell Balancing (BMS) Penting?", "id": "Memperpanjang Umur Baterai." },
  { "en": "Apa Itu Baterai Seri?", "id": "Menghubungkan Baterai (Menambah Tegangan)." },
  { "en": "Apa Itu Baterai Paralel?", "id": "Menghubungkan Baterai (Menambah Kapasitas Arus)." },
  { "en": "Apa Satuan Kapasitas Baterai?", "id": "Amper-Jam (Ah) Atau Miliamper-Jam (mAh)." },
  { "en": "Apa Itu C-Rate (Baterai)?", "id": "Ukuran Kecepatan Pengisian Pengosongan." },
  { "en": "Apa Arti 1C (C-Rate)?", "id": "Arus Sama Dengan Kapasitas (Satu Jam)." },
  { "en": "Apa Arti 5C (C-Rate)?", "id": "Arus Lima Kali Kapasitas." },
  { "en": "Apa Itu Siklus Hidup (Cycle Life) Baterai?", "id": "Jumlah Siklus Isi Ulang Baterai." },
  { "en": "Apa Itu Self-Discharge (Baterai)?", "id": "Baterai Kehilangan Muatan Saat Disimpan." },
  { "en": "Apa Itu Memory Effect (Efek Memori) Baterai?", "id": "Penurunan Kapasitas Baterai (Tipe NiCd)." },
  { "en": "Apa Itu Baterai NiCd (Nickel-Cadmium)?", "id": "Tipe Baterai Isi Ulang (Lama)." },
  { "en": "Apa Itu Baterai NiMH (Nickel-Metal Hydride)?", "id": "Tipe Baterai Isi Ulang (Pengganti NiCd)." },
  { "en": "Apa Itu Baterai LiPo (Lithium-Polymer)?", "id": "Baterai Lithium (Bentuk Fleksibel)." },
  { "en": "Apa Kelemahan Baterai LiPo (Lithium-Polymer)?", "id": "Rentan Terbakar Jika Rusak." },
  { "en": "Apa Itu Baterai Koin (Coin Cell)?", "id": "Baterai Kecil Bulat Tipis." },
  { "en": "Contoh Baterai Koin?", "id": "CR2032 (Untuk Jam RTC)." },
  { "en": "Apa Itu Elektrolit (Baterai)?", "id": "Cairan Kimia Penghantar Ion." },
  { "en": "Apa Itu Anoda (Baterai)?", "id": "Elektroda Negatif (Saat Pengosongan)." },
  { "en": "Apa Itu Katoda (Baterai)?", "id": "Elektroda Positif (Saat Pengosongan)." },
  { "en": "Apa Itu Separator (Baterai)?", "id": "Pemisah Isolator Antara Anoda Katoda." },
  { "en": "Apa Itu Ventilasi Thermal (Thermal Runaway) Baterai?", "id": "Kondisi Baterai Panas Berlebih (Terbakar)." },
  { "en": "Apa Itu Alat Solder (Soldering Iron)?", "id": "Alat Pemanas Untuk Melelehkan Timah." },
  { "en": "Apa Itu Mata Solder (Soldering Tip)?", "id": "Ujung Logam Panas Pada Solder." },
  { "en": "Bentuk Mata Solder (Soldering Tip)?", "id": "Runcing, Pahat, Bevel." },
  { "en": "Apa Itu Dudukan Solder (Soldering Stand)?", "id": "Tempat Penyimpanan Solder Saat Panas." },
  { "en": "Apa Itu Spons Solder (Solder Sponge)?", "id": "Spons Basah Pembersih Mata Solder." },
  { "en": "Apa Itu Serabut Logam (Brass Wool) Solder?", "id": "Pembersih Mata Solder Kering." },
  { "en": "Apa Itu Timah Solder (Solder Wire)?", "id": "Kawat Logam Paduan (Timah, Timbal)." },
  { "en": "Apa Itu Inti Flux (Flux Core) Timah?", "id": "Flux Di Bagian Tengah Timah." },
  { "en": "Apa Itu Flux (Pasta Solder)?", "id": "Bahan Kimia Pembersih Oksidasi." },
  { "en": "Kenapa Perlu Flux Saat Menyolder?", "id": "Agar Timah Menempel Sempurna." },
  { "en": "Apa Itu Solder Ber-Timbal (Leaded Solder)?", "id": "Timah Solder Campuran Timbal (Contoh 60/40)." },
  { "en": "Apa Itu Solder Bebas Timbal (Lead-Free)?", "id": "Timah Solder Tanpa Timbal (RoHS)." },
  { "en": "Apa Titik Leleh Solder Bebas Timbal?", "id": "Lebih Tinggi Dari Solder Bertimbal." },
  { "en": "Apa Itu Solder Sucker (Penyedot Timah)?", "id": "Pompa Vakum Manual Penghisap Timah." },
  { "en": "Apa Itu Solder Wick (Kawat Pembersih)?", "id": "Serabut Tembaga Penghisap Sisa Timah." },
  { "en": "Apa Itu Stasiun Solder (Soldering Station)?", "id": "Solder Dengan Kontrol Suhu." },
  { "en": "Apa Keuntungan Stasiun Solder?", "id": "Suhu Stabil, Mata Solder Awet." },
  { "en": "Apa Itu Stasiun Solder Uap (Hot Air)?", "id": "Alat Solder SMD Menggunakan Udara Panas." },
  { "en": "Apa Itu Pemanas Bawah (Preheater)?", "id": "Pemanas PCB Dari Bawah (Bantu Solder)." },
  { "en": "Apa Itu Jembatan Solder (Solder Bridge)?", "id": "Timah Yang Menghubung Singkat Dua Pin." },
  { "en": "Bagaimana Memperbaiki Jembatan Solder?", "id": "Gunakan Solder Wick Atau Flux." },
  { "en": "Apa Itu Solderan Kering (Dry Joint)?", "id": "Sama Dengan Solder Dingin (Retak)." },
  { "en": "Apa Itu Teknik Wetting (Solder)?", "id": "Kemampuan Timah Cair Membasahi Permukaan." },
  { "en": "Apa Itu Penjepit PCB (PCB Vise)?", "id": "Alat Bantu Penahan PCB (Helping Hands)." },
  { "en": "Apa Itu Fume Extractor (Penghisap Asap)?", "id": "Alat Penghisap Asap Solder Beracun." },
  { "en": "Apa Itu Tang Kupas Kabel (Wire Stripper)?", "id": "Tang Pengupas Isolasi Kawat." },
  { "en": "Apa Itu Tang Krimping (Crimping Tool)?", "id": "Tang Pemasang Konektor (Skun)." },
  { "en": "Apa Itu Skun (Konektor)?", "id": "Terminal Logam Ujung Kabel." },
  { "en": "Apa Itu Ferrule (Kabel Serabut)?", "id": "Selongsong Logam Ujung Kabel Serabut." },
  { "en": "Apa Itu Kunci L (Hex Key)?", "id": "Kunci Baut Berbentuk Segi Enam." },
  { "en": "Apa Itu Pisau Cutter (Utility Knife)?", "id": "Pisau Serbaguna Untuk Memotong." },
  { "en": "Apa Itu Jangka Sorong (Caliper)?", "id": "Alat Ukur Dimensi Presisi (Digital/Analog)." },
  { "en": "Apa Itu Mikrometer Sekrup (Micrometer)?", "id": "Alat Ukur Dimensi Sangat Presisi." },
  { "en": "Apa Itu Bor PCB (PCB Drill)?", "id": "Bor Kecil Khusus Melubangi PCB." },
  { "en": "Apa Itu Mata Bor (Drill Bit)?", "id": "Ujung Tajam Bor Untuk Melubangi." },
  { "en": "Apa Itu Dremel (Rotary Tool)?", "id": "Alat Serbaguna (Potong, Bor, Amplas)." },
  { "en": "Apa Itu Lem Tembak (Hot Glue Gun)?", "id": "Pistol Lem Panas (Perekat Isolator)." },
  { "en": "Apa Itu Selotip Listrik (Electrical Tape)?", "id": "Selotip Isolasi Sambungan Kabel." },
  { "en": "Apa Itu Selang Bakar (Heat Shrink)?", "id": "Selang Plastik Isolasi (Mengerut Panas)." },
  { "en": "Apa Itu Label Kabel (Cable Marker)?", "id": "Penanda Identifikasi Kabel." },
  { "en": "Apa Itu Pengikat Kabel (Cable Tie)?", "id": "Plastik Pengikat Untuk Merapikan Kabel." },
  { "en": "Apa Itu Spiral Kabel (Spiral Wrap)?", "id": "Pelindung Pembungkus Kabel Spiral." },
  { "en": "Apa Itu Terminal Blok (Terminal Block)?", "id": "Konektor Kabel Menggunakan Sekrup." },
  { "en": "Apa Itu Konektor Sekrup Hijau?", "id": "Terminal Blok Hijau (Umum Di PCB)." },
  { "en": "Apa Itu Konektor Wago?", "id": "Konektor Kabel Cepat (Push-In/Lever)." },
  { "en": "Apa Itu Kotak Proyek (Project Enclosure)?", "id": "Boks Plastik Penyimpan Rangkaian." },
  { "en": "Apa Itu Kaki Karet (Rubber Feet)?", "id": "Kaki Alas Anti Selip Kotak Proyek." },
  { "en": "Apa Itu Standoff (Spacer)?", "id": "Penyangga PCB (Beri Jarak)." },
  { "en": "Bahan Standoff (Spacer)?", "id": "Logam (Kuningan) Atau Plastik (Nilon)." },
  { "en": "Apa Itu Saklar Panel (Panel Mount)?", "id": "Saklar Yang Dipasang Di Dinding Boks." },
  { "en": "Apa Itu LED (Light Emitting Diode) Holder?", "id": "Dudukan LED Untuk Pemasangan Panel." },
  { "en": "Apa Itu Bezel (Layar)?", "id": "Bingkai Penutup Sekitar Layar LCD." },
  { "en": "Apa Itu Grommet Karet?", "id": "Cincin Karet Pelindung Lubang Kabel." },
  { "en": "Apa Itu Cable Gland?", "id": "Konektor Tahan Air Masuknya Kabel Ke Boks." },
  { "en": "Apa Itu Baterai AA?", "id": "Ukuran Standar Baterai Silinder." },
  { "en": "Apa Itu Baterai AAA?", "id": "Ukuran Baterai (Lebih Kecil Dari AA)." },
  { "en": "Apa Itu Baterai 9V (Kotak)?", "id": "Baterai Persegi Tegangan 9 Volt." },
  { "en": "Apa Itu Baterai 18650?", "id": "Baterai Lithium (Diameter 18mm, Panjang 65mm)." },
  { "en": "Di Mana Baterai 18650 Digunakan?", "id": "Laptop, Power Bank, Senter, Vape." },
  { "en": "Apa Itu Tempat Baterai (Battery Holder)?", "id": "Wadah Plastik Rangkaian Baterai." },
  { "en": "Apa Itu Klip Baterai (Battery Clip)?", "id": "Konektor Kabel Baterai 9V." },
  { "en": "Apa Itu Stop Kontak (Wall Outlet)?", "id": "Sumber Listrik AC Dinding (PLN)." },
  { "en": "Apa Itu Steker (Plug)?", "id": "Colokan Listrik Jantan (Berkaki)." },
  { "en": "Apa Itu Soket (Socket)?", "id": "Colokan Listrik Betina (Berlubang)." },
  { "en": "Apa Itu Kabel Ekstensi (Extension Cord)?", "id": "Kabel Perpanjangan Stop Kontak." },
  { "en": "Apa Itu Pelindung Lonjakan (Surge Protector)?", "id": "Stop Kontak Dengan Pelindung MOV." },
  { "en": "Apa Itu UPS (Uninterruptible Power Supply)?", "id": "Catu Daya Darurat (Baterai Internal)." },
  { "en": "Fungsi UPS (Uninterruptible Power Supply)?", "id": "Memberi Waktu Simpan Data Saat Listrik Mati." },
  { "en": "Apa Itu Tipe UPS Standby (Offline)?", "id": "UPS Pindah Ke Baterai Saat Listrik Mati." },
  { "en": "Apa Itu Tipe UPS Line-Interactive?", "id": "UPS Standby Dengan Stabilizer (AVR)." },
  { "en": "Apa Itu Tipe UPS Online (Double Conversion)?", "id": "UPS Paling Stabil (Selalu Dari Baterai)." },
  { "en": "Apa Itu AVR (Automatic Voltage Regulator)?", "id": "Alat Penstabil Tegangan Listrik (Stavolt)." },
  { "en": "Apa Itu Inverter Pure Sine Wave (PSW)?", "id": "Inverter Penghasil Gelombang Sinus Murni." },
  { "en": "Apa Itu Inverter Modified Sine Wave (MSW)?", "id": "Inverter Gelombang Sinus Modifikasi (Kotak)." },
  { "en": "Mana Inverter Lebih Baik Untuk Elektronik Sensitif?", "id": "Inverter Pure Sine Wave (PSW)." },
  { "en": "Apa Itu Grounding (Pentanahan) Listrik?", "id": "Koneksi Sistem Listrik Ke Tanah (Bumi)." },
  { "en": "Apa Fungsi Grounding (Pentanahan)?", "id": "Keamanan (Mencegah Kejut Listrik)." },
  { "en": "Apa Itu Batang Ground (Ground Rod)?", "id": "Logam Yang Ditanam Ke Tanah." },
  { "en": "Apa Itu Kawat Fasa (Live Wire)?", "id": "Kawat Listrik AC Beraliran Tegangan." },
  { "en": "Apa Itu Kawat Netral (Neutral Wire)?", "id": "Kawat Listrik AC Referensi Nol." },
  { "en": "Apa Itu Kawat Ground (Ground Wire)?", "id": "Kawat Listrik Pengaman (Ke Bodi Alat)." },
  { "en": "Warna Kawat Fasa Di Indonesia?", "id": "Biasanya Hitam, Coklat, Atau Abu-Abu." },
  { "en": "Warna Kawat Netral Di Indonesia?", "id": "Biasanya Biru." },
  { "en": "Warna Kawat Ground Di Indonesia?", "id": "Selalu Hijau-Kuning." },
  { "en": "Apa Itu Konsleting (Hubung Singkat)?", "id": "Kontak Langsung Antara Fasa Dan Netral." },
  { "en": "Apa Itu Beban Berlebih (Overload)?", "id": "Penggunaan Arus Melebihi Kapasitas Kabel." },
  { "en": "Bahaya Beban Berlebih (Overload)?", "id": "Kabel Panas Yang Bisa Menyebabkan Kebakaran." },
  { "en": "Apa Itu Arus Sisa (Residual Current)?", "id": "Arus Bocor Ke Tanah (Sangat Berbahaya)." },
  { "en": "Alat Pelindung Arus Sisa?", "id": "ELCB (Earth Leakage Circuit Breaker)." },
  { "en": "Apa Itu Motor Listrik?", "id": "Mesin Pengubah Energi Listrik Ke Mekanik." },
  { "en": "Apa Itu Motor DC (Direct Current) Brushless?", "id": "Motor DC Tanpa Sikat Arang (Efisiensi)." },
  { "en": "Apa Itu Motor AC (Alternating Current)?", "id": "Motor Listrik Berarus Bolak-Balik." },
  { "en": "Apa Itu Motor Induksi (Induction Motor)?", "id": "Tipe Motor AC Paling Umum (Industri)." },
  { "en": "Apa Itu Motor Sinkron (Synchronous Motor)?", "id": "Motor AC Dengan Kecepatan Presisi." },
  { "en": "Apa Itu Stator (Motor)?", "id": "Bagian Motor Yang Diam (Kumparan)." },
  { "en": "Apa Itu Rotor (Motor)?", "id": "Bagian Motor Yang Berputar (Poros)." },
  { "en": "Apa Itu Sikat Arang (Brush) Motor DC?", "id": "Komponen Penghantar Listrik Ke Rotor." },
  { "en": "Apa Itu Komutator (Commutator) Motor DC?", "id": "Segmen Pembalik Arus Di Rotor." },
  { "en": "Apa Itu Motor Universal?", "id": "Motor Yang Bisa Beroperasi Di AC/DC." },
  { "en": "Contoh Penggunaan Motor Universal?", "id": "Bor Listrik, Blender, Vacuum Cleaner." },
  { "en": "Apa Itu VFD (Variable Frequency Drive)?", "id": "Inverter Pengatur Kecepatan Motor AC." },
  { "en": "Bagaimana VFD (Variable Frequency Drive) Bekerja?", "id": "Mengubah Frekuensi Tegangan Input." },
  { "en": "Apa Itu Soft Starter (Motor)?", "id": "Rangkaian Pembatas Arus Awal Motor." },
  { "en": "Apa Itu Rem Dinamis (Dynamic Braking)?", "id": "Metode Pengereman Motor (Membuang Energi)." },
  { "en": "Apa Itu Rem Regeneratif (Regenerative Braking)?", "id": "Pengereman Motor (Mengembalikan Energi)." },
  { "en": "Di Mana Rem Regeneratif Digunakan?", "id": "Mobil Listrik, Kereta Listrik." },
  { "en": "Apa Itu Generator (Dinamo)?", "id": "Mesin Pengubah Energi Mekanik Ke Listrik." },
  { "en": "Apa Kebalikan Dari Motor?", "id": "Generator Adalah Kebalikan Dari Motor." },
  { "en": "Apa Itu Alternator?", "id": "Generator Listrik Penghasil Arus AC." },
  { "en": "Di Mana Alternator Digunakan?", "id": "Mobil (Pengisi Aki), Pembangkit Listrik." },
  { "en": "Apa Itu Rectifier (Penyearah)?", "id": "Rangkaian Pengubah Arus AC Ke DC." },
  { "en": "Apa Itu Inverter (Pembalik)?", "id": "Rangkaian Pengubah Arus DC Ke AC." },
  { "en": "Apa Itu Konverter (Converter)?", "id": "Rangkaian Pengubah Level Energi Listrik." },
  { "en": "Apa Itu SMPS (Switch Mode Power Supply)?", "id": "Catu Daya Berbasis Switching Efisiensi Tinggi." },
  { "en": "Apa Beda SMPS (Switch Mode Power Supply) Dan Trafo Linear?", "id": "SMPS (Ringan Efisien), Linear (Berat Boros)." },
  { "en": "Apa Kelemahan SMPS (Switch Mode Power Supply)?", "id": "Menghasilkan Noise Frekuensi Tinggi." },
  { "en": "Komponen Utama SMPS (Switch Mode Power Supply)?", "id": "Saklar (MOSFET), Trafo Ferit, Dioda Cepat." },
  { "en": "Apa Itu Topologi Flyback SMPS?", "id": "Topologi SMPS Sederhana (Daya Rendah)." },
  { "en": "Apa Itu Topologi Forward SMPS?", "id": "Topologi SMPS (Daya Menengah)." },
  { "en": "Apa Itu Topologi Half-Bridge SMPS?", "id": "Topologi SMPS (Daya Tinggi)." },
  { "en": "Apa Itu Topologi Full-Bridge SMPS?", "id": "Topologi SMPS (Daya Sangat Tinggi)." },
  { "en": "Apa Itu Kontrol PWM (Pulse Width Modulation) Di SMPS?", "id": "Mengatur Duty Cycle Saklar (Regulasi)." },
  { "en": "Apa Itu Optocoupler Di SMPS?", "id": "Memberi Umpan Balik (Feedback) Terisolasi." },
  { "en": "Apa Itu Hot Side (Sisi Panas) SMPS?", "id": "Sisi Primer Tegangan Tinggi (Berbahaya)." },
  { "en": "Apa Itu Cold Side (Sisi Dingin) SMPS?", "id": "Sisi Sekunder Tegangan Rendah (Aman)." },
  { "en": "Apa Itu Kapasitor Bulk (Bulk Capacitor)?", "id": "Kapasitor Elco Besar Di Sisi Primer." },
  { "en": "Apa Itu Dioda Bridge Di Input SMPS?", "id": "Penyearah Tegangan Listrik AC Input." },
  { "en": "Apa Itu Thermistor NTC Di SMPS?", "id": "Pembatas Arus Inrush Saat Dinyalakan." },
  { "en": "Apa Itu Filter EMI (Electromagnetic Interference) Di SMPS?", "id": "Filter Pencegah Noise Ke Jaringan PLN." },
  { "en": "Apa Itu PFC (Power Factor Correction)?", "id": "Rangkaian Perbaikan Faktor Daya (Di SMPS)." },
  { "en": "Kenapa PFC (Power Factor Correction) Penting?", "id": "Meningkatkan Efisiensi, Mengurangi Beban Jaringan." },
  { "en": "Apa Itu PFC (Power Factor Correction) Pasif?", "id": "Koreksi Faktor Daya Menggunakan Induktor." },
  { "en": "Apa Itu PFC (Power Factor Correction) Aktif?", "id":-"Koreksi Faktor Daya Menggunakan Rangkaian Elektronik." },
  { "en": "Apa Itu Ripple (Riak) Tegangan Output?", "id": "Fluktuasi Kecil Pada Tegangan DC." },
  { "en": "Apa Itu Efisiensi (Efficiency) Catu Daya?", "id": "Rasio Daya Output Terhadap Daya Input." },
  { "en": "Apa Itu Disipasi Panas (Heat Dissipation)?", "id": "Energi Yang Hilang Menjadi Panas." },
  { "en": "Apa Itu Standby Power (Daya Siaga)?", "id": "Daya Yang Digunakan Saat Perangkat Mati." },
  { "en": "Apa Itu Elektronika Daya (Power Electronics)?", "id": "Bidang Elektronika Kontrol Energi Listrik." },
  { "en": "Apa Itu Sirkuit Terpadu (IC) Analog?", "id": "IC Pengolah Sinyal Kontinu (Op-Amp)." },
  { "en": "Apa Itu Sirkuit Terpadu (IC) Digital?", "id": "IC Pengolah Sinyal Diskrit (Gerbang Logika)." },
  { "en": "Apa Itu Sirkuit Terpadu (IC) Mixed-Signal?", "id": "IC Gabungan Analog Dan Digital (ADC/DAC)." },
  { "en": "Apa Itu SoC (System on a Chip)?", "id": "Sistem Komputer Lengkap Dalam Satu IC." },
  { "en": "Contoh SoC (System on a Chip)?", "id": "Prosesor Smartphone, ESP32." },
  { "en": "Apa Itu Wafer (Silikon)?", "id": "Bahan Dasar Pembuatan Sirkuit Terpadu." },
  { "en": "Apa Itu Fabrikasi (Semikonduktor)?", "id": "Proses Pembuatan Chip IC." },
  { "en": "Apa Itu Litografi (Photolithography)?", "id": "Proses Pencetakan Pola Sirkuit Di Wafer." },
  { "en": "Apa Itu Ukuran Proses (Process Node)?", "id": "Ukuran Terkecil Transistor (Contoh: 7nm)." },
  { "en": "Apa Itu Clean Room (Ruang Bersih)?", "id": "Ruang Produksi IC Bebas Debu." },
  { "en": "Apa Itu Hukum Moore (Moore's Law)?", "id": "Prediksi Jumlah Transistor Ganda Tiap Dua Tahun." },
  { "en": "Apa Itu Die (IC)?", "id": "Potongan Kecil Wafer (Satu Chip)." },
  { "en": "Apa Itu Bonding Kawat (Wire Bonding)?", "id": "Menyambung Die Ke Kaki Paket IC." },
  { "en": "Apa Itu Flip-Chip?", "id": "Metode Pemasangan Die (Bola Solder)." },
  { "en": "Apa Itu Yield (Manufaktur IC)?", "id": "Persentase Chip Bagus Dari Satu Wafer." },
  { "en": "Apa Itu Bipolar (Teknologi IC)?", "id": "Teknologi Berbasis Transistor BJT (TTL, ECL)." },
  { "en": "Apa Itu MOS (Metal-Oxide-Semiconductor) (Teknologi IC)?", "id": "Teknologi Berbasis MOSFET (CMOS)." },
  { "en": "Apa Itu BiCMOS (Bipolar-CMOS)?", "id": "Gabungan Teknologi Bipolar Dan CMOS." },
  { "en": "Apa Itu Gallium Nitride (GaN)?", "id": "Bahan Semikonduktor (Efisiensi Tinggi)." },
  { "en": "Di Mana Gallium Nitride (GaN) Digunakan?", "id": "Charger Cepat, SMPS Frekuensi Tinggi." },
  { "en": "Apa Itu Silicon Carbide (SiC)?", "id": "Bahan Semikonduktor (Tegangan Tinggi)." },
  { "en": "Di Mana Silicon Carbide (SiC) Digunakan?", "id": "Mobil Listrik, Inverter Daya Tinggi." },
  { "en": "Apa Itu Resistansi Termal (Thermal Resistance)?", "id": "Ukuran Hambatan Aliran Panas." },
  { "en": "Apa Satuan Resistansi Termal?", "id": "Derajat Celcius Per Watt (°C/W)." },
  { "en": "Apa Itu Junction Temperature (TJ)?", "id": "Suhu Internal Inti Semikonduktor." },
  { "en": "Apa Itu Ambient Temperature (TA)?", "id": "Suhu Udara Lingkungan Sekitar." },
  { "en": "Apa Itu Case Temperature (TC)?", "id": "Suhu Permukaan Bodi Komponen." },
  { "en": "Apa Itu Derating (Penurunan Nilai)?", "id": "Mengurangi Kapasitas Komponen Di Suhu Tinggi." },
  { "en": "Apa Itu Safe Operating Area (SOA)?", "id": "Grafik Batas Aman Operasi Transistor." },
  { "en": "Apa Saja Batasan SOA (Safe Operating Area)?", "id": "Arus Maksimal, Tegangan Maksimal, Daya." },
  { "en": "Apa Itu Second Breakdown (Transistor)?", "id": "Kerusakan Transistor BJT (Hotspot Lokal)." },
  { "en": "Apa Itu Hot-Plugging (Colok Panas)?", "id": "Memasang Komponen Saat Rangkaian Menyala." },
  { "en": "Apa Itu Hot-Swapping (Tukar Panas)?", "id": "Mengganti Komponen Saat Rangkaian Menyala." },
  { "en": "Apakah USB (Universal Serial Bus) Hot-Pluggable?", "id": "Ya, USB Didesain Untuk Hot-Plugging." },
  { "en": "Apa Itu Firmware Update (Pembaruan Firmware)?", "id": "Proses Memperbarui Perangkat Lunak Tertanam." },
  { "en": "Apa Itu OTA (Over-The-Air) Update?", "id": "Pembaruan Firmware Nirkabel (Wi-Fi/Bluetooth)." },
  { "en": "Apa Itu Bricking (Menjadi Bata)?", "id": "Kondisi Perangkat Mati Total (Gagal Update)." },
  { "en": "Apa Itu Bootloader (Mikrokontroler)?", "id": "Program Kecil Pemuat Kode Utama." },
  { "en": "Apa Itu Watchdog Timer (WDT)?", "id": "Timer Pencegah Hang (Reset Otomatis)." },
  { "en": "Bagaimana Watchdog Timer Bekerja?", "id": "Harus Direset Periodik (Ditendang)." },
  { "en": "Apa Itu Sleep Mode (Mode Tidur)?", "id": "Mode Hemat Daya Mikrokontroler." },
  { "en": "Apa Itu Deep Sleep (Tidur Pulas)?", "id": "Mode Hemat Daya Paling Rendah." },
  { "en": "Apa Itu Wake-Up Source (Sumber Bangun)?", "id": "Sinyal Pembangun (Timer, Pin Eksternal)." },
  { "en": "Apa Itu Brown-Out Reset (BOR)?", "id": "Reset Otomatis Jika Tegangan Turun." },
  { "en": "Apa Itu Power-On Reset (POR)?", "id": "Sirkuit Reset Otomatis Saat Dinyalakan." },
  { "en": "Apa Itu Interupsi (Interrupt)?", "id": "Sinyal Penunda Eksekusi Program Utama." },
  { "en": "Apa Itu ISR (Interrupt Service Routine)?", "id": "Fungsi Khusus Penangan Interupsi." },
  { "en": "Apa Itu Interupsi Eksternal?", "id": "Interupsi Dari Sinyal Pin Eksternal." },
  { "en": "Apa Itu Interupsi Timer?", "id": "Interupsi Yang Dihasilkan Oleh Timer." },
  { "en": "Apa Itu Interupsi Vektor?", "id": "Tabel Alamat ISR (Penangan Interupsi)." },
  { "en": "Apa Itu Prioritas Interupsi?", "id": "Urutan Interupsi Yang Didahulukan." },
  { "en": "Apa Itu Nesting Interupsi?", "id": "Terjadinya Interupsi Di Dalam ISR." },
  { "en": "Apa Itu Polling (Komunikasi)?", "id": "Metode Pengecekan Status Berulang Kali." },
  { "en": "Apa Beda Interupsi Dan Polling?", "id": "Interupsi (Reaktif), Polling (Proaktif)." },
  { "en": "Apa Itu Timer (Pewaktu)?", "id": "Modul Perangkat Keras Penghitung Waktu." },
  { "en": "Apa Itu Counter (Pencacah)?", "id": "Modul Perangkat Keras Penghitung Pulsa." },
  { "en": "Apa Itu Prescaler (Timer)?", "id": "Pembagi Frekuensi Clock Untuk Timer." },
  { "en": "Fungsi Prescaler (Timer)?", "id": "MemperpanjAng Durasi Timer." },
  { "en": "Apa Itu Timer Overflow (Limpahan)?", "id": "Kondisi Timer Mencapai Nilai Maksimal." },
  { "en": "Apa Itu Mode Capture (Timer)?", "id": "Mode Perekam Waktu Kejadian Eksternal." },
  { "en": "Apa Itu Mode Compare (Timer)?", "id": "Mode Pemicu Output Saat Nilai Sama." },
  { "en": "Apa Itu PWM (Pulse Width Modulation) Cepat?", "id": "Mode PWM Frekuensi Tinggi." },
  { "en": "Apa Itu PWM (Pulse Width Modulation) Phase-Correct?", "id": "Mode PWM Simetris (Kontrol Motor)." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter)?", "id": "Pengubah Sinyal Analog Ke Digital." },
  { "en": "Apa Itu Resolusi ADC (Analog-to-Digital Converter)?", "id": "Jumlah Bit Hasil Konversi." },
  { "en": "Apa Arti Resolusi ADC 10-bit?", "id": "Output Digital Bernilai 0 Hingga 1023." },
  { "en": "Apa Itu ADC (Analog-to-Digital Converter) Sampling Rate?", "id": "Seberapa Cepat ADC Mengambil Sampel." },
  { "en": "Apa Itu Aliasing (Sinyal)?", "id": "Error Akibat Sampling Rate Terlalu Rendah." },
  { "en": "Apa Itu Teorema Nyquist-Shannon?", "id": "Sampling Rate Harus Dua Kali Frekuensi." },
  { "en": "Apa Itu Filter Anti-Aliasing?", "id": "Filter Low-Pass Sebelum ADC." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter)?", "id": "Pengubah Sinyal Digital Ke Analog." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter) R-2R?", "id": "Tipe DAC Menggunakan Jaringan Resistor." },
  { "en": "Apa Itu DAC (Digital-to-Analog Converter) PWM?", "id": "Membuat Sinyal Analog Menggunakan PWM." },
  { "en": "Apa Itu Kuantisasi (Quantization)?", "id": "Proses Pembulatan Nilai Analog Ke Digital." },
  { "en": "Apa Itu Error Kuantisasi?", "id": "Selisih Antara Nilai Asli Dan Kuantisasi." },
  { "en": "Apa Itu Dither (Audio Digital)?", "id": "Menambah Noise Kecil (Mengurangi Error)." },
  { "en": "Apa Itu DSP (Digital Signal Processing)?", "id": "Pemrosesan Sinyal Dalam Domain Digital." },
  { "en": "Apa Itu Filter FIR (Finite Impulse Response)?", "id": "Tipe Filter Digital (Stabil)." },
  { "en": "Apa Itu Filter IIR (Infinite Impulse Response)?", "id": "Tipe Filter Digital (Rekursif)." },
  { "en": "Apa Itu FFT (Fast Fourier Transform)?", "id": "Algoritma Analisis Spektrum Frekuensi." },
  { "en": "Apa Fungsi FFT (Fast Fourier Transform)?", "id": "Mengubah Sinyal Domain Waktu Ke Frekuensi." },
  { "en": "Apa Itu Domain Waktu?", "id": "Grafik Sinyal Amplitudo Terhadap Waktu." },
  { "en": "Apa Itu Domain Frekuensi?", "id": "Grafik Sinyal Amplitudo Terhadap Frekuensi." },
  { "en": "Apa Itu PID (Proportional Integral Derivative)?", "id": "Algoritma Kontrol Umpan Balik." },
  { "en": "Apa Itu Sistem Kontrol Loop Terbuka?", "id": "Sistem Kontrol Tanpa Umpan Balik." },
  { "en": "Apa Itu Sistem Kontrol Loop Tertutup?", "id": "Sistem Kontrol Dengan Umpan Balik." },
  { "en": "Contoh Sistem Loop Terbuka?", "id": "Kipas Angin (Kecepatan Tidak Diukur)." },
  { "en": "Contoh Sistem Loop Tertutup?", "id": "AC Pendingin Ruangan (Sensor Suhu)." },
  { "en": "Apa Itu Setpoint (Kontrol PID)?", "id": "Nilai Target Yang Diinginkan." },
  { "en": "Apa Itu Process Variable (Kontrol PID)?", "id": "Nilai Aktual Yang Diukur Sensor." },
  { "en": "Apa Itu Error (Kontrol PID)?", "id": "Selisih Antara Setpoint Dan Process Variable." },
  { "en": "Apa Itu Kontrol Proporsional (P)?", "id": "Output Sebanding Dengan Error Saat Ini." },
  { "en": "Apa Itu Kontrol Integral (I)?", "id": "Output Sebanding Akumulasi Error Lalu." },
  { "en": "Apa Itu Kontrol Derivatif (D)?", "id": "Output Sebanding Laju Perubahan Error." },
  { "en": "Fungsi Kontrol Integral (I)?", "id": "Menghilangkan Error Kondisi Stabil (Steady-State)." },
  { "en": "Fungsi Kontrol Derivatif (D)?", "id": "Meredam Osilasi (Respon Cepat)." },
  { "en": "Apa Itu Tuning (Kontrol PID)?", "id": "Proses Penyetelan Konstanta (Kp, Ki, Kd)." },
  { "en": "Apa Itu Metode Tuning Ziegler-Nichols?", "id": "Metode Klasik Tuning PID." },
  { "en": "Apa Itu Overshoot (Kontrol)?", "id": "Kondisi Respon Melebihi Setpoint." },
  { "en": "Apa Itu Undershoot (Kontrol)?", "id": "Kondisi Respon Turun Dibawah Setpoint." },
  { "en": "Apa Itu Osilasi (Kontrol)?", "id": "Respon Berayun Di Sekitar Setpoint." },
  { "en": "Apa Itu Steady-State (Kondisi Stabil)?", "id": "Kondisi Respon Sistem Menetap." },
  { "en": "Apa Itu Rise Time (Waktu Naik)?", "id": "Waktu Respon Naik (Contoh: 10% Ke 90%)." },
  { "en": "Apa Itu Settling Time (Waktu Menetap)?", "id": "Waktu Respon Masuk Toleransi Setpoint." },
  { "en": "Apa Itu PLC (Programmable Logic Controller)?", "id": "Komputer Kontrol Industri Tangguh." },
  { "en": "Bahasa Pemrograman PLC (Programmable Logic Controller)?", "id": "Ladder Logic (Diagram Tangga)." },
  { "en": "Apa Itu Ladder Logic (Diagram Tangga)?", "id": "Bahasa Pemrograman Grafis Mirip Skema Relay." },
  { "en": "Apa Itu HMI (Human Machine Interface)?", "id": "Layar Tampilan Operator Mesin Industri." },
  { "en": "Apa Itu SCADA (Supervisory Control And Data Acquisition)?", "id": "Sistem Pemantauan Kontrol Skala Besar." },
  { "en": "Apa Itu DCS (Distributed Control System)?", "id": "Sistem Kontrol Terdistribusi (Proses Besar)." },
  { "en": "Apa Itu Industri 4.0?", "id": "Revolusi Industri (Otomasi Cerdas, IoT)." },
  { "en": "Apa Itu IIoT (Industrial Internet of Things)?", "id": "IoT Untuk Aplikasi Industri." },
  { "en": "Apa Itu Robotika (Robotics)?", "id": "Ilmu Desain Dan Kontrol Robot." },
  { "en": "Apa Itu Aktuator (Robot)?", "id": "Komponen Penggerak Robot (Motor, Servo)." },
  { "en": "Apa Itu End Effector (Robot)?", "id": "Alat Kerja Di Ujung Lengan Robot." },
  { "en": "Contoh End Effector?", "id": "Gripper (Penjepit), Alat Las, Obeng." },
  { "en": "Apa Itu Lengan Robot (Robotic Arm)?", "id": "Robot Mekanis Mirip Lengan Manusia." },
  { "en": "Apa Itu Derajat Kebebasan (DOF)?", "id": "Jumlah Sumbu Gerakan Independen Robot." },
  { "en": "Apa Itu Kinematika (Robot)?", "id": "Ilmu Gerakan Geometris Robot." },
  { "en": "Apa Itu Kinematika Maju (Forward)?", "id": "Menghitung Posisi Ujung (Dari Sudut Sendi)." },
  { "en": "Apa Itu Kinematika Mundur (Inverse)?", "id": "Menghitung Sudut Sendi (Dari Posisi Ujung)." },
  { "en": "Apa Itu Robot Bergerak (Mobile Robot)?", "id": "Robot Yang Dapat Berpindah Tempat (Roda)." },
  { "en": "Apa Itu Drone (Pesawat Tanpa Awak)?", "id": "Kendaraan Udara Tanpa Awak (UAV)." },
  { "en": "Apa Itu Quadcopter?", "id": "Drone Dengan Empat Baling-Baling." },
  { "en": "Apa Itu Sensor LiDAR (Light Detection and Ranging)?", "id": "Sensor Pemetaan Jarak Menggunakan Laser." },
  { "en": "Apa Itu SLAM (Simultaneous Localization and Mapping)?", "id": "Algoritma Robot (Memetakan Sambil Bergerak)." }



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
