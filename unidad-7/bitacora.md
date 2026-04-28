# Unidad 7

## Bitácora de proceso de aprendizaje


## Bitácora de aplicación 
1. Palabra elegida: **VIENTO**

2. Justificación conceptual: La pieza parte de la palabra “VIENTO”, entendida como una fuerza invisible que se percibe por sus efectos. Por eso, las letras se comportan como cuerpos livianos afectados por corrientes de aire, ráfagas y turbulencias. La palabra no solo se lee, sino que se transforma en una experiencia visual, física y sonora.

3. Análisis de su significado visual y comportamental: Visualmente, la palabra se presenta con letras separadas, brillantes y flotantes, reforzando una sensación de ligereza. Comportamentalmente, cada letra funciona como un cuerpo físico independiente que puede desplazarse, rotar y perder estabilidad. Esto permite que la palabra pase del orden a la dispersión, representando el carácter cambiante e inestable del viento.

4. Moodboard o referencias:

5. Bocetos:

6. Mapa de decisiones: <img width="1448" height="1086" alt="Unid7_Mapa de dicisiones" src="https://github.com/user-attachments/assets/41c15b45-872d-493c-9047-6003d48863c9" />


7. Mapa de interpretación: <img width="1491" height="1055" alt="Unid7_Mapa de Interpretacion" src="https://github.com/user-attachments/assets/1f1fabf3-2b88-477d-bd11-48d8f27fcc4e" />


8. Explicación de la relación entre audio y comportamiento: El audio funciona como la fuerza que activa el viento dentro de la pieza. La amplitud del archivo sonoro controla la intensidad del movimiento: los sonidos suaves generan una brisa ligera, mientras que los picos de volumen producen ráfagas, expansión y mayor desorden en las letras. Así, el sonido no acompaña la imagen, sino que determina su comportamiento.

9. Evidencia del uso de IA: 

