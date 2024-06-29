<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>¿Quieres ser mi novia?</title>
  <style>
    body {
      margin: 0;
      box-sizing: border-box;
      padding: 0;
      display: flex;
      width: 100vw;
      height: 100vh; /* Ajuste: corregido "100dvh" a "100vh" */
      justify-content: center;
      margin-top: -35px;
      flex-direction: column;
      align-items: center;
      background: #efd7ec;
    }
    h1 {
      text-align: center;
      font-family: Georgia, Times, "Times New Roman", serif;
      font-size: 28px;
    }
    .timer {
      position: absolute;
      top: 20px;
      right: 30px;
      font-size: 16px;
    }
    .container {
      width: 90%;
      display: flex;
      justify-content: center;
    }
    .section-1 {
      display: grid;
      grid-template-columns: 1fr 1fr;
      gap: 20px;
    }
    .box {
      height: 100px;
      position: relative;
      perspective: 1000px;
    }
    .face {
      position: absolute;
      width: 100%;
      height: 100%;
      backface-visibility: hidden;
      transition: transform 0.5s;
    }
    .front {
      transform: rotateY(0deg);
    }
    .back {
      transform: rotateY(180deg);
      background: #fff;
      display: flex;
      justify-content: center;
      align-items: center;
    }
    .encontrado .front,
    .encontrado .back {
      pointer-events: none;
      border: 3px solid rgb(216, 116, 206);
    }
    .desaparecer {
      display: none;
    }
  </style>
</head>
<body>
  <p id="timer" class="timer">Tiempo: 30s</p>
  <h1 id="title">Encuentra las cosas que <br />deberían ir juntas 👀</h1>
  <div class="container" id="container">
    <div class="section-1">
      <div class="col-1">
        <div class="box" id="box1" data-col="izq" data-pair="1">
          <div class="face front">
            <img src="https://i.pinimg.com/736x/7e/bf/7d/7ebf7d67340bdc0ed5adb3dc58fafd2b.jpg" alt="corazon" />
          </div>
          <div class="face back">
            <p class="back_texto">YO</p>
          </div>
        </div>
        <div class="box" id="box2" data-col="izq" data-pair="2">
          <div class="face front">
            <img src="https://i.pinimg.com/736x/7e/bf/7d/7ebf7d67340bdc0ed5adb3dc58fafd2b.jpg" alt="corazon" />
          </div>
          <div class="face back">
            <p class="back_texto">Café</p>
          </div>
        </div>
        <div class="box" id="box3" data-col="izq" data-pair="3">
          <div class="face front">
            <img src="https://i.pinimg.com/736x/7e/bf/7d/7ebf7d67340bdc0ed5adb3dc58fafd2b.jpg" alt="corazon" />
          </div>
          <div class="face back">
            <p class="back_texto">Cereal</p>
          </div>
        </div>
      </div>
      <div class="col-2">
        <div class="box" id="box4" data-col="der" data-pair="2">
          <div class="face front">
            <img src="https://i.pinimg.com/736x/7e/bf/7d/7ebf7d67340bdc0ed5adb3dc58fafd2b.jpg" alt="corazon" />
          </div>
          <div class="face back">
            <p class="back_texto">Pan</p>
          </div>
        </div>
        <div class="box" id="box5" data-col="der" data-pair="1">
          <div class="face front">
            <img src="https://i.pinimg.com/736x/7e/bf/7d/7ebf7d67340bdc0ed5adb3dc58fafd2b.jpg" alt="corazon" />
          </div>
          <div class="face back">
            <p class="back_texto">TÚ</p>
          </div>
        </div>
        <div class="box" id="box6" data-col="der" data-pair="3">
          <div class="face front">
            <img src="https://i.pinimg.com/736x/7e/bf/7d/7ebf7d67340bdc0ed5adb3dc58fafd2b.jpg" alt="corazon" />
          </div>
          <div class="face back">
            <p class="back_texto">Leche</p>
          </div>
        </div>
      </div>
    </div>
  </div>
  <script>
    const $ = (e) => document.getElementById(e);
    const target = (box, e) => box.querySelector("." + e);

    const $timer = $("timer");
    const $container = $("container");
    const $title = $("title");

    const $box1 = $("box1");
    const $front1 = target($box1, "front");
    const $back1 = target($box1, "back");

    const $box2 = $("box2");
    const $front2 = target($box2, "front");
    const $back2 = target($box2, "back");

    const $box3 = $("box3");
    const $front3 = target($box3, "front");
    const $back3 = target($box3, "back");

    const $box4 = $("box4");
    const $front4 = target($box4, "front");
    const $back4 = target($box4, "back");

    const $box5 = $("box5");
    const $front5 = target($box5, "front");
    const $back5 = target($box5, "back");

    const $box6 = $("box6");
    const $front6 = target($box6, "front");
    const $back6 = target($box6, "back");

    let termino = false;
    let time = 30; // Ajuste: tiempo inicial a 30 segundos

    let estado = {
      izq: true,
      der: false,
    };

    let volteadas = [];
    let paresEncontrados = 0;

    function manejarClick(box, front, back) {
      if ((estado.izq && box.dataset.col === "izq") || (estado.der && box.dataset.col === "der")) {
        if (!box.classList.contains("encontrado")) { // Solo voltear si no ha sido encontrado
          col(estado, front, back);
          volteadas.push({ box, front, back });

          if (volteadas.length === 2) {
            verificarPares();
          }
        }
      }
    }

    $box1.addEventListener("click", () => manejarClick($box1, $front1, $back1));
    $box2.addEventListener("click", () => manejarClick($box2, $front2, $back2));
    $box3.addEventListener("click", () => manejarClick($box3, $front3, $back3));
    $box4.addEventListener("click", () => manejarClick($box4, $front4, $back4));
    $box5.addEventListener("click", () => manejarClick($box5, $front5, $back5));
    $box6.addEventListener("click", () => manejarClick($box6, $front6, $back6));

    const intervalId = setInterval(() => {
      if (time > 0) {
        $timer.textContent = `Tiempo: ${time}s`; // Ajuste: corregido la interpolación de cadena
        time--;
      } else {
        endGame();
      }
    }, 1000);

    function endGame() {
      if (!termino) {
        termino = true;
        clearInterval(intervalId);
        $container.classList.add("desaparecer");
        $timer.classList.add("desaparecer");
        $title.innerHTML = `Oye te quería preguntar... ¿quieres volver hacer mi novia de nuevo mi amor Natally?
