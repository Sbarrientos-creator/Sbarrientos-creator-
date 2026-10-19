---
layout: post
title: "Simulador de inversión en texto con Python"
date: 2026-10-18
categories: coding
---

Un juego de consola donde compras y vendes acciones durante 30 días. Los precios se simulan con Movimiento Browniano Geométrico (GBM) y al final se dibujan gráficos con Matplotlib. *Contenido educativo, no es consejo financiero.*

## El código completo

{% raw %}
```python
import math
import random
import matplotlib.pyplot as plt

# ==========================================
# 1. ASSET CONFIGURATION & GBM PARAMETERS
# ==========================================
# Each asset has distinct annual drift (mu) and annual volatility (sigma)
ASSETS = {
    "ALPHA": {
        "name": "Alpha ETF (Low Risk / Index)",
        "price": 100.00,
        "mu": 0.08,    # 8% annual expected drift
        "sigma": 0.15   # 15% annual volatility
    },
    "BETA": {
        "name": "Beta Tech (Growth Stock)",
        "price": 50.00,
        "mu": 0.18,    # 18% annual expected drift
        "sigma": 0.35   # 35% annual volatility
    },
    "GAMMA": {
        "name": "Gamma Crypto (Speculative)",
        "price": 25.00,
        "mu": 0.05,    # 5% annual drift
        "sigma": 0.90   # 90% extreme volatility
    }
}

# Market news events that apply temporary macro shocks
MARKET_EVENTS = [
    ("Fed announces interest rate hike!", -0.04, 0.05),
    ("Tech sector rallies on AI earnings surge!", 0.06, -0.02),
    ("Regulatory scrutiny hits speculative assets!", -0.02, 0.10),
    ("Strong GDP growth report released!", 0.03, -0.01)
]

def apply_gbm_step(current_price: float, mu: float, sigma: float, dt: float = 1/252) -> float:
    """
    Simulates a 1-day step using Geometric Brownian Motion (GBM):
    S_t = S_{t-1} * exp((mu - 0.5 * sigma^2)*dt + sigma * sqrt(dt) * Z)
    """
    Z = random.gauss(0, 1)  # Standard normal random variable N(0, 1)
    drift = (mu - 0.5 * (sigma ** 2)) * dt
    diffusion = sigma * math.sqrt(dt) * Z
    new_price = current_price * math.exp(drift + diffusion)
    return round(max(0.01, new_price), 2)


def advanced_stock_trader_game():
    cash = 1000.00
    portfolio = {s: 0 for s in ASSETS}
    cost_basis = {s: 0.0 for s in ASSETS}  # Tracks invested dollars per asset
    
    prices = {s: ASSETS[s]["price"] for s in ASSETS}
    total_days = 30
    dt = 1 / 252  # 1 trading day fraction of a 252-day trading year

    # Historical metrics tracking for plotting
    history = {
        "days": list(range(1, total_days + 1)),
        "net_worth": [],
        "prices": {s: [] for s in ASSETS}
    }

    print("=" * 65)
    print("      ADVANCED QUANT TRADER & INVESTMENT SIMULATOR")
    print(" Model: Geometric Brownian Motion (GBM) + Stochastic Shocks")
    print(" Goal:  Survive 30 Days & Maximize Risk-Adjusted Net Worth")
    print("=" * 65)

    for day in range(1, total_days + 1):
        # Track initial price history for the day
        for s in prices:
            history["prices"][s].append(prices[s])

        # Random news shock (15% chance daily)
        event_impact = 0.0
        if random.random() < 0.15:
            event_text, shock, vol_change = random.choice(MARKET_EVENTS)
            event_impact = shock
            print(f"\n[MARKET EVENT]: {event_text}")

        while True:
            holdings_val = sum(portfolio[s] * prices[s] for s in portfolio)
            net_worth = cash + holdings_val

            print(f"\n--- DAY {day} OF {total_days} ---")
            print(f"Cash: ${cash:,.2f} | Portfolio Value: ${holdings_val:,.2f} | Net Worth: ${net_worth:,.2f}")
            print("\nMarket Prices & Asset Performance:")
            
            for stock, info in ASSETS.items():
                owned = portfolio[stock]
                curr_price = prices[stock]
                val = owned * curr_price
                
                # Calculate Unrealized P&L
                if owned > 0:
                    avg_cost = cost_basis[stock] / owned
                    unrealized_pnl = val - cost_basis[stock]
                    pnl_str = f"| P&L: ${unrealized_pnl:+,.2f} (Avg Cost: ${avg_cost:.2f})"
                else:
                    pnl_str = "| Owned: 0"

                print(f"  [{stock}] {info['name']}")
                print(f"      Price: ${curr_price:.2f} | Shares: {owned} (${val:,.2f}) {pnl_str}")

            print("\nCommands: [1] Buy  [2] Sell  [3] Hold / End Day")
            choice = input("Select an option (1-3): ").strip()

            if choice == "1":
                stock = input("Stock symbol to BUY (ALPHA, BETA, GAMMA): ").strip().upper()
                if stock not in prices:
                    print("Invalid stock symbol!")
                    continue
                try:
                    qty = int(input(f"How many shares of {stock} to buy? "))
                    if qty <= 0:
                        print("Quantity must be greater than 0.")
                        continue
                    cost = qty * prices[stock]
                    if cost > cash:
                        print(f"Insufficient funds. Cost: ${cost:,.2f}, Cash: ${cash:,.2f}")
                    else:
                        cash -= cost
                        portfolio[stock] += qty
                        cost_basis[stock] += cost
                        print(f"Successfully bought {qty} shares of {stock} for ${cost:,.2f}.")
                except ValueError:
                    print("Please enter a valid integer.")

            elif choice == "2":
                stock = input("Stock symbol to SELL (ALPHA, BETA, GAMMA): ").strip().upper()
                if stock not in prices:
                    print("Invalid stock symbol!")
                    continue
                try:
                    qty = int(input(f"How many shares of {stock} to sell? "))
                    if qty <= 0:
                        print("Quantity must be greater than 0.")
                        continue
                    if qty > portfolio[stock]:
                        print(f"You don't own enough shares. Owned: {portfolio[stock]}")
                    else:
                        revenue = qty * prices[stock]
                        # Reduce cost basis proportionally
                        avg_cost = cost_basis[stock] / portfolio[stock]
                        cost_basis[stock] -= avg_cost * qty
                        
                        cash += revenue
                        portfolio[stock] -= qty
                        print(f"Successfully sold {qty} shares of {stock} for ${revenue:,.2f}.")
                except ValueError:
                    print("Please enter a valid integer.")

            elif choice == "3":
                break
            else:
                print("Invalid choice. Select 1, 2, or 3.")

        # Record daily net worth history
        current_holdings = sum(portfolio[s] * prices[s] for s in portfolio)
        history["net_worth"].append(cash + current_holdings)

        # Update Asset Prices using GBM + Macro Shock
        for stock in prices:
            mu = ASSETS[stock]["mu"] + event_impact
            sigma = ASSETS[stock]["sigma"]
            prices[stock] = apply_gbm_step(prices[stock], mu, sigma, dt)

    # ==========================================
    # GAME OVER SUMMARY & VISUALIZATIONS
    # ==========================================
    final_holdings = sum(portfolio[s] * prices[s] for s in portfolio)
    final_net_worth = cash + final_holdings
    total_return = ((final_net_worth - 1000.00) / 1000.00) * 100

    print("\n" + "=" * 65)
    print("               GAME OVER - 30 DAYS COMPLETED")
    print("=" * 65)
    print(f"Final Cash:           ${cash:,.2f}")
    print(f"Final Portfolio Value:${final_holdings:,.2f}")
    print(f"Final Net Worth:      ${final_net_worth:,.2f}")
    print(f"Total Profit / Loss:  ${final_net_worth - 1000.00:+,.2f} ({total_return:+.2f}%)")
    print("=" * 65)

    # Plot Performance Charts
    fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(10, 8), sharex=True)

    # Plot Net Worth Trajectory
    ax1.plot(history["days"], history["net_worth"], color="green", linewidth=2, label="Net Worth ($)")
    ax1.axhline(1000, color="gray", linestyle="--", alpha=0.7, label="Initial Capital ($1,000)")
    ax1.set_title("Portfolio Performance Over Time", fontsize=12, fontweight="bold")
    ax1.set_ylabel("Portfolio Net Worth ($)")
    ax1.grid(True, linestyle=":", alpha=0.6)
    ax1.legend(loc="upper left")

    # Plot Stock Price Trajectories
    for stock in ASSETS:
        ax2.plot(history["days"], history["prices"][stock], label=f"{stock} ({ASSETS[stock]['name']})")
    ax2.set_title("Asset Price Trajectories (Geometric Brownian Motion)", fontsize=12, fontweight="bold")
    ax2.set_xlabel("Day")
    ax2.set_ylabel("Stock Price ($)")
    ax2.grid(True, linestyle=":", alpha=0.6)
    ax2.legend(loc="upper left")

    plt.tight_layout()
    plt.show()

if __name__ == "__main__":
    advanced_stock_trader_game()
```
{% endraw %}

