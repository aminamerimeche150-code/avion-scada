<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8">
  <title>Inclinaison Avion - ESP32</title>
  <meta name="viewport" content="width=device-width, initial-scale=1">

  <style>
    body {
      font-family: Arial, sans-serif;
      background: #f4f6f8;
      padding: 20px;
    }

    .box {
      max-width: 400px;
      margin: auto;
      background: white;
      padding: 20px;
      border-radius: 10px;
      box-shadow: 0 4px 10px rgba(0,0,0,0.1);
    }

    h2 {
      text-align: center;
    }

    .value {
      font-size: 22px;
      margin: 10px 0;
    }

    .status {
      margin-top: 15px;
      padding: 10px;
      text-align: center;
      font-weight: bold;
      border-radius: 8px;
    }

    .NORMAL  { background: #d4f8d4; color: #146b14; }
    .WARNING { background: #fff0c2; color: #8a5a00; }
    .DANGER  { background: #ffd6d6; color: #8a0000; }
  </style>
</head>

<body>
  <div class="box">
    <h2>Inclinaison Avion</h2>

    <div class="value">X (Roll) : <span id="x">--</span> °</div>
    <div class="value">Y (Pitch) : <span id="y">--</span> °</div>
    <div class="value">Z (Yaw) : <span id="z">--</span> °</div>

    <div id="etat" class="status NORMAL">---</div>
  </div>

  <script>
    async function updateData() {
      try {
        const response = await fetch("/angles");
        const data = await response.json();

        document.getElementById("x").innerText = data.roll.toFixed(2);
        document.getElementById("y").innerText = data.pitch.toFixed(2);
        document.getElementById("z").innerText = data.yaw.toFixed(2);

        const etatDiv = document.getElementById("etat");
        etatDiv.innerText = data.state;
        etatDiv.className = "status " + data.state;

      } catch (error) {
        console.log("Erreur de connexion à l’ESP32");
      }
    }

    setInterval(updateData, 200); // mise à jour chaque 200 ms
    updateData();
  </script>
</body>
</html>
