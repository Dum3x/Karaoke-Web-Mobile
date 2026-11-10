<html lang="id">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, viewport-fit=cover">
  <title>Karaoke Web</title>
  <style>
    :root {
      --bg-main: #0a0d14;
      --bg-card: #121824;
      --bg-card-header: #1a2234;
      --accent-cyan: #00f0ff;
      --accent-green: #00ff66;
      --accent-magenta: #ff0055;
      --accent-orange: #ff9900;
      --text-main: #e2e8f0;
      --text-muted: #64748b;
      --border-color: #1e293b;
    }

    * { box-sizing: border-box; margin: 0; padding: 0; user-select: none; -webkit-tap-highlight-color: transparent; }
    
    body { 
      background-color: var(--bg-main); 
      color: var(--text-main); 
      font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, monospace; 
      min-height: 100vh; 
      display: flex; 
      flex-direction: column; 
      font-size: 12px;
      align-items: center;
    }

    /* Fixed Layout Outer Container for PC & Mobile */
    .app-container {
      width: 100%;
      max-width: 850px;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
      background: #07090e;
      border-left: 1px solid var(--border-color);
      border-right: 1px solid var(--border-color);
      box-shadow: 0 0 20px rgba(0,0,0,0.8);
    }

    header { 
      background: var(--bg-card-header); 
      border-bottom: 1px solid var(--border-color); 
      padding: 10px 14px; 
      display: flex; 
      justify-content: space-between; 
      align-items: center; 
      position: sticky; 
      top: 0; 
      z-index: 100; 
    }
    .brand { display: flex; align-items: center; gap: 8px; font-weight: 800; font-size: 13px; color: var(--accent-cyan); }
    .badge { background: rgba(0, 255, 102, 0.2); color: var(--accent-green); border: 1px solid var(--accent-green); font-size: 9px; font-weight: 700; padding: 2px 6px; border-radius: 4px; }

    .tab-bar { display: flex; background: #0d121c; border-bottom: 1px solid var(--border-color); padding: 4px; gap: 4px; }
    .tab-btn { flex: 1; padding: 10px; text-align: center; background: transparent; border: none; color: var(--text-muted); font-weight: 700; font-size: 11px; border-radius: 6px; cursor: pointer; transition: 0.2s; }
    .tab-btn.active { background: var(--bg-card); color: var(--accent-cyan); border: 1px solid rgba(0, 240, 255, 0.3); }

    main { flex: 1; padding: 12px; display: flex; flex-direction: column; gap: 12px; width: 100%; }
    .card { background: var(--bg-card); border: 1px solid var(--border-color); border-radius: 8px; padding: 12px; display: flex; flex-direction: column; gap: 10px; }
    .card-title { font-size: 10px; font-weight: 700; text-transform: uppercase; color: var(--text-muted); letter-spacing: 1px; display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #1a2336; padding-bottom: 4px; }

    /* Responsive Grid for PC vs Mobile */
    .mixer-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 10px; }
    .channel-strip { background: #0f1420; border: 1px solid #1a2234; border-radius: 6px; padding: 12px; display: flex; flex-direction: column; gap: 10px; }
    .channel-header { display: flex; justify-content: space-between; align-items: center; font-weight: 700; color: var(--accent-cyan); }

    .meter-container { width: 100%; height: 8px; background: #06090e; border-radius: 4px; overflow: hidden; border: 1px solid #1a2234; }
    .meter-bar { height: 100%; width: 0%; background: linear-gradient(90deg, #00ff66 60%, #ff9900 85%, #ff0055 100%); transition: width 0.05s ease; }

    .slider-group { display: flex; flex-direction: column; gap: 4px; }
    .slider-info { display: flex; justify-content: space-between; font-size: 10px; color: var(--text-main); }
    input[type=range] { -webkit-appearance: none; width: 100%; height: 6px; background: #06090e; border-radius: 3px; outline: none; }
    input[type=range]::-webkit-slider-thumb { -webkit-appearance: none; width: 18px; height: 18px; border-radius: 50%; background: var(--accent-cyan); cursor: pointer; box-shadow: 0 0 8px rgba(0, 240, 255, 0.6); }

    .route-matrix { display: flex; gap: 6px; margin-top: 4px; }
    .route-btn { flex: 1; padding: 8px; font-size: 10px; font-weight: 800; background: #0a0d14; border: 1px solid var(--border-color); color: var(--text-muted); border-radius: 4px; text-align: center; cursor: pointer; }
    .route-btn.active-a { background: rgba(0, 240, 255, 0.2); border-color: var(--accent-cyan); color: var(--accent-cyan); }
    .route-btn.active-b { background: rgba(0, 255, 102, 0.2); border-color: var(--accent-green); color: var(--accent-green); }

    .btn { background: #1a2234; color: #fff; border: 1px solid var(--border-color); padding: 8px 12px; border-radius: 6px; font-weight: 700; font-size: 11px; cursor: pointer; display: inline-flex; align-items: center; justify-content: center; gap: 6px; }
    .btn-cyan { background: var(--accent-cyan); color: #000; border: none; }
    .btn-green { background: var(--accent-green); color: #000; border: none; }
    .btn-danger { background: var(--accent-magenta); color: #fff; border: none; }

    .toggle-container { display: flex; align-items: center; justify-content: space-between; background: #0a0d14; padding: 10px; border-radius: 6px; border: 1px solid var(--border-color); }
    .switch { position: relative; display: inline-block; width: 44px; height: 22px; }
    .switch input { opacity: 0; width: 0; height: 0; }
    .slider { position: absolute; cursor: pointer; top: 0; left: 0; right: 0; bottom: 0; background-color: #1a2234; transition: .3s; border-radius: 22px; border: 1px solid var(--border-color); }
    .slider:before { position: absolute; content: ""; height: 16px; width: 16px; left: 2px; bottom: 2px; background-color: var(--text-muted); transition: .3s; border-radius: 50%; }
    input:checked + .slider { background-color: rgba(0, 255, 102, 0.2); border-color: var(--accent-green); }
    input:checked + .slider:before { transform: translateX(22px); background-color: var(--accent-green); box-shadow: 0 0 8px var(--accent-green); }

    .lyrics-box { background: #06090e; border: 1px solid rgba(0, 240, 255, 0.3); border-radius: 6px; min-height: 85px; padding: 10px; display: flex; flex-direction: column; align-items: center; justify-content: center; text-align: center; overflow: hidden; position: relative; box-shadow: inset 0 0 15px rgba(0, 240, 255, 0.05); }
    .lyrics-current { font-size: 14px; font-weight: 800; color: var(--accent-cyan); text-shadow: 0 0 12px rgba(0, 240, 255, 0.6); margin-bottom: 4px; line-height: 1.3; }
    .lyrics-next { font-size: 10px; color: var(--text-muted); font-weight: 600; }
    
    .player-controls { display: flex; gap: 6px; align-items: center; justify-content: center; }
    .lyrics-controls { display: flex; gap: 4px; align-items: center; justify-content: space-between; flex-wrap: wrap; }

    canvas { width: 100%; height: 90px; background: #05070a; border-radius: 6px; border: 1px solid var(--border-color); display: block; }
    .file-input-wrapper { position: relative; overflow: hidden; display: inline-block; width: 100%; }
    .file-input-wrapper input[type=file] { font-size: 100px; position: absolute; left: 0; top: 0; opacity: 0; cursor: pointer; }
    .hidden { display: none !important; }
    #yt-wrap { position: relative; width: 100%; padding-top: 56.25%; background: #000; border-radius: 6px; overflow: hidden; }
    #yt-wrap iframe, #yt-wrap #yt-player { position: absolute; inset: 0; width: 100%; height: 100%; border: 0; }
    .sel { flex: 1; background: #0a0d14; color: var(--text-main); border: 1px solid var(--border-color); border-radius: 6px; padding: 8px; font-size: 11px; font-weight: 700; }

    /* ===== RESPONSIVE: otomatis pas di HP, tablet, dan PC ===== */
    html { -webkit-text-size-adjust: 100%; }
    header { padding-top: calc(10px + env(safe-area-inset-top, 0px)); padding-left: calc(14px + env(safe-area-inset-left, 0px)); padding-right: calc(14px + env(safe-area-inset-right, 0px)); }
    main { padding-bottom: calc(12px + env(safe-area-inset-bottom, 0px)); }
    #view-mixer, #view-dsp { display: flex; flex-direction: column; gap: 12px; min-width: 0; }
    #view-mixer.hidden, #view-dsp.hidden { display: none !important; }
    #view-mixer > *, #view-dsp > * { margin-top: 0 !important; }
    .card { min-width: 0; }
    .btn, .route-btn, .tab-btn { min-height: 38px; }
    input[type=range]::-webkit-slider-thumb { width: 22px; height: 22px; }
    input[type=range] { height: 8px; }
    .lyrics-current { font-size: clamp(14px, 4.2vw, 20px); }

    /* Tablet: container lebih lebar, kartu vocal effect jadi 2 kolom */
    @media (min-width: 700px) {
      body { font-size: 13px; }
      .app-container { max-width: 980px; }
      main { padding: 16px; gap: 16px; }
      canvas { height: 120px; }
      #view-dsp { display: grid; grid-template-columns: 1fr 1fr; grid-template-rows: max-content 1fr; gap: 12px; align-items: start; }
      #view-dsp.hidden { display: none !important; }
      #view-dsp > .card:first-child { grid-row: span 2; }
    }

    /* PC: Live Mixer dan Vocal Effect tampil berdampingan, tab disembunyikan */
    @media (min-width: 1100px) {
      body { font-size: 14px; }
      .app-container { max-width: 1400px; }
      .tab-bar { display: none; }
      main { flex-direction: row; align-items: flex-start; gap: 20px; padding: 20px; }
      #view-mixer, #view-dsp.hidden, #view-mixer.hidden { display: flex !important; }
      #view-mixer { flex: 1.5 1 0; }
      #view-dsp, #view-dsp.hidden { flex: 1 1 0; display: flex !important; flex-direction: column; grid-template-columns: none; grid-template-rows: none; }
      #view-dsp > .card:first-child { grid-row: auto; }
      canvas { height: 150px; }
      .lyrics-box { min-height: 110px; }
    }
  </style>
</head>
<body>

<div class="app-container">
  <header>
    <div class="brand">
      <span>Karaoke Web</span>
      <span class="badge">V0.5</span>
    </div>
    <button id="btn-power" onclick="toggleEngine()" class="btn btn-cyan">START</button>
  </header>

  <div class="tab-bar">
    <button class="tab-btn active" onclick="switchTab('mixer')" id="tab-mixer">Live Mixer</button>
    <button class="tab-btn" onclick="switchTab('dsp')" id="tab-dsp">Vocal Effect</button>
  </div>

  <main>
    <!-- LIVE MIXER -->
    <div id="view-mixer">
      <div class="card">
        <div class="card-title">
          <span>Master Output Visualizer</span>
          <span id="status-text" style="color: var(--accent-magenta);">● Engine Offline</span>
        </div>
        <canvas id="masterCanvas"></canvas>
      </div>

      <div class="mixer-grid" style="margin-top: 10px;">
        <div class="channel-strip">
          <div class="channel-header"><span>Microphone</span></div>
          <div class="meter-container"><div id="meter-ch1" class="meter-bar"></div></div>
          <button id="btn-mic-toggle" class="btn btn-green" onclick="toggleMic()">Activate Microphone</button>
          <div class="slider-group">
            <div class="slider-info"><span>Volume Microphone</span><span id="val-gain-ch1">100%</span></div>
            <input type="range" min="0" max="2" step="0.05" value="1" oninput="setGain(0, this.value)">
          </div>

          <div class="route-matrix">
            <button id="r-0-0" class="route-btn active-a" onclick="toggleRoute(0, 0)">Monitor (Out)</button>
            <button id="r-0-1" class="route-btn active-b" onclick="toggleRoute(0, 1)">Record (Bus)</button>
          </div>
        </div>

        <div class="channel-strip">
          <div class="channel-header"><span>Music</span></div>
          <div class="meter-container"><div id="meter-ch2" class="meter-bar"></div></div>

          <div style="display:flex; flex-direction:column; gap:6px;">
            <div class="file-input-wrapper">
              <button class="btn" style="width: 100%;">Upload File MP3/WAV</button>
              <input type="file" accept="audio/*" onchange="handleAudioUpload(event)">
            </div>

            <div class="file-input-wrapper">
              <button class="btn" style="width: 100%; background:#0e2a38; border-color:var(--accent-cyan);">Upload Lirik File (.LRC)</button>
              <input type="file" accept=".lrc,.txt" onchange="handleLrcUpload(event)">
            </div>
          </div>

          <div class="player-controls">
            <button id="btn-restart" class="btn" onclick="restartAudio()" style="background:#0a0d14; border-color:#1a2234;">Restart</button>
            <button id="btn-play-pause" class="btn btn-green" onclick="togglePlayPause()" style="flex:1;">Pause</button>
          </div>

          <div class="slider-group">
            <div class="slider-info">
              <span id="track-current-time">0:00</span>
              <span id="track-total-time">0:00</span>
            </div>
            <input type="range" id="seek-bar" min="0" max="100" value="0" oninput="seekDragging=true; previewSeek(this.value)" onchange="seekAudio(this.value); seekDragging=false">
          </div>

          <div class="lyrics-box">
            <div id="lyric-current" class="lyrics-current">Get Ready To Take Vocal</div>
            <div id="lyric-next" class="lyrics-next">Have a nice day</div>
          </div>

          <div class="lyrics-controls">
            <button class="btn" style="flex:1; padding:4px; font-size:10px; background:#1a2234;" onclick="adjustSync(-1.0)">< -1s</button>
            <button class="btn" style="flex:1; padding:4px; font-size:10px; background:#1a2234;" onclick="adjustSync(-0.5)"><< -0.5s</button>
            <span id="sync-offset-label" style="font-size:9px; color:var(--accent-white); font-weight:700;">Offset: 0s</span>
            <button class="btn" style="flex:1; padding:4px; font-size:10px; background:#1a2234;" onclick="adjustSync(0.5)">+0.5s >></button>
            <button class="btn" style="flex:1; padding:4px; font-size:10px; background:#1a2234;" onclick="adjustSync(1.0)">+1s ></button>
          </div>

          <div class="slider-group">
            <div class="slider-info"><span>Volume Music</span><span id="val-gain-ch2">100%</span></div>
            <input type="range" min="0" max="2" step="0.05" value="1" oninput="setGain(1, this.value)">
          </div>

          <div class="route-matrix">
            <button id="r-1-0" class="route-btn active-a" onclick="toggleRoute(1, 0)">Monitor (Out)</button>
            <button id="r-1-1" class="route-btn active-b" onclick="toggleRoute(1, 1)">Record (Bus)</button>
          </div>
        </div>
      </div>

      <!-- YOUTUBE INSTRUMENTAL -->
      <div class="card" style="margin-top: 10px;">
        <div class="card-title"><span>YouTube Instrumental</span><span id="yt-status" style="color: var(--text-muted);">–</span></div>
        <div style="display:flex; gap:6px;">
          <input id="yt-url" type="text" placeholder="Tempel link YouTube / ketik judul lagu..." style="flex:1; min-width:0; background:#0a0d14; color:var(--text-main); border:1px solid var(--border-color); border-radius:6px; padding:8px; font-size:12px; -webkit-user-select:text; user-select:text;">
          <button class="btn btn-cyan" onclick="loadYouTube()">Muat</button>
        </div>
        <button class="btn btn-cyan" onclick="openYouTubeSite()">Buka YouTube (cari lagu di sana)</button>
        <div id="yt-wrap" class="hidden"><div id="yt-player"></div></div>
        <div class="slider-group">
          <div class="slider-info"><span>Volume YouTube</span><span id="val-yt-vol">100%</span></div>
          <input id="yt-vol" type="range" min="0" max="100" step="5" value="100" oninput="setYtVolume(this.value)">
        </div>
        <div style="display:flex; gap:6px;">
          <button class="btn" style="flex:1;" onclick="openYouTube()">Buka di YouTube</button>
          <button class="btn" style="flex:1;" onclick="stopYouTube()">Tutup Player</button>
        </div>
        <div id="yt-help" style="font-size:10px; color:var(--accent-orange);"></div>
        <div style="font-size:9px; color:var(--text-muted);">Ketik judul lalu tekan Buka YouTube, pilih video, salin link-nya, tempel di kolom atas lalu tekan Muat. Musik YouTube diputar oleh player-nya sendiri gak bisa di record.</div>
      </div>

      <div class="card" style="margin-top: 10px;">
        <div class="card-title">Master Output Bus</div>
        <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 10px;">
          <div class="slider-group">
            <div class="slider-info"><span>Monitor Speaker/Headset</span><span id="val-master-a">100%</span></div>
            <input type="range" min="0" max="1.5" step="0.05" value="1" oninput="setMasterGain(0, this.value)">
          </div>
          <div class="slider-group">
            <div class="slider-info"><span>Bus Rekam</span><span id="val-master-b">100%</span></div>
            <input type="range" min="0" max="1.5" step="0.05" value="1" oninput="setMasterGain(1, this.value)">
          </div>
        </div>
      </div>

      <div class="card" style="margin-top: 10px;">
        <div class="card-title">
          <span>Record harus aktif recordnya Busnya</span>
          <span id="rec-status" style="color: var(--text-muted);">● Idle 0:00</span>
        </div>
        <button id="btn-rec" class="btn btn-danger" onclick="toggleRecord()">Start Record</button>
        <div id="rec-result" class="hidden" style="flex-direction:column; gap:8px;">
          <audio id="rec-player" controls style="width:100%;"></audio>
          <div id="rec-info" style="font-size:10px; color:var(--text-muted);"></div>
          <button class="btn btn-green" onclick="saveRecording()">Simpan MP3 (128 kbps)</button>
        </div>
        <div style="font-size:9px; color:var(--text-muted);">Yang direkam semua channel recordnya harus aktif ya (mic + lagu), sesuai fader dan slider B1. Disimpan sebagai MP3 128 kbps.</div>
      </div>
    </div>

    <!-- VOCALEFFECT -->
    <div id="view-dsp" class="hidden">
      <!-- VOCALEFFECT -->
      <div class="card">
        <div class="card-title">Vocal Effect &amp; Vocal Shaping</div>
        <div class="toggle-container">
          <span style="font-weight: 700; color: var(--accent-cyan);">Effect Vocal<span style="font-weight:400; color:var(--text-muted); font-size:10px;"> (Low EQ,Mid EQ,High EQ, Delay, Reverb)</span></span>
          <label class="switch">
            <input type="checkbox" id="toggle-dsp" onchange="toggleDSPChain()">
            <span class="slider"></span>
          </label>
        </div>

        <div class="slider-group">
          <div class="slider-info"><span>Low Equalizer (Bass)</span><span id="val-eq-low">0 dB</span></div>
          <input type="range" min="-12" max="12" step="0.5" value="0" oninput="setEQ('low', this.value)">
        </div>

        <div class="slider-group">
          <div class="slider-info"><span>Mid Equalizer (Vocal Clarity)</span><span id="val-eq-mid">0 dB</span></div>
          <input type="range" min="-12" max="12" step="0.5" value="0" oninput="setEQ('mid', this.value)">
        </div>

        <div class="slider-group">
          <div class="slider-info"><span>High Equalizer (Treble)</span><span id="val-eq-high">0 dB</span></div>
          <input type="range" min="-12" max="12" step="0.5" value="0" oninput="setEQ('high', this.value)">
        </div>

        <div class="slider-group">
          <div class="slider-info"><span>Vocal Echo / Delay Time</span><span id="val-delay">0 ms</span></div>
          <input type="range" min="0" max="0.5" step="0.02" value="0" oninput="setDelay(this.value)">
        </div>

        <div class="slider-group">
          <div class="slider-info"><span>Vocal Reverb Room Size</span><span id="val-reverb">30%</span></div>
          <input type="range" min="0" max="1.0" step="0.05" value="0.3" oninput="setReverb(this.value)">
        </div>

        <div style="background:#0a0d14; padding:8px; border-radius:6px; border:1px solid #1a2234;" class="slider-group">
          <div class="toggle-container" style="background:transparent; padding:0; border:none; margin-bottom:6px;">
            <span style="font-weight: 700; color: var(--accent-cyan);">Compressor Vocal</span>
            <label class="switch">
              <input type="checkbox" id="toggle-comp" onchange="toggleCompressor()">
              <span class="slider"></span>
            </label>
          </div>
          
          <div class="slider-group">
            <div class="slider-info"><span>Comp Threshold</span><span id="val-comp-thresh">-30 dB</span></div>
            <input type="range" min="-60" max="0" step="1" value="-24" oninput="setCompressorThreshold(this.value)">
          </div>
          
          <div class="slider-group">
            <div class="slider-info"><span>Comp Ratio</span><span id="val-comp-ratio">10:1</span></div>
            <input type="range" min="1" max="20" step="1" value="4" oninput="setCompressorRatio(this.value)">
          </div>
        </div>
      </div>

      <!-- NOISEGATE -->
      <div class="card">
        <div class="card-title">Noise Gate</div>
        <div class="slider-group">
          <div class="slider-info">
            <span style="font-weight: 700; color: var(--accent-white);">Noise Gate</span>
            <span id="val-gate-knob" style="font-weight: 700; color: var(--accent-cyan);">0.0</span>
          </div>
          <input type="range" min="0" max="10" step="0.5" value="0" oninput="setGateKnob(this.value)">
        </div>
      </div>

      <!-- AUTOTUNE -->
      <div class="card">
        <div class="card-title"><span>Auto Tune Vocal</span><span id="tune-note" style="color: var(--accent-cyan);">–</span></div>
        <div class="toggle-container">
          <span style="font-weight: 700; color: var(--accent-cyan);">Auto Tune</span>
          <label class="switch">
            <input type="checkbox" id="toggle-tune" onchange="toggleAutotune()">
            <span class="slider"></span>
          </label>
        </div>
        <div style="display:flex; gap:6px;">
          <select class="sel" onchange="setTuneKey(this.value)"><option value="0">C</option><option value="1">C#</option><option value="2">D</option><option value="3">D#</option><option value="4">E</option><option value="5">F</option><option value="6">F#</option><option value="7">G</option><option value="8">G#</option><option value="9">A</option><option value="10">A#</option><option value="11">B</option></select>
          <select class="sel" onchange="setTuneMode(this.value)">
            <option value="chromatic">Chromatic (semua nada)</option>
            <option value="major">Major</option>
            <option value="minor">Minor</option>
          </select>
        </div>
        <div class="slider-group">
          <div class="slider-info"><span>Strength (seberapa kuat dikoreksi)</span><span id="val-tune-amount">100%</span></div>
          <input type="range" min="0" max="100" step="5" value="100" oninput="setTuneAmount(this.value)">
        </div>
        <div class="slider-group">
          <div class="slider-info"><span>Retune Speed</span><span id="val-tune-speed">Instan (robot)</span></div>
          <input type="range" min="0" max="100" step="5" value="0" oninput="setTuneSpeed(this.value)">
        </div>
        <div style="font-size:9px; color:var(--text-muted);">Pakai headset. Auto Tune menambah delay ±20–40 ms.</div>
      </div>

      <!-- NOISE CLEANER -->
      <div class="card">
        <div class="card-title"><span>Noise Cleaner</span><span id="clean-status" style="color: var(--text-muted);">–</span></div>
        <div class="toggle-container">
          <span style="font-weight: 700; color: var(--accent-cyan);">Anti Dengung &amp; Desis Mic</span>
          <label class="switch">
            <input type="checkbox" id="toggle-clean" onchange="toggleClean()">
            <span class="slider"></span>
          </label>
        </div>
        <select class="sel" id="clean-hum" onchange="setCleanHum(this.value)">
          <option value="50">Dengung listrik 50 Hz (Indonesia)</option>
          <option value="60">Dengung listrik 60 Hz</option>
          <option value="0">Tanpa notch dengung (filter bass saja)</option>
        </select>
        <div class="slider-group">
          <div class="slider-info"><span>Strength (peredam desis / noise)</span><span id="val-clean">60%</span></div>
          <input type="range" min="0" max="100" step="5" value="60" oninput="setCleanStrength(this.value)">
        </div>
        <div style="font-size:9px; color:var(--text-muted);">Filter dengung listrik + peredam noise otomatis di jalur mic (sebelum Gate). Menambah delay sekitar 10 ms. Kalau suara terdengar "bergelembung", turunkan Strength.</div>
      </div>
    </div>
  </main>
</div>

  <script>
    let audioCtx = null;
    let isRunning = false;
    let isDspActive = false;
    let rafId = null, seekDragging = false, delayWet = null, reverbWet = null;
    const S = { gain: [1, 1], master: [1, 1], eq: { low: 0, mid: 0, high: 0 }, delay: 0, reverb: 0.3, thresh: -24, ratio: 4 };
    let isCompActive = false;
    // Samakan switch dengan state (semua mati saat awal; browser kadang memulihkan centang lama)
    ["toggle-dsp", "toggle-comp", "toggle-tune"].forEach(id => { const el = document.getElementById(id); if (el) el.checked = false; });

    let micStream = null;
    let micSourceNode = null;
    let audioBuffer = null;
    let fileSourceNode = null;
    let isPlaying = false;
    let pauseOffset = 0;
    let startTime = 0;
    let lyricOffset = 0;

    let chGains = [null, null];
    let chAnalysers = [null, null];
    let masterGains = [null, null];
    let masterAnalyser = null;

    let gateGainNode = null;
    let gateAnalyser = null;
    let gateInterval = null;
    let gateKnobValue = 0.0;

    let compressorNode = null;
    let eqLow = null, eqMid = null, eqHigh = null;
    let delayNode = null, delayFeedback = null;
    let reverbNode = null, reverbGain = null, reverbFilter = null;

    let currentLyrics = [
      { time: 0.0, text: "Ready to take your vocal" },
      { time: 3.0, text: "Keren" }
    ];

    let routing = [[1, 1], [1, 1]];

    function toggleEngine() {
      if (!isRunning) initAudioEngine();
      else stopAudioEngine();
    }

    function initAudioEngine() {
      if (isRunning) return;
      const AudioContext = window.AudioContext || window.webkitAudioContext;
      audioCtx = new AudioContext({ latencyHint: 'interactive' });
      if (audioCtx.state === 'suspended') audioCtx.resume();

      for (let i = 0; i < 2; i++) {
        masterGains[i] = audioCtx.createGain();
        masterGains[i].gain.value = 1.0;
      }
      limA = makeLimiter(); limB = makeLimiter();
      masterGains[0].connect(limA); limA.connect(audioCtx.destination);
      masterGains[1].connect(limB);

      masterAnalyser = audioCtx.createAnalyser();
      masterAnalyser.fftSize = 128;
      masterGains[0].connect(masterAnalyser);

      for (let i = 0; i < 2; i++) {
        chGains[i] = audioCtx.createGain();
        chAnalysers[i] = audioCtx.createAnalyser();
        chAnalysers[i].fftSize = 64;
        chGains[i].connect(chAnalysers[i]);
      }

      gateGainNode = audioCtx.createGain();
      gateGainNode.gain.value = 1.0;

      gateAnalyser = audioCtx.createAnalyser();
      gateAnalyser.fftSize = 256;

      compressorNode = audioCtx.createDynamicsCompressor();
      compressorNode.threshold.setValueAtTime(-24, audioCtx.currentTime);
      compressorNode.ratio.setValueAtTime(4, audioCtx.currentTime);

      eqLow = audioCtx.createBiquadFilter(); eqLow.type = 'lowshelf'; eqLow.frequency.value = 250;
      eqMid = audioCtx.createBiquadFilter(); eqMid.type = 'peaking'; eqMid.frequency.value = 1500;
      eqHigh = audioCtx.createBiquadFilter(); eqHigh.type = 'highshelf'; eqHigh.frequency.value = 4000;
      
      delayNode = audioCtx.createDelay(); delayNode.delayTime.value = 0;
      delayFeedback = audioCtx.createGain(); delayFeedback.gain.value = 0.3;
      delayNode.connect(delayFeedback); delayFeedback.connect(delayNode);
      delayWet = audioCtx.createGain(); delayNode.connect(delayWet); delayWet.connect(chGains[0]);

      reverbNode = audioCtx.createDelay(); reverbNode.delayTime.value = 0.15;
      reverbGain = audioCtx.createGain(); reverbGain.gain.value = 0.35;
      reverbFilter = audioCtx.createBiquadFilter(); reverbFilter.type = 'lowpass'; reverbFilter.frequency.value = 3000;
      reverbNode.connect(reverbFilter); reverbFilter.connect(reverbGain); reverbGain.connect(reverbNode);
      reverbWet = audioCtx.createGain(); reverbFilter.connect(reverbWet); reverbWet.connect(chGains[0]);

      applyState();
      updateRoutingConnections();
      startAudibilityGateProcessor();
      if (isCleanActive) setupClean().then(connectMicNode).catch(() => {});
      if (isTuneActive) setupAutotune().then(connectMicNode).catch(e => { isTuneActive = false; document.getElementById('toggle-tune').checked = false; alert(e.message); });

      isRunning = true;
      document.getElementById('btn-power').innerText = 'STOP ENGINE';
      document.getElementById('btn-power').className = 'btn btn-danger';
      document.getElementById('status-text').innerText = '● Live Audio Engine';
      document.getElementById('status-text').style.color = 'var(--accent-green)';
      const ctxLat = audioCtx;
      setTimeout(() => {
        if (!isRunning || audioCtx !== ctxLat) return;
        const ms = Math.round(((ctxLat.baseLatency || 0) + (ctxLat.outputLatency || 0)) * 1000);
        if (ms > 0) document.getElementById('status-text').innerText = `● Live Audio Engine · output ±${ms} ms`;
      }, 1000);

      renderVisualizers();
    }

    function stopAudioEngine() {
      stopRecording();
      tuneNode = null; tuneCore = null; cleanNodes = null; cleanCore = null;
      if (isPlaying && audioCtx) pauseOffset = audioCtx.currentTime - startTime;
      isPlaying = false;
      if (gateInterval) { clearInterval(gateInterval); gateInterval = null; }
      if (rafId) { cancelAnimationFrame(rafId); rafId = null; }
      stopMic();
      fileSourceNode = null;
      if (audioCtx) { audioCtx.close(); audioCtx = null; }
      isRunning = false;
      document.getElementById('btn-power').innerText = 'START ENGINE';
      document.getElementById('btn-power').className = 'btn btn-cyan';
      document.getElementById('status-text').innerText = '● Engine Offline';
      document.getElementById('status-text').style.color = 'var(--accent-magenta)';
      document.getElementById('btn-play-pause').innerText = '▶️ Play';
      document.getElementById('btn-play-pause').className = 'btn btn-cyan';
      document.getElementById('meter-ch1').style.width = '0%';
      document.getElementById('meter-ch2').style.width = '0%';
    }

    function applyState() {
      for (let i = 0; i < 2; i++) { chGains[i].gain.value = S.gain[i]; masterGains[i].gain.value = S.master[i]; }
      eqLow.gain.value = S.eq.low; eqMid.gain.value = S.eq.mid; eqHigh.gain.value = S.eq.high;
      delayNode.delayTime.value = S.delay; delayWet.gain.value = S.delay > 0 ? 0.5 : 0;
      reverbWet.gain.value = S.reverb;
      compressorNode.threshold.value = S.thresh; compressorNode.ratio.value = S.ratio;
    }

    function startAudibilityGateProcessor() {
      if (gateInterval) clearInterval(gateInterval);
      const dataArray = new Float32Array(gateAnalyser.fftSize);
      let gateOpen = true, holdUntil = 0;

      gateInterval = setInterval(() => {
        if (!isRunning || !audioCtx) return;
        const now = audioCtx.currentTime;
        if (gateKnobValue === 0) {
          gateOpen = true;
          gateGainNode.gain.setTargetAtTime(1.0, now, 0.02);
          return;
        }
        gateAnalyser.getFloatTimeDomainData(dataArray);
        let sum = 0;
        for (let i = 0; i < dataArray.length; i++) sum += dataArray[i] * dataArray[i];
        const rms = Math.sqrt(sum / dataArray.length);

        const openTh = (gateKnobValue / 10) * 0.1;
        const closeTh = openTh * 0.7;
        if (rms >= openTh) { gateOpen = true; holdUntil = now + 0.25; }
        else if (gateOpen && rms < closeTh && now > holdUntil) gateOpen = false;

        gateGainNode.gain.setTargetAtTime(gateOpen ? 1.0 : 0.0, now, gateOpen ? 0.005 : 0.06);
      }, 10);
    }

    function updateRoutingConnections() {
      if (!isRunning) return;
      for (let input = 0; input < 2; input++) {
        try { chGains[input].disconnect(); chGains[input].connect(chAnalysers[input]); } catch(e){}
        for (let out = 0; out < 2; out++) {
          if (routing[input][out]) chGains[input].connect(masterGains[out]);
        }
      }
    }
    function stopMic() {
      if (micStream) { micStream.getTracks().forEach(t => t.stop()); micStream = null; }
      if (micSourceNode) { try { micSourceNode.disconnect(); } catch(e){} micSourceNode = null; }
      const b = document.getElementById('btn-mic-toggle');
      b.innerText = 'Activate Microphone'; b.className = 'btn btn-green';
    }

    async function toggleMic() {
      if (micStream) { stopMic(); return; }
      if (!isRunning) initAudioEngine();
      try {
        micStream = await navigator.mediaDevices.getUserMedia({
          audio: { echoCancellation: false, noiseSuppression: false, autoGainControl: false, channelCount: { ideal: 1 }, latency: { ideal: 0 } }
        });
        if (!isRunning) { stopMic(); return; }
        micSourceNode = audioCtx.createMediaStreamSource(micStream);
        // Monitor (Out) mic otomatis ON setiap kali microphone diaktifkan
        routing[0][0] = 1;
        document.getElementById('r-0-0').classList.add('active-a');
        updateRoutingConnections();
        connectMicNode();
        document.getElementById('btn-mic-toggle').innerText = 'Mic Connected (tap = matikan)';
        document.getElementById('btn-mic-toggle').className = 'btn btn-cyan';
      } catch (err) { stopMic(); alert("Microphone Access Denied: " + err.message); }
    }

    function connectMicNode() {
      if (!micSourceNode) return;
      try {
        micSourceNode.disconnect();
        if (gateGainNode) gateGainNode.disconnect();
        if (compressorNode) compressorNode.disconnect();
        if (eqLow) eqLow.disconnect();
        if (tuneNode) tuneNode.disconnect();
      } catch(e){}
      if (cleanNodes) [cleanNodes.hpf, ...cleanNodes.notches, cleanNodes.node].forEach(n => { try { n.disconnect(); } catch(e){} });

      micSourceNode.connect(gateAnalyser);
      connectCleanFront();
      let lastNode = gateGainNode;

      if (isDspActive && isCompActive) {
        lastNode.connect(compressorNode);
        lastNode = compressorNode;
      }

      if (isTuneActive && tuneNode) {
        lastNode.connect(tuneNode);
        lastNode = tuneNode;
      }

      if (isDspActive) {
        lastNode.connect(eqLow);
        eqLow.connect(eqMid);
        eqMid.connect(eqHigh);

        eqHigh.connect(chGains[0]);
        eqHigh.connect(delayNode);
        eqHigh.connect(reverbNode);
      } else {
        lastNode.connect(chGains[0]);
      }
    }

    // ===== RECORDVOCAL + MUSIC =====
    let limA = null, limB = null;
    let recNode = null, recSink = null, recChunksL = [], recChunksR = [], recRate = 48000;
    let recTimer = null, recStart = 0, recUrl = null, recPCM = null, recFinish = null;

    function makeLimiter() {
      const c = audioCtx.createDynamicsCompressor();
      c.threshold.value = -6; c.knee.value = 0; c.ratio.value = 20; c.attack.value = 0.003; c.release.value = 0.15;
      return c;
    }

    function softClip(x) {
      const a = Math.abs(x);
      if (a <= 0.8) return x;
      const y = 0.8 + 0.2 * Math.tanh((a - 0.8) / 0.2);
      return x < 0 ? -y : y;
    }

    function pushPCM(l, r) {
      const cv = f => { const o = new Int16Array(f.length); for (let i = 0; i < f.length; i++) { const v = softClip(f[i]); o[i] = v < 0 ? v * 32768 : v * 32767; } return o; };
      recChunksL.push(cv(l)); recChunksR.push(cv(r));
    }

    const REC_WORKLET = `class RecProc extends AudioWorkletProcessor {
      constructor() {
        super(); this.n = 0; this.l = new Float32Array(4096); this.r = new Float32Array(4096);
        this.port.onmessage = () => { this.port.postMessage({ l: this.l.slice(0, this.n), r: this.r.slice(0, this.n), done: true }); };
      }
      process(inputs) {
        const i = inputs[0];
        if (i && i[0]) {
          const a = i[0], b = i[1] || i[0];
          for (let k = 0; k < a.length; k++) {
            this.l[this.n] = a[k]; this.r[this.n] = b[k];
            if (++this.n === 4096) { this.port.postMessage({ l: this.l, r: this.r }); this.l = new Float32Array(4096); this.r = new Float32Array(4096); this.n = 0; }
          }
        }
        return true;
      }
    }
    registerProcessor('rec-proc', RecProc);`;

    function toggleRecord() { if (recNode) stopRecording(); else startRecording(); }

    async function startRecording() {
      if (!isRunning) initAudioEngine();
      const ctx = audioCtx;
      recChunksL = []; recChunksR = []; recRate = ctx.sampleRate;
      const sink = ctx.createGain(); sink.gain.value = 0; sink.connect(ctx.destination); recSink = sink;
      let node = null;
      if (ctx.audioWorklet) {
        try {
          if (!ctx._recLoaded) {
            await addWorklet(ctx, REC_WORKLET);
            ctx._recLoaded = true;
          }
          node = new AudioWorkletNode(ctx, 'rec-proc', { numberOfInputs: 1, numberOfOutputs: 1, outputChannelCount: [2], channelCount: 2, channelCountMode: 'explicit' });
          node.port.onmessage = e => { pushPCM(e.data.l, e.data.r); if (e.data.done && recFinish) recFinish(); };
        } catch (err) { node = null; }
      }
      if (!node) {
        node = ctx.createScriptProcessor(4096, 2, 2);
        node.onaudioprocess = e => pushPCM(e.inputBuffer.getChannelData(0), e.inputBuffer.getChannelData(1));
      }
      if (!isRunning || ctx !== audioCtx) return;
      limB.connect(node); node.connect(sink); recNode = node;
      recStart = Date.now();
      document.getElementById('rec-result').classList.add('hidden');
      document.getElementById('btn-rec').innerText = 'Stop Record';
      document.getElementById('rec-status').style.color = 'var(--accent-magenta)';
      recTimer = setInterval(() => { document.getElementById('rec-status').innerText = '● REC ' + formatTime((Date.now() - recStart) / 1000); }, 250);
    }

    function joinI16(chunks) {
      let n = 0; chunks.forEach(c => n += c.length);
      const o = new Int16Array(n); let p = 0;
      chunks.forEach(c => { o.set(c, p); p += c.length; });
      return o;
    }

    function stopRecording() {
      if (!recNode) return;
      const node = recNode, sink = recSink; recNode = null;
      clearInterval(recTimer); recTimer = null;
      try { limB.disconnect(node); } catch (e) {}
      document.getElementById('btn-rec').innerText = 'Start Record';
      document.getElementById('rec-status').style.color = 'var(--text-muted)';
      document.getElementById('rec-status').innerText = '● Idle';
      const finish = () => {
        if (!recFinish && node.port) return;
        recFinish = null;
        try { node.disconnect(); sink.disconnect(); } catch (e) {}
        node.onaudioprocess = null; if (node.port) node.port.onmessage = null;
        buildPreview();
      };
      if (node.port) { recFinish = finish; node.port.postMessage('flush'); setTimeout(finish, 500); }
      else finish();
    }

    function buildPreview() {
      const L = joinI16(recChunksL), R = joinI16(recChunksR);
      recChunksL = []; recChunksR = [];
      if (L.length < 16) { alert('Rekaman terlalu pendek.'); return; }
      recPCM = { L, R, rate: recRate };
      if (recUrl) URL.revokeObjectURL(recUrl);
      recUrl = URL.createObjectURL(encodeWav(L, R, recRate));
      document.getElementById('rec-player').src = recUrl;
      document.getElementById('rec-info').innerText = `Durasi ${formatTime(L.length / recRate)} • perkiraan MP3 ±${(L.length / recRate * 16000 / 1048576).toFixed(1)} MB`;
      const box = document.getElementById('rec-result');
      box.classList.remove('hidden'); box.style.display = 'flex';
    }

    function encodeWav(L, R, rate) {
      const n = L.length, buf = new ArrayBuffer(44 + n * 4), dv = new DataView(buf);
      const str = (o, t) => { for (let i = 0; i < t.length; i++) dv.setUint8(o + i, t.charCodeAt(i)); };
      str(0, 'RIFF'); dv.setUint32(4, 36 + n * 4, true); str(8, 'WAVE'); str(12, 'fmt ');
      dv.setUint32(16, 16, true); dv.setUint16(20, 1, true); dv.setUint16(22, 2, true);
      dv.setUint32(24, rate, true); dv.setUint32(28, rate * 4, true); dv.setUint16(32, 4, true); dv.setUint16(34, 16, true);
      str(36, 'data'); dv.setUint32(40, n * 4, true);
      for (let i = 0, o = 44; i < n; i++, o += 4) { dv.setInt16(o, L[i], true); dv.setInt16(o + 2, R[i], true); }
      return new Blob([buf], { type: 'audio/wav' });
    }

    function loadLame() {
      return new Promise((res, rej) => {
        if (window.lamejs) return res();
        const sc = document.createElement('script');
        sc.src = 'https://cdnjs.cloudflare.com/ajax/libs/lamejs/1.2.1/lame.min.js';
        sc.onload = () => res();
        sc.onerror = () => rej(new Error('Encoder MP3 gagal dimuat (butuh internet sekali).'));
        document.head.appendChild(sc);
      });
    }

    async function encodeMp3(L, R, rate, onProgress) {
      await loadLame();
      const enc = new lamejs.Mp3Encoder(2, rate, 128), out = [];
      for (let i = 0, c = 0; i < L.length; i += 1152, c++) {
        const b = enc.encodeBuffer(L.subarray(i, i + 1152), R.subarray(i, i + 1152));
        if (b.length) out.push(new Uint8Array(b));
        if (c % 200 === 0) { onProgress(Math.round(i / L.length * 100)); await new Promise(r => setTimeout(r, 0)); }
      }
      const f = enc.flush(); if (f.length) out.push(new Uint8Array(f));
      return new Blob(out, { type: 'audio/mpeg' });
    }

    async function saveRecording() {
      if (!recPCM) return;
      const st = document.getElementById('rec-info'), info = st.innerText;
      st.innerText = 'Memproses MP3...';
      await new Promise(r => setTimeout(r, 50));
      const { L, R, rate } = recPCM;
      let blob, ext = 'mp3';
      try { blob = await encodeMp3(L, R, rate, pct => { st.innerText = '⏳ Memproses MP3... ' + pct + '%'; }); }
      catch (err) {
        if (!confirm(err.message + '\n\nSimpan sebagai WAV saja (file lebih besar)?')) { st.innerText = info; return; }
        blob = encodeWav(L, R, rate); ext = 'wav';
      }
      const d = new Date(), p = x => String(x).padStart(2, '0');
      const a = document.createElement('a'), url = URL.createObjectURL(blob);
      a.href = url; a.download = `rekaman-${d.getFullYear()}${p(d.getMonth() + 1)}${p(d.getDate())}-${p(d.getHours())}${p(d.getMinutes())}${p(d.getSeconds())}.${ext}`;
      document.body.appendChild(a); a.click(); a.remove();
      setTimeout(() => URL.revokeObjectURL(url), 60000);
      st.innerText = info + ' • Tersimpan ' + ext.toUpperCase() + ' (' + (blob.size / 1048576).toFixed(1) + ' MB)';
    }

    // ===== AUTOTUNE =====
    let tuneNode = null, tuneCore = null, isTuneActive = false;
    const TP = { key: 0, mode: 'chromatic', amount: 1, speed: 0 };
    const NOTE_NAMES = ['C', 'C#', 'D', 'D#', 'E', 'F', 'F#', 'G', 'G#', 'A', 'A#', 'B'];

    const TUNE_CORE = `class TuneCore {
      constructor(sr, post) {
        this.sr = sr; this.post = post;
        this.buf = new Float32Array(8192); this.w = 0; this.cnt = 0; this.miss = 0; this.msg = 0;
        this.p = { key: 0, mode: 'chromatic', amount: 1, speed: 0 };
        this.ph = 0; this.ratio = 1; this.tRatio = 1; this.W = 2048; this.Wt = 2048;
        this.d = new Float32Array(768); this.cm = new Float32Array(320);
      }
      detect() {
        const sr = this.sr / 2, N = 468, M = 8191, d = this.d, cm = this.cm;
        const minL = Math.max(2, Math.floor(sr / 800)), maxL = Math.min(300, Math.ceil(sr / 70));
        for (let i = 0; i < N + maxL + 1; i++) { const x = this.w - 1 - 2 * i; d[i] = (this.buf[x & M] + this.buf[(x - 1) & M]) * 0.5; }
        let e = 0; for (let j = 0; j < N; j++) e += d[j] * d[j];
        if (e / N < 1e-4) return 0;
        let run = 0;
        for (let t = 1; t <= maxL; t++) {
          let s = 0; for (let j = 0; j < N; j++) { const x = d[j] - d[j + t]; s += x * x; }
          run += s; cm[t] = s * t / (run || 1e-9);
        }
        let t = minL; while (t < maxL && cm[t] > 0.15) t++;
        if (t >= maxL) return 0;
        while (t + 1 < maxL && cm[t + 1] < cm[t]) t++;
        const a = cm[t - 1], b = cm[t], c = cm[t + 1], den = a - 2 * b + c;
        return sr / (t + (den ? 0.5 * (a - c) / den : 0));
      }
      snap(m) {
        const sc = this.p.mode === 'major' ? [0, 2, 4, 5, 7, 9, 11] : this.p.mode === 'minor' ? [0, 2, 3, 5, 7, 8, 10] : [0, 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11];
        let best = Math.round(m), bd = 99;
        for (let n = Math.floor(m) - 3; n <= Math.ceil(m) + 3; n++) {
          if (sc.indexOf((((n - this.p.key) % 12) + 12) % 12) < 0) continue;
          const dd = Math.abs(n - m); if (dd < bd) { bd = dd; best = n; }
        }
        return best;
      }
      update() {
        const f = this.detect();
        if (f > 70 && f < 900) {
          const m = 69 + 12 * Math.log2(f / 440), tg = this.snap(m);
          const semi = Math.max(-6, Math.min(6, (tg - m) * this.p.amount));
          this.tRatio = Math.pow(2, semi / 12); this.miss = 0;
          this.Wt = Math.max(256, Math.min(4000, 4 * this.sr / f));
          if (++this.msg % 6 === 0) this.post({ voiced: true, midi: m, target: tg });
        } else if (++this.miss > 4) {
          this.tRatio = 1;
          if (this.miss % 12 === 5) this.post({ voiced: false });
        }
      }
      run(inp, out) {
        const M = 8191;
        const tc = 0.002 + this.p.speed * 0.25, al = 1 - Math.exp(-1 / (tc * this.sr));
        for (let i = 0; i < out.length; i++) {
          this.buf[this.w] = inp ? inp[i] : 0;
          if (++this.cnt >= 512) { this.cnt = 0; this.update(); }
          this.ratio += (this.tRatio - this.ratio) * al;
          this.W += (this.Wt - this.W) * 0.0002;
          this.ph += (1 - this.ratio) / this.W; this.ph -= Math.floor(this.ph);
          let y = 0;
          for (let k = 0; k < 2; k++) {
            let q = this.ph + k * 0.5; if (q >= 1) q -= 1;
            const rp = this.w - (q * this.W + 1), i0 = Math.floor(rp), fr = rp - i0;
            y += (this.buf[i0 & M] * (1 - fr) + this.buf[(i0 + 1) & M] * fr) * (0.5 - 0.5 * Math.cos(6.283185307 * q));
          }
          out[i] = y; this.w = (this.w + 1) & M;
        }
      }
    }`;

    const TUNE_WORKLET = TUNE_CORE + `
    class TuneProc extends AudioWorkletProcessor {
      constructor() {
        super();
        this.c = new TuneCore(sampleRate, m => this.port.postMessage(m));
        this.port.onmessage = e => { Object.assign(this.c.p, e.data); };
      }
      process(inputs, outputs) {
        const out = outputs[0][0];
        if (out) this.c.run(inputs[0][0], out);
        return true;
      }
    }
    registerProcessor('tune-proc', TuneProc);`;

    async function addWorklet(ctx, code) {
      const urls = ['data:text/javascript;charset=utf-8,' + encodeURIComponent(code), URL.createObjectURL(new Blob([code], { type: 'application/javascript' }))];
      let last;
      for (const u of urls) { try { await ctx.audioWorklet.addModule(u); return; } catch (e) { last = e; } }
      throw last;
    }

    async function setupAutotune() {
      const ctx = audioCtx;
      if (!ctx) return;
      let node = null, core = null;
      if (ctx.audioWorklet) {
        try {
          if (!ctx._tuneLoaded) { await addWorklet(ctx, TUNE_WORKLET); ctx._tuneLoaded = true; }
          node = new AudioWorkletNode(ctx, 'tune-proc', { numberOfInputs: 1, numberOfOutputs: 1, outputChannelCount: [1], channelCount: 1, channelCountMode: 'explicit' });
          node.port.onmessage = e => showTune(e.data);
        } catch (err) { node = null; }
      }
      if (!node) {
        core = new (new Function(TUNE_CORE + '; return TuneCore;')())(ctx.sampleRate, showTune);
        node = ctx.createScriptProcessor(512, 1, 1);
        node.onaudioprocess = e => core.run(e.inputBuffer.getChannelData(0), e.outputBuffer.getChannelData(0));
      }
      if (ctx !== audioCtx) return;
      tuneCore = core; tuneNode = node; sendTune();
    }

    function sendTune() {
      if (tuneCore) Object.assign(tuneCore.p, TP);
      else if (tuneNode && tuneNode.port) tuneNode.port.postMessage(TP);
    }

    async function toggleAutotune() {
      const cb = document.getElementById('toggle-tune'), on = cb.checked;
      if (!isRunning) { isTuneActive = on; return; }
      if (on && !tuneNode) {
        try { await setupAutotune(); } catch (e) { cb.checked = false; alert(e.message); return; }
      }
      isTuneActive = on; connectMicNode();
      if (!on) document.getElementById('tune-note').innerText = '–';
    }

    // ===== NOISE CLEANER: filter dengung listrik (HPF + notch) + peredam noise spektral (AudioWorklet) =====
    let isCleanActive = false, cleanNodes = null, cleanCore = null;
    const CP = { strength: 0.6, hum: 50 };
    document.getElementById('toggle-clean').checked = isCleanActive;

    const CLEAN_CORE = `class CleanCore {
      constructor(sr) {
        const N = 512; this.N = N; this.H = 128; this.p = { strength: 0.6 }; this.init = 0; this.idleN = 0; this.gv = 1; this.hold = 0;
        this.win = new Float32Array(N); this.frame = new Float32Array(N); this.acc = new Float32Array(N);
        this.re = new Float32Array(N); this.im = new Float32Array(N);
        this.cs = new Float32Array(N / 2); this.sn = new Float32Array(N / 2); this.rev = new Uint16Array(N);
        const B = N / 2 + 1; this.noise = new Float32Array(B); this.ps = new Float32Array(B); this.g = new Float32Array(B).fill(1); this.gs = new Float32Array(B);
        this.zero = new Float32Array(128);
        for (let i = 0; i < N; i++) this.win[i] = 0.5 - 0.5 * Math.cos(2 * Math.PI * (i + 0.5) / N);
        for (let i = 0; i < N / 2; i++) { this.cs[i] = Math.cos(2 * Math.PI * i / N); this.sn[i] = -Math.sin(2 * Math.PI * i / N); }
        const bits = Math.log2(N);
        for (let i = 0; i < N; i++) { let r = 0; for (let k = 0; k < bits; k++) if (i & (1 << k)) r |= 1 << (bits - 1 - k); this.rev[i] = r; }
      }
      fft(re, im) {
        const N = this.N, rev = this.rev;
        for (let i = 0; i < N; i++) { const j = rev[i]; if (j > i) { let t = re[i]; re[i] = re[j]; re[j] = t; t = im[i]; im[i] = im[j]; im[j] = t; } }
        for (let size = 2; size <= N; size <<= 1) {
          const half = size >> 1, step = N / size;
          for (let i = 0; i < N; i += size) for (let j = 0, k = 0; j < half; j++, k += step) {
            const a = i + j, b = a + half, c = this.cs[k], s = this.sn[k];
            const tr = re[b] * c - im[b] * s, ti = re[b] * s + im[b] * c;
            re[b] = re[a] - tr; im[b] = im[a] - ti; re[a] += tr; im[a] += ti;
          }
        }
      }
      hop(x, y) {
        const N = this.N, H = this.H, B = N / 2 + 1, w = this.win, re = this.re, im = this.im, ps = this.ps, nz = this.noise, g = this.g, gs = this.gs;
        this.frame.copyWithin(0, H); this.frame.set(x, N - H);
        for (let i = 0; i < N; i++) { re[i] = this.frame[i] * w[i]; im[i] = 0; }
        this.fft(re, im);
        const s = this.p.strength, alpha = 1.5 + 3.5 * s, gmin = Math.pow(10, -(8 + 22 * s) / 20), n0 = this.init < 24;
        let tp = 0, tn = 0;
        for (let k = 0; k < B; k++) {
          const pw = re[k] * re[k] + im[k] * im[k] + 1e-12;
          ps[k] = this.init === 0 ? pw : 0.8 * ps[k] + 0.2 * pw;
          if (n0) nz[k] += (ps[k] - nz[k]) / (this.init + 1);
          else nz[k] = ps[k] < 3 * nz[k] ? nz[k] + 0.02 * (ps[k] - nz[k]) : nz[k] * 1.0003;
          if (k >= 1 && k <= 64) { tp += pw; tn += nz[k]; }
          const p2 = Math.max(pw, ps[k]);
          let G = Math.sqrt(Math.max(1 - alpha * nz[k] / p2, 0)); if (G < gmin) G = gmin;
          g[k] = G > g[k] ? 0.5 * g[k] + 0.5 * G : 0.85 * g[k] + 0.15 * G;
        }
        this.init++;
        if (this.init > 24) {
          if (tp > 2.5 * tn) { this.hold = 40; this.gv += (1 - this.gv) * 0.6; }
          else if (this.hold > 0) this.hold--;
          else this.gv = Math.max(Math.pow(10, -(10 + 20 * s) / 20), this.gv * 0.96);
        }
        for (let k = 0; k < B; k++) gs[k] = 0.25 * g[k > 0 ? k - 1 : 0] + 0.5 * g[k] + 0.25 * g[k < B - 1 ? k + 1 : B - 1];
        for (let k = 0; k < B; k++) { re[k] *= gs[k] * this.gv; im[k] *= gs[k] * this.gv; }
        for (let k = 1; k < N / 2; k++) { re[N - k] = re[k]; im[N - k] = -im[k]; }
        for (let i = 0; i < N; i++) im[i] = -im[i];
        this.fft(re, im);
        for (let i = 0; i < N; i++) this.acc[i] += re[i] / N * w[i] * 0.6666667;
        for (let i = 0; i < H; i++) y[i] = this.acc[i];
        this.acc.copyWithin(0, H); this.acc.fill(0, N - H);
      }
      run(inp, out) {
        for (let k = 0; k + this.H <= out.length; k += this.H) {
          const x = inp ? inp.subarray(k, k + this.H) : null, y = out.subarray(k, k + this.H);
          let live = false;
          if (x) for (let i = 0; i < x.length; i++) if (x[i] !== 0) { live = true; break; }
          // Saat tidak ada input (cleaner dimatikan / mic belum nyala) jangan ikut "belajar" dari sinyal kosong,
          // supaya profil noise yang sudah dipelajari tidak terhapus.
          if (!live) { this.idleN++; if (this.idleN > 8) { y.fill(0); continue; } }
          else { if (this.idleN > 8) { this.frame.fill(0); this.acc.fill(0); } this.idleN = 0; }
          this.hop(x || this.zero, y);
        }
      }
    }`;

    const CLEAN_WORKLET = CLEAN_CORE + `
    class CleanProc extends AudioWorkletProcessor {
      constructor(opts) {
        super(); this.c = new CleanCore(sampleRate);
        if (opts && opts.processorOptions) Object.assign(this.c.p, opts.processorOptions);
        this.port.onmessage = e => { Object.assign(this.c.p, e.data); };
      }
      process(inputs, outputs) {
        const out = outputs[0][0];
        if (out) this.c.run(inputs[0][0], out);
        return true;
      }
    }
    registerProcessor('clean-proc', CleanProc);`;

    async function setupClean() {
      const ctx = audioCtx;
      if (!ctx) return;
      const hpf = ctx.createBiquadFilter(); hpf.type = 'highpass'; hpf.frequency.value = 80; hpf.Q.value = 0.707;
      const notches = [1, 2, 3].map(() => { const n = ctx.createBiquadFilter(); n.type = 'notch'; n.Q.value = 30; return n; });
      let node = null, core = null;
      if (ctx.audioWorklet) {
        try {
          if (!ctx._cleanLoaded) { await addWorklet(ctx, CLEAN_WORKLET); ctx._cleanLoaded = true; }
          node = new AudioWorkletNode(ctx, 'clean-proc', { numberOfInputs: 1, numberOfOutputs: 1, outputChannelCount: [1], channelCount: 1, channelCountMode: 'explicit', processorOptions: { strength: CP.strength } });
        } catch (err) { node = null; }
      }
      if (!node) {
        core = new (new Function(CLEAN_CORE + '; return CleanCore;')())(ctx.sampleRate);
        node = ctx.createScriptProcessor(512, 1, 1);
        node.onaudioprocess = e => core.run(e.inputBuffer.getChannelData(0), e.outputBuffer.getChannelData(0));
      }
      if (ctx !== audioCtx) return;
      cleanCore = core; cleanNodes = { hpf, notches, node }; applyClean();
    }

    function applyClean() {
      if (!cleanNodes) return;
      cleanNodes.notches.forEach((n, i) => { n.frequency.value = (CP.hum || 50) * (i + 1); });
      if (cleanCore) cleanCore.p.strength = CP.strength;
      else if (cleanNodes.node.port) cleanNodes.node.port.postMessage({ strength: CP.strength });
    }

    function connectCleanFront() {
      if (!isCleanActive || !cleanNodes) { micSourceNode.connect(gateGainNode); return; }
      let prev = micSourceNode;
      [cleanNodes.hpf, ...(CP.hum ? cleanNodes.notches : []), cleanNodes.node].forEach(n => { prev.connect(n); prev = n; });
      prev.connect(gateGainNode);
    }

    function cleanStatus() { document.getElementById('clean-status').innerText = isCleanActive ? 'Aktif' : 'Mati'; document.getElementById('clean-status').style.color = isCleanActive ? 'var(--accent-green)' : 'var(--text-muted)'; }

    async function toggleClean() {
      const cb = document.getElementById('toggle-clean'), on = cb.checked;
      if (!isRunning) { isCleanActive = on; cleanStatus(); return; }
      if (on && !cleanNodes) {
        try { await setupClean(); } catch (e) { cb.checked = false; alert(e.message); return; }
      }
      isCleanActive = on; connectMicNode(); cleanStatus();
    }
    function setCleanHum(v) { CP.hum = +v; applyClean(); if (isRunning) connectMicNode(); }
    function setCleanStrength(v) { CP.strength = v / 100; document.getElementById('val-clean').innerText = `${v}%`; applyClean(); }
    cleanStatus();

    function noteName(m) { const n = Math.round(m); return NOTE_NAMES[((n % 12) + 12) % 12] + (Math.floor(n / 12) - 1); }
    function showTune(d) {
      const el = document.getElementById('tune-note');
      if (!d.voiced) { el.innerText = '–'; return; }
      const c = Math.round((d.midi - Math.round(d.midi)) * 100);
      el.innerText = `${noteName(d.midi)} ${c >= 0 ? '+' : ''}${c}c → ${noteName(d.target)}`;
    }
    function setTuneKey(v) { TP.key = +v; sendTune(); }
    function setTuneMode(v) { TP.mode = v; sendTune(); }
    function setTuneAmount(v) {
      TP.amount = v / 100; sendTune();
      document.getElementById('val-tune-amount').innerText = `${v}%`;
    }
    function setTuneSpeed(v) {
      TP.speed = v / 100; sendTune();
      document.getElementById('val-tune-speed').innerText = v == 0 ? 'Instan (robot)' : v + '%';
    }

    // ===== YOUTUBE INSTRUMENTAL (Player Terpisah, Tidak Lewat Mixer) =====
    let ytPlayer = null, ytReady = false, ytTimer = null, ytApiLoading = null;

    function parseYouTubeId(u) {
      const t = String(u).trim();
      const m = t.match(/(?:youtu\.be\/|[?&]v=|\/embed\/|\/shorts\/|\/live\/)([\w-]{11})/) || t.match(/^([\w-]{11})$/);
      return m ? m[1] : '';
    }
    function ytMsg(t, c) { const el = document.getElementById('yt-status'); el.innerText = t; el.style.color = c || 'var(--text-muted)'; }
    function ytHelp(t) { document.getElementById('yt-help').innerText = t || ''; }

    function loadYtApi() {
      if (window.YT && YT.Player) return Promise.resolve();
      if (ytApiLoading) return ytApiLoading;
      ytApiLoading = new Promise((res, rej) => {
        window.onYouTubeIframeAPIReady = () => res();
        const sc = document.createElement('script');
        sc.src = 'https://www.youtube.com/iframe_api';
        sc.onerror = () => { ytApiLoading = null; rej(new Error('Gagal memuat YouTube (butuh internet).')); };
        document.head.appendChild(sc);
      });
      return ytApiLoading;
    }

    async function loadYouTube() {
      const id = parseYouTubeId(document.getElementById('yt-url').value);
      if (!id) { openYouTubeSite(); return; }
      document.getElementById('yt-wrap').classList.remove('hidden');
      ytHelp(''); ytMsg('Memuat...');
      try { await loadYtApi(); } catch (e) { ytMsg('Gagal', 'var(--accent-magenta)'); ytHelp(e.message); return; }
      if (ytPlayer && ytReady) { ytPlayer.loadVideoById(id); return; }
      ytPlayer = new YT.Player('yt-player', {
        videoId: id,
        playerVars: { playsinline: 1, rel: 0 },
        events: {
          onReady: () => { ytReady = true; setYtVolume(document.getElementById('yt-vol').value); ytMsg('Siap'); startYtTimer(); },
          onStateChange: e => ytMsg(e.data === 1 ? '▶ Play' : e.data === 2 ? '⏸ Pause' : e.data === 3 ? 'Buffering...' : e.data === 0 ? 'Selesai' : 'Siap'),
          onError: e => {
            const c = e.data;
            ytMsg('Error ' + c, 'var(--accent-magenta)');
            ytHelp(c === 153 || c === 152 ? 'YouTube menolak embed dari halaman file lokal (error ' + c + '). Buka halaman ini lewat http://localhost (mis. Termux: python -m http.server 8080) atau https (GitHub Pages), atau pakai tombol "Buka di YouTube".'
              : c === 101 || c === 150 ? 'Pemilik video melarang diputar di luar YouTube. Coba video lain atau "Buka di YouTube".'
              : c === 100 ? 'Video tidak ditemukan atau privat.' : 'Gagal memutar (kode ' + c + ').');
          }
        }
      });
    }

    function startYtTimer() {
      if (ytTimer) return;
      ytTimer = setInterval(() => {
        if (isPlaying || !ytPlayer || !ytReady || !currentLyrics.length || ytPlayer.getPlayerState() !== 1) return;
        const elapsed = ytPlayer.getCurrentTime() + lyricOffset;
        let a = -1;
        for (let i = 0; i < currentLyrics.length; i++) if (elapsed >= currentLyrics[i].time) a = i;
        document.getElementById('lyric-current').innerText = currentLyrics[a] ? currentLyrics[a].text : "Musik Berjalan...";
        document.getElementById('lyric-next').innerText = `Berikutnya: ${currentLyrics[a + 1] ? currentLyrics[a + 1].text : "(Akhir Lirik)"}`;
      }, 200);
    }

    function setYtVolume(v) {
      document.getElementById('val-yt-vol').innerText = `${v}%`;
      if (ytPlayer && ytReady) ytPlayer.setVolume(+v);
    }

    function openYouTubeSite() {
      const q = document.getElementById('yt-url').value.trim();
      const id = /youtu/i.test(q) ? parseYouTubeId(q) : '';
      if (id) { window.open('https://www.youtube.com/watch?v=' + id, '_blank'); return; }
      const term = q && !/instrumental|karaoke/i.test(q) ? q + ' instrumental' : q;
      window.open(term ? 'https://www.youtube.com/results?search_query=' + encodeURIComponent(term) : 'https://www.youtube.com/', '_blank');
    }

    function openYouTube() {
      const id = parseYouTubeId(document.getElementById('yt-url').value);
      if (!id) { alert('Tempel link YouTube dulu.'); return; }
      window.open('https://www.youtube.com/watch?v=' + id, '_blank');
    }

    function stopYouTube() {
      if (ytTimer) { clearInterval(ytTimer); ytTimer = null; }
      if (ytPlayer) { try { ytPlayer.destroy(); } catch (e) {} ytPlayer = null; ytReady = false; }
      const w = document.getElementById('yt-wrap');
      w.innerHTML = '<div id="yt-player"></div>'; w.classList.add('hidden');
      ytMsg('–'); ytHelp('');
    }

    if (location.protocol === 'file:') ytHelp('Halaman ini dibuka dari file lokal: YouTube biasanya menolak embed (error 153). Solusi: buka lewat http://localhost (mis. Termux: python -m http.server 8080) atau hosting https. Atau pakai "Buka di YouTube".');

    function toggleDSPChain() { isDspActive = document.getElementById('toggle-dsp').checked; connectMicNode(); }
    function toggleCompressor() { isCompActive = document.getElementById('toggle-comp').checked; connectMicNode(); }

    function setGateKnob(val) {
      gateKnobValue = parseFloat(val);
      document.getElementById('val-gate-knob').innerText = gateKnobValue.toFixed(1);
    }

    function setCompressorThreshold(val) {
      S.thresh = +val;
      document.getElementById('val-comp-thresh').innerText = `${val} dB`;
      ap(compressorNode && compressorNode.threshold, val);
    }

    function setCompressorRatio(val) {
      S.ratio = +val;
      document.getElementById('val-comp-ratio').innerText = `${val}:1`;
      ap(compressorNode && compressorNode.ratio, val);
    }

    function handleAudioUpload(e) {
      if (!isRunning) initAudioEngine();
      const file = e.target.files[0]; if (!file) return;
      const reader = new FileReader();
      reader.onload = function(evt) {
        audioCtx.decodeAudioData(evt.target.result, function(buffer) {
          audioBuffer = buffer;
          pauseOffset = 0;
          document.getElementById('track-total-time').innerText = formatTime(buffer.duration);
          playAudioAtOffset(0);
        });
      };
      reader.readAsArrayBuffer(file);
    }

    function playAudioAtOffset(offset) {
      if (!audioBuffer || !isRunning) return;
      if (fileSourceNode) { try { fileSourceNode.stop(); } catch(e){} }
      fileSourceNode = audioCtx.createBufferSource();
      fileSourceNode.buffer = audioBuffer;
      fileSourceNode.connect(chGains[1]);
      startTime = audioCtx.currentTime - offset;
      pauseOffset = offset;
      fileSourceNode.start(0, offset);
      isPlaying = true;
      document.getElementById('btn-play-pause').innerText = 'Pause';
      document.getElementById('btn-play-pause').className = 'btn btn-green';
    }

    function togglePlayPause() {
      if (!audioBuffer || !isRunning) return;
      const btn = document.getElementById('btn-play-pause');
      if (isPlaying) {
        pauseOffset = audioCtx.currentTime - startTime;
        if (fileSourceNode) { try { fileSourceNode.stop(); } catch(e){} }
        isPlaying = false;
        btn.innerText = 'Play';
        btn.className = 'btn btn-cyan';
      } else {
        playAudioAtOffset(pauseOffset);
      }
    }

    function restartAudio() {
      if (!audioBuffer || !isRunning) return;
      pauseOffset = 0;
      playAudioAtOffset(0);
    }

    function previewSeek(percent) {
      if (!audioBuffer) return;
      document.getElementById('track-current-time').innerText = formatTime((percent / 100) * audioBuffer.duration);
    }

    function seekAudio(percent) {
      if (!audioBuffer || !isRunning) return;
      const t = (percent / 100) * audioBuffer.duration;
      if (isPlaying) playAudioAtOffset(t); else pauseOffset = t;
    }

    function formatTime(seconds) {
      const mins = Math.floor(seconds / 60);
      const secs = Math.floor(seconds % 60);
      return `${mins}:${secs < 10 ? '0' : ''}${secs}`;
    }

    function handleLrcUpload(e) {
      const file = e.target.files[0];
      if (!file) return;
      const reader = new FileReader();
      reader.onload = function(evt) {
        const text = evt.target.result;
        const lines = text.split('\n');
        const parsed = [];
        const timeRegex = /\[(\d{1,3}):(\d{2})(?:[.:](\d{1,3}))?\]/g;

        lines.forEach(line => {
          let match;
          const timestamps = [];
          while ((match = timeRegex.exec(line)) !== null) {
            const min = parseInt(match[1]);
            const sec = parseInt(match[2]);
            const frac = match[3] ? parseFloat('0.' + match[3]) : 0;
            timestamps.push(min * 60 + sec + frac);
          }
          const lyricText = line.replace(/\[\d{1,3}:\d{2}(?:[.:]\d{1,3})?\]/g, '').trim();
          if (lyricText && timestamps.length > 0) {
            timestamps.forEach(time => parsed.push({ time, text: lyricText }));
          }
        });

        if (parsed.length > 0) {
          currentLyrics = parsed.sort((a, b) => a.time - b.time);
          alert(`Sukses! Memuat ${currentLyrics.length} baris lirik.`);
        } else {
          alert("Format LRC tidak valid.");
        }
      };
      reader.readAsText(file);
    }

    function adjustSync(sec) {
      lyricOffset += sec;
      document.getElementById('sync-offset-label').innerText = `Offset: ${lyricOffset > 0 ? '+' : ''}${lyricOffset.toFixed(1)}s`;
    }

    function updateLyricsAndProgress() {
      if (!audioBuffer || !isRunning) return;
      let currentPos = 0;
      if (isPlaying) {
        currentPos = audioCtx.currentTime - startTime;
        if (currentPos >= audioBuffer.duration) {
          isPlaying = false; currentPos = audioBuffer.duration; pauseOffset = 0;
          document.getElementById('btn-play-pause').innerText = '▶️ Play';
          document.getElementById('btn-play-pause').className = 'btn btn-cyan';
        }
      } else {
        currentPos = pauseOffset;
      }

      if (!seekDragging) {
        document.getElementById('track-current-time').innerText = formatTime(currentPos);
        document.getElementById('seek-bar').value = (currentPos / audioBuffer.duration) * 100;
      }

      if (currentLyrics.length > 0) {
        const elapsed = currentPos + lyricOffset;
        let activeIndex = -1;
        for (let i = 0; i < currentLyrics.length; i++) {
          if (elapsed >= currentLyrics[i].time) activeIndex = i;
        }
        document.getElementById('lyric-current').innerText = currentLyrics[activeIndex] ? currentLyrics[activeIndex].text : "Musik Berjalan...";
        document.getElementById('lyric-next').innerText = `Berikutnya: ${currentLyrics[activeIndex + 1] ? currentLyrics[activeIndex + 1].text : "(Akhir Lirik)"}`;
      }
    }

    function toggleRoute(inIdx, outIdx) {
      routing[inIdx][outIdx] = routing[inIdx][outIdx] ? 0 : 1;
      const btn = document.getElementById(`r-${inIdx}-${outIdx}`);
      const activeClass = outIdx === 0 ? 'active-a' : 'active-b';
      if (routing[inIdx][outIdx]) btn.classList.add(activeClass);
      else btn.classList.remove(activeClass);
      updateRoutingConnections();
    }

    function ap(param, v) { if (isRunning && param) param.setValueAtTime(parseFloat(v), audioCtx.currentTime); }

    function setGain(ch, val) {
      S.gain[ch] = +val; ap(chGains[ch] && chGains[ch].gain, val);
      document.getElementById(`val-gain-ch${ch+1}`).innerText = `${Math.round(val * 100)}%`;
    }

    function setMasterGain(idx, val) {
      S.master[idx] = +val; ap(masterGains[idx] && masterGains[idx].gain, val);
      document.getElementById(idx === 0 ? 'val-master-a' : 'val-master-b').innerText = `${Math.round(val * 100)}%`;
    }

    function setEQ(band, val) {
      S.eq[band] = +val;
      document.getElementById(`val-eq-${band}`).innerText = `${val} dB`;
      const n = { low: eqLow, mid: eqMid, high: eqHigh }[band];
      ap(n && n.gain, val);
    }

    function setDelay(val) {
      S.delay = +val;
      document.getElementById('val-delay').innerText = `${Math.round(val * 1000)} ms`;
      ap(delayNode && delayNode.delayTime, val);
      ap(delayWet && delayWet.gain, val > 0 ? 0.5 : 0);
    }

    function setReverb(val) {
      S.reverb = +val;
      document.getElementById('val-reverb').innerText = `${Math.round(val * 100)}%`;
      ap(reverbWet && reverbWet.gain, val);
    }

    function renderVisualizers() {
      if (rafId) cancelAnimationFrame(rafId);
      const canvas = document.getElementById('masterCanvas');
      const ctx = canvas.getContext('2d');
      canvas.width = canvas.clientWidth; canvas.height = canvas.clientHeight;

      function draw() {
        if (!isRunning) return;
        if (canvas.width !== canvas.clientWidth || canvas.height !== canvas.clientHeight) { canvas.width = canvas.clientWidth; canvas.height = canvas.clientHeight; }
        const bufferLength = masterAnalyser.frequencyBinCount;
        const dataArray = new Uint8Array(bufferLength);
        masterAnalyser.getByteFrequencyData(dataArray);

        ctx.fillStyle = '#05070a';
        ctx.fillRect(0, 0, canvas.width, canvas.height);

        const barCount = 32; 
        const barWidth = (canvas.width / barCount) - 2;
        let x = 0;

        for (let i = 0; i < barCount; i++) {
          const index = Math.floor(i * (bufferLength / barCount));
          const barHeight = (dataArray[index] / 255) * canvas.height;
          const gradient = ctx.createLinearGradient(0, canvas.height, 0, 0);
          gradient.addColorStop(0, '#00f0ff'); gradient.addColorStop(0.6, '#00ff66'); gradient.addColorStop(1, '#ff0055');
          ctx.fillStyle = gradient;
          ctx.fillRect(x, canvas.height - barHeight, barWidth, barHeight);
          x += barWidth + 2;
        }

        updateLyricsAndProgress();

        for (let i = 0; i < 2; i++) {
          if (chAnalysers[i]) {
            const arr = new Uint8Array(chAnalysers[i].frequencyBinCount);
            chAnalysers[i].getByteFrequencyData(arr);
            let sum = 0; for (let b = 0; b < arr.length; b++) sum += arr[b];
            let meterPercent = Math.min(100, ((sum / arr.length) / 128) * 100);
            document.getElementById(`meter-ch${i+1}`).style.width = `${meterPercent}%`;
          }
        }
        rafId = requestAnimationFrame(draw);
      }
      draw();
    }

    function switchTab(tabId) {
      ['mixer', 'dsp'].forEach(t => {
        document.getElementById(`view-${t}`).classList.toggle('hidden', t !== tabId);
        document.getElementById(`tab-${t}`).classList.toggle('active', t === tabId);
      });
    }
  </script>
</body>
</html>