Para ejecutarlo necesitas Python 3 y Matplotlib (`pip install matplotlib`).

## ¿Cómo poder hacerlo?

### 1. Importar las librerías necesarias

El programa requiere tres herramientas básicas de Python:

```python
import math
import random
import matplotlib.pyplot as plt
```

- `math`: proporciona la función exponencial (e elevado a x) y la raíz cuadrada para los cálculos financieros.
- `random`: genera números aleatorios (utilizando una distribución normal o gaussiana para simular las fluctuaciones de precio).
- `matplotlib.pyplot`: dibuja los gráficos de rendimiento al finalizar el juego.

### 2. Definir los activos y sus parámetros financieros

Las acciones no cambian de precio de forma totalmente aleatoria; siguen dos factores clave:

1. **Drift (mu):** la tendencia promedio de la acción a subir o bajar a lo largo del tiempo (rendimiento esperado).
2. **Volatilidad (sigma):** qué tan bruscos son los saltos diarios de precio (el riesgo).

En el código se representan estos parámetros mediante un diccionario:

{% raw %}
```python
ASSETS = {
    "ALPHA": {"name": "Alpha ETF", "price": 100.00, "mu": 0.08, "sigma": 0.15},
    "BETA": {"name": "Beta Tech", "price": 50.00, "mu": 0.18, "sigma": 0.35},
    "GAMMA": {"name": "Gamma Crypto", "price": 25.00, "mu": 0.05, "sigma": 0.90}
}
```
{% endraw %}

