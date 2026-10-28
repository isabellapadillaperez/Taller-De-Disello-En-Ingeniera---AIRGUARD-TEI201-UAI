# AIRGUARD - Dispositivo de Monitoreo de Calidad de Aire para Taller Mecánico
**Curso:** Taller de Diseño en Ingeniería (TEI201)

---

## 1. Modelo 3D del Prototipo
Se adjuntan los archivos `.stl` de las piezas de la estructura y carcasa:
- `Caja ventilador.stl`
- `Solo placa con sensor.stl`
- `Gancho grande.stl` y `Gancho cuadrado.stl`

**Consideraciones de diseño:**
- Espacios interiores y anclajes referenciales para el alojamiento y fijación del microcontrolador ESP32.
- Abertura frontal para visualización en pantalla OLED de 1.3" y tobera inferior para la recámara de sensado del MQ-135.

---

## 2. Código de Control (Arduino)
- Archivo fuente: `AIRGUARD_MQ135_OLED.zip` (programado para ESP32 Dev Module).
- **Lógica implementada:** Conversión analógica-digital de señal del sensor MQ-135 (GPIO 34 con divisor de voltaje) y visualización en tiempo real en pantalla OLED SH1106 mediante librería `U8g2`.

### Umbrales de Calidad de Aire (PPM):
| Rango de PPM | Diagnóstico en Pantalla | Estado |
| :--- | :--- | :--- |
| `< 800 PPM` | **BUENA** | Calidad óptima / Área segura |
| `800 - 1600 PPM` | **REGULAR** | Alerta preventiva ante vapores moderados |
| `> 1600 PPM` | **PELIGRO!** | Concentración crítica / Gases nocivos en taller |

---

## 3. Demostración y Validación de Circuito Funcional
- **Video demostrativo (Funcionamiento físico punto a punto):**  
  👉 [Ver Video en Google Drive](https://drive.google.com/file/d/1kX5lnDCkN8Mrl-k8V4xY36ZFNQReRqOQ/view?usp=drivesdk)

*Nota: La validación física se realizó mediante cableado directo punto a punto entre la placa ESP32, el sensor MQ-135 y la pantalla OLED, demostrando lectura continua en tiempo real y respuesta inmediata ante variaciones de concentración.*
