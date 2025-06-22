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


  { "en": "Alat ukur listrik serbaguna?", "id": "Multimeter." },
  { "en": "Nama lama untuk multimeter?", "id": "AVO-meter." },
  { "en": "Kepanjangan dari AVO?", "id": "Ampere, Volt, Ohm." },
  { "en": "Tiga besaran utama yang diukur?", "id": "Tegangan, Arus, Hambatan." },
  { "en": "Multimeter dengan jarum penunjuk?", "id": "Multimeter Analog." },
  { "en": "Multimeter dengan tampilan angka?", "id": "Multimeter Digital (DMM)." },
  { "en": "Kepanjangan DMM?", "id": "Digital Multimeter." },
  { "en": "Bagian untuk memilih mode ukur?", "id": "Saklar pemilih (selector dial)." },
  { "en": "Bagian untuk menampilkan hasil ukur?", "id": "Layar (display)." },
  { "en": "Kabel pengukur pada multimeter?", "id": "Probe." },
  { "en": "Warna kabel probe standar?", "id": "Merah dan Hitam." },
  { "en": "Probe merah untuk polaritas?", "id": "Positif (+)." },
  { "en": "Probe hitam untuk polaritas?", "id": "Negatif (-)." },
  { "en": "Lubang colokan untuk probe hitam?", "id": "Port COM." },
  { "en": "COM singkatan dari?", "id": "Common (referensi ground)." },
  { "en": "Lubang colokan untuk ukur tegangan/hambatan?", "id": "Port VΩmA." },
  { "en": "Lubang colokan untuk ukur arus besar?", "id": "Port 10A atau 20A." },
  { "en": "Satuan untuk mengukur tegangan?", "id": "Volt (V)." },
  { "en": "Satuan untuk mengukur arus?", "id": "Ampere (A)." },
  { "en": "Satuan untuk mengukur hambatan?", "id": "Ohm (Ω)." },
  { "en": "Simbol untuk tegangan DC?", "id": "V⎓ atau VDC." },
  { "en": "Simbol untuk tegangan AC?", "id": "V~ atau VAC." },
  { "en": "Sumber tegangan DC?", "id": "Baterai, adaptor." },
  { "en": "Sumber tegangan AC?", "id": "Stop kontak PLN." },
  { "en": "Cara mengukur tegangan?", "id": "Secara paralel dengan komponen." },
  { "en": "Probe merah ditempatkan di?", "id": "Titik potensial lebih tinggi." },
  { "en": "Probe hitam ditempatkan di?", "id": "Titik potensial lebih rendah." },
  { "en": "Hasil ukur tegangan negatif berarti?", "id": "Posisi probe terbalik." },
  { "en": "Aturan memilih rentang (range)?", "id": "Pilih rentang di atas nilai perkiraan." },
  { "en": "Multimeter yang memilih rentang otomatis?", "id": "Multimeter auto-ranging." },
  { "en": "Apa itu 'tegangan tembus'?", "id": "Tegangan maksimum yang bisa diukur." },
  { "en": "Simbol untuk arus DC?", "id": "A⎓ atau ADC." },
  { "en": "Simbol untuk arus AC?", "id": "A~ atau AAC." },
  { "en": "Cara mengukur arus?", "id": "Secara seri dengan beban." },
  { "en": "Apa yang harus dilakukan pada sirkuit?", "id": "Putuskan sirkuit terlebih dahulu." },
  { "en": "Mengapa sirkuit harus diputus?", "id": "Agar arus mengalir melewati multimeter." },
  { "en": "Risiko salah ukur arus?", "id": "Sekring putus atau multimeter rusak." },
  { "en": "Pelindung internal multimeter dari arus lebih?", "id": "Sekring (fuse)." },
  { "en": "Mengukur arus di atas 200mA?", "id": "Pindahkan probe ke port 10A." },
  { "en": "Simbol untuk mengukur hambatan?", "id": "Ohm (Ω)." },
  { "en": "Syarat utama mengukur hambatan?", "id": "Komponen harus tanpa daya listrik." },
  { "en": "Mengapa harus tanpa daya?", "id": "Agar tidak merusak multimeter." },
  { "en": "Cara mengukur hambatan resistor?", "id": "Sentuhkan probe di kedua kakinya." },
  { "en": "Hasil ukur hambatan sirkuit terbuka?", "id": "OL (Over Load/Limit)." },
  { "en": "OL artinya apa?", "id": "Hambatan tak terhingga." },
  { "en": "Hasil ukur hambatan kabel terhubung?", "id": "Mendekati nol Ohm." },
  { "en": "Fungsi untuk memeriksa jalur tersambung?", "id": "Uji kontinuitas (Continuity Test)." },
  { "en": "Simbol untuk uji kontinuitas?", "id": "Simbol suara atau dioda." },
  { "en": "Indikator jalur tersambung?", "id": "Multimeter berbunyi 'beep'." },
  { "en": "Fungsi untuk memeriksa dioda?", "id": "Uji Dioda (Diode Test)." },
  { "en": "Simbol untuk uji dioda?", "id": "Simbol dioda." },
  { "en": "Hasil ukur uji dioda yang baik?", "id": "Menunjukkan tegangan maju (forward voltage)." },
  { "en": "Tegangan maju dioda silikon?", "id": "Sekitar 0.7 Volt." },
  { "en": "Tegangan maju dioda LED?", "id": "Tergantung warna (1.5V - 3V)." },
  { "en": "Jika probe dibalik pada dioda?", "id": "Menunjukkan OL (sirkuit terbuka)." },
  { "en": "Fungsi untuk mengukur penguatan transistor?", "id": "hFE." },
  { "en": "Fungsi untuk mengukur kapasitansi?", "id": "Mode kapasitor (Farad)." },
  { "en": "Fungsi untuk mengukur frekuensi?", "id": "Mode frekuensi (Hertz)." },
  { "en": "Fungsi untuk mengukur suhu?", "id": "Mode suhu (dengan termokopel)." },
  { "en": "Apa itu 'True RMS'?", "id": "Pengukuran AC akurat untuk non-sinus." },
  { "en": "Tombol untuk menahan hasil ukur?", "id": "Tombol HOLD." },
  { "en": "Tombol untuk lampu latar layar?", "id": "Tombol backlight." },
  { "en": "Tombol untuk memilih rentang manual?", "id": "Tombol RANGE." },
  { "en": "Tombol untuk memilih fungsi sekunder?", "id": "Tombol FUNC atau SELECT." },
  { "en": "Apa itu 'ghost voltage'?", "id": "Tegangan liar karena induksi." },
  { "en": "Fitur untuk mendeteksi tegangan tanpa kontak?", "id": "NCV (Non-Contact Voltage)." },
  { "en": "Tampilan 'LoZ' pada multimeter?", "id": "Mode impedansi rendah." },
  { "en": "Tujuan mode LoZ?", "id": "Menghilangkan 'ghost voltage'." },
  { "en": "Apa itu 'relative mode' (REL)?", "id": "Mengatur nilai saat ini jadi nol." },
  { "en": "Tujuan 'relative mode'?", "id": "Mengukur selisih dari nilai referensi." },
  { "en": "Apa itu 'counts' pada DMM?", "id": "Jumlah digit yang bisa ditampilkan." },
  { "en": "DMM 4000 count artinya?", "id": "Menampilkan angka dari 0-3999." },
  { "en": "Semakin tinggi count, semakin?", "id": "Tinggi resolusi pengukuran." },
  { "en": "Peringkat keamanan pada multimeter?", "id": "CAT rating." },
  { "en": "Tujuan CAT rating?", "id": "Menunjukkan tingkat proteksi tegangan." },
  { "en": "CAT rating untuk elektronik tegangan rendah?", "id": "CAT I." },
  { "en": "CAT rating untuk stop kontak rumah?", "id": "CAT II." },
  { "en": "CAT rating untuk panel distribusi?", "id": "CAT III." },
  { "en": "CAT rating untuk outdoor/gardu listrik?", "id": "CAT IV." },
  { "en": "Apa itu 'input impedance'?", "id": "Hambatan internal multimeter." },
  { "en": "Impedansi input yang baik untuk ukur tegangan?", "id": "Sangat tinggi (Megaohm)." },
  { "en": "Mengapa impedansi input harus tinggi?", "id": "Agar tidak membebani sirkuit." },
  { "en": "Apa itu 'burden voltage'?", "id": "Jatuh tegangan saat ukur arus." },
  { "en": "Burden voltage yang baik?", "id": "Se-rendah mungkin." },
  { "en": "Tombol untuk merekam nilai min/max?", "id": "Tombol MIN/MAX." },
  { "en": "Apa itu 'auto power off'?", "id": "Fitur mati otomatis." },
  { "en": "Tujuan 'auto power off'?", "id": "Menghemat daya baterai." },
  { "en": "Indikator baterai lemah di layar?", "id": "Simbol baterai." },
  { "en": "Apa yang harus dilakukan jika baterai lemah?", "id": "Segera ganti baterai." },
  { "en": "Mengapa akurasi bisa menurun?", "id": "Baterai lemah, suhu ekstrim." },
  { "en": "Bagaimana menyimpan multimeter?", "id": "Matikan, simpan di tempat kering." },
  { "en": "Sebelum mengukur, lakukan apa?", "id": "Periksa kondisi probe dan multimeter." },
  { "en": "Jangan sentuh ujung logam probe saat?", "id": "Mengukur di sirkuit hidup." },
  { "en": "Posisi saklar saat multimeter tidak dipakai?", "id": "Posisi OFF." },
  { "en": "Kesalahan pembacaan pada multimeter analog?", "id": "Kesalahan paralaks." },
  { "en": "Cara memeriksa sekring (fuse)?", "id": "Gunakan mode kontinuitas atau Ohm." },
  { "en": "Sekring yang bagus menunjukkan?", "id": "Kontinuitas (berbunyi) atau hambatan rendah." },
  { "en": "Sekring yang putus menunjukkan?", "id": "Tidak ada kontinuitas (OL)." },
  { "en": "Cara memeriksa saklar (switch)?", "id": "Uji kontinuitas pada setiap posisi." },
  { "en": "Cara memeriksa kabel?", "id": "Uji kontinuitas di kedua ujungnya." },
  { "en": "Cara mengukur tegangan baterai?", "id": "Gunakan mode VDC, hubungkan paralel." },
  { "en": "Baterai yang bagus tegangannya?", "id": "Sesuai atau sedikit di atas rating." },
  { "en": "Cara memeriksa dioda?", "id": "Gunakan mode uji dioda." },
  { "en": "Anoda dioda dihubungkan ke probe?", "id": "Merah (untuk forward bias)." },
  { "en": "Katoda dioda dihubungkan ke probe?", "id": "Hitam (untuk forward bias)." },
  { "en": "Dioda bagus jika dibalik (reverse bias)?", "id": "Menunjukkan OL (tidak menghantar)." },
  { "en": "Dioda rusak jika?", "id": "Menunjukkan OL atau nol di kedua arah." },
  { "en": "Cara memeriksa LED?", "id": "Gunakan mode uji dioda (LED menyala)." },
  { "en": "Cara memeriksa transistor BJT?", "id": "Tes seperti dua dioda yang terhubung." },
  { "en": "Basis ke Emitor dan Basis ke Kolektor?", "id": "Harus berperilaku seperti dioda." },
  { "en": "Cara mengetahui tipe transistor (NPN/PNP)?", "id": "Berdasarkan polaritas probe saat tes." },
  { "en": "Cara mengukur kapasitor non-polar?", "id": "Gunakan mode kapasitansi." },
  { "en": "Cara memeriksa kapasitor dengan mode Ohm?", "id": "Nilai naik lalu OL (mengisi)." },
  { "en": "Kapasitor rusak (short) jika?", "id": "Menunjukkan hambatan mendekati nol." },
  { "en": "Cara mengukur potensiometer?", "id": "Ukur hambatan total dan hambatan wiper." },
  { "en": "Kedekatan hasil ukur ke nilai sebenarnya?", "id": "Akurasi (Accuracy)." },
  { "en": "Kemampuan hasil ukur untuk konsisten?", "id": "Presisi (Precision)." },
  { "en": "Perubahan terkecil yang bisa diukur?", "id": "Resolusi (Resolution)." },
  { "en": "Arti '3 ½ digit' pada DMM?", "id": "3 digit penuh, 1 setengah digit." },
  { "en": "Setengah digit pertama bisa menampilkan?", "id": "Angka 0 atau 1." },
  { "en": "Cermin pada skala multimeter analog?", "id": "Untuk menghindari kesalahan paralaks." },
  { "en": "Cara membaca skala dengan cermin?", "id": "Pastikan jarum dan bayangannya menyatu." },
  { "en": "Tombol kalibrasi nol pada mode Ohm analog?", "id": "Zero Ohm Adjustment." },
  { "en": "Kapan Zero Ohm Adjustment dilakukan?", "id": "Setiap kali berpindah rentang Ohm." },
  { "en": "Sensitivitas multimeter analog diukur dalam?", "id": "Ohm per Volt (Ω/V)." },
  { "en": "Semakin tinggi Ohm/Volt, semakin?", "id": "Baik dan sensitif." },
  { "en": "Mengukur tegangan saat saklar di mode arus?", "id": "Sangat berbahaya, jangan lakukan." },
  { "en": "Mengapa berbahaya?", "id": "Menyebabkan korsleting melalui multimeter." },
  { "en": "Apa itu 'tegangan jatuh' (voltage drop)?", "id": "Penurunan tegangan pada suatu komponen." },
  { "en": "Cara mengukur voltage drop?", "id": "Ukur tegangan secara paralel." },
  { "en": "Mengukur arus bocor (leakage current)?", "id": "Gunakan rentang arus sangat kecil (µA)." },
  { "en": "Aksesoris probe seperti capit buaya?", "id": "Alligator clips." },
  { "en": "Aksesoris probe seperti jarum tajam?", "id": "Fine point test probes." },
  { "en": "Aksesoris probe seperti pengait?", "id": "Hook clips." },
  { "en": "Fungsi MIN/MAX merekam apa?", "id": "Nilai terendah dan tertinggi." },
  { "en": "Fungsi AVG menampilkan apa?", "id": "Nilai rata-rata pengukuran." },
  { "en": "Rasio puncak gelombang ke nilai RMS?", "id": "Crest Factor." },
  { "en": "True RMS penting untuk gelombang?", "id": "Non-sinusoidal (kotak, segitiga)." },
  { "en": "Mengukur frekuensi sinyal AC?", "id": "Gunakan mode frekuensi (Hz)." },
  { "en": "Mengukur siklus kerja (duty cycle)?", "id": "Gunakan mode duty cycle (%)." },
  { "en": "Termokopel digunakan untuk mengukur?", "id": "Suhu." },
  { "en": "Jenis termokopel paling umum?", "id": "Tipe-K." },
  { "en": "Layar bar graph analog pada DMM?", "id": "Visualisasi perubahan nilai cepat." },
  { "en": "Mengapa perlu memeriksa probe secara berkala?", "id": "Memastikan tidak ada kerusakan/retak." },
  { "en": "Jangan gunakan multimeter jika?", "id": "Kabelnya rusak atau terkelupas." },
  { "en": "Saat ragu, selalu asumsikan sirkuit?", "id": "Masih memiliki tegangan (live)." },
  { "en": "Perbedaan antara ground dan netral?", "id": "Ground untuk keamanan, netral jalur balik." },
  { "en": "Uji kontinuitas pada motor?", "id": "Memeriksa apakah kumparan tidak putus." },
  { "en": "Hambatan kumparan motor yang baik?", "id": "Sangat rendah, tidak OL." },
  { "en": "Mengukur resistansi isolasi?", "id": "Gunakan Insulation Tester (Megger)." },
  { "en": "Bisa ukur resistansi isolasi dengan multimeter?", "id": "Tidak bisa, tegangan tidak cukup." },
  { "en": "Uji 'ground fault'?", "id": "Memeriksa kebocoran arus ke ground." },
  { "en": "Alat khusus untuk uji 'ground fault'?", "id": "GFCI Tester." },
  { "en": "Mengukur arus start motor?", "id": "Gunakan clamp meter dengan 'inrush'." },
  { "en": "Bisa ukur arus start dengan multimeter?", "id": "Tidak direkomendasikan, arus terlalu besar." },
  { "en": "Multimeter dengan koneksi ke smartphone?", "id": "Ya, via Bluetooth." },
  { "en": "Keuntungan koneksi ke smartphone?", "id": "Logging data, pemantauan jarak jauh." },
  { "en": "Apa itu 'kalibrasi' multimeter?", "id": "Menyesuaikan agar hasil ukur akurat." },
  { "en": "Siapa yang melakukan kalibrasi?", "id": "Laboratorium kalibrasi terakreditasi." },
  { "en": "Tampilan '- ' di depan angka?", "id": "Polaritas terbalik." },
  { "en": "Mengukur tegangan riak (ripple voltage)?", "id": "Gunakan mode VAC pada output DC." },
  { "en": "Riak adalah sisa komponen?", "id": "AC pada sinyal DC." },
  { "en": "Catu daya yang baik memiliki riak?", "id": "Sangat kecil (milivolt)." },
  { "en": "Tampilan 'touch hold'?", "id": "Menahan pembacaan stabil secara otomatis." },
  { "en": "Apa itu 'clamp meter'?", "id": "Multimeter untuk ukur arus tanpa kontak." },
  { "en": "Prinsip kerja clamp meter?", "id": "Induksi magnetik." },
  { "en": "Clamp meter mengukur arus?", "id": "AC (umumnya), beberapa bisa DC." },
  { "en": "Cara menggunakan NCV?", "id": "Dekatkan ujung multimeter ke kabel." },
  { "en": "Indikasi NCV mendeteksi tegangan?", "id": "Bunyi 'beep' dan lampu menyala." },
  { "en": "Siklus kerja 100% berarti?", "id": "Sinyal selalu HIGH." },
  { "en": "Siklus kerja 0% berarti?", "id": "Sinyal selalu LOW." },
  { "en": "Mengukur tegangan pada kapasitor terisi?", "id": "Hati-hati, masih menyimpan muatan." },
  { "en": "Cara aman mengosongkan kapasitor?", "id": "Hubung singkat melalui resistor." },
  { "en": "Tampilan DMM yang berkedip?", "id": "Menandakan nilai di luar rentang." },
  { "en": "Mengukur hambatan kulit manusia?", "id": "Bisa, tapi nilainya sangat bervariasi." },
  { "en": "Multimeter analog lebih baik untuk?", "id": "Melihat perubahan nilai yang cepat." },
  { "en": "Multimeter digital lebih baik untuk?", "id": "Akurasi dan kemudahan pembacaan." },
  { "en": "Apa itu 'ghost reading'?", "id": "Pembacaan palsu karena kapasitansi liar." },
  { "en": "Mode 'low pass filter' (LPF)?", "id": "Menolak noise frekuensi tinggi." },
  { "en": "LPF berguna saat mengukur?", "id": "Output Variable Frequency Drive (VFD)." },
  { "en": "Mengapa perlu membersihkan probe?", "id": "Untuk kontak yang baik dan akurat." },
  { "en": "Penunjuk pada multimeter analog?", "id": "Jarum (needle)." },
  { "en": "Skala pada multimeter analog?", "id": "Memiliki beberapa skala berbeda." },
  { "en": "Penting membaca skala yang benar?", "id": "Ya, agar tidak salah interpretasi." },
  { "en": "Baterai yang diukur harus?", "id": "Memiliki beban agar akurat." },
  { "en": "Uji 'diode drop' sama dengan?", "id": "Uji tegangan maju dioda." },
  { "en": "Apa itu 'burden voltage' pada ukur arus?", "id": "Tegangan yang 'hilang' di multimeter." },
  { "en": "Burden voltage yang tinggi akan?", "id": "Mempengaruhi sirkuit yang diukur." },
  { "en": "Sarung pelindung multimeter?", "id": "Holster atau boot." },
  { "en": "Tujuan holster?", "id": "Melindungi dari benturan." },
  { "en": "Pengukuran induktansi kumparan?", "id": "Gunakan mode Induktansi (Henry)." },
  { "en": "Satuan dari induktansi?", "id": "Henry (H)." },
  { "en": "Pengukuran level logika digital?", "id": "Gunakan mode Logic Level." },
  { "en": "Hasil ukur mode Logic Level?", "id": "HIGH atau LOW." },
  { "en": "Pengukuran rasio daya (desibel)?", "id": "Gunakan mode dBm." },
  { "en": "Kebalikan dari resistansi?", "id": "Konduktansi." },
  { "en": "Satuan dari konduktansi?", "id": "Siemens (S)." },
  { "en": "Fitur merekam data dari waktu ke waktu?", "id": "Data Logging." },
  { "en": "Perbedaan 'Peak Hold' dan 'Data Hold'?", "id": "Peak Hold menangkap nilai puncak." },
  { "en": "Data Hold hanya?", "id": "Membekukan nilai di layar." },
  { "en": "Mengukur tegangan output panel surya?", "id": "Tegangan sirkuit terbuka (Voc)." },
  { "en": "Mengukur arus output panel surya?", "id": "Arus hubung singkat (Isc)." },
  { "en": "Pengukuran hambatan koneksi ke tanah?", "id": "Earth Ground Resistance Test." },
  { "en": "Bagaimana DMM mengukur hambatan?", "id": "Memberi arus, lalu ukur tegangan." },
  { "en": "Hukum apa yang digunakan?", "id": "Hukum Ohm (V = IR)." },
  { "en": "Bagaimana DMM mengukur arus?", "id": "Mengukur tegangan di resistor shunt." },
  { "en": "Resistor shunt memiliki hambatan?", "id": "Sangat rendah dan presisi." },
  { "en": "Komponen inti pada DMM?", "id": "ADC (Analog-to-Digital Converter)." },
  { "en": "Fungsi ADC pada DMM?", "id": "Mengubah tegangan analog ke digital." },
  { "en": "Komponen proteksi dari lonjakan tegangan?", "id": "MOV (Metal Oxide Varistor)." },
  { "en": "Komponen proteksi dari arus lebih?", "id": "PTC Resettable Fuse." },
  { "en": "Nilai RMS gelombang sinus murni?", "id": "0.707 x Vpeak." },
  { "en": "Nilai rata-rata (average) gelombang sinus murni?", "id": "0.637 x Vpeak." },
  { "en": "Multimeter non-True RMS mengukur?", "id": "Nilai rata-rata (average)." },
  { "en": "Mengapa hasilnya bisa salah?", "id": "Hasilnya dikalibrasi untuk sinus." },
  { "en": "Bagaimana mengatasi pembacaan yang tidak stabil?", "id": "Gunakan MIN/MAX atau tahan probe lebih lama." },
  { "en": "Multimeter untuk laboratorium presisi tinggi?", "id": "Benchtop Multimeter." },
  { "en": "Keuntungan benchtop multimeter?", "id": "Akurasi sangat tinggi, fitur lengkap." },
  { "en": "Multimeter presisi tinggi memiliki berapa digit?", "id": "6 ½ atau lebih." },
  { "en": "Probe berbentuk pinset?", "id": "SMD Tweezers." },
  { "en": "Tujuan SMD Tweezers?", "id": "Mengukur komponen SMD kecil." },
  { "en": "Pengukuran hambatan sangat rendah?", "id": "Gunakan metode empat kabel (Kelvin)." },
  { "en": "Tujuan pengukuran empat kabel?", "id": "Menghilangkan hambatan kabel probe." },
  { "en": "Mengapa probe tidak boleh ditinggal di soket arus?", "id": "Risiko korsleting saat ukur tegangan." },
  { "en": "Probe dengan sekring di dalamnya?", "id": "Fused test leads." },
  { "en": "Prosedur keamanan sebelum kerja di sirkuit?", "id": "Lock-Out Tag-Out (LOTO)." },
  { "en": "Peringkat ketahanan terhadap debu dan air?", "id": "IP Rating." },
  { "en": "Arti IP67?", "id": "Tahan debu total, tahan air." },
  { "en": "Apa itu 'resolusi' multimeter?", "id": "Perubahan terkecil yang bisa dideteksi." },
  { "en": "Apa itu 'akurasi' multimeter?", "id": "Seberapa dekat hasil ke nilai sebenarnya." },
  { "en": "Spesifikasi akurasi biasanya dalam?", "id": "Persentase (%) plus jumlah digit." },
  { "en": "Aksesoris magnet untuk menggantung multimeter?", "id": "Magnetic hanger." },
  { "en": "Uji baterai 9V di bawah beban?", "id": "Hasilnya lebih dapat diandalkan." },
  { "en": "Mengukur arus bocor pada mobil?", "id": "Parasitic drain test." },
  { "en": "Arus yang ditarik saat mobil mati?", "id": "Parasitic drain." },
  { "en": "Mencari sekring putus di mobil?", "id": "Ukur tegangan di kedua sisi sekring." },
  { "en": "Membaca kode warna resistor 4 gelang?", "id": "Dua digit, pengali, toleransi." },
  { "en": "Membaca kode warna resistor 5 gelang?", "id": "Tiga digit, pengali, toleransi." },
  { "en": "Warna gelang toleransi 5%?", "id": "Emas." },
  { "en": "Warna gelang toleransi 10%?", "id": "Perak." },
  { "en": "Perbedaan antara VAC dan VDC?", "id": "Tegangan bolak-balik dan searah." },
  { "en": "Mengukur tegangan output USB?", "id": "Gunakan mode VDC (sekitar 5V)." },
  { "en": "Mengukur hambatan speaker?", "id": "Gunakan mode Ohm (misal 4Ω, 8Ω)." },
  { "en": "Mode 'relative' (REL) disebut juga?", "id": "Zero mode." },
  { "en": "Menghilangkan hambatan probe dari pengukuran?", "id": "Gunakan mode REL." },
  { "en": "Mengukur arus dengan clamp meter?", "id": "Jepit kabel tunggal, bukan keduanya." },
  { "en": "Mengapa hanya satu kabel?", "id": "Agar medan magnet tidak saling meniadakan." },
  { "en": "Mengukur arus DC dengan clamp meter?", "id": "Butuh clamp meter efek Hall." },
  { "en": "Alat untuk memverifikasi akurasi multimeter?", "id": "Multifunction Calibrator." },
  { "en": "Apa itu 'autoranging'?", "id": "Multimeter memilih rentang secara otomatis." },
  { "en": "Keuntungan autoranging?", "id": "Lebih mudah dan cepat digunakan." },
  { "en": "Kerugian autoranging?", "id": "Sedikit lebih lambat dari manual." },
  { "en": "Tampilan 1 di digit paling kiri?", "id": "Over-range atau melebihi batas." },
  { "en": "Mengukur tegangan yang berfluktuasi?", "id": "Gunakan MIN/MAX/AVG atau bar graph." },
  { "en": "Probe RF (frekuensi tinggi)?", "id": "Untuk mengukur tegangan sinyal RF." },
  { "en": "Pengukuran miliampere (mA)?", "id": "Untuk sirkuit elektronik daya rendah." },
  { "en": "Pengukuran mikroampere (µA)?", "id": "Untuk arus sangat kecil." },
  { "en": "Arus listrik adalah aliran?", "id": "Elektron." },
  { "en": "Tegangan listrik adalah?", "id": "Beda potensial listrik." },
  { "en": "Hambatan listrik adalah?", "id": "Oposisi terhadap aliran arus." },
  { "en": "Daya listrik dihitung dengan?", "id": "Tegangan dikali arus (P=VI)." },
  { "en": "Bisa ukur daya dengan multimeter?", "id": "Tidak langsung, harus hitung manual." },
  { "en": "Alat ukur daya listrik langsung?", "id": "Wattmeter." },
  { "en": "Menguji kontinuitas ground pada alat?", "id": "Probe antara pin ground dan sasis." },
  { "en": "Tampilan bar graph pada DMM?", "id": "Memberi 'rasa' seperti multimeter analog." },
  { "en": "Memilih multimeter pertama kali?", "id": "Pilih yang auto-ranging, aman." },
  { "en": "Membersihkan multimeter?", "id": "Gunakan kain lembab, jangan pelarut." },
  { "en": "Jangan pernah mengukur hambatan pada?", "id": "Kapasitor yang masih terisi." },
  { "en": "Mengapa berbahaya?", "id": "Pelepasan muatan bisa merusak meter." },
  { "en": "Bentuk gelombang AC dari PLN?", "id": "Gelombang sinus (sine wave)." },
  { "en": "Bentuk gelombang output inverter termodifikasi?", "id": "Modified sine wave." },
  { "en": "DMM murah akan salah membaca?", "id": "Gelombang non-sinusoidal." },
  { "en": "Mengapa hasil ukur tidak pernah 100% akurat?", "id": "Karena ada toleransi dan batasan." },
  { "en": "Mengecek bias transistor?", "id": "Ukur tegangan Vbe dan Vce." },
  { "en": "Tegangan Vbe transistor silikon aktif?", "id": "Sekitar 0.7V." },
  { "en": "Mode 'Peak Hold' berguna untuk?", "id": "Menangkap lonjakan tegangan/arus sesaat." },
  { "en": "Contoh penggunaan Peak Hold?", "id": "Mengukur arus 'inrush'." },
  { "en": "Tombol 'Select' pada DMM?", "id": "Memilih fungsi pada posisi saklar sama." },
  { "en": "Apa itu 'standoff voltage'?", "id": "Tegangan maksimum sebelum komponen rusak." },
  { "en": "Peringatan 'lead' di layar?", "id": "Peringatan probe di soket yang salah." },
  { "en": "Selalu hubungkan probe ground terlebih dahulu?", "id": "Ya, untuk keamanan." },
  { "en": "Saat melepas, lepas probe 'live' dulu?", "id": "Ya, untuk keamanan." },
  { "en": "Kelas insulasi pada probe?", "id": "Menunjukkan proteksi ganda (double insulated)." },
  { "en": "Simbol kotak di dalam kotak?", "id": "Insolasi ganda." },
  { "en": "Frekuensi maksimum yang bisa diukur?", "id": "Tergantung spesifikasi multimeter." },
  { "en": "Mengukur hambatan internal baterai?", "id": "Membutuhkan metode pengukuran khusus." },
  { "en": "Alat ukur LCR?", "id": "Untuk ukur Induktansi, Kapasitansi, Resistansi." },
  { "en": "Kapan pakai osiloskop, bukan multimeter?", "id": "Saat ingin melihat bentuk gelombang." },
  { "en": "Kapan pakai multimeter, bukan osiloskop?", "id": "Untuk pengukuran presisi cepat." },
  { "en": "Alat ukur L, C, dan R presisi?", "id": "LCR Meter." },
  { "en": "Mengapa LCR meter lebih baik?", "id": "Menggunakan frekuensi tes yang spesifik." },
  { "en": "Kelebihan multimeter analog?", "id": "Responsif terhadap perubahan cepat." },
  { "en": "Kelebihan multimeter digital?", "id": "Akurasi tinggi, mudah dibaca." },
  { "en": "Kekurangan multimeter analog?", "id": "Kurang akurat, bisa salah baca." },
  { "en": "Kekurangan multimeter auto-ranging?", "id": "Respon sedikit lebih lambat." },
  { "en": "Mengapa mengukur arus tidak boleh paralel?", "id": "Karena akan menyebabkan korsleting." },
  { "en": "Hambatan internal multimeter mode amperemeter?", "id": "Sangat rendah (hampir nol)." },
  { "en": "Mengapa ukur hambatan tidak boleh ada daya?", "id": "Tegangan eksternal merusak meter." },
  { "en": "Tegangan maju LED warna merah?", "id": "Sekitar 1.8V - 2.0V." },
  { "en": "Tegangan maju LED warna biru/putih?", "id": "Sekitar 3.0V - 3.3V." },
  { "en": "Menguji linearitas potensiometer?", "id": "Ukur hambatan wiper saat diputar." },
  { "en": "Sambungan solder yang baik hambatannya?", "id": "Sangat rendah, mendekati nol." },
  { "en": "Mencari 'cold solder joint'?", "id": "Cari hambatan yang tidak stabil." },
  { "en": "Cara cek ground di stop kontak?", "id": "Ukur tegangan antara netral-ground." },
  { "en": "Tegangan netral ke ground idealnya?", "id": "Sangat kecil atau nol Volt." },
  { "en": "Spesifikasi akurasi: ±(1% + 2)?", "id": "1% dari pembacaan + 2 digit." },
  { "en": "Arti 'digit' dalam spesifikasi akurasi?", "id": "Hitungan pada digit terakhir." },
  { "en": "Batas Crest Factor untuk True RMS?", "id": "Menentukan seberapa puncak gelombang bisa diukur." },
  { "en": "Bandwidth AC pada multimeter?", "id": "Rentang frekuensi pengukuran akurat." },
  { "en": "Jika frekuensi sinyal terlalu tinggi?", "id": "Pembacaan tegangan AC tidak akurat." },
  { "en": "Pengukuran tanpa referensi ke ground bumi?", "id": "Floating measurement." },
  { "en": "Contoh 'floating measurement'?", "id": "Mengukur antar dua titik di sirkuit." },
  { "en": "Ground sasis adalah?", "id": "Titik referensi 0V pada sasis." },
  { "en": "Ground bumi adalah?", "id": "Koneksi fisik ke tanah." },
  { "en": "Cara kerja uji kontinuitas?", "id": "Mengirim arus kecil, cek tegangan." },
  { "en": "Ambang batas (threshold) untuk 'beep'?", "id": "Biasanya di bawah 50 Ohm." },
  { "en": "Cara kerja uji dioda?", "id": "Memberi arus, mengukur jatuh tegangan." },
  { "en": "Prinsip kerja pengukuran Kelvin?", "id": "Memisahkan jalur arus dan tegangan." },
  { "en": "Dua probe pada metode Kelvin untuk?", "id": "Mengirim arus (force)." },
  { "en": "Dua probe lainnya untuk?", "id": "Membaca tegangan (sense)." },
  { "en": "Cara memeriksa sekring internal multimeter?", "id": "Gunakan mode resistansi." },
  { "en": "Langkah pertama cek sekring internal?", "id": "Pindahkan saklar ke mode Ohm." },
  { "en": "Sekring bagus jika meter menunjukkan?", "id": "Hambatan rendah (tidak OL)." },
  { "en": "Gunakan probe dengan rating CAT yang?", "id": "Sama atau lebih tinggi dari meter." },
  { "en": "Menyimpan multimeter untuk waktu lama?", "id": "Lepaskan baterai." },
  { "en": "Mengapa baterai harus dilepas?", "id": "Mencegah kebocoran dan korosi." },
  { "en": "Apa itu 'insulation resistance'?", "id": "Hambatan antara konduktor dan ground." },
  { "en": "Alat ukur 'insulation resistance'?", "id": "Megohmmeter atau Insulation Tester." },
  { "en": "Tegangan uji pada Megohmmeter?", "id": "Sangat tinggi (500V, 1000V)." },
  { "en": "Mengapa tegangan uji harus tinggi?", "id": "Untuk menemukan kebocoran isolasi kecil." },
  { "en": "Uji kapasitor starter motor?", "id": "Ukur kapasitansinya dengan mode Farad." },
  { "en": "Menemukan kaki Basis transistor?", "id": "Kaki yang terhubung ke dua lainnya." },
  { "en": "Mengukur arus pengisian baterai?", "id": "Pasang meter seri antara charger-baterai." },
  { "en": "Arus AC dan DC dalam satu sirkuit?", "id": "Ukur terpisah dengan mode AC/DC." },
  { "en": "Fitur 'Auto-Hold' atau 'Touch-Hold'?", "id": "Menahan pembacaan stabil di layar." },
  { "en": "Bagaimana multimeter mengukur frekuensi?", "id": "Menghitung jumlah 'zero crossing'." },
  { "en": "Mengukur hambatan dari sensor suhu?", "id": "Gunakan mode Ohm (untuk termistor)." },
  { "en": "Termistor NTC?", "id": "Hambatan turun jika suhu naik." },
  { "en": "Termistor PTC?", "id": "Hambatan naik jika suhu naik." },
  { "en": "Probe dengan penjepit untuk komponen?", "id": "IC test clips." },
  { "en": "Menghubungkan multimeter ke osiloskop?", "id": "Tidak umum, fungsi berbeda." },
  { "en": "Sinyal kalibrasi dari multimeter?", "id": "Tidak ada, multimeter alat ukur." },
  { "en": "Mengukur tegangan bias pada transistor?", "id": "Gunakan mode VDC presisi." },
  { "en": "Apa itu 'common mode voltage'?", "id": "Tegangan yang sama pada kedua input." },
  { "en": "Differential amplifier menolak?", "id": "Common mode voltage." },
  { "en": "Kapan kalibrasi ulang diperlukan?", "id": "Secara berkala atau setelah diperbaiki." },
  { "en": "Fungsi 'Peak MIN/MAX'?", "id": "Menangkap puncak durasi sangat singkat." },
  { "en": "Resolusi pembacaan adalah?", "id": "Digit paling kanan." },
  { "en": "Pengaruh suhu pada akurasi?", "id": "Bisa menyebabkan penyimpangan (drift)." },
  { "en": "Koefisien suhu pada spesifikasi?", "id": "Menunjukkan seberapa besar drift." },
  { "en": "Uji 'load test' pada catu daya?", "id": "Mengukur tegangan saat diberi beban." },
  { "en": "Catu daya yang baik tegangannya?", "id": "Tetap stabil di bawah beban." },
  { "en": "Mencari tahu kaki anoda/katoda LED?", "id": "Kaki lebih panjang biasanya anoda." },
  { "en": "Cara lain mengetahui kaki LED?", "id": "Sisi datar pada bodi adalah katoda." },
  { "en": "Mengukur arus pada loop 4-20mA?", "id": "Gunakan mode mA DC secara seri." },
  { "en": "Loop 4-20mA digunakan di?", "id": "Kontrol proses industri." },
  { "en": "4mA pada loop merepresentasikan?", "id": "Nilai minimum (0%)." },
  { "en": "20mA pada loop merepresentasikan?", "id": "Nilai maksimum (100%)." },
  { "en": "Memeriksa 'ground integrity'?", "id": "Uji kontinuitas antara sasis dan ground." },
  { "en": "Alat yang menggabungkan banyak fungsi?", "id": "Multimeter." },
  { "en": "Kesalahan paling umum pemula?", "id": "Salah pasang probe saat ukur arus." },
  { "en": "Indikator baris ganda pada layar?", "id": "Bisa menampilkan dua pengukuran." },
  { "en": "Contoh tampilan ganda?", "id": "Tegangan AC dan frekuensinya." },
  { "en": "Pelindung ujung probe?", "id": "Probe tip cover." },
  { "en": "Tujuan probe tip cover?", "id": "Mencegah korsleting tak sengaja." },
  { "en": "Apa itu 'in-circuit' measurement?", "id": "Pengukuran saat komponen di rangkaian." },
  { "en": "Pengukuran 'in-circuit' bisa tidak akurat?", "id": "Ya, karena dipengaruhi komponen lain." },
  { "en": "Mode 'Ohm' relatif terhadap?", "id": "Resistansi internal probe." },
  { "en": "Pembacaan 'OL' saat uji dioda berarti?", "id": "Reverse bias atau dioda putus." },
  { "en": "Pembacaan '0.0V' saat uji dioda berarti?", "id": "Dioda korsleting (short)." },
  { "en": "Dapatkah multimeter mengukur daya (Watt)?", "id": "Tidak secara langsung." },
  { "en": "Perhitungan daya dari hasil ukur?", "id": "Daya = Tegangan x Arus." },
  { "en": "Kategori pengukuran I, II, III, IV?", "id": "CAT I, II, III, IV." },
  { "en": "Semakin tinggi nomor CAT, semakin?", "id": "Tinggi proteksi terhadap transien." },
  { "en": "Mengapa proteksi transien penting?", "id": "Melindungi pengguna dari lonjakan bahaya." },
  { "en": "Jangan pernah mengganti sekring dengan?", "id": "Nilai yang salah atau kawat." },
  { "en": "Mengukur tegangan pada stop kontak?", "id": "Atur ke VAC rentang tertinggi." },
  { "en": "Ujung probe tidak boleh menyentuh?", "id": "Satu sama lain saat mengukur." },
  { "en": "Sarung tangan untuk kerja tegangan tinggi?", "id": "Sarung tangan berinsulasi." },
  { "en": "Selalu bekerja dengan satu tangan jika?", "id": "Memungkinkan, untuk mengurangi risiko." },
  { "en": "Mengapa satu tangan lebih aman?", "id": "Mencegah arus melewati jantung." },
  { "en": "Arti 'average responding' DMM?", "id": "Mengukur rata-rata, akurat untuk sinus." },
  { "en": "Multimeter untuk HVAC biasanya punya?", "id": "Pengukuran suhu dan mikroampere." },
  { "en": "Multimeter untuk otomotif biasanya punya?", "id": "Pengukuran RPM dan Dwell Angle." },
  { "en": "Layar multimeter mati, periksa apa?", "id": "Periksa baterai dan posisi saklar." },
  { "en": "Hasil pengukuran tidak masuk akal, periksa?", "id": "Posisi saklar dan colokan probe." },
  { "en": "Bahaya terbesar saat menggunakan multimeter?", "id": "Mengukur tegangan di mode arus." },
  { "en": "Menguji fotodioda?", "id": "Ukur hambatan saat terang dan gelap." },
  { "en": "Menguji termokopel?", "id": "Ukur tegangan milivolt saat dipanaskan." },
  { "en": "Hambatan seri internal kapasitor?", "id": "ESR (Equivalent Series Resistance)." },
  { "en": "ESR yang tinggi menandakan?", "id": "Kapasitor sudah kering atau rusak." },
  { "en": "Alat ukur ESR presisi?", "id": "ESR Meter." },
  { "en": "Menguji gerbang (gate) pada SCR/TRIAC?", "id": "Ukur seperti dioda ke katoda." },
  { "en": "Mengukur respons frekuensi speaker?", "id": "Butuh generator sinyal dan multimeter." },
  { "en": "Jatuh tegangan pada mode Amperemeter?", "id": "Burden voltage." },
  { "en": "Efek 'burden voltage'?", "id": "Bisa mengubah perilaku sirkuit." },
  { "en": "Efek 'loading' saat ukur tegangan?", "id": "Menurunkan tegangan sirkuit sedikit." },
  { "en": "Resolusi DMM ditentukan oleh?", "id": "Jumlah 'count' atau digit." },
  { "en": "Bagaimana auto-ranging bekerja?", "id": "Memilih pembagi tegangan secara otomatis." },
  { "en": "Bagaimana True RMS bekerja?", "id": "Menghitung nilai efektif panas sinyal." },
  { "en": "Noise yang masuk melalui kedua probe?", "id": "Common-mode noise." },
  { "en": "Noise yang ada pada sinyal itu sendiri?", "id": "Normal-mode noise." },
  { "en": "Contoh APD (PPE) saat mengukur?", "id": "Sarung tangan, kacamata pengaman." },
  { "en": "Prinsip 'test before you touch'?", "id": "Pastikan meter bekerja sebelum sentuh sirkuit." },
  { "en": "Cara memverifikasi multimeter bekerja?", "id": "Ukur sumber yang diketahui (stop kontak)." },
  { "en": "Pentingnya mengetahui limitasi instrumen?", "id": "Untuk pengukuran aman dan akurat." },
  { "en": "Multimeter analog pertama yang sukses?", "id": "AVOmeter." },
  { "en": "Mengapa teknisi radio lama suka meter analog?", "id": "Mudah melihat 'tuning peaks'." },
  { "en": "Masa depan multimeter?", "id": "Konektivitas, analisis data, lebih aman." },
  { "en": "Mengukur V_fwd LED putih?", "id": "Gunakan mode dioda, sekitar 3V." },
  { "en": "Menguji saklar DPDT?", "id": "Uji kontinuitas di semua kombinasi." },
  { "en": "Apa itu 'AC+DC' pada DMM?", "id": "Mengukur nilai True RMS total." },
  { "en": "Menentukan kaki-kaki transistor tak dikenal?", "id": "Gunakan mode hFE atau uji dioda." },
  { "en": "Pengukuran empat titik disebut juga?", "id": "Pengukuran Kelvin." },
  { "en": "Mengapa DMM butuh waktu untuk stabil?", "id": "Waktu settling ADC dan sirkuit." },
  { "en": "Tes 'ring' pada transformator?", "id": "Metode usang, butuh alat khusus." },
  { "en": "Mengukur tegangan bias di sirkuit RF?", "id": "Butuh probe RF DC-coupled." },
  { "en": "Tampilan '-OL' atau '-.---'?", "id": "Input negatif melebihi rentang." },
  { "en": "Fitur 'capture' pada DMM?", "id": "Merekam event transien cepat." },
  { "en": "Logam pada ujung probe terbuat dari?", "id": "Baja atau kuningan berlapis." },
  { "en": "Mengapa ujung probe harus runcing?", "id": "Untuk kontak presisi pada titik kecil." },
  { "en": "Apa itu 'dry cell test'?", "id": "Mode uji baterai (misal 1.5V)." },
  { "en": "Kalibrasi DMM biasanya menyesuaikan?", "id": "Referensi tegangan internal." },
  { "en": "Rentang dinamis (dynamic range)?", "id": "Rasio nilai terbesar dan terkecil." },
  { "en": "Standar industri untuk kalibrasi?", "id": "ISO/IEC 17025." },
  { "en": "Nilai rata-rata dari gelombang AC sinus?", "id": "Nol Volt." },
  { "en": "Multimeter AC non-RMS mengukur apa?", "id": "Nilai rata-rata yang diperbaiki." },
  { "en": "Fungsi 'crest factor' penting untuk?", "id": "Sinyal dengan puncak tinggi." },
  { "en": "Contoh sinyal 'high crest factor'?", "id": "Output dari beberapa catu daya." },
  { "en": "Mengukur arus bocor AC?", "id": "Gunakan clamp meter arus bocor." },
  { "en": "Resolusi clamp meter arus bocor?", "id": "Sangat tinggi (mikroampere)." },
  { "en": "Mengukur hambatan dari tubuh manusia?", "id": "Bisa, tapi sangat tidak konsisten." },
  { "en": "Faktor yang mempengaruhi hambatan tubuh?", "id": "Kelembaban kulit, tegangan." },
  { "en": "Alat ukur ketahanan isolasi?", "id": "Insulation Tester / Megger." },
  { "en": "Mengukur komponen SMD tanpa probe khusus?", "id": "Sangat sulit dan berisiko." },
  { "en": "Multimeter dengan fitur osiloskop?", "id": "Scopemeter." },
  { "en": "Multimeter grafis?", "id": "Bisa memplot data dari waktu ke waktu." },
  { "en": "Mode 'auto-ranging' bisa dikunci?", "id": "Ya, dengan tombol RANGE." },
  { "en": "Membaca resistansi 'in-circuit'?", "id": "Hasilnya bisa lebih rendah." },
  { "en": "Mengapa hasilnya bisa lebih rendah?", "id": "Karena ada jalur paralel lain." },
  { "en": "Mengukur arus dengan probe tegangan?", "id": "Tidak bisa dilakukan." },
  { "en": "Mengukur tegangan dengan probe arus?", "id": "Tidak bisa dilakukan." },
  { "en": "Cara memeriksa kapasitor elektrolit?", "id": "Perhatikan polaritasnya." },
  { "en": "Polaritas kapasitor elektrolit?", "id": "Tanda negatif (-) pada bodi." },
  { "en": "Memasang kapasitor elektrolit terbalik?", "id": "Bisa meledak atau rusak." },
  { "en": "Mengukur tegangan AC di atas frekuensi tinggi?", "id": "Hasilnya akan turun (tidak akurat)." },
  { "en": "Berapa impedansi input mode Volts?", "id": "Biasanya 10 Megaohm." },
  { "en": "Berapa impedansi input mode Amps?", "id": "Sangat rendah (mendekati nol)." },
  { "en": "Penyebab utama kerusakan multimeter?", "id": "Kesalahan pengguna." },
  { "en": "Selalu periksa pengaturan sebelum?", "id": "Menyentuhkan probe ke sirkuit." },
  { "en": "Mengukur tegangan yang tidak diketahui?", "id": "Mulai dari rentang VAC tertinggi." },
  { "en": "Mengapa mulai dari rentang tertinggi?", "id": "Untuk melindungi multimeter." },
  { "en": "Fungsi 'zero' pada mode resistansi?", "id": "Mengurangi resistansi kabel probe." },
  { "en": "DMM yang bisa dihubungkan ke komputer?", "id": "Untuk akuisisi data." },
  { "en": "DMM yang bisa mencatat data tanpa komputer?", "id": "Memiliki fitur data logging." },
  { "en": "Mengapa ada sekring terpisah untuk mA dan A?", "id": "Untuk proteksi di rentang berbeda." },
  { "en": "Sekring untuk rentang Ampere biasanya?", "id": "Sekring berkapasitas tinggi." },
  { "en": "Sekring untuk rentang miliampere biasanya?", "id": "Sekring berkapasitas rendah." },
  { "en": "DMM tahan banting?", "id": "Memiliki rating 'drop proof'." },
  { "en": "Mengukur arus pada sirkuit DC?", "id": "Perhatikan polaritas masuk dan keluar." },
  { "en": "Polaritas terbalik saat ukur arus DC?", "id": "Hasilnya akan negatif." },
  { "en": "Apa itu 'slew rate'?", "id": "Laju perubahan sinyal." },
  { "en": "Bisa ukur slew rate dengan multimeter?", "id": "Tidak, butuh osiloskop." },
  { "en": "Menguji 'solar cell'?", "id": "Ukur Voc dan Isc." },
  { "en": "Voc pada solar cell diukur saat?", "id": "Tidak ada beban." },
  { "en": "Isc pada solar cell diukur saat?", "id": "Output dihubung singkat." },
  { "en": "Multimeter analog menggunakan sumber daya?", "id": "Baterai (hanya untuk mode Ohm)." },
  { "en": "Multimeter analog ukur V/A tanpa baterai?", "id": "Ya, daya dari sirkuit." },
  { "en": "Bagaimana multimeter analog ukur AC?", "id": "Menggunakan penyearah dioda internal." },
  { "en": "Simbol tak terhingga di skala analog?", "id": "∞." },
  { "en": "Uji kontinuitas pada PCB?", "id": "Memeriksa apakah jalur (trace) terhubung." },
  { "en": "Uji isolasi pada PCB?", "id": "Memeriksa jalur tidak saling terhubung." },
  { "en": "Mengapa tidak boleh menyentuh komponen saat ukur Ohm?", "id": "Hambatan tubuh mempengaruhi hasil." },
  { "en": "Apa itu 'out-of-circuit' measurement?", "id": "Mengukur komponen yang dilepas." },
  { "en": "Pengukuran paling akurat?", "id": "Pengukuran out-of-circuit." },
  { "en": "Tampilan 'Auto' pada layar DMM?", "id": "Mode auto-ranging aktif." },
  { "en": "Mengukur tegangan baterai mobil?", "id": "Gunakan mode VDC (sekitar 12.6V)." },
  { "en": "Mengukur tegangan saat mesin menyala?", "id": "Tegangan pengisian (sekitar 14V)." },
  { "en": "Alat bantu untuk probe yang aman?", "id": "Probe grabbers atau hook clips." },
  { "en": "Tujuan utama multimeter bagi pemula?", "id": "Debugging dan verifikasi sirkuit." },
  { "en": "Langkah pertama dalam troubleshooting?", "id": "Periksa sumber daya (tegangan)." },
  { "en": "Langkah pertama sebelum menyentuh sirkuit?", "id": "Verifikasi sirkuit tidak bertegangan." },
  { "en": "Kesalahan paling fatal bagi pemula?", "id": "Mengukur tegangan saat probe di soket A." },
  { "en": "Ukur tegangan selalu secara?", "id": "Paralel." },
  { "en": "Ukur arus selalu secara?", "id": "Seri." },
  { "en": "Ukur hambatan selalu saat?", "id": "Sirkuit mati total." },
  { "en": "Siapa penemu multimeter pertama (AVOmeter)?", "id": "Donald Macadie." },
  { "en": "Mengapa multimeter digital menggantikan analog?", "id": "Lebih akurat dan mudah dibaca." },
  { "en": "Kapan meter analog masih berguna?", "id": "Melihat tren atau puncak cepat." },
  { "en": "Multimeter untuk mengukur RPM mesin?", "id": "Multimeter otomotif." },
  { "en": "Fungsi 'dwell angle' untuk?", "id": "Sistem pengapian mobil lama." },
  { "en": "Multimeter untuk teknisi pemanas/pendingin?", "id": "Multimeter HVAC." },
  { "en": "Fitur khusus multimeter HVAC?", "id": "Ukur suhu, arus mikroampere (µA)." },
  { "en": "Arus mikroampere pada HVAC untuk?", "id": "Menguji sensor api (flame sensor)." },
  { "en": "Multimeter untuk lingkungan mudah meledak?", "id": "Intrinsically Safe (IS) Multimeter." },
  { "en": "Setiap pengukuran memiliki?", "id": "Ketidakpastian (uncertainty)." },
  { "en": "Standar referensi untuk kalibrasi?", "id": "Calibration standard." },
  { "en": "Apakah multimeter membebani sirkuit?", "id": "Ya, sedikit (loading effect)." },
  { "en": "Koneksi hambatan sangat rendah tak diinginkan?", "id": "Hubung singkat (short circuit)." },
  { "en": "Koneksi terputus tak diinginkan?", "id": "Sirkuit terbuka (open circuit)." },
  { "en": "Kondisi dioda menghantarkan arus?", "id": "Bias maju (forward bias)." },
  { "en": "Kondisi dioda memblokir arus?", "id": "Bias mundur (reverse bias)." },
  { "en": "Titik referensi ground pada sasis alat?", "id": "Chassis ground." },
  { "en": "Koneksi ke tanah untuk keamanan?", "id": "Earth ground." },
  { "en": "Tips merawat probe multimeter?", "id": "Jaga kebersihan, jangan ditekuk." },
  { "en": "Jika ujung probe tumpul?", "id": "Bisa diasah atau diganti." },
  { "en": "Hal utama saat membeli multimeter?", "id": "Keamanan (rating CAT), fitur." },
  { "en": "Fitur yang wajib ada untuk elektronikawan?", "id": "Uji kontinuitas dan dioda." },
  { "en": "Fitur yang wajib ada untuk listrik?", "id": "Pengukuran VAC dan AAC." },
  { "en": "Cara menemukan 'parasitic drain' mobil?", "id": "Ukur arus saat mobil mati." },
  { "en": "Apa itu 'AC ripple'?", "id": "Sisa tegangan AC pada output DC." },
  { "en": "Penyebab AC ripple pada catu daya?", "id": "Kapasitor filter yang buruk." },
  { "en": "Alat untuk melihat AC ripple terbaik?", "id": "Osiloskop." },
  { "en": "Bisa ukur AC ripple dengan DMM?", "id": "Ya, gunakan mode VAC." },
  { "en": "Apa itu 'tan-delta' test?", "id": "Pengujian faktor disipasi kapasitor." },
  { "en": "Bisa ukur 'tan-delta' dengan DMM?", "id": "Tidak, butuh LCR meter." },
  { "en": "Tampilan 1 di digit pertama DMM?", "id": "Menandakan Over Load (OL)." },
  { "en": "Mengukur dioda Zener?", "id": "Butuh sumber tegangan eksternal." },
  { "en": "Menguji SCR (Silicon-Controlled Rectifier)?", "id": "Membutuhkan metode pengujian khusus." },
  { "en": "Penjepit untuk probe agar bebas tangan?", "id": "Alligator clips." },
  { "en": "Mengukur hambatan motor tiga fasa?", "id": "Hambatan antar fasa harus seimbang." },
  { "en": "Hambatan motor ke ground (sasis)?", "id": "Harus sangat tinggi (tak terhingga)." },
  { "en": "Pembacaan yang 'melayang' (drifting)?", "id": "Bisa karena koneksi buruk." },
  { "en": "Mode REL berguna untuk?", "id": "Mengukur deviasi dari suatu nilai." },
  { "en": "Mencocokkan beberapa resistor?", "id": "Gunakan mode REL atau Ohm presisi." },
  { "en": "Kenapa ada kategori sekring (FF, F, T)?", "id": "Menunjukkan kecepatan putus sekring." },
  { "en": "Sekring 'fast-acting' (F)?", "id": "Cepat putus saat ada lonjakan." },
  { "en": "Sekring 'slow-blow' (T)?", "id": "Tahan terhadap lonjakan sesaat." },
  { "en": "Satuan 'counts' berhubungan dengan?", "id": "Resolusi multimeter." },
  { "en": "Lebih tinggi 'count', lebih baik?", "id": "Ya, untuk resolusi yang lebih baik." },
  { "en": "Simbol 'baterai' di layar berarti?", "id": "Baterai perlu segera diganti." },
  { "en": "Pengukuran yang dipengaruhi suhu sekitar?", "id": "Hambatan dan semikonduktor." },
  { "en": "Uji 'drop test' pada multimeter?", "id": "Menunjukkan ketahanan terhadap benturan." },
  { "en": "Mengapa ada lubang kecil di ujung probe?", "id": "Untuk memasang aksesoris." },
  { "en": "Batas frekuensi untuk pengukuran VAC?", "id": "Lihat di spesifikasi (AC bandwidth)." },
  { "en": "Multimeter dengan pencatatan data?", "id": "Data logging multimeter." },
  { "en": "Mengapa penting membaca manual multimeter?", "id": "Memahami semua fitur dan keamanannya." },
  { "en": "Apa itu 'EMF'?", "id": "Electromotive Force (sinonim tegangan)." },
  { "en": "Mengukur EMF baterai?", "id": "Ukur tegangan saat tanpa beban." },
  { "en": "Mengukur 'terminal voltage' baterai?", "id": "Ukur tegangan saat ada beban." },
  { "en": "Terminal voltage selalu lebih rendah dari EMF?", "id": "Ya, karena ada hambatan internal." },
  { "en": "Penjepit 'bed of nails'?", "id": "Untuk mengakses banyak titik uji." },
  { "en": "Menguji varistor (VDR)?", "id": "Hambatan tinggi di bawah tegangan nominal." },
  { "en": "Menguji transformator?", "id": "Ukur hambatan kumparan primer/sekunder." },
  { "en": "Hambatan kumparan transformator yang baik?", "id": "Rendah, tidak putus (OL)." },
  { "en": "Uji isolasi antar kumparan?", "id": "Hambatan harus tak terhingga (OL)." },
  { "en": "Uji kontinuitas pada 'thermal fuse'?", "id": "Harus menunjukkan kontinuitas." },
  { "en": "Multimeter sebagai termometer?", "id": "Ya, dengan probe suhu." },
  { "en": "Konektor untuk probe suhu tipe-K?", "id": "Konektor pipih dua pin." },
  { "en": "Menentukan fase, netral, ground?", "id": "Fase-Netral (220V), Fase-Ground (220V)." },
  { "en": "Tegangan Netral ke Ground?", "id": "Idealnya 0V." },
  { "en": "Mengukur arus tanpa memutus sirkuit?", "id": "Gunakan clamp meter." },
  { "en": "Apa itu 'auto-polarity'?", "id": "DMM otomatis menunjukkan tanda +/-." },
  { "en": "Fitur 'touch hold' berbeda dari 'hold'?", "id": "Touch hold otomatis, hold manual." },
  { "en": "Mengecek kesehatan kapasitor elektrolit?", "id": "Cek fisik (menggembung/bocor)." },
  { "en": "Ukur kapasitansi 'in-circuit'?", "id": "Tidak akurat, lepas dulu komponen." },
  { "en": "Multimeter dengan rating IP54?", "id": "Tahan debu dan percikan air." },
  { "en": "Selalu periksa sekring yang benar?", "id": "Rating tegangan dan arusnya." },
  { "en": "Mengapa sekring DMM khusus?", "id": "Punya kapasitas pemutusan tinggi (HRC)." },
  { "en": "Multimeter analog disebut juga?", "id": "VOM (Volt-Ohm-Milliammeter)." },
  { "en": "Angka '2000 counts' berarti?", "id": "Bisa menampilkan 0 hingga 1999." },
  { "en": "Apa itu 'burden voltage' yang rendah?", "id": "Kurang mempengaruhi sirkuit yang diukur." },
  { "en": "Uji 'forward gain' transistor?", "id": "Menggunakan mode hFE." },
  { "en": "Mengukur arus quiescent (diam)?", "id": "Arus saat sirkuit menyala tanpa sinyal." },
  { "en": "Pengukuran desibel (dB) relatif terhadap?", "id": "Tegangan referensi (misal 1mW di 600Ω)." },
  { "en": "Fungsi 'duty cycle' berguna untuk?", "id": "Menganalisis sinyal PWM." },
  { "en": "Mengapa tidak boleh memutar saklar saat terhubung?", "id": "Bisa merusak multimeter atau sirkuit." },
  { "en": "Pembersihan layar DMM?", "id": "Gunakan kain lembut dan bersih." },
  { "en": "Perbedaan 'precision' dan 'accuracy'?", "id": "Konsistensi vs kedekatan dengan nilai asli." },
  { "en": "Multimeter yang ideal?", "id": "Aman, akurat, dan andal." },
  { "en": "Menguji kontinuitas saklar 'push button'?", "id": "Harus berbunyi saat ditekan." },
  { "en": "Mengapa DMM lebih populer dari analog?", "id": "Lebih akurat, tahan banting, banyak fitur." },
  { "en": "Memeriksa 'leakage' pada kapasitor?", "id": "Gunakan mode Ohm rentang tertinggi." },
  { "en": "Kapasitor bagus pada mode Ohm?", "id": "Hambatan harus menuju tak terhingga." },
  { "en": "Selalu ingat untuk mematikan multimeter?", "id": "Ya, untuk menghemat baterai." }




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
            }, 7500);
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