- **ALPHA** representa un fondo estable: bajo retorno esperado (mu = 8%), pero poca volatilidad (sigma = 15%).
- **GAMMA** representa un activo cripto/especulativo: retorno modesto, pero altísima volatilidad (sigma = 90%).

### 3. Crear el motor de simulación de precios (GBM)

Esta es la función principal. La fórmula del Movimiento Browniano Geométrico calcula el precio del día siguiente:

```
S_t = S_(t-1) * exp( (mu - 0.5 * sigma^2) * dt + sigma * sqrt(dt) * Z )
```

En Python, la función se escribe de la siguiente manera:

```python
def apply_gbm_step(current_price, mu, sigma, dt=1/252):
    Z = random.gauss(0, 1)  # Número aleatorio con distribución normal (campana de Gauss)
    drift = (mu - 0.5 * (sigma ** 2)) * dt
    diffusion = sigma * math.sqrt(dt) * Z
    new_price = current_price * math.exp(drift + diffusion)
    return round(max(0.01, new_price), 2)
```

- `dt = 1/252`: representa 1 día hábil dentro de un año bursátil estándar (252 días de negociación).
- `random.gauss(0, 1)`: genera el impacto aleatorio del mercado (Z).
- `max(0.01, new_price)`: evita que una acción tome valores negativos o de cero absoluto.

### 4. Estructurar el bucle principal del juego

El juego transcurre durante un número determinado de días (30). Se utiliza un bucle `for` para avanzar día a día y un bucle `while` interno para permitir múltiples operaciones antes de cerrar la jornada.

**Variables de estado iniciales:**

- `cash`: dinero en efectivo disponible ($1,000.00).
- `portfolio`: cantidad de acciones poseídas de cada empresa.
- `cost_basis`: suma total gastada en las acciones actuales (para calcular el precio medio de compra y la ganancia no realizada).
- `history`: listas para almacenar el patrimonio diario y los precios, permitiendo graficarlos al final.

```python
for day in range(1, total_days + 1):
    # 1. Guardar historial para los gráficos
    # 2. Aplicar evento aleatorio de noticias (si aplica)
    # 3. Mostrar estado del portafolio (Efectivo, Acciones, P&L)
    # 4. Procesar comando del usuario (1: Comprar, 2: Vender, 3: Pasar día)
    # 5. Actualizar los precios para el día siguiente con apply_gbm_step()
```

### 5. Lógica de compra, venta y precio promedio

Para que el juego sea representativo de la operativa real, se debe controlar el dinero y los costos de adquisición:

- **Al comprar:** se verifica que `costo <= cash`. Si la compra es válida, se resta el costo del efectivo disponible, se suman las acciones al portafolio y se suma el dinero gastado a `cost_basis`.
- **Al vender:** se verifica que `cantidad <= acciones_poseídas`. Al vender, se calcula el precio promedio de compra acumulado y se reduce proporcionalmente el costo base de la posición.

```
Precio Promedio = Costo Base / Acciones Poseídas
```

### 6. Generar gráficos con Matplotlib

Al completar los 30 días, el programa toma los datos guardados en `history` y genera una figura con dos paneles (`plt.subplots(2, 1)`):

```python
fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(10, 8), sharex=True)

# Gráfico 1: Evolución del valor total de la cuenta
ax1.plot(history["days"], history["net_worth"], color="green")

# Gráfico 2: Trayectoria de precios de las 3 acciones
for stock in ASSETS:
    ax2.plot(history["days"], history["prices"][stock])

plt.show()
```

El gráfico superior muestra si lograste superar los $1,000 iniciales, mientras que el inferior ilustra la volatilidad individual de cada activo generada por la simulación Monte Carlo diaria.

## Video explicativo

Aquí hay un video que explica este código por si lo prefieren: [Ver en YouTube](https://www.youtube.com/watch?v=LWc-9v8RVwM)
