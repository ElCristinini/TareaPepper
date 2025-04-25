# TareaPepper
# 🤖 Proyecto Pepper - Coreografía Macarena

Este repositorio contiene un proyecto en Python para controlar al robot **Pepper** (SoftBank/Aldebaran) y hacer que realice la coreografía de la canción **"Macarena"**. Incluye instrucciones de configuración, uso desde la terminal, estructura de código y manejo de errores.

---

## 📋 Tabla de Contenidos

1. [Autores](#autores)
2. [Descripción](#descripción)
3. [Requisitos Previos](#requisitos-previos)
4. [Instalación de Choregraphe](#instalación-de-choregraphe)
5. [Configuración del Entorno](#configuración-del-entorno)
6. [Uso desde la Terminal](#uso-desde-la-terminal)
7. [Estructura del Proyecto](#estructura-del-proyecto)
8. [Explicación del Código](#explicación-del-código)
   - [Creación de Proxies](#creación-de-proxies)
   - [Preparación del Robot](#preparación-del-robot)
   - [Activación de Movimiento](#activación-de-movimiento)
   - [Función `macarena_cycle`](#función-macarena_cycle)
   - [Secuencia de Movimientos](#secuencia-de-movimientos)
   - [Pequeño Giro ("¡Hey Macarena!")](#pequeño-giro-hey-macarena)
   - [Repetición de la Coreografía](#repetición-de-la-coreografía)
   - [Manejo de Errores](#manejo-de-errores)
9. [Contribuciones](#contribuciones)
10. [Licencia](#licencia)
11. [Contacto](#contacto)

---

## 👥 Autores

- **Andrés Badillo**
- **Ana Vargas**
- **Cristian Olarte**

*Fecha de creación: 25 de abril de 2025*

---

## 📝 Descripción

Este proyecto permite conectar y programar el robot Pepper para que ejecute la coreografía de la "Macarena". Utiliza el framework `qi` para la comunicación con los servicios del robot, controlar movimientos, síntesis de voz y más.

---

## 🔧 Requisitos Previos

- **Sistema Operativo**: Ubuntu (o cualquier distribución Linux con acceso SSH)
- **Python 3.x**
- **Choregraphe** (versión de prueba de 90 días)
- Conexión de red al robot Pepper
- Acceso SSH habilitado en el robot

---

## 📥 Instalación de Choregraphe

1. Descarga el instalador de Choregraphe desde MEGA:
   ```
   https://mega.nz/file/N6IHzSbY#0om2GPPUeKcS-sY29pgRW5MZ4CO1WAC2WEhZFHn6V88
   ```
2. Ejecuta el instalador y sigue los pasos:
   - Next → Aceptar términos → Next
   - Selecciona *Quick Install* → Next → Install
   - Finaliza (Finish) y elige la prueba de 90 días
   - Al abrir, conecta con el robot (ícono verde) y comienza a usar

---

## ⚙️ Configuración del Entorno

1. Instala las librerías Python necesarias:
   ```bash
   pip install qi argparse almath
   ```
2. Asegúrate de que el robot Pepper esté en la misma red y que puedas acceder vía SSH:
   ```bash
   ssh nao@<IP_DEL_ROBOT>
   ```
3. Clona este repositorio (opcional):
   ```bash
   git clone https://github.com/TU_USUARIO/TareaPepper.git
   cd TareaPepper
   ```

---

## 🚀 Uso desde la Terminal

1. En Ubuntu, abre la terminal y conéctate al robot:
   ```bash
   ssh nao@<IP_DEL_ROBOT>
   ```
2. Navega al directorio que contiene los scripts:
   ```bash
   cd /home/nao/TareaPepper
   ```
3. Ejecuta el script principal:
   ```bash
   python3 Bakend.py
   ```
4. Opcional: crea tus propios archivos `.py` usando las librerías mencionadas y ejecútalos.

---

## 📁 Estructura del Proyecto

```bash
TareaPepper/
├── README.md            # Documentación del proyecto
├── Bakend.py            # Script principal con la coreografía
├── choregraphe_flows/   # Flujos exportados de Choregraphe
└── utils/               # Módulos auxiliares (si aplica)
```

---

## 🧩 Explicación del Código

### Creación de Proxies

Se establecen conexiones (proxies) a los servicios del robot:
```python
from qi import Session
session = Session()
session.connect("tcp://<IP_DEL_ROBOT>:9559")
motion = session.service("ALMotion")
posture = session.service("ALRobotPosture")
tts = session.service("ALTextToSpeech")
```

### Preparación del Robot

- Lleva a Pepper a postura inicial:
  ```python
  posture.goToPosture("StandInit", 0.5)
  tts.say("Voy a bailar la Macarena")
  ```

### Activación de Movimiento

- Despierta motores y habilita movimiento de brazos:
  ```python
  motion.wakeUp()
  motion.setMoveArmsEnabled(True, True)
  ```

### Función `macarena_cycle`

Define los ángulos y tiempos para cada paso de la Macarena:
```python
def macarena_cycle():
    names = ["LArm", "RArm", ...]
    angles = [...]
    times = [...]
    motion.angleInterpolation(names, angles, times, True)
```

### Secuencia de Movimientos

Cada ángulo corresponde a una articulación (brazo, cadera). Se usan listas paralelas de nombres, ángulos y tiempos para interpolar.

### Pequeño Giro ("¡Hey Macarena!")

Al finalizar la coreografía, el robot hace un giro y dice:
```python
motion.moveTo(0, 0, math.radians(45))
tts.say("¡Hey Macarena!")
```

### Repetición de la Coreografía

El ciclo de baile se repite 3 veces:
```python
for i in range(3):
    macarena_cycle()
```

### Manejo de Errores

Se utiliza un bloque `try-except-finally` para asegurar que, en caso de fallo:
- Se detengan movimientos (`motion.stopMove()`)
- Se vuelva a la postura inicial
- Se desactiven motores (`motion.rest()`)

```python
try:
    # ejecución del baile
except Exception as e:
    print(f"Error: {e}")
finally:
    motion.stopMove()
    posture.goToPosture("StandInit", 0.5)
    motion.rest()
```

---

## 🤝 Contribuciones

¡Las contribuciones son bienvenidas! Por favor, abre un *issue* o un *pull request* para sugerencias, mejoras o correcciones.

---

## 📄 Licencia

Este proyecto está bajo la licencia MIT. Consulta el archivo `LICENSE` para más detalles.

---

