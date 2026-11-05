<!DOCTYPE html>
<html lang="th">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Football Quiz Arena - เกมควิซฟุตบอลสุดมันส์</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts: Kanit -->
    <link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;600;700;800&display=swap" rel="stylesheet">
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Kanit', 'sans-serif'],
                    },
                    colors: {
                        pitch: {
                            800: '#0B1914',
                            900: '#050D0A',
                        },
                        neon: {
                            green: '#00FF66',
                            yellow: '#FFDD00',
                            red: '#FF2A5F',
                            cyan: '#00F0FF'
                        }
                    },
                    boxShadow: {
                        'neon-green': '0 0 20px rgba(0, 255, 102, 0.4)',
                        'neon-cyan': '0 0 20px rgba(0, 240, 255, 0.4)',
                        'neon-red': '0 0 20px rgba(255, 42, 95, 0.4)',
                        'card': '0 10px 30px rgba(0,0,0,0.5)',
                    }
                }
            }
        }
    </script>
    
    <style>
        body {
            font-family: 'Kanit', sans-serif;
            background-color: #050D0A;
            background-image: 
                radial-gradient(circle at 50% 0%, rgba(0, 255, 102, 0.15), transparent 70%),
                radial-gradient(circle at 80% 80%, rgba(0, 240, 255, 0.08), transparent 50%);
            min-height: 100vh;
            color: #ffffff;
            overflow-x: hidden;
        }

        /* Glassmorphism background */
        .glass-panel {
            background: rgba(15, 30, 24, 0.75);
            backdrop-filter: blur(16px);
            -webkit-backdrop-filter: blur(16px);
            border: 1px solid rgba(255, 255, 255, 0.1);
        }

        /* Animated grid background effect */
        .bg-grid {
            background-size: 40px 40px;
            background-image: 
                linear-gradient(to right, rgba(255, 255, 255, 0.03) 1px, transparent 1px),
                linear-gradient(to bottom, rgba(255, 255, 255, 0.03) 1px, transparent 1px);
        }

        /* Glow animation for buttons */
        @keyframes pulseGlow {
            0%, 100% { opacity: 0.8; transform: scale(1); }
            50% { opacity: 1; transform: scale(1.02); }
        }

        .btn-glow {
            animation: pulseGlow 3s infinite ease-in-out;
        }

        /* Answer Option Animations */
        .option-btn {
            transition: all 0.25s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .option-btn:hover:not(:disabled) {
            transform: translateY(-2px);
            border-color: rgba(0, 255, 102, 0.6);
            box-shadow: 0 4px 20px rgba(0, 255, 102, 0.2);
        }

        /* Custom Scrollbar */
        ::-webkit-scrollbar {
            width: 8px;
        }
        ::-webkit-scrollbar-track {
            background: #050D0A;
        }
        ::-webkit-scrollbar-thumb {
            background: #1f3a2e;
            border-radius: 4px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #00FF66;
        }

        /* Progress Bar Smooth Transition */
        .timer-progress {
            transition: width 1s linear;
        }
    </style>
</head>
<body class="bg-grid flex flex-col justify-between items-center min-h-screen p-4 sm:p-6 relative">

    <!-- Header Navigation -->
    <header class="w-full max-w-3xl flex justify-between items-center py-3 px-4 glass-panel rounded-2xl mb-4 z-10">
        <div class="flex items-center space-x-3 cursor-pointer" onclick="showScreen('level-screen')">
            <div class="w-10 h-10 rounded-xl bg-gradient-to-br from-neon-green to-emerald-700 flex items-center justify-center text-pitch-900 font-extrabold text-xl shadow-neon-green">
                <i class="fa-solid font-black fa-futbol"></i>
            </div>
            <div>
                <h1 class="font-extrabold text-lg sm:text-xl tracking-wider text-transparent bg-clip-text bg-gradient-to-r from-white via-gray-200 to-neon-green">FOOTBALL QUIZ</h1>
                <p class="text-xs text-gray-400 font-light">ARENA CHALLENGE</p>
            </div>
        </div>
        
        <!-- Sound Toggle -->
        <button id="sound-btn" onclick="toggleSound()" class="w-10 h-10 rounded-xl glass-panel flex items-center justify-center text-gray-300 hover:text-neon-green transition-colors">
            <i id="sound-icon" class="fa-solid fa-volume-high text-lg"></i>
        </button>
    </header>

    <!-- MAIN CONTAINER -->
    <main class="w-full max-w-3xl flex-1 flex flex-col justify-center items-center z-10 relative">

        <!-- ==================== SCREEN 1: LEVEL SELECTOR ==================== -->
        <section id="level-screen" class="w-full flex flex-col items-center text-center space-y-6">
            
            <div class="space-y-2">
                <span class="px-4 py-1.5 rounded-full text-xs font-semibold uppercase tracking-widest bg-emerald-500/10 text-neon-green border border-neon-green/30 inline-block">
                    ทดสอบความรู้ลูกหนัง
                </span>
                <h2 class="text-3xl sm:text-5xl font-extrabold tracking-tight">เลือกระดับความท้าทาย</h2>
                <p class="text-gray-400 text-sm sm:text-base max-w-md mx-auto">เลือกความยากเพื่อเริ่มสะสมคะแนน และพิสูจน์ความเป็นแฟนพันธุ์แท้ตัวจริง!</p>
            </div>

            <!-- Level Cards -->
            <div class="grid grid-cols-1 md:grid-cols-3 gap-4 w-full mt-4">
                
                <!-- EASY -->
                <div onclick="startQuiz('easy')" class="glass-panel p-6 rounded-2xl hover:border-neon-green transition-all duration-300 cursor-pointer group flex flex-col justify-between space-y-4 hover:shadow-neon-green relative overflow-hidden">
                    <div class="absolute top-0 right-0 w-24 h-24 bg-neon-green/5 rounded-full blur-xl group-hover:bg-neon-green/20 transition-all"></div>
                    <div class="flex justify-between items-start">
                        <div class="w-12 h-12 rounded-xl bg-emerald-500/20 text-neon-green flex items-center justify-center text-2xl font-bold border border-neon-green/30">
                            🟢
                        </div>
                        <span class="text-xs font-semibold px-2.5 py-1 rounded-full bg-emerald-950 text-neon-green border border-emerald-800">5 ข้อ</span>
                    </div>
                    <div class="text-left">
                        <h3 class="text-xl font-bold text-white group-hover:text-neon-green transition-colors">ระดับ: ง่าย</h3>
                        <p class="text-xs text-gray-400 mt-1">คำถามพื้นฐาน นักเตะดัง สโมสรยอดนิยม ใครๆ ก็ตอบได้</p>
                    </div>
                    <div class="pt-2 flex items-center justify-between text-xs font-semibold text-neon-green">
                        <span>เริ่มเลเวลนี้</span>
                        <i class="fa-solid fa-arrow-right group-hover:translate-x-1 transition-transform"></i>
                    </div>
                </div>

                <!-- MEDIUM -->
                <div onclick="startQuiz('medium')" class="glass-panel p-6 rounded-2xl hover:border-neon-yellow transition-all duration-300 cursor-pointer group flex flex-col justify-between space-y-4 hover:shadow-lg relative overflow-hidden">
                    <div class="absolute top-0 right-0 w-24 h-24 bg-neon-yellow/5 rounded-full blur-xl group-hover:bg-neon-yellow/20 transition-all"></div>
                    <div class="flex justify-between items-start">
                        <div class="w-12 h-12 rounded-xl bg-amber-500/20 text-neon-yellow flex items-center justify-center text-2xl font-bold border border-neon-yellow/30">
                            🟡
                        </div>
                        <span class="text-xs font-semibold px-2.5 py-1 rounded-full bg-amber-950 text-neon-yellow border border-amber-800">5 ข้อ</span>
                    </div>
                    <div class="text-left">
                        <h3 class="text-xl font-bold text-white group-hover:text-neon-yellow transition-colors">ระดับ: ปานกลาง</h3>
                        <p class="text-xs text-gray-400 mt-1">ต้องติดตามข่าวสาร รางวัลใหญ่ และประวัติศาสตร์ย่อยๆ</p>
                    </div>
                    <div class="pt-2 flex items-center justify-between text-xs font-semibold text-neon-yellow">
                        <span>เริ่มเลเวลนี้</span>
                        <i class="fa-solid fa-arrow-right group-hover:translate-x-1 transition-transform"></i>
                    </div>
                </div>

                <!-- HARD -->
                <div onclick="startQuiz('hard')" class="glass-panel p-6 rounded-2xl hover:border-neon-red transition-all duration-300 cursor-pointer group flex flex-col justify-between space-y-4 hover:shadow-neon-red relative overflow-hidden">
                    <div class="absolute top-0 right-0 w-24 h-24 bg-neon-red/5 rounded-full blur-xl group-hover:bg-neon-red/20 transition-all"></div>
                    <div class="flex justify-between items-start">
                        <div class="w-12 h-12 rounded-xl bg-rose-500/20 text-neon-red flex items-center justify-center text-2xl font-bold border border-neon-red/30">
                            🔴
                        </div>
                        <span class="text-xs font-semibold px-2.5 py-1 rounded-full bg-rose-950 text-neon-red border border-rose-800">5 ข้อ</span>
                    </div>
                    <div class="text-left">
                        <h3 class="text-xl font-bold text-white group-hover:text-neon-red transition-colors">ระดับ: ยากแฟนพันธุ์แท้</h3>
                        <p class="text-xs text-gray-400 mt-1">สถิติลึก เกร็ดประวัติศาสตร์ และเรื่องราวตำนานวงการฟุตบอล</p>
                    </div>
                    <div class="pt-2 flex items-center justify-between text-xs font-semibold text-neon-red">
                        <span>เริ่มเลเวลนี้</span>
                        <i class="fa-solid fa-arrow-right group-hover:translate-x-1 transition-transform"></i>
                    </div>
                </div>

            </div>

            <!-- Footer Stats Hint -->
            <div class="glass-panel py-3 px-6 rounded-xl flex items-center space-x-6 text-xs text-gray-400 mt-4">
                <div class="flex items-center space-x-2">
                    <i class="fa-solid fa-bolt text-neon-green"></i>
                    <span>ทำเวลาเร็วได้โบนัสคะแนน</span>
                </div>
                <div class="h-3 w-px bg-gray-700"></div>
                <div class="flex items-center space-x-2">
                    <i class="fa-solid fa-fire text-neon-yellow"></i>
                    <span>สะสม Streak คูณคะแนน</span>
                </div>
            </div>

        </section>

        <!-- ==================== SCREEN 2: QUIZ SCREEN ==================== -->
        <section id="quiz-screen" class="w-full hidden flex-col space-y-4">
            
            <!-- Top Dashboard Stats -->
            <div class="grid grid-cols-3 gap-3 w-full">
                <!-- Progress -->
                <div class="glass-panel p-3 rounded-xl flex flex-col items-center justify-center">
                    <span class="text-xs text-gray-400">คำถาม</span>
                    <span id="question-tracker" class="text-lg font-bold text-white">1 / 5</span>
                </div>
                <!-- Timer -->
                <div class="glass-panel p-3 rounded-xl flex flex-col items-center justify-center relative overflow-hidden">
                    <span class="text-xs text-gray-400">เวลาเหลือ</span>
                    <span id="timer-text" class="text-lg font-bold text-neon-green">15s</span>
                </div>
                <!-- Score & Streak -->
                <div class="glass-panel p-3 rounded-xl flex flex-col items-center justify-center">
                    <div class="flex items-center space-x-1">
                        <span class="text-xs text-gray-400">คะแนน</span>
                        <span id="streak-badge" class="hidden text-[10px] bg-neon-yellow text-black font-extrabold px-1.5 py-0.2 rounded-full">x2</span>
                    </div>
                    <span id="score-text" class="text-lg font-bold text-neon-cyan">0</span>
                </div>
            </div>

            <!-- Progress Bar -->
            <div class="w-full bg-gray-800/60 h-2 rounded-full overflow-hidden p-0.5 border border-white/5">
                <div id="timer-bar" class="timer-progress bg-gradient-to-r from-neon-green via-neon-yellow to-neon-red h-full rounded-full" style="width: 100%;"></div>
            </div>

            <!-- Question Card -->
            <div class="glass-panel p-6 sm:p-8 rounded-2xl relative shadow-card">
                <!-- Category/Badge tag -->
                <div class="flex justify-between items-center mb-4">
                    <span id="difficulty-badge" class="text-xs font-bold px-3 py-1 rounded-full bg-emerald-500/20 text-neon-green border border-neon-green/30">
                        ระดับง่าย
                    </span>
                    <span id="question-category" class="text-xs text-gray-400 font-light flex items-center gap-1">
                        <i class="fa-solid fa-trophy text-neon-yellow"></i> ฟุตบอลโลก
                    </span>
                </div>

                <!-- Question Text -->
                <h2 id="question-text" class="text-xl sm:text-2xl font-bold leading-relaxed mb-6 text-white">
                    คำถามกำลังโหลด...
                </h2>

                <!-- Answer Choices Grid -->
                <div id="options-container" class="grid grid-cols-1 gap-3">
                    <!-- Option buttons will be generated by JS -->
                </div>

                <!-- Explanation Box (Hidden by default) -->
                <div id="explanation-box" class="hidden mt-6 p-4 rounded-xl bg-gray-900/90 border border-gray-700 text-sm leading-relaxed text-gray-300 animate-fadeIn">
                    <div class="flex items-center gap-2 font-bold mb-1 text-neon-green" id="explanation-title">
                        <i class="fa-solid fa-circle-info"></i> เฉลย & คำอธิบาย
                    </div>
                    <p id="explanation-text"></p>
                </div>
            </div>

            <!-- Action Button / Next -->
            <div class="flex justify-end pt-2">
                <button id="next-btn" onclick="nextQuestion()" class="hidden px-6 py-3 rounded-xl bg-gradient-to-r from-neon-green to-emerald-600 text-pitch-900 font-extrabold flex items-center space-x-2 shadow-neon-green hover:brightness-110 active:scale-95 transition-all">
                    <span>ข้อถัดไป</span>
                    <i class="fa-solid fa-chevron-right"></i>
                </button>
            </div>

        </section>

        <!-- ==================== SCREEN 3: RESULT SCREEN ==================== -->
        <section id="result-screen" class="w-full hidden flex-col items-center text-center space-y-6">
            
            <div class="glass-panel p-8 rounded-3xl w-full flex flex-col items-center shadow-card relative overflow-hidden">
                <!-- Glowing Orb Background -->
                <div class="absolute -top-10 left-1/2 -translate-x-1/2 w-40 h-40 bg-neon-green/20 rounded-full blur-3xl"></div>

                <!-- Rank Trophy Icon -->
                <div class="w-24 h-24 rounded-full bg-gradient-to-tr from-emerald-900 to-pitch-800 border-2 border-neon-green flex items-center justify-center text-5xl mb-4 shadow-neon-green relative">
                    <span id="rank-icon">🏆</span>
                </div>

                <span class="text-xs uppercase tracking-widest text-gray-400 font-semibold mb-1">ผลการแข่งขันของคุณ</span>
                <h2 id="rank-title" class="text-3xl sm:text-4xl font-extrabold text-transparent bg-clip-text bg-gradient-to-r from-neon-green via-white to-neon-cyan mb-2">
                    ตำนานลูกหนัง!
                </h2>
                <p id="rank-desc" class="text-sm text-gray-300 max-w-sm mb-6">คุณคือแฟนบอลตัวจริง รู้ลึกรู้จริงทุกคำถาม!</p>

                <!-- Final Score Circle / Card -->
                <div class="grid grid-cols-2 gap-4 w-full max-w-md mb-6">
                    <div class="bg-pitch-900/80 p-4 rounded-2xl border border-white/5 flex flex-col items-center">
                        <span class="text-xs text-gray-400">ตอบถูกทั้งหมด</span>
                        <span id="result-correct-count" class="text-2xl font-black text-neon-green">5 / 5</span>
                        <span id="result-percentage" class="text-[11px] text-gray-400 mt-1">(100%)</span>
                    </div>
                    <div class="bg-pitch-900/80 p-4 rounded-2xl border border-white/5 flex flex-col items-center">
                        <span class="text-xs text-gray-400">คะแนนรวมสูงสุด</span>
                        <span id="result-final-score" class="text-2xl font-black text-neon-cyan">2,500</span>
                        <span class="text-[11px] text-neon-yellow mt-1"><i class="fa-solid fa-bolt"></i> มีโบนัสความเร็ว</span>
                    </div>
                </div>

                <!-- Action Buttons -->
                <div class="flex flex-col sm:flex-row gap-3 w-full max-w-md">
                    <button onclick="retryCurrentLevel()" class="flex-1 py-3.5 px-4 rounded-xl bg-gradient-to-r from-neon-green to-emerald-600 text-pitch-900 font-extrabold hover:brightness-110 active:scale-95 transition-all shadow-neon-green flex justify-center items-center gap-2">
                        <i class="fa-solid fa-rotate-right"></i>
                        <span>เล่นอีกครั้ง</span>
                    </button>
                    <button onclick="showScreen('level-screen')" class="flex-1 py-3.5 px-4 rounded-xl glass-panel text-white font-bold hover:bg-white/10 active:scale-95 transition-all flex justify-center items-center gap-2">
                        <i class="fa-solid fa-list-check text-neon-cyan"></i>
                        <span>เปลี่ยนระดับความยาก</span>
                    </button>
                </div>
            </div>

        </section>

    </main>

    <!-- Footer -->
    <footer class="w-full max-w-3xl text-center py-4 text-xs text-gray-500 font-light z-10">
        Football Quiz Arena &copy; 2026 - ออกแบบเพื่อคอฟุตบอลตัวจริง
    </footer>

    <!-- ==================== JAVASCRIPT LOGIC ==================== -->
    <script>
        /* ================= Sound Synthesizer (Web Audio API) ================= */
        let soundEnabled = true;
        const AudioContext = window.AudioContext || window.webkitAudioContext;
        let audioCtx = null;

        function initAudio() {
            if (!audioCtx) {
                audioCtx = new AudioContext();
            }
        }

        function playSound(type) {
            if (!soundEnabled) return;
            initAudio();
            if (audioCtx.state === 'suspended') {
                audioCtx.resume();
            }

            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();
            osc.connect(gain);
            gain.connect(audioCtx.destination);

            const now = audioCtx.currentTime;

            if (type === 'click') {
                osc.type = 'sine';
                osc.frequency.setValueAtTime(400, now);
                osc.frequency.exponentialRampToValueAtTime(800, now + 0.05);
                gain.gain.setValueAtTime(0.1, now);
                gain.gain.linearRampToValueAtTime(0.01, now + 0.05);
                osc.start(now);
                osc.stop(now + 0.05);
            } else if (type === 'correct') {
                // Happy Chord
                osc.type = 'triangle';
                osc.frequency.setValueAtTime(523.25, now); // C5
                osc.frequency.setValueAtTime(659.25, now + 0.1); // E5
                osc.frequency.setValueAtTime(783.99, now + 0.2); // G5
                gain.gain.setValueAtTime(0.15, now);
                gain.gain.exponentialRampToValueAtTime(0.01, now + 0.4);
                osc.start(now);
                osc.stop(now + 0.4);
            } else if (type === 'wrong') {
                // Low Buzz
                osc.type = 'sawtooth';
                osc.frequency.setValueAtTime(180, now);
                osc.frequency.linearRampToValueAtTime(110, now + 0.25);
                gain.gain.setValueAtTime(0.2, now);
                gain.gain.exponentialRampToValueAtTime(0.01, now + 0.3);
                osc.start(now);
                osc.stop(now + 0.3);
            } else if (type === 'victory') {
                // Victory Fanfare
                const notes = [440, 554.37, 659.25, 880];
                notes.forEach((freq, index) => {
                    const noteOsc = audioCtx.createOscillator();
                    const noteGain = audioCtx.createGain();
                    noteOsc.type = 'triangle';
                    noteOsc.frequency.setValueAtTime(freq, now + index * 0.12);
                    noteGain.gain.setValueAtTime(0.15, now + index * 0.12);
                    noteGain.gain.exponentialRampToValueAtTime(0.001, now + index * 0.12 + 0.25);
                    noteOsc.connect(noteGain);
                    noteGain.connect(audioCtx.destination);
                    noteOsc.start(now + index * 0.12);
                    noteOsc.stop(now + index * 0.12 + 0.25);
                });
            }
        }

        function toggleSound() {
            soundEnabled = !soundEnabled;
            const icon = document.getElementById('sound-icon');
            if (soundEnabled) {
                icon.className = 'fa-solid fa-volume-high text-lg text-neon-green';
            } else {
                icon.className = 'fa-solid fa-volume-xmark text-lg text-gray-500';
            }
        }

        /* ================= Question Bank ================= */
        const quizData = {
            easy: [
                {
                    category: "นักเตะดังระดับโลก",
                    question: "นักฟุตบอลเจ้าของฉายา \"CR7\" ที่ปัจจุบันย้ายไปเล่นกับสโมสร อัล นาสเซอร์ คือใคร?",
                    options: [
                        "ลิโอเนล เมสซี่",
                        "คริสเตียโน โรนัลโด้",
                        "เนย์มาร์ จูเนียร์",
                        "คาริม เบนเซม่า"
                    ],
                    answer: 1,
                    explanation: "คริสเตียโน โรนัลโด้ สวมเสื้อหมายเลข 7 จนเป็นเครื่องหมายการค้า CR7 และย้ายซบ อัล นาสเซอร์ ในซาอุดีอาระเบีย"
                },
                {
                    category: "สโมสรฟุตบอล",
                    question: "สโมสรฟุตบอลใดในพรีเมียร์ลีกอังกฤษที่ใช้สนามเหย้าชื่อว่า \"แอนฟิลด์\" (Anfield)?",
                    options: [
                        "แมนเชสเตอร์ ยูไนเต็ด",
                        "อาร์เซน่อล",
                        "ลิเวอร์พูล",
                        "เชลซี"
                    ],
                    answer: 2,
                    explanation: "สนามแอนฟิลด์ เป็นรังเหย้าอันมนต์ขลังของสโมสรลิเวอร์พูลมาตั้งแต่ปี ค.ศ. 1892"
                },
                {
                    category: "ฟุตบอลโลก",
                    question: "นักเตะชาวอาร์เจนตินาผู้พาทีมชาติคว้าแชมป์ฟุตบอลโลก 2022 ที่กาตาร์ คือใคร?",
                    options: [
                        "อังเคล ดิ มาเรีย",
                        "เซร์คิโอ อเกวโร่",
                        "ลิโอเนล เมสซี่",
                        "เลาตาโร มาร์ติเนซ"
                    ],
                    answer: 2,
                    explanation: "ลิโอเนล เมสซี่ พาทีมชาติอาร์เจนตินาคว้าแชมป์โลก 2022 อย่างยิ่งใหญ่และได้รับรางวัลนักเตะยอดเยี่ยมประจำทัวร์นาเมนต์"
                },
                {
                    category: "ฟุตบอลโลก",
                    question: "ทีมชาติใดครองสถิติคว้าแชมป์ฟุตบอลโลกมากที่สุดในประวัติศาสตร์ (5 สมัย)?",
                    options: [
                        "เยอรมนี",
                        "อิตาลี",
                        "บราซิล",
                        "ฝรั่งเศส"
                    ],
                    answer: 2,
                    explanation: "บราซิล หรือฉายา \"แซมบ้า\" คว้าแชมป์โลกไปครองได้มากที่สุด 5 สมัย (1958, 1962, 1970, 1994, 2002)"
                },
                {
                    category: "ลีกฟุตบอล",
                    question: "ลีกฟุตบอลสูงสุดของประเทศสเปนมีชื่อเรียกว่าอะไร?",
                    options: [
                        "บุนเดสลีกา",
                        "ลาลีกา",
                        "กัลโช เซเรีย อา",
                        "ลีกเอิง"
                    ],
                    answer: 1,
                    explanation: "ลาลีกา (La Liga) คือการแข่งขันฟุตบอลลีกระดับสูงสุดของประเทศสเปน"
                }
            ],
            medium: [
                {
                    category: "ยูฟ่า แชมเปียนส์ลีก",
                    question: "สโมสรใดเป็นผู้ชนะเลิศคว้าแชมป์ ยูฟ่า แชมเปียนส์ลีก ประจำฤดูกาล 2023-24?",
                    options: [
                        "โบรุสเซีย ดอร์ทมุนด์",
                        "เรอัล มาดริด",
                        "แมนเชสเตอร์ ซิตี้",
                        "บาเยิร์น มิวนิค"
                    ],
                    answer: 1,
                    explanation: "เรอัล มาดริด เอาชนะ โบรุสเซีย ดอร์ทมุนด์ 2-0 ในรอบชิงชนะเลิศ คว้าแชมป์ยุโรปสมัยที่ 15 มาครอง"
                },
                {
                    category: "กุนซือระดับโลก",
                    question: "ใครคือผู้จัดการทีมชาวสเปนที่พา แมนเชสเตอร์ ซิตี้ คว้าแชมป์พรีเมียร์ลีกได้มากมาย?",
                    options: [
                        "ชาบี เอร์นานเดซ",
                        "มิเกล อาร์เตต้า",
                        "เป๊ป กวาร์ดิโอล่า",
                        "หลุยส์ เอ็นริเก้"
                    ],
                    answer: 2,
                    explanation: "เป๊ป กวาร์ดิโอล่า เข้ามารับตำแหน่งกุนซือแมนฯ ซิตี้ ตั้งแต่ปี 2016 และสร้างยุคยิ่งใหญ่พาทีมกวาดถ้วยรางวัลนับไม่ถ้วน"
                },
                {
                    category: "ตลาดซื้อขายนักเตะ",
                    question: "กองหน้าทีมชาติฝรั่งเศสชุดแชมป์โลก 2018 ที่ย้ายจาก PSG ไปร่วมทีม เรอัล มาดริด คือใคร?",
                    options: [
                        "อุสมาน เดมเบเล่",
                        "อองตวน กรีซมันน์",
                        "คีลิยัน เอ็มบัปเป้",
                        "มาร์คัส ตูราม"
                    ],
                    answer: 2,
                    explanation: "คีลิยัน เอ็มบัปเป้ ย้ายเข้าร่วมทีม เรอัล มาดริด แบบไม่มีค่าตัวในฤดูร้อนปี 2024"
                },
                {
                    category: "รางวัลยอดเยี่ยม",
                    question: "เจ้าของรางวัลบัลลงดอร์ (Ballon d'Or) ประจำปี 2024 ฝ่ายชาย คือกองกลางคนใด?",
                    options: [
                        "วินิซิอุส จูเนียร์",
                        "จู๊ด เบลลิงแฮม",
                        "โรดรี (Rodri)",
                        "เควิน เดอ บรอยน์"
                    ],
                    answer: 2,
                    explanation: "โรดรี กองกลางทีมชาติสเปนและแมนเชสเตอร์ ซิตี้ คว้ารางวัลบัลลงดอร์ 2024 จากผลงานยอดเยี่ยมทั้งระดับสโมสรและทีมชาติ"
                },
                {
                    category: "ฉายาสโมสร",
                    question: "สโมสรในพรีเมียร์ลีกอังกฤษฉายา \"เดอะ แม็กพายส์\" (The Magpies) คือทีมใด?",
                    options: [
                        "นิวคาสเซิล ยูไนเต็ด",
                        "คริสตัล พาเลซ",
                        "ฟูแล่ม",
                        "ไบรท์ตัน"
                    ],
                    answer: 0,
                    explanation: "นิวคาสเซิล ยูไนเต็ด มีฉายาว่า The Magpies (สาลิกาดง) จากชุดแข่งโทนสีขาว-ดำอันเป็นเอกลักษณ์"
                }
            ],
            hard: [
                {
                    category: "สถิติระดับโลก",
                    question: "ใครคือผู้ครองสถิต ยิงประตูสูงสุดตลอดกาล ในศึก ยูฟ่า แชมเปียนส์ลีก (140 ประตู)?",
                    options: [
                        "ลิโอเนล เมสซี่",
                        "คริสเตียโน โรนัลโด้",
                        "โรเบิร์ต เลวานดอฟสกี้",
                        "คาริม เบนเซม่า"
                    ],
                    answer: 1,
                    explanation: "คริสเตียโน โรนัลโด้ ยิงไปถึง 140 ประตูใน UCL นำเป็นอันดับ 1 ตลอดกาล โดยมี เมสซี่ ตามมาที่ 129 ประตู"
                },
                {
                    category: "ประวัติศาสตร์ฟุตบอล",
                    question: "ทีมชาติใดคือผู้ชนะเลิศการแข่งขันฟุตบอล ยูโร (UEFA Euro) ครั้งแรกในประวัติศาสตร์เมื่อปี 1960?",
                    options: [
                        "เยอรมนีตะวันตก",
                        "สหภาพโซเวียต",
                        "ฝรั่งเศส",
                        "ยูโกสลาเวีย"
                    ],
                    answer: 1,
                    explanation: "สหภาพโซเวียต (USSR) ชนะ ยูโกสลาเวีย 2-1 ในรอบชิงชนะเลิศที่ประเทศฝรั่งเศส คว้าแชมป์ยูโรครั้งแรกไปครอง"
                },
                {
                    category: "สโมสรในตำนาน",
                    question: "สโมสรใดใน กัลโช เซเรีย อา อิตาลี ที่ไม่เคยตกชั้น ลงสู่น้องใหม่ เซเรีย บี เลยนับตั้งแต่ก่อตั้ง?",
                    options: [
                        "ยูเวนตุส",
                        "เอซี มิลาน",
                        "อินเตอร์ มิลาน",
                        "โรม่า"
                    ],
                    answer: 2,
                    explanation: "อินเตอร์ มิลาน เป็นสโมสรเดียวในอิตาลีที่ลงเล่นบนลีกสูงสุด (Serie A) มาตลอดทุกฤดูกาลโดยไม่เคยตกชั้น"
                },
                {
                    category: "รางวัลบัลลงดอร์",
                    question: "ใครคือ ผู้รักษาประตู เพียงคนเดียวในประวัติศาสตร์ที่คว้ารางวัลบัลลงดอร์ (ปี 1963)?",
                    options: [
                        "จันลุยจี บุฟฟอน",
                        "เลฟ ยาชิน (Lev Yashin)",
                        "อิเคร์ กาซิยาส",
                        "โอลิเวอร์ คาห์น"
                    ],
                    answer: 1,
                    explanation: "เลฟ ยาชิน ตำนานจอมหนึบทีมชาติโซเวียต เจ้าของฉายา 'แมงมุมดำ' เป็นนายประตูคนเดียวที่ชนะรางวัล Ballon d'Or"
                },
                {
                    category: "เกร็ดฟุตบอลโลก",
                    question: "ในรอบชิงฟุตบอลโลก 1994 ใครคือคนยิงจุดโทษลูกสุดท้ายให้อิตาลีข้ามคานจนพ่ายบราซิล?",
                    options: [
                        "โรแบร์โต้ บาจโจ้",
                        "ฟรังโก บาเรซี่",
                        "ดานิเอเล มัสซาโร",
                        "อเลสซานโดร เดล ปิเอโร่"
                    ],
                    answer: 0,
                    explanation: "โรแบร์โต้ บาจโจ้ (Roberto Baggio) ซูเปอร์สตาร์ของอิตาลียิงจุดโทษเหินข้ามคาน ส่งผลให้บราซิลคว้าแชมป์โลกสมัยที่ 4"
                }
            ]
        };

        /* ================= State Variables ================= */
        let currentDifficulty = 'easy';
        let currentQuestions = [];
        let questionIndex = 0;
        let score = 0;
        let correctAnswersCount = 0;
        let streakCount = 0;

        let timerInterval = null;
        const QUESTION_TIME_LIMIT = 15; // 15 seconds per question
        let timeLeft = QUESTION_TIME_LIMIT;

        /* ================= Core Functions ================= */
        
        function showScreen(screenId) {
            document.getElementById('level-screen').classList.add('hidden');
            document.getElementById('quiz-screen').classList.add('hidden');
            document.getElementById('result-screen').classList.add('hidden');

            document.getElementById(screenId).classList.remove('hidden');
            document.getElementById(screenId).classList.add('flex');
            playSound('click');
        }

        function startQuiz(difficulty) {
            currentDifficulty = difficulty;
            currentQuestions = [...quizData[difficulty]];
            questionIndex = 0;
            score = 0;
            correctAnswersCount = 0;
            streakCount = 0;

            // Set UI Badge for level
            const diffBadge = document.getElementById('difficulty-badge');
            if (difficulty === 'easy') {
                diffBadge.textContent = '🟢 ระดับง่าย';
                diffBadge.className = 'text-xs font-bold px-3 py-1 rounded-full bg-emerald-500/20 text-neon-green border border-neon-green/30';
            } else if (difficulty === 'medium') {
                diffBadge.textContent = '🟡 ระดับปานกลาง';
                diffBadge.className = 'text-xs font-bold px-3 py-1 rounded-full bg-amber-500/20 text-neon-yellow border border-neon-yellow/30';
            } else {
                diffBadge.textContent = '🔴 ระดับยาก';
                diffBadge.className = 'text-xs font-bold px-3 py-1 rounded-full bg-rose-500/20 text-neon-red border border-neon-red/30';
            }

            showScreen('quiz-screen');
            loadQuestion();
        }

        function loadQuestion() {
            clearInterval(timerInterval);
            
            const q = currentQuestions[questionIndex];

            // Update Header Stats
            document.getElementById('question-tracker').textContent = `${questionIndex + 1} / ${currentQuestions.length}`;
            document.getElementById('score-text').textContent = score.toLocaleString();
            document.getElementById('question-category').innerHTML = `<i class="fa-solid fa-trophy text-neon-yellow"></i> ${q.category}`;
            document.getElementById('question-text').textContent = q.question;

            // Streak Multiplier Display
            const streakBadge = document.getElementById('streak-badge');
            if (streakCount >= 2) {
                streakBadge.textContent = `x${streakCount}`;
                streakBadge.classList.remove('hidden');
            } else {
                streakBadge.classList.add('hidden');
            }

            // Render Options
            const container = document.getElementById('options-container');
            container.innerHTML = '';
            
            const optionLabels = ['A', 'B', 'C', 'D'];
            
            q.options.forEach((opt, idx) => {
                const btn = document.createElement('button');
                btn.onclick = () => checkAnswer(idx);
                btn.className = `option-btn w-full p-4 rounded-xl glass-panel text-left flex items-center justify-between group hover:bg-white/5 border border-white/10`;
                btn.innerHTML = `
                    <div class="flex items-center space-x-3">
                        <span class="w-8 h-8 rounded-lg bg-gray-800 text-gray-300 flex items-center justify-center font-bold text-sm group-hover:bg-neon-green group-hover:text-pitch-900 transition-colors">${optionLabels[idx]}</span>
                        <span class="text-base font-medium text-white">${opt}</span>
                    </div>
                    <i class="fa-regular fa-circle text-gray-500 text-lg group-hover:text-neon-green"></i>
                `;
                container.appendChild(btn);
            });

            // Hide Explanation & Next button
            document.getElementById('explanation-box').classList.add('hidden');
            document.getElementById('next-btn').classList.add('hidden');

            // Reset Timer
            startTimer();
        }

        function startTimer() {
            timeLeft = QUESTION_TIME_LIMIT;
            const timerText = document.getElementById('timer-text');
            const timerBar = document.getElementById('timer-bar');

            timerText.textContent = `${timeLeft}s`;
            timerBar.style.width = '100%';

            timerInterval = setInterval(() => {
                timeLeft--;
                timerText.textContent = `${timeLeft}s`;
                
                const percent = (timeLeft / QUESTION_TIME_LIMIT) * 100;
                timerBar.style.width = `${percent}%`;

                if (timeLeft <= 0) {
                    clearInterval(timerInterval);
                    handleTimeOut();
                }
            }, 1000);
        }

        function checkAnswer(selectedIndex) {
            clearInterval(timerInterval);
            playSound('click');

            const q = currentQuestions[questionIndex];
            const isCorrect = (selectedIndex === q.answer);
            const buttons = document.querySelectorAll('.option-btn');

            // Disable all buttons after choice
            buttons.forEach(btn => btn.disabled = true);

            if (isCorrect) {
                playSound('correct');
                correctAnswersCount++;
                streakCount++;

                // Score Calculation: Base 300 points + Time Bonus + Streak Bonus
                const baseScore = 300;
                const timeBonus = timeLeft * 20;
                const multiplier = Math.min(streakCount, 3);
                const gainedScore = (baseScore + timeBonus) * multiplier;
                score += gainedScore;

                document.getElementById('score-text').textContent = score.toLocaleString();

                // Highlight Correct Button
                const selectedBtn = buttons[selectedIndex];
                selectedBtn.className = "option-btn w-full p-4 rounded-xl bg-emerald-950/80 border-2 border-neon-green text-left flex items-center justify-between shadow-neon-green";
                selectedBtn.querySelector('i').className = "fa-solid fa-circle-check text-neon-green text-xl";

            } else {
                playSound('wrong');
                streakCount = 0; // Reset streak

                // Highlight Selected Wrong Button
                if (selectedIndex !== null) {
                    const selectedBtn = buttons[selectedIndex];
                    selectedBtn.className = "option-btn w-full p-4 rounded-xl bg-rose-950/80 border-2 border-neon-red text-left flex items-center justify-between shadow-neon-red";
                    selectedBtn.querySelector('i').className = "fa-solid fa-circle-xmark text-neon-red text-xl";
                }

                // Show Real Correct Button
                const correctBtn = buttons[q.answer];
                correctBtn.className = "option-btn w-full p-4 rounded-xl bg-emerald-950/40 border border-neon-green/60 text-left flex items-center justify-between";
                correctBtn.querySelector('i').className = "fa-solid fa-circle-check text-neon-green text-xl";
            }

            // Show Explanation Box
            const expBox = document.getElementById('explanation-box');
            const expTitle = document.getElementById('explanation-title');
            const expText = document.getElementById('explanation-text');

            if (isCorrect) {
                expTitle.className = "flex items-center gap-2 font-bold mb-1 text-neon-green";
                expTitle.innerHTML = `<i class="fa-solid fa-circle-check"></i> ถูกต้องแม่นยำ!`;
            } else {
                expTitle.className = "flex items-center gap-2 font-bold mb-1 text-neon-red";
                expTitle.innerHTML = `<i class="fa-solid fa-circle-xmark"></i> ยังไม่ถูกต้อง`;
            }

            expText.textContent = q.explanation;
            expBox.classList.remove('hidden');

            // Show Next Button
            document.getElementById('next-btn').classList.remove('hidden');
        }

        function handleTimeOut() {
            playSound('wrong');
            streakCount = 0;

            const buttons = document.querySelectorAll('.option-btn');
            buttons.forEach(btn => btn.disabled = true);

            const q = currentQuestions[questionIndex];
            const correctBtn = buttons[q.answer];
            correctBtn.className = "option-btn w-full p-4 rounded-xl bg-emerald-950/40 border border-neon-green/60 text-left flex items-center justify-between";
            correctBtn.querySelector('i').className = "fa-solid fa-circle-check text-neon-green text-xl";

            // Show Explanation Box
            const expBox = document.getElementById('explanation-box');
            const expTitle = document.getElementById('explanation-title');
            const expText = document.getElementById('explanation-text');

            expTitle.className = "flex items-center gap-2 font-bold mb-1 text-neon-yellow";
            expTitle.innerHTML = `<i class="fa-solid fa-clock"></i> หมดเวลาตอบ!`;
            expText.textContent = q.explanation;
            expBox.classList.remove('hidden');

            document.getElementById('next-btn').classList.remove('hidden');
        }

        function nextQuestion() {
            questionIndex++;
            if (questionIndex < currentQuestions.length) {
                loadQuestion();
            } else {
                showResults();
            }
        }

        function showResults() {
            showScreen('result-screen');
            playSound('victory');

            const total = currentQuestions.length;
            const percentage = (correctAnswersCount / total) * 100;

            document.getElementById('result-correct-count').textContent = `${correctAnswersCount} / ${total}`;
            document.getElementById('result-percentage').textContent = `(${percentage.toFixed(0)}%)`;
            document.getElementById('result-final-score').textContent = score.toLocaleString();

            // Set Title Rank & Icon based on Score
            const rankTitle = document.getElementById('rank-title');
            const rankDesc = document.getElementById('rank-desc');
            const rankIcon = document.getElementById('rank-icon');

            if (percentage === 100) {
                rankIcon.textContent = "👑";
                rankTitle.textContent = "ตำนานลูกหนังโลก!";
                rankDesc.textContent = "สมบูรณ์แบบ! คุณรู้ลึกรู้จริงทุกเกร็ดฟุตบอล ไร้ข้อพกพร่อง!";
            } else if (percentage >= 80) {
                rankIcon.textContent = "🌟";
                rankTitle.textContent = "ดาวรุ่งพุ่งแรง!";
                rankDesc.textContent = "ฝีมือไม่ธรรมดา! ความรู้แน่นปึ้ก อีกก้าวเดียวจะกลายเป็นตำนาน!";
            } else if (percentage >= 60) {
                rankIcon.textContent = "⚽";
                rankTitle.textContent = "ผู้เล่นตัวจริง!";
                rankDesc.textContent = "ทำได้ดี! คุณเป็นแฟนบอลที่ติดตามข่าวสารอย่างใกล้ชิด!";
            } else {
                rankIcon.textContent = "🪑";
                rankTitle.textContent = "ตัวสำรองข้างสนาม!";
                rankDesc.textContent = "ลองฝึกซ้อมอีกนิด แล้วกลับมาแก้มือใหม่ในรอบหน้านะ!";
            }
        }

        function retryCurrentLevel() {
            startQuiz(currentDifficulty);
        }

        // Initialize App on load
        window.onload = function() {
            // First screen is level selector
            showScreen('level-screen');
        };
    </script>
</body>
</html>
