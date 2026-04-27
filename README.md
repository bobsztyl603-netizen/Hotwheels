<!DOCTYPE html>
<html lang="pl">
<head>
<meta charset="UTF-8">
<title>Hot Wheels Ultimate</title>

<style>
body {
    margin: 0;
    font-family: Arial;
    background: black;
    color: white;
    text-align: center;
}

/* HEADER */
header {
    background: linear-gradient(90deg, red, orange);
    padding: 20px;
    animation: glow 2s infinite alternate;
}

@keyframes glow {
    from { box-shadow: 0 0 10px red; }
    to { box-shadow: 0 0 30px orange; }
}

/* SEKCJE */
.section {
    padding: 30px;
}

/* PRZYCISKI */
button {
    padding: 10px 20px;
    background: red;
    border: none;
    color: white;
    cursor: pointer;
    border-radius: 10px;
}

button:hover {
    background: orange;
}

/* GRA */
#gameBox {
    width: 300px;
    height: 200px;
    background: #111;
    margin: auto;
    position: relative;
    overflow: hidden;
    border: 2px solid red;
}

#car {
    width: 40px;
    height: 40px;
    background: orange;
    position: absolute;
    bottom: 10px;
    left: 130px;
}

.obstacle {
    width: 40px;
    height: 40px;
    background: red;
    position: absolute;
    top: 0;
}

/* TABELA */
table {
    margin: auto;
    border-collapse: collapse;
    width: 80%;
}

th, td {
    border: 1px solid white;
    padding: 10px;
}

th {
    cursor: pointer;
    background: #222;
}

</style>
</head>

<body>

<header>
<h1>🔥 HOT WHEELS ULTIMATE 🔥</h1>
<p>Strona z grą, ciekawostkami i tabelą</p>
</header>

<!-- CIEKAWOSTKI -->
<div class="section">
<h2>Ciekawostki</h2>
<p id="fact">Kliknij przycisk, aby zobaczyć ciekawostkę!</p>
<button onclick="newFact()">Losuj ciekawostkę</button>
</div>

<!-- GRA -->
<div class="section">
<h2>Mini Gra 🚗</h2>
<p>Unikaj przeszkód (strzałki ← →)</p>
<div id="gameBox">
<div id="car"></div>
</div>
<p>Wynik: <span id="score">0</span></p>
<button onclick="startGame()">Start</button>
</div>

<!-- TABELA -->
<div class="section">
<h2>Top auta (Spreadsheet)</h2>
<table id="table">
<tr>
<th onclick="sortTable(0)">Model</th>
<th onclick="sortTable(1)">Prędkość</th>
</tr>
<tr><td>Fire Storm</td><td>320</td></tr>
<tr><td>Turbo Beast</td><td>300</td></tr>
<tr><td>Night Racer</td><td>280</td></tr>
</table>
</div>

<script>

/* 🔥 CIEKAWOSTKI */
const facts = [
"Hot Wheels powstało w 1968 roku",
"Pierwsze modele były szybsze niż Matchbox",
"Co roku powstaje ponad 250 nowych modeli",
"Najdroższy model kosztował ponad 100 000$"
];

function newFact() {
    document.getElementById("fact").innerText =
    facts[Math.floor(Math.random()*facts.length)];
}

/* 🎮 GRA */
let car = document.getElementById("car");
let gameBox = document.getElementById("gameBox");
let score = 0;
let gameRunning = false;

document.addEventListener("keydown", function(e){
    let left = car.offsetLeft;
    if(e.key === "ArrowLeft" && left > 0) {
        car.style.left = left - 20 + "px";
    }
    if(e.key === "ArrowRight" && left < 260) {
        car.style.left = left + 20 + "px";
    }
});

function startGame() {
    if(gameRunning) return;
    gameRunning = true;
    score = 0;
    document.getElementById("score").innerText = score;

    let interval = setInterval(() => {
        let obs = document.createElement("div");
        obs.classList.add("obstacle");
        obs.style.left = Math.random()*260 + "px";
        gameBox.appendChild(obs);

        let move = setInterval(() => {
            let top = obs.offsetTop;
            if(top > 160) {
                if(Math.abs(obs.offsetLeft - car.offsetLeft) < 40) {
                    alert("Game Over! Wynik: " + score);
                    location.reload();
                }
            }
            obs.style.top = top + 5 + "px";

            if(top > 200) {
                obs.remove();
                clearInterval(move);
                score++;
                document.getElementById("score").innerText = score;
            }
        }, 30);

    }, 1000);
}

/* 📊 SORTOWANIE TABELI */
function sortTable(n) {
    let table = document.getElementById("table");
    let rows = Array.from(table.rows).slice(1);
    let asc = table.getAttribute("data-sort") !== "asc";

    rows.sort((a,b) => {
        let x = a.cells[n].innerText;
        let y = b.cells[n].innerText;
        return asc ? x.localeCompare(y, undefined, {numeric:true}) 
                   : y.localeCompare(x, undefined, {numeric:true});
    });

    table.setAttribute("data-sort", asc ? "asc" : "desc");

    rows.forEach(r => table.appendChild(r));
}

</script>

</body>
</html>
