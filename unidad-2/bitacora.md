# Unidad 2
https://juanferfranco.github.io/simulacion-2026-10/units/unit2/

## Bitácora de proceso de aprendizaje

Actividad #2

1. Se suma componente a componente.
2. Posrque position y velocity no son numeros son objetos que contienen al vector, la linea no representa una suma matematica, para eso se deberia usar:
   
        position.add(velocity);

Actividad #3

Cambie las variables de X y Y por un vector posición y el Perlin Noise genera valores que se asignan a las componentes del vector.

        let walker;
        
        function setup() {
          createCanvas(640, 240);
          walker = new Walker();
          background(255);
        }
        
        function draw() {
          walker.step();
          walker.show();
        }
        
        class Walker {
          constructor() {
            // Vector de posición
            this.position = createVector(width / 2, height / 2);
        
            // Variables de Perlin Noise (tiempo)
            this.tx = 0;
            this.ty = 10000;
          }
        
          step() {
            // Generamos posiciones usando Perlin Noise
            let x = map(noise(this.tx), 0, 1, 0, width);
            let y = map(noise(this.ty), 0, 1, 0, height);
        
            // Actualizamos el vector de posición
            this.position.set(x, y);
        
            // Avanzamos en el "tiempo" del noise
            this.tx += 0.01;
            this.ty += 0.01;
          }
        
          show() {
            strokeWeight(2);
            fill(127);
            stroke(0);
            circle(this.position.x, this.position.y, 48);
          }
        }

Actividad #4
  1. Que se imprima en la consola los valores del vector.
  2. Se imprimen en la consola los valores del vector y luego los valores del vector modificado.
  4. Paso por referencia.

Actividad #5
  1. Para encontrar la magnitud del vector, la diferencia es que mag() encuentra la magnitud y magSq() Coje cada componente lo y lo suma pero no le saca raiz, sirve para comparar si un vector es más grande que otro.
  2. Normalize()

Actividad #6
                      
          let colorRed;
          let interP = 0;
          let dir = 1;
                
          function setup() {
              createCanvas(500, 500);
              colorRed = color('red');
              colorGreen = color('green');
          }
          
          function draw() {
              background(200);
          
              let v0 = createVector(50, 50);
              let v1 = createVector(400, 0);
              let v2 = createVector(0, 400);
              let v3 = p5.Vector.lerp(v1, v2, interP);
              let v4 = createVector(450,50);
              let v5 = createVector(-400,400);
              drawArrow(v4,v5,colorGreen);
              drawArrow(v0, v1, colorRed);
              drawArrow(v0, v2, 'blue');
              drawArrow(v0, v3, lerpColor(colorRed, color('blue'), interP) );
            
              interP = interP + dir*0.01;
              if(interP >= 1) dir = -1;
              else if(interP <= 0) dir = 1;
          }
          
          function drawArrow(base, vec, myColor) {
              push();
              stroke(myColor);
              strokeWeight(3);
              fill(myColor);
              translate(base.x, base.y);
              line(0, 0, vec.x, vec.y);
              rotate(vec.heading());
              let arrowSize = 7;
              translate(vec.mag() - arrowSize, 0);
              triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
              pop();
          }

El lerp() se mueve suavemente entre dos parametros y el lerpcolor() se mueve suavemente entre dos colores.

La función drawArrow() dibuja una flecha trasladando el sistema de coordenadas al punto base del vector.


Actuvidad #7
1. Motion 101 utiliza vectores de posición, velocidad y aceleración. En cada frame, la velocidad se actualiza.
2. En el update() la velocidad se actualiza a partir de la aceleracion y la posicion se actualiza a partir de la velocidad.
   
             update() {
            this.velocity.add(this.acceleration);
            this.position.add(this.velocity);
          }



## Bitácora de aplicación 

- la aceleración depende de la dirección hacia el mouse y controlar la intensidad de la fuerza que atrae al objeto.

            let position;
            let velocity;
            let acceleration;
            
            let accStrength = 0.05;
            let r = 10;
            
            function setup() {
              createCanvas(600, 400);
              background(255);
            
              position = createVector(random(width), random(height));
              velocity = p5.Vector.random2D();
              acceleration = createVector(0, 0);
            }
            
            function draw() {
              
            
              // Dirección hacia el mouse
              let mouse = createVector(mouseX, mouseY);
              let dir = p5.Vector.sub(mouse, position);
              dir.normalize();
              dir.mult(accStrength);
            
              acceleration = dir;
            
              
              velocity.add(acceleration);
              velocity.limit(4);
              position.add(velocity);
            
              
              wrapAround();
            
              
              noStroke();
              fill(0, 25);
              circle(position.x, position.y, r * 2);
            }
            
            
            
            function keyPressed() {
              if (key === '+') {
                accStrength += 0.05;
              } 
              else if (key === '-') {
                accStrength = max(0.05, accStrength - 0.05);
              } 
              else if (key === 'c' || key === 'C') {
                background(255);
              }
            }
            
            function wrapAround() {
              if (position.x > width) position.x = 0;
              if (position.x < 0) position.x = width;
              if (position.y > height) position.y = 0;
              if (position.y < 0) position.y = height;
            }

https://editor.p5js.org/julinatera002/sketches/px0ow-I8B

<img width="895" height="587" alt="image" src="https://github.com/user-attachments/assets/fc150fea-396c-46d5-8ea5-fa7a007dd8d2" />


## Bitácora de reflexión








