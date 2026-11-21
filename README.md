<!DOCTYPE html>

<html lang="ms">

<head>

<meta charset="UTF-8">

<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">

<title>Secret Task Game</title>

<style>

* { box-sizing: border-box; }

html, body {

    margin: 0;

    padding: 0;

    width: 100%;

    min-height: 100%;

    font-family: Arial, sans-serif;

    background: #111827;

    color: white;

}

body {

    min-height: 100vh;

    display: flex;

    justify-content: center;

    align-items: center;

}

.game {

    width: 100%;

    max-width: 430px;

    min-height: 100vh;

    padding: 65px 16px 20px;

    display: flex;

    justify-content: center;

    align-items: center;

    position: relative;

}

.reset-btn {

    position: fixed;

    top: 14px;

    right: 14px;

    width: auto;

    min-width: 65px;

    min-height: 36px;

    padding: 8px 12px;

    margin: 0;

    border-radius: 10px;

    background: #374151;

    color: #d1d5db;

    border: 1px solid #4b5563;

    font-size: 11px;

    font-weight: bold;

    z-index: 9999;

}

.card {

    width: 100%;

    background: #1f2937;

    border-radius: 24px;

    padding: 28px 20px;

    box-shadow: 0 15px 40px rgba(0,0,0,.35);

    text-align: center;

}

h1 {

    margin: 0;

    font-size: 28px;

    font-weight: 900;

    letter-spacing: .5px;

}

.subtitle {

    margin-top: 8px;

    color: #9ca3af;

    font-size: 14px;

    line-height: 1.5;

}

.notice {

    margin: 20px 0;

    padding: 15px;

    background: #111827;

    border-radius: 15px;

    color: #d1d5db;

    font-size: 14px;

    line-height: 1.5;

}

button {

    width: 100%;

    min-height: 52px;

    border: 0;

    border-radius: 14px;

    padding: 14px;

    margin-top: 10px;

    font-size: 16px;

    font-weight: 700;

    cursor: pointer;

    background: #6366f1;

    color: white;

    -webkit-tap-highlight-color: transparent;

}

button:active {

    transform: scale(.97);

}

.player-inputs {

    width: 100%;

    margin-top: 20px;

    display: flex;

    flex-direction: column;

    gap: 10px;

    max-height: 45vh;

    overflow-y: auto;

    padding: 2px;

}

.player-input-row {

    display: flex;

    align-items: center;

    gap: 8px;

}

.player-number {

    width: 34px;

    min-width: 34px;

    height: 50px;

    display: flex;

    justify-content: center;

    align-items: center;

    background: #374151;

    border-radius: 12px;

    color: #9ca3af;

    font-size: 13px;

    font-weight: bold;

}

.player-input {

    flex: 1;

    min-width: 0;

    width: 100%;

    height: 50px;

    border: 2px solid #374151;

    border-radius: 12px;

    background: #111827;

    color: white;

    padding: 0 14px;

    font-size: 16px;

    outline: none;

}

.player-input:focus {

    border-color: #6366f1;

}

.player-input::placeholder {

    color: #6b7280;

}

.remove-player {

    width: 40px;

    min-width: 40px;

    height: 50px;

    min-height: 50px;

    padding: 0;

    margin: 0;

    border-radius: 12px;

    background: #374151;

    color: #f87171;

    font-size: 20px;

}

.add-player-btn {

    background: #374151;

    border: 1px dashed #6b7280;

    color: #d1d5db;

    margin-top: 12px;

}

.players {

    width: 100%;

    display: grid;

    grid-template-columns: repeat(2, minmax(0, 1fr));

    gap: 10px;

    margin-top: 20px;

    max-height: 48vh;

    overflow-y: auto;

    padding-right: 2px;

}

.player {

    margin: 0;

    background: #374151;

    min-width: 0;

    overflow-wrap: anywhere;

}

.player.selected {

    background: #4f46e5;

}

.reveal-box {

    margin-top: 20px;

    padding: 25px 15px;

    background: #111827;

    border-radius: 18px;

}

.label {

    color: #9ca3af;

    font-size: 12px;

    text-transform: uppercase;

    letter-spacing: 1px;

}

