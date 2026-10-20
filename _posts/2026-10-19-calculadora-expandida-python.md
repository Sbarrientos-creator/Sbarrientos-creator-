---
layout: post
title: "Calculadora expandida en Python: raíz cuadrada y conversiones"
date: 2026-10-19
categories: code
author: CodeCapital Team
---

Esta es la versión mejorada de la calculadora sencilla. Además de sumar, restar, multiplicar, dividir y elevar a una potencia, ahora calcula raíces cuadradas y convierte temperaturas y distancias.

## El código

{% raw %}
```python
import math

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

def raiz_cuadrada(a):
    if a < 0:
        return "Error: No existe raíz cuadrada real para números negativos"
    return math.sqrt(a)

def celsius_a_fahrenheit(celsius):
    return (celsius * 9/5) + 32

def km_a_millas(km):
    return km * 0.621371

def calculadora():
    while True:
        print("\n=== CALCULADORA EXPANDIDA ===")
        print("1. Sumar")
        print("2. Restar")
        print("3. Multiplicar")
        print("4. Dividir")
        print("5. Potencia")
        print("6. Raíz cuadrada")
        print("7. Convertir Celsius a Fahrenheit")
        print("8. Convertir Kilómetros a Millas")
        print("9. Salir")
        
        opcion = input("Selecciona una opción (1-9): ")
        
        if opcion == '9':
            print("¡Gracias por usar la calculadora!")
            break
            
        # Operaciones con dos números
        if opcion in ('1', '2', '3', '4', '5'):
            try:
                num1 = float(input("Ingresa el primer número: "))
                num2 = float(input("Ingresa el segundo número (o exponente): "))
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

        # Operaciones con un solo número (raíz y conversiones)
        elif opcion in ('6', '7', '8'):
            try:
                valor = float(input("Ingresa el valor a calcular/convertir: "))
            except ValueError:
                print("Error: Por favor, ingresa un número válido.")
                continue

            if opcion == '6':
                print(f"Resultado: √{valor} = {raiz_cuadrada(valor)}")
            elif opcion == '7':
                print(f"Resultado: {valor} °C = {celsius_a_fahrenheit(valor)} °F")
            elif opcion == '8':
                print(f"Resultado: {valor} km = {km_a_millas(valor)} millas")
        else:
            print("Opción no válida. Intenta de nuevo.")

if __name__ == "__main__":
    calculadora()
```
{% endraw %}

## Explicación del código

### 1. Importación del módulo matemático

Para utilizar la función de raíz cuadrada, que no está incluida en los operadores básicos de Python, el primer paso es importar la librería estándar `math`:

```python
import math
```

Este módulo da acceso a la función `math.sqrt()`, que calcula la raíz cuadrada de un número flotante o entero.

### 2. Definición de funciones independientes

El código organiza cada operación matemática y conversión dentro de una función individual. Esto mantiene el código modular, fácil de leer y reutilizable.

**Operaciones aritméticas básicas y potencias**

- `sumar(a, b)`, `restar(a, b)`, `multiplicar(a, b)`: reciben dos parámetros y retornan el resultado mediante los operadores `+`, `-` y `*`.
- `dividir(a, b)`: incluye una validación condicional (`if b == 0:`) antes de realizar la operación. Si el divisor es cero, retorna un texto advirtiendo el error para evitar que el programa falle con una excepción `ZeroDivisionError`.
- `potencia(a, b)`: utiliza el operador `**` para elevar la base `a` al exponente `b`.

**Funciones agregadas (mejoras)**

- `raiz_cuadrada(a)`: valida que el número no sea negativo (`if a < 0:`). Dado que los números negativos no tienen raíz cuadrada en el conjunto de los números reales, devuelve un mensaje de error. Si el número es igual o mayor a cero, ejecuta `math.sqrt(a)`.
- `celsius_a_fahrenheit(celsius)`: aplica la fórmula de conversión `(C × 9/5) + 32`.
- `km_a_millas(km)`: multiplica el valor ingresado por la constante `0.621371` (equivalencia estándar de 1 kilómetro en millas).

### 3. Construcción del menú e interacción (`calculadora()`)

La función principal controla el flujo de la aplicación mediante un bucle infinito `while True`, lo que permite al usuario realizar múltiples cálculos sin que el programa se cierre tras cada operación.

1. **Impresión del menú:** muestra las 9 opciones disponibles en la consola.
2. **Captura de entrada:** la variable `opcion = input(...)` guarda la selección del usuario en formato de texto (`string`).
3. **Opción de salida:** si el usuario ingresa `'9'`, el programa imprime un mensaje de despedida y ejecuta la sentencia `break`, la cual interrumpe el bucle `while` y finaliza la ejecución.

### 4. Separación del flujo de datos según el tipo de operación

Una de las mejoras clave en la estructura es diferenciar las operaciones por la cantidad de entradas requeridas.

**Grupo 1: operaciones binarias (opciones 1 a 5)**

Requieren dos operandos. Si la opción ingresada está en `('1', '2', '3', '4', '5')`, el programa solicita `num1` y `num2`. Luego evalúa con estructuras `elif` la función correspondiente y muestra el resultado con cadenas formateadas (`f-strings`).

**Grupo 2: operaciones unarias (opciones 6 a 8)**

La raíz cuadrada y las conversiones de unidades solo necesitan un valor de entrada. Si la opción está en `('6', '7', '8')`, el programa solicita únicamente un dato (`valor`), evitando pedir una segunda variable innecesaria al usuario.

### 5. Manejo de excepciones (`try-except`)

Para evitar que el programa se detenga si el usuario ingresa letras o símbolos en lugar de números, cada captura de datos numéricos está envuelta en un bloque de control de errores:

```python
try:
    num1 = float(input("..."))
except ValueError:
    print("Error: Por favor, ingresa un número válido.")
    continue
```

- `float()`: intenta transformar el texto ingresado a un número decimal.
- `except ValueError`: si la conversión falla (por ejemplo, si el usuario escribe "hola"), captura el error, imprime una advertencia y ejecuta `continue` para saltar el resto del código y volver al inicio del menú.

### 6. Punto de entrada principal (`if __name__ == "__main__":`)

El bloque al final del archivo asegura el comportamiento estándar de los scripts en Python:

```python
if __name__ == "__main__":
    calculadora()
```

Esta condición verifica si el archivo se está ejecutando directamente. Si es así, llama a la función `calculadora()`. Si el archivo es importado como un módulo dentro de otro script, la calculadora no se iniciará automáticamente, permitiendo reutilizar sus funciones en otros programas.
