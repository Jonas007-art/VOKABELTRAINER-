<!DOCTYPE html>
<html lang="de">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1" />
  <title>Latein-Vokabeltrainer</title>
  <link rel="manifest" href="manifest.json" />
  <style>
    body {
      margin: 0;
      font-family: sans-serif;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      height: 100vh;
      background: linear-gradient(to bottom, #4facfe, #00f2fe);
      color: white;
      text-align: center;
    }
    #points {
      position: absolute;
      top: 15px;
      left: 15px;
      background: white;
      color: #4facfe;
      padding: 8px 12px;
      border-radius: 10px;
      font-weight: bold;
      font-size: 1.2em;
      user-select: none;
    }
    #word {
      font-size: 4em;
      margin-bottom: 20px;
      cursor: pointer;
      user-select: none;
      padding: 20px 40px;
      border-radius: 20px;
      background: white;
      color: #4facfe;
      box-shadow: 0 8px 15px rgba(0, 0, 0, 0.1);
      transition: background-color 0.3s ease, color 0.3s ease;
    }
    #word:hover {
      background-color: #00f2fe;
      color: white;
    }
    #input-container {
      margin-bottom: 20px;
    }
    input[type="text"] {
      font-size: 1.5em;
      padding: 10px 15px;
      border-radius: 12px;
      border: none;
      width: 250px;
      max-width: 90vw;
      text-align: center;
      outline: none;
    }
    button {
      font-size: 1.5em;
      background-color: #ffce00;
      border: none;
      border-radius: 12px;
      padding: 10px 25px;
      cursor: pointer;
      font-weight: bold;
      color: #4facfe;
      box-shadow: 0 5px 8px rgba(0, 0, 0, 0.2);
      transition: background-color 0.3s ease;
    }
    button:hover {
      background-color: #e6b800;
    }
    #feedback {
      font-size: 1.4em;
      height: 40px;
      margin-top: 10px;
      user-select: none;
    }
  </style>
</head>
<body>
  <div id="points">Punkte: 0</div>
  <div id="word">Tippe hier!</div>

  <div id="input-container">
    <input id="answer" type="text" placeholder="Übersetzung hier eingeben" autocomplete="off" autocorrect="off" autocapitalize="none" spellcheck="false" />
  </div>
  <button id="checkBtn">Prüfen</button>
  <div id="feedback"></div>

  <script>
    const vokabeln = [
      { latein: "vidēre", deutsch: "sehen", perfekt: "vīdi" },
      { latein: "currere", deutsch: "laufen", perfekt: "cucurri" },
      { latein: "dīcere", deutsch: "sagen", perfekt: "dīxī" },
      { latein: "venīre", deutsch: "kommen", perfekt: "vēnī" },
      { latein: "capere", deutsch: "fassen, nehmen", perfekt: "cēpī" },
      { latein: "facere", deutsch: "machen, tun", perfekt: "fēcī" },
      { latein: "dūcere", deutsch: "führen", perfekt: "dūxī" },
      { latein: "accēdere", deutsch: "herankommen", perfekt: "accessī" },
      { latein: "legiō", deutsch: "Legion", perfekt: "" },
      { latein: "vincere", deutsch: "(be)siegen", perfekt: "vīcī" },
      { latein: "puls", deutsch: "schlagen", perfekt: "" },
      { latein: "narrāre", deutsch: "erzählen", perfekt: "narrāvī" },
      { latein: "animadvertere", deutsch: "wahrnehmen", perfekt: "animadvertī" },
      { latein: "manēre", deutsch: "bleiben", perfekt: "mansī" },
      { latein: "rīdēre", deutsch: "lachen", perfekt: "rīsī" },
      { latein: "expellere", deutsch: "vertreiben", perfekt: "expulī" },
      { latein: "altus", deutsch: "hoch, tief", perfekt: "" },
      { latein: "bellum", deutsch: "Krieg", perfekt: "" },
      { latein: "Italia", deutsch: "Italien", perfekt: "" },
      { latein: "Romam", deutsch: "nach Rom", perfekt: "" }
    ];

    let points = parseInt(localStorage.getItem("points")) || 0;
    let index = 0;

    const wordEl = document.getElementById("word");
    const pointsEl = document.getElementById("points");
    const inputEl = document.getElementById("answer");
    const checkBtn = document.getElementById("checkBtn");
    const feedbackEl = document.getElementById("feedback");

    function showWord() {
      inputEl.value = "";
      feedbackEl.textContent = "";
      let vocab = vokabeln[index];
      wordEl.textContent = vocab.latein;
    }

    function normalize(str) {
      return str.toLowerCase().normalize("NFD").replace(/[\u0300-\u036f]/g, "").trim();
    }

    checkBtn.addEventListener("click", () => {
      let userAnswer = normalize(inputEl.value);
      let correctAnswer = normalize(vokabeln[index].deutsch);

      if (userAnswer === "") {
        feedbackEl.textContent = "Bitte gib eine Übersetzung ein!";
        return;
      }

      if (userAnswer === correctAnswer) {
        points++;
        pointsEl.textContent = `Punkte: ${points}`;
        localStorage.setItem("points", points);
        feedbackEl.textContent = "✅ Richtig!";
        index = (index + 1) % vokabeln.length;
        setTimeout(showWord, 1000);
      } else {
        feedbackEl.textContent = `❌ Falsch! Richtig ist: ${vokabeln[index].deutsch}`;
        // Optional: Punkte abziehen oder nicht
      }
    });

    // Start:
    pointsEl.textContent = `Punkte: ${points}`;
    showWord();
  </script>
</body>
</html>
