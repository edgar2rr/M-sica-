<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MP3 Player</title>
    <link rel="stylesheet" href="styles.css">
</head>
<body>
    <div class="player-container">
        <h2>🎵 MP3 Player</h2>

        <div class="screen">
            <p id="currentSong">Nenhuma música selecionada</p>
        </div>

        <audio id="audioPlayer"></audio>
        <input type="range" id="progressBar" value="0">

        <div class="buttons">
            <button id="prevButton">⏮️</button>
            <button id="playButton">▶️</button>
            <button id="pauseButton">⏸️</button>
            <button id="stopButton">⏹️</button>
            <button id="nextButton">⏭️</button>
        </div>

        <button id="importButton">📥 Importar Áudio</button>
        <input type="file" id="fileInput" style="display: none;" multiple />

        <ul id="musicList"></ul> 
    </div>

    <script src="script.js"></script>
</body>
</html>


body {
    font-family: Arial, sans-serif;
    text-align: center;
    background-color: #222;
    color: white;
    padding: 20px;
}

.player-container {
    max-width: 350px;
    margin: auto;
    background: #333;
    padding: 20px;
    border-radius: 15px;
    box-shadow: 0 0 10px rgba(255, 255, 255, 0.2);
}

h2 {
    font-size: 18px;
}

.screen {
    background: black;
    color: lime;
    padding: 10px;
    margin: 10px 0;
    border-radius: 10px;
}

.buttons {
    display: flex;
    justify-content: center;
    gap: 10px;
}

button {
    background: #444;
    color: white;
    border: none;
    padding: 10px;
    font-size: 20px;
    border-radius: 50%;
    cursor: pointer;
}

button:hover {
    background: #666;
}

#progressBar {
    width: 100%;
    margin: 10px 0;
}

#musicList {
    list-style: none;
    padding: 0;
    margin-top: 10px;
}

#musicList li {
    padding: 10px;
    background: #444;
    border-bottom: 1px solid #555;
    cursor: pointer;
}

#musicList li:hover {
    background: #666;
}



const fileInput = document.getElementById('fileInput');
const audioPlayer = document.getElementById('audioPlayer');
const progressBar = document.getElementById('progressBar');
const musicList = document.getElementById('musicList');
const currentSong = document.getElementById('currentSong');

const playButton = document.getElementById('playButton');
const pauseButton = document.getElementById('pauseButton');
const stopButton = document.getElementById('stopButton');
const nextButton = document.getElementById('nextButton');
const prevButton = document.getElementById('prevButton');
const importButton = document.getElementById('importButton');

let musicFiles = [];
let currentIndex = 0;

// Abrir o seletor de arquivos ao clicar no botão de importação
importButton.addEventListener("click", () => fileInput.click());

// Importar músicas do dispositivo
fileInput.addEventListener('change', function() {
    for (const file of this.files) {
        if (file.type.startsWith('audio/')) {
            const objectURL = URL.createObjectURL(file);
            musicFiles.push({ name: file.name, url: objectURL });

            const listItem = document.createElement('li');
            listItem.textContent = file.name;
            listItem.addEventListener('click', () => playSelectedSong(musicFiles.indexOf(musicFiles.find(m => m.name === file.name))));

            musicList.appendChild(listItem);
        }
    }
});

// Reproduzir a música selecionada
function playSelectedSong(index) {
    currentIndex = index;
    audioPlayer.src = musicFiles[currentIndex].url;
    currentSong.textContent = `🎶 ${musicFiles[currentIndex].name}`;
    playAudio();
}

// Controles de reprodução
function playAudio() {
    if (audioPlayer.src) {
        audioPlayer.play();
    }
}

function pauseAudio() {
    audioPlayer.pause();
}

function stopAudio() {
    audioPlayer.pause();
    audioPlayer.currentTime = 0;
}

function nextSong() {
    if (musicFiles.length > 0) {
        currentIndex = (currentIndex + 1) % musicFiles.length;
        playSelectedSong(currentIndex);
    }
}

function prevSong() {
    if (musicFiles.length > 0) {
        currentIndex = (currentIndex - 1 + musicFiles.length) % musicFiles.length;
        playSelectedSong(currentIndex);
    }
}

// Atualizar a barra de progresso
audioPlayer.addEventListener("timeupdate", function () {
    progressBar.value = (audioPlayer.currentTime / audioPlayer.duration) * 100;
});

// Permitir avançar na música clicando na barra
progressBar.addEventListener("input", function () {
    audioPlayer.currentTime = (progressBar.value / 100) * audioPlayer.duration;
});

// Conectar botões às funções
playButton.addEventListener("click", playAudio);
pauseButton.addEventListener("click", pauseAudio);
stopButton.addEventListener("click", stopAudio);
nextButton.addEventListener("click", nextSong);
prevButton.addEventListener("click", prevSong);
