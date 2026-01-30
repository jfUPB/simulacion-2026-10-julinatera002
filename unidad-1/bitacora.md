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
- El sistema se mueve combinando los conceptos de random walk, Perlin noise y levy flight, 
    
        let walker;
        
        // Variable interactiva: tamaño del brush
        let brushSize = 10;
        
        function setup() {
          createCanvas(640, 400);
          background(255);
        
          walker = new HybridWalker();
        }
        
        function draw() {
          walker.update();
          walker.display();
        }
        
       
        // INTERACTIVIDAD CON TECLADO
     
        function keyPressed() {
    
      // Aumentar tamaño del brush
      if (key === '+') {
        brushSize += 5;
      }
    
      // Reducir tamaño del brush
      if (key === '-') {
        brushSize = max(5, brushSize - 5);
      }
    
      // Limpiar pantalla
       if (key === 'c' || key === 'C') {
        background(255);
      }
        }
        
       
        // WALKER HÍBRIDO (3 conceptos)
      
        class HybridWalker {
          constructor() {
            this.x = width / 2;
            this.y = height / 2;
    
        // Variables Perlin Noise
        this.tx = random(1000);
        this.ty = random(2000);
          }
    
      update() {

      
        // 1. RANDOM WALK (pasos pequeños)
      
        let stepX = random(-2, 2);
        let stepY = random(-2, 2);
    
     
        // 2. PERLIN NOISE (movimiento suave)
      
        let noiseX = map(noise(this.tx), 0, 1, -1.5, 1.5);
        let noiseY = map(noise(this.ty), 0, 1, -1.5, 1.5);
    
        this.tx += 0.01;
        this.ty += 0.01;
    
       
        // 3. LÉVY FLIGHT (saltos raros)
       
        let levyJump = 0;
    
        if (random(1) > 0.97) {
          levyJump = random(-80, 80);
        }
    
       
        // COMBINACIÓN FINAL
       
        this.x += stepX + noiseX + levyJump;
        this.y += stepY + noiseY + random(-levyJump, levyJump);
    
        this.x = constrain(this.x, 0, width);
        this.y = constrain(this.y, 0, height);
          }
    
      display() {
        noStroke();

        fill(0, 20);
    
        circle(this.x, this.y, brushSize);

      }
        }

<img width="962" height="597" alt="image" src="https://github.com/user-attachments/assets/181ada1f-862a-456a-90e6-3a6b2dc62f28" />

Enlace Sketch: https://editor.p5js.org/julinatera002/sketches/yNioJyuM_




## Bitácora de reflexión

1. Random genera números alcázares, es decir, puede generar un 2 seguido de un 8, esto permite que el movimiento sea más brusco, en cambio el perlin noise, genera un movimiento más suave.
   El noise lo usaría para generar un movimiento más natural y organico y el Random para un movimiento más aleatorio.

2. La distribucion uniforme tiene la misma probabilidad de generer los numeros aleatorios, es decir, en un rango de 1 a 10 la probabilidad de que salga el 5 y el 10 es la misma, en cambio,
   La distribucion normal aumenta la probabilidad en la media del rango, es decir, en un rango de 1 a 10, la probabilidad de que salga 5 es mcho mayor a que salga 10.

3. La aleatoriedad permite que cada pieza sea única, aunque se ejecute el codigo varias veces la obra será diferente y tambien permite resultados inesperados al combinar reglas estructuradas con elementos aleatorios.

4. El concepto de Levy Flight me permite generar saltos largos ocacionalmente, la alateoriedad permite que dicho salto se genere.

5. El walk describe el movimiento de un agente y su desplazo y el walk en levy flight se caracteriza por dar pasos pequeños pero ocacionalmente. 









