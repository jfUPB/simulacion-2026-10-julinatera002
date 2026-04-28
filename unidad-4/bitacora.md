# Unidad 4
https://juanferfranco.github.io/simulacion-2026-10/units/unit4/
## Bitácora de proceso de aprendizaje


## Bitácora de aplicación 
1. Partiendo del ejercicio de la curva, pensè en modificarlo para que la curva se mueva dentro del canvas y que los circulos cambien de tamaño, agregue unos ojos que se abren y cierran dependiendo de la posicion del maouse, si los ojos están cerrados la curva se mueve pero si se abren la curva se deja de mover.
2.       
                let angle = 0;
                let angleVelocity = 0.2;
                let amplitude = 80;
                let initAngle = 0;
                
                let tam = 25;
                
                // Motion 101
                let posicion;
                let velocidad;
                let aceleracion;
                
                function setup() {
                  createCanvas(640, 240);
                
                  posicion = createVector(width/2, height/2);
                  velocidad = createVector(2,1.5);
                  aceleracion = createVector(0,0);
                }
                
                function draw() {
                
                  background(255);
                
                  if(mouseX < 320){
                    levyFlightTam();
                    actualizarMovimiento();
                    initAngle += map(mouseX, 0, width, 0.1, 0, true);
                  }
                
                  curva();
                
                  if (mouseX >= 320) {
                    openEyes();
                  } else {
                    closedEyes();
                  }
                }
                
                function aplicarFuerza(f){
                  aceleracion.add(f);
                }
                
                function actualizarMovimiento(){
                
                  velocidad.add(aceleracion);
                
                  velocidad.limit(3);
                
                  posicion.add(velocidad);
                
                  aceleracion.mult(0);
                
                  // rebote en bordes
                  if(posicion.x > width || posicion.x < 0){
                    velocidad.x *= -1;
                  }
                
                  if(posicion.y > height || posicion.y < 0){
                    velocidad.y *= -1;
                  }
                }
                
                function levyFlightTam(){
                
                  let paso;
                
                  if(random() < 0.9){
                    paso = random(-1,1);
                  } else {
                    paso = random(-10,10);
                  }
                
                  tam += paso;
                
                  tam = constrain(tam,10,50);
                }
                
                function openEyes(){
                  fill(233);
                  ellipse(100, 120, 30, 50);
                  ellipse(50, 120, 30, 50);
                  
                  noStroke();
                  fill(39);
                  ellipse(109, 120, 10, 30);
                  ellipse(59, 120, 10, 30);
                }
                
                function closedEyes(){
                  fill(233);
                  ellipse(100, 120, 30, 50);
                  ellipse(50, 120, 30, 50);
                  
                  line(38, 130, 63, 130);
                  line(87, 130, 113, 130);
                }
                
                function curva(){
                
                  stroke(0);
                  strokeWeight(2);
                  fill(127);
                
                  angle = initAngle;
                
                  for (let x = 0; x <= 200; x += 25) {
                
                    let y = amplitude * sin(angle);
                
                    let posX = posicion.x + x;
                    let posY = posicion.y + y;
                
                    circle(posX, posY, tam);
                
                    angle += angleVelocity;
                  }
                }
3. https://editor.p5js.org/julinatera002/sketches/xqyAY0hpm
4. <img width="641" height="254" alt="2026-03-12_21h37_18" src="https://github.com/user-attachments/assets/1f1e7dbb-d309-4f46-806b-b05b7814195a" />


## Bitácora de reflexión

