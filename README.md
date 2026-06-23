Rubik3D es un simulador avanzado del cubo de Rubik clásico de 3x3, desarrollado con tecnología nativa (C++ / OpenGL ES 3.0 / GameActivity) para un máximo rendimiento y fidelidad visual.

ESTADO ACTUAL:
- Motor gráfico: Renderer.cpp y RubikCube.cpp manejan la lógica de renderizado y rotación.
- Detección de Victoria: Implementada mediante inspección cromática de caras (isSolved). El juego se considera resuelto si las 6 caras son monocromáticas, permitiendo rotaciones de centros.
- UI: MainActivity.kt con controles overlay Material Design 3. Inmersión total (Fullscreen) mejorada para dispositivos modernos y manejo de notch.
- Sistema de Pausa: Bloqueo total de interacciones durante la pausa con feedback visual (blink del timer).
- Lógica Competitiva: El modo competitivo y el cronómetro SOLO se activan tras pulsar "Scramble". Los movimientos manuales antes de esto se consideran "Juego Libre" (tiempo en 00:00).
- Contador Scramble: Inicia en -20 durante la mezcla para que la partida del usuario comience exactamente en el movimiento 0.
- Historial Visual: Representación con iconos de cuadrícula y flechas de rotación intuitivas (CW/CCW).
- Mapeo de Colores Oficial: Z+(Blanco), Z-(Amarillo), X+(Rojo), X-(Naranja), Y+(Azul), Y-(Verde).
- Configuración: Modo Debug e Inercia desactivados por defecto. Scoreboard accesible desde Settings.
- Visual: Temas neón, carbón, cristal, madera y metal. Scoreboard profesional con tarjetas detalladas.
- Legal & Privacidad: Nueva sección "Privacy & Open Source" en la pantalla About, diseñada como una tarjeta profesional con política de privacidad "Zero Data Collection" y créditos detallados para librerías Apache 2.0.

<table>
  <tr>
    <td><img src="https://raw.githubusercontent.com/Wardenclyffe00/rubik3d/refs/heads/main/Screenshot_20260623_180310.png" width="300" alt="Img 1"></td>
    <td><img src="https://raw.githubusercontent.com/Wardenclyffe00/rubik3d/refs/heads/main/Screenshot_20260623_180348.png" width="300" alt="Img 2"></td>
    <td><img src="https://raw.githubusercontent.com/Wardenclyffe00/rubik3d/refs/heads/main/Screenshot_20260623_180552.png" width="300" alt="Img 3"></td>
    <td><img src="https://raw.githubusercontent.com/Wardenclyffe00/rubik3d/refs/heads/main/Screenshot_20260623_180447.png" width="300" alt="Img 4"></td>
    <td><img src="https://raw.githubusercontent.com/Wardenclyffe00/rubik3d/refs/heads/main/Screenshot_20260623_180507.png" width="300" alt="Img 5"></td>
  </tr>
</table>

MEJORAS RECIENTES:
- Privacidad: Confirmación de que la app no recolecta datos, no usa internet y no requiere permisos.
- UI: Rediseño de la sección de licencias en About, pasando de un link de texto a una tarjeta interactiva profesional.
- Seguridad: Inhabilitación de la cola de movimientos y rotación de cámara durante la pausa.
- Estética: Unificación de temas entre todas las pantallas secundarias (Settings, Scoreboard, About, Licenses).

LA MATEMÁTICA DETRÁS DE LA MAGIA:
1. Rotaciones Matriciales: En lugar de ángulos de Euler (sujetos a Gimbal Lock), el motor utiliza matrices de transformación 4x4. Cada cubito mantiene su propia matriz de estado que se multiplica por una matriz de rotación incremental (90°) tras cada movimiento.
2. Jerarquía Espacial: El renderizado aplica una "Whole Cube Matrix" (rotación global de cámara/usuario) sobre la "Local Cubie Matrix", permitiendo que el usuario gire el cubo entero mientras las piezas internas mantienen su lógica de movimiento.
3. Proyección de Vectores: Para convertir un deslizamiento 2D en pantalla a un giro 3D, el motor proyecta la normal de la cara tocada y calcula el producto punto (dot product) entre el vector de movimiento del dedo y los vectores tangentes proyectados de los ejes posibles (X, Y, Z). El eje con mayor puntaje de correlación es el seleccionado.
4. Detección Cromática (isSolved): El algoritmo no compara matrices (evitando falsos negativos por rotación de centros), sino que lanza vectores normales desde el centro de cada cara del mundo (-X a +X, etc.). Identifica qué pegatinas (stickers) colisionan con estas normales y verifica que el 100% de las pegatinas en esa cara compartan el mismo valor RGB.
5. Snap-to-Grid: Para evitar derivas numéricas por precisión de punto flotante, después de cada rotación de 90°, las matrices de los cubitos se "limpian" (snap) forzando los valores cercanos a 0, 1 o -1, garantizando estabilidad infinita en el motor.
