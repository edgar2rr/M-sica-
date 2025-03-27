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
