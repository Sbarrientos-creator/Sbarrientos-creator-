---
layout: post
title: "Cómo hacer un Tamagotchi en Python"
date: 2026-10-18
categories: coding
---

Este programa implementa una mascota virtual interactiva basada en consola (estilo Tamagotchi) escrita en Python, donde el usuario puede nombrar, elegir la especie y gestionar los niveles de salud, energía, hambre y felicidad de su mascota a través de un menú interactivo.

## El código

{% raw %}
```python
import random
import time

# List of random names if user doesn't enter one
random_names = ["Mochi", "Ziggy", "Pixel", "Boba", "Gismo"]

# Pet Setup
user_name = input("Enter a pet name (or press Enter for a random name): ")
my_pet_name = user_name if user_name.strip() else random.choice(random_names)

pet_types = ["Dog", "Lizard", "Tiger"]
print(f"Available pet species: {pet_types}")
my_pet_type = input("Choose a species: ")

# Initial Randomized Stats (0 - 100 scale)
health = random.randint(70, 100)
energy = random.randint(50, 90)
hunger = random.randint(30, 70)     # Higher = hungrier
happiness = random.randint(30, 80)

print(f"\nMeet your new pet: {my_pet_name} the {my_pet_type}!")

# Main Game Loop
playing = True
while playing:
    print(f"\n*** What would you like to do with {my_pet_name}? ***")
    print("1. Feed food")
    print("2. Play soccer")
    print("3. Sleep")
    print("4. Check stats")
    print("5. Quit")
    
    choice = input("Enter choice (1-5): ")

    if choice == "1":
        print(f"\nYou gave food to {my_pet_name}!")
        hunger = max(0, hunger - 25)
        health = min(100, health + 10)
        time.sleep(1)

    elif choice == "2":
        if energy < 20:
            print(f"\n{my_pet_name} is too tired to play!")
        else:
            print(f"\nYou went to play soccer in the park with {my_pet_name}!")
            happiness = min(100, happiness + 20)
            energy = max(0, energy - 20)
            hunger = min(100, hunger + 15)
        time.sleep(1)

    elif choice == "3":
        print(f"\n{my_pet_name} is sleeping... zzz")
        energy = min(100, energy + 35)
        hunger = min(100, hunger + 10)
        time.sleep(1)

    elif choice == "4":
        print(f"\n--- {my_pet_name}'s Status ---")
        print(f"Health:    {health}/100")
        print(f"Energy:    {energy}/100")
        print(f"Hunger:    {hunger}/100")
        print(f"Happiness: {happiness}/100")

    elif choice == "5":
        print(f"\nGoodbye! Thanks for caring for {my_pet_name}.")
        playing = False
    else:
        print("\nInvalid selection. Choose a number from 1 to 5.")
```
{% endraw %}

## Explicación del código

### 1. Importación de módulos y configuración inicial

El código inicia importando dos librerías nativas de Python:

- `random`: permite generar números y elecciones aleatorias.
- `time`: se utiliza para pausar la ejecución del programa brevemente (`time.sleep(1)`), dando la sensación de animación o tiempo de espera entre acciones.

A continuación, se define una lista de nombres predeterminados (`random_names`) y se solicita información al usuario:

- **Nombre de la mascota:** si el usuario no escribe nada y presiona Enter, la función `random.choice(random_names)` asigna un nombre al azar.
- **Especie:** muestra una lista de opciones (Dog, Lizard, Tiger) y guarda la elección introducida por teclado.

### 2. Asignación de estadísticas iniciales

El programa define cuatro atributos representados en una escala numérica de 0 a 100:

- `health` (Salud): entre 70 y 100.
- `energy` (Energía): entre 50 y 90.
- `hunger` (Hambre): entre 30 y 70 (a mayor número, más hambrienta está la mascota).
- `happiness` (Felicidad): entre 30 y 80.

Todas se inicializan con valores aleatorios dentro de sus respectivos rangos usando `random.randint(mín, máx)`.

### 3. Bucle principal del juego (`while playing`)

La lógica del programa se mantiene activa mediante un bucle `while playing:`, el cual despliega de forma continua un menú de 5 opciones:

1. **Alimentar (opción 1):**
   - Disminuye la variable `hunger` en 25 puntos (usando `max(0, ...)` para asegurar que el hambre nunca sea menor que 0).
   - Aumenta la salud en 10 puntos (usando `min(100, ...)` para evitar superar el límite de 100).
2. **Jugar al fútbol (opción 2):**
   - Incluye una validación previa: si `energy < 20`, la mascota está demasiado cansada para jugar.
   - Si tiene suficiente energía, la felicidad sube 20 puntos, la energía disminuye 20 puntos y el hambre aumenta 15 puntos.
3. **Dormir (opción 3):**
   - Recupera 35 puntos de energía y aumenta el hambre en 10 puntos.
4. **Consultar estadísticas (opción 4):**
   - Muestra en pantalla el estado actualizado de las cuatro variables de la mascota.
5. **Salir (opción 5):**
   - Muestra un mensaje de despedida y cambia la variable `playing` a `False`, lo que rompe el bucle `while` y finaliza la ejecución de la aplicación.

Si el usuario ingresa una opción distinta de 1 a 5, la cláusula `else` captura la entrada inválida y muestra un aviso sin cerrar el programa.

## Funciones clave utilizadas

- `min()` y `max()`: se utilizan como límites de control. `min(100, atributo + X)` evita que la estadística sobrepase 100, mientras que `max(0, atributo - X)` impide que caiga por debajo de 0.
- `user_name.strip()`: elimina espacios en blanco invisibles al inicio y final del texto ingresado para comprobar si la respuesta realmente quedó vacía.
