# Unidad 1

## Bitácora de proceso de aprendizaje
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
-

Actividad #4

       function setup() {
         createCanvas(640, 240);
         background(255);
       }

       function draw() {
         //{!1} A normal distribution with mean 320 and standard deviation 60
         let y = randomGaussian(120, 20);
         noStroke();
         fill(0, 10);
         circle(320,y , 16);
       }
<img width="642" height="227" alt="image" src="https://github.com/user-attachments/assets/c66ecc9d-1b93-4163-af15-f9428c36da4a" />

Cambie La variable fija X a Y para que la imagen se desplace sobre el eje Y y así generar una 


## Bitácora de aplicación 



## Bitácora de reflexión
