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


  { "en": "Hukum Ohm?", "id": "Menghitung hubungan tegangan, arus, dan resistansi." },
  { "en": "Daya Listrik?", "id": "Menghitung daya dari tegangan dan arus." },
  { "en": "Daya Listrik (Resistif)?", "id": "Menghitung daya disipasi pada sebuah resistor." },
  { "en": "Daya Listrik (Bentuk Lain)?", "id": "Menghitung daya jika arus tidak diketahui." },
  { "en": "Energi Listrik?", "id": "Menghitung energi dari daya dan waktu." },
  { "en": "Hukum Arus Kirchhoff (KCL)?", "id": "Total arus pada percabangan adalah nol." },
  { "en": "Hukum Tegangan Kirchhoff (KVL)?", "id": "Total tegangan dalam loop tertutup nol." },
  { "en": "Resistor Seri?", "id": "Menjumlahkan total resistansi dalam rangkaian seri." },
  { "en": "Resistor Paralel?", "id": "Menghitung total resistansi dalam rangkaian paralel." },
  { "en": "Kapasitor Seri?", "id": "Menghitung total kapasitansi dalam rangkaian seri." },
  { "en": "Kapasitor Paralel?", "id": "Menjumlahkan total kapasitansi rangkaian paralel." },
  { "en": "Induktor Seri?", "id": "Menjumlahkan total induktansi rangkaian seri." },
  { "en": "Induktor Paralel?", "id": "Menghitung total induktansi rangkaian paralel." },
  { "en": "Muatan Kapasitor?", "id": "Menghitung muatan yang tersimpan dalam kapasitor." },
  { "en": "Arus Kapasitor?", "id": "Menghitung arus yang mengalir melalui kapasitor." },
  { "en": "Tegangan Induktor?", "id": "Menghitung tegangan pada sebuah induktor." },
  { "en": "Energi Kapasitor?", "id": "Menghitung energi yang tersimpan dalam kapasitor." },
  { "en": "Energi Induktor?", "id": "Menghitung energi yang tersimpan dalam induktor." },
  { "en": "Pembagi Tegangan?", "id": "Menghitung tegangan output pada pembagi resistif." },
  { "en": "Pembagi Arus?", "id": "Menghitung arus cabang pada pembagi arus." },
  { "en": "Resistivitas?", "id": "Menghitung resistansi dari sifat material." },
  { "en": "Konduktansi?", "id": "Ukuran kemudahan arus mengalir, kebalikan resistansi." },
  { "en": "Periode Gelombang?", "id": "Menghitung waktu untuk satu siklus gelombang." },
  { "en": "Frekuensi Sudut?", "id": "Menghitung frekuensi dalam radian per detik." },
  { "en": "Tegangan RMS (Root Mean Square)?", "id": "Menghitung nilai efektif tegangan sinusoidal." },
  { "en": "Arus RMS (Root Mean Square)?", "id": "Menghitung nilai efektif arus sinusoidal." },
  { "en": "Hukum Coulomb?", "id": "Menghitung gaya antara dua muatan listrik." },
  { "en": "Medan Listrik?", "id": "Menghitung kekuatan medan listrik pada muatan." },
  { "en": "Medan Listrik (Muatan Titik)?", "id": "Menghitung medan listrik dari muatan titik." },
  { "en": "Hukum Gauss?", "id": "Menghubungkan fluks listrik dengan muatan terlampir." },
  { "en": "Potensial Listrik?", "id": "Energi potensial listrik per satuan muatan." },
  { "en": "Potensial Listrik (Muatan Titik)?", "id": "Menghitung potensial dari suatu muatan titik." },
  { "en": "Kapasitansi Pelat Paralel?", "id": "Menghitung kapasitansi dari geometri pelat paralel." },
  { "en": "Hukum Ampere?", "id": "Menghubungkan medan magnet dengan arus terlampir." },
  { "en": "Gaya Lorentz?", "id": "Menghitung gaya pada muatan bergerak." },
  { "en": "Gaya Magnetik (Kawat)?", "id": "Menghitung gaya pada kawat berarus." },
  { "en": "Medan Magnet (Kawat Lurus)?", "id": "Menghitung medan magnet di sekitar kawat." },
  { "en": "Medan Magnet (Solenoida)?", "id": "Menghitung medan magnet di dalam solenoida." },
  { "en": "Hukum Faraday (Induksi)?", "id": "Menghitung GGL induksi dari perubahan fluks." },
  { "en": "Fluks Magnetik?", "id": "Jumlah garis medan magnet melalui area." },
  { "en": "Induktansi Solenoida?", "id": "Menghitung induktansi dari geometri solenoida." },
  { "en": "Konstanta Waktu RC?", "id": "Waktu pengisian kapasitor sebesar 63.2%." },
  { "en": "Konstanta Waktu RL?", "id": "Waktu arus mencapai 63.2% nilai maksimum." },
  { "en": "Tegangan Kapasitor (Charging)?", "id": "Menghitung tegangan kapasitor saat pengisian." },
  { "en": "Tegangan Kapasitor (Discharging)?", "id": "Menghitung tegangan kapasitor saat pengosongan." },
  { "en": "Arus Induktor (Charging)?", "id": "Menghitung arus induktor saat pengisian." },
  { "en": "Arus Induktor (Discharging)?", "id": "Menghitung arus induktor saat pengosongan." },
  { "en": "Efisiensi?", "id": "Rasio daya output terhadap daya input." },
  { "en": "Faktor Puncak (Crest Factor)?", "id": "Rasio nilai puncak terhadap nilai RMS." },
  { "en": "Faktor Bentuk (Form Factor)?", "id": "Rasio nilai RMS terhadap nilai rata-rata." },
  { "en": "Tegangan Rata-rata (Rectified Sine)?", "id": "Nilai DC dari gelombang sinus disearahkan." },
  { "en": "Rapat Arus?", "id": "Besar arus listrik per satuan luas." },
  { "en": "Mobilitas Elektron?", "id": "Ukuran kecepatan drift elektron dalam medan." },
  { "en": "Kecepatan Drift?", "id": "Menghubungkan arus dengan kecepatan rata-rata elektron." },
  { "en": "Hukum Ohm (Mikroskopik)?", "id": "Menghubungkan rapat arus dengan medan listrik." },
  { "en": "Konduktivitas?", "id": "Ukuran kemampuan material menghantarkan arus." },
  { "en": "Transformasi Bintang-Delta (Y-D)?", "id": "Mengubah konfigurasi resistor bintang ke delta." },
  { "en": "Transformasi Delta-Bintang (D-Y)?", "id": "Mengubah konfigurasi resistor delta ke bintang." },
  { "en": "Teorema Thevenin (Tegangan)?", "id": "Tegangan rangkaian terbuka pada terminal." },
  { "en": "Teorema Thevenin (Resistansi)?", "id": "Resistansi ekuivalen dilihat dari terminal." },
  { "en": "Teorema Norton (Arus)?", "id": "Arus hubung singkat pada terminal." },
  { "en": "Teorema Norton (Resistansi)?", "id": "Resistansi ekuivalen sama dengan resistansi Thevenin." },
  { "en": "Transfer Daya Maksimum?", "id": "Syarat daya maksimum dikirim ke beban." },
  { "en": "Daya Maksimum?", "id": "Menghitung daya maksimum yang dapat dikirim." },
  { "en": "Arus Listrik (Definisi)?", "id": "Laju aliran muatan listrik." },
  { "en": "Tegangan (Definisi)?", "id": "Kerja yang dilakukan per satuan muatan." },
  { "en": "Frekuensi Resonansi RLC Seri?", "id": "Frekuensi saat reaktansi induktif dan kapasitif sama." },
  { "en": "Impedansi (Umum)?", "id": "Hambatan total pada rangkaian AC." },
  { "en": "Reaktansi Kapasitif?", "id": "Hambatan kapasitor terhadap arus AC." },
  { "en": "Reaktansi Induktif?", "id": "Hambatan induktor terhadap arus AC." },
  { "en": "Impedansi RLC Seri?", "id": "Menghitung impedansi total rangkaian RLC seri." },
  { "en": "Hukum Ohm untuk AC?", "id": "Menghitung hubungan tegangan, arus, impedansi AC." },
  { "en": "Admitansi?", "id": "Kebalikan dari impedansi." },
  { "en": "Susceptansi?", "id": "Bagian imajiner dari admitansi." },
  { "en": "Daya Nyata (Real Power)?", "id": "Daya yang benar-benar digunakan oleh beban." },
  { "en": "Daya Reaktif (Reactive Power)?", "id": "Daya untuk medan magnet atau listrik." },
  { "en": "Daya Semu (Apparent Power)?", "id": "Daya total dalam sirkuit AC." },
  { "en": "Segitiga Daya?", "id": "Hubungan antara daya nyata, reaktif, semu." },
  { "en": "Faktor Daya (Power Factor)?", "id": "Rasio daya nyata terhadap daya semu." },
  { "en": "Arus Dioda Shockley?", "id": "Model arus-tegangan ideal dari dioda." },
  { "en": "Tegangan Termal?", "id": "Tegangan terkait energi termal pembawa muatan." },
  { "en": "Gain Arus BJT (Common-Emitter)?", "id": "Rasio arus kolektor terhadap arus basis." },
  { "en": "Gain Arus BJT (Common-Base)?", "id": "Rasio arus kolektor terhadap arus emitor." },
  { "en": "Hubungan Alpha-Beta?", "id": "Mengonversi antara gain arus BJT." },
  { "en": "Arus Emitor BJT?", "id": "Aplikasi KCL pada terminal transistor BJT." },
  { "en": "Transkonduktansi MOSFET?", "id": "Ukuran penguatan dari MOSFET." },
  { "en": "Arus Drain MOSFET (Triode)?", "id": "Menghitung arus drain di daerah triode." },
  { "en": "Arus Drain MOSFET (Saturasi)?", "id": "Menghitung arus drain di daerah saturasi." },
  { "en": "Gain Tegangan Op-Amp (Inverting)?", "id": "Menghitung penguatan op-amp konfigurasi inverting." },
  { "en": "Gain Tegangan Op-Amp (Non-inverting)?", "id": "Menghitung penguatan op-amp non-inverting." },
  { "en": "Slew Rate?", "id": "Laju perubahan tegangan output maksimum." },
  { "en": "Gain-Bandwidth Product?", "id": "Hasil kali gain dengan bandwidth konstan." },
  { "en": "Common-Mode Rejection Ratio (CMRR)?", "id": "Kemampuan menolak sinyal common-mode." },
  { "en": "Hukum De Morgan (NAND)?", "id": "Aturan dasar dalam aljabar Boolean." },
  { "en": "Hukum De Morgan (NOR)?", "id": "Aturan dasar lain dalam aljabar Boolean." },
  { "en": "Tegangan Sinusoidal?", "id": "Menyatakan gelombang tegangan AC sesaat." },
  { "en": "Arus Sinusoidal?", "id": "Menyatakan gelombang arus AC sesaat." },
  { "en": "Impedansi Kapasitor?", "id": "Hambatan kapasitor terhadap arus bolak-balik (AC)." },
  { "en": "Impedansi Induktor?", "id": "Hambatan induktor terhadap arus bolak-balik (AC)." },
  { "en": "Impedansi Resistor?", "id": "Resistansi murni tidak memiliki komponen imajiner." },
  { "en": "Representasi Phasor (Polar)?", "id": "Menyatakan besaran AC dengan magnitudo dan fasa." },
  { "en": "Representasi Phasor (Rectangular)?", "id": "Menyatakan besaran AC dengan komponen real-imajiner." },
  { "en": "Konversi Rectangular ke Polar (Magnitudo)?", "id": "Menghitung magnitudo dari bentuk rektangular." },
  { "en": "Konversi Rectangular ke Polar (Fasa)?", "id": "Menghitung sudut fasa dari bentuk rektangular." },
  { "en": "Konversi Polar ke Rectangular (Real)?", "id": "Menghitung komponen real dari bentuk polar." },
  { "en": "Konversi Polar ke Rectangular (Imaginer)?", "id": "Menghitung komponen imajiner dari bentuk polar." },
  { "en": "Identitas Euler?", "id": "Menghubungkan eksponensial kompleks dengan fungsi trigonometri." },
  { "en": "Impedansi Seri?", "id": "Menjumlahkan impedansi dalam rangkaian seri." },
  { "en": "Impedansi Paralel?", "id": "Menghitung total impedansi dalam rangkaian paralel." },
  { "en": "Sudut Fasa Impedansi?", "id": "Menentukan beda fasa antara tegangan dan arus." },
  { "en": "Daya Kompleks?", "id": "Menggabungkan daya nyata dan reaktif." },
  { "en": "Daya Kompleks (Phasor)?", "id": "Menghitung daya kompleks dari fasor V I." },
  { "en": "Magnitudo Daya Semu?", "id": "Menghitung besarnya daya semu (apparent power)." },
  { "en": "Faktor Daya Lagging?", "id": "Menandakan beban bersifat induktif." },
  { "en": "Faktor Daya Leading?", "id": "Menandakan beban bersifat kapasitif." },
  { "en": "Koreksi Faktor Daya (Kapasitor)?", "id": "Menghitung daya reaktif kapasitor untuk perbaikan." },
  { "en": "Admitansi Paralel?", "id": "Menjumlahkan admitansi dalam rangkaian paralel." },
  { "en": "Konduktansi (AC)?", "id": "Komponen real dari admitansi." },
  { "en": "Susceptansi (AC)?", "id": "Komponen imajiner dari admitansi." },
  { "en": "Frekuensi Resonansi RLC Paralel?", "id": "Frekuensi saat admitansi rangkaian minimum." },
  { "en": "Faktor Kualitas (Q Factor) RLC Seri?", "id": "Ukuran selektivitas frekuensi rangkaian resonansi." },
  { "en": "Faktor Kualitas (Q Factor) Seri Bentuk Lain?", "id": "Bentuk lain rumus Q Factor seri." },
  { "en": "Faktor Kualitas (Q Factor) RLC Paralel?", "id": "Ukuran selektivitas frekuensi resonansi paralel." },
  { "en": "Bandwidth (BW)?", "id": "Rentang frekuensi dimana daya setengah maksimum." },
  { "en": "Frekuensi Cutoff (Low Pass RC)?", "id": "Frekuensi saat daya output setengah." },
  { "en": "Frekuensi Cutoff (High Pass RC)?", "id": "Frekuensi saat daya output setengah." },
  { "en": "Frekuensi Cutoff (Low Pass RL)?", "id": "Frekuensi saat daya output setengah." },
  { "en": "Frekuensi Cutoff (High Pass RL)?", "id": "Frekuensi saat daya output setengah." },
  { "en": "Fungsi Transfer (Umum)?", "id": "Rasio output terhadap input di domain frekuensi." },
  { "en": "Nilai Rata-rata (Average Value) Gelombang Penuh?", "id": "Nilai rata-rata gelombang AC murni nol." },
  { "en": "Nilai Rata-rata (Average Value) Setengah Gelombang?", "id": "Nilai DC dari sinus setengah gelombang." },
  { "en": "Daya Rata-rata?", "id": "Daya nyata yang dikonsumsi selama satu periode." },
  { "en": "Daya Rata-rata (Resistor)?", "id": "Daya rata-rata yang didisipasikan oleh resistor." },
  { "en": "Daya Sesaat?", "id": "Daya pada suatu waktu tertentu." },
  { "en": "Daya Rata-rata (Umum)?", "id": "Menghitung daya nyata dari magnitudo puncak." },
  { "en": "Tegangan Fasa (Hubungan Y)?", "id": "Hubungan tegangan fasa dan line (Y)." },
  { "en": "Arus Fasa (Hubungan Y)?", "id": "Arus line sama dengan arus fasa (Y)." },
  { "en": "Tegangan Fasa (Hubungan Delta)?", "id": "Tegangan line sama dengan tegangan fasa (Delta)." },
  { "en": "Arus Fasa (Hubungan Delta)?", "id": "Hubungan arus line dan fasa (Delta)." },
  { "en": "Daya Total Tiga Fasa?", "id": "Menghitung daya total dari parameter fasa." },
  { "en": "Daya Total Tiga Fasa (Line)?", "id": "Menghitung daya total dari parameter line." },
  { "en": "Rasio Transformator (Tegangan)?", "id": "Hubungan tegangan dengan jumlah lilitan." },
  { "en": "Rasio Transformator (Arus)?", "id": "Hubungan arus dengan jumlah lilitan." },
  { "en": "Impedansi Refleksi Transformator?", "id": "Impedansi beban dilihat dari sisi primer." },
  { "en": "Transformator Ideal?", "id": "Daya input sama dengan daya output." },
  { "en": "Kopling Mutual?", "id": "Tegangan induksi pada satu koil." },
  { "en": "Koefisien Kopling?", "id": "Ukuran seberapa erat dua induktor terhubung." },
  { "en": "Energi Tersimpan (Kopling Mutual)?", "id": "Total energi dalam induktor terkopling." },
  { "en": "Kondisi Resonansi?", "id": "Kondisi saat reaktansi induktif dan kapasitif sama." },
  { "en": "Impedansi Karakteristik?", "id": "Impedansi saluran transmisi tanpa rugi." },
  { "en": "Konstanta Propagasi?", "id": "Menentukan atenuasi dan pergeseran fasa gelombang." },
  { "en": "Konstanta Atenuasi?", "id": "Menentukan pelemahan amplitudo gelombang." },
  { "en": "Konstanta Fasa?", "id": "Menentukan pergeseran fasa per satuan panjang." },
  { "en": "Panjang Gelombang?", "id": "Jarak spasial satu siklus gelombang." },
  { "en": "Kecepatan Propagasi?", "id": "Kecepatan rambat gelombang di media." },
  { "en": "Koefisien Refleksi?", "id": "Rasio gelombang pantul terhadap gelombang datang." },
  { "en": "VSWR (Voltage Standing Wave Ratio)?", "id": "Rasio tegangan maksimum terhadap minimum." },
  { "en": "Impedansi Input Saluran Transmisi?", "id": "Impedansi dilihat dari input saluran." },
  { "en": "Skin Depth?", "id": "Kedalaman penetrasi arus AC dalam konduktor." },
  { "en": "Hukum Maxwell 1 (Gauss Listrik)?", "id": "Sumber medan listrik adalah muatan." },
  { "en": "Hukum Maxwell 2 (Gauss Magnet)?", "id": "Tidak ada sumber tunggal medan magnet." },
  { "en": "Hukum Maxwell 3 (Faraday)?", "id": "Medan magnet berubah menghasilkan medan listrik." },
  { "en": "Hukum Maxwell 4 (Ampere-Maxwell)?", "id": "Arus dan medan listrik berubah hasilkan magnet." },
  { "en": "Vektor Poynting?", "id": "Menyatakan arah dan magnitudo aliran energi EM." },
  { "en": "Impedansi Intrinsik?", "id": "Rasio medan listrik terhadap medan magnet." },
  { "en": "Hubungan D dan E?", "id": "Hubungan fluks listrik dan medan listrik." },
  { "en": "Hubungan B dan H?", "id": "Hubungan fluks magnet dan medan magnet." },
  { "en": "Permitivitas?", "id": "Kemampuan material membentuk medan listrik." },
  { "en": "Permeabilitas?", "id": "Kemampuan material membentuk medan magnet." },
  { "en": "Kecepatan Cahaya?", "id": "Kecepatan gelombang EM di ruang hampa." },
  { "en": "Persamaan Gelombang?", "id": "Menggambarkan propagasi gelombang elektromagnetik." },
  { "en": "Persamaan Kontinuitas Arus?", "id": "Menyatakan kekekalan muatan listrik." },
  { "en": "Arus Konduksi?", "id": "Arus yang disebabkan oleh gerakan muatan." },
  { "en": "Arus Perpindahan (Displacement)?", "id": "Arus akibat perubahan fluks listrik." },
  { "en": "Kapasitansi (Definisi Umum)?", "id": "Rasio muatan terhadap beda potensial." },
  { "en": "Induktansi (Definisi Umum)?", "id": "Rasio fluks magnetik terhadap arus." },
  { "en": "Waktu Relaksasi?", "id": "Waktu muatan bebas untuk meluruh." },
  { "en": "Kondisi Batas (E Tangensial)?", "id": "Komponen tangensial E kontinu di batas." },
  { "en": "Kondisi Batas (H Tangensial)?", "id": "Komponen tangensial H diskontinu oleh arus." },
  { "en": "Kondisi Batas (D Normal)?", "id": "Komponen normal D diskontinu oleh muatan." },
  { "en": "Kondisi Batas (B Normal)?", "id": "Komponen normal B selalu kontinu." },
  { "en": "Persamaan Laplace?", "id": "Menentukan potensial di daerah bebas muatan." },
  { "en": "Persamaan Poisson?", "id": "Menentukan potensial di daerah bermuatan." },
  { "en": "Uniqueness Theorem?", "id": "Solusi persamaan Laplace/Poisson adalah unik." },
  { "en": "Energi Medan Listrik?", "id": "Energi yang tersimpan dalam medan listrik." },
  { "en": "Energi Medan Magnet?", "id": "Energi yang tersimpan dalam medan magnet." },
  { "en": "Potensial Vektor Magnetik?", "id": "Mendefinisikan medan magnet dari potensial vektor." },
  { "en": "Teorema Divergensi?", "id": "Menghubungkan integral volume dan permukaan." },
  { "en": "Teorema Stokes?", "id": "Menghubungkan integral permukaan dan garis." },
  { "en": "Definisi Gradien?", "id": "Vektor yang menunjuk arah kenaikan maksimum." },
  { "en": "Definisi Divergensi?", "id": "Ukuran aliran sumber vektor keluar." },
  { "en": "Definisi Curl?", "id": "Ukuran sirkulasi atau rotasi vektor." },
  { "en": "Polarisasi Linear?", "id": "Medan listrik berosilasi pada satu garis." },
  { "en": "Polarisasi Melingkar?", "id": "Ujung vektor medan listrik membentuk lingkaran." },
  { "en": "Polarisasi Elips?", "id": "Ujung vektor medan listrik membentuk elips." },
  { "en": "Rasio Aksial (AR)?", "id": "Ukuran 'kebulatan' dari polarisasi elips." },
  { "en": "Hukum Snell?", "id": "Menentukan pembiasan gelombang di antara dua media." },
  { "en": "Indeks Refraksi?", "id": "Rasio kecepatan cahaya vakum terhadap di media." },
  { "en": "Frekuensi Kritis?", "id": "Frekuensi maksimum yang dipantulkan oleh ionosfer." },
  { "en": "Sudut Brewster (Polarisasi Paralel)?", "id": "Sudut datang tanpa pantulan (polarisasi paralel)." },
  { "en": "Koefisien Refleksi (Tegak Lurus)?", "id": "Rasio amplitudo pantul-datang untuk polarisasi tegak lurus." },
  { "en": "Koefisien Transmisi?", "id": "Rasio amplitudo transmisi-datang." },
  { "en": "Loss Tangent?", "id": "Ukuran rugi-rugi dielektrik pada material." },
  { "en": "Kondisi Konduktor Baik?", "id": "Kondisi dimana material bersifat konduktif." },
  { "en": "Kondisi Dielektrik Baik?", "id": "Kondisi dimana material bersifat isolator." },
  { "en": "Frekuensi Cutoff Waveguide?", "id": "Frekuensi terendah yang dapat merambat." },
  { "en": "Panjang Gelombang Waveguide?", "id": "Panjang gelombang di dalam pemandu gelombang." },
  { "en": "Impedansi Gelombang (TE)?", "id": "Impedansi untuk mode Transverse Electric (TE)." },
  { "en": "Impedansi Gelombang (TM)?", "id": "Impedansi untuk mode Transverse Magnetic (TM)." },
  { "en": "Mode Dominan?", "id": "Mode dengan frekuensi cutoff terendah." },
  { "en": "Daya Rata-rata (Vektor Poynting)?", "id": "Menghitung aliran daya rata-rata gelombang EM." },
  { "en": "Panjang Antena Dipol Setengah Gelombang?", "id": "Panjang fisik antena dipol standar." },
  { "en": "Panjang Antena Monopol Seperempat Gelombang?", "id": "Panjang fisik antena monopol standar." },
  { "en": "Directivity Antena?", "id": "Kemampuan antena memfokuskan energi ke satu arah." },
  { "en": "Gain Antena?", "id": "Directivity yang memperhitungkan efisiensi antena." },
  { "en": "Efisiensi Radiasi?", "id": "Rasio daya teradiasi terhadap daya input." },
  { "en": "Aperture Efektif?", "id": "Area efektif antena dalam menerima daya." },
  { "en": "Rumus Transmisi Friis?", "id": "Menghitung daya terima dalam ruang bebas." },
  { "en": "Path Loss (Ruang Bebas)?", "id": "Pelemahan sinyal karena jarak dan frekuensi." },
  { "en": "Radar Range Equation?", "id": "Menghubungkan daya terima radar dengan parameter target." },
  { "en": "Penampang Radar (RCS)?", "id": "Ukuran seberapa besar target memantulkan sinyal." },
  { "en": "Definisi Polarisasi Gelombang?", "id": "Orientasi osilasi medan listrik gelombang EM." },
  { "en": "Intensitas Radiasi?", "id": "Daya teradiasi per satuan sudut ruang." },
  { "en": "Resistansi Radiasi?", "id": "Resistansi ekuivalen yang merepresentasikan daya radiasi." },
  { "en": "Teorema Resiprositas?", "id": "Sifat transmisi dan terima antena sama." },
  { "en": "Beamwidth Antena?", "id": "Lebar sudut dari lobe utama antena." },
  { "en": "Half-Power Beamwidth (HPBW)?", "id": "Lebar sudut saat daya turun setengah." },
  { "en": "Side Lobe Level (SLL)?", "id": "Rasio intensitas lobe utama dan samping." },
  { "en": "Front-to-Back Ratio?", "id": "Rasio gain arah depan dan belakang." },
  { "en": "Panjang Listrik Antena?", "id": "Panjang antena dalam satuan panjang gelombang." },
  { "en": "Efek Doppler (Frekuensi)?", "id": "Perubahan frekuensi akibat gerakan relatif." },
  { "en": "Hukum Biot-Savart?", "id": "Menghitung medan magnet dari elemen arus." },
  { "en": "Kapasitansi Saluran Koaksial?", "id": "Kapasitansi per satuan panjang kabel koaksial." },
  { "en": "Induktansi Saluran Koaksial?", "id": "Induktansi per satuan panjang kabel koaksial." },
  { "en": "Persamaan Telegrafer (Tegangan)?", "id": "Persamaan diferensial tegangan pada saluran transmisi." },
  { "en": "Persamaan Telegrafer (Arus)?", "id": "Persamaan diferensial arus pada saluran transmisi." },
  { "en": "Smith Chart?", "id": "Alat grafis untuk analisis saluran transmisi." },
  { "en": "Transformator Lambda/4?", "id": "Mencocokkan impedansi dengan saluran seperempat gelombang." },
  { "en": "Single-Stub Matching?", "id": "Mencocokkan impedansi menggunakan satu stub." },
  { "en": "Resonator Rongga (Cavity)?", "id": "Frekuensi resonansi dari rongga logam tertutup." },
  { "en": "Momen Dipol Listrik?", "id": "Ukuran pemisahan muatan positif dan negatif." },
  { "en": "Potensial dari Dipol?", "id": "Potensial listrik yang dihasilkan oleh dipol." },
  { "en": "Torsi pada Dipol Listrik?", "id": "Torsi yang dialami dipol dalam medan listrik." },
  { "en": "Momen Dipol Magnetik?", "id": "Ukuran kekuatan sumber magnetik." },
  { "en": "Torsi pada Dipol Magnetik?", "id": "Torsi yang dialami loop arus." },
  { "en": "Magnetisasi?", "id": "Kerapatan momen dipol magnetik dalam material." },
  { "en": "Suseptibilitas Magnetik?", "id": "Ukuran seberapa magnetik suatu material." },
  { "en": "Polarisasi Dielektrik?", "id": "Kerapatan momen dipol listrik dalam dielektrik." },
  { "en": "Arus Batas (Bound Current)?", "id": "Arus ekuivalen akibat magnetisasi material." },
  { "en": "Muatan Batas (Bound Charge)?", "id": "Muatan ekuivalen akibat polarisasi dielektrik." },
  { "en": "Reluktansi Magnetik?", "id": "Hambatan terhadap pembentukan fluks magnetik." },
  { "en": "Hukum Hopkinson?", "id": "Analogi hukum Ohm untuk rangkaian magnetik." },
  { "en": "Magnetomotive Force (MMF)?", "id": "Gaya gerak magnetik dari lilitan." },
  { "en": "Persamaan London (Superkonduktor)?", "id": "Hubungan medan listrik dan arus superkonduktor." },
  { "en": "Kedalaman Penetrasi London?", "id": "Jarak medan magnet menembus superkonduktor." },
  { "en": "Efek Hall?", "id": "Tegangan yang timbul melintasi konduktor." },
  { "en": "Hukum Curie (Paramagnetik)?", "id": "Magnetisasi berbanding terbalik dengan suhu." },
  { "en": "Siklus Histeresis?", "id": "Kurva B-H untuk material feromagnetik." },
  { "en": "Retentivity?", "id": "Sisa magnetisasi setelah medan dihilangkan." },
  { "en": "Coercivity?", "id": "Kekuatan medan untuk menghilangkan sisa magnetisasi." },
  { "en": "Arus Eddy (Eddy Current)?", "id": "Arus induksi dalam konduktor masif." },
  { "en": "Rugi Histeresis?", "id": "Energi yang hilang dalam siklus histeresis." },
  { "en": "Rugi Arus Eddy?", "id": "Energi hilang akibat arus eddy." },
  { "en": "Angka Gelombang (Wavenumber)?", "id": "Frekuensi spasial dari suatu gelombang." },
  { "en": "Dispersi?", "id": "Fenomena kecepatan fasa bergantung pada frekuensi." },
  { "en": "Hubungan Dispersi?", "id": "Hubungan antara frekuensi sudut dan angka gelombang." },
  { "en": "Kecepatan Grup?", "id": "Kecepatan rambat dari envelope gelombang." },
  { "en": "Media Anisotropik?", "id": "Sifat material bergantung pada arah." },
  { "en": "Tensor Permitivitas?", "id": "Merepresentasikan permitivitas pada media anisotropik." },
  { "en": "Efek Faraday?", "id": "Rotasi polarisasi akibat medan magnet." },
  { "en": "Birefringence?", "id": "Indeks refraksi bergantung pada polarisasi." },
  { "en": "Potensial Liénard-Wiechert?", "id": "Potensial EM dari muatan titik bergerak." },
  { "en": "Radiasi Sinkrotron?", "id": "Radiasi EM dari muatan relativistik dipercepat." },
  { "en": "Radiasi Cherenkov?", "id": "Radiasi EM saat partikel lewati medium." },
  { "en": "Array Factor (AF)?", "id": "Pola radiasi dari susunan antena." },
  { "en": "Broadside Array?", "id": "Radiasi maksimum tegak lurus sumbu array." },
  { "en": "End-fire Array?", "id": "Radiasi maksimum sepanjang sumbu array." },
  { "en": "Phased Array?", "id": "Array antena yang arahnya dikontrol elektronik." },
  { "en": "Hukum Kekekalan Energi (EM)?", "id": "Laju energi keluar sama dengan penurunan energi." },
  { "en": "Tekanan Radiasi?", "id": "Tekanan yang diberikan oleh gelombang EM." },
  { "en": "Dualitas Gelombang-Partikel?", "id": "Cahaya bersifat gelombang dan partikel (foton)." },
  { "en": "Energi Foton?", "id": "Energi satu kuantum cahaya (foton)." },
  { "en": "Momentum Foton?", "id": "Momentum yang dibawa oleh satu foton." },
  { "en": "Konsentrasi Intrinsik?", "id": "Kepadatan pembawa muatan dalam semikonduktor murni." },
  { "en": "Hukum Aksi Massa?", "id": "Hasil kali konsentrasi elektron-hole konstan." },
  { "en": "Level Energi Fermi?", "id": "Level energi dengan probabilitas okupansi 50%." },
  { "en": "Distribusi Fermi-Dirac?", "id": "Probabilitas sebuah state energi diisi elektron." },
  { "en": "Celah Energi (Energy Gap)?", "id": "Energi pemisah pita valensi dan konduksi." },
  { "en": "Konsentrasi Elektron (Tipe-n)?", "id": "Konsentrasi elektron didominasi oleh atom donor." },
  { "en": "Konsentrasi Hole (Tipe-p)?", "id": "Konsentrasi hole didominasi oleh atom akseptor." },
  { "en": "Arus Drift?", "id": "Arus akibat medan listrik." },
  { "en": "Arus Difusi?", "id": "Arus akibat gradien konsentrasi." },
  { "en": "Arus Total?", "id": "Jumlah dari komponen arus drift dan difusi." },
  { "en": "Hubungan Einstein?", "id": "Menghubungkan konstanta difusi dan mobilitas." },
  { "en": "Tegangan Kontak (Built-in)?", "id": "Beda potensial pada sambungan p-n." },
  { "en": "Lebar Daerah Deplesi?", "id": "Lebar daerah miskin pembawa muatan." },
  { "en": "Kapasitansi Deplesi (Junction)?", "id": "Kapasitansi sambungan p-n akibat daerah deplesi." },
  { "en": "Kapasitansi Difusi?", "id": "Kapasitansi akibat injeksi pembawa muatan minoritas." },
  { "en": "Panjang Difusi Elektron?", "id": "Jarak rata-rata elektron berdifusi sebelum rekombinasi." },
  { "en": "Panjang Difusi Hole?", "id": "Jarak rata-rata hole berdifusi sebelum rekombinasi." },
  { "en": "Efisiensi Injeksi Emitor?", "id": "Ukuran efektivitas emitor menginjeksi elektron." },
  { "en": "Faktor Transport Basis?", "id": "Fraksi elektron yang berhasil melintasi basis." },
  { "en": "Model Ebers-Moll (BJT)?", "id": "Model fisika arus pada transistor BJT." },
  { "en": "Tegangan Early?", "id": "Parameter yang memodelkan modulasi lebar basis." },
  { "en": "Resistansi Output (Efek Early)?", "id": "Resistansi output BJT karena efek Early." },
  { "en": "Frekuensi Transisi?", "id": "Frekuensi saat gain arus BJT jadi 1." },
  { "en": "Muatan di Inversi (MOSFET)?", "id": "Muatan pada kanal MOSFET saat ON." },
  { "en": "Kapasitansi Oksida?", "id": "Kapasitansi per satuan luas gerbang oksida." },
  { "en": "Tegangan Threshold (MOSFET)?", "id": "Tegangan gerbang minimum untuk membentuk kanal." },
  { "en": "Modulasi Panjang Kanal?", "id": "Parameter yang memodelkan perubahan panjang kanal." },
  { "en": "Resistansi Output (MOSFET)?", "id": "Resistansi output karena modulasi panjang kanal." },
  { "en": "Efek Body?", "id": "Perubahan V_t akibat tegangan substrat." },
  { "en": "Model Hybrid-Pi (BJT)?", "id": "Model sinyal-kecil untuk transistor BJT." },
  { "en": "Resistansi Input Pi?", "id": "Resistansi input pada model Hybrid-Pi." },
  { "en": "Transkonduktansi (BJT)?", "id": "Ukuran penguatan BJT (arus/tegangan)." },
  { "en": "Kapasitansi Miller?", "id": "Efek amplifikasi kapasitansi umpan balik." },
  { "en": "Efek Miller?", "id": "Meningkatnya kapasitansi input ekuivalen." },
  { "en": "Dioda Zener?", "id": "Dioda yang bekerja pada daerah breakdown." },
  { "en": "Dioda Varactor?", "id": "Dioda yang kapasitansinya dikontrol oleh tegangan." },
  { "en": "Dioda Tunnel?", "id": "Dioda yang menunjukkan resistansi negatif." },
  { "en": "Dioda Schottky?", "id": "Sambungan metal-semikonduktor dengan tegangan ON rendah." },
  { "en": "LED (Light Emitting Diode)?", "id": "Dioda yang memancarkan cahaya saat diberi bias maju." },
  { "en": "Fotodioda?", "id": "Dioda yang menghasilkan arus saat terkena cahaya." },
  { "en": "Persamaan Sel Surya?", "id": "Arus-tegangan sel surya di bawah iluminasi." },
  { "en": "Faktor Pengisian (Fill Factor)?", "id": "Ukuran kualitas dari sebuah sel surya." },
  { "en": "Tegangan Rangkaian Terbuka (Voc)?", "id": "Tegangan maksimum dari sel surya." },
  { "en": "Arus Hubung Singkat (Isc)?", "id": "Arus maksimum dari sel surya." },
  { "en": "SCR (Silicon Controlled Rectifier)?", "id": "Piranti thyristor untuk kontrol daya." },
  { "en": "TRIAC (Triode for Alternating Current)?", "id": "Thyristor dua arah untuk kontrol daya AC." },
  { "en": "JFET (Junction Field-Effect Transistor)?", "id": "Transistor efek medan dengan gerbang sambungan p-n." },
  { "en": "Persamaan Shockley (JFET)?", "id": "Arus-tegangan untuk JFET." },
  { "en": "Tegangan Pinch-off (Vp)?", "id": "Tegangan saat kanal JFET tertutup." },
  { "en": "Generasi Pembawa Muatan?", "id": "Penciptaan pasangan elektron-hole (misal oleh cahaya)." },
  { "en": "Rekombinasi Pembawa Muatan?", "id": "Lenyapnya pasangan elektron-hole." },
  { "en": "Waktu Hidup (Lifetime)?", "id": "Waktu rata-rata pembawa muatan ada." },
  { "en": "Level Fermi Intrinsik?", "id": "Level Fermi untuk semikonduktor murni." },
  { "en": "Netralitas Muatan?", "id": "Kondisi muatan total dalam semikonduktor nol." },
  { "en": "Persamaan Kontinuitas (Elektron)?", "id": "Menyatakan laju perubahan konsentrasi elektron." },
  { "en": "Persamaan Kontinuitas (Hole)?", "id": "Menyatakan laju perubahan konsentrasi hole." },
  { "en": "Kondisi Kesetimbangan Termal?", "id": "Tidak ada gaya eksternal (medan, cahaya)." },
  { "en": "Kondisi Steady-State?", "id": "Kondisi tidak berubah terhadap waktu." },
  { "en": "Efek Avalanche Breakdown?", "id": "Ioniasi tumbukan pada medan listrik tinggi." },
  { "en": "Efek Zener Breakdown?", "id": "Quantum tunneling pada medan listrik sangat tinggi." },
  { "en": "Hukum Child-Langmuir?", "id": "Membatasi arus dalam dioda vakum." },
  { "en": "Emisi Termionik?", "id": "Pancaran elektron dari permukaan panas." },
  { "en": "Fungsi Kerja (Work Function)?", "id": "Energi minimum untuk melepaskan elektron." },
  { "en": "Resistansi Lembaran (Sheet Resistance)?", "id": "Resistansi lapisan tipis dengan bentuk bujursangkar." },
  { "en": "Efek Gunn?", "id": "Menghasilkan osilasi gelombang mikro dalam semikonduktor." },
  { "en": "Efek Piezoelektrik?", "id": "Tegangan mekanik menghasilkan potensial listrik." },
  { "en": "Efek Piroelektrik?", "id": "Perubahan suhu menghasilkan potensial listrik." },
  { "en": "Efek Termoelektrik (Seebeck)?", "id": "Beda suhu menghasilkan tegangan." },
  { "en": "Efek Peltier?", "id": "Arus listrik menghasilkan transfer panas." },
  { "en": "Efek Thomson?", "id": "Pemanasan/pendinginan konduktor berarus dengan gradien suhu." },
  { "en": "Noise Termal (Johnson-Nyquist)?", "id": "Noise akibat agitasi termal pembawa muatan." },
  { "en": "Noise Tembak (Shot Noise)?", "id": "Noise akibat sifat diskrit muatan listrik." },
  { "en": "Noise Kedip (Flicker Noise)?", "id": "Noise dominan pada frekuensi rendah." },
  { "en": "Angka Bising (Noise Figure)?", "id": "Ukuran degradasi sinyal-ke-noise oleh perangkat." },
  { "en": "Suhu Bising (Noise Temperature)?", "id": "Representasi noise internal sebagai suhu." },
  { "en": "Rumus Friis (Noise)?", "id": "Menghitung total noise figure sistem kaskade." },
  { "en": "Kerapatan Keadaan (Density of States)?", "id": "Jumlah state energi yang tersedia per volume." },
  { "en": "Massa Efektif?", "id": "Massa partikel dalam kristal." },
  { "en": "Model Kronig-Penney?", "id": "Model potensial periodik untuk elektron." },
  { "en": "Teorema Bloch?", "id": "Bentuk fungsi gelombang elektron dalam kristal." },
  { "en": "Zona Brillouin?", "id": "Sel primitif dalam ruang resiprokal." },
  { "en": "Struktur Pita Energi?", "id": "Hubungan antara energi dan momentum elektron." },
  { "en": "Semikonduktor Celah Pita Langsung?", "id": "Pita konduksi-valensi sejajar dalam momentum." },
  { "en": "Semikondutor Celah Pita Tak Langsung?", "id": "Pita konduksi-valensi tidak sejajar." },
  { "en": "Fonon?", "id": "Kuantum getaran kisi kristal." },
  { "en": "Hamburan (Scattering)?", "id": "Interaksi yang mengubah momentum pembawa muatan." },
  { "en": "Aturan Seleksi?", "id": "Menentukan transisi elektron yang diizinkan." },
  { "en": "Saturasi Kecepatan?", "id": "Kecepatan drift maksimum pada medan tinggi." },
  { "en": "Model SPICE?", "id": "Model piranti untuk simulasi rangkaian." },
  { "en": "Parameter Gummel-Poon?", "id": "Model fisika detail untuk BJT." },
  { "en": "Model BSIM?", "id": "Model standar industri untuk MOSFET." },
  { "en": "Hukum Scaling Dennard?", "id": "Aturan penskalaan dimensi transistor MOSFET." },
  { "en": "Hukum Moore?", "id": "Jumlah transistor pada chip berlipat ganda." },
  { "en": "Yield Manufaktur?", "id": "Persentase chip fungsional per wafer." },
  { "en": "Doping?", "id": "Menambahkan atom pengotor ke semikonduktor." },
  { "en": "Implantasi Ion?", "id": "Metode doping dengan menembakkan ion." },
  { "en": "Difusi (Manufaktur)?", "id": "Proses pergerakan dopan pada suhu tinggi." },
  { "en": "Litografi?", "id": "Proses transfer pola ke wafer semikonduktor." },
  { "en": "Etsa (Etching)?", "id": "Proses menghilangkan material secara selektif." },
  { "en": "Deposisi?", "id": "Proses penambahan lapisan material tipis." },
  { "en": "Identitas Idempoten (OR)?", "id": "Menjumlahkan variabel dengan dirinya sendiri." },
  { "en": "Identitas Idempoten (AND)?", "id": "Mengalikan variabel dengan dirinya sendiri." },
  { "en": "Identitas Involusi?", "id": "Negasi ganda mengembalikan nilai asli." },
  { "en": "Hukum Komutatif (OR)?", "id": "Urutan tidak penting dalam operasi ATAU." },
  { "en": "Hukum Komutatif (AND)?", "id": "Urutan tidak penting dalam operasi DAN." },
  { "en": "Hukum Asosiatif (OR)?", "id": "Pengelompokan tidak penting dalam operasi ATAU." },
  { "en": "Hukum Asosiatif (AND)?", "id": "Pengelompokan tidak penting dalam operasi DAN." },
  { "en": "Hukum Distributif (AND over OR)?", "id": "Menyebarkan operasi DAN ke dalam ATAU." },
  { "en": "Hukum Distributif (OR over AND)?", "id": "Menyebarkan operasi ATAU ke dalam DAN." },
  { "en": "Hukum Penyerapan (Absorpsi)?", "id": "Menyederhanakan ekspresi logika." },
  { "en": "Hukum Penyerapan (Bentuk Lain)?", "id": "Bentuk lain hukum absorpsi." },
  { "en": "Teorema Konsensus?", "id": "Menghilangkan suku redundan." },
  { "en": "Operasi XOR?", "id": "Output TINGGI jika input berbeda." },
  { "en": "Operasi XNOR?", "id": "Output TINGGI jika input sama." },
  { "en": "Bentuk Kanonik Sum-of-Products (SOP)?", "id": "Penjumlahan dari hasil perkalian (minterm)." },
  { "en": "Bentuk Kanonik Product-of-Sums (POS)?", "id": "Perkalian dari hasil penjumlahan (maxterm)." },
  { "en": "Minterm?", "id": "Suku DAN yang mencakup semua variabel." },
  { "en": "Maxterm?", "id": "Suku ATAU yang mencakup semua variabel." },
  { "en": "Karnaugh Map (K-Map)?", "id": "Metode grafis untuk penyederhanaan logika." },
  { "en": "Metode Quine-McCluskey?", "id": "Metode tabular untuk penyederhanaan logika." },
  { "en": "Gerbang NAND?", "id": "Gerbang universal, negasi dari AND." },
  { "en": "Gerbang NOR?", "id": "Gerbang universal, negasi dari OR." },
  { "en": "Fan-in?", "id": "Jumlah input maksimum sebuah gerbang." },
  { "en": "Fan-out?", "id": "Jumlah input yang bisa dikemudikan output." },
  { "en": "Propagation Delay?", "id": "Waktu tunda sinyal melewati gerbang." },
  { "en": "Noise Margin?", "id": "Toleransi level tegangan terhadap noise." },
  { "en": "Power Dissipation?", "id": "Daya yang dikonsumsi oleh gerbang logika." },
  { "en": "Half Adder (Sum)?", "id": "Menghitung bit jumlah dari dua input." },
  { "en": "Half Adder (Carry)?", "id": "Menghitung bit bawaan dari dua input." },
  { "en": "Full Adder (Sum)?", "id": "Menghitung bit jumlah dari tiga input." },
  { "en": "Full Adder (Carry)?", "id": "Menghitung bit bawaan dari tiga input." },
  { "en": "Multiplexer (MUX) 2-ke-1?", "id": "Memilih satu dari dua input data." },
  { "en": "Demultiplexer (DEMUX) 1-ke-2?", "id": "Mengarahkan satu input ke salah satu output." },
  { "en": "Decoder 2-ke-4?", "id": "Mengaktifkan satu dari empat output." },
  { "en": "Encoder 4-ke-2?", "id": "Menghasilkan kode biner dari input aktif." },
  { "en": "Fungsi Latch SR (Set)?", "id": "Memaksa output ke keadaan TINGGI." },
  { "en": "Fungsi Latch SR (Reset)?", "id": "Memaksa output ke keadaan RENDAH." },
  { "en": "Latch D?", "id": "Menyimpan nilai input D." },
  { "en": "Flip-Flop T (Toggle)?", "id": "Membalik keadaan output saat T=1." },
  { "en": "Flip-Flop JK?", "id": "Flip-flop universal yang serbaguna." },
  { "en": "Setup Time?", "id": "Waktu data harus stabil sebelum clock." },
  { "en": "Hold Time?", "id": "Waktu data harus stabil setelah clock." },
  { "en": "Clock-to-Q Delay?", "id": "Waktu tunda dari clock ke output." },
  { "en": "Register Geser (Shift Register)?", "id": "Menggeser data satu posisi per clock." },
  { "en": "Pencacah (Counter)?", "id": "Rangkaian sekuensial yang menghitung pulsa." },
  { "en": "State Diagram?", "id": "Representasi grafis dari mesin keadaan." },
  { "en": "State Table?", "id": "Representasi tabular dari mesin keadaan." },
  { "en": "Mesin Moore?", "id": "Output hanya bergantung pada keadaan saat ini." },
  { "en": "Mesin Mealy?", "id": "Output bergantung pada keadaan dan input." },
  { "en": "Hubungan Jumlah Keadaan?", "id": "Hubungan jumlah keadaan dan flip-flop." },
  { "en": "Representasi Biner?", "id": "Konversi dari biner ke desimal." },
  { "en": "Representasi Gray Code?", "id": "Kode dimana dua nilai berurutan beda 1 bit." },
  { "en": "Representasi BCD (Binary Coded Decimal)?", "id": "Mengkodenkan setiap digit desimal dengan 4 bit." },
  { "en": "Representasi Two's Complement?", "id": "Metode standar untuk merepresentasikan bilangan bertanda." },
  { "en": "Parity Bit?", "id": "Bit tambahan untuk deteksi error." },
  { "en": "Hamming Code?", "id": "Kode untuk deteksi dan koreksi error." },
  { "en": "Jarak Hamming?", "id": "Jumlah posisi bit yang berbeda." },
  { "en": "CRC (Cyclic Redundancy Check)?", "id": "Teknik deteksi error menggunakan pembagian polinomial." },
  { "en": "Checksum?", "id": "Metode sederhana untuk deteksi error." },
  { "en": "FPGA (Field-Programmable Gate Array)?", "id": "Sirkuit terintegrasi yang dapat diprogram." },
  { "en": "LUT (Look-Up Table)?", "id": "Blok pembangun dasar dalam FPGA." },
  { "en": "CPLD (Complex Programmable Logic Device)?", "id": "Perangkat logika terprogram yang lebih sederhana." },
  { "en": "HDL (Hardware Description Language)?", "id": "Bahasa untuk mendeskripsikan sirkuit digital." },
  { "en": "Contoh HDL?", "id": "VHDL dan Verilog." },
  { "en": "Sintesis Logika?", "id": "Proses mengubah HDL menjadi daftar gerbang." },
  { "en": "Static Timing Analysis (STA)?", "id": "Menganalisis timing sirkuit tanpa simulasi." },
  { "en": "Race Condition?", "id": "Output tak terduga akibat tunda sinyal." },
  { "en": "Hazard Statis?", "id": "Glitch sesaat saat input berubah." },
  { "en": "Hazard Dinamis?", "id": "Output berubah beberapa kali saat harusnya sekali." },
  { "en": "Sistem Bilangan Basis-R?", "id": "Representasi bilangan dengan basis (radix) R." },
  { "en": "Komplemen Radiks?", "id": "Metode untuk representasi bilangan negatif." },
  { "en": "Carry Lookahead Adder?", "id": "Penjumlah cepat dengan logika carry terprediksi." },
  { "en": "Syarat Propagate (Carry)?", "id": "Syarat carry merambat di penjumlah." },
  { "en": "Syarat Generate (Carry)?", "id": "Syarat carry dihasilkan di penjumlah." },
  { "en": "ROM (Read-Only Memory)?", "id": "Memori yang hanya bisa dibaca." },
  { "en": "RAM (Random-Access Memory)?", "id": "Memori yang bisa dibaca dan ditulis." },
  { "en": "SRAM (Static RAM)?", "id": "RAM berbasis flip-flop, cepat tapi mahal." },
  { "en": "DRAM (Dynamic RAM)?", "id": "RAM berbasis kapasitor, padat tapi perlu refresh." },
  { "en": "PAL (Programmable Array Logic)?", "id": "Perangkat logika dengan array AND terprogram." },
  { "en": "PLA (Programmable Logic Array)?", "id": "Perangkat logika dengan array AND-OR terprogram." },
  { "en": "Floating Point?", "id": "Representasi bilangan real (tanda, mantissa, eksponen)." },
  { "en": "Standar IEEE 754?", "id": "Standar umum untuk aritmetika floating-point." },
  { "en": "Gerbang Tri-state?", "id": "Memiliki keadaan ketiga: impedansi tinggi (Hi-Z)." },
  { "en": "Siklus Tugas (Duty Cycle)?", "id": "Persentase waktu sinyal dalam keadaan TINGGI." },
  { "en": "Astable Multivibrator?", "id": "Rangkaian yang menghasilkan gelombang kotak." },
  { "en": "Monostable Multivibrator?", "id": "Menghasilkan satu pulsa dengan lebar tertentu." },
  { "en": "Bistable Multivibrator?", "id": "Memiliki dua keadaan stabil (flip-flop)." },
  { "en": "Timer 555 (Astable Frequency)?", "id": "Menghitung frekuensi output timer 555." },
  { "en": "Timer 555 (Monostable Pulse Width)?", "id": "Menghitung lebar pulsa output timer 555." },
  { "en": "Keluarga Logika TTL (Transistor-Transistor Logic)?", "id": "Keluarga logika digital berbasis BJT." },
  { "en": "Keluarga Logika CMOS (Complementary MOS)?", "id": "Keluarga logika digital berbasis MOSFET." },
  { "en": "Keluarga Logika ECL (Emitter-Coupled Logic)?", "id": "Keluarga logika sangat cepat tapi boros daya." },
  { "en": "Metastability?", "id": "Keadaan tidak stabil pada flip-flop." },
  { "en": "Sinkronizer?", "id": "Rangkaian untuk mengurangi masalah metastability." },
  { "en": "Transformasi Laplace?", "id": "Mengubah fungsi domain waktu ke domain-s." },
  { "en": "Transformasi Laplace Invers?", "id": "Mengubah fungsi domain-s kembali ke waktu." },
  { "en": "Fungsi Step Unit?", "id": "Sinyal yang bernilai 1 untuk t>=0." },
  { "en": "Laplace dari Step Unit?", "id": "Transformasi Laplace dari fungsi step." },
  { "en": "Fungsi Impuls (Dirac Delta)?", "id": "Sinyal dengan luas 1 pada t=0." },
  { "en": "Laplace dari Impuls?", "id": "Transformasi Laplace dari fungsi impuls." },
  { "en": "Laplace dari Ramp?", "id": "Transformasi Laplace dari fungsi ramp." },
  { "en": "Laplace dari Eksponensial?", "id": "Transformasi Laplace dari peluruhan eksponensial." },
  { "en": "Laplace dari Sinus?", "id": "Transformasi Laplace dari fungsi sinus." },
  { "en": "Laplace dari Cosinus?", "id": "Transformasi Laplace dari fungsi cosinus." },
  { "en": "Properti Turunan (Laplace)?", "id": "Transformasi Laplace dari sebuah turunan." },
  { "en": "Properti Integral (Laplace)?", "id": "Transformasi Laplace dari sebuah integral." },
  { "en": "Teorema Nilai Awal?", "id": "Mencari nilai awal dari domain-s." },
  { "en": "Teorema Nilai Akhir?", "id": "Mencari nilai akhir dari domain-s." },
  { "en": "Fungsi Transfer?", "id": "Rasio output terhadap input di domain-s." },
  { "en": "Impedansi Kapasitor (s-domain)?", "id": "Representasi impedansi kapasitor di domain-s." },
  { "en": "Impedansi Induktor (s-domain)?", "id": "Representasi impedansi induktor di domain-s." },
  { "en": "Pole dari Sistem?", "id": "Akar dari penyebut fungsi transfer." },
  { "en": "Zero dari Sistem?", "id": "Akar dari pembilang fungsi transfer." },
  { "en": "Sistem Orde Pertama?", "id": "Sistem dengan satu penyimpan energi." },
  { "en": "Konstanta Waktu (Sistem Kontrol)?", "id": "Ukuran kecepatan respons sistem orde pertama." },
  { "en": "Sistem Orde Kedua?", "id": "Bentuk standar fungsi transfer orde kedua." },
  { "en": "Frekuensi Natural (Undamped)?", "id": "Frekuensi osilasi tanpa redaman." },
  { "en": "Rasio Redaman (Damping Ratio)?", "id": "Menentukan sifat osilasi respons sistem." },
  { "en": "Kondisi Overdamped?", "id": "Respons lambat tanpa osilasi." },
  { "en": "Kondisi Critically Damped?", "id": "Respons tercepat tanpa overshoot." },
  { "en": "Kondisi Underdamped?", "id": "Respons dengan osilasi sebelum stabil." },
  { "en": "Kondisi Undamped?", "id": "Osilasi terus-menerus tanpa henti." },
  { "en": "Rise Time?", "id": "Waktu respons naik dari 10% ke 90%." },
  { "en": "Peak Time?", "id": "Waktu untuk mencapai puncak pertama." },
  { "en": "Settling Time?", "id": "Waktu respons masuk dalam batas toleransi." },
  { "en": "Percent Overshoot?", "id": "Puncak maksimum melampaui nilai akhir." },
  { "en": "Error Steady-State?", "id": "Selisih antara input dan output." },
  { "en": "Konstanta Error Posisi?", "id": "Menentukan error steady-state untuk input step." },
  { "en": "Error untuk Step Input?", "id": "Error steady-state untuk sistem tipe 0." },
  { "en": "Konstanta Error Kecepatan?", "id": "Menentukan error steady-state untuk input ramp." },
  { "en": "Error untuk Ramp Input?", "id": "Error steady-state untuk sistem tipe 1." },
  { "en": "Konstanta Error Akselerasi?", "id": "Menentukan error untuk input parabola." },
  { "en": "Tipe Sistem?", "id": "Jumlah pole pada titik origin (s=0)." },
  { "en": "Fungsi Transfer Loop Tertutup?", "id": "Fungsi transfer sistem dengan umpan balik." },
  { "en": "Fungsi Transfer Loop Terbuka?", "id": "Fungsi transfer tanpa menutup loop umpan balik." },
  { "en": "Persamaan Karakteristik?", "id": "Menentukan stabilitas sistem loop tertutup." },
  { "en": "Stabilitas BIBO (Bounded-Input, Bounded-Output)?", "id": "Input terbatas menghasilkan output terbatas." },
  { "en": "Kondisi Stabilitas?", "id": "Semua pole loop tertutup di setengah bidang kiri." },
  { "en": "Kriteria Stabilitas Routh-Hurwitz?", "id": "Metode tabular menentukan stabilitas tanpa mencari pole." },
  { "en": "Root Locus?", "id": "Plot lokasi pole saat gain bervariasi." },
  { "en": "Sudut Asimtot (Root Locus)?", "id": "Arah cabang root locus menuju tak hingga." },
  { "en": "Titik Pusat (Centroid)?", "id": "Titik potong asimtot di sumbu real." },
  { "en": "Kondisi Sudut (Root Locus)?", "id": "Titik pada bidang-s yang merupakan root locus." },
  { "en": "Kondisi Magnitudo (Root Locus)?", "id": "Menentukan gain K pada titik root locus." },
  { "en": "Bode Plot?", "id": "Plot respons frekuensi (magnitudo dan fasa)." },
  { "en": "Gain Margin (GM)?", "id": "Faktor gain bisa dinaikkan sebelum tak stabil." },
  { "en": "Phase Margin (PM)?", "id": "Fasa bisa diturunkan sebelum tak stabil." },
  { "en": "Frekuensi Crossover Gain?", "id": "Frekuensi saat magnitudo loop terbuka = 1 (0 dB)." },
  { "en": "Frekuensi Crossover Fasa?", "id": "Frekuensi saat fasa loop terbuka = -180 derajat." },
  { "en": "Nyquist Plot?", "id": "Plot respons frekuensi dalam koordinat polar." },
  { "en": "Kriteria Stabilitas Nyquist?", "id": "Menentukan stabilitas dari plot Nyquist." },
  { "en": "Kontroler Proporsional (P)?", "id": "Output kontroler sebanding dengan error." },
  { "en": "Kontroler Integral (I)?", "id": "Output kontroler sebanding dengan akumulasi error." },
  { "en": "Fungsi Kontroler Integral?", "id": "Menghilangkan error steady-state." },
  { "en": "Kontroler Derivatif (D)?", "id": "Output kontroler sebanding dengan laju perubahan error." },
  { "en": "Fungsi Kontroler Derivatif?", "id": "Meningkatkan redaman dan memprediksi error." },
  { "en": "Kontroler PID (Proporsional-Integral-Derivatif)?", "id": "Kontroler umpan balik yang paling umum." },
  { "en": "Metode Tuning Ziegler-Nichols?", "id": "Metode heuristik untuk menentukan parameter PID." },
  { "en": "Lead Compensator?", "id": "Meningkatkan phase margin dan kecepatan respons." },
  { "en": "Lag Compensator?", "id": "Memperbaiki error steady-state." },
  { "en": "Lag-Lead Compensator?", "id": "Menggabungkan keuntungan dari lag dan lead." },
  { "en": "Representasi State-Space (Persamaan Keadaan)?", "id": "Persamaan keadaan sistem." },
  { "en": "Representasi State-Space (Persamaan Output)?", "id": "Persamaan output sistem." },
  { "en": "Matriks Transisi Keadaan?", "id": "Menentukan evolusi keadaan sistem." },
  { "en": "Solusi Persamaan Keadaan?", "id": "Solusi total dari sistem state-space." },
  { "en": "Fungsi Transfer dari State-Space?", "id": "Mengubah representasi state-space ke fungsi transfer." },
  { "en": "Keterkontrolan (Controllability)?", "id": "Kemampuan input untuk menggerakkan semua keadaan." },
  { "en": "Matriks Keterkontrolan?", "id": "Matriks untuk menguji keterkontrolan." },
  { "en": "Keteramatan (Observability)?", "id": "Kemampuan output untuk merekonstruksi semua keadaan." },
  { "en": "Matriks Keteramatan?", "id": "Matriks untuk menguji keteramatan." },
  { "en": "Penempatan Pole (Pole Placement)?", "id": "Menentukan gain umpan balik untuk pole." },
  { "en": "Hukum Kontrol Umpan Balik Keadaan?", "id": "Menggunakan semua variabel keadaan untuk umpan balik." },
  { "en": "Persamaan Karakteristik (Umpan Balik Keadaan)?", "id": "Menentukan pole sistem dengan umpan balik keadaan." },
  { "en": "Estimator Keadaan (Observer)?", "id": "Memperkirakan keadaan sistem dari input-output." },
  { "en": "Observer Luenberger?", "id": "Jenis umum dari estimator keadaan." },
  { "en": "Prinsip Separasi?", "id": "Desain kontroler dan observer dapat dilakukan terpisah." },
  { "en": "Sistem Waktu Diskrit?", "id": "Sistem yang beroperasi pada interval waktu diskrit." },
  { "en": "Transformasi-Z?", "id": "Analogi transformasi Laplace untuk sinyal diskrit." },
  { "en": "Fungsi Transfer Pulsa?", "id": "Fungsi transfer untuk sistem waktu diskrit." },
  { "en": "Kondisi Stabilitas (Diskrit)?", "id": "Semua pole berada di dalam lingkaran satuan." },
  { "en": "Kriteria Stabilitas Jury?", "id": "Analogi kriteria Routh-Hurwitz untuk sistem diskrit." },
  { "en": "Zero-Order Hold (ZOH)?", "id": "Mengubah sinyal diskrit menjadi sinyal tangga." },
  { "en": "Diagram Blok?", "id": "Representasi grafis dari fungsi-fungsi sistem." },
  { "en": "Signal Flow Graph?", "id": "Representasi grafis lain dari sistem linear." },
  { "en": "Rumus Gain Mason?", "id": "Menentukan fungsi transfer dari signal flow graph." },
  { "en": "Sensitivitas?", "id": "Ukuran seberapa besar perubahan parameter mempengaruhi sistem." },
  { "en": "Robustness?", "id": "Kemampuan sistem bekerja baik meski ada ketidakpastian." },
  { "en": "Sistem Non-linear?", "id": "Sistem yang tidak mematuhi prinsip superposisi." },
  { "en": "Linearisasi?", "id": "Membuat aproksimasi linear dari sistem non-linear." },
  { "en": "Describing Function?", "id": "Analisis sistem non-linear untuk input sinusoidal." },
  { "en": "Phase Plane Analysis?", "id": "Metode grafis untuk sistem orde kedua." },
  { "en": "Stabilitas Lyapunov?", "id": "Metode untuk menentukan stabilitas sistem non-linear." },
  { "en": "Kontrol Optimal?", "id": "Mendesain kontroler untuk meminimalkan indeks performa." },
  { "en": "Linear-Quadratic Regulator (LQR)?", "id": "Jenis umum dari kontroler optimal." },
  { "en": "Persamaan Riccati?", "id": "Persamaan untuk menemukan gain LQR." },
  { "en": "Kontrol Adaptif?", "id": "Kontroler yang menyesuaikan parameternya secara online." },
  { "en": "Teorema Sampling Nyquist?", "id": "Syarat laju sampling untuk menghindari aliasing." },
  { "en": "Modulasi Amplitudo (AM)?", "id": "Amplitudo sinyal pembawa diubah oleh sinyal informasi." },
  { "en": "Indeks Modulasi AM?", "id": "Ukuran seberapa dalam amplitudo dimodulasi." },
  { "en": "Daya Total AM?", "id": "Total daya sinyal AM." },
  { "en": "Efisiensi Modulasi AM?", "id": "Persentase daya yang membawa informasi." },
  { "en": "AM DSB-SC (Double Sideband Suppressed Carrier)?", "id": "Sinyal AM tanpa komponen pembawa." },
  { "en": "AM SSB (Single Sideband)?", "id": "Hanya mentransmisikan satu sideband." },
  { "en": "Modulasi Frekuensi (FM)?", "id": "Frekuensi sesaat sinyal pembawa diubah." },
  { "en": "Frekuensi Sesaat FM?", "id": "Frekuensi pembawa berubah seiring sinyal informasi." },
  { "en": "Deviasi Frekuensi Maksimum?", "id": "Pergeseran frekuensi maksimum dari frekuensi pembawa." },
  { "en": "Indeks Modulasi FM?", "id": "Rasio deviasi frekuensi terhadap frekuensi modulasi." },
  { "en": "Aturan Carson (Bandwidth FM)?", "id": "Memperkirakan bandwidth sinyal FM." },
  { "en": "Modulasi Fasa (PM)?", "id": "Fasa sesaat sinyal pembawa diubah." },
  { "en": "Pre-emphasis dan De-emphasis?", "id": "Meningkatkan SNR pada sistem FM." },
  { "en": "Superheterodyne Receiver?", "id": "Prinsip penerima radio yang umum digunakan." },
  { "en": "Frekuensi Antara (IF)?", "id": "Frekuensi hasil percampuran sinyal RF dan lokal." },
  { "en": "Pulse Code Modulation (PCM)?", "id": "Proses mengubah sinyal analog ke digital." },
  { "en": "Error Kuantisasi?", "id": "Selisih antara nilai asli dan terkuantisasi." },
  { "en": "Signal-to-Quantization Noise Ratio (SQNR)?", "id": "Rasio daya sinyal terhadap noise kuantisasi." },
  { "en": "Bit Rate?", "id": "Jumlah bit per detik." },
  { "en": "Amplitude Shift Keying (ASK)?", "id": "Merepresentasikan data dengan amplitudo berbeda." },
  { "en": "Frequency Shift Keying (FSK)?", "id": "Merepresentasikan data dengan frekuensi berbeda." },
  { "en": "Phase Shift Keying (PSK)?", "id": "Merepresentasikan data dengan fasa berbeda." },
  { "en": "Binary PSK (BPSK)?", "id": "Menggunakan dua fasa (0 dan 180 derajat)." },
  { "en": "Quadrature PSK (QPSK)?", "id": "Menggunakan empat fasa, membawa 2 bit/simbol." },
  { "en": "Quadrature Amplitude Modulation (QAM)?", "id": "Menggunakan kombinasi amplitudo dan fasa." },
  { "en": "Bandwidth Efisiensi?", "id": "Jumlah bit/detik per satuan bandwidth." },
  { "en": "Probabilitas Error Bit (BER)?", "id": "Rasio bit yang salah diterima." },
  { "en": "BER untuk BPSK?", "id": "Kinerja BPSK dalam noise Gaussian." },
  { "en": "Energi per Bit?", "id": "Energi yang digunakan untuk mentransmisikan satu bit." },
  { "en": "Kerapatan Spektral Noise?", "id": "Daya noise per satuan bandwidth." },
  { "en": "Rasio Eb/N0?", "id": "Rasio energi per bit terhadap noise." },
  { "en": "Matched Filter?", "id": "Filter optimal untuk memaksimalkan SNR." },
  { "en": "Inter-Symbol Interference (ISI)?", "id": "Satu simbol mengganggu simbol berikutnya." },
  { "en": "Kriteria Nyquist untuk Zero ISI?", "id": "Syarat bentuk pulsa untuk transmisi bebas ISI." },
  { "en": "Raised-Cosine Filter?", "id": "Filter praktis yang memenuhi kriteria Nyquist." },
  { "en": "Entropy Informasi?", "id": "Ukuran ketidakpastian rata-rata suatu sumber." },
  { "en": "Teorema Source Coding Shannon?", "id": "Batas kompresi data adalah entropi sumber." },
  { "en": "Efisiensi Kode?", "id": "Rasio entropi terhadap panjang kode rata-rata." },
  { "en": "Pengkodean Huffman?", "id": "Algoritma untuk membuat kode prefiks optimal." },
  { "en": "Kapasitas Kanal?", "id": "Laju transmisi data maksimum yang andal." },
  { "en": "Informasi Mutual?", "id": "Ukuran informasi bersama antara input dan output." },
  { "en": "Teorema Channel Coding Shannon?", "id": "Transmisi andal mungkin jika R < C." },
  { "en": "Kapasitas Kanal (AWGN)?", "id": "Hukum Shannon-Hartley." },
  { "en": "Signal-to-Noise Ratio (SNR)?", "id": "Rasio daya sinyal terhadap daya noise." },
  { "en": "Shannon Limit?", "id": "Batas bawah teoritis untuk komunikasi andal." },
  { "en": "Code Rate?", "id": "Rasio bit informasi (k) terhadap bit total (n)." },
  { "en": "Frequency Division Multiplexing (FDM)?", "id": "Membagi kanal berdasarkan frekuensi." },
  { "en": "Time Division Multiplexing (TDM)?", "id": "Membagi kanal berdasarkan slot waktu." },
  { "en": "Code Division Multiple Access (CDMA)?", "id": "Pengguna dibedakan oleh kode unik." },
  { "en": "Orthogonal Frequency Division Multiplexing (OFDM)?", "id": "Teknik modulasi untuk komunikasi kecepatan tinggi." },
  { "en": "Spread Spectrum?", "id": "Menyebarkan sinyal ke bandwidth yang lebih lebar." },
  { "en": "Processing Gain (Spread Spectrum)?", "id": "Peningkatan ketahanan terhadap noise dan interferensi." },
  { "en": "Frequency Hopping Spread Spectrum (FHSS)?", "id": "Mengubah frekuensi pembawa secara periodik." },
  { "en": "Direct Sequence Spread Spectrum (DSSS)?", "id": "Mengalikan sinyal data dengan kode penyebar." },
  { "en": "Fading?", "id": "Fluktuasi kekuatan sinyal yang diterima." },
  { "en": "Multipath Fading?", "id": "Interferensi akibat sinyal tiba melalui banyak lintasan." },
  { "en": "Rayleigh Fading?", "id": "Model statistik untuk fading tanpa lintasan dominan." },
  { "en": "Rician Fading?", "id": "Model statistik untuk fading dengan lintasan dominan." },
  { "en": "Diversity?", "id": "Teknik untuk melawan fading (spasial, frekuensi, waktu)." },
  { "en": "Multiple-Input Multiple-Output (MIMO)?", "id": "Menggunakan banyak antena di pemancar dan penerima." },
  { "en": "Kapasitas MIMO?", "id": "Meningkatkan kapasitas kanal secara signifikan." },
  { "en": "Beamforming?", "id": "Mengarahkan energi transmisi ke arah tertentu." },
  { "en": "Protokol ALOHA?", "id": "Protokol akses media acak yang sederhana." },
  { "en": "Carrier Sense Multiple Access (CSMA)?", "id": "Mendengarkan kanal sebelum mentransmisikan." },
  { "en": "CSMA/CD (Collision Detection)?", "id": "Berhenti transmisi jika terdeteksi tabrakan." },
  { "en": "CSMA/CA (Collision Avoidance)?", "id": "Mencoba menghindari tabrakan (digunakan di Wi-Fi)." },
  { "en": "Error Detection vs Correction?", "id": "Mendeteksi error vs memperbaiki error." },
  { "en": "Forward Error Correction (FEC)?", "id": "Menambahkan redundansi untuk memperbaiki error." },
  { "en": "Automatic Repeat Request (ARQ)?", "id": "Meminta transmisi ulang jika terdeteksi error." },
  { "en": "Stop-and-Wait ARQ?", "id": "Mengirim satu frame, menunggu ACK." },
  { "en": "Go-Back-N ARQ?", "id": "Mengirim ulang frame yang salah dan semua sesudahnya." },
  { "en": "Selective Repeat ARQ?", "id": "Hanya mengirim ulang frame yang salah." },
  { "en": "Convolutional Codes?", "id": "Jenis kode FEC yang umum." },
  { "en": "Trellis Diagram?", "id": "Representasi grafis dari convolutional encoder." },
  { "en": "Viterbi Algorithm?", "id": "Algoritma untuk mendekode convolutional codes." },
  { "en": "Block Codes?", "id": "Jenis kode FEC dimana data dibagi jadi blok." },
  { "en": "Reed-Solomon Codes?", "id": "Kode blok yang kuat untuk burst error." },
  { "en": "Turbo Codes?", "id": "Kode yang performanya mendekati batas Shannon." },
  { "en": "LDPC (Low-Density Parity-Check) Codes?", "id": "Kode modern lain dengan performa sangat baik." },
  { "en": "Polar Codes?", "id": "Kode yang terbukti mencapai kapasitas kanal." },
  { "en": "OSI Model (Open Systems Interconnection)?", "id": "Model konseptual 7 lapisan untuk jaringan." },
  { "en": "TCP/IP Model?", "id": "Model 4 lapisan yang digunakan internet." },
  { "en": "Bandwidth-Delay Product?", "id": "Jumlah data yang bisa 'mengisi' jaringan." },
  { "en": "Throughput?", "id": "Laju transfer data aktual yang berhasil." },
  { "en": "Latency (Delay)?", "id": "Waktu yang dibutuhkan data untuk perjalanan." },
  { "en": "Jitter?", "id": "Variasi dalam latency." },
  { "en": "Hukum Little?", "id": "Hubungan rata-rata jumlah item dalam sistem." },
  { "en": "Teori Antrian (Queueing)?", "id": "Studi matematis tentang antrian atau garis tunggu." },
  { "en": "Erlang (Unit)?", "id": "Ukuran beban lalu lintas telekomunikasi." },
  { "en": "Kualitas Layanan (QoS)?", "id": "Kemampuan jaringan untuk menyediakan layanan lebih baik." },
  { "en": "PLL (Phase-Locked Loop)?", "id": "Sistem kontrol untuk sinkronisasi sinyal." },
  { "en": "Costas Loop?", "id": "Sirkuit untuk demodulasi sinyal DSB-SC." },
  { "en": "Fiber Optic Communication?", "id": "Komunikasi menggunakan cahaya melalui serat." },
  { "en": "Numerical Aperture (NA)?", "id": "Ukuran kemampuan serat optik menerima cahaya." },
  { "en": "Atenuasi Serat Optik?", "id": "Pelemahan sinyal cahaya dalam serat." },
  { "en": "Dispersi Kromatik?", "id": "Penyebaran pulsa akibat kecepatan cahaya berbeda." },
  { "en": "Komunikasi Satelit?", "id": "Menggunakan satelit sebagai repeater di angkasa." },
  { "en": "Link Budget?", "id": "Perhitungan semua gain dan loss transmisi." },
  { "en": "Sistem Per-Unit?", "id": "Menormalisasi besaran sistem tenaga." },
  { "en": "Perubahan Basis Per-Unit?", "id": "Mengkonversi nilai per-unit ke basis baru." },
  { "en": "Arus Gangguan Hubung Singkat (3-Fasa)?", "id": "Menghitung arus gangguan simetris." },
  { "en": "Komponen Simetris (Urutan Positif)?", "id": "Komponen seimbang dengan urutan fasa asli." },
  { "en": "Komponen Simetris (Urutan Negatif)?", "id": "Komponen seimbang dengan urutan fasa terbalik." },
  { "en": "Komponen Simetris (Urutan Nol)?", "id": "Tiga komponen sefasa." },
  { "en": "Operator 'a'?", "id": "Operator rotasi fasa 120 derajat." },
  { "en": "Arus Gangguan Fasa-ke-Tanah?", "id": "Menghitung arus gangguan satu fasa ke tanah." },
  { "en": "Analisis Aliran Daya (Power Flow)?", "id": "Analisis aliran daya dalam jaringan listrik." },
  { "en": "Matriks Admitansi Bus (Ybus)?", "id": "Representasi admitansi dari sebuah jaringan listrik." },
  { "en": "Persamaan Aliran Daya?", "id": "Persamaan non-linear untuk analisis aliran daya." },
  { "en": "Metode Gauss-Seidel?", "id": "Metode iteratif untuk menyelesaikan aliran daya." },
  { "en": "Metode Newton-Raphson?", "id": "Metode iteratif yang lebih cepat untuk aliran daya." },
  { "en": "Slack Bus (Swing Bus)?", "id": "Bus referensi dalam analisis aliran daya." },
  { "en": "PV Bus (Generator Bus)?", "id": "Bus dengan daya nyata dan tegangan tertentu." },
  { "en": "PQ Bus (Load Bus)?", "id": "Bus dengan daya nyata dan reaktif tertentu." },
  { "en": "Stabilitas Sistem Tenaga?", "id": "Kemampuan sistem untuk tetap sinkron." },
  { "en": "Stabilitas Steady-State?", "id": "Stabilitas terhadap gangguan kecil." },
  { "en": "Stabilitas Transien?", "id": "Stabilitas terhadap gangguan besar (misal, hubung singkat)." },
  { "en": "Swing Equation?", "id": "Model dinamika sudut rotor mesin sinkron." },
  { "en": "Konstanta Inersia?", "id": "Ukuran energi kinetik tersimpan di rotor." },
  { "en": "Kriteria Area Sama (Equal Area Criterion)?", "id": "Metode grafis untuk analisis stabilitas transien." },
  { "en": "Regulasi Tegangan Transformator?", "id": "Perubahan tegangan sekunder dari tanpa beban ke beban penuh." },
  { "en": "Efisiensi Transformator?", "id": "Rasio daya output terhadap daya input." },
  { "en": "Rugi Tembaga (Copper Loss)?", "id": "Rugi daya akibat resistansi lilitan." },
  { "en": "Rugi Inti (Core Loss)?", "id": "Rugi daya akibat histeresis dan arus eddy." },
  { "en": "Uji Rangkaian Terbuka (OC Test)?", "id": "Menentukan parameter cabang eksitasi transformator." },
  { "en": "Uji Hubung Singkat (SC Test)?", "id": "Menentukan impedansi seri transformator." },
  { "en": "Autotransformer?", "id": "Transformator dengan satu lilitan." },
  { "en": "Kecepatan Sinkron?", "id": "Kecepatan putar medan magnet stator." },
  { "en": "Slip Mesin Induksi?", "id": "Perbedaan fraksional antara kecepatan sinkron dan rotor." },
  { "en": "Frekuensi Arus Rotor?", "id": "Frekuensi arus yang diinduksikan di rotor." },
  { "en": "Daya Celah Udara (Air-gap Power)?", "id": "Daya yang ditransfer dari stator ke rotor." },
  { "en": "Daya Mekanik Dikembangkan?", "id": "Daya listrik yang diubah menjadi daya mekanik." },
  { "en": "Torsi Induksi?", "id": "Torsi yang dihasilkan oleh mesin induksi." },
  { "en": "Rangkaian Ekuivalen Mesin Induksi?", "id": "Model rangkaian untuk menganalisis performa mesin." },
  { "en": "Torsi Maksimum (Breakdown Torque)?", "id": "Torsi maksimum yang bisa dihasilkan mesin." },
  { "en": "Torsi Awal (Starting Torque)?", "id": "Torsi yang dihasilkan saat mesin mulai berputar." },
  { "en": "GGL Induksi (Mesin Sinkron)?", "id": "Tegangan yang dibangkitkan di lilitan stator." },
  { "en": "Sudut Torsi (Sudut Daya)?", "id": "Sudut antara tegangan GGL dan terminal." },
  { "en": "Daya (Mesin Sinkron)?", "id": "Daya nyata yang dikirim oleh generator." },
  { "en": "Reaktansi Sinkron?", "id": "Reaktansi ekuivalen per fasa mesin sinkron." },
  { "en": "Kurva V (V-Curve)?", "id": "Plot arus jangkar vs arus medan." },
  { "en": "Motor DC (GGL)?", "id": "GGL lawan yang diinduksikan di jangkar." },
  { "en": "Motor DC (Torsi)?", "id": "Torsi yang dikembangkan oleh motor DC." },
  { "en": "Persamaan Tegangan Motor DC?", "id": "Hukum KVL untuk rangkaian jangkar motor DC." },
  { "en": "Pengaturan Kecepatan Motor DC?", "id": "Mengatur kecepatan dengan mengubah tegangan/fluks/resistansi." },
  { "en": "Motor Shunt?", "id": "Lilitan medan paralel dengan jangkar." },
  { "en": "Motor Seri?", "id": "Lilitan medan seri dengan jangkar." },
  { "en": "Motor Kompon?", "id": "Memiliki lilitan medan seri dan shunt." },
  { "en": "Komutasi Mesin DC?", "id": "Proses membalik arah arus di lilitan." },
  { "en": "Reaksi Jangkar?", "id": "Efek medan magnet jangkar pada medan utama." },
  { "en": "Motor Universal?", "id": "Dapat beroperasi pada suplai DC atau AC." },
  { "en": "Motor Stepper?", "id": "Motor yang berputar dalam langkah-langkah diskrit." },
  { "en": "Sudut Langkah (Step Angle)?", "id": "Sudut putaran per satu pulsa input." },
  { "en": "Motor Reluktansi?", "id": "Motor sinkron tanpa lilitan medan di rotor." },
  { "en": "Motor Histeresis?", "id": "Menggunakan efek histeresis untuk menghasilkan torsi." },
  { "en": "BLDC (Brushless DC) Motor?", "id": "Motor DC tanpa sikat dengan komutasi elektronik." },
  { "en": "Motor Induksi Fasa Tunggal?", "id": "Membutuhkan mekanisme start (misal, lilitan bantu)." },
  { "en": "Teori Medan Berputar Ganda?", "id": "Analisis medan magnet pada motor fasa tunggal." },
  { "en": "Starting Kapasitor?", "id": "Membantu menghasilkan torsi awal yang lebih tinggi." },
  { "en": "Running Kapasitor?", "id": "Meningkatkan performa motor saat beroperasi." },
  { "en": "Motor Shaded-Pole?", "id": "Motor fasa tunggal berdaya sangat rendah." },
  { "en": "Generator Induksi?", "id": "Mesin induksi yang dioperasikan di atas kecepatan sinkron." },
  { "en": "Sinkronisasi Generator?", "id": "Proses menghubungkan generator ke jaringan (grid)." },
  { "en": "Syarat Sinkronisasi?", "id": "Tegangan, frekuensi, urutan fasa harus sama." },
  { "en": "Economic Dispatch?", "id": "Membagi beban di antara generator secara ekonomis." },
  { "en": "Incremental Cost (IC)?", "id": "Biaya untuk menghasilkan satu megawatt-jam tambahan." },
  { "en": "Penalty Factor?", "id": "Faktor yang memperhitungkan rugi-rugi transmisi." },
  { "en": "FACTS (Flexible AC Transmission Systems)?", "id": "Perangkat elektronika daya untuk kontrol aliran daya." },
  { "en": "SVC (Static VAR Compensator)?", "id": "Menyuntikkan atau menyerap daya reaktif." },
  { "en": "STATCOM (Static Synchronous Compensator)?", "id": "SVC berbasis voltage-sourced converter." },
  { "en": "UPFC (Unified Power Flow Controller)?", "id": "Mengontrol tegangan, impedansi, dan sudut fasa." },
  { "en": "HVDC (High-Voltage Direct Current)?", "id": "Transmisi daya jarak jauh menggunakan DC." },
  { "en": "Gardu Induk (Substation)?", "id": "Fasilitas untuk mengubah level tegangan." },
  { "en": "Circuit Breaker (CB)?", "id": "Saklar otomatis untuk proteksi gangguan." },
  { "en": "Relai Proteksi?", "id": "Mendeteksi kondisi abnormal dan memerintahkan CB." },
  { "en": "Overcurrent Relay?", "id": "Bekerja berdasarkan arus yang berlebih." },
  { "en": "Differential Relay?", "id": "Membandingkan arus masuk dan keluar zona proteksi." },
  { "en": "Distance Relay?", "id": "Mengukur impedansi untuk menentukan lokasi gangguan." },
  { "en": "Isolator?", "id": "Komponen untuk mengisolasi peralatan dari tegangan." },
  { "en": "Lightning Arrester?", "id": "Melindungi peralatan dari sambaran petir." },
  { "en": "Sistem Pentanahan (Grounding)?", "id": "Menghubungkan sistem ke bumi untuk keamanan." },
  { "en": "Kurva Kapabilitas Generator?", "id": "Grafik yang menunjukkan batas operasi generator." },
  { "en": "Pembangkit Listrik Tenaga Air (PLTA)?", "id": "Menggunakan energi potensial air." },
  { "en": "Pembangkit Listrik Tenaga Uap (PLTU)?", "id": "Menggunakan uap untuk memutar turbin." },
  { "en": "Pembangkit Listrik Tenaga Nuklir (PLTN)?", "id": "Menggunakan reaksi fisi untuk menghasilkan panas." },
  { "en": "Pembangkit Listrik Tenaga Surya (PLTS)?", "id": "Menggunakan sel fotovoltaik." },
  { "en": "Pembangkit Listrik Tenaga Angin (PLTB)?", "id": "Menggunakan turbin angin." },
  { "en": "Manajemen Beban (Load Management)?", "id": "Upaya untuk mengatur permintaan listrik konsumen." },
  { "en": "Jaringan Cerdas (Smart Grid)?", "id": "Modernisasi jaringan listrik dengan teknologi informasi." },
  { "en": "Distributed Generation (DG)?", "id": "Pembangkitan listrik skala kecil di dekat konsumen." },
  { "en": "Kualitas Daya (Power Quality)?", "id": "Ukuran deviasi tegangan dan arus dari ideal." },
  { "en": "Harmonisa?", "id": "Komponen frekuensi kelipatan dari frekuensi fundamental." },
  { "en": "Total Harmonic Distortion (THD)?", "id": "Ukuran total distorsi harmonisa." },
  { "en": "Sag (Dip) Tegangan?", "id": "Penurunan tegangan sesaat." },
  { "en": "Swell Tegangan?", "id": "Kenaikan tegangan sesaat." },
  { "en": "Transient?", "id": "Perubahan tegangan/arus sesaat yang drastis." },
  { "en": "Flicker?", "id": "Persepsi visual akibat fluktuasi tegangan." },
  { "en": "Topologi Jaringan Distribusi (Radial)?", "id": "Bentuk paling sederhana, satu jalur suplai." },
  { "en": "Affective Computing?", "id": "Komputasi yang berhubungan dengan emosi manusia." },
  { "en": "Deteksi emosi dari suara?", "id": "Menganalisis pitch, energi, dan fitur spektral." },
  { "en": "Deteksi emosi dari wajah?", "id": "Menganalisis ekspresi mikro dan unit aksi." },
  { "en": "Facial Action Coding System (FACS)?", "id": "Sistem untuk mengklasifikasikan gerakan otot wajah." },
  { "en": "Sinyal fisiologis?", "id": "Sinyal dari tubuh (detak jantung, konduktansi kulit)." },
  { "en": "Galvanic Skin Response (GSR)?", "id": "Mengukur perubahan konduktansi kulit akibat emosi." },
  { "en": "Granger Causality?", "id": "Uji statistik untuk kausalitas prediktif." },
  { "en": "Fungsi Granger Causality?", "id": "Menentukan apakah satu time series berguna." },
  { "en": "Transfer Entropy?", "id": "Ukuran aliran informasi antara dua proses." },
  { "en": "Computational imaging?", "id": "Pencitraan yang bergantung pada pemrosesan komputasional." },
  { "en": "Single-pixel camera?", "id": "Kamera yang membentuk citra dengan satu detektor." },
  { "en": "Ghost imaging?", "id": "Membentuk citra objek tanpa cahaya menyentuhnya." },
  { "en": "Diffraction tomography?", "id": "Teknik pencitraan 3D menggunakan data difraksi." },
  { "en": "Ptychography?", "id": "Teknik mikroskopi resolusi tinggi." },
  { "en": "Event-based camera?", "id": "Sensor yang hanya merekam perubahan kecerahan piksel." },
  { "en": "Keuntungan event-based camera?", "id": "Rentang dinamis tinggi dan latensi rendah." },
  { "en": "Finite Rate of Innovation (FRI)?", "id": "Prinsip sampling sinyal non-bandlimited." },
  { "en": "Syarat sinyal FRI?", "id": "Memiliki struktur parametrik seperti aliran Dirac." },
  { "en": "Xampling?", "id": "Framework hardware-efficient untuk sampling sinyal FRI." },
  { "en": "Teorema Sampling Shannon?", "id": "Menetapkan laju sampling minimum untuk sinyal bandlimited." },
  { "en": "Siapa Claude Shannon?", "id": "Bapak dari teori informasi." },
  { "en": "Siapa Norbert Wiener?", "id": "Perintis sibernetika dan filter Wiener." },
  { "en": "Kolmogorov Complexity?", "id": "Ukuran kompleksitas algoritmik dari suatu objek." },
  { "en": "Hubungan Komplesitas Kolmogorov dengan kompresi?", "id": "Batas bawah teoretis dari kompresi lossless." },
  { "en": "The Lottery Ticket Hypothesis?", "id": "Jaringan saraf padat berisi 'subnetwork' pemenang." },
  { "en": "Neural Tangent Kernel (NTK)?", "id": "Mendeskripsikan dinamika training jaringan saraf lebar." },
  { "en": "Infinite-width limit?", "id": "Batas teoretis saat jumlah neuron tak terbatas." },
  { "en": "WaveNet?", "id": "Model generatif dalam untuk audio mentah." },
  { "en": "Physical modeling synthesis?", "id": "Sintesis suara dengan memodelkan instrumen fisik." },
  { "en": "Granular synthesis?", "id": "Sintesis suara dengan menggabungkan 'butiran' audio kecil." },
  { "en": "Subtractive synthesis?", "id": "Sintesis dengan memfilter bentuk gelombang kaya harmonik." },
  { "en": "Additive synthesis?", "id": "Sintesis dengan menjumlahkan gelombang sinus." },
  { "en": "Frequency modulation (FM) synthesis?", "id": "Sintesis suara dengan memodulasi frekuensi osilator." },
  { "en": "Sinyal dari LIGO?", "id": "Mendeteksi riak kecil dalam ruang-waktu." },
  { "en": "Sumber sinyal gelombang gravitasi?", "id": "Tabrakan lubang hitam atau bintang neutron." },
  { "en": "Matched filtering?", "id": "Teknik untuk mendeteksi sinyal yang diketahui." },
  { "en": "Aplikasi matched filtering di LIGO?", "id": "Mencari sinyal gelombang gravitasi dalam noise." },
  { "en": "Radio astronomy?", "id": "Mempelajari objek langit pada frekuensi radio." },
  { "en": "Interferometry (astronomi)?", "id": "Menggabungkan sinyal dari banyak teleskop." },
  { "en": "Tujuan interferometry?", "id": "Mencapai resolusi sudut yang sangat tinggi." },
  { "en": "Deconvolution (astronomi)?", "id": "Menghapus efek instrumental dari citra." },
  { "en": "Algoritma CLEAN?", "id": "Algoritma dekonvolusi populer dalam radio astronomi." },
  { "en": "Full Waveform Inversion (FWI)?", "id": "Teknik pencitraan seismik resolusi tinggi." },
  { "en": "Reverse Time Migration (RTM)?", "id": "Metode migrasi seismik berbasis persamaan gelombang." },
  { "en": "Phasor Measurement Unit (PMU)?", "id": "Mengukur magnitudo dan fasa tegangan." },
  { "en": "Synchrophasor?", "id": "Pengukuran phasor yang disinkronkan dengan waktu." },
  { "en": "Aplikasi PMU?", "id": "Monitoring dan kontrol jaringan listrik." },
  { "en": "Wide Area Measurement System (WAMS)?", "id": "Jaringan PMU untuk memantau area luas." },
  { "en": "Modal analysis?", "id": "Studi properti dinamis dari suatu struktur." },
  { "en": "Operational Deflection Shape (ODS)?", "id": "Visualisasi getaran struktur pada frekuensi tertentu." },
  { "en": "Occupancy Grid Mapping?", "id": "Metode robotik untuk merepresentasikan lingkungan." },
  { "en": "Particle filter?", "id": "Metode Monte Carlo untuk estimasi Bayesian." },
  { "en": "Aplikasi particle filter?", "id": "Pelacakan objek dan lokalisasi robot." },
  { "en": "Unscented Kalman filter (UKF)?", "id": "Filter Kalman non-linear menggunakan unscented transform." },
  { "en": "Extended Kalman filter (EKF)?", "id": "Filter Kalman non-linear menggunakan linearisasi." },
  { "en": "Covariance intersection?", "id": "Menggabungkan estimasi tanpa mengetahui korelasi." },
  { "en": "Fuzzy logic?", "id": "Logika yang berurusan dengan 'derajat kebenaran'." },
  { "en": "Fuzzy c-means clustering?", "id": "Versi fuzzy dari algoritma k-means." },
  { "en": "Genetic algorithm?", "id": "Algoritma optimisasi yang terinspirasi oleh evolusi." },
  { "en": "Swarm intelligence?", "id": "Perilaku kolektif dari sistem terdesentralisasi." },
  { "en": "Ant Colony Optimization?", "id": "Algoritma optimisasi berdasarkan perilaku semut." },
  { "en": "Particle Swarm Optimization?", "id": "Algoritma optimisasi berdasarkan perilaku kawanan." },
  { "en": "Dempster-Shafer theory?", "id": "Kerangka kerja untuk penalaran di bawah ketidakpastian." },
  { "en": "Random field?", "id": "Generalisasi dari proses stokastik ke banyak dimensi." },
  { "en": "Markov Random Field (MRF)?", "id": "Random field dimana setiap simpul bersifat Markovian." },
  { "en": "Aplikasi MRF dalam citra?", "id": "Segmentasi citra, denoising, dan inpainting." },
  { "en": "Gibbs sampling?", "id": "Algoritma MCMC untuk sampling dari distribusi." },
  { "en": "Simulated annealing?", "id": "Metode optimisasi global probabilistik." },
  { "en": "Graph cuts?", "id": "Metode optimisasi untuk masalah seperti segmentasi." },
  { "en": "Dynamic time warping (DTW)?", "id": "Menyelaraskan dua sekuens temporal yang berbeda kecepatan." },
  { "en": "Aplikasi DTW?", "id": "Pengenalan ucapan, analisis deret waktu." },
  { "en": "Latent Dirichlet Allocation (LDA)?", "id": "Model generatif untuk menemukan topik dalam teks." },
  { "en": "Word embedding?", "id": "Representasi vektor dari kata-kata." },
  { "en": "Contoh word embedding?", "id": "Word2Vec, GloVe, dan FastText." },
  { "en": "BERT (Bidirectional Encoder Representations from Transformers)?", "id": "Model bahasa berbasis transformer yang kuat." },
  { "en": "Generative Pre-trained Transformer (GPT)?", "id": "Keluarga model bahasa generatif dari OpenAI." },
  { "en": "Natural language processing (NLP)?", "id": "Bidang AI untuk interaksi komputer-bahasa." },
  { "en": "Named Entity Recognition (NER)?", "id": "Mengidentifikasi entitas bernama (orang, tempat) dalam teks." },
  { "en": "Part-of-speech (POS) tagging?", "id": "Melabeli kata dengan kelas katanya (kata benda, kerja)." },
  { "en": "Sentiment analysis?", "id": "Menentukan sentimen (positif/negatif) dari teks." },
  { "en": "Reinforcement learning?", "id": "Belajar melalui interaksi dengan lingkungan (rewards)." },
  { "en": "Q-learning?", "id": "Algoritma reinforcement learning bebas model." },
  { "en": "Deep Q-Network (DQN)?", "id": "Menggunakan jaringan saraf untuk aproksimasi fungsi-Q." },
  { "en": "Policy Gradient Methods?", "id": "Metode RL yang secara langsung mengoptimalkan kebijakan." },
  { "en": "Actor-Critic methods?", "id": "Kombinasi policy gradient dan value-based methods." },
  { "en": "Multi-agent reinforcement learning?", "id": "Beberapa agen belajar secara bersamaan." },
  { "en": "Inverse reinforcement learning?", "id": "Menentukan fungsi reward dari perilaku yang diamati." },
  { "en": "Curriculum learning?", "id": "Melatih model dari contoh mudah ke sulit." },
  { "en": "Active learning?", "id": "Model secara aktif memilih data untuk dilabeli." },
  { "en": "Dimensionality reduction?", "id": "Mengurangi jumlah variabel acak." },
  { "en": "t-SNE?", "id": "Teknik reduksi dimensi untuk visualisasi data." },
  { "en": "UMAP (Uniform Manifold Approximation and Projection)?", "id": "Teknik reduksi dimensi modern yang efisien." },
  { "en": "Isomap?", "id": "Teknik reduksi dimensi non-linear berbasis jarak geodesic." },
  { "en": "Locally Linear Embedding (LLE)?", "id": "Reduksi dimensi yang mempertahankan lingkungan lokal." },
  { "en": "Information Geometry?", "id": "Studi geometri dari manifold model statistik." },
  { "en": "Fisher Information Matrix?", "id": "Metrik Riemannian pada manifold statistik." },
  { "en": "Amari-Chentsov tensor?", "id": "Struktur koneksi affine pada manifold statistik." },
  { "en": "Natural gradient?", "id": "Gradien yang menghormati geometri ruang parameter." },
  { "en": "Teori chaos?", "id": "Studi sistem dinamis yang sensitif terhadap kondisi awal." },
  { "en": "Strange attractor?", "id": "Set titik dimana sistem chaotic berevolusi." },
  { "en": "Lyapunov exponent?", "id": "Ukuran laju pemisahan lintasan yang sangat dekat." },
  { "en": "Arti Lyapunov exponent positif?", "id": "Sistem tersebut bersifat chaotic." },
  { "en": "Bifurcation diagram?", "id": "Peta yang menunjukkan kemungkinan nilai jangka panjang." },
  { "en": "Dimensi fraktal?", "id": "Ukuran 'kekasaran' atau kompleksitas dari suatu set." },
  { "en": "Contoh fraktal?", "id": "Himpunan Mandelbrot, Koch snowflake, dan Sierpinski triangle." },
  { "en": "Phase retrieval?", "id": "Merekonstruksi sinyal dari magnitudo transformasi Fouriernya." },
  { "en": "Kesulitan phase retrieval?", "id": "Informasi fasa hilang, masalahnya ill-posed." },
  { "en": "Aplikasi phase retrieval?", "id": "Kristalografi, astronomi, dan mikroskopi." },
  { "en": "Algoritma Gerchberg-Saxton?", "id": "Algoritma iteratif klasik untuk phase retrieval." },
  { "en": "Total Variation (TV) denoising?", "id": "Metode denoising yang menjaga ketajaman tepi." },
  { "en": "Fungsi TV regularization?", "id": "Mendorong solusi yang piecewise-constant." },
  { "en": "Bregman iteration?", "id": "Algoritma untuk menyelesaikan masalah optimisasi teregulasi." },
  { "en": "Algoritma MUSIC?", "id": "Algoritma estimasi frekuensi super-resolusi." },
  { "en": "Algoritma ESPRIT?", "id": "Estimasi parameter sinyal via teknik rotasi." },
  { "en": "Array processing?", "id": "Pemrosesan sinyal dari sekumpulan sensor." },
  { "en": "Arah kedatangan (Direction of Arrival)?", "id": "Estimasi arah datangnya sinyal." },
  { "en": "Functional Data Analysis (FDA)?", "id": "Analisis data dimana setiap observasi adalah fungsi." },
  { "en": "Functional PCA?", "id": "Versi Principal Component Analysis untuk data fungsional." },
  { "en": "Blind dereverberation?", "id": "Menghilangkan gema dari audio tanpa informasi ruangan." },
  { "en": "Homomorphic encryption?", "id": "Memungkinkan komputasi pada data terenkripsi." },
  { "en": "Aplikasi homomorphic encryption di ML?", "id": "Melatih model tanpa mendekripsi data (privacy)." },
  { "en": "Secure multi-party computation?", "id": "Komputasi bersama tanpa mengungkapkan data pribadi." },
  { "en": "Differential privacy?", "id": "Jaminan privasi statistik dalam analisis data." },
  { "en": "Data non-IID?", "id": "Data dengan distribusi yang tidak seragam." },
  { "en": "Masalah data non-IID di Federated Learning?", "id": "Membuat konvergensi model global menjadi sulit." },
  { "en": "Algoritma FedAvg?", "id": "Algoritma agregasi standar dalam federated learning." },
  { "en": "Scientific Machine Learning (SciML)?", "id": "Gabungan ML dengan pemodelan ilmiah." },
  { "en": "Universal Differential Equations?", "id": "Persamaan diferensial yang digabungkan dengan jaringan saraf." },
  { "en": "Network science?", "id": "Studi jaringan kompleks (graf)." },
  { "en": "Degree centrality?", "id": "Ukuran jumlah koneksi yang dimiliki simpul." },
  { "en": "Betweenness centrality?", "id": "Ukuran seberapa sering simpul menjadi jembatan." },
  { "en": "Closeness centrality?", "id": "Ukuran seberapa dekat simpul dengan simpul lain." },
  { "en": "Eigenvector centrality?", "id": "Ukuran pengaruh simpul dalam suatu jaringan." },
  { "en": "Modularity?", "id": "Ukuran kekuatan pembagian jaringan menjadi komunitas." },
  { "en": "Small-world network?", "id": "Jaringan dengan path pendek dan klastering tinggi." },
  { "en": "Scale-free network?", "id": "Jaringan dengan distribusi derajat power-law." },
  { "en": "Preferential attachment?", "id": "Mekanisme pertumbuhan jaringan (yang kaya makin kaya)." },
  { "en": "Tomosynthesis?", "id": "Bentuk tomografi 3D dengan sudut terbatas." },
  { "en": "Mammography?", "id": "Pencitraan X-ray payudara untuk deteksi kanker." },
  { "en": "Computer-Aided Detection (CADe)?", "id": "Sistem yang menandai area mencurigakan untuk radiolog." },
  { "en": "Computer-Aided Diagnosis (CADx)?", "id": "Sistem yang membantu diagnosis penyakit." },
  { "en": "Digital pathology?", "id": "Mendigitalkan slide jaringan untuk analisis." },
  { "en": "Histopathology?", "id": "Studi mikroskopis dari jaringan yang sakit." },
  { "en": "Cytology?", "id": "Studi mikroskopis dari sel tunggal." },
  { "en": "Fourier Ptychography?", "id": "Teknik mikroskopi komputasional untuk resolusi tinggi." },
  { "en": "Structured illumination microscopy (SIM)?", "id": "Teknik super-resolusi menggunakan pola cahaya." },
  { "en": "Optical coherence tomography (OCT)?", "id": "Pencitraan 3D resolusi tinggi berbasis interferometri." },
  { "en": "Aplikasi OCT?", "id": "Oftalmologi (pencitraan retina) dan kardiologi." },
  { "en": "Optoacoustic (photoacoustic) imaging?", "id": "Pencitraan berbasis efek fotoakustik." },
  { "en": "Bolometer?", "id": "Detektor untuk radiasi elektromagnetik (panas)." },
  { "en": "Charge-coupled device (CCD)?", "id": "Sensor citra yang umum digunakan." },
  { "en": "CMOS sensor?", "id": "Sensor citra alternatif untuk CCD." },
  { "en": "Perbedaan utama CCD dan CMOS?", "id": "Cara pembacaan muatan (piksel) berbeda." },
  { "en": "F-number (lensa)?", "id": "Rasio panjang fokus terhadap diameter aperture." },
  { "en": "Arti F-number kecil?", "id": "Aperture besar, lebih banyak cahaya masuk." },
  { "en": "Depth of field (DOF)?", "id": "Rentang jarak dimana objek tampak tajam." },
  { "en": "Hubungan Aperture dan DOF?", "id": "Depth of field yang lebih dalam (luas)." },
  { "en": "Circle of confusion?", "id": "Titik blur optik dari sumber titik." },
  { "en": "Hyperfocal distance?", "id": "Jarak fokus yang memberikan DOF maksimum." },
  { "en": "Point cloud registration?", "id": "Menyelaraskan beberapa point cloud menjadi satu." },
  { "en": "Algoritma Iterative Closest Point (ICP)?", "id": "Algoritma umum untuk registrasi point cloud." },
  { "en": "Volumetric video?", "id": "Video yang menangkap bentuk 3D secara real-time." },
  { "en": "Neural rendering?", "id": "Sintesis citra menggunakan model jaringan saraf." },
  { "en": "Implicit neural representation?", "id": "Merepresentasikan sinyal/bentuk sebagai fungsi neural." },
  { "en": "Contoh implicit neural representation?", "id": "NeRF (Neural Radiance Fields)." },
  { "en": "Diffeomorphic registration?", "id": "Registrasi non-rigid yang menjaga topologi." },
  { "en": "Atlas (citra medis)?", "id": "Citra referensi standar dari suatu anatomi." },
  { "en": "Multi-modal registration?", "id": "Menyelaraskan citra dari modalitas berbeda (CT-MRI)." },
  { "en": "Mutual information sebagai metrik registrasi?", "id": "Mengukur dependensi statistik antara intensitas citra." },
  { "en": "Sum of squared differences (SSD)?", "id": "Metrik sederhana untuk registrasi citra." },
  { "en": "Normalized cross-correlation (NCC)?", "id": "Metrik registrasi yang invarian terhadap kecerahan." },
  { "en": "Sampling Importance Resampling (SIR)?", "id": "Langkah kunci dalam algoritma particle filter." },
  { "en": "Rao-Blackwellization?", "id": "Teknik untuk meningkatkan efisiensi estimator." },
  { "en": "Cramér-Rao lower bound (CRLB)?", "id": "Batas bawah varians dari estimator." },
  { "en": "Estimator efisien?", "id": "Estimator yang variansnya mencapai batas Cramér-Rao." },
  { "en": "Sufficient statistic?", "id": "Statistik yang menangkap semua informasi tentang parameter." },
  { "en": "Exponential family?", "id": "Keluarga distribusi probabilitas dengan bentuk tertentu." },
  { "en": "Conjugate prior?", "id": "Prior yang menghasilkan posterior dari keluarga sama." },
  { "en": "Bayesian network?", "id": "Model grafis probabilitas untuk variabel acak." },
  { "en": "d-separation?", "id": "Kriteria independensi kondisional dalam Bayesian network." },
  { "en": "Junction tree algorithm?", "id": "Algoritma untuk inferensi eksak pada graf." },
  { "en": "ELBO pada VAE?", "id": "Evidence Lower Bound, objektif yang dioptimalkan." },
  { "en": "Reparameterization trick?", "id": "Memungkinkan backpropagation melalui simpul acak di VAE." },
  { "en": "Disentangled representation?", "id": "Representasi dimana faktor variasinya terpisah." },
  { "en": "World model?", "id": "Model internal yang dipelajari agen RL." },
  { "en": "Model-based reinforcement learning?", "id": "RL yang menggunakan model lingkungan." },
  { "en": "Model-free reinforcement learning?", "id": "RL yang belajar kebijakan secara langsung." },
  { "en": "Exploration-exploitation tradeoff?", "id": "Dilema antara mengeksplorasi aksi baru/memanfaatkan yang sudah diketahui." },
  { "en": "Epsilon-greedy policy?", "id": "Kebijakan sederhana untuk menyeimbangkan eksplorasi/eksploitasi." },
  { "en": "Intrinsic motivation?", "id": "Mendorong agen untuk menjelajah karena 'rasa ingin tahu'." },
  { "en": "Green AI?", "id": "Riset AI yang berfokus pada efisiensi komputasi." },
  { "en": "Red AI?", "id": "Riset AI yang mengabaikan biaya komputasi." },
  { "en": "Algorithmic Fairness?", "id": "Memastikan model tidak menghasilkan hasil diskriminatif." },
  { "en": "Bias (dalam AI)?", "id": "Kesalahan sistematis yang merugikan kelompok tertentu." },
  { "en": "Contoh bias dalam computer vision?", "id": "Model pengenalan wajah yang kurang akurat." },
  { "en": "Disparate Impact?", "id": "Metrik fairness yang mengukur dampak pada kelompok." },
  { "en": "Neuro-Symbolic AI?", "id": "Menggabungkan neural networks dengan penalaran simbolik." },
  { "en": "Keuntungan Neuro-Symbolic AI?", "id": "Lebih dapat diinterpretasikan dan efisien data." },
  { "en": "Structural Causal Model (SCM)?", "id": "Model grafis yang merepresentasikan hubungan kausal." },
  { "en": "Intervensi (kausal)?", "id": "Memanipulasi variabel untuk mengamati efeknya." },
  { "en": "Counterfactual?", "id": "Penalaran tentang 'apa yang akan terjadi jika'." },
  { "en": "Spherical Harmonics?", "id": "Basis fungsi ortogonal pada permukaan bola." },
  { "en": "Aplikasi Spherical Harmonics?", "id": "Grafik komputer, kosmologi, dan geofisika." },
  { "en": "Cosmic Microwave Background (CMB)?", "id": "Sisa radiasi dari Big Bang." },
  { "en": "Analisis CMB?", "id": "Dekomposisi sinyal pada bola (Spherical Harmonics)." },
  { "en": "Zernike polynomials?", "id": "Fungsi ortogonal yang mendeskripsikan aberasi optik." },
  { "en": "Bessel functions?", "id": "Solusi untuk persamaan diferensial Bessel." },
  { "en": "Aplikasi Bessel functions?", "id": "Analisis getaran membran, propagasi gelombang." },
  { "en": "Legendre polynomials?", "id": "Solusi polinomial untuk persamaan diferensial Legendre." },
  { "en": "Green's function?", "id": "Respon impuls dari operator diferensial linear." },
  { "en": "Optimisasi konveks?", "id": "Meminimalkan fungsi konveks pada set konveks." },
  { "en": "Pentingnya optimisasi konveks?", "id": "Setiap minimum lokal adalah minimum global." },
  { "en": "ADMM (Alternating Direction Method of Multipliers)?", "id": "Algoritma optimisasi untuk masalah terstruktur." },
  { "en": "Proximal algorithm?", "id": "Algoritma untuk meminimalkan fungsi non-smooth." },
  { "en": "Primal-dual method?", "id": "Metode optimisasi yang menyelesaikan masalah primal/dual." },
  { "en": "Subgradient method?", "id": "Generalisasi gradient descent untuk fungsi non-differentiable." },
  { "en": "Frank-Wolfe algorithm?", "id": "Algoritma optimisasi konveks bebas proyeksi." },
  { "en": "Interior-point method?", "id": "Kelas algoritma untuk optimisasi linear/non-linear." },
  { "en": "Kondisi Karush-Kuhn-Tucker (KKT)?", "id": "Kondisi perlu untuk optimalitas dalam optimisasi." },
  { "en": "Lagrangian dual problem?", "id": "Masalah optimisasi terkait yang memberikan batas bawah." },
  { "en": "Quantum Machine Learning (QML)?", "id": "Menggunakan komputasi kuantum untuk tugas machine learning." },
  { "en": "Quantum PCA?", "id": "Algoritma kuantum untuk Principal Component Analysis." },
  { "en": "Quantum SVM?", "id": "Algoritma kuantum untuk Support Vector Machines." },
  { "en": "Variational Quantum Eigensolver (VQE)?", "id": "Algoritma hibrida kuantum-klasik." },
  { "en": "Quantum Fourier Transform (QFT)?", "id": "Analog kuantum dari Discrete Fourier Transform." },
  { "en": "Algoritma Shor?", "id": "Algoritma kuantum untuk faktorisasi (berbasis QFT)." },
  { "en": "Differential geometry?", "id": "Studi geometri menggunakan kalkulus." },
  { "en": "Geodesic?", "id": "Jalur terpendek antara dua titik pada manifold." },
  { "en": "Aplikasi geodesic dalam citra?", "id": "Segmentasi menggunakan active contours." },
  { "en": "Curvature?", "id": "Ukuran seberapa banyak kurva/permukaan melengkung." },
  { "en": "Ricci flow?", "id": "Proses yang menghaluskan metrik suatu manifold." },
  { "en": "Formal verification?", "id": "Membuktikan atau menyangkal kebenaran sistem." },
  { "en": "Aplikasi formal verification di AI?", "id": "Memverifikasi keamanan dan robustnes dari model." },
  { "en": "Model checking?", "id": "Teknik verifikasi untuk sistem finite-state." },
  { "en": "Abstract interpretation?", "id": "Teori aproksimasi semantik dari program komputer." },
  { "en": "AI alignment problem?", "id": "Masalah memastikan AI bertindak sesuai keinginan manusia." },
  { "en": "Value learning?", "id": "Proses dimana AI mempelajari nilai-nilai manusia." },
  { "en": "Corrigibility?", "id": "Sifat AI yang tidak menolak untuk dikoreksi." },
  { "en": "Teorema Konvolusi?", "id": "Konvolusi di satu domain adalah perkalian." },
  { "en": "Teorema Konvolusi (Fourier Transform)?", "id": "Konvolusi waktu menjadi perkalian frekuensi." },
  { "en": "Teorema Konvolusi (Laplace Transform)?", "id": "Konvolusi waktu menjadi perkalian domain-s." },
  { "en": "Teorema Konvolusi (Z-Transform)?", "id": "Konvolusi waktu diskrit menjadi perkalian domain-z." },
  { "en": "Teorema Parseval?", "id": "Total energi sinyal sama di domain waktu/frekuensi." },
  { "en": "Teorema Plancherel?", "id": "Generalisasi dari teorema Parseval." },
  { "en": "Teorema Wiener-Khinchin?", "id": "Menghubungkan autokorelasi dengan Power Spectral Density." },
  { "en": "Prinsip Ketidakpastian Gabor?", "id": "Trade-off resolusi antara waktu dan frekuensi." },
  { "en": "Time-bandwidth product?", "id": "Ukuran kuantitatif dari prinsip ketidakpastian." },
  { "en": "Aturan Cramer?", "id": "Solusi eksplisit untuk sistem persamaan linear." },
  { "en": "Dekomposisi Cholesky?", "id": "Faktorisasi matriks simetris menjadi segitiga." },
  { "en": "Dekomposisi QR?", "id": "Faktorisasi matriks menjadi ortogonal dan segitiga." },
  { "en": "Dekomposisi LU?", "id": "Faktorisasi matriks menjadi lower dan upper." },
  { "en": "Dekomposisi Schur?", "id": "Faktorisasi matriks menjadi matriks segitiga." },
  { "en": "Bentuk Normal Jordan?", "id": "Representasi matriks dalam bentuk 'hampir' diagonal." },
  { "en": "Teorema Cayley-Hamilton?", "id": "Setiap matriks memenuhi persamaan karakteristiknya sendiri." },
  { "en": "Teorema Perron-Frobenius?", "id": "Teorema tentang eigenvector dari matriks positif." },
  { "en": "Teorema Lingkaran Gershgorin?", "id": "Memberikan batas untuk eigenvalue suatu matriks." },
  { "en": "Condition number?", "id": "Ukuran sensitivitas solusi terhadap error." },
  { "en": "Arti condition number besar?", "id": "Masalah tersebut 'ill-conditioned'." },
  { "en": "Vanishing gradient problem?", "id": "Gradien menjadi sangat kecil saat backpropagation." },
  { "en": "Exploding gradient problem?", "id": "Gradien menjadi sangat besar saat backpropagation." },
  { "en": "Gradient clipping?", "id": "Teknik untuk mengatasi exploding gradient." },
  { "en": "Skip connection?", "id": "Koneksi yang 'melompati' beberapa layer." },
  { "en": "Highway network?", "id": "Jaringan saraf dengan 'gerbang' informasi." },
  { "en": "Dense network (DenseNet)?", "id": "Setiap layer terhubung ke semua layer berikutnya." },
  { "en": "Teorema 'no free lunch'?", "id": "Tidak ada satu algoritma yang terbaik." },
  { "en": "Pisau Cukur Occam?", "id": "Prinsip memilih penjelasan yang paling sederhana." },
  { "en": "Bias-Variance tradeoff?", "id": "Trade-off antara error dari asumsi dan data." },
  { "en": "Model dengan bias tinggi?", "id": "Terlalu sederhana, mengalami underfitting." },
  { "en": "Model dengan varians tinggi?", "id": "Terlalu kompleks, mengalami overfitting." },
  { "en": "Ensemble learning?", "id": "Menggabungkan beberapa model untuk meningkatkan performa." },
  { "en": "Bagging?", "id": "Membuat ansambel dengan training pada subset data." },
  { "en": "Boosting?", "id": "Melatih model secara sekuensial untuk memperbaiki error." },
  { "en": "Stacking?", "id": "Menggunakan model untuk menggabungkan output model lain." },
  { "en": "Dropout sebagai ansambel?", "id": "Dapat diinterpretasikan sebagai ansambel dari subnetwork." },
  { "en": "Bayesian neural network?", "id": "Jaringan saraf yang bobotnya adalah distribusi." },
  { "en": "Gaussian process?", "id": "Model stokastik untuk regresi non-parametrik." },
  { "en": "Dirichlet process?", "id": "Distribusi probabilitas pada distribusi probabilitas." },
  { "en": "Chinese Restaurant Process?", "id": "Proses stokastik untuk clustering." },
  { "en": "Multi-task learning?", "id": "Melatih satu model untuk beberapa tugas sekaligus." },
  { "en": "Zero-shot learning?", "id": "Klasifikasi tanpa melihat sampel kelas tersebut." },
  { "en": "Self-distillation?", "id": "Model 'mengajar' dirinya sendiri." },
  { "en": "Knowledge graph?", "id": "Representasi pengetahuan terstruktur dalam bentuk graf." },
  { "en": "Common sense reasoning?", "id": "Kemampuan AI untuk membuat asumsi seperti manusia." },
  { "en": "Inductive Logic Programming (ILP)?", "id": "Sub-bidang AI yang menggunakan logika untuk learning." },
  { "en": "Abductive Reasoning?", "id": "Penalaran untuk mencari penjelasan terbaik." },
  { "en": "Analogical reasoning?", "id": "Penalaran berdasarkan analogi atau kemiripan." },
  { "en": "Vision Transformer (ViT)?", "id": "Arsitektur berbasis Transformer untuk tugas Computer Vision." },
  { "en": "Cara kerja ViT?", "id": "Membagi citra menjadi patch dan memprosesnya." },
  { "en": "Self-attention?", "id": "Mekanisme yang memungkinkan input berinteraksi satu sama lain." },
  { "en": "Multi-head attention?", "id": "Menjalankan mekanisme attention secara paralel." },
  { "en": "Positional embedding?", "id": "Menambahkan informasi posisi ke dalam input sekuensial." },
  { "en": "Mixture of Experts (MoE)?", "id": "Model yang mengaktifkan sebagian jaringan ('expert')." },
  { "en": "Keuntungan model MoE?", "id": "Kapasitas model besar dengan biaya komputasi rendah." },
  { "en": "SimCLR?", "id": "Framework simple untuk contrastive learning visual." },
  { "en": "MoCo (Momentum Contrast)?", "id": "Contrastive learning menggunakan dictionary dan momentum encoder." },
  { "en": "BYOL (Bootstrap Your Own Latent)?", "id": "Pendekatan self-supervised tanpa sampel negatif." },
  { "en": "Deepfake?", "id": "Media sintetis dimana seseorang digantikan orang lain." },
  { "en": "Teknologi di balik Deepfake?", "id": "Biasanya menggunakan autoencoder atau GAN." },
  { "en": "Style transfer?", "id": "Menerapkan gaya artistik satu citra ke citra lain." },
  { "en": "AdaIN (Adaptive Instance Normalization)?", "id": "Layer kunci dalam algoritma style transfer." },
  { "en": "VSLAM (Visual SLAM)?", "id": "SLAM yang hanya menggunakan input kamera." },
  { "en": "ORB-SLAM?", "id": "Sistem SLAM real-time berbasis fitur ORB." },
  { "en": "Visual odometry?", "id": "Memperkirakan posisi dengan menganalisis frame kamera." },
  { "en": "Fusi sensor?", "id": "Menggabungkan data dari beberapa sensor berbeda." },
  { "en": "Contoh fusi sensor?", "id": "Menggabungkan data kamera dan IMU (Inertial Measurement Unit)." },
  { "en": "Masalah invers (inverse problem)?", "id": "Menentukan sebab dari hasil yang diobservasi." },
  { "en": "Ill-posed problem?", "id": "Masalah invers yang solusinya tidak unik/stabil." },
  { "en": "Tikhonov regularization (ridge regression)?", "id": "Regularisasi L2 yang umum digunakan." },
  { "en": "LASSO?", "id": "Regularisasi L1 yang mendorong sparsity." },
  { "en": "Elastic Net?", "id": "Kombinasi dari regularisasi L1 dan L2." },
  { "en": "Diagram mata (eye diagram)?", "id": "Visualisasi kualitas sinyal komunikasi digital." },
  { "en": "Arti 'eye opening'?", "id": "Area terbuka di tengah diagram mata." },
  { "en": "Makna eye opening lebar?", "id": "Sinyal berkualitas baik dengan noise rendah." },
  { "en": "Diagram konstelasi?", "id": "Plot sinyal modulasi pada bidang kompleks." },
  { "en": "Channel equalization?", "id": "Memperbaiki distorsi yang disebabkan oleh kanal." },
  { "en": "Zero-Forcing Equalizer?", "id": "Equalizer yang mencoba membalik respons kanal." },
  { "en": "MMSE (Minimum Mean Square Error) Equalizer?", "id": "Meminimalkan error kuadrat rata-rata." },
  { "en": "Decision feedback equalizer (DFE)?", "id": "Menggunakan keputusan sebelumnya untuk menghilangkan ISI." },
  { "en": "Fixed-point arithmetic?", "id": "Representasi angka dengan jumlah desimal tetap." },
  { "en": "Floating-point arithmetic?", "id": "Representasi angka menggunakan mantissa dan eksponen." },
  { "en": "Keuntungan fixed-point?", "id": "Lebih cepat dan efisien daya pada hardware." },
  { "en": "Kekurangan fixed-point?", "id": "Rentang dinamis terbatas, potensi overflow." },
  { "en": "ASIC (Application-Specific Integrated Circuit)?", "id": "Chip yang dirancang untuk tugas spesifik." },
  { "en": "Keuntungan ASIC untuk DSP?", "id": "Performa dan efisiensi daya sangat tinggi." },
  { "en": "Systolic array?", "id": "Jaringan unit pemrosesan untuk komputasi paralel." },
  { "en": "CORDIC?", "id": "Algoritma efisien untuk fungsi trigonometri." },
  { "en": "Timbre?", "id": "Kualitas suara yang membedakan instrumen musik." },
  { "en": "ADSR envelope?", "id": "Attack, Decay, Sustain, Release." },
  { "en": "Acoustic scene classification?", "id": "Mengidentifikasi lingkungan dari rekaman audio." },
  { "en": "Sound event detection?", "id": "Mendeteksi kapan suara tertentu terjadi." },
  { "en": "Audio captioning?", "id": "Menghasilkan deskripsi teks dari konten audio." },
  { "en": "Fractional calculus?", "id": "Generalisasi turunan dan integral ke orde non-bilangan bulat." },
  { "en": "Fejér kernel?", "id": "Rata-rata dari kernel Dirichlet." },
  { "en": "Gibbs ringing phenomenon?", "id": "Artefak osilasi dekat diskontinuitas tajam." },
  { "en": "Lanczos resampling?", "id": "Metode interpolasi berbasis fungsi sinc berjendela." },
  { "en": "Analytic signal?", "id": "Sinyal bernilai kompleks tanpa komponen frekuensi negatif." },
  { "en": "Phase unwrapping?", "id": "Merekonstruksi fasa absolut dari fasa 'wrapped'." },
  { "en": "Filter separable?", "id": "Filter 2D yang dapat dipecah jadi dua filter 1D." },
  { "en": "Keuntungan filter separable?", "id": "Komputasi jauh lebih efisien." },
  { "en": "Steerable filter?", "id": "Filter yang dapat diorientasikan ke segala arah." },
  { "en": "Bag-of-visual-words (BoVW)?", "id": "Metode representasi citra dengan menghitung 'kata' visual." },
  { "en": "VLAD?", "id": "Peningkatan dari model BoVW." },
  { "en": "Fisher Vector?", "id": "Representasi citra yang kuat, peningkatan dari BoVW." },
  { "en": "Hashing peka lokalitas (LSH)?", "id": "Teknik hashing untuk pencarian tetangga terdekat." },
  { "en": "Pohon kd (k-d tree)?", "id": "Struktur data untuk pencarian di ruang k-dimensi." },
  { "en": "Kutukan dimensi (curse of dimensionality)?", "id": "Masalah yang timbul saat menganalisis data high-dimensional." },
  { "en": "Sub-pixel accuracy?", "id": "Estimasi posisi dengan presisi lebih tinggi dari piksel." },
  { "en": "Inverse synthetic-aperture radar (ISAR)?", "id": "Teknik radar untuk mencitrakan target bergerak." },
  { "en": "Bistatic radar?", "id": "Sistem radar dengan pemancar dan penerima terpisah." },
  { "en": "Clutter (radar)?", "id": "Gema yang tidak diinginkan dari lingkungan." },
  { "en": "MTI (Moving Target Indication)?", "id": "Teknik untuk menekan clutter stasioner." },
  { "en": "Pulse-Doppler radar?", "id": "Radar yang mengukur jangkauan dan kecepatan target." },
  { "en": "Super-resolution microscopy?", "id": "Teknik mikroskop melampaui batas difraksi." },
  { "en": "Shack-Hartmann wavefront sensor?", "id": "Sensor untuk mengukur aberasi optik." },
  { "en": "Optik adaptif?", "id": "Teknologi untuk mengoreksi distorsi optik." },
  { "en": "Speckle imaging?", "id": "Teknik untuk mengatasi blur akibat turbulensi atmosfer." },
  { "en": "Lucky imaging?", "id": "Memilih frame paling tajam dari video." },
  { "en": "Image registration?", "id": "Menyelaraskan dua atau lebih citra." },
  { "en": "Rigid registration?", "id": "Penyelarasan hanya dengan translasi dan rotasi." },
  { "en": "Affine registration?", "id": "Memungkinkan scaling dan shearing selain rigid." },
  { "en": "Deformable (non-rigid) registration?", "id": "Memungkinkan transformasi lokal yang kompleks." },
  { "en": "Forensik citra?", "id": "Analisis citra untuk pengumpulan bukti digital." },
  { "en": "Deteksi copy-move?", "id": "Menemukan bagian citra yang disalin & ditempel." },
  { "en": "Deteksi splicing?", "id": "Mendeteksi penggabungan dua citra yang berbeda." },
  { "en": "Photo Response Non-Uniformity (PRNU)?", "id": "Noise pola tetap unik untuk setiap sensor kamera." },
  { "en": "Fungsi PRNU dalam forensik?", "id": "Mengidentifikasi kamera sumber dari sebuah citra." },
  { "en": "Steganalysis?", "id": "Ilmu mendeteksi adanya pesan tersembunyi." },
  { "en": "Watermark rapuh (fragile)?", "id": "Watermark yang rusak jika citra dimodifikasi." },
  { "en": "Watermark kokoh (robust)?", "id": "Watermark yang bertahan terhadap modifikasi citra." },
  { "en": "Perceptual hashing?", "id": "Membuat 'sidik jari' unik untuk konten visual." },
  { "en": "LIDAR?", "id": "Penginderaan jauh aktif menggunakan pulsa laser." },
  { "en": "Pan-sharpening?", "id": "Menggabungkan citra pankromatik dan multispektral." },
  { "en": "Koreksi atmosferik?", "id": "Menghilangkan efek distorsi atmosfer dari citra." },
  { "en": "Orthorectification?", "id": "Mengoreksi distorsi geometris dari citra satelit." },
  { "en": "Quantum Image Processing (QIP)?", "id": "Pemrosesan citra menggunakan prinsip mekanika kuantum." },
  { "en": "FRQI (Flexible Representation of Quantum Images)?", "id": "Metode untuk merepresentasikan citra pada komputer kuantum." },
  { "en": "Just Noticeable Difference (JND)?", "id": "Perubahan stimulus terkecil yang dapat dirasakan." },
  { "en": "Model psychovisual?", "id": "Model matematis dari persepsi visual manusia." },
  { "en": "Saliency map?", "id": "Peta yang menyoroti wilayah paling menonjol." },
  { "en": "F0 estimation?", "id": "Memperkirakan frekuensi pitch dasar dalam audio." },
  { "en": "Chroma features?", "id": "Fitur audio yang merepresentasikan 12 kelas pitch." },
  { "en": "Onset detection?", "id": "Mendeteksi awal dari sebuah event musikal." },
  { "en": "Beat tracking?", "id": "Mengidentifikasi ketukan dalam sinyal musik." },
  { "en": "Penyearah (Rectifier)?", "id": "Mengubah AC menjadi DC." },
  { "en": "Penyearah Setengah Gelombang?", "id": "Hanya melewatkan setengah siklus gelombang AC." },
  { "en": "Penyearah Gelombang Penuh (Jembatan)?", "id": "Menggunakan empat dioda untuk menyearahkan kedua siklus." },
  { "en": "Tegangan Ripple?", "id": "Variasi AC yang tersisa pada output DC." },
  { "en": "Faktor Ripple?", "id": "Ukuran efektivitas dari sebuah penyearah." },
  { "en": "Filter Kapasitor?", "id": "Menghaluskan tegangan DC hasil penyearahan." },
  { "en": "Regulator Tegangan?", "id": "Menjaga tegangan output tetap konstan." },
  { "en": "Regulator Linier?", "id": "Menggunakan transistor sebagai resistor variabel." },
  { "en": "Regulator Switching?", "id": "Menggunakan saklar elektronik untuk efisiensi tinggi." },
  { "en": "Inverter?", "id": "Mengubah DC menjadi AC." },
  { "en": "Inverter Gelombang Kotak?", "id": "Menghasilkan output AC berbentuk gelombang kotak." },
  { "en": "Inverter Gelombang Sinus Termodifikasi?", "id": "Bentuk gelombang antara kotak dan sinus murni." },
  { "en": "Inverter Gelombang Sinus Murni?", "id": "Menghasilkan output AC sinusoidal berkualitas tinggi." },
  { "en": "Pulse Width Modulation (PWM)?", "id": "Teknik mengontrol daya dengan mengubah lebar pulsa." },
  { "en": "DC-DC Converter?", "id": "Mengubah level tegangan DC ke level lain." },
  { "en": "Buck Converter?", "id": "Menurunkan tegangan DC (step-down)." },
  { "en": "Boost Converter?", "id": "Menaikkan tegangan DC (step-up)." },
  { "en": "Buck-Boost Converter?", "id": "Dapat menaikkan atau menurunkan tegangan DC." },
  { "en": "Cuk Converter?", "id": "Converter DC-DC dengan polaritas output terbalik." },
  { "en": "Flyback Converter?", "id": "Converter DC-DC terisolasi berbasis buck-boost." },
  { "en": "Forward Converter?", "id": "Converter DC-DC terisolasi berbasis buck." },
  { "en": "Continuous Conduction Mode (CCM)?", "id": "Arus induktor tidak pernah mencapai nol." },
  { "en": "Discontinuous Conduction Mode (DCM)?", "id": "Arus induktor mencapai nol selama siklus." },
  { "en": "Thyristor?", "id": "Saklar semikonduktor terkendali untuk daya tinggi." },
  { "en": "Kontrol Sudut Fasa?", "id": "Mengontrol daya AC dengan menunda penyulutan thyristor." },
  { "en": "Cycloconverter?", "id": "Mengubah frekuensi AC secara langsung." },
  { "en": "Matrix Converter?", "id": "Converter AC-AC langsung tanpa penyimpan energi." },
  { "en": "Snubber Circuit?", "id": "Melindungi saklar elektronik dari lonjakan tegangan." },
  { "en": "Heat Sink?", "id": "Komponen untuk membuang panas dari piranti daya." },
  { "en": "IGBT (Insulated Gate Bipolar Transistor)?", "id": "Piranti daya gabungan keunggulan BJT dan MOSFET." },
  { "en": "Power MOSFET?", "id": "MOSFET yang dirancang untuk aplikasi daya tinggi." },
  { "en": "GTO (Gate Turn-Off Thyristor)?", "id": "Thyristor yang bisa dimatikan lewat gerbang." },
  { "en": "Active Power Filter?", "id": "Menggunakan elektronika daya untuk mitigasi harmonisa." },
  { "en": "Shunt Active Filter?", "id": "Terhubung paralel untuk mengkompensasi arus harmonisa." },
  { "en": "Series Active Filter?", "id": "Terhubung seri untuk mengkompensasi tegangan harmonisa." },
  { "en": "UPQC (Unified Power Quality Conditioner)?", "id": "Menggabungkan filter aktif seri dan shunt." },
  { "en": "Kendali Motor (Drives)?", "id": "Sistem untuk mengontrol kecepatan/torsi motor listrik." },
  { "en": "Kendali Skalar (V/Hz)?", "id": "Menjaga rasio tegangan/frekuensi konstan." },
  { "en": "Kendali Vektor (Field-Oriented Control)?", "id": "Mengontrol komponen fluks dan torsi secara independen." },
  { "en": "Direct Torque Control (DTC)?", "id": "Mengontrol fluks dan torsi secara langsung." },
  { "en": "Space Vector Modulation (SVM)?", "id": "Teknik PWM canggih untuk inverter tiga fasa." },
  { "en": "Encoder?", "id": "Sensor umpan balik untuk posisi dan kecepatan." },
  { "en": "Resolver?", "id": "Sensor posisi analog yang sangat kokoh." },
  { "en": "Pengereman Regeneratif (Regenerative Braking)?", "id": "Mengembalikan energi pengereman ke sumber." },
  { "en": "Pengereman Dinamis (Dynamic Braking)?", "id": "Membuang energi pengereman sebagai panas di resistor." },
  { "en": "Chopper (DC)?", "id": "Rangkaian untuk mengontrol motor DC." },
  { "en": "Inverter Sumber Tegangan (VSI)?", "id": "Inverter yang inputnya adalah sumber tegangan DC." },
  { "en": "Inverter Sumber Arus (CSI)?", "id": "Inverter yang inputnya adalah sumber arus DC." },
  { "en": "Multilevel Inverter?", "id": "Menghasilkan output AC dengan banyak level tegangan." },
  { "en": "Diode-Clamped Multilevel Inverter?", "id": "Topologi inverter multilevel menggunakan dioda." },
  { "en": "Cascaded H-Bridge Inverter?", "id": "Topologi inverter multilevel menggunakan jembatan-H." },
  { "en": "Soft Switching?", "id": "Teknik switching saat tegangan/arus nol." },
  { "en": "Zero Voltage Switching (ZVS)?", "id": "Menyalakan saklar saat tegangan nol." },
  { "en": "Zero Current Switching (ZCS)?", "id": "Menyalakan/mematikan saklar saat arus nol." },
  { "en": "Resonant Converter?", "id": "Converter yang menggunakan rangkaian resonan LC." },
  { "en": "Power Factor Correction (PFC)?", "id": "Membuat beban elektronik tampak seperti resistor." },
  { "en": "Active PFC?", "id": "Menggunakan converter switching untuk koreksi faktor daya." },
  { "en": "Passive PFC?", "id": "Menggunakan induktor dan kapasitor untuk koreksi." },
  { "en": "Uninterruptible Power Supply (UPS)?", "id": "Menyediakan daya cadangan saat listrik padam." },
  { "en": "Online UPS?", "id": "Selalu menyuplai daya melalui baterai dan inverter." },
  { "en": "Offline (Standby) UPS?", "id": "Beralih ke baterai saat listrik padam." },
  { "en": "Line-Interactive UPS?", "id": "UPS offline dengan regulasi tegangan." },
  { "en": "Grid-Tied Inverter?", "id": "Inverter yang tersinkronisasi dengan jaringan listrik." },
  { "en": "Anti-Islanding Protection?", "id": "Mencegah inverter menyuplai daya ke jaringan mati." },
  { "en": "Maximum Power Point Tracking (MPPT)?", "id": "Algoritma untuk mendapat daya maksimum dari PV." },
  { "en": "Perturb and Observe (P&O)?", "id": "Metode MPPT yang umum digunakan." },
  { "en": "Incremental Conductance?", "id": "Metode MPPT lain yang lebih akurat." },
  { "en": "Solid State Transformer (SST)?", "id": "Transformator berbasis elektronika daya." },
  { "en": "Kendali Empat Kuadran (Four-Quadrant)?", "id": "Mengontrol motor di semua mode (maju/mundur, motor/generator)." },
  { "en": "Interleaved Converter?", "id": "Mengoperasikan beberapa converter secara paralel." },
  { "en": "Synchronous Rectifier?", "id": "Mengganti dioda dengan MOSFET untuk efisiensi." },
  { "en": "Dead Time?", "id": "Waktu tunda untuk mencegah hubung singkat." },
  { "en": "Gate Driver?", "id": "Sirkuit untuk menyalakan dan mematikan saklar daya." },
  { "en": "Safe Operating Area (SOA)?", "id": "Batas tegangan-arus aman untuk piranti daya." },
  { "en": "Thermal Management?", "id": "Manajemen panas pada sistem elektronika daya." },
  { "en": "EMI (Electromagnetic Interference) Filter?", "id": "Filter untuk menekan noise dari switching." },
  { "en": "Wireless Power Transfer?", "id": "Mentransfer daya listrik tanpa kabel." },
  { "en": "Inductive Coupling?", "id": "Metode transfer daya nirkabel jarak dekat." },
  { "en": "Magnetic Resonance Coupling?", "id": "Metode transfer daya nirkabel jarak menengah." },
  { "en": "SiC (Silicon Carbide)?", "id": "Material semikonduktor celah-pita lebar." },
  { "en": "GaN (Gallium Nitride)?", "id": "Material semikonduktor lain untuk piranti daya." },
  { "en": "Keuntungan SiC/GaN?", "id": "Frekuensi switching lebih tinggi, rugi lebih rendah." },
  { "en": "Supercapacitor?", "id": "Kapasitor dengan kapasitansi sangat tinggi." },
  { "en": "Battery Management System (BMS)?", "id": "Sistem elektronik untuk mengelola baterai." },
  { "en": "State of Charge (SoC)?", "id": "Tingkat pengisian baterai saat ini." },
  { "en": "State of Health (SoH)?", "id": "Ukuran 'kesehatan' atau penuaan baterai." },
  { "en": "Cell Balancing?", "id": "Menyamakan tegangan sel dalam paket baterai." },
  { "en": "Electric Vehicle (EV) Charger?", "id": "Peralatan untuk mengisi ulang baterai mobil listrik." },
  { "en": "On-Board Charger (OBC)?", "id": "Charger yang terpasang di dalam kendaraan." },
  { "en": "DC Fast Charger?", "id": "Mengisi baterai secara langsung dengan daya DC tinggi." },
  { "en": "Vehicle-to-Grid (V2G)?", "id": "Mobil listrik menyuplai daya kembali ke jaringan." },
  { "en": "Microgrid?", "id": "Jaringan listrik lokal yang dapat beroperasi mandiri." },
  { "en": "Energy Storage System (ESS)?", "id": "Sistem untuk menyimpan energi listrik." },
  { "en": "Pumped Hydro Storage?", "id": "Menyimpan energi dengan memompa air ke tempat tinggi." },
  { "en": "Kontrol Digital?", "id": "Implementasi kontroler menggunakan komputer digital." },
  { "en": "Pemetaan Bidang-s ke Bidang-z?", "id": "Mentransformasikan sistem kontinu ke diskrit." },
  { "en": "Bilinear Transform?", "id": "Metode pemetaan populer dari s-plane ke z-plane." },
  { "en": "Tustin's Method?", "id": "Nama lain untuk Bilinear Transform." },
  { "en": "Frequency Warping?", "id": "Efek distorsi frekuensi akibat bilinear transform." },
  { "en": "Kontroler Deadbeat?", "id": "Kontroler digital yang mencapai setpoint secepat mungkin." },
  { "en": "Kontroler Dahlin?", "id": "Kontroler digital lain dengan respons tertentu." },
  { "en": "Aliasing (Sistem Kontrol)?", "id": "Kesalahan akibat laju sampling terlalu rendah." },
  { "en": "Filter Anti-Aliasing?", "id": "Filter low-pass sebelum proses sampling." },
  { "en": "Reconstruction Filter?", "id": "Filter low-pass setelah konverter digital-ke-analog." },
  { "en": "Sistem Multivariable?", "id": "Sistem dengan banyak input dan banyak output (MIMO)." },
  { "en": "Matriks Fungsi Transfer?", "id": "Generalisasi fungsi transfer untuk sistem MIMO." },
  { "en": "Decoupling Control?", "id": "Desain kontroler agar input hanya mempengaruhi outputnya." },
  { "en": "Relative Gain Array (RGA)?", "id": "Metode untuk memilih pasangan input-output." },
  { "en": "Singular Value Decomposition (SVD) dalam Kontrol?", "id": "Menganalisis gain sistem MIMO." },
  { "en": "Kontrol Robust?", "id": "Desain kontroler yang tahan terhadap ketidakpastian model." },
  { "en": "Model Ketidakpastian?", "id": "Representasi matematis dari error pemodelan." },
  { "en": "Small Gain Theorem?", "id": "Kondisi stabilitas untuk sistem dengan ketidakpastian." },
  { "en": "H-infinity Control?", "id": "Metode desain kontrol robust." },
  { "en": "Mu-synthesis?", "id": "Teknik kontrol robust untuk ketidakpastian terstruktur." },
  { "en": "Sliding Mode Control (SMC)?", "id": "Teknik kontrol non-linear yang sangat robust." },
  { "en": "Sliding Surface?", "id": "Permukaan di state-space yang dituju sistem." },
  { "en": "Chattering?", "id": "Osilasi frekuensi tinggi pada sliding mode control." },
  { "en": "Boundary Layer?", "id": "Metode untuk mengurangi chattering." },
  { "en": "Kontrol Model Prediktif (MPC)?", "id": "Menggunakan model untuk memprediksi dan mengoptimalkan masa depan." },
  { "en": "Prediction Horizon?", "id": "Jangka waktu prediksi di masa depan." },
  { "en": "Control Horizon?", "id": "Jangka waktu aksi kontrol yang dioptimalkan." },
  { "en": "Receding Horizon?", "id": "Prinsip MPC: terapkan aksi pertama, ulangi." },
  { "en": "Fungsi Biaya (Cost Function)?", "id": "Fungsi yang diminimalkan dalam kontrol optimal/MPC." },
  { "en": "Model Internal Control (IMC)?", "id": "Struktur desain kontroler berbasis model." },
  { "en": "Gain Scheduling?", "id": "Mengubah parameter kontroler berdasarkan kondisi operasi." },
  { "en": "Feedback Linearization?", "id": "Mengubah sistem non-linear menjadi ekuivalen linear." },
  { "en": "Backstepping?", "id": "Metode desain rekursif untuk sistem non-linear." },
  { "en": "Passivity-Based Control?", "id": "Desain kontrol berdasarkan sifat pasif sistem." },
  { "en": "Sistem Hibrida?", "id": "Sistem yang memiliki dinamika kontinu dan diskrit." },
  { "en": "Automata?", "id": "Model matematis untuk merepresentasikan logika diskrit." },
  { "en": "Petri Nets?", "id": "Model grafis untuk sistem dengan event diskrit." },
  { "en": "Identifikasi Sistem?", "id": "Membangun model matematis dari data eksperimen." },
  { "en": "Model ARX?", "id": "Model AutoRegressive with eXogenous input." },
  { "en": "Metode Least Squares?", "id": "Metode umum untuk estimasi parameter model." },
  { "en": "Recursive Least Squares?", "id": "Versi online dari metode least squares." },
  { "en": "Pseudo-Random Binary Sequence (PRBS)?", "id": "Sinyal uji yang baik untuk identifikasi sistem." },
  { "en": "Kontrol Stokastik?", "id": "Kontrol sistem yang dipengaruhi oleh keacakan." },
  { "en": "Linear-Quadratic-Gaussian (LQG) Control?", "id": "Gabungan LQR dan filter Kalman." },
  { "en": "Teorema Separasi (Stokastik)?", "id": "Desain LQR dan Kalman filter dapat dipisahkan." },
  { "en": "Dynamic Programming?", "id": "Metode optimisasi untuk masalah keputusan sekuensial." },
  { "en": "Persamaan Hamilton-Jacobi-Bellman (HJB)?", "id": "Persamaan dasar dalam teori kontrol optimal." },
  { "en": "Pontryagin's Minimum Principle?", "id": "Kondisi perlu untuk kontrol optimal." },
  { "en": "Bang-Bang Control?", "id": "Kontroler yang beralih antara nilai ekstrim." },
  { "en": "Time-Optimal Control?", "id": "Mencapai target dalam waktu sesingkat mungkin." },
  { "en": "Fuel-Optimal Control?", "id": "Mencapai target dengan penggunaan bahan bakar minimum." },
  { "en": "Sistem Waktu Tunda (Time-Delay)?", "id": "Sistem dimana output bergantung pada input masa lalu." },
  { "en": "Smith Predictor?", "id": "Struktur kontrol untuk mengatasi waktu tunda." },
  { "en": "Sistem Fraksional-Orde?", "id": "Sistem yang dimodelkan dengan turunan fraksional." },
  { "en": "Kontrol Jaringan (Networked Control System)?", "id": "Sistem kontrol dengan loop tertutup lewat jaringan." },
  { "en": "Event-Triggered Control?", "id": "Aksi kontrol hanya saat event penting terjadi." },
  { "en": "Sistem Fuzzy?", "id": "Sistem berbasis aturan logika fuzzy." },
  { "en": "Fuzzification?", "id": "Mengubah input numerik menjadi derajat keanggotaan." },
  { "en": "Inference Engine?", "id": "Menerapkan aturan fuzzy pada input." },
  { "en": "Defuzzification?", "id": "Mengubah hasil fuzzy kembali ke nilai numerik." },
  { "en": "Adaptive Neuro-Fuzzy Inference System (ANFIS)?", "id": "Menggabungkan jaringan saraf dan sistem fuzzy." },
  { "en": "Kontrol Cerdas (Intelligent Control)?", "id": "Menggunakan metode AI dalam sistem kontrol." },
  { "en": "Sistem Diskrit-Event?", "id": "Sistem dengan keadaan yang berubah akibat event." },
  { "en": "Fault Detection and Diagnosis (FDD)?", "id": "Mendeteksi dan mengisolasi kegagalan dalam sistem." },
  { "en": "Fault-Tolerant Control?", "id": "Menjaga stabilitas sistem meski ada kegagalan." },
  { "en": "Residual Generation?", "id": "Sinyal yang dihasilkan untuk deteksi kegagalan." },
  { "en": "Parity Space Method?", "id": "Metode FDD berbasis redundansi analitik." },
  { "en": "Observer-Based FDD?", "id": "Menggunakan observer untuk mendeteksi kegagalan." },
  { "en": "Sistem Agen-Banyak (Multi-Agent)?", "id": "Sekelompok agen otonom yang berinteraksi." },
  { "en": "Kontrol Konsensus?", "id": "Membuat semua agen mencapai kesepakatan." },
  { "en": "Formation Control?", "id": "Mengontrol sekelompok agen untuk membentuk formasi." },
  { "en": "Flocking?", "id": "Perilaku kolektif terkoordinasi seperti kawanan burung." },
  { "en": "Sistem Terdistribusi (Distributed System)?", "id": "Sistem dengan banyak komponen terdesentralisasi." },
  { "en": "Kontrol Kooperatif?", "id": "Agen-agen bekerja sama untuk tujuan bersama." },
  { "en": "Game Theory dalam Kontrol?", "id": "Menganalisis interaksi strategis antara agen." },
  { "en": "Nash Equilibrium?", "id": "Kondisi dimana tidak ada agen untung." },
  { "en": "Differential Game?", "id": "Permainan dimana dinamika keadaan kontinu." },
  { "en": "Teleoperation?", "id": "Mengontrol robot dari jarak jauh." },
  { "en": "Haptics?", "id": "Memberikan umpan balik sentuhan pada operator." },
  { "en": "Bilateral Control?", "id": "Kontrol dua arah antara master dan slave." },
  { "en": "Transparency (Teleoperation)?", "id": "Membuat operator merasa seolah-olah hadir." },
  { "en": "Sistem Cyber-Physical (CPS)?", "id": "Integrasi erat antara komputasi dan proses fisik." },
  { "en": "Co-design (CPS)?", "id": "Mendesain aspek kontrol dan komputasi bersamaan." },
  { "en": "Reachability Analysis?", "id": "Menentukan set keadaan yang dapat dicapai." },
  { "en": "Invariant Set?", "id": "Set keadaan dimana sistem akan tetap berada." },
  { "en": "Lyapunov Stability (Diskrit)?", "id": "Analisis stabilitas untuk sistem waktu diskrit." },
  { "en": "LaSalle's Invariance Principle?", "id": "Generalisasi dari teori stabilitas Lyapunov." },
  { "en": "Popov Criterion?", "id": "Kriteria stabilitas untuk sistem dengan non-linearitas." },
  { "en": "Circle Criterion?", "id": "Kriteria stabilitas lain untuk sistem non-linear." },
  { "en": "L-infinity Norm?", "id": "Ukuran nilai puncak dari sebuah sinyal." },
  { "en": "L2 Norm?", "id": "Ukuran energi dari sebuah sinyal." },
  { "en": "Observabilitas Kalman?", "id": "Dekomposisi sistem menjadi bagian teramati/tak teramati." },
  { "en": "Dualitas?", "id": "Prinsip hubungan antara keterkontrolan dan keteramatan." },
  { "en": "Differential Flatness?", "id": "Sistem non-linear yang dapat disederhanakan." },
  { "en": "Real-Time Operating System (RTOS)?", "id": "Sistem operasi untuk aplikasi dengan batasan waktu." },
  { "en": "Worst-Case Execution Time (WCET)?", "id": "Waktu eksekusi terlama dari sebuah tugas." },
  { "en": "Rate-Monotonic Scheduling?", "id": "Algoritma penjadwalan prioritas statis." },
  { "en": "Generasi Seluler (1G, 2G, 3G, 4G, 5G)?", "id": "Evolusi teknologi komunikasi seluler." },
  { "en": "1G (First Generation)?", "id": "Komunikasi suara analog." },
  { "en": "2G (Second Generation)?", "id": "Komunikasi suara digital (misalnya, GSM)." },
  { "en": "3G (Third Generation)?", "id": "Suara digital dan pengenalan data mobile (misalnya, UMTS)." },
  { "en": "4G (Fourth Generation)?", "id": "Data mobile berkecepatan tinggi (misalnya, LTE)." },
  { "en": "5G (Fifth Generation)?", "id": "Kecepatan sangat tinggi, latensi rendah, konektivitas masif." },
  { "en": "LTE (Long-Term Evolution)?", "id": "Standar utama untuk teknologi 4G." },
  { "en": "5G NR (New Radio)?", "id": "Antarmuka udara standar untuk 5G." },
  { "en": "Small Cells?", "id": "Stasiun pangkalan berdaya rendah untuk meningkatkan kapasitas." },
  { "en": "Femtocell/Picocell?", "id": "Jenis small cell untuk cakupan dalam ruangan." },
  { "en": "HetNet (Heterogeneous Network)?", "id": "Jaringan yang terdiri dari berbagai jenis sel." },
  { "en": "Massive MIMO?", "id": "Sistem MIMO dengan jumlah antena sangat banyak." },
  { "en": "Millimeter Wave (mmWave)?", "id": "Pita frekuensi sangat tinggi untuk 5G." },
  { "en": "Network Slicing?", "id": "Membuat beberapa jaringan virtual di atas infrastruktur fisik." },
  { "en": "Edge Computing?", "id": "Memindahkan komputasi lebih dekat ke pengguna." },
  { "en": "Internet of Things (IoT)?", "id": "Jaringan perangkat fisik yang saling terhubung." },
  { "en": "LPWAN (Low-Power Wide-Area Network)?", "id": "Jaringan nirkabel untuk perangkat IoT." },
  { "en": "LoRaWAN?", "id": "Salah satu protokol LPWAN populer." },
  { "en": "NB-IoT (Narrowband-IoT)?", "id": "Standar LPWAN berbasis seluler." },
  { "en": "Zigbee?", "id": "Protokol nirkabel untuk jaringan area personal." },
  { "en": "Bluetooth Low Energy (BLE)?", "id": "Versi Bluetooth hemat daya untuk IoT." },
  { "en": "6LoWPAN?", "id": "Memungkinkan IPv6 pada jaringan nirkabel daya rendah." },
  { "en": "MQTT (Message Queuing Telemetry Transport)?", "id": "Protokol pesan ringan untuk IoT." },
  { "en": "CoAP (Constrained Application Protocol)?", "id": "Protokol aplikasi untuk perangkat terbatas." },
  { "en": "Wi-Fi (Wireless Fidelity)?", "id": "Standar IEEE 802.11 untuk WLAN." },
  { "en": "Wi-Fi 6 (802.11ax)?", "id": "Generasi terbaru Wi-Fi dengan efisiensi tinggi." },
  { "en": "OFDMA (Orthogonal Frequency-Division Multiple Access)?", "id": "Memungkinkan banyak pengguna berbagi kanal sekaligus." },
  { "en": "BSS Color?", "id": "Fitur di Wi-Fi 6 untuk mengurangi interferensi." },
  { "en": "Target Wake Time (TWT)?", "id": "Fitur hemat daya pada Wi-Fi 6." },
  { "en": "WPA3 (Wi-Fi Protected Access 3)?", "id": "Standar keamanan Wi-Fi terbaru." },
  { "en": "Mesh Networking?", "id": "Setiap node dapat berkomunikasi dengan node lain." },
  { "en": "Software-Defined Networking (SDN)?", "id": "Memisahkan bidang kontrol dan data jaringan." },
  { "en": "Network Function Virtualization (NFV)?", "id": "Virtualisasi fungsi jaringan (misalnya, router, firewall)." },
  { "en": "OpenFlow?", "id": "Protokol komunikasi antara kontroler dan switch SDN." },
  { "en": "Data Plane?", "id": "Bagian jaringan yang meneruskan paket data." },
  { "en": "Control Plane?", "id": "Bagian jaringan yang membuat keputusan routing." },
  { "en": "Cognitive Radio?", "id": "Radio cerdas yang dapat beradaptasi dengan lingkungan." },
  { "en": "Spectrum Sensing?", "id": "Mendeteksi pita frekuensi yang tidak digunakan." },
  { "en": "Dynamic Spectrum Access?", "id": "Menggunakan spektrum secara oportunistik." },
  { "en": "Visible Light Communication (VLC)?", "id": "Komunikasi menggunakan cahaya tampak (misalnya, Li-Fi)." },
  { "en": "Li-Fi (Light Fidelity)?", "id": "VLC berkecepatan tinggi menggunakan lampu LED." },
  { "en": "Free-Space Optical (FSO) Communication?", "id": "Komunikasi optik melalui udara atau luar angkasa." },
  { "en": "Non-Orthogonal Multiple Access (NOMA)?", "id": "Memungkinkan banyak pengguna berbagi sumber daya." },
  { "en": "Successive Interference Cancellation (SIC)?", "id": "Teknik penerima untuk mendekode sinyal NOMA." },
  { "en": "Device-to-Device (D2D) Communication?", "id": "Komunikasi langsung antar perangkat tanpa base station." },
  { "en": "Caching (Jaringan)?", "id": "Menyimpan konten populer lebih dekat ke pengguna." },
  { "en": "Content Delivery Network (CDN)?", "id": "Jaringan server terdistribusi untuk pengiriman konten." },
  { "en": "Information Centric Networking (ICN)?", "id": "Arsitektur jaringan masa depan berbasis konten." },
  { "en": "Physical Layer Security?", "id": "Memanfaatkan sifat fisik kanal untuk keamanan." },
  { "en": "Full Duplex?", "id": "Transmisi dan penerimaan pada frekuensi sama." },
  { "en": "Self-Interference Cancellation?", "id": "Tantangan utama dalam komunikasi full duplex." },
  { "en": "Quality of Experience (QoE)?", "id": "Ukuran kepuasan subjektif pengguna terhadap layanan." },
  { "en": "Mean Opinion Score (MOS)?", "id": "Metrik umum untuk mengukur QoE." },
  { "en": "Routing Protocol?", "id": "Aturan yang menentukan jalur data dalam jaringan." },
  { "en": "OSPF (Open Shortest Path First)?", "id": "Protokol routing link-state." },
  { "en": "BGP (Border Gateway Protocol)?", "id": "Protokol routing yang digunakan di internet." },
  { "en": "MPLS (Multiprotocol Label Switching)?", "id": "Teknik routing yang menggunakan label." },
  { "en": "IP Address (Internet Protocol)?", "id": "Alamat unik untuk setiap perangkat di jaringan." },
  { "en": "IPv4 vs IPv6?", "id": "Dua versi protokol internet (32-bit vs 128-bit)." },
  { "en": "Subnet Mask?", "id": "Memisahkan alamat IP menjadi ID jaringan dan host." },
  { "en": "NAT (Network Address Translation)?", "id": "Memetakan alamat IP privat ke publik." },
  { "en": "DHCP (Dynamic Host Configuration Protocol)?", "id": "Memberikan alamat IP secara otomatis." },
  { "en": "DNS (Domain Name System)?", "id": "Menerjemahkan nama domain menjadi alamat IP." },
  { "en": "TCP (Transmission Control Protocol)?", "id": "Protokol transport yang andal dan connection-oriented." },
  { "en": "UDP (User Datagram Protocol)?", "id": "Protokol transport yang cepat tapi tidak andal." },
  { "en": "TCP Three-Way Handshake?", "id": "Proses untuk memulai koneksi TCP (SYN, SYN-ACK, ACK)." },
  { "en": "Congestion Control?", "id": "Mekanisme TCP untuk menghindari kemacetan jaringan." },
  { "en": "Sliding Window Protocol?", "id": "Mekanisme flow control pada TCP." },
  { "en": "HTTP (Hypertext Transfer Protocol)?", "id": "Protokol untuk mentransfer data di World Wide Web." },
  { "en": "SSL/TLS (Secure Sockets Layer/Transport Layer Security)?", "id": "Protokol untuk mengamankan komunikasi internet." },
  { "en": "Firewall?", "id": "Sistem keamanan yang memonitor lalu lintas jaringan." },
  { "en": "VPN (Virtual Private Network)?", "id": "Membuat koneksi aman melalui jaringan publik." },
  { "en": "Intrusion Detection System (IDS)?", "id": "Mendeteksi aktivitas mencurigakan di jaringan." },
  { "en": "Packet Sniffing?", "id": "Menangkap dan menganalisis paket data jaringan." },
  { "en": "MAC Address (Media Access Control)?", "id": "Alamat perangkat keras unik dari antarmuka jaringan." },
  { "en": "ARP (Address Resolution Protocol)?", "id": "Memetakan alamat IP ke alamat MAC." },
  { "en": "VLAN (Virtual LAN)?", "id": "Membuat beberapa LAN logis di atas satu LAN fisik." },
  { "en": "Spanning Tree Protocol (STP)?", "id": "Mencegah adanya loop pada jaringan switched." },
  { "en": "Latency vs Bandwidth?", "id": "Waktu tunda vs kapasitas data." },
  { "en": "Handoff (Handover)?", "id": "Proses memindahkan koneksi dari satu sel ke sel lain." },
  { "en": "Soft Handoff?", "id": "Terhubung ke sel baru sebelum meninggalkan sel lama." },
  { "en": "Hard Handoff?", "id": "Memutus koneksi lama sebelum menyambung ke yang baru." },
  { "en": "Cell Breathing?", "id": "Ukuran cakupan sel berubah berdasarkan beban." },
  { "en": "Interference Management?", "id": "Teknik untuk mengurangi interferensi antar sel." },
  { "en": "Location-Based Services (LBS)?", "id": "Layanan yang menggunakan data lokasi geografis." },
  { "en": "GPS (Global Positioning System)?", "id": "Sistem navigasi satelit." },
  { "en": "Triangulation?", "id": "Menentukan posisi berdasarkan jarak ke tiga titik." },
  { "en": "Simultaneous Wireless Information and Power Transfer (SWIPT)?", "id": "Mengirim informasi dan daya secara bersamaan." },
  { "en": "Molecular Communication?", "id": "Komunikasi menggunakan molekul sebagai pembawa informasi." },
  { "en": "Terahertz (THz) Communication?", "id": "Komunikasi pada pita frekuensi antara gelombang mikro dan inframerah." },
  { "en": "Intelligent Reflecting Surface (IRS)?", "id": "Permukaan pasif untuk memanipulasi sinyal nirkabel." },
  { "en": "Quantum Communication?", "id": "Komunikasi berbasis prinsip-prinsip mekanika kuantum." },
  { "en": "Quantum Key Distribution (QKD)?", "id": "Memungkinkan pertukaran kunci kriptografi yang aman." },
  { "en": "Rekayasa Biomedis?", "id": "Aplikasi prinsip teknik pada bidang medis." },
  { "en": "Pencitraan Medis?", "id": "Teknik untuk memvisualisasikan bagian dalam tubuh." },
  { "en": "Sinyal Biomedis?", "id": "Sinyal listrik yang dihasilkan oleh tubuh (EKG, EEG)." },
  { "en": "Instrumentasi Medis?", "id": "Desain dan pengembangan peralatan medis." },
  { "en": "Biomekanika?", "id": "Studi mekanika pada sistem biologis." },
  { "en": "Teknik Jaringan (Tissue Engineering)?", "id": "Mengembangkan pengganti biologis untuk jaringan tubuh." },
  { "en": "Antarmuka Otak-Komputer (BCI)?", "id": "Jalur komunikasi langsung antara otak dan komputer." },
  { "en": "Prostetik?", "id": "Alat buatan untuk menggantikan bagian tubuh." },
  { "en": "Drug Delivery System?", "id": "Teknologi untuk menghantarkan obat secara tertarget." },
  { "en": "Telemedicine?", "id": "Penyediaan layanan kesehatan dari jarak jauh." },
  { "en": "Bioinformatika?", "id": "Menggunakan komputasi untuk menganalisis data biologis." },
  { "en": "Robotika?", "id": "Desain, konstruksi, dan operasi robot." },
  { "en": "Kinematika Robot?", "id": "Studi tentang gerakan robot tanpa memperhitungkan gaya." },
  { "en": "Dinamika Robot?", "id": "Studi gerakan robot yang memperhitungkan gaya." },
  { "en": "Forward Kinematics?", "id": "Menghitung posisi end-effector dari sudut sendi." },
  { "en": "Inverse Kinematics?", "id": "Menghitung sudut sendi dari posisi end-effector." },
  { "en": "End-Effector?", "id": "Alat di ujung lengan robot (misalnya, gripper)." },
  { "en": "Jacobian Matrix (Robot)?", "id": "Menghubungkan kecepatan sendi dengan kecepatan end-effector." },
  { "en": "Path Planning?", "id": "Menemukan jalur bebas halangan untuk robot." },
  { "en": "Motion Planning?", "id": "Merencanakan gerakan robot sepanjang jalur." },
  { "en": "Human-Robot Interaction (HRI)?", "id": "Studi interaksi antara manusia dan robot." },
  { "en": "Swarm Robotics?", "id": "Sistem multi-robot sederhana yang bekerja sama." },
  { "en": "Soft Robotics?", "id": "Robot yang terbuat dari material yang lentur." },
  { "en": "MEMS (Micro-Electro-Mechanical Systems)?", "id": "Sistem miniatur yang mengintegrasikan mekanik dan elektronik." },
  { "en": "NEMS (Nano-Electro-Mechanical Systems)?", "id": "Versi skala nano dari MEMS." },
  { "en": "Akselerometer?", "id": "Sensor MEMS untuk mengukur percepatan." },
  { "en": "Giroskop?", "id": "Sensor MEMS untuk mengukur orientasi." },
  { "en": "Pressure Sensor?", "id": "Sensor MEMS untuk mengukur tekanan." },
  { "en": "Microfluidics?", "id": "Manipulasi cairan dalam saluran skala mikro." },
  { "en": "Lab-on-a-Chip?", "id": "Integrasi fungsi laboratorium pada satu chip." },
  { "en": "Fabrikasi MEMS?", "id": "Proses manufaktur untuk perangkat MEMS." },
  { "en": "Bulk Micromachining?", "id": "Teknik fabrikasi MEMS dengan mengetsa substrat." },
  { "en": "Surface Micromachining?", "id": "Teknik fabrikasi MEMS dengan membangun lapisan." },
  { "en": "Fotonik?", "id": "Ilmu dan teknologi pembangkitan dan manipulasi foton." },
  { "en": "Fotonika Silikon?", "id": "Membuat perangkat fotonik menggunakan material silikon." },
  { "en": "Sirkuit Terpadu Fotonik (PIC)?", "id": "Integrasi banyak komponen optik pada satu chip." },
  { "en": "Optical Switch?", "id": "Mengalihkan sinyal optik dari satu jalur ke jalur lain." },
  { "en": "Modulator Optik?", "id": "Memodulasi sinyal listrik ke sinyal cahaya." },
  { "en": "Waveguide (Optik)?", "id": "Struktur yang memandu gelombang cahaya." },
  { "en": "Plasmonik?", "id": "Studi interaksi cahaya dengan elektron di permukaan logam." },
  { "en": "Metamaterial?", "id": "Material rekayasa dengan sifat optik tak biasa." },
  { "en": "Invisibility Cloak?", "id": "Aplikasi potensial dari metamaterial." },
  { "en": "Komputasi Kuantum?", "id": "Komputasi menggunakan fenomena mekanika kuantum." },
  { "en": "Qubit (Quantum Bit)?", "id": "Unit dasar informasi kuantum." },
  { "en": "Superposisi?", "id": "Kemampuan qubit untuk berada di banyak keadaan sekaligus." },
  { "en": "Keterkaitan Kuantum (Entanglement)?", "id": "Koneksi misterius antara dua atau lebih qubit." },
  { "en": "Gerbang Kuantum?", "id": "Operasi dasar pada qubit." },
  { "en": "Dekoherensi Kuantum?", "id": "Hilangnya sifat kuantum akibat interaksi lingkungan." },
  { "en": "Quantum Annealing?", "id": "Metode komputasi kuantum untuk masalah optimisasi." },
  { "en": "Sensor Kuantum?", "id": "Sensor yang memanfaatkan efek kuantum untuk presisi tinggi." },
  { "en": "Energi Terbarukan?", "id": "Sumber energi yang tidak akan habis (matahari, angin)." },
  { "en": "Fotovoltaik (PV)?", "id": "Teknologi sel surya." },
  { "en": "Turbin Angin?", "id": "Mengubah energi kinetik angin menjadi listrik." },
  { "en": "Geotermal?", "id": "Energi panas dari dalam bumi." },
  { "en": "Biomassa?", "id": "Energi dari material organik." },
  { "en": "Fuel Cell (Sel Bahan Bakar)?", "id": "Menghasilkan listrik dari reaksi kimia (misalnya, hidrogen)." },
  { "en": "Manajemen Energi?", "id": "Optimalisasi penggunaan dan penyimpanan energi." },
  { "en": "Demand Response?", "id": "Mendorong konsumen mengubah penggunaan listrik." },
  { "en": "Efisiensi Energi?", "id": "Menggunakan lebih sedikit energi untuk fungsi sama." },
  { "en": "Kelistrikan Kendaraan (Vehicular Electrification)?", "id": "Pengembangan mobil listrik dan hibrida." },
  { "en": "Hybrid Electric Vehicle (HEV)?", "id": "Mobil dengan mesin bensin dan motor listrik." },
  { "en": "Plug-in Hybrid (PHEV)?", "id": "HEV yang baterainya bisa diisi dari listrik." },
  { "en": "Battery Electric Vehicle (BEV)?", "id": "Mobil yang sepenuhnya ditenagai oleh baterai." },
  { "en": "Fuel Cell Vehicle (FCV)?", "id": "Mobil listrik yang ditenagai oleh sel bahan bakar." },
  { "en": "Rekayasa Audio?", "id": "Teknik yang berhubungan dengan suara dan getaran." },
  { "en": "Akustik?", "id": "Ilmu tentang suara." },
  { "en": "Psikoakustik?", "id": "Studi tentang persepsi suara oleh manusia." },
  { "en": "Sintesis Suara?", "id": "Penciptaan suara secara artifisial." },
  { "en": "Audio Spasial?", "id": "Menciptakan pengalaman suara 3D yang imersif." },
  { "en": "Active Noise Cancellation?", "id": "Mengurangi noise dengan menghasilkan suara anti-fasa." },
  { "en": "Ekonofisika?", "id": "Aplikasi metode fisika statistik pada masalah ekonomi." },
  { "en": "Computational Finance?", "id": "Penggunaan komputasi untuk menganalisis pasar keuangan." },
  { "en": "Perdagangan Algoritmik?", "id": "Menggunakan program komputer untuk melakukan trading." },
  { "en": "Analisis Deret Waktu Finansial?", "id": "Menganalisis data harga saham dari waktu ke waktu." },
  { "en": "Model Black-Scholes?", "id": "Model matematis untuk menentukan harga opsi." },
  { "en": "Volatility Smile?", "id": "Pola yang diamati pada harga opsi." },
  { "en": "Nanoteknologi?", "id": "Rekayasa pada skala nanometer." },
  { "en": "Quantum Dot?", "id": "Nanokristal semikonduktor dengan sifat optik unik." },
  { "en": "Carbon Nanotube?", "id": "Struktur karbon silindris dengan kekuatan luar biasa." },
  { "en": "Graphene?", "id": "Lapisan tunggal atom karbon." },
  { "en": "Spintronik?", "id": "Elektronik yang memanfaatkan spin elektron." },
  { "en": "Teknologi Layar?", "id": "Teknologi di balik display elektronik." },
  { "en": "LCD (Liquid Crystal Display)?", "id": "Menggunakan kristal cair dan cahaya latar." },
  { "en": "LED (Light Emitting Diode) Display?", "id": "Menggunakan LED sebagai sumber cahaya." },
  { "en": "OLED (Organic LED)?", "id": "Setiap piksel menghasilkan cahayanya sendiri." },
  { "en": "MicroLED?", "id": "Teknologi layar masa depan dengan LED anorganik." },
  { "en": "E-Ink (Electronic Ink)?", "id": "Tampilan bistabil yang meniru tinta di kertas." },
  { "en": "Virtual Reality (VR)?", "id": "Menciptakan lingkungan virtual yang imersif." },
  { "en": "Augmented Reality (AR)?", "id": "Menambahkan informasi digital ke dunia nyata." },
  { "en": "Mixed Reality (MR)?", "id": "Interaksi objek digital dengan dunia nyata." },
  { "en": "Teknik Forensik Digital?", "id": "Pemulihan dan investigasi material dari perangkat digital." },
  { "en": "Akuisi Data?", "id": "Proses menyalin data untuk analisis forensik." },
  { "en": "Analisis File System?", "id": "Memeriksa struktur file dan metadata." },
  { "en": "Analisis Jaringan Forensik?", "id": "Menganalisis lalu lintas jaringan untuk bukti." },
  { "en": "Chain of Custody?", "id": "Dokumentasi penanganan barang bukti digital." }



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
