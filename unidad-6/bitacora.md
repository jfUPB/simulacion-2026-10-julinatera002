# Unidad 6

## Bitácora de proceso de aprendizaje


## Bitácora de aplicación 
PRESENTACIÓN: https://canva.link/0wxk2imnt9y7wgc

CONCEPTO DE LA OBRA:
La obra propone el mar como una metáfora de lo emocional: un sistema en constante movimiento donde todo emerge, se transforma y desaparece. Inspirada en Desde la orilla del álbum Mar adentro, la pieza interpreta el mar no como un paisaje, sino como un estado interno.
Visualmente, el mar se construye a partir de múltiples partículas que nacen desde lo profundo y ascienden, evocando procesos de cambio, memoria y flujo. Su acumulación genera una sensación de continuidad, mientras que su disolución en la parte superior sugiere lo efímero.
El sistema responde al sonido, haciendo visible la energía de la música: el mar “respira”, se intensifica o se calma, reflejando variaciones emocionales.
La aparición de peces introduce una dimensión simbólica: representan momentos de conciencia o intervención dentro de ese flujo, alterando temporalmente el sistema y reforzando la idea de un mar vivo, sensible y en constante transformación.

RELACIÓN ENTRE LO VISUAL Y LA CANCIÓN:
La visual traduce los elementos sonoros de Desde la orilla en comportamiento visual, generando una conexión directa entre lo auditivo y lo perceptivo.
La amplitud de la canción controla la intensidad del sistema: cuando el sonido crece, el mar se vuelve más denso, luminoso y dinámico; cuando disminuye, el movimiento se suaviza y el sistema respira con mayor calma. De esta forma, el mar no solo acompaña la música, sino que la encarna.
El flujo constante de partículas refleja la continuidad emocional presente en el álbum Mar adentro, mientras que su transformación y desaparición evocan la idea de procesos internos en cambio.
Además, la posibilidad de intervenir con la aparición de peces introduce una relación performativa: así como la canción sugiere una conexión íntima con lo emocional, la visual permite al usuario afectar ese “mar interno”, generando momentos de ruptura o énfasis dentro del flujo.

MOODBOARD:
<img width="1633" height="883" alt="2026-04-17_09h32_17" src="https://github.com/user-attachments/assets/7c40fe6c-f6c5-48d7-bf47-5dc43a033161" />

MAPA DE DECISIÓN:<img width="1536" height="1024" alt="Mapa de decisiones en capas" src="https://github.com/user-attachments/assets/4ee4e56d-8f54-4ddd-ba2b-12bba23449d7" />

MAPA DE INTERPRETACIÓN: <img width="1536" height="1024" alt="Mapa de interpretación en el agua" src="https://github.com/user-attachments/assets/5571c836-30b9-4664-932d-9fc9bee68a7b" />



JUSTIFICACIÓN DEL ALGORITMO:
Se eligió el uso de flow fields como base del sistema porque permiten generar movimientos orgánicos, continuos y coherentes, similares a las corrientes del mar. A diferencia de un movimiento aleatorio, este algoritmo produce trayectorias fluidas que refuerzan la sensación de un sistema natural y vivo.
El uso de partículas permite construir el mar como un fenómeno emergente: no es una forma fija, sino el resultado de múltiples elementos en interacción. Esto se alinea con el concepto de transformación constante presente en la obra.
Adicionalmente, se incorporan agentes autónomos (peces) con comportamientos diferenciados, lo que introduce una capa de variación y control performativo sin romper la lógica del sistema.
En conjunto, estos algoritmos permiten traducir la música en dinámicas visuales expresivas, donde el comportamiento del sistema responde tanto al sonido como a la intervención del usuario.

