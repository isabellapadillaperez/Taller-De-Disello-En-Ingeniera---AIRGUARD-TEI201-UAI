# AIRGUARD - Dispositivo de Monitoreo de Calidad de Aire para Taller Mecánico
**Curso:** Taller de Diseño en Ingeniería (TEI201)

---

## 1. Modelo 3D del Prototipo
Se adjuntan los archivos `.stl` que componen la estructura, carcasa y elementos de sujeción:
- `Caja ventilador.stl`
- `Solo placa con sensor.stl`
- `Gancho grande.stl` y `Gancho cuadrado.stl`

### Distribución de Componentes y Espacios Interiores:
![Distribución y Componentes del Prototipo 3D](IMG_2168.jpeg)

**Consideraciones de diseño y arquitectura interna:**
- **Alojamiento interior:** Espacio volumétrico dimensionado para contener de forma protegida la Protoboard, la placa microcontroladora ESP32 (Dev Module) y el módulo MOSFET para control de potencia.
- **Sistema de ventilación forzada:** Aberturas cuadradas en la base diseñadas específicamente para el acople de 2 microventiladores de 5V (tipo Raspberry Pi), canalizando una cortina de aire ascendente hacia las toberas superiores.
- **Alojamiento frontal e interfaz exterior:** Placa frontal con calado rectangular para el montaje visible de la pantalla OLED I2C de 1.3" (controlador SH1106) y perforación circular con ajuste referencial para la recámara de sensado del sensor de gases MQ-135.

---

## 2. Código de Control (Arduino / ESP32)
- **Archivo fuente:** `AIRGUARD_MQ135_OLED.zip`
- **Lógica implementada:** 
  - Adquisición de señal analógica mediante pin GPIO 34 (con divisor de tensión resistivo para protección del ADC a 3.3V).
  - Filtrado de señal por promedio móvil (15 muestras) y procesamiento empírico para cálculo de concentración en PPM.
  - Visualización en tiempo real sobre pantalla OLED SH1106 mediante bus I2C (SDA en GPIO 21, SCL en GPIO 22) usando la librería `U8g2`.

### Umbrales de Decisión de Calidad de Aire:
| Concentración (PPM) | Diagnóstico en Pantalla | Estado del Entorno |
| :--- | :--- | :--- |
| `< 800 PPM` | **BUENA** | Aire seguro / Operación normal |
| `800 - 1600 PPM` | **REGULAR** | Alerta preventiva ante presencia moderada de solventes o gases |
| `> 1600 PPM` | **PELIGRO!** | Nivel crítico / Concentración nociva en taller |

---

## 3. Demostración y Validación de Circuito Funcional
- **Video demostrativo en el repositorio:** [Ver Video Directo](897d9a06-1a2d-4225-96fa-78840c0166c0.mov)
- **Enlace de respaldo externo:** [Ver Video Demostrativo en Google Drive](https://drive.google.com/file/d/1kX5lnDCkN8Mrl-k8V4xY36ZFNQReRqOQ/view?usp=drivesdk)

### Protocolo de validación física:
La prueba se realizó mediante conexión punto a punto entre el ESP32, el sensor MQ-135 y la pantalla OLED:
1. **Línea base (Condición normal):** El sistema inicia con lectura estable en aire ambiente limpio, indicando `Estado: BUENA` y `Concentración: 350 PPM`.
2. **Respuesta ante hidrocarburos volátiles:** Al exponer el sensor a una emisión de gas butano, se activa de forma instantánea el indicador LED físico de la placa del sensor y la pantalla OLED conmuta a `Estado: PELIGRO!` saturando a `5000 PPM`.
3. **Recuperación:** Una vez disipado el gas, las lecturas decaen y el sistema regresa automáticamente a la condición basal segura.