10. Código fuente:

                          // ----------------------------
                    // ALIAS SEGUROS DE MATTER.JS
                    // Evitan el error: Identifier 'Engine' has already been declared
                    // ----------------------------
                    
                    const MEngine = Matter.Engine;
                    const MWorld = Matter.World;
                    const MBodies = Matter.Bodies;
                    const MBody = Matter.Body;
                    const MConstraint = Matter.Constraint;
                    
                    // ----------------------------
                    // CONFIGURACIÓN PRINCIPAL
                    // ----------------------------
                    
                    const AUDIO_FILE = "Wind Heavy Sound Effect.mp3";
                    
                    let canvas;
                    
                    let engine;
                    let world;
                    
                    let letters = [];
                    let constraints = [];
                    let boundaries = [];
                    let streaks = [];
                    
                    let word = "VIENTO";
                    let fontSize;
                    
                    let song;
                    let ampAnalyzer;
                    
                    let audioReady = false;
                    let performanceStarted = false;
                    let performancePlaying = false;
                    
                    let startButton;
                    
                    let audioLevel = 0;
                    let smoothedLevel = 0;
                    let gust = 0;
                    
                    let windAngle = 0;
                    let burstEnergy = 0;
                    let lastPeakFrame = 0;
                    
                    // ----------------------------
                    // CARGA DEL AUDIO
                    // ----------------------------
                    
                    function preload() {
                      song = loadSound(
                        AUDIO_FILE,
                        function () {
                          audioReady = true;
                          console.log("Audio cargado correctamente.");
                        },
                        function (err) {
                          audioReady = false;
                          console.log("No se pudo cargar el audio. Revisa el nombre exacto del archivo.");
                          console.log(err);
                        }
                      );
                    }
                    
                    // ----------------------------
                    // SETUP
                    // ----------------------------
                    
                    function setup() {
                      canvas = createCanvas(windowWidth, windowHeight);
                      canvas.mousePressed(enterFullscreenOnly);
                    
                      textAlign(CENTER, CENTER);
                      rectMode(CENTER);
                    
                      engine = MEngine.create();
                      world = engine.world;
                      world.gravity.y = 0.015;
                    
                      fontSize = min(width, height) * 0.16;
                    
                      createAudioAnalyzer();
                      createBoundaries();
                      createWord();
                      createStreaks();
                      createStartButton();
                    }
                    
                    function createAudioAnalyzer() {
                      ampAnalyzer = new p5.Amplitude();
                      ampAnalyzer.smooth(0.85);
                    
                      if (song) {
                        ampAnalyzer.setInput(song);
                      }
                    }
                    
                    // ----------------------------
                    // DRAW
                    // ----------------------------
                    
                    function draw() {
                      // Fondo sólido.
                      // Esto elimina el rastro visual de letras y partículas.
                      background(6, 9, 14);
                    
                      MEngine.update(engine, 1000 / 60);
                    
                      updateAudioFromFile();
                      updateWind();
                    
                      if (performanceStarted) {
                        applyWindForces();
                      }
                    
                      drawAtmosphere();
                      drawLetters();
                    
                      burstEnergy *= 0.93;
                    }
                    
                    // ----------------------------
                    // PANTALLA COMPLETA SIN INICIAR
                    // ----------------------------
                    
                    function enterFullscreenOnly() {
                      // Cualquier clic sobre el canvas entra a pantalla completa,
                      // pero NO reproduce el audio ni inicia la pieza.
                      if (!fullscreen()) {
                        fullscreen(true);
                      }
                    }
                    
                    // ----------------------------
                    // BOTÓN INICIAL
                    // ----------------------------
                    
                    function createStartButton() {
                      startButton = createButton("INICIAR");
                      startButton.position(width / 2 - 60, height / 2 + fontSize * 0.9);
                      startButton.size(120, 42);
                    
                      startButton.style("background", "rgba(255, 255, 255, 0.08)");
                      startButton.style("color", "rgba(235, 245, 255, 0.9)");
                      startButton.style("border", "1px solid rgba(235, 245, 255, 0.35)");
                      startButton.style("border-radius", "24px");
                      startButton.style("font-size", "14px");
                      startButton.style("letter-spacing", "2px");
                      startButton.style("font-family", "sans-serif");
                      startButton.style("cursor", "pointer");
                    
                      startButton.mousePressed(startPerformance);
                    }
                    
                    function startPerformance() {
                      userStartAudio();
                    
                      if (!audioReady || !song) {
                        console.log("El audio no está listo o no se pudo cargar.");
                        return;
                      }
                    
                      if (!ampAnalyzer) {
                        createAudioAnalyzer();
                      }
                    
                      ampAnalyzer.setInput(song);
                    
                      performanceStarted = true;
                      performancePlaying = true;
                    
                      if (startButton) {
                        startButton.hide();
                      }
                    
                      if (song.isPlaying()) {
                        song.stop();
                      }
                    
                      song.play();
                    
                      song.onended(function () {
                        performancePlaying = false;
                      });
                    }
                    
                    // ----------------------------
                    // CREACIÓN DE LA PALABRA
                    // ----------------------------
                    
                    function createWord() {
                      letters = [];
                      constraints = [];
                    
                      textSize(fontSize);
                      textStyle(BOLD);
                    
                      let totalWidth = 0;
                      let spacing = fontSize * 0.18;
                    
                      for (let i = 0; i < word.length; i++) {
                        totalWidth += textWidth(word[i]) + spacing;
                      }
                    
                      let startX = width / 2 - totalWidth / 2;
                      let y = height / 2;
                    
                      for (let i = 0; i < word.length; i++) {
                        let char = word[i];
                        let w = textWidth(char) * 0.85;
                        let h = fontSize * 0.82;
                    
                        let x = startX + w / 2;
                    
                        let body = MBodies.rectangle(x, y, w, h, {
                          frictionAir: 0.035,
                          restitution: 0.8,
                          density: 0.00075,
                          chamfer: { radius: 8 }
                        });
                    
                        let home = { x, y };
                    
                        let tether = MConstraint.create({
                          pointA: home,
                          bodyB: body,
                          stiffness: 0.0012,
                          damping: 0.045
                        });
                    
                        MWorld.add(world, [body, tether]);
                    
                        letters.push({
                          char,
                          body,
                          home,
                          w,
                          h,
                          phase: random(TWO_PI),
                          seed: random(1000)
                        });
                    
                        constraints.push(tether);
                    
                        startX += w + spacing;
                      }
                    }
                    
                    // ----------------------------
                    // LÍMITES FÍSICOS
                    // ----------------------------
                    
                    function createBoundaries() {
                      boundaries = [];
                    
                      let thickness = 120;
                    
                      boundaries.push(
                        MBodies.rectangle(width / 2, height + thickness / 2, width + 400, thickness, {
                          isStatic: true
                        })
                      );
                    
                      boundaries.push(
                        MBodies.rectangle(width / 2, -thickness / 2, width + 400, thickness, {
                          isStatic: true
                        })
                      );
                    
                      boundaries.push(
                        MBodies.rectangle(-thickness / 2, height / 2, thickness, height + 400, {
                          isStatic: true
                        })
                      );
                    
                      boundaries.push(
                        MBodies.rectangle(width + thickness / 2, height / 2, thickness, height + 400, {
                          isStatic: true
                        })
                      );
                    
                      MWorld.add(world, boundaries);
                    }
                    
                    // ----------------------------
                    // PARTÍCULAS / LÍNEAS DE VIENTO
                    // ----------------------------
                    
                    function createStreaks() {
                      streaks = [];
                    
                      for (let i = 0; i < 120; i++) {
                        streaks.push({
                          x: random(width),
                          y: random(height),
                          len: random(25, 140),
                          speed: random(0.8, 4),
                          seed: random(1000)
                        });
                      }
                    }
                    
                    // ----------------------------
                    // AUDIO DESDE ARCHIVO
                    // ----------------------------
                    
                    function updateAudioFromFile() {
                      if (!performanceStarted || !song || !song.isPlaying() || !ampAnalyzer) {
                        audioLevel = 0;
                      } else {
                        audioLevel = ampAnalyzer.getLevel();
                      }
                    
                      if (!isFinite(audioLevel)) {
                        audioLevel = 0;
                      }
                    
                      smoothedLevel = lerp(smoothedLevel, audioLevel, 0.18);
                    
                      if (!isFinite(smoothedLevel)) {
                        smoothedLevel = 0;
                      }
                    
                      // Sensibilidad de reacción al audio.
                      // Si se mueve poco, baja el 0.13 a 0.09 o 0.07.
                      // Si se mueve demasiado, sube el 0.13 a 0.18 o 0.22.
                      gust = map(smoothedLevel, 0.005, 0.13, 0, 1);
                      gust = constrain(gust, 0, 1);
                    
                      if (!isFinite(gust)) {
                        gust = 0;
                      }
                    
                      // Picos fuertes del audio generan ráfagas automáticas.
                      if (gust > 0.58 && frameCount - lastPeakFrame > 18) {
                        triggerBurst(gust);
                        lastPeakFrame = frameCount;
                      }
                    }
                    
                    // ----------------------------
                    // DIRECCIÓN DEL VIENTO
                    // ----------------------------
                    
                    function updateWind() {
                      let targetAngle = map(mouseY, 0, height, -PI / 4, PI / 4);
                    
                      if (mouseIsPressed) {
                        targetAngle = atan2(mouseY - height / 2, mouseX - width / 2);
                      }
                    
                      windAngle = lerpAngle(windAngle, targetAngle, 0.06);
                    
                      if (!isFinite(windAngle)) {
                        windAngle = 0;
                      }
                    }
                    
                    // ----------------------------
                    // FUERZAS FÍSICAS
                    // ----------------------------
                    
                    function applyWindForces() {
                      for (let i = 0; i < letters.length; i++) {
                        let l = letters[i];
                        let b = l.body;
                    
                        constraints[i].stiffness = lerp(0.0012, 0.00005, gust);
                        b.frictionAir = lerp(0.035, 0.006, gust);
                    
                        let n = noise(l.seed, frameCount * 0.018);
                        let turbulence = map(n, 0, 1, -1, 1);
                    
                        let baseStrength = performancePlaying ? 0.00004 : 0;
                        let audioStrength = gust * 0.0018;
                        let burstStrength = burstEnergy * 0.0025;
                    
                        let forceMag = (baseStrength + audioStrength + burstStrength) * b.mass;
                    
                        if (!isFinite(forceMag)) {
                          forceMag = 0;
                        }
                    
                        let fx = cos(windAngle) * forceMag;
                        let fy = sin(windAngle) * forceMag;
                    
                        fy += turbulence * forceMag * 3.5;
                    
                        if (!isFinite(fx)) fx = 0;
                        if (!isFinite(fy)) fy = 0;
                    
                        MBody.applyForce(b, b.position, {
                          x: fx,
                          y: fy
                        });
                    
                        let offsetPoint = {
                          x: b.position.x + sin(l.phase + frameCount * 0.05) * l.w * 0.55,
                          y: b.position.y + cos(l.phase + frameCount * 0.04) * l.h * 0.55
                        };
                    
                        MBody.applyForce(b, offsetPoint, {
                          x: -fy * 1.4,
                          y: fx * 1.4
                        });
                    
                        let floatForce = sin(frameCount * 0.035 + l.phase) * 0.00006 * b.mass;
                    
                        if (!isFinite(floatForce)) {
                          floatForce = 0;
                        }
                    
                        MBody.applyForce(b, b.position, {
                          x: 0,
                          y: floatForce
                        });
                      }
                    }
                    
                    // ----------------------------
                    // ATMÓSFERA VISUAL
                    // ----------------------------
                    
                    function drawAtmosphere() {
                      let windSpeed = 1;
                    
                      if (performanceStarted) {
                        windSpeed = 2.5 + gust * 24 + burstEnergy * 28;
                      }
                    
                      if (!isFinite(windSpeed)) {
                        windSpeed = 1;
                      }
                    
                      let lineAlpha = performanceStarted
                        ? 28 + gust * 130 + burstEnergy * 150
                        : 18;
                    
                      lineAlpha = constrain(lineAlpha, 0, 255);
                    
                      stroke(220, 235, 255, lineAlpha);
                      strokeWeight(1.2);
                    
                      for (let s of streaks) {
                        let localTurbulence = map(noise(s.seed, frameCount * 0.02), 0, 1, -1, 1);
                    
                        s.x += cos(windAngle) * windSpeed * s.speed;
                        s.y += sin(windAngle) * windSpeed * s.speed;
                        s.y += localTurbulence * (0.5 + gust * 7);
                    
                        let dx = cos(windAngle) * s.len * (0.35 + gust * 1.3);
                        let dy = sin(windAngle) * s.len * (0.35 + gust * 1.3);
                    
                        line(s.x, s.y, s.x - dx, s.y - dy);
                    
                        if (
                          s.x > width + 150 ||
                          s.x < -150 ||
                          s.y > height + 150 ||
                          s.y < -150
                        ) {
                          resetStreak(s);
                        }
                      }
                    }
                    
                    function resetStreak(s) {
                      if (cos(windAngle) >= 0) {
                        s.x = random(-120, 0);
                      } else {
                        s.x = random(width, width + 120);
                      }
                    
                      s.y = random(height);
                      s.len = random(25, 140);
                      s.speed = random(0.8, 4);
                    }
                    
                    // ----------------------------
                    // DIBUJO DE LETRAS
                    // ----------------------------
                    
                    function drawLetters() {
                      noStroke();
                    
                      for (let i = 0; i < letters.length; i++) {
                        let l = letters[i];
                        let b = l.body;
                        let speed = b.speed;
                    
                        if (!isFinite(speed)) {
                          speed = 0;
                        }
                    
                        let glow = constrain(map(speed, 0, 22, 55, 210), 55, 210);
                    
                        push();
                        translate(b.position.x, b.position.y);
                        rotate(b.angle);
                    
                        textSize(fontSize);
                        textStyle(BOLD);
                    
                        // Aura fija de la letra.
                        // No deja rastro porque el fondo ya no es transparente.
                        fill(120, 180, 255, glow * 0.28);
                        text(l.char, 4, 6);
                    
                        // Letra principal.
                        fill(235, 245, 255, 235);
                        text(l.char, 0, 0);
                    
                        pop();
                      }
                    }
                    
                    // ----------------------------
                    // RÁFAGAS
                    // ----------------------------
                    
                    function triggerBurst(amount = 1) {
                      if (!isFinite(amount)) {
                        amount = 1;
                      }
                    
                      amount = constrain(amount, 0, 1.5);
                      burstEnergy = max(burstEnergy, amount);
                    
                      for (let l of letters) {
                        let b = l.body;
                    
                        let strength = 0.025 * amount * b.mass;
                    
                        if (!isFinite(strength)) {
                          strength = 0;
                        }
                    
                        MBody.applyForce(b, b.position, {
                          x: cos(windAngle) * strength,
                          y: sin(windAngle) * strength + random(-strength, strength) * 0.8
                        });
                    
                        MBody.setAngularVelocity(
                          b,
                          b.angularVelocity + random(-0.25, 0.25) * amount
                        );
                      }
                    }
                    
                    // ----------------------------
                    // INTERACCIÓN PERFORMATIVA
                    // ----------------------------
                    
                    function keyPressed() {
                      // Espacio: ráfaga manual, pero solo después de iniciar.
                      if (key === " ") {
                        if (performanceStarted) {
                          triggerBurst(1);
                        }
                      }
                    
                      // P: pausa / reproduce audio.
                      if (key === "p" || key === "P") {
                        togglePlayPause();
                      }
                    
                      // R: reiniciar pieza.
                      if (key === "r" || key === "R") {
                        resetPerformance();
                      }
                    
                      // F: salir o entrar manualmente a pantalla completa.
                      if (key === "f" || key === "F") {
                        let fs = fullscreen();
                        fullscreen(!fs);
                      }
                    }
                    
                    function togglePlayPause() {
                      if (!performanceStarted || !song) return;
                    
                      if (song.isPlaying()) {
                        song.pause();
                        performancePlaying = false;
                      } else {
                        song.play();
                        performancePlaying = true;
                      }
                    }
                    
                    function resetPerformance() {
                      resetWord();
                    
                      smoothedLevel = 0;
                      audioLevel = 0;
                      gust = 0;
                      burstEnergy = 0;
                      performancePlaying = false;
                      performanceStarted = false;
                    
                      if (song) {
                        song.stop();
                      }
                    
                      if (startButton) {
                        startButton.show();
                      }
                    }
                    
                    // ----------------------------
                    // RESET DE PALABRA
                    // ----------------------------
                    
                    function resetWord() {
                      for (let l of letters) {
                        MBody.setPosition(l.body, {
                          x: l.home.x + random(-10, 10),
                          y: l.home.y + random(-10, 10)
                        });
                    
                        MBody.setVelocity(l.body, { x: 0, y: 0 });
                        MBody.setAngularVelocity(l.body, 0);
                        MBody.setAngle(l.body, random(-0.05, 0.05));
                      }
                    }
                    
                    // ----------------------------
                    // RESPONSIVE
                    // ----------------------------
                    
                    function windowResized() {
                      resizeCanvas(windowWidth, windowHeight);
                    
                      MWorld.clear(world);
                      MEngine.clear(engine);
                    
                      engine = MEngine.create();
                      world = engine.world;
                      world.gravity.y = 0.015;
                    
                      fontSize = min(width, height) * 0.16;
                    
                      createBoundaries();
                      createWord();
                      createStreaks();
                    
                      if (startButton) {
                        startButton.position(width / 2 - 60, height / 2 + fontSize * 0.9);
                      }
                    
                      if (ampAnalyzer && song) {
                        ampAnalyzer.setInput(song);
                      }
                    }
                    
                    // ----------------------------
                    // UTILIDAD
                    // ----------------------------
                    
                    function lerpAngle(a, b, t) {
                      let diff = atan2(sin(b - a), cos(b - a));
                      return a + diff * t;
                    }

12. Enlace al sketch: https://editor.p5js.org/julinatera002/sketches/zYXZMfP3o

13. Capturas o registros de la pieza: <img width="652" height="656" alt="2026-04-28_17h26_30" src="https://github.com/user-attachments/assets/93d9192a-eed6-4485-a9ce-26c0406c04e6" />


## Bitácora de reflexión