.target {

    margin-top: 6px;

    margin-bottom: 24px;

    font-size: 34px;

    font-weight: 900;

    overflow-wrap: anywhere;

}

.word {

    margin-top: 6px;

    font-size: 28px;

    font-weight: 900;

    color: #facc15;

    overflow-wrap: anywhere;

}

.warning {

    margin-top: 16px;

    color: #f87171;

    font-size: 12px;

    line-height: 1.5;

}

.round-info {

    margin-top: 15px;

    color: #9ca3af;

    font-size: 13px;

}

.hidden {

    display: none !important;

}

@media (max-width: 360px) {

    .game { padding: 60px 12px 15px; }

    .card { padding: 24px 16px; border-radius: 20px; }

    h1 { font-size: 25px; }

    .target { font-size: 30px; }

    .word { font-size: 24px; }

    .player { font-size: 14px; padding: 12px 8px; }

}

</style>

</head>

<body>

<div class="game">

<button id="resetButton" class="reset-btn" type="button">RESET</button>

<div id="setupScreen" class="card">

    <h1>SECRET TASK</h1>

    <div class="subtitle">Masukkan nama semua pemain</div>

    <div class="notice">

        Taip nama pemain dan tekan <strong>ENTER</strong>

        untuk tambah pemain seterusnya.

    </div>

    <div id="playerInputs" class="player-inputs"></div>

    <button id="addPlayerButton" class="add-player-btn" type="button">

        + TAMBAH PEMAIN

    </button>

    <button id="startButton" type="button">MULA GAME</button>

</div>

<div id="playerScreen" class="card hidden">

    <h1>PILIH NAMA ANDA?</h1>

    <div class="subtitle">Pilih nama anda.</div>

    <div class="round-info">

        Round <span id="roundNumber">1</span>

    </div>

    <div id="playersContainer" class="players"></div>

    <div id="selectedMessage" class="notice hidden">

        Pastikan anda adalah <strong id="selectedName"></strong>

    </div>

    <button id="revealButton" class="hidden" type="button">

        REVEAL WORD

    </button>

    <div class="warning">

        Pastikan tiada orang lain sedang tengok skrin.

    </div>

</div>

<div id="revealScreen" class="card hidden">

    <h1>TUGASAN RAHSIA</h1>

    <div class="subtitle">Jangan tunjuk kepada orang lain.</div>

    <div class="reveal-box">

        <div class="label">TARGET ANDA PERLU PAKSA</div>

        <div id="targetName" class="target">----</div>

        <div class="label">UNTUK SEBUT PERKATAAN</div>

        <div id="targetWord" class="word">----</div>

    </div>

    <button id="hideButton" type="button">HIDE &amp; PASS PHONE</button>

</div>

<div id="endScreen" class="card hidden">
    <h1>ROUND SELESAI</h1>
    <div id="endNotice" class="notice">
        WAH! SEMUA PEMAIN DAH MELIHAT TUGASAN MASING-MASING. BINCANGKAN SUASANA DAN SITUASI KALIAN BERADA ⁉️
        <br><br>
        Sekarang mula bermain. Selepas game tamat, tekan REVEAL WORD sebelum Round Two.
    </div>
    <div id="finalWordReveal" class="reveal-box hidden">
        <div class="label">PERKATAAN ROUND INI</div>
        <div id="finalWordList" class="word"></div>
    </div>
    <button id="finalRevealButton" type="button">REVEAL WORD</button>
    <button id="newRoundButton" type="button">ROUND TWO</button>
</div>

</div>

<script>

"use strict";

/* =========================

   SENARAI PERKATAAN

========================= */

