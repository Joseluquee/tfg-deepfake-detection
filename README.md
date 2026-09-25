# TFG Detección Multimodal de Deepfakes

Este repositorio contiene mi Trabajo de Fin de Grado en Ingeniería Informática por la Universidad de Málaga. El objetivo es construir desde cero un modelo ligero capaz de detectar deepfakes combinando dos señales que normalmente se analizan por separado, el movimiento de los labios en el vídeo y el espectrograma del audio, buscando las inconsistencias de sincronización labial que delatan a un vídeo manipulado.

La idea es sencilla. Cuando alguien genera un deepfake, suele acertar bastante bien con la imagen o con el audio por separado, pero rara vez consigue que ambos encajen perfectamente en el tiempo. Este proyecto explota justamente ese desajuste, entrenando dos ramas independientes (una visual y otra de audio) cuyos resultados se fusionan para dar una predicción final de si el vídeo es real o falso.

El modelo está pensado para entrenarse en una sola GPU de consumo, en concreto una RTX 3050 con 6 GB de memoria, así que se prioriza una arquitectura razonable en tamaño frente a soluciones más pesadas basadas en transformers.

Por ahora el repositorio solo tiene montado el esqueleto de carpetas del proyecto. La implementación todavía no ha empezado.

## Estructura del proyecto

En `data/` vive todo lo relacionado con los datos. Dentro, `raw/` guarda los datasets originales sin tocar, cada uno en su propia carpeta. `AVLips/` es el dataset principal, con los vídeos reales y falsos separados y sus pistas de audio aparte, y `FakeAVCeleb_v1.2/` se usa para comprobar si el modelo generaliza a un dataset distinto. La carpeta `processed/` recoge la salida del preprocesado, es decir los frames de labios ya recortados, los espectrogramas mel calculados a partir del audio, y los splits de entrenamiento, validación y test.

Los datasets no se suben al repositorio porque pesan demasiado y sus licencias no permiten redistribuirlos, así que solo se versiona la estructura de carpetas. Para reproducir el proyecto hay que descargarlos de sus fuentes oficiales y descomprimirlos en `data/raw/`.

En `src/` está todo el código fuente, organizado por responsabilidad. Dentro de `data/` se define cómo se leen y preparan los datos para el entrenamiento. En `models/` viven las distintas piezas de la red, el encoder visual que procesa los labios, el encoder de audio que procesa el espectrograma, el módulo que fusiona ambas ramas y el modelo completo que las junta. La carpeta `training/` contiene el bucle de entrenamiento, la evaluación del modelo y las funciones de pérdida. Por último, `utils/` reúne utilidades comunes como la generación de gráficas y la configuración centralizada de hiperparámetros.

`notebooks/` guarda los cuadernos de Jupyter que se usan para explorar los datos, probar el preprocesado de forma interactiva y analizar los resultados una vez entrenado el modelo.

`checkpoints/` almacena los pesos del modelo que se van guardando durante el entrenamiento, y `results/` recoge las métricas, gráficas y tablas que salen de las distintas pruebas y experimentos.

## Estado actual

De momento el proyecto está en fase de planificación y organización inicial. Iré actualizando este README a medida que las distintas partes del pipeline (preprocesado, modelo, entrenamiento) se vayan implementando.

## Licencia

Este proyecto está bajo licencia MIT, ver el archivo LICENSE para más detalles.
