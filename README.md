# 1. Informacion Del Proyecto
-Nombre:WildVision

-Equipo:

-Roles:

Este proyecto consiste en el desarrollo de un visor inteligente capaz de identificar aves en tiempo real mediante el uso de inteligencia artificial y microcontroladores.

# 2. Descripcion y Justificacion 

Problema que se aborda:
La observación de aves y otros animales silvestres es difícil sin alterar su comportamiento natural. Además, identificar especies y comportamientos manualmente requiere tiempo, presencia constante y conocimientos técnicos. Estas limitaciones dificultan la participación de estudiantes, comunidades rurales y personas con poco acceso a recursos tecnológicos.

Importancia y contexto:
El proyecto WildVision ofrece una alternativa moderna y accesible para el estudio de la fauna silvestre mediante un visor inteligente que combina sensores, cámara, y herramientas de inteligencia artificial. En un contexto de creciente pérdida de biodiversidad y cambio climático, contar con tecnologías que ayuden a monitorear y proteger el entorno natural es crucial para investigadores, educadores y comunidades.

Este sistema está diseñado para operar de forma autónoma, sin perturbar el ecosistema, y sin necesidad de interacción humana directa constante. Al no utilizar partes móviles para la dispensación de alimento, reduce la complejidad mecánica y el consumo energético, haciéndolo ideal para zonas rurales, bosques o jardines escolares.

Usuarios/beneficiarios:
Observadores de aves, biólogos y naturalistas.

Docentes y estudiantes en áreas de ciencias, biología o ecología.

Comunidades rurales y ecoturísticas que deseen promover la fauna local.

Niños, niñas y jóvenes interesados en el aprendizaje interactivo sobre la naturaleza.

Justificación del Proyecto:
El visor inteligente WildVision facilita el estudio, monitoreo y apreciación de la fauna silvestre mediante una herramienta tecnológica de bajo costo, bajo consumo y alto valor educativo y científico. Su diseño sin motores en el dispensador permite que el sistema sea más robusto y sencillo de mantener, ideal para contextos en los que no hay acceso continuo a electricidad o personal técnico.

La capacidad de capturar imágenes y analizarlas con inteligencia artificial agrega una capa innovadora al proyecto, permitiendo no solo registrar eventos, sino también generar conocimiento automático sobre las especies observadas. Esto democratiza el acceso a la ciencia y a la educación ambiental, haciendo que cualquier persona pueda aprender y contribuir al cuidado de la biodiversidad desde su entorno cercano.

Además, el proyecto promueve valores sostenibles, como el uso de tecnologías libres, la reducción de componentes mecánicos innecesarios, y el aprovechamiento de energías alternativas, como baterías recargables o paneles solares. Así, WildVision no solo observa la naturaleza, sino que también la respeta.
# 3. Objetivos del Proyecto




# 4. Requisitos Iniciales 



# 5. Diseño Preliminar

<img width="1280" height="290" alt="image" src="https://github.com/user-attachments/assets/b9e35859-a9e2-4888-bd11-1b0affd8ba40" />

El sistema está centrado en un ESP32-CAM, que actúa como microcontrolador y cámara. Se conecta a sensores (como el de temperatura y un comedero dividido) y utiliza un módulo WiFi para enviar imágenes y datos a una API de inteligencia artificial, la cual analiza las especies de aves. La información puede almacenarse localmente o enviarse a una aplicación web o móvil para que el usuario final visualice los resultados.



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
