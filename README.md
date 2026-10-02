<!DOCTYPE html>
<html lang="id" class="scroll-smooth">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EsHist - Sejarah Pergerakan Nasional Indonesia</title>
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        history: {
                            50: '#fffbeb',
                            100: '#fef3c7',
                            200: '#fde68a',
                            600: '#d97706',
                            700: '#b45309',
                            800: '#92400e',
                            900: '#78350f',
                            maroon: '#800000',
                            maroonDark: '#500000',
                            gold: '#D4AF37'
                        }
                    },
                    fontFamily: {
                        serif: ['Georgia', 'Cambria', 'serif'],
                        sans: ['Inter', 'sans-serif']
                    }
                }
            }
        }
    </script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Canvas Confetti for Game and Quiz celebrations -->
    <script src="https://cdn.jsdelivr.com/npm/canvas-confetti@1.6.0/dist/confetti.browser.min.js"></script>
    <!-- Google Fonts -->
    <link href="https://fonts.googleapis.com/css2?family=Cinzel:wght@600;700;800&family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">

    <style>
        .font-cinzel { font-family: 'Cinzel', serif; }
        .bg-pattern {
            background-color: #fdfbf7;
            background-image: radial-gradient(#d97706 0.5px, transparent 0.5px), radial-gradient(#d97706 0.5px, #fdfbf7 0.5px);
            background-size: 20px 20px;
            background-position: 0 0,10px 10px;
            background-opacity: 0.05;
        }
        .parchment-card {
            background: linear-gradient(135deg, #ffffff 0%, #fefcf6 100%);
            border: 1px solid #f3e8d2;
            box-shadow: 0 10px 25px -5px rgba(120, 53, 15, 0.08);
        }
        .hero-gradient {
            background: linear-gradient(135deg, #4a0000 0%, #800000 50%, #2b0000 100%);
        }
        .gold-border {
            border-image: linear-gradient(to right, #bf953f, #fcf6ba, #b38728, #fbf5b7) 1;
        }
        .game-card {
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .game-card.selected {
            transform: scale(1.03);
            border-color: #d97706;
            box-shadow: 0 0 15px rgba(217, 119, 6, 0.4);
        }
        .game-card.matched {
            background-color: #ecfdf5 !important;
            border-color: #10b981 !important;
            opacity: 0.7;
            pointer-events: none;
        }
        .game-card.wrong {
            animation: shake 0.4s ease-in-out;
            border-color: #ef4444 !important;
        }
        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            20%, 60% { transform: translateX(-6px); }
            40%, 80% { transform: translateX(6px); }
        }
        /* Custom Scrollbar */
        ::-webkit-scrollbar { width: 8px; }
        ::-webkit-scrollbar-track { background: #f1f1f1; }
        ::-webkit-scrollbar-thumb { background: #b45309; border-radius: 4px; }
        ::-webkit-scrollbar-thumb:hover { background: #78350f; }
    </style>
</head>
<body class="bg-pattern text-gray-800 font-sans antialiased min-h-screen flex flex-col justify-between">

    <!-- Navigation Bar -->
    <header class="sticky top-0 z-50 bg-white/90 backdrop-blur-md border-b border-amber-200 shadow-sm">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex justify-between items-center h-20">
                <!-- Logo -->
                <a href="#" class="flex items-center space-x-3 group">
                    <div class="w-12 h-12 rounded-xl bg-gradient-to-tr from-history-maroon to-red-700 flex items-center justify-center text-amber-300 text-2xl font-bold shadow-md transform group-hover:rotate-6 transition duration-300">
                        <i class="fa-solid me-0 fa-landmark"></i>
                    </div>
                    <div>
                        <span class="text-2xl font-bold font-cinzel tracking-wider text-history-maroon block leading-none">EsHist</span>
                        <span class="text-xs text-history-700 font-semibold tracking-widest uppercase">Pergerakan Nasional</span>
                    </div>
                </a>

                <!-- Desktop Nav -->
                <nav class="hidden md:flex items-center space-x-1 lg:space-x-2">
                    <button onclick="switchTab('beranda')" id="nav-beranda" class="nav-btn px-4 py-2 rounded-lg text-sm font-semibold transition text-history-maroon bg-amber-100/80">
                        <i class="fa-solid fa-house mr-1.5"></i> Beranda
                    </button>
                    <button onclick="switchTab('materi')" id="nav-materi" class="nav-btn px-4 py-2 rounded-lg text-sm font-semibold transition text-gray-600 hover:text-history-maroon hover:bg-amber-50">
                        <i class="fa-solid fa-book-open mr-1.5"></i> Materi
                    </button>
                    <button onclick="switchTab('organisasi')" id="nav-organisasi" class="nav-btn px-4 py-2 rounded-lg text-sm font-semibold transition text-gray-600 hover:text-history-maroon hover:bg-amber-50">
                        <i class="fa-solid fa-sitemap mr-1.5"></i> Organisasi
                    </button>
                    <button onclick="switchTab('video')" id="nav-video" class="nav-btn px-4 py-2 rounded-lg text-sm font-semibold transition text-gray-600 hover:text-history-maroon hover:bg-amber-50">
                        <i class="fa-solid fa-circle-play mr-1.5"></i> Video
                    </button>
                    <button onclick="switchTab('minigame')" id="nav-minigame" class="nav-btn px-4 py-2 rounded-lg text-sm font-semibold transition text-gray-600 hover:text-history-maroon hover:bg-amber-50">
                        <i class="fa-solid fa-gamepad mr-1.5"></i> Game Tokoh
                    </button>
                    <button onclick="switchTab('evaluasi')" id="nav-evaluasi" class="nav-btn px-4 py-2 rounded-lg text-sm font-semibold transition text-white bg-history-maroon hover:bg-history-maroonDark shadow-md ml-2">
                        <i class="fa-solid fa-file-pen mr-1.5"></i> Evaluasi (20 Soal)
                    </button>
                </nav>

                <!-- Mobile Menu Button -->
                <div class="md:hidden">
                    <button id="mobile-menu-btn" onclick="toggleMobileMenu()" class="p-2 rounded-lg text-gray-700 hover:bg-amber-100 focus:outline-none">
                        <i class="fa-solid fa-bars text-xl"></i>
                    </button>
                </div>
            </div>
        </div>

        <!-- Mobile Nav Menu -->
        <div id="mobile-menu" class="hidden md:hidden bg-white border-b border-amber-200 px-4 pt-2 pb-4 space-y-2">
            <button onclick="switchTab('beranda'); toggleMobileMenu()" class="w-full text-left px-3 py-2 rounded-md font-medium text-gray-700 hover:bg-amber-50"><i class="fa-solid fa-house mr-2"></i> Beranda</button>
            <button onclick="switchTab('materi'); toggleMobileMenu()" class="w-full text-left px-3 py-2 rounded-md font-medium text-gray-700 hover:bg-amber-50"><i class="fa-solid fa-book-open mr-2"></i> Materi</button>
            <button onclick="switchTab('organisasi'); toggleMobileMenu()" class="w-full text-left px-3 py-2 rounded-md font-medium text-gray-700 hover:bg-amber-50"><i class="fa-solid fa-sitemap mr-2"></i> Organisasi</button>
            <button onclick="switchTab('video'); toggleMobileMenu()" class="w-full text-left px-3 py-2 rounded-md font-medium text-gray-700 hover:bg-amber-50"><i class="fa-solid fa-circle-play mr-2"></i> Video Edukasi</button>
            <button onclick="switchTab('minigame'); toggleMobileMenu()" class="w-full text-left px-3 py-2 rounded-md font-medium text-gray-700 hover:bg-amber-50"><i class="fa-solid fa-gamepad mr-2"></i> Game Tokoh</button>
            <button onclick="switchTab('evaluasi'); toggleMobileMenu()" class="w-full text-left px-3 py-2 rounded-md font-medium text-white bg-history-maroon"><i class="fa-solid fa-file-pen mr-2"></i> Evaluasi (20 Soal)</button>
        </div>
    </header>

    <!-- Main Content Containers -->
    <main class="flex-grow max-w-7xl w-full mx-auto px-4 sm:px-6 lg:px-8 py-8">

        <!-- SECTION: BERANDA -->
        <section id="tab-beranda" class="tab-content space-y-12">
            <!-- Hero Banner -->
            <div class="relative rounded-3xl hero-gradient text-white overflow-hidden shadow-2xl p-8 sm:p-12 lg:p-16 border border-amber-500/30">
                <div class="absolute right-0 top-0 bottom-0 w-1/2 opacity-15 pointer-events-none hidden md:block flex items-center justify-center">
                    <i class="fa-solid fa-landmark text-[300px]"></i>
                </div>
                <div class="relative z-10 max-w-2xl">
                    <span class="inline-flex items-center gap-2 px-3 py-1 rounded-full bg-amber-400/20 text-amber-300 text-xs font-semibold tracking-wider uppercase border border-amber-400/30 mb-4">
                        <i class="fa-solid fa-compass"></i> Media Pembelajaran Sejarah Indonesia
                    </span>
                    <h1 class="text-3xl sm:text-5xl font-extrabold font-cinzel leading-tight tracking-wide text-amber-100 mb-6">
                        Menelusuri Kebangkitan Kesadaran Berbangsa
                    </h1>
                    <p class="text-amber-100/90 text-base sm:text-lg mb-8 leading-relaxed">
                        Selamat datang di <strong>EsHist</strong>! Jelajahi peristiwa lahirnya Pergerakan Nasional Indonesia (1908-1942), peranan para pahlawan terpelajar, strategi organisasi, hingga persatuan Sumpah Pemuda secara interaktif.
                    </p>
                    <div class="flex flex-wrap gap-4">
                        <button onclick="switchTab('materi')" class="px-6 py-3.5 bg-gradient-to-r from-amber-500 to-amber-600 hover:from-amber-600 hover:to-amber-700 text-slate-950 font-bold rounded-xl shadow-lg hover:shadow-amber-500/30 transition transform hover:-translate-y-0.5 flex items-center gap-2">
                            <i class="fa-solid fa-book-reader"></i> Mulai Belajar Materi
                        </button>
                        <button onclick="switchTab('minigame')" class="px-6 py-3.5 bg-white/10 hover:bg-white/20 text-white font-semibold rounded-xl border border-white/20 backdrop-blur-sm transition flex items-center gap-2">
                            <i class="fa-solid fa-puzzle-piece text-amber-300"></i> Mainkan Minigame
                        </button>
                    </div>
                </div>
            </div>

            <!-- Features Grid -->
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6">
                <!-- Card 1 -->
                <div onclick="switchTab('materi')" class="parchment-card rounded-2xl p-6 cursor-pointer hover:border-amber-500 transition group hover:-translate-y-1">
                    <div class="w-14 h-14 rounded-xl bg-amber-100 text-history-maroon flex items-center justify-center text-2xl mb-4 group-hover:bg-history-maroon group-hover:text-white transition">
                        <i class="fa-solid fa-scroll"></i>
                    </div>
                    <h3 class="text-xl font-bold font-cinzel text-gray-900 mb-2">Materi Lengkap</h3>
                    <p class="text-gray-600 text-sm leading-relaxed">
                        Faktor pendorong internal/eksternal, tahapan pergerakan, hingga strategi perlawanan non-fisik.
                    </p>
                </div>
                <!-- Card 2 -->
                <div onclick="switchTab('organisasi')" class="parchment-card rounded-2xl p-6 cursor-pointer hover:border-amber-500 transition group hover:-translate-y-1">
                    <div class="w-14 h-14 rounded-xl bg-amber-100 text-history-maroon flex items-center justify-center text-2xl mb-4 group-hover:bg-history-maroon group-hover:text-white transition">
                        <i class="fa-solid fa-shield-halved"></i>
                    </div>
                    <h3 class="text-xl font-bold font-cinzel text-gray-900 mb-2">Profil Organisasi</h3>
                    <p class="text-gray-600 text-sm leading-relaxed">
                        Budi Utomo, Sarekat Islam, Indische Partij, PNI, PI, Taman Siswa lengkap beserta filosofi lambang.
                    </p>
                </div>
                <!-- Card 3 -->
                <div onclick="switchTab('minigame')" class="parchment-card rounded-2xl p-6 cursor-pointer hover:border-amber-500 transition group hover:-translate-y-1">
                    <div class="w-14 h-14 rounded-xl bg-amber-100 text-history-maroon flex items-center justify-center text-2xl mb-4 group-hover:bg-history-maroon group-hover:text-white transition">
                        <i class="fa-solid fa-brain"></i>
                    </div>
                    <h3 class="text-xl font-bold font-cinzel text-gray-900 mb-2">Game Tokoh</h3>
                    <p class="text-gray-600 text-sm leading-relaxed">
                        Uji ingatan dengan menyamakan tokoh-tokoh pahlawan nasional dengan organisasi pendirinya.
                    </p>
                </div>
                <!-- Card 4 -->
                <div onclick="switchTab('evaluasi')" class="parchment-card rounded-2xl p-6 cursor-pointer hover:border-amber-500 transition group hover:-translate-y-1">
                    <div class="w-14 h-14 rounded-xl bg-amber-100 text-history-maroon flex items-center justify-center text-2xl mb-4 group-hover:bg-history-maroon group-hover:text-white transition">
                        <i class="fa-solid fa-award"></i>
                    </div>
                    <h3 class="text-xl font-bold font-cinzel text-gray-900 mb-2">Evaluasi 20 Soal</h3>
                    <p class="text-gray-600 text-sm leading-relaxed">
                        20 Soal pilihan ganda berstandar kurikulum dengan pembahasan lengkap & sertifikat digital.
                    </p>
                </div>
            </div>

            <!-- Timeline Highlights -->
            <div class="bg-white rounded-3xl p-8 border border-amber-200/80 shadow-md">
                <div class="text-center max-w-xl mx-auto mb-10">
                    <h2 class="text-2xl sm:text-3xl font-bold font-cinzel text-history-maroon">Garis Waktu Pergerakan Kebangsaan</h2>
                    <p class="text-gray-600 text-sm mt-2">Empat periode penting perubahan perjuangan bangsa Indonesia dari kedaerahan menuju persatuan nasional</p>
                </div>
                <div class="grid grid-cols-1 md:grid-cols-4 gap-4 relative">
                    <!-- Period 1 -->
                    <div class="bg-amber-50/50 p-5 rounded-2xl border border-amber-200 hover:shadow-md transition">
                        <span class="px-3 py-1 bg-amber-200 text-amber-900 rounded-full text-xs font-bold">1908 - 1913</span>
                        <h4 class="font-bold font-cinzel text-lg text-history-900 mt-3">Masa Perintis</h4>
                        <p class="text-xs text-gray-600 mt-2">Mulai tumbuhnya kesadaran berorganisasi modern. Ditanandai lahirnya Budi Utomo & Sarekat Islam.</p>
                    </div>
                    <!-- Period 2 -->
                    <div class="bg-amber-50/50 p-5 rounded-2xl border border-amber-200 hover:shadow-md transition">
                        <span class="px-3 py-1 bg-amber-200 text-amber-900 rounded-full text-xs font-bold">1913 - 1928</span>
                        <h4 class="font-bold font-cinzel text-lg text-history-900 mt-3">Masa Radikal</h4>
                        <p class="text-xs text-gray-600 mt-2">Organisasi secara tegas menuntut kemerdekaan penuh & bersifat non-kooperatif terhadap Belanda (Indische Partij, PI, PNI).</p>
                    </div>
                    <!-- Period 3 -->
                    <div class="bg-amber-50/50 p-5 rounded-2xl border border-amber-200 hover:shadow-md transition">
                        <span class="px-3 py-1 bg-amber-200 text-amber-900 rounded-full text-xs font-bold">1928</span>
                        <h4 class="font-bold font-cinzel text-lg text-history-900 mt-3">Masa Penegas</h4>
                        <p class="text-xs text-gray-600 mt-2">Puncak ikrar persatuan tanah air, bangsa, dan bahasa Indonesia melalui Kongres Pemuda II (Sumpah Pemuda).</p>
                    </div>
                    <!-- Period 4 -->
                    <div class="bg-amber-50/50 p-5 rounded-2xl border border-amber-200 hover:shadow-md transition">
                        <span class="px-3 py-1 bg-amber-200 text-amber-900 rounded-full text-xs font-bold">1930 - 1942</span>
                        <h4 class="font-bold font-cinzel text-lg text-history-900 mt-3">Masa Moderat</h4>
                        <p class="text-xs text-gray-600 mt-2">Taktik kooperatif memanfaatkan dewan Volksraad akibat pengawasan ketat pemerintah kolonial (Parindra, GAPI).</p>
                    </div>
                </div>
            </div>
        </section>

        <!-- SECTION: MATERI -->
        <section id="tab-materi" class="tab-content hidden space-y-8">
            <div class="border-b border-amber-200 pb-5">
                <h2 class="text-3xl font-bold font-cinzel text-history-maroon">Materi Pembelajaran Pergerakan Nasional</h2>
                <p class="text-gray-600 mt-1">Pelajari konsep latar belakang, faktor pendorong, strategi, serta tahapan sejarah perjuangan.</p>
            </div>

            <!-- Sub Nav Tabs for Materi -->
            <div class="flex flex-wrap gap-2 border-b border-gray-200 pb-3">
                <button onclick="switchMateriSubTab('materi-1')" id="subnav-materi-1" class="materi-sub-btn px-4 py-2 rounded-xl text-sm font-semibold bg-history-maroon text-white shadow-sm">
                    1. Latar Belakang & Faktor
                </button>
                <button onclick="switchMateriSubTab('materi-2')" id="subnav-materi-2" class="materi-sub-btn px-4 py-2 rounded-xl text-sm font-semibold bg-gray-100 text-gray-700 hover:bg-amber-100">
                    2. Tahapan & Taktik Perjuangan
                </button>
                <button onclick="switchMateriSubTab('materi-3')" id="subnav-materi-3" class="materi-sub-btn px-4 py-2 rounded-xl text-sm font-semibold bg-gray-100 text-gray-700 hover:bg-amber-100">
                    3. Peristiwa Sumpah Pemuda 1928
                </button>
                <button onclick="switchMateriSubTab('materi-4')" id="subnav-materi-4" class="materi-sub-btn px-4 py-2 rounded-xl text-sm font-semibold bg-gray-100 text-gray-700 hover:bg-amber-100">
                    4. Peran Pers & Gerakan Wanita
                </button>
            </div>

            <!-- Sub Content 1 -->
            <div id="materi-1" class="materi-sub-content space-y-6">
                <div class="parchment-card p-6 sm:p-8 rounded-2xl space-y-6">
                    <h3 class="text-2xl font-bold font-cinzel text-history-800 flex items-center gap-3">
                        <i class="fa-solid fa-compass-drafting text-amber-600"></i> Faktor Lahirnya Pergerakan Nasional
                    </h3>
                    <p class="text-gray-700 leading-relaxed">
                        Lahirnya Pergerakan Nasional merupakan masa berpindahnya bentuk perjuangan rakyat Indonesia dari yang semula bersifat kedaerahan, bergantung pada pahlawan karismatik, dan mengandalkan fisik, menjadi perjuangan yang tersistem melalui organisasi modern bertaraf nasional.
                    </p>

                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6 pt-4">
                        <!-- Internal Factors -->
                        <div class="bg-amber-50/70 p-6 rounded-xl border border-amber-200">
                            <h4 class="font-bold text-lg text-history-maroon mb-3 flex items-center gap-2">
                                <i class="fa-solid fa-user-graduate"></i> Faktor Internal (Dari Dalam)
                            </h4>
                            <ul class="space-y-2 text-sm text-gray-700 list-disc list-inside leading-relaxed">
                                <li><strong>Penderitaan Rakyat:</strong> Penjajahan yang panjang menimbulkan kemiskinan dan penderitaan mendalam.</li>
                                <li><strong>Lahirnya Golongan Terpelajar:</strong> Akibat Politik Etis (sekolah STOVIA, OSVIA, ITB), lahir cendekiawan muda yang sadar akan pentingnya persatuan.</li>
                                <li><strong>Kenangan Kejayaan Masa Lalu:</strong> Kejayaan kerajaan Majapahit dan Sriwijaya sebagai simbol bangsa yang pernah bersatu dan berdaulat.</li>
                            </ul>
                        </div>

                        <!-- External Factors -->
                        <div class="bg-red-50/70 p-6 rounded-xl border border-red-200">
                            <h4 class="font-bold text-lg text-history-maroon mb-3 flex items-center gap-2">
                                <i class="fa-solid fa-globe"></i> Faktor Eksternal (Dari Luar)
                            </h4>
                            <ul class="space-y-2 text-sm text-gray-700 list-disc list-inside leading-relaxed">
                                <li><strong>Kemenangan Jepang atas Rusia (1905):</strong> Membuktikan bahwa bangsa Asia mampu mengalahkan bangsa Barat/Eropa.</li>
                                <li><strong>Kebangkitan Nasionalisme Asia-Afrika:</strong> Gerakan kemerdekaan di India (Mahatma Gandhi), Filipina (Jose Rizal), dan Turki Muda (Mustafa Kemal).</li>
                                <li><strong>Paham Baru dari Luar:</strong> Masuknya paham Demokrasi, Nasionalisme, Liberalisme, dan Sosialisme ke Indonesia.</li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Sub Content 2 -->
            <div id="materi-2" class="materi-sub-content hidden space-y-6">
                <div class="parchment-card p-6 sm:p-8 rounded-2xl space-y-6">
                    <h3 class="text-2xl font-bold font-cinzel text-history-800">
                        Perbedaan Taktik Kooperatif vs Non-Kooperatif
                    </h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                        <div class="p-5 bg-emerald-50 border border-emerald-200 rounded-xl">
                            <span class="px-3 py-1 bg-emerald-200 text-emerald-900 rounded-md font-bold text-xs uppercase">Taktik Kooperatif (Moderat)</span>
                            <h4 class="font-bold text-lg text-emerald-950 mt-2">Bekerja Sama dengan Kolonial</h4>
                            <p class="text-sm text-gray-700 mt-2 leading-relaxed">
                                Bersedia duduk di Dewan Rakyat (Volksraad) untuk memperjuangkan nasib rakyat melalui jalur parlemen dan perundang-undangan tanpa konfrontasi langsung.
                            </p>
                            <p class="text-xs font-semibold text-emerald-800 mt-3">Contoh Organisasi: Budi Utomo, Parindra, GAPI.</p>
                        </div>

                        <div class="p-5 bg-rose-50 border border-rose-200 rounded-xl">
                            <span class="px-3 py-1 bg-rose-200 text-rose-900 rounded-md font-bold text-xs uppercase">Taktik Non-Kooperatif (Radikal)</span>
                            <h4 class="font-bold text-lg text-rose-950 mt-2">Menolak Kerja Sama</h4>
                            <p class="text-sm text-gray-700 mt-2 leading-relaxed">
                                Menolak duduk di lembaga buatan Belanda. Mengandalkan kekuatan sendiri (Self-Help) secara tegas menuntut kemerdekaan penuh Indonesia.
                            </p>
                            <p class="text-xs font-semibold text-rose-800 mt-3">Contoh Organisasi: Indische Partij, Perhimpunan Indonesia, PNI.</p>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Sub Content 3 -->
            <div id="materi-3" class="materi-sub-content hidden space-y-6">
                <div class="parchment-card p-6 sm:p-8 rounded-2xl space-y-6">
                    <h3 class="text-2xl font-bold font-cinzel text-history-800 flex items-center gap-3">
                        <i class="fa-solid fa-flag text-red-600"></i> Kongres Pemuda II & Sumpah Pemuda (1928)
                    </h3>
                    <p class="text-gray-700 leading-relaxed">
                        Kongres Pemuda II diselenggarakan pada 27-28 Oktober 1928 di Batavia. Dipimpin oleh Sugondo Djojopuspito (PPPI), kongres ini melahirkan ikrar monumental **Sumpah Pemuda** dan memperdengarkan lagu *Indonesia Raya* karya W.R. Supratman untuk pertama kalinya.
                    </p>
                    <div class="bg-amber-100/60 p-6 rounded-2xl border-l-4 border-history-maroon text-center space-y-3 shadow-inner">
                        <h4 class="font-cinzel text-xl font-bold text-history-maroon">IKRAR SUMPAH PEMUDA</h4>
                        <p class="italic text-sm text-gray-800">
                            "Kami poetra dan poetri Indonesia, mengakoe bertoempah darah jang satoe, tanah Indonesia."<br>
                            "Kami poetra dan poetri Indonesia mengakoe berbangsa jang satoe, bangsa Indonesia."<br>
                            "Kami poetra dan poetri Indonesia meandjoendjoeng bahasa persatoean, bahasa Indonesia."
                        </p>
                    </div>
                </div>
            </div>

            <!-- Sub Content 4 -->
            <div id="materi-4" class="materi-sub-content hidden space-y-6">
                <div class="parchment-card p-6 sm:p-8 rounded-2xl space-y-6">
                    <h3 class="text-2xl font-bold font-cinzel text-history-800">Peran Pers & Emansipasi Gerakan Wanita</h3>
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                        <div class="p-5 bg-amber-50/80 rounded-xl border border-amber-200">
                            <h4 class="font-bold text-history-maroon text-lg mb-2"><i class="fa-solid fa-newspaper mr-2"></i> Peranan Pers Nasional</h4>
                            <p class="text-sm text-gray-700 leading-relaxed">
                                Surat kabar seperti *Medan Prijaji* (Tirto Adhi Soerjo), *Fadjar Asia*, dan *Indonesia Merdeka* berfungsi membakar semangat kebangsaan, menyebarkan ideologi antikolonial, serta mengkritik kebijakan pemerintah Hindia Belanda.
                            </p>
                        </div>
                        <div class="p-5 bg-pink-50/80 rounded-xl border border-pink-200">
                            <h4 class="font-bold text-pink-900 text-lg mb-2"><i class="fa-solid fa-venus mr-2"></i> Pergerakan Wanita</h4>
                            <p class="text-sm text-gray-700 leading-relaxed">
                                Diperlopori R.A. Kartini (Jepara), Dewi Sartika (Bandung dengan Sakola Istri), serta Kongres Perempuan Indonesia I (22 Desember 1928 di Yogyakarta) yang memperjuangkan emansipasi, pendidikan perempuan, dan hak-hak sosial.
                            </p>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- SECTION: ORGANISASI -->
        <section id="tab-organisasi" class="tab-content hidden space-y-8">
            <div class="border-b border-amber-200 pb-5">
                <h2 class="text-3xl font-bold font-cinzel text-history-maroon">Profil & Lambang Organisasi Pergerakan</h2>
                <p class="text-gray-600 mt-1">Klik salah satu kartu organisasi di bawah ini untuk melihat detail pendiri, sejarah, dan filosofi lambangnya.</p>
            </div>

            <!-- Grid Card Organisasi -->
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6" id="org-cards-container">
                <!-- JS will inject organizations dynamic cards here -->
            </div>
        </section>

        <!-- SECTION: VIDEO -->
        <section id="tab-video" class="tab-content hidden space-y-8">
            <div class="border-b border-amber-200 pb-5">
                <h2 class="text-3xl font-bold font-cinzel text-history-maroon">Video Edukasi Pergerakan Nasional</h2>
                <p class="text-gray-600 mt-1">Tonton dokumenter dan penjelasan animasi interaktif seputar sejarah kemerdekaan Indonesia.</p>
            </div>

            <!-- Featured Main Video Player -->
            <div class="parchment-card rounded-3xl overflow-hidden shadow-xl border border-amber-300">
                <div class="aspect-video w-full bg-black">
                    <iframe id="main-youtube-player" class="w-full h-full" src="https://www.youtube.com/embed/G-rWqpjX5EY" title="Video Edukasi Sejarah" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
                </div>
                <div class="p-6">
                    <span class="px-3 py-1 bg-amber-100 text-history-800 rounded-md text-xs font-bold uppercase">Sedang Diputar</span>
                    <h3 id="main-video-title" class="text-xl font-bold text-gray-900 font-cinzel mt-2">Materi Sejarah: Pergerakan Nasional | Ciri, Faktor, dan Sikap Organisasi</h3>
                    <p id="main-video-desc" class="text-sm text-gray-600 mt-2">Penjelasan komprehensif mengenai latar belakang berdirinya organisasi pergerakan nasional serta perbandingannya.</p>
                </div>
            </div>

            <!-- Playlist Items -->
            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                <!-- Video Item 1 -->
                <div onclick="playVideo('G-rWqpjX5EY', 'Materi Sejarah: Pergerakan Nasional | Ciri, Faktor, dan Sikap Organisasi', 'Penjelasan komprehensif mengenai latar belakang berdirinya organisasi pergerakan nasional.')" class="bg-white rounded-2xl p-4 border border-amber-200 cursor-pointer hover:shadow-lg hover:border-amber-500 transition group">
                    <div class="relative aspect-video rounded-xl overflow-hidden bg-gray-200 mb-3">
                        <img src="https://img.youtube.com/vi/G-rWqpjX5EY/hqdefault.jpg" alt="Thumb" class="w-full h-full object-cover group-hover:scale-105 transition duration-300">
                        <div class="absolute inset-0 bg-black/30 flex items-center justify-center group-hover:bg-black/10 transition">
                            <i class="fa-solid fa-play text-white text-3xl drop-shadow-md"></i>
                        </div>
                    </div>
                    <h4 class="font-bold text-sm text-gray-900 line-clamp-2">Ciri, Faktor, dan Sikap Organisasi Pergerakan</h4>
                    <p class="text-xs text-amber-700 font-semibold mt-1">Edcent ID</p>
                </div>

                <!-- Video Item 2 -->
                <div onclick="playVideo('-WdlE8EjR80', 'Pergerakan Nasional: Ciri, Faktor, dan 3 Fase', 'Pembahasan mendalam 3 fase pergerakan nasional yaitu fase perintis, radikal, dan moderat.')" class="bg-white rounded-2xl p-4 border border-amber-200 cursor-pointer hover:shadow-lg hover:border-amber-500 transition group">
                    <div class="relative aspect-video rounded-xl overflow-hidden bg-gray-200 mb-3">
                        <img src="https://img.youtube.com/vi/-WdlE8EjR80/hqdefault.jpg" alt="Thumb" class="w-full h-full object-cover group-hover:scale-105 transition duration-300">
                        <div class="absolute inset-0 bg-black/30 flex items-center justify-center group-hover:bg-black/10 transition">
                            <i class="fa-solid fa-play text-white text-3xl drop-shadow-md"></i>
                        </div>
                    </div>
                    <h4 class="font-bold text-sm text-gray-900 line-clamp-2">Pergerakan Nasional: 3 Fase Utama</h4>
                    <p class="text-xs text-amber-700 font-semibold mt-1">Oy Historia</p>
                </div>

                <!-- Video Item 3 -->
                <div onclick="playVideo('hvDjREWeQg4', 'SMA Kelas 11 Sejarah - Lahirnya Pergerakan Nasional', 'Ringkasan materi sejarah SMA kelas 11 mengenai lahirnya pergerakan nasional Indonesia.')" class="bg-white rounded-2xl p-4 border border-amber-200 cursor-pointer hover:shadow-lg hover:border-amber-500 transition group">
                    <div class="relative aspect-video rounded-xl overflow-hidden bg-gray-200 mb-3">
                        <img src="https://img.youtube.com/vi/hvDjREWeQg4/hqdefault.jpg" alt="Thumb" class="w-full h-full object-cover group-hover:scale-105 transition duration-300">
                        <div class="absolute inset-0 bg-black/30 flex items-center justify-center group-hover:bg-black/10 transition">
                            <i class="fa-solid fa-play text-white text-3xl drop-shadow-md"></i>
                        </div>
                    </div>
                    <h4 class="font-bold text-sm text-gray-900 line-clamp-2">Lahirnya Pergerakan Nasional</h4>
                    <p class="text-xs text-amber-700 font-semibold mt-1">EDUON ID</p>
                </div>

                <!-- Video Item 4 -->
                <div onclick="playVideo('fWCzd284OZQ', 'Organisasi Pergerakan Nasional Indonesia', 'Mengenal organisasi pergerakan dari Budi Utomo hingga Sumpah Pemuda.')" class="bg-white rounded-2xl p-4 border border-amber-200 cursor-pointer hover:shadow-lg hover:border-amber-500 transition group">
                    <div class="relative aspect-video rounded-xl overflow-hidden bg-gray-200 mb-3">
                        <img src="https://img.youtube.com/vi/fWCzd284OZQ/hqdefault.jpg" alt="Thumb" class="w-full h-full object-cover group-hover:scale-105 transition duration-300">
                        <div class="absolute inset-0 bg-black/30 flex items-center justify-center group-hover:bg-black/10 transition">
                            <i class="fa-solid fa-play text-white text-3xl drop-shadow-md"></i>
                        </div>
                    </div>
                    <h4 class="font-bold text-sm text-gray-900 line-clamp-2">Organisasi Kebangsaan & Pahlawan</h4>
                    <p class="text-xs text-amber-700 font-semibold mt-1">Dian Afuarita</p>
                </div>
            </div>
        </section>

        <!-- SECTION: MINIGAME -->
        <section id="tab-minigame" class="tab-content hidden space-y-8">
            <div class="bg-gradient-to-r from-amber-900 via-history-maroon to-amber-900 rounded-3xl p-6 sm:p-8 text-white shadow-xl flex flex-col md:flex-row justify-between items-center gap-6">
                <div>
                    <span class="px-3 py-1 bg-amber-400 text-slate-950 font-bold rounded-full text-xs uppercase">Minigame Edukasi</span>
                    <h2 class="text-3xl font-bold font-cinzel mt-2 text-amber-100">Cocokkan Tokoh & Organisasinya!</h2>
                    <p class="text-amber-100/80 text-sm mt-1 max-w-xl">
                        Pilih satu kartu <strong>Tokoh Pahlawan</strong> di sebelah kiri, kemudian pilih kartu <strong>Organisasi</strong> yang sesuai di sebelah kanan!
                    </p>
                </div>
                <div class="flex items-center gap-4 bg-white/10 px-6 py-4 rounded-2xl border border-white/20 backdrop-blur-md">
                    <div class="text-center">
                        <span class="text-xs text-amber-200 block uppercase font-semibold">Skor Game</span>
                        <span id="game-score" class="text-3xl font-bold text-amber-300">0</span>
                    </div>
                    <div class="h-8 w-px bg-white/20"></div>
                    <div class="text-center">
                        <span class="text-xs text-amber-200 block uppercase font-semibold">Tersisa</span>
                        <span id="game-left" class="text-3xl font-bold text-white">8</span>
                    </div>
                    <button onclick="resetGame()" class="ml-2 p-3 bg-amber-500 hover:bg-amber-600 text-slate-950 rounded-xl transition shadow" title="Reset Game">
                        <i class="fa-solid fa-rotate-right text-lg"></i>
                    </button>
                </div>
            </div>

            <!-- Game Board -->
            <div class="grid grid-cols-1 md:grid-cols-2 gap-8">
                <!-- Column Left: Tokoh -->
                <div class="space-y-4">
                    <h3 class="font-bold font-cinzel text-xl text-history-maroon flex items-center gap-2">
                        <i class="fa-solid fa-user-tag text-amber-600"></i> Kartu Tokoh Pahlawan
                    </h3>
                    <div id="tokoh-cards" class="space-y-3">
                        <!-- Dynamic Tokoh Cards -->
                    </div>
                </div>

                <!-- Column Right: Organisasi -->
                <div class="space-y-4">
                    <h3 class="font-bold font-cinzel text-xl text-history-maroon flex items-center gap-2">
                        <i class="fa-solid fa-building-columns text-amber-600"></i> Kartu Organisasi
                    </h3>
                    <div id="org-game-cards" class="space-y-3">
                        <!-- Dynamic Organization Cards -->
                    </div>
                </div>
            </div>
        </section>

        <!-- SECTION: EVALUASI -->
        <section id="tab-evaluasi" class="tab-content hidden space-y-8">
            <!-- Quiz Start Screen -->
            <div id="quiz-start-screen" class="parchment-card rounded-3xl p-8 sm:p-12 text-center max-w-3xl mx-auto space-y-6">
                <div class="w-20 h-20 bg-amber-100 text-history-maroon rounded-3xl mx-auto flex items-center justify-center text-4xl shadow-md">
                    <i class="fa-solid fa-graduation-cap"></i>
                </div>
                <h2 class="text-3xl font-extrabold font-cinzel text-history-maroon">Evaluasi Hasil Belajar</h2>
                <p class="text-gray-600 text-sm sm:text-base leading-relaxed max-w-xl mx-auto">
                    Uji pemahaman Anda mengenai sejarah Pergerakan Nasional Indonesia melalui <strong>20 Soal Pilihan Ganda</strong>. Setiap soal memiliki bobot skor. Sertifikat kelulusan dapat diunduh di akhir kuis!
                </p>
                <div class="inline-flex flex-wrap justify-center gap-6 py-4 px-6 bg-amber-50 rounded-2xl border border-amber-200 text-xs font-semibold text-gray-700">
                    <span class="flex items-center gap-2"><i class="fa-solid fa-list-check text-amber-600"></i> 20 Soal Pilihan Ganda</span>
                    <span class="flex items-center gap-2"><i class="fa-solid fa-clock text-amber-600"></i> Tanpa Batas Waktu</span>
                    <span class="flex items-center gap-2"><i class="fa-solid fa-certificate text-amber-600"></i> Sertifikat Skor ≥ 70</span>
                </div>
                <div class="pt-2">
                    <button onclick="startQuiz()" class="px-8 py-4 bg-history-maroon hover:bg-history-maroonDark text-white font-bold text-lg rounded-2xl shadow-xl transition transform hover:scale-105">
                        <i class="fa-solid fa-play mr-2"></i> Mulai Kerjakan Evaluasi
                    </button>
                </div>
            </div>

            <!-- Active Quiz Screen -->
            <div id="quiz-active-screen" class="hidden space-y-6 max-w-4xl mx-auto">
                <!-- Progress Header -->
                <div class="bg-white p-6 rounded-2xl border border-amber-200 shadow-sm flex flex-col sm:flex-row justify-between items-center gap-4">
                    <div>
                        <span class="text-xs font-bold text-amber-700 tracking-wider uppercase">Soal Ke-<span id="quiz-current-num">1</span> dari 20</span>
                        <div class="w-48 sm:w-64 bg-gray-200 h-2.5 rounded-full mt-2 overflow-hidden">
                            <div id="quiz-progress-bar" class="bg-history-maroon h-full transition-all duration-300 w-5"></div>
                        </div>
                    </div>
                    <div class="flex items-center gap-2 text-sm font-semibold text-gray-600">
                        <i class="fa-solid fa-circle-question text-amber-600"></i> Terjawab: <span id="quiz-answered-count" class="text-history-maroon font-bold">0</span>/20
                    </div>
                </div>

                <!-- Main Question Box -->
                <div class="parchment-card p-6 sm:p-10 rounded-3xl shadow-lg border border-amber-200 space-y-6">
                    <h3 id="quiz-question-text" class="text-lg sm:text-xl font-bold text-gray-900 leading-snug">
                        Loading pertanyaan...
                    </h3>

                    <!-- Options Container -->
                    <div id="quiz-options-container" class="space-y-3">
                        <!-- Options injected dynamically -->
                    </div>
                </div>

                <!-- Quiz Navigation Buttons -->
                <div class="flex justify-between items-center pt-2">
                    <button id="prev-q-btn" onclick="prevQuestion()" class="px-5 py-2.5 bg-gray-200 hover:bg-gray-300 text-gray-800 font-semibold rounded-xl text-sm transition disabled:opacity-40">
                        <i class="fa-solid fa-arrow-left mr-2"></i> Sebelumnya
                    </button>
                    <button id="next-q-btn" onclick="nextQuestion()" class="px-6 py-2.5 bg-amber-600 hover:bg-amber-700 text-white font-bold rounded-xl text-sm shadow transition">
                        Berikutnya <i class="fa-solid fa-arrow-right ml-2"></i>
                    </button>
                    <button id="submit-quiz-btn" onclick="submitQuiz()" class="hidden px-6 py-2.5 bg-emerald-600 hover:bg-emerald-700 text-white font-bold rounded-xl text-sm shadow transition">
                        <i class="fa-solid fa-check-double mr-2"></i> Selesaikan Quiz
                    </button>
                </div>

                <!-- Number Grid Navigator -->
                <div class="bg-white p-5 rounded-2xl border border-amber-200">
                    <span class="text-xs font-bold text-gray-500 uppercase block mb-3">Navigasi Soal:</span>
                    <div id="quiz-grid-nav" class="flex flex-wrap gap-2">
                        <!-- Grid 1-20 buttons injected by JS -->
                    </div>
                </div>
            </div>

            <!-- Quiz Result Screen -->
            <div id="quiz-result-screen" class="hidden max-w-3xl mx-auto space-y-8">
                <div class="parchment-card rounded-3xl p-8 sm:p-12 text-center shadow-xl border border-amber-300 space-y-6">
                    <div id="result-badge" class="w-24 h-24 rounded-full mx-auto flex items-center justify-center text-5xl shadow-inner">
                        🏆
                    </div>
                    <h2 class="text-3xl font-extrabold font-cinzel text-history-maroon">Hasil Evaluasi Pembelajaran</h2>
                    
                    <div class="grid grid-cols-3 gap-4 max-w-md mx-auto py-4 bg-amber-50 rounded-2xl border border-amber-200">
                        <div>
                            <span class="text-xs text-gray-500 block">Skor Akhir</span>
                            <span id="final-score" class="text-3xl font-extrabold text-history-maroon">0</span>
                        </div>
                        <div>
                            <span class="text-xs text-gray-500 block">Benar</span>
                            <span id="final-correct" class="text-3xl font-extrabold text-emerald-600">0</span>
                        </div>
                        <div>
                            <span class="text-xs text-gray-500 block">Salah</span>
                            <span id="final-wrong" class="text-3xl font-extrabold text-rose-600">0</span>
                        </div>
                    </div>

                    <p id="result-feedback" class="text-gray-700 text-sm sm:text-base italic max-w-lg mx-auto">
                        Pesan umpan balik hasil kuis...
                    </p>

                    <div class="flex flex-wrap justify-center gap-4 pt-4">
                        <button onclick="startQuiz()" class="px-6 py-3 bg-amber-600 hover:bg-amber-700 text-white font-bold rounded-xl shadow transition">
                            <i class="fa-solid fa-rotate-left mr-2"></i> Ulangi Evaluasi
                        </button>
                        <button onclick="toggleReviewMode()" class="px-6 py-3 bg-gray-800 hover:bg-gray-900 text-white font-bold rounded-xl shadow transition">
                            <i class="fa-solid fa-magnifying-glass mr-2"></i> Pembahasan Soal
                        </button>
                    </div>
                </div>

                <!-- Review Section -->
                <div id="quiz-review-container" class="hidden space-y-4">
                    <h3 class="text-2xl font-bold font-cinzel text-history-maroon border-b border-amber-200 pb-3">Pembahasan Kuis Lengkap</h3>
                    <div id="review-items-list" class="space-y-4">
                        <!-- Injected review items -->
                    </div>
                </div>
            </div>
        </section>

    </main>

    <!-- Modal Detail Organisasi -->
    <div id="org-modal" class="fixed inset-0 z-50 bg-black/60 backdrop-blur-sm hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-3xl max-w-2xl w-full p-6 sm:p-8 space-y-6 max-h-[90vh] overflow-y-auto relative border border-amber-300 shadow-2xl">
            <button onclick="closeOrgModal()" class="absolute top-6 right-6 text-gray-400 hover:text-gray-700 text-2xl font-bold">
                &times;
            </button>
            <div class="flex items-center gap-4 border-b border-amber-200 pb-4">
                <div id="modal-org-badge" class="w-16 h-16 rounded-2xl flex items-center justify-center text-white text-2xl shadow-md font-bold">
                    LOGO
                </div>
                <div>
                    <h3 id="modal-org-title" class="text-2xl font-bold font-cinzel text-history-maroon">Nama Organisasi</h3>
                    <span id="modal-org-year" class="px-2.5 py-0.5 bg-amber-100 text-amber-800 rounded-full text-xs font-bold">Tahun</span>
                </div>
            </div>
            <div class="space-y-4 text-sm text-gray-700 leading-relaxed">
                <div>
                    <strong class="text-gray-900 block font-semibold text-base mb-1">Pendiri & Tokoh Kunci:</strong>
                    <p id="modal-org-founders" class="bg-amber-50 p-3 rounded-xl border border-amber-100">Tokoh-tokoh...</p>
                </div>
                <div>
                    <strong class="text-gray-900 block font-semibold text-base mb-1">Tujuan Organisasi:</strong>
                    <p id="modal-org-goals">Tujuan...</p>
                </div>
                <div>
                    <strong class="text-gray-900 block font-semibold text-base mb-1">Strategi & Bentuk Perjuangan:</strong>
                    <p id="modal-org-strategy">Strategi...</p>
                </div>
                <div>
                    <strong class="text-gray-900 block font-semibold text-base mb-1">Filosofi Lambang:</strong>
                    <p id="modal-org-emblem" class="italic text-gray-600 bg-gray-50 p-3 rounded-xl border border-gray-200">Arti lambang...</p>
                </div>
            </div>
        </div>
    </div>

    <!-- Footer -->
    <footer class="bg-gradient-to-b from-gray-900 to-slate-950 text-amber-100/80 border-t border-amber-900/50 mt-16 py-8">
        <div class="max-w-7xl mx-auto px-4 text-center space-y-3">
            <div class="flex justify-center items-center gap-2">
                <div class="w-8 h-8 rounded-lg bg-amber-600 text-white flex items-center justify-center text-sm font-bold">
                    <i class="fa-solid fa-landmark"></i>
                </div>
                <span class="font-cinzel text-xl font-bold text-white tracking-wider">EsHist</span>
            </div>
            <p class="text-xs text-amber-200/60">
                Aplikasi Interactive E-Learning Sejarah Pergerakan Nasional Indonesia &copy; 2026 EsHist Education.
            </p>
        </div>
    </footer>

    <script>
        // ORGANIZATIONS DATASET
        const orgData = [
            {
                id: 'budi-utomo',
                name: 'Budi Utomo',
                year: '20 Mei 1908',
                founders: 'Dr. Sutomo, Dr. Wahidin Sudirohusodo, dan Mahasiswa STOVIA',
                bgGradient: 'from-amber-700 to-yellow-600',
                icon: 'fa-sun',
                goals: 'Memajukan pendidikan, kebudayaan Jawa, serta pengajaran bagi kaum pribumi Hindia Belanda.',
                strategy: 'Kooperatif (Moderat & Sosio-Kultural)',
                emblem: 'Matahari terbit melambangkan fajar kesadaran baru dan pencerahan ilmu pengetahuan bagi bangsa.'
            },
            {
                id: 'sarekat-islam',
                name: 'Sarekat Islam (SI)',
                year: '1911 (SDI) / 1912 (SI)',
                founders: 'K.H. Samanhudi & H.O.S. Tjokroaminoto',
                bgGradient: 'from-emerald-800 to-green-600',
                icon: 'fa-moon',
                goals: 'Mengembangkan jiwa berdagang pribumi, membela hak-hak rakyat kecil, serta memajukan kehidupan berdasarkan syariat Islam.',
                strategy: 'Masa awal Kooperatif kemudian berkembang menjadi pergerakan politik kritis massa.',
                emblem: 'Bulan sabit & bintang melambangkan naungan nilai keagamaan dan keadilan bagi seluruh rakyat.'
            },
            {
                id: 'indische-partij',
                name: 'Indische Partij',
                year: '25 Desember 1912',
                founders: 'Tiga Serangkai (E.F.E. Douwes Dekker, Dr. Cipto Mangunkusumo, Ki Hajar Dewantara)',
                bgGradient: 'from-rose-800 to-red-600',
                icon: 'fa-fire',
                goals: 'Membangun rasa patriotisme seluruh "Indiërs" (pribumi & keturunan) menuju kemerdekaan tanah air Hindia.',
                strategy: 'Non-Kooperatif & Radikal (Slogan: "Indie voor Indiërs")',
                emblem: 'Obor menyala melambangkan semangat perlawanan radikal menumbangkan kolonialisme.'
            },
            {
                id: 'perhimpunan-indonesia',
                name: 'Perhimpunan Indonesia',
                year: '1908 (Indische Vereeniging) / 1925',
                founders: 'Mohammad Hatta, Sutan Sjahrir, Iwa Kusumasumantri (di Belanda)',
                bgGradient: 'from-blue-800 to-indigo-600',
                icon: 'fa-ship',
                goals: 'Menuntut kemerdekaan penuh Indonesia tanpa bantuan kolonial Belanda di tingkat internasional.',
                strategy: 'Non-Kooperatif & Diri Sendiri (Self-Help)',
                emblem: 'Kepala Banteng & Bendera Merah Putih melambangkan keberanian dan kedaulatan bangsa Indonesia.'
            },
            {
                id: 'pni',
                name: 'Partai Nasional Indonesia',
                year: '4 Juli 1927',
                founders: 'Ir. Soekarno & Algemeene Studieclub Bandung',
                bgGradient: 'from-red-900 to-amber-700',
                icon: 'fa-bullhorn',
                goals: 'Mencapai Indonesia Merdeka penuh dengan asas Marhaenisme (membela rakyat kecil).',
                strategy: 'Non-Kooperatif & Massal',
                emblem: 'Kepala Banteng dalam lingkaran melambangkan kekuatan rakyat jelata yang bersatu teguh.'
            },
            {
                id: 'taman-siswa',
                name: 'Taman Siswa',
                year: '3 Juli 1922',
                founders: 'Ki Hajar Dewantara (Soewardi Soerjaningrat)',
                bgGradient: 'from-teal-800 to-cyan-600',
                icon: 'fa-book-bookmark',
                goals: 'Menyediakan pendidikan berjiwa kebangsaan dan kebebasan berpikir bagi anak-anak Indonesia.',
                strategy: 'Sosio-Pendidikan Kebangsaan (Semboyan: Tut Wuri Handayani)',
                emblem: 'Garuda dan Cakra melambangkan kebebasan jiwa dan budi pekerti yang luhur.'
            }
        ];

        // MINIGAME PAIRS (8 Pair Items)
        const gamePairs = [
            { id: 1, tokoh: 'Dr. Sutomo', org: 'Budi Utomo' },
            { id: 2, tokoh: 'H.O.S. Tjokroaminoto', org: 'Sarekat Islam' },
            { id: 3, tokoh: 'Ki Hajar Dewantara', org: 'Taman Siswa' },
            { id: 4, tokoh: 'Ir. Soekarno', org: 'PNI (Partai Nasional Indonesia)' },
            { id: 5, tokoh: 'Mohammad Hatta', org: 'Perhimpunan Indonesia' },
            { id: 6, tokoh: 'Douwes Dekker', org: 'Indische Partij' },
            { id: 7, tokoh: 'K.H. Ahmad Dahlan', org: 'Muhammadiyah' },
            { id: 8, tokoh: 'M.H. Thamrin', org: 'GAPI (Gabungan Politiek Indonesia)' }
        ];

        // EVALUASI 20 QUIZ QUESTIONS
        const quizQuestions = [
            {
                q: "1. Organisasi pergerakan nasional yang berdiri pada 20 Mei 1908 dan tanggal berdirinya diperingati sebagai Hari Kebangkitan Nasional adalah...",
                options: ["Sarekat Islam", "Budi Utomo", "Indische Partij", "Perhimpunan Indonesia"],
                answer: 1,
                explanation: "Budi Utomo didirikan oleh Dr. Sutomo dan para mahasiswa STOVIA pada 20 Mei 1908, menandai dimulainya era pergerakan nasional modern."
            },
            {
                q: "2. Salah satu faktor INTERNAL yang mendorong lahirnya Pergerakan Nasional Indonesia adalah...",
                options: ["Kemenangan Jepang atas Rusia tahun 1905", "Lahirnya golongan terpelajar akibat Politik Etis", "Pergerakan kemerdekaan Turki Muda", "Masuknya paham liberalisme dari Eropa"],
                answer: 1,
                explanation: "Politik Etis di bidang edukasi melahirkan golongan terpelajar (intelektual) yang menyadarkan bangsa akan pentingnya persatuan."
            },
            {
                q: "3. Peristiwa luar negeri yang membakar semangat bangsa Asia, termasuk Indonesia, bahwa bangsa kulit putih dapat dikalahkan adalah...",
                options: ["Perang Dunia I", "Kemenangan Jepang atas Rusia (1905)", "Revolusi Prancis", "Perang Kemerdekaan Amerika"],
                answer: 1,
                explanation: "Kemenangan Jepang (bangsa Asia) atas Rusia (bangsa Eropa/kulit putih) pada 1905 meruntuhkan mitos superioritas bangsa Barat."
            },
            {
                q: "4. Sebelum berganti nama menjadi Sarekat Islam pada tahun 1912, organisasi ini awalnya bernama...",
                options: ["Sarekat Dagang Islam (SDI)", "Indische Vereeniging", "Majelis Islam A'la Indonesia", "Muhammadiyah"],
                answer: 0,
                explanation: "Didirikan oleh KH Samanhudi di Surakarta pada 1911 dengan nama Sarekat Dagang Islam untuk melindungi pedagang batik lokal."
            },
            {
                q: "5. Tokoh yang dikenal dengan julukan 'Tiga Serangkai' pendiri Indische Partij (1912) terdiri dari...",
                options: ["Soekarno, Hatta, Sjahrir", "Sutomo, Wahidin, Cipto", "Douwes Dekker, Cipto Mangunkusumo, Ki Hajar Dewantara", "Tjokroaminoto, Samanhudi, Agus Salim"],
                answer: 2,
                explanation: "Indische Partij didirikan oleh E.F.E. Douwes Dekker (Danudirja Setiabudi), Dr. Cipto Mangunkusumo, dan Suwardi Suryaningrat (Ki Hajar Dewantara)."
            },
            {
                q: "6. Slogan terkenal dari Indische Partij yang menegaskan bahwa tanah Hindia adalah milik seluruh warganya adalah...",
                options: ["Tut Wuri Handayani", "Indie voor Indiërs", "Indonesia Merdeka Now", "Bhinneka Tunggal Ika"],
                answer: 1,
                explanation: "'Indie voor Indiërs' berarti Hindia untuk orang Hindia, menolak dominasi eksploitasi kolonial Belanda."
            },
            {
                q: "7. Organisasi mahasiswa Indonesia di Belanda yang secara berani mempopulerkan istilah 'Indonesia' di dunia internasional adalah...",
                options: ["Indische Partij", "Perhimpunan Indonesia", "PNI Baru", "Gerindo"],
                answer: 1,
                explanation: "Perhimpunan Indonesia (PI) di Belanda (dipimpin Moh. Hatta dkk.) mengganti nama majalahnya menjadi 'Indonesia Merdeka'."
            },
            {
                q: "8. Asas perjuangan Partai Nasional Indonesia (PNI) yang didirikan Ir. Soekarno pada tahun 1927 adalah...",
                options: ["Kooperatif dan Liberal", "Self-Help, Non-Kooperasi, dan Marhaenisme", "Religius dan Sosialis", "Parlementer dan Etis"],
                answer: 1,
                explanation: "PNI bersikap non-kooperatif (menolak kerja sama dengan pemerintah kolonial) dan mengusung paham Marhaenisme membela rakyat kecil."
            },
            {
                q: "9. Kongres Pemuda II yang menghasilkan ikrar Sumpah Pemuda dilaksanakan pada tanggal...",
                options: ["20 Mei 1908", "17 Agustus 1945", "27-28 Oktober 1928", "22 Desember 1928"],
                answer: 2,
                explanation: "Kongres Pemuda II berlangsung tanggal 27-28 Oktober 1928 di Batavia, menetapkan satu tanah air, satu bangsa, dan satu bahasa Indonesia."
            },
            {
                q: "10. Lembaga pendidikan kebangsaan yang didirikan oleh Ki Hajar Dewantara pada tanggal 3 Juli 1922 bernama...",
                options: ["STOVIA", "Taman Siswa", "Sakola Istri", "Muhammadiyah"],
                answer: 1,
                explanation: "Taman Siswa didirikan di Yogyakarta untuk memberikan kesempatan pendidikan kebangsaan bagi seluruh rakyat pribumi."
            },
            {
                q: "11. Semboyan pendidikan 'Tut Wuri Handayani' ciptaan Ki Hajar Dewantara memiliki arti...",
                options: ["Di depan memberi contoh", "Di tengah membangun semangat", "Di belakang memberikan dorongan", "Di mana-mana memberikan pertolongan"],
                answer: 2,
                explanation: "'Tut Wuri Handayani' berarti dari belakang memberikan dorongan dan arahan moral."
            },
            {
                q: "12. Tuntutan utama dari Gabungan Politiek Indonesia (GAPI) yang dibentuk M.H. Thamrin pada tahun 1939 adalah...",
                options: ["Indonesia Berparlemen", "Indonesia Merdeka Seketika", "Penghapusan Tanam Paksa", "Pengembalian Raja Jawa"],
                answer: 0,
                explanation: "GAPI mengusung tuntutan 'Indonesia Berparlemen', yaitu meminta parlemen sejati yang anggotanya dipilih oleh rakyat."
            },
            {
                q: "13. Apa perbedaan mendasar antara strategi perjuangan Kooperatif dan Non-Kooperatif?",
                options: ["Kooperatif menggunakan perang, Non-Kooperatif damai", "Kooperatif mau duduk di parlemen Belanda, Non-Kooperatif menolak kerja sama", "Kooperatif didirikan di luar negeri, Non-Kooperatif di dalam negeri", "Kooperatif dipimpin agama, Non-Kooperatif dipimpin umum"],
                answer: 1,
                explanation: "Strategi kooperatif bersedia bekerja sama dengan pemerintah kolonial (seperti Volksraad), sedangkan non-kooperatif menolak tegas."
            },
            {
                q: "14. Surat kabar 'Medan Prijaji' yang dianggap sebagai pelopor pers nasional Indonesia diterbitkan oleh...",
                options: ["Tirto Adhi Soerjo", "W.R. Supratman", "Mohammad Hatta", "Semaun"],
                answer: 0,
                explanation: "Tirto Adhi Soerjo mendirikan 'Medan Prijaji' pada tahun 1907 dan dikenal sebagai Bapak Pers Nasional Indonesia."
            },
            {
                q: "15. Organisasi kemasyarakatan Islam yang didirikan oleh K.H. Ahmad Dahlan di Yogyakarta pada tahun 1912 adalah...",
                options: ["Nahdlatul Ulama", "Muhammadiyah", "Sarekat Islam", "Persis"],
                answer: 1,
                explanation: "Muhammadiyah didirikan pada 18 November 1912 di Yogyakarta untuk pembaharuan sosial, pendidikan, dan kesehatan Islam."
            },
            {
                q: "16. Tokoh pejuang emansipasi wanita dari Jepara yang surat-suratnya dikumpulkan dalam buku 'Habis Gelap Terbitlah Terang' adalah...",
                options: ["Cut Nyak Dien", "R.A. Kartini", "Dewi Sartika", "Rasuna Said"],
                answer: 1,
                explanation: "Raden Ajeng Kartini memperjuangkan emansipasi dan hak pendidikan bagi kaum perempuan Indonesia."
            },
            {
                q: "17. Tokoh pimpinan Sarekat Islam yang sangat berpengaruh dan dijuluki oleh Belanda sebagai 'Raja Jawa Tanpa Mahkota' adalah...",
                options: ["H.O.S. Tjokroaminoto", "Dr. Sutomo", "Ki Hajar Dewantara", "Muso"],
                answer: 0,
                explanation: "H.O.S. Tjokroaminoto (De Ongekroonde Koning van Java) adalah guru dari banyak tokoh bangsa seperti Soekarno, Muso, dan Kartosuwiryo."
            },
            {
                q: "18. Tokoh emansipasi wanita dari Jawa Barat yang mendirikan Sekolah 'Sakola Istri' pada tahun 1904 adalah...",
                options: ["R.A. Kartini", "Dewi Sartika", "Christina Martha Tiahahu", "Nyi Ageng Serang"],
                answer: 1,
                explanation: "Dewi Sartika mendirikan Sakola Istri di Bandung untuk mendidik anak-anak perempuan keterampilan dan budi pekerti."
            },
            {
                q: "19. Pengajuan 'Petisi Soetardjo' pada tahun 1936 di Volksraad berisi tentang permintaan...",
                options: ["Kemerdekaan bertahap Indonesia dalam waktu 10 tahun", "Penangkapan Soekarno", "Pembubaran PNI", "Kenaikan pajak hasil bumi"],
                answer: 0,
                explanation: "Soetardjo Kartohadikoesoemo mengajukan petisi agar Indonesia diberi otonomi dan kemerdekaan secara bertahap melalui konferensi."
            },
            {
                q: "20. Mengapa pergerakan nasional Indonesia pada era 1930-an beralih ke taktik Moderat (Kooperatif)?",
                options: ["Karena Belanda bersikap sangat represif dan menangkap pimpinan organisasi radikal", "Karena rakyat sudah bosan merdeka", "Karena organisasi radikal kekurangan dana", "Atas perintah pemerintah Jepang"],
                answer: 0,
                explanation: "Gubernur Jenderal de Jonge menerapkan tindakan keras, penangkapan, dan pengasingan tokoh radikal (Soekarno, Hatta, Sjahrir) sehingga taktik dialihkan ke kooperatif."
            }
        ];

        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
            document.getElementById(`tab-${tabId}`).classList.remove('hidden');

            document.querySelectorAll('.nav-btn').forEach(btn => {
                btn.classList.remove('text-history-maroon', 'bg-amber-100/80');
                btn.classList.add('text-gray-600');
            });
            const activeNav = document.getElementById(`nav-${tabId}`);
            if (activeNav && tabId !== 'evaluasi') {
                activeNav.classList.add('text-history-maroon', 'bg-amber-100/80');
                activeNav.classList.remove('text-gray-600');
            }
            window.scrollTo({ top: 0, behavior: 'smooth' });
        }

        function toggleMobileMenu() {
            document.getElementById('mobile-menu').classList.toggle('hidden');
        }

        function switchMateriSubTab(subId) {
            document.querySelectorAll('.materi-sub-content').forEach(el => el.classList.add('hidden'));
            document.getElementById(subId).classList.remove('hidden');

            document.querySelectorAll('.materi-sub-btn').forEach(btn => {
                btn.classList.remove('bg-history-maroon', 'text-white');
                btn.classList.add('bg-gray-100', 'text-gray-700');
            });
            const activeBtn = document.getElementById(`subnav-${subId}`);
            if (activeBtn) {
                activeBtn.classList.add('bg-history-maroon', 'text-white');
                activeBtn.classList.remove('bg-gray-100', 'text-gray-700');
            }
        }

        function renderOrganizations() {
            const container = document.getElementById('org-cards-container');
            container.innerHTML = orgData.map(org => `
                <div onclick="openOrgModal('${org.id}')" class="parchment-card rounded-2xl p-6 cursor-pointer hover:border-amber-500 transition-all duration-300 transform hover:-translate-y-1 shadow-md flex flex-col justify-between">
                    <div>
                        <div class="flex items-center justify-between mb-4">
                            <div class="w-14 h-14 rounded-2xl bg-gradient-to-tr ${org.bgGradient} text-white flex items-center justify-center text-2xl shadow-md">
                                <i class="fa-solid ${org.icon}"></i>
                            </div>
                            <span class="px-3 py-1 bg-amber-100 text-history-800 rounded-full text-xs font-bold">${org.year}</span>
                        </div>
                        <h3 class="text-xl font-bold font-cinzel text-gray-900 mb-1">${org.name}</h3>
                        <p class="text-xs text-amber-700 font-semibold mb-3">Pendiri: ${org.founders}</p>
                        <p class="text-gray-600 text-sm line-clamp-3">${org.goals}</p>
                    </div>
                    <div class="mt-4 pt-3 border-t border-amber-100 flex items-center justify-between text-xs font-bold text-history-maroon">
                        <span>Lihat Detail Lambang & Sejarah</span>
                        <i class="fa-solid fa-arrow-right"></i>
                    </div>
                </div>
            `).join('');
        }

        function openOrgModal(orgId) {
            const org = orgData.find(o => o.id === orgId);
            if (!org) return;

            document.getElementById('modal-org-title').innerText = org.name;
            document.getElementById('modal-org-year').innerText = org.year;
            document.getElementById('modal-org-founders').innerText = org.founders;
            document.getElementById('modal-org-goals').innerText = org.goals;
            document.getElementById('modal-org-strategy').innerText = org.strategy;
            document.getElementById('modal-org-emblem').innerText = org.emblem;

            const badge = document.getElementById('modal-org-badge');
            badge.className = `w-16 h-16 rounded-2xl flex items-center justify-center text-white text-2xl shadow-md font-bold bg-gradient-to-tr ${org.bgGradient}`;
            badge.innerHTML = `<i class="fa-solid ${org.icon}"></i>`;

            document.getElementById('org-modal').classList.remove('hidden');
        }

        function closeOrgModal() {
            document.getElementById('org-modal').classList.add('hidden');
        }

        function playVideo(ytId, title, desc) {
            document.getElementById('main-youtube-player').src = `https://www.youtube.com/embed/${ytId}?autoplay=1`;
            document.getElementById('main-video-title').innerText = title;
            document.getElementById('main-video-desc').innerText = desc;
            window.scrollTo({ top: document.getElementById('tab-video').offsetTop - 100, behavior: 'smooth' });
        }

        let selectedTokoh = null;
        let selectedOrg = null;
        let gameScore = 0;
        let pairsLeft = 8;

        function initMinigame() {
            gameScore = 0;
            pairsLeft = gamePairs.length;
            document.getElementById('game-score').innerText = '0';
            document.getElementById('game-left').innerText = pairsLeft;

            // Shuffle pairs for both columns
            const shuffledTokoh = [...gamePairs].sort(() => Math.random() - 0.5);
            const shuffledOrg = [...gamePairs].sort(() => Math.random() - 0.5);

            const containerTokoh = document.getElementById('tokoh-cards');
            const containerOrg = document.getElementById('org-game-cards');

            containerTokoh.innerHTML = shuffledTokoh.map(item => `
                <div id="tokoh-card-${item.id}" onclick="selectGameCard('tokoh', ${item.id})" class="game-card p-4 bg-white rounded-xl border border-amber-200 cursor-pointer shadow-sm flex items-center justify-between hover:border-amber-400">
                    <span class="font-bold text-gray-800 text-sm sm:text-base"><i class="fa-solid fa-user text-amber-600 mr-2"></i> ${item.tokoh}</span>
                    <i class="fa-solid fa-circle-dot text-gray-300 status-icon"></i>
                </div>
            `).join('');

            containerOrg.innerHTML = shuffledOrg.map(item => `
                <div id="org-card-${item.id}" onclick="selectGameCard('org', ${item.id})" class="game-card p-4 bg-white rounded-xl border border-amber-200 cursor-pointer shadow-sm flex items-center justify-between hover:border-amber-400">
                    <span class="font-bold text-gray-800 text-sm sm:text-base"><i class="fa-solid fa-building text-amber-600 mr-2"></i> ${item.org}</span>
                    <i class="fa-solid fa-circle-dot text-gray-300 status-icon"></i>
                </div>
            `).join('');
        }

        function selectGameCard(type, id) {
            if (type === 'tokoh') {
                document.querySelectorAll('#tokoh-cards .game-card').forEach(el => el.classList.remove('selected'));
                const el = document.getElementById(`tokoh-card-${id}`);
                if (el.classList.contains('matched')) return;
                el.classList.add('selected');
                selectedTokoh = id;
            } else {
                document.querySelectorAll('#org-game-cards .game-card').forEach(el => el.classList.remove('selected'));
                const el = document.getElementById(`org-card-${id}`);
                if (el.classList.contains('matched')) return;
                el.classList.add('selected');
                selectedOrg = id;
            }

            if (selectedTokoh !== null && selectedOrg !== null) {
                checkMatch();
            }
        }

        function checkMatch() {
            const cardTokoh = document.getElementById(`tokoh-card-${selectedTokoh}`);
            const cardOrg = document.getElementById(`org-card-${selectedOrg}`);

            if (selectedTokoh === selectedOrg) {
                // CORRECT MATCH!
                cardTokoh.classList.remove('selected');
                cardOrg.classList.remove('selected');
                cardTokoh.classList.add('matched');
                cardOrg.classList.add('matched');

                cardTokoh.querySelector('.status-icon').className = 'fa-solid fa-circle-check text-emerald-500';
                cardOrg.querySelector('.status-icon').className = 'fa-solid fa-circle-check text-emerald-500';

                gameScore += 100;
                pairsLeft--;
                document.getElementById('game-score').innerText = gameScore;
                document.getElementById('game-left').innerText = pairsLeft;

                selectedTokoh = null;
                selectedOrg = null;

                if (pairsLeft === 0) {
                    if (typeof confetti === 'function') {
                        try {
                            confetti({ particleCount: 100, spread: 70, origin: { y: 0.6 } });
                        } catch (e) {
                            console.warn('Confetti unavailable:', e);
                        }
                    }
                }
            } else {
                // WRONG MATCH!
                cardTokoh.classList.add('wrong');
                cardOrg.classList.add('wrong');

                setTimeout(() => {
                    cardTokoh.classList.remove('wrong', 'selected');
                    cardOrg.classList.remove('wrong', 'selected');
                    selectedTokoh = null;
                    selectedOrg = null;
                }, 500);
            }
        }

        function resetGame() {
            selectedTokoh = null;
            selectedOrg = null;
            initMinigame();
        }

        let currentQuestionIdx = 0;
        let userAnswers = new Array(20).fill(null);

        function startQuiz() {
            document.getElementById('quiz-start-screen').classList.add('hidden');
            document.getElementById('quiz-result-screen').classList.add('hidden');
            document.getElementById('quiz-active-screen').classList.remove('hidden');

            currentQuestionIdx = 0;
            userAnswers = new Array(20).fill(null);

            renderQuestion();
            renderGridNav();
        }

        function renderQuestion() {
            const q = quizQuestions[currentQuestionIdx];
            document.getElementById('quiz-current-num').innerText = currentQuestionIdx + 1;
            document.getElementById('quiz-question-text').innerText = q.q;

            // Progress bar
            const pct = ((currentQuestionIdx + 1) / 20) * 100;
            document.getElementById('quiz-progress-bar').style.width = `${pct}%`;

            // Options
            const optionsContainer = document.getElementById('quiz-options-container');
            const letters = ['A', 'B', 'C', 'D'];
            optionsContainer.innerHTML = q.options.map((opt, i) => {
                const isSelected = userAnswers[currentQuestionIdx] === i;
                return `
                    <div onclick="selectAnswer(${i})" class="p-4 rounded-2xl border ${isSelected ? 'border-amber-600 bg-amber-100/80 shadow-md' : 'border-amber-200 bg-white hover:bg-amber-50'} cursor-pointer transition flex items-center gap-3">
                        <div class="w-8 h-8 rounded-xl flex items-center justify-center font-bold text-sm ${isSelected ? 'bg-history-maroon text-white' : 'bg-amber-200/60 text-amber-900'}">
                            ${letters[i]}
                        </div>
                        <span class="text-sm sm:text-base font-medium text-gray-800">${opt}</span>
                    </div>
                `;
            }).join('');

            // Nav Buttons visibility
            document.getElementById('prev-q-btn').disabled = currentQuestionIdx === 0;

            if (currentQuestionIdx === 19) {
                document.getElementById('next-q-btn').classList.add('hidden');
                document.getElementById('submit-quiz-btn').classList.remove('hidden');
            } else {
                document.getElementById('next-q-btn').classList.remove('hidden');
                document.getElementById('submit-quiz-btn').classList.add('hidden');
            }

            updateAnsweredCount();
            updateGridNav();
        }

        function selectAnswer(optIdx) {
            userAnswers[currentQuestionIdx] = optIdx;
            renderQuestion();
        }

        function prevQuestion() {
            if (currentQuestionIdx > 0) {
                currentQuestionIdx--;
                renderQuestion();
            }
        }

        function nextQuestion() {
            if (currentQuestionIdx < 19) {
                currentQuestionIdx++;
                renderQuestion();
            }
        }

        function jumpToQuestion(idx) {
            currentQuestionIdx = idx;
            renderQuestion();
        }

        function updateAnsweredCount() {
            const answered = userAnswers.filter(a => a !== null).length;
            document.getElementById('quiz-answered-count').innerText = answered;
        }

        function renderGridNav() {
            const container = document.getElementById('quiz-grid-nav');
            container.innerHTML = quizQuestions.map((_, i) => `
                <button id="grid-num-${i}" onclick="jumpToQuestion(${i})" class="w-9 h-9 rounded-xl font-bold text-xs border border-amber-200 transition">
                    ${i + 1}
                </button>
            `).join('');
            updateGridNav();
        }

        function updateGridNav() {
            quizQuestions.forEach((_, i) => {
                const btn = document.getElementById(`grid-num-${i}`);
                if (!btn) return;
                
                btn.className = "w-9 h-9 rounded-xl font-bold text-xs transition border ";
                if (i === currentQuestionIdx) {
                    btn.className += "bg-history-maroon text-white border-history-maroon ring-2 ring-amber-400";
                } else if (userAnswers[i] !== null) {
                    btn.className += "bg-emerald-100 text-emerald-900 border-emerald-300";
                } else {
                    btn.className += "bg-gray-100 text-gray-700 hover:bg-amber-100 border-amber-200";
                }
            });
        }

        function submitQuiz() {
            let correctCount = 0;
            quizQuestions.forEach((q, i) => {
                if (userAnswers[i] === q.answer) {
                    correctCount++;
                }
            });

            const score = (correctCount / 20) * 100;
            const wrongCount = 20 - correctCount;

            document.getElementById('quiz-active-screen').classList.add('hidden');
            document.getElementById('quiz-result-screen').classList.remove('hidden');

            document.getElementById('final-score').innerText = score;
            document.getElementById('final-correct').innerText = correctCount;
            document.getElementById('final-wrong').innerText = wrongCount;

            const badgeEl = document.getElementById('result-badge');
            const feedbackEl = document.getElementById('result-feedback');

            if (score >= 80) {
                badgeEl.innerText = '🥇';
                badgeEl.className = 'w-24 h-24 rounded-full mx-auto flex items-center justify-center text-5xl shadow-inner bg-amber-100 text-amber-700';
                feedbackEl.innerText = "Luar biasa! Anda memiliki pemahaman yang sangat mendalam mengenai Sejarah Pergerakan Nasional Indonesia.";
                if (typeof confetti === 'function') {
                    try {
                        confetti({ particleCount: 150, spread: 80, origin: { y: 0.5 } });
                    } catch (e) {
                        console.warn('Confetti unavailable:', e);
                    }
                }
            } else if (score >= 60) {
                badgeEl.innerText = '🥈';
                badgeEl.className = 'w-24 h-24 rounded-full mx-auto flex items-center justify-center text-5xl shadow-inner bg-blue-100 text-blue-700';
                feedbackEl.innerText = "Bagus sekali! Pemahaman Anda tentang materi pergerakan nasional sudah memuaskan.";
            } else {
                badgeEl.innerText = '📜';
                badgeEl.className = 'w-24 h-24 rounded-full mx-auto flex items-center justify-center text-5xl shadow-inner bg-gray-100 text-gray-700';
                feedbackEl.innerText = "Tetap semangat! Pelajari kembali modul materi dan tonton video edukasi untuk meningkatkan pemahaman Anda.";
            }

            renderReviewItems();
        }

        function toggleReviewMode() {
            const container = document.getElementById('quiz-review-container');
            container.classList.toggle('hidden');
        }

        function renderReviewItems() {
            const container = document.getElementById('review-items-list');
            const letters = ['A', 'B', 'C', 'D'];

            container.innerHTML = quizQuestions.map((q, i) => {
                const userAns = userAnswers[i];
                const isCorrect = userAns === q.answer;

                return `
                    <div class="p-5 rounded-2xl border ${isCorrect ? 'border-emerald-200 bg-emerald-50/50' : 'border-rose-200 bg-rose-50/50'} space-y-3">
                        <div class="flex items-start justify-between gap-2">
                            <h4 class="font-bold text-gray-900 text-sm sm:text-base">${q.q}</h4>
                            <span class="px-2.5 py-1 rounded-full text-xs font-bold ${isCorrect ? 'bg-emerald-200 text-emerald-900' : 'bg-rose-200 text-rose-900'}">
                                ${isCorrect ? 'Benar' : 'Salah'}
                            </span>
                        </div>
                        <div class="text-xs sm:text-sm space-y-1">
                            <p class="text-gray-700"><strong>Jawaban Anda:</strong> ${userAns !== null ? letters[userAns] + '. ' + q.options[userAns] : 'Tidak dijawab'}</p>
                            <p class="text-emerald-800"><strong>Jawaban Tepat:</strong> ${letters[q.answer]}. ${q.options[q.answer]}</p>
                        </div>
                        <div class="bg-white p-3 rounded-xl border border-gray-200 text-xs text-gray-600">
                            <strong>Pembahasan:</strong> ${q.explanation}
                        </div>
                    </div>
                `;
            }).join('');
        }

        // Initialize on page load
        window.onload = function() {
            renderOrganizations();
            initMinigame();
        };
    </script>
</body>
</html>
