# 1. Informacion Del Proyecto
- Nombre: WildVision
- Equipo: WildVision 
- Roles:
- Ariela Jiménez Rodríguez- Coordinadora
- Allison Romero Jiménez - Desarrolladora principal
- Anthony Blanco Paniagua - Diseñador
- Oscar Vásquez Aguilera - Desarrollador 

Este proyecto consiste en el desarrollo de un visor inteligente capaz de identificar aves en tiempo real mediante el uso de inteligencia artificial y microcontroladores.

# 2. Descripcion y Justificacion 

La observación de aves y vida silvestre es una actividad esencial para la educación ambiental, la investigación científica y la conservación de la biodiversidad. Sin embargo, muchos de los métodos tradicionales para estudiar el comportamiento animal en su entorno natural requieren una gran inversión de tiempo, presencia humana constante y conocimientos técnicos para identificar especies con precisión.
El proyecto WildVision responde a esta necesidad mediante el desarrollo de un visor inteligente, autónomo y de bajo consumo energético, capaz de atraer, registrar y analizar la presencia de aves, mariposas y otros animales silvestres. Al integrar un sistema de cámara (ESP32-CAM), sensores de temperatura, un comedero pasivo dividido, y herramientas de inteligencia artificial, se ofrece una solución práctica y moderna para capturar evidencia visual y contextual de la fauna local.
Además, este dispositivo no solo permite el registro visual, sino que también analiza las imágenes mediante APIs de inteligencia artificial para identificar especies y comportamientos. Esto democratiza el acceso al conocimiento natural, permitiendo que estudiantes, educadores, científicos y comunidades rurales puedan obtener información valiosa sin necesidad de experiencia técnica avanzada.
Finalmente, el proyecto se alinea con los principios de sostenibilidad y bajo impacto ambiental, al no depender de mecanismos motorizados para el dispensador y al promover el uso de tecnologías libres y de bajo costo.

# 3. Objetivos del Proyecto

### Objetivo General
Desarrollar un visor inteligente de aves basado en el ESP32-CAM que, mediante sensores y una API de inteligencia artificial, pueda capturar imágenes, analizar especies y mostrar resultados al usuario final a través de una aplicación web o móvil.

### Objetivos Específicos
- Capturar imágenes de aves mediante la cámara del ESP32-CAM.
- Medir condiciones del entorno (como temperatura) mediante sensores.
- Enviar datos e imágenes a una API de IA para identificación de la especie.
- Mostrar los resultados en una interfaz accesible (web o app móvil).
- Permitir almacenamiento local de información cuando no haya conexión.


# 4. Requisitos Iniciales 
### El sistema debe ser capaz de:
-  Capturar imágenes con calidad suficiente desde el ESP32-CAM.
-  Medir temperatura ambiente utilizando sensores DHT22 o BME280.
-  Conectarse a una red WiFi y enviar datos a la nube o API externa.
-  Recibir la especie analizada desde una API de inteligencia artificial.
-  Mostrar los resultados al usuario y guardar respaldo localmente si es necesario


# 5. Diseño Preliminar

<img width="1280" height="290" alt="image" src="https://github.com/user-attachments/assets/b9e35859-a9e2-4888-bd11-1b0affd8ba40" />

El sistema está centrado en un ESP32-CAM, que actúa como microcontrolador y cámara. Se conecta a sensores (como el de temperatura y un comedero dividido) y utiliza un módulo WiFi para enviar imágenes y datos a una API de inteligencia artificial, la cual analiza las especies de aves. La información puede almacenarse localmente o enviarse a una aplicación web o móvil para que el usuario final visualice los resultados.

### Componentes previstos

- **Microcontrolador:**  
  ESP32-CAM (con cámara integrada)

- **Sensores / Actuadores:**  
  - Sensor de temperatura: DHT22 o BME280  
  - Comedero dividido: para distintos tipos de alimento (semillas y néctar)

- **LLM / API de IA:**  
  - API para reconocimiento de especies de aves (se contempla su uso ) 
  - Puede estar entrenada en plataformas como Google AutoML, Teachable Machine, Hugging Face, etc.

- **Librerías y herramientas:**  
  - `esp_camera.h` (control de cámara)  
  - `WiFi.h` (para conectividad)  
  - `ArduinoJson.h` (manejo de JSON si se usa HTTP)  
  - Backend opcional con Python (Flask o FastAPI)



# 6. Plan de Trabajo 
| Fecha          | Actividad                                                               |
| -------------- | ----------------------------------------------------------------------- |
| **03/08/2025** | Revisión del código base, definición de funciones del sistema           |
| **04–08/08**   | Configuración y pruebas de cámara en ESP32, captura de imagen básica    |
| **09–11/08**   | Integración de sensores de temperatura, envío de datos vía serial       |
| **12–13/08**   | Conexión entre ESP32 y modelo de IA (prueba local o en servidor ligero) |
| **14–15/08**   | Desarrollo de app receptora (puede ser móvil o web embebida)            |
| **16/08/2025** | Pruebas integradas: captura de imagen → análisis IA → visualización     |
| **17–18/08**   | Documentación, capturas, pruebas finales, limpieza de código            |
| **19/08/2025** |  **Entrega del repositorio final**                                      |
| **20/08/2025** |  **Presentación final del proyecto**                                    |

