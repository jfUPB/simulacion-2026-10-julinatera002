# Unidad 2

## Bitácora de proceso de aprendizaje
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
            }
            
            function draw() {
                background(200);
            
                let v0 = createVector(50, 50);
                let v1 = createVector(400, 0);
                let v2 = createVector(0, 400);
                let v3 = p5.Vector.lerp(v1, v2, interP);
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
   

## Bitácora de aplicación 



## Bitácora de reflexión