const words = [

"Sofa","Merapu","Minuman","Makanan","Sikat","Cermin",

"Tingkap","Langsir","Karpet","TV","Lampu","Jam",

"Penyapu","Vakum","Tong Sampah","Buku","Bantal",

"Selimut","Almari","Plug","Meja","Laci","Kotak",

"Decor","Tema","Baju","Iron","Kunci","Tapis",

"Tinggi","Heater","Penyapu Lidi","Peti Ais","Lampu LED",

"Jam Alarm","Rak","Cinta","Tilam","Topi","Nasi Lemak",

"Bintang","Malam","Ayam","Siang","Laksa","Mee","Nasi",

"Langit","Bangun","Tidur","Makan","Minum","Air","Kopi",

"Teh","Roti","Lapar","Kenyang","Tandas","Mandian",

"Sabun","Syampu","Berus Gigi","Ubat Gigi","Selipar",

"Kasut","Seluar","Telefon","Charger","Powerbank","WiFi",

"Internet","Password","WhatsApp","Call","Message","Group",

"Reply","Seen","Online","Offline","Kerja","Office","Bos",

"Meeting","Email","Printer","Dokumen","File","Folder",

"Pen","Kertas","Kerusi","Komputer","Laptop","Mouse",

"Keyboard","Remote","Aircond","Kipas","Pintu","Rumah",

"Bilik","Dapur","Kereta","Motor","Minyak","Parking",

"Jalan","Traffic","Grab","Bas","Petrol","Kedai","Pasar",

"Restoran","Warung","Makan Luar","Order","Delivery",

"GrabFood","Bil","Duit","Wallet","Bank","Transfer","Cash",

"QR","Shopping","Barang","Plastik","Beg","Dompet","Alarm",

"Cuaca","Hujan","Panas","Sejuk","Penat","Malas","Busy",

"Free","Esok","Nanti","Sekejap","Jap","Mana","Kenapa",

"Macam Mana","Betul","Tak","Boleh","Tak Boleh","Okay",

"Serius","Alamak","Jap Lagi","Jom","Balik","Keluar",

"Masuk","Pergi","Datang","Tunggu","Cepat","Lambat","Diam",

"Dengar","Tengok","Pegang","Letak","Ambil","Bagitahu",

"Jangan","Buat","Siap","Belum","Dah","Sudah","Belum Lagi",

"Mana Tahu","Tak Tahu","Takpe","Betul Ke","Serius Lah",

"Bro","Weh","Oi","Guys","Member","Boss","Abang","Dik",

"Mak","Ayah","Kawan","Orang","Nama","Cool","Kelakar",

"Garang","Comel","Pandai","Pemalas","Rajin","Pendiam",

"Bising","Baik","Nakal","Kedekut","Boros","Penakut",

"Berani","Penipu","Penyayang","Sombong","Peramah","Poyo",

"Gila-gila","Bijak","Laju","Kuat","Lemah","Panas Baran",

"Sejuk Hati","Tenang","Pelupa","Kepoh","Rahsia","Yakin",

"Pemalu","Kreatif","Cermat","Cuai","Kemas","Serabut",


];

/* =========================

   STATE

========================= */

let players = [];

let assignments = {};

let selectedPlayer = null;

let viewedPlayers = [];

let round = 1;

/* =========================

   ELEMENTS

========================= */

const $ = id => document.getElementById(id);

const setupScreen = $("setupScreen");

const playerScreen = $("playerScreen");

const revealScreen = $("revealScreen");

const endScreen = $("endScreen");

const playerInputs = $("playerInputs");

const playersContainer = $("playersContainer");

const selectedMessage = $("selectedMessage");

const selectedName = $("selectedName");

const revealButton = $("revealButton");

const targetName = $("targetName");

const targetWord = $("targetWord");

const roundNumber = $("roundNumber");
const finalRevealButton = $("finalRevealButton");

/* =========================

   ADD PLAYER INPUT

========================= */

function addPlayerInput(focusInput = true) {

    const number = playerInputs.querySelectorAll(".player-input-row").length + 1;

    const row = document.createElement("div");

    row.className = "player-input-row";

    const numberBox = document.createElement("div");

    numberBox.className = "player-number";

    numberBox.textContent = number;

    const input = document.createElement("input");

    input.type = "text";

    input.className = "player-input";

    input.placeholder = "Nama pemain " + number;

    input.autocomplete = "off";

    input.spellcheck = false;

    const removeButton = document.createElement("button");

    removeButton.type = "button";

    removeButton.className = "remove-player";

    removeButton.textContent = "×";

    removeButton.title = "Buang pemain";

    input.addEventListener("keydown", event => {

        if (event.key === "Enter") {

            event.preventDefault();

            addPlayerInput(true);

        }

    });

    removeButton.addEventListener("click", () => {

        row.remove();

        renumberInputs();

        if (!playerInputs.querySelector(".player-input-row")) {

            addPlayerInput(false);

        }

    });

    row.append(numberBox, input, removeButton);

    playerInputs.appendChild(row);

    if (focusInput) input.focus();

}