**Riesgos identificados y mitigaciones**
Riesgo 1: Conexión inestable entre ESP32 y cámara o IA.

Mitigación: Realizar pruebas con múltiples cables, asegurar fuente de alimentación estable y tener versiones offline del modelo si es posible.

Riesgo 2: El modelo de IA no reconoce bien las aves por baja resolución.

Mitigación: Usar resolución mínima aceptable (QVGA o superior) y entrenar el modelo con imágenes similares a las que entrega la cámara del ESP32.

# 7.Prototipos conceptuales
**Código mínimo de prueba**

#include "esp_camera.h"  // Librería para controlar la cámara del ESP32

// Definición de los pines GPIO conectados a la cámara AI Thinker
#define PWDN_GPIO_NUM    -1   // No se utiliza
#define RESET_GPIO_NUM   -1   // No se utiliza
#define XCLK_GPIO_NUM     0
#define SIOD_GPIO_NUM    26
#define SIOC_GPIO_NUM    27

#define Y9_GPIO_NUM      35
#define Y8_GPIO_NUM      34
#define Y7_GPIO_NUM      39
#define Y6_GPIO_NUM      36
#define Y5_GPIO_NUM      21
#define Y4_GPIO_NUM      19
#define Y3_GPIO_NUM      18
#define Y2_GPIO_NUM       5

#define VSYNC_GPIO_NUM   25
#define HREF_GPIO_NUM    23
#define PCLK_GPIO_NUM    22

/**
 * Función de configuración inicial.
 * Inicializa el puerto serial y la cámara del ESP32.
 */
void setup() {
  Serial.begin(115200);  // Inicia comunicación serial a 115200 baudios

  /**
   * Estructura que contiene la configuración de la cámara:
   * Se definen los pines, resolución, calidad y formato de imagen.
   */
  camera_config_t config;
  config.ledc_channel = LEDC_CHANNEL_0;
  config.ledc_timer = LEDC_TIMER_0;
  
  // Asignación de pines de datos (D0 a D7)
  config.pin_d0 = Y2_GPIO_NUM;
  config.pin_d1 = Y3_GPIO_NUM;
  config.pin_d2 = Y4_GPIO_NUM;
  config.pin_d3 = Y5_GPIO_NUM;
  config.pin_d4 = Y6_GPIO_NUM;
  config.pin_d5 = Y7_GPIO_NUM;
  config.pin_d6 = Y8_GPIO_NUM;
  config.pin_d7 = Y9_GPIO_NUM;

  // Asignación de pines de sincronización y control
  config.pin_xclk = XCLK_GPIO_NUM;
  config.pin_pclk = PCLK_GPIO_NUM;
  config.pin_vsync = VSYNC_GPIO_NUM;
  config.pin_href = HREF_GPIO_NUM;
  config.pin_sscb_sda = SIOD_GPIO_NUM;
  config.pin_sscb_scl = SIOC_GPIO_NUM;
  config.pin_pwdn = PWDN_GPIO_NUM;
  config.pin_reset = RESET_GPIO_NUM;

  // Configuraciones adicionales
  config.xclk_freq_hz = 20000000;        // Frecuencia del reloj XCLK
  config.pixel_format = PIXFORMAT_JPEG;  // Formato de imagen JPEG
  config.frame_size = FRAMESIZE_QVGA;    // Resolución 320x240 (QVGA)
  config.jpeg_quality = 12;              // Calidad de imagen JPEG (0-63, menor es mejor calidad)
  config.fb_count = 1;                   // Número de frame buffers (1 para bajo consumo)

  /**
   * Inicializa la cámara con la configuración especificada.
   * Si falla, imprime un mensaje de error por el monitor serial.
   */
  if (esp_camera_init(&config) != ESP_OK) {
    Serial.println("Error al iniciar cámara");
    return;
  }

  Serial.println("Cámara lista");
}

/**
 * Función principal que se ejecuta en bucle.
 * Captura una imagen y muestra su tamaño por el puerto serial.
 */
void loop() {
  // Captura un frame de la cámara
  camera_fb_t *fb = esp_camera_fb_get();
  if (!fb) {
    Serial.println("Error capturando imagen");
    return;
  }

  // Muestra el tamaño de la imagen capturada
  Serial.printf("Imagen capturada: %u bytes\n", fb->len);

  // Libera el buffer de la imagen para futuras capturas
  esp_camera_fb_return(fb);

  // Espera 5 segundos antes de capturar otra imagen
  delay(5000);
}
