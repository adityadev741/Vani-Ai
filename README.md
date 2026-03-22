<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes">
    <title>Vani AI | Text to Speech with MP3 Export – English & Hindi</title>
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
        .btn-download {
            background: linear-gradient(105deg, #10b981, #059669);
        }
        .btn-download:hover {
            background: linear-gradient(105deg, #34d399, #10b981);
            transform: translateY(-2px);
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
        @keyframes pulse {
            0%,100% { opacity: 1; }
            50% { opacity: 0.6; }
        }
        .speaking-active {
            animation: pulse 1s infinite;
        }
        .progress-bar {
            transition: width 0.3s ease;
        }
    </style>
</head>
<body class="text-gray-200 font-sans antialiased">
    <div class="max-w-4xl mx-auto px-4 py-8 md:py-12">
        <!-- Header -->
        <div class="text-center mb-8">
            <div class="inline-flex items-center gap-2 bg-white/10 backdrop-blur-sm rounded-full px-4 py-1.5 border border-white/20 mb-3">
                <i class="fas fa-microphone-alt text-blue-400"></i>
                <span class="text-xs font-mono tracking-wider text-blue-200">LIVE SPEECH · MP3 EXPORT</span>
            </div>
            <h1 class="text-5xl md:text-6xl font-bold bg-gradient-to-r from-blue-300 via-indigo-300 to-purple-300 bg-clip-text text-transparent">
                Vani <span class="text-white">AI</span>
            </h1>
            <p class="text-gray-300 mt-2">English · हिन्दी · Speak & Download High-Quality MP3</p>
        </div>

        <div class="glass-card p-6 md:p-8">
            <!-- Language & Voice Selection -->
            <div class="grid md:grid-cols-2 gap-6 mb-6">
                <div>
                    <label class="block text-sm font-semibold mb-2 flex items-center gap-2">
                        <i class="fas fa-language text-indigo-400"></i> Language
                    </label>
                    <select id="languageSelect" class="w-full p-2.5">
                        <option value="en-US">🇺🇸 English (US)</option>
                        <option value="hi-IN">🇮🇳 हिन्दी (Hindi)</option>
                        <option value="en-GB">🇬🇧 English (UK)</option>
                    </select>
                    <p id="hindiWarning" class="text-xs text-amber-400 mt-1 hidden">
                        ⚠️ Hindi live voice may not be available. Download works perfectly.
                    </p>
                </div>
                <div>
                    <label class="block text-sm font-semibold mb-2 flex items-center gap-2">
                        <i class="fas fa-user-astronaut text-green-400"></i> Live Voice
                    </label>
                    <select id="voiceSelect" class="w-full p-2.5">
                        <option value="">🤖 Auto (Best match)</option>
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
                <textarea id="textInput" rows="5" class="w-full p-3 text-gray-200 placeholder-gray-500 rounded-xl" placeholder="Type in English or Hindi...&#10;Example: नमस्ते, मैं हिंदी में बोल सकता हूँ।"></textarea>
                <div class="flex justify-between mt-2 text-xs text-gray-400">
                    <span id="charCount">0 characters</span>
                    <button id="clearBtn" class="hover:text-white"><i class="fas fa-eraser"></i> Clear</button>
                </div>
            </div>

            <!-- Rate & Pitch Controls (live speech only) -->
            <div class="grid md:grid-cols-2 gap-6 mb-8">
                <div>
                    <label class="block text-sm font-semibold mb-2">🔊 Speed (Rate)</label>
                    <div class="flex items-center gap-3">
                        <span class="text-xs">🐢</span>
                        <input type="range" id="rateSlider" min="0.5" max="2" step="0.05" value="1" class="range-slider flex-1">
                        <span class="text-xs">🐇</span>
                    </div>
                    <div class="text-center text-sm mt-1"><span id="rateValue">1.0</span>x</div>
                </div>
                <div>
                    <label class="block text-sm font-semibold mb-2">🎵 Pitch</label>
                    <div class="flex items-center gap-3">
                        <span class="text-xs">🔻</span>
                        <input type="range" id="pitchSlider" min="0.5" max="2" step="0.05" value="1" class="range-slider flex-1">
                        <span class="text-xs">🔺</span>
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
                <button id="downloadBtn" class="btn-download px-5 py-2.5 rounded-full font-semibold flex items-center gap-2 shadow-lg">
                    <i class="fas fa-download"></i> Download MP3
                </button>
            </div>

            <!-- Download Progress -->
            <div id="downloadProgress" class="hidden mb-4">
                <div class="flex justify-between text-xs mb-1">
                    <span>Generating MP3...</span>
                    <span id="progressPercent">0%</span>
                </div>
                <div class="w-full bg-slate-700 rounded-full h-1.5">
                    <div id="progressBar" class="bg-green-500 h-1.5 rounded-full progress-bar" style="width:0%"></div>
                </div>
            </div>

            <!-- Status Panel -->
            <div class="flex items-center justify-between flex-wrap gap-2 pt-2 border-t border-white/10 text-sm">
                <div id="status" class="flex items-center gap-2 text-gray-300">
                    <i class="fas fa-circle-info text-blue-300"></i>
                    <span>Ready</span>
                </div>
                <div class="text-xs text-gray-400 flex gap-3">
                    <span><i class="fas fa-check-circle text-green-500"></i> Live speech</span>
                    <span><i class="fas fa-cloud-download-alt"></i> MP3 export</span>
                </div>
            </div>
        </div>

        <footer class="text-center text-gray-500 text-xs mt-6">
            <p>💡 <strong>Download MP3:</strong> Uses StreamElements TTS – high quality, supports English & Hindi. Long text split automatically.</p>
            <p>🎙️ <strong>Live speech:</strong> Uses browser's built-in voices. Adjust speed & pitch.</p>
            <p class="mt-1">🔊 If download fails, check your internet connection or try again later.</p>
        </footer>
    </div>

    <script>
        // Wait for DOM to be fully loaded
        document.addEventListener('DOMContentLoaded', function() {
            // ==================== DOM Elements ====================
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
            const downloadBtn = document.getElementById('downloadBtn');
            const clearBtn = document.getElementById('clearBtn');
            const charCount = document.getElementById('charCount');
            const statusSpan = document.getElementById('status');
            const hindiWarning = document.getElementById('hindiWarning');
            const refreshVoicesBtn = document.getElementById('refreshVoicesBtn');
            const downloadProgressDiv = document.getElementById('downloadProgress');
            const progressBar = document.getElementById('progressBar');
            const progressPercent = document.getElementById('progressPercent');

            // ==================== Helper Functions ====================
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

            function updateCharCount() {
                const len = textInput.value.length;
                charCount.textContent = `${len} characters`;
            }
            textInput.addEventListener('input', updateCharCount);
            updateCharCount();
            clearBtn.addEventListener('click', () => {
                textInput.value = '';
                updateCharCount();
                stopSpeech();
                setStatus('Text cleared', 'info');
            });

            // ==================== LIVE SPEECH (Web Speech API) ====================
            let voices = [];
            let currentUtterance = null;

            // Check if speech synthesis is supported
            if (!window.speechSynthesis) {
                setStatus('Your browser does not support speech synthesis.', 'error');
                speakBtn.disabled = true;
                pauseBtn.disabled = true;
                resumeBtn.disabled = true;
                stopBtn.disabled = true;
            }

            function loadVoices() {
                voices = window.speechSynthesis.getVoices();
                if (voices.length === 0) {
                    setTimeout(loadVoices, 200);
                    return;
                }
                populateVoiceDropdown();
                checkHindiVoiceAvailability();
            }

            function populateVoiceDropdown() {
                const selectedLang = languageSelect.value;
                const filtered = voices.filter(v => v.lang.startsWith(selectedLang));
                voiceSelect.innerHTML = '<option value="">🤖 Auto (Best match)</option>';
                if (filtered.length === 0) {
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

            function refreshVoices() {
                if (window.speechSynthesis) {
                    window.speechSynthesis.cancel();
                    window.speechSynthesis.getVoices(); // trigger reload
                    setTimeout(() => {
                        loadVoices();
                        setStatus('Voices refreshed', 'success');
                    }, 200);
                }
            }

            function getSelectedVoice() {
                const selectedName = voiceSelect.value;
                if (!selectedName) return null;
                return voices.find(v => v.name === selectedName) || null;
            }

            function speak() {
                const text = textInput.value.trim();
                if (text === "") {
                    setStatus("Please enter some text", "error");
                    return;
                }
                if (!window.speechSynthesis) {
                    setStatus("Speech synthesis not supported", "error");
                    return;
                }
                // Cancel any ongoing speech
                if (window.speechSynthesis.speaking || window.speechSynthesis.pending) {
                    window.speechSynthesis.cancel();
                    setTimeout(() => performSpeak(text), 100);
                } else {
                    performSpeak(text);
                }
            }

            function performSpeak(text) {
                const utterance = new SpeechSynthesisUtterance(text);
                utterance.lang = languageSelect.value;
                utterance.rate = parseFloat(rateSlider.value);
                utterance.pitch = parseFloat(pitchSlider.value);
                const selectedVoice = getSelectedVoice();
                if (selectedVoice) utterance.voice = selectedVoice;
                else {
                    const autoVoice = voices.find(v => v.lang.startsWith(utterance.lang));
                    if (autoVoice) utterance.voice = autoVoice;
                }
                utterance.onstart = () => {
                    setStatus("Speaking...", "speaking");
                    speakBtn.classList.add('speaking-active');
                };
                utterance.onend = () => {
                    setStatus("Finished", "success");
                    speakBtn.classList.remove('speaking-active');
                    currentUtterance = null;
                };
                utterance.onerror = (event) => {
                    setStatus(`Error: ${event.error}`, "error");
                    speakBtn.classList.remove('speaking-active');
                    currentUtterance = null;
                };
                utterance.onpause = () => {
                    setStatus("Paused", "info");
                };
                utterance.onresume = () => {
                    setStatus("Speaking...", "speaking");
                };
                currentUtterance = utterance;
                window.speechSynthesis.speak(utterance);
            }

            function pauseSpeech() {
                if (window.speechSynthesis && window.speechSynthesis.speaking && !window.speechSynthesis.paused) {
                    window.speechSynthesis.pause();
                    setStatus("Paused", "info");
                } else if (!window.speechSynthesis || !window.speechSynthesis.speaking) {
                    setStatus("No speech to pause", "error");
                }
            }

            function resumeSpeech() {
                if (window.speechSynthesis && window.speechSynthesis.paused) {
                    window.speechSynthesis.resume();
                    setStatus("Resumed", "speaking");
                } else if (window.speechSynthesis && window.speechSynthesis.speaking) {
                    setStatus("Already speaking", "info");
                } else {
                    speak();
                }
            }

            function stopSpeech() {
                if (window.speechSynthesis && (window.speechSynthesis.speaking || window.speechSynthesis.pending)) {
                    window.speechSynthesis.cancel();
                    setStatus("Stopped", "info");
                    speakBtn.classList.remove('speaking-active');
                    currentUtterance = null;
                } else {
                    setStatus("No speech to stop", "info");
                }
            }

            rateSlider.addEventListener('input', () => rateValue.textContent = rateSlider.value);
            pitchSlider.addEventListener('input', () => pitchValue.textContent = pitchSlider.value);
            languageSelect.addEventListener('change', () => {
                populateVoiceDropdown();
                checkHindiVoiceAvailability();
                stopSpeech();
                setStatus("Language changed", "info");
            });
            speakBtn.addEventListener('click', speak);
            pauseBtn.addEventListener('click', pauseSpeech);
            resumeBtn.addEventListener('click', resumeSpeech);
            stopBtn.addEventListener('click'
