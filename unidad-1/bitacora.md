# Unidad 1

## Bitácora de proceso de aprendizaje
Actividad #1
introduce variación y sorpresa en las obras producidas por algoritmos. Permite que un mismo sistema genere resultados únicos o incluso inesperados.

Actividad #2
Tenemos un codigo el cual debemos modificar la funcion step y tratar de predecir que sucederá
En clase planteamos el siguiente cambio:

step() {
    const choice = floor(random(4));
    if (choice == 0) {
      this.x++;
    } else if (choice == 0) {
      this.x--;
    } else if (choice == 0) {
      this.y++;
    } else {
      this.y--;
    }

Y tuvimos dos hipotesis, la primera es que la figura no se iba a mover y la segunda que la figura tendería hacia arriba a la derecha, esta última fue la hipotesis correcta.

Actividad #3
- Una distribución uniforme es cuando la aleatoriedad de los numeros tienen la misma probabilidad a diferencia de una no uniforme en donde puede que uno o más numeros tengan más probabilidad.

          let x, y;
  
       function setup() {
         createCanvas(400, 400);
         background(220);

         x = width / 2;
         y = height / 2;
       }

       function draw() {
         stroke(0);
         point(x, y);

         let step = random(1);

         // Distribución no uniforme: más probabilidad hacia la derecha
         if (step < 0.5) {
           x += 1; // 50% derecha
         } 
         else if (step < 0.7) {
           x -= 1; // 20% izquierda
         } 
         else if (step < 0.85) {
           y += 1; // 15% abajo
         } 
         else {
           y -= 1; // 15% arriba
         }
       }


Actividad #4

       let x;
       function setup() {
         createCanvas(640, 240);
         background(255);
       }

       function draw() {
         // Distribución uniforme entre 0 y 640
         let x = random(40, 600);
         
        noStroke();
        fill(0, 10);
        circle(x, 120, 16);
    }
<img width="949" height="334" alt="image" src="https://github.com/user-attachments/assets/0c502a49-e9b9-448f-896a-3223357122af" />


Cambié el randomGaussian() por un random() lo que permitirá una distribucion uniforme entre 40 y 600.

Enlace a Sketch: https://editor.p5js.org/julinatera002/sketches/v88tuFsUr

Actividad #5

    let x;

    function setup() {
      createCanvas(640, 240);
      background(255);

      // Empezamos en el centro
      x = width / 2;
    }

    function draw() {

      // Dibujar círculo en la posición actual
      noStroke();
      fill(0, 10);
      circle(x, 120, 16);

      // ----------------------------
      // Lévy flight: pasos pequeños + saltos raros
      // ----------------------------

      // Genera un número aleatorio entre 0 y 1
      let r = random(1);

    // Tamaño del paso (cola pesada)
      let stepSize;

      if (r < 0.9) {
    // 90% del tiempo: pasos pequeños
    stepSize = random(-5, 5);
      } else {
    // 10% del tiempo: salto largo
    stepSize = random(-80, 80);
      }

      // Actualizar posición
      x += stepSize;

     // Evitar que se salga del canvas
     x = constrain(x, 0, width);
    }

La funcion random() escoje un numero alazar entre 0 y 1, si el numero es menor a 0.9 entonces se desplazara entre -5 y 5 pasos pero si el numero es mayor o igual a 0.9 entonces se desplazara entre -80 y 80 pasos


<img width="951" height="349" alt="image" src="https://github.com/user-attachments/assets/4206e0d7-2d54-4de2-95c2-767ddbcd93a4" />


Enlace a Sketch: https://editor.p5js.org/julinatera002/sketches/lGaPRXHy6

Actividad #6

    let t = 0;
    let x = 0;

    function setup() {
    createCanvas(360, 240);
     background(255);
    }

    function draw() {
      let y = noise(t) * height;

      fill(0, 20);
      noStroke();
      circle(x, y, 10);

      x += 2;
      t += 0.01;

      if (x > width) {
        x = 0;
        background(255);
      }
    }


Use un noise para mover un walker que genere diferentes patrones cada vez que se genere


<img width="541" height="350" alt="image" src="https://github.com/user-attachments/assets/d1d344d9-a359-4867-974a-3aecb4992c8d" />

Enlace Sketch: https://editor.p5js.org/julinatera002/sketches/Wd-AEXEBA


## Bitácora de aplicación 
Conjunto de reglas que producen multiples salidas


## Bitácora de reflexión







