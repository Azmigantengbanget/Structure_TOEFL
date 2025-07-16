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


  { "en": "The committee members ___ angry about the decision.", "id": "Jawaban: are. Penjelasan: Subjek 'committee members' adalah jamak, jadi diperlukan kata kerja jamak 'are'." },
  { "en": "The news from the war-torn country ___ very disturbing.", "id": "Jawaban: is. Penjelasan: 'News' adalah kata benda yang tidak dapat dihitung dan menggunakan kata kerja tunggal." },
  { "en": "Each of the students ___ responsible for their own project.", "id": "Jawaban: is. Penjelasan: 'Each' adalah kata ganti tunggal dan membutuhkan kata kerja tunggal." },
  { "en": "Neither the students nor the teacher ___ satisfied with the results.", "id": "Jawaban: is. Penjelasan: Ketika 'neither...nor' menghubungkan dua subjek, kata kerja setuju dengan subjek yang lebih dekat dengannya ('teacher')." },
  { "en": "A number of people ___ waiting outside.", "id": "Jawaban: are. Penjelasan: 'A number of' adalah quantifier yang menggunakan kata kerja jamak." },
  { "en": "The number of accidents ___ increased this year.", "id": "Jawaban: has. Penjelasan: 'The number of' menggunakan kata kerja tunggal." },
  { "en": "Everybody ___ to be respected.", "id": "Jawaban: wants. Penjelasan: 'Everybody' adalah kata ganti tak tentu tunggal dan menggunakan kata kerja tunggal." },
  { "en": "The dog, along with its puppies, ___ sleeping peacefully.", "id": "Jawaban: is. Penjelasan: Frasa 'along with its puppies' adalah elemen parenthetical. Subjek utamanya adalah 'The dog,' yang tunggal." },
  { "en": "There ___ many reasons for his failure.", "id": "Jawaban: are. Penjelasan: Dalam kalimat yang diawali dengan 'there is/are,' kata kerja setuju dengan kata benda yang mengikutinya ('many reasons')." },
  { "en": "My favorite pair of jeans ___ a hole in the knee.", "id": "Jawaban: has. Penjelasan: 'Pair' adalah tunggal." },
  { "en": "She enjoys reading, writing, and ___ to music.", "id": "Jawaban: listening. Penjelasan: Untuk menjaga struktur paralel dengan gerund 'reading' dan 'writing,' gerund 'listening' harus digunakan." },
  { "en": "The new employee is diligent, efficient, and ___.", "id": "Jawaban: hardworking. Penjelasan: Kalimat tersebut berisi daftar kata sifat. 'Hardworking' adalah kata sifat yang sesuai dengan struktur paralel." },
  { "en": "The movie was not only entertaining ___ educational.", "id": "Jawaban: but also. Penjelasan: Konjungsi korelatif 'not only...but also' digunakan untuk menghubungkan dua elemen paralel." },
  { "en": "He decided to either study medicine ___ law.", "id": "Jawaban: or. Penjelasan: Konjungsi korelatif 'either...or' digunakan untuk menyajikan dua pilihan." },
  { "en": "The report was clear, concise, and ___.", "id": "Jawaban: informative. Penjelasan: Kata sifat 'clear' dan 'concise' membutuhkan kata sifat lain untuk menjaga struktur paralel." },
  { "en": "They are responsible for packing their bags, checking out of the hotel, and ___ to the airport.", "id": "Jawaban: going. Penjelasan: Struktur paralel membutuhkan penggunaan gerund 'going'." },
  { "en": "The professor advised the students to review their notes, do the practice exercises, and ___ for the exam.", "id": "Jawaban: prepare. Penjelasan: Bentuk dasar dari kata kerja diperlukan untuk menjaga paralelisme dengan 'review' dan 'do'." },
  { "en": "The food was delicious, the service was excellent, and ___ was pleasant.", "id": "Jawaban: the atmosphere. Penjelasan: Struktur kalimat membutuhkan frasa kata benda agar sejajar dengan 'the food' dan 'the service'." },
  { "en": "The company's goals are to increase profits, expand market share, and ___ customer satisfaction.", "id": "Jawaban: improve. Penjelasan: Infinitif 'to improve' diperlukan untuk struktur paralel dengan 'to increase' dan 'to expand'." },
  { "en": "She is a talented singer, a gifted dancer, and an accomplished ___.", "id": "Jawaban: actress. Penjelasan: Kalimat tersebut membutuhkan kata benda agar sejajar dengan 'singer' dan 'dancer'." },
  { "en": "The woman ___ I met yesterday is my new boss.", "id": "Jawaban: whom. Penjelasan: 'Whom' adalah bentuk objek dari kata ganti relatif dan mengacu pada 'the woman,' objek dari kata kerja 'met'." },
  { "en": "This is the book ___ I was telling you about.", "id": "Jawaban: that. Penjelasan: 'That' adalah kata ganti relatif yang digunakan untuk memperkenalkan klausa yang mengidentifikasi kata benda 'book'." },
  { "en": "Everyone should bring ___ own lunch.", "id": "Jawaban: their. Penjelasan: Dalam bahasa Inggris modern, 'their' sering digunakan sebagai kata ganti tunggal untuk menghindari bahasa yang bias gender saat merujuk pada 'everyone'." },
  { "en": "The cat washed ___ paws.", "id": "Jawaban: its. Penjelasan: 'Its' adalah kata ganti posesif untuk 'the cat'." },
  { "en": "The prize will be given to ___ finishes the race first.", "id": "Jawaban: whoever. Penjelasan: 'Whoever' adalah subjek dari kata kerja 'finishes'." },
  { "en": "Between you and ___, I think this plan will fail.", "id": "Jawaban: me. Penjelasan: 'Me' adalah objek dari preposisi 'between'." },
  { "en": "The students, ___ were from all over the world, enjoyed the conference.", "id": "Jawaban: who. Penjelasan: 'Who' adalah bentuk subjek dari kata ganti relatif dan mengacu pada 'the students'." },
  { "en": "The company for ___ she works is very successful.", "id": "Jawaban: which. Penjelasan: 'Which' adalah kata ganti relatif yang digunakan untuk merujuk pada 'the company'." },
  { "en": "___ of the two options do you prefer?", "id": "Jawaban: Which. Penjelasan: 'Which' digunakan saat memilih dari sejumlah pilihan yang terbatas." },
  { "en": "The book is about a man ___ loses his memory.", "id": "Jawaban: who. Penjelasan: 'Who' adalah kata ganti relatif yang digunakan untuk merujuk pada 'a man' sebagai subjek dari kata kerja 'loses'." },
  { "en": "By the time I arrived, the party ___.", "id": "Jawaban: had already started. Penjelasan: Past perfect tense 'had already started' digunakan untuk menunjukkan tindakan yang telah selesai sebelum tindakan lain di masa lalu." },
  { "en": "I ___ to the gym every day after work.", "id": "Jawaban: go. Penjelasan: Simple present tense digunakan untuk tindakan kebiasaan." },
  { "en": "She ___ for the company for ten years.", "id": "Jawaban: has been working. Penjelasan: Present perfect continuous tense digunakan untuk menggambarkan tindakan yang dimulai di masa lalu dan masih berlangsung." },
  { "en": "While I was studying, the phone ___.", "id": "Jawaban: rang. Penjelasan: Past simple tense digunakan untuk tindakan yang lebih pendek yang menyela tindakan yang lebih lama di masa lalu." },
  { "en": "If I ___ you, I would take the job.", "id": "Jawaban: were. Penjelasan: Subjunctive mood 'were' digunakan dalam conditional kedua untuk membicarakan situasi hipotetis." },
  { "en": "He will call you as soon as he ___.", "id": "Jawaban: arrives. Penjelasan: Dalam klausa waktu masa depan, simple present tense digunakan." },
  { "en": "Look! The children ___ in the garden.", "id": "Jawaban: are playing. Penjelasan: Present continuous tense digunakan untuk menggambarkan tindakan yang sedang terjadi sekarang." },
  { "en": "I wish I ___ more time to travel.", "id": "Jawaban: had. Penjelasan: Setelah 'wish,' past simple tense digunakan untuk menyatakan keinginan agar sesuatu berbeda di masa sekarang." },
  { "en": "The Earth ___ around the Sun.", "id": "Jawaban: revolves. Penjelasan: Simple present tense digunakan untuk kebenaran umum dan fakta ilmiah." },
  { "en": "They ___ to a new house next month.", "id": "Jawaban: are moving. Penjelasan: Present continuous tense dapat digunakan untuk membicarakan pengaturan di masa depan." },
  { "en": "You ___ smoke in the hospital.", "id": "Jawaban: must not. Penjelasan: 'Must not' menyatakan larangan yang kuat." },
  { "en": "I ___ be able to help you tomorrow.", "id": "Jawaban: might. Penjelasan: 'Might' menyatakan kemungkinan." },
  { "en": "___ I borrow your pen, please?", "id": "Jawaban: May. Penjelasan: 'May' digunakan untuk meminta izin dengan sopan." },
  { "en": "He ___ have studied harder for the exam.", "id": "Jawaban: should. Penjelasan: 'Should have' digunakan untuk menyatakan penyesalan atau kritik tentang tindakan di masa lalu." },
  { "en": "She ___ be at home; her car is in the driveway.", "id": "Jawaban: must. Penjelasan: 'Must' menyatakan kesimpulan logis atau kemungkinan besar." },
  { "en": "When I was a child, I ___ climb trees.", "id": "Jawaban: could. Penjelasan: 'Could' digunakan untuk membicarakan kemampuan di masa lalu." },
  { "en": "You ___ see a doctor if you're not feeling well.", "id": "Jawaban: should. Penjelasan: 'Should' digunakan untuk memberi nasihat." },
  { "en": "We ___ to finish the project by Friday.", "id": "Jawaban: have. Penjelasan: 'Have to' menyatakan keharusan atau kewajiban." },
  { "en": "They ___ have left already; it's still early.", "id": "Jawaban: can't. Penjelasan: 'Can't have' menyatakan bahwa sesuatu tidak mungkin terjadi di masa lalu." },
  { "en": "He ___ be late for the meeting.", "id": "Jawaban: shouldn't. Penjelasan: 'Shouldn't' menyatakan bahwa sesuatu tidak disarankan atau diharapkan." },
  { "en": "I saw ___ eagle soaring in the sky.", "id": "Jawaban: an. Penjelasan: 'An' digunakan sebelum kata benda tunggal yang diawali dengan bunyi vokal." },
  { "en": "___ moon is very bright tonight.", "id": "Jawaban: The. Penjelasan: 'The' digunakan karena hanya ada satu bulan." },
  { "en": "She is studying to be ___ doctor.", "id": "Jawaban: a. Penjelasan: 'A' digunakan sebelum kata benda tunggal yang diawali dengan bunyi konsonan." },
  { "en": "I need to buy ___ new pair of shoes.", "id": "Jawaban: a. Penjelasan: 'A' digunakan sebelum kata benda tunggal 'pair'." },
  { "en": "There are many ___ in the library.", "id": "Jawaban: books. Penjelasan: 'Many' diikuti oleh kata benda jamak." },
  { "en": "The government is responsible for ___ welfare of its citizens.", "id": "Jawaban: the. Penjelasan: 'The' digunakan karena 'welfare of its citizens' adalah konsep yang spesifik." },
  { "en": "I would like to have ___ cup of coffee.", "id": "Jawaban: a. Penjelasan: 'A' digunakan untuk merujuk pada satu item yang tidak spesifik." },
  { "en": "___ information you gave me was very helpful.", "id": "Jawaban: The. Penjelasan: 'The' digunakan karena merujuk pada informasi yang spesifik." },
  { "en": "She has a lot of ___.", "id": "Jawaban: experience. Penjelasan: 'Experience' adalah kata benda yang tidak dapat dihitung." },
  { "en": "He gave me some good ___.", "id": "Jawaban: advice. Penjelasan: 'Advice' adalah kata benda yang tidak dapat dihitung." },
  { "en": "The music is too ___.", "id": "Jawaban: loud. Penjelasan: 'Loud' adalah kata sifat yang mendeskripsikan kata benda 'music'." },
  { "en": "He speaks English very ___.", "id": "Jawaban: well. Penjelasan: 'Well' adalah kata keterangan yang memodifikasi kata kerja 'speaks'." },
  { "en": "The cake tastes ___.", "id": "Jawaban: delicious. Penjelasan: 'Delicious' adalah kata sifat yang mengikuti kata kerja penghubung 'tastes' dan mendeskripsikan subjek 'cake'." },
  { "en": "She drives her car very ___.", "id": "Jawaban: carefully. Penjelasan: 'Carefully' adalah kata keterangan yang memodifikasi kata kerja 'drives'." },
  { "en": "The patient is feeling ___ better today.", "id": "Jawaban: much. Penjelasan: 'Much' adalah kata keterangan yang memodifikasi kata sifat komparatif 'better'." },
  { "en": "I have ___ seen that movie before.", "id": "Jawaban: never. Penjelasan: 'Never' adalah kata keterangan frekuensi." },
  { "en": "The ___ dog barked all night.", "id": "Jawaban: noisy. Penjelasan: 'Noisy' adalah kata sifat yang mendeskripsikan kata benda 'dog'." },
  { "en": "He is a ___ talented musician.", "id": "Jawaban: highly. Penjelasan: 'Highly' adalah kata keterangan yang memodifikasi kata sifat 'talented'." },
  { "en": "The children were playing ___ in the park.", "id": "Jawaban: happily. Penjelasan: 'Happily' adalah kata keterangan yang memodifikasi kata kerja 'playing'." },
  { "en": "The report is ___ complete.", "id": "Jawaban: almost. Penjelasan: 'Almost' adalah kata keterangan yang memodifikasi kata sifat 'complete'." },
  { "en": "The book is ___ the table.", "id": "Jawaban: on. Penjelasan: 'On' digunakan untuk menunjukkan posisi di atas permukaan." },
  { "en": "I will see you ___ Monday.", "id": "Jawaban: on. Penjelasan: 'On' digunakan dengan hari dalam seminggu." },
  { "en": "The meeting is ___ 10 AM.", "id": "Jawaban: at. Penjelasan: 'At' digunakan dengan waktu tertentu." },
  { "en": "He lives ___ a small village.", "id": "Jawaban: in. Penjelasan: 'In' digunakan untuk area yang lebih besar seperti kota, negara, dan desa." },
  { "en": "She is afraid ___ spiders.", "id": "Jawaban: of. Penjelasan: Kata sifat 'afraid' diikuti oleh preposisi 'of'." },
  { "en": "I am interested ___ learning a new language.", "id": "Jawaban: in. Penjelasan: Kata sifat 'interested' diikuti oleh preposisi 'in'." },
  { "en": "He is responsible ___ the marketing department.", "id": "Jawaban: for. Penjelasan: Kata sifat 'responsible' diikuti oleh preposisi 'for'." },
  { "en": "The cat is hiding ___ the bed.", "id": "Jawaban: under. Penjelasan: 'Under' digunakan untuk menunjukkan posisi di bawah sesuatu." },
  { "en": "The store is open ___ 9 AM to 9 PM.", "id": "Jawaban: from. Penjelasan: 'From...to' digunakan untuk menunjukkan periode waktu." },
  { "en": "I am looking forward ___ seeing you.", "id": "Jawaban: to. Penjelasan: 'Look forward to' adalah kata kerja frasa yang diikuti oleh gerund." },
  { "en": "I wanted to go to the party, ___ I was too tired.", "id": "Jawaban: but. Penjelasan: 'But' digunakan untuk menghubungkan dua ide yang kontras." },
  { "en": "She studied hard, ___ she passed the exam easily.", "id": "Jawaban: so. Penjelasan: 'So' digunakan untuk menunjukkan hasil atau konsekuensi." },
  { "en": "I will call you ___ I get home.", "id": "Jawaban: when. Penjelasan: 'When' adalah konjungsi subordinatif yang memperkenalkan klausa waktu." },
  { "en": "___ it was raining, we still went for a walk.", "id": "Jawaban: Although. Penjelasan: 'Although' digunakan untuk memperkenalkan klausa yang kontras dengan klausa utama." },
  { "en": "He is not only a great leader ___ a kind person.", "id": "Jawaban: but also. Penjelasan: 'Not only...but also' adalah konjungsi korelatif." },
  { "en": "You can have either tea ___ coffee.", "id": "Jawaban: or. Penjelasan: 'Either...or' adalah konjungsi korelatif yang menyajikan pilihan." },
  { "en": "I need to finish my homework ___ I can watch TV.", "id": "Jawaban: before. Penjelasan: 'Before' adalah konjungsi subordinatif yang menunjukkan urutan kejadian." },
  { "en": "She has been working here ___ she graduated from college.", "id": "Jawaban: since. Penjelasan: 'Since' adalah konjungsi subordinatif yang menunjukkan titik awal waktu." },
  { "en": "Never ___ I seen such a beautiful sunset.", "id": "Jawaban: have. Penjelasan: Ketika sebuah kalimat dimulai dengan kata keterangan negatif seperti 'never,' subjek dan kata kerja bantu dibalik." },
  { "en": "Not only ___ he a great singer, but he is also a talented actor.", "id": "Jawaban: is. Penjelasan: Setelah 'not only' di awal kalimat, subjek dan kata kerja bantu dibalik." },
  { "en": "Little ___ she know that he was planning a surprise party for her.", "id": "Jawaban: did. Penjelasan: Setelah kata keterangan negatif seperti 'little,' subjek dan kata kerja bantu dibalik." },
  { "en": "On no occasion ___ the company break its promises to its customers.", "id": "Jawaban: did. Penjelasan: Ketika sebuah kalimat dimulai dengan frasa negatif seperti 'on no occasion,' subjek dan kata kerja bantu dibalik." },
  { "en": "Seldom ___ we see such a magnificent display of fireworks.", "id": "Jawaban: do. Penjelasan: Ketika sebuah kalimat dimulai dengan kata keterangan negatif seperti 'seldom,' subjek dan kata kerja bantu dibalik." },
  { "en": "So ___ was the movie that I saw it twice.", "id": "Jawaban: good. Penjelasan: 'So + kata sifat + was/is/etc. + subjek' adalah struktur yang digunakan untuk penekanan." },
  { "en": "Such ___ his strength that he could lift the heavy rock.", "id": "Jawaban: was. Penjelasan: Setelah 'such' di awal kalimat, kata kerja seringkali mendahului subjek." },
  { "en": "Had I known you were coming, I ___ have baked a cake.", "id": "Jawaban: would. Penjelasan: Dalam kalimat conditional ketiga, 'if' dapat dihilangkan dan subjek serta 'had' dapat dibalik." },
  { "en": "Only after the exam ___ I realize how much I had forgotten.", "id": "Jawaban: did. Penjelasan: Ketika sebuah kalimat dimulai dengan 'only after,' subjek dan kata kerja bantu dalam klausa utama dibalik." },
  { "en": "No sooner ___ the sun risen than the birds began to sing.", "id": "Jawaban: had. Penjelasan: Struktur 'no sooner...than' digunakan untuk membicarakan dua tindakan yang terjadi secara berurutan. Setelah 'no sooner,' subjek dan 'had' dibalik." },
  { "en": "The hotel ___ we stayed last summer was excellent.", "id": "Jawaban: where. Penjelasan: 'Where' adalah kata keterangan relatif yang digunakan untuk memodifikasi tempat (the hotel)." },
  { "en": "I don't know ___ he decided to leave so suddenly.", "id": "Jawaban: why. Penjelasan: Klausa kata benda 'why he decided to leave' berfungsi sebagai objek dari kata kerja 'know'. 'Why' memperkenalkan alasan." },
  { "en": "She will be late ___ she gets stuck in traffic.", "id": "Jawaban: if. Penjelasan: 'If' memperkenalkan klausa keterangan kondisional." },
  { "en": "The man ___ in the corner is my uncle.", "id": "Jawaban: sitting. Penjelasan: Ini adalah klausa kata sifat yang disingkat dari 'who is sitting'. Partisipel sekarang 'sitting' digunakan untuk kata kerja aktif." },
  { "en": "The ideas ___ in the report were revolutionary.", "id": "Jawaban: presented. Penjelasan: Ini adalah klausa kata sifat yang disingkat dari 'which were presented'. Partisipel lampau 'presented' digunakan untuk kata kerja pasif." },
  { "en": "___ by the good news, she called her family immediately.", "id": "Jawaban: Excited. Penjelasan: Ini adalah klausa keterangan yang disingkat dari 'Because she was excited'. Partisipel lampau 'Excited' digunakan untuk menggambarkan perasaannya." },
  { "en": "This book is ___ than the last one I read.", "id": "Jawaban: more interesting. Penjelasan: Untuk kata sifat dengan dua suku kata atau lebih, 'more' digunakan untuk membentuk komparatif." },
  { "en": "She is the ___ student in the entire class.", "id": "Jawaban: smartest. Penjelasan: Bentuk superlatif 'smartest' digunakan dengan 'the' untuk membandingkan satu orang dengan seluruh kelompok." },
  { "en": "He runs ___ than his brother.", "id": "Jawaban: faster. Penjelasan: 'Faster' adalah bentuk komparatif dari kata keterangan 'fast'." },
  { "en": "Of the three options, this one is the ___ expensive.", "id": "Jawaban: least. Penjelasan: 'Least' adalah bentuk superlatif dari 'little' dan digunakan untuk menunjukkan jumlah atau tingkatan terkecil." },
  { "en": "If he had taken my advice, he ___ in trouble now.", "id": "Jawaban: would not be. Penjelasan: Ini adalah conditional campuran, menggabungkan kondisi masa lalu (If + past perfect) dengan hasil sekarang (would + kata kerja dasar)." },
  { "en": "I would have gone to the party if I ___ invited.", "id": "Jawaban: had been. Penjelasan: Ini adalah conditional ketiga, digunakan untuk situasi hipotetis masa lalu (if + past perfect, would have + past participle)." },
  { "en": "If you ___ water, it boils.", "id": "Jawaban: heat. Penjelasan: Ini adalah conditional nol, digunakan untuk kebenaran umum atau fakta ilmiah (If + simple present, simple present)." },
  { "en": "___ you see him, please give him this message.", "id": "Jawaban: Should. Penjelasan: Ini adalah bentuk terbalik dari conditional pertama ('If you should see him...'), yang lebih formal." },
  { "en": "She would travel more if she ___ more vacation time.", "id": "Jawaban: had. Penjelasan: Ini adalah conditional kedua, digunakan untuk situasi hipotetis saat ini (if + simple past, would + kata kerja dasar)." },
  { "en": "I can't help ___ at his jokes.", "id": "Jawaban: laughing. Penjelasan: Ungkapan 'can't help' diikuti oleh gerund." },
  { "en": "He promised ___ me with my homework.", "id": "Jawaban: to help. Penjelasan: Kata kerja 'promise' diikuti oleh infinitive." },
  { "en": "She is considering ___ to a new city.", "id": "Jawaban: moving. Penjelasan: Kata kerja 'consider' diikuti oleh gerund." },
  { "en": "It is important ___ the instructions carefully.", "id": "Jawaban: to read. Penjelasan: Struktur 'It is + adjective + infinitive' umum digunakan." },
  { "en": "He avoids ___ in heavy traffic.", "id": "Jawaban: driving. Penjelasan: Kata kerja 'avoid' diikuti oleh gerund." },
  { "en": "The manager had the assistant ___ the report.", "id": "Jawaban: type. Penjelasan: Dengan 'have' sebagai kata kerja kausatif, bentuk dasar kata kerja digunakan (have someone do something)." },
  { "en": "My parents made me ___ my room every weekend.", "id": "Jawaban: clean. Penjelasan: Dengan 'make' sebagai kata kerja kausatif, bentuk dasar kata kerja digunakan (make someone do something)." },
  { "en": "She got her husband ___ the groceries.", "id": "Jawaban: to buy. Penjelasan: Dengan 'get' sebagai kata kerja kausatif, infinitive digunakan (get someone to do something)." },
  { "en": "The teacher let the students ___ the classroom early.", "id": "Jawaban: leave. Penjelasan: Dengan 'let' sebagai kata kerja kausatif, bentuk dasar kata kerja digunakan (let someone do something)." },
  { "en": "I had my car ___ by a professional mechanic.", "id": "Jawaban: repaired. Penjelasan: Ketika objek dari kata kerja kausatif 'have' menerima tindakan, partisipel lampau digunakan (have something done)." },
  { "en": "Dr. Evans, ___ of the chemistry department, will give the opening speech.", "id": "Jawaban: the head. Penjelasan: Frasa apositif 'the head of the chemistry department' memberi nama lain atau informasi lebih lanjut tentang 'Dr. Evans'." },
  { "en": "The book, ___, was an international bestseller.", "id": "Jawaban: a novel by a first-time author. Penjelasan: Frasa apositif ini memberikan informasi lebih lanjut tentang 'The book'." },
  { "en": "We visited Stratford-upon-Avon, ___ of William Shakespeare.", "id": "Jawaban: the birthplace. Penjelasan: Apositif 'the birthplace of William Shakespeare' memberi nama lain untuk kota 'Stratford-upon-Avon'." },
  { "en": "The platypus, ___, is native to Australia.", "id": "Jawaban: a semi-aquatic mammal. Penjelasan: Frasa apositif ini mendefinisikan 'platypus'." },
  { "en": "The new policy will ___ everyone in the company.", "id": "Jawaban: affect. Penjelasan: 'Affect' adalah kata kerja yang berarti 'mempengaruhi'. 'Effect' biasanya adalah kata benda yang berarti 'hasil'." },
  { "en": "The medicine had a positive ___ on her health.", "id": "Jawaban: effect. Penjelasan: 'Effect' adalah kata benda yang berarti 'hasil' atau 'pengaruh'." },
  { "en": "There are ___ books on the table than on the shelf.", "id": "Jawaban: fewer. Penjelasan: 'Fewer' digunakan untuk kata benda yang dapat dihitung seperti 'books'. 'Less' digunakan untuk kata benda yang tidak dapat dihitung." },
  { "en": "I need to ___ down for a few minutes.", "id": "Jawaban: lie. Penjelasan: 'Lie' berarti berbaring. 'Lay' berarti meletakkan sesuatu dan membutuhkan objek." },
  { "en": "Please ___ the book on the desk.", "id": "Jawaban: lay. Penjelasan: 'Lay' berarti meletakkan sesuatu dan membutuhkan objek ('the book')." },
  { "en": "The flock of birds ___ south for the winter.", "id": "Jawaban: flies. Penjelasan: 'Flock' adalah kata benda kolektif. Ketika bertindak sebagai satu unit, ia menggunakan kata kerja tunggal." },
  { "en": "The data ___ that the new strategy is effective.", "id": "Jawaban: show. Penjelasan: 'Data' adalah bentuk jamak dari 'datum' dan biasanya menggunakan kata kerja jamak dalam tulisan formal dan akademis." },
  { "en": "One of the main reasons for his success ___ his perseverance.", "id": "Jawaban: is. Penjelasan: Subjeknya adalah 'one', yang tunggal. Frasa preposisional 'of the main reasons' tidak mempengaruhi kata kerja." },
  { "en": "Mathematics ___ a subject I've always enjoyed.", "id": "Jawaban: is. Penjelasan: Kata benda yang berakhiran '-ics' yang merujuk pada bidang studi adalah tunggal." },
  { "en": "The police ___ investigating the incident.", "id": "Jawaban: are. Penjelasan: 'Police' adalah kata benda jamak." },
  { "en": "It was ___ a difficult test that few students passed.", "id": "Jawaban: such. Penjelasan: Struktur 'such + a/an + adjective + noun + that' digunakan untuk penekanan guna menunjukkan hasil." },
  { "en": "Not until the next day ___ she realize her mistake.", "id": "Jawaban: did. Penjelasan: Ketika sebuah kalimat dimulai dengan 'Not until', klausa utamanya dibalik (kata kerja bantu + subjek)." },
  { "en": "The new regulations require that every employee ___ a training course.", "id": "Jawaban: attend. Penjelasan: Ini adalah subjunctive mood. Setelah kata kerja seperti 'require', 'suggest', 'demand' + 'that', bentuk dasar kata kerja digunakan." },
  { "en": "___ of the rain, the game was not canceled.", "id": "Jawaban: In spite. Penjelasan: 'In spite of' adalah preposisi yang digunakan untuk menunjukkan kontras dan diikuti oleh kata benda atau gerund." },
  { "en": "He is known for his ability ___ complex problems.", "id": "Jawaban: to solve. Penjelasan: Kata benda 'ability' diikuti oleh infinitive." },
  { "en": "The higher the temperature, ___ the water evaporates.", "id": "Jawaban: the faster. Penjelasan: Ini adalah struktur komparatif paralel ('The + comparative..., the + comparative...')." },
  { "en": "The equipment, ___ was purchased last year, is already obsolete.", "id": "Jawaban: which. Penjelasan: 'Which' digunakan untuk memperkenalkan klausa non-restriktif yang memberikan informasi tambahan tentang 'the equipment'." },
  { "en": "He insisted on ___ for the entire meal.", "id": "Jawaban: paying. Penjelasan: Preposisi 'on' diikuti oleh gerund ('paying')." },
  { "en": "Rarely ___ such a talented performer on this stage.", "id": "Jawaban: have we seen. Penjelasan: Ketika sebuah kalimat dimulai dengan kata keterangan negatif seperti 'Rarely', subjek dan kata kerja bantu dibalik." },
  { "en": "___ to get a promotion, she has been working extra hours.", "id": "Jawaban: Hoping. Penjelasan: Ini adalah klausa keterangan yang disingkat ('Because she is hoping...')." },
  { "en": "That is the man ___ car was stolen.", "id": "Jawaban: whose. Penjelasan: 'Whose' adalah kata ganti relatif posesif yang digunakan untuk menunjukkan bahwa mobil itu milik pria tersebut." },
  { "en": "I would rather ___ at home than go out tonight.", "id": "Jawaban: stay. Penjelasan: Ungkapan 'would rather' diikuti oleh bentuk dasar kata kerja." },
  { "en": "All of the furniture in the house ___ old.", "id": "Jawaban: is. Penjelasan: Ketika 'all of' diikuti oleh kata benda yang tidak dapat dihitung ('furniture'), kata kerjanya tunggal." },
  { "en": "___ he is very rich, he is not a happy man.", "id": "Jawaban: Although. Penjelasan: 'Although' adalah konjungsi yang digunakan untuk memperkenalkan ide yang kontras." },
  { "en": "The project must be finished ___ of the cost.", "id": "Jawaban: regardless. Penjelasan: 'Regardless of' adalah frasa preposisional yang berarti 'tanpa mempertimbangkan'." },
  { "en": "The committee is made ___ of ten members.", "id": "Jawaban: up. Penjelasan: 'To be made up of' adalah kata kerja frasa yang berarti terdiri dari atau tersusun dari." },
  { "en": "She is accustomed ___ with a team.", "id": "Jawaban: to working. Penjelasan: Frasa 'be accustomed to' diikuti oleh gerund ('working')." },
  { "en": "Hardly ___ I walked in the door when the phone rang.", "id": "Jawaban: had. Penjelasan: Struktur 'Hardly...when' membutuhkan pembalikan di klausa pertama ('Hardly had I...')." },
  { "en": "He has little experience, ___ he got the job.", "id": "Jawaban: yet. Penjelasan: 'Yet' digunakan sebagai konjungsi yang berarti 'tapi' atau 'namun demikian', menunjukkan kontras yang tak terduga." },
  { "en": "The final decision is ___ to the manager.", "id": "Jawaban: up. Penjelasan: Frasa 'be up to someone' berarti itu adalah tanggung jawab mereka untuk memutuskan." },
  { "en": "The old building was torn ___ to make way for a new one.", "id": "Jawaban: down. Penjelasan: 'Tear down' adalah kata kerja frasa yang berarti menghancurkan." },
  { "en": "Neither of the answers ___ correct.", "id": "Jawaban: is. Penjelasan: 'Neither' adalah kata ganti tunggal dan menggunakan kata kerja tunggal." },
  { "en": "The lecture was so ___ that I almost fell asleep.", "id": "Jawaban: boring. Penjelasan: 'Boring' adalah kata sifat partisipel sekarang yang digunakan untuk menggambarkan kuliah (hal yang menyebabkan perasaan). 'Bored' menggambarkan bagaimana perasaan seseorang." },
  { "en": "Having ___ the book, he could answer all the questions.", "id": "Jawaban: read. Penjelasan: Ini adalah frasa partisipel sempurna, menunjukkan bahwa tindakan membaca telah selesai sebelum tindakan berikutnya. Strukturnya adalah 'Having + past participle'." },
  { "en": "He is as tall ___ his father.", "id": "Jawaban: as. Penjelasan: Struktur untuk perbandingan yang setara adalah 'as + adjective + as'." },
  { "en": "The company plans on ___ more employees next year.", "id": "Jawaban: hiring. Penjelasan: Preposisi 'on' diikuti oleh gerund ('hiring')." },
  { "en": "This is ___ the best movie I have ever seen.", "id": "Jawaban: by far. Penjelasan: 'By far' adalah idiom yang digunakan untuk menambah penekanan pada kata sifat superlatif ('the best')." },
  { "en": "She is capable ___ handling the situation herself.", "id": "Jawaban: of. Penjelasan: Kata sifat 'capable' diikuti oleh preposisi 'of'." },
  { "en": "The amount of information ___ overwhelming.", "id": "Jawaban: was. Penjelasan: Subjeknya adalah 'amount', yang tunggal." },
  { "en": "The faster he drove, ___ he became.", "id": "Jawaban: the more nervous. Penjelasan: Ini adalah struktur komparatif paralel ('The + comparative..., the + comparative...')." },
  { "en": "It's no use ___ about the past.", "id": "Jawaban: worrying. Penjelasan: Ungkapan 'it's no use' diikuti oleh gerund." },
  { "en": "The politician's speech had a strong ___ on the audience.", "id": "Jawaban: influence. Penjelasan: 'Influence' adalah kata benda di sini, yang berarti kekuatan untuk memiliki efek." },
  { "en": "___ the cold weather, she went out without a coat.", "id": "Jawaban: Despite. Penjelasan: 'Despite' adalah preposisi yang diikuti oleh kata benda, digunakan untuk menunjukkan kontras." },
  { "en": "The jury ___ unable to agree on a verdict.", "id": "Jawaban: was. Penjelasan: Ketika kata benda kolektif 'jury' bertindak sebagai satu unit, ia menggunakan kata kerja tunggal." },
  { "en": "She found the key ___ under the mat.", "id": "Jawaban: hidden. Penjelasan: 'Hidden' adalah partisipel lampau yang digunakan sebagai kata sifat untuk menggambarkan kunci. Ini adalah klausa yang disingkat dari 'which was hidden'." },
  { "en": "He objected ___ treated like a child.", "id": "Jawaban: to being. Penjelasan: Kata kerja 'object to' diikuti oleh gerund ('being')." },
  { "en": "A large percentage of the students ___ from out of state.", "id": "Jawaban: come. Penjelasan: Saat menggunakan 'percentage of', kata kerja setuju dengan kata benda dalam frasa preposisional ('students', yang jamak)." },
  { "en": "He works too ___ and needs to take a break.", "id": "Jawaban: hard. Penjelasan: 'Hard' bisa menjadi kata sifat dan kata keterangan. Di sini ia berfungsi sebagai kata keterangan yang memodifikasi 'works'. 'Hardly' berarti 'hampir tidak sama sekali'." },
  { "en": "The news about the merger ___ announced yesterday.", "id": "Jawaban: was. Penjelasan: 'News' adalah kata benda yang tidak dapat dihitung dan selalu menggunakan kata kerja tunggal." },
  { "en": "That is a matter ___ which we have little control.", "id": "Jawaban: over. Penjelasan: Frasa 'to have control over something' adalah kolokasi standar." },
  { "en": "Her job involves ___ with clients from all over the world.", "id": "Jawaban: dealing. Penjelasan: Kata kerja 'involves' diikuti oleh gerund ('dealing')." },
  { "en": "The discovery was ___ to years of dedicated research.", "id": "Jawaban: due. Penjelasan: 'Due to' adalah frasa yang berarti 'sebagai hasil dari' atau 'karena'." },
  { "en": "They were disappointed ___ the results of the experiment.", "id": "Jawaban: with. Penjelasan: Kata sifat 'disappointed' sering diikuti oleh preposisi 'with' atau 'by'." },
  { "en": "___ of his warnings, they went ahead with the plan.", "id": "Jawaban: Regardless. Penjelasan: 'Regardless of' adalah frasa preposisional yang berarti 'tanpa terpengaruh oleh'." },
  { "en": "The more I think about it, ___ I understand.", "id": "Jawaban: the less. Penjelasan: Ini adalah struktur komparatif paralel yang menunjukkan hubungan terbalik." },
  { "en": "___ its small size, the device is very powerful.", "id": "Jawaban: For. Penjelasan: 'For' dapat digunakan untuk berarti 'mengingat' atau 'meskipun', terutama dengan ukuran atau usia." },
  { "en": "The witness's testimony was inconsistent ___ the evidence.", "id": "Jawaban: with. Penjelasan: Kata sifat 'inconsistent' diikuti oleh preposisi 'with'." },
  { "en": "Scarcely had she finished speaking ___ the audience burst into applause.", "id": "Jawaban: when. Penjelasan: Strukturnya adalah 'Scarcely...when', mirip dengan 'Hardly...when'." },
  { "en": "I am not used ___ up so early in the morning.", "id": "Jawaban: to getting. Penjelasan: Frasa 'be used to' diikuti oleh gerund dan berarti 'terbiasa'." },
  { "en": "___ a talented artist, he is also a successful businessman.", "id": "Jawaban: Besides being. Penjelasan: 'Besides being' berarti 'selain menjadi'." },
  { "en": "His health has improved ___ since he started exercising.", "id": "Jawaban: significantly. Penjelasan: 'Significantly' adalah kata keterangan yang memodifikasi kata kerja 'has improved'." },
  { "en": "The two companies are ___ to merge by the end of the year.", "id": "Jawaban: expected. Penjelasan: Struktur pasif 'are expected to' digunakan untuk menunjukkan apa yang diantisipasi." },
  { "en": "The defendant was accused ___ lying under oath.", "id": "Jawaban: of. Penjelasan: Kata kerja 'accuse' diikuti oleh preposisi 'of'." },
  { "en": "He found it difficult ___ his true feelings.", "id": "Jawaban: to express. Penjelasan: Struktur 'find it + adjective + infinitive' umum digunakan." },
  { "en": "The quality of the products ___ decreased over the years.", "id": "Jawaban: has. Penjelasan: Subjek kalimatnya adalah 'The quality', yang tunggal, sehingga diperlukan kata kerja tunggal 'has'." },
  { "en": "The professor recommended that he ___ his paper again.", "id": "Jawaban: write. Penjelasan: Setelah kata kerja seperti 'recommend' + 'that', digunakan subjunctive mood (bentuk dasar kata kerja)." },
  { "en": "It is essential that all passengers ___ their seatbelts.", "id": "Jawaban: fasten. Penjelasan: Setelah frasa 'It is essential that...', digunakan subjunctive mood (bentuk dasar kata kerja)." },
  { "en": "The law requires that every citizen ___ taxes.", "id": "Jawaban: pay. Penjelasan: Kata kerja 'require' yang diikuti oleh 'that' memicu penggunaan subjunctive." },
  { "en": "Her boss insisted that she ___ the meeting.", "id": "Jawaban: attend. Penjelasan: Kata kerja 'insist' yang diikuti oleh 'that' memicu penggunaan subjunctive." },
  { "en": "It is vital that you ___ on time.", "id": "Jawaban: be. Penjelasan: Frasa 'It is vital that...' memicu penggunaan subjunctive. Bentuk dasar dari 'to be' adalah 'be'." },
  { "en": "So confusing ___ the instructions that nobody understood them.", "id": "Jawaban: were. Penjelasan: Untuk penekanan, ketika kalimat dimulai dengan 'So + adjective', kata kerja dan subjeknya dibalik. 'Instructions' adalah jamak, jadi 'were' digunakan." },
  { "en": "Such ___ the force of the storm that trees were uprooted.", "id": "Jawaban: was. Penjelasan: Ketika sebuah kalimat dimulai dengan 'Such', kata kerja ('was') sering mendahului subjek frasa benda ('the force')." },
  { "en": "Were I to win the lottery, I ___ a new house.", "id": "Jawaban: would buy. Penjelasan: Ini adalah kalimat conditional kedua terbalik, yang berarti 'If I were to win...'." },
  { "en": "Only by working hard ___ you succeed.", "id": "Jawaban: will. Penjelasan: Ketika sebuah kalimat dimulai dengan 'Only by...', klausa utamanya dibalik (kata kerja bantu + subjek)." },
  { "en": "At no time ___ the doors be left unlocked.", "id": "Jawaban: should. Penjelasan: Frasa negatif 'At no time' di awal kalimat memerlukan pembalikan subjek dan kata kerja modal." },
  { "en": "The problem is ___ to be more serious than first thought.", "id": "Jawaban: considered. Penjelasan: Ini adalah struktur infinitif pasif, 'to be + past participle'." },
  { "en": "He remembers ___ to the zoo as a child.", "id": "Jawaban: being taken. Penjelasan: Ini adalah gerund pasif. Dia adalah orang yang dibawa (pasif), jadi 'being + past participle' digunakan setelah 'remembers'." },
  { "en": "New employees must ___ a security check.", "id": "Jawaban: be given. Penjelasan: Ini adalah konstruksi kalimat pasif dengan kata kerja modal ('must be + past participle')." },
  { "en": "The proposal is currently ___ reviewed by the board.", "id": "Jawaban: being. Penjelasan: Ini adalah present continuous passive voice ('is being + past participle')." },
  { "en": "Having ___ about the opportunity, he applied immediately.", "id": "Jawaban: been told. Penjelasan: Ini adalah frasa perfect passive participle, menunjukkan bahwa dia diberitahu sebelum dia melamar." },
  { "en": "After finishing the report, ___ was sent to the manager.", "id": "Jawaban: it. Penjelasan: Subjek 'it' diperlukan agar klausa utama terhubung secara logis dengan frasa pembuka. Tanpa 'it', frasa tersebut akan salah memodifikasi 'the manager'." },
  { "en": "Walking down the street, ___ caught my eye.", "id": "Jawaban: a beautiful display. Penjelasan: Frasa pembuka 'Walking down the street' harus memodifikasi subjek kalimat (pajangan yang indah)." },
  { "en": "To apply for this job, ___ must be submitted by Friday.", "id": "Jawaban: an application. Penjelasan: Frasa infinitif 'To apply for this job' harus merujuk pada subjek kalimat, yaitu 'an application' (lamaran)." },
  { "en": "While still in college, ___ on his first novel.", "id": "Jawaban: he began working. Penjelasan: Klausa pembuka 'While still in college' harus menggambarkan subjek dari klausa utama, yaitu 'he'." },
  { "en": "The team's success can be attributed ___ their hard work.", "id": "Jawaban: to. Penjelasan: Kolokasi yang benar adalah 'to attribute something to something' (mengaitkan sesuatu dengan sesuatu)." },
  { "en": "She prides herself ___ her ability to speak three languages.", "id": "Jawaban: on. Penjelasan: Kolokasi yang benar adalah 'to pride oneself on something' (bangga akan sesuatu)." },
  { "en": "The new law will go ___ effect next month.", "id": "Jawaban: into. Penjelasan: Idiomnya adalah 'to go into effect', yang berarti mulai berlaku." },
  { "en": "He was preoccupied ___ his own thoughts.", "id": "Jawaban: with. Penjelasan: Kata sifat 'preoccupied' diikuti oleh preposisi 'with'." },
  { "en": "The curriculum is designed to be compatible ___ the needs of the students.", "id": "Jawaban: with. Penjelasan: Kata sifat 'compatible' diikuti oleh preposisi 'with'." },
  { "en": "___ he said was not true.", "id": "Jawaban: What. Penjelasan: Klausa kata benda 'What he said' adalah subjek kalimat. 'What' berarti 'hal yang'." },
  { "en": "___ the team will win the championship is uncertain.", "id": "Jawaban: Whether. Penjelasan: Klausa kata benda 'Whether the team will win...' adalah subjeknya. 'Whether' digunakan untuk memperkenalkan keraguan atau pilihan." },
  { "en": "___ is responsible for the damage must pay for the repairs.", "id": "Jawaban: Whoever. Penjelasan: Klausa kata benda 'Whoever is responsible...' adalah subjeknya. 'Whoever' berarti 'siapa pun yang'." },
  { "en": "___ the Earth is round is a well-known fact.", "id": "Jawaban: That. Penjelasan: Klausa kata benda 'That the Earth is round' berfungsi sebagai subjek dari kata kerja 'is'." },
  { "en": "He enjoys classical music, ___ his wife prefers jazz.", "id": "Jawaban: whereas. Penjelasan: 'Whereas' adalah konjungsi yang digunakan untuk menunjukkan kontras langsung antara dua gagasan." },
  { "en": "You can borrow the car, ___ that you return it by 6 PM.", "id": "Jawaban: provided. Penjelasan: 'Provided that' adalah konjungsi yang berarti 'jika' atau 'dengan syarat bahwa'." },
  { "en": "The research is important ___ it provides a new perspective on the issue.", "id": "Jawaban: inasmuch as. Penjelasan: 'Inasmuch as' adalah konjungsi formal yang berarti 'karena' atau 'sejauh'." },
  { "en": "He must be smart; ___, he wouldn't have been accepted into that university.", "id": "Jawaban: otherwise. Penjelasan: 'Otherwise' adalah kata keterangan penghubung yang digunakan untuk menunjukkan apa yang akan terjadi dalam keadaan yang berbeda." },
  { "en": "She worked two jobs ___ she could save money for college.", "id": "Jawaban: so that. Penjelasan: 'So that' adalah konjungsi yang digunakan untuk menunjukkan tujuan." },
  { "en": "There is ___ hope of finding any survivors.", "id": "Jawaban: little. Penjelasan: 'Little' (tanpa 'a') memiliki arti negatif, yang berarti hampir tidak ada harapan." },
  { "en": "___ of the participants was given a certificate.", "id": "Jawaban: Each. Penjelasan: 'Each' digunakan untuk merujuk pada setiap individu dalam sebuah kelompok dan menggunakan kata kerja tunggal ('was')." },
  { "en": "He has made ___ mistakes in his report, so he needs to revise it.", "id": "Jawaban: a few. Penjelasan: 'A few' berarti sejumlah kecil (beberapa). 'Few' (tanpa 'a') akan memiliki makna yang lebih negatif, menyiratkan tidak banyak." },
  { "en": "He has very ___ patience for incompetence.", "id": "Jawaban: little. Penjelasan: 'Patience' adalah kata benda yang tidak dapat dihitung, jadi 'little' digunakan. 'Very little' menekankan jumlah yang kecil." },
  { "en": "There isn't ___ sugar left; we need to buy some.", "id": "Jawaban: much. Penjelasan: 'Much' digunakan dengan kata benda yang tidak dapat dihitung dalam kalimat negatif." },
  { "en": "The report was ___ and to the point.", "id": "Jawaban: concise. Penjelasan: 'Concise' adalah kata sifat yang berarti singkat namun komprehensif. Ini paralel dengan 'to the point'." },
  { "en": "The ancient ruins were discovered ___ by accident.", "id": "Jawaban: purely. Penjelasan: 'Purely' adalah kata keterangan yang berarti 'hanya' atau 'sepenuhnya', memodifikasi 'by accident'." },
  { "en": "His argument was not very ___.", "id": "Jawaban: convincing. Penjelasan: 'Convincing' adalah kata sifat yang berarti mampu membuat seseorang percaya bahwa sesuatu itu benar atau nyata." },
  { "en": "The results of the study were ___ with previous findings.", "id": "Jawaban: consistent. Penjelasan: 'Consistent with' berarti sesuai dengan atau sejalan dengan." },
  { "en": "For a ___ of reasons, the project was delayed.", "id": "Jawaban: variety. Penjelasan: 'A variety of' adalah frasa umum yang berarti berbagai jenis." },
  { "en": "The city is known for its ___ architecture.", "id": "Jawaban: diverse. Penjelasan: 'Diverse' adalah kata sifat yang berarti menunjukkan banyak variasi." },
  { "en": "It is highly ___ that he will accept the offer.", "id": "Jawaban: unlikely. Penjelasan: 'Unlikely' adalah kata sifat yang berarti tidak mungkin terjadi." },
  { "en": "The changes were implemented ___ a three-month period.", "id": "Jawaban: over. Penjelasan: 'Over' digunakan di sini untuk berarti 'selama' atau 'sepanjang' suatu periode waktu." },
  { "en": "The two theories are ___ exclusive.", "id": "Jawaban: mutually. Penjelasan: 'Mutually exclusive' adalah kolokasi umum yang berarti kedua hal tersebut tidak mungkin benar pada saat yang bersamaan." },
  { "en": "The new system is far superior ___ the old one.", "id": "Jawaban: to. Penjelasan: Kata sifat 'superior' diikuti oleh preposisi 'to', bukan 'than'." },
  { "en": "It is ___ knowledge that the company is in financial trouble.", "id": "Jawaban: common. Penjelasan: 'Common knowledge' adalah idiom yang berarti sesuatu yang diketahui oleh kebanyakan orang." },
  { "en": "The contract is ___ to renewal in December.", "id": "Jawaban: subject. Penjelasan: 'Subject to' berarti tergantung pada atau dikondisikan oleh." },
  { "en": "The senator's speech was intended to ___ support for the new bill.", "id": "Jawaban: generate. Penjelasan: 'Generate' adalah kata kerja yang berarti menyebabkan sesuatu muncul atau terjadi." },
  { "en": "They failed to take ___ account the changing market conditions.", "id": "Jawaban: into. Penjelasan: Idiom 'to take something into account' berarti mempertimbangkan sesuatu saat membuat keputusan." },
  { "en": "The witness was unable to ___ the suspect in the lineup.", "id": "Jawaban: identify. Penjelasan: 'Identify' berarti mengenali atau menetapkan siapa atau apa seseorang atau sesuatu itu." },
  { "en": "The machine is complex; ___, its operation is quite simple.", "id": "Jawaban: however. Penjelasan: 'However' adalah kata keterangan penghubung yang digunakan untuk memperkenalkan pernyataan yang kontras dengan yang sebelumnya." },
  { "en": "His decision was based ___ on the financial report.", "id": "Jawaban: solely. Penjelasan: 'Solely' adalah kata keterangan yang berarti 'hanya' atau 'tidak melibatkan orang lain atau hal lain'." },
  { "en": "The museum has a ___ collection of modern art.", "id": "Jawaban: vast. Penjelasan: 'Vast' adalah kata sifat yang berarti memiliki jangkauan atau kuantitas yang sangat besar." },
  { "en": "She has a tendency ___ things until the last minute.", "id": "Jawaban: to postpone. Penjelasan: Kata benda 'tendency' diikuti oleh infinitif." },
  { "en": "The company is committed ___ providing excellent customer service.", "id": "Jawaban: to. Penjelasan: Frasa 'committed to' memerlukan preposisi 'to', dan diikuti oleh gerund ('providing')." },
  { "en": "The rules are applicable ___ all employees, without exception.", "id": "Jawaban: to. Penjelasan: Kata sifat 'applicable' diikuti oleh preposisi 'to'." },
  { "en": "The investigation brought several ___ facts to light.", "id": "Jawaban: disturbing. Penjelasan: 'Disturbing' adalah kata sifat partisipatif yang menggambarkan kata benda 'facts'." },
  { "en": "He was praised for his ___ handling of the crisis.", "id": "Jawaban: adept. Penjelasan: 'Adept' adalah kata sifat yang berarti sangat terampil atau mahir dalam sesuatu." },
  { "en": "This software is not ___ with your operating system.", "id": "Jawaban: compatible. Penjelasan: 'Compatible with' berarti dapat ada atau terjadi bersama tanpa konflik." },
  { "en": "The company's profits have remained ___ for the past two years.", "id": "Jawaban: stable. Penjelasan: 'Stable' adalah kata sifat yang berarti tidak mungkin berubah atau gagal." },
  { "en": "He has an ___ for detail that makes him a great editor.", "id": "Jawaban: eye. Penjelasan: 'To have an eye for detail' adalah idiom yang berarti pandai memperhatikan detail-detail kecil." },
  { "en": "Despite his initial reluctance, he ___ agreed to help.", "id": "Jawaban: eventually. Penjelasan: 'Eventually' adalah kata keterangan yang berarti pada akhirnya, terutama setelah penundaan yang lama." },
  { "en": "A thorough understanding of the topic is a ___ for this course.", "id": "Jawaban: prerequisite. Penjelasan: 'Prerequisite' adalah sesuatu yang diperlukan sebagai syarat sebelumnya agar hal lain terjadi." },
  { "en": "The new building will ___ the university's library and classrooms.", "id": "Jawaban: house. Penjelasan: 'House' digunakan di sini sebagai kata kerja yang berarti menyediakan ruang untuk atau menampung." },
  { "en": "The goal is to make the information easily ___ to everyone.", "id": "Jawaban: accessible. Penjelasan: 'Accessible' adalah kata sifat yang berarti dapat dijangkau atau dimasuki." },
  { "en": "His work is ___ of the best research in the field.", "id": "Jawaban: representative. Penjelasan: 'Representative of' berarti khas dari suatu kelas, kelompok, atau badan pendapat." },
  { "en": "The government has launched a new ___ to combat unemployment.", "id": "Jawaban: initiative. Penjelasan: 'Initiative' adalah rencana atau proses baru untuk mencapai sesuatu atau memecahkan masalah." },
  { "en": "The two countries agreed to ___ in the fields of science and technology.", "id": "Jawaban: cooperate. Penjelasan: 'Cooperate' berarti bekerja sama untuk tujuan yang sama." },
  { "en": "The instructions were ___ and difficult to follow.", "id": "Jawaban: ambiguous. Penjelasan: 'Ambiguous' berarti terbuka untuk lebih dari satu interpretasi; tidak memiliki satu makna yang jelas." },
  { "en": "She was chosen for the job on the ___ of her experience.", "id": "Jawaban: basis. Penjelasan: 'On the basis of' adalah frasa yang berarti 'karena' atau 'didasarkan pada'." },
  { "en": "He had to ___ his appointment with the doctor.", "id": "Jawaban: cancel. Penjelasan: 'Cancel' berarti memutuskan bahwa acara yang telah diatur tidak akan berlangsung." },
  { "en": "The effects of the new law have yet to be ___.", "id": "Jawaban: determined. Penjelasan: 'To be determined' adalah frasa infinitif pasif yang berarti untuk ditemukan atau ditetapkan." },
  { "en": "It is the responsibility of the government to ___ its citizens.", "id": "Jawaban: protect. Penjelasan: 'Protect' berarti menjaga agar aman dari bahaya atau cedera." },
  { "en": "The speaker's comments were ___ to the topic of the conference.", "id": "Jawaban: irrelevant. Penjelasan: 'Irrelevant to' berarti tidak berhubungan dengan atau tidak relevan dengan sesuatu." },
  { "en": "He is widely ___ as one of the best writers of his generation.", "id": "Jawaban: regarded. Penjelasan: 'To be regarded as' adalah frasa pasif yang berarti dianggap atau dipandang dengan cara tertentu." },
  { "en": "The committee is comprised ___ experts from various fields.", "id":"Jawaban: of. Penjelasan: Kata kerja 'comprise' dapat digunakan secara pasif sebagai 'to be comprised of', yang berarti 'terdiri dari'." },
  { "en": "The company has a strict policy ___ lateness.", "id": "Jawaban: regarding. Penjelasan: 'Regarding' adalah preposisi yang berarti 'tentang' atau 'mengenai'." },
  { "en": "___ to popular belief, the Earth is not a perfect sphere.", "id": "Jawaban: Contrary. Penjelasan: 'Contrary to popular belief' adalah idiom umum yang digunakan untuk memperkenalkan fakta yang berlawanan dengan apa yang dipikirkan kebanyakan orang." },
  { "en": "The chemical is ___ to the environment.", "id": "Jawaban: harmful. Penjelasan: 'Harmful to' berarti menyebabkan atau kemungkinan menyebabkan kerusakan." },
  { "en": "The evidence presented was not sufficient ___ a conviction.", "id": "Jawaban: for. Penjelasan: Kata sifat 'sufficient' diikuti oleh preposisi 'for' ketika mendahului kata benda." },
  { "en": "The author provides a ___ analysis of the historical event.", "id": "Jawaban: detailed. Penjelasan: 'Detailed' adalah kata sifat yang berarti memiliki banyak detail atau fakta." },
  { "en": "There has been a ___ decline in the company's profits.", "id": "Jawaban: steady. Penjelasan: 'Steady' adalah kata sifat yang berarti tetap dan konsisten." },
  { "en": "The purpose of the meeting is to ___ a solution to the problem.", "id": "Jawaban: find. Penjelasan: Infinitif 'to find' digunakan untuk menunjukkan tujuan dari pertemuan tersebut." },
  { "en": "He is ___ to winning the competition this year.", "id": "Jawaban: dedicated. Penjelasan: 'Dedicated to' berarti mengabdikan diri pada suatu tugas atau tujuan." },
  { "en": "The data is stored ___ on a secure server.", "id": "Jawaban: electronically. Penjelasan: 'Electronically' adalah kata keterangan yang memodifikasi kata kerja 'is stored'." },
  { "en": "He has a reputation ___ being difficult to work with.", "id": "Jawaban: for. Penjelasan: Frasanya adalah 'to have a reputation for something' atau 'for doing something'." },
  { "en": "The factory was shut down ___ to safety concerns.", "id": "Jawaban: due. Penjelasan: 'Due to' adalah frasa preposisional yang berarti 'karena'." },
  { "en": "The new research has ___ implications for the future of medicine.", "id": "Jawaban: profound. Penjelasan: 'Profound' adalah kata sifat yang berarti sangat besar atau intens." },
  { "en": "It is ___ for parents to support their children.", "id": "Jawaban: natural. Penjelasan: 'Natural' adalah kata sifat yang digunakan dalam struktur 'It is + adj + for someone + to do something'." },
  { "en": "This approach is fundamentally ___ from the previous one.", "id": "Jawaban: different. Penjelasan: 'Different from' adalah kolokasi standar dalam Bahasa Inggris Amerika." },
  { "en": "She adapted ___ to her new environment.", "id": "Jawaban: quickly. Penjelasan: 'Quickly' adalah kata keterangan yang memodifikasi kata kerja 'adapted'." },
  { "en": "His duties ___ of managing the team and reporting to the director.", "id": "Jawaban: consist. Penjelasan: Kata kerja 'consist of' berarti 'terdiri dari'." },
  { "en": "The book provides a critical ___ of the government's policies.", "id": "Jawaban: assessment. Penjelasan: 'Assessment' adalah kata benda yang berarti evaluasi atau penilaian." },
  { "en": "The organization works to ___ the rights of children.", "id": "Jawaban: promote. Penjelasan: 'Promote' berarti mendukung atau secara aktif mendorong suatu tujuan." },
  { "en": "The negotiations ___ in a mutually beneficial agreement.", "id": "Jawaban: resulted. Penjelasan: Kata kerja 'result in' berarti menghasilkan suatu hasil tertentu." },
  { "en": "The research team will ___ a study on the effects of climate change.", "id": "Jawaban: carry out. Penjelasan: 'Carry out' adalah kata kerja frasa yang berarti melakukan atau melaksanakan." },
  { "en": "Before making a decision, we need to ___ all the available options.", "id": "Jawaban: look into. Penjelasan: 'Look into' berarti menyelidiki atau memeriksa." },
  { "en": "The speaker ___ that the new policy would have significant consequences.", "id": "Jawaban: pointed out. Penjelasan: 'Point out' berarti menunjukkan atau menarik perhatian pada suatu fakta." },
  { "en": "The meeting has been ___ until next Friday.", "id": "Jawaban: put off. Penjelasan: 'Put off' berarti menunda." },
  { "en": "Scientists are trying to ___ a cure for the disease.", "id": "Jawaban: come up with. Penjelasan: 'Come up with' berarti memikirkan atau menciptakan sesuatu, seperti ide atau rencana." },
  { "en": "The new software is not only efficient but also ___.", "id": "Jawaban: easy to use. Penjelasan: Untuk menjaga struktur paralel dengan kata sifat 'efficient', diperlukan kata sifat atau frasa kata sifat lain." },
  { "en": "You can ___ pay by credit card or cash.", "id": "Jawaban: either. Penjelasan: Pasangan konjungsi korelatifnya adalah 'either...or'." },
  { "en": "Neither the manager ___ the employees were happy with the outcome.", "id": "Jawaban: nor. Penjelasan: Pasangan konjungsi korelatifnya adalah 'neither...nor'." },
  { "en": "The course covers ___ theoretical concepts and practical applications.", "id": "Jawaban: both. Penjelasan: Pasangan konjungsi korelatifnya adalah 'both...and'." },
  { "en": "The goal is not to punish but ___ behavior.", "id": "Jawaban: to correct. Penjelasan: Struktur paralel memerlukan infinitif ('to correct') agar sesuai dengan 'to punish'." },
  { "en": "Her new car is the same color ___ her old one.", "id": "Jawaban: as. Penjelasan: Struktur yang benar untuk perbandingan ini adalah 'the same...as'." },
  { "en": "This winter is not ___ cold as last year's.", "id": "Jawaban: so. Penjelasan: Struktur 'not so...as' atau 'not as...as' digunakan untuk perbandingan yang tidak setara." },
  { "en": "His qualifications are different ___ what the job requires.", "id": "Jawaban: from. Penjelasan: Dalam bahasa Inggris standar, kolokasinya adalah 'different from'." },
  { "en": "The more he read, ___ he understood the complexity of the issue.", "id": "Jawaban: the more. Penjelasan: Ini adalah struktur komparatif ganda yang digunakan untuk menunjukkan hubungan langsung." },
  { "en": "He speaks English ___ a native speaker.", "id": "Jawaban: like. Penjelasan: 'Like' digunakan sebagai preposisi untuk membuat perbandingan, yang berarti 'mirip dengan'." },
  { "en": "The country is experiencing rapid ___ growth.", "id": "Jawaban: economic. Penjelasan: 'Economic' adalah bentuk kata sifat yang diperlukan untuk mendeskripsikan kata benda 'growth'." },
  { "en": "For the ___ of the environment, we should recycle more.", "id": "Jawaban: protection. Penjelasan: 'Protection' adalah bentuk kata benda yang diperlukan setelah artikel 'the'." },
  { "en": "The artist is known for her ___ use of color.", "id": "Jawaban: creative. Penjelasan: 'Creative' adalah bentuk kata sifat yang diperlukan untuk mendeskripsikan kata benda 'use'." },
  { "en": "He explained the process very ___.", "id": "Jawaban: clearly. Penjelasan: 'Clearly' adalah bentuk kata keterangan yang diperlukan untuk memodifikasi kata kerja 'explained'." },
  { "en": "The ___ of the new policy has been widely debated.", "id": "Jawaban: effectiveness. Penjelasan: 'Effectiveness' adalah bentuk kata benda yang diperlukan untuk menjadi subjek kalimat." },
  { "en": "He went to the meeting ___ feeling very ill.", "id": "Jawaban: despite. Penjelasan: 'Despite' adalah preposisi yang digunakan untuk menunjukkan kontras dan diikuti oleh kata benda atau gerund ('feeling')." },
  { "en": "She studied hard ___ pass the exam.", "id": "Jawaban: in order to. Penjelasan: 'In order to' memperkenalkan klausa tujuan, menjelaskan mengapa dia belajar keras." },
  { "en": "___ the fact that he was tired, he continued working.", "id": "Jawaban: In spite of. Penjelasan: 'In spite of' adalah frasa preposisional yang digunakan untuk konsesi. Sering diikuti oleh 'the fact that'." },
  { "en": "The company lowered its prices ___ attract more customers.", "id": "Jawaban: so as to. Penjelasan: 'So as to' adalah cara lain untuk memperkenalkan klausa tujuan, mirip dengan 'in order to'." },
  { "en": "He is a great leader, ___ his lack of experience.", "id": "Jawaban: notwithstanding. Penjelasan: 'Notwithstanding' adalah preposisi formal yang berarti 'meskipun'." },
  { "en": "Every student must submit ___ assignment by Friday.", "id": "Jawaban: their. Penjelasan: 'Every student' secara gramatikal tunggal, tetapi kata ganti jamak netral-gender 'their' diterima secara luas dalam bahasa Inggris modern." },
  { "en": "The company will announce ___ decision next week.", "id": "Jawaban: its. Penjelasan: Perusahaan adalah entitas tunggal, jadi kata ganti posesif tunggal 'its' digunakan." },
  { "en": "The team members celebrated ___ victory.", "id": "Jawaban: their. Penjelasan: 'Team members' adalah subjek jamak, jadi kata ganti posesif jamak 'their' diperlukan." },
  { "en": "The professor, along with his students, presented ___ research at the conference.", "id": "Jawaban: his. Penjelasan: Subjeknya adalah 'The professor' (tunggal). Frasa 'along with his students' bersifat parenthetical, sehingga kata ganti merujuk kembali ke subjek utama." },
  { "en": "If anyone calls, please take a message for ___.", "id": "Jawaban: them. Penjelasan: Mirip dengan 'everyone', 'anyone' adalah tunggal tetapi sering menggunakan kata ganti jamak 'them' agar tetap netral-gender." },
  { "en": "___ Lake Superior is the largest of the Great Lakes.", "id": "Jawaban: (no article). Penjelasan: Umumnya, tidak ada artikel yang digunakan sebelum nama danau individu." },
  { "en": "He is studying the history of ___ United Kingdom.", "id": "Jawaban: the. Penjelasan: 'The' digunakan sebelum negara yang namanya jamak atau mengandung istilah politik seperti 'Kingdom', 'Republic', atau 'States'." },
  { "en": "___ honesty is a valuable quality.", "id": "Jawaban: (no article). Penjelasan: Tidak ada artikel yang digunakan dengan kata benda abstrak seperti 'honesty' ketika berbicara secara umum." },
  { "en": "She plays ___ piano beautifully.", "id": "Jawaban: the. Penjelasan: 'The' digunakan sebelum nama alat musik saat berbicara tentang memainkannya." },
  { "en": "They traveled to ___ Philippines for their vacation.", "id": "Jawaban: the. Penjelasan: 'The' digunakan sebelum nama negara yang berbentuk jamak, seperti Filipina, Belanda, dan Maladewa." },
  { "en": "If I had more time, I ___ join a gym.", "id": "Jawaban: could. Penjelasan: Dalam kalimat conditional kedua, 'could' dapat digunakan sebagai pengganti 'would' untuk menyatakan kemampuan atau kemungkinan." },
  { "en": "If he had studied more, he ___ have passed the exam.", "id": "Jawaban: might. Penjelasan: Dalam kalimat conditional ketiga, 'might have' dapat digunakan sebagai pengganti 'would have' untuk menyatakan kemungkinan di masa lalu." },
  { "en": "If you see any errors, you ___ let me know immediately.", "id": "Jawaban: should. Penjelasan: Dalam kalimat conditional pertama, 'should' dapat digunakan untuk memberikan nasihat atau menunjukkan kewajiban." },
  { "en": "She ___ have been on time if the traffic hadn't been so bad.", "id": "Jawaban: would. Penjelasan: Ini adalah struktur conditional ketiga standar ('would have' + past participle) untuk hasil hipotetis di masa lalu." },
  { "en": "If it were not for his help, we ___ still be working on this project.", "id": "Jawaban: would. Penjelasan: Ini adalah struktur conditional kedua yang menyatakan hasil hipotetis saat ini." },
  { "en": "The plan was rejected on the ___ that it was too expensive.", "id": "Jawaban: grounds. Penjelasan: Frasa 'on the grounds that' berarti 'dengan alasan bahwa'." },
  { "en": "The two artists have ___ different styles.", "id": "Jawaban: distinctly. Penjelasan: 'Distinctly' adalah kata keterangan yang berarti 'dengan cara yang jelas terlihat atau pasti'." },
  { "en": "He made a ___ effort to improve his work.", "id": "Jawaban: conscious. Penjelasan: 'Conscious' adalah kata sifat yang berarti disengaja atau disadari." },
  { "en": "The government must address the ___ needs of its citizens.", "id": "Jawaban: fundamental. Penjelasan: 'Fundamental' adalah kata sifat yang berarti 'membentuk dasar yang diperlukan; yang paling penting'." },
  { "en": "The new evidence ___ the defendant's story.", "id": "Jawaban: contradicts. Penjelasan: 'Contradicts' adalah kata kerja yang berarti 'menyangkal kebenaran (suatu pernyataan) dengan menyatakan sebaliknya'." },
  { "en": "Her contributions to the field of medicine are ___.", "id": "Jawaban: undeniable. Penjelasan: 'Undeniable' adalah kata sifat yang berarti 'tidak dapat disangkal atau diperdebatkan'." },
  { "en": "The process is ___ simple, but it requires precision.", "id": "Jawaban: relatively. Penjelasan: 'Relatively' adalah kata keterangan yang berarti 'dalam kaitannya, perbandingan, atau proporsi dengan sesuatu yang lain'." },
  { "en": "The factory must ___ with strict environmental regulations.", "id": "Jawaban: comply. Penjelasan: 'Comply with' adalah kata kerja frasa yang berarti bertindak sesuai dengan keinginan atau perintah." },
  { "en": "His theory is based on the ___ that all people are rational.", "id": "Jawaban: assumption. Penjelasan: 'Assumption' adalah kata benda yang berarti 'sesuatu yang diterima sebagai benar atau pasti akan terjadi, tanpa bukti'." },
  { "en": "The company is seeking to ___ its operations into Asia.", "id": "Jawaban: expand. Penjelasan: 'Expand' adalah kata kerja yang berarti menjadi atau membuat lebih besar atau lebih luas." },
  { "en": "The primary ___ of this study is to test the hypothesis.", "id": "Jawaban: objective. Penjelasan: 'Objective' adalah kata benda yang berarti 'sesuatu yang dituju atau dicari; sebuah tujuan'." },
  { "en": "The politician's comments ___ a great deal of controversy.", "id": "Jawaban: sparked. Penjelasan: 'Sparked' adalah kata kerja yang berarti 'memicu' atau 'menyebabkan dimulai'." },
  { "en": "We have ___ reason to believe that the project will be successful.", "id": "Jawaban: every. Penjelasan: Frasa 'every reason to believe' adalah idiom yang berarti ada alasan yang sangat kuat untuk mempercayai sesuatu." },
  { "en": "The issue is far more ___ than it appears at first glance.", "id": "Jawaban: complex. Penjelasan: 'Complex' adalah kata sifat yang berarti 'terdiri dari banyak bagian yang berbeda dan terhubung'." },
  { "en": "He is an ___ supporter of environmental protection.", "id": "Jawaban: avid. Penjelasan: 'Avid' adalah kata sifat yang berarti 'memiliki atau menunjukkan minat atau antusiasme yang besar terhadap sesuatu'." },
  { "en": "The book gives a fascinating ___ into the culture of ancient Rome.", "id": "Jawaban: insight. Penjelasan: 'Insight into' adalah kolokasi umum yang berarti 'pemahaman yang mendalam tentang'." },
  { "en": "The new drug has not yet been approved for ___ use.", "id": "Jawaban: widespread. Penjelasan: 'Widespread' adalah kata sifat yang berarti 'ditemukan atau didistribusikan di area yang luas atau banyak orang'." },
  { "en": "The company is known for its ___ to quality.", "id": "Jawaban: commitment. Penjelasan: 'Commitment to' adalah frasa benda yang berarti keadaan berdedikasi pada suatu tujuan atau aktivitas." },
  { "en": "The evidence is ___ to prove his guilt beyond a reasonable doubt.", "id": "Jawaban: insufficient. Penjelasan: 'Insufficient' adalah kata sifat yang berarti 'tidak cukup; tidak memadai'." },
  { "en": "The defendant was ___ of all charges.", "id": "Jawaban: acquitted. Penjelasan: 'Acquitted of' adalah istilah hukum yang berarti secara resmi dinyatakan tidak bersalah atas tuduhan pidana." },
  { "en": "The device can ___ slight changes in temperature.", "id": "Jawaban: detect. Penjelasan: 'Detect' adalah kata kerja yang berarti 'menemukan atau mengidentifikasi keberadaan'." },
  { "en": "They reached a ___ agreement after weeks of negotiation.", "id": "Jawaban: compromise. Penjelasan: 'Compromise agreement' adalah frasa benda untuk perjanjian yang dicapai oleh setiap pihak yang membuat konsesi." },
  { "en": "The rise in sea levels is a direct ___ of global warming.", "id": "Jawaban: consequence. Penjelasan: 'Consequence' adalah kata benda yang berarti 'hasil atau efek dari suatu tindakan atau kondisi'." },
  { "en": "His work has had a ___ impact on the industry.", "id": "Jawaban: lasting. Penjelasan: 'Lasting' adalah kata sifat yang berarti 'bertahan lama'." },
  { "en": "The system was designed to be ___ with existing technology.", "id": "Jawaban: integrated. Penjelasan: 'Integrated with' berarti digabungkan dengan untuk membentuk suatu kesatuan." },
  { "en": "It is ___ that all staff attend the safety training.", "id": "Jawaban: mandatory. Penjelasan: 'Mandatory' adalah kata sifat yang berarti 'diwajibkan oleh hukum atau peraturan; wajib'." },
  { "en": "The project was completed ahead of ___.", "id": "Jawaban: schedule. Penjelasan: Idiom 'ahead of schedule' berarti 'sebelum waktu yang direncanakan'." },
  { "en": "She has a ___ understanding of the subject.", "id": "Jawaban: comprehensive. Penjelasan: 'Comprehensive' adalah kata sifat yang berarti 'lengkap; mencakup semua atau hampir semua elemen'." },
  { "en": "The two departments often ___ on joint projects.", "id": "Jawaban: collaborate. Penjelasan: 'Collaborate' adalah kata kerja yang berarti 'bekerja sama dalam suatu proyek'." },
  { "en": "His promotion was a ___ of his hard work and dedication.", "id": "Jawaban: reflection. Penjelasan: 'Reflection' digunakan di sini untuk berarti 'indikasi atau hasil dari sesuatu'." },
  { "en": "The organization provides ___ aid to developing countries.", "id": "Jawaban: humanitarian. Penjelasan: 'Humanitarian' adalah kata sifat yang menggambarkan bantuan yang bertujuan untuk memajukan kesejahteraan manusia." },
  { "en": "He was ___ to accept the award on behalf of his team.", "id": "Jawaban: honored. Penjelasan: 'Honored' adalah kata sifat yang berarti 'merasa bangga dan senang'." },
  { "en": "The study's findings have been ___ by several other researchers.", "id": "Jawaban: validated. Penjelasan: 'Validated' adalah kata kerja yang berarti 'memeriksa atau membuktikan keabsahan atau keakuratan'." },
  { "en": "The castle is perched ___ on a steep cliff.", "id": "Jawaban: precariously. Penjelasan: 'Precariously' adalah kata keterangan yang berarti 'dengan cara yang tidak aman dan kemungkinan akan jatuh'." },
  { "en": "The company has a ___ advantage over its competitors.", "id": "Jawaban: significant. Penjelasan: 'Significant' adalah kata sifat yang berarti 'cukup besar atau penting untuk diperhatikan'." },
  { "en": "The library offers a quiet ___ for studying.", "id": "Jawaban: environment. Penjelasan: 'Environment' adalah kata benda yang berarti 'lingkungan atau kondisi di mana seseorang beroperasi'." },
  { "en": "The volcano is ___, but it could erupt at any time.", "id": "Jawaban: dormant. Penjelasan: 'Dormant' adalah kata sifat yang digunakan untuk menggambarkan gunung berapi yang tidak aktif untuk sementara waktu." },
  { "en": "The new manager hopes to ___ better communication within the team.", "id": "Jawaban: foster. Penjelasan: 'Foster' adalah kata kerja yang berarti 'mendorong perkembangan'." },
  { "en": "The experiment was designed to ___ a specific theory.", "id": "Jawaban: test. Penjelasan: 'Test' adalah kata kerja yang berarti 'melakukan prosedur untuk memeriksa kualitas, kinerja, atau keandalan sesuatu'." },
  { "en": "He is an expert in the ___ of international relations.", "id": "Jawaban: field. Penjelasan: 'Field' digunakan di sini untuk berarti 'bidang studi atau kegiatan'." },
  { "en": "The government plans to ___ the new tax policy next year.", "id": "Jawaban: implement. Penjelasan: 'Implement' adalah kata kerja yang berarti 'memberlakukan (keputusan, rencana, dll.)'." },
  { "en": "The painting is an excellent ___ of the artist's early style.", "id": "Jawaban: example. Penjelasan: 'Example' adalah kata benda yang berarti 'sesuatu yang menjadi ciri khas jenisnya atau menggambarkan aturan umum'." },
  { "en": "The company is ___ for its innovative products.", "id": "Jawaban: renowned. Penjelasan: 'Renowned for' adalah frasa yang berarti 'dikenal atau dibicarakan oleh banyak orang; terkenal'." },
  { "en": "She has a ___ talent for music.", "id": "Jawaban: remarkable. Penjelasan: 'Remarkable' adalah kata sifat yang berarti 'patut diperhatikan; mencolok'." },
  { "en": "The doctor gave him a ___ examination.", "id": "Jawaban: thorough. Penjelasan: 'Thorough' adalah kata sifat yang berarti 'lengkap dalam setiap detail'." },
  { "en": "The conference will ___ on topics related to renewable energy.", "id": "Jawaban: focus. Penjelasan: 'Focus on' adalah kata kerja frasa yang berarti 'berkonsentrasi pada'." },
  { "en": "His theory is not ___ by the available evidence.", "id": "Jawaban: supported. Penjelasan: 'Supported' dalam konteks ini berarti 'didukung' atau 'dikuatkan' oleh bukti." },
  { "en": "The ___ of the rainforest is a global concern.", "id": "Jawaban: destruction. Penjelasan: 'Destruction' adalah bentuk kata benda dari 'destroy' dan merupakan subjek dari kalimat." },
  { "en": "The two systems are ___ in many ways, but they also have key differences.", "id": "Jawaban: similar. Penjelasan: 'Similar' adalah kata sifat yang berarti 'menyerupai tanpa identik'." },
  { "en": "The project is a ___ venture between two leading companies.", "id": "Jawaban: joint. Penjelasan: 'Joint' adalah kata sifat yang berarti 'dibagi, dimiliki, atau dibuat oleh dua orang atau lebih secara bersama-sama'." },
  { "en": "They had to overcome ___ obstacles to achieve their goal.", "id": "Jawaban: numerous. Penjelasan: 'Numerous' adalah kata sifat yang berarti 'banyak jumlahnya'." },
  { "en": "The law is intended to protect ___ species.", "id": "Jawaban: endangered. Penjelasan: 'Endangered' adalah kata sifat partisipatif yang menggambarkan spesies yang berisiko punah." },
  { "en": "The results were ___ and required further investigation.", "id": "Jawaban: inconclusive. Penjelasan: 'Inconclusive' adalah kata sifat yang berarti 'tidak mengarah pada kesimpulan yang pasti'." },
  { "en": "She has made ___ progress in her recovery.", "id": "Jawaban: substantial. Penjelasan: 'Substantial' adalah kata sifat yang berarti 'penting, besar, atau bernilai'." },
  { "en": "He gave a ___ account of what had happened.", "id": "Jawaban: factual. Penjelasan: 'Factual' adalah kata sifat yang berarti 'berkaitan dengan apa yang sebenarnya terjadi daripada interpretasi'." },
  { "en": "The city has undergone a ___ transformation in the last decade.", "id": "Jawaban: radical. Penjelasan: 'Radical' adalah kata sifat yang berarti '(terutama tentang perubahan) yang berkaitan dengan sifat dasar sesuatu'." },
  { "en": "The program is designed to be ___ to the needs of each student.", "id": "Jawaban: adaptable. Penjelasan: 'Adaptable' adalah kata sifat yang berarti 'mampu menyesuaikan diri dengan kondisi baru'." },
  { "en": "The ___ of the study were published in a scientific journal.", "id": "Jawaban: findings. Penjelasan: 'Findings' adalah kata benda yang berarti 'informasi yang ditemukan sebagai hasil dari penyelidikan'." },
  { "en": "He is considered a ___ figure in modern art.", "id": "Jawaban: pivotal. Penjelasan: 'Pivotal' adalah kata sifat yang berarti 'sangat penting dalam kaitannya dengan perkembangan atau keberhasilan sesuatu yang lain'." },
  { "en": "The new vaccine is ___ to be highly effective.", "id": "Jawaban: believed. Penjelasan: Ini adalah struktur kalimat pasif ('is believed to be') yang digunakan untuk melaporkan pendapat umum." },
  { "en": "The tour guide gave us some interesting ___ about the history of the castle.", "id": "Jawaban: information. Penjelasan: 'Information' adalah kata benda yang tidak dapat dihitung (uncountable) dan tidak memiliki bentuk jamak '-s'." },
  { "en": "We don't have ___ luggage, just a couple of small bags.", "id": "Jawaban: much. Penjelasan: 'Luggage' adalah kata benda yang tidak dapat dihitung, jadi quantifier 'much' digunakan dalam kalimat negatif." },
  { "en": "The number of ___ applying for the job has increased significantly.", "id": "Jawaban: people. Penjelasan: 'The number of' diikuti oleh kata benda jamak yang dapat dihitung. 'People' adalah bentuk jamak dari 'person'." },
  { "en": "The company has invested in new ___ to improve production.", "id": "Jawaban: equipment. Penjelasan: 'Equipment' adalah kata benda yang tidak dapat dihitung dan tidak menggunakan bentuk jamak '-s'." },
  { "en": "These strange ___ have been observed in the night sky.", "id": "Jawaban: phenomena. Penjelasan: 'Phenomena' adalah bentuk jamak dari kata benda 'phenomenon'." },
  { "en": "She bought a ___ Italian leather jacket.", "id": "Jawaban: beautiful black. Penjelasan: Urutan kata sifat yang standar adalah opini ('beautiful') sebelum warna ('black') sebelum asal ('Italian')." },
  { "en": "He lives in a ___ house by the sea.", "id": "Jawaban: lovely little old. Penjelasan: Urutannya biasanya adalah opini ('lovely'), ukuran ('little'), dan usia ('old')." },
  { "en": "They found a ___ wooden box hidden under the floorboards.", "id": "Jawaban: small square. Penjelasan: Urutannya adalah ukuran ('small') sebelum bentuk ('square') sebelum bahan ('wooden')." },
  { "en": "___ no further questions, the meeting was adjourned.", "id": "Jawaban: There being. Penjelasan: Ini adalah 'absolute clause', sejenis klausa partisipial di mana subjeknya ('there') berbeda dari subjek klausa utama ('the meeting')." },
  { "en": "___ from a distance, the city looks like a collection of tiny lights.", "id": "Jawaban: Seen. Penjelasan: Ini adalah klausa partisipial pasif, yang disingkat dari 'When it is seen...'. Kota itu 'dilihat'." },
  { "en": "___ his keys, he could not get into his apartment.", "id": "Jawaban: Having lost. Penjelasan: 'Perfect participle' ('Having lost') menunjukkan bahwa tindakan kehilangan kunci terjadi sebelum dia mencoba masuk ke apartemennya." },
  { "en": "___ what to do, she asked for advice.", "id": "Jawaban: Not knowing. Penjelasan: Klausa 'present participle' ('Not knowing...') memberikan alasan untuk tindakan di klausa utama." },
  { "en": "The problem, ___ unresolved, continued to cause issues.", "id": "Jawaban: left. Penjelasan: 'Left' adalah 'past participle' yang digunakan untuk menyingkat klausa kata sifat 'which was left unresolved'." },
  { "en": "___ a new language can be a challenging but rewarding experience.", "id": "Jawaban: Learning. Penjelasan: Sebuah 'gerund' ('Learning') digunakan sebagai subjek dari sebuah kalimat." },
  { "en": "He went to the library ___ a book.", "id": "Jawaban: to borrow. Penjelasan: Sebuah 'infinitive' ('to borrow') digunakan untuk menyatakan tujuan dari tindakannya." },
  { "en": "I look forward to ___ from you soon.", "id": "Jawaban: hearing. Penjelasan: Kata kerja frasa 'look forward to' diikuti oleh 'gerund' ('hearing')." },
  { "en": "The manager postponed ___ a decision until more information was available.", "id": "Jawaban: making. Penjelasan: Kata kerja 'postpone' diikuti oleh 'gerund'." },
  { "en": "She is talented at ___ stories.", "id": "Jawaban: telling. Penjelasan: Sebuah preposisi ('at') diikuti oleh 'gerund'." },
  { "en": "She ___ arrives at the office before 9 AM.", "id": "Jawaban: always. Penjelasan: Kata keterangan frekuensi seperti 'always' biasanya diletakkan sebelum kata kerja utama." },
  { "en": "He drove ___ to avoid the potholes in the road.", "id": "Jawaban: carefully. Penjelasan: Kata keterangan cara seperti 'carefully' biasanya diletakkan setelah kata kerja atau frasa kerja." },
  { "en": "I have ___ finished my homework; I just have one question left.", "id": "Jawaban: almost. Penjelasan: Kata keterangan derajat seperti 'almost' diletakkan sebelum kata kerja atau kata sifat yang dimodifikasinya." },
  { "en": "He has ___ traveled outside of his home country.", "id": "Jawaban: never. Penjelasan: 'Never' adalah kata keterangan frekuensi yang diletakkan di antara kata kerja bantu ('has') dan kata kerja utama ('traveled')." },
  { "en": "The company performed well last quarter; ___, its stock price increased.", "id": "Jawaban: consequently. Penjelasan: 'Consequently' adalah kata keterangan transisi yang berarti 'sebagai hasilnya'." },
  { "en": "The plan is well-designed. ___, it requires a significant investment.", "id": "Jawaban: Nevertheless. Penjelasan: 'Nevertheless' digunakan untuk menunjukkan kontras, mirip dengan 'however'." },
  { "en": "The historical evidence is clear. ___, the new findings support the theory.", "id": "Jawaban: Furthermore. Penjelasan: 'Furthermore' digunakan untuk menambahkan informasi lain yang mendukung poin sebelumnya." },
  { "en": "He did not study for the test; ___, he failed.", "id": "Jawaban: hence. Penjelasan: 'Hence' adalah kata transisi formal yang berarti 'karena alasan ini' atau 'akibatnya'." },
  { "en": "My car, ___ I bought last year, has been very reliable.", "id": "Jawaban: which. Penjelasan: 'Which' digunakan untuk memperkenalkan klausa non-defining (tidak penting). Koma menunjukkan bahwa klausa tersebut adalah informasi tambahan. 'That' tidak bisa digunakan di sini." },
  { "en": "This is the only one of his books ___ has been translated into English.", "id": "Jawaban: that. Penjelasan: 'That' lebih disukai daripada 'which' dalam klausa defining, terutama setelah superlatif ('the only')." },
  { "en": "Summer is the season ___ many people go on vacation.", "id": "Jawaban: when. Penjelasan: 'When' adalah kata keterangan relatif yang digunakan untuk merujuk pada waktu ('the season')." },
  { "en": "I visited the town ___ my father grew up.", "id": "Jawaban: where. Penjelasan: 'Where' adalah kata keterangan relatif yang digunakan untuk merujuk pada tempat ('the town')." },
  { "en": "It ___ that the new policy will be announced next week.", "id": "Jawaban: is expected. Penjelasan: Ini adalah struktur pasif umum ('It is + past participle + that...') yang digunakan untuk melaporkan keyakinan atau harapan umum." },
  { "en": "The fugitive is believed ___ hiding in the mountains.", "id": "Jawaban: to be. Penjelasan: Ini adalah struktur pelaporan umum lainnya: 'Subject + is believed/said/thought + to + verb'." },
  { "en": "It was ___ that the company would merge with its competitor.", "id": "Jawaban: rumored. Penjelasan: Ini adalah struktur pelaporan pasif dalam bentuk lampau." },
  { "en": "Many historical artifacts were thought ___ lost forever.", "id": "Jawaban: to have been. Penjelasan: 'Perfect infinitive' ('to have been') digunakan untuk menunjukkan bahwa tindakan hilang terjadi sebelum waktu pemikiran." },
  { "en": "She is held in high ___ by all of her colleagues.", "id": "Jawaban: regard. Penjelasan: 'To be held in high regard' adalah idiom yang berarti dihormati dan dikagumi." },
  { "en": "We need to ___ advantage of this opportunity.", "id": "Jawaban: take. Penjelasan: 'To take advantage of' adalah kolokasi umum yang berarti memanfaatkan suatu kesempatan dengan baik." },
  { "en": "Her opinion is irrelevant ___ the matter at hand.", "id": "Jawaban: to. Penjelasan: Kolokasi yang benar adalah 'irrelevant to'." },
  { "en": "I started working here two years ___.", "id": "Jawaban: ago. Penjelasan: 'Ago' digunakan dengan 'simple past tense' untuk menghitung mundur dari masa sekarang." },
  { "en": "She has been studying English ___ five years.", "id": "Jawaban: for. Penjelasan: 'For' digunakan dengan 'perfect tenses' untuk menunjukkan durasi waktu." },
  { "en": "He hasn't seen his family ___ last Christmas.", "id": "Jawaban: since. Penjelasan: 'Since' digunakan dengan 'perfect tenses' untuk menunjukkan titik awal waktu." },
  { "en": "They lived in that house ___ a long time before moving.", "id": "Jawaban: for. Penjelasan: 'For' digunakan dengan 'simple past tense' untuk menggambarkan durasi waktu yang sudah selesai." },
  { "en": "It has been a year ___ I last saw him.", "id": "Jawaban: since. Penjelasan: 'Since' digunakan di sini untuk menghubungkan peristiwa masa lalu dengan kerangka waktu sekarang ('It has been a year')." },
  { "en": "The amount of work he does is ___.", "id": "Jawaban: amazing. Penjelasan: 'Amazing' adalah kata sifat yang menggambarkan kata benda 'amount'. 'Amazed' akan menggambarkan perasaan seseorang." },
  { "en": "The evidence was convincing; ___, the jury found him guilty.", "id": "Jawaban: therefore. Penjelasan: 'Therefore' adalah kata transisi yang berarti 'karena itu' atau 'akibatnya'." },
  { "en": "The more you ignore the problem, ___ it will become.", "id": "Jawaban: the worse. Penjelasan: Ini adalah struktur komparatif ganda: 'the more..., the worse...'." },
  { "en": "It is difficult to ___ the effects of the new law at this stage.", "id": "Jawaban: assess. Penjelasan: 'Assess' adalah kata kerja yang berarti 'mengevaluasi atau memperkirakan sifat, kemampuan, atau kualitas'." },
  { "en": "The university is known for its high ___ standards.", "id": "Jawaban: academic. Penjelasan: 'Academic' adalah bentuk kata sifat yang diperlukan untuk mendeskripsikan kata benda 'standards'." },
  { "en": "He has a ___ grasp of the main issues.", "id": "Jawaban: firm. Penjelasan: 'A firm grasp' adalah kolokasi umum yang berarti pemahaman yang kuat dan percaya diri." },
  { "en": "The region is ___ to earthquakes.", "id": "Jawaban: prone. Penjelasan: 'Prone to' adalah frasa kata sifat yang berarti 'cenderung menderita, melakukan, atau mengalami sesuatu'." },
  { "en": "The two companies operate independently of ___ another.", "id": "Jawaban: one. Penjelasan: Frasanya adalah 'one another' (atau 'each other'), digunakan untuk tindakan timbal balik." },
  { "en": "She has made a ___ contribution to the project.", "id": "Jawaban: valuable. Penjelasan: 'Valuable' adalah kata sifat yang berarti 'sangat berharga' atau 'sangat berguna atau penting'." },
  { "en": "His comments were not ___ for the situation.", "id": "Jawaban: appropriate. Penjelasan: 'Appropriate' adalah kata sifat yang berarti 'sesuai atau pantas dalam situasi tersebut'." },
  { "en": "___ of the bad weather, the flight was delayed.", "id": "Jawaban: Because. Penjelasan: 'Because of' adalah frasa preposisional yang digunakan untuk memberikan alasan, diikuti oleh frasa kata benda." },
  { "en": "The purpose of the research is ___ a new theory.", "id": "Jawaban: to develop. Penjelasan: Sebuah 'infinitive' ('to develop') digunakan untuk menyatakan tujuan." },
  { "en": "I am not accustomed to ___ in such a hot climate.", "id": "Jawaban: living. Penjelasan: Frasa 'be accustomed to' diikuti oleh 'gerund' ('living')." },
  { "en": "The discovery of penicillin was a ___ in the history of medicine.", "id": "Jawaban: milestone. Penjelasan: 'Milestone' adalah kata benda yang berarti 'tindakan atau peristiwa yang menandai perubahan atau tahap penting'." },
  { "en": "The government took measures to ___ the economy.", "id": "Jawaban: stimulate. Penjelasan: 'Stimulate' adalah kata kerja yang berarti mendorong atau meningkatkan tingkat aktivitas." },
  { "en": "The artist's work is on ___ at the local gallery.", "id": "Jawaban: display. Penjelasan: Idiom 'on display' berarti 'dipamerkan untuk umum'." },
  { "en": "The negotiations broke ___ without an agreement.", "id": "Jawaban: down. Penjelasan: Kata kerja frasa 'break down' bisa berarti 'gagal atau runtuh'." },
  { "en": "He is ___ regarded as an expert in his field.", "id": "Jawaban: highly. Penjelasan: 'Highly' adalah kata keterangan yang digunakan untuk menambah penekanan pada 'regarded'." },
  { "en": "The law applies to all citizens, ___ of their age or status.", "id": "Jawaban: regardless. Penjelasan: 'Regardless of' adalah frasa yang berarti 'tanpa memperhatikan'." },
  { "en": "He is fluent ___ both Spanish and Portuguese.", "id": "Jawaban: in. Penjelasan: Kata sifat 'fluent' diikuti oleh preposisi 'in'." },
  { "en": "The study was limited in ___ because of its small sample size.", "id": "Jawaban: scope. Penjelasan: 'Scope' adalah kata benda yang berarti 'cakupan area atau materi pelajaran yang dibahas'." },
  { "en": "The final exam will account ___ 40% of the total grade.", "id": "Jawaban: for. Penjelasan: Kata kerja frasa 'account for' berarti 'membentuk sejumlah atau bagian tertentu dari sesuatu'." },
  { "en": "The new product is designed to ___ the needs of modern consumers.", "id": "Jawaban: meet. Penjelasan: 'To meet the needs' adalah kolokasi umum yang berarti 'memenuhi kebutuhan'." },
  { "en": "His work is a testament ___ his dedication and talent.", "id": "Jawaban: to. Penjelasan: 'A testament to' adalah frasa yang berarti 'bukti dari'." },
  { "en": "The city's infrastructure is in ___ need of an upgrade.", "id": "Jawaban: dire. Penjelasan: 'Dire' adalah kata sifat yang berarti 'sangat serius atau mendesak'." },
  { "en": "She was unable to attend the meeting ___ a prior commitment.", "id": "Jawaban: due to. Penjelasan: 'Due to' adalah frasa yang berarti 'karena'." },
  { "en": "The two countries have strong economic ___.", "id": "Jawaban: ties. Penjelasan: 'Economic ties' adalah frasa benda umum yang merujuk pada hubungan ekonomi antar negara." },
  { "en": "The company has ___ on a new marketing strategy.", "id": "Jawaban: decided. Penjelasan: Kata kerja 'decide on' berarti memilih sesuatu dari sejumlah kemungkinan." },
  { "en": "The professor's explanation was ___ and easy to understand.", "id": "Jawaban: lucid. Penjelasan: 'Lucid' adalah kata sifat yang berarti 'dinyatakan dengan jelas; mudah dimengerti'." },
  { "en": "He is responsible for ___ that the project is completed on time.", "id": "Jawaban: ensuring. Penjelasan: Setelah preposisi ('for'), diperlukan 'gerund' ('ensuring')." },
  { "en": "The author draws an ___ between the human brain and a computer.", "id": "Jawaban: analogy. Penjelasan: 'To draw an analogy' adalah idiom yang berarti membuat perbandingan antara dua hal." },
  { "en": "The hotel is ___ located near the city center.", "id": "Jawaban: conveniently. Penjelasan: 'Conveniently' adalah kata keterangan yang memodifikasi 'located'." },
  { "en": "It is ___ to assume that he will agree to the proposal.", "id": "Jawaban: reasonable. Penjelasan: 'Reasonable' adalah kata sifat yang berarti 'adil dan masuk akal'." },
  { "en": "The course provides students with the ___ skills for a career in marketing.", "id": "Jawaban: necessary. Penjelasan: 'Necessary' adalah kata sifat yang berarti 'diperlukan untuk dilakukan, dicapai, atau ada; penting'." },
  { "en": "The mountain's peak is ___ visible from the town.", "id": "Jawaban: rarely. Penjelasan: 'Rarely' adalah kata keterangan frekuensi." },
  { "en": "The new law has been the ___ of much debate.", "id": "Jawaban: subject. Penjelasan: 'The subject of debate' adalah frasa yang umum." },
  { "en": "The two designs are ___ identical, but there are minor differences.", "id": "Jawaban: virtually. Penjelasan: 'Virtually' adalah kata keterangan yang berarti 'hampir; nyaris'." },
  { "en": "The experiment's results were ___ by external factors.", "id": "Jawaban: affected. Penjelasan: 'Affected' adalah bentuk kata kerja yang berarti 'dipengaruhi'." },
  { "en": "There is ___ evidence to support his claim.", "id": "Jawaban: ample. Penjelasan: 'Ample' adalah kata sifat yang berarti 'cukup atau lebih dari cukup; berlimpah'." },
  { "en": "The library is ___ to university students and staff only.", "id": "Jawaban: accessible. Penjelasan: 'Accessible to' berarti 'dapat dijangkau atau digunakan oleh'." },
  { "en": "The government has put a greater ___ on education.", "id": "Jawaban: emphasis. Penjelasan: 'To put an emphasis on' adalah idiom yang berarti memberikan kepentingan khusus pada sesuatu." },
  { "en": "The treaty was a ___ achievement in international diplomacy.", "id": "Jawaban: landmark. Penjelasan: 'Landmark achievement' adalah peristiwa atau pencapaian penting yang menandai titik balik." },
  { "en": "She has always ___ to be a successful writer.", "id": "Jawaban: aspired. Penjelasan: 'To aspire to be' adalah frasa kerja yang berarti memiliki keinginan kuat untuk mencapai sesuatu." },
  { "en": "His decision to resign was ___, to say the least.", "id": "Jawaban: unexpected. Penjelasan: 'Unexpected' adalah kata sifat yang berarti 'tidak diharapkan atau dianggap mungkin terjadi'." },
  { "en": "The company must ___ to the new safety standards.", "id": "Jawaban: adhere. Penjelasan: 'To adhere to' adalah kata kerja formal yang berarti mematuhi atau mengikuti aturan atau standar." },
  { "en": "The information is ___ from a reliable source.", "id": "Jawaban: derived. Penjelasan: 'Derived from' berarti 'diperoleh dari'." },
  { "en": "He spoke ___ on a wide range of topics.", "id": "Jawaban: eloquently. Penjelasan: 'Eloquently' adalah kata keterangan yang berarti 'dengan cara yang fasih atau persuasif'." },
  { "en": "The agreement is ___ on both parties.", "id": "Jawaban: binding. Penjelasan: 'Binding' adalah kata sifat yang berarti 'mengenakan kewajiban hukum'." },
  { "en": "They are working to find a ___ solution to the conflict.", "id": "Jawaban: peaceful. Penjelasan: 'Peaceful' adalah kata sifat yang diperlukan untuk menggambarkan kata benda 'solution'." },
  { "en": "It was the manager ___ approved the final budget.", "id": "Jawaban: that. Penjelasan: Ini adalah 'cleft sentence' untuk penekanan. Strukturnya adalah 'It was [noun] that/who [verb phrase]'. 'That' umum digunakan untuk orang dan benda." },
  { "en": "___ I need right now is a long vacation.", "id": "Jawaban: What. Penjelasan: Ini adalah 'pseudo-cleft sentence' yang dimulai dengan 'What'. Seluruh klausa 'What I need right now' berfungsi sebagai subjek." },
  { "en": "It is in this city ___ the treaty was signed.", "id": "Jawaban: that. Penjelasan: 'Cleft sentence' dapat menekankan frasa preposisional: 'It is [prep. phrase] that [clause]'." },
  { "en": "___ surprised the investors was the company's sudden bankruptcy.", "id": "Jawaban: What. Penjelasan: Sebuah 'wh-cleft sentence' di mana 'What surprised the investors' berfungsi sebagai subjek dari kata kerja 'was'." },
  { "en": "It was not until yesterday ___ I received the news.", "id": "Jawaban: that. Penjelasan: Ini adalah struktur 'cleft' umum yang digunakan dengan 'not until' untuk penekanan." },
  { "en": "I can't find my keys ___.", "id": "Jawaban: anywhere. Penjelasan: Dalam kalimat negatif, 'anywhere' digunakan sebagai pengganti 'somewhere'. Menggunakan 'nowhere' akan menciptakan 'double negative'." },
  { "en": "He had ___ money left after his trip.", "id": "Jawaban: no. Penjelasan: 'No' digunakan sebagai 'determiner' sebelum kata benda untuk menyatakan kuantitas negatif." },
  { "en": "It is not ___ for students to feel nervous before an exam.", "id": "Jawaban: uncommon. Penjelasan: 'Not uncommon' adalah bentuk 'litotes' (pernyataan yang diperhalus) yang berarti 'umum' atau 'biasa'." },
  { "en": "She could ___ believe what she was hearing.", "id": "Jawaban: hardly. Penjelasan: 'Hardly' adalah kata keterangan negatif yang berarti 'hampir tidak'. Menggunakan 'not' bersamanya akan menjadi 'double negative'." },
  { "en": "There isn't ___ reason to doubt his story.", "id": "Jawaban: any. Penjelasan: 'Any' digunakan dengan kata kerja negatif yang berarti 'tidak satu pun' atau 'sama sekali tidak'." },
  { "en": "I wish I ___ more about the topic before the discussion.", "id": "Jawaban: had known. Penjelasan: Untuk penyesalan di masa lalu, 'wish' diikuti oleh 'past perfect tense' ('had known')." },
  { "en": "He wishes he ___ taller.", "id": "Jawaban: were. Penjelasan: Untuk keinginan masa kini yang bertentangan dengan fakta, 'wish' diikuti oleh 'simple past'. Subjunctive 'were' digunakan untuk 'to be'." },
  { "en": "If only I ___ to her advice.", "id": "Jawaban: had listened. Penjelasan: 'If only' berfungsi seperti 'wish' dan menggunakan 'past perfect' untuk penyesalan di masa lalu." },
  { "en": "She wishes she ___ travel more often.", "id": "Jawaban: could. Penjelasan: Untuk keinginan masa kini tentang kemampuan, 'wish' diikuti oleh 'could'." },
  { "en": "They wish the rain ___ stop so they can go outside.", "id": "Jawaban: would. Penjelasan: 'Wish' diikuti oleh 'would' untuk menyatakan keinginan agar suatu situasi berubah atau seseorang mengubah perilakunya." },
  { "en": "I saw the man ___ out of the building.", "id": "Jawaban: run. Penjelasan: Setelah kata kerja persepsi ('saw'), bentuk dasar kata kerja ('run') digunakan untuk menunjukkan bahwa seluruh tindakan disaksikan." },
  { "en": "We watched the sun ___ over the horizon.", "id": "Jawaban: setting. Penjelasan: Setelah kata kerja persepsi ('watched'), 'present participle' ('-ing') dapat digunakan untuk menekankan sifat berkelanjutan dari tindakan tersebut." },
  { "en": "Did you hear the baby ___ last night?", "id": "Jawaban: cry. Penjelasan: Setelah 'hear', bentuk dasar dari kata kerja digunakan." },
  { "en": "I can feel the ground ___ beneath my feet.", "id": "Jawaban: shaking. Penjelasan: 'Present participle' menekankan tindakan berkelanjutan yang dirasakan." },
  { "en": "The main cause of these accidents ___ driver error.", "id": "Jawaban: is. Penjelasan: Subjeknya adalah 'cause' (tunggal), bukan 'accidents' (jamak). Kata kerja harus setuju dengan 'cause'." },
  { "en": "The quality of the products ___ declined recently.", "id": "Jawaban: has. Penjelasan: Subjeknya adalah 'quality' (tunggal), bukan 'products' (jamak). Kata kerjanya harus tunggal ('has')." },
  { "en": "A box of old letters ___ found in the attic.", "id": "Jawaban: was. Penjelasan: Subjeknya adalah 'A box' (tunggal), sehingga diperlukan kata kerja tunggal 'was'." },
  { "en": "The list of candidates ___ not been finalized yet.", "id": "Jawaban: has. Penjelasan: Subjeknya adalah 'The list' (tunggal), bukan 'candidates'." },
  { "en": "The CEO, together with the board members, ___ to make a statement.", "id": "Jawaban: is. Penjelasan: Subjeknya adalah 'The CEO' (tunggal). Frasa 'together with...' bersifat parenthetical dan tidak mempengaruhi jumlah kata kerja." },
  { "en": "Some of the information ___ out of date.", "id": "Jawaban: is. Penjelasan: Ketika objek preposisi adalah kata benda yang tidak dapat dihitung ('information'), kata kerjanya tunggal." },
  { "en": "Most of the students ___ passed the exam.", "id": "Jawaban: have. Penjelasan: Ketika objek preposisi adalah kata benda jamak ('students'), kata kerjanya jamak." },
  { "en": "None of the equipment ___ working properly.", "id": "Jawaban: is. Penjelasan: 'None of' yang diikuti oleh kata benda yang tidak dapat dihitung ('equipment') menggunakan kata kerja tunggal." },
  { "en": "All of the cake ___ been eaten.", "id": "Jawaban: has. Penjelasan: 'All of' dengan kata benda yang tidak dapat dihitung ('cake') menggunakan kata kerja tunggal." },
  { "en": "Half of the apples ___ rotten.", "id": "Jawaban: are. Penjelasan: 'Half of' dengan kata benda jamak ('apples') menggunakan kata kerja jamak." },
  { "en": "I have finished this book; can I borrow ___ one?", "id": "Jawaban: another. Penjelasan: 'Another' berarti 'satu lagi' atau 'yang lain' dan digunakan dengan kata benda tunggal." },
  { "en": "Some people like to travel; ___ prefer to stay at home.", "id": "Jawaban: others. Penjelasan: 'Others' adalah kata ganti yang digunakan untuk merujuk pada orang atau benda lain secara umum." },
  { "en": "This shoe fits perfectly, but ___ one is too small.", "id": "Jawaban: the other. Penjelasan: 'The other' digunakan untuk merujuk pada item kedua dari dua item tertentu." },
  { "en": "She works much harder than ___.", "id": "Jawaban: he does. Penjelasan: Dalam bahasa Inggris formal, perbandingan diselesaikan dengan kata ganti subjek dan kata kerja bantu untuk menghindari ambiguitas." },
  { "en": "While ___ on vacation, he rarely checked his email.", "id": "Jawaban: he was. Penjelasan: Ini adalah klausa eliptis di mana subjek dan kata kerja ('he was') sering dihilangkan tetapi dapat dipahami." },
  { "en": "The government is taking steps to ___ the problem of unemployment.", "id": "Jawaban: tackle. Penjelasan: 'Tackle a problem' adalah kolokasi yang kuat yang berarti berusaha keras untuk menanganinya." },
  { "en": "His research provided ___ evidence for his theory.", "id": "Jawaban: empirical. Penjelasan: 'Empirical' adalah kata sifat yang berarti 'berdasarkan pengamatan atau pengalaman daripada teori atau logika murni'." },
  { "en": "The two countries have a ___ relationship, often disagreeing on policy.", "id": "Jawaban: contentious. Penjelasan: 'Contentious' adalah kata sifat yang berarti 'menyebabkan atau cenderung menyebabkan argumen; kontroversial'." },
  { "en": "The transition to a new system has not been entirely ___.", "id": "Jawaban: seamless. Penjelasan: 'Seamless' adalah kata sifat yang berarti 'lancar dan tanpa jeda atau sambungan yang jelas'." },
  { "en": "He has a ___ ability to simplify complex ideas.", "id": "Jawaban: unique. Penjelasan: 'Unique' adalah kata sifat yang berarti 'menjadi satu-satunya; tidak seperti yang lain'." },
  { "en": "The company is trying to ___ its public image.", "id": "Jawaban: enhance. Penjelasan: 'Enhance' adalah kata kerja yang berarti 'meningkatkan atau memperbaiki kualitas, nilai, atau jangkauan'." },
  { "en": "The new law places several ___ on small businesses.", "id": "Jawaban: constraints. Penjelasan: 'Constraints' adalah kata benda yang berarti 'batasan atau pembatasan'." },
  { "en": "The lecture was ___ to a wide audience.", "id": "Jawaban: geared. Penjelasan: 'Geared to' atau 'geared towards' adalah kata kerja frasa yang berarti 'dirancang atau diatur untuk tujuan atau audiens tertentu'." },
  { "en": "It is ___ that you read the contract carefully before signing.", "id": "Jawaban: imperative. Penjelasan: 'Imperative' adalah kata sifat yang berarti 'sangat penting; krusial'. Ini sering memicu subjunctive." },
  { "en": "The project was a ___ failure, costing the company millions.", "id": "Jawaban: colossal. Penjelasan: 'Colossal' adalah kata sifat yang berarti 'sangat besar'." },
  { "en": "The report highlights the ___ need for reform.", "id": "Jawaban: urgent. Penjelasan: 'Urgent' adalah kata sifat yang berarti 'membutuhkan tindakan atau perhatian segera'." },
  { "en": "The artist's early work was heavily ___ by cubism.", "id": "Jawaban: influenced. Penjelasan: 'Influenced' adalah 'past participle' yang digunakan dalam konstruksi pasif ini." },
  { "en": "We must use our natural resources ___.", "id": "Jawaban: wisely. Penjelasan: 'Wisely' adalah kata keterangan yang memodifikasi kata kerja 'use'." },
  { "en": "The defendant's alibi was ___ by two witnesses.", "id": "Jawaban: corroborated. Penjelasan: 'Corroborated' adalah kata kerja formal yang berarti 'mengkonfirmasi atau memberikan dukungan kepada (pernyataan, teori, atau temuan)'." },
  { "en": "The city is a ___ pot of different cultures.", "id": "Jawaban: melting. Penjelasan: 'Melting pot' adalah idiom terkenal untuk tempat di mana berbagai orang, gaya, teori, dll., bercampur menjadi satu." },
  { "en": "The internet has ___ the way we communicate.", "id": "Jawaban: revolutionized. Penjelasan: 'Revolutionized' adalah kata kerja yang berarti 'mengubah (sesuatu) secara radikal atau fundamental'." },
  { "en": "The company is facing ___ competition from foreign firms.", "id": "Jawaban: fierce. Penjelasan: 'Fierce competition' (persaingan sengit) adalah kolokasi yang kuat." },
  { "en": "He was forced to ___ his position as chairman.", "id": "Jawaban: relinquish. Penjelasan: 'Relinquish' adalah kata kerja formal yang berarti 'secara sukarela berhenti mempertahankan atau mengklaim; melepaskan'." },
  { "en": "His health has shown a ___ improvement.", "id": "Jawaban: marked. Penjelasan: 'Marked' adalah kata sifat yang berarti 'terlihat jelas; nyata'." },
  { "en": "The documentary offers a ___ glimpse into the lives of wolves.", "id": "Jawaban: rare. Penjelasan: 'Rare' adalah kata sifat yang berarti 'tidak sering terjadi'." },
  { "en": "The new software is not backwards ___ with older versions.", "id": "Jawaban: compatible. Penjelasan: 'Compatible with' berarti 'dapat ada atau terjadi bersama tanpa konflik'." },
  { "en": "The organization is ___ to protecting the environment.", "id": "Jawaban: dedicated. Penjelasan: 'Dedicated to' berarti 'mengabdikan diri pada suatu tugas atau tujuan'." },
  { "en": "He is a ___ of the company's new environmental policy.", "id": "Jawaban: proponent. Penjelasan: 'Proponent' adalah orang yang mendukung suatu teori, proposal, atau proyek." },
  { "en": "The rules are ___ and must be followed by everyone.", "id": "Jawaban: explicit. Penjelasan: 'Explicit' adalah kata sifat yang berarti 'dinyatakan dengan jelas dan terperinci, tidak meninggalkan ruang untuk kebingungan'." },
  { "en": "She has ___ a great deal of knowledge on the subject.", "id": "Jawaban: accumulated. Penjelasan: 'Accumulated' adalah kata kerja yang berarti 'mengumpulkan atau memperoleh sejumlah atau kuantitas yang meningkat'." },
  { "en": "The economic forecast for next year is ___.", "id": "Jawaban: bleak. Penjelasan: 'Bleak' adalah kata sifat yang berarti '(situasi) tidak ada harapan atau tidak menggembirakan'." },
  { "en": "The two events happened ___.", "id": "Jawaban: simultaneously. Penjelasan: 'Simultaneously' adalah kata keterangan yang berarti 'pada saat yang bersamaan'." },
  { "en": "The project is still in its ___ stages.", "id": "Jawaban: initial. Penjelasan: 'Initial' adalah kata sifat yang berarti 'ada atau terjadi di awal'." },
  { "en": "The company's success is ___ on its ability to innovate.", "id": "Jawaban: dependent. Penjelasan: 'Dependent on' berarti 'bergantung pada atau ditentukan oleh'." },
  { "en": "It is ___ to drink plenty of water in hot weather.", "id": "Jawaban: advisable. Penjelasan: 'Advisable' adalah kata sifat yang berarti 'dianjurkan; masuk akal'." },
  { "en": "The treaty aims to ___ peace between the two nations.", "id": "Jawaban: ensure. Penjelasan: 'Ensure' adalah kata kerja yang berarti 'memastikan bahwa (sesuatu) akan terjadi atau menjadi kenyataan'." },
  { "en": "The witness gave a ___ description of the suspect.", "id": "Jawaban: vague. Penjelasan: 'Vague' adalah kata sifat yang berarti 'tidak pasti, tidak jelas, atau tidak terang karakter atau maknanya'." },
  { "en": "His ___ to the team has been invaluable.", "id": "Jawaban: contribution. Penjelasan: 'Contribution' adalah bentuk kata benda yang diperlukan di sini." },
  { "en": "The museum's most prized ___ is a painting by Van Gogh.", "id": "Jawaban: possession. Penjelasan: 'Possession' adalah kata benda yang berarti 'barang milik; sesuatu yang dimiliki seseorang'." },
  { "en": "The effects of the drought will be ___-reaching.", "id": "Jawaban: far. Penjelasan: 'Far-reaching' adalah kata sifat majemuk yang berarti 'memiliki pengaruh atau efek yang luas'." },
  { "en": "The results of the study were not statistically ___.", "id": "Jawaban: significant. Penjelasan: 'Statistically significant' adalah frasa standar dalam penelitian, yang berarti hasilnya tidak mungkin terjadi secara kebetulan." },
  { "en": "The students were asked to give a ___ presentation.", "id": "Jawaban: brief. Penjelasan: 'Brief' adalah kata sifat yang berarti 'berdurasi singkat'." },
  { "en": "His leadership was ___ in resolving the conflict.", "id": "Jawaban: instrumental. Penjelasan: 'Instrumental in' adalah frasa yang berarti 'berperan sebagai sarana, agen, atau alat penting'." },
  { "en": "The population of the city has grown at an ___ rate.", "id": "Jawaban: unprecedented. Penjelasan: 'Unprecedented' adalah kata sifat yang berarti 'belum pernah dilakukan atau diketahui sebelumnya'." },
  { "en": "The company has a ___ for excellent customer service.", "id": "Jawaban: reputation. Penjelasan: 'Reputation for' berarti 'keyakinan atau pendapat yang umumnya dipegang tentang seseorang atau sesuatu'." },
  { "en": "The details of the plan are still ___.", "id": "Jawaban: tentative. Penjelasan: 'Tentative' adalah kata sifat yang berarti 'tidak pasti atau tetap; sementara'." },
  { "en": "The new ___ are intended to improve safety in the workplace.", "id": "Jawaban: regulations. Penjelasan: 'Regulations' adalah kata benda jamak yang berarti 'aturan atau arahan yang dibuat dan dipelihara oleh otoritas'." },
  { "en": "The forest is home to a ___ array of wildlife.", "id": "Jawaban: diverse. Penjelasan: 'Diverse' adalah kata sifat yang berarti 'menunjukkan banyak variasi'." },
  { "en": "His argument is based on a ___ premise.", "id": "Jawaban: flawed. Penjelasan: 'Flawed' adalah kata sifat yang berarti 'memiliki atau ditandai oleh kelemahan atau ketidaksempurnaan mendasar'." },
  { "en": "The decision was made by ___ among the committee members.", "id": "Jawaban: consensus. Penjelasan: 'Consensus' adalah kata benda yang berarti 'kesepakatan umum'." },
  { "en": "The athlete is at the ___ of her career.", "id": "Jawaban: peak. Penjelasan: 'The peak of her career' adalah idiom yang berarti titik paling sukses." },
  { "en": "The company is looking for ways to ___ its costs.", "id": "Jawaban: minimize. Penjelasan: 'Minimize' adalah kata kerja yang berarti 'mengurangi hingga jumlah atau tingkat sekecil mungkin'." },
  { "en": "The archaeological dig yielded some ___ artifacts.", "id": "Jawaban: priceless. Penjelasan: 'Priceless' adalah kata sifat yang berarti 'sangat berharga sehingga nilainya tidak dapat ditentukan'." },
  { "en": "The evidence is ___ and does not point to a single conclusion.", "id": "Jawaban: ambiguous. Penjelasan: 'Ambiguous' adalah kata sifat yang berarti 'terbuka untuk lebih dari satu interpretasi'." },
  { "en": "He took a ___ approach to solving the problem.", "id": "Jawaban: systematic. Penjelasan: 'Systematic' adalah kata sifat yang berarti 'dilakukan atau bertindak sesuai dengan rencana atau sistem yang tetap'." },
  { "en": "There has been a ___ shift in public opinion.", "id": "Jawaban: noticeable. Penjelasan: 'Noticeable' adalah kata sifat yang berarti 'mudah dilihat atau diperhatikan'." },
  { "en": "The negotiations reached a ___ and could not proceed.", "id": "Jawaban: stalemate. Penjelasan: 'Stalemate' adalah kata benda untuk situasi di mana tindakan atau kemajuan lebih lanjut oleh pihak yang berlawanan tampaknya mustahil." },
  { "en": "The two systems are essentially the same, ___ with minor variations.", "id": "Jawaban: albeit. Penjelasan: 'Albeit' adalah konjungsi formal yang berarti 'meskipun'." },
  { "en": "The book provides a ___ overview of the country's history.", "id": "Jawaban: concise. Penjelasan: 'Concise' adalah kata sifat yang berarti 'memberikan banyak informasi dengan jelas dan dalam beberapa kata'." },
  { "en": "The city has a ___ public transportation system.", "id": "Jawaban: reliable. Penjelasan: 'Reliable' adalah kata sifat yang berarti 'konsisten baik dalam kualitas atau kinerja'." },
  { "en": "The manager is ___ for the day-to-day operations of the store.", "id": "Jawaban: responsible. Penjelasan: 'Responsible for' berarti 'memiliki kewajiban untuk melakukan sesuatu'." },
  { "en": "The launch of the new product was ___ with a major marketing campaign.", "id": "Jawaban: coordinated. Penjelasan: 'Coordinated with' berarti 'diorganisir bersama dengan'." },
  { "en": "The company is vulnerable ___ a hostile takeover.", "id": "Jawaban: to. Penjelasan: Kata sifat 'vulnerable' diikuti oleh preposisi 'to'." },
  { "en": "The investigation ___ a number of irregularities.", "id": "Jawaban: uncovered. Penjelasan: 'Uncovered' adalah kata kerja yang berarti 'menemukan (sesuatu yang sebelumnya rahasia atau tidak diketahui)'." },
  { "en": "The long-term ___ of the new policy are still unknown.", "id": "Jawaban: ramifications. Penjelasan: 'Ramifications' adalah kata benda yang berarti 'konsekuensi yang rumit atau tidak diinginkan dari suatu tindakan atau peristiwa'." },
  { "en": "I need to ___ my car serviced before the long trip.", "id": "Jawaban: have. Penjelasan: Struktur 'causative passive' yaitu 'have something done' digunakan untuk mengatur agar orang lain melakukan sesuatu untuk Anda." },
  { "en": "She is going to get her portrait ___ by a famous artist.", "id": "Jawaban: painted. Penjelasan: Struktur 'get something done' memerlukan 'past participle' ('painted'). Artinya mirip dengan 'have something done'." },
  { "en": "The company had all its computers ___ with the new software.", "id": "Jawaban: updated. Penjelasan: Ini menggunakan 'past perfect causative passive' ('had...updated') untuk menunjukkan bahwa tindakan tersebut telah selesai." },
  { "en": "He couldn't fix the leak himself, so he ___ a plumber do it.", "id": "Jawaban: had. Penjelasan: Ini adalah causative aktif 'have someone do something' (tanpa 'to'), menggunakan kata kerja dasar 'do' setelah objek 'a plumber'." },
  { "en": "We must get this document ___ by a notary public.", "id": "Jawaban: signed. Penjelasan: Struktur 'get something done' digunakan dengan modal 'must'." },
  { "en": "When I was a child, I ___ play in this park every day.", "id": "Jawaban: used to. Penjelasan: 'Used to + kata kerja dasar' digunakan untuk menggambarkan tindakan atau keadaan berulang di masa lalu yang tidak lagi benar." },
  { "en": "He is not ___ the cold weather yet; he just moved from a tropical country.", "id": "Jawaban: used to. Penjelasan: 'Be used to + gerund/noun' berarti 'terbiasa dengan'. Bentuk negatifnya adalah 'be not used to'." },
  { "en": "It took me a while to get ___ driving on the left side of the road.", "id": "Jawaban: used to. Penjelasan: 'Get used to + gerund/noun' menggambarkan proses menjadi terbiasa dengan sesuatu." },
  { "en": "Did you ___ live in this neighborhood?", "id": "Jawaban: use to. Penjelasan: Dalam pertanyaan dan kalimat negatif dengan 'did', huruf 'd' pada 'used' dihilangkan, menjadi 'use to'." },
  { "en": "She is a flight attendant, so she is used to ___.", "id": "Jawaban: flying. Penjelasan: 'Be used to' diikuti oleh 'gerund' (bentuk -ing)." },
  { "en": "The movie was ___ good, but it wasn't the best I've ever seen.", "id": "Jawaban: quite. Penjelasan: 'Quite' adalah kata keterangan tingkat yang berarti 'lumayan' atau 'agak'." },
  { "en": "I would ___ not go out tonight; I'm very tired.", "id": "Jawaban: rather. Penjelasan: 'Would rather' adalah ekspresi yang digunakan untuk menyatakan preferensi." },
  { "en": "The temperature was ___ cold for this time of year.", "id": "Jawaban: extremely. Penjelasan: 'Extremely' adalah kata keterangan tingkat yang kuat yang digunakan untuk mengintensifkan kata sifat 'cold'." },
  { "en": "He speaks so fast that it's ___ impossible to understand him.", "id": "Jawaban: almost. Penjelasan: 'Almost' adalah kata keterangan tingkat yang berarti 'hampir'." },
  { "en": "The exam was ___ difficult, but most students managed to pass.", "id": "Jawaban: fairly. Penjelasan: 'Fairly' menunjukkan tingkat sedang dan sering digunakan dengan kata sifat positif atau netral." },
  { "en": "The lights are on, so they ___ be home.", "id": "Jawaban: must. Penjelasan: 'Must be' digunakan untuk menyatakan kesimpulan logis yang kuat atau kepastian tentang situasi saat ini." },
  { "en": "He didn't answer his phone. He ___ have been sleeping.", "id": "Jawaban: might. Penjelasan: 'Might have' digunakan untuk menyatakan kemungkinan tentang situasi di masa lalu." },
  { "en": "She looks very unhappy. She ___ have heard some bad news.", "id": "Jawaban: must. Penjelasan: 'Must have' digunakan untuk kesimpulan logis yang kuat tentang masa lalu." },
  { "en": "He just ate a huge meal. He ___ be hungry again already.", "id": "Jawaban: can't. Penjelasan: 'Can't be' digunakan untuk menyatakan bahwa sesuatu secara logis tidak mungkin terjadi saat ini." },
  { "en": "The package arrived this morning, but I didn't order anything. It ___ be for my roommate.", "id": "Jawaban: must. Penjelasan: Ini adalah kesimpulan logis tentang saat ini." },
  { "en": "In ___ to being a great musician, he is also a talented painter.", "id": "Jawaban: addition. Penjelasan: Frasa 'in addition to' diikuti oleh 'gerund' ('being')." },
  { "en": "He was criticized ___ not finishing the project on time.", "id": "Jawaban: for. Penjelasan: Preposisi 'for' digunakan untuk memberikan alasan kritik dan diikuti oleh 'gerund' ('not finishing')." },
  { "en": "The new law aims at ___ pollution in the city.", "id": "Jawaban: reducing. Penjelasan: Preposisi 'at' (dalam frasa 'aims at') diikuti oleh 'gerund'." },
  { "en": "She insisted ___ paying for everyone's dinner.", "id": "Jawaban: on. Penjelasan: Kata kerja 'insist' diikuti oleh preposisi 'on' dan kemudian 'gerund'." },
  { "en": "There is no point ___ about something you cannot change.", "id": "Jawaban: in worrying. Penjelasan: Ekspresi 'there is no point in' diikuti oleh 'gerund'." },
  { "en": "Could you please ___ the music down? It's too loud.", "id": "Jawaban: turn. Penjelasan: 'Turn down' adalah kata kerja frasa yang dapat dipisahkan. Objek 'the music' bisa diletakkan setelah partikel, atau di antara kata kerja dan partikel." },
  { "en": "I need to ___ this form out before I submit my application.", "id": "Jawaban: fill. Penjelasan: 'Fill out' adalah kata kerja frasa yang dapat dipisahkan." },
  { "en": "She had to ___ her children up from school at 3 PM.", "id": "Jawaban: pick. Penjelasan: 'Pick up' adalah kata kerja frasa yang dapat dipisahkan." },
  { "en": "Before you leave, please make sure to ___ all the lights.", "id": "Jawaban: turn off. Penjelasan: 'Turn off' dapat dipisahkan. Jika objeknya adalah kata ganti, harus diletakkan di tengah (turn them off)." },
  { "en": "He ___ the meeting off until the following week.", "id": "Jawaban: put. Penjelasan: 'Put off' berarti menunda dan dapat dipisahkan." },
  { "en": "The Amazon River flows through ___ South America.", "id": "Jawaban: (no article). Penjelasan: Tidak ada artikel yang digunakan sebelum nama sebagian besar benua." },
  { "en": "They went hiking in ___ Rocky Mountains last summer.", "id": "Jawaban: the. Penjelasan: 'The' digunakan sebelum nama pegunungan." },
  { "en": "The ship sailed across ___ Pacific Ocean.", "id": "Jawaban: the. Penjelasan: 'The' digunakan sebelum nama samudra, laut, dan sungai." },
  { "en": "___ Sahara Desert is the largest hot desert in the world.", "id": "Jawaban: The. Penjelasan: 'The' digunakan sebelum nama gurun." },
  { "en": "Mount Everest, ___ highest mountain in the world, is in the Himalayas.", "id": "Jawaban: the. Penjelasan: 'The' digunakan di sini karena merupakan bagian dari frasa superlatif ('the highest mountain')." },
  { "en": "The government implemented new policies to ___ poverty.", "id": "Jawaban: alleviate. Penjelasan: 'Alleviate' adalah kata kerja formal yang berarti 'membuat (penderitaan, kekurangan, atau masalah) menjadi tidak terlalu parah'." },
  { "en": "The company plans to ___ its workforce by hiring 100 new employees.", "id": "Jawaban: augment. Penjelasan: 'Augment' adalah kata kerja formal yang berarti 'membuat (sesuatu) menjadi lebih besar dengan menambahinya; meningkatkan'." },
  { "en": "There was a significant ___ between the two reports.", "id": "Jawaban: discrepancy. Penjelasan: 'Discrepancy' adalah kata benda yang berarti 'kurangnya kesesuaian atau kemiripan yang tidak logis atau mengejutkan antara dua atau lebih fakta'." },
  { "en": "The committee is looking for a ___ solution to the budget crisis.", "id": "Jawaban: feasible. Penjelasan: 'Feasible' adalah kata sifat yang berarti 'mungkin untuk dilakukan dengan mudah atau nyaman'." },
  { "en": "The sudden drop in sales was an ___ for the company's financial problems.", "id": "Jawaban: indicator. Penjelasan: 'Indicator' adalah kata benda yang berarti 'sesuatu yang menunjukkan keadaan atau tingkat sesuatu'." },
  { "en": "The new CEO's main ___ is to improve profitability.", "id": "Jawaban: priority. Penjelasan: 'Priority' adalah kata benda yang berarti 'sesuatu yang dianggap lebih penting daripada yang lain'." },
  { "en": "The witness's statement was ___ to the investigation.", "id": "Jawaban: crucial. Penjelasan: 'Crucial' adalah kata sifat yang berarti 'menentukan atau kritis, terutama dalam keberhasilan atau kegagalan sesuatu'." },
  { "en": "The two companies are working in ___ to develop a new product.", "id": "Jawaban: collaboration. Penjelasan: 'In collaboration' adalah frasa yang berarti bekerja sama." },
  { "en": "The ancient manuscript is ___; it's very delicate.", "id": "Jawaban: fragile. Penjelasan: 'Fragile' adalah kata sifat yang berarti '(benda) mudah pecah atau rusak'." },
  { "en": "The project requires a ___ amount of time and resources.", "id": "Jawaban: considerable. Penjelasan: 'Considerable' adalah kata sifat yang berarti 'cukup besar dalam ukuran, jumlah, atau luas'." },
  { "en": "The new drug is a ___ treatment for the disease.", "id": "Jawaban: potent. Penjelasan: 'Potent' adalah kata sifat yang berarti 'memiliki kekuatan, pengaruh, atau efek yang besar'." },
  { "en": "The final chapter ___ the main points of the book.", "id": "Jawaban: summarizes. Penjelasan: 'Summarizes' adalah kata kerja yang berarti 'memberikan pernyataan singkat tentang poin-poin utama dari (sesuatu)'." },
  { "en": "The company has an ___ to provide a safe working environment.", "id": "Jawaban: obligation. Penjelasan: 'Obligation' adalah kata benda yang berarti 'tindakan atau rangkaian tindakan yang secara moral atau hukum mengikat seseorang'." },
  { "en": "The results of the experiment were ___ with our initial hypothesis.", "id": "Jawaban: inconsistent. Penjelasan: 'Inconsistent with' berarti 'tidak tetap sama; tidak sesuai dengan'." },
  { "en": "His argument is logical and ___.", "id": "Jawaban: coherent. Penjelasan: 'Coherent' adalah kata sifat yang berarti 'logis dan konsisten'. Ini paralel dengan 'logical'." },
  { "en": "The goal is to ___ the gap between the rich and the poor.", "id": "Jawaban: bridge. Penjelasan: 'Bridge the gap' adalah idiom yang berarti 'menghubungkan dua hal atau mengurangi perbedaan di antara keduanya'." },
  { "en": "The city is facing a severe water ___.", "id": "Jawaban: shortage. Penjelasan: 'Shortage' adalah kata benda yang berarti 'keadaan atau situasi di mana sesuatu yang dibutuhkan tidak dapat diperoleh dalam jumlah yang cukup'." },
  { "en": "The two designs ___ in several important ways.", "id": "Jawaban: differ. Penjelasan: 'Differ' adalah bentuk kata kerja yang berarti 'tidak sama atau berbeda'." },
  { "en": "She has a ___ knowledge of art history.", "id": "Jawaban: profound. Penjelasan: 'Profound' adalah kata sifat yang berarti 'sangat hebat atau intens', sering digunakan dengan kata benda abstrak seperti 'knowledge' atau 'understanding'." },
  { "en": "The government is expected to ___ new guidelines soon.", "id": "Jawaban: issue. Penjelasan: 'Issue' digunakan di sini sebagai kata kerja yang berarti 'mengirimkan atau mengumumkan secara resmi'." },
  { "en": "The old building is a fire ___.", "id": "Jawaban: hazard. Penjelasan: 'Hazard' adalah kata benda yang berarti 'bahaya atau risiko'." },
  { "en": "The new discovery has ___ for the future of space exploration.", "id": "Jawaban: implications. Penjelasan: 'Implications' adalah kata benda yang berarti 'kesimpulan yang dapat ditarik dari sesuatu; konsekuensi'." },
  { "en": "It is ___ that he will win the election.", "id": "Jawaban: probable. Penjelasan: 'Probable' adalah kata sifat yang berarti 'kemungkinan akan terjadi atau menjadi kenyataan'." },
  { "en": "The company is conducting a ___ review of its policies.", "id": "Jawaban: comprehensive. Penjelasan: 'Comprehensive' adalah kata sifat yang berarti 'lengkap; mencakup semua atau hampir semua elemen'." },
  { "en": "The defendant was ___ to three years in prison.", "id": "Jawaban: sentenced. Penjelasan: 'Sentenced to' adalah istilah hukum yang benar untuk memberikan hukuman." },
  { "en": "The artist's work is a ___ of her personal experiences.", "id": "Jawaban: reflection. Penjelasan: 'Reflection' adalah kata benda yang digunakan di sini untuk berarti 'gambaran atau representasi dari sesuatu'." },
  { "en": "The system is designed to be highly ___.", "id": "Jawaban: efficient. Penjelasan: 'Efficient' adalah kata sifat yang berarti 'mencapai produktivitas maksimum dengan usaha atau biaya yang terbuang minimum'." },
  { "en": "The two countries have ___ diplomatic relations for decades.", "id": "Jawaban: maintained. Penjelasan: 'Maintained' adalah kata kerja yang berarti 'menyebabkan atau memungkinkan (suatu kondisi atau keadaan) untuk terus berlanjut'." },
  { "en": "The study's results were ___ by a team of independent researchers.", "id": "Jawaban: verified. Penjelasan: 'Verified' adalah kata kerja yang berarti 'memastikan atau membuktikan bahwa (sesuatu) itu benar, akurat, atau dapat dibenarkan'." },
  { "en": "He provided a ___ explanation for his absence.", "id": "Jawaban: plausible. Penjelasan: 'Plausible' adalah kata sifat yang berarti '(argumen atau pernyataan) tampak masuk akal atau mungkin'." },
  { "en": "The city has a ___ cultural heritage.", "id": "Jawaban: rich. Penjelasan: 'Rich cultural heritage' (warisan budaya yang kaya) adalah kolokasi yang sangat umum." },
  { "en": "The new manager plans to ___ several changes.", "id": "Jawaban: introduce. Penjelasan: 'Introduce' digunakan di sini untuk berarti 'memulai penggunaan atau operasi (sesuatu) untuk pertama kalinya'." },
  { "en": "The success of the project is ___ to good planning.", "id": "Jawaban: attributable. Penjelasan: 'Attributable to' berarti 'disebabkan oleh'." },
  { "en": "The museum's collection ___ several famous paintings.", "id": "Jawaban: includes. Penjelasan: 'Includes' adalah kata kerja yang berarti 'mengandung sebagai bagian dari keseluruhan'." },
  { "en": "The company had to ___ a number of employees during the recession.", "id": "Jawaban: lay off. Penjelasan: 'Lay off' adalah kata kerja frasa yang berarti mengakhiri pekerjaan karyawan karena alasan ekonomi." },
  { "en": "The two languages have some ___ similarities.", "id": "Jawaban: striking. Penjelasan: 'Striking' adalah kata sifat yang berarti 'menarik perhatian karena tidak biasa, ekstrem, atau menonjol'." },
  { "en": "The program aims to ___ students for the workforce.", "id": "Jawaban: prepare. Penjelasan: 'Prepare for' berarti 'menyiapkan untuk'." },
  { "en": "The decision was made ___ with the company's policy.", "id": "Jawaban: in accordance. Penjelasan: 'In accordance with' adalah frasa formal yang berarti 'dengan cara yang sesuai atau mengikuti (aturan, hukum, atau keinginan)'." },
  { "en": "The research focuses ___ on the effects of social media.", "id": "Jawaban: primarily. Penjelasan: 'Primarily' adalah kata keterangan yang berarti 'sebagian besar; terutama'." },
  { "en": "The author's writing style is very ___.", "id": "Jawaban: distinctive. Penjelasan: 'Distinctive' adalah kata sifat yang berarti 'khas dari satu orang atau benda, dan berfungsi untuk membedakannya dari yang lain'." },
  { "en": "The two sides have reached a ___ agreement.", "id": "Jawaban: tentative. Penjelasan: 'Tentative agreement' adalah perjanjian sementara yang belum final atau dikonfirmasi." },
  { "en": "The company is making a ___ effort to reduce waste.", "id": "Jawaban: concerted. Penjelasan: 'Concerted effort' adalah upaya yang terkoordinasi dan penuh tekad." },
  { "en": "The new evidence ___ him from all suspicion.", "id": "Jawaban: absolved. Penjelasan: 'Absolved' adalah kata kerja formal yang berarti 'menyatakan (seseorang) bebas dari rasa bersalah, kewajiban, atau hukuman'." },
  { "en": "The government failed to ___ the needs of the homeless.", "id": "Jawaban: address. Penjelasan: 'Address the needs' adalah kolokasi yang berarti menangani atau memperhatikan kebutuhan." },
  { "en": "Her work is a ___ source of inspiration for many young artists.", "id": "Jawaban: constant. Penjelasan: 'Constant' adalah kata sifat yang berarti 'terjadi terus menerus selama periode waktu tertentu'." },
  { "en": "He is ___ to making a decision until he has all the facts.", "id": "Jawaban: reluctant. Penjelasan: 'Reluctant to' berarti 'tidak mau dan ragu-ragu'." },
  { "en": "The two nations have a ___ agreement to share intelligence.", "id": "Jawaban: reciprocal. Penjelasan: 'Reciprocal' adalah kata sifat yang berarti '(perjanjian atau kewajiban) yang berlaku atau mengikat kedua belah pihak secara setara'." },
  { "en": "The project was a ___ success.", "id": "Jawaban: resounding. Penjelasan: 'Resounding success' adalah idiom untuk keberhasilan yang sangat besar." },
  { "en": "The law is not ___ in this particular case.", "id": "Jawaban: applicable. Penjelasan: 'Applicable' adalah kata sifat yang berarti 'relevan atau sesuai'." },
  { "en": "The company is under ___ to improve its financial performance.", "id": "Jawaban: pressure. Penjelasan: 'Under pressure' (di bawah tekanan) adalah frasa yang umum." },
  { "en": "The evidence against him was ___.", "id": "Jawaban: overwhelming. Penjelasan: 'Overwhelming' adalah kata sifat yang berarti 'sangat besar jumlahnya' atau 'sangat kuat'." },
  { "en": "The doctor recommended a ___ change in his diet.", "id": "Jawaban: gradual. Penjelasan: 'Gradual' adalah kata sifat yang berarti 'terjadi atau berlangsung perlahan atau bertahap'." },
  { "en": "The company has ___ a new strategy for growth.", "id": "Jawaban: adopted. Penjelasan: 'Adopted' adalah kata kerja yang berarti 'memilih untuk mengambil, mengikuti, atau menggunakan'." },
  { "en": "Her knowledge of the subject is ___.", "id": "Jawaban: extensive. Penjelasan: 'Extensive' adalah kata sifat yang berarti 'mencakup atau mempengaruhi area yang luas'." },
  { "en": "He is ___ as the leading authority on the topic.", "id": "Jawaban: recognized. Penjelasan: 'Recognized as' berarti 'diakui memiliki status tertentu'." },
  { "en": "The two companies ___ to form a new, larger corporation.", "id": "Jawaban: merged. Penjelasan: 'Merged' adalah kata kerja yang berarti 'menggabungkan atau menyebabkan bergabung untuk membentuk satu kesatuan'." },
  { "en": "The city has a ___ of parks and green spaces.", "id": "Jawaban: network. Penjelasan: 'Network' adalah sekelompok atau sistem orang atau benda yang saling berhubungan." },
  { "en": "It is ___ to check your work for errors before submitting it.", "id": "Jawaban: prudent. Penjelasan: 'Prudent' adalah kata sifat yang berarti 'bertindak dengan atau menunjukkan kehati-hatian dan pemikiran untuk masa depan'." },
  { "en": "The novel gives a ___ portrayal of life in the 19th century.", "id": "Jawaban: vivid. Penjelasan: 'Vivid' adalah kata sifat yang berarti 'menghasilkan perasaan yang kuat atau gambaran yang jelas di benak'." },
  { "en": "The company had to ___ production due to a lack of raw materials.", "id": "Jawaban: suspend. Penjelasan: 'Suspend' adalah kata kerja yang berarti 'menghentikan sementara agar tidak berlanjut atau berlaku'." },
  { "en": "The new system is ___ to be more user-friendly.", "id": "Jawaban: intended. Penjelasan: 'Intended to be' berarti 'dirancang atau direncanakan untuk menjadi'." },
  { "en": "___ his help, we would not have finished the project on time.", "id": "Jawaban: Without. Penjelasan: 'Without' dapat digunakan untuk memulai frasa yang menyatakan kondisi negatif, mirip dengan 'If it had not been for...'." },
  { "en": "But ___ your timely intervention, the situation would have been much worse.", "id": "Jawaban: for. Penjelasan: Frasa 'But for...' memiliki arti yang sama dengan 'Without...' atau 'If it hadn't been for...'." },
  { "en": "The loan will be approved ___ that you provide proof of income.", "id": "Jawaban: on the condition. Penjelasan: 'On the condition that' adalah cara formal untuk menyatakan persyaratan agar sesuatu terjadi, mirip dengan 'if'." },
  { "en": "In case ___ an emergency, please use the nearest exit.", "id": "Jawaban: of. Penjelasan: 'In case of' adalah frasa preposisional yang diikuti oleh kata benda, digunakan untuk membicarakan tindakan pencegahan." },
  { "en": "___ I known about the traffic, I would have left earlier.", "id": "Jawaban: Had. Penjelasan: Ini adalah 'inverted third conditional', di mana 'If' dihilangkan dan subjek serta kata kerja bantu posisinya dibalik." },
  { "en": "The weather was ___ cold that the river froze solid.", "id": "Jawaban: so. Penjelasan: 'So' digunakan sebelum kata sifat ('cold') untuk membentuk struktur sebab-akibat 'so...that'." },
  { "en": "It was ___ a difficult test that many students failed.", "id": "Jawaban: such. Penjelasan: 'Such' digunakan sebelum frasa kata benda ('a difficult test') dalam struktur 'such...that'." },
  { "en": "He spoke ___ quickly that I could not understand him.", "id": "Jawaban: so. Penjelasan: 'So' digunakan sebelum kata keterangan ('quickly') dalam struktur ini." },
  { "en": "She has ___ a beautiful voice that she should become a professional singer.", "id": "Jawaban: such. Penjelasan: 'Such' digunakan dengan 'a/an + adjective + noun'." },
  { "en": "There were ___ many people at the concert that we couldn't find a seat.", "id": "Jawaban: so. Penjelasan: 'So' digunakan dengan quantifier seperti 'many' dan 'much'." },
  { "en": "The person to ___ I spoke was very helpful.", "id": "Jawaban: whom. Penjelasan: Ini adalah struktur formal di mana preposisi 'to' mendahului objeknya, kata ganti relatif 'whom'. 'Whom' digunakan untuk orang dalam posisi objek." },
  { "en": "This is the project on ___ we have been working for months.", "id": "Jawaban: which. Penjelasan: Dalam struktur formal ini, preposisi 'on' mendahului kata ganti relatif 'which' (digunakan untuk benda)." },
  { "en": "The manager, for ___ we have the utmost respect, is retiring next year.", "id": "Jawaban: whom. Penjelasan: Frasa preposisional 'for whom' memperkenalkan klausa kata sifat." },
  { "en": "The theory upon ___ his research is based is quite complex.", "id": "Jawaban: which. Penjelasan: 'Upon which' adalah cara yang sangat formal untuk mengatakan 'which...on'." },
  { "en": "The period during ___ the empire flourished was known as the Golden Age.", "id": "Jawaban: which. Penjelasan: Preposisi 'during' ditempatkan sebelum 'which' untuk merujuk pada 'the period'." },
  { "en": "We are all eager ___ the results of the election.", "id": "Jawaban: to see. Penjelasan: Kata sifat 'eager' diikuti oleh infinitif ('to see')." },
  { "en": "He is very good ___ solving complex puzzles.", "id": "Jawaban: at. Penjelasan: Kata sifat 'good' diikuti oleh preposisi 'at' dan kemudian 'gerund' ('solving')." },
  { "en": "It is likely ___ rain later this afternoon.", "id": "Jawaban: to. Penjelasan: Kata sifat 'likely' diikuti oleh infinitif ('to rain')." },
  { "en": "I am tired ___ the same thing over and over again.", "id": "Jawaban: of doing. Penjelasan: Kata sifat 'tired' diikuti oleh preposisi 'of' dan 'gerund'." },
  { "en": "She was hesitant ___ her opinion.", "id": "Jawaban: to share. Penjelasan: Kata sifat 'hesitant' diikuti oleh infinitif." },
  { "en": "On the hill ___ a beautiful old castle.", "id": "Jawaban: stands. Penjelasan: Ketika kalimat dimulai dengan frasa preposisional tempat, kata kerja dapat mendahului subjek. Subjeknya adalah 'a beautiful old castle' (tunggal), jadi kata kerjanya adalah 'stands'." },
  { "en": "Among the documents ___ a letter from the president.", "id": "Jawaban: was. Penjelasan: Subjeknya adalah 'a letter' (tunggal), yang muncul setelah kata kerja." },
  { "en": "Beyond the mountains ___ a vast desert.", "id": "Jawaban: lies. Penjelasan: Subjek 'a vast desert' adalah tunggal, jadi kata kerja tunggal 'lies' digunakan." },
  { "en": "Never before ___ I witnessed such a spectacle.", "id": "Jawaban: have. Penjelasan: Ketika kalimat dimulai dengan adverbial negatif seperti 'Never before', subjek dan kata kerja bantu dibalik." },
  { "en": "The prize was divided between my brother and ___.", "id": "Jawaban: me. Penjelasan: Setelah preposisi ('between'), digunakan kata ganti kasus objektif ('me')." },
  { "en": "My colleague and ___ will be presenting the report.", "id": "Jawaban: I. Penjelasan: 'I' adalah bagian dari subjek majemuk dari kata kerja 'will be presenting'." },
  { "en": "She is at least as qualified as ___.", "id": "Jawaban: he is. Penjelasan: Dalam perbandingan formal dengan 'as', kata ganti subjek ('he') digunakan, seringkali diikuti oleh kata kerja ('is') untuk melengkapi klausa." },
  { "en": "The team practiced hard; ___, they won the championship.", "id": "Jawaban: consequently. Penjelasan: 'Consequently' adalah kata keterangan penghubung yang berarti 'sebagai hasilnya'." },
  { "en": "He is an expert in physics; ___, he is a talented musician.", "id": "Jawaban: moreover. Penjelasan: 'Moreover' digunakan untuk menambahkan informasi lain yang terkait." },
  { "en": "The new model is more efficient. ___, it is also more expensive.", "id": "Jawaban: However. Penjelasan: 'However' digunakan untuk memperkenalkan poin yang kontras." },
  { "en": "The ___ of the new policy will be evaluated after six months.", "id": "Jawaban: efficacy. Penjelasan: 'Efficacy' adalah kata benda formal yang berarti 'kemampuan untuk menghasilkan hasil yang diinginkan'." },
  { "en": "The professor's explanation helped to ___ the complex theory.", "id": "Jawaban: elucidate. Penjelasan: 'Elucidate' adalah kata kerja formal yang berarti 'membuat (sesuatu) menjadi jelas; menjelaskan'." },
  { "en": "His argument was full of logical ___.", "id": "Jawaban: fallacies. Penjelasan: 'Fallacy' adalah kata benda yang berarti 'keyakinan yang salah, terutama yang didasarkan pada argumen yang tidak sehat'." },
  { "en": "The two cultures share an ___ respect for their elders.", "id": "Jawaban: inherent. Penjelasan: 'Inherent' adalah kata sifat yang berarti 'ada dalam sesuatu sebagai atribut permanen, esensial, atau karakteristik'." },
  { "en": "The government's new approach represents a ___ shift in policy.", "id": "Jawaban: paradigm. Penjelasan: 'Paradigm shift' adalah perubahan mendasar dalam pendekatan atau asumsi yang mendasarinya." },
  { "en": "The project was a ___ undertaking, involving teams from three countries.", "id": "Jawaban: collaborative. Penjelasan: 'Collaborative' adalah kata sifat yang berarti 'dihasilkan oleh atau melibatkan dua pihak atau lebih yang bekerja sama'." },
  { "en": "The symptoms of the disease can be quite ___.", "id": "Jawaban: subtle. Penjelasan: 'Subtle' adalah kata sifat yang berarti '(terutama perubahan atau perbedaan) begitu halus atau presisi sehingga sulit untuk dianalisis atau dijelaskan'." },
  { "en": "The peace talks were a ___ effort to end the long-standing conflict.", "id": "Jawaban: deliberate. Penjelasan: 'Deliberate' adalah kata sifat yang berarti 'dilakukan dengan sadar dan sengaja'." },
  { "en": "The lawyer presented ___ evidence to support her client's case.", "id": "Jawaban: compelling. Penjelasan: 'Compelling' adalah kata sifat yang berarti 'membangkitkan minat, perhatian, atau kekaguman dengan cara yang sangat tak tertahankan'." },
  { "en": "The ruins of the ancient city are ___ of its former glory.", "id": "Jawaban: reminiscent. Penjelasan: 'Reminiscent of' adalah frasa yang berarti 'cenderung mengingatkan seseorang akan sesuatu'." },
  { "en": "The organization is ___ to the welfare of animals.", "id": "Jawaban: committed. Penjelasan: 'Committed to' berarti 'berdedikasi dan mengabdi pada'." },
  { "en": "The study was ___ in its approach, covering all possible angles.", "id": "Jawaban: exhaustive. Penjelasan: 'Exhaustive' adalah kata sifat yang berarti 'mencakup atau mempertimbangkan semua elemen atau aspek; sepenuhnya komprehensif'." },
  { "en": "The CEO gave a ___ speech about the company's future.", "id": "Jawaban: rousing. Penjelasan: 'Rousing' adalah kata sifat yang berarti 'menggairahkan, menggetarkan'." },
  { "en": "His decision to leave was ___ by a desire for new challenges.", "id": "Jawaban: motivated. Penjelasan: 'Motivated by' berarti 'diberi alasan atau insentif untuk melakukan sesuatu oleh'." },
  { "en": "The government must take ___ measures to control inflation.", "id": "Jawaban: decisive. Penjelasan: 'Decisive measures' adalah tindakan yang diambil dengan pasti untuk menyelesaikan suatu masalah." },
  { "en": "The discovery of the new species was purely ___.", "id": "Jawaban: fortuitous. Penjelasan: 'Fortuitous' adalah kata sifat formal yang berarti 'terjadi secara kebetulan daripada direncanakan'." },
  { "en": "The two accounts of the event are not mutually ___.", "id": "Jawaban: exclusive. Penjelasan: 'Mutually exclusive' adalah frasa umum yang berarti jika yang satu benar, yang lain pasti salah." },
  { "en": "The professor is a ___ expert on ancient civilizations.", "id": "Jawaban: preeminent. Penjelasan: 'Preeminent' adalah kata sifat yang berarti 'melampaui semua yang lain; sangat terkemuka dalam beberapa hal'." },
  { "en": "The new evidence ___ the need for a new investigation.", "id": "Jawaban: underscores. Penjelasan: 'Underscores' adalah kata kerja yang berarti 'menekankan'." },
  { "en": "The manual provides ___ instructions for assembling the furniture.", "id": "Jawaban: step-by-step. Penjelasan: 'Step-by-step' adalah kata sifat majemuk yang digunakan untuk menggambarkan proses yang dilakukan secara berurutan." },
  { "en": "The company's business practices are ethically ___.", "id": "Jawaban: questionable. Penjelasan: 'Questionable' adalah kata sifat yang berarti 'diragukan kebenaran atau kualitasnya'." },
  { "en": "The treaty was a ___ of years of negotiation.", "id": "Jawaban: culmination. Penjelasan: 'Culmination' adalah kata benda yang berarti 'titik tertinggi atau klimaks dari sesuatu, terutama yang dicapai setelah waktu yang lama'." },
  { "en": "His writing style is ___ and often difficult to understand.", "id": "Jawaban: convoluted. Penjelasan: 'Convoluted' adalah kata sifat yang berarti '(terutama argumen, cerita, atau kalimat) sangat kompleks dan sulit untuk diikuti'." },
  { "en": "The organization provides a ___ for professionals to network and share ideas.", "id": "Jawaban: forum. Penjelasan: 'Forum' adalah kata benda yang berarti 'tempat, pertemuan, atau media di mana ide dan pandangan tentang masalah tertentu dapat dipertukarkan'." },
  { "en": "The new vaccine is expected to ___ immunity against the virus.", "id": "Jawaban: confer. Penjelasan: 'Confer' adalah kata kerja formal yang berarti 'memberikan atau menganugerahkan'." },
  { "en": "The two politicians have ___ opposing views on the issue.", "id": "Jawaban: diametrically. Penjelasan: 'Diametrically' adalah kata keterangan yang digunakan untuk menekankan pertentangan." },
  { "en": "The ___ of the problem lies in a lack of funding.", "id": "Jawaban: crux. Penjelasan: 'Crux' dari sebuah masalah adalah titik yang paling penting atau menentukan." },
  { "en": "The new system is ___ superior to the old one.", "id": "Jawaban: demonstrably. Penjelasan: 'Demonstrably' adalah kata keterangan yang berarti 'dengan cara yang jelas terlihat atau dapat dibuktikan secara logis'." },
  { "en": "The army's ___ of the city lasted for several months.", "id": "Jawaban: siege. Penjelasan: 'Siege' adalah operasi militer di mana pasukan musuh mengepung sebuah kota atau bangunan." },
  { "en": "His speech was met with a ___ of applause.", "id": "Jawaban: torrent. Penjelasan: 'A torrent of applause' adalah idiom untuk tepuk tangan yang sangat banyak dan meriah." },
  { "en": "The evidence is ___ and open to interpretation.", "id": "Jawaban: subjective. Penjelasan: 'Subjective' adalah kata sifat yang berarti 'berdasarkan atau dipengaruhi oleh perasaan, selera, atau pendapat pribadi'." },
  { "en": "The company tries to ___ a positive work environment.", "id": "Jawaban: cultivate. Penjelasan: 'Cultivate' adalah kata kerja yang digunakan di sini untuk berarti 'mencoba untuk memperoleh atau mengembangkan (kualitas, sentimen, atau keterampilan)'." },
  { "en": "His theories were once considered radical but are now ___.", "id": "Jawaban: mainstream. Penjelasan: 'Mainstream' adalah kata sifat yang berarti 'dimiliki atau diterima oleh sebagian besar orang'." },
  { "en": "The witness gave a ___ account of the events leading up to the accident.", "id": "Jawaban: chronological. Penjelasan: 'Chronological' berarti '(catatan peristiwa) mengikuti urutan kejadiannya'." },
  { "en": "The plan is still in its ___ stages of development.", "id": "Jawaban: preliminary. Penjelasan: 'Preliminary' adalah kata sifat yang menunjukkan tindakan atau peristiwa yang mendahului atau dilakukan sebagai persiapan untuk sesuatu yang lebih penuh atau lebih penting." },
  { "en": "He is known for his ___ attention to detail.", "id": "Jawaban: meticulous. Penjelasan: 'Meticulous' adalah kata sifat yang berarti 'menunjukkan perhatian besar terhadap detail; sangat hati-hati dan teliti'." },
  { "en": "The project was ___ with difficulties from the start.", "id": "Jawaban: fraught. Penjelasan: 'Fraught with' adalah frasa kata sifat yang berarti 'penuh dengan (sesuatu yang tidak diinginkan)'." },
  { "en": "Her ___ as a skilled negotiator is well-deserved.", "id": "Jawaban: reputation. Penjelasan: 'Reputation' adalah kata benda yang berarti 'keyakinan atau pendapat yang umumnya dipegang tentang seseorang atau sesuatu'." },
  { "en": "The company has a ___ of not paying its suppliers on time.", "id": "Jawaban: track record. Penjelasan: 'Track record' adalah idiom untuk kinerja masa lalu seseorang atau organisasi dalam bidang tertentu." },
  { "en": "The economic ___ for the coming year is uncertain.", "id": "Jawaban: outlook. Penjelasan: 'Outlook' adalah kata benda yang berarti 'ramalan' atau 'prospek'." },
  { "en": "The two sides are unwilling to ___ on the key issues.", "id": "Jawaban: compromise. Penjelasan: 'Compromise' adalah kata kerja yang berarti 'menyelesaikan perselisihan dengan konsesi bersama'." },
  { "en": "The building is a prime ___ of modern architecture.", "id": "Jawaban: example. Penjelasan: 'Prime example' adalah contoh yang sangat baik atau representatif." },
  { "en": "The committee has a ___ from the board to investigate the matter.", "id": "Jawaban: mandate. Penjelasan: 'Mandate' adalah kata benda yang berarti 'perintah atau komisi resmi untuk melakukan sesuatu'." },
  { "en": "The findings of the report are ___ and require more data.", "id": "Jawaban: provisional. Penjelasan: 'Provisional' adalah kata sifat yang berarti 'diatur atau ada untuk saat ini, mungkin akan diubah nanti'." },
  { "en": "The defendant's story was not ___.", "id": "Jawaban: credible. Penjelasan: 'Credible' adalah kata sifat yang berarti 'dapat dipercaya; meyakinkan'." },
  { "en": "The two versions of the story ___ significantly.", "id": "Jawaban: diverge. Penjelasan: 'Diverge' adalah kata kerja yang berarti '(garis, rute, atau perjalanan) berpisah dari rute lain dan pergi ke arah yang berbeda'. Digunakan secara metaforis di sini." },
  { "en": "His knowledge of the subject is ___, but not very deep.", "id": "Jawaban: broad. Penjelasan: 'Broad' berarti 'mencakup berbagai subjek atau area yang luas'." },
  { "en": "The company's failure was ___ to poor marketing.", "id": "Jawaban: attributed. Penjelasan: 'Attributed to' berarti 'dianggap disebabkan oleh'." },
  { "en": "The software has a very ___ user interface.", "id": "Jawaban: intuitive. Penjelasan: 'Intuitive' berarti 'mudah digunakan dan dipahami'." },
  { "en": "The city is ___ for its vibrant nightlife.", "id": "Jawaban: noted. Penjelasan: 'Noted for' mirip dengan 'famous for' (terkenal karena)." },
  { "en": "The long-term ___ of climate change are a major concern.", "id": "Jawaban: consequences. Penjelasan: 'Consequences' adalah hasil atau efek dari suatu tindakan." },
  { "en": "She has a ___ for saying the right thing at the right time.", "id": "Jawaban: knack. Penjelasan: 'Knack' adalah keterampilan yang diperoleh atau alami dalam melakukan sesuatu." },
  { "en": "The debate was ___ and failed to resolve the key issues.", "id": "Jawaban: acrimonious. Penjelasan: 'Acrimonious' adalah kata sifat yang berarti '(biasanya pidato atau debat) penuh kemarahan dan kepahitan'." },
  { "en": "The new policy is designed to ___ economic growth.", "id": "Jawaban: foster. Penjelasan: 'Foster' berarti 'mendorong atau mempromosikan perkembangan'." },
  { "en": "The student's essay was ___ and well-argued.", "id": "Jawaban: articulate. Penjelasan: 'Articulate' adalah kata sifat yang berarti 'memiliki atau menunjukkan kemampuan untuk berbicara dengan lancar dan koheren'." },
  { "en": "The two nations have agreed to ___ hostilities.", "id": "Jawaban: cease. Penjelasan: 'Cease' adalah kata kerja formal yang berarti 'mengakhiri atau berakhir'." },
  { "en": "The company is at the ___ of technological innovation.", "id": "Jawaban: forefront. Penjelasan: 'At the forefront' adalah idiom yang berarti 'di posisi terdepan'." },
  { "en": "The evidence is ___ to support a conviction.", "id": "Jawaban: inadequate. Penjelasan: 'Inadequate' adalah kata sifat yang berarti 'kurang kualitas atau kuantitas yang dibutuhkan'." },
  { "en": "The new law gives the police ___ powers.", "id": "Jawaban: sweeping. Penjelasan: 'Sweeping' adalah kata sifat yang berarti 'luas dalam jangkauan atau efek'." },
  { "en": "No sooner had the president started speaking ___ the power went out.", "id": "Jawaban: than. Penjelasan: Pasangan konjungsi korelatif yang benar untuk 'No sooner' adalah 'than'." },
  { "en": "Hardly had she arrived at the station ___ the train departed.", "id": "Jawaban: when. Penjelasan: Pasangan korelatif yang benar untuk 'Hardly' adalah 'when'." },
  { "en": "Scarcely had we sat down for dinner ___ the doorbell rang.", "id": "Jawaban: when. Penjelasan: 'Scarcely', seperti 'Hardly', dipasangkan dengan 'when'." },
  { "en": "No sooner ___ the team score a goal than the crowd erupted in cheers.", "id": "Jawaban: did. Penjelasan: Struktur ini memerlukan inversi. Karena kata kerja utamanya adalah 'score' (bentuk dasar), kata kerja bantu 'did' diperlukan untuk simple past." },
  { "en": "He talks as if he ___ the expert on everything.", "id": "Jawaban: were. Penjelasan: Setelah 'as if', subjunctive 'were' digunakan untuk menggambarkan situasi yang tidak nyata atau hipotetis di masa sekarang." },
  { "en": "She acted as though she ___ me before, but I'm sure we've never met.", "id": "Jawaban: had met. Penjelasan: 'Past perfect' ('had met') digunakan setelah 'as though' untuk membicarakan situasi masa lalu yang tidak nyata." },
  { "en": "The child looked at the broken toy as if the world ___ to an end.", "id": "Jawaban: had come. Penjelasan: Ini menggambarkan peristiwa masa lalu yang tidak nyata (dunia tidak benar-benar berakhir), jadi digunakan 'past perfect'." },
  { "en": "It looks as if it ___ going to rain.", "id": "Jawaban: is. Penjelasan: Dalam kasus ini, tidak ada ketidaknyataan; ini adalah kemungkinan nyata. Subjunctive tidak digunakan ketika 'as if' memperkenalkan sesuatu yang mungkin benar." },
  { "en": "The coffee is not hot ___ for me.", "id": "Jawaban: enough. Penjelasan: 'Enough' diletakkan setelah kata sifat ('hot')." },
  { "en": "We don't have ___ to complete the project.", "id": "Jawaban: enough time. Penjelasan: 'Enough' diletakkan sebelum kata benda ('time')." },
  { "en": "Is he old ___ to vote in the election?", "id": "Jawaban: enough. Penjelasan: Strukturnya adalah 'adjective + enough'." },
  { "en": "He didn't run quickly ___ to win the race.", "id": "Jawaban: enough. Penjelasan: 'Enough' diletakkan setelah kata keterangan ('quickly')." },
  { "en": "The decision depends on ___ we get the funding.", "id": "Jawaban: whether. Penjelasan: Setelah preposisi ('on'), harus digunakan 'whether', bukan 'if'." },
  { "en": "The main question is ___ to proceed with the plan or not.", "id": "Jawaban: whether. Penjelasan: Sebelum infinitif ('to proceed'), harus digunakan 'whether'." },
  { "en": "I doubt ___ he will finish on time.", "id": "Jawaban: whether. Penjelasan: Setelah kata kerja yang menyatakan keraguan ('doubt'), 'whether' seringkali lebih disukai daripada 'if'." },
  { "en": "This form needs ___ by tomorrow morning.", "id": "Jawaban: to be signed. Penjelasan: Infinitif pasif ('to be + past participle') digunakan setelah kata kerja 'need'." },
  { "en": "He resents ___ what to do by his younger colleagues.", "id": "Jawaban: being told. Penjelasan: Kata kerja 'resents' diikuti oleh 'gerund'. Karena tindakannya pasif (dia diberitahu), digunakan 'gerund' pasif ('being + past participle')." },
  { "en": "The new software is expected ___ on all computers by next week.", "id": "Jawaban: to be installed. Penjelasan: Diperlukan infinitif pasif di sini karena perangkat lunak adalah penerima tindakan." },
  { "en": "She avoids ___ for her political opinions.", "id": "Jawaban: being criticized. Penjelasan: 'Avoid' diikuti oleh 'gerund'. 'Gerund' pasif digunakan karena dia adalah orang yang dikritik." },
  { "en": "The house requires ___ before we can move in.", "id": "Jawaban: painting. Penjelasan: Setelah kata kerja seperti 'require', 'need', dan 'want', 'gerund' aktif terkadang dapat memiliki makna pasif ('requires painting' = 'requires to be painted')." },
  { "en": "Her responsibilities include managing the team, preparing the budget, and ___ with clients.", "id": "Jawaban: liaising. Penjelasan: Untuk menjaga struktur paralel dengan 'gerund' lainnya ('managing', 'preparing'), diperlukan 'liaising'." },
  { "en": "He is a man of great intelligence, courage, and ___.", "id": "Jawaban: integrity. Penjelasan: Daftar tersebut terdiri dari kata benda, jadi kata benda 'integrity' diperlukan untuk struktur paralel." },
  { "en": "The report criticized the company for its poor safety record, its lack of transparency, and ___ it treated its employees.", "id": "Jawaban: the way. Penjelasan: Item daftar adalah frasa benda. 'the way it treated its employees' adalah klausa kata benda yang berfungsi sebagai frasa benda, membuatnya paralel." },
  { "en": "The company's goals are not only to maximize profit but also ___ a positive impact on the community.", "id": "Jawaban: to create. Penjelasan: Konjungsi korelatif 'not only...but also...' memerlukan struktur paralel. 'to maximize' adalah infinitif, jadi 'to create' diperlukan." },
  { "en": "Smartphones have become ___ in modern society.", "id": "Jawaban: ubiquitous. Penjelasan: 'Ubiquitous' adalah kata sifat yang berarti 'hadir, muncul, atau ditemukan di mana-mana'." },
  { "en": "Youth is ___; it does not last forever.", "id": "Jawaban: ephemeral. Penjelasan: 'Ephemeral' adalah kata sifat yang berarti 'berlangsung untuk waktu yang sangat singkat'." },
  { "en": "We need a ___ solution to this problem, not just a theoretical one.", "id": "Jawaban: pragmatic. Penjelasan: 'Pragmatic' adalah kata sifat yang berarti 'menangani sesuatu secara masuk akal dan realistis dengan cara yang didasarkan pada pertimbangan praktis'." },
  { "en": "___, he was a supporter of the new policy, but in reality, he opposed it.", "id": "Jawaban: Ostensibly. Penjelasan: 'Ostensibly' adalah kata keterangan yang berarti 'seperti yang tampak atau dinyatakan benar, meskipun belum tentu demikian'." },
  { "en": "Misinformation tends to ___ quickly on social media.", "id": "Jawaban: proliferate. Penjelasan: 'Proliferate' adalah kata kerja yang berarti 'meningkat pesat jumlahnya; berkembang biak'." },
  { "en": "The research aims to ___ the relationship between diet and health.", "id": "Jawaban: investigate. Penjelasan: 'Investigate' adalah kata kerja yang berarti 'melakukan penyelidikan sistematis atau formal untuk menemukan dan memeriksa fakta'." },
  { "en": "There is a ___ difference between the two concepts.", "id": "Jawaban: fundamental. Penjelasan: 'Fundamental' adalah kata sifat yang berarti 'membentuk dasar yang diperlukan; yang paling penting'." },
  { "en": "The government must ___ resources effectively to meet the needs of the people.", "id": "Jawaban: allocate. Penjelasan: 'Allocate' adalah kata kerja yang berarti 'mendistribusikan (sumber daya atau tugas) untuk tujuan tertentu'." },
  { "en": "His speech had a ___ effect on the audience.", "id": "Jawaban: profound. Penjelasan: 'Profound' adalah kata sifat yang berarti 'sangat hebat atau intens'." },
  { "en": "The company has a ___ process for handling customer complaints.", "id": "Jawaban: standardized. Penjelasan: 'Standardized' adalah kata sifat yang berarti 'dibuat sesuai dengan standar'." },
  { "en": "The committee reached a ___ decision to approve the project.", "id": "Jawaban: unanimous. Penjelasan: 'Unanimous' adalah kata sifat yang berarti '(dua orang atau lebih) sepenuhnya setuju'." },
  { "en": "The historical ___ of the event is still debated by scholars.", "id": "Jawaban: significance. Penjelasan: 'Significance' adalah kata benda yang berarti 'kualitas yang layak mendapat perhatian; kepentingan'." },
  { "en": "The country's economy is heavily ___ on tourism.", "id": "Jawaban: reliant. Penjelasan: 'Reliant on' adalah frasa kata sifat yang berarti 'bergantung pada seseorang atau sesuatu'." },
  { "en": "The new evidence ___ the defendant's claim of innocence.", "id": "Jawaban: reinforces. Penjelasan: 'Reinforces' adalah kata kerja yang berarti 'memperkuat atau mendukung'." },
  { "en": "The plan is ___ because it requires too many resources.", "id": "Jawaban: unfeasible. Penjelasan: 'Unfeasible' adalah kata sifat yang berarti 'tidak praktis atau tidak mungkin dilakukan'." },
  { "en": "The two nations have a ___ agreement to support each other in times of crisis.", "id": "Jawaban: mutual. Penjelasan: 'Mutual' adalah kata sifat yang berarti '(perasaan atau tindakan) yang dialami atau dilakukan oleh masing-masing dari dua pihak atau lebih terhadap yang lain'." },
  { "en": "The doctor gave a ___ diagnosis after examining the patient.", "id": "Jawaban: preliminary. Penjelasan: Diagnosis 'preliminary' adalah diagnosis awal yang mungkin dapat berubah." },
  { "en": "The city has a ___ problem with traffic congestion.", "id": "Jawaban: chronic. Penjelasan: 'Chronic' adalah kata sifat yang berarti '(masalah) yang berlangsung lama dan sulit diberantas'." },
  { "en": "The ___ of the problem is more complex than it first appears.", "id": "Jawaban: nature. Penjelasan: 'Nature' dari sesuatu mengacu pada kualitas atau karakter dasarnya." },
  { "en": "The old bridge is no longer structurally ___.", "id": "Jawaban: sound. Penjelasan: 'Structurally sound' adalah kolokasi umum yang berarti 'kuat dan tidak dalam bahaya runtuh'." },
  { "en": "The professor tried to ___ a sense of curiosity in her students.", "id": "Jawaban: instill. Penjelasan: 'Instill' adalah kata kerja yang berarti 'secara bertahap tetapi kuat menanamkan (ide atau sikap) dalam pikiran seseorang'." },
  { "en": "The book provides a ___ critique of modern society.", "id": "Jawaban: scathing. Penjelasan: 'Scathing' adalah kata sifat yang kuat yang berarti 'sangat kritis dan pedas'." },
  { "en": "His promotion was the ___ of years of hard work.", "id": "Jawaban: culmination. Penjelasan: 'Culmination' adalah kata benda untuk titik akhir atau tertinggi." },
  { "en": "The new manager is expected to ___ the company's new policies.", "id": "Jawaban: implement. Penjelasan: 'Implement' adalah kata kerja yang berarti 'melaksanakan'." },
  { "en": "The witness's testimony was found to be ___.", "id": "Jawaban: unreliable. Penjelasan: 'Unreliable' berarti 'tidak dapat diandalkan'." },
  { "en": "The two countries have a long history of ___ cooperation.", "id": "Jawaban: bilateral. Penjelasan: 'Bilateral' adalah kata sifat yang berarti 'memiliki atau berkaitan dengan dua sisi; mempengaruhi kedua belah pihak'." },
  { "en": "The government is facing ___ pressure to lower taxes.", "id": "Jawaban: immense. Penjelasan: 'Immense' adalah kata sifat yang berarti 'sangat besar atau hebat'." },
  { "en": "The new drug is a ___ breakthrough in cancer treatment.", "id": "Jawaban: potential. Penjelasan: 'Potential breakthrough' adalah terobosan yang mungkin bisa terjadi." },
  { "en": "The company has a ___ process for quality control.", "id": "Jawaban: rigorous. Penjelasan: 'Rigorous' adalah kata sifat yang berarti 'sangat teliti, menyeluruh, atau akurat'." },
  { "en": "The team's ___ was high after winning the championship.", "id": "Jawaban: morale. Penjelasan: 'Morale' adalah kata benda yang berarti 'kepercayaan diri, antusiasme, dan disiplin seseorang atau kelompok pada waktu tertentu'." },
  { "en": "The artist's work is a ___ to the beauty of nature.", "id": "Jawaban: tribute. Penjelasan: 'Tribute' adalah tindakan, pernyataan, atau hadiah yang dimaksudkan untuk menunjukkan rasa terima kasih, hormat, atau kekaguman." },
  { "en": "The new regulations will have ___-reaching effects on the industry.", "id": "Jawaban: far. Penjelasan: 'Far-reaching' adalah kata sifat majemuk yang berarti 'memiliki pengaruh atau efek yang luas'." },
  { "en": "The company's profits ___ all expectations last year.", "id": "Jawaban: exceeded. Penjelasan: 'Exceeded' adalah kata kerja yang berarti 'lebih besar dalam jumlah atau ukuran dari (kuantitas, angka, atau hal lain yang dapat diukur)'." },
  { "en": "The negotiations reached an ___ and could not proceed.", "id": "Jawaban: impasse. Penjelasan: 'Impasse' adalah situasi di mana tidak ada kemajuan yang mungkin, terutama karena ketidaksepakatan; jalan buntu." },
  { "en": "The government is trying to ___ the flow of illegal drugs.", "id": "Jawaban: curb. Penjelasan: 'Curb' adalah kata kerja yang berarti 'menahan atau mengendalikan'." },
  { "en": "His decision was ___ by logic and reason.", "id": "Jawaban: guided. Penjelasan: 'Guided by' berarti 'diarahkan atau dipengaruhi oleh'." },
  { "en": "The company is known for its ___ commitment to sustainability.", "id": "Jawaban: unwavering. Penjelasan: 'Unwavering' adalah kata sifat yang berarti 'teguh atau mantap; tidak goyah'." },
  { "en": "The book gives a ___ account of the war.", "id": "Jawaban: harrowing. Penjelasan: 'Harrowing' adalah kata sifat yang berarti 'sangat menyedihkan'." },
  { "en": "He is a ___ figure in the environmental movement.", "id": "Jawaban: prominent. Penjelasan: 'Prominent' berarti 'penting; terkenal'." },
  { "en": "The old traditions are gradually ___ away.", "id": "Jawaban: fading. Penjelasan: 'Fading away' adalah kata kerja frasa yang berarti menghilang secara perlahan." },
  { "en": "The meeting was ___ to discuss the quarterly report.", "id": "Jawaban: convened. Penjelasan: 'Convened' adalah kata kerja formal yang berarti 'datang atau berkumpul untuk rapat atau kegiatan'." },
  { "en": "His theory, ___ simple, is actually quite complex.", "id": "Jawaban: deceptively. Penjelasan: 'Deceptively' adalah kata keterangan yang berarti 'pada tingkat yang lebih rendah dari yang terlihat'." },
  { "en": "The two projects will be managed ___ until a decision is made.", "id": "Jawaban: concurrently. Penjelasan: 'Concurrently' adalah kata keterangan yang berarti 'pada saat yang bersamaan'." },
  { "en": "The museum's collection is ___ and covers many centuries.", "id": "Jawaban: vast. Penjelasan: 'Vast' berarti 'sangat luas atau banyak jumlahnya; sangat besar'." },
  { "en": "The new discovery has ___ a lot of interest among scientists.", "id": "Jawaban: generated. Penjelasan: 'Generated' berarti 'dihasilkan atau diciptakan'." },
  { "en": "She is an ___ for human rights.", "id": "Jawaban: advocate. Penjelasan: 'Advocate' adalah orang yang secara publik mendukung atau merekomendasikan suatu tujuan atau kebijakan tertentu." },
  { "en": "The company had to ___ its strategy in response to market changes.", "id": "Jawaban: modify. Penjelasan: 'Modify' berarti 'membuat perubahan parsial atau kecil pada (sesuatu)'." },
  { "en": "The two ideas are not ___; in fact, they support each other.", "id": "Jawaban: contradictory. Penjelasan: 'Contradictory' berarti 'saling bertentangan atau tidak konsisten'." },
  { "en": "The project is a ___ between the university and a private company.", "id": "Jawaban: collaboration. Penjelasan: 'Collaboration' adalah tindakan bekerja sama dengan seseorang untuk menghasilkan sesuatu." },
  { "en": "The evidence provided was ___ to reach a conclusion.", "id": "Jawaban: insufficient. Penjelasan: 'Insufficient' berarti 'tidak cukup; tidak memadai'." },
  { "en": "The new policy will be ___ from the first of next month.", "id": "Jawaban: effective. Penjelasan: 'Effective' di sini berarti 'berlaku'." },
  { "en": "He has a ___ for remembering names and faces.", "id": "Jawaban: talent. Penjelasan: 'Talent' adalah bakat atau keterampilan alami." },
  { "en": "The study's findings are ___ with previous research.", "id": "Jawaban: consistent. Penjelasan: 'Consistent with' berarti 'sesuai dengan'." },
  { "en": "The company offers ___ benefits to its employees.", "id": "Jawaban: generous. Penjelasan: 'Generous' berarti 'menunjukkan kesiapan untuk memberi lebih banyak sesuatu, seperti uang atau waktu, daripada yang sebenarnya diperlukan atau diharapkan'." },
  { "en": "The issue is ___ to the main topic of discussion.", "id": "Jawaban: peripheral. Penjelasan: 'Peripheral' berarti 'bersifat sekunder atau kurang penting'." },
  { "en": "The organization is ___ by a board of directors.", "id": "Jawaban: governed. Penjelasan: 'Governed' berarti 'menjalankan kebijakan, tindakan, dan urusan (suatu negara, organisasi, atau orang)'." },
  { "en": "The witness's memory of the event was ___.", "id": "Jawaban: hazy. Penjelasan: 'Hazy' berarti 'samar-samar atau tidak jelas'." },
  { "en": "The new plan is a ___ improvement over the old one.", "id": "Jawaban: demonstrable. Penjelasan: 'Demonstrable' berarti 'jelas terlihat atau dapat dibuktikan secara logis'." },
  { "en": "His ___ to the cause is unquestionable.", "id": "Jawaban: dedication. Penjelasan: 'Dedication' adalah kata benda yang berarti 'kualitas berdedikasi atau berkomitmen pada suatu tugas atau tujuan'." },
  { "en": "The report provides a ___ analysis of the market trends.", "id": "Jawaban: thorough. Penjelasan: 'Thorough' berarti 'lengkap dalam setiap detail'." },
  { "en": "The new discovery could be the ___ to solving the mystery.", "id": "Jawaban: key. Penjelasan: 'Key' digunakan di sini secara metaforis untuk berarti elemen penting." },
  { "en": "The company has ___ its position as a market leader.", "id": "Jawaban: consolidated. Penjelasan: 'Consolidated' berarti 'membuat (sesuatu) lebih kuat atau lebih kokoh secara fisik; memperkuat (posisi atau kekuasaan seseorang)'." },
  { "en": "It's late. It's high time we ___ home.", "id": "Jawaban: went. Penjelasan: Setelah ekspresi 'It's (high) time', digunakan 'simple past tense' untuk menyatakan bahwa sesuatu seharusnya dilakukan sekarang." },
  { "en": "You have been procrastinating for weeks. It's time you ___ your project.", "id": "Jawaban: started. Penjelasan: Struktur 'It's time + subject + past tense verb' digunakan untuk tindakan sekarang atau di masa depan yang sudah terlambat." },
  { "en": "It's about time the government ___ something about this problem.", "id": "Jawaban: did. Penjelasan: 'It's about time' mengikuti aturan yang sama, menggunakan 'past subjunctive' ('did')." },
  { "en": "I think it's time for us ___ the discussion.", "id": "Jawaban: to start. Penjelasan: Ini adalah struktur yang berbeda. Ketika 'for + object' digunakan, itu diikuti oleh infinitif ('to start'), bukan klausa lampau." },
  { "en": "It looks like it's going to rain. We ___ take an umbrella.", "id": "Jawaban: had better. Penjelasan: 'Had better + kata kerja dasar' digunakan untuk memberikan nasihat yang kuat atau peringatan untuk situasi tertentu." },
  { "en": "I would rather ___ at home than go to the movies tonight.", "id": "Jawaban: stay. Penjelasan: 'Would rather' diikuti oleh bentuk dasar dari kata kerja ('stay')." },
  { "en": "The deadline is tomorrow. You ___ finish that report now.", "id": "Jawaban: had better. Penjelasan: Ini menyatakan nasihat kuat atau urgensi." },
  { "en": "She would rather her son ___ a doctor than a lawyer.", "id": "Jawaban: became. Penjelasan: Ketika 'would rather' diikuti oleh subjek, kata kerjanya dalam bentuk lampau (subjunctive) untuk membicarakan preferensi mengenai orang lain." },
  { "en": "He'd better ___ late again, or he will be in serious trouble.", "id": "Jawaban: not be. Penjelasan: Bentuk negatif dari 'had better' adalah 'had better not'." },
  { "en": "Please remember ___ the door when you leave.", "id": "Jawaban: to lock. Penjelasan: 'Remember to do' mengacu pada tindakan di masa depan (tugas). 'Remember doing' mengacu pada ingatan tindakan masa lalu." },
  { "en": "He stopped ___ because the doctor advised him to.", "id": "Jawaban: smoking. Penjelasan: 'Stop doing' berarti berhenti dari suatu aktivitas. 'Stop to do' berarti berhenti dari satu aktivitas untuk melakukan yang lain." },
  { "en": "I forgot ___ him about the meeting, so he didn't attend.", "id": "Jawaban: to tell. Penjelasan: 'Forget to do' berarti Anda gagal melakukan sesuatu. 'Forget doing' berarti Anda tidak memiliki ingatan tentang tindakan masa lalu." },
  { "en": "I'll never forget ___ the Grand Canyon for the first time.", "id": "Jawaban: seeing. Penjelasan: 'Forget doing' mengacu pada ingatan tentang peristiwa masa lalu." },
  { "en": "She tried ___ the heavy box, but it was too heavy for her.", "id": "Jawaban: to lift. Penjelasan: 'Try to do' berarti berusaha atau mencoba sesuatu. 'Try doing' berarti bereksperimen dengan sesuatu untuk melihat apakah berhasil." },
  { "en": "___ students find the final exam to be the most difficult.", "id": "Jawaban: Most. Penjelasan: 'Most' (tanpa 'of the') digunakan untuk membuat pernyataan umum tentang siswa." },
  { "en": "Most of ___ in my class are from other countries.", "id": "Jawaban: the students. Penjelasan: 'Most of the' digunakan untuk membicarakan kelompok tertentu ('the students in this class')." },
  { "en": "Some of ___ you gave me was incorrect.", "id": "Jawaban: the information. Penjelasan: 'Some of the' digunakan untuk bagian spesifik dari informasi." },
  { "en": "___ cars in this parking lot are electric.", "id": "Jawaban: Many of the. Penjelasan: 'Many of the' mengacu pada sejumlah besar dalam kelompok tertentu ('the cars in this parking lot')." },
  { "en": "She is very proficient ___ several programming languages.", "id": "Jawaban: in. Penjelasan: Kata sifat 'proficient' diikuti oleh preposisi 'in'." },
  { "en": "The final grade is contingent ___ your exam results.", "id": "Jawaban: on. Penjelasan: 'Contingent on' berarti 'bergantung pada'." },
  { "en": "He was exonerated ___ all charges against him.", "id": "Jawaban: from. Penjelasan: Kata kerja 'exonerate' diikuti oleh preposisi 'from' (atau kadang-kadang 'of')." },
  { "en": "The new design is a departure ___ the company's traditional style.", "id": "Jawaban: from. Penjelasan: 'A departure from' berarti penyimpangan dari apa yang normal." },
  { "en": "He graduated from ___ Harvard University.", "id": "Jawaban: (no article). Penjelasan: Umumnya tidak ada artikel yang digunakan sebelum nama universitas." },
  { "en": "The current ___ of the United States is giving a speech tonight.", "id": "Jawaban: president. Penjelasan: Gelar 'president' tidak dikapitalisasi dan memerlukan 'the' ketika tidak diikuti oleh nama." },
  { "en": "___ President Smith will attend the ceremony.", "id": "Jawaban: (no article). Penjelasan: Tidak ada artikel yang digunakan ketika gelar seperti 'President' digunakan langsung sebelum nama." },
  { "en": "He studied diligently ___ he should fail the exam.", "id": "Jawaban: lest. Penjelasan: 'Lest' adalah konjungsi formal yang berarti 'karena takut bahwa' atau 'agar tidak'. Sering diikuti oleh 'should'." },
  { "en": "The policy is a good one ___ it is implemented fairly.", "id": "Jawaban: provided that. Penjelasan: 'Provided that' berarti 'dengan syarat bahwa' atau 'jika'." },
  { "en": "___ he is an expert, his opinion is highly valued.", "id": "Jawaban: Inasmuch as. Penjelasan: 'Inasmuch as' adalah cara formal untuk mengatakan 'karena' atau 'sejauh'." },
  { "en": "They were found guilty ___ they had a strong alibi.", "id": "Jawaban: even though. Penjelasan: 'Even though' digunakan untuk menunjukkan kontras yang kuat dan mengejutkan." },
  { "en": "The value of friendship is ___; it comes from within and cannot be bought.", "id": "Jawaban: intrinsic. Penjelasan: 'Intrinsic' adalah kata sifat yang berarti 'dimiliki secara alami; esensial'." },
  { "en": "The decision to close the factory was completely ___, with no logical reason given.", "id": "Jawaban: arbitrary. Penjelasan: 'Arbitrary' adalah kata sifat yang berarti 'berdasarkan pilihan acak atau keinginan pribadi, bukan alasan atau sistem apa pun'." },
  { "en": "The evidence he presented was later found to be ___.", "id": "Jawaban: spurious. Penjelasan: 'Spurious' adalah kata sifat yang berarti 'tidak seperti yang tampak; salah atau palsu'." },
  { "en": "His claim to the throne was rather ___, based on a distant ancestor.", "id": "Jawaban: tenuous. Penjelasan: 'Tenuous' adalah kata sifat yang berarti 'sangat lemah atau sedikit'." },
  { "en": "The report tries to ___ the city's modern architecture with its ancient ruins.", "id": "Jawaban: juxtapose. Penjelasan: 'Juxtapose' adalah kata kerja yang berarti 'menempatkan atau menangani berdekatan untuk efek kontras'." },
  { "en": "His ___ for the job was clear from his enthusiastic interview.", "id": "Jawaban: zeal. Penjelasan: 'Zeal' adalah kata benda yang berarti 'energi atau antusiasme besar dalam mengejar suatu tujuan'." },
  { "en": "The politician's speech was full of empty ___.", "id": "Jawaban: rhetoric. Penjelasan: 'Rhetoric' adalah kata benda yang berarti 'bahasa yang dirancang untuk memiliki efek persuasif atau mengesankan, tetapi sering dianggap kurang tulus'." },
  { "en": "The company had to ___ its operations during the economic crisis.", "id": "Jawaban: curtail. Penjelasan: 'Curtail' adalah kata kerja yang berarti 'mengurangi jangkauan atau kuantitas; memberlakukan pembatasan'." },
  { "en": "The two theories are not contradictory; in fact, they are ___.", "id": "Jawaban: complementary. Penjelasan: 'Complementary' adalah kata sifat yang berarti 'berkombinasi sedemikian rupa untuk meningkatkan atau menekankan kualitas satu sama lain'." },
  { "en": "The documentary was a ___ indictment of the government's policies.", "id": "Jawaban: damning. Penjelasan: 'Damning' adalah kata sifat yang berarti '(keadaan atau bukti) yang sangat menunjukkan kesalahan atau kekeliruan'." },
  { "en": "The professor's lectures were often ___ and difficult for the students to follow.", "id": "Jawaban: esoteric. Penjelasan: 'Esoteric' adalah kata sifat yang berarti 'dimaksudkan untuk atau kemungkinan besar hanya dipahami oleh sejumlah kecil orang dengan pengetahuan khusus'." },
  { "en": "There was a ___ agreement among the team members to support their leader.", "id": "Jawaban: tacit. Penjelasan: 'Tacit' adalah kata sifat yang berarti 'dipahami atau tersirat tanpa dinyatakan'." },
  { "en": "He gave the report a ___ glance before signing it.", "id": "Jawaban: perfunctory. Penjelasan: 'Perfunctory' adalah kata sifat yang berarti '(tindakan) yang dilakukan dengan usaha atau refleksi minimum'." },
  { "en": "Her ___ nature made her an excellent diplomat.", "id": "Jawaban: tactful. Penjelasan: 'Tactful' adalah kata sifat yang berarti 'memiliki atau menunjukkan keterampilan dan kepekaan dalam berurusan dengan orang lain atau dengan masalah sulit'." },
  { "en": "The rise of remote work has ___ the need for large office spaces.", "id": "Jawaban: obviated. Penjelasan: 'Obviated' adalah kata kerja formal yang berarti 'menghilangkan (kebutuhan atau kesulitan)'." },
  { "en": "The new policy has been met with ___ from the public.", "id": "Jawaban: skepticism. Penjelasan: 'Skepticism' adalah kata benda yang berarti 'sikap skeptis; keraguan akan kebenaran sesuatu'." },
  { "en": "The city's ___ population has grown rapidly in recent years.", "id": "Jawaban: indigenous. Penjelasan: 'Indigenous' adalah kata sifat yang berarti 'berasal atau terjadi secara alami di tempat tertentu; asli'." },
  { "en": "The artist is known for his ___ use of bold colors.", "id": "Jawaban: audacious. Penjelasan: 'Audacious' bisa berarti 'menunjukkan kesediaan untuk mengambil risiko yang sangat berani', yang sesuai dengan konteks seni." },
  { "en": "The company is trying to ___ its brand identity.", "id": "Jawaban: redefine. Penjelasan: 'Redefine' berarti 'mendefinisikan kembali atau secara berbeda'." },
  { "en": "The evidence was ___ and did not support his claim.", "id": "Jawaban: flimsy. Penjelasan: 'Flimsy' adalah kata sifat yang berarti 'tidak substansial dan mudah rusak' atau '(argumen) lemah dan tidak meyakinkan'." },
  { "en": "The manager's ___ leadership style was very effective.", "id": "Jawaban: hands-on. Penjelasan: 'Hands-on' adalah kata sifat majemuk yang berarti 'melibatkan partisipasi aktif daripada teori'." },
  { "en": "The new law is an ___ on personal freedom.", "id": "Jawaban: infringement. Penjelasan: 'Infringement' adalah 'tindakan melanggar ketentuan hukum, perjanjian, dll.; pelanggaran'." },
  { "en": "The company is facing a ___ of lawsuits.", "id": "Jawaban: plethora. Penjelasan: 'Plethora' adalah sejumlah besar atau berlebihan dari sesuatu." },
  { "en": "He has a ___ ability to predict market trends.", "id": "Jawaban: uncanny. Penjelasan: 'Uncanny' adalah kata sifat yang berarti 'aneh atau misterius, terutama dengan cara yang meresahkan'." },
  { "en": "The two versions of the document need to be ___.", "id": "Jawaban: reconciled. Penjelasan: 'Reconciled' berarti 'dipulihkan ke hubungan yang bersahabat' atau 'dibuat kompatibel'." },
  { "en": "The project was a ___ failure, despite all the planning.", "id": "Jawaban: dismal. Penjelasan: 'Dismal' adalah kata sifat yang berarti 'menyedihkan; suram' atau di sini berarti 'sangat buruk'." },
  { "en": "The new evidence may ___ the defendant.", "id": "Jawaban: exonerate. Penjelasan: 'Exonerate' berarti 'membebaskan (seseorang) dari kesalahan atas suatu kesalahan'." },
  { "en": "His ___ to change is a major obstacle to progress.", "id": "Jawaban: resistance. Penjelasan: 'Resistance' adalah kata benda yang berarti 'penolakan untuk menerima atau mematuhi sesuatu'." },
  { "en": "The two countries have a ___ trade agreement.", "id": "Jawaban: long-standing. Penjelasan: 'Long-standing' adalah kata sifat majemuk yang berarti 'telah ada sejak lama'." },
  { "en": "The organization is ___ with finding a cure for the disease.", "id": "Jawaban: tasked. Penjelasan: 'Tasked with' berarti 'diberi tugas tertentu untuk dilakukan'." },
  { "en": "The company's code of conduct is ___ and applies to all employees.", "id": "Jawaban: non-negotiable. Penjelasan: 'Non-negotiable' berarti 'tidak terbuka untuk diskusi atau modifikasi'." },
  { "en": "The new discovery has ___ our understanding of the universe.", "id": "Jawaban: transformed. Penjelasan: 'Transformed' berarti 'membuat perubahan menyeluruh atau dramatis dalam bentuk, penampilan, atau karakter'." },
  { "en": "The project is financially ___ and likely to succeed.", "id": "Jawaban: viable. Penjelasan: 'Viable' adalah kata sifat yang berarti 'mampu bekerja dengan sukses; layak'." },
  { "en": "The two sides are in a ___ over the terms of the contract.", "id": "Jawaban: dispute. Penjelasan: 'Dispute' adalah ketidaksepakatan, argumen, atau perdebatan." },
  { "en": "The manager's ___ approval is required for all expenses.", "id": "Jawaban: prior. Penjelasan: 'Prior' adalah kata sifat yang berarti 'ada atau datang sebelum dalam waktu, urutan, atau kepentingan'." },
  { "en": "The city is a ___ hub for trade and commerce.", "id": "Jawaban: major. Penjelasan: 'Major' adalah kata sifat yang berarti 'penting, serius, atau signifikan'." },
  { "en": "The new evidence ___ the case against him.", "id": "Jawaban: strengthens. Penjelasan: 'Strengthens' adalah kata kerja yang berarti 'membuat atau menjadi lebih kuat'." },
  { "en": "His work is ___ of a true professional.", "id": "Jawaban: characteristic. Penjelasan: 'Characteristic of' berarti 'khas dari orang, tempat, atau benda tertentu'." },
  { "en": "The company has a ___ approach to problem-solving.", "id": "Jawaban: proactive. Penjelasan: 'Proactive' adalah kata sifat yang berarti 'menciptakan atau mengendalikan situasi dengan menyebabkan sesuatu terjadi daripada merespons setelah terjadi'." },
  { "en": "The two leaders met to ___ their differences.", "id": "Jawaban: resolve. Penjelasan: 'Resolve' adalah kata kerja yang berarti 'menyelesaikan atau mencari solusi untuk (masalah, perselisihan, atau hal yang diperdebatkan)'." },
  { "en": "The study's results were ___ and require further analysis.", "id": "Jawaban: preliminary. Penjelasan: 'Preliminary' berarti 'dilakukan sebagai persiapan untuk sesuatu yang lebih penuh atau lebih penting'." },
  { "en": "The company is committed to ___ diversity in the workplace.", "id": "Jawaban: promoting. Penjelasan: 'Promoting' adalah 'gerund' di sini, yang berarti 'mendukung atau mendorong'." },
  { "en": "The defendant's alibi was ___ by several witnesses.", "id": "Jawaban: confirmed. Penjelasan: 'Confirmed' berarti 'menetapkan kebenaran atau kebenaran dari'." },
  { "en": "The new regulations are ___ to all businesses, large and small.", "id": "Jawaban: applicable. Penjelasan: 'Applicable to' berarti 'relevan atau sesuai untuk'." },
  { "en": "The company is at a ___ in its development.", "id": "Jawaban: crossroads. Penjelasan: 'At a crossroads' adalah idiom yang berarti 'pada titik keputusan atau momen kritis'." },
  { "en": "The project must be completed within the ___ timeframe.", "id": "Jawaban: allotted. Penjelasan: 'Allotted' adalah kata sifat partisipatif yang berarti 'diberikan atau ditugaskan'." },
  { "en": "The company's success is ___ on innovation.", "id": "Jawaban: predicated. Penjelasan: 'Predicated on' adalah frasa formal yang berarti 'didasarkan pada'." },
  { "en": "His ___ comments were not helpful to the discussion.", "id": "Jawaban: cynical. Penjelasan: 'Cynical' berarti 'percaya bahwa orang dimotivasi oleh kepentingan pribadi; tidak percaya pada ketulusan manusia'." },
  { "en": "The organization works to ___ the effects of natural disasters.", "id": "Jawaban: mitigate. Penjelasan: 'Mitigate' adalah kata kerja formal yang berarti 'membuat tidak terlalu parah, serius, atau menyakitkan'." },
  { "en": "The ___ between the two candidates was televised nationally.", "id": "Jawaban: debate. Penjelasan: 'Debate' adalah diskusi formal tentang topik tertentu." },
  { "en": "The ___ of the matter is that we are running out of time.", "id": "Jawaban: reality. Penjelasan: 'Reality' adalah keadaan sebagaimana adanya." },
  { "en": "The company is taking ___ to improve its environmental record.", "id": "Jawaban: steps. Penjelasan: 'Taking steps' adalah idiom yang berarti mulai bertindak." },
  { "en": "His ___ to detail is remarkable.", "id": "Jawaban: attention. Penjelasan: 'Attention to detail' adalah frasa standar." },
  { "en": "The two systems work in ___ to achieve the same goal.", "id": "Jawaban: tandem. Penjelasan: 'In tandem' adalah idiom yang berarti 'berdampingan; bersama-sama'." },
  { "en": "The city's ___ plan includes new parks and public spaces.", "id": "Jawaban: urban. Penjelasan: 'Urban' adalah kata sifat yang berkaitan dengan kota." },
  { "en": "The project's success is ___ to the hard work of the entire team.", "id": "Jawaban: a testament. Penjelasan: 'A testament to' berarti 'bukti dari'." },
  { "en": "The new drug has shown ___ results in clinical trials.", "id": "Jawaban: promising. Penjelasan: 'Promising' adalah kata sifat yang berarti 'menunjukkan tanda-tanda keberhasilan di masa depan'." },
  { "en": "The company's financial situation is ___, but it is expected to improve.", "id": "Jawaban: precarious. Penjelasan: 'Precarious' adalah kata sifat yang berarti 'tidak aman atau pasti; berbahaya'." },
  { "en": "He has a ___ for getting into trouble.", "id": "Jawaban: propensity. Penjelasan: 'Propensity' adalah kata benda formal untuk 'kecenderungan atau kecenderungan alami untuk berperilaku dengan cara tertentu'." },
  { "en": "The new software is designed to ___ with existing systems.", "id": "Jawaban: integrate. Penjelasan: 'Integrate with' berarti 'menggabungkan dengan yang lain sehingga menjadi satu kesatuan'." },
  { "en": "___ by the complexity of the problem, the scientists requested more time.", "id": "Jawaban: Baffled. Penjelasan: Ini adalah klausa adverbia yang disingkat dari 'Because they were baffled...'. Partisipel lampau 'Baffled' digunakan karena subjek (the scientists) menerima perasaan bingung." },
  { "en": "___ the necessary qualifications, he was not considered for the position.", "id": "Jawaban: Lacking. Penjelasan: Disingkat dari 'Because he lacked...'. Partisipel sekarang 'Lacking' digunakan untuk kata kerja aktif." },
  { "en": "Once ___, this new technology will revolutionize the industry.", "id": "Jawaban: implemented. Penjelasan: Disingkat dari 'Once it is implemented...'. Partisipel lampau digunakan untuk kalimat pasif." },
  { "en": "___ for many hours, the driver began to feel sleepy.", "id": "Jawaban: Having driven. Penjelasan: 'Perfect participle' ('Having driven') digunakan untuk menekankan bahwa tindakan mengemudi terjadi selama periode waktu sebelum rasa kantuk dimulai." },
  { "en": "When ___ about her future plans, the candidate gave a confident answer.", "id": "Jawaban: asked. Penjelasan: Disingkat dari 'When she was asked...'. Kalimat pasif memerlukan partisipel lampau 'asked'." },
  { "en": "My brother works in finance, and so ___ my sister.", "id": "Jawaban: does. Penjelasan: 'So + auxiliary + subject' digunakan untuk persetujuan afirmatif. Kata kerja bantu 'does' digunakan untuk setuju dengan kata kerja simple present 'works'." },
  { "en": "She has never been to Asia, and ___ have I.", "id": "Jawaban: neither. Penjelasan: 'Neither + auxiliary + subject' digunakan untuk persetujuan negatif. Kata kerja bantu 'have' cocok dengan present perfect tense." },
  { "en": "They will not accept the proposal, and nor ___ the other committee members.", "id": "Jawaban: will. Penjelasan: 'Nor + auxiliary + subject' adalah cara lain untuk menyatakan persetujuan negatif. Kata kerja bantu 'will' cocok dengan future tense." },
  { "en": "A more experienced diplomat ___ have prevented the conflict.", "id": "Jawaban: might. Penjelasan: Ini menyiratkan kondisi seperti 'If the diplomat had been more experienced...'. 'might have' menyatakan kemungkinan di masa lalu." },
  { "en": "I would have attended the conference, but I ___ a prior commitment.", "id": "Jawaban: had. Penjelasan: Klausa utama 'I would have attended...' adalah conditional ketiga. Klausa 'but' menjelaskan situasi nyata yang mencegah situasi hipotetis. Kondisi tersiratnya adalah 'if I had not had a prior commitment'." },
  { "en": "He ran to the station; otherwise, he ___ the train.", "id": "Jawaban: would have missed. Penjelasan: 'Otherwise' memperkenalkan hasil dari kondisi negatif tersirat (yaitu, 'jika dia tidak lari ke stasiun')." },
  { "en": "To hear him talk, you ___ think he was the CEO.", "id": "Jawaban: would. Penjelasan: Frasa infinitif 'To hear him talk' menyiratkan kondisi ('If you heard him talk...'), yang memerlukan 'would' dalam klausa hasilnya." },
  { "en": "Understanding the cultural nuances of a foreign country ___ essential for successful business negotiations.", "id": "Jawaban: is. Penjelasan: Subjek kalimat adalah frasa gerund 'Understanding...', yang tunggal. Frasa penengah yang panjang tidak mengubah persetujuan kata kerja." },
  { "en": "Implementing such radical changes across all departments ___ a significant challenge.", "id": "Jawaban: presents. Penjelasan: Subjeknya adalah gerund 'Implementing', yang tunggal dan memerlukan kata kerja tunggal 'presents'." },
  { "en": "Having so many responsibilities at once ___ causing him a great deal of stress.", "id": "Jawaban: is. Penjelasan: Frasa gerund 'Having so many responsibilities' bertindak sebagai subjek tunggal." },
  { "en": "A number of important issues ___ discussed at the meeting.", "id": "Jawaban: were. Penjelasan: 'A number of' adalah idiom yang berarti 'banyak' dan menggunakan kata kerja jamak ('were')." },
  { "en": "The number of applicants for the program ___ exceeded our expectations.", "id": "Jawaban: has. Penjelasan: 'The number of' mengacu pada satu angka dan menggunakan kata kerja tunggal ('has')." },
  { "en": "The main office is located ___ 123 Main Street.", "id": "Jawaban: at. Penjelasan: 'At' digunakan dengan alamat spesifik." },
  { "en": "He lives ___ a small apartment on the third floor.", "id": "Jawaban: in. Penjelasan: 'In' digunakan untuk ruang tertutup seperti apartemen. 'On' digunakan untuk lantai." },
  { "en": "The store is closed ___ public holidays.", "id": "Jawaban: on. Penjelasan: 'On' digunakan untuk hari dan tanggal tertentu, termasuk hari libur." },
  { "en": "___ the day, he works as a clerk, but at night he is a musician.", "id": "Jawaban: During. Penjelasan: 'During' adalah preposisi yang digunakan untuk berarti 'sepanjang berlangsungnya' suatu periode waktu. 'While' adalah konjungsi dan akan memerlukan klausa." },
  { "en": "The tech startup is still in its ___ stage, but it shows great promise.", "id": "Jawaban: nascent. Penjelasan: 'Nascent' adalah kata sifat yang berarti '(terutama proses atau organisasi) yang baru saja muncul dan mulai menunjukkan tanda-tanda potensi di masa depan'." },
  { "en": "It is ___ upon the government to protect its citizens.", "id": "Jawaban: incumbent. Penjelasan: 'Incumbent upon' adalah frasa formal yang berarti 'perlu bagi (seseorang) sebagai tugas atau tanggung jawab'." },
  { "en": "The controversial remarks ___ an angry response from the public.", "id": "Jawaban: precipitated. Penjelasan: 'Precipitate' adalah kata kerja yang berarti 'menyebabkan (suatu peristiwa atau situasi, biasanya yang buruk) terjadi secara tiba-tiba, tak terduga, atau prematur'." },
  { "en": "The lack of rain ___ the effects of the drought.", "id": "Jawaban: exacerbated. Penjelasan: 'Exacerbate' adalah kata kerja yang berarti 'memperburuk (masalah, situasi buruk, atau perasaan negatif)'." },
  { "en": "He only has a ___ understanding of the subject; he needs to study more.", "id": "Jawaban: rudimentary. Penjelasan: 'Rudimentary' adalah kata sifat yang berarti 'melibatkan atau terbatas pada prinsip-prinsip dasar'." },
  { "en": "The speaker's argument was ___ and well-supported by evidence.", "id": "Jawaban: cogent. Penjelasan: 'Cogent' adalah kata sifat yang berarti '(argumen atau kasus) yang jelas, logis, dan meyakinkan'." },
  { "en": "His ___ comments during the meeting were not helpful.", "id": "Jawaban: flippant. Penjelasan: 'Flippant' adalah kata sifat yang berarti 'tidak menunjukkan sikap serius atau hormat'." },
  { "en": "The two countries have a ___ relationship, marked by years of conflict.", "id": "Jawaban: volatile. Penjelasan: 'Volatile' adalah kata sifat yang berarti 'cenderung berubah dengan cepat dan tak terduga, terutama menjadi lebih buruk'." },
  { "en": "The new policy is designed to ___ the transition to a green economy.", "id": "Jawaban: facilitate. Penjelasan: 'Facilitate' adalah kata kerja yang berarti 'membuat (suatu tindakan atau proses) menjadi mudah atau lebih mudah'." },
  { "en": "The ancient text is full of ___ references that are difficult for modern readers to understand.", "id": "Jawaban: archaic. Penjelasan: 'Archaic' adalah kata sifat yang berarti 'sangat tua atau kuno'." },
  { "en": "He is known for his ___ lifestyle, avoiding all modern technology.", "id": "Jawaban: ascetic. Penjelasan: 'Ascetic' menggambarkan gaya hidup yang ditandai dengan disiplin diri yang keras dan pantang dari segala bentuk pemanjaan." },
  { "en": "The CEO's ___ speech inspired the employees to work harder.", "id": "Jawaban: eloquent. Penjelasan: 'Eloquent' berarti 'fasih atau persuasif dalam berbicara atau menulis'." },
  { "en": "The company is looking for ways to ___ its market share.", "id": "Jawaban: consolidate. Penjelasan: 'Consolidate' berarti 'memperkuat (posisi kekuasaan atau kesuksesan seseorang) sehingga menjadi lebih aman'." },
  { "en": "The old laws are no longer ___ in today's society.", "id": "Jawaban: relevant. Penjelasan: 'Relevant' adalah kata sifat yang berarti 'berkaitan erat atau sesuai dengan apa yang sedang dilakukan atau dipertimbangkan'." },
  { "en": "The politician made a ___ attempt to hide his involvement in the scandal.", "id": "Jawaban: blatant. Penjelasan: 'Blatant' adalah kata sifat yang berarti '(perilaku buruk) yang dilakukan secara terbuka dan tanpa malu-malu'." },
  { "en": "The company's profits have been ___ for the past three years.", "id": "Jawaban: stagnant. Penjelasan: 'Stagnant' adalah kata sifat yang berarti 'tidak menunjukkan aktivitas; lesu dan lamban'." },
  { "en": "The new evidence completely ___ the prosecution's case.", "id": "Jawaban: undermines. Penjelasan: 'Undermines' adalah kata kerja yang berarti 'mengurangi keefektifan, kekuatan, atau kemampuan, terutama secara bertahap atau diam-diam'." },
  { "en": "The two companies are ___ for the same government contract.", "id": "Jawaban: vying. Penjelasan: 'Vying' adalah partisipel sekarang dari 'vie', yang berarti 'bersaing dengan penuh semangat dengan seseorang untuk melakukan atau mencapai sesuatu'." },
  { "en": "The team is composed ___ of experts from various fields.", "id": "Jawaban: exclusively. Penjelasan: 'Exclusively' adalah kata keterangan yang berarti 'dengan mengecualikan yang lain; hanya'." },
  { "en": "The new law has been met with ___ opposition.", "id": "Jawaban: widespread. Penjelasan: 'Widespread' adalah kata sifat yang berarti 'ditemukan atau didistribusikan di area yang luas atau oleh banyak orang'." },
  { "en": "The government has a ___ interest in maintaining economic stability.", "id": "Jawaban: vested. Penjelasan: 'Vested interest' adalah alasan pribadi untuk terlibat dalam suatu usaha, terutama harapan keuntungan finansial atau lainnya." },
  { "en": "The project was ___ from the beginning due to a lack of funding.", "id": "Jawaban: doomed. Penjelasan: 'Doomed' adalah kata sifat yang berarti 'kemungkinan besar akan mengalami nasib yang tidak menguntungkan dan tak terhindarkan'." },
  { "en": "He has a ___ for dramatic storytelling.", "id": "Jawaban: flair. Penjelasan: 'Flair' adalah kata benda yang berarti 'bakat atau kemampuan khusus atau naluriah untuk melakukan sesuatu dengan baik'." },
  { "en": "The two accounts of the event are ___ different.", "id": "Jawaban: fundamentally. Penjelasan: 'Fundamentally' adalah kata keterangan yang berarti 'pada dasarnya atau utamanya'." },
  { "en": "The company's ___ into the European market was a huge success.", "id": "Jawaban: foray. Penjelasan: 'Foray' adalah kata benda yang digunakan secara metaforis untuk usaha atau percobaan pertama dalam suatu area." },
  { "en": "The city is a ___ of activity during the festival.", "id": "Jawaban: hub. Penjelasan: 'Hub' adalah 'pusat efektif dari suatu aktivitas, wilayah, atau jaringan'." },
  { "en": "The witness's statement was ___ and provided no useful information.", "id": "Jawaban: nebulous. Penjelasan: 'Nebulous' adalah kata sifat yang berarti '(konsep atau ide) yang tidak jelas, samar-samar, atau tidak terdefinisi dengan baik'." },
  { "en": "The country is on the ___ of a major political change.", "id": "Jawaban: brink. Penjelasan: 'On the brink of' adalah idiom yang berarti 'di ambang'." },
  { "en": "His ___ argument convinced the jury of his innocence.", "id": "Jawaban: plausible. Penjelasan: 'Plausible' berarti '(argumen atau pernyataan) yang tampak masuk akal atau mungkin'." },
  { "en": "The new policy is a ___ for disaster.", "id": "Jawaban: recipe. Penjelasan: 'A recipe for disaster' adalah idiom yang berarti 'situasi yang pasti akan mengarah pada bencana'." },
  { "en": "The company has a ___ on the local market.", "id": "Jawaban: monopoly. Penjelasan: 'Monopoly' adalah 'kepemilikan atau kontrol eksklusif atas pasokan atau perdagangan suatu komoditas atau jasa'." },
  { "en": "The ___ of the matter is that we must act now.", "id": "Jawaban: crux. Penjelasan: 'Crux' adalah 'poin yang menentukan atau paling penting dalam suatu masalah'." },
  { "en": "The old traditions have been ___ by modern technology.", "id": "Jawaban: supplanted. Penjelasan: 'Supplanted' adalah kata kerja yang berarti 'menggantikan'." },
  { "en": "The company is facing ___ financial difficulties.", "id": "Jawaban: dire. Penjelasan: 'Dire' adalah kata sifat yang berarti '(situasi atau peristiwa) yang sangat serius atau mendesak'." },
  { "en": "The author's latest novel has received ___ reviews from critics.", "id": "Jawaban: rave. Penjelasan: 'Rave reviews' adalah kolokasi untuk ulasan yang sangat antusias." },
  { "en": "The project is a ___ of the company's commitment to innovation.", "id": "Jawaban: reflection. Penjelasan: 'Reflection' adalah 'indikasi sifat sesuatu atau sikap seseorang'." },
  { "en": "The new CEO plans to ___ the company's structure.", "id": "Jawaban: overhaul. Penjelasan: 'Overhaul' adalah kata kerja yang berarti 'menganalisis dan memperbaiki secara menyeluruh'." },
  { "en": "His work is ___ to the success of the entire team.", "id": "Jawaban: integral. Penjelasan: 'Integral' adalah kata sifat yang berarti 'perlu untuk membuat keseluruhan menjadi lengkap; esensial atau fundamental'." },
  { "en": "The city's ___ includes a modern subway system and efficient buses.", "id": "Jawaban: infrastructure. Penjelasan: 'Infrastructure' adalah 'struktur dan fasilitas fisik dan organisasi dasar (misalnya, bangunan, jalan, pasokan listrik)'." },
  { "en": "The two issues are ___ and must be considered together.", "id": "Jawaban: intertwined. Penjelasan: 'Intertwined' adalah kata sifat yang digunakan secara metaforis untuk isu-isu yang saling terkait erat." },
  { "en": "The witness's testimony was ___ from the official record.", "id": "Jawaban: stricken. Penjelasan: 'Stricken from the record' adalah istilah hukum yang berarti 'dihapus secara resmi'." },
  { "en": "The ___ of the problem is a lack of communication.", "id": "Jawaban: root. Penjelasan: 'The root of the problem' adalah idiom untuk penyebab dasarnya." },
  { "en": "The new policy is a ___ departure from previous approaches.", "id": "Jawaban: radical. Penjelasan: 'Radical' berarti 'berkaitan dengan atau mempengaruhi sifat dasar sesuatu; jangkauannya luas atau menyeluruh'." },
  { "en": "He is ___ to criticism and does not take feedback well.", "id": "Jawaban: sensitive. Penjelasan: 'Sensitive to' berarti 'cepat mendeteksi atau merespons perubahan kecil' atau 'mudah tersinggung'." },
  { "en": "The company's profits ___ last quarter.", "id": "Jawaban: plummeted. Penjelasan: 'Plummeted' adalah kata kerja yang berarti 'jatuh atau turun lurus ke bawah dengan kecepatan tinggi'." },
  { "en": "The two candidates have ___ different visions for the country's future.", "id": "Jawaban: starkly. Penjelasan: 'Starkly' adalah kata keterangan yang menekankan perbedaan secara tajam atau jelas." },
  { "en": "The city is a ___ of cultural diversity.", "id": "Jawaban: mosaic. Penjelasan: 'Mosaic' digunakan secara metaforis untuk menggambarkan kombinasi elemen yang beragam." },
  { "en": "The company is trying to ___ its losses after a difficult year.", "id": "Jawaban: recoup. Penjelasan: 'Recoup' adalah kata kerja yang berarti 'mendapatkan kembali (sesuatu yang hilang atau telah dikeluarkan)'." },
  { "en": "His ___ to the details of the plan was impressive.", "id": "Jawaban: adherence. Penjelasan: 'Adherence' adalah bentuk kata benda yang berarti 'keterikatan atau komitmen pada seseorang, tujuan, atau keyakinan'." },
  { "en": "The new evidence ___ the defendant's alibi.", "id": "Jawaban: bolsters. Penjelasan: 'Bolsters' adalah kata kerja yang berarti 'mendukung atau memperkuat'." },
  { "en": "The old castle is ___ in mystery.", "id": "Jawaban: shrouded. Penjelasan: 'Shrouded in mystery' adalah idiom yang berarti 'diselimuti atau tersembunyi oleh misteri'." },
  { "en": "The decision was made to ___ the potential risks.", "id": "Jawaban: minimize. Penjelasan: 'Minimize' berarti 'mengurangi hingga jumlah atau tingkat sekecil mungkin'." },
  { "en": "The two countries have agreed to ___ trade barriers.", "id": "Jawaban: dismantle. Penjelasan: 'Dismantle' berarti 'membongkar', digunakan secara metaforis di sini untuk hambatan." },
  { "en": "The company's ___ in the market has grown steadily.", "id": "Jawaban: foothold. Penjelasan: 'Foothold' adalah 'posisi aman dari mana kemajuan lebih lanjut dapat dibuat'." },
  { "en": "His speech was a ___ call to action.", "id": "Jawaban: clarion. Penjelasan: 'Clarion call' adalah permintaan atau tuntutan tindakan yang diungkapkan dengan kuat." },
  { "en": "The evidence is ___ and does not prove anything.", "id": "Jawaban: circumstantial. Penjelasan: Bukti 'circumstantial' adalah bukti yang bergantung pada inferensi untuk menghubungkannya dengan kesimpulan fakta." },
  { "en": "The government plans to ___ the tax system.", "id": "Jawaban: reform. Penjelasan: 'Reform' berarti 'membuat perubahan untuk memperbaikinya'." },
  { "en": "The project is a ___ to the team's creativity and hard work.", "id": "Jawaban: monument. Penjelasan: 'Monument' dapat digunakan secara metaforis untuk berarti 'peringatan yang abadi dan pantas'." },
  { "en": "The new policy is ___ to be controversial.", "id": "Jawaban: bound. Penjelasan: 'Bound to be' adalah idiom yang berarti 'pasti akan terjadi'." },
  { "en": "He has a ___ for making friends easily.", "id": "Jawaban: penchant. Penjelasan: 'Penchant' adalah kesukaan yang kuat atau kecenderungan untuk melakukan sesuatu." },
  { "en": "The company's financial records are under ___ by auditors.", "id": "Jawaban: scrutiny. Penjelasan: 'Scrutiny' adalah 'pengamatan atau pemeriksaan kritis'." },
  { "en": "The two theories ___ on several key points.", "id": "Jawaban: converge. Penjelasan: 'Converge' berarti 'cenderung bertemu di satu titik'. Kebalikan dari 'diverge'." },
  { "en": "The ___ of the problem is that both sides refuse to compromise.", "id": "Jawaban: paradox. Penjelasan: 'Paradox' adalah proposisi yang tampaknya absurd atau kontradiktif tetapi mungkin terbukti benar." },
  { "en": "The company is on a ___ financial footing.", "id": "Jawaban: solid. Penjelasan: 'A solid footing' adalah idiom yang berarti dasar yang aman atau stabil." },
  { "en": "His explanation was ___ and did not satisfy the committee.", "id": "Jawaban: inadequate. Penjelasan: 'Inadequate' berarti 'kurang kualitas atau kuantitas yang dibutuhkan; tidak cukup untuk suatu tujuan'." },
  { "en": "The new law will ___ a new era of economic prosperity.", "id": "Jawaban: usher in. Penjelasan: 'Usher in' adalah kata kerja frasa yang berarti 'menandai dimulainya'." },
  { "en": "Of the three brothers, he is the ___ at playing chess.", "id": "Jawaban: best. Penjelasan: 'Best' adalah bentuk superlatif dari kata sifat tidak beraturan 'good'. Superlatif digunakan ketika membandingkan tiga hal atau lebih." },
  { "en": "The situation is far ___ than we had anticipated.", "id": "Jawaban: worse. Penjelasan: 'Worse' adalah bentuk komparatif dari kata sifat tidak beraturan 'bad'." },
  { "en": "The new evidence took the investigation ___ than the previous leads.", "id": "Jawaban: further. Penjelasan: 'Further' (atau 'farther') adalah bentuk komparatif dari 'far', digunakan untuk jarak atau tingkatan." },
  { "en": "This is by far the ___ interesting book I have read all year.", "id": "Jawaban: most. Penjelasan: Superlatif 'most interesting' digunakan dengan 'by far' untuk penekanan." },
  { "en": "The problem was more complicated ___ we first imagined.", "id": "Jawaban: than. Penjelasan: 'Than' digunakan untuk memperkenalkan bagian kedua dari sebuah perbandingan." },
  { "en": "You can sit ___ you like; there are plenty of empty seats.", "id": "Jawaban: wherever. Penjelasan: 'Wherever' berarti 'di mana saja' dan memperkenalkan klausa adverbial tempat." },
  { "en": "___ he says, you should not believe him because he often lies.", "id": "Jawaban: Whatever. Penjelasan: 'Whatever' berarti 'tidak peduli apa' dan memperkenalkan klausa kata benda yang berfungsi sebagai objek dari 'believe'." },
  { "en": "Feel free to call me ___ you need help.", "id": "Jawaban: whenever. Penjelasan: 'Whenever' berarti 'kapan saja' dan memperkenalkan klausa adverbial waktu." },
  { "en": "The company will hire ___ is best qualified for the job, regardless of their background.", "id": "Jawaban: whoever. Penjelasan: 'Whoever' berarti 'siapa pun yang' dan berfungsi sebagai subjek dari klausa 'is best qualified'." },
  { "en": "We should deal with the problem at ___ before it gets worse.", "id": "Jawaban: hand. Penjelasan: Idiom 'at hand' berarti 'saat ini dan perlu ditangani'. Ini adalah bentuk tetap tanpa artikel." },
  { "en": "In the ___ run, investing in education will benefit the entire country.", "id": "Jawaban: long. Penjelasan: Idiomnya adalah 'in the long run', yang berarti 'dalam jangka panjang; pada akhirnya'." },
  { "en": "Many people prefer to travel by ___ because it is more convenient.", "id": "Jawaban: car. Penjelasan: 'By car', 'by train', 'by plane' adalah frasa preposisional tetap yang tidak menggunakan artikel." },
  { "en": "As a ___ of fact, I have already finished the report.", "id": "Jawaban: matter. Penjelasan: Idiom 'as a matter of fact' digunakan untuk menambahkan detail atau menyangkal sesuatu." },
  { "en": "He spoke slowly and clearly ___ everyone could understand him.", "id": "Jawaban: so that. Penjelasan: 'So that' memperkenalkan klausa tujuan (alasan mengapa dia berbicara perlahan)." },
  { "en": "The storm was ___ powerful that it knocked down trees.", "id": "Jawaban: so. Penjelasan: 'So + adjective + that' memperkenalkan klausa hasil (efek dari badai yang kuat)." },
  { "en": "She saved her money ___ she could buy a new laptop.", "id": "Jawaban: so that. Penjelasan: Ini adalah klausa tujuan." },
  { "en": "There was ___ much traffic that we were an hour late.", "id": "Jawaban: so. Penjelasan: Ini adalah klausa hasil, menggunakan struktur 'so + much/many + noun + that'." },
  { "en": "After the long hike, the soup tasted ___.", "id": "Jawaban: wonderful. Penjelasan: 'Taste' digunakan di sini sebagai kata kerja penghubung, yang diikuti oleh kata sifat ('wonderful') yang mendeskripsikan subjek ('the soup')." },
  { "en": "He feels ___ about his performance in the interview.", "id": "Jawaban: bad. Penjelasan: 'Feel' adalah kata kerja penghubung yang menghubungkan subjek dengan kata sifat yang menggambarkan keadaan subjek." },
  { "en": "The roses in the garden smell ___.", "id": "Jawaban: sweet. Penjelasan: 'Smell' adalah kata kerja penghubung yang diikuti oleh kata sifat ('sweet')." },
  { "en": "She looked at the data ___.", "id": "Jawaban: carefully. Penjelasan: Di sini, 'look' adalah kata kerja aksi, bukan kata kerja penghubung. Harus dimodifikasi oleh kata keterangan ('carefully')." },
  { "en": "The ___ results of the experiment were surprising and differed from expectations.", "id": "Jawaban: anomalous. Penjelasan: 'Anomalous' adalah kata sifat yang berarti 'menyimpang dari apa yang standar, normal, atau diharapkan'." },
  { "en": "The government provided aid to ___ the suffering of the flood victims.", "id": "Jawaban: assuage. Penjelasan: 'Assuage' adalah kata kerja formal yang berarti 'membuat (perasaan tidak menyenangkan) menjadi tidak terlalu intens'." },
  { "en": "The long, boring lecture seemed to ___ the students rather than inspire them.", "id": "Jawaban: enervate. Penjelasan: 'Enervate' adalah kata kerja yang berarti 'menyebabkan (seseorang) merasa kehabisan energi atau vitalitas; melemahkan'." },
  { "en": "Her efforts to help the homeless are truly ___.", "id": "Jawaban: laudable. Penjelasan: 'Laudable' adalah kata sifat yang berarti '(tindakan, ide, atau tujuan) yang patut dipuji'." },
  { "en": "The politician's speech was deliberately ___, leaving his true intentions unclear.", "id": "Jawaban: opaque. Penjelasan: 'Opaque' dapat digunakan secara metaforis untuk berarti 'tidak mudah dipahami; tidak jelas'." },
  { "en": "His ___ spending habits led him into serious debt.", "id": "Jawaban: prodigal. Penjelasan: 'Prodigal' adalah kata sifat yang berarti 'menghabiskan uang atau sumber daya secara bebas dan sembrono; boros'." },
  { "en": "He is a ___ of the new policy, defending it against all criticism.", "id": "Jawaban: zealot. Penjelasan: 'Zealot' adalah orang yang fanatik dan tidak kompromi dalam mengejar cita-cita agama, politik, atau lainnya." },
  { "en": "The doctor advised him to ___ from alcohol for several weeks.", "id": "Jawaban: abstain. Penjelasan: 'Abstain from' adalah frasa kerja yang berarti 'menahan diri dari melakukan atau menikmati sesuatu'." },
  { "en": "It is illegal to ___ food products with cheaper, low-quality ingredients.", "id": "Jawaban: adulterate. Penjelasan: 'Adulterate' adalah kata kerja yang berarti 'membuat (sesuatu) menjadi lebih buruk kualitasnya dengan menambahkan zat lain'." },
  { "en": "She was ___ towards the election, feeling that her vote wouldn't make a difference.", "id": "Jawaban: apathetic. Penjelasan: 'Apathetic' adalah kata sifat yang berarti 'tidak menunjukkan minat, antusiasme, atau kepedulian'." },
  { "en": "Neither the players nor the coach ___ satisfied with the referee's decision.", "id": "Jawaban: was. Penjelasan: Dengan 'neither...nor', kata kerja setuju dengan subjek yang lebih dekat ('the coach', yang tunggal)." },
  { "en": "On the board ___ a list of instructions for the new employees.", "id": "Jawaban: was. Penjelasan: Kalimat ini terbalik. Subjeknya adalah 'a list' (tunggal), yang muncul setelah kata kerja." },
  { "en": "It's high time you ___ responsibility for your actions.", "id": "Jawaban: took. Penjelasan: Ekspresi 'It's high time' diikuti oleh subjek dan kata kerja dalam bentuk simple past." },
  { "en": "I would have called you, but my phone ___.", "id": "Jawaban: was broken. Penjelasan: Ini menyiratkan conditional ketiga ('If my phone hadn't been broken...'). Klausa 'but' memberikan situasi nyata di masa lalu." },
  { "en": "She is one of the few people ___ I can truly trust.", "id": "Jawaban: whom. Penjelasan: 'Whom' adalah objek dari kata kerja 'trust'. Dalam bahasa Inggris formal, 'whom' digunakan untuk orang dalam posisi objek." },
  { "en": "Not only ___ a talented singer, but she is also a gifted actress.", "id": "Jawaban: is she. Penjelasan: Ketika sebuah kalimat dimulai dengan 'Not only', subjek dan kata kerja bantu dibalik." },
  { "en": "He works too hard; ___, his health is starting to suffer.", "id": "Jawaban: consequently. Penjelasan: 'Consequently' adalah kata keterangan transisi yang berarti 'akibatnya'." },
  { "en": "He talks as if he ___ everything, but he actually knows very little.", "id": "Jawaban: knew. Penjelasan: Setelah 'as if', bentuk lampau ('knew') digunakan untuk menyatakan situasi yang tidak nyata di masa sekarang." },
  { "en": "The report needs ___ before it is submitted.", "id": "Jawaban: to be reviewed. Penjelasan: Infinitif pasif ('to be reviewed') diperlukan setelah kata kerja 'needs'." },
  { "en": "The company has an ___ reputation for quality and reliability.", "id": "Jawaban: impeccable. Penjelasan: 'Impeccable' adalah kata sifat yang berarti 'sesuai dengan standar tertinggi; tanpa cacat'." },
  { "en": "The old traditions have been ___ by modern customs.", "id": "Jawaban: eroded. Penjelasan: 'Eroded' digunakan secara metaforis untuk berarti 'terkikis atau hilang secara bertahap'." },
  { "en": "The politician tried to ___ the issue by changing the subject.", "id": "Jawaban: deflect. Penjelasan: 'Deflect' adalah kata kerja yang berarti 'menyebabkan (sesuatu) berubah arah' atau 'menyimpangkan'." },
  { "en": "The two companies are engaged in a ___ legal battle.", "id": "Jawaban: protracted. Penjelasan: 'Protracted' adalah kata sifat yang berarti 'berlangsung lama atau lebih lama dari yang diharapkan'." },
  { "en": "His ___ remarks made everyone in the room feel uncomfortable.", "id": "Jawaban: inappropriate. Penjelasan: 'Inappropriate' berarti 'tidak sesuai atau tidak pantas dalam situasi tersebut'." },
  { "en": "The documentary provides a ___ look at the lives of factory workers.", "id": "Jawaban: candid. Penjelasan: 'Candid' adalah kata sifat yang berarti 'jujur dan terus terang; blak-blakan'." },
  { "en": "The new evidence ___ the defendant's alibi.", "id": "Jawaban: corroborates. Penjelasan: 'Corroborates' adalah kata kerja yang berarti 'mengkonfirmasi atau memberikan dukungan kepada (pernyataan, teori, atau temuan)'." },
  { "en": "The company's failure was ___ due to poor management.", "id": "Jawaban: largely. Penjelasan: 'Largely' adalah kata keterangan yang berarti 'sebagian besar; pada umumnya'." },
  { "en": "The two cultures, ___ different, share some common values.", "id": "Jawaban: though. Penjelasan: 'Though' dapat digunakan sebagai kata keterangan yang berarti 'namun'." },
  { "en": "The project is a ___ to the team's perseverance.", "id": "Jawaban: testament. Penjelasan: 'A testament to' berarti 'berfungsi sebagai bukti nyata dari'." },
  { "en": "The new policy is designed to ___ innovation.", "id": "Jawaban: foster. Penjelasan: 'Foster' berarti 'mendorong atau mempromosikan pengembangan'." },
  { "en": "He has a ___ for getting his own way.", "id": "Jawaban: knack. Penjelasan: 'Knack' adalah keterampilan atau bakat khusus." },
  { "en": "The ___ of the matter is that we are out of options.", "id": "Jawaban: reality. Penjelasan: 'The reality of the matter' adalah frasa yang berarti 'situasi yang sebenarnya'." },
  { "en": "The new system is ___ with all previous versions of the software.", "id": "Jawaban: compatible. Penjelasan: 'Compatible with' berarti 'dapat ada atau berfungsi bersama tanpa konflik'." },
  { "en": "The country's economy has been ___ for several years.", "id": "Jawaban: sluggish. Penjelasan: 'Sluggish' adalah kata sifat yang berarti 'bergerak lambat atau tidak aktif'." },
  { "en": "His ___ to the company's success is undeniable.", "id": "Jawaban: contribution. Penjelasan: 'Contribution' adalah bentuk kata benda yang diperlukan di sini." },
  { "en": "The two sides have reached a ___ understanding.", "id": "Jawaban: mutual. Penjelasan: 'Mutual' berarti '(perasaan atau tindakan) yang dialami atau dilakukan oleh masing-masing dari dua pihak atau lebih terhadap yang lain'." },
  { "en": "The new regulations will have a ___ impact on small businesses.", "id": "Jawaban: significant. Penjelasan: 'Significant' berarti 'cukup besar atau penting untuk diperhatikan'." },
  { "en": "He is widely ___ as the father of modern computing.", "id": "Jawaban: regarded. Penjelasan: 'Regarded as' berarti 'dianggap atau dipandang sebagai'." },
  { "en": "The company is taking ___ to reduce its carbon footprint.", "id": "Jawaban: measures. Penjelasan: 'Taking measures' adalah kolokasi yang berarti 'mengambil tindakan'." },
  { "en": "The witness gave a ___ account of the accident.", "id": "Jawaban: coherent. Penjelasan: 'Coherent' berarti 'logis dan konsisten'." },
  { "en": "The old building is scheduled for ___.", "id": "Jawaban: demolition. Penjelasan: 'Demolition' adalah kata benda yang berarti 'tindakan atau proses penghancuran'." },
  { "en": "The new discovery has opened up a whole new ___ of research.", "id": "Jawaban: avenue. Penjelasan: 'Avenue of research' adalah kolokasi yang berarti 'jalur penyelidikan'." },
  { "en": "The team's victory was a ___ of their hard work.", "id": "Jawaban: result. Penjelasan: 'Result' adalah kata benda yang berarti 'konsekuensi, efek, atau hasil dari sesuatu'." },
  { "en": "The government has a ___ to protect its citizens.", "id": "Jawaban: duty. Penjelasan: 'Duty' adalah kewajiban moral atau hukum." },
  { "en": "His ___ is based on a misunderstanding of the facts.", "id": "Jawaban: argument. Penjelasan: 'Argument' adalah kata benda yang berarti 'alasan atau serangkaian alasan yang diberikan dengan tujuan meyakinkan orang lain'." },
  { "en": "The project was completed ___ budget and ahead of schedule.", "id": "Jawaban: under. Penjelasan: Idiomnya adalah 'under budget', yang berarti menghabiskan biaya kurang dari jumlah yang direncanakan." },
  { "en": "The two candidates have ___ different approaches to the problem.", "id": "Jawaban: completely. Penjelasan: 'Completely' adalah kata keterangan tingkat yang memodifikasi 'different'." },
  { "en": "The company has a ___ of being a great place to work.", "id": "Jawaban: reputation. Penjelasan: 'Reputation' adalah kumpulan keyakinan atau pendapat yang umumnya dipegang tentang seseorang atau sesuatu." },
  { "en": "The new policy is a ___ in the right direction.", "id": "Jawaban: step. Penjelasan: 'A step in the right direction' adalah idiom yang umum." },
  { "en": "The evidence is ___ and does not point to a clear conclusion.", "id": "Jawaban: ambiguous. Penjelasan: 'Ambiguous' berarti 'terbuka untuk lebih dari satu interpretasi'." },
  { "en": "The company is ___ for its customer service.", "id": "Jawaban: renowned. Penjelasan: 'Renowned for' berarti 'terkenal karena'." },
  { "en": "The two countries have a ___ relationship.", "id": "Jawaban: strained. Penjelasan: Hubungan yang 'strained' adalah hubungan yang tegang dan tidak santai." },
  { "en": "The new evidence ___ the original theory.", "id": "Jawaban: supports. Penjelasan: 'Supports' berarti 'mendukung' atau 'menguatkan'." },
  { "en": "The company's ___ is to provide high-quality products at affordable prices.", "id": "Jawaban: mission. Penjelasan: 'Mission' adalah tugas atau tujuan penting." },
  { "en": "The two designs are ___ the same.", "id": "Jawaban: essentially. Penjelasan: 'Essentially' adalah kata keterangan yang berarti 'digunakan untuk menekankan sifat dasar, fundamental, atau intrinsik dari seseorang atau sesuatu'." },
  { "en": "The project is a ___ of several different ideas.", "id": "Jawaban: synthesis. Penjelasan: 'Synthesis' adalah kombinasi ide untuk membentuk teori atau sistem." },
  { "en": "The new regulations are ___ and difficult to understand.", "id": "Jawaban: complex. Penjelasan: 'Complex' berarti 'terdiri dari banyak bagian yang berbeda dan terhubung'." },
  { "en": "The two sides are ___ to reach an agreement.", "id": "Jawaban: unable. Penjelasan: 'Unable' adalah kata sifat yang berarti 'tidak memiliki keterampilan, sarana, atau kesempatan untuk melakukan sesuatu'." },
  { "en": "The ___ of the problem is its complexity.", "id": "Jawaban: essence. Penjelasan: 'Essence' berarti 'sifat intrinsik atau kualitas yang sangat diperlukan dari sesuatu'." },
  { "en": "The new system is ___ to be more secure.", "id": "Jawaban: designed. Penjelasan: 'Designed to be' berarti 'direncanakan atau dimaksudkan untuk tujuan tertentu'." },
  { "en": "The two countries have a ___ of non-aggression.", "id": "Jawaban: pact. Penjelasan: 'Pact' adalah perjanjian formal antara individu atau pihak." },
  { "en": "His work is a ___ contribution to the field.", "id": "Jawaban: significant. Penjelasan: 'Significant' berarti 'penting' atau 'patut dicatat'." },
  { "en": "The company is ___ in a small town.", "id": "Jawaban: based. Penjelasan: 'Based in' digunakan untuk mengatakan di mana kantor utama atau bagian dari perusahaan berada." },
  { "en": "The two systems are ___ and cannot work together.", "id": "Jawaban: incompatible. Penjelasan: 'Incompatible' adalah kebalikan dari 'compatible'." },
  { "en": "The project is a ___ between public and private sectors.", "id": "Jawaban: partnership. Penjelasan: 'Partnership' adalah asosiasi dua orang atau lebih sebagai mitra." },
  { "en": "The new policy is a ___ issue among the employees.", "id": "Jawaban: divisive. Penjelasan: 'Divisive' adalah kata sifat yang berarti 'cenderung menyebabkan ketidaksepakatan atau permusuhan antara orang-orang'." },
  { "en": "The company is taking a ___ risk by entering the new market.", "id": "Jawaban: calculated. Penjelasan: 'Calculated risk' adalah risiko yang diambil setelah pertimbangan cermat terhadap hasil yang mungkin." },
  { "en": "The two leaders have a ___ to discuss the current crisis.", "id": "Jawaban: meeting. Penjelasan: 'Meeting' adalah pertemuan orang untuk tujuan tertentu." },
  { "en": "If the company ___ more in research and development, it would be a market leader now.", "id": "Jawaban: had invested. Penjelasan: Ini adalah 'mixed conditional'. Klausa 'if' tentang masa lalu ('had invested'), dan klausa hasilnya tentang masa sekarang ('would be...now')." },
  { "en": "If you ___ the blue button, the machine turns on.", "id": "Jawaban: press. Penjelasan: Ini adalah 'zero conditional', digunakan untuk kebenaran umum atau fakta. Strukturnya adalah 'If + simple present, simple present'." },
  { "en": "Should you ___ any problems, please do not hesitate to contact customer service.", "id": "Jawaban: encounter. Penjelasan: Ini adalah 'inverted first conditional' ('Should you encounter...' = 'If you encounter...'), sebuah struktur yang lebih formal." },
  { "en": "I wouldn't be in this difficult situation if I ___ your advice.", "id": "Jawaban: had taken. Penjelasan: Ini adalah 'mixed conditional', yang menyatakan hasil sekarang ('wouldn't be') dari kondisi masa lalu yang tidak nyata ('had taken')." },
  { "en": "If I ___ you, I would apologize for the mistake.", "id": "Jawaban: were. Penjelasan: Ini adalah 'second conditional' standar yang menggunakan subjunctive 'were' untuk memberikan nasihat." },
  { "en": "The report should ___ by the end of the week, but it's still not finished.", "id": "Jawaban: have been completed. Penjelasan: 'Should have been + past participle' adalah struktur modal pasif yang digunakan untuk membicarakan sesuatu yang seharusnya terjadi di masa lalu tetapi tidak." },
  { "en": "This issue must ___ with extreme care.", "id": "Jawaban: be handled. Penjelasan: Ini adalah struktur modal pasif sederhana: 'must + be + past participle'." },
  { "en": "More research could ___ in this area to verify the results.", "id": "Jawaban: be done. Penjelasan: 'could + be + past participle' menyatakan kemungkinan untuk tindakan pasif." },
  { "en": "The package might ___ to the wrong address by mistake.", "id": "Jawaban: have been sent. Penjelasan: 'might have been + past participle' menyatakan kemungkinan masa lalu untuk tindakan pasif." },
  { "en": "The rules can't ___ without approval from the board.", "id": "Jawaban: be changed. Penjelasan: 'can't + be + past participle' menyatakan bahwa tindakan pasif tidak mungkin atau tidak diizinkan." },
  { "en": "___ the heavy rain, the game continued as planned.", "id": "Jawaban: Despite. Penjelasan: 'Despite' adalah preposisi yang diikuti oleh frasa kata benda ('the heavy rain'). 'Although' adalah konjungsi dan memerlukan klausa." },
  { "en": "He decided to go for a walk, ___ he was feeling tired.", "id": "Jawaban: even though. Penjelasan: 'Even though' adalah konjungsi subordinatif yang diikuti oleh klausa ('he was feeling tired')." },
  { "en": "In spite ___ the difficulties, the team managed to succeed.", "id": "Jawaban: of. Penjelasan: Frasa preposisional yang benar adalah 'in spite of', yang diikuti oleh kata benda ('the difficulties')." },
  { "en": "The new plan has some advantages, ___ it also has several major flaws.", "id": "Jawaban: although. Penjelasan: 'Although' adalah konjungsi yang digunakan untuk menghubungkan dua klausa yang kontras." },
  { "en": "She passed the exam with a high score ___ not studying very much.", "id": "Jawaban: despite. Penjelasan: 'Despite' dapat diikuti oleh 'gerund' ('not studying')." },
  { "en": "I decided to write an email ___ making a phone call.", "id": "Jawaban: instead of. Penjelasan: 'Instead of' adalah preposisi yang diikuti oleh 'gerund' ('making')." },
  { "en": "He chose to resign ___ be fired from his position.", "id": "Jawaban: rather than. Penjelasan: 'Rather than' dapat menghubungkan dua kata kerja paralel dalam bentuk dasar ('resign'... 'be fired')." },
  { "en": "___ complaining about the problem, you should try to find a solution.", "id": "Jawaban: Instead of. Penjelasan: 'Instead of' digunakan di awal kalimat diikuti oleh 'gerund' untuk menyajikan tindakan alternatif." },
  { "en": "She prefers walking ___ driving to work.", "id": "Jawaban: rather than. Penjelasan: 'Rather than' sering digunakan dengan 'prefer' untuk menunjukkan pilihan antara dua tindakan (dalam hal ini, gerund)." },
  { "en": "The new manager's policies will ___ the entire department.", "id": "Jawaban: affect. Penjelasan: 'Affect' adalah kata kerja yang berarti 'mempengaruhi'." },
  { "en": "The positive ___ of the new medicine were immediately obvious.", "id": "Jawaban: effects. Penjelasan: 'Effect' adalah kata benda yang berarti 'hasil'." },
  { "en": "You should ___ the book on the table before you leave.", "id": "Jawaban: lay. Penjelasan: 'Lay' adalah kata kerja transitif yang berarti 'meletakkan sesuatu' dan memerlukan objek ('the book')." },
  { "en": "The sun ___ in the east.", "id": "Jawaban: rises. Penjelasan: 'Rise' adalah kata kerja intransitif yang berarti 'terbit' dan tidak memerlukan objek. 'Raise' memerlukan objek." },
  { "en": "Her blue scarf ___ her green eyes.", "id": "Jawaban: complements. Penjelasan: 'Complement' (dengan 'e') berarti 'melengkapi atau menyempurnakan'. 'Compliment' (dengan 'i') berarti 'memuji'." },
  { "en": "Her decisions often seem ___, changing from one day to the next without reason.", "id": "Jawaban: capricious. Penjelasan: 'Capricious' adalah kata sifat yang berarti 'cenderung berubah-ubah suasana hati atau perilaku secara tiba-tiba dan tidak dapat dijelaskan'." },
  { "en": "The author's vivid descriptions ___ a feeling of nostalgia in the reader.", "id": "Jawaban: engender. Penjelasan: 'Engender' adalah kata kerja formal yang berarti 'menyebabkan atau menimbulkan (perasaan, situasi, atau kondisi)'." },
  { "en": "He was too ___ and believed every promise the salesperson made.", "id": "Jawaban: gullible. Penjelasan: 'Gullible' adalah kata sifat yang berarti 'mudah dibujuk untuk mempercayai sesuatu; mudah tertipu'." },
  { "en": "The manager tried to ___ the angry employees by offering them a bonus.", "id": "Jawaban: placate. Penjelasan: 'Placate' adalah kata kerja yang berarti 'membuat (seseorang) tidak terlalu marah atau bermusuhan'." },
  { "en": "He tends to ___ between wanting to start a business and wanting a stable job.", "id": "Jawaban: vacillate. Penjelasan: 'Vacillate' adalah kata kerja yang berarti 'berubah-ubah antara pendapat atau tindakan yang berbeda; bimbang'." },
  { "en": "In many cultures, it is a tradition to ___ one's ancestors.", "id": "Jawaban: venerate. Penjelasan: 'Venerate' adalah kata kerja formal yang berarti 'sangat menghormati; memuja'." },
  { "en": "The professor's lecture was so ___ that half the class fell asleep.", "id": "Jawaban: soporific. Penjelasan: 'Soporific' adalah kata sifat yang berarti 'cenderung menyebabkan kantuk atau tidur'." },
  { "en": "The strange experimental result was considered an ___ by the research team.", "id": "Jawaban: aberration. Penjelasan: 'Aberration' adalah 'penyimpangan dari apa yang normal, biasa, atau diharapkan, biasanya yang tidak diinginkan'." },
  { "en": "She accepted the new assignment with ___, eager to begin.", "id": "Jawaban: alacrity. Penjelasan: 'Alacrity' adalah kata benda yang berarti 'kesiapan yang cepat dan ceria'." },
  { "en": "There is a ___ of evidence to support his theory, making it unconvincing.", "id": "Jawaban: paucity. Penjelasan: 'Paucity' adalah kata benda yang berarti 'kehadiran sesuatu hanya dalam jumlah kecil atau tidak mencukupi; kelangkaan'." },
  { "en": "A number of reasons ___ given for the project's delay.", "id": "Jawaban: were. Penjelasan: 'A number of' berarti 'banyak' dan menggunakan kata kerja jamak ('were')." },
  { "en": "Never ___ I heard such a ridiculous excuse.", "id": "Jawaban: have. Penjelasan: Ketika kalimat dimulai dengan kata keterangan negatif 'Never', subjek dan kata kerja bantu dibalik." },
  { "en": "It was ___ a beautiful day that we decided to have a picnic.", "id": "Jawaban: such. Penjelasan: 'Such a + adjective + noun + that' adalah struktur yang digunakan untuk menyatakan hasil." },
  { "en": "The committee, which ___ of ten members, meets once a month.", "id": "Jawaban: consists. Penjelasan: Subjek klausa adalah 'which' (merujuk pada 'The committee', tunggal), sehingga diperlukan kata kerja tunggal 'consists'." },
  { "en": "I would rather you ___ me the truth.", "id": "Jawaban: told. Penjelasan: Ketika 'would rather' diikuti oleh subjek, kata kerja berikutnya dalam bentuk lampau subjunctive ('told')." },
  { "en": "By the time the fire brigade arrived, the building ___ already burned down.", "id": "Jawaban: had. Penjelasan: 'Past perfect' ('had burned') digunakan untuk tindakan yang telah selesai sebelum tindakan lampau lainnya." },
  { "en": "It is imperative that every employee ___ the safety regulations.", "id": "Jawaban: follow. Penjelasan: Setelah ekspresi seperti 'It is imperative that', digunakan subjunctive mood (bentuk dasar kata kerja, 'follow')." },
  { "en": "I am not sure about ___ to invite to the party.", "id": "Jawaban: whom. Penjelasan: 'Whom' adalah objek dari infinitif 'to invite'." },
  { "en": "This is the university ___ my parents first met.", "id": "Jawaban: where. Penjelasan: 'Where' adalah kata keterangan relatif yang digunakan untuk merujuk pada suatu tempat." },
  { "en": "___ of the bad weather, all flights have been canceled.", "id": "Jawaban: Because. Penjelasan: 'Because of' adalah frasa preposisional yang diikuti oleh kata benda, digunakan untuk menunjukkan alasan." },
  { "en": "Her expertise lies ___ the field of corporate law.", "id": "Jawaban: within. Penjelasan: 'Within' adalah preposisi yang digunakan untuk menunjukkan sesuatu berada di dalam batas suatu area atau subjek." },
  { "en": "The company's main ___ is to increase market share.", "id": "Jawaban: objective. Penjelasan: 'Objective' adalah tujuan atau sasaran." },
  { "en": "The new evidence casts ___ on the defendant's story.", "id": "Jawaban: doubt. Penjelasan: 'To cast doubt on' adalah idiom umum yang berarti 'membuat sesuatu tampak kurang pasti atau dapat dipercaya'." },
  { "en": "The project was a ___ effort between several organizations.", "id": "Jawaban: joint. Penjelasan: 'Joint' adalah kata sifat yang berarti 'dibagi, dipegang, atau dibuat oleh dua pihak atau lebih secara bersama-sama'." },
  { "en": "The ___ of the matter is that we are behind schedule.", "id": "Jawaban: crux. Penjelasan: 'Crux' adalah titik terpenting dari suatu masalah." },
  { "en": "The new regulations are intended to ___ fair competition.", "id": "Jawaban: ensure. Penjelasan: 'Ensure' adalah kata kerja yang berarti 'memastikan bahwa (sesuatu) akan terjadi'." },
  { "en": "His work is ___ and always of the highest quality.", "id": "Jawaban: meticulous. Penjelasan: 'Meticulous' berarti 'menunjukkan perhatian besar terhadap detail; sangat hati-hati dan teliti'." },
  { "en": "The company's profits ___ by 20% last quarter.", "id": "Jawaban: increased. Penjelasan: 'Increased' adalah kata kerja simple past yang dibutuhkan di sini." },
  { "en": "The new system is far superior ___ the old one.", "id": "Jawaban: to. Penjelasan: Kata sifat 'superior' diikuti oleh 'to', bukan 'than'." },
  { "en": "The government is facing pressure to ___ the problem.", "id": "Jawaban: address. Penjelasan: 'To address a problem' berarti 'menangani atau memikirkan suatu masalah'." },
  { "en": "The new policy is a ___ shift from the old one.", "id": "Jawaban: significant. Penjelasan: 'Significant' berarti 'penting' atau 'patut dicatat'." },
  { "en": "The two departments must ___ to achieve the company's goals.", "id": "Jawaban: cooperate. Penjelasan: 'Cooperate' berarti 'bekerja sama'." },
  { "en": "The ___ of the study were published in a medical journal.", "id": "Jawaban: findings. Penjelasan: 'Findings' adalah kata benda yang digunakan untuk hasil penelitian." },
  { "en": "He is a ___ supporter of the environmental cause.", "id": "Jawaban: staunch. Penjelasan: 'Staunch' adalah kata sifat yang berarti 'sangat setia dan berkomitmen dalam sikap'." },
  { "en": "The new evidence makes his story seem more ___.", "id": "Jawaban: plausible. Penjelasan: 'Plausible' berarti 'tampak masuk akal atau mungkin'." },
  { "en": "The ___ of the project will be determined by its profitability.", "id": "Jawaban: success. Penjelasan: 'Success' adalah bentuk kata benda yang dibutuhkan sebagai subjek." },
  { "en": "The new drug has shown ___ results in early trials.", "id": "Jawaban: promising. Penjelasan: 'Promising' adalah kata sifat yang berarti 'menunjukkan tanda-tanda keberhasilan di masa depan'." },
  { "en": "The two countries have a ___ agreement.", "id": "Jawaban: bilateral. Penjelasan: 'Bilateral' berarti 'melibatkan dua pihak, terutama negara'." },
  { "en": "The company is required to ___ with federal regulations.", "id": "Jawaban: comply. Penjelasan: 'Comply with' adalah kata kerja frasa yang berarti 'bertindak sesuai dengan keinginan atau perintah'." },
  { "en": "The two systems are ___ in function but different in design.", "id": "Jawaban: similar. Penjelasan: 'Similar' adalah kata sifat yang berarti 'menyerupai tanpa identik'." },
  { "en": "The project was completed ___ schedule.", "id": "Jawaban: on. Penjelasan: Idiomnya adalah 'on schedule', yang berarti 'tepat waktu sesuai rencana'." },
  { "en": "The new evidence ___ the defendant from all blame.", "id": "Jawaban: absolves. Penjelasan: 'Absolves' adalah kata kerja yang berarti 'menyatakan seseorang bebas dari kesalahan atau hukuman'." },
  { "en": "The two sides are ___ to reach a compromise.", "id": "Jawaban: trying. Penjelasan: 'Trying' adalah 'present participle' yang membentuk 'present continuous tense'." },
  { "en": "The new policy has ___ a lot of debate.", "id": "Jawaban: generated. Penjelasan: 'Generated' berarti 'menyebabkan atau menghasilkan'." },
  { "en": "The company's performance has been ___ expectations.", "id": "Jawaban: below. Penjelasan: 'Below expectations' adalah frasa umum yang berarti 'di bawah harapan'." },
  { "en": "The two leaders have a ___ of views on the issue.", "id": "Jawaban: difference. Penjelasan: 'A difference of views' berarti mereka tidak setuju." },
  { "en": "The company is known for its ___ business practices.", "id": "Jawaban: ethical. Penjelasan: 'Ethical' berarti 'baik atau benar secara moral'." },
  { "en": "The two departments work in ___ with each other.", "id": "Jawaban: conjunction. Penjelasan: 'In conjunction with' adalah frasa yang berarti 'bersama-sama dengan'." },
  { "en": "The new regulations are ___ and confusing.", "id": "Jawaban: ambiguous. Penjelasan: 'Ambiguous' berarti 'tidak jelas atau terbuka untuk lebih dari satu interpretasi'." },
  { "en": "The company is taking a ___ approach to risk management.", "id": "Jawaban: cautious. Penjelasan: 'Cautious' berarti 'berhati-hati untuk menghindari masalah atau bahaya potensial'." },
  { "en": "The two countries have a ___ relationship based on trade.", "id": "Jawaban: strong. Penjelasan: 'Strong' adalah kata sifat yang menggambarkan hubungan tersebut." },
  { "en": "The ___ of the problem is a lack of funding.", "id": "Jawaban: core. Penjelasan: 'The core of the problem' adalah bagian sentral atau terpenting dari masalah tersebut." },
  { "en": "The new system is designed to be ___ and easy to use.", "id": "Jawaban: intuitive. Penjelasan: 'Intuitive' berarti 'mudah digunakan dan dipahami'." },
  { "en": "The two companies have ___ their resources to work on the project.", "id": "Jawaban: pooled. Penjelasan: 'To pool resources' berarti menggabungkan sumber daya untuk tujuan bersama." },
  { "en": "His work is a ___ example of modern art.", "id": "Jawaban: prime. Penjelasan: 'Prime example' adalah contoh yang sangat baik atau representatif." },
  { "en": "The company is ___ in London.", "id": "Jawaban: headquartered. Penjelasan: 'Headquartered in' berarti memiliki kantor pusat di tempat tertentu." },
  { "en": "The two systems are ___ dependent on each other.", "id": "Jawaban: mutually. Penjelasan: 'Mutually dependent' berarti mereka saling bergantung satu sama lain." },
  { "en": "The project is a ___ for future development.", "id": "Jawaban: blueprint. Penjelasan: 'Blueprint' adalah rencana desain; digunakan secara metaforis untuk rencana terperinci." },
  { "en": "The new policy is a ___ issue.", "id": "Jawaban: contentious. Penjelasan: 'Contentious' berarti 'menyebabkan atau cenderung menyebabkan argumen'." },
  { "en": "The company is taking a ___ step to enter the new market.", "id": "Jawaban: bold. Penjelasan: 'Bold' berarti 'percaya diri dan berani'." },
  { "en": "The two leaders have a ___ to meet annually.", "id": "Jawaban: commitment. Penjelasan: 'Commitment' adalah perjanjian atau kewajiban yang membatasi kebebasan bertindak." },
  { "en": "The plot of the movie was very ___, and it kept me on the edge of my seat.", "id": "Jawaban: exciting. Penjelasan: 'Exciting' (bentuk -ing) digunakan untuk mendeskripsikan hal yang menyebabkan perasaan (plot film)." },
  { "en": "I was ___ to hear that I had won the competition.", "id": "Jawaban: thrilled. Penjelasan: 'Thrilled' (bentuk -ed) digunakan untuk mendeskripsikan bagaimana perasaan seseorang." },
  { "en": "The professor's lecture on quantum physics was ___.", "id": "Jawaban: fascinating. Penjelasan: Kuliah tersebut adalah penyebab perasaan, jadi bentuk -ing digunakan." },
  { "en": "After working a 12-hour shift, the nurses were completely ___.", "id": "Jawaban: exhausted. Penjelasan: Bentuk -ed mendeskripsikan keadaan para perawat." },
  { "en": "The instructions for the new software are quite ___.", "id": "Jawaban: confusing. Penjelasan: 'Confusing' (-ing) mendeskripsikan instruksi. Seseorang akan merasa 'confused' (-ed) olehnya." },
  { "en": "You will not be allowed to take the exam ___ you have a valid ID.", "id": "Jawaban: unless. Penjelasan: 'Unless' adalah konjungsi yang berarti 'kecuali jika'." },
  { "en": "___ he is known for being quiet, he is actually quite talkative among his friends.", "id": "Jawaban: While. Penjelasan: 'While' digunakan di sini untuk menunjukkan kontras, mirip dengan 'although'." },
  { "en": "___ the company is based in the US, it has offices all over the world.", "id": "Jawaban: Although. Penjelasan: 'Although' memperkenalkan klausa konsesi atau kontras." },
  { "en": "___ you have already completed the prerequisite courses, you may register for the advanced class.", "id": "Jawaban: Since. Penjelasan: 'Since' digunakan di sini untuk berarti 'karena' atau untuk memperkenalkan alasan." },
  { "en": "He is a respected expert, ___ his colleague is still a novice.", "id": "Jawaban: whereas. Penjelasan: 'Whereas' digunakan untuk membuat perbandingan langsung atau menunjukkan kontras yang kuat antara dua fakta." },
  { "en": "The museum features works by famous impressionist painters, ___ Monet and Renoir.", "id": "Jawaban: such as. Penjelasan: 'Such as' digunakan untuk memperkenalkan contoh spesifik dari kategori yang baru saja disebutkan. Ini lebih formal daripada 'like'." },
  { "en": "He enjoys outdoor activities ___ hiking and camping.", "id": "Jawaban: like. Penjelasan: 'Like' sering digunakan secara informal untuk memperkenalkan contoh. 'Such as' juga benar dan lebih formal." },
  { "en": "Many countries, ___ Japan and Germany, have aging populations.", "id": "Jawaban: such as. Penjelasan: 'Such as' sesuai untuk memperkenalkan contoh dalam konteks formal atau akademis." },
  { "en": "The committee ___ scheduled its next meeting for Tuesday.", "id": "Jawaban: has. Penjelasan: Di sini, 'committee' bertindak sebagai satu unit, sehingga menggunakan kata kerja tunggal ('has')." },
  { "en": "The jury ___ still deliberating, unable to agree on a verdict.", "id": "Jawaban: are. Penjelasan: Di sini, 'jury' menyiratkan anggota individu yang tidak dapat setuju, sehingga kata kerja jamak ('are') lebih tepat untuk menekankan perpecahan internal." },
  { "en": "The faculty ___ unanimously approved the new proposal.", "id": "Jawaban: has. Penjelasan: Kata 'unanimously' menunjukkan fakultas bertindak sebagai satu unit, memerlukan kata kerja tunggal." },
  { "en": "He worked ___ to meet the deadline.", "id": "Jawaban: diligently in his office all week. Penjelasan: Urutan standar kata keterangan adalah Cara ('diligently'), Tempat ('in his office'), dan Waktu ('all week')." },
  { "en": "The birds migrate ___ every year.", "id": "Jawaban: south in the winter. Penjelasan: Urutannya adalah Tempat ('south') kemudian Waktu ('in the winter')." },
  { "en": "She spoke ___ during the presentation.", "id": "Jawaban: confidently at the conference yesterday. Penjelasan: Urutannya adalah Cara ('confidently'), Tempat ('at the conference'), dan Waktu ('yesterday')." },
  { "en": "The teacher made the students ___ their essays.", "id": "Jawaban: rewrite. Penjelasan: Kausatif aktif 'make someone do something' menggunakan bentuk dasar kata kerja ('rewrite')." },
  { "en": "I need to have my passport ___.", "id": "Jawaban: renewed. Penjelasan: Kausatif pasif 'have something done' menggunakan partisipel lampau ('renewed')." },
  { "en": "She let her son ___ to the party.", "id": "Jawaban: go. Penjelasan: 'Let someone do something' menggunakan bentuk dasar ('go')." },
  { "en": "He got a professional ___ his house.", "id": "Jawaban: to paint. Penjelasan: Kausatif aktif 'get someone to do something' menggunakan infinitif ('to paint')." },
  { "en": "His argument that the Earth is flat is a clear ___ in the 21st century.", "id": "Jawaban: anachronism. Penjelasan: 'Anachronism' adalah sesuatu yang pantas atau berasal dari periode selain dari periode keberadaannya saat ini." },
  { "en": "The crowd was ___ after their team won the championship.", "id": "Jawaban: boisterous. Penjelasan: 'Boisterous' adalah kata sifat yang berarti 'berisik, energik, dan ceria; gaduh'." },
  { "en": "There is a strong sense of ___ among the members of the team.", "id": "Jawaban: camaraderie. Penjelasan: 'Camaraderie' adalah kata benda yang berarti 'saling percaya dan persahabatan di antara orang-orang yang menghabiskan banyak waktu bersama'." },
  { "en": "The professor's lecture was interesting, but he made a long ___ about his vacation.", "id": "Jawaban: digression. Penjelasan: 'Digression' adalah penyimpangan sementara dari subjek utama dalam pidato atau tulisan." },
  { "en": "The beauty of the cherry blossoms is ___, lasting only for a week or two.", "id": "Jawaban: evanescent. Penjelasan: 'Evanescent' adalah kata sifat yang berarti 'segera berlalu dari pandangan, ingatan, atau keberadaan; cepat memudar atau menghilang'." },
  { "en": "Known for his ___, he lived a simple life and saved most of his money.", "id": "Jawaban: frugality. Penjelasan: 'Frugality' adalah kualitas hemat dengan uang atau makanan; sifat hemat." },
  { "en": "He is a ___ person who enjoys parties and social gatherings.", "id": "Jawaban: gregarious. Penjelasan: 'Gregarious' adalah kata sifat yang berarti '(seseorang) yang suka berteman; ramah'." },
  { "en": "The coach delivered a fiery ___ to the team during halftime.", "id": "Jawaban: harangue. Penjelasan: 'Harangue' adalah pidato yang panjang dan agresif." },
  { "en": "His ___ decision to quit his job without another one lined up worried his family.", "id": "Jawaban: impetuous. Penjelasan: 'Impetuous' adalah kata sifat yang berarti 'bertindak atau dilakukan dengan cepat dan tanpa berpikir atau berhati-hati'." },
  { "en": "The soup was ___, lacking any real flavor.", "id": "Jawaban: insipid. Penjelasan: 'Insipid' adalah kata sifat yang berarti 'kurang rasa; hambar atau tawar'." },
  { "en": "The number of people attending the conference ___ smaller than expected.", "id": "Jawaban: was. Penjelasan: Subjeknya adalah 'The number' (tunggal), yang memerlukan kata kerja tunggal ('was')." },
  { "en": "If he ___ about the meeting, he would have been there.", "id": "Jawaban: had known. Penjelasan: Ini adalah conditional ketiga standar untuk situasi masa lalu yang hipotetis." },
  { "en": "The problem is far more serious than ___.", "id": "Jawaban: it appears. Penjelasan: Sebuah klausa sering melengkapi perbandingan, dengan 'than' bertindak sebagai konjungsi." },
  { "en": "___ the bad weather, we enjoyed our vacation.", "id": "Jawaban: In spite of. Penjelasan: 'In spite of' adalah frasa preposisional yang diikuti oleh frasa kata benda ('the bad weather')." },
  { "en": "He is considering ___ his job and starting his own business.", "id": "Jawaban: leaving. Penjelasan: Kata kerja 'consider' diikuti oleh 'gerund' ('leaving')." },
  { "en": "The manager, to ___ all employees report, will be retiring soon.", "id": "Jawaban: whom. Penjelasan: 'Whom' adalah objek dari preposisi 'to' dalam struktur klausa kata sifat formal ini." },
  { "en": "So loudly ___ that the neighbors complained.", "id": "Jawaban: did he shout. Penjelasan: Ketika kalimat dimulai dengan 'So + adverb', subjek dan kata kerja bantu dibalik." },
  { "en": "It's time the children ___ in bed.", "id": "Jawaban: were. Penjelasan: Setelah 'It's time', digunakan 'past subjunctive' ('were')." },
  { "en": "She is not used to ___ in such a large city.", "id": "Jawaban: living. Penjelasan: 'Be used to' diikuti oleh 'gerund' ('living')." },
  { "en": "Hardly had the sun set ___ it began to rain heavily.", "id": "Jawaban: when. Penjelasan: Strukturnya adalah 'Hardly...when'." },
  { "en": "His speech was designed to ___ support for his campaign.", "id": "Jawaban: garner. Penjelasan: 'Garner' adalah kata kerja yang berarti 'mengumpulkan atau mengoleksi (sesuatu, terutama informasi atau persetujuan)'." },
  { "en": "The professor's ___ style made complex subjects easy to understand.", "id": "Jawaban: lucid. Penjelasan: 'Lucid' berarti 'dinyatakan dengan jelas; mudah dimengerti'." },
  { "en": "The new evidence ___ the original theory.", "id": "Jawaban: refutes. Penjelasan: 'Refutes' adalah kata kerja yang berarti 'membuktikan (pernyataan atau teori) salah; menyangkal'." },
  { "en": "The company's ___ to safety is its top priority.", "id": "Jawaban: commitment. Penjelasan: 'Commitment' adalah kata benda untuk 'janji atau keputusan tegas untuk melakukan sesuatu'." },
  { "en": "The project was a ___ success, praised by critics and the public alike.", "id": "Jawaban: resounding. Penjelasan: 'Resounding success' adalah idiom untuk keberhasilan yang sangat besar dan diakui secara luas." },
  { "en": "The politician's comments were a ___ attempt to win votes.", "id": "Jawaban: transparent. Penjelasan: 'Transparent' dapat digunakan untuk berarti 'mudah dilihat atau dideteksi', seringkali dengan konotasi negatif karena terlalu jelas." },
  { "en": "The two countries have a ___ relationship, often cooperating on key issues.", "id": "Jawaban: cordial. Penjelasan: 'Cordial' adalah kata sifat yang berarti 'hangat dan ramah'." },
  { "en": "The manager is ___ for all final decisions.", "id": "Jawaban: accountable. Penjelasan: 'Accountable for' berarti 'diharuskan atau diharapkan untuk mempertanggungjawabkan tindakan atau keputusan; bertanggung jawab'." },
  { "en": "The artist's work is a ___ of his personal struggles.", "id": "Jawaban: manifestation. Penjelasan: 'Manifestation' adalah 'peristiwa, tindakan, atau objek yang dengan jelas menunjukkan atau mewujudkan sesuatu, terutama teori atau ide abstrak'." },
  { "en": "The new law is intended to ___ the rights of minorities.", "id": "Jawaban: safeguard. Penjelasan: 'Safeguard' adalah kata kerja yang berarti 'melindungi dari bahaya atau kerusakan dengan tindakan yang tepat'." },
  { "en": "His argument was based on ___ logic.", "id": "Jawaban: flawed. Penjelasan: 'Flawed' berarti 'memiliki atau ditandai oleh kelemahan atau ketidaksempurnaan mendasar'." },
  { "en": "The ___ of the company is located in New York.", "id": "Jawaban: headquarters. Penjelasan: 'Headquarters' adalah kata benda untuk kantor utama suatu organisasi." },
  { "en": "The two systems are ___ and serve different purposes.", "id": "Jawaban: distinct. Penjelasan: 'Distinct' berarti 'secara jelas berbeda sifatnya dari sesuatu yang lain dari jenis yang sama'." },
  { "en": "The company has ___ a new approach to customer service.", "id": "Jawaban: adopted. Penjelasan: 'Adopted' berarti 'memilih untuk mengambil atau mengikuti'." },
  { "en": "His ___ to the rules is very strict.", "id": "Jawaban: adherence. Penjelasan: 'Adherence' adalah kata benda yang berarti 'keterikatan atau komitmen pada suatu tujuan atau keyakinan'." },
  { "en": "The new evidence ___ his innocence.", "id": "Jawaban: proves. Penjelasan: 'Proves' berarti 'menunjukkan kebenaran atau keberadaan (sesuatu) dengan bukti atau argumen'." },
  { "en": "The company is a ___ in the field of renewable energy.", "id": "Jawaban: pioneer. Penjelasan: 'Pioneer' adalah orang yang termasuk yang pertama mengembangkan bidang pengetahuan atau aktivitas baru." },
  { "en": "The two designs are ___ in many respects.", "id": "Jawaban: analogous. Penjelasan: 'Analogous' adalah kata sifat yang berarti 'dapat dibandingkan dalam hal-hal tertentu'." },
  { "en": "The project is a ___ of international cooperation.", "id": "Jawaban: model. Penjelasan: 'Model' adalah 'sesuatu yang digunakan sebagai contoh untuk diikuti atau ditiru'." },
  { "en": "The new regulations are ___ for all employees.", "id": "Jawaban: mandatory. Penjelasan: 'Mandatory' berarti 'diwajibkan oleh hukum atau peraturan; wajib'." },
  { "en": "His ___ to detail is a great asset.", "id": "Jawaban: attention. Penjelasan: 'Attention to detail' adalah frasa standar." },
  { "en": "The new system is designed to ___ operations.", "id": "Jawaban: streamline. Penjelasan: 'Streamline' berarti 'membuat (organisasi atau sistem) lebih efisien dan efektif'." },
  { "en": "The two countries have a ___ history of conflict.", "id": "Jawaban: complex. Penjelasan: 'Complex' berarti 'terdiri dari banyak bagian yang berbeda dan terhubung'." },
  { "en": "The project is a ___ success.", "id": "Jawaban: monumental. Penjelasan: 'Monumental' adalah kata sifat yang berarti 'sangat besar pentingnya, luasnya, atau ukurannya'." },
  { "en": "The new evidence is ___ with the defendant's story.", "id": "Jawaban: consistent. Penjelasan: 'Consistent with' berarti 'sesuai dengan'." },
  { "en": "The company is a ___ of the local community.", "id": "Jawaban: pillar. Penjelasan: 'Pillar' komunitas adalah anggota yang penting dan dihormati." },
  { "en": "The two designs are ___ different.", "id": "Jawaban: stylistically. Penjelasan: 'Stylistically' adalah kata keterangan yang diperlukan untuk memodifikasi kata sifat 'different'." },
  { "en": "The project is a ___ of many people's hard work.", "id": "Jawaban: product. Penjelasan: 'Product' digunakan di sini untuk berarti 'hasil dari suatu tindakan atau proses'." },
  { "en": "The two companies are ___ on a new project.", "id": "Jawaban: collaborating. Penjelasan: 'Collaborating' adalah 'present participle' yang membentuk 'present continuous tense'." },
  { "en": "His ___ to the team has been crucial.", "id": "Jawaban: value. Penjelasan: 'Value' adalah kata benda yang berarti 'pentingnya, nilai, atau kegunaan sesuatu'." },
  { "en": "The new system is ___ to be more reliable.", "id": "Jawaban: supposed. Penjelasan: 'Supposed to be' digunakan untuk menunjukkan apa yang dimaksudkan atau diharapkan." },
  { "en": "The two countries have a ___ relationship.", "id": "Jawaban: cooperative. Penjelasan: 'Cooperative' adalah bentuk kata sifat." },
  { "en": "The project is a ___ of modern engineering.", "id": "Jawaban: marvel. Penjelasan: 'Marvel' adalah orang atau benda yang menakjubkan atau mengagumkan." },
  { "en": "The new regulations are ___ to follow.", "id": "Jawaban: easy. Penjelasan: 'Easy' adalah kata sifat." },
  { "en": "The two companies have a ___ to compete fairly.", "id": "Jawaban: pact. Penjelasan: 'Pact' adalah perjanjian formal." },
  { "en": "His work is ___ of his dedication.", "id": "Jawaban: indicative. Penjelasan: 'Indicative of' berarti 'berfungsi sebagai tanda atau indikasi dari'." },
  { "en": "The project is a ___ success story.", "id": "Jawaban: classic. Penjelasan: 'Classic' adalah 'dinilai selama periode waktu tertentu memiliki kualitas tertinggi dan luar biasa dari jenisnya'." },
  { "en": "The new system is designed to be ___ with mobile devices.", "id": "Jawaban: compatible. Penjelasan: 'Compatible' berarti 'dapat ada atau terjadi bersama tanpa masalah atau konflik'." },
  { "en": "The two countries have ___ diplomatic ties.", "id": "Jawaban: severed. Penjelasan: 'To sever ties' berarti 'memutuskan hubungan'." },
  { "en": "His ___ of the situation was very accurate.", "id": "Jawaban: assessment. Penjelasan: 'Assessment' adalah evaluasi." },
  { "en": "The new policy is a ___ of the company's new direction.", "id": "Jawaban: sign. Penjelasan: 'Sign' adalah indikasi." },
  { "en": "The two systems are ___ linked.", "id": "Jawaban: inextricably. Penjelasan: 'Inextricably' adalah kata keterangan yang berarti 'dengan cara yang tidak mungkin diurai atau dipisahkan'." },
  { "en": "The project is a ___ of our commitment to sustainability.", "id": "Jawaban: demonstration. Penjelasan: 'Demonstration' adalah tindakan menunjukkan bahwa sesuatu ada atau benar dengan memberikan bukti." },
  { "en": "The new regulations are ___ and must be followed.", "id": "Jawaban: binding. Penjelasan: 'Binding' berarti '(perjanjian atau janji) yang melibatkan kewajiban yang tidak dapat dilanggar'." },
  { "en": "The two companies have a ___ for innovation.", "id": "Jawaban: reputation. Penjelasan: 'Reputation' adalah kumpulan keyakinan yang dipegang tentang sesuatu." },
  { "en": "His work is a ___ of modern art.", "id": "Jawaban: masterpiece. Penjelasan: 'Masterpiece' adalah karya seni, keterampilan, atau pengerjaan yang luar biasa." },
  { "en": "___ he practices, the more confident he becomes.", "id": "Jawaban: The more. Penjelasan: Ini adalah struktur komparatif ganda yang digunakan untuk menunjukkan hubungan langsung antara dua hal yang berubah." },
  { "en": "The sooner we leave, ___ we will arrive.", "id": "Jawaban: the earlier. Penjelasan: Strukturnya adalah 'The + comparative..., the + comparative...'. 'Earlier' adalah bentuk komparatif dari 'early'." },
  { "en": "The more complex the problem, ___ it is to solve.", "id": "Jawaban: the harder. Penjelasan: Struktur ini menunjukkan korelasi langsung antara kompleksitas dan kesulitan." },
  { "en": "The less you worry about it, ___ you will feel.", "id": "Jawaban: the better. Penjelasan: Struktur ini menunjukkan hubungan terbalik untuk kekhawatiran dan hubungan langsung untuk perasaan yang lebih baik." },
  { "en": "___ he was able to finish the project so quickly remains a mystery.", "id": "Jawaban: How. Penjelasan: Klausa kata benda 'How he was able to finish...' berfungsi sebagai subjek dari kata kerja 'remains'." },
  { "en": "I am interested in ___ the new policy will affect our department.", "id": "Jawaban: how. Penjelasan: Klausa kata benda 'how the new policy will affect our department' adalah objek dari preposisi 'in'." },
  { "en": "We need to decide ___ we should proceed.", "id": "Jawaban: whether. Penjelasan: Klausa kata benda 'whether we should proceed' adalah objek dari kata kerja 'decide'." },
  { "en": "___ caused the power outage is still under investigation.", "id": "Jawaban: What. Penjelasan: 'What' memperkenalkan klausa kata benda yang merupakan subjek dari kata kerja 'is'." },
  { "en": "He works ___ a consultant for a major tech firm.", "id": "Jawaban: as. Penjelasan: 'As' digunakan di sini sebagai preposisi yang berarti 'dalam kapasitas sebagai'." },
  { "en": "___ I was walking home, it began to snow.", "id": "Jawaban: As. Penjelasan: 'As' digunakan di sini sebagai konjungsi yang berarti 'saat' atau 'pada waktu yang sama'." },
  { "en": "___ you know, the deadline has been extended by one week.", "id": "Jawaban: As. Penjelasan: 'As' digunakan untuk memperkenalkan fakta yang sudah diketahui oleh pendengar." },
  { "en": "He is not as tall ___ his brother.", "id": "Jawaban: as. Penjelasan: Struktur 'as...as' digunakan untuk perbandingan kesetaraan." },
  { "en": "The manager demanded that the report ___ on her desk by 5 PM.", "id": "Jawaban: be. Penjelasan: Setelah kata kerja urgensi seperti 'demand', digunakan subjunctive mood (bentuk dasar kata kerja, 'be')." },
  { "en": "I suggest that he ___ to a specialist for a second opinion.", "id": "Jawaban: go. Penjelasan: Kata kerja 'suggest' memicu penggunaan subjunctive ('go')." },
  { "en": "The committee recommended that the proposal ___ rejected.", "id": "Jawaban: be. Penjelasan: 'Recommend' diikuti oleh klausa 'that' dengan kata kerja subjunctive." },
  { "en": "It is crucial that she ___ all the facts before making a decision.", "id": "Jawaban: have. Penjelasan: Frasa 'It is crucial that...' memerlukan subjunctive." },
  { "en": "By this time next year, the new bridge ___.", "id": "Jawaban: will have been built. Penjelasan: 'Future Perfect Passive' ('will have been + past participle') digunakan untuk tindakan yang akan selesai sebelum waktu tertentu di masa depan." },
  { "en": "The problem ___ for weeks, but no solution has been found yet.", "id": "Jawaban: has been discussed. Penjelasan: 'Present Perfect Passive' ('has been + past participle') digunakan untuk tindakan yang dimulai di masa lalu dan berlanjut hingga sekarang." },
  { "en": "While the new system ___, the old one will remain in operation.", "id": "Jawaban: is being installed. Penjelasan: 'Present Continuous Passive' ('is being + past participle') digunakan untuk tindakan yang sedang berlangsung sekarang." },
  { "en": "When I arrived, the documents ___ by the manager.", "id": "Jawaban: were being signed. Penjelasan: 'Past Continuous Passive' ('were being + past participle') digunakan untuk tindakan pasif yang sedang berlangsung pada waktu tertentu di masa lalu." },
  { "en": "His ___ comments were intended to wound and criticize.", "id": "Jawaban: acerbic. Penjelasan: 'Acerbic' adalah kata sifat yang berarti '(terutama komentar atau gaya bicara) yang tajam dan terus terang; pedas'." },
  { "en": "The dictator sought to ___ his power by controlling the media.", "id": "Jawaban: aggrandize. Penjelasan: 'Aggrandize' adalah kata kerja yang berarti 'meningkatkan kekuasaan, status, atau kekayaan'." },
  { "en": "The ___ organization donates millions to charity each year.", "id": "Jawaban: benevolent. Penjelasan: 'Benevolent' adalah kata sifat yang berarti 'bermaksud baik dan ramah'." },
  { "en": "The new regulations ___ the authority of local governments.", "id": "Jawaban: circumscribe. Penjelasan: 'Circumscribe' adalah kata kerja formal yang berarti 'membatasi (sesuatu) dalam batas-batas'." },
  { "en": "He launched into a long ___ against the company's policies.", "id": "Jawaban: diatribe. Penjelasan: 'Diatribe' adalah serangan verbal yang keras dan pahit terhadap seseorang atau sesuatu." },
  { "en": "The ___ tour guide talked nonstop for the entire three-hour trip.", "id": "Jawaban: garrulous. Penjelasan: 'Garrulous' adalah kata sifat yang berarti 'terlalu banyak bicara, terutama tentang hal-hal sepele'." },
  { "en": "The team's ___ defeat in the final match was a shock to their fans.", "id": "Jawaban: ignominious. Penjelasan: 'Ignominious' adalah kata sifat yang berarti 'pantas atau menyebabkan aib atau malu publik'." },
  { "en": "He was so ___ that he agreed to every request and flattered his boss constantly.", "id": "Jawaban: obsequious. Penjelasan: 'Obsequious' adalah kata sifat yang berarti 'patuh atau perhatian secara berlebihan atau membudak'." },
  { "en": "After the political turmoil, the country entered a ___ period of peace.", "id": "Jawaban: quiescent. Penjelasan: 'Quiescent' adalah kata sifat yang berarti 'dalam keadaan atau periode tidak aktif atau dorman'." },
  { "en": "He felt a deep-seated ___ towards his old rival.", "id": "Jawaban: rancor. Penjelasan: 'Rancor' adalah kata benda yang berarti 'kepahitan atau kebencian, terutama yang sudah berlangsung lama'." },
  { "en": "If I ___ more time, I would learn another language.", "id": "Jawaban: had. Penjelasan: Ini adalah kalimat conditional kedua standar untuk situasi sekarang yang hipotetis." },
  { "en": "The number of books in the library ___ grown significantly.", "id": "Jawaban: has. Penjelasan: Subjeknya adalah 'The number' (tunggal), yang memerlukan kata kerja tunggal 'has'." },
  { "en": "Scarcely had I finished my dinner ___ the phone began to ring.", "id": "Jawaban: when. Penjelasan: Pasangan yang benar untuk kata keterangan 'Scarcely' dalam struktur terbalik ini adalah 'when'." },
  { "en": "This is the town in ___ I was born.", "id": "Jawaban: which. Penjelasan: Ini adalah struktur klausa kata sifat formal di mana preposisi 'in' mendahului kata ganti relatif 'which'." },
  { "en": "It was ___ cold that we decided to stay indoors.", "id": "Jawaban: so. Penjelasan: 'So' digunakan sebelum kata sifat ('cold') dalam klausa hasil 'so...that'." },
  { "en": "You had better ___ a doctor about that cough.", "id": "Jawaban: see. Penjelasan: 'Had better' diikuti oleh bentuk dasar kata kerja ('see') tanpa 'to'." },
  { "en": "In spite ___ feeling unwell, she went to work.", "id": "Jawaban: of. Penjelasan: Frasa yang benar adalah 'in spite of', sebuah preposisi yang diikuti oleh kata benda atau gerund." },
  { "en": "I'm not tired, and ___ is my friend.", "id": "Jawaban: neither. Penjelasan: 'Neither + auxiliary + subject' digunakan untuk persetujuan negatif." },
  { "en": "The lecture was so ___ that I could hardly stay awake.", "id": "Jawaban: boring. Penjelasan: Kata sifat -ing ('boring') digunakan untuk mendeskripsikan kuliah (penyebab perasaan)." },
  { "en": "A great deal of research ___ been done on this topic.", "id": "Jawaban: has. Penjelasan: 'Research' adalah kata benda yang tidak dapat dihitung, jadi menggunakan kata kerja tunggal ('has')." },
  { "en": "That is the man ___ son won the scholarship.", "id": "Jawaban: whose. Penjelasan: 'Whose' adalah kata ganti relatif posesif." },
  { "en": "It's important for the project ___ on time.", "id": "Jawaban: to be finished. Penjelasan: Struktur ini memerlukan infinitif pasif ('to be finished')." },
  { "en": "His work is ___ praised by critics and academics.", "id": "Jawaban: widely. Penjelasan: 'Widely' adalah kata keterangan yang berarti 'oleh banyak orang' atau 'secara luas'." },
  { "en": "The new policy is a ___ departure from the past.", "id": "Jawaban: radical. Penjelasan: 'Radical' berarti 'menjangkau jauh atau menyeluruh'." },
  { "en": "The two countries have a ___ of shared history.", "id": "Jawaban: legacy. Penjelasan: 'Legacy' adalah sesuatu yang ditinggalkan atau diwariskan dari leluhur atau pendahulu." },
  { "en": "The results of the study were ___ and could not be trusted.", "id": "Jawaban: invalid. Penjelasan: 'Invalid' berarti 'tidak sah atau tidak dapat diterima secara resmi' atau '(argumen) tidak logis atau benar'." },
  { "en": "The company has a ___ approach to employee training.", "id": "Jawaban: systematic. Penjelasan: 'Systematic' berarti 'dilakukan atau bertindak sesuai dengan rencana atau sistem yang tetap'." },
  { "en": "His ___ to the cause was never in doubt.", "id": "Jawaban: loyalty. Penjelasan: 'Loyalty' adalah kata benda yang berarti 'kualitas kesetiaan'." },
  { "en": "The new evidence ___ the defendant's story.", "id": "Jawaban: contradicts. Penjelasan: 'Contradicts' berarti 'bertentangan dengan'." },
  { "en": "The two systems are ___ in their design.", "id": "Jawaban: identical. Penjelasan: 'Identical' berarti 'serupa dalam setiap detail; persis sama'." },
  { "en": "The project is a ___ of the team's creative talents.", "id": "Jawaban: showcase. Penjelasan: 'Showcase' adalah 'pengaturan di mana sesuatu ditampilkan untuk keuntungan terbaik'." },
  { "en": "The new regulations are ___ to prevent accidents.", "id": "Jawaban: designed. Penjelasan: 'Designed to' berarti 'dimaksudkan untuk tujuan tertentu'." },
  { "en": "The two countries have a ___ relationship.", "id": "Jawaban: friendly. Penjelasan: 'Friendly' adalah kata sifat yang dibutuhkan di sini." },
  { "en": "The ___ of the problem is its scale.", "id": "Jawaban: magnitude. Penjelasan: 'Magnitude' berarti 'ukuran atau jangkauan yang besar dari sesuatu'." },
  { "en": "The new evidence ___ him as a suspect.", "id": "Jawaban: implicates. Penjelasan: 'Implicates' berarti 'menunjukkan (seseorang) terlibat dalam kejahatan'." },
  { "en": "The two systems are ___ but not the same.", "id": "Jawaban: similar. Penjelasan: 'Similar' berarti 'menyerupai tanpa identik'." },
  { "en": "The project is a ___ to complete in such a short time.", "id": "Jawaban: challenge. Penjelasan: 'Challenge' adalah 'tugas atau situasi yang menguji kemampuan seseorang'." },
  { "en": "The new regulations are ___ for all businesses.", "id": "Jawaban: compulsory. Penjelasan: 'Compulsory' berarti 'diwajibkan oleh hukum atau aturan; wajib'." },
  { "en": "His ___ of the topic is impressive.", "id": "Jawaban: knowledge. Penjelasan: 'Knowledge' adalah bentuk kata benda." },
  { "en": "The new system is ___ to be implemented next year.", "id": "Jawaban: scheduled. Penjelasan: 'Scheduled to be' berarti 'direncanakan untuk waktu tertentu'." },
  { "en": "The two countries have a ___ of cultural exchange.", "id": "Jawaban: tradition. Penjelasan: 'Tradition' adalah 'penyampaian adat atau kepercayaan dari generasi ke generasi'." },
  { "en": "The project is a ___ undertaking.", "id": "Jawaban: massive. Penjelasan: 'Massive' berarti 'besar dan berat atau padat'." },
  { "en": "The new regulations are ___ to understand.", "id": "Jawaban: difficult. Penjelasan: 'Difficult' adalah kata sifat yang dibutuhkan." },
  { "en": "The two companies have a ___ to share profits.", "id": "Jawaban: deal. Penjelasan: 'Deal' adalah perjanjian." },
  { "en": "His ___ is well-known in the industry.", "id": "Jawaban: expertise. Penjelasan: 'Expertise' adalah 'keterampilan atau pengetahuan ahli dalam bidang tertentu'." },
  { "en": "The new system is ___ to be a major improvement.", "id": "Jawaban: expected. Penjelasan: 'Expected to be' berarti 'diyakini akan terjadi'." },
  { "en": "The two countries have a ___ relationship.", "id": "Jawaban: turbulent. Penjelasan: 'Turbulent' berarti 'ditandai oleh konflik, kekacauan, atau kebingungan; tidak terkendali atau tenang'." },
  { "en": "The project is a ___ for innovation in the field.", "id": "Jawaban: catalyst. Penjelasan: 'Catalyst' adalah 'orang atau benda yang memicu suatu peristiwa'." },
  { "en": "The new regulations are ___ and will be enforced strictly.", "id": "Jawaban: clear. Penjelasan: 'Clear' adalah kata sifat." },
  { "en": "The two companies have ___ an agreement.", "id": "Jawaban: reached. Penjelasan: 'To reach an agreement' adalah kolokasi standar." },
  { "en": "His ___ to the project was crucial for its success.", "id": "Jawaban: involvement. Penjelasan: 'Involvement' adalah bentuk kata benda." },
  { "en": "The new system is ___ to be more efficient.", "id": "Jawaban: guaranteed. Penjelasan: 'Guaranteed to be' berarti 'pasti akan menjadi'." },
  { "en": "The two countries have a ___ on most issues.", "id": "Jawaban: consensus. Penjelasan: 'Consensus' berarti 'kesepakatan umum'." },
  { "en": "The project is a ___ of hard work and dedication.", "id": "Jawaban: product. Penjelasan: 'Product' di sini berarti 'hasil'." },
  { "en": "The new regulations are ___ to many small businesses.", "id": "Jawaban: harmful. Penjelasan: 'Harmful' adalah kata sifat." },
  { "en": "The two companies have a ___ to merge.", "id": "Jawaban: proposal. Penjelasan: 'Proposal' adalah rencana atau saran." },
  { "en": "His ___ is evident in his work.", "id": "Jawaban: talent. Penjelasan: 'Talent' adalah keterampilan alami." },
  { "en": "The new system is ___ to be revolutionary.", "id": "Jawaban: considered. Penjelasan: 'Considered to be' berarti 'dianggap sebagai'." },
  { "en": "The two countries have a ___ of mutual defense.", "id": "Jawaban: treaty. Penjelasan: 'Treaty' adalah perjanjian yang disepakati dan diratifikasi secara resmi antara negara." },
  { "en": "The project is a ___ of teamwork.", "id": "Jawaban: triumph. Penjelasan: 'Triumph' adalah kemenangan atau pencapaian besar." },
  { "en": "The new regulations are ___ to interpret.", "id": "Jawaban: hard. Penjelasan: 'Hard' adalah kata sifat." },
  { "en": "The two companies have a ___ of interest.", "id": "Jawaban: conflict. Penjelasan: 'Conflict of interest' adalah situasi di mana seseorang berada dalam posisi untuk mendapatkan keuntungan pribadi dari tindakan atau keputusan yang dibuat dalam kapasitas resminya." },
  { "en": "His ___ in the matter is clear.", "id": "Jawaban: position. Penjelasan: 'Position' di sini berarti 'sudut pandang atau sikap seseorang terhadap sesuatu'." },
  { "en": "The new system is ___ to be in place by January.", "id": "Jawaban: due. Penjelasan: 'Due to be' berarti 'diharapkan pada atau direncanakan untuk waktu tertentu'." },
  { "en": "The two countries have a ___ to resolve the dispute peacefully.", "id": "Jawaban: desire. Penjelasan: 'Desire' adalah perasaan kuat ingin memiliki sesuatu atau berharap sesuatu terjadi." },
  { "en": "The project is a ___ of our company's values.", "id": "Jawaban: reflection. Penjelasan: 'Reflection' adalah 'gambaran; representasi' atau 'indikasi sifat sesuatu'." },
  { "en": "The new regulations are ___ and need to be revised.", "id": "Jawaban: flawed. Penjelasan: 'Flawed' berarti 'memiliki kelemahan atau ketidaksempurnaan'." },
  { "en": "The two companies have a ___ to dominate the market.", "id": "Jawaban: strategy. Penjelasan: 'Strategy' adalah rencana aksi yang dirancang untuk mencapai tujuan jangka panjang atau keseluruhan." },
  { "en": "His ___ is highly respected.", "id": "Jawaban: opinion. Penjelasan: 'Opinion' adalah pandangan atau penilaian yang terbentuk tentang sesuatu, belum tentu berdasarkan fakta atau pengetahuan." },
  { "en": "The new system is ___ to be operational next month.", "id": "Jawaban: slated. Penjelasan: 'Slated to be' adalah idiom yang berarti 'dijadwalkan atau direncanakan untuk menjadi'." },
  { "en": "The two countries have a ___ to improve relations.", "id": "Jawaban: commitment. Penjelasan: 'Commitment' adalah 'keadaan atau kualitas berdedikasi pada suatu tujuan, aktivitas, dll.'." },
  { "en": "Seldom ___ a more beautiful sunset.", "id": "Jawaban: have I seen. Penjelasan: Ketika sebuah kalimat dimulai dengan kata keterangan negatif seperti 'Seldom', subjek dan kata kerja bantu dibalik." },
  { "en": "Only after the storm had passed ___ the true extent of the damage.", "id": "Jawaban: did we see. Penjelasan: Ketika sebuah kalimat dimulai dengan 'Only after...', klausa utamanya dibalik." },
  { "en": "Such ___ the power of his speech that the entire audience was moved to tears.", "id": "Jawaban: was. Penjelasan: Dengan 'Such' di awal kalimat, kata kerja sering mendahului subjeknya ('the power')." },
  { "en": "Not a single word ___ she say during the entire meeting.", "id": "Jawaban: did. Penjelasan: Frasa negatif 'Not a single word' di awal kalimat memerlukan inversi ('did she say')." },
  { "en": "Had it not been for the quick response of the firefighters, the building ___ have been destroyed.", "id": "Jawaban: would. Penjelasan: Ini adalah 'inverted third conditional' ('Had it not been for...' = 'If it had not been for...')." },
  { "en": "The ideal candidate should be experienced, reliable, and ___ to work in a team.", "id": "Jawaban: able. Penjelasan: Daftar tersebut terdiri dari kata sifat. 'able' adalah kata sifat yang menjaga struktur paralel." },
  { "en": "The course teaches not only how to write code but also ___ it effectively.", "id": "Jawaban: how to debug. Penjelasan: Struktur 'how to write' harus paralel dengan 'how to debug'." },
  { "en": "She enjoys both reading novels and ___ short stories.", "id": "Jawaban: writing. Penjelasan: Konjungsi korelatif 'both...and' memerlukan bentuk paralel. 'reading' dan 'writing' adalah gerund yang paralel." },
  { "en": "It is often easier to ask for forgiveness ___ for permission.", "id": "Jawaban: than to ask. Penjelasan: Struktur komparatif memerlukan bentuk paralel: 'to ask...than to ask...'." },
  { "en": "The collection of rare stamps ___ worth a considerable amount of money.", "id": "Jawaban: is. Penjelasan: Subjeknya adalah 'The collection' (tunggal), bukan 'stamps'. Kata kerja harus tunggal." },
  { "en": "Neither of the proposed solutions ___ to be effective.", "id": "Jawaban: seems. Penjelasan: 'Neither' adalah kata ganti tunggal dan menggunakan kata kerja tunggal ('seems')." },
  { "en": "The news from the front lines ___ not encouraging.", "id": "Jawaban: is. Penjelasan: 'News' adalah kata benda yang tidak dapat dihitung dan selalu menggunakan kata kerja tunggal." },
  { "en": "Each of the team members ___ given a specific task.", "id": "Jawaban: was. Penjelasan: Kata ganti 'Each' adalah tunggal dan memerlukan kata kerja tunggal ('was')." },
  { "en": "The phenomena observed during the experiment ___ still not fully understood.", "id": "Jawaban: are. Penjelasan: 'Phenomena' adalah bentuk jamak dari 'phenomenon' dan memerlukan kata kerja jamak ('are')." },
  { "en": "He is committed ___ the highest standards of quality.", "id": "Jawaban: to maintaining. Penjelasan: Frasa 'committed to' diikuti oleh 'gerund' ('maintaining')." },
  { "en": "I regret ___ you that your application has been unsuccessful.", "id": "Jawaban: to inform. Penjelasan: 'Regret to inform' adalah frasa formal yang digunakan untuk memberikan kabar buruk." },
  { "en": "It is no use ___ over spilled milk.", "id": "Jawaban: crying. Penjelasan: Idiom 'it's no use' diikuti oleh 'gerund'." },
  { "en": "The company has decided ___ its headquarters to a new city.", "id": "Jawaban: to move. Penjelasan: Kata kerja 'decide' diikuti oleh infinitif." },
  { "en": "She is capable ___ complex tasks under pressure.", "id": "Jawaban: of handling. Penjelasan: Frasa 'capable of' diikuti oleh 'gerund'." },
  { "en": "Every employee is responsible for keeping ___ own workspace clean.", "id": "Jawaban: their. Penjelasan: 'Their' adalah kata ganti tunggal netral-gender yang diterima secara umum untuk merujuk kembali ke 'Every employee'." },
  { "en": "The board of directors will announce ___ decision tomorrow.", "id": "Jawaban: its. Penjelasan: 'The board' adalah satu kesatuan, jadi kata ganti posesif tunggal 'its' digunakan." },
  { "en": "The books on the shelf, ___ are covered in dust, have not been touched for years.", "id": "Jawaban: which. Penjelasan: 'Which' memperkenalkan klausa non-defining yang memberikan informasi tambahan tentang 'the books'." },
  { "en": "Can you please give this report to ___?", "id": "Jawaban: him. Penjelasan: 'him' adalah objek dari preposisi 'to'." },
  { "en": "It was ___ who designed the award-winning building.", "id": "Jawaban: they. Penjelasan: Dalam bahasa Inggris formal, setelah 'it was', digunakan kata ganti subjek ('they')." },
  { "en": "As the storm began to ___, we were able to see the horizon again.", "id": "Jawaban: abate. Penjelasan: 'Abate' adalah kata kerja yang berarti '(sesuatu yang dianggap negatif) menjadi kurang intens atau meluas'." },
  { "en": "He dreamed of a quiet, ___ life in the countryside, far from the noise of the city.", "id": "Jawaban: bucolic. Penjelasan: 'Bucolic' adalah kata sifat yang berkaitan dengan aspek menyenangkan dari pedesaan dan kehidupan desa." },
  { "en": "He used legal ___ to avoid paying his taxes for years.", "id": "Jawaban: chicanery. Penjelasan: 'Chicanery' adalah kata benda yang berarti 'penggunaan tipu daya untuk mencapai tujuan politik, keuangan, atau hukum'." },
  { "en": "Despite his expertise, he spoke with great ___ about his accomplishments.", "id": "Jawaban: diffidence. Penjelasan: 'Diffidence' adalah kata benda yang berarti 'kesopanan atau rasa malu yang timbul dari kurangnya rasa percaya diri'." },
  { "en": "The crowd was ___ after the home team scored the winning goal.", "id": "Jawaban: ebullient. Penjelasan: 'Ebullient' adalah kata sifat yang berarti 'ceria dan penuh energi'." },
  { "en": "She was a ___ supporter of the cause, donating both time and money.", "id": "Jawaban: fervid. Penjelasan: 'Fervid' adalah kata sifat yang berarti 'sangat antusias atau bersemangat, terutama secara berlebihan'." },
  { "en": "It is impossible to ___ the fact that the company has been losing money.", "id": "Jawaban: gainsay. Penjelasan: 'Gainsay' adalah kata kerja formal yang berarti 'menyangkal atau membantah (fakta atau pernyataan)'." },
  { "en": "The company has achieved cultural ___ with its products being used worldwide.", "id": "Jawaban: hegemony. Penjelasan: 'Hegemony' adalah kata benda yang berarti 'kepemimpinan atau dominasi, terutama oleh satu negara atau kelompok sosial atas yang lain'." },
  { "en": "His plans were still ___ and not yet ready to be presented.", "id": "Jawaban: inchoate. Penjelasan: 'Inchoate' adalah kata sifat yang berarti 'baru dimulai sehingga belum sepenuhnya terbentuk atau berkembang; belum sempurna'." },
  { "en": "His ___ reply of 'maybe' was not helpful.", "id": "Jawaban: laconic. Penjelasan: 'Laconic' adalah kata sifat yang berarti '(seseorang, pidato, atau gaya penulisan) yang menggunakan sangat sedikit kata'." },
  { "en": "___ of the budget cuts, several programs will be eliminated.", "id": "Jawaban: As a result. Penjelasan: 'As a result of' adalah frasa transisi yang menunjukkan hubungan sebab-akibat." },
  { "en": "The more options you have, ___ it can be to make a decision.", "id": "Jawaban: the harder. Penjelasan: Ini adalah struktur komparatif ganda standar." },
  { "en": "All luggage must ___ before boarding the plane.", "id": "Jawaban: be checked. Penjelasan: Ini adalah konstruksi pasif dengan modal ('must be + past participle')." },
  { "en": "Had I seen the warning sign, I ___ that road.", "id": "Jawaban: would not have taken. Penjelasan: Ini adalah 'inverted third conditional'." },
  { "en": "The committee, as well as the manager, ___ the proposal.", "id": "Jawaban: supports. Penjelasan: Subjeknya adalah 'The committee' (tunggal). Frasa 'as well as the manager' bersifat parenthetical dan tidak mengubah jumlah kata kerja." },
  { "en": "He not only finished the race but also ___ a new record.", "id": "Jawaban: set. Penjelasan: 'Not only...but also' memerlukan struktur paralel. 'finished' dan 'set' keduanya adalah kata kerja simple past." },
  { "en": "It is recommended that she ___ a break from her work.", "id": "Jawaban: take. Penjelasan: Kata kerja 'recommend' memicu subjunctive mood ('take')." },
  { "en": "He is interested ___ how the new technology works.", "id": "Jawaban: in. Penjelasan: 'Interested in' adalah kolokasi yang benar. Diikuti oleh klausa kata benda." },
  { "en": "___ the project was difficult, it was also very rewarding.", "id": "Jawaban: While. Penjelasan: 'While' digunakan di sini sebagai konjungsi yang berarti 'meskipun', menunjukkan kontras." },
  { "en": "The paintings, ___ were created in the 17th century, are priceless.", "id": "Jawaban: which. Penjelasan: 'Which' memperkenalkan klausa non-defining yang memberikan informasi tambahan tentang 'The paintings'." },
  { "en": "A number of the participants ___ from out of the country.", "id": "Jawaban: come. Penjelasan: 'A number of' diperlakukan sebagai jamak dan memerlukan kata kerja jamak ('come')." },
  { "en": "It was so late ___ we decided to take a taxi home.", "id": "Jawaban: that. Penjelasan: Ini melengkapi struktur sebab-akibat 'so...that'." },
  { "en": "I wish I ___ how to play the piano.", "id": "Jawaban: knew. Penjelasan: Untuk keinginan masa kini tentang keadaan yang tidak benar, digunakan simple past tense ('knew')." },
  { "en": "The quality of the students' work ___ improved this semester.", "id": "Jawaban: has. Penjelasan: Subjeknya adalah 'quality' (tunggal), bukan 'students'." },
  { "en": "She went to the store ___ some milk.", "id": "Jawaban: to buy. Penjelasan: Infinitif tujuan ('to buy') digunakan untuk menjelaskan mengapa dia pergi ke toko." },
  { "en": "The ___ of the matter is that we cannot afford it.", "id": "Jawaban: truth. Penjelasan: 'The truth of the matter' adalah frasa umum." },
  { "en": "The new policy is a ___ for the company.", "id": "Jawaban: milestone. Penjelasan: 'Milestone' adalah tahap atau peristiwa penting dalam perkembangan sesuatu." },
  { "en": "His work is ___ and often cited by other researchers.", "id": "Jawaban: influential. Penjelasan: 'Influential' adalah kata sifat yang berarti 'memiliki pengaruh besar pada seseorang atau sesuatu'." },
  { "en": "The two countries have a ___ relationship.", "id": "Jawaban: peaceful. Penjelasan: 'Peaceful' adalah kata sifat yang dibutuhkan di sini." },
  { "en": "The project is a ___ of the team's dedication.", "id": "Jawaban: symbol. Penjelasan: 'Symbol' adalah 'sesuatu yang mewakili atau melambangkan sesuatu yang lain'." },
  { "en": "The new regulations are ___ to be implemented next year.", "id": "Jawaban: set. Penjelasan: 'Set to be' digunakan untuk berarti 'siap atau kemungkinan akan'." },
  { "en": "The two companies have a ___ to develop new technologies.", "id": "Jawaban: partnership. Penjelasan: 'Partnership' adalah asosiasi antara dua pihak atau lebih." },
  { "en": "His ___ to the problem was very innovative.", "id": "Jawaban: approach. Penjelasan: 'Approach' adalah 'cara menangani suatu situasi atau masalah'." },
  { "en": "The new system is ___ to be a game-changer.", "id": "Jawaban: poised. Penjelasan: 'Poised to be' berarti 'siap dan siap untuk melakukan sesuatu'." },
  { "en": "The two countries have a ___ history of cooperation.", "id": "Jawaban: long. Penjelasan: 'Long' adalah kata sifat yang memodifikasi 'history'." },
  { "en": "The project is a ___ to the power of collaboration.", "id": "Jawaban: testament. Penjelasan: 'A testament to' berarti 'berfungsi sebagai bukti nyata dari'." },
  { "en": "The new regulations are ___ and will be difficult to enforce.", "id": "Jawaban: controversial. Penjelasan: 'Controversial' berarti 'menimbulkan atau cenderung menimbulkan perselisihan publik'." },
  { "en": "His ___ is respected throughout the industry.", "id": "Jawaban: judgment. Penjelasan: 'Judgment' adalah 'kemampuan untuk membuat keputusan yang dipertimbangkan atau sampai pada kesimpulan yang masuk akal'." },
  { "en": "The new system is ___ to be more secure than the old one.", "id": "Jawaban: designed. Penjelasan: 'Designed to be' berarti 'dimaksudkan untuk menjadi'." },
  { "en": "The two countries have a ___ trade relationship.", "id": "Jawaban: robust. Penjelasan: 'Robust' berarti 'kuat dan sehat; kuat'." },
  { "en": "The project is a ___ of the company's vision.", "id": "Jawaban: cornerstone. Penjelasan: 'Cornerstone' adalah 'kualitas atau fitur penting yang menjadi sandaran atau dasar sesuatu'." },
  { "en": "The new regulations are ___ and need to be simplified.", "id": "Jawaban: cumbersome. Penjelasan: 'Cumbersome' berarti 'rumit dan karena itu lambat atau tidak efisien'." },
  { "en": "His ___ of the events was very clear.", "id": "Jawaban: recollection. Penjelasan: 'Recollection' adalah ingatan atau tindakan mengingat sesuatu." },
  { "en": "The new system is ___ to be a major step forward.", "id": "Jawaban: seen. Penjelasan: 'Seen to be' berarti 'dianggap sebagai'." },
  { "en": "The two countries have a ___ of mutual respect.", "id": "Jawaban: foundation. Penjelasan: 'Foundation' adalah 'dasar atau prinsip yang mendasari'." },
  { "en": "The project is a ___ of our commitment to excellence.", "id": "Jawaban: manifestation. Penjelasan: 'Manifestation' adalah tindakan atau objek yang dengan jelas menunjukkan sesuatu." },
  { "en": "The new regulations are ___ to change.", "id": "Jawaban: subject. Penjelasan: 'Subject to change' berarti 'kemungkinan akan diubah'." },
  { "en": "The two companies have a ___ to share research.", "id": "Jawaban: contract. Penjelasan: 'Contract' adalah perjanjian tertulis atau lisan." },
  { "en": "His ___ is beyond question.", "id": "Jawaban: integrity. Penjelasan: 'Integrity' adalah 'kualitas jujur dan memiliki prinsip moral yang kuat'." },
  { "en": "The new system is ___ to be more efficient.", "id": "Jawaban: proven. Penjelasan: 'Proven to be' berarti 'telah dibuktikan dengan bukti'." },
  { "en": "The two countries have a ___ that needs to be resolved.", "id": "Jawaban: dispute. Penjelasan: 'Dispute' adalah ketidaksepakatan." },
  { "en": "The project is a ___ of innovative design.", "id": "Jawaban: masterpiece. Penjelasan: 'Masterpiece' adalah karya dengan keterampilan luar biasa." },
  { "en": "The new regulations are ___ and open to interpretation.", "id": "Jawaban: vague. Penjelasan: 'Vague' berarti 'tidak pasti atau tidak jelas maknanya'." },
  { "en": "The two companies have a ___ to not hire each other's employees.", "id": "Jawaban: tacit agreement. Penjelasan: 'Tacit agreement' adalah perjanjian yang dipahami tanpa dinyatakan." },
  { "en": "His ___ on the matter is well-documented.", "id": "Jawaban: stance. Penjelasan: 'Stance' adalah sikap atau posisi terhadap suatu masalah." },
  { "en": "The new system is ___ to fail without proper funding.", "id": "Jawaban: likely. Penjelasan: 'Likely to' berarti 'kemungkinan akan'." },
  { "en": "The two countries have a ___ to protect the environment.", "id": "Jawaban: joint commitment. Penjelasan: 'Joint commitment' adalah kewajiban bersama." },
  { "en": "The project is a ___ of our company's capabilities.", "id": "Jawaban: demonstration. Penjelasan: 'Demonstration' adalah pameran praktis dan penjelasan tentang cara kerja sesuatu." },
  { "en": "The new regulations are ___ and will be implemented soon.", "id": "Jawaban: forthcoming. Penjelasan: 'Forthcoming' berarti 'segera terjadi atau muncul'." },
  { "en": "The two companies have a ___ to innovate.", "id": "Jawaban: mandate. Penjelasan: 'Mandate' adalah 'perintah atau komisi resmi untuk melakukan sesuatu'." },
  { "en": "His ___ is highly valued by the team.", "id": "Jawaban: input. Penjelasan: 'Input' di sini berarti kontribusi ide." },
  { "en": "The new system is ___ to be a significant investment.", "id": "Jawaban: certain. Penjelasan: 'Certain to be' berarti 'pasti akan menjadi'." },
  { "en": "The two countries have a ___ on border control.", "id": "Jawaban: protocol. Penjelasan: 'Protocol' adalah 'prosedur resmi atau sistem aturan yang mengatur urusan negara atau acara diplomatik'." },
  { "en": "The project is a ___ of our long-term strategy.", "id": "Jawaban: key component. Penjelasan: 'Key component' adalah bagian penting." },
  { "en": "The new regulations are ___ to be challenged in court.", "id": "Jawaban: likely. Penjelasan: 'Likely to be' berarti 'kemungkinan akan'." },
  { "en": "The two companies have a ___ to explore a merger.", "id": "Jawaban: tentative plan. Penjelasan: 'Tentative plan' adalah rencana yang belum pasti." },
  { "en": "His ___ is a valuable asset to the company.", "id": "Jawaban: experience. Penjelasan: 'Experience' adalah bentuk kata benda." },
  { "en": "The new system is ___ to be launched in the spring.", "id": "Jawaban: on track. Penjelasan: 'On track to be' adalah idiom yang berarti 'berjalan sesuai rencana'." },
  { "en": "The two countries have a ___ to promote cultural exchange.", "id": "Jawaban: common goal. Penjelasan: 'Common goal' adalah tujuan bersama." },
  { "en": "The project is a ___ of our company's innovation.", "id": "Jawaban: shining example. Penjelasan: 'Shining example' adalah contoh yang sangat baik." },
  { "en": "The new regulations are ___ and need further clarification.", "id": "Jawaban: unclear. Penjelasan: 'Unclear' adalah kata sifat yang dibutuhkan." },
  { "en": "The two companies have a ___ to keep the negotiations secret.", "id": "Jawaban: vested interest. Penjelasan: 'Vested interest' adalah kepentingan pribadi dalam suatu masalah." },
  { "en": "His ___ is evident in the quality of his work.", "id": "Jawaban: craftsmanship. Penjelasan: 'Craftsmanship' adalah 'keterampilan dalam kerajinan tertentu'." }

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