CODIGO FUENTE:

        let audio;
        let amplitude;
        let audioStarted = false;
        let isLoading = true;
        
        let roots = [];
        let fishes = [];
        
        let flowfield;
        let spacing = 20;
        let cols, rows;
        let firstFrame = false;
        
        let emitters = [];
        
        let palettes;
        let currentPaletteIndex = 0;
        
        let seaLayer; // 🌊 capa del mar
        
        function preload() {
          soundFormats("mp3", "ogg");
        }
        
        function setup() {
          createCanvas(1280, 720);
          frameRate(60);
          colorMode(HSB, 360, 100, 100, 255);
        
          seaLayer = createGraphics(width, height);
          seaLayer.colorMode(HSB, 360, 100, 100, 255);
          seaLayer.background(0);
        
          audio = loadSound("Desde La Orilla.mp3", soundLoaded);
        
          setupPalettes();
          amplitude = new p5.Amplitude();
          initializeFlowField();
          setupEmitters();
        }
        
        function setupEmitters() {
          emitters = [];
          let step = 25;
        
          for (let x = 0; x < width; x += step) {
            let y = height - 2;
            emitters.push(createVector(x, y));
          }
        }
        
        function soundLoaded() {
          isLoading = false;
        }
        
        function draw() {
        
          if (!audioStarted) {
            initialText();
            return;
          }
        
          // 🧼 limpiar SOLO peces
          background(0);
        
          // 🌊 dibujar mar acumulado
          image(seaLayer, 0, 0);
        
          let amp_ = amplitude.getLevel();
        
          let brightness = map(amp_, 0, 0.3, 20, 100);
          let speed_ = map(amp_, 0, 0.3, 0.5, 3);
        
          flowfield.update(amp_);
        
          // 🌊 EMISIÓN MAR
          for (let emitter of emitters) {
        
            let baseAmount = map(amp_, 0, 0.3, 1, 3);
        
            let densityFactor = map(roots.length, 0, 2000, 1, 0.2);
            densityFactor = constrain(densityFactor, 0.2, 1);
        
            let amount = floor(baseAmount * densityFactor);
        
            for (let i = 0; i < amount; i++) {
              let x = emitter.x + random(-2, 2);
              let y = emitter.y + random(-1, 1);
        
              roots.push(new Root(x, y, flowfield));
            }
          }
        
          if (roots.length > 2500) {
            roots.splice(0, 10);
          }
        
          if (fishes.length > 200) {
            fishes.splice(0, 5);
          }
        
          // 🌊 UPDATE MAR (dibujando en layer)
          for (let i = roots.length - 1; i >= 0; i--) {
            let r = roots[i];
            r.update(speed_);
            r.display(brightness);
        
            if (r.isDead()) {
              roots.splice(i, 1);
            }
          }
        
          // 🐟 UPDATE PECES (dibujan normal)
          for (let i = fishes.length - 1; i >= 0; i--) {
            let f = fishes[i];
            f.update();
            f.display();
        
            if (f.isDead()) {
              fishes.splice(i, 1);
            }
          }
        }
        
        function initialText() {
          background(0);
          textAlign(CENTER, CENTER);
          fill(255);
          textSize(32);
          text("Click to start", width / 2, height / 2);
        }
        
        function mousePressed() {
          if (!audioStarted && !isLoading) {
            audio.loop();
            audioStarted = true;
        
            fullscreen(true);
            resizeCanvas(windowWidth, windowHeight);
        
            // 🔥 recrear capa
            seaLayer = createGraphics(width, height);
            seaLayer.colorMode(HSB, 360, 100, 100, 255);
            seaLayer.background(0);
        
            setupEmitters();
            initializeFlowField();
        
            noCursor();
          }
        }
        
        function keyPressed() {
          if (key === "r") {
            initializeFlowField();
            setupEmitters();
        
            seaLayer.background(0); // limpiar mar
            roots = [];
          }
        
          if (key === "c") {
            currentPaletteIndex = (currentPaletteIndex + 1) % palettes.length;
          }
        
          // 🐟 UN pez por tecla
          if (key === "f" || key === "F") {
        
            let fishEmitters = [
              createVector(width * 0.3, height - 5),
              createVector(width * 0.7, height - 5)
            ];
        
            let emitter = random(fishEmitters);
        
            let x = emitter.x + random(-5, 5);
            let y = emitter.y + random(-2, 2);
        
            fishes.push(new Fish(x, y, flowfield));
          }
        }
        
        function windowResized() {
          resizeCanvas(windowWidth, windowHeight);
        
          seaLayer = createGraphics(width, height);
          seaLayer.colorMode(HSB, 360, 100, 100, 255);
          seaLayer.background(0);
        
          initializeFlowField();
          setupEmitters();
          roots = [];
        }
        
        // -------- FLOWFIELD --------
        function initializeFlowField() {
          cols = floor(width / spacing);
          rows = floor(height / spacing);
          flowfield = new FlowField(spacing);
        }
        
        class FlowField {
          constructor(spacing) {
            this.spacing = spacing;
            this.cols = floor(width / spacing);
            this.rows = floor(height / spacing);
            this.field = [];
            this.zoff = 0;
            this.init();
          }
        
          init() {
            noiseSeed(floor(random(10000)));
            let xoff = 0;
            for (let i = 0; i < this.cols; i++) {
              this.field[i] = [];
              let yoff = 0;
              for (let j = 0; j < this.rows; j++) {
                let angle = map(noise(xoff, yoff), 0, 1, 0, TWO_PI);
                this.field[i][j] = p5.Vector.fromAngle(angle);
                yoff += 0.1;
              }
              xoff += 0.1;
            }
          }
        
          update(amp_) {
            let zIncrement = map(amp_, 0, 0.3, 0.0005, 0.005);
            this.zoff += zIncrement;
        
            for (let x = 0; x < this.cols; x++) {
              for (let y = 0; y < this.rows; y++) {
                let angle = noise(x * 0.05, y * 0.05, this.zoff) * TWO_PI * 2;
                this.field[x][y] = p5.Vector.fromAngle(angle);
              }
            }
          }
        
          lookup(lookup) {
            let column = floor(constrain(lookup.x / this.spacing, 0, this.cols - 1));
            let row = floor(constrain(lookup.y / this.spacing, 0, this.rows - 1));
            return this.field[column][row].copy();
          }
        }
        
        // -------- ROOT (MAR) --------
        class Root {
          constructor(x, y, flowfield) {
            this.pos = createVector(x, y);
            this.vel = createVector(random(-0.3, 0.3), random(-2, -1));
            this.acc = createVector(0, 0);
            this.r = 10;
            this.flowfield = flowfield;
            this.age = 0;
        
            const currentPalette = palettes[currentPaletteIndex];
            this.baseColor = lerpColor(currentPalette.c1, currentPalette.c2, random(1));
          }
        
          update(speed_) {
            this.age++;
        
            if (this.age < 20) {
              this.vel.add(createVector(0, -0.8));
            } else {
              let force = this.flowfield.lookup(this.pos);
              this.acc.add(force);
              this.acc.add(createVector(0, -0.2));
              this.vel.add(this.acc);
            }
        
            this.vel.limit(speed_);
            this.pos.add(this.vel);
        
            this.acc.mult(0);
            this.r *= 0.995;
          }
        
          display(brightness) {
            let c = color(
              hue(this.baseColor),
              saturation(this.baseColor),
              brightness,
              200
            );
        
            seaLayer.fill(c);
            seaLayer.noStroke();
            seaLayer.ellipse(this.pos.x, this.pos.y, this.r * 2);
          }
        
          isDead() {
            return (
              this.r < 0.5 ||
              this.pos.y < -20 ||
              this.pos.x < -20 ||
              this.pos.x > width + 20
            );
          }
        }
        
        // -------- FISH --------
        class Fish {
          constructor(x, y, flowfield) {
            this.pos = createVector(x, y);
            this.vel = createVector(random(-1, 1), random(-1.5, -0.5));
            this.acc = createVector(0, 0);
            this.flowfield = flowfield;
            this.r = random(6, 10);
          }
        
          update() {
            let flow = this.flowfield.lookup(this.pos);
            flow.mult(0.3);
        
            let horizontal = createVector(random(-0.5, 0.5), 0);
            let centerPull = createVector(width / 2 - this.pos.x, 0).mult(0.0005);
        
            this.acc.add(flow);
            this.acc.add(horizontal);
            this.acc.add(centerPull);
        
            this.vel.add(this.acc);
            this.vel.limit(2.5);
            this.pos.add(this.vel);
        
            this.acc.mult(0);
            this.r *= 0.999;
          }
        
          display() {
            push();
            translate(this.pos.x, this.pos.y);
            rotate(this.vel.heading());
        
            fill(120, 80, 60, 200);
            noStroke();
        
            ellipse(0, 0, this.r * 2, this.r);
            triangle(-this.r, 0, -this.r * 2, this.r / 2, -this.r * 2, -this.r / 2);
        
            pop();
          }
        
          isDead() {
            return (
              this.r < 0.5 ||
              this.pos.y < -20 ||
              this.pos.x < -20 ||
              this.pos.x > width + 20
            );
          }
        }
        
        // -------- PALETAS --------
        function setupPalettes() {
          palettes = [
            {
              name: "Ocean",
              c1: color(180, 80, 90),
              c2: color(240, 70, 100),
            },
            {
              name: "Fire",
              c1: color(0, 90, 100),
              c2: color(50, 100, 100),
            },
            {
              name: "Nebula",
              c1: color(270, 90, 100),
              c2: color(330, 80, 100),
            }
          ];
        }

ENLACE: https://editor.p5js.org/julinatera002/sketches/irs5sN-_W

CAPTURA: <img width="1262" height="1164" alt="2026-04-17_09h36_55" src="https://github.com/user-attachments/assets/efe696dd-8a02-4761-870e-8fb2252c0898" />



## Bitácora de reflexión
