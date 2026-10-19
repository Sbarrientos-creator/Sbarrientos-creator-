---
layout: post
title: "Cómo hacer una calculadora sencilla en Python"
date: 2026-10-18
categories: coding
---

Una calculadora de consola con suma, resta, multiplicación, división y potencia, hecha con funciones, un menú y validación de entradas.

## Guía de uso e instalación

1. **Requisito previo:** tener Python instalado en tu computadora (versión 3.x recomendada).
2. **Crear el archivo:** copia el código en un editor de texto (como VS Code, PyCharm o el Bloc de notas) y guárdalo con el nombre `calculadora.py`.
3. **Ejecutar el programa:**
   - Abre la terminal o consola de comandos.
   - Navega hasta la carpeta donde guardaste el archivo usando `cd ruta/de/la/carpeta`.
   - Ejecuta el comando: `python calculadora.py` (o `python3 calculadora.py` en macOS/Linux).

## El código

{% raw %}
```python
def sumar(a, b):
    return a + b

def restar(a, b):
    return a - b

def multiplicar(a, b):
    return a * b

def dividir(a, b):
    if b == 0:
        return "Error: No se puede dividir entre cero"
    return a / b

def potencia(a, b):
    return a ** b

def calculadora():
    while True:
        print("\n=== CALCULADORA ===")
        print("1. Sumar")
        print("2. Restar")
        print("3. Multiplicar")
        print("4. Dividir")
        print("5. Potencia")
        print("6. Salir")
        
        opcion = input("Selecciona una opción (1-6): ")
        
        if opcion == '6':
            print("¡Gracias por usar la calculadora!")
            break
            
        if opcion in ('1', '2', '3', '4', '5'):
            try:
                num1 = float(input("Ingresa el primer número: "))
                num2 = float(input("Ingresa el segundo número (o el exponente): "))
            except ValueError:
                print("Error: Por favor, ingresa un número válido.")
                continue

            if opcion == '1':
                print(f"Resultado: {num1} + {num2} = {sumar(num1, num2)}")
            elif opcion == '2':
                print(f"Resultado: {num1} - {num2} = {restar(num1, num2)}")
            elif opcion == '3':
                print(f"Resultado: {num1} * {num2} = {multiplicar(num1, num2)}")
            elif opcion == '4':
                print(f"Resultado: {num1} / {num2} = {dividir(num1, num2)}")
            elif opcion == '5':
                print(f"Resultado: {num1} ^ {num2} = {potencia(num1, num2)}")
        else:
            print("Opción no válida. Intenta de nuevo.")

if __name__ == "__main__":
    calculadora()
```
{% endraw %}

## Explicación de cada parte del código

### 1. Definición de funciones matemáticas

Al inicio del programa se declaran bloques independientes para cada operación básica utilizando la palabra clave `def`:

- `sumar(a, b)`, `restar(a, b)`, `multiplicar(a, b)`: reciben dos variables y devuelven el resultado con sus operadores correspondientes (`+`, `-`, `*`).
- `dividir(a, b)`: incluye una validación condicional `if b == 0`. Evita que el programa falle por un error matemático al intentar dividir entre cero.
- `potencia(a, b)`: utiliza el operador `**` de Python para elevar el primer número (a) a la potencia del segundo número (b).

### 2. Bucle principal y menú

La función `calculadora()` controla el flujo continuo de la aplicación:

- `while True:` crea un ciclo infinito para que la calculadora siga pidiendo operaciones hasta que el usuario decida salir explícitamente.
- `input()` muestra el menú de opciones en consola y captura la elección realizada por el usuario en texto.

### 3. Control de salida y validación de entradas

- **Opción de salida** (`opcion == '6'`): rompe el bucle con la instrucción `break`, finalizando la ejecución del programa.
- **Estructura `try - except ValueError`**: intenta convertir las entradas del usuario a números de punto flotante (`float`). Si el usuario escribe letras o símbolos no numéricos, captura el error para no interrumpir el programa abruptamente y le pide intentar otra vez mediante `continue`.

### 4. Evaluación de operaciones y ejecución

Una vez validados los datos, la condición `if / elif` evalúa qué opción eligió el usuario, invoca a la función matemática correspondiente enviando los dos números ingresados y muestra el resultado en pantalla mediante f-strings (`f"Resultado: ..."`).

### 5. Punto de entrada

El bloque final `if __name__ == "__main__":` garantiza que la función `calculadora()` solo se ejecute automáticamente si el archivo se ejecuta de forma directa desde la terminal.
