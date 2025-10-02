# kuizpapansusiutama
<html lang="ms">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Kuiz Papan Suis Utama</title>
    <!-- Memuatkan Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Gaya tersuai untuk font dan latar belakang */
        body {
            font-family: 'Inter', sans-serif;
            background-color: #f0f4f8; /* Biru muda pucat */
        }
        .card {
            background-color: #ffffff;
            box-shadow: 0 10px 15px rgba(0, 0, 0, 0.1);
        }
        .question-item:nth-child(odd) {
            background-color: #f7f7f7;
        }
        /* Menggunakan warna tema elektrik */
        .primary-color {
            background-color: #007bff; /* Biru terang */
        }
        .primary-text {
            color: #007bff;
        }
    </style>
</head>
<body class="p-4 sm:p-8">

    <div id="app-container" class="max-w-4xl mx-auto card rounded-xl p-6 sm:p-10 transition-all duration-500">
        <!-- HEADER KUIZ -->
        <header class="text-center mb-8 border-b-2 border-blue-500 pb-4">
            <h1 class="text-3xl sm:text-4xl font-extrabold primary-text">Kuiz Papan Suis Utama (PSU)</h1>
            <p class="text-lg text-gray-600 mt-2">Ujian Kefahaman Elektrik Voltan Rendah</p>
        </header>

        <!-- FASA 1: BORANG PENDAFTARAN -->
        <div id="registration-screen">
            <h2 class="text-2xl font-semibold mb-6 text-gray-800">Daftar Masuk Pelajar</h2>
            <form id="registration-form" class="space-y-6">
                <div>
                    <label for="student-name" class="block text-sm font-medium text-gray-700 mb-1">Nama Pelajar:</label>
                    <input type="text" id="student-name" name="student-name" required class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500" placeholder="Contoh: Ali bin Abu">
                </div>
                <div>
                    <label for="ic-number" class="block text-sm font-medium text-gray-700 mb-1">Nombor Kad Pengenalan (IC):</label>
                    <input type="text" id="ic-number" name="ic-number" required pattern="\d{6}-?\d{2}-?\d{4}" title="Format IC: YYMMDD-XX-XXXX" class="w-full px-4 py-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-blue-500 focus:border-blue-500" placeholder="Contoh: 950101-10-5000">
                </div>
                <button type="submit" class="w-full primary-color text-white font-bold py-3 rounded-lg hover:bg-blue-700 transition duration-300 transform hover:scale-[1.01] shadow-md">
                    Mulakan Kuiz (30 Minit)
                </button>
            </form>
        </div>

        <!-- FASA 2: KUIZ (Akan dipaparkan oleh JS) -->
        <div id="quiz-screen" class="hidden">
            <!-- PAPARAN MASA -->
            <div id="timer-display" class="text-xl sm:text-2xl font-bold mb-6 p-3 bg-blue-100 text-blue-700 rounded-lg text-center transition-colors duration-500">
                Masa Tinggal: <span id="countdown">30:00</span>
            </div>
            
            <form id="quiz-form">
                <div id="questions-container" class="space-y-6 mb-8">
                    <!-- Soalan (telah di-shuffle) akan dimuatkan di sini oleh JavaScript -->
                </div>
                <button type="submit" id="submit-button" class="w-full primary-color text-white font-bold py-3 rounded-lg hover:bg-blue-700 transition duration-300 transform hover:scale-[1.01] shadow-lg">
                    Hantar Jawapan dan Semak Markah
                </button>
            </form>
        </div>

        <!-- FASA 3: KEPUTUSAN -->
        <div id="result-screen" class="hidden text-center">
            <h2 class="text-3xl font-bold mb-4 primary-text">Keputusan Ujian</h2>
            <div id="user-info" class="mb-4 p-4 bg-blue-100 rounded-lg text-left inline-block">
                <p class="text-md text-gray-800"><span class="font-bold">Nama:</span> <span id="display-name"></span></p>
                <p class="text-md text-gray-800"><span class="font-bold">IC:</span> <span id="display-ic"></span></p>
            </div>
            <div class="space-y-4">
                <p class="text-xl font-semibold text-gray-700">Jumlah Soalan: <span class="font-bold" id="total-questions"></span></p>
                <p class="text-2xl font-bold text-green-600">Jawapan Betul: <span id="correct-answers"></span></p>
                <p class="text-2xl font-bold text-red-600">Jawapan Salah: <span id="incorrect-answers"></span></p>
                <p class="text-3xl font-extrabold mt-6">Markah Anda: <span id="final-score" class="primary-text"></span> / 30</p>

                <div id="result-message" class="p-5 rounded-lg font-bold text-xl mt-6">
                    <!-- Mesej Lulus/Gagal di sini -->
                </div>
                
                <button onclick="window.location.reload()" class="mt-8 px-6 py-3 border-2 border-blue-500 text-blue-500 font-bold rounded-lg hover:bg-blue-500 hover:text-white transition duration-300">
                    Cuba Semula Kuiz
                </button>
            </div>
        </div>
    </div>

    <script>
        // Data Soalan dan Jawapan
        const questions = [
            { q: "Apakah fungsi utama Papan Suis Utama (PSU) dalam sistem pemasangan elektrik?", a: "B", options: { A: "Menjana bekalan elektrik.", B: "Menerima, mengawal, dan mengagihkan bekalan elektrik ke seluruh pemasangan.", C: "Mengubah voltan dari voltan rendah ke voltan tinggi.", D: "Hanya menyediakan perlindungan daripada arus lebihan." } },
            { q: "Gambarajah Satu Garisan (Single Line Diagram) yang terdapat dalam PSU bertujuan untuk:", a: "C", options: { A: "Menunjukkan litar kawalan sepenuhnya.", B: "Menggambarkan pendawaian sebenar dengan setiap wayar.", C: "Merupakan lukisan ringkas yang menggunakan satu garisan untuk mewakili litar berbilang fasa.", D: "Menunjukkan lokasi fizikal sebenar setiap komponen dalam bangunan." } },
            { q: "Dalam Rajah Satu Garisan, sumber kuasa utama (Incoming 1) datang dari LV Feeder Pillar. LV adalah singkatan bagi:", a: "A", options: { A: "Low Voltage (Voltan Rendah).", B: "Large Volume.", C: "Local Voltage.", D: "Load Variation." } },
            { q: "Sumber bekalan Incoming 2 dalam sistem ini adalah dari:", a: "C", options: { A: "LV Sub Switchboard.", B: "LV Main Switchboard.", C: "Generator (Janakuasa).", D: "LV Feeder Pillar." } },
            { q: "Palang Bas TPN (Triple Pole and Neutral Busbar) berfungsi sebagai:", a: "B", options: { A: "Pengukur voltan sistem.", B: "Lebuh raya utama untuk pengagihan kuasa ke litar-litar keluar.", C: "Peranti perlindungan utama.", D: "Kapasitor bank untuk memperbaiki faktor kuasa." } },
            { q: "Apakah fungsi utama Pemutus Litar Udara (ACB) dalam PSU?", a: "C", options: { A: "Mengukur tenaga yang digunakan.", B: "Menukar arus ulang-alik kepada arus terus.", C: "Bertindak sebagai suis pintar yang melindungi sistem dengan memutus litar secara automatik semasa kerosakan.", D: "Mengawal kelajuan motor elektrik." } },
            { q: "Apakah yang diwakili oleh simbol seperti kod 51 dan 51N (geganti perlindungan) yang digelar 'otak sistem' dalam keselamatan PSU?", a: "B", options: { A: "Pengatur Voltan.", B: "Geganti perlindungan (Protection Relay) yang mengesan keadaan tidak normal.", C: "Alatubah Arus (Current Transformer, CT).", D: "Suis Tukar Alih Automatik (ATS)." } },
            { q: "Langkah terakhir dalam rantaian perlindungan geganti adalah Pelantik (Tripping). Apakah yang dilakukan oleh Pelantik?", a: "B", options: { A: "Mengukur arus tinggi.", B: "Membuka Pemutus Litar (ACB) untuk memutus aliran kuasa.", C: "Menghantar isyarat kecemasan kepada geganti.", D: "Menetapkan semula faktor kuasa." } },
            { q: "Sistem Pembumian (Earthing) direka untuk menyediakan:", a: "C", options: { A: "Perlindungan daripada beban lampau (overload).", B: "Laluan selamat untuk arus voltan tinggi.", C: "Laluan selamat untuk arus rosak ke bumi bagi melindungi manusia daripada renjatan elektrik.", D: "Sambungan automatik antara dua sumber bekalan." } },
            { q: "Perlindungan dalam sistem PSU direka untuk melindungi tiga lapisan utama. Lapisan yang PALING PENTING adalah:", a: "C", options: { A: "Peralatan elektrik dari kerosakan.", B: "Bangunan dari kebakaran.", C: "Manusia dari bahaya renjatan elektrik.", D: "Kabel daripada terlalu panas." } },
            { q: "Ammeter (A) pada PSU digunakan untuk mengukur:", a: "C", options: { A: "Voltan.", B: "Tenaga.", C: "Arus (aliran).", D: "Faktor kuasa." } },
            { q: "Alat pengukur manakah yang merekodkan jumlah penggunaan elektrik keseluruhan?", a: "C", options: { A: "Voltmeter (V).", B: "Ammeter (A).", C: "Meter kWj (Kilowatt-jam).", D: "Pengatur Faktor Kuasa." } },
            { q: "Tujuan utama Pengatur Faktor Kuasa dalam PSU adalah untuk:", a: "B", options: { A: "Mengurangkan voltan bekalan.", B: "Meningkatkan kecekapan sistem elektrik.", C: "Mengira beban maksimum.", D: "Menstabilkan frekuensi." } },
            { q: "Dalam operasi Normal, PSU dengan dua bekalan masuk akan mengendalikan Pengganding Palang Bas (Bus Coupler) dalam keadaan:", a: "B", options: { A: "Tertutup (Close).", B: "Terbuka (Open).", C: "Beroperasi secara automatik.", D: "Dihidupkan secara manual." } },
            { q: "Tujuan utama Pengganding Palang Bas (Bus Coupler) dalam situasi kegagalan bekalan adalah untuk:", a: "B", options: { A: "Memisahkan sistem sepenuhnya.", B: "Meningkatkan kebolehpercayaan dan meminimumkan masa henti sistem.", C: "Mengehadkan arus litar pintas.", D: "Menukar bekalan ke voltan lebih tinggi." } },
            { q: "Bilakah Bus Coupler akan tertutup secara automatik?", a: "C", options: { A: "Semasa penyelenggaraan berjadual.", B: "Ketika kedua-dua bekalan Incoming 1 dan Incoming 2 beroperasi normal.", C: "Apabila satu sumber bekalan (misalnya Incoming 1) gagal (Plan B).", D: "Apabila faktor kuasa rendah dikesan." } },
            { q: "Voltmeter (V) pada papan pemuka sistem mengukur:", a: "B", options: { A: "Arus (aliran).", B: "Tekanan (voltan).", C: "Frekuensi.", D: "Kuasa aktif." } },
            { q: "Mengapakah Bus Coupler dikekalkan dalam keadaan terbuka (Open) semasa operasi normal?", a: "B", options: { A: "Untuk menjimatkan tenaga.", B: "Untuk membenarkan setiap bahagian sistem beroperasi secara berasingan.", C: "Untuk memastikan isyarat geganti sentiasa aktif.", D: "Untuk mengasingkan litar pembumian." } },
            { q: "Pemutus Litar Udara (ACB) juga dikenali sebagai 'Penjaga Pintu Utama' kerana:", a: "C", options: { A: "Ia adalah suis paling kecil dalam sistem.", B: "Ia mengawal lampu kecemasan sahaja.", C: "Ia adalah peranti perlindungan dan kawalan yang mula-mula dilalui oleh bekalan masuk.", D: "Ia mengukur kelajuan arus." } },
            { q: "Apakah yang dimaksudkan dengan 'arus rosak' (fault current) yang dialihkan oleh sistem Pembumian?", a: "B", options: { A: "Arus yang terlalu perlahan.", B: "Arus yang bocor ke bahagian logam peralatan.", C: "Arus yang terlalu tinggi (overcurrent).", D: "Arus yang diukur oleh Ammeter." } },
            { q: "Simbol 'CB' dalam rajah skematik biasanya merujuk kepada:", a: "A", options: { A: "Circuit Breaker (Pemutus Litar).", B: "Capacitor Bank.", C: "Current Balancer.", D: "Control Box." } },
            { q: "Komponen manakah yang digunakan untuk menaik taraf Faktor Kuasa sistem?", a: "C", options: { A: "Pemutus Litar Udara (ACB).", B: "Pengganding Palang Bas (Bus Coupler).", C: "Bank Kapasitor (Capacitor Bank).", D: "Geganti Perlindungan (Protection Relay)." } },
            { q: "Dalam situasi kerosakan, geganti perlindungan akan menghantar Isyarat 'trip' (ke ACB) pada langkah ke:", a: "B", options: { A: "Pertama (Kesan kerosakan).", B: "Kedua (Hantar isyarat).", C: "Ketiga (Putus bekalan).", D: "Keempat (Reset sistem)." } },
            { q: "Tujuan utama reka bentuk PSU dengan bekalan sandaran (seperti Generator) dan Bus Coupler adalah untuk mencapai:", a: "B", options: { A: "Penggunaan kuasa yang minimum.", B: "Kelangsungan bekalan (Redundancy).", C: "Voltan keluaran yang stabil.", D: "Pembumian yang lebih berkesan." } },
            { q: "Apakah tugas Voltmeter pada papan suis utama?", a: "B", options: { A: "Mengukur arus.", B: "Mengukur tekanan elektrik (voltan).", C: "Mengira kuasa.", D: "Memperbaiki faktor kuasa." } },
            { q: "Apakah yang akan berlaku kepada bekalan jika ACB tidak berfungsi (Trip) ketika kerosakan berlaku?", a: "C", options: { A: "Bekalan akan diteruskan seperti biasa.", B: "Geganti perlindungan akan mengambil alih fungsi ACB.", C: "Kerosakan dan gangguan bekalan ke bahagian litar tersebut akan berterusan.", D: "Bus Coupler akan tertutup." } },
            { q: "Mengapakah suis dan lubang udara pada bilik PSU perlu dipasang dengan jaring anti vermin proff netting?", a: "C", options: { A: "Untuk mencantikkan bilik suis.", B: "Untuk menghalang habuk masuk.", C: "Untuk menghalang serangga (vermin) daripada masuk dan menyebabkan kerosakan.", D: "Untuk mengawal suhu." } },
            { q: "Apakah maksud TPN dalam konteks Palang Bas TPN?", a: "C", options: { A: "Transformer, Protection, Neutral.", B: "Two Pole and Neutral.", C: "Triple Pole and Neutral.", D: "Total Power and Neutral." } },
            { q: "Dalam PSU Voltan Rendah, bekalan masuk utama diterima sebelum diagih-agihkan ke:", a: "B", options: { A: "Pencawang masuk utama.", B: "Bahagian-bahagian lain beban atau bangunan.", C: "Sistem voltan tinggi.", D: "Bus Coupler." } },
            { q: "PSU juga dikenali sebagai pusat kawalan kepada keseluruhan pemasangan kerana:", a: "B", options: { A: "Ia menempatkan semua lampu.", B: "Ia adalah satu-satunya tempat bekalan boleh di-ON atau di-OFF secara utama.", C: "Ia menyediakan laluan selamat untuk arus rosak.", D: "Ia mengukur faktor kuasa." } },
        ];

        const passingScore = 25; // Markah lulus

        let studentName = '';
        let icNumber = '';
        let timerInterval;
        const TIME_LIMIT = 1800; // 30 minit dalam saat

        // Fungsi untuk mengocok susunan array (Fisher-Yates Shuffle)
        function shuffleArray(array) {
            for (let i = array.length - 1; i > 0; i--) {
                const j = Math.floor(Math.random() * (i + 1));
                [array[i], array[j]] = [array[j], array[i]];
            }
        }

        // Fungsi untuk memuatkan soalan ke dalam UI
        function loadQuestions() {
            const container = document.getElementById('questions-container');
            container.innerHTML = questions.map((qData, index) => {
                const qNum = index + 1;
                return `
                    <div class="p-4 rounded-lg question-item border border-gray-200">
                        <p class="font-semibold text-gray-900 mb-3">${qNum}. ${qData.q}</p>
                        <div class="space-y-2 text-sm">
                            ${Object.keys(qData.options).map(key => `
                                <label class="flex items-center p-2 rounded-lg cursor-pointer hover:bg-blue-50 transition duration-150">
                                    <input type="radio" name="q${qNum}" value="${key}" class="form-radio h-4 w-4 text-blue-600 focus:ring-blue-500">
                                    <span class="ml-3 text-gray-700">(${key}) ${qData.options[key]}</span>
                                </label>
                            `).join('')}
                        </div>
                    </div>
                `;
            }).join('');
        }
        
        // Fungsi untuk memulakan pemasa
        function startTimer() {
            let timeLeft = TIME_LIMIT;
            const countdownDisplay = document.getElementById('countdown');
            const timerBox = document.getElementById('timer-display');

            timerInterval = setInterval(() => {
                const minutes = Math.floor(timeLeft / 60);
                const seconds = timeLeft % 60;
                
                countdownDisplay.textContent = `${minutes.toString().padStart(2, '0')}:${seconds.toString().padStart(2, '0')}`;
                
                // Perubahan warna berdasarkan masa
                if (timeLeft <= 300 && timeLeft > 60) { // 5 minit terakhir
                    timerBox.classList.remove('bg-blue-100', 'text-blue-700');
                    timerBox.classList.add('bg-orange-100', 'text-orange-700');
                } else if (timeLeft <= 60) { // 1 minit terakhir
                    timerBox.classList.remove('bg-orange-100', 'text-orange-700');
                    timerBox.classList.add('bg-red-400', 'text-white');
                }

                if (timeLeft <= 0) {
                    clearInterval(timerInterval);
                    
                    // Kunci borang dan hantar secara automatik
                    document.getElementById('submit-button').disabled = true;
                    document.getElementById('quiz-form').querySelectorAll('input').forEach(input => input.disabled = true);

                    // Panggil pengendali penghantaran kuiz
                    document.getElementById('quiz-form').dispatchEvent(new Event('submit'));
                    
                    // Kemas kini mesej tamat masa di skrin keputusan (akan ditimpa di result-message)
                    document.getElementById('result-screen').setAttribute('data-timed-out', 'true');
                    
                    return;
                }
                
                timeLeft--;
            }, 1000);
        }


        // --- PENGENDALI ACARA ---

        // 1. Pengendali Pendaftaran
        document.getElementById('registration-form').addEventListener('submit', function(event) {
            event.preventDefault();
            studentName = document.getElementById('student-name').value;
            icNumber = document.getElementById('ic-number').value;

            if (studentName && icNumber) {
                // 1. SHUFFLE QUESTIONS
                shuffleArray(questions);
                
                document.getElementById('registration-screen').classList.add('hidden');
                document.getElementById('quiz-screen').classList.remove('hidden');
                loadQuestions();
                
                // 2. START TIMER
                startTimer();
            } else {
                // Gunakan mesej custom UI untuk mengelakkan alert()
                alert('Sila lengkapkan Nama Pelajar dan Nombor Kad Pengenalan.');
            }
        });

        // 2. Pengendali Penghantaran Kuiz
        document.getElementById('quiz-form').addEventListener('submit', function(event) {
            event.preventDefault();
            
            // Hentikan pemasa apabila kuiz dihantar
            clearInterval(timerInterval); 
            
            let correctCount = 0;
            const form = event.target;
            const totalQuestions = questions.length;

            for (let i = 0; i < totalQuestions; i++) {
                const qNum = i + 1;
                // Mengambil nilai radio button yang dipilih
                const selectedInput = form.elements[`q${qNum}`];
                const selected = selectedInput ? selectedInput.value : null;

                if (selected === questions[i].a) {
                    correctCount++;
                }
            }

            const incorrectCount = totalQuestions - correctCount;
            const finalScore = correctCount;

            // Pindah ke skrin keputusan
            document.getElementById('quiz-screen').classList.add('hidden');
            document.getElementById('result-screen').classList.remove('hidden');

            // Paparkan Maklumat Pelajar
            document.getElementById('display-name').textContent = studentName;
            document.getElementById('display-ic').textContent = icNumber;

            // Paparkan Keputusan
            document.getElementById('total-questions').textContent = totalQuestions;
            document.getElementById('correct-answers').textContent = correctCount;
            document.getElementById('incorrect-answers').textContent = incorrectCount;
            document.getElementById('final-score').textContent = finalScore;

            const resultMessage = document.getElementById('result-message');
            const isTimedOut = document.getElementById('result-screen').getAttribute('data-timed-out') === 'true';

            // Logik Mesej Keputusan
            if (isTimedOut) {
                resultMessage.classList.remove('bg-green-200', 'bg-red-200', 'text-green-800', 'text-red-800');
                resultMessage.classList.add('bg-yellow-200', 'text-yellow-800');
                resultMessage.innerHTML = `MASA TAMAT! Jawapan anda telah dihantar secara automatik. Sila lihat markah anda di bawah.`;
            }

            if (finalScore >= passingScore) {
                // LULUS
                resultMessage.classList.remove('bg-red-200', 'text-red-800', 'bg-yellow-200', 'text-yellow-800');
                resultMessage.classList.add('bg-green-200', 'text-green-800');
                resultMessage.innerHTML = `TAHNIAH! Anda **LULUS** ujian ini.`;
            } else {
                // GAGAL
                resultMessage.classList.remove('bg-green-200', 'text-green-800', 'bg-yellow-200', 'text-yellow-800');
                resultMessage.classList.add('bg-red-200', 'text-red-800');
                resultMessage.innerHTML = `MARKAH TIDAK MENCUKUPI. Sila **MENGULANG SEMULA UJIAN** ini.`;
            }
        });
    </script>

</body>
</html>
