<!DOCTYPE html>
<html lang="id" class="h-full">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>EsHist - Eksplorasi Sejarah Interaktif</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        esRed: '#DC2626',
                        esDarkRed: '#991B1B',
                        esCream: '#FDFBF7',
                        esWarmWhite: '#FFFDF9',
                        esGrayText: '#334155'
                    },
                    fontFamily: {
                        sans: ['Inter', 'sans-serif']
                    }
                }
            }
        }
    </script>
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #FDFBF7;
            color: #334155;
            overflow-x: hidden;
        }

        /* Page Transition Animations */
        .page-view {
            display: none;
            opacity: 0;
            transform: translateY(8px) scale(0.99);
            transition: opacity 350ms cubic-bezier(0.16, 1, 0.3, 1), transform 350ms cubic-bezier(0.16, 1, 0.3, 1);
        }

        .page-view.active {
            display: block;
            opacity: 1;
            transform: translateY(0) scale(1);
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #F1F5F9;
        }
        ::-webkit-scrollbar-thumb {
            boolean: true;
            background: #CBD5E1;
            border-radius: 9999px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #94A3B8;
        }

        /* Card Hover & Micro-interactions */
        .es-card {
            transition: all 0.25s ease;
        }
        .es-card:hover {
            transform: translateY(-3px);
            box-shadow: 0 10px 25px -5px rgba(0, 0, 0, 0.08), 0 8px 10px -6px rgba(0, 0, 0, 0.08);
        }

        /* Game Memory Card Flip */
        .perspective-1000 {
            perspective: 1000px;
        }
        .transform-style-3d {
            transform-style: preserve-3d;
            transition: transform 0.5s ease;
        }
        .rotate-y-180 {
            transform: rotateY(180deg);
        }
        .backface-hidden {
            backface-visibility: hidden;
        }
    </style>
