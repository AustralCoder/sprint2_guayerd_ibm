# Documentación del Proyecto: Tienda Aurelion

## 1. Problema, Objetivo y Solución

### Problema
La tienda Aurelion necesita analizar patrones de venta para poder tomar decisiones estratégicas. Actualmente se desconoce con certeza cuáles son los productos estrella, qué métodos de pago prefieren los clientes y quiénes son los compradores más leales. Además, los datos crudos requieren procesamiento para futuros modelos de predicción.

### Objetivo
1.  **Negocio:** Identificar productos más vendidos, categorías dominantes, métodos de pago preferidos y clientes VIP.
2.  **Técnico:** Realizar una limpieza de datos (ETL), Análisis Exploratorio (EDA), y preparar el dataset para Machine Learning mediante técnicas de normalización y codificación.

### Solución
Desarrollar un programa en Python que analice un dataset de ventas históricas para extraer información clave. La solución consta de dos componentes:
1.  **Notebook de Análisis (`segundosprint.ipynb`):** Motor de procesamiento que ingesta los Excels originales, limpia errores, unifica tablas, realiza análisis estadísticos profundos (outliers, correlaciones) y exporta un dataset procesado.
2.  **Aplicación de Consola (`programa.py`):** Interfaz de usuario que permite navegar por la documentación técnica y visualizar los hallazgos de negocio.

---

## 2. Dataset: Fuente, Definición y Estructura

**Fuente:** Base de datos creada con fines educativos.
**Definición:** Base que contiene información de la tienda Aurelion. Contiene cuatro tablas: clientes, productos, ventas y detalle de ventas.

### Estructura Original de las Tablas

#### Clientes (`clientes.xlsx`)
| Campo | Tipo | Escala |
| :--- | :--- | :--- |
| `id_cliente` | Int | Nominal |
| `nombre_cliente` | String | Nominal |
| `email` | String | Nominal |
| `ciudad` | String | Nominal |
| `fecha_alta` | Date | Intervalo |

#### Productos (`productos.xlsx`)
| Campo | Tipo | Escala |
| :--- | :--- | :--- |
| `id_producto` | Int | Nominal |
| `nombre_producto` | String | Nominal |
| `categoria` | String | Nominal |
| `precio_unitario` | Float | Razón |

#### Ventas (`ventas.xlsx`)
| Campo | Tipo | Escala |
| :--- | :--- | :--- |
| `id_venta` | Int | Nominal |
| `fecha` | Date | Intervalo |
| `id_cliente` | Int | Nominal |
| `nombre_cliente` | String | Nominal |
| `email` | String | Nominal |
| `medio_pago` | String | Nominal |

#### Detalle_ventas (`detalle_ventas.xlsx`)
| Campo | Tipo | Escala |
| :--- | :--- | :--- |
| `id_venta` | Int | Nominal |
| `id_producto` | Int | Nominal |
| `nombre_producto` | String | Nominal |
| `cantidad` | Int | Razón |
| `precio_unitario` | Float | Razón |
| `importe` | Float | Razón |

---

## 3. Resultados del Análisis de Datos (Sprint 2)

Esta sección resume los hallazgos y transformaciones realizadas en el notebook `segundosprint.ipynb`.

### 3.1 Resumen Técnico
* **Entrada:** Archivos Excel originales (`databases/`).
* **Salida:** DataFrame unificado y archivo `Entregable_Sprint2_Aurelion.csv`.
* **Transformaciones:** Merge de las 4 tablas, limpieza de duplicados, codificación de variables categóricas (Encoding) y normalización.

### 3.2 Estadísticas Descriptivas y Distribución
Se analizó la variable crítica **`importe`**:
* **Tendencia Central:** La **media** ($7,730) es mayor que la **mediana** ($6,702), indicando un sesgo positivo (asimetría a la derecha).
* **Dispersión:** Desviación estándar de $5,265, indicando alta variabilidad en los montos de compra.
* **Curtosis:** Elevada, lo que refuerza la presencia de colas pesadas (valores extremos).

### 3.3 Detección de Outliers y Preprocesamiento
* **Método:** Regla del Rango Intercuartílico (IQR).
* **Hallazgo:** Se detectaron múltiples ventas superiores a $20,000 (Outliers).
* **Decisión de ML:** Debido a la presencia de outliers significativos y la asimetría de la distribución, se optó por utilizar **`RobustScaler`** para la normalización de datos, descartando `StandardScaler` para evitar sesgos en futuros modelos.

### 3.4 Correlaciones
* **Lógica:** Correlación positiva fuerte entre `importe` y `cantidad`/`precio`.
* **Medios de Pago:** Las correlaciones entre `importe` y los medios de pago (codificados con One-Hot) resultaron cercanas a cero.
    * *Interpretación:* **No existe una relación lineal clara entre el monto gastado y el medio de pago elegido.** Los clientes usan tarjeta o efectivo indistintamente del precio.

### 3.5 Conclusiones de Negocio
1.  **Productos Estrella:** El producto más vendido por volumen es **'Salsa de Tomate 500g'**.
2.  **Clientes VIP:** Se identificó un Top de clientes liderado por **Agustina Flores** y **Bruno Castro**, candidatos ideales para programas de fidelización.
3.  **Inventario:** Las categorías de Alimentos y Limpieza muestran un comportamiento de precios mixto (visualizado en Scatterplot), sugiriendo que ambas categorías tienen productos de alta y baja gama.

---

## 4. Información del Programa (Script Python)

### 4.1 Contenidos del Menú Interactivo
El archivo `programa.py` ha sido actualizado para incluir los nuevos hallazgos:

1.  **Tema, problema y solución**
2.  **Dataset de referencia**
3.  **Información, pasos, pseudocódigo**
4.  **Diagrama de flujo del programa**
5.  **Sugerencias y mejoras (Copilot)**
6.  **📊 Hallazgos Estadísticos y ML** *(Nuevo - Sprint 2)*
7.  **🏆 Conclusiones de Negocio** *(Nuevo - Sprint 2)*
0.  **Salir**

### 4.2 Pseudocódigo Actualizado

```text
INICIO
    Importar librerías
    Definir funciones de visualización (mostrar_estadisticas, mostrar_conclusiones, etc.)
    
    Mientras True:
        Limpiar pantalla
        Mostrar Menú Principal (Opciones 0 a 7)
        Leer entrada del usuario -> op_raw
        
        Intentar convertir op_raw a entero -> op
        Si error: Mostrar "Entrada inválida" y Continuar
        
        Si op == 0:
            Mostrar "Finalizado", Romper Bucle
        
        Si op está entre 1 y 5:
            Mostrar documentación estática del Sprint 1
        
        Si op == 6:
            Mostrar Resumen Estadístico (Media, Mediana, Outliers, Decisión de Scaler)
        
        Si op == 7:
            Mostrar Conclusiones de Negocio (Top Productos, Clientes, Pagos)
            
        Si op no es válido:
            Mostrar "Opción fuera de rango"
            
        Esperar "Enter" del usuario antes de limpiar pantalla
    Fin Mientras
FIN