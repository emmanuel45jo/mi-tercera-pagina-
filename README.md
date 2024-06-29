<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>¿Quieres ser mi novia?</title>
  </head>
  <style>
    body {
      margin: 0;
      box-sizing: border-box;
      padding: 0;
      display: flex;
      width: 100vw;
      height: 100dvh;
      justify-content: center;
      margin-top: -35px;
      flex-direction: column;
      align-items: center;
      background: #efd7ec;
      h1 {
        text-align: center;
        text-wrap: pretty;
        font-family: Georgia, Times, "Times New Roman", serif;
        font-size: 28px;
      }
    }
    .timer {
      position: absolute;
      top: 20px;
      right: 30px;
      font-size: 16px;
    }
    .container {
      width: 90%;
    }
    .section-1 {
      display: grid;
      grid-template-columns: 1fr 1fr;
    }
    .box {
      height: 100px;
      margin: 20px 10px;
      position: relative;
    }
    .face {
      position: absolute;
      backface-visibility: hidden;
      overflow: hidden;
      width: 100%;
      height: 100%;
      transition: 0.5s;
    }
    .front {
      transform: perspective(600px) rotateY(0deg);
      img {
        position: absolute;
        width: 100%;
        height: 100%;
        object-fit: cover;
      }
    }
    .back {
      background: #fff;
      display: flex;
      justify-content: center;
      align-items: center;
      transform: perspective(600px) rotateY(180deg);
      p {
        font-size: 20px;
      }
    }
    .voltear-front {
      transform: perspective(600px) rotateY(180deg);
    }
    .voltear-back {
      transform: perspective(600px) rotateY(360deg);
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
  <script type="module">
    const $ = (e) => document.getElementById(e);
const target = (box, e) => box.querySelector(.${e});

const $timer = $("timer");
const $container = $("container")
const $title = $("title")

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
let time = 15;

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
      volteadas.push({box, front, back});
      
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
    $timer.textContent = Tiempo: ${time}s;
    time--;
  } else {
    endGame();
  }
}, 1000);

function endGame() {
  if (!termino) {
    termino = true;
    clearInterval(intervalId);
    $container.classList.add("desaparecer")
    $timer.classList.add("desaparecer")
    $title.textContent = "Oye te quería preguntar... ¿ quieres ser mi novia denuevo mi amor Natally ? ❤️"
  }
}

function col(estado, front, back) {
  front.classList.add("voltear-front");
  back.classList.add("voltear-back");
  estado.izq = !estado.izq;
  estado.der = !estado.der;
}

function verificarPares() {
  const [carta1, carta2] = volteadas;

  // Comparar usando el atributo data-pair
  if (carta1.box.dataset.pair === carta2.box.dataset.pair) {
    paresEncontrados++;
    carta1.box.classList.add("encontrado");
    carta2.box.classList.add("encontrado");
    volteadas = [];
    if (paresEncontrados === 3) { // Asumiendo que hay 3 pares en total
      setTimeout(() => {
        endGame();
      }, 1000);
    }
  } else {
    setTimeout(() => {
      carta1.front.classList.remove("voltear-front");
      carta1.back.classList.remove("voltear-back");
      carta2.front.classList.remove("voltear-front");
      carta2.back.classList.remove("voltear-back");
      volteadas = [];
    }, 1000);
  }
}

// Asignar data attributes para diferenciar columnas (izq o der)
$box1.dataset.col = "izq";
$box2.dataset.col = "izq";
$box3.dataset.col = "izq";
$box4.dataset.col = "der";
$box5.dataset.col = "der";
$box6.dataset.col = "der";

// Asignar identificadores únicos a las cartas para formar pares usando data-pair
$box1.dataset.pair = "1";
$box1.querySelector('.back').textContent = "YO";
$box2.dataset.pair = "2";
$box2.querySelector('.back').textContent = "Cafe";
$box3.dataset.pair = "3";
$box3.querySelector('.back').textContent = "Cereal";
$box4.dataset.pair = "2";
$box4.querySelector('.back').textContent = "Pan";
$box5.dataset.pair = "1";
$box5.querySelector('.back').textContent = "TU";
$box6.dataset.pair = "3";
$box6.querySelector('.back').textContent = "Leche";

  </script>
  <body>
    <p id="timer" class="timer">Tiempo: 30s</p>
    <h1 id="title">Encuentra las cosas que <br />deberían ir juntas 👀</h1>
    <div class="container" id="container" >
      <div class="section-1">
        <div col-1>
          <div class="box" id="box1">
            <div class="face front">
              <img
                src="https://i.pinimg.com/736x/7e/bf/7d/7ebf7d67340bdc0ed5adb3dc58fafd2b.jpg"
                alt="corazon"
              />
            </div>
            <div class="face back">
              <p class="back_texto">Yo</p>
            </div>
          </div>
          <div class="box" id="box2">
            <div class="face front">
              <img
                src="https://i.pinimg.com/736x/7e/bf/7d/7ebf7d67340bdc0ed5adb3dc58fafd2b.jpg"
                alt="corazon"
              />
            </div>
            <div class="face back">
              <p class="back_texto">Café</p>
            </div>
          </div>
          <div class="box" id="box3">
            <div class="face front">
              <img
                src="https://i.pinimg.com/736x/7e/bf/7d/7ebf7d67340bdc0ed5adb3dc58fafd2b.jpg"
                alt="corazon"
              />
            </div>
            <div class="face back">
              <p class="back_texto">Cereal</p>
            </div>
          </div>
        </div>
        <div col-2>
          <div class="box" id="box4">
            <div class="face front">
              <img
                src="https://i.pinimg.com/736x/7e/bf/7d/7ebf7d67340bdc0ed5adb3dc58fafd2b.jpg"
                alt="corazon"
              />
            </div>
            <div class="face back">
              <p class="back_texto">Pan</p>
            </div>
          </div>
          <div class="box" id="box5">
            <div class="face front">
              <img
                src="https://i.pinimg.com/736x/7e/bf/7d/7ebf7d67340bdc0ed5adb3dc58fafd2b.jpg"
                alt="corazon"
              />
            </div>
            <div class="face back">
              <p class="back_texto">TÚ</p>
            </div>
          </div>
          <div class="box" id="box6">
            <div class="face front">
              <img
                src="https://i.pinimg.com/736x/7e/bf/7d/7ebf7d67340bdc0ed5adb3dc58fafd2b.jpg"
                alt="corazon"
              />
            </div>
            <div class="face back">
              <p class="back_texto">Leche</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </body>
</html>