</head>
<body class="h-full flex flex-col md:flex-row bg-esCream text-esGrayText antialiased">

    <!-- DESKTOP SIDEBAR -->
    <aside id="sidebar" class="hidden md:flex flex-col w-64 bg-white border-r border-stone-200 p-6 z-30 shrink-0 select-none shadow-sm">
        <div class="flex items-center gap-3 mb-8 px-2">
            <div class="w-10 h-10 rounded-2xl bg-gradient-to-tr from-esRed to-esDarkRed flex items-center justify-center text-white font-bold text-xl shadow-md shadow-red-500/20">
                Es
            </div>
            <div>
                <h1 class="font-bold text-lg leading-tight text-stone-900">EsHist</h1>
                <p class="text-xs text-stone-500 font-medium">Eksplorasi Sejarah Interaktif</p>
            </div>
        </div>

        <nav class="space-y-1.5 flex-1">
            <button onclick="switchPage('home')" data-target="home" class="nav-btn w-full flex items-center gap-3.5 px-4 py-3 rounded-2xl text-sm font-medium transition-all text-stone-600 hover:bg-stone-50 hover:text-stone-900">
                <span class="text-lg">🏠</span> Beranda
            </button>
            <button onclick="switchPage('timeline')" data-target="timeline" class="nav-btn w-full flex items-center gap-3.5 px-4 py-3 rounded-2xl text-sm font-medium transition-all text-stone-600 hover:bg-stone-50 hover:text-stone-900">
                <span class="text-lg">⏳</span> Jejak Sejarah
            </button>
            <button onclick="switchPage('videos')" data-target="videos" class="nav-btn w-full flex items-center gap-3.5 px-4 py-3 rounded-2xl text-sm font-medium transition-all text-stone-600 hover:bg-stone-50 hover:text-stone-900">
                <span class="text-lg">🎬</span> Video Edukasi
            </button>
            <button onclick="switchPage('materials')" data-target="materials" class="nav-btn w-full flex items-center gap-3.5 px-4 py-3 rounded-2xl text-sm font-medium transition-all text-stone-600 hover:bg-stone-50 hover:text-stone-900">
                <span class="text-lg">📚</span> Materi
            </button>
            <button onclick="switchPage('game')" data-target="game" class="nav-btn w-full flex items-center gap-3.5 px-4 py-3 rounded-2xl text-sm font-medium transition-all text-stone-600 hover:bg-stone-50 hover:text-stone-900">
                <span class="text-lg">🎮</span> Game
            </button>
            <button onclick="switchPage('evaluation')" data-target="evaluation" class="nav-btn w-full flex items-center gap-3.5 px-4 py-3 rounded-2xl text-sm font-medium transition-all text-stone-600 hover:bg-stone-50 hover:text-stone-900">
                <span class="text-lg">📝</span> Evaluasi
            </button>
        </nav>

        <div class="pt-6 border-t border-stone-100">
            <button onclick="switchPage('about')" data-target="about" class="nav-btn w-full flex items-center gap-3.5 px-4 py-3 rounded-2xl text-sm font-medium transition-all text-stone-600 hover:bg-stone-50 hover:text-stone-900">
                <span class="text-lg">ℹ️</span> Tentang
            </button>
            <div class="mt-4 p-4 rounded-2xl bg-stone-50 border border-stone-100">
                <div class="flex justify-between items-center text-xs font-semibold mb-1.5 text-stone-700">
                    <span>Progress EsHist</span>
                    <span id="global-progress-text">0%</span>
                </div>
                <div class="w-full bg-stone-200 rounded-full h-2 overflow-hidden">
                    <div id="global-progress-bar" class="bg-esRed h-full transition-all duration-500 rounded-full" style="width: 0%"></div>
                </div>
            </div>
        </div>
    </aside>

    <!-- MOBILE BOTTOM NAVIGATION -->
    <nav class="md:hidden fixed bottom-0 left-0 right-0 bg-white/95 backdrop-blur-md border-t border-stone-200 z-40 px-3 py-2 flex justify-around items-center shadow-lg">
        <button onclick="switchPage('home')" data-target="home" class="mob-nav-btn flex flex-col items-center p-1.5 text-xs text-stone-500 font-medium transition-all">
            <span class="text-xl">🏠</span>
            <span class="text-[10px] mt-0.5">Beranda</span>
        </button>
        <button onclick="switchPage('timeline')" data-target="timeline" class="mob-nav-btn flex flex-col items-center p-1.5 text-xs text-stone-500 font-medium transition-all">
            <span class="text-xl">⏳</span>
            <span class="text-[10px] mt-0.5">Jejak</span>
        </button>
        <button onclick="switchPage('videos')" data-target="videos" class="mob-nav-btn flex flex-col items-center p-1.5 text-xs text-stone-500 font-medium transition-all">
            <span class="text-xl">🎬</span>
            <span class="text-[10px] mt-0.5">Video</span>
        </button>
        <button onclick="switchPage('materials')" data-target="materials" class="mob-nav-btn flex flex-col items-center p-1.5 text-xs text-stone-500 font-medium transition-all">
            <span class="text-xl">📚</span>
            <span class="text-[10px] mt-0.5">Materi</span>
        </button>
        <button onclick="switchPage('game')" data-target="game" class="mob-nav-btn flex flex-col items-center p-1.5 text-xs text-stone-500 font-medium transition-all">
            <span class="text-xl">🎮</span>
            <span class="text-[10px] mt-0.5">Game</span>
        </button>
        <button onclick="switchPage('evaluation')" data-target="evaluation" class="mob-nav-btn flex flex-col items-center p-1.5 text-xs text-stone-500 font-medium transition-all">
            <span class="text-xl">📝</span>
            <span class="text-[10px] mt-0.5">Evaluasi</span>
        </button>
        <button onclick="switchPage('about')" data-target="about" class="mob-nav-btn flex flex-col items-center p-1.5 text-xs text-stone-500 font-medium transition-all">
            <span class="text-xl">ℹ️️</span>
            <span class="text-[10px] mt-0.5">Tentang</span>
        </button>
    </nav>

    <!-- MAIN APP CONTAINER -->
    <main class="flex-1 flex flex-col min-w-0 h-screen overflow-hidden pb-16 md:pb-0">
        <!-- TOPBAR -->
        <header class="bg-white/80 backdrop-blur-md border-b border-stone-200 px-6 py-4 flex items-center justify-between z-20 shrink-0">
            <div class="flex items-center gap-3">
                <span id="topbar-icon" class="text-xl">🏠</span>
                <h2 id="topbar-title" class="font-bold text-lg text-stone-900 tracking-tight">Beranda</h2>
            </div>
            <div class="flex items-center gap-4">
                <div class="hidden sm:flex flex-col items-end">
                    <span class="text-xs font-semibold text-stone-500">Progress Belajar</span>
                    <span id="topbar-progress-text" class="text-xs font-bold text-esRed">0%</span>
                </div>
                <div class="w-28 bg-stone-200 rounded-full h-2.5 overflow-hidden">
                    <div id="topbar-progress-bar" class="bg-esRed h-full transition-all duration-500 rounded-full" style="width: 0%"></div>
                </div>
            </div>
        </header>

        <!-- CONTENT VIEWS CONTAINER -->
        <div class="flex-1 overflow-y-auto p-4 sm:p-8">

            <!-- 1. BERANDA VIEW -->
            <div id="view-home" class="page-view max-w-6xl mx-auto space-y-8">
                <!-- Hero Section -->
                <div class="relative bg-gradient-to-br from-stone-900 via-stone-800 to-esDarkRed text-white rounded-3xl p-8 sm:p-12 overflow-hidden shadow-xl">
                    <div class="absolute -right-12 -bottom-12 w-64 h-64 bg-esRed/20 rounded-full blur-3xl pointer-events-none"></div>
                    <div class="relative z-10 max-w-2xl space-y-4">
                        <span class="inline-block px-3 py-1 bg-esRed/30 border border-esRed/40 text-red-200 rounded-full text-xs font-semibold tracking-wide uppercase">
                            Platform Pembelajaran SMA
                        </span>
                        <h1 class="text-3xl sm:text-5xl font-bold tracking-tight text-white leading-tight">
                            Eksplorasi Pergerakan Nasional Indonesia
                        </h1>
                        <p class="text-stone-300 text-sm sm:text-base leading-relaxed">
                            “Telusuri perkembangan organisasi, tokoh, dan peristiwa penting yang membentuk perjalanan Pergerakan Nasional Indonesia.”
                        </p>
                        <p class="text-stone-400 text-xs sm:text-sm font-medium">
                            Belajar sejarah melalui eksplorasi, visual, permainan, dan evaluasi.
                        </p>
                        <div class="flex flex-wrap gap-3 pt-2">
                            <button onclick="switchPage('timeline')" class="px-6 py-3 bg-esRed hover:bg-red-700 text-white rounded-2xl font-semibold text-sm transition-all shadow-lg shadow-red-600/30 flex items-center gap-2">
                                Mulai Eksplorasi →
                            </button>
                            <button onclick="switchPage('videos')" class="px-6 py-3 bg-white/10 hover:bg-white/20 border border-white/20 text-white rounded-2xl font-semibold text-sm transition-all backdrop-blur-sm flex items-center gap-2">
                                Tonton Video →
                            </button>
                        </div>
                    </div>
                </div>

                <!-- 4 Statistik -->
                <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
                    <div class="bg-white p-6 rounded-3xl border border-stone-200/80 shadow-sm flex flex-col items-center text-center es-card">
                        <span class="text-3xl mb-1">🏛️</span>
                        <h3 class="text-2xl font-bold text-stone-900">10+</h3>
                        <p class="text-xs text-stone-500 font-medium mt-1">Organisasi</p>
                    </div>
                    <div class="bg-white p-6 rounded-3xl border border-stone-200/80 shadow-sm flex flex-col items-center text-center es-card">
                        <span class="text-3xl mb-1">⏳</span>
                        <h3 class="text-2xl font-bold text-stone-900">8</h3>
                        <p class="text-xs text-stone-500 font-medium mt-1">Peristiwa</p>
                    </div>
                    <div class="bg-white p-6 rounded-3xl border border-stone-200/80 shadow-sm flex flex-col items-center text-center es-card">
                        <span class="text-3xl mb-1">🎬</span>
                        <h3 class="text-2xl font-bold text-stone-900">4</h3>
                        <p class="text-xs text-stone-500 font-medium mt-1">Video Edukasi</p>
                    </div>
                    <div class="bg-white p-6 rounded-3xl border border-stone-200/80 shadow-sm flex flex-col items-center text-center es-card">
                        <span class="text-3xl mb-1">📝</span>
                        <h3 class="text-2xl font-bold text-stone-900">20</h3>
                        <p class="text-xs text-stone-500 font-medium mt-1">Soal Evaluasi</p>
                    </div>
                </div>

                <!-- 4 Quick-Access Cards -->
                <div>
                    <h3 class="font-bold text-lg text-stone-900 mb-4">Akses Cepat</h3>
                    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                        <div onclick="switchPage('timeline')" class="bg-white p-6 rounded-3xl border border-stone-200/80 shadow-sm cursor-pointer es-card group">
                            <div class="w-12 h-12 rounded-2xl bg-amber-50 text-amber-600 flex items-center justify-center text-2xl mb-4 group-hover:scale-110 transition-transform">⏳</div>
                            <h4 class="font-bold text-stone-900 mb-1">Jejak Sejarah</h4>
                            <p class="text-xs text-stone-500">Sistem satu peristiwa interaktif dari Budi Utomo hingga Sumpah Pemuda.</p>
                        </div>
                        <div onclick="switchPage('videos')" class="bg-white p-6 rounded-3xl border border-stone-200/80 shadow-sm cursor-pointer es-card group">
                            <div class="w-12 h-12 rounded-2xl bg-red-50 text-esRed flex items-center justify-center text-2xl mb-4 group-hover:scale-110 transition-transform">🎬</div>
                            <h4 class="font-bold text-stone-900 mb-1">Video Edukasi</h4>
                            <p class="text-xs text-stone-500">Tonton video visual lengkap dengan catatan belajar dan status progress.</p>
                        </div>
                        <div onclick="switchPage('materials')" class="bg-white p-6 rounded-3xl border border-stone-200/80 shadow-sm cursor-pointer es-card group">
                            <div class="w-12 h-12 rounded-2xl bg-blue-50 text-blue-600 flex items-center justify-center text-2xl mb-4 group-hover:scale-110 transition-transform">📚</div>
                            <h4 class="font-bold text-stone-900 mb-1">Materi</h4>
                            <p class="text-xs text-stone-500">Pelajari profil mendalam organisasi dan tokoh pergerakan nasional.</p>
                        </div>
                        <div onclick="switchPage('game')" class="bg-white p-6 rounded-3xl border border-stone-200/80 shadow-sm cursor-pointer es-card group">
                            <div class="w-12 h-12 rounded-2xl bg-emerald-50 text-emerald-600 flex items-center justify-center text-2xl mb-4 group-hover:scale-110 transition-transform">🎮</div>
                            <h4 class="font-bold text-stone-900 mb-1">Game</h4>
                            <p class="text-xs text-stone-500">Uji daya ingat dengan permainan Memory Match organisasi dan tokoh.</p>
                        </div>
                    </div>
                </div>
            </div>

            <!-- 2. JEJAK SEJARAH VIEW -->
            <div id="view-timeline" class="page-view max-w-4xl mx-auto space-y-6">
                <div class="text-center max-w-xl mx-auto space-y-2">
                    <span class="px-3 py-1 bg-red-100 text-esRed rounded-full text-xs font-semibold uppercase tracking-wider">Kronologi Pergerakan</span>
                    <h2 class="text-2xl sm:text-3xl font-bold text-stone-900">Jejak Pergerakan Nasional</h2>
                    <p class="text-stone-500 text-sm">“Telusuri perkembangan pergerakan nasional dari organisasi awal hingga Sumpah Pemuda.”</p>
                </div>

                <!-- One-Event-At-A-Time Card Container -->
                <div class="bg-white rounded-3xl border border-stone-200/80 shadow-md p-6 sm:p-10 relative overflow-hidden transition-all">
                    <div id="timeline-card-content" class="space-y-6">
                        <!-- Dynamic JavaScript injection -->
                    </div>

                    <!-- Navigation controls -->
                    <div class="flex items-center justify-between pt-8 mt-8 border-t border-stone-100">
                        <button onclick="prevTimelineEvent()" id="timeline-prev-btn" class="px-5 py-2.5 bg-stone-100 hover:bg-stone-200 text-stone-700 font-semibold text-sm rounded-2xl transition-all flex items-center gap-2">
                            ← Sebelumnya
                        </button>
                        <span id="timeline-counter" class="text-xs font-bold text-stone-500 uppercase tracking-widest">1 / 8</span>
                        <button onclick="nextTimelineEvent()" id="timeline-next-btn" class="px-5 py-2.5 bg-esRed hover:bg-red-700 text-white font-semibold text-sm rounded-2xl transition-all flex items-center gap-2 shadow-md shadow-red-500/20">
                            Berikutnya →
                        </button>
                    </div>
                </div>
            </div>

            <!-- 3. VIDEO EDUKASI VIEW -->
            <div id="view-videos" class="page-view max-w-6xl mx-auto space-y-8">
                <div class="text-center max-w-xl mx-auto space-y-2">
                    <span class="px-3 py-1 bg-red-100 text-esRed rounded-full text-xs font-semibold uppercase tracking-wider">Media Visual</span>
                    <h2 class="text-2xl sm:text-3xl font-bold text-stone-900">Video Edukasi</h2>
                    <p class="text-stone-500 text-sm">“Pelajari Pergerakan Nasional Indonesia melalui video edukatif yang singkat, visual, dan mudah dipahami.”</p>
                </div>

                <!-- Video Progress Banner -->
                <div class="bg-white p-5 rounded-3xl border border-stone-200/80 shadow-sm flex flex-col sm:flex-row items-center justify-between gap-4">
                    <div class="flex items-center gap-3">
                        <div class="w-10 h-10 rounded-2xl bg-red-50 text-esRed flex items-center justify-center font-bold text-lg">🎬</div>
                        <div>
                            <h4 class="font-bold text-sm text-stone-900">Progress Video Pembelajaran</h4>
                            <p id="video-progress-count" class="text-xs text-stone-500">0 / 4 video selesai</p>
                        </div>
                    </div>
                    <div class="w-full sm:w-64 bg-stone-200 rounded-full h-3 overflow-hidden">
                        <div id="video-progress-bar" class="bg-esRed h-full rounded-full transition-all duration-500" style="width: 0%"></div>
                    </div>
                </div>

                <!-- Main Active Video Player -->
                <div class="bg-white rounded-3xl border border-stone-200/80 shadow-md overflow-hidden p-6 sm:p-8 space-y-6">
                    <div class="relative w-full aspect-video bg-stone-900 rounded-2xl overflow-hidden shadow-inner flex items-center justify-center">
                        <video id="main-video-player" class="w-full h-full object-cover" controls onended="handleVideoEnded()">
                            Your browser does not support the video tag.
                        </video>
                        <div id="video-fallback-msg" class="absolute inset-0 flex flex-col items-center justify-center bg-stone-900/90 text-white p-6 text-center hidden">
                            <span class="text-4xl mb-2">🎬</span>
                            <h4 class="font-bold text-lg mb-1">Video Belum Tersedia</h4>
                            <p class="text-xs text-stone-400 max-w-md">“Tambahkan file video ke folder videos untuk memutar video ini atau gunakan tombol putar di bawah.”</p>
                        </div>
                    </div>

                    <div class="flex flex-col sm:flex-row sm:items-center justify-between gap-4 pb-6 border-b border-stone-100">
                        <div>
                            <div class="flex items-center gap-2 mb-1">
                                <span id="main-video-category" class="px-2.5 py-0.5 bg-red-50 text-esRed rounded-full text-xs font-semibold">Pengantar</span>
                                <span id="main-video-duration" class="text-xs text-stone-400 font-medium">05:30</span>
                                <span id="main-video-status-badge" class="px-2.5 py-0.5 bg-stone-100 text-stone-600 rounded-full text-xs font-semibold">Belum ditonton</span>
                            </div>
                            <h3 id="main-video-title" class="text-xl font-bold text-stone-900">Pergerakan Nasional Indonesia</h3>
                        </div>
                        <button id="related-material-btn" onclick="openMaterialFromVideo()" class="px-4 py-2 bg-stone-100 hover:bg-stone-200 text-stone-700 font-semibold text-xs rounded-xl transition-all flex items-center gap-2 self-start sm:self-auto">
                            Pelajari Materi Terkait →
                        </button>
                    </div>

                    <p id="main-video-desc" class="text-sm text-stone-600 leading-relaxed">Mengenal perkembangan Pergerakan Nasional Indonesia.</p>

                    <!-- Catatan Belajar -->
                    <div class="bg-stone-50 p-5 rounded-2xl border border-stone-200/60 space-y-3">
                        <h4 class="font-bold text-sm text-stone-900 flex items-center gap-2">
                            <span>📝</span> Catatan Belajar
                        </h4>
                        <textarea id="video-notes-input" placeholder="Apa yang kamu pelajari dari video ini?" class="w-full p-3 bg-white border border-stone-200 rounded-xl text-sm focus:outline-none focus:ring-2 focus:ring-esRed/50 resize-none h-24"></textarea>
                        <div class="flex justify-end">
                            <button onclick="saveVideoNotes()" class="px-4 py-2 bg-esRed hover:bg-red-700 text-white font-semibold text-xs rounded-xl transition-all shadow-sm">
                                Simpan Catatan
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Search and Filters -->
                <div class="space-y-4">
                    <div class="flex flex-col sm:flex-row gap-3 items-center justify-between">
                        <div class="relative w-full sm:w-80">
                            <span class="absolute left-3.5 top-3 text-stone-400">🔍</span>
                            <input type="text" id="video-search-input" oninput="filterVideos()" placeholder="Cari video berdasarkan judul, kategori, tokoh..." class="w-full pl-10 pr-4 py-2.5 bg-white border border-stone-200 rounded-2xl text-sm focus:outline-none focus:ring-2 focus:ring-esRed/50">
                        </div>
                        <div class="flex flex-wrap gap-1.5 w-full sm:w-auto" id="video-filter-buttons">
                            <button onclick="setVideoFilter('Semua')" class="video-filter-btn px-4 py-2 rounded-xl text-xs font-semibold bg-esRed text-white transition-all">Semua</button>
                            <button onclick="setVideoFilter('Pengantar')" class="video-filter-btn px-4 py-2 rounded-xl text-xs font-semibold bg-white text-stone-600 border border-stone-200 hover:bg-stone-50 transition-all">Pengantar</button>
                            <button onclick="setVideoFilter('Organisasi')" class="video-filter-btn px-4 py-2 rounded-xl text-xs font-semibold bg-white text-stone-600 border border-stone-200 hover:bg-stone-50 transition-all">Organisasi</button>
                            <button onclick="setVideoFilter('Peristiwa')" class="video-filter-btn px-4 py-2 rounded-xl text-xs font-semibold bg-white text-stone-600 border border-stone-200 hover:bg-stone-50 transition-all">Peristiwa</button>
                        </div>
                    </div>

                    <!-- Video Playlist Cards Grid -->
                    <div id="video-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
                        <!-- Dynamic generated cards -->
                    </div>
                    <div id="video-not-found" class="hidden text-center py-12 text-stone-400 text-sm font-medium">
                        Video tidak ditemukan.
                    </div>
                </div>
            </div>

            <!-- 4. MATERI VIEW -->
            <div id="view-materials" class="page-view max-w-6xl mx-auto space-y-6">
                <div class="text-center max-w-xl mx-auto space-y-2">
                    <span class="px-3 py-1 bg-red-100 text-esRed rounded-full text-xs font-semibold uppercase tracking-wider">Pustaka Organisasi</span>
                    <h2 class="text-2xl sm:text-3xl font-bold text-stone-900">Organisasi Pergerakan Nasional</h2>
                    <p class="text-stone-500 text-sm">“Pelajari profil mendalam organisasi dan tokoh penting dalam pergerakan nasional.”</p>
                </div>

                <!-- Search and Filters -->
                <div class="space-y-4">
                    <div class="flex flex-col sm:flex-row gap-3 items-center justify-between">
                        <div class="relative w-full sm:w-80">
                            <span class="absolute left-3.5 top-3 text-stone-400">🔍</span>
                            <input type="text" id="material-search-input" oninput="filterMaterials()" placeholder="Cari organisasi..." class="w-full pl-10 pr-4 py-2.5 bg-white border border-stone-200 rounded-2xl text-sm focus:outline-none focus:ring-2 focus:ring-esRed/50">
                        </div>
                        <div class="flex flex-wrap gap-1.5 w-full sm:w-auto" id="material-filter-buttons">
                            <button onclick="setMaterialFilter('Semua')" class="material-filter-btn px-4 py-2 rounded-xl text-xs font-semibold bg-esRed text-white transition-all">Semua</button>
                            <button onclick="setMaterialFilter('Politik')" class="material-filter-btn px-4 py-2 rounded-xl text-xs font-semibold bg-white text-stone-600 border border-stone-200 hover:bg-stone-50 transition-all">Politik</button>
                            <button onclick="setMaterialFilter('Sosial')" class="material-filter-btn px-4 py-2 rounded-xl text-xs font-semibold bg-white text-stone-600 border border-stone-200 hover:bg-stone-50 transition-all">Sosial</button>
                            <button onclick="setMaterialFilter('Pendidikan')" class="material-filter-btn px-4 py-2 rounded-xl text-xs font-semibold bg-white text-stone-600 border border-stone-200 hover:bg-stone-50 transition-all">Pendidikan</button>
                            <button onclick="setMaterialFilter('Pemuda')" class="material-filter-btn px-4 py-2 rounded-xl text-xs font-semibold bg-white text-stone-600 border border-stone-200 hover:bg-stone-50 transition-all">Pemuda</button>
                        </div>
                    </div>

                    <!-- Material Cards Grid -->
                    <div id="material-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4">
                        <!-- Dynamic generated cards -->
                    </div>
                    <div id="material-not-found" class="hidden text-center py-12 text-stone-400 text-sm font-medium">
                        Organisasi tidak ditemukan.
                    </div>
                </div>
            </div>

            <!-- 5. GAME VIEW -->
            <div id="view-game" class="page-view max-w-4xl mx-auto space-y-6">
                <div class="text-center max-w-xl mx-auto space-y-2">
                    <span class="px-3 py-1 bg-emerald-100 text-emerald-700 rounded-full text-xs font-semibold uppercase tracking-wider">Mini Game Edukasi</span>
                    <h2 class="text-2xl sm:text-3xl font-bold text-stone-900">Memory Match Organisasi & Tokoh</h2>
                    <p class="text-stone-500 text-sm">“Pasangkan organisasi pergerakan nasional dengan tokoh pendirinya dengan tepat!”</p>
                </div>

                <!-- Game Stats Bar -->
                <div class="bg-white p-5 rounded-3xl border border-stone-200/80 shadow-sm flex flex-wrap items-center justify-between gap-4">
                    <div class="flex items-center gap-6">
                        <div>
                            <span class="text-xs text-stone-400 font-medium block">Percobaan</span>
                            <span id="game-attempts" class="font-bold text-lg text-stone-900">0</span>
                        </div>
                        <div>
                            <span class="text-xs text-stone-400 font-medium block">Pasangan</span>
                            <span id="game-matches" class="font-bold text-lg text-esRed">0 / 6</span>
                        </div>
                        <div>
                            <span class="text-xs text-stone-400 font-medium block">Waktu</span>
                            <span id="game-timer" class="font-bold text-lg text-stone-900">00:00</span>
                        </div>
                    </div>
                    <button onclick="initGame()" class="px-4 py-2 bg-stone-100 hover:bg-stone-200 text-stone-700 font-semibold text-xs rounded-xl transition-all flex items-center gap-2">
                        🔄 Reset Game
                    </button>
                </div>

                <!-- Game Cards Grid (12 Cards) -->
                <div id="game-board" class="grid grid-cols-3 sm:grid-cols-4 gap-3 sm:gap-4">
                    <!-- Dynamic cards -->
                </div>

                <!-- Game Complete Modal Overlay inside view -->
                <div id="game-win-banner" class="hidden bg-emerald-50 border border-emerald-200 p-6 rounded-3xl text-center space-y-4 shadow-sm">
                    <span class="text-4xl">🎉</span>
                    <h3 class="text-xl font-bold text-emerald-900">Permainan Selesai!</h3>
                    <p id="game-win-stats" class="text-sm text-emerald-700">Kamu berhasil menyelesaikan permainan dalam waktu 00:45 dengan 12 percobaan.</p>
                    <div class="flex justify-center gap-3 pt-2">
                        <button onclick="initGame()" class="px-5 py-2.5 bg-emerald-600 hover:bg-emerald-700 text-white font-semibold text-xs rounded-2xl transition-all shadow-md">
                            Coba Lagi
                        </button>
                        <button onclick="switchPage('evaluation')" class="px-5 py-2.5 bg-esRed hover:bg-red-700 text-white font-semibold text-xs rounded-2xl transition-all shadow-md">
                            Lanjut ke Evaluasi →
                        </button>
                    </div>
                </div>
            </div>

            <!-- 6. EVALUASI VIEW -->
            <div id="view-evaluation" class="page-view max-w-3xl mx-auto space-y-6">
                <div class="text-center max-w-xl mx-auto space-y-2">
                    <span class="px-3 py-1 bg-red-100 text-esRed rounded-full text-xs font-semibold uppercase tracking-wider">Uji Kompetensi</span>
                    <h2 class="text-2xl sm:text-3xl font-bold text-stone-900">Evaluasi Pembelajaran</h2>
                    <p class="text-stone-500 text-sm">“Uji pemahamanmu mengenai materi Pergerakan Nasional Indonesia melalui 20 soal pilihan ganda.”</p>
                </div>

                <!-- Quiz Container -->
                <div id="quiz-container" class="bg-white rounded-3xl border border-stone-200/80 shadow-md p-6 sm:p-10 space-y-6">
                    <div class="flex items-center justify-between pb-4 border-b border-stone-100">
                        <span id="quiz-counter" class="text-xs font-bold text-esRed uppercase tracking-widest">PERTANYAAN 01 / 20</span>
                        <div class="w-32 bg-stone-200 rounded-full h-2 overflow-hidden">
                            <div id="quiz-progress-bar" class="bg-esRed h-full transition-all duration-300 rounded-full" style="width: 5%"></div>
                        </div>
                    </div>

                    <h3 id="quiz-question" class="text-lg sm:text-xl font-bold text-stone-900 leading-snug">
                        <!-- Question text -->
                    </h3>

                    <div id="quiz-options" class="space-y-3">
                        <!-- Option buttons -->
                    </div>

                    <div id="quiz-feedback" class="hidden p-4 rounded-2xl text-sm font-medium">
                        <!-- Feedback message -->
                    </div>

                    <div class="flex justify-end pt-4 border-t border-stone-100">
                        <button id="quiz-next-btn" onclick="nextQuizQuestion()" class="hidden px-6 py-3 bg-esRed hover:bg-red-700 text-white font-semibold text-sm rounded-2xl transition-all shadow-md shadow-red-500/20">
                            Soal Berikutnya →
                        </button>
                    </div>
                </div>

                <!-- Result Screen -->
                <div id="quiz-result-card" class="hidden bg-white rounded-3xl border border-stone-200/80 shadow-md p-8 sm:p-12 text-center space-y-6">
                    <div class="w-20 h-20 bg-red-50 text-esRed rounded-3xl mx-auto flex items-center justify-center text-3xl font-bold shadow-sm">
                        🎓
                    </div>
                    <div>
                        <span class="px-3 py-1 bg-red-100 text-esRed rounded-full text-xs font-semibold uppercase tracking-wider">Hasil Evaluasi</span>
                        <h3 class="text-2xl font-bold text-stone-900 mt-2">EVALUASI SELESAI</h3>
                    </div>

                    <div class="flex justify-center items-center gap-8 py-4">
                        <div class="text-center">
                            <span class="text-3xl font-bold text-esRed" id="result-score">85</span>
                            <span class="text-xs text-stone-400 block font-medium mt-1">Nilai Akhir</span>
                        </div>
                        <div class="w-px h-12 bg-stone-200"></div>
                        <div class="text-center">
                            <span class="text-3xl font-bold text-emerald-600" id="result-correct">17</span>
                            <span class="text-xs text-stone-400 block font-medium mt-1">Benar</span>
                        </div>
                        <div class="w-px h-12 bg-stone-200"></div>
                        <div class="text-center">
                            <span class="text-3xl font-bold text-red-500" id="result-wrong">3</span>
                            <span class="text-xs text-stone-400 block font-medium mt-1">Salah</span>
                        </div>
                    </div>

                    <p id="result-feedback-text" class="text-sm text-stone-600 max-w-md mx-auto leading-relaxed">
                        Luar biasa! Pemahamanmu tentang sejarah pergerakan nasional sudah sangat baik. Pertahankan prestasimu!
                    </p>

                    <div class="flex flex-wrap justify-center gap-3 pt-4">
                        <button onclick="restartEvaluation()" class="px-5 py-3 bg-stone-100 hover:bg-stone-200 text-stone-700 font-semibold text-xs rounded-2xl transition-all">
                            Ulangi Evaluasi
                        </button>
                        <button onclick="switchPage('materials')" class="px-5 py-3 bg-stone-100 hover:bg-stone-200 text-stone-700 font-semibold text-xs rounded-2xl transition-all">
                            Pelajari Materi Lagi
                        </button>
                        <button onclick="switchPage('home')" class="px-5 py-3 bg-esRed hover:bg-red-700 text-white font-semibold text-xs rounded-2xl transition-all shadow-md">
                            Kembali ke Beranda
                        </button>
                    </div>
                </div>
            </div>

            <!-- 7. ABOUT VIEW -->
            <div id="view-about" class="page-view max-w-3xl mx-auto space-y-6">
                <div class="bg-white rounded-3xl border border-stone-200/80 shadow-md p-8 sm:p-12 space-y-6">
                    <div class="flex items-center gap-4">
                        <div class="w-14 h-14 rounded-2xl bg-gradient-to-tr from-esRed to-esDarkRed flex items-center justify-center text-white font-bold text-2xl shadow-md shadow-red-500/20">
                            Es
                        </div>
                        <div>
                            <h2 class="text-2xl font-bold text-stone-900">Tentang EsHist</h2>
                            <p class="text-xs text-stone-500 font-medium">Eksplorasi Sejarah Interaktif • Versi 1.0</p>
                        </div>
                    </div>

                    <div class="space-y-4 text-sm text-stone-600 leading-relaxed">
                        <p>
                            “EsHist merupakan media pembelajaran sejarah interaktif yang dirancang untuk membantu siswa memahami Pergerakan Nasional Indonesia melalui timeline, video edukasi, materi, permainan, dan evaluasi.”
                        </p>
                        <div class="bg-stone-50 p-6 rounded-2xl border border-stone-200/60 space-y-2">
                            <h4 class="font-bold text-stone-900">Tujuan Pembelajaran</h4>
                            <p class="text-stone-600">
                                “Menghadirkan pengalaman belajar sejarah yang interaktif, visual, dan mudah dipahami oleh siswa SMA tanpa ketergantungan koneksi internet atau server eksternal.”
                            </p>
                        </div>
                        <div class="pt-2">
                            <h4 class="font-bold text-stone-900 mb-2">Fitur Utama EsHist:</h4>
                            <ul class="list-disc list-inside space-y-1 text-stone-600">
                                <li>Sistem Navigasi Dashboard Satu Halaman (Single-Page Navigation)</li>
                                <li>Jejak Sejarah Interaktif Satu Peristiwa (One-Event-At-A-Time)</li>
                                <li>Video Edukasi Lengkap dengan Status Tontonan & Catatan Belajar</li>
                                <li>Pustaka Organisasi Pergerakan dengan Filter & Modal Detail</li>
                                <li>Mini Game Memory Match Organisasi dan Tokoh</li>
                                <li>Evaluasi 20 Soal Pilihan Ganda dengan Penilaian Otomatis</li>
                                <li>Sistem Progress Keseluruhan & Penyimpanan Offline (localStorage)</li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>

        </div>
    </main>

    <!-- MATERIAL DETAIL MODAL -->
    <div id="material-modal" class="fixed inset-0 bg-stone-900/60 backdrop-blur-sm z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-white rounded-3xl max-w-2xl w-full max-h-[90vh] overflow-y-auto shadow-2xl p-6 sm:p-8 space-y-6 relative">
            <button onclick="closeMaterialModal()" class="absolute top-6 right-6 w-10 h-10 rounded-full bg-stone-100 hover:bg-stone-200 flex items-center justify-center text-stone-600 font-bold transition-all">
                ✕
            </button>
            <div id="modal-content" class="space-y-4">
                <!-- Dynamic modal content -->
            </div>
        </div>
    </div>

    <script>
        // --- DATA STRUCTURES & CONFIGURATIONS ---

        // 1. Timeline Data (8 Events)
        const timelineEvents = [
            {
                year: "1908",
                title: "Budi Utomo",
                category: "Organisasi Modern Pertama",
                description: "Didirikan oleh para mahasiswa STOVIA di Jakarta atas prakarsa Dr. Soetomo. Organisasi ini menandai kebangkitan kesadaran nasional bangsa Indonesia untuk berorganisasi secara modern.",
                 tokoh: "Dr. Soetomo, Goenawan Mangoenkoesoemo, Soeradji",
                icon: "🏛️"
            },
            {
                year: "1911–1912",
                title: "Sarekat Islam",
                category: "Gerakan Sosial & Ekonomi",
                description: "Berawal dari Sarekat Dagang Islam yang didirikan oleh H. Samanhudi di Surakarta untuk melindungi pedagang pribumi. Kemudian bertransformasi menjadi Sarekat Islam di bawah H.O.S. Tjokroaminoto.",
                tokoh: "H. Samanhudi, H.O.S. Tjokroaminoto",
                icon: "📜"
            },
            {
                year: "1912",
                title: "Indische Partij",
                category: "Partai Politik Nasionalis",
                description: "Partai politik pertama berhaluan nasionalis Hindia yang didirikan oleh Tiga Serangkai. Menuntut kemerdekaan Hindia Belanda dari kolonialisme secara terang-terangan.",
                tokoh: "E.F.E. Douwes Dekker, Tjipto Mangoenkoesoemo, Ki Hadjar Dewantara",
                icon: "⭐"
            },
            {
                year: "1912",
                title: "Muhammadiyah",
                category: "Gerakan Sosial & Keagamaan",
                description: "Didirikan oleh K.H. Ahmad Dahlan di Yogyakarta. Bergerak di bidang pendidikan, sosial, dan keagamaan untuk memajukan umat Islam dan bangsa Indonesia.",
                tokoh: "K.H. Ahmad Dahlan",
                icon: "📖"
            },
            {
                year: "1922",
                title: "Taman Siswa",
                category: "Gerakan Pendidikan",
                description: "Perguruan Nasional Taman Siswa didirikan oleh Ki Hadjar Dewantara di Yogyakarta. Menanamkan semangat nasionalisme dan pendidikan merdeka bagi pribumi.",
                tokoh: "Ki Hadjar Dewantara",
                icon: "🎓"
            },
            {
                year: "1925",
                title: "Perhimpunan Indonesia",
                category: "Gerakan Mahasiswa di Belanda",
                description: "Organisasi mahasiswa pelajar Indonesia di negeri Belanda yang memperjuangkan kemerdekaan Indonesia di kancah internasional dan memelopori penggunaan nama 'Indonesia'.",
                tokoh: "Mohammad Hatta, Iwa Kusuma Soemantri, Ali Sastroamidjojo",
                icon: "🌍"
            },
            {
                year: "1927",
                title: "Partai Nasional Indonesia (PNI)",
                category: "Partai Politik Radikal",
                description: "Didirikan oleh Ir. Soekarno di Bandung dengan asas marhaenisme dan tujuan mencapai Indonesia merdeka secara non-kooperatif terhadap pemerintah kolonial.",
                tokoh: "Ir. Soekarno, Mr. Iskaq Tjokrohadisurjo",
                icon: "✊"
            },
            {
                year: "1928",
                title: "Sumpah Pemuda",
                category: "Tonggak Persatuan Nasional",
                description: "Kongres Pemuda II melahirkan ikrar monumental: Satu Nusa, Satu Bangsa, dan Menjunjung Bahasa Persatuan, Bahasa Indonesia. Menyatukan seluruh elemen pemuda Nusantara.",
                tokoh: "Soegondo Djojopoesposo, Mohammad Yamin, Wage Rudolf Soepratman",
                icon: "🇮🇩"
            }
        ];

        // 2. Video Data (4 Videos)
        // // TAMBAHKAN VIDEO BARU DI SINI
        const videos = [
            {
                id: 1,
                title: "Pergerakan Nasional Indonesia",
                category: "Pengantar",
                duration: "05:30",
                file: "videos/pergerakan-nasional.mp4",
                description: "Mengenal perkembangan awal Pergerakan Nasional Indonesia, faktor pemicu dari dalam dan luar negeri, serta babak baru perjuangan bangsa."
            },
            {
                id: 2,
                title: "Budi Utomo dan Awal Kebangkitan Nasional",
                category: "Organisasi",
                duration: "04:20",
                file: "videos/budi-utomo.mp4",
                description: "Kisah berdirinya Budi Utomo pada 20 Mei 1908 oleh para pelajar STOVIA yang menjadi tonggak Hari Kebangkitan Nasional."
            },
            {
                id: 3,
                title: "Organisasi Pergerakan Nasional",
                category: "Organisasi",
                duration: "06:10",
                file: "videos/organisasi-pergerakan.mp4",
                description: "Eksplorasi mendalam berbagai organisasi pergerakan dari Sarekat Islam, Indische Partij, Muhammadiyah, hingga PNI."
            },
            {
                id: 4,
                title: "Sumpah Pemuda 1928",
                category: "Peristiwa",
                duration: "05:00",
                file: "videos/sumpah-pemuda.mp4",
                description: "Momen bersejarah Kongres Pemuda II yang melahirkan ikrar Sumpah Pemuda serta lagu kebangsaan Indonesia Raya."
            }
        ];

        // 3. Materials / Organizations Data (10 Organizations)
        const materials = [
            {
                id: "budi-utomo",
                name: "Budi Utomo",
                category: "Pendidikan",
                year: "1908",
                background: "Didirikan di Jakarta oleh para mahasiswa STOVIA atas dorongan Dr. Wahidin Soedirohoesodo yang berkeliling menggalang dana Beasiswa Fonds.",
                tujuan: "Memajukan pengajaran dan kebudayaan bagi bangsa bumiputera.",
                tokoh: "Dr. Soetomo, Dr. Wahidin Soedirohoesodo, Goenawan Mangoenkoesoemo",
                peran: "Menjadi pelopor kebangkitan nasional dan mengubah bentuk perjuangan dari fisik menjadi organisasi modern.",
                fakta: "Tanggal berdirinya Budi Utomo (20 Mei) kemudian ditetapkan sebagai Hari Kebangkitan Nasional."
            },
            {
                id: "sarekat-islam",
                name: "Sarekat Islam",
                category: "Ekonomi",
                year: "1911",
                background: "Transformasi dari Sarekat Dagang Islam yang didirikan H. Samanhudi di Surakarta untuk menghadapi dominasi pedagang asing.",
                tujuan: "Melindungi kepentingan pedagang pribumi dan memajukan perekonomian Islam serta kemerdekaan bangsa.",
                tokoh: "H. Samanhudi, H.O.S. Tjokroaminoto, Abdul Muis",
                peran: "Menjadi organisasi massa terbesar pertama yang memiliki jutaan anggota dari berbagai lapisan masyarakat.",
                fakta: "H.O.S. Tjokroaminoto dikenal sebagai 'Raja Tanpa Mahkota' yang membimbing banyak tokoh besar."
            },
            {
                id: "indische-partij",
                name: "Indische Partij",
                category: "Politik",
                year: "1912",
                background: "Didirikan di Bandung oleh Tiga Serangkai sebagai partai politik pertama yang secara tegas bercorak nasionalis Hindia.",
                tujuan: "Membangun kesadaran nasional Hindia dan mencapai kemerdekaan dari penjajahan Belanda.",
                tokoh: "E.F.E. Douwes Dekker, Tjipto Mangoenkoesoemo, Ki Hadjar Dewantara",
                peran: "Menanamkan gagasan nasionalisme radikal dan anti-kolonial tanpa memandang ras.",
                fakta: "Ketiga pendirinya pernah diasingkan oleh pemerintah kolonial Belanda ke negeri Belanda."
            },
            {
                id: "muhammadiyah",
                name: "Muhammadiyah",
                category: "Sosial",
                year: "1912",
                background: "Didirikan oleh K.H. Ahmad Dahlan di Yogyakarta melalui pemahaman Islam yang berkemajuan dan modern.",
                tujuan: "Menyebarkan ajaran Islam serta memajukan pendidikan dan kesejahteraan sosial masyarakat.",
                tokoh: "K.H. Ahmad Dahlan, Nyai Ahmad Dahlan",
                peran: "Mendirikan ribuan sekolah, rumah sakit, dan panti asuhan modern di seluruh Nusantara.",
                fakta: "Menggabungkan sistem pendidikan agama tradisional dengan pendidikan umum gaya barat."
            },
            {
                id: "taman-siswa",
                name: "Taman Siswa",
                category: "Pendidikan",
                year: "1922",
                background: "Didirikan oleh Ki Hadjar Dewantara di Yogyakarta sebagai perlawanan terhadap sistem pendidikan kolonial yang diskriminatif.",
                tujuan: "Menyelenggarakan pendidikan nasional yang merdeka bagi kaum pribumi berbasis kebudayaan bangsa.",
                tokoh: "Ki Hadjar Dewantara",
                peran: "Melahirkan konsep kepemimpinan 'Ing Ngarsa Sung Tuladha, Ing Madya Mangun Karsa, Tut Wuri Handayani'.",
                fakta: "Menjadi wadah pendidikan merdeka yang menolak subsidi dari pemerintah kolonial."
            },
            {
                id: "perhimpunan-indonesia",
                name: "Perhimpunan Indonesia",
                category: "Politik",
                year: "1925",
                background: "Transformasi dari Indische Vereeniging yang digerakkan oleh para mahasiswa Indonesia di Belanda.",
                tujuan: "Memperjuangkan kemerdekaan Indonesia dan menyuarakan suara rakyat tertindas di kancah internasional.",
                tokoh: "Mohammad Hatta, Iwa Kusuma Soemantri, Ali Sastroamidjojo",
                peran: "Memelopori penggunaan nama 'Indonesia' dan 'Bahasa Indonesia' dalam setiap manifesto perjuangan.",
                fakta: "Mohammad Hatta sempat ditangkap dan diadili di Den Haag sebelum akhirnya dibebaskan."
            },
            {
                id: "pni",
                name: "Partai Nasional Indonesia",
                category: "Politik",
                year: "1927",
                background: "Didirikan di Bandung oleh Ir. Soekarno dan para kaum intelektual Algemeene Studieclub.",
                tujuan: "Mencapai Indonesia merdeka dengan asas self-help (menolong diri sendiri) dan non-kooperasi.",
                tokoh: "Ir. Soekarno, Mr. Iskaq Tjokrohadisurjo, Mr. Anwari",
                peran: "Menyatukan berbagai golongan dalam satu front nasional radikal anti-kolonial.",
                fakta: "Ir. Soekarno menyampaikan pembelaan fenomenal berjudul 'Indonesia Menggugat' di pengadilan Bandung."
            },
            {
                id: "jong-java",
                name: "Jong Java",
                category: "Pemuda",
                year: "1915",
                background: "Didirikan oleh Satiman Wirjosandjojo dengan nama awal Tri Koro Dharmo di Gedung STOVIA Jakarta.",
                tujuan: "Membangun persatuan di antara para pemuda pelajar asal Jawa, Sunda, Madura, dan Bali.",
                tokoh: "Satiman Wirjosandjojo, Wongsonegoro",
                peran: "Menjadi wadah pelatihan kepemimpinan pemuda yang kemudian melebur dalam Sumpah Pemuda.",
                fakta: "Nama Jong Java resmi digunakan dalam Kongres Pertama di Solo tahun 1918."
            },
            {
                id: "jong-sumatranen-bond",
                name: "Jong Sumatranen Bond",
                category: "Pemuda",
                year: "1917",
                background: "Organisasi pemuda pelajar asal pulau Sumatra yang didirikan di Jakarta.",
                tujuan: "Mempererat tali persaudaraan antar pelajar Sumatra dan memajukan kebudayaan.",
                tokoh: "Mohammad Hatta, A.K. Gani, Mohammad Yamin",
                peran: "Melahirkan tokoh-tokoh penting yang memprakarsai Sumpah Pemuda 1928.",
                fakta: "Mohammad Yamin aktif sebagai pengurus dan sastrawan terkemuka dalam organisasi ini."
            },
            {
                id: "nahdlatul-ulama",
                name: "Nahdlatul Ulama (NU)",
                category: "Sosial",
                year: "1926",
                background: "Didirikan di Surabaya oleh para ulama besar pesantren atas prakarsa K.H. Hasyim Asy'ari.",
                tujuan: "Menjaga ajaran Islam Ahlussunnah wal Jamaah serta memajukan pendidikan dan kebangsaan.",
                tokoh: "K.H. Hasyim Asy'ari, K.H. Wahab Chasbullah, K.H. Bisri Sansuri",
                peran: "Menggerakkan perlawanan religius dan nasionalis dari basis kaum santri dan pesantren.",
                fakta: "Lambang bumi bintang sembilan diciptakan oleh K.H. Ridwan Abdullah."
            }
        ];

        // 4. Evaluation Quiz Data (20 Questions)
        const quizQuestions = [
            {
                question: "Organisasi modern pertama di Indonesia yang didirikan pada tanggal 20 Mei 1908 adalah...",
                options: ["A. Sarekat Islam", "B. Budi Utomo", "C. Indische Partij", "D. Taman Siswa"],
                answer: 1
            },
            {
                question: "Tokoh yang memprakarsai berdirinya Budi Utomo dan mahasiswa STOVIA adalah...",
                options: ["A. Dr. Soetomo", "B. H. Samanhudi", "C. Ki Hadjar Dewantara", "D. Douwes Dekker"],
                answer: 0
            },
            {
                question: "Tanggal berdirinya Budi Utomo (20 Mei) diperingati setiap tahun sebagai...",
                options: ["A. Hari Sumpah Pemuda", "B. Hari Pahlawan", "C. Hari Kebangkitan Nasional", "D. Hari Kemerdekaan"],
                answer: 2
            },
            {
                question: "Sarekat Dagang Islam (SDI) yang kemudian menjadi Sarekat Islam didirikan oleh...",
                options: ["A. H.O.S. Tjokroaminoto", "B. H. Samanhudi", "C. K.H. Ahmad Dahlan", "D. Ir. Soekarno"],
                answer: 1
            },
            {
                question: "Tiga Serangkai (Douwes Dekker, Tjipto Mangoenkoesoemo, Ki Hadjar Dewantara) mendirikan partai bernama...",
                options: ["A. PNI", "B. Indische Partij", "C. Perhimpunan Indonesia", "D. Gerindo"],
                answer: 1
            },
            {
                question: "Muhammadiyah sebagai organisasi sosial dan keagamaan didirikan di Yogyakarta pada tahun 1912 oleh...",
                options: ["A. K.H. Ahmad Dahlan", "B. K.H. Hasyim Asy'ari", "C. K.H. Wahab Chasbullah", "D. Buya Hamka"],
                answer: 0
            },
            {
                question: "Ki Hadjar Dewantara mendirikan Perguruan Nasional Taman Siswa pada tahun...",
                options: ["A. 1918", "B. 1920", "C. 1922", "D. 1928"],
                answer: 2
            },
            {
                question: "Semboyan kepemimpinan 'Tut Wuri Handayani' dicetuskan oleh...",
                options: ["A. Ir. Soekarno", "B. Mohammad Hatta", "C. Ki Hadjar Dewantara", "D. Douwes Dekker"],
                answer: 2
            },
            {
                question: "Organisasi pelajar Indonesia di negeri Belanda yang memperjuangkan kemerdekaan di kancah internasional adalah...",
                options: ["A. Jong Java", "B. Perhimpunan Indonesia", "C. Indische Vereeniging", "D. PNI Pelajar"],
                answer: 1
            },
            {
                question: "Tokoh utama Perhimpunan Indonesia yang terkenal dengan pledoi 'Indonesia Merdeka' di pengadilan Den Haag adalah...",
                options: ["A. Mohammad Hatta", "B. Soekarno", "C. Sutan Sjahrir", "D. Tan Malaka"],
                answer: 0
            },
            {
                question: "Partai Nasional Indonesia (PNI) didirikan di Bandung pada tahun 1927 oleh...",
                options: ["A. Ir. Soekarno", "B. Mohammad Yamin", "C. Amir Sjarifuddin", "D. Ali Sastroamidjojo"],
                answer: 0
            },
            {
                question: "Pledoi atau pembelaan terkenal yang disampaikan Ir. Soekarno di muka pengadilan Landraad Bandung berjudul...",
                options: ["A. Indonesia Menggugat", "B. Mencapai Indonesia Merdeka", "C. Indonesia Merdeka", "D. Mentjapai Indonesia Merdeka"],
                answer: 0
            },
            {
                question: "Kongres Pemuda II yang melahirkan Sumpah Pemuda diselenggarakan pada tanggal...",
                options: ["A. 20 Mei 1908", "B. 28 Oktober 1928", "C. 17 Agustus 1945", "D. 1 Juni 1945"],
                answer: 1
            },
            {
                question: "Ketua panitia Kongres Pemuda II tahun 1928 adalah...",
                options: ["A. Mohammad Yamin", "B. Soegondo Djojopoesposo", "C. Wage Rudolf Soepratman", "D. Amir Sjarifuddin"],
                answer: 1
            },
            {
                question: "Lagu kebangsaan Indonesia Raya pertama kali diperdengarkan secara instrumental pada Kongres Pemuda II oleh...",
                options: ["A. Ismail Marzuki", "B. Wage Rudolf Soepratman", "C. Kusbini", "D. C. Simanjuntak"],
                answer: 1
            },
            {
                question: "Nahdlatul Ulama (NU) didirikan di Surabaya pada tahun 1926 atas prakarsa ulama besar bernama...",
                options: ["A. K.H. Ahmad Dahlan", "B. K.H. Hasyim Asy'ari", "C. K.H. Mas Mansur", "D. K.H. Wahid Hasyim"],
                answer: 1
            },
            {
                question: "Organisasi pemuda pelajar yang pertama kali berdiri (1915) dengan nama awal Tri Koro Dharmo adalah...",
                options: ["A. Jong Sumatranen Bond", "B. Jong Java", "C. Jong Ambon", "D. Jong Celebes"],
                answer: 1
            },
            {
                question: "Asas perjuangan PNI dalam mencapai kemerdekaan Indonesia adalah...",
                options: ["A. Koperasi dengan Belanda", "B. Self-help (menolong diri sendiri) dan Non-kooperasi", "C. Volksraad", "D. Parlementer kolonial"],
                answer: 1
            },
            {
                question: "Faktor internal yang mendorong lahirnya Pergerakan Nasional Indonesia adalah...",
                options: ["A. Kemenangan Jepang atas Rusia", "B. Munculnya golongan intelektual pribumi terpelajar", "C. Kebijakan Etis kolonial", "D. Gerakan nasionalisme di India dan Tiongkok"],
                answer: 1
            },
            {
                question: "Politik Etis (Balas Budi) yang diterapkan Belanda dan secara tidak langsung memicu pergerakan nasional dicetuskan oleh...",
                options: ["A. Van Deventer", "B. J.P. Coen", "C. Daendels", "D. Raffles"],
                answer: 0
            }
        ];

        // --- APPLICATION STATE VARIABLES ---
        let currentTimelineIndex = 0;
        let activeVideoId = 1;
        let currentVideoFilter = "Semua";
        let currentMaterialFilter = "Semua";

        // Game State
        let gameCards = [];
        let flippedCards = [];
        let matchedPairs = 0;
        let gameAttempts = 0;
        let gameTimerInterval = null;
        let gameSeconds = 0;
        let isGameStarted = false;

        // Quiz State
        let currentQuizIndex = 0;
        let quizAnswers = new Array(quizQuestions.length).fill(null);
        let quizScore = 0;

        // --- LOCALSTORAGE & PROGRESS MANAGEMENT ---
        function getSavedProgress() {
            try {
                const data = localStorage.getItem('eshist_progress');
                return data ? JSON.parse(data) : { watchedVideos: [], completedGame: false, completedEvaluation: false };
            } catch (e) {
                return { watchedVideos: [], completedGame: false, completedEvaluation: false };
            }
        }

        function saveProgressData(data) {
            try {
                localStorage.setItem('eshist_progress', JSON.stringify(data));
            } catch (e) {}
            updateGlobalProgress();
        }

        function updateGlobalProgress() {
            const progress = getSavedProgress();
            let score = 0;
            // 4 video items
            const watchedCount = progress.watchedVideos ? progress.watchedVideos.length : 0;
            score += (watchedCount / 4) * 40;
            // Game completed
            if (progress.completedGame) score += 30;
            // Evaluation completed
            if (progress.completedEvaluation) score += 30;

            const finalPercent = Math.min(Math.round(score), 100);

            // Update DOM progress bars
            document.getElementById('global-progress-bar').style.width = finalPercent + '%';
            document.getElementById('global-progress-text').innerText = finalPercent + '%';
            document.getElementById('topbar-progress-bar').style.width = finalPercent + '%';
            document.getElementById('topbar-progress-text').innerText = finalPercent + '%';
        }

        // --- NAVIGATION & PAGE TRANSITIONS ---
        const pageMeta = {
            home: { title: "Beranda", icon: "🏠" },
            timeline: { title: "Jejak Sejarah", icon: "⏳" },
            videos: { title: "Video Edukasi", icon: "🎬" },
            materials: { title: "Materi Organisasi", icon: "📚" },
            game: { title: "Game Memory Match", icon: "🎮" },
            evaluation: { title: "Evaluasi Pembelajaran", icon: "📝" },
            about: { title: "Tentang EsHist", icon: "ℹ️" }
        };

        function switchPage(pageId) {
            // Hide all views
            const views = document.querySelectorAll('.page-view');
            views.forEach(v => v.classList.remove('active'));

            // Show target view
            const target = document.getElementById(`view-${pageId}`);
            if (target) {
                target.classList.add('active');
            }

            // Update Topbar
            if (pageMeta[pageId]) {
                document.getElementById('topbar-title').innerText = pageMeta[pageId].title;
                document.getElementById('topbar-icon').innerText = pageMeta[pageId].icon;
            }

            // Update Sidebar / Mobile Nav Active Styles
            document.querySelectorAll('.nav-btn').forEach(btn => {
                if (btn.dataset.target === pageId) {
                    btn.classList.add('bg-stone-100', 'text-stone-900', 'font-bold');
                } else {
                    btn.classList.remove('bg-stone-100', 'text-stone-900', 'font-bold');
                }
            });

            document.querySelectorAll('.mob-nav-btn').forEach(btn => {
                if (btn.dataset.target === pageId) {
                    btn.classList.add('text-esRed', 'font-bold');
                } else {
                    btn.classList.remove('text-esRed', 'font-bold');
                }
            });

            // Scroll content view to top
            const contentContainer = document.querySelector('.flex-1.overflow-y-auto');
            if (contentContainer) contentContainer.scrollTop = 0;

            // Trigger specific page initializers
            if (pageId === 'timeline') renderTimelineEvent();
            if (pageId === 'videos') renderVideos();
            if (pageId === 'materials') renderMaterials();
            if (pageId === 'game' && !isGameStarted && matchedPairs === 0) initGame();
            if (pageId === 'evaluation') renderQuizQuestion();
        }

        // --- 1. TIMELINE LOGIC (ONE-EVENT-AT-A-TIME) ---
        function renderTimelineEvent() {
            const event = timelineEvents[currentTimelineIndex];
            const container = document.getElementById('timeline-card-content');
            
            container.innerHTML = `
                <div class="flex flex-col md:flex-row items-center gap-8">
                    <div class="w-full md:w-1/3 flex flex-col items-center justify-center p-8 bg-stone-50 rounded-3xl border border-stone-100 text-center">
                        <span class="text-6xl mb-3">${event.icon}</span>
                        <span class="px-3 py-1 bg-red-100 text-esRed rounded-full text-xs font-bold mb-2">${event.year}</span>
                        <h4 class="text-xs text-stone-500 font-semibold uppercase tracking-wider">${event.category}</h4>
                    </div>
                    <div class="w-full md:w-2/3 space-y-4">
                        <h3 class="text-2xl sm:text-3xl font-bold text-stone-900">${event.title}</h3>
                        <p class="text-stone-600 text-sm sm:text-base leading-relaxed">${event.description}</p>
                        <div class="p-4 bg-stone-50 rounded-2xl border border-stone-100">
                            <span class="text-xs font-semibold text-stone-400 block mb-1">Tokoh Utama:</span>
                            <span class="text-sm font-bold text-stone-900">${event.tokoh}</span>
                        </div>
                        <div class="pt-2">
                            <button onclick="openMaterialByTitle('${event.title}')" class="px-5 py-2.5 bg-stone-100 hover:bg-stone-200 text-stone-700 font-semibold text-xs rounded-xl transition-all inline-flex items-center gap-2">
                                Pelajari Materi Organisasi →
                            </button>
                        </div>
                    </div>
                </div>
            `;

            document.getElementById('timeline-counter').innerText = `${currentTimelineIndex + 1} / ${timelineEvents.length}`;
            document.getElementById('timeline-prev-btn').disabled = currentTimelineIndex === 0;
            document.getElementById('timeline-prev-btn').style.opacity = currentTimelineIndex === 0 ? '0.4' : '1';
            document.getElementById('timeline-next-btn').disabled = currentTimelineIndex === timelineEvents.length - 1;
            document.getElementById('timeline-next-btn').style.opacity = currentTimelineIndex === timelineEvents.length - 1 ? '0.4' : '1';
        }

        function nextTimelineEvent() {
            if (currentTimelineIndex < timelineEvents.length - 1) {
                currentTimelineIndex++;
                renderTimelineEvent();
            }
        }

        function prevTimelineEvent() {
            if (currentTimelineIndex > 0) {
                currentTimelineIndex--;
                renderTimelineEvent();
            }
        }

        function openMaterialByTitle(title) {
            const found = materials.find(m => m.name.toLowerCase().includes(title.toLowerCase()) || title.toLowerCase().includes(m.name.toLowerCase()));
            switchPage('materials');
            if (found) {
                openMaterialModal(found.id);
            }
        }

        // --- 2. VIDEO EDUKASI LOGIC ---
        function renderVideos() {
            const searchTerm = document.getElementById('video-search-input').value.toLowerCase();
            const grid = document.getElementById('video-grid');
            const notFound = document.getElementById('video-not-found');

            const progress = getSavedProgress();
            const watchedList = progress.watchedVideos || [];

            // Filter videos
            const filtered = videos.filter(v => {
                const matchCategory = currentVideoFilter === 'Semua' || v.category === currentVideoFilter;
                const matchSearch = v.title.toLowerCase().includes(searchTerm) || v.category.toLowerCase().includes(searchTerm) || v.description.toLowerCase().includes(searchTerm);
                return matchCategory && matchSearch;
            });

            // Update Progress count
            const completedCount = watchedList.length;
            document.getElementById('video-progress-count').innerText = `${completedCount} / ${videos.length} video selesai`;
            const pct = Math.round((completedCount / videos.length) * 100);
            document.getElementById('video-progress-bar').style.width = pct + '%';

            if (filtered.length === 0) {
                grid.innerHTML = '';
                notFound.classList.remove('hidden');
                return;
            }
            notFound.classList.add('hidden');

            grid.innerHTML = filtered.map(v => {
                const isWatched = watchedList.includes(v.id);
                return `
                    <div onclick="selectVideo(${v.id})" class="bg-white rounded-3xl border border-stone-200/80 p-4 shadow-sm cursor-pointer es-card flex flex-col justify-between group">
                        <div>
                            <div class="relative w-full aspect-video bg-stone-900 rounded-2xl overflow-hidden mb-3 flex items-center justify-center text-white">
                                <div class="absolute inset-0 bg-gradient-to-t from-stone-900/60 to-transparent"></div>
                                <span class="text-3xl group-hover:scale-110 transition-transform relative z-10">▶</span>
                                <span class="absolute bottom-2 right-2 px-2 py-0.5 bg-black/60 backdrop-blur-sm text-white text-[10px] font-semibold rounded-lg">${v.duration}</span>
                                <span class="absolute top-2 left-2 px-2 py-0.5 bg-esRed text-white text-[10px] font-semibold rounded-lg">${v.category}</span>
                            </div>
                            <h4 class="font-bold text-sm text-stone-900 mb-1 group-hover:text-esRed transition-colors">${v.title}</h4>
                            <p class="text-xs text-stone-500 line-clamp-2">${v.description}</p>
                        </div>
                        <div class="pt-3 mt-3 border-t border-stone-100 flex items-center justify-between">
                            <span class="text-[11px] font-semibold ${isWatched ? 'text-emerald-600' : 'text-stone-400'}">
                                ${isWatched ? '✓ Sudah ditonton' : 'Belum ditonton'}
                            </span>
                            <span class="text-xs font-bold text-esRed">Putar →</span>
                        </div>
                    </div>
                `;
            }).join('');

            // Load Active Video Player Details
            const activeV = videos.find(v => v.id === activeVideoId) || videos[0];
            document.getElementById('main-video-title').innerText = activeV.title;
            document.getElementById('main-video-category').innerText = activeV.category;
            document.getElementById('main-video-duration').innerText = activeV.duration;
            document.getElementById('main-video-desc').innerText = activeV.description;
            
            const badge = document.getElementById('main-video-status-badge');
            if (watchedList.includes(activeV.id)) {
                badge.innerText = '✓ Sudah ditonton';
                badge.className = 'px-2.5 py-0.5 bg-emerald-50 text-emerald-700 rounded-full text-xs font-semibold';
            } else {
                badge.innerText = 'Belum ditonton';
                badge.className = 'px-2.5 py-0.5 bg-stone-100 text-stone-600 rounded-full text-xs font-semibold';
            }

            // Load saved notes for this video
            try {
                const notes = JSON.parse(localStorage.getItem(`eshist_note_${activeV.id}`) || '""');
                document.getElementById('video-notes-input').value = notes;
            } catch(e) {
                document.getElementById('video-notes-input').value = '';
            }

            const player = document.getElementById('main-video-player');
            const fallback = document.getElementById('video-fallback-msg');
            player.src = activeV.file;
            player.onerror = function() {
                fallback.classList.remove('hidden');
            };
            player.onloadeddata = function() {
                fallback.classList.add('hidden');
            };
        }

        function selectVideo(id) {
            activeVideoId = id;
            renderVideos();
            const contentContainer = document.querySelector('.flex-1.overflow-y-auto');
            if (contentContainer) contentContainer.scrollTop = 0;
        }

        function setVideoFilter(cat) {
            currentVideoFilter = cat;
            document.querySelectorAll('.video-filter-btn').forEach(btn => {
                if (btn.innerText === cat) {
                    btn.className = 'video-filter-btn px-4 py-2 rounded-xl text-xs font-semibold bg-esRed text-white transition-all';
                } else {
                    btn.className = 'video-filter-btn px-4 py-2 rounded-xl text-xs font-semibold bg-white text-stone-600 border border-stone-200 hover:bg-stone-50 transition-all';
                }
            });
            renderVideos();
        }

        function filterVideos() {
            renderVideos();
        }

        function handleVideoEnded() {
            const progress = getSavedProgress();
            if (!progress.watchedVideos) progress.watchedVideos = [];
            if (!progress.watchedVideos.includes(activeVideoId)) {
                progress.watchedVideos.push(activeVideoId);
                saveProgressData(progress);
                renderVideos();
            }
        }

        function saveVideoNotes() {
            const val = document.getElementById('video-notes-input').value;
            try {
                localStorage.setItem(`eshist_note_${activeVideoId}`, JSON.stringify(val));
                alertBox('Catatan belajar berhasil disimpan!');
            } catch(e) {}
        }

        function openMaterialFromVideo() {
            const activeV = videos.find(v => v.id === activeVideoId);
            if (!activeV) return;
            openMaterialByTitle(activeV.title);
        }

        // Custom notification box instead of alert()
        function alertBox(msg) {
            const div = document.createElement('div');
            div.className = 'fixed bottom-6 right-6 bg-stone-900 text-white px-6 py-3 rounded-2xl shadow-xl z-50 text-sm font-medium animate-bounce';
            div.innerText = msg;
            document.body.appendChild(div);
            setTimeout(() => div.remove(), 2500);
        }

        // --- 3. MATERIALS / ORGANIZATIONS LOGIC ---
        function renderMaterials() {
            const searchTerm = document.getElementById('material-search-input').value.toLowerCase();
            const grid = document.getElementById('material-grid');
            const notFound = document.getElementById('material-not-found');

            const filtered = materials.filter(m => {
                const matchCategory = currentMaterialFilter === 'Semua' || m.category === currentMaterialFilter;
                const matchSearch = m.name.toLowerCase().includes(searchTerm) || m.tokoh.toLowerCase().includes(searchTerm) || m.background.toLowerCase().includes(searchTerm);
                return matchCategory && matchSearch;
            });

            if (filtered.length === 0) {
                grid.innerHTML = '';
                notFound.classList.remove('hidden');
                return;
            }
            notFound.classList.add('hidden');

            grid.innerHTML = filtered.map(m => `
                <div onclick="openMaterialModal('${m.id}')" class="bg-white p-6 rounded-3xl border border-stone-200/80 shadow-sm cursor-pointer es-card space-y-3 flex flex-col justify-between">
                    <div>
                        <div class="flex items-center justify-between mb-2">
                            <span class="px-2.5 py-0.5 bg-red-50 text-esRed rounded-full text-xs font-semibold">${m.category}</span>
                            <span class="text-xs font-bold text-stone-400">${m.year}</span>
                        </div>
                        <h3 class="font-bold text-lg text-stone-900 mb-1">${m.name}</h3>
                        <p class="text-xs text-stone-500 line-clamp-2">${m.background}</p>
                    </div>
                    <div class="pt-3 border-t border-stone-100 flex items-center justify-between">
                        <span class="text-xs font-medium text-stone-600 truncate max-w-[180px]">Tokoh: ${m.tokoh}</span>
                        <span class="text-xs font-bold text-esRed">Detail →</span>
                    </div>
                </div>
            `).join('');
        }

        function setMaterialFilter(cat) {
            currentMaterialFilter = cat;
            document.querySelectorAll('.material-filter-btn').forEach(btn => {
                if (btn.innerText === cat) {
                    btn.className = 'material-filter-btn px-4 py-2 rounded-xl text-xs font-semibold bg-esRed text-white transition-all';
                } else {
                    btn.className = 'material-filter-btn px-4 py-2 rounded-xl text-xs font-semibold bg-white text-stone-600 border border-stone-200 hover:bg-stone-50 transition-all';
                }
            });
            renderMaterials();
        }

        function filterMaterials() {
            renderMaterials();
        }

        function openMaterialModal(id) {
            const m = materials.find(item => item.id === id);
            if (!m) return;

            const modal = document.getElementById('material-modal');
            const content = document.getElementById('modal-content');

            content.innerHTML = `
                <div class="flex items-center gap-3 mb-2">
                    <span class="px-3 py-1 bg-red-100 text-esRed rounded-full text-xs font-bold">${m.category}</span>
                    <span class="text-xs font-bold text-stone-400">Tahun ${m.year}</span>
                </div>
                <h2 class="text-2xl sm:text-3xl font-bold text-stone-900">${m.name}</h2>
                <div class="space-y-4 text-sm text-stone-600 leading-relaxed pt-2">
                    <div class="bg-stone-50 p-4 rounded-2xl border border-stone-100">
                        <h4 class="font-bold text-stone-900 mb-1">Latar Belakang</h4>
                        <p>${m.background}</p>
                    </div>
                    <div class="bg-stone-50 p-4 rounded-2xl border border-stone-100">
                        <h4 class="font-bold text-stone-900 mb-1">Tujuan Utama</h4>
                        <p>${m.tujuan}</p>
                    </div>
                    <div class="bg-stone-50 p-4 rounded-2xl border border-stone-100">
                        <h4 class="font-bold text-stone-900 mb-1">Tokoh Penting</h4>
                        <p class="font-semibold text-stone-900">${m.tokoh}</p>
                    </div>
                    <div class="bg-stone-50 p-4 rounded-2xl border border-stone-100">
                        <h4 class="font-bold text-stone-900 mb-1">Peran dalam Pergerakan</h4>
                        <p>${m.peran}</p>
                    </div>
                    <div class="bg-red-50/50 p-4 rounded-2xl border border-red-100">
                        <h4 class="font-bold text-esRed mb-1">Fakta Menarik</h4>
                        <p class="text-stone-700">${m.fakta}</p>
                    </div>
                </div>
            `;
            modal.classList.remove('hidden');
        }

        function closeMaterialModal() {
            document.getElementById('material-modal').classList.add('hidden');
        }

        // --- 4. GAME LOGIC (MEMORY MATCH) ---
        const gamePairs = [
            { org: "Budi Utomo", figure: "dr. Soetomo" },
            { org: "Muhammadiyah", figure: "K.H. Ahmad Dahlan" },
            { org: "Indische Partij", figure: "Douwes Dekker" },
            { org: "PNI", figure: "Soekarno" },
            { org: "Taman Siswa", figure: "Ki Hadjar Dewantara" },
            { org: "Perhimpunan Indonesia", figure: "Mohammad Hatta" }
        ];

        function initGame() {
            clearInterval(gameTimerInterval);
            gameSeconds = 0;
            gameAttempts = 0;
            matchedPairs = 0;
            isGameStarted = true;
            flippedCards = [];

            document.getElementById('game-attempts').innerText = '0';
            document.getElementById('game-matches').innerText = '0 / 6';
            document.getElementById('game-timer').innerText = '00:00';
            document.getElementById('game-win-banner').classList.add('hidden');

            // Start timer
            gameTimerInterval = setInterval(() => {
                gameSeconds++;
                const mins = String(Math.floor(gameSeconds / 60)).padStart(2, '0');
                const secs = String(gameSeconds % 60).padStart(2, '0');
                document.getElementById('game-timer').innerText = `${mins}:${secs}`;
            }, 1000);

            // Build 12 cards (6 organizations + 6 figures)
            let rawCards = [];
            gamePairs.forEach((p, idx) => {
                rawCards.push({ id: idx, text: p.org, type: 'org', pairId: idx });
                rawCards.push({ id: idx + 100, text: p.figure, type: 'figure', pairId: idx });
            });

            // Shuffle cards
            gameCards = rawCards.sort(() => Math.random() - 0.5);

            renderGameBoard();
        }

        function renderGameBoard() {
            const board = document.getElementById('game-board');
            board.innerHTML = gameCards.map((card, idx) => `
                <div onclick="flipCard(${idx})" id="card-${idx}" class="perspective-1000 h-28 cursor-pointer select-none">
                    <div class="transform-style-3d relative w-full h-full rounded-2xl shadow-sm ${card.isFlipped || card.isMatched ? 'rotate-y-180' : ''}">
                        <!-- Card Front (Hidden) -->
                        <div class="absolute inset-0 bg-stone-900 rounded-2xl flex items-center justify-center text-white font-bold text-xl backface-hidden shadow-sm">
                            🏛️
                        </div>
                        <!-- Card Back (Revealed) -->
                        <div class="absolute inset-0 bg-white border-2 ${card.isMatched ? 'border-emerald-500 bg-emerald-50/50' : 'border-stone-200'} rounded-2xl flex flex-col items-center justify-center p-3 text-center backface-hidden rotate-y-180">
                            <span class="text-[10px] font-bold ${card.isMatched ? 'text-emerald-600' : 'text-esRed'} uppercase mb-1">${card.type === 'org' ? 'Organisasi' : 'Tokoh'}</span>
                            <span class="text-xs sm:text-sm font-bold text-stone-900 leading-snug">${card.text}</span>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        function flipCard(idx) {
            const card = gameCards[idx];
            if (card.isFlipped || card.isMatched || flippedCards.length >= 2) return;

            card.isFlipped = true;
            flippedCards.push({ idx, ...card });
            renderGameBoard();

            if (flippedCards.length === 2) {
                gameAttempts++;
                document.getElementById('game-attempts').innerText = gameAttempts;

                const [c1, c2] = flippedCards;
                if (c1.pairId === c2.pairId && c1.type !== c2.type) {
                    // Match found
                    gameCards[c1.idx].isMatched = true;
                    gameCards[c2.idx].isMatched = true;
                    matchedPairs++;
                    document.getElementById('game-matches').innerText = `${matchedPairs} / 6`;
                    flippedCards = [];

                    if (matchedPairs === 6) {
                        clearInterval(gameTimerInterval);
                        const mins = String(Math.floor(gameSeconds / 60)).padStart(2, '0');
                        const secs = String(gameSeconds % 60).padStart(2, '0');
                        document.getElementById('game-win-stats').innerText = `Kamu berhasil menyelesaikan permainan dalam waktu ${mins}:${secs} dengan ${gameAttempts} percobaan.`;
                        document.getElementById('game-win-banner').classList.remove('hidden');

                        // Save progress
                        const progress = getSavedProgress();
                        progress.completedGame = true;
                        saveProgressData(progress);
                    }
                } else {
                    // Mismatch
                    setTimeout(() => {
                        gameCards[c1.idx].isFlipped = false;
                        gameCards[c2.idx].isFlipped = false;
                        flippedCards = [];
                        renderGameBoard();
                    }, 800);
                }
            }
        }

        // --- 5. EVALUASI QUIZ LOGIC ---
        function renderQuizQuestion() {
            const q = quizQuestions[currentQuizIndex];
            document.getElementById('quiz-counter').innerText = `PERTANYAAN ${String(currentQuizIndex + 1).padStart(2, '0')} / 20`;
            const pct = Math.round(((currentQuizIndex + 1) / quizQuestions.length) * 100);
            document.getElementById('quiz-progress-bar').style.width = pct + '%';
            document.getElementById('quiz-question').innerText = q.question;

            const optionsContainer = document.getElementById('quiz-options');
            const feedbackBox = document.getElementById('quiz-feedback');
            const nextBtn = document.getElementById('quiz-next-btn');

            feedbackBox.classList.add('hidden');
            nextBtn.classList.add('hidden');

            const savedAns = quizAnswers[currentQuizIndex];

            optionsContainer.innerHTML = q.options.map((opt, i) => {
                let btnStyle = "w-full text-left p-4 rounded-2xl border border-stone-200 bg-white font-medium text-sm text-stone-800 hover:border-esRed hover:bg-red-50/30 transition-all";
                if (savedAns !== null) {
                    if (i === q.answer) {
                        btnStyle = "w-full text-left p-4 rounded-2xl border border-emerald-500 bg-emerald-50 font-semibold text-sm text-emerald-900";
                    } else if (i === savedAns && savedAns !== q.answer) {
                        btnStyle = "w-full text-left p-4 rounded-2xl border border-red-500 bg-red-50 font-semibold text-sm text-red-900";
                    } else {
                        btnStyle = "w-full text-left p-4 rounded-2xl border border-stone-200 bg-stone-50 opacity-60 text-sm text-stone-500";
                    }
                }
                return `
                    <button onclick="selectQuizAnswer(${i})" ${savedAns !== null ? 'disabled' : ''} class="${btnStyle}">
                        ${opt}
                    </button>
                `;
            }).join('');

            if (savedAns !== null) {
                feedbackBox.classList.remove('hidden');
                if (savedAns === q.answer) {
                    feedbackBox.className = "p-4 rounded-2xl text-sm font-semibold bg-emerald-50 text-emerald-800 border border-emerald-200";
                    feedbackBox.innerText = "✓ Benar! Jawabanmu tepat.";
                } else {
                    feedbackBox.className = "p-4 rounded-2xl text-sm font-semibold bg-red-50 text-red-800 border border-red-200";
                    feedbackBox.innerText = `✗ Kurang tepat. Jawaban yang benar adalah: ${q.options[q.answer]}`;
                }
                nextBtn.classList.remove('hidden');
            }
        }

        function selectQuizAnswer(selectedIdx) {
            quizAnswers[currentQuizIndex] = selectedIdx;
            renderQuizQuestion();
        }

        function nextQuizQuestion() {
            if (currentQuizIndex < quizQuestions.length - 1) {
                currentQuizIndex++;
                renderQuizQuestion();
            } else {
                showQuizResult();
            }
        }

        function showQuizResult() {
            let correctCount = 0;
            quizQuestions.forEach((q, idx) => {
                if (quizAnswers[idx] === q.answer) correctCount++;
            });
            const wrongCount = quizQuestions.length - correctCount;
            const finalScore = Math.round((correctCount / quizQuestions.length) * 100);

            quizScore = finalScore;

            document.getElementById('quiz-container').classList.add('hidden');
            const resultCard = document.getElementById('quiz-result-card');
            resultCard.classList.remove('hidden');

            document.getElementById('result-score').innerText = finalScore;
            document.getElementById('result-correct').innerText = correctCount;
            document.getElementById('result-wrong').innerText = wrongCount;

            const feedbackText = document.getElementById('result-feedback-text');
            if (finalScore >= 80) {
                feedbackText.innerText = "Luar biasa! Pemahamanmu tentang sejarah pergerakan nasional sudah sangat luar biasa. Pertahankan prestasi belajarmu!";
            } else if (finalScore >= 60) {
                feedbackText.innerText = "Bagus! Kamu sudah memahami sebagian besar materi, namun masih ada beberapa bagian yang perlu ditinjau kembali di menu Materi.";
            } else {
                feedbackText.innerText = "Jangan menyerah! Mari ulangi materi organisasi pergerakan nasional dan tonton kembali video edukasi untuk memperdalam pemahamanmu.";
            }

            // Save evaluation progress
            const progress = getSavedProgress();
            progress.completedEvaluation = true;
            saveProgressData(progress);
        }

        function restartEvaluation() {
            currentQuizIndex = 0;
            quizAnswers = new Array(quizQuestions.length).fill(null);
            document.getElementById('quiz-container').classList.remove('hidden');
            document.getElementById('quiz-result-card').classList.add('hidden');
            renderQuizQuestion();
        }

        // --- INITIALIZATION ON WINDOW LOAD ---
        window.onload = function() {
            updateGlobalProgress();
            switchPage('home');
        };
    </script>
</body>
</html>
