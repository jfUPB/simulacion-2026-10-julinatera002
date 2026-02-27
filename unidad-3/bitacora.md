# Unidad 3
https://juanferfranco.github.io/simulacion-2026-10/units/unit3/

## Bitácora de proceso de aprendizaje


## Bitácora de aplicación 
1. Juanito está entrenando en una pista pero empieza a llover y aparecen charcos de agua en la pista que juanito debe evitar. El carro se mueve a diferentes objetivos de la pista lo que hace que el carro se mueva mediante fuerzas sobre la pista, la hacer click sobre la pista se coloca un charco, de este se deriva un vector que tiene como origen el centro del charco y direccion hacia el carro, cuando el carro está a cierta distancia del vector del charco lo evita.

2. 

        let posicion;
        let velocidad;
        let aceleracion;
        
        let objetivos = [];
        let indiceObjetivo = 0;
        
        let charcosLista = [];
        
        function setup() {
          createCanvas(800, 800);
        
          posicion = createVector(180, 470);
          velocidad = createVector(0, -2);
          aceleracion = createVector(0, 0);
        
          objetivos.push(createVector(180,150));
          objetivos.push(createVector(580,150));
          objetivos.push(createVector(580,610));
          objetivos.push(createVector(180,610));
        }
        
        function draw() {
          background(30, 120, 40);
        
          pista();
          meta();
          mostrarCharcos();
        
          seguirRuta();
          evitarCharcos();
          actualizarFisica();
        
         
          luces();
        
          arbusto(50, 100);
          arbusto(450, 225);
          arbusto(700, 300);
          arbusto(80, 480);
          arbusto(350, 580);
          arbusto(300, 350);
          arbusto(500, 450);
          
          carro(posicion.x, posicion.y);
        }
        
        
        function aplicarFuerza(f) {
          aceleracion.add(f);
        }
        
        function actualizarFisica() {
          velocidad.add(aceleracion);
          velocidad.limit(4);
          posicion.add(velocidad);
          aceleracion.mult(0);
        }
        
        
        function seguirRuta(){
        
          let objetivo = objetivos[indiceObjetivo];
        
          let deseado = p5.Vector.sub(objetivo, posicion);
          let distancia = deseado.mag();
        
          deseado.normalize();
          deseado.mult(3);
        
          let steering = p5.Vector.sub(deseado, velocidad);
          steering.limit(0.2);
        
          aplicarFuerza(steering);
        
          if (distancia < 20) {
            indiceObjetivo++;
            if (indiceObjetivo >= objetivos.length) {
              indiceObjetivo = 0;
            }
          }
        }
        
        
        function evitarCharcos() {
        
          for (let c of charcosLista) {
        
            let centro = createVector(c.x, c.y);
            let escape = p5.Vector.sub(posicion, centro);
            let distancia = escape.mag();
        
            if (distancia < 50) {
        
              escape.normalize();
              escape.mult(0.5);
        
              aplicarFuerza(escape);
            }
          }
        }
        
        
        function mousePressed() {
          charcosLista.push({x: mouseX, y: mouseY});
        }
        
        
        
        function keyPressed() {
          if (key === 'b' || key === 'B') {
            charcosLista = [];   // Vacía todos los charcos
          }
        }
        
        
        function mostrarCharcos(){
          for (let c of charcosLista) {
            push();
            fill('#03A9F4');
            noStroke();
            circle(c.x, c.y, 40);
            pop();
          }
        }
        
        
        function carro(x, y){
        
          push();
        
          translate(x + 19, y + 35);
        
          let angulo = velocidad.heading();
          rotate(angulo);
        
          translate(-19, -35);
        
          noStroke();
          fill('red');
          rect(0, 0, 70, 38, 12);
        
          stroke('rgb(116,19,19)');
          strokeWeight(2);
          fill('#C23527');
          rect(40, 8, 28, 20, 10);
        
          noStroke();
          fill('#131313');
          rect(38, 4, 15, 28, 2, 15,15, 2);
        
          noStroke();
          fill('#131313');
          rect(10, 5, 10, 28, 15, 2, 2, 15);
        
          pop();
        }
        
        function pista() {
          push();
          translate(width / 2, height / 2);
        
          fill(60);
          noStroke();
          rect(-250, -300, 500, 600, 100);
        
          fill(30, 120, 40);
          rect(-150, -200, 300, 400, 80);
        
          pop();
        }
        
        
        function meta() {
          push();
          translate(width / 2 - 260, height / 2);
        
          for (let i = 0; i < 8; i++) {
            for (let j = 0; j < 4; j++) {
              if ((i + j) % 2 === 0) fill(255);
              else fill(0);
        
              noStroke();
              rect(i * 15, j * 15, 15, 15);
            }
          }
        
          pop();
        }
        
        
        function luces (){
          stroke(255, 200);
          strokeWeight(3);
        
          for (let i = 25; i < width; i += 50) {
            point(i, 10);
            point(i, height - 10);
          }
        }
        
        
        function arbusto(x, y) {
          push();
          translate(x, y);
          fill('#2B5E2D');
          stroke('rgb(1,69,1)');
          circle(0, 0, 20);
          circle(10, 5, 20);
          circle(20, 5, 20);
          circle(15, -5, 20);
          circle(30, 0, 20);
          pop();
        }
3. https://editor.p5js.org/julinatera002/sketches/Bia5J1Zwi
4. <img width="799" height="795" alt="image" src="https://github.com/user-attachments/assets/f7a51e1e-deae-438f-b35b-5b0dcb499cd7" />

## Bitácora de reflexión
1. Motion 101 es una forma de pensar el movimiento como proceso dinámico acumulativo, o sea que no es como un orden que debe seguir el movimiento si no unas reglas que debe seguir para moverse.
   Por ejemplo una hoja cayendo, el viento es una fuerza externa que cambia la direccion (dependiendo de la direccion en que esté soplando el viento) 


