<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Vani AI | Text to Speech тАУ English & Hindi (Fully Functional)</title>
    <!-- Tailwind + Font Awesome -->
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        body {
            background: linear-gradient(145deg, #0f172a 0%, #0a0f1a 100%);
            min-height: 100vh;
        }
        .glass-card {
            background: rgba(15, 25, 45, 0.7);
            backdrop-filter: blur(12px);
            border: 1px solid rgba(255,255,255,0.1);
            border-radius: 2rem;
        }
        .btn-primary {
            background: linear-gradient(105deg, #3b82f6, #2563eb);
            transition: 0.2s;
            box-shadow: 0 8px 14px -6px rgba(59,130,246,0.4);
        }
        .btn-primary:hover {
            transform: translateY(-2px);
            box-shadow: 0 12px 20px -8px rgba(59,130,246,0.6);
        }
        .btn-outline {
            background: rgba(255,255,255,0.05);
            border: 1px solid rgba(255,255,255,0.2);
        }
        .btn-outline:hover {
            background: rgba(255,255,255,0.15);
        }
        select, textarea {
            background: rgba(0,0,0,0.3);
            border: 1px solid #334155;
            border-radius: 1rem;
            transition: all 0.2s;
        }
        select:focus, textarea:focus {
            border-color: #3b82f6;
            outline: none;
            box-shadow: 0 0 0 2px rgba(59,130,246,0.3);
        }
        .range-slider {
            -webkit-appearance: none;
            width: 100%;
            height: 4px;
            background: #334155;
            border-radius: 4px;
        }
        .range-slider::-webkit-slider-thumb {
            -webkit-appearance: none;
            width: 16px;
            height: 16px;
            background: #3b82f6;
            border-radius: 50%;
            cursor: pointer;
        }
        .status-badge {
            transition: all 0.2s;
        }
        @keyframes pulse {
            0%,100% { opacity: 1; }
            50% { opacity: 0.6; }
        }
        .speaking-active {
            animation: pulse 1s infinite;
        }
    </style>
</head>
<body class="text-gray-200 font-sans antialiased">
    <div class="max-w-4xl mx-auto px-4 py-8 md:py-12">
        <!-- Header -->
        <div class="text-center mb-8">
            <div class="inline-flex items-center gap-2 bg-white/10 backdrop-blur-sm rounded-full px-4 py-1.5 border border-white/20 mb-3">
                <i class="fas fa-microphone-alt text-blue-400"></i>
                <span class="text-xs font-mono tracking-wider text-blue-200">WEB SPEECH API ┬╖ MULTILINGUAL</span>
            </div>
            <h1 class="text-5xl md:text-6xl font-bold bg-gradient-to-r from-blue-300 via-indigo-300 to-purple-300 bg-clip-text text-transparent">
                Vani <span class="text-white">AI</span>
            </h1>
            <p class="text-gray-300 mt-2">English ┬╖ рд╣рд┐рдиреНрджреА ┬╖ Adjustable Voice & Speed</p>
        </div>

        <div class="glass-card p-6 md:p-8">
            <!-- Language & Voice Selection -->
            <div class="grid md:grid-cols-2 gap-6 mb-6">
                <div>
                    <label class="block text-sm font-semibold mb-2 flex items-center gap-2">
                        <i class="fas fa-language text-indigo-400"></i> Language
                    </label>
                    <select id="languageSelect" class="w-full p-2.5">
                        <option value="en-US">ЁЯЗ║ЁЯЗ╕ English (US)</option>
                        <option value="hi-IN">ЁЯЗоЁЯЗ│ рд╣рд┐рдиреНрджреА (Hindi)</option>
                        <option value="en-GB">ЁЯЗмЁЯЗз English (UK)</option>
                    </select>
                    <p id="hindiWarning" class="text-xs text-amber-400 mt-1 hidden">
                        тЪая╕П Hindi voice may not be available. System will use default (still works for Hindi text).
                    </p>
                </div>
                <div>
                    <label class="block text-sm font-semibold mb-2 flex items-center gap-2">
                        <i class="fas fa-user-astronaut text-green-400"></i> Voice
                    </label>
                    <select id="voiceSelect" class="w-full p-2.5">
                        <option value="">ЁЯдЦ Auto (Best match)</option>
                    </select>
                    <button id="refreshVoicesBtn" class="text-xs text-blue-300 mt-1 flex items-center gap-1 hover:text-blue-200">
                        <i class="fas fa-sync-alt"></i> Refresh voices
                    </button>
                </div>
            </div>

            <!-- Text Input -->
            <div class="mb-6">
                <label class="block text-sm font-semibold mb-2 flex items-center gap-2">
                    <i class="fas fa-edit text-cyan-400"></i> Enter text
                </label>
                <textarea id="textInput" rows="5" class="w-full p-3 text-gray-200 placeholder-gray-500 rounded-xl" placeholder="Type in English or Hindi...&#10;Example: рдирдорд╕реНрддреЗ, рдореИрдВ рд╣рд┐рдВрджреА рдореЗрдВ рдмреЛрд▓ рд╕рдХрддрд╛ рд╣реВрдБред"></textarea>
                <div class="flex justify-between mt-2 text-xs text-gray-400">
                    <span id="charCount">0 characters</span>
                    <button id="clearBtn" class="hover:text-white"><i class="fas fa-eraser"></i> Clear</button>
                </div>
            </div>

            <!-- Rate & Pitch Controls -->
            <div class="grid md:grid-cols-2 gap-6 mb-8">
                <div>
                    <label class="block text-sm font-semibold mb-2">ЁЯФК Speed (Rate)</label>
                    <div class="flex items-center gap-3">
                        <span class="text-xs">ЁЯРв</span>
                        <input type="range" id="rateSlider" min="0.5" max="2" step="0.05" value="1" class="range-slider flex-1">
                        <span class="text-xs">ЁЯРЗ</span>
                    </div>
                    <div class="text-center text-sm mt-1"><span id="rateValue">1.0</span>x</div>
                </div>
                <div>
                    <label class="block text-sm font-semibold mb-2">ЁЯО╡ Pitch</label>
                    <div class="flex items-center gap-3">
                        <span class="text-xs">ЁЯФ╗</span>
                        <input type="range" id="pitchSlider" min="0.5" max="2" step="0.05" value="1" class="range-slider flex-1">
                        <span class="text-xs">ЁЯФ║</span>
                    </div>
                    <div class="text-center text-sm mt-1"><span id="pitchValue">1.0</span></div>
                </div>
            </div>

            <!-- Action Buttons -->
            <div class="flex flex-wrap gap-3 justify-center mb-6">
                <button id="speakBtn" class="btn-primary px-8 py-2.5 rounded-full font-semibold flex items-center gap-2 shadow-lg">
                    <i class="fas fa-play"></i> Speak
                </button>
                <button id="pauseBtn" class="btn-outline px-5 py-2.5 rounded-full flex items-center gap-2">
                    <i class="fas fa-pause"></i> Pause
                </button>
                <button id="resumeBtn" class="btn-outline px-5 py-2.5 rounded-full flex items-center gap-2">
                    <i class="fas fa-play-circle"></i> Resume
                </button>
                <button id="stopBtn" class="btn-outline px-5 py-2.5 rounded-full flex items-center gap-2">
                    <i class="fas fa-stop"></i> Stop
                </button>
            </div>

            <!-- Status Panel -->
            <div class="flex items-center justify-between flex-wrap gap-2 pt-2 border-t border-white/10 text-sm">
                <div id="status" class="flex items-center gap-2 text-gray-300">
                    <i class="fas fa-circle-info text-blue-300"></i>
                    <span>Ready</span>
                </div>
                <div class="text-xs text-gray-400 flex gap-3">
                    <span><i class="fas fa-check-circle text-green-500"></i> Works offline</span>
                    <span><i class="fas fa-globe"></i> Hindi & English</span>
                </div>
            </div>
        </div>

        <footer class="text-center text-gray-500 text-xs mt-6">
            <p>ЁЯТб Tip: If you don't hear Hindi voices, the system will still read Hindi text using a default voice. For best Hindi experience, use Chrome/Edge on Windows/Mac with Hindi TTS installed.</p>
        </footer>
    </div>

    <script>
        // DOM Elements
        const languageSelect = document.getElementById('languageSelect');
        const voiceSelect = document.getElementById('voiceSelect');
        const textInput = document.getElementById('textInput');
        const rateSlider = document.getElementById('rateSlider');
        const pitchSlider = document.getElementById('pitchSlider');
        const rateValue = document.getElementById('rateValue');
        const pitchValue = document.getElementById('pitchValue');
        const speakBtn = document.getElementById('speakBtn');
        const pauseBtn = document.getElementById('pauseBtn');
        const resumeBtn = document.getElementById('resumeBtn');
        const stopBtn = document.getElementById('stopBtn');
        const clearBtn = document.getElementById('clearBtn');
        const charCount = document.getElementById('charCount');
        const statusSpan = document.getElementById('status');
        const hindiWarning = document.getElementById('hindiWarning');
        const refreshVoicesBtn = document.getElementById('refreshVoicesBtn');

        // Global variables
        let voices = [];
        let currentUtterance = null;
        let isPaused = false;

        // Update character count
        function updateCharCount() {
            const len = textInput.value.length;
            charCount.textContent = `${len} characters`;
        }
        textInput.addEventListener('input', updateCharCount);
        updateCharCount();

        // Clear text
        clearBtn.addEventListener('click', () => {
            textInput.value = '';
            updateCharCount();
            stopSpeech();
            setStatus('Text cleared', 'info');
        });

        // Helper to set status with icon
        function setStatus(message, type = 'info') {
            let icon = '';
            switch(type) {
                case 'speaking': icon = '<i class="fas fa-volume-up text-green-400 animate-pulse"></i>'; break;
                case 'error': icon = '<i class="fas fa-exclamation-triangle text-red-400"></i>'; break;
                case 'success': icon = '<i class="fas fa-check-circle text-green-400"></i>'; break;
                default: icon = '<i class="fas fa-circle-info text-blue-300"></i>';
            }
            statusSpan.innerHTML = `${icon} <span>${message}</span>`;
        }

        // Load voices and populate dropdown
        function loadVoices() {
            voices = window.speechSynthesis.getVoices();
            if (voices.length === 0) {
                // Voices not loaded yet, try again in a bit
                setTimeout(loadVoices, 200);
                return;
            }
            populateVoiceDropdown();
            checkHindiVoiceAvailability();
        }

        function populateVoiceDropdown() {
            const selectedLang = languageSelect.value;
            const filtered = voices.filter(v => v.lang.startsWith(selectedLang));
            voiceSelect.innerHTML = '<option value="">ЁЯдЦ Auto (Best match)</option>';
            if (filtered.length === 0) {
                // Show all voices but mark them
                voices.forEach(voice => {
                    const option = document.createElement('option');
                    option.value = voice.name;
                    option.textContent = `${voice.name} (${voice.lang})`;
                    voiceSelect.appendChild(option);
                });
            } else {
                filtered.forEach(voice => {
                    const option = document.createElement('option');
                    option.value = voice.name;
                    option.textContent = `${voice.name} (${voice.lang})`;
                    voiceSelect.appendChild(option);
                });
            }
        }

        function checkHindiVoiceAvailability() {
            const hasHindi = voices.some(v => v.lang.startsWith('hi-IN'));
            if (!hasHindi) {
                hindiWarning.classList.remove('hidden');
            } else {
                hindiWarning.classList.add('hidden');
            }
        }

        // Refresh voices manually
        function refreshVoices() {
            window.speechSynthesis.cancel(); // cancel any ongoing
            // Force voice list reload
            window.speechSynthesis.getVoices(); // sometimes forces refresh
            setTimeout(() => {
                loadVoices();
                setStatus('Voices refreshed', 'success');
            }, 100);
        }

        // Get selected voice object
        function getSelectedVoice() {
            const selectedName = voiceSelect.value;
            if (!selectedName) return null;
            return voices.find(v => v.name === selectedName) || null;
        }

        // Core speech function
        function speak() {
            const text = textInput.value.trim();
            if (text === "") {
                setStatus("Please enter some text", "error");
                return;
            }

            // Cancel any ongoing speech to avoid queue conflicts
            if (window.speechSynthesis.speaking || window.speechSynthesis.pending) {
                window.speechSynthesis.cancel();
                if (currentUtterance) {
                    currentUtterance = null;
                }
                // small delay to allow cancel to complete
                setTimeout(() => performSpeak(text), 50);
            } else {
                performSpeak(text);
            }
        }

        function performSpeak(text) {
            const utterance = new SpeechSynthesisUtterance(text);
            // Set language
            utterance.lang = languageSelect.value;
            // Set rate & pitch
            utterance.rate = parseFloat(rateSlider.value);
            utterance.pitch = parseFloat(pitchSlider.value);

            // Set voice if manually selected
            const selectedVoice = getSelectedVoice();
            if (selectedVoice) {
                utterance.voice = selectedVoice;
            } else {
                // Auto: try to find a voice that matches the language
                const autoVoice = voices.find(v => v.lang.startsWith(utterance.lang));
                if (autoVoice) utterance.voice = autoVoice;
            }

            // Event handlers
            utterance.onstart = () => {
                isPaused = false;
                setStatus("Speaking...", "speaking");
                speakBtn.classList.add('speaking-active');
            };
            utterance.onend = () => {
                isPaused = false;
                setStatus("Finished", "success");
                speakBtn.classList.remove('speaking-active');
                currentUtterance = null;
            };
            utterance.onerror = (event) => {
                console.error('Speech error:', event);
                let errorMsg = "Error occurred";
                if (event.error === 'not-allowed') errorMsg = "Speech not allowed. Please check permissions.";
                else if (event.error === 'language-unavailable') errorMsg = "Language not supported on this device.";
                else errorMsg = `Error: ${event.error}`;
                setStatus(errorMsg, "error");
                speakBtn.classList.remove('speaking-active');
                currentUtterance = null;
            };
            utterance.onpause = () => {
                isPaused = true;
                setStatus("Paused", "info");
            };
            utterance.onresume = () => {
                isPaused = false;
                setStatus("Speaking...", "speaking");
            };

            currentUtterance = utterance;
            window.speechSynthesis.speak(utterance);
        }

        function pauseSpeech() {
            if (window.speechSynthesis.speaking && !window.speechSynthesis.paused) {
                window.speechSynthesis.pause();
                setStatus("Paused", "info");
            } else if (!window.speechSynthesis.speaking) {
                setStatus("No speech to pause", "error");
            } else if (window.speechSynthesis.paused) {
                setStatus("Already paused", "info");
            }
        }

        function resumeSpeech() {
            if (window.speechSynthesis.paused) {
                window.speechSynthesis.resume();
                setStatus("Resumed", "speaking");
            } else if (window.speechSynthesis.speaking) {
                setStatus("Already speaking", "info");
            } else {
                // If nothing is playing, start new speech
                speak();
            }
        }

        function stopSpeech() {
            if (window.speechSynthesis.speaking || window.speechSynthesis.pending) {
                window.speechSynthesis.cancel();
                setStatus("Stopped", "info");
                speakBtn.classList.remove('speaking-active');
                if (currentUtterance) {
                    currentUtterance = null;
                }
            } else {
                setStatus("No speech to stop", "info");
            }
        }

        // Slider display updates
        rateSlider.addEventListener('input', () => {
            rateValue.textContent = rateSlider.value;
        });
        pitchSlider.addEventListener('input', () => {
            pitchValue.textContent = pitchSlider.value;
        });

        // Language change updates voice dropdown
        languageSelect.addEventListener('change', () => {
            populateVoiceDropdown();
            checkHindiVoiceAvailability();
            // Optionally stop any current speech when language changes
            stopSpeech();
            setStatus("Language changed", "info");
        });

        // Button events
        speakBtn.addEventListener('click', speak);
        pauseBtn.addEventListener('click', pauseSpeech);
        resumeBtn.addEventListener('click', resumeSpeech);
        stopBtn.addEventListener('click', stopSpeech);
        refreshVoicesBtn.addEventListener('click', refreshVoices);

        // Load voices when page loads
        if (window.speechSynthesis.onvoiceschanged !== undefined) {
            window.speechSynthesis.onvoiceschanged = () => {
                loadVoices();
            };
        }
        loadVoices(); // initial call

        // Fallback: if voices not loaded after 1 sec, retry
        setTimeout(() => {
            if (voices.length === 0) loadVoices();
        }, 1000);

        // Set initial slider values
        rateValue.textContent = rateSlider.value;
        pitchValue.textContent = pitchSlider.value;

        // Add a demo text if empty
        if (textInput.value === "") {
            textInput.value = "Namaste! Welcome to Vani AI. рдЖрдк рд╣рд┐рдВрджреА рдореЗрдВ рднреА рдЯрд╛рдЗрдк рдХрд░ рд╕рдХрддреЗ рд╣реИрдВ рдФрд░ рдореИрдВ рд╕реНрдкрд╖реНрдЯ рдЖрд╡рд╛рдЬрд╝ рдореЗрдВ рдкрдврд╝реВрдВрдЧрд╛ред";
            updateCharCount();
        }
    </script>
</body>
</html>