function renumberInputs() {

    playerInputs.querySelectorAll(".player-input-row").forEach((row, index) => {

        const number = index + 1;

        row.querySelector(".player-number").textContent = number;

        row.querySelector(".player-input").placeholder = "Nama pemain " + number;

    });

}

$("addPlayerButton").addEventListener("click", () => addPlayerInput());

/* =========================

   GET PLAYER NAMES

========================= */

function getPlayerNames() {

    const names = [];

    playerInputs.querySelectorAll(".player-input").forEach(input => {

        const name = input.value.trim();

        if (!name) return;

        const duplicate = names.some(

            existing => existing.toLowerCase() === name.toLowerCase()

        );

        if (!duplicate) names.push(name);

    });

    return names;

}

/* =========================

   SHUFFLE

========================= */

function shuffle(array) {

    const result = [...array];

    for (let i = result.length - 1; i > 0; i--) {

        const j = Math.floor(Math.random() * (i + 1));

        [result[i], result[j]] = [result[j], result[i]];

    }

    return result;

}

/* =========================

   GENERATE TARGETS

   NO SELF-TARGET

========================= */

function generateTargets() {

    const targets = [...players];

    // Derangement: setiap pemain mendapat target orang lain.

    // Putaran rawak mengelakkan gelung shuffle tanpa henti.

    for (let i = targets.length - 1; i > 0; i--) {

        const j = Math.floor(Math.random() * i);

        [targets[i], targets[j]] = [targets[j], targets[i]];

    }

    return targets;

}

/* =========================

   CREATE ASSIGNMENTS

========================= */

function createAssignments() {

    if (players.length < 2) {

        alert("Game memerlukan sekurang-kurangnya 2 pemain.");

        return false;

    }

    if (words.length < players.length) {

        alert("Jumlah perkataan tidak cukup untuk semua pemain.");

        return false;

    }

    // Pastikan tiada pemain menjadi target dirinya sendiri.

    let targets;

    let attempts = 0;

    do {

        targets = shuffle(players);

        attempts++;

    } while (

        targets.some((target, index) => target === players[index]) &&

        attempts < 1000

    );

    // Fallback selamat jika susunan rawak masih sama.

    if (targets.some((target, index) => target === players[index])) {

        targets = [...players.slice(1), players[0]];

    }

    const randomWords = shuffle(words);

    assignments = {};

    players.forEach((player, index) => {

        assignments[player] = {

            target: targets[index],

            word: randomWords[index]

        };

    });

    viewedPlayers = [];

    selectedPlayer = null;

    return true;

}

/* =========================

   RENDER PLAYERS

========================= */

function renderPlayers() {

    playersContainer.innerHTML = "";

    players.forEach(player => {

        const button = document.createElement("button");

        button.className = "player";

        button.type = "button";

        button.dataset.player = player;

        button.textContent = player;

        button.addEventListener("click", () => selectPlayer(player, button));

        playersContainer.appendChild(button);

    });

}

/* =========================

   SELECT PLAYER

========================= */

function selectPlayer(player, button) {

    if (viewedPlayers.includes(player)) return;

    selectedPlayer = player;

    playersContainer.querySelectorAll(".player").forEach(btn => {

        btn.classList.remove("selected");

    });

    button.classList.add("selected");

    selectedName.textContent = selectedPlayer;

    selectedMessage.classList.remove("hidden");

    revealButton.classList.remove("hidden");

}

/* =========================

   START GAME

========================= */

