/**
 * EJERCICIO PRÃCTICO â€” SEMANA 2
 * ProgramaciÃ³n Avanzada I (PCO-106)
 *
 * Este cÃ³digo funciona, pero estÃ¡ escrito en JavaScript plano y repite
 * lÃ³gica entre los dos tipos de vehÃ­culo (Carro y Moto).
 *
 * TU TAREA: refactorizar este mÃ³dulo hacia clases TypeScript tipadas.
 *
 * Pasos sugeridos:
 *   1. Identifica quÃ© propiedades y comportamiento se repiten entre
 *      carro y moto (son casi el mismo objeto con un par de campos distintos).
 *   2. ConviÃ©rtelos en clases con constructor y propiedades tipadas.
 *   3. Aplica modificadores de acceso donde corresponda
 *      (por ejemplo: precio y vendido no deberÃ­an modificarse libremente
 *      desde fuera de la clase).
 *   4. Como hay comportamiento compartido, crea una clase base (Vehiculo)
 *      y usa herencia (extends) para Carro y Moto.
 *
 * No es necesario conservar los nombres de las funciones tal cual â€”
 * el objetivo es el diseÃ±o de clases, no la sintaxis exacta.
 */

/**
 * EJERCICIO PRÁCTICO — SEMANA 2
 * Programación Avanzada I (PCO-106)
 */

// 1 y 4. Clase base con herencia
class Vehiculo {
  // Propiedades privadas para evitar que se modifiquen libremente desde fuera
  #precio;
  #vendido;

  constructor(marca, modelo, anio, precio) {
    this.marca = marca;
    this.modelo = modelo;
    this.anio = anio;
    this.#precio = precio;
    this.#vendido = false;
  }

  // Getters para lectura segura del precio y estado de venta
  get precio() {
    return this.#precio;
  }

  get vendido() {
    return this.#vendido;
  }

  // Comportamiento compartido
  aplicarDescuento(porcentaje) {
    if (porcentaje < 0 || porcentaje > 50) {
      throw new Error("Descuento inválido");
    }
    this.#precio -= (this.#precio * porcentaje) / 100;
  }

  marcarVendido() {
    this.#vendido = true;
  }

  describir() {
    return `${this.marca} ${this.modelo} (${this.anio})`;
  }
}

// 2 y 4. Subclase Carro
class Carro extends Vehiculo {
  constructor(marca, modelo, anio, puertas, precio) {
    super(marca, modelo, anio, precio);
    this.puertas = puertas;
  }

  describir() {
    return `${this.marca} ${this.modelo} (${this.anio}) - ${this.puertas} puertas`;
  }
}

// 2 y 4. Subclase Moto
class Moto extends Vehiculo {
  constructor(marca, modelo, anio, cilindrada, precio) {
    super(marca, modelo, anio, precio);
    this.cilindrada = cilindrada;
  }

  describir() {
    return `${this.marca} ${this.modelo} (${this.anio}) - ${this.cilindrada}cc`;
  }
}

// ------------------- Uso del módulo refactorizado -------------------

const carro1 = new Carro("Toyota", "Corolla", 2023, 4, 850000);
const moto1 = new Moto("Honda", "CBR500", 2022, 500, 420000);

console.log(carro1.describir());
console.log(moto1.describir());

carro1.aplicarDescuento(10);
moto1.marcarVendido();

console.log(carro1.precio);  // Muestra 765000 (usando el getter)
console.log(moto1.vendido); // Muestra true (usando el getter)

