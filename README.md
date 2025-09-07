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


  { "en": "Arti Kata 'Bae'?", "id": "Sayang Atau Pacar." },
  { "en": "Arti Kata 'Lit'?", "id": "Keren Sekali, Seru Sekali." },
  { "en": "Arti Kata 'Gucci'?", "id": "Bagus, Keren, Baik-Baik Saja." },
  { "en": "Arti Kata 'Salty'?", "id": "Kesal Karena Hal Sepele." },
  { "en": "Arti Kata 'Flex'?", "id": "Pamer Kekayaan Atau Pencapaian." },
  { "en": "Arti Kata 'Ghosting'?", "id": "Menghilang Tiba-Tiba Tanpa Kabar." },
  { "en": "Arti Kata 'Shook'?", "id": "Kaget, Terkejut, Tidak Percaya." },
  { "en": "Arti Kata 'Fam'?", "id": "Teman-Teman Terdekat Seperti Keluarga." },
  { "en": "Arti Kata 'Lowkey'?", "id": "Secara Diam-Diam, Tidak Terlalu Mencolok." },
  { "en": "Arti Kata 'Highkey'?", "id": "Secara Terang-Terangan, Sangat Jelas." },
  { "en": "Arti Kata 'Dope'?", "id": "Keren, Luar Biasa." },
  { "en": "Arti Kata 'Slay'?", "id": "Melakukan Sesuatu Dengan Sangat Baik." },
  { "en": "Arti Kata 'Stan'?", "id": "Penggemar Yang Sangat Setia." },
  { "en": "Arti Kata 'Tea'?", "id": "Gossip Atau Cerita Menarik." },
  { "en": "Arti Frasa 'Spill The Tea'?", "id": "Bongkar Gosipnya, Ceritakan Semuanya." },
  { "en": "Arti Kata 'Woke'?", "id": "Sadar Akan Isu Sosial." },
  { "en": "Arti Kata 'Cringe'?", "id": "Merasa Geli Atau Jijik." },
  { "en": "Arti Kata 'Squad'?", "id": "Geng Atau Kelompok Teman Dekat." },
  { "en": "Arti Kata 'Goals'?", "id": "Sesuatu Yang Sangat Diinginkan." },
  { "en": "Arti Frasa 'No Cap'?", "id": "Serius, Tidak Bohong." },
  { "en": "Arti Kata 'Bet'?", "id": "Oke, Setuju, Ayo Lakukan." },
  { "en": "Arti Kata 'Simp'?", "id": "Terlalu Baik Pada Seseorang (Naksir)." },
  { "en": "Arti Frasa 'Glow Up'?", "id": "Perubahan Penampilan Jadi Lebih Baik." },
  { "en": "Arti Kata 'Mood'?", "id": "Sangat Menggambarkan Perasaan Saat Ini." },
  { "en": "Arti Kata 'Vibe'?", "id": "Suasana Atau Aura." },
  { "en": "Arti Kata 'Boujee'?", "id": "Bergaya Mewah Atau Kelas Atas." },
  { "en": "Arti Kata 'Clout'?", "id": "Pengaruh Atau Popularitas Di Media Sosial." },
  { "en": "Arti Kata 'GOAT'?", "id": "Terbaik Sepanjang Masa." },
  { "en": "Kepanjangan 'GOAT'?", "id": "Greatest Of All Time." },
  { "en": "Arti Kata 'Shade'?", "id": "Menyindir Secara Tidak Langsung." },
  { "en": "Arti Frasa 'Throwing Shade'?", "id": "Sedang Menyindir Seseorang." },
  { "en": "Arti Kata 'Thirsty'?", "id": "Sangat Butuh Perhatian." },
  { "en": "Arti Kata 'Canceled'?", "id": "Diboikot Atau Tidak Didukung Lagi." },
  { "en": "Arti Kata 'Finna'?", "id": "Akan Melakukan Sesuatu." },
  { "en": "Arti Kata 'Bop'?", "id": "Lagu Yang Sangat Bagus." },
  { "en": "Arti Kata 'Whip'?", "id": "Mobil Yang Keren." },
  { "en": "Arti Kata 'Snack'?", "id": "Orang Yang Sangat Menarik." },
  { "en": "Arti Kata 'Yeet'?", "id": "Melempar Sesuatu Dengan Kekuatan." },
  { "en": "Arti Kata 'Drip'?", "id": "Gaya Berpakaian Yang Keren." },
  { "en": "Arti Frasa 'I'm Dead'?", "id": "Sangat Lucu Sampai Tidak Tahan." },
  { "en": "Arti Kata 'Basic'?", "id": "Sangat Umum, Mainstream." },
  { "en": "Arti Kata 'Extra'?", "id": "Berlebihan, Dramatis." },
  { "en": "Arti Kata 'Savage'?", "id": "Keren, Berani, Tidak Peduli." },
  { "en": "Arti Kata 'Fire'?", "id": "Sangat Bagus, Keren." },
  { "en": "Arti Kata 'Wack'?", "id": "Aneh, Jelek, Tidak Keren." },
  { "en": "Arti Kata 'Chill'?", "id": "Tenang, Santai." },
  { "en": "Arti Frasa 'Hang Out'?", "id": "Menghabiskan Waktu Bersama." },
  { "en": "Arti Frasa 'My Bad'?", "id": "Salahku, Maaf." },
  { "en": "Arti Kata 'Dude'?", "id": "Bung, Kawan (Panggilan Akrab)." },
  { "en": "Arti Kata 'Bro'?", "id": "Kawan, Saudara (Panggilan Akrab)." },
  { "en": "Arti Frasa 'What's Up'?", "id": "Apa Kabar?" },
  { "en": "Arti Kata 'Bummer'?", "id": "Sial, Menyebalkan." },
  { "en": "Arti Kata 'Legit'?", "id": "Asli, Keren, Sah." },
  { "en": "Arti Kata 'Swag'?", "id": "Gaya Yang Keren Dan Percaya Diri." },
  { "en": "Arti Kata 'YOLO'?", "id": "Hidup Cuma Sekali." },
  { "en": "Kepanjangan 'YOLO'?", "id": "You Only Live Once." },
  { "en": "Arti Frasa 'Couch Potato'?", "id": "Orang Yang Malas, Nonton TV Terus." },
  { "en": "Arti Kata 'Epic'?", "id": "Sangat Keren, Luar Biasa." },
  { "en": "Arti Kata 'Lame'?", "id": "Garing, Tidak Keren." },
  { "en": "Arti Kata 'Sick'?", "id": "Keren Banget (Konteks Positif)." },
  { "en": "Arti Frasa 'I Feel You'?", "id": "Aku Mengerti Perasaanmu." },
  { "en": "Arti Kata 'Nuts'?", "id": "Gila, Aneh." },
  { "en": "Arti Frasa 'Hooked On'?", "id": "Kecanduan Sesuatu." },
  { "en": "Arti Frasa 'Screw Up'?", "id": "Mengacaukan Sesuatu." },
  { "en": "Arti Kata 'Awesome'?", "id": "Keren, Luar Biasa." },
  { "en": "Arti Kata 'Cool'?", "id": "Keren, Santai." },
  { "en": "Arti Frasa 'I'm In'?", "id": "Aku Ikut." },
  { "en": "Arti Frasa 'I'm Out'?", "id": "Aku Tidak Ikut." },
  { "en": "Arti Kata 'Busted'?", "id": "Ketahuan Melakukan Sesuatu." },
  { "en": "Arti Kata 'Creep'?", "id": "Orang Aneh Yang Menyeramkan." },
  { "en": "Arti Kata 'Geek'?", "id": "Orang Yang Sangat Suka Teknologi." },
  { "en": "Arti Kata 'Nerd'?", "id": "Orang Yang Sangat Pintar/Kutu Buku." },
  { "en": "Arti Kata 'Kudos'?", "id": "Pujian, Salut." },
  { "en": "Arti Kata 'Gig'?", "id": "Pekerjaan Sementara, Pertunjukan Musik." },
  { "en": "Arti Kata 'Crash'?", "id": "Tidur, Menginap (Tidak Direncanakan)." },
  { "en": "Arti Frasa 'Aced It'?", "id": "Berhasil Dengan Sangat Baik." },
  { "en": "Arti Frasa 'Nailed It'?", "id": "Berhasil Dengan Sempurna." },
  { "en": "Arti Kata 'Flunk'?", "id": "Gagal (Ujian)." },
  { "en": "Arti Kata 'Dump'?", "id": "Memutuskan Pacar." },
  { "en": "Arti Frasa 'Have A Crush On'?", "id": "Naksir Seseorang." },
  { "en": "Arti Frasa 'Ride Or Die'?", "id": "Teman Yang Sangat Setia." },
  { "en": "Arti Kata 'BFF'?", "id": "Sahabat Selamanya." },
  { "en": "Kepanjangan 'BFF'?", "id": "Best Friends Forever." },
  { "en": "Arti Frasa 'On Fleek'?", "id": "Sangat Pas, Sempurna (Alis, Riasan)." },
  { "en": "Arti Kata 'FOMO'?", "id": "Takut Ketinggalan." },
  { "en": "Kepanjangan 'FOMO'?", "id": "Fear Of Missing Out." },
  { "en": "Arti Kata 'JOMO'?", "id": "Senang Ketinggalan." },
  { "en": "Kepanjangan 'JOMO'?", "id": "Joy Of Missing Out." },
  { "en": "Arti Kata 'Snatched'?", "id": "Terlihat Sangat Bagus (Fashion)." },
  { "en": "Arti Kata 'Periodt'?", "id": "Titik, Tidak Ada Bantahan." },
  { "en": "Arti Frasa 'It's A Look'?", "id": "Gaya Yang Unik Dan Keren." },
  { "en": "Arti Kata 'Wig'?", "id": "Sesuatu Yang Sangat Mengejutkan." },
  { "en": "Arti Kata 'Beat'?", "id": "Riasan Wajah Yang Sempurna." },
  { "en": "Arti Kata 'Cheesy'?", "id": "Norak, Murahan." },
  { "en": "Arti Kata 'Corny'?", "id": "Garing, Tidak Lucu." },
  { "en": "Arti Kata 'Rizz'?", "id": "Daya Tarik Atau Pesona." },
  { "en": "Arti Kata 'IYKYK'?", "id": "Hanya Orang Tertentu Yang Paham." },
  { "en": "Kepanjangan 'IYKYK'?", "id": "If You Know, You Know." },
  { "en": "Arti Kata 'Noob'?", "id": "Orang Baru Atau Tidak Berpengalaman." },
  { "en": "Arti Kata 'TBH'?", "id": "Sejujurnya." },
  { "en": "Kepanjangan 'TBH'?", "id": "To Be Honest." },
  { "en": "Arti Kata 'Ick'?", "id": "Sesuatu Yang Bikin Ilfeel." },
  { "en": "Arti Kata 'Slayed'?", "id": "Telah Berhasil Dengan Sangat Baik." },
  { "en": "Arti Kata 'Bussin''?", "id": "Sangat Enak (Untuk Makanan)." },
  { "en": "Arti Kata 'Mid'?", "id": "Biasa Saja, Tidak Istimewa." },
  { "en": "Arti Kata 'Gassed'?", "id": "Terlalu Memuji Atau Membangggakan Diri." },
  { "en": "Arti Kata 'Gaslighting'?", "id": "Memanipulasi Seseorang Hingga Meragukan Dirinya." },
  { "en": "Arti Kata 'Gatekeeping'?", "id": "Membatasi Akses Ke Sesuatu." },
  { "en": "Arti Kata 'Thicc'?", "id": "Memiliki Tubuh Berisi Yang Menarik." },
  { "en": "Arti Kata 'Cuffing Season'?", "id": "Musim Mencari Pacar (Musim Dingin)." },
  { "en": "Arti Kata 'Ship'?", "id": "Mendukung Hubungan Dua Orang." },
  { "en": "Arti Kata 'GOATED'?", "id": "Menjadi Yang Terbaik Sepanjang Masa." },
  { "en": "Arti Kata 'Sus'?", "id": "Mencurigakan." },
  { "en": "Arti Kata 'Karen'?", "id": "Stereotip Wanita Pengadu." },
  { "en": "Arti Kata 'Chad'?", "id": "Stereotip Pria Populer Dan Atletis." },
  { "en": "Arti Frasa 'Clap Back'?", "id": "Memberi Balasan Yang Cerdas Dan Pedas." },
  { "en": "Arti Kata 'Dank'?", "id": "Sangat Bagus (Biasanya Untuk Meme)." },
  { "en": "Arti Kata 'Drip'?", "id": "Gaya Berpakaian Yang Sangat Keren." },
  { "en": "Arti Kata 'E-boy' / 'E-girl'?", "id": "Gaya Emo/Goth Di Internet." },
  { "en": "Arti Kata 'Finsta'?", "id": "Akun Instagram Kedua Yang Privat." },
  { "en": "Arti Frasa 'Main Character'?", "id": "Orang Yang Merasa Jadi Pusat Perhatian." },
  { "en": "Arti Frasa 'It's Giving'?", "id": "Memberikan Kesan Atau Aura Tertentu." },
  { "en": "Arti Kata 'OOF'?", "id": "Ekspresi Simpati Untuk Situasi Canggung." },
  { "en": "Arti Kata 'Based'?", "id": "Berani Menjadi Diri Sendiri." },
  { "en": "Arti Kata 'Cope'?", "id": "Mencoba Mengatasi Kesulitan (Sering Sarkastik)." },
  { "en": "Arti Kata 'L'?", "id": "Kekalahan (Loss)." },
  { "en": "Arti Frasa 'Take The L'?", "id": "Menerima Kekalahan." },
  { "en": "Arti Kata 'W'?", "id": "Kemenangan (Win)." },
  { "en": "Arti Frasa 'Take The W'?", "id": "Meraih Kemenangan." },
  { "en": "Arti Kata 'NPC'?", "id": "Orang Yang Bertindak Tanpa Berpikir." },
  { "en": "Kepanjangan 'NPC'?", "id": "Non-Player Character." },
  { "en": "Arti Kata 'OG'?", "id": "Asli, Original, Senior." },
  { "en": "Kepanjangan 'OG'?", "id": "Original Gangster." },
  { "en": "Arti Kata 'Peeps'?", "id": "Orang-Orang, Teman-Teman." },
  { "en": "Arti Kata 'Props'?", "id": "Penghargaan Atau Pengakuan." },
  { "en": "Arti Frasa 'On Point'?", "id": "Sangat Tepat, Sempurna." },
  { "en": "Arti Kata 'Beef'?", "id": "Permasalahan Atau Konflik." },
  { "en": "Arti Frasa 'Let's Roll'?", "id": "Ayo Pergi, Ayo Mulai." },
  { "en": "Arti Kata 'Bail'?", "id": "Pergi Meninggalkan Suatu Tempat." },
  { "en": "Arti Kata 'Photobomb'?", "id": "Tiba-Tiba Masuk Ke Dalam Foto Orang." },
  { "en": "Arti Kata 'Amped'?", "id": "Sangat Bersemangat, Tidak Sabar." },
  { "en": "Arti Kata 'Zonked'?", "id": "Sangat Lelah, Tertidur Pulas." },
  { "en": "Arti Kata 'Tight'?", "id": "Keren, Atau Hubungan Yang Erat." },
  { "en": "Arti Kata 'Airhead'?", "id": "Orang Yang Bodoh Atau Pelupa." },
  { "en": "Arti Frasa 'Bite Me'?", "id": "Ekspresi Kesal, 'Terserah'." },
  { "en": "Arti Kata 'Flake'?", "id": "Orang Yang Sering Batalin Janji." },
  { "en": "Arti Kata 'Jam'?", "id": "Lagu Favorit." },
  { "en": "Arti Kata 'Kicks'?", "id": "Sepatu (Biasanya Sneakers)." },
  { "en": "Arti Kata 'Munchies'?", "id": "Rasa Lapar Setelah Mabuk." },
  { "en": "Arti Frasa 'Sweet Sixteen'?", "id": "Pesta Ulang Tahun Ke-16." },
  { "en": "Arti Frasa 'Get Hitched'?", "id": "Menikah." },
  { "en": "Arti Frasa 'Pass The Buck'?", "id": "Melempar Tanggung Jawab." },
  { "en": "Arti Frasa 'Spill The Beans'?", "id": "Membocorkan Rahasia." },
  { "en": "Arti Frasa 'Piece Of Cake'?", "id": "Sesuatu Yang Sangat Mudah." },
  { "en": "Arti Frasa 'Hit The Road'?", "id": "Mulai Perjalanan, Pergi." },
  { "en": "Arti Frasa 'Break A Leg'?", "id": "Semoga Berhasil (Di Panggung)." },
  { "en": "Arti Frasa 'Once In A Blue Moon'?", "id": "Sesuatu Yang Sangat Jarang Terjadi." },
  { "en": "Arti Kata 'Wasted'?", "id": "Sangat Mabuk." },
  { "en": "Arti Kata 'BS'?", "id": "Omong Kosong." },
  { "en": "Kepanjangan 'BS'?", "id": "Bullshit." },
  { "en": "Arti Frasa 'Rock The Boat'?", "id": "Membuat Masalah." },
  { "en": "Arti Frasa 'Third Wheel'?", "id": "Orang Ketiga Dalam Hubungan (Nyamuk)." },
  { "en": "Arti Frasa 'Spaced Out'?", "id": "Melamun, Tidak Fokus." },
  { "en": "Arti Frasa 'Zone Out'?", "id": "Kehilangan Fokus." },
  { "en": "Arti Frasa 'Sleep On It'?", "id": "Memikirkannya Semalaman." },
  { "en": "Arti Frasa 'All-Nighter'?", "id": "Begadang Semalaman (Belajar/Bekerja)." },
  { "en": "Arti Frasa 'Go Bananas'?", "id": "Menjadi Sangat Marah Atau Gila." },
  { "en": "Arti Frasa 'What's Cooking'?", "id": "Apa Yang Sedang Terjadi?" },
  { "en": "Arti Frasa 'Give A Ring'?", "id": "Menelepon Seseorang." },
  { "en": "Arti Frasa 'Hot Potato'?", "id": "Isu Yang Kontroversial." },
  { "en": "Arti Kata 'Moola'?", "id": "Uang." },
  { "en": "Arti Kata 'Bread'?", "id": "Uang (Konteks Lain)." },
  { "en": "Arti Kata 'Buck'?", "id": "Satu Dolar." },
  { "en": "Arti Kata 'Grand'?", "id": "Seribu Dolar." },
  { "en": "Arti Kata 'Benjamins'?", "id": "Uang Seratus Dolar." },
  { "en": "Arti Kata 'Bling'?", "id": "Perhiasan Yang Berkilauan." },
  { "en": "Arti Kata 'Swerve'?", "id": "Menghindar." },
  { "en": "Arti Kata 'Trill'?", "id": "Benar Dan Nyata." },
  { "en": "Arti Kata 'Totes'?", "id": "Singkatan Dari 'Totally' (Banget)." },
  { "en": "Arti Kata 'Whack'?", "id": "Sesuatu Yang Jelek Atau Aneh." },
  { "en": "Arti Kata 'Hundo P'?", "id": "Seratus Persen Setuju." },
  { "en": "Arti Kata 'Dough'?", "id": "Uang." },
  { "en": "Arti Kata 'Hyped'?", "id": "Sangat Bersemangat." },
  { "en": "Arti Frasa 'Keep It Real'?", "id": "Jadilah Diri Sendiri." },
  { "en": "Arti Frasa 'What's The Catch'?", "id": "Apa Udang Di Balik Batu?" },
  { "en": "Arti Frasa 'Get Lost'?", "id": "Pergi Sana! (Kasar)." },
  { "en": "Arti Kata 'Phony'?", "id": "Palsu, Tidak Tulus." },
  { "en": "Arti Kata 'Poppin''?", "id": "Populer, Sedang Tren." },
  { "en": "Arti Kata 'Juice'?", "id": "Respek, Kekuasaan." },
  { "en": "Arti Kata 'Baddie'?", "id": "Wanita Yang Sangat Menarik Dan Percaya Diri." },
  { "en": "Arti Kata 'Cheugy'?", "id": "Ketinggalan Zaman, Tidak Keren Lagi." },
  { "en": "Arti Kata 'Drip'?", "id": "Gaya Berpakaian Yang Sangat Keren." },
  { "en": "Arti Kata 'G.O.A.T'?", "id": "Terbaik Sepanjang Masa." },
  { "en": "Kepanjangan 'G.O.A.T'?", "id": "Greatest Of All Time." },
  { "en": "Arti Kata 'Iykyk'?", "id": "Jika Kamu Tahu, Kamu Tahu." },
  { "en": "Kepanjangan 'Iykyk'?", "id": "If You Know, You Know." },
  { "en": "Arti Kata 'Szn'?", "id": "Singkatan Dari 'Season' (Musim)." },
  { "en": "Arti Kata 'Vibing'?", "id": "Menikmati Suasana Atau Musik." },
  { "en": "Arti Kata 'Whew'?", "id": "Ekspresi Lega Atau Terkesan." },
  { "en": "Arti Kata 'Zaddy'?", "id": "Pria Tampan Dan Modis." },
  { "en": "Arti Frasa 'Let Him Cook'?", "id": "Biar Dia Lakukan Urusannya." },
  { "en": "Arti Kata 'Delulu'?", "id": "Singkatan Dari 'Delusional' (Delusi)." },
  { "en": "Arti Frasa 'Is The Solulu'?", "id": "Adalah Solusinya." },
  { "en": "Arti Kata 'Era'?", "id": "Periode Waktu Tertentu Dalam Hidup." },
  { "en": "Arti Frasa 'In My ... Era'?", "id": "Sedang Dalam Fase Tertentu." },
  { "en": "Arti Frasa 'No Worries'?", "id": "Tidak Masalah, Santai Saja." },
  { "en": "Arti Frasa 'For Real'?", "id": "Seriusan, Sungguhan." },
  { "en": "Arti Frasa 'I'm Down'?", "id": "Aku Mau Ikut." },
  { "en": "Arti Kata 'Homie'?", "id": "Teman Dekat Dari Lingkungan Sama." },
  { "en": "Arti Kata 'Broke'?", "id": "Tidak Punya Uang, Bokek." },
  { "en": "Arti Kata 'Loaded'?", "id": "Sangat Kaya." },
  { "en": "Arti Kata 'Wheels'?", "id": "Mobil." },
  { "en": "Arti Frasa 'Chill Out'?", "id": "Tenang, Santai." },
  { "en": "Arti Frasa 'Freak Out'?", "id": "Panik Atau Menjadi Sangat Emosional." },
  { "en": "Arti Kata 'Sucks'?", "id": "Menyebalkan, Jelek." },
  { "en": "Arti Kata 'Sweet'?", "id": "Keren, Bagus." },
  { "en": "Arti Kata 'Dude'?", "id": "Panggilan Akrab (Bung, Sob)." },
  { "en": "Arti Frasa 'Give Me A Break'?", "id": "Sudah Cukup, Jangan Ganggu." },
  { "en": "Arti Frasa 'I Get It'?", "id": "Aku Paham." },
  { "en": "Arti Frasa 'Not A Big Deal'?", "id": "Bukan Masalah Besar." },
  { "en": "Arti Frasa 'Hang On'?", "id": "Tunggu Sebentar." },
  { "en": "Arti Frasa 'Shut Up'?", "id": "Diam (Kasar)." },
  { "en": "Arti Kata 'Whatever'?", "id": "Terserah." },
  { "en": "Arti Kata 'Loser'?", "id": "Pecundang." },
  { "en": "Arti Kata 'Winner'?", "id": "Pemenang." },
  { "en": "Arti Kata 'Hot'?", "id": "Sangat Menarik Atau Seksi." },
  { "en": "Arti Frasa 'Hit Me Up'?", "id": "Hubungi Aku Nanti." },
  { "en": "Arti Frasa 'It's On Me'?", "id": "Aku Yang Traktir." },
  { "en": "Arti Frasa 'Rip-Off'?", "id": "Harga Yang Terlalu Mahal." },
  { "en": "Arti Frasa 'Pass Out'?", "id": "Pingsan Atau Tertidur Pulas." },
  { "en": "Arti Kata 'Pissed Off'?", "id": "Sangat Marah." },
  { "en": "Arti Kata 'Ace'?", "id": "Sangat Bagus Atau Berhasil." },
  { "en": "Arti Kata 'All Ears'?", "id": "Mendengarkan Dengan Seksama." },
  { "en": "Arti Frasa 'Go Dutch'?", "id": "Bayar Masing-Masing." },
  { "en": "Arti Frasa 'John Hancock'?", "id": "Tanda Tangan." },
  { "en": "Arti Frasa 'Lighten Up'?", "id": "Jangan Terlalu Serius." },
  { "en": "Arti Frasa 'No Sweat'?", "id": "Tidak Masalah, Gampang." },
  { "en": "Arti Frasa 'Shoot The Breeze'?", "id": "Mengobrol Santai Tanpa Tujuan." },
  { "en": "Arti Frasa 'Spill It'?", "id": "Katakan Saja." },
  { "en": "Arti Frasa 'You Bet'?", "id": "Tentu Saja, Pasti." },
  { "en": "Arti Frasa 'Sleep On It'?", "id": "Memikirkannya Semalaman." },
  { "en": "Arti Frasa 'Twenty-Four Seven (24/7)'?", "id": "Setiap Saat, Sepanjang Waktu." },
  { "en": "Arti Kata 'Bae'?", "id": "Singkatan Dari 'Before Anyone Else'." },
  { "en": "Arti Kata 'Flex'?", "id": "Pamer." },
  { "en": "Arti Kata 'Salty'?", "id": "Iri Atau Dengki." },
  { "en": "Arti Kata 'Shook'?", "id": "Terkejut." },
  { "en": "Arti Kata 'Tea'?", "id": "Gossip." },
  { "en": "Arti Kata 'Woke'?", "id": "Sadar Isu Sosial." },
  { "en": "Arti Kata 'Cringe'?", "id": "Merasa Geli." },
  { "en": "Arti Frasa 'No Cap'?", "id": "Tidak Bohong." },
  { "en": "Arti Kata 'Simp'?", "id": "Terobsesi Pada Seseorang." },
  { "en": "Arti Kata 'Drip'?", "id": "Gaya Yang Keren." },
  { "en": "Arti Kata 'Fam'?", "id": "Keluarga Atau Teman Dekat." },
  { "en": "Arti Kata 'Fire'?", "id": "Sesuatu Yang Luar Biasa." },
  { "en": "Arti Kata 'Clout'?", "id": "Popularitas." },
  { "en": "Arti Kata 'Ghosting'?", "id": "Menghilang Tanpa Kabar." },
  { "en": "Arti Kata 'Lowkey'?", "id": "Diam-Diam." },
  { "en": "Arti Kata 'Highkey'?", "id": "Terang-Terangan." },
  { "en": "Arti Kata 'Bet'?", "id": "Oke." },
  { "en": "Arti Kata 'Dope'?", "id": "Keren." },
  { "en": "Arti Kata 'Mood'?", "id": "Sesuai Perasaan." },
  { "en": "Arti Kata 'Vibe'?", "id": "Aura." },
  { "en": "Arti Kata 'Finna'?", "id": "Akan." },
  { "en": "Arti Kata 'Yeet'?", "id": "Melempar." },
  { "en": "Arti Kata 'Basic'?", "id": "Mainstream." },
  { "en": "Arti Kata 'Extra'?", "id": "Berlebihan." },
  { "en": "Arti Kata 'Savage'?", "id": "Keren Dan Pedas." },
  { "en": "Arti Frasa 'Glow Up'?", "id": "Transformasi Menjadi Lebih Baik." },
  { "en": "Arti Frasa 'On Fleek'?", "id": "Sempurna." },
  { "en": "Arti Kata 'FOMO'?", "id": "Takut Ketinggalan." },
  { "en": "Arti Kata 'JOMO'?", "id": "Senang Ketinggalan." },
  { "en": "Arti Kata 'Snack'?", "id": "Seseorang Yang Menarik." },
  { "en": "Arti Kata 'Thirsty'?", "id": "Mencari Perhatian." },
  { "en": "Arti Kata 'Stan'?", "id": "Penggemar Berat." },
  { "en": "Arti Kata 'Wig'?", "id": "Terkejut." },
  { "en": "Arti Kata 'Periodt'?", "id": "Titik (Menegaskan)." },
  { "en": "Arti Kata 'Snatched'?", "id": "Sangat Modis." },
  { "en": "Arti Frasa 'It's A Look'?", "id": "Gaya Unik." },
  { "en": "Arti Frasa 'Big Yikes'?", "id": "Sangat Canggung." },
  { "en": "Arti Kata 'Boujee'?", "id": "Mewah." },
  { "en": "Arti Kata 'Bop'?", "id": "Lagu Bagus." },
  { "en": "Arti Kata 'Whip'?", "id": "Mobil." },
  { "en": "Arti Kata 'G.G.'?", "id": "Permainan Yang Bagus (Game Over)." },
  { "en": "Kepanjangan 'G.G.'?", "id": "Good Game." },
  { "en": "Arti Kata 'AFK'?", "id": "Jauh Dari Keyboard." },
  { "en": "Kepanjangan 'AFK'?", "id": "Away From Keyboard." },
  { "en": "Arti Kata 'IRL'?", "id": "Di Dunia Nyata." },
  { "en": "Kepanjangan 'IRL'?", "id": "In Real Life." },
  { "en": "Arti Kata 'TFW'?", "id": "Perasaan Saat..." },
  { "en": "Kepanjangan 'TFW'?", "id": "That Feeling When." },
  { "en": "Arti Kata 'LMAO'?", "id": "Tertawa Terbahak-Bahak." },
  { "en": "Kepanjangan 'LMAO'?", "id": "Laughing My Ass Off." },
  { "en": "Arti Kata 'ROFL'?", "id": "Tertawa Sampai Berguling-Guling." },
  { "en": "Kepanjangan 'ROFL'?", "id": "Rolling On The Floor Laughing." },
  { "en": "Arti Kata 'BRB'?", "id": "Segera Kembali." },
  { "en": "Kepanjangan 'BRB'?", "id": "Be Right Back." },
  { "en": "Arti Kata 'ASAP'?", "id": "Secepat Mungkin." },
  { "en": "Kepanjangan 'ASAP'?", "id": "As Soon As Possible." },
  { "en": "Arti Kata 'Hangry'?", "id": "Marah Karena Lapar." },
  { "en": "Arti Kata 'Mansplain'?", "id": "Pria Menjelaskan Sesuatu Dengan Merendahkan." },
  { "en": "Arti Kata 'Binge-Watch'?", "id": "Menonton Serial TV Maraton." },
  { "en": "Arti Kata 'Photobomb'?", "id": "Tiba-Tiba Masuk Ke Dalam Foto Orang." },
  { "en": "Arti Kata 'Clickbait'?", "id": "Judul Yang Memancing Untuk Diklik." },
  { "en": "Arti Frasa 'Down To Earth'?", "id": "Orang Yang Ramah Dan Membumi." },
  { "en": "Arti Frasa 'It Is What It Is'?", "id": "Ya Begitulah Adanya." },
  { "en": "Arti Frasa 'No Biggie'?", "id": "Bukan Masalah Besar." },
  { "en": "Arti Kata 'P.O.V.'?", "id": "Sudut Pandang." },
  { "en": "Kepanjangan 'P.O.V.'?", "id": "Point Of View." },
  { "en": "Arti Frasa 'Long Story Short'?", "id": "Singkatnya..." },
  { "en": "Arti Kata 'TMI'?", "id": "Informasi Yang Terlalu Pribadi." },
  { "en": "Kepanjangan 'TMI'?", "id": "Too Much Information." },
  { "en": "Arti Kata 'R.I.P.'?", "id": "Beristirahat Dalam Damai." },
  { "en": "Kepanjangan 'R.I.P.'?", "id": "Rest In Peace." },
  { "en": "Arti Frasa 'Bless Your Heart'?", "id": "Kasihan Sekali (Bisa Tulus/Menyindir)." },
  { "en": "Arti Frasa 'Bite The Bullet'?", "id": "Menghadapi Sesuatu Yang Sulit." },
  { "en": "Arti Frasa 'Break The Ice'?", "id": "Mencairkan Suasana." },
  { "en": "Arti Frasa 'Call It A Day'?", "id": "Menyelesaikan Pekerjaan Hari Ini." },
  { "en": "Arti Frasa 'Cut To The Chase'?", "id": "Langsung Ke Intinya." },
  { "en": "Arti Frasa 'Get Over It'?", "id": "Lupakan Saja, Move On." },
  { "en": "Arti Frasa 'Go With The Flow'?", "id": "Ikuti Saja Arusnya." },
  { "en": "Arti Frasa 'Hold Your Horses'?", "id": "Sabar Dulu, Jangan Terburu-Buru." },
  { "en": "Arti Frasa 'In A Nutshell'?", "id": "Secara Singkat." },
  { "en": "Arti Frasa 'Killin' It'?", "id": "Melakukan Sesuatu Dengan Sangat Baik." },
  { "en": "Arti Frasa 'Off The Hook'?", "id": "Sangat Keren Atau Lolos Dari Masalah." },
  { "en": "Arti Frasa 'On The House'?", "id": "Gratis Dari Restoran/Bar." },
  { "en": "Arti Frasa 'Take A Rain Check'?", "id": "Menunda Janji Ke Lain Waktu." },
  { "en": "Arti Frasa 'The Bomb'?", "id": "Sesuatu Yang Sangat Keren." },
  { "en": "Arti Frasa 'Under The Weather'?", "id": "Sedang Merasa Kurang Sehat." },
  { "en": "Arti Kata 'Jinx'?", "id": "Sial, Pembawa Sial." },
  { "en": "Arti Kata 'M.I.A.'?", "id": "Hilang Tanpa Jejak." },
  { "en": "Kepanjangan 'M.I.A.'?", "id": "Missing In Action." },
  { "en": "Arti Kata 'Payback'?", "id": "Balas Dendam." },
  { "en": "Arti Kata 'Score'?", "id": "Berhasil Mendapatkan Sesuatu." },
  { "en": "Arti Frasa 'That's The Spirit'?", "id": "Nah, Gitu Dong (Pujian Semangat)." },
  { "en": "Arti Frasa 'You Rock'?", "id": "Kamu Keren Banget." },
  { "en": "Arti Frasa 'Good Call'?", "id": "Keputusan Yang Bagus." },
  { "en": "Arti Frasa 'You Wish'?", "id": "Mimpi Saja (Sarkastik)." },
  { "en": "Arti Kata 'Wimp'?", "id": "Pengecut, Cemen." },
  { "en": "Arti Kata 'Wuss'?", "id": "Sinonim Dari 'Wimp'." },
  { "en": "Arti Frasa 'Veg Out'?", "id": "Bersantai Tanpa Melakukan Apa-Apa." },
  { "en": "Arti Kata 'Sleazy'?", "id": "Murahan, Tidak Bermoral." },
  { "en": "Arti Kata 'Shady'?", "id": "Mencurigakan, Tidak Bisa Dipercaya." },
  { "en": "Arti Frasa 'Get A Life'?", "id": "Urus Saja Hidupmu Sendiri." },
  { "en": "Arti Kata 'Guts'?", "id": "Keberanian." },
  { "en": "Arti Frasa 'Spit It Out'?", "id": "Cepat Katakan Saja." },
  { "en": "Arti Kata 'Jacked'?", "id": "Sangat Berotot." },
  { "en": "Arti Frasa 'Head Over Heels'?", "id": "Jatuh Cinta Berat." },
  { "en": "Arti Frasa 'Cold Feet'?", "id": "Gugup Sebelum Melakukan Hal Besar." },
  { "en": "Arti Frasa 'Give The Cold Shoulder'?", "id": "Mengabaikan Atau Mendiamkan Seseorang." },
  { "en": "Arti Kata 'Downer'?", "id": "Sesuatu Yang Bikin Sedih." },
  { "en": "Arti Kata 'Upbeat'?", "id": "Ceria, Optimis." },
  { "en": "Arti Frasa 'Drive Someone Up The Wall'?", "id": "Membuat Seseorang Sangat Kesal." },
  { "en": "Arti Frasa 'Blow Off Steam'?", "id": "Melepas Penat Atau Amarah." },
  { "en": "Arti Frasa 'By The Skin of My Teeth'?", "id": "Hampir Saja, Nyaris." },
  { "en": "Arti Frasa 'Cat Got Your Tongue'?", "id": "Kenapa Kamu Diam Saja?" },
  { "en": "Arti Kata 'Toasted'?", "id": "Sangat Mabuk." },
  { "en": "Arti Frasa 'Face The Music'?", "id": "Menghadapi Konsekuensi." },
  { "en": "Arti Frasa 'It's A Wrap'?", "id": "Selesai Sudah." },
  { "en": "Arti Frasa 'Hit The Sack'?", "id": "Pergi Tidur." },
  { "en": "Arti Frasa 'Twist My Arm'?", "id": "Meyakinkan Seseorang Melakukan Sesuatu." },
  { "en": "Arti Frasa 'Lose Your Touch'?", "id": "Kehilangan Kemampuan Atau Bakat." },
  { "en": "Arti Frasa 'Sit Tight'?", "id": "Tunggu Dengan Sabar." },
  { "en": "Arti Frasa 'Go Cold Turkey'?", "id": "Berhenti Total Dari Kebiasaan Buruk." },
  { "en": "Arti Frasa 'Ring A Bell'?", "id": "Terdengar Familiar." },
  { "en": "Arti Frasa 'Cut Corners'?", "id": "Mengambil Jalan Pintas (Kualitas Buruk)." },
  { "en": "Arti Frasa 'Get Out Of Hand'?", "id": "Menjadi Tidak Terkendali." },
  { "en": "Arti Frasa 'Hang In There'?", "id": "Bertahanlah, Jangan Menyerah." },
  { "en": "Arti Frasa 'Pull Yourself Together'?", "id": "Tenangkan Dirimu." },
  { "en": "Arti Frasa 'So Far So Good'?", "id": "Sejauh Ini Baik-Baik Saja." },
  { "en": "Arti Frasa 'A Blessing In Disguise'?", "id": "Hikmah Di Balik Musibah." },
  { "en": "Arti Frasa 'Comparing Apples To Oranges'?", "id": "Membandingkan Dua Hal Yang Berbeda." },
  { "en": "Arti Frasa 'The Elephant In The Room'?", "id": "Masalah Besar Yang Dihindari." },
  { "en": "Arti Frasa 'A Dime A Dozen'?", "id": "Sesuatu Yang Sangat Umum." },
  { "en": "Arti Frasa 'Beat Around The Bush'?", "id": "Bertele-Tele." },
  { "en": "Arti Frasa 'The Best Of Both Worlds'?", "id": "Mendapat Keuntungan Dari Dua Sisi." },
  { "en": "Arti Frasa 'Bite Your Tongue'?", "id": "Menahan Diri Untuk Tidak Berbicara." },
  { "en": "Arti Frasa 'Get The Ball Rolling'?", "id": "Memulai Sesuatu." },
  { "en": "Arti Frasa 'Hit The Books'?", "id": "Mulai Belajar Dengan Serius." },
  { "en": "Arti Frasa 'It's Not Rocket Science'?", "id": "Ini Bukan Hal Yang Sulit." },
  { "en": "Arti Frasa 'Miss The Boat'?", "id": "Kehilangan Kesempatan." },
  { "en": "Arti Frasa 'Pull Someone's Leg'?", "id": "Bercanda Atau Mengerjai Seseorang." },
  { "en": "Arti Frasa 'Speak Of The Devil'?", "id": "Panjang Umur (Orangnya Muncul)." },
  { "en": "Arti Frasa 'The Last Straw'?", "id": "Batas Kesabaran Terakhir." },
  { "en": "Arti Frasa 'Your Guess Is As Good As Mine'?", "id": "Aku Juga Tidak Tahu." },
  { "en": "Arti Kata 'A-Game'?", "id": "Performa Terbaik Seseorang." },
  { "en": "Arti Frasa 'Bring Your A-Game'?", "id": "Berikan Performa Terbaikmu." },
  { "en": "Arti Kata 'Bae'?", "id": "Sayang (Before Anyone Else)." },
  { "en": "Arti Kata 'Beat'?", "id": "Sangat Lelah." },
  { "en": "Arti Frasa 'Binge On'?", "id": "Melakukan Sesuatu Berlebihan (Makan/Nonton)." },
  { "en": "Arti Frasa 'Blow Me Away'?", "id": "Membuatku Sangat Terkesan." },
  { "en": "Arti Kata 'Bogus'?", "id": "Palsu Atau Tidak Benar." },
  { "en": "Arti Kata 'Bossy'?", "id": "Suka Menyuruh-Nyuruh." },
  { "en": "Arti Kata 'Cheesy'?", "id": "Norak Atau Terlalu Melodramatis." },
  { "en": "Arti Kata 'Cram'?", "id": "Belajar Kebut Semalam." },
  { "en": "Arti Kata 'Diss'?", "id": "Tidak Menghormati Atau Menghina." },
  { "en": "Arti Kata 'Ditch'?", "id": "Meninggalkan Sesuatu Atau Seseorang." },
  { "en": "Arti Kata 'Flirty'?", "id": "Suka Menggoda Atau Merayu." },
  { "en": "Arti Kata 'Frenemy'?", "id": "Teman Yang Juga Musuh." },
  { "en": "Arti Kata 'Goofy'?", "id": "Konyol Atau Lucu." },
  { "en": "Arti Frasa 'Pass The Buck'?", "id": "Melempar Tanggung Jawab." },
  { "en": "Arti Frasa 'Have A Blast'?", "id": "Bersenang-Senang." },
  { "en": "Arti Frasa 'Have Mixed Feelings'?", "id": "Merasa Senang Sekaligus Sedih." },
  { "en": "Arti Frasa 'No Brainer'?", "id": "Sesuatu Yang Sangat Jelas." },
  { "en": "Arti Frasa 'Read Between The Lines'?", "id": "Memahami Makna Tersirat." },
  { "en": "Arti Kata 'Rip-Off'?", "id": "Penipuan, Harga Terlalu Mahal." },
  { "en": "Arti Kata 'Tacky'?", "id": "Murahan Atau Norak." },
  { "en": "Arti Kata 'Trashy'?", "id": "Berkualitas Sangat Buruk." },
  { "en": "Arti Frasa 'Give The Green Light'?", "id": "Memberikan Izin Untuk Memulai." },
  { "en": "Arti Frasa 'Gray Area'?", "id": "Sesuatu Yang Tidak Jelas Benar/Salahnya." },
  { "en": "Arti Frasa 'Red Tape'?", "id": "Birokrasi Yang Rumit." },
  { "en": "Arti Frasa 'See Eye To Eye'?", "id": "Setuju Sepenuhnya Dengan Seseorang." },
  { "en": "Arti Frasa 'Sleep On It'?", "id": "Menunda Keputusan Sampai Besok." },
  { "en": "Arti Frasa 'Take It With A Pinch Of Salt'?", "id": "Jangan Langsung Dipercaya." },
  { "en": "Arti Frasa 'The Ball Is In Your Court'?", "id": "Sekarang Giliranmu Mengambil Keputusan." },
  { "en": "Arti Kata 'Dunno'?", "id": "Singkatan Dari 'Don't Know'." },
  { "en": "Arti Kata 'Gonna'?", "id": "Singkatan Dari 'Going To'." },
  { "en": "Arti Kata 'Wanna'?", "id": "Singkatan Dari 'Want To'." },
  { "en": "Arti Kata 'Gimme'?", "id": "Singkatan Dari 'Give Me'." },
  { "en": "Arti Kata 'Lemme'?", "id": "Singkatan Dari 'Let Me'." },
  { "en": "Arti Kata 'IDK'?", "id": "Aku Tidak Tahu." },
  { "en": "Kepanjangan 'IDK'?", "id": "I Don't Know." },
  { "en": "Arti Kata 'LOL'?", "id": "Tertawa Terbahak-Bahak." },
  { "en": "Kepanjangan 'LOL'?", "id": "Laughing Out Loud." },
  { "en": "Arti Kata 'OMG'?", "id": "Ya Tuhan." },
  { "en": "Kepanjangan 'OMG'?", "id": "Oh My God." },
  { "en": "Arti Kata 'NVM'?", "id": "Lupakan Saja." },
  { "en": "Kepanjangan 'NVM'?", "id": "Nevermind." },
  { "en": "Arti Kata 'TGIF'?", "id": "Syukurlah Hari Jumat." },
  { "en": "Kepanjangan 'TGIF'?", "id": "Thank God It's Friday." },
  { "en": "Arti Frasa 'For Dummies'?", "id": "Panduan Untuk Pemula." },
  { "en": "Arti Kata 'Jackass'?", "id": "Orang Bodoh Atau Idiot." },
  { "en": "Arti Kata 'Knucklehead'?", "id": "Orang Bodoh." },
  { "en": "Arti Kata 'Mellow'?", "id": "Tenang Dan Santai." },
  { "en": "Arti Kata 'Psyched'?", "id": "Sangat Bersemangat." },
  { "en": "Arti Kata 'Sucker'?", "id": "Orang Yang Mudah Ditipu." },
  { "en": "Arti Kata 'Uptight'?", "id": "Terlalu Kaku Atau Tegang." },
  { "en": "Arti Kata 'Deadbeat'?", "id": "Orang Yang Tidak Bayar Utang." },
  { "en": "Arti Frasa 'Bite The Dust'?", "id": "Mati Atau Gagal Total." },
  { "en": "Arti Frasa 'Call The Shots'?", "id": "Memegang Kendali, Membuat Keputusan." },
  { "en": "Arti Frasa 'Get Your Act Together'?", "id": "Perbaiki Sikapmu." },
  { "en": "Arti Frasa 'Give Someone The Slip'?", "id": "Lolos Dari Seseorang." },
  { "en": "Arti Frasa 'Have A Ball'?", "id": "Sangat Bersenang-Senang." },
  { "en": "Arti Frasa 'In The Nick Of Time'?", "id": "Tepat Pada Waktunya." },
  { "en": "Arti Frasa 'Keep Your Chin Up'?", "id": "Tetap Semangat." },
  { "en": "Arti Frasa 'Let The Cat Out Of The Bag'?", "id": "Membocorkan Rahasia." },
  { "en": "Arti Frasa 'Play It By Ear'?", "id": "Lihat Nanti Saja (Improvisasi)." },
  { "en": "Arti Frasa 'Rain On Someone's Parade'?", "id": "Merusak Momen Bahagia Seseorang." },
  { "en": "Arti Frasa 'Throw In The Towel'?", "id": "Menyerah." },
  { "en": "Arti Frasa 'You Can Say That Again'?", "id": "Aku Sangat Setuju." },
  { "en": "Arti Kata 'Zilch'?", "id": "Nol, Tidak Ada Sama Sekali." },
  { "en": "Arti Kata 'Zit'?", "id": "Jerawat." },
  { "en": "Arti Kata 'Flick'?", "id": "Film." },
  { "en": "Arti Frasa 'Old School'?", "id": "Gaya Lama, Jadul." },
  { "en": "Arti Kata 'Party pooper'?", "id": "Orang Yang Merusak Suasana Pesta." },
  { "en": "Arti Frasa 'Rule Of Thumb'?", "id": "Aturan Praktis." },
  { "en": "Arti Frasa 'Sit On The Fence'?", "id": "Ragu-Ragu, Tidak Memihak." },
  { "en": "Arti Frasa 'Spitting Image'?", "id": "Sangat Mirip, Kembaran." },
  { "en": "Arti Frasa 'Through Thick And Thin'?", "id": "Dalam Suka Dan Duka." },
  { "en": "Arti Frasa 'A Whole Nine Yards'?", "id": "Semuanya, Selengkap-Lengkapnya." },
  { "en": "Arti Frasa 'Cost An Arm And A Leg'?", "id": "Sangat Mahal." },
  { "en": "Arti Frasa 'Cross Your Fingers'?", "id": "Berharap Keberuntungan." },
  { "en": "Arti Frasa 'Cry Wolf'?", "id": "Memberi Alarm Palsu." },
  { "en": "Arti Frasa 'Curiosity Killed The Cat'?", "id": "Rasa Ingin Tahu Bisa Berbahaya." },
  { "en": "Arti Frasa 'Don't Judge A Book By Its Cover'?", "id": "Jangan Menilai Dari Penampilan." },
  { "en": "Arti Frasa 'Get Wind Of Something'?", "id": "Mendengar Rumor." },
  { "en": "Arti Frasa 'Hear It On The Grapevine'?", "id": "Mendengar Dari Mulut Ke Mulut." },
  { "en": "Arti Frasa 'Jump On The Bandwagon'?", "id": "Ikut-Ikutan Tren." },
  { "en": "Arti Frasa 'Method To My Madness'?", "id": "Ada Tujuan Dibalik Kegilaan." },
  { "en": "Arti Frasa 'Add Fuel To The Fire'?", "id": "Memperburuk Situasi." },
  { "en": "Arti Frasa 'Go Down In Flames'?", "id": "Gagal Secara Spektakuler." },
  { "en": "Arti Frasa 'Like A Chicken With Its Head Cut Off'?", "id": "Bertindak Panik Tanpa Arah." },
  { "en": "Arti Frasa 'An Arm And A Leg'?", "id": "Sangat Mahal." },
  { "en": "Arti Frasa 'At The Drop Of A Hat'?", "id": "Seketika, Tanpa Ragu-Ragu." },
  { "en": "Arti Frasa 'Back To The Drawing Board'?", "id": "Mulai Lagi Dari Awal." },
  { "en": "Arti Frasa 'Ball Is In Your Court'?", "id": "Giliranmu Mengambil Tindakan." },
  { "en": "Arti Frasa 'Barking Up The Wrong Tree'?", "id": "Salah Sasaran Atau Salah Tuduh." },
  { "en": "Arti Frasa 'Be Glad To See The Back Of'?", "id": "Senang Seseorang Telah Pergi." },
  { "en": "Arti Frasa 'Against The Clock'?", "id": "Berpacu Dengan Waktu." },
  { "en": "Arti Frasa 'Don't Give Up The Day Job'?", "id": "Kamu Tidak Begitu Bagus." },
  { "en": "Arti Frasa 'Get The Sack'?", "id": "Dipecat Dari Pekerjaan." },
  { "en": "Arti Frasa 'Hit The Nail On The Head'?", "id": "Sangat Tepat Sasaran." },
  { "en": "Arti Frasa 'It Takes Two To Tango'?", "id": "Kedua Pihak Sama-Sama Salah." },
  { "en": "Arti Frasa 'Let Sleeping Dogs Lie'?", "id": "Jangan Ungkit Masalah Lama." },
  { "en": "Arti Frasa 'On The Ball'?", "id": "Cepat Tanggap Dan Kompeten." },
  { "en": "Arti Frasa 'Over The Top'?", "id": "Sangat Berlebihan." },
  { "en": "Arti Frasa 'Pulling Your Leg'?", "id": "Bercanda Atau Mengerjaimu." },
  { "en": "Arti Frasa 'Steal Someone's Thunder'?", "id": "Mencuri Perhatian Dari Seseorang." },
  { "en": "Arti Frasa 'Straight From The Horse's Mouth'?", "id": "Langsung Dari Sumber Terpercaya." },
  { "en": "Arti Frasa 'The Whole Nine Yards'?", "id": "Semuanya, Tanpa Terkecuali." },
  { "en": "Arti Frasa 'Wouldn't Be Caught Dead'?", "id": "Tidak Akan Pernah Melakukannya." },
  { "en": "Arti Kata 'IMO'?", "id": "Menurut Pendapatku." },
  { "en": "Kepanjangan 'IMO'?", "id": "In My Opinion." },
  { "en": "Arti Kata 'IMHO'?", "id": "Menurut Pendapatku Yang Rendah Hati." },
  { "en": "Kepanjangan 'IMHO'?", "id": "In My Humble Opinion." },
  { "en": "Arti Kata 'SMH'?", "id": "Menggelengkan Kepala (Kecewa/Heran)." },
  { "en": "Kepanjangan 'SMH'?", "id": "Shaking My Head." },
  { "en": "Arti Kata 'ICYMI'?", "id": "Kalau-Kalau Kamu Ketinggalan." },
  { "en": "Kepanjangan 'ICYMI'?", "id": "In Case You Missed It." },
  { "en": "Arti Kata 'TBF'?", "id": "Sejujurnya (Untuk Bersikap Adil)." },
  { "en": "Kepanjangan 'TBF'?", "id": "To Be Fair." },
  { "en": "Arti Kata 'ELI5'?", "id": "Jelaskan Seakan Aku Lima Tahun." },
  { "en": "Kepanjangan 'ELI5'?", "id": "Explain Like I'm 5." },
  { "en": "Arti Kata 'Grub'?", "id": "Makanan (Slang)." },
  { "en": "Arti Kata 'Knackered'?", "id": "Sangat Lelah (Slang Inggris)." },
  { "en": "Arti Kata 'Gutted'?", "id": "Sangat Kecewa (Slang Inggris)." },
  { "en": "Arti Kata 'Chuffed'?", "id": "Sangat Senang (Slang Inggris)." },
  { "en": "Arti Kata 'Bloke'?", "id": "Pria, Cowok (Slang Inggris)." },
  { "en": "Arti Kata 'Mate'?", "id": "Kawan, Sobat (Slang Inggris/Australia)." },
  { "en": "Arti Kata 'Gobsmacked'?", "id": "Sangat Terkejut (Slang Inggris)." },
  { "en": "Arti Kata 'Cheeky'?", "id": "Kurang Ajar Tapi Lucu." },
  { "en": "Arti Kata 'Dodgy'?", "id": "Mencurigakan Atau Tidak Bisa Diandalkan." },
  { "en": "Arti Kata 'Quid'?", "id": "Satu Poundsterling (Slang Inggris)." },
  { "en": "Arti Kata 'Fiver'?", "id": "Uang Lima Pound (Slang Inggris)." },
  { "en": "Arti Kata 'Tenner'?", "id": "Uang Sepuluh Pound (Slang Inggris)." },
  { "en": "Arti Frasa 'Donkey's Years'?", "id": "Waktu Yang Sangat Lama." },
  { "en": "Arti Frasa 'Bob's Your Uncle'?", "id": "Dan Selesai, Semudah Itu." },
  { "en": "Arti Kata 'Naff'?", "id": "Tidak Keren, Norak (Slang Inggris)." },
  { "en": "Arti Kata 'Posh'?", "id": "Mewah, Berkelas." },
  { "en": "Arti Frasa 'Take The Piss'?", "id": "Mengejek Atau Mengolok-Olok." },
  { "en": "Arti Kata 'Rubbish'?", "id": "Omong Kosong Atau Sampah." },
  { "en": "Arti Kata 'Shambles'?", "id": "Kondisi Yang Sangat Berantakan." },
  { "en": "Arti Kata 'Ace'?", "id": "Keren, Luar Biasa (Slang Inggris)." },
  { "en": "Arti Kata 'Blimey'?", "id": "Ekspresi Terkejut." },
  { "en": "Arti Kata 'Bloody'?", "id": "Kata Seru Penegas (Agak Kasar)." },
  { "en": "Arti Kata 'Cheers'?", "id": "Terima Kasih (Slang Inggris)." },
  { "en": "Arti Kata 'Cuppa'?", "id": "Secangkir Teh." },
  { "en": "Arti Frasa 'Full Of Beans'?", "id": "Penuh Energi." },
  { "en": "Arti Kata 'Gander'?", "id": "Melihat-Lihat." },
  { "en": "Arti Frasa 'Have A Gander'?", "id": "Coba Lihat." },
  { "en": "Arti Frasa 'Go Pear-Shaped'?", "id": "Menjadi Gagal Total." },
  { "en": "Arti Frasa 'Easy Peasy'?", "id": "Sangat Mudah." },
  { "en": "Arti Frasa 'Lost The Plot'?", "id": "Menjadi Tidak Rasional Atau Gila." },
  { "en": "Arti Frasa 'Not My Cup Of Tea'?", "id": "Bukan Seleraku." },
  { "en": "Arti Kata 'Skint'?", "id": "Bokek, Tidak Punya Uang." },
  { "en": "Arti Kata 'Tad'?", "id": "Sedikit." },
  { "en": "Arti Frasa 'A Tad'?", "id": "Sedikit Saja." },
  { "en": "Arti Frasa 'The Bee's Knees'?", "id": "Sesuatu Yang Sangat Bagus." },
  { "en": "Arti Frasa 'Throw A Spanner In The Works'?", "id": "Menggagalkan Sebuah Rencana." },
  { "en": "Arti Kata 'Toff'?", "id": "Orang Kelas Atas (Agak Merendahkan)." },
  { "en": "Arti Kata 'Telly'?", "id": "Televisi." },
  { "en": "Arti Kata 'Barmy'?", "id": "Gila Atau Aneh." },
  { "en": "Arti Kata 'Bonkers'?", "id": "Gila (Lebih Kuat Dari Barmy)." },
  { "en": "Arti Frasa 'All Greek To Me'?", "id": "Aku Tidak Mengerti Sama Sekali." },
  { "en": "Arti Frasa 'An Apple A Day Keeps The Doctor Away'?", "id": "Makan Sehat Mencegah Penyakit." },
  { "en": "Arti Frasa 'Back Seat Driver'?", "id": "Orang Yang Suka Mengomentari." },
  { "en": "Arti Frasa 'Break The Bank'?", "id": "Menghabiskan Seluruh Uang." },
  { "en": "Arti Frasa 'Drive A Hard Bargain'?", "id": "Pandai Menawar." },
  { "en": "Arti Frasa 'Fit As A Fiddle'?", "id": "Sangat Sehat Dan Bugar." },
  { "en": "Arti Frasa 'Get A Second Wind'?", "id": "Mendapat Energi Baru." },
  { "en": "Arti Frasa 'Go For Broke'?", "id": "Mengambil Risiko Penuh." },
  { "en": "Arti Frasa 'Have A Sweet Tooth'?", "id": "Suka Makanan Manis." },
  { "en": "Arti Frasa 'It's A Steal'?", "id": "Harganya Sangat Murah." },
  { "en": "Arti Frasa 'Let Your Hair Down'?", "id": "Bersantai Dan Bersenang-Senang." },
  { "en": "Arti Frasa 'Mumbo Jumbo'?", "id": "Omong Kosong Yang Tidak Masuk Akal." },
  { "en": "Arti Frasa 'Off The Top Of My Head'?", "id": "Tanpa Berpikir Panjang." },
  { "en": "Arti Frasa 'On Cloud Nine'?", "id": "Sangat Bahagia." },
  { "en": "Arti Frasa 'Put A Sock In It'?", "id": "Diam! (Sangat Kasar)." },
  { "en": "Arti Frasa 'Quick And Dirty'?", "id": "Cepat Tapi Tidak Sempurna." },
  { "en": "Arti Frasa 'Rise And Shine'?", "id": "Bangun Dan Semangat (Di Pagi Hari)." },
  { "en": "Arti Frasa 'Status Quo'?", "id": "Keadaan Saat Ini." },
  { "en": "Arti Frasa 'Take It Easy'?", "id": "Santai Saja." },
  { "en": "Arti Frasa 'The Plot Thickens'?", "id": "Situasi Menjadi Semakin Rumit." },
  { "en": "Arti Frasa 'A Tough Nut To Crack'?", "id": "Masalah Atau Orang Yang Sulit." },
  { "en": "Arti Frasa 'An Axe To Grind'?", "id": "Punya Dendam Atau Maksud Tersembunyi." },
  { "en": "Arti Frasa 'At The End Of Your Rope'?", "id": "Putus Asa, Kehabisan Pilihan." },
  { "en": "Arti Frasa 'Back To Square One'?", "id": "Harus Mengulang Dari Awal." },
  { "en": "Arti Frasa 'Bark Is Worse Than Your Bite'?", "id": "Terlihat Galak Tapi Sebenarnya Baik." },
  { "en": "Arti Frasa 'Beggars Can't Be Choosers'?", "id": "Jangan Banyak Menuntut Jika Diberi." },
  { "en": "Arti Frasa 'Between A Rock And A Hard Place'?", "id": "Dalam Situasi Sulit." },
  { "en": "Arti Frasa 'Bite Off More Than You Can Chew'?", "id": "Mengambil Tugas Melebihi Kemampuan." },
  { "en": "Arti Frasa 'Blood Is Thicker Than Water'?", "id": "Keluarga Lebih Penting Dari Siapapun." },
  { "en": "Arti Frasa 'Burn The Midnight Oil'?", "id": "Bekerja Atau Belajar Sampai Larut Malam." },
  { "en": "Arti Frasa 'Call A Spade A Spade'?", "id": "Berbicara Jujur Dan Terus Terang." },
  { "en": "Arti Frasa 'Chip On Your Shoulder'?", "id": "Merasa Rendah Diri Dan Mudah Tersinggung." },
  { "en": "Arti Frasa 'Come Hell Or High Water'?", "id": "Apapun Yang Terjadi." },
  { "en": "Arti Frasa 'Crack Someone Up'?", "id": "Membuat Seseorang Tertawa Terbahak-Bahak." },
  { "en": "Arti Frasa 'Cry Over Spilt Milk'?", "id": "Menyesali Sesuatu Yang Sudah Terjadi." },
  { "en": "Arti Frasa 'Cut The Mustard'?", "id": "Memenuhi Standar Atau Harapan." },
  { "en": "Arti Frasa 'Devil's Advocate'?", "id": "Berargumen Melawan Untuk Menguji." },
  { "en": "Arti Frasa 'Don't Put All Your Eggs In One Basket'?", "id": "Jangan Mengambil Satu Risiko Saja." },
  { "en": "Arti Frasa 'Drastic Times Call For Drastic Measures'?", "id": "Situasi Ekstrem Butuh Tindakan Ekstrem." },
  { "en": "Arti Frasa 'Elvis Has Left The Building'?", "id": "Pertunjukan Telah Selesai." },
  { "en": "Arti Frasa 'Every Dog Has His Day'?", "id": "Semua Orang Akan Dapat Kesempatan." },
  { "en": "Arti Frasa 'Far Cry From'?", "id": "Sangat Berbeda Dari." },
  { "en": "Arti Frasa 'Field Day'?", "id": "Momen Yang Sangat Menyenangkan." },
  { "en": "Arti Frasa 'Finding Your Feet'?", "id": "Mulai Merasa Nyaman Dan Beradaptasi." },
  { "en": "Arti Frasa 'Fixed In Your Ways'?", "id": "Keras Kepala, Sulit Berubah." },
  { "en": "Arti Frasa 'Get A Word In Edgewise'?", "id": "Kesulitan Mendapat Kesempatan Berbicara." },
  { "en": "Arti Frasa 'Give The Benefit Of The Doubt'?", "id": "Percaya Seseorang Meskipun Ragu." },
  { "en": "Arti Frasa 'Go Down That Road'?", "id": "Membahas Topik Sensitif Itu." },
  { "en": "Arti Frasa 'Hear It Through The Grapevine'?", "id": "Mendengar Rumor Atau Gosip." },
  { "en": "Arti Frasa 'Heard It All Before'?", "id": "Sudah Sering Mendengarnya." },
  { "en": "Arti Frasa 'Hit The Hay'?", "id": "Pergi Tidur." },
  { "en": "Arti Frasa 'Icing On The Cake'?", "id": "Sesuatu Yang Baik Di Atas Kebaikan." },
  { "en": "Arti Frasa 'In Full Swing'?", "id": "Sedang Berada Di Puncaknya." },
  { "en": "Arti Frasa 'It's A Small World'?", "id": "Dunia Ini Sempit." },
  { "en": "Arti Frasa 'Jump The Gun'?", "id": "Memulai Sesuatu Terlalu Cepat." },
  { "en": "Arti Frasa 'Keep Something At Bay'?", "id": "Menjaga Jarak Dari Sesuatu." },
  { "en": "Arti Frasa 'Kill Two Birds With One Stone'?", "id": "Satu Aksi Untuk Dua Hasil." },
  { "en": "Arti Frasa 'Last But Not Least'?", "id": "Terakhir Tapi Tidak Kalah Penting." },
  { "en": "Arti Frasa 'Leave No Stone Unturned'?", "id": "Mencari Di Semua Tempat." },
  { "en": "Arti Kata 'NSFW'?", "id": "Tidak Aman Untuk Dilihat Di Tempat Kerja." },
  { "en": "Kepanjangan 'NSFW'?", "id": "Not Safe For Work / Tidak Aman Untuk Kerja." },
  { "en": "Arti Kata 'TL;DR'?", "id": "Ringkasan Dari Teks Yang Panjang." },
  { "en": "Kepanjangan 'TL;DR'?", "id": "Too Long; Didn't Read / Terlalu Panjang; Tidak Dibaca." },
  { "en": "Arti Kata 'OOTD'?", "id": "Pakaian Yang Dikenakan Hari Ini." },
  { "en": "Kepanjangan 'OOTD'?", "id": "Outfit Of The Day / Pakaian Hari Ini." },
  { "en": "Arti Kata 'AMA'?", "id": "Sesi Tanya Jawab Apapun." },
  { "en": "Kepanjangan 'AMA'?", "id": "Ask Me Anything / Tanyakan Apapun." },
  { "en": "Arti Kata 'FTW'?", "id": "Demi Kemenangan (Ekspresi Antusias)." },
  { "en": "Kepanjangan 'FTW'?", "id": "For The Win / Demi Kemenangan." },
  { "en": "Arti Kata 'GTG' / 'G2G'?", "id": "Aku Harus Pergi Sekarang." },
  { "en": "Kepanjangan 'GTG' / 'G2G'?", "id": "Got To Go / Harus Pergi." },
  { "en": "Arti Kata 'IDC'?", "id": "Aku Tidak Peduli." },
  { "en": "Kepanjangan 'IDC'?", "id": "I Don't Care / Aku Tidak Peduli." },
  { "en": "Arti Kata 'JK'?", "id": "Cuma Bercanda." },
  { "en": "Kepanjangan 'JK'?", "id": "Just Kidding / Cuma Bercanda." },
  { "en": "Arti Kata 'NP'?", "id": "Tidak Masalah." },
  { "en": "Kepanjangan 'NP'?", "id": "No Problem / Tidak Masalah." },
  { "en": "Arti Kata 'NGL'?", "id": "Jujur Saja, Sebenarnya." },
  { "en": "Kepanjangan 'NGL'?", "id": "Not Gonna Lie / Tidak Akan Bohong." },
  { "en": "Arti Kata 'RN'?", "id": "Saat Ini Juga." },
  { "en": "Kepanjangan 'RN'?", "id": "Right Now / Saat Ini Juga." },
  { "en": "Arti Kata 'TBA'?", "id": "Akan Diumumkan Nanti." },
  { "en": "Kepanjangan 'TBA'?", "id": "To Be Announced / Akan Diumumkan." },
  { "en": "Arti Kata 'TBD'?", "id": "Akan Ditentukan Nanti." },
  { "en": "Kepanjangan 'TBD'?", "id": "To Be Determined / Akan Ditentukan." },
  { "en": "Arti Kata 'Arvo'?", "id": "Sore Hari." },
  { "en": "Arti Kata 'Barbie'?", "id": "Barbekyu." },
  { "en": "Arti Kata 'Brekky'?", "id": "Sarapan." },
  { "en": "Arti Kata 'Chockers'?", "id": "Sangat Penuh." },
  { "en": "Arti Frasa 'Good On Ya'?", "id": "Kerja Bagus." },
  { "en": "Arti Kata 'Maccas'?", "id": "Restoran Mcdonald's." },
  { "en": "Arti Kata 'Roo'?", "id": "Kanguru." },
  { "en": "Arti Frasa 'Spit The Dummy'?", "id": "Marah-Marah." },
  { "en": "Arti Frasa 'The Real Mccoy'?", "id": "Yang Asli, Bukan Tiruan." },
  { "en": "Arti Kata 'Yabber'?", "id": "Banyak Bicara." },
  { "en": "Arti Kata 'Blud'?", "id": "Teman Dekat." },
  { "en": "Arti Kata 'Fit'?", "id": "Sangat Menarik." },
  { "en": "Arti Kata 'Loo'?", "id": "Toilet." },
  { "en": "Arti Kata 'Nutter'?", "id": "Orang Gila." },
  { "en": "Arti Kata 'Plastered'?", "id": "Sangat Mabuk." },
  { "en": "Arti Kata 'Snog'?", "id": "Ciuman Panas." },
  { "en": "Arti Kata 'Wicked'?", "id": "Sangat Keren." },
  { "en": "Arti Frasa 'My G'?", "id": "Panggilan Untuk Teman Dekat." },
  { "en": "Arti Kata 'OFC'?", "id": "Tentu Saja." },
  { "en": "Kepanjangan 'OFC'?", "id": "Of Course / Tentu Saja." },
  { "en": "Arti Kata 'OMW'?", "id": "Sedang Dalam Perjalanan." },
  { "en": "Kepanjangan 'OMW'?", "id": "On My Way / Sedang Dalam Perjalanan." },
  { "en": "Arti Kata 'TTYL'?", "id": "Nanti Kita Ngobrol Lagi." },
  { "en": "Kepanjangan 'TTYL'?", "id": "Talk To You Later / Nanti Bicara Lagi." },
  { "en": "Arti Kata 'HMU'?", "id": "Hubungi Aku." },
  { "en": "Kepanjangan 'HMU'?", "id": "Hit Me Up / Hubungi Aku." },
  { "en": "Arti Kata 'IKR'?", "id": "Iya Kan? (Setuju)." },
  { "en": "Kepanjangan 'IKR'?", "id": "I Know, Right? / Aku Tahu, Kan?" },
  { "en": "Arti Kata 'LMK'?", "id": "Beri Tahu Aku." },
  { "en": "Kepanjangan 'LMK'?", "id": "Let Me Know / Beri Tahu Aku." },
  { "en": "Arti Frasa 'Touch Grass'?", "id": "Kembali Ke Dunia Nyata." },
  { "en": "Arti Frasa 'A Penny For Your Thoughts'?", "id": "Apa Yang Sedang Kamu Pikirkan?" },
  { "en": "Arti Frasa 'Actions Speak Louder Than Words'?", "id": "Tindakan Lebih Penting Dari Kata-Kata." },
  { "en": "Arti Frasa 'Add Insult To Injury'?", "id": "Memperburuk Keadaan Yang Sudah Buruk." },
  { "en": "Arti Frasa 'All Thumbs'?", "id": "Sangat Ceroboh Atau Kaku." },
  { "en": "Arti Frasa 'An Arm And A Leg'?", "id": "Sesuatu Yang Sangat Mahal." },
  { "en": "Arti Frasa 'At The Drop Of A Hat'?", "id": "Melakukan Sesuatu Seketika." },
  { "en": "Arti Frasa 'Back To The Drawing Board'?", "id": "Mulai Lagi Dari Awal." },
  { "en": "Arti Frasa 'Barking Up The Wrong Tree'?", "id": "Salah Sasaran Atau Salah Tuduh." },
  { "en": "Arti Frasa 'Beat A Dead Horse'?", "id": "Membahas Sesuatu Yang Sudah Selesai." },
  { "en": "Arti Frasa 'A Chip Off The Old Block'?", "id": "Anak Yang Sangat Mirip Orang Tuanya." },
  { "en": "Arti Frasa 'A Clean Slate'?", "id": "Awal Yang Baru, Tanpa Kesalahan Lama." },
  { "en": "Arti Frasa 'A Gut Feeling'?", "id": "Firasat Atau Intuisi." },
  { "en": "Arti Frasa 'A Leopard Can't Change His Spots'?", "id": "Sifat Asli Sulit Diubah." },
  { "en": "Arti Frasa 'A Slap On The Wrist'?", "id": "Hukuman Yang Sangat Ringan." },
  { "en": "Arti Frasa 'A Taste Of Your Own Medicine'?", "id": "Merasakan Perlakuan Burukmu Sendiri." },
  { "en": "Arti Frasa 'All In The Same Boat'?", "id": "Berada Dalam Situasi Sulit Yang Sama." },
  { "en": "Arti Frasa 'Ballpark Figure'?", "id": "Perkiraan Kasar." },
  { "en": "Arti Frasa 'Bend Over Backwards'?", "id": "Berusaha Sangat Keras Membantu." },
  { "en": "Arti Frasa 'Black Sheep Of The Family'?", "id": "Anggota Keluarga Yang Dianggap Berbeda." },
  { "en": "Arti Frasa 'Blow Your Own Horn'?", "id": "Menyombongkan Diri Sendiri." },
  { "en": "Arti Frasa 'Bolt From The Blue'?", "id": "Kejadian Tiba-Tiba Yang Mengejutkan." },
  { "en": "Arti Frasa 'Break New Ground'?", "id": "Melakukan Sesuatu Yang Belum Pernah." },
  { "en": "Arti Frasa 'Wouldn't Harm A Fly'?", "id": "Orang Yang Sangat Lembut Dan Baik." },
  { "en": "Arti Frasa 'Burn Your Bridges'?", "id": "Merusak Hubungan Secara Permanen." },
  { "en": "Arti Frasa 'Bury Your Head In The Sand'?", "id": "Mengabaikan Masalah." },
  { "en": "Arti Frasa 'By The Book'?", "id": "Mengikuti Aturan Dengan Ketat." },
  { "en": "Arti Frasa 'Clear The Air'?", "id": "Menjernihkan Kesalahpahaman." },
  { "en": "Arti Frasa 'Come Out Of Your Shell'?", "id": "Menjadi Lebih Terbuka Dan Percaya Diri." },
  { "en": "Arti Frasa 'Cross That Bridge When You Come To It'?", "id": "Pikirkan Nanti Jika Sudah Terjadi." },
  { "en": "Arti Frasa 'Cry Your Eyes Out'?", "id": "Menangis Tersedu-Sedu." },
  { "en": "Arti Frasa 'Cut To The Chase'?", "id": "Langsung Ke Inti Pembicaraan." },
  { "en": "Arti Frasa 'Dig Your Own Grave'?", "id": "Menyebabkan Masalah Bagi Diri Sendiri." },
  { "en": "Arti Frasa 'Down In The Dumps'?", "id": "Merasa Sangat Sedih." },
  { "en": "Arti Frasa 'Drop In The Ocean'?", "id": "Jumlah Yang Sangat Kecil." },
  { "en": "Kepanjangan 'FYI'?", "id": "For Your Information / Sebagai Informasi." },
  { "en": "Kepanjangan 'A.K.A.'?", "id": "Also Known As / Dikenal Juga Sebagai." },
  { "en": "Kepanjangan 'T.G.I.F.'?", "id": "Thank God It's Friday / Syukurlah Hari Jumat." },
  { "en": "Kepanjangan 'D.I.Y.'?", "id": "Do It Yourself / Kerjakan Sendiri." },
  { "en": "Kepanjangan 'F.A.Q.'?", "id": "Frequently Asked Questions / Pertanyaan Sering Diajukan." },
  { "en": "Kepanjangan 'R.S.V.P.'?", "id": "Répondez S'il Vous Plaît / Harap Balas Undangan." },
  { "en": "Kepanjangan 'E.T.A.'?", "id": "Estimated Time of Arrival / Perkiraan Waktu Tiba." },
  { "en": "Arti Kata 'TG'?", "id": "Grup Telegram." },
  { "en": "Arti Kata 'IG'?", "id": "Instagram." },
  { "en": "Arti Kata 'YT'?", "id": "YouTube." },
  { "en": "Arti Frasa 'Eat Your Words'?", "id": "Mengakui Telah Berkata Salah." },
  { "en": "Arti Frasa 'Fair And Square'?", "id": "Jujur Dan Adil." },
  { "en": "Arti Frasa 'Feel The Pinch'?", "id": "Mulai Merasakan Kesulitan Keuangan." },
  { "en": "Arti Frasa 'Fifth Wheel'?", "id": "Orang Yang Tidak Dibutuhkan (Nyamuk)." },
  { "en": "Arti Frasa 'Flesh And Blood'?", "id": "Keluarga Kandung." },
  { "en": "Arti Frasa 'Get It Off Your Chest'?", "id": "Mengungkapkan Apa Yang Mengganjal." },
  { "en": "Arti Frasa 'Get Your Foot In The Door'?", "id": "Mendapatkan Kesempatan Awal." },
  { "en": "Arti Frasa 'Give It A Whirl'?", "id": "Mencobanya." },
  { "en": "Arti Frasa 'Go For It'?", "id": "Lakukan Saja, Kejar Itu." },
  { "en": "Arti Frasa 'Grease Someone's Palm'?", "id": "Menyuap Seseorang." },
  { "en": "Arti Frasa 'Green Thumb'?", "id": "Pintar Berkebun." },
  { "en": "Arti Frasa 'Hard Pill To Swallow'?", "id": "Kenyataan Pahit Yang Sulit Diterima." },
  { "en": "Arti Frasa 'Have Your Head In The Clouds'?", "id": "Berkhayal, Tidak Realistis." },
  { "en": "Arti Frasa 'Heart In Your Mouth'?", "id": "Sangat Gugup Atau Takut." },
  { "en": "Arti Frasa 'Hold Your Tongue'?", "id": "Menahan Diri Untuk Tidak Berbicara." },
  { "en": "Arti Frasa 'In The Same Breath'?", "id": "Mengatakan Dua Hal Bertentangan." },
  { "en": "Arti Frasa 'It's A Rip-Off'?", "id": "Harganya Kemahalan." },
  { "en": "Arti Frasa 'It's Not My Thing'?", "id": "Itu Bukan Seleraku." },
  { "en": "Arti Frasa 'Jump To Conclusions'?", "id": "Terlalu Cepat Mengambil Kesimpulan." },
  { "en": "Arti Frasa 'Keep A Straight Face'?", "id": "Berusaha Untuk Tidak Tertawa." },
  { "en": "Arti Frasa 'Kick The Habit'?", "id": "Menghentikan Kebiasaan Buruk." },
  { "en": "Arti Frasa 'Know The Ropes'?", "id": "Sangat Paham Seluk Beluknya." },
  { "en": "Arti Frasa 'Let The Chips Fall Where They May'?", "id": "Biarkan Terjadi Apa Adanya." },
  { "en": "Arti Frasa 'Like Two Peas In A Pod'?", "id": "Sangat Mirip." },
  { "en": "Arti Frasa 'Live And Learn'?", "id": "Belajar Dari Pengalaman." },
  { "en": "Arti Frasa 'Make A Long Story Short'?", "id": "Singkatnya..." },
  { "en": "Arti Frasa 'My Lips Are Sealed'?", "id": "Aku Akan Menjaga Rahasia." },
  { "en": "Arti Frasa 'Off The Record'?", "id": "Tidak Untuk Dipublikasikan." },
  { "en": "Arti Frasa 'On Pins And Needles'?", "id": "Sangat Gugup Dan Cemas." },
  { "en": "Arti Frasa 'On The Same Page'?", "id": "Memiliki Pemahaman Yang Sama." },
  { "en": "Arti Frasa 'Out Of The Blue'?", "id": "Tiba-Tiba." },
  { "en": "Arti Frasa 'Out Of This World'?", "id": "Sangat Luar Biasa." },
  { "en": "Arti Frasa 'Pour Your Heart Out'?", "id": "Mencurahkan Seluruh Isi Hati." },
  { "en": "Arti Frasa 'Put Your Foot In Your Mouth'?", "id": "Salah Bicara Dan Menyinggung." },
  { "en": "Arti Frasa 'Read Someone's Mind'?", "id": "Menebak Pikiran Seseorang." },
  { "en": "Arti Frasa 'Saved By The Bell'?", "id": "Terselamatkan Tepat Pada Waktunya." },
  { "en": "Arti Frasa 'Sick And Tired'?", "id": "Sangat Muak." },
  { "en": "Arti Frasa 'Scrape The Bottom Of The Barrel'?", "id": "Mengambil Dari Pilihan Terburuk." },
  { "en": "Arti Frasa 'The Whole Kit And Caboodle'?", "id": "Semuanya, Seluruh Rangkaian Barang." },
  { "en": "Arti Frasa 'Throw Caution To The Wind'?", "id": "Mengambil Risiko Tanpa Ragu." },
  { "en": "Arti Frasa 'Up In The Air'?", "id": "Belum Diputuskan, Tidak Pasti." },
  { "en": "Arti Frasa 'Wear Your Heart On Your Sleeve'?", "id": "Menunjukkan Perasaan Secara Terbuka." },
  { "en": "Arti Frasa 'A Wet Blanket'?", "id": "Orang Yang Merusak Suasana." },
  { "en": "Arti Frasa 'You Can't Make An Omelette Without Breaking Some Eggs'?", "id": "Perlu Pengorbanan Untuk Mencapai Sesuatu." },
  { "en": "Arti Frasa 'Zero Tolerance'?", "id": "Tidak Ada Toleransi Sama Sekali." },
  { "en": "Arti Frasa 'A Man Of Few Words'?", "id": "Pria Yang Tidak Banyak Bicara." },
  { "en": "Arti Frasa 'All In Good Time'?", "id": "Sabar, Akan Terjadi Pada Waktunya." },
  { "en": "Arti Frasa 'All It's Cracked Up To Be'?", "id": "Sesuai Dengan Reputasinya Yang Hebat." },
  { "en": "Arti Frasa 'Anything But'?", "id": "Sama Sekali Bukan." },
  { "en": "Arti Frasa 'As Thick As Thieves'?", "id": "Sangat Akrab Dan Punya Rahasia." },
  { "en": "Arti Frasa 'At Your Wits' End'?", "id": "Frustrasi Dan Kehabisan Ide." },
  { "en": "Arti Frasa 'Back To The Salt Mines'?", "id": "Kembali Bekerja Keras." },
  { "en": "Arti Frasa 'Bad Blood'?", "id": "Perasaan Benci Atau Permusuhan." },
  { "en": "Arti Frasa 'Basket Case'?", "id": "Orang Yang Sangat Cemas Atau Gila." },
  { "en": "Arti Frasa 'Beating A Dead Horse'?", "id": "Membahas Sesuatu Yang Sudah Usai." },
  { "en": "Arti Frasa 'Bed Of Roses'?", "id": "Situasi Yang Mudah Dan Nyaman." },
  { "en": "Arti Frasa 'Bend The Rules'?", "id": "Melanggar Aturan Sedikit." },
  { "en": "Arti Frasa 'Bet Your Bottom Dollar'?", "id": "Sangat Yakin Akan Sesuatu." },
  { "en": "Arti Frasa 'Big Shot'?", "id": "Orang Penting Atau Berkuasa." },
  { "en": "Arti Frasa 'Birds Of A Feather Flock Together'?", "id": "Orang-Orang Mirip Akan Berkumpul." },
  { "en": "Arti Frasa 'Bite The Big One'?", "id": "Mati Atau Gagal Total." },
  { "en": "Arti Frasa 'Blew It'?", "id": "Menyia-Nyiakan Kesempatan." },
  { "en": "Arti Frasa 'Blow Smoke'?", "id": "Berkata Omong Kosong." },
  { "en": "Arti Frasa 'Bottom Line'?", "id": "Intinya, Poin Paling Penting." },
  { "en": "Arti Frasa 'Bull In A China Shop'?", "id": "Orang Yang Sangat Ceroboh." },
  { "en": "Arti Frasa 'Bump Heads'?", "id": "Bertentangan Atau Berkonflik." },
  { "en": "Arti Frasa 'Butter Someone Up'?", "id": "Merayu Atau Menjilat Seseorang." },
  { "en": "Arti Frasa 'By All Means'?", "id": "Tentu Saja, Silakan." },
  { "en": "Arti Frasa 'Can Of Worms'?", "id": "Masalah Baru Yang Rumit." },
  { "en": "Arti Frasa 'Can't Hold A Candle To'?", "id": "Tidak Ada Apa-Apanya Dibanding." },
  { "en": "Arti Frasa 'Cat Nap'?", "id": "Tidur Siang Sebentar." },
  { "en": "Arti Frasa 'Champagne Taste On A Beer Budget'?", "id": "Gaya Hidup Melebihi Kemampuan." },
  { "en": "Arti Frasa 'Chow Down'?", "id": "Makan Dengan Lahap." },
  { "en": "Arti Frasa 'Clean As A Whistle'?", "id": "Sangat Bersih." },
  { "en": "Arti Frasa 'Close, But No Cigar'?", "id": "Hampir Berhasil, Tapi Gagal." },
  { "en": "Arti Frasa 'Clutch'?", "id": "Berhasil Di Momen Krusial." },
  { "en": "Arti Frasa 'Come Clean'?", "id": "Mengaku Jujur." },
  { "en": "Arti Frasa 'Cool Your Jets'?", "id": "Tenangkan Dirimu." },
  { "en": "Arti Frasa 'Cop Out'?", "id": "Menghindar Dari Tanggung Jawab." },
  { "en": "Arti Frasa 'Cream Of The Crop'?", "id": "Yang Terbaik Dari Semuanya." },
  { "en": "Kepanjangan 'NSFL'?", "id": "Not Safe For Life / Tidak Aman Untuk Kehidupan." },
  { "en": "Kepanjangan 'YMMV'?", "id": "Your Mileage May Vary / Hasil Bisa Bervariasi." },
  { "en": "Kepanjangan 'O.T.P.'?", "id": "One True Pairing / Pasangan Fiksi Sempurna." },
  { "en": "Kepanjangan 'AFAIK'?", "id": "As Far As I Know / Sepengetahuanku." },
  { "en": "Kepanjangan 'J.S.Y.K.'?", "id": "Just So You Know / Sekadar Informasi Untukmu." },
  { "en": "Arti Frasa 'Crocodile Tears'?", "id": "Air Mata Buaya (Palsu)." },
  { "en": "Arti Frasa 'Cut The Gordian Knot'?", "id": "Menyelesaikan Masalah Rumit Dengan Cepat." },
  { "en": "Arti Frasa 'Dead Ringer'?", "id": "Sangat Mirip, Kembaran." },
  { "en": "Arti Frasa 'Dog Days'?", "id": "Hari-Hari Yang Sangat Panas." },
  { "en": "Arti Frasa 'Don't Look A Gift Horse In The Mouth'?", "id": "Jangan Mengkritik Hadiah." },
  { "en": "Arti Frasa 'Down To The Wire'?", "id": "Sampai Detik-Detik Terakhir." },
  { "en": "Arti Frasa 'Eat Crow'?", "id": "Mengakui Kesalahan Dengan Malu." },
  { "en": "Arti Frasa 'Eighty-Six'?", "id": "Menolak Atau Membuang Sesuatu." },
  { "en": "Arti Frasa 'Every Tom, Dick, And Harry'?", "id": "Semua Orang, Siapa Saja." },
  { "en": "Arti Frasa 'Fall On Your Sword'?", "id": "Bertanggung Jawab Dan Menerima Konsekuensi." },
  { "en": "Arti Frasa 'Feather In Your Cap'?", "id": "Sebuah Pencapaian Yang Membanggakan." },
  { "en": "Arti Frasa 'Flavor Of The Month'?", "id": "Sesuatu Yang Populer Saat Ini." },
  { "en": "Arti Frasa 'Flea Market'?", "id": "Pasar Barang Bekas." },
  { "en": "Arti Frasa 'Flesh Out'?", "id": "Memberikan Lebih Banyak Detail." },
  { "en": "Arti Frasa 'Fly By The Seat Of Your Pants'?", "id": "Berimprovisasi Tanpa Rencana." },
  { "en": "Arti Frasa 'Fly On The Wall'?", "id": "Pengamat Rahasia." },
  { "en": "Arti Frasa 'For Kicks'?", "id": "Untuk Bersenang-Senang Saja." },
  { "en": "Arti Frasa 'Freudian Slip'?", "id": "Kesalahan Ucap Yang Mengungkapkan Pikiran." },
  { "en": "Arti Frasa 'Full Monty'?", "id": "Keseluruhan, Semuanya." },
  { "en": "Arti Frasa 'Get A Grip'?", "id": "Kendalikan Dirimu." },
  { "en": "Arti Frasa 'Get Cold Feet'?", "id": "Takut Tepat Sebelum Melakukan Sesuatu." },
  { "en": "Arti Frasa 'Get The Show On The Road'?", "id": "Mari Kita Mulai." },
  { "en": "Arti Frasa 'Get Up On The Wrong Side Of The Bed'?", "id": "Bangun Dengan Suasana Hati Buruk." },
  { "en": "Arti Frasa 'Give Someone The Axe'?", "id": "Memecat Seseorang." },
  { "en": "Arti Frasa 'Go Against The Grain'?", "id": "Melawan Arus Atau Kebiasaan." },
  { "en": "Arti Frasa 'Go Out On A Limb'?", "id": "Mengambil Risiko." },
  { "en": "Arti Frasa 'Graveyard Shift'?", "id": "Shift Kerja Malam." },
  { "en": "Arti Frasa 'Greased Lightning'?", "id": "Sesuatu Yang Sangat Cepat." },
  { "en": "Arti Frasa 'Hat Trick'?", "id": "Tiga Kesuksesan Berturut-Turut." },
  { "en": "Arti Frasa 'Have Bitten Off More Than Can Chew'?", "id": "Mengambil Tugas Terlalu Berat." },
  { "en": "Arti Frasa 'Heads Will Roll'?", "id": "Akan Ada Yang Dihukum." },
  { "en": "Arti Frasa 'High And Dry'?", "id": "Ditinggalkan Dalam Situasi Sulit." },
  { "en": "Arti Frasa 'Hit The Ground Running'?", "id": "Langsung Bekerja Keras." },
  { "en": "Arti Frasa 'Hit The Spot'?", "id": "Sangat Memuaskan." },
  { "en": "Arti Frasa 'Honest To God'?", "id": "Jujur Demi Tuhan." },
  { "en": "Arti Frasa 'In A Jam'?", "id": "Dalam Masalah." },
  { "en": "Arti Frasa 'In The Fast Lane'?", "id": "Gaya Hidup Penuh Aksi." },
  { "en": "Arti Frasa 'In The Hot Seat'?", "id": "Dalam Posisi Sulit/Disalahkan." },
  { "en": "Arti Frasa 'It's A Bust'?", "id": "Gagal Total." },
  { "en": "Arti Frasa 'Jack Of All Trades'?", "id": "Bisa Melakukan Banyak Hal." },
  { "en": "Arti Frasa 'Jig Is Up'?", "id": "Permainannya Sudah Berakhir." },
  { "en": "Arti Frasa 'Keep A Low Profile'?", "id": "Tidak Menarik Perhatian." },
  { "en": "Arti Frasa 'Kick The Bucket'?", "id": "Meninggal Dunia." },
  { "en": "Arti Frasa 'Knock On Wood'?", "id": "Mengetuk Kayu (Supaya Tidak Sial)." },
  { "en": "Arti Frasa 'Lick Your Wounds'?", "id": "Memulihkan Diri Setelah Gagal." },
  { "en": "Arti Frasa 'Look Like A Million Bucks'?", "id": "Terlihat Sangat Menawan Dan Mahal." },
  { "en": "Arti Frasa 'Make Ends Meet'?", "id": "Mencukupi Kebutuhan Hidup Sehari-Hari." },
  { "en": "Arti Frasa 'My Way Or The Highway'?", "id": "Ikuti Caraku Atau Pergi." },
  { "en": "Arti Frasa 'Muddy The Waters'?", "id": "Membuat Situasi Menjadi Lebih Rumit." },
  { "en": "Arti Frasa 'Neck And Neck'?", "id": "Bersaing Sangat Ketat, Seimbang." },
  { "en": "Arti Frasa 'New Kid On The Block'?", "id": "Anak Baru, Orang Baru." },
  { "en": "Arti Frasa 'No Room To Swing A Cat'?", "id": "Ruangan Yang Sangat Sempit." },
  { "en": "Arti Frasa 'Not Playing With A Full Deck'?", "id": "Orang Yang Agak Bodoh." },
  { "en": "Arti Frasa 'On The Back Burner'?", "id": "Dikesampingkan Untuk Sementara." },
  { "en": "Arti Frasa 'On The Same Wavelength'?", "id": "Memiliki Pemikiran Atau Ide Yang Sama." },
  { "en": "Arti Frasa 'Out Of The Frying Pan And Into The Fire'?", "id": "Dari Masalah Ke Masalah Lebih Besar." },
  { "en": "Arti Frasa 'Par For The Course'?", "id": "Sesuatu Yang Normal Atau Biasa." },
  { "en": "Arti Frasa 'Pass With Flying Colors'?", "id": "Lulus Dengan Nilai Sangat Baik." },
  { "en": "Arti Frasa 'Pick Up The Tab'?", "id": "Membayar Tagihan." },
  { "en": "Arti Frasa 'Plain As Day'?", "id": "Sangat Jelas Untuk Dilihat." },
  { "en": "Arti Frasa 'Play Devil's Advocate'?", "id": "Berpura-Pura Menentang Untuk Diskusi." },
  { "en": "Arti Frasa 'Pop The Question'?", "id": "Melamar Seseorang Untuk Menikah." },
  { "en": "Arti Frasa 'Pushing Up Daisies'?", "id": "Sudah Meninggal Dunia." },
  { "en": "Arti Frasa 'Put All Your Eggs In One Basket'?", "id": "Menaruh Semua Risiko Di Satu Tempat." },
  { "en": "Arti Frasa 'Put Your Best Foot Forward'?", "id": "Berusaha Memberikan Kesan Terbaik." },
  { "en": "Arti Frasa 'Queer The Pitch'?", "id": "Merusak Rencana Seseorang." },
  { "en": "Arti Frasa 'Rain Cats And Dogs'?", "id": "Hujan Sangat Deras." },
  { "en": "Arti Frasa 'Ring A Bell'?", "id": "Terdengar Familiar." },
  { "en": "Arti Frasa 'Rock The Boat'?", "id": "Membuat Masalah, Mengganggu Ketenangan." },
  { "en": "Arti Frasa 'Run Out Of Steam'?", "id": "Kehabisan Energi Atau Antusiasme." },
  { "en": "Arti Frasa 'Save For A Rainy Day'?", "id": "Menyimpan Uang Untuk Masa Sulit." },
  { "en": "Arti Frasa 'Scapegoat'?", "id": "Kambing Hitam." },
  { "en": "Arti Frasa 'Scot-Free'?", "id": "Lolos Dari Hukuman." },
  { "en": "Arti Frasa 'Second To None'?", "id": "Yang Terbaik." },
  { "en": "Arti Frasa 'Shake A Leg'?", "id": "Cepat, Bergegas." },
  { "en": "Arti Frasa 'Shot In The Dark'?", "id": "Tebakan Asal." },
  { "en": "Arti Frasa 'Sing A Different Tune'?", "id": "Mengubah Pendapat." },
  { "en": "Arti Frasa 'Sink Or Swim'?", "id": "Berhasil Atau Gagal Sendiri." },
  { "en": "Arti Frasa 'Sitting On The Fence'?", "id": "Ragu-Ragu, Tidak Memihak." },
  { "en": "Arti Frasa 'Sixth Sense'?", "id": "Indra Keenam, Intuisi." },
  { "en": "Arti Frasa 'Skating On Thin Ice'?", "id": "Berada Dalam Situasi Berisiko." },
  { "en": "Arti Frasa 'Small Talk'?", "id": "Basa-Basi." },
  { "en": "Arti Frasa 'Smell A Rat'?", "id": "Mencurigai Adanya Pengkhianatan." },
  { "en": "Arti Frasa 'Start From Scratch'?", "id": "Memulai Dari Nol." },
  { "en": "Arti Kata 'S.O.'?", "id": "Orang Terpenting (Pasangan)." },
  { "en": "Kepanjangan 'S.O.'?", "id": "Significant Other / Orang Terpenting." },
  { "en": "Arti Kata 'N.B.D.'?", "id": "Bukan Masalah Besar." },
  { "en": "Kepanjangan 'N.B.D.'?", "id": "No Big Deal / Bukan Masalah Besar." },
  { "en": "Arti Kata 'T.L.C.'?", "id": "Perhatian Dan Kasih Sayang." },
  { "en": "Kepanjangan 'T.L.C.'?", "id": "Tender Loving Care." },
  { "en": "Arti Kata 'P.D.A.'?", "id": "Bermesraan Di Depan Umum." },
  { "en": "Kepanjangan 'P.D.A.'?", "id": "Public Display of Affection." },
  { "en": "Arti Kata 'F.W.B.'?", "id": "Teman Tapi Mesra." },
  { "en": "Kepanjangan 'F.W.B.'?", "id": "Friends With Benefits." },
  { "en": "Arti Kata 'D.T.R.'?", "id": "Membicarakan Status Hubungan." },
  { "en": "Kepanjangan 'D.T.R.'?", "id": "Define The Relationship." },
  { "en": "Arti Kata 'I.R.L.'?", "id": "Di Dunia Nyata." },
  { "en": "Kepanjangan 'I.R.L.'?", "id": "In Real Life." },
  { "en": "Arti Kata 'Y.W.'?", "id": "Sama-Sama." },
  { "en": "Kepanjangan 'Y.W.'?", "id": "You're Welcome." },
  { "en": "Arti Kata 'N.P.'?", "id": "Tidak Masalah." },
  { "en": "Kepanjangan 'N.P.'?", "id": "No Problem." },
  { "en": "Arti Kata 'I.C.Y.M.I.'?", "id": "Kalau-Kalau Kamu Ketinggalan Info." },
  { "en": "Kepanjangan 'I.C.Y.M.I.'?", "id": "In Case You Missed It." },
  { "en": "Arti Kata 'T.M.I.'?", "id": "Informasi Yang Terlalu Pribadi." },
  { "en": "Kepanjangan 'T.M.I.'?", "id": "Too Much Information." },
  { "en": "Apa Beda 'Geek' Dan 'Nerd'?", "id": "'Geek' Suka Hobi, 'Nerd' Suka Akademis." },
  { "en": "Apa Beda 'Cheesy' Dan 'Corny'?", "id": "'Cheesy' Norak, 'Corny' Garing." },
  { "en": "Arti Frasa 'Steal The Show'?", "id": "Mencuri Perhatian Utama." },
  { "en": "Arti Frasa 'Stir Up A Hornet's Nest'?", "id": "Membuat Banyak Orang Marah." },
  { "en": "Arti Frasa 'Straw That Broke The Camel's Back'?", "id": "Masalah Kecil Terakhir Yang Jadi Pemicu." },
  { "en": "Arti Frasa 'Take A Backseat'?", "id": "Mengambil Peran Yang Kurang Penting." },
  { "en": "Arti Frasa 'Take For Granted'?", "id": "Menganggap Remeh Sesuatu." },
  { "en": "Arti Frasa 'Take With A Grain Of Salt'?", "id": "Jangan Percaya Sepenuhnya." },
  { "en": "Arti Frasa 'That Ship Has Sailed'?", "id": "Kesempatannya Sudah Lewat." },
  { "en": "Arti Frasa 'The Last Word'?", "id": "Keputusan Atau Pendapat Final." },
  { "en": "Arti Frasa 'There Are Other Fish In The Sea'?", "id": "Masih Banyak Pilihan Lain." },
  { "en": "Arti Frasa 'Think Outside The Box'?", "id": "Berpikir Kreatif." },
  { "en": "Arti Frasa 'Third Time's A Charm'?", "id": "Percobaan Ketiga Akan Berhasil." },
  { "en": "Arti Frasa 'Throw In The Sponge'?", "id": "Menyerah." },
  { "en": "Arti Frasa 'Thumb Your Nose At'?", "id": "Menunjukkan Sikap Tidak Hormat." },
  { "en": "Arti Frasa 'Tie The Knot'?", "id": "Menikah." },
  { "en": "Arti Frasa 'Tongue-In-Cheek'?", "id": "Tidak Serius, Hanya Bercanda." },
  { "en": "Arti Frasa 'Too Many Chiefs And Not Enough Indians'?", "id": "Terlalu Banyak Pemimpin, Sedikit Pekerja." },
  { "en": "Arti Frasa 'Turn A Blind Eye'?", "id": "Pura-Pura Tidak Melihat." },
  { "en": "Arti Frasa 'Twenty-Three Skidoo'?", "id": "Pergi Cepat-Cepat (Slang Tua)." },
  { "en": "Arti Frasa 'Under Your Belt'?", "id": "Pengalaman Yang Sudah Dimiliki." },
  { "en": "Arti Frasa 'Up To Speed'?", "id": "Memiliki Informasi Terbaru." },
  { "en": "Arti Frasa 'Uphill Battle'?", "id": "Perjuangan Yang Sangat Sulit." },
  { "en": "Arti Frasa 'Wag The Dog'?", "id": "Mengalihkan Isu." },
  { "en": "Arti Frasa 'Water Under The Bridge'?", "id": "Masalah Lalu Yang Sudah Dilupakan." },
  { "en": "Arti Frasa 'Weakest Link'?", "id": "Bagian Paling Lemah." },
  { "en": "Arti Frasa 'White Elephant'?", "id": "Barang Mahal Tapi Tidak Berguna." },
  { "en": "Arti Frasa 'Wild Goose Chase'?", "id": "Pengejaran Yang Sia-Sia." },
  { "en": "Arti Frasa 'Word Of Mouth'?", "id": "Dari Mulut Ke Mulut." },
  { "en": "Arti Frasa 'You Scratch My Back, I'll Scratch Yours'?", "id": "Saling Membantu Demi Keuntungan." },
  { "en": "Arti Frasa 'Your Call'?", "id": "Keputusan Ada Di Tanganmu." },
  { "en": "Arti Frasa 'Zero In On'?", "id": "Fokus Penuh Pada Sesuatu." },
  { "en": "Arti Frasa 'A Snowball's Chance In Hell'?", "id": "Tidak Ada Harapan Sama Sekali." },
  { "en": "Arti Frasa 'Ahead Of The Curve'?", "id": "Lebih Maju Dari Yang Lain." },
  { "en": "Arti Frasa 'Knock Your Socks Off'?", "id": "Membuat Sangat Terkesan." },
  { "en": "Arti Frasa 'Let Bygones Be Bygones'?", "id": "Lupakan Masalah Yang Lalu." },
  { "en": "Arti Frasa 'Make No Bones About It'?", "id": "Menyatakan Sesuatu Secara Terus Terang." },
  { "en": "Arti Frasa 'Mum's The Word'?", "id": "Jangan Beri Tahu Siapa-Siapa." },
  { "en": "Arti Frasa 'Murphy's Law'?", "id": "Apapun Yang Bisa Salah, Akan Salah." },
  { "en": "Arti Frasa 'Never Bite The Hand That Feeds You'?", "id": "Jangan Menyakiti Yang Membantumu." },
  { "en": "Arti Frasa 'On The Ropes'?", "id": "Dalam Keadaan Sulit, Hampir Kalah." },
  { "en": "Arti Frasa 'Par For The Course'?", "id": "Sesuatu Yang Normal Atau Diharapkan." },
  { "en": "Arti Frasa 'Put All Your Cards On The Table'?", "id": "Jujur Dan Terbuka Sepenuhnya." },
  { "en": "Arti Frasa 'Put The Cat Among The Pigeons'?", "id": "Menyebabkan Kekacauan Atau Masalah." },
  { "en": "Arti Frasa 'Right Off The Bat'?", "id": "Seketika, Sejak Awal." },
  { "en": "Arti Frasa 'Run Rings Around Someone'?", "id": "Jauh Lebih Unggul Dari Seseorang." },
  { "en": "Arti Frasa 'Salt Of The Earth'?", "id": "Orang Yang Sangat Baik Dan Jujur." },
  { "en": "Arti Frasa 'See Red'?", "id": "Menjadi Sangat Marah." },
  { "en": "Arti Frasa 'Shape Up Or Ship Out'?", "id": "Perbaiki Diri Atau Pergi." },
  { "en": "Arti Frasa 'Shoot From The Hip'?", "id": "Berbicara Tanpa Berpikir Panjang." },
  { "en": "Arti Frasa 'Sick As A Dog'?", "id": "Sangat Sakit." },
  { "en": "Arti Frasa 'Sink Your Teeth Into'?", "id": "Benar-Benar Terlibat Dalam Sesuatu." },
  { "en": "Arti Frasa 'Skating On Thin Ice'?", "id": "Melakukan Sesuatu Yang Sangat Berisiko." },
  { "en": "Arti Frasa 'Stand Your Ground'?", "id": "Mempertahankan Pendapat Atau Posisi." },
  { "en": "Arti Frasa 'Take A Hike'?", "id": "Pergi Sana! (Kasar)." },
  { "en": "Arti Frasa 'Take Five'?", "id": "Istirahat Sebentar." },
  { "en": "Arti Frasa 'The Stigma'?", "id": "Tanda Negatif Sosial." },
  { "en": "Arti Frasa 'Think Tank'?", "id": "Kelompok Ahli Pemberi Ide." },
  { "en": "Arti Frasa 'Throw Someone Under The Bus'?", "id": "Mengorbankan Seseorang Demi Diri Sendiri." },
  { "en": "Arti Frasa 'Tickled Pink'?", "id": "Sangat Senang." },
  { "en": "Arti Frasa 'Tie Up Loose Ends'?", "id": "Menyelesaikan Detail-Detail Terakhir." },
  { "en": "Arti Frasa 'Toot Your Own Horn'?", "id": "Menyombongkan Diri." },
  { "en": "Arti Frasa 'Touch And Go'?", "id": "Situasi Yang Tidak Pasti." },
  { "en": "Arti Frasa 'Turn Over A New Leaf'?", "id": "Memulai Awal Yang Baru." },
  { "en": "Arti Frasa 'Under The Table'?", "id": "Secara Ilegal Atau Rahasia." },
  { "en": "Arti Frasa 'Wash Your Hands Of Something'?", "id": "Melepas Tanggung Jawab." },
  { "en": "Arti Frasa 'Watch Your Back'?", "id": "Hati-Hati." },
  { "en": "Arti Frasa 'Water Off A Duck's Back'?", "id": "Tidak Berpengaruh Sama Sekali." },
  { "en": "Arti Frasa 'Wet Behind The Ears'?", "id": "Muda Dan Tidak Berpengalaman." },
  { "en": "Arti Frasa 'What Goes Around, Comes Around'?", "id": "Karma." },
  { "en": "Arti Frasa 'When It Rains, It Pours'?", "id": "Masalah Datang Bertubi-Tubi." },
  { "en": "Arti Frasa 'When Pigs Fly'?", "id": "Sesuatu Yang Tidak Mungkin Terjadi." },
  { "en": "Arti Frasa 'Wild Goose Chase'?", "id": "Pengejaran Yang Sia-Sia." },
  { "en": "Arti Frasa 'With Bells On'?", "id": "Dengan Sangat Antusias." },
  { "en": "Arti Frasa 'X Marks The Spot'?", "id": "Tepat Di Sini Tempatnya." },
  { "en": "Arti Frasa 'You Are What You Eat'?", "id": "Makanan Mempengaruhi Kesehatanmu." },
  { "en": "Arti Frasa 'You Can't Win 'Em All'?", "id": "Kamu Tidak Bisa Selalu Menang." },
  { "en": "Kepanjangan 'B.T.W.'?", "id": "By The Way / Ngomong-Ngomong." },
  { "en": "Kepanjangan 'T.Y.'?", "id": "Thank You / Terima Kasih." },
  { "en": "Kepanjangan 'G.L.'?", "id": "Good Luck / Semoga Beruntung." },
  { "en": "Kepanjangan 'H.F.'?", "id": "Have Fun / Selamat Bersenang-Senang." },
  { "en": "Kepanjangan 'I.R.L.'?", "id": "In Real Life / Di Dunia Nyata." },
  { "en": "Kepanjangan 'F.Y.I.'?", "id": "For Your Information / Sebagai Informasi." },
  { "en": "Kepanjangan 'A.K.A.'?", "id": "Also Known As / Dikenal Juga Sebagai." },
  { "en": "Kepanjangan 'D.I.Y.'?", "id": "Do It Yourself / Kerjakan Sendiri." },
  { "en": "Kepanjangan 'F.A.Q.'?", "id": "Frequently Asked Questions / Pertanyaan Yang Sering Diajukan." },
  { "en": "Kepanjangan 'E.T.A.'?", "id": "Estimated Time of Arrival / Perkiraan Waktu Tiba." },
  { "en": "Kepanjangan 'R.S.V.P.'?", "id": "Répondez S'il Vous Plaît / Harap Balas Undangan." },
  { "en": "Arti Kata 'P.C.'?", "id": "Benar Secara Politik." },
  { "en": "Kepanjangan 'P.C.'?", "id": "Politically Correct." },
  { "en": "Arti Kata 'Whiz'?", "id": "Orang Yang Sangat Hebat." },
  { "en": "Arti Frasa 'Get Canned'?", "id": "Dipecat." },
  { "en": "Arti Frasa 'In The Red'?", "id": "Mengalami Kerugian Finansial." },
  { "en": "Arti Frasa 'In The Black'?", "id": "Menghasilkan Keuntungan Finansial." },
  { "en": "Arti Frasa 'Learn The Ropes'?", "id": "Mempelajari Dasar-Dasar Sesuatu." },
  { "en": "Arti Frasa 'Raise The Bar'?", "id": "Meningkatkan Standar." },
  { "en": "Arti Frasa 'Think On Your Feet'?", "id": "Berpikir Cepat." },
  { "en": "Arti Frasa 'Touch Base'?", "id": "Menghubungi Seseorang Sebentar." },
  { "en": "Arti Frasa 'Word Of Mouth'?", "id": "Informasi Dari Mulut Ke Mulut." },
  { "en": "Arti Frasa 'Get The Axe'?", "id": "Dipecat." },
  { "en": "Arti Frasa 'Punch In'?", "id": "Masuk Kerja (Absen)." },
  { "en": "Arti Frasa 'Punch Out'?", "id": "Pulang Kerja (Absen)." },
  { "en": "Arti Frasa 'Pink Slip'?", "id": "Surat Pemberitahuan PHK." },
  { "en": "Arti Frasa 'Glass Ceiling'?", "id": "Hambatan Tak Terlihat Untuk Promosi." },
  { "en": "Arti Frasa 'Crunch Time'?", "id": "Periode Sibuk Menjelang Tenggat Waktu." },
  { "en": "Arti Frasa 'Dog-Eat-Dog World'?", "id": "Dunia Yang Sangat Kompetitif." },
  { "en": "Arti Frasa 'Eager Beaver'?", "id": "Orang Yang Sangat Bersemangat." },
  { "en": "Arti Frasa 'Go The Extra Mile'?", "id": "Melakukan Usaha Ekstra." },
  { "en": "Arti Frasa 'A Lame Duck'?", "id": "Pejabat Yang Akan Segera Berakhir." },
  { "en": "Arti Frasa 'A Vicious Cycle'?", "id": "Lingkaran Setan." },
  { "en": "Arti Frasa 'All That Jazz'?", "id": "Dan Hal-Hal Sejenisnya." },
  { "en": "Arti Frasa 'Back To The Grind'?", "id": "Kembali Bekerja Rutin." },
  { "en": "Arti Frasa 'Behind The Scenes'?", "id": "Di Balik Layar." },
  { "en": "Arti Frasa 'Big Cheese'?", "id": "Orang Penting, Bos." },
  { "en": "Arti Frasa 'Bite Off More Than You Can Chew'?", "id": "Mengambil Tugas Terlalu Banyak." },
  { "en": "Arti Frasa 'Blow A Fuse'?", "id": "Menjadi Sangat Marah." },
  { "en": "Arti Frasa 'Brownie Points'?", "id": "Poin Plus, Pujian." },
  { "en": "Arti Frasa 'Call It A Night'?", "id": "Menyudahi Aktivitas Malam Ini." },
  { "en": "Arti Frasa 'Corner The Market'?", "id": "Mendominasi Pasar." },
  { "en": "Arti Frasa 'Draw The Line'?", "id": "Menetapkan Batas." },
  { "en": "Arti Frasa 'Easier Said Than Done'?", "id": "Lebih Mudah Diucapkan Daripada Dilakukan." },
  { "en": "Arti Frasa 'Get Your Ducks In A Row'?", "id": "Mengatur Segalanya Dengan Rapi." },
  { "en": "Arti Frasa 'Give It Your Best Shot'?", "id": "Berikan Usaha Terbaikmu." },
  { "en": "Arti Frasa 'Go Down The Rabbit Hole'?", "id": "Tersesat Dalam Topik Yang Rumit." },
  { "en": "Arti Frasa 'Go For It'?", "id": "Lakukan Saja, Jangan Ragu." },
  { "en": "Arti Frasa 'Go The Distance'?", "id": "Menyelesaikan Sesuatu Sampai Akhir." },
  { "en": "Arti Frasa 'Good Samaritan'?", "id": "Orang Baik Yang Suka Menolong." },
  { "en": "Arti Frasa 'Grass Is Always Greener On The Other Side'?", "id": "Rumput Tetangga Selalu Lebih Hijau." },
  { "en": "Arti Frasa 'Great Minds Think alike'?", "id": "Pemikiran Hebat Seringkali Sama." },
  { "en": "Arti Frasa 'Grind To A Halt'?", "id": "Berhenti Total Secara Perlahan." },
  { "en": "Arti Frasa 'Gung Ho'?", "id": "Sangat Antusias Dan Bersemangat." },
  { "en": "Arti Frasa 'Hairy Situation'?", "id": "Situasi Yang Berbahaya Atau Sulit." },
  { "en": "Arti Frasa 'Hand To Mouth'?", "id": "Hidup Pas-Pasan." },
  { "en": "Arti Frasa 'Have A Change Of Heart'?", "id": "Berubah Pikiran." },
  { "en": "Arti Frasa 'Have The Guts'?", "id": "Punya Keberanian." },
  { "en": "Arti Frasa 'Heads Up'?", "id": "Peringatan Atau Informasi Awal." },
  { "en": "Arti Frasa 'High And Mighty'?", "id": "Sangat Sombong." },
  { "en": "Arti Frasa 'Hit A Snag'?", "id": "Menemui Masalah Tak Terduga." },
  { "en": "Arti Frasa 'Hit Below The Belt'?", "id": "Serangan Yang Tidak Adil." },
  { "en": "Arti Frasa 'Hit The Ceiling'?", "id": "Menjadi Sangat Marah." },
  { "en": "Arti Frasa 'Iffy'?", "id": "Mencurigakan Atau Tidak Pasti." },
  { "en": "Arti Frasa 'In A Bind'?", "id": "Dalam Situasi Sulit." },
  { "en": "Arti Frasa 'In A Fix'?", "id": "Dalam Masalah." },
  { "en": "Arti Frasa 'In Stitches'?", "id": "Tertawa Terpingkal-Pingkal." },
  { "en": "Arti Frasa 'In The Limelight'?", "id": "Menjadi Pusat Perhatian." },
  { "en": "Arti Frasa 'In The Loop'?", "id": "Selalu Mendapat Informasi Terbaru." },
  { "en": "Arti Frasa 'Out Of The Loop'?", "id": "Tidak Tahu Informasi Terbaru." },
  { "en": "Arti Frasa 'It's A Wash'?", "id": "Impas, Tidak Ada Untung Rugi." },
  { "en": "Arti Frasa 'Itchy Feet'?", "id": "Keinginan Kuat Untuk Bepergian." },
  { "en": "Arti Frasa 'Jaw-Dropping'?", "id": "Sangat Mengagumkan." },
  { "en": "Arti Frasa 'Jelly'?", "id": "Cemburu." },
  { "en": "Arti Frasa 'John Doe'?", "id": "Nama Untuk Pria Tak Dikenal." },
  { "en": "Arti Frasa 'Jane Doe'?", "id": "Nama Untuk Wanita Tak Dikenal." },
  { "en": "Arti Frasa 'Keep A Lid On It'?", "id": "Menjaga Sesuatu Tetap Rahasia." },
  { "en": "Arti Frasa 'Keep Your Shirt On'?", "id": "Sabar, Jangan Marah." },
  { "en": "Arti Frasa 'Kick Back'?", "id": "Bersantai." },
  { "en": "Arti Frasa 'Knock It Off'?", "id": "Hentikan Itu!" },
  { "en": "Arti Frasa 'Know-It-All'?", "id": "Orang Yang Sok Tahu." },
  { "en": "Arti Frasa 'Last Ditch Effort'?", "id": "Upaya Terakhir Saat Hampir Gagal." },
  { "en": "Arti Frasa 'Lay Low'?", "id": "Bersembunyi Atau Tidak Menonjolkan Diri." },
  { "en": "Arti Frasa 'Lion's Share'?", "id": "Bagian Terbesar." },
  { "en": "Arti Frasa 'Lip Service'?", "id": "Omong Doang, Tidak Ada Tindakan." },
  { "en": "Arti Frasa 'Live It Up'?", "id": "Bersenang-Senang Sepuasnya." },
  { "en": "Arti Frasa 'Long Face'?", "id": "Wajah Sedih." },
  { "en": "Arti Frasa 'Look Down On'?", "id": "Memandang Rendah Seseorang." },
  { "en": "Arti Frasa 'Look Up To'?", "id": "Menghormati Atau Mengagumi Seseorang." },
  { "en": "Arti Frasa 'Lose Your Marbles'?", "id": "Menjadi Gila." },
  { "en": "Arti Frasa 'Low Blow'?", "id": "Komentar Atau Tindakan Kejam." },
  { "en": "Arti Frasa 'Make A Killing'?", "id": "Menghasilkan Banyak Uang Dengan Cepat." },
  { "en": "Arti Frasa 'Make Waves'?", "id": "Membuat Masalah Atau Perubahan Besar." },
  { "en": "Kepanjangan 'A.W.O.L.'?", "id": "Absent Without Leave / Mangkir Tanpa Izin." },
  { "en": "Kepanjangan 'D.O.A.'?", "id": "Dead On Arrival / Tiba Dalam Keadaan Mati." },
  { "en": "Kepanjangan 'P.S.'?", "id": "Postscript / Catatan Tambahan." },
  { "en": "Kepanjangan 'P.P.S.'?", "id": "Post-Postscript / Catatan Tambahan Kedua." },
  { "en": "Kepanjangan 'R.S.V.P.'?", "id": "Répondez S'il Vous Plaît / Harap Balas Undangan." },
  { "en": "Arti Kata 'NSFW'?", "id": "Tidak Aman Untuk Dilihat Di Kantor." },
  { "en": "Kepanjangan 'NSFW'?", "id": "Not Safe For Work / Tidak Aman Untuk Kerja." },
  { "en": "Arti Kata 'NSFL'?", "id": "Tidak Aman Untuk Kehidupan (Mengerikan)." },
  { "en": "Kepanjangan 'NSFL'?", "id": "Not Safe For Life / Tidak Aman Untuk Kehidupan." },
  { "en": "Arti Frasa 'Mind Over Matter'?", "id": "Kekuatan Pikiran Mengalahkan Materi." },
  { "en": "Arti Frasa 'Money To Burn'?", "id": "Punya Uang Sangat Banyak." },
  { "en": "Arti Frasa 'More Bang For Your Buck'?", "id": "Lebih Untung." },
  { "en": "Arti Frasa 'Needless To Say'?", "id": "Sudah Jelas." },
  { "en": "Arti Frasa 'Nitty-Gritty'?", "id": "Detail-Detail Penting." },
  { "en": "Arti Frasa 'No Strings Attached'?", "id": "Tanpa Ikatan Atau Kewajiban." },
  { "en": "Arti Frasa 'Not My Day'?", "id": "Hari Yang Sial Bagiku." },
  { "en": "Arti Frasa 'Nutty As A Fruitcake'?", "id": "Sangat Aneh Atau Gila." },
  { "en": "Arti Frasa 'Off Base'?", "id": "Benar-Benar Salah." },
  { "en": "Arti Frasa 'Old As The Hills'?", "id": "Sangat Tua." },
  { "en": "Arti Frasa 'On The Back Burner'?", "id": "Dikesampingkan Untuk Sementara." },
  { "en": "Arti Frasa 'On The Brink Of'?", "id": "Di Ambang Sesuatu." },
  { "en": "Arti Frasa 'On The Dot'?", "id": "Tepat Waktu." },
  { "en": "Arti Frasa 'On The Fritz'?", "id": "Rusak." },
  { "en": "Arti Frasa 'On The Hook'?", "id": "Bertanggung Jawab." },
  { "en": "Arti Frasa 'Off The Hook'?", "id": "Lolos Dari Tanggung Jawab." },
  { "en": "Arti Frasa 'On The Mend'?", "id": "Sedang Dalam Proses Pemulihan." },
  { "en": "Arti Frasa 'On The Same Wavelength'?", "id": "Berpikir Dengan Cara Yang Sama." },
  { "en": "Arti Frasa 'Out Of Your Mind'?", "id": "Gila." },
  { "en": "Arti Frasa 'Out On A Limb'?", "id": "Mengambil Risiko." },
  { "en": "Arti Frasa 'Over My Dead Body'?", "id": "Langkahi Dulu Mayatku." },
  { "en": "Arti Frasa 'Paint The Town Red'?", "id": "Pergi Keluar Untuk Bersenang-Senang." },
  { "en": "Arti Frasa 'Panic Stations'?", "id": "Situasi Panik." },
  { "en": "Arti Frasa 'Paper Over The Cracks'?", "id": "Menutupi Masalah." },
  { "en": "Arti Frasa 'Party Animal'?", "id": "Orang Yang Sangat Suka Pesta." },
  { "en": "Arti Frasa 'Pass The Hat Around'?", "id": "Mengumpulkan Uang Sumbangan." },
  { "en": "Arti Frasa 'Pay Through The Nose'?", "id": "Membayar Harga Sangat Mahal." },
  { "en": "Arti Frasa 'Pecking Order'?", "id": "Urutan Pangkat Atau Hierarki." },
  { "en": "Arti Frasa 'Pep Talk'?", "id": "Pidato Untuk Memberi Semangat." },
  { "en": "Arti Frasa 'Pick Your Brain'?", "id": "Bertanya Untuk Mendapat Informasi." },
  { "en": "Arti Frasa 'Pipe Dream'?", "id": "Impian Yang Mustahil." },
  { "en": "Arti Frasa 'Play It By Ear'?", "id": "Melihat Situasi Nanti (Improvisasi)." },
  { "en": "Arti Frasa 'Point Of No Return'?", "id": "Titik Di Mana Tidak Bisa Kembali." },
  { "en": "Arti Frasa 'Poke Your Nose Into'?", "id": "Ikut Campur Urusan Orang Lain." },
  { "en": "Arti Frasa 'Pot Calling The Kettle Black'?", "id": "Maling Teriak Maling." },
  { "en": "Arti Frasa 'Pull A Fast One'?", "id": "Menipu Seseorang." },
  { "en": "Arti Frasa 'Pull Out All The Stops'?", "id": "Melakukan Segala Cara." },
  { "en": "Arti Frasa 'Put A Damper On'?", "id": "Merusak Suasana." },
  { "en": "Arti Frasa 'Put On Your Thinking Cap'?", "id": "Berpikir Keras." },
  { "en": "Arti Frasa 'Put Someone On The Spot'?", "id": "Membuat Seseorang Dalam Posisi Sulit." },
  { "en": "Arti Frasa 'Put Up Or Shut Up'?", "id": "Buktikan Atau Diam." },
  { "en": "Arti Frasa 'Put Your Foot Down'?", "id": "Bersikap Tegas." },
  { "en": "Arti Frasa 'Queer The Pitch'?", "id": "Merusak Rencana Seseorang." },
  { "en": "Arti Frasa 'Quick On The Uptake'?", "id": "Cepat Mengerti." },
  { "en": "Arti Frasa 'Rain On My Parade'?", "id": "Merusak Momen Bahagiaku." },
  { "en": "Arti Frasa 'Rake In The Moolah'?", "id": "Menghasilkan Banyak Uang." },
  { "en": "Arti Frasa 'Rant And Rave'?", "id": "Mengomel Dengan Marah." },
  { "en": "Arti Frasa 'Read My Lips'?", "id": "Dengarkan Baik-Baik." },
  { "en": "Arti Frasa 'Red Herring'?", "id": "Sesuatu Yang Mengalihkan Perhatian." },
  { "en": "Arti Frasa 'Red-Letter Day'?", "id": "Hari Yang Sangat Penting." },
  { "en": "Arti Frasa 'Ring A Bell'?", "id": "Terdengar Familiar." },
  { "en": "Arti Frasa 'Rub Salt In The Wound'?", "id": "Membuat Penderitaan Lebih Buruk." },
  { "en": "Arti Frasa 'Rule Of Thumb'?", "id": "Aturan Praktis." },
  { "en": "Arti Frasa 'Run A Tight Ship'?", "id": "Mengelola Sesuatu Dengan Sangat Disiplin." },
  { "en": "Arti Frasa 'Run Aground'?", "id": "Gagal Karena Kehabisan Uang." },
  { "en": "Arti Frasa 'Run For Your Money'?", "id": "Memberi Persaingan Yang Ketat." },
  { "en": "Arti Frasa 'Run Of The Mill'?", "id": "Biasa Saja, Tidak Istimewa." },
  { "en": "Arti Frasa 'Sail Through Something'?", "id": "Melewati Sesuatu Dengan Mudah." },
  { "en": "Arti Frasa 'Say Your Piece'?", "id": "Katakan Pendapatmu." },
  { "en": "Arti Frasa 'Scare The Living Daylights Out Of Someone'?", "id": "Membuat Seseorang Sangat Ketakutan." },
  { "en": "Arti Frasa 'Second Guessing'?", "id": "Meragukan Keputusan Yang Sudah Dibuat." },
  { "en": "Arti Frasa 'See Eye To Eye'?", "id": "Setuju Dengan Seseorang." },
  { "en": "Arti Frasa 'Sell Like Hotcakes'?", "id": "Sangat Laris Terjual." },
  { "en": "Arti Frasa 'Set In Stone'?", "id": "Sesuatu Yang Tidak Bisa Diubah." },
  { "en": "Arti Frasa 'Shake In Your Boots'?", "id": "Gemetar Karena Takut." },
  { "en": "Arti Frasa 'Shipshape'?", "id": "Sangat Rapi Dan Teratur." },
  { "en": "Arti Frasa 'Shoot The Messenger'?", "id": "Menyalahkan Pembawa Berita Buruk." },
  { "en": "Arti Frasa 'Show Your True Colors'?", "id": "Menunjukkan Sifat Asli." },
  { "en": "Arti Frasa 'Skeleton In The Closet'?", "id": "Rahasia Memalukan Di Masa Lalu." },
  { "en": "Arti Frasa 'Slippery Slope'?", "id": "Tindakan Awal Yang Berujung Buruk." },
  { "en": "Arti Frasa 'Smell Something Fishy'?", "id": "Mencurigai Sesuatu." },
  { "en": "Arti Frasa 'Speak Volumes'?", "id": "Mengungkapkan Banyak Hal Tanpa Kata." },
  { "en": "Arti Frasa 'Spick And Span'?", "id": "Sangat Bersih Dan Rapi." },
  { "en": "Arti Frasa 'Stand On Your Own Two Feet'?", "id": "Menjadi Mandiri." },
  { "en": "Arti Frasa 'Step Up To The Plate'?", "id": "Mengambil Tanggung Jawab." },
  { "en": "Arti Frasa 'Stool Pigeon'?", "id": "Informan Polisi, Pengkhianat." },
  { "en": "Arti Frasa 'Straight And Narrow'?", "id": "Jalan Hidup Yang Jujur." },
  { "en": "Arti Frasa 'Straw Man Argument'?", "id": "Argumen Palsu Yang Mudah Diserang." },
  { "en": "Arti Frasa 'Strike While The Iron Is Hot'?", "id": "Manfaatkan Kesempatan Selagi Ada." },
  { "en": "Arti Frasa 'Sweep It Under The Rug'?", "id": "Menyembunyikan Masalah." },
  { "en": "Arti Frasa 'Take A Beating'?", "id": "Mengalami Kerugian Besar." },
  { "en": "Arti Frasa 'Take A Hike'?", "id": "Pergi Sana! (Kasar)." },
  { "en": "Arti Frasa 'Take Center Stage'?", "id": "Menjadi Pusat Perhatian." },
  { "en": "Arti Frasa 'Take Five'?", "id": "Istirahat Sebentar." },
  { "en": "Arti Frasa 'Take The Bull By The Horns'?", "id": "Menghadapi Masalah Dengan Berani." },
  { "en": "Arti Frasa 'Take The Cake'?", "id": "Menjadi Yang Paling Ekstrem." },
  { "en": "Arti Frasa 'Take The Fifth'?", "id": "Menolak Untuk Menjawab." },
  { "en": "Arti Frasa 'That's The Way The Cookie Crumbles'?", "id": "Begitulah Kenyataannya." },
  { "en": "Kepanjangan 'I.O.U.'?", "id": "I Owe You / Aku Berhutang Padamu." },
  { "en": "Kepanjangan 'T.B.C.'?", "id": "To Be Continued / Bersambung." },
  { "en": "Arti Kata 'W.I.P.'?", "id": "Sedang Dalam Pengerjaan." },
  { "en": "Kepanjangan 'W.I.P.'?", "id": "Work In Progress." },
  { "en": "Arti Kata 'A.S.A.I.K.'?", "id": "Sepengetahuanku." },
  { "en": "Kepanjangan 'A.S.A.I.K.'?", "id": "As Soon As I Know." },
  { "en": "Arti Kata 'F.A.C.T.S.'?", "id": "Ekspresi Untuk Menyatakan Kebenaran." },
  { "en": "Arti Kata 'Simmer Down'?", "id": "Tenang Dulu." },
  { "en": "Arti Kata 'Goofball'?", "id": "Orang Yang Konyol Dan Lucu." },
  { "en": "Arti Kata 'Dweeb'?", "id": "Orang Aneh Dan Kutu Buku." },
  { "en": "Arti Kata 'Fender-Bender'?", "id": "Tabrakan Mobil Ringan." },
  { "en": "Arti Kata 'Freeloader'?", "id": "Orang Yang Suka Gratisan." },
  { "en": "Arti Kata 'Fuzz'?", "id": "Polisi." },
  { "en": "Arti Kata 'Gag'?", "id": "Lelucon Atau Candaan." },
  { "en": "Arti Kata 'Go-Getter'?", "id": "Orang Yang Sangat Ambisius." },
  { "en": "Arti Kata 'Gold Digger'?", "id": "Orang Yang Mengejar Uang." },
  { "en": "Arti Kata 'Goody Two-Shoes'?", "id": "Orang Yang Sok Suci." },
  { "en": "Arti Frasa 'To Throw A Wrench In'?", "id": "Menggagalkan Rencana." },
  { "en": "Arti Frasa 'Tip Of The Iceberg'?", "id": "Hanya Bagian Kecil Dari Masalah." },
  { "en": "Arti Frasa 'Top Dog'?", "id": "Orang Yang Paling Berkuasa." },
  { "en": "Arti Frasa 'Touch-And-Go'?", "id": "Situasi Yang Tidak Pasti." },
  { "en": "Arti Frasa 'Turn A Deaf Ear'?", "id": "Pura-Pura Tidak Mendengar." },
  { "en": "Arti Frasa 'Turn On A Dime'?", "id": "Berubah Arah Dengan Sangat Cepat." },
  { "en": "Arti Frasa 'Turn The Tables'?", "id": "Membalikkan Keadaan." },
  { "en": "Arti Frasa 'Two Cents'?", "id": "Pendapat Pribadi." },
  { "en": "Arti Frasa 'Two-Faced'?", "id": "Bermuka Dua." },
  { "en": "Arti Frasa 'Under The Radar'?", "id": "Tidak Terdeteksi Atau Diperhatikan." },
  { "en": "Arti Frasa 'Under Wraps'?", "id": "Dijaga Tetap Rahasia." },
  { "en": "Arti Frasa 'Until The Cows Come Home'?", "id": "Untuk Waktu Yang Sangat Lama." },
  { "en": "Arti Frasa 'Uptight'?", "id": "Terlalu Kaku Atau Tegang." },
  { "en": "Arti Frasa 'Vanishes Into Thin Air'?", "id": "Menghilang Tanpa Jejak." },
  { "en": "Arti Frasa 'Walk On Eggshells'?", "id": "Berjalan Sangat Hati-Hati." },
  { "en": "Arti Frasa 'Weak At The Knees'?", "id": "Lutut Terasa Lemas (Karena Kagum)." },
  { "en": "Arti Frasa 'What Makes You Tick'?", "id": "Apa Yang Membuatmu Termotivasi." },
  { "en": "Arti Frasa 'When Hell Freezes Over'?", "id": "Tidak Akan Pernah Terjadi." },
  { "en": "Arti Frasa 'Wiggle Room'?", "id": "Ruang Untuk Fleksibilitas." },
  { "en": "Arti Frasa 'Wine And Dine'?", "id": "Menjamu Seseorang Dengan Mewah." },
  { "en": "Arti Frasa 'Climb On The Bandwagon'?", "id": "Ikut-Ikutan Tren." },
  { "en": "Arti Frasa 'Cock And Bull Story'?", "id": "Cerita Omong Kosong Yang Tidak Masuk Akal." },
  { "en": "Arti Frasa 'A Cold Day In Hell'?", "id": "Sesuatu Yang Tidak Akan Pernah Terjadi." },
  { "en": "Arti Frasa 'Come Full Circle'?", "id": "Kembali Ke Titik Awal." },
  { "en": "Arti Frasa 'Cook The Books'?", "id": "Memalsukan Laporan Keuangan." },
  { "en": "Arti Frasa 'Cool As A Cucumber'?", "id": "Sangat Tenang Dan Rileks." },
  { "en": "Arti Frasa 'Cross To Bear'?", "id": "Beban Berat Yang Harus Ditanggung." },
  { "en": "Arti Frasa 'Crunch The Numbers'?", "id": "Melakukan Banyak Perhitungan." },
  { "en": "Arti Frasa 'Cry Wolf'?", "id": "Memberi Peringatan Palsu." },
  { "en": "Arti Frasa 'Cup Of Joe'?", "id": "Secangkir Kopi." },
  { "en": "Arti Frasa 'Cut From The Same Cloth'?", "id": "Sangat Mirip (Karakter)." },
  { "en": "Arti Frasa 'Dark Horse'?", "id": "Kuda Hitam, Pemenang Tak Terduga." },
  { "en": "Arti Frasa 'Day In And Day Out'?", "id": "Setiap Hari Tanpa Henti." },
  { "en": "Arti Frasa 'Dead As A Doornail'?", "id": "Benar-Benar Mati." },
  { "en": "Arti Frasa 'Dime A Dozen'?", "id": "Sesuatu Yang Sangat Umum." },
  { "en": "Arti Frasa 'Dressed To The Nines'?", "id": "Berpakaian Sangat Rapi Dan Modis." },
  { "en": "Arti Frasa 'Drink Like A Fish'?", "id": "Minum Alkohol Sangat Banyak." },
  { "en": "Arti Frasa 'Drop Like Flies'?", "id": "Gugur Atau Menyerah Dalam Jumlah Besar." },
  { "en": "Arti Frasa 'Dutch Courage'?", "id": "Keberanian Palsu Akibat Alkohol." },
  { "en": "Arti Frasa 'Early Bird Catches The Worm'?", "id": "Bangun Pagi Banyak Rezeki." },
  { "en": "Arti Frasa 'Easier Said Than Done'?", "id": "Lebih Mudah Dikatakan Daripada Dilakukan." },
  { "en": "Arti Frasa 'Eat Like A Bird'?", "id": "Makan Sangat Sedikit." },
  { "en": "Arti Frasa 'Eat Like A Horse'?", "id": "Makan Sangat Banyak." },
  { "en": "Arti Frasa 'Egg On Your Face'?", "id": "Terlihat Bodoh Atau Malu." },
  { "en": "Arti Frasa 'Elbow Room'?", "id": "Ruang Gerak Yang Cukup." },
  { "en": "Arti Frasa 'End Of Your Tether'?", "id": "Di Batas Kesabaran." },
  { "en": "Arti Frasa 'Everything But The Kitchen Sink'?", "id": "Membawa Hampir Segalanya." },
  { "en": "Arti Frasa 'Eye Candy'?", "id": "Sesuatu Yang Enak Dilihat." },
  { "en": "Arti Frasa 'Face The Music'?", "id": "Menghadapi Konsekuensi." },
  { "en": "Arti Frasa 'Fall Off The Wagon'?", "id": "Kembali Ke Kebiasaan Buruk." },
  { "en": "Arti Frasa 'Fan The Flames'?", "id": "Membuat Situasi Buruk Menjadi Lebih Buruk." },
  { "en": "Arti Frasa 'Feast Your Eyes On'?", "id": "Menikmati Pemandangan Sesuatu." },
  { "en": "Arti Frasa 'Feel Blue'?", "id": "Merasa Sedih." },
  { "en": "Arti Frasa 'Fender Bender'?", "id": "Kecelakaan Mobil Ringan." },
  { "en": "Arti Frasa 'Fight Tooth And Nail'?", "id": "Berjuang Mati-Matian." },
  { "en": "Arti Frasa 'Find Your Voice'?", "id": "Berani Mengungkapkan Pendapat." },
  { "en": "Arti Frasa 'Finger-Licking Good'?", "id": "Sangat Enak." },
  { "en": "Arti Frasa 'Fish Out Of Water'?", "id": "Merasa Canggung Di Lingkungan Baru." },
  { "en": "Arti Frasa 'Fit Like A Glove'?", "id": "Ukurannya Sangat Pas." },
  { "en": "Arti Frasa 'Flash In The Pan'?", "id": "Kesuksesan Sesaat." },
  { "en": "Arti Frasa 'Flea In Your Ear'?", "id": "Teguran Keras." },
  { "en": "Arti Frasa 'Flesh And Blood'?", "id": "Keluarga Sendiri." },
  { "en": "Arti Frasa 'Flip The Bird'?", "id": "Mengacungkan Jari Tengah." },
  { "en": "Arti Frasa 'Foam At The Mouth'?", "id": "Sangat Marah." },
  { "en": "Arti Frasa 'Follow Your Nose'?", "id": "Jalan Lurus Terus." },
  { "en": "Arti Frasa 'Food For Thought'?", "id": "Bahan Untuk Dipikirkan." },
  { "en": "Arti Frasa 'Foot The Bill'?", "id": "Membayar Tagihan." },
  { "en": "Arti Frasa 'For Ages'?", "id": "Untuk Waktu Yang Sangat Lama." },
  { "en": "Arti Frasa 'From Rags To Riches'?", "id": "Dari Miskin Menjadi Kaya." },
  { "en": "Arti Frasa 'Full Of Hot Air'?", "id": "Penuh Omong Kosong." },
  { "en": "Arti Frasa 'Funny Farm'?", "id": "Rumah Sakit Jiwa." },
  { "en": "Kepanjangan 'FWIW'?", "id": "For What It's Worth / Sekadar Info Saja." },
  { "en": "Kepanjangan 'WFH'?", "id": "Working From Home / Kerja Dari Rumah." },
  { "en": "Kepanjangan 'BOLO'?", "id": "Be On The Lookout / Harap Waspada." },
  { "en": "Kepanjangan 'FUBAR'?", "id": "Fouled Up Beyond All Recognition / Rusak Parah." },
  { "en": "Kepanjangan 'SNAFU'?", "id": "Situation Normal: All Fouled Up / Situasi Kacau Seperti Biasa." },
  { "en": "Arti Frasa 'Get A Word In'?", "id": "Mendapat Kesempatan Berbicara." },
  { "en": "Arti Frasa 'Get Carried Away'?", "id": "Terlalu Antusias, Kehilangan Kontrol." },
  { "en": "Arti Frasa 'Get Off My Back'?", "id": "Berhenti Menggangguku." },
  { "en": "Arti Frasa 'Get The Drift'?", "id": "Mengerti Maksudnya." },
  { "en": "Arti Frasa 'Get Wind Of'?", "id": "Mendengar Rumor Tentang." },
  { "en": "Arti Frasa 'Gild The Lily'?", "id": "Menghias Sesuatu Yang Sudah Indah." },
  { "en": "Arti Frasa 'Give Someone A Piece Of Your Mind'?", "id": "Memarahi Seseorang." },
  { "en": "Arti Frasa 'Give Someone The Slip'?", "id": "Lolos Dari Seseorang." },
  { "en": "Arti Frasa 'Go Against The Flow'?", "id": "Melawan Arus." },
  { "en": "Arti Frasa 'Go All Out'?", "id": "Melakukan Sesuatu Dengan Maksimal." },
  { "en": "Arti Frasa 'Go Down Like A Lead Balloon'?", "id": "Gagal Total (Untuk Lelucon/Ide)." },
  { "en": "Arti Frasa 'Go For A Song'?", "id": "Dijual Sangat Murah." },
  { "en": "Arti Frasa 'Go Haywire'?", "id": "Menjadi Rusak Atau Kacau." },
  { "en": "Arti Frasa 'Go Off On A Tangent'?", "id": "Tiba-Tiba Membicarakan Topik Lain." },
  { "en": "Arti Frasa 'Go To The Dogs'?", "id": "Menjadi Sangat Buruk." },
  { "en": "Arti Frasa 'Goody Two-Shoes'?", "id": "Orang Yang Sok Suci." },
  { "en": "Arti Frasa 'Grass Roots'?", "id": "Akar Rumput, Rakyat Biasa." },
  { "en": "Arti Frasa 'Gravy Train'?", "id": "Pekerjaan Mudah Dengan Gaji Besar." },
  { "en": "Arti Frasa 'Grease Monkey'?", "id": "Montir." },
  { "en": "Arti Frasa 'Green-Eyed Monster'?", "id": "Rasa Cemburu." },
  { "en": "Arti Frasa 'Grist For The Mill'?", "id": "Sesuatu Yang Bisa Dimanfaatkan." },
  { "en": "Arti Frasa 'Guts Feeling'?", "id": "Firasat." },
  { "en": "Arti Frasa 'Hairy'?", "id": "Berbahaya Atau Menakutkan." },
  { "en": "Arti Frasa 'Hanky-Panky'?", "id": "Perilaku Tidak Jujur Atau Asusila." },
  { "en": "Arti Frasa 'Happy-Go-Lucky'?", "id": "Ceria Dan Tanpa Beban." },
  { "en": "Arti Frasa 'Hard As Nails'?", "id": "Sangat Keras Dan Tidak Emosional." },
  { "en": "Arti Frasa 'Have A Field Day'?", "id": "Sangat Menikmati Sesuatu." },
  { "en": "Arti Frasa 'Have An Egg On Your Face'?", "id": "Terlihat Bodoh Atau Malu." },
  { "en": "Arti Frasa 'Have Someone's Back'?", "id": "Mendukung Seseorang." },
  { "en": "Arti Frasa 'Have Your Work Cut Out For You'?", "id": "Menghadapi Tugas Yang Sangat Sulit." },
  { "en": "Arti Frasa 'Heads Up'?", "id": "Peringatan Awal." },
  { "en": "Arti Frasa 'Heart And Soul'?", "id": "Dengan Sepenuh Hati." },
  { "en": "Arti Frasa 'Heart Missed A Beat'?", "id": "Sangat Terkejut Atau Gugup." },
  { "en": "Arti Frasa 'Heavy Hitter'?", "id": "Orang Penting Dan Berpengaruh." },
  { "en": "Arti Frasa 'High Five'?", "id": "Tos." },
  { "en": "Arti Frasa 'Highway Robbery'?", "id": "Harga Yang Terlalu Mahal." },
  { "en": "Arti Frasa 'Hit A Wall'?", "id": "Menemui Jalan Buntu." },
  { "en": "Arti Frasa 'Climb On The Bandwagon'?", "id": "Ikut-Ikutan Tren." },
  { "en": "Arti Frasa 'Cock And Bull Story'?", "id": "Cerita Omong Kosong Yang Tidak Masuk Akal." },
  { "en": "Arti Frasa 'A Cold Day In Hell'?", "id": "Sesuatu Yang Tidak Akan Pernah Terjadi." },
  { "en": "Arti Frasa 'Come Full Circle'?", "id": "Kembali Ke Titik Awal." },
  { "en": "Arti Frasa 'Cook The Books'?", "id": "Memalsukan Laporan Keuangan." },
  { "en": "Arti Frasa 'Cool As A Cucumber'?", "id": "Sangat Tenang Dan Rileks." },
  { "en": "Arti Frasa 'Cross To Bear'?", "id": "Beban Berat Yang Harus Ditanggung." },
  { "en": "Arti Frasa 'Crunch The Numbers'?", "id": "Melakukan Banyak Perhitungan." },
  { "en": "Arti Frasa 'Cry Wolf'?", "id": "Memberi Peringatan Palsu." },
  { "en": "Arti Frasa 'Cup Of Joe'?", "id": "Secangkir Kopi." },
  { "en": "Arti Frasa 'Cut From The Same Cloth'?", "id": "Sangat Mirip (Karakter)." },
  { "en": "Arti Frasa 'Dark Horse'?", "id": "Kuda Hitam, Pemenang Tak Terduga." },
  { "en": "Arti Frasa 'Day In And Day Out'?", "id": "Setiap Hari Tanpa Henti." },
  { "en": "Arti Frasa 'Dead As A Doornail'?", "id": "Benar-Benar Mati." },
  { "en": "Arti Frasa 'Dime A Dozen'?", "id": "Sesuatu Yang Sangat Umum." },
  { "en": "Arti Frasa 'Dressed To The Nines'?", "id": "Berpakaian Sangat Rapi Dan Modis." },
  { "en": "Arti Frasa 'Drink Like A Fish'?", "id": "Minum Alkohol Sangat Banyak." },
  { "en": "Arti Frasa 'Drop Like Flies'?", "id": "Gugur Atau Menyerah Dalam Jumlah Besar." },
  { "en": "Arti Frasa 'Dutch Courage'?", "id": "Keberanian Palsu Akibat Alkohol." },
  { "en": "Arti Frasa 'Early Bird Catches The Worm'?", "id": "Bangun Pagi Banyak Rezeki." },
  { "en": "Arti Frasa 'Easier Said Than Done'?", "id": "Lebih Mudah Dikatakan Daripada Dilakukan." },
  { "en": "Arti Frasa 'Eat Like A Bird'?", "id": "Makan Sangat Sedikit." },
  { "en": "Arti Frasa 'Eat Like A Horse'?", "id": "Makan Sangat Banyak." },
  { "en": "Arti Frasa 'Egg On Your Face'?", "id": "Terlihat Bodoh Atau Malu." },
  { "en": "Arti Frasa 'Elbow Room'?", "id": "Ruang Gerak Yang Cukup." },
  { "en": "Arti Frasa 'End Of Your Tether'?", "id": "Di Batas Kesabaran." },
  { "en": "Arti Frasa 'Everything But The Kitchen Sink'?", "id": "Membawa Hampir Segalanya." },
  { "en": "Arti Frasa 'Eye Candy'?", "id": "Sesuatu Yang Enak Dilihat." },
  { "en": "Arti Frasa 'Face The Music'?", "id": "Menghadapi Konsekuensi." },
  { "en": "Arti Frasa 'Fall Off The Wagon'?", "id": "Kembali Ke Kebiasaan Buruk." },
  { "en": "Arti Frasa 'Fan The Flames'?", "id": "Membuat Situasi Buruk Menjadi Lebih Buruk." },
  { "en": "Arti Frasa 'Feast Your Eyes On'?", "id": "Menikmati Pemandangan Sesuatu." },
  { "en": "Arti Frasa 'Feel Blue'?", "id": "Merasa Sedih." },
  { "en": "Arti Frasa 'Fender Bender'?", "id": "Kecelakaan Mobil Ringan." },
  { "en": "Arti Frasa 'Fight Tooth And Nail'?", "id": "Berjuang Mati-Matian." },
  { "en": "Arti Frasa 'Find Your Voice'?", "id": "Berani Mengungkapkan Pendapat." },
  { "en": "Arti Frasa 'Finger-Licking Good'?", "id": "Sangat Enak." },
  { "en": "Arti Frasa 'Fish Out Of Water'?", "id": "Merasa Canggung Di Lingkungan Baru." },
  { "en": "Arti Frasa 'Fit Like A Glove'?", "id": "Ukurannya Sangat Pas." },
  { "en": "Arti Frasa 'Flash In The Pan'?", "id": "Kesuksesan Sesaat." },
  { "en": "Arti Frasa 'Flea In Your Ear'?", "id": "Teguran Keras." },
  { "en": "Arti Frasa 'Flesh And Blood'?", "id": "Keluarga Sendiri." },
  { "en": "Arti Frasa 'Flip The Bird'?", "id": "Mengacungkan Jari Tengah." },
  { "en": "Arti Frasa 'Foam At The Mouth'?", "id": "Sangat Marah." },
  { "en": "Arti Frasa 'Follow Your Nose'?", "id": "Jalan Lurus Terus." },
  { "en": "Arti Frasa 'Food For Thought'?", "id": "Bahan Untuk Dipikirkan." },
  { "en": "Arti Frasa 'Foot The Bill'?", "id": "Membayar Tagihan." },
  { "en": "Arti Frasa 'For Ages'?", "id": "Untuk Waktu Yang Sangat Lama." },
  { "en": "Arti Frasa 'From Rags To Riches'?", "id": "Dari Miskin Menjadi Kaya." },
  { "en": "Arti Frasa 'Full Of Hot Air'?", "id": "Penuh Omong Kosong." },
  { "en": "Arti Frasa 'Funny Farm'?", "id": "Rumah Sakit Jiwa." },
  { "en": "Kepanjangan 'FWIW'?", "id": "For What It's Worth / Sekadar Info Saja." },
  { "en": "Kepanjangan 'WFH'?", "id": "Working From Home / Kerja Dari Rumah." },
  { "en": "Kepanjangan 'BOLO'?", "id": "Be On The Lookout / Harap Waspada." },
  { "en": "Kepanjangan 'FUBAR'?", "id": "Fouled Up Beyond All Recognition / Rusak Parah." },
  { "en": "Kepanjangan 'SNAFU'?", "id": "Situation Normal: All Fouled Up / Situasi Kacau Seperti Biasa." },
  { "en": "Arti Frasa 'Get A Word In'?", "id": "Mendapat Kesempatan Berbicara." },
  { "en": "Arti Frasa 'Get Carried Away'?", "id": "Terlalu Antusias, Kehilangan Kontrol." },
  { "en": "Arti Frasa 'Get Off My Back'?", "id": "Berhenti Menggangguku." },
  { "en": "Arti Frasa 'Get The Drift'?", "id": "Mengerti Maksudnya." },
  { "en": "Arti Frasa 'Get Wind Of'?", "id": "Mendengar Rumor Tentang." },
  { "en": "Arti Frasa 'Gild The Lily'?", "id": "Menghias Sesuatu Yang Sudah Indah." },
  { "en": "Arti Frasa 'Give Someone A Piece Of Your Mind'?", "id": "Memarahi Seseorang." },
  { "en": "Arti Frasa 'Give Someone The Slip'?", "id": "Lolos Dari Seseorang." },
  { "en": "Arti Frasa 'Go Against The Flow'?", "id": "Melawan Arus." },
  { "en": "Arti Frasa 'Go All Out'?", "id": "Melakukan Sesuatu Dengan Maksimal." },
  { "en": "Arti Frasa 'Go Down Like A Lead Balloon'?", "id": "Gagal Total (Untuk Lelucon/Ide)." },
  { "en": "Arti Frasa 'Go For A Song'?", "id": "Dijual Sangat Murah." },
  { "en": "Arti Frasa 'Go Haywire'?", "id": "Menjadi Rusak Atau Kacau." },
  { "en": "Arti Frasa 'Go Off On A Tangent'?", "id": "Tiba-Tiba Membicarakan Topik Lain." },
  { "en": "Arti Frasa 'Go To The Dogs'?", "id": "Menjadi Sangat Buruk." },
  { "en": "Arti Frasa 'Goody Two-Shoes'?", "id": "Orang Yang Sok Suci." },
  { "en": "Arti Frasa 'Grass Roots'?", "id": "Akar Rumput, Rakyat Biasa." },
  { "en": "Arti Frasa 'Gravy Train'?", "id": "Pekerjaan Mudah Dengan Gaji Besar." },
  { "en": "Arti Frasa 'Grease Monkey'?", "id": "Montir." },
  { "en": "Arti Frasa 'Green-Eyed Monster'?", "id": "Rasa Cemburu." },
  { "en": "Arti Frasa 'Grist For The Mill'?", "id": "Sesuatu Yang Bisa Dimanfaatkan." },
  { "en": "Arti Frasa 'Guts Feeling'?", "id": "Firasat." },
  { "en": "Arti Frasa 'Hairy'?", "id": "Berbahaya Atau Menakutkan." },
  { "en": "Arti Frasa 'Hanky-Panky'?", "id": "Perilaku Tidak Jujur Atau Asusila." },
  { "en": "Arti Frasa 'Happy-Go-Lucky'?", "id": "Ceria Dan Tanpa Beban." },
  { "en": "Arti Frasa 'Hard As Nails'?", "id": "Sangat Keras Dan Tidak Emosional." },
  { "en": "Arti Frasa 'Have A Field Day'?", "id": "Sangat Menikmati Sesuatu." },
  { "en": "Arti Frasa 'Have An Egg On Your Face'?", "id": "Terlihat Bodoh Atau Malu." },
  { "en": "Arti Frasa 'Have Someone's Back'?", "id": "Mendukung Seseorang." },
  { "en": "Arti Frasa 'Have Your Work Cut Out For You'?", "id": "Menghadapi Tugas Yang Sangat Sulit." },
  { "en": "Arti Frasa 'Heads Up'?", "id": "Peringatan Awal." },
  { "en": "Arti Frasa 'Heart And Soul'?", "id": "Dengan Sepenuh Hati." },
  { "en": "Arti Frasa 'Heart Missed A Beat'?", "id": "Sangat Terkejut Atau Gugup." },
  { "en": "Arti Frasa 'Heavy Hitter'?", "id": "Orang Penting Dan Berpengaruh." },
  { "en": "Arti Frasa 'High Five'?", "id": "Tos." },
  { "en": "Arti Frasa 'Highway Robbery'?", "id": "Harga Yang Terlalu Mahal." },
  { "en": "Arti Frasa 'Hit A Wall'?", "id": "Menemui Jalan Buntu." },
  { "en": "Arti Frasa 'Knuckle Down'?", "id": "Mulai Bekerja Atau Belajar Serius." },
  { "en": "Arti Frasa 'Labor Of Love'?", "id": "Pekerjaan Yang Dilakukan Karena Suka." },
  { "en": "Arti Frasa 'Land On Your Feet'?", "id": "Berhasil Setelah Masa Sulit." },
  { "en": "Arti Frasa 'Last Laugh'?", "id": "Tertawa Terakhir (Setelah Diremehkan)." },
  { "en": "Arti Frasa 'Lead Astray'?", "id": "Menyesatkan Seseorang." },
  { "en": "Arti Frasa 'Leave Well Enough Alone'?", "id": "Jangan Mengubah Yang Sudah Baik." },
  { "en": "Arti Frasa 'Lemon'?", "id": "Barang Yang Rusak (Terutama Mobil)." },
  { "en": "Arti Frasa 'Let Your Freak Flag Fly'?", "id": "Tunjukkan Sisi Unikmu." },
  { "en": "Arti Frasa 'Lie Low'?", "id": "Bersembunyi Atau Tidak Menonjol." },
  { "en": "Arti Frasa 'Like Clockwork'?", "id": "Berjalan Sangat Lancar Dan Teratur." },
  { "en": "Arti Frasa 'Lip Service'?", "id": "Omong Doang, Tidak Ada Tindakan." },
  { "en": "Arti Frasa 'Lock, Stock, And Barrel'?", "id": "Semuanya, Tanpa Terkecuali." },
  { "en": "Arti Frasa 'Long Shot'?", "id": "Sesuatu Yang Peluangnya Kecil." },
  { "en": "Arti Frasa 'Loose Cannon'?", "id": "Orang Yang Tidak Terduga." },
  { "en": "Arti Frasa 'Lose Your Head'?", "id": "Kehilangan Kendali Diri." },
  { "en": "Arti Frasa 'Mad As A Hatter'?", "id": "Sangat Gila." },
  { "en": "Arti Frasa 'Make A Mountain Out Of A Molehill'?", "id": "Membesar-Besarkan Masalah Kecil." },
  { "en": "Arti Frasa 'Make Ends Meet'?", "id": "Mencukupi Kebutuhan Hidup." },
  { "en": "Arti Frasa 'Make Hay While The Sun Shines'?", "id": "Manfaatkan Kesempatan Selagi Ada." },
  { "en": "Arti Frasa 'Man Of His Word'?", "id": "Orang Yang Bisa Dipegang Janjinya." },
  { "en": "Arti Frasa 'Mickey Mouse'?", "id": "Sesuatu Yang Konyol Atau Tidak Penting." },
  { "en": "Arti Frasa 'Mince Words'?", "id": "Berbicara Terus Terang (Biasanya Negatif)." },
  { "en": "Arti Frasa 'More Holes Than Swiss Cheese'?", "id": "Penuh Dengan Celah Atau Masalah." },
  { "en": "Arti Frasa 'Mouth-Watering'?", "id": "Sangat Menggugah Selera." },
  { "en": "Arti Frasa 'Mover And Shaker'?", "id": "Orang Berpengaruh Di Bidangnya." },
  { "en": "Arti Frasa 'Mud In Your Eye'?", "id": "Ucapan Selamat Minum (Cheers)." },
  { "en": "Arti Frasa 'Mull It Over'?", "id": "Memikirkannya Dengan Matang." },
  { "en": "Arti Frasa 'Needle In A Haystack'?", "id": "Mencari Sesuatu Yang Mustahil." },
  { "en": "Arti Frasa 'Neither Here Nor There'?", "id": "Tidak Relevan." },
  { "en": "Arti Frasa 'New Lease On Life'?", "id": "Kesempatan Kedua Dalam Hidup." },
  { "en": "Arti Frasa 'New York Minute'?", "id": "Waktu Yang Sangat Singkat." },
  { "en": "Arti Frasa 'Nine-To-Five'?", "id": "Pekerjaan Kantor Biasa." },
  { "en": "Arti Frasa 'Nip It In The Bud'?", "id": "Menghentikan Masalah Sejak Dini." },
  { "en": "Arti Frasa 'No Love Lost'?", "id": "Saling Tidak Menukai." },
  { "en": "Arti Frasa 'No Rhyme Or Reason'?", "id": "Tanpa Alasan Yang Jelas." },
  { "en": "Arti Frasa 'Not The Sharpest Tool In The Shed'?", "id": "Orang Yang Kurang Cerdas." },
  { "en": "Arti Frasa 'Nothing To Sneeze At'?", "id": "Sesuatu Yang Tidak Bisa Diremehkan." },
  { "en": "Arti Frasa 'Now You're Talking'?", "id": "Nah, Itu Ide Yang Bagus." },
  { "en": "Arti Frasa 'Off The Cuff'?", "id": "Spontan, Tanpa Persiapan." },
  { "en": "Arti Frasa 'Off The Wall'?", "id": "Sangat Aneh Atau Tidak Biasa." },
  { "en": "Arti Frasa 'Old Stomping Grounds'?", "id": "Tempat Nongkrong Lama." },
  { "en": "Arti Frasa 'On A Roll'?", "id": "Sedang Dalam Rentetan Keberhasilan." },
  { "en": "Arti Frasa 'On Bended Knee'?", "id": "Memohon Dengan Sangat." },
  { "en": "Arti Frasa 'On The Back Foot'?", "id": "Dalam Posisi Bertahan." },
  { "en": "Arti Frasa 'On The Blink'?", "id": "Rusak." },
  { "en": "Arti Frasa 'On The Button'?", "id": "Tepat Sekali." },
  { "en": "Arti Frasa 'On The Fly'?", "id": "Sambil Jalan, Tanpa Rencana." },
  { "en": "Arti Frasa 'On The Wagon'?", "id": "Berhenti Minum Alkohol." },
  { "en": "Arti Frasa 'Open-And-Shut Case'?", "id": "Kasus Yang Sangat Jelas." },
  { "en": "Arti Frasa 'Out In The Boonies'?", "id": "Di Daerah Terpencil." },
  { "en": "Arti Frasa 'Over The Moon'?", "id": "Sangat Senang." },
  { "en": "Arti Frasa 'Pain In The Neck'?", "id": "Sesuatu Atau Seseorang Yang Menyebalkan." },
  { "en": "Arti Frasa 'Pandora's Box'?", "id": "Sumber Dari Banyak Masalah Baru." },
  { "en": "Arti Frasa 'Paper Tiger'?", "id": "Terlihat Kuat Tapi Sebenarnya Lemah." },
  { "en": "Arti Frasa 'Pass The Torch'?", "id": "Meneruskan Tanggung Jawab." },
  { "en": "Arti Frasa 'Pay An Arm And A Leg'?", "id": "Membayar Sangat Mahal." },
  { "en": "Arti Frasa 'Peeping Tom'?", "id": "Pengintip." },
  { "en": "Arti Frasa 'Pick Up The Pieces'?", "id": "Memulai Kembali Setelah Kegagalan." },
  { "en": "Arti Frasa 'Pinch Pennies'?", "id": "Sangat Hemat." },
  { "en": "Arti Frasa 'Pipe Down'?", "id": "Diam, Jangan Berisik." },
  { "en": "Arti Frasa 'Play It Cool'?", "id": "Bersikap Tenang." },
  { "en": "Arti Frasa 'Pressing Flesh'?", "id": "Bersalaman (Biasanya Politisi)." },
  { "en": "Arti Frasa 'Pull The Plug'?", "id": "Menghentikan Sesuatu." },
  { "en": "Arti Frasa 'Pull The Wool Over Someone's Eyes'?", "id": "Menipu Seseorang." },
  { "en": "Arti Frasa 'Puppy Love'?", "id": "Cinta Monyet." },
  { "en": "Arti Frasa 'Put On Ice'?", "id": "Menunda Sesuatu." },
  { "en": "Arti Frasa 'Put Your Money Where Your Mouth Is'?", "id": "Buktikan Omonganmu Dengan Tindakan." },
  { "en": "Arti Frasa 'Rake Over The Coals'?", "id": "Memarahi Habis-Habisan." },
  { "en": "Arti Frasa 'Read Between The Lines'?", "id": "Memahami Makna Tersirat." },
  { "en": "Arti Frasa 'Red Carpet Treatment'?", "id": "Perlakuan Yang Sangat Istimewa." },
  { "en": "Arti Frasa 'Ride Shotgun'?", "id": "Duduk Di Kursi Depan Mobil." },
  { "en": "Arti Frasa 'Right As Rain'?", "id": "Dalam Kondisi Sempurna." },
  { "en": "Arti Frasa 'Roll With The Punches'?", "id": "Beradaptasi Dengan Situasi Sulit." },
  { "en": "Arti Frasa 'Rub The Wrong Way'?", "id": "Membuat Seseorang Kesal." },
  { "en": "Arti Frasa 'Rule Out'?", "id": "Mengesampingkan Kemungkinan." },
  { "en": "Arti Frasa 'Run Aground'?", "id": "Gagal." },
  { "en": "Arti Frasa 'Run Amok'?", "id": "Mengamuk, Menjadi Liar." },
  { "en": "Arti Frasa 'Run Out Of Gas'?", "id": "Kehabisan Energi." },
  { "en": "Arti Frasa 'Russian Roulette'?", "id": "Aktivitas Yang Sangat Berbahaya." },
  { "en": "Arti Frasa 'Sack Out'?", "id": "Pergi Tidur." },
  { "en": "Arti Frasa 'Sacred Cow'?", "id": "Sesuatu Yang Dianggap Suci." },
  { "en": "Arti Frasa 'Salt Away'?", "id": "Menyimpan Uang." },
  { "en": "Arti Frasa 'Same Old Story'?", "id": "Cerita Lama, Sudah Biasa." },
  { "en": "Arti Frasa 'Save Your Breath'?", "id": "Percuma Berbicara." },
  { "en": "Arti Frasa 'Say Uncle'?", "id": "Mengaku Kalah." },
  { "en": "Arti Frasa 'Scare Stiff'?", "id": "Membuat Sangat Ketakutan." },
  { "en": "Arti Frasa 'Scrape Together'?", "id": "Mengumpulkan Dengan Susah Payah." },
  { "en": "Arti Frasa 'Screaming Bloody Murder'?", "id": "Berteriak Sangat Keras." },
  { "en": "Arti Frasa 'Screw Loose'?", "id": "Orang Aneh Atau Gila." },
  { "en": "Arti Frasa 'Second Nature'?", "id": "Kebiasaan Alami." },
  { "en": "Arti Frasa 'See Pink Elephants'?", "id": "Berhalusinasi Karena Mabuk." },
  { "en": "Arti Frasa 'See eye to eye'?", "id": "Setuju Dengan Seseorang." },
  { "en": "Arti Frasa 'Shoot from the hip'?", "id": "Berbicara Spontan Tanpa Berpikir." },
  { "en": "Arti Frasa 'A bitter pill to swallow'?", "id": "Kenyataan Pahit Untuk Diterima." },
  { "en": "Arti Frasa 'A dime a dozen'?", "id": "Sesuatu Yang Sangat Umum." },
  { "en": "Arti Frasa 'A piece of cake'?", "id": "Sesuatu Yang Sangat Mudah." },
  { "en": "Arti Frasa 'Back against the wall'?", "id": "Dalam Posisi Sulit Tanpa Pilihan." },
  { "en": "Arti Frasa 'Ball is in your court'?", "id": "Sekarang Giliranmu Mengambil Keputusan." },
  { "en": "Arti Frasa 'Barking up the wrong tree'?", "id": "Salah Sasaran Atau Salah Arah." },
  { "en": "Arti Frasa 'Beating around the bush'?", "id": "Bertele-Tele." },
  { "en": "Arti Frasa 'Best thing since sliced bread'?", "id": "Inovasi Yang Sangat Hebat." },
  { "en": "Arti Frasa 'Bite off more than you can chew'?", "id": "Mengambil Tugas Melebihi Kemampuan." },
  { "en": "Arti Frasa 'Blessing in disguise'?", "id": "Hikmah Dibalik Musibah." },
  { "en": "Arti Frasa 'Burn the midnight oil'?", "id": "Bekerja Hingga Larut Malam." },
  { "en": "Arti Frasa 'Call it a day'?", "id": "Mengakhiri Pekerjaan Hari Ini." },
  { "en": "Arti Frasa 'Caught between a rock and a hard place'?", "id": "Terjebak Di Antara Pilihan Sulit." },
  { "en": "Arti Frasa 'Cry over spilt milk'?", "id": "Menyesali Yang Sudah Terjadi." },
  { "en": "Arti Frasa 'Cut corners'?", "id": "Mengambil Jalan Pintas (Mengurangi Kualitas)." },
  { "en": "Arti Frasa 'Cut the mustard'?", "id": "Memenuhi Standar." },
  { "en": "Arti Frasa 'Devil's advocate'?", "id": "Mengambil Sisi Berlawanan Untuk Diskusi." },
  { "en": "Arti Frasa 'Don't judge a book by its cover'?", "id": "Jangan Menilai Dari Penampilan Luar." },
  { "en": "Arti Frasa 'Don't put all your eggs in one basket'?", "id": "Jangan Menaruh Semua Resiko Di Satu Tempat." },
  { "en": "Arti Frasa 'Elvis has left the building'?", "id": "Pertunjukan Telah Usai." },
  { "en": "Arti Frasa 'Every cloud has a silver lining'?", "id": "Selalu Ada Sisi Positif." },
  { "en": "Arti Frasa 'Far cry from'?", "id": "Sangat Berbeda Dari." },
  { "en": "Arti Frasa 'Feel under the weather'?", "id": "Merasa Kurang Enak Badan." },
  { "en": "Arti Frasa 'Fit as a fiddle'?", "id": "Sangat Sehat Dan Bugar." },
  { "en": "Arti Frasa 'Get out of hand'?", "id": "Menjadi Tidak Terkendali." },
  { "en": "Arti Frasa 'Get something out of your system'?", "id": "Melakukan Sesuatu Hingga Puas." },
  { "en": "Arti Frasa 'Give the benefit of the doubt'?", "id": "Memberi Kepercayaan Meski Ragu." },
  { "en": "Arti Frasa 'Go back to the drawing board'?", "id": "Mulai Lagi Dari Awal." },
  { "en": "Arti Frasa 'Go cold turkey'?", "id": "Berhenti Total Dari Kebiasaan Buruk." },
  { "en": "Arti Frasa 'Go down in flames'?", "id": "Gagal Total Secara Spektakuler." },
  { "en": "Arti Frasa 'Hang in there'?", "id": "Bertahanlah." },
  { "en": "Arti Frasa 'Hit the nail on the head'?", "id": "Sangat Tepat." },
  { "en": "Arti Frasa 'Hit the sack'?", "id": "Pergi Tidur." },
  { "en": "Arti Frasa 'Icing on the cake'?", "id": "Tambahan Yang Menyenangkan." },
  { "en": "Arti Frasa 'In the heat of the moment'?", "id": "Saat Terbawa Emosi." },
  { "en": "Arti Frasa 'It takes two to tango'?", "id": "Dibutuhkan Dua Pihak (Untuk Konflik)." },
  { "en": "Arti Frasa 'Jump on the bandwagon'?", "id": "Ikut-Ikutan Tren." },
  { "en": "Arti Frasa 'Keep something at bay'?", "id": "Menjaga Sesuatu Tetap Jauh." },
  { "en": "Arti Frasa 'Kill two birds with one stone'?", "id": "Satu Aksi, Dua Hasil." },
  { "en": "Arti Frasa 'Let someone off the hook'?", "id": "Membebaskan Seseorang Dari Tanggung Jawab." },
  { "en": "Arti Frasa 'Let the cat out of the bag'?", "id": "Membocorkan Rahasia." },
  { "en": "Arti Frasa 'Like riding a bike'?", "id": "Kemampuan Yang Tidak Akan Hilang." },
  { "en": "Arti Frasa 'Look before you leap'?", "id": "Pikirkan Dulu Sebelum Bertindak." },
  { "en": "Arti Frasa 'Miss the boat'?", "id": "Kehilangan Kesempatan." },
  { "en": "Arti Frasa 'Mumbo jumbo'?", "id": "Omong Kosong." },
  { "en": "Arti Frasa 'Not a spark of decency'?", "id": "Sama Sekali Tidak Sopan." },
  { "en": "Arti Frasa 'Not my cup of tea'?", "id": "Bukan Selera Atau Minatku." },
  { "en": "Arti Frasa 'On the ball'?", "id": "Cepat Tanggap." },
  { "en": "Arti Frasa 'Once in a blue moon'?", "id": "Sangat Jarang Sekali." },
  { "en": "Arti Frasa 'Picture perfect'?", "id": "Sangat Sempurna." },
  { "en": "Arti Frasa 'Piece of cake'?", "id": "Sangat Mudah." },
  { "en": "Arti Frasa 'Pull a rabbit out of a hat'?", "id": "Melakukan Sesuatu Yang Mengejutkan." },
  { "en": "Arti Frasa 'Pull yourself together'?", "id": "Kendalikan Dirimu." },
  { "en": "Arti Frasa 'Put a sock in it'?", "id": "Diam! (Kasar)." },
  { "en": "Arti Frasa 'Put your foot in your mouth'?", "id": "Salah Bicara." },
  { "en": "Arti Frasa 'Raining cats and dogs'?", "id": "Hujan Sangat Deras." },
  { "en": "Arti Frasa 'Ring a bell'?", "id": "Terdengar Familiar." },
  { "en": "Arti Frasa 'Rule of thumb'?", "id": "Aturan Praktis." },
  { "en": "Arti Frasa 'Saved by the bell'?", "id": "Terselamatkan Tepat Waktu." },
  { "en": "Arti Frasa 'Scapegoat'?", "id": "Kambing Hitam." },
  { "en": "Arti Frasa 'Sit on the fence'?", "id": "Tidak Memihak." },
  { "en": "Arti Frasa 'Sleep like a log'?", "id": "Tidur Sangat Pulas." },
  { "en": "Arti Frasa 'Smell a rat'?", "id": "Mencurigai Sesuatu." },
  { "en": "Arti Frasa 'Sooner or later'?", "id": "Cepat Atau Lambat." },
  { "en": "Arti Frasa 'Spill the beans'?", "id": "Membocorkan Rahasia." },
  { "en": "Arti Frasa 'Steal someone's thunder'?", "id": "Mencuri Perhatian Dari Seseorang." },
  { "en": "Arti Frasa 'Take it with a grain of salt'?", "id": "Jangan Langsung Percaya." },
  { "en": "Arti Frasa 'The ball is in your court'?", "id": "Giliranmu Bertindak." },
  { "en": "Arti Frasa 'The best of both worlds'?", "id": "Mendapat Keuntungan Ganda." },
  { "en": "Arti Frasa 'The elephant in the room'?", "id": "Masalah Besar Yang Diabaikan." },
  { "en": "Arti Frasa 'The last straw'?", "id": "Batas Kesabaran." },
  { "en": "Arti Frasa 'The whole nine yards'?", "id": "Semuanya, Lengkap." },
  { "en": "Arti Frasa 'Through thick and thin'?", "id": "Dalam Suka Dan Duka." },
  { "en": "Arti Frasa 'Throw caution to the wind'?", "id": "Mengambil Resiko." },
  { "en": "Arti Frasa 'Time flies when you're having fun'?", "id": "Waktu Cepat Berlalu Saat Senang." },
  { "en": "Arti Frasa 'To make matters worse'?", "id": "Memperburuk Keadaan." },
  { "en": "Arti Frasa 'Turn a blind eye'?", "id": "Pura-Pura Tidak Tahu." },
  { "en": "Arti Frasa 'Under the weather'?", "id": "Sedang Tidak Enak Badan." },
  { "en": "Arti Frasa 'Up in the air'?", "id": "Belum Pasti." },
  { "en": "Arti Frasa 'We'll cross that bridge when we come to it'?", "id": "Pikirkan Nanti Saja." },
  { "en": "Arti Frasa 'Wear your heart on your sleeve'?", "id": "Terbuka Soal Perasaan." },
  { "en": "Arti Frasa 'When pigs fly'?", "id": "Sesuatu Yang Mustahil." },
  { "en": "Arti Frasa 'Wild goose chase'?", "id": "Pengejaran Sia-Sia." },
  { "en": "Arti Frasa 'Without a doubt'?", "id": "Tanpa Keraguan." },
  { "en": "Arti Frasa 'You can say that again'?", "id": "Aku Sangat Setuju." },
  { "en": "Arti Frasa 'Your guess is as good as mine'?", "id": "Aku Juga Tidak Tahu." },
  { "en": "Kepanjangan 'TBD'?", "id": "To Be Determined / Akan Ditentukan." },
  { "en": "Kepanjangan 'YOLO'?", "id": "You Only Live Once / Hidup Cuma Sekali." },
  { "en": "Kepanjangan 'TBH'?", "id": "To Be Honest / Sejujurnya." },
  { "en": "Kepanjangan 'SMH'?", "id": "Shaking My Head / Menggelengkan Kepala." }




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