$("startButton").addEventListener("click", () => {

    const names = getPlayerNames();

    if (names.length < 2) {

        alert("Masukkan sekurang-kurangnya 2 nama pemain.");

        return;

    }

    players = names;

    if (!createAssignments()) return;

    renderPlayers();

    setupScreen.classList.add("hidden");

    revealScreen.classList.add("hidden");

    endScreen.classList.add("hidden");

    playerScreen.classList.remove("hidden");

});

/* =========================

   REVEAL TASK

========================= */

revealButton.addEventListener("click", () => {

    if (!selectedPlayer) return;

    const task = assignments[selectedPlayer];

    if (!task) return;

    targetName.textContent = task.target;

    targetWord.textContent = task.word;

    playerScreen.classList.add("hidden");

    revealScreen.classList.remove("hidden");

});

/* =========================

   HIDE AND PASS PHONE

========================= */

$("hideButton").addEventListener("click", () => {

    if (!selectedPlayer) return;

    viewedPlayers.push(selectedPlayer);

    selectedPlayer = null;

    targetName.textContent = "----";

    targetWord.textContent = "----";

    revealScreen.classList.add("hidden");

    if (viewedPlayers.length >= players.length) {

        endScreen.classList.remove("hidden");

    } else {

        resetPlayerSelection();

        playerScreen.classList.remove("hidden");

    }

});

/* =========================

   RESET PLAYER SELECTION

========================= */

function resetPlayerSelection() {

    playersContainer.querySelectorAll(".player").forEach(button => {

        button.classList.remove("selected");

        const alreadyViewed = viewedPlayers.includes(button.dataset.player);

        button.disabled = alreadyViewed;

        button.style.opacity = alreadyViewed ? "0.35" : "1";

    });

    selectedMessage.classList.add("hidden");

    revealButton.classList.add("hidden");

}

/* =========================

   NEW ROUND

========================= */

finalRevealButton.addEventListener("click", () => {
    const wordsThisRound = [...new Set(players.map(player => assignments[player]?.word).filter(Boolean))];
    $("finalWordList").textContent = wordsThisRound.join(" • ");
    $("finalWordReveal").classList.remove("hidden");
    $("endNotice").classList.add("hidden");
    finalRevealButton.classList.add("hidden");
});

$("newRoundButton").addEventListener("click", () => {

    round++;

    $("finalWordReveal").classList.add("hidden");
    $("endNotice").classList.remove("hidden");
    finalRevealButton.classList.remove("hidden");
    $("newRoundButton").textContent = "ROUND " + (round === 2 ? "TWO" : round);

    if (!createAssignments()) return;

    roundNumber.textContent = round;

    renderPlayers();

    resetPlayerSelection();

    endScreen.classList.add("hidden");

    revealScreen.classList.add("hidden");

    playerScreen.classList.remove("hidden");

});

/* =========================

   RESET FULL GAME - FIXED

========================= */

$("resetButton").addEventListener("click", () => {

    // Kosongkan semua data permainan

    players = [];

    assignments = {};

    selectedPlayer = null;

    viewedPlayers = [];

    round = 1;

    // Kosongkan paparan tugasan

    targetName.textContent = "----";

    targetWord.textContent = "----";

    selectedName.textContent = "";

    roundNumber.textContent = "1";

    // Kosongkan butang pemain dan input nama

    playersContainer.innerHTML = "";

    playerInputs.innerHTML = "";

    // Sembunyikan komponen pilihan

    selectedMessage.classList.add("hidden");

    revealButton.classList.add("hidden");

    // Paparkan skrin awal dan sembunyikan skrin lain

    setupScreen.classList.remove("hidden");

    playerScreen.classList.add("hidden");

    revealScreen.classList.add("hidden");

    endScreen.classList.add("hidden");
    $("finalWordReveal").classList.add("hidden");
    $("endNotice").classList.remove("hidden");
    finalRevealButton.classList.remove("hidden");
    $("newRoundButton").textContent = "ROUND TWO";

    // Bina semula satu input kosong

    addPlayerInput(false);

    // Kembali ke bahagian atas

    window.scrollTo(0, 0);

});

/* =========================

   INITIALIZE

========================= */

addPlayerInput(false);

</script>

</body>

</html>