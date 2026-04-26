# Week10_10


## Explicación del Sistema de Reconocimiento Facial

Este código implementa un pipeline de aprendizaje métrico (*Metric Learning*) diseñado para que el modelo aprenda a distinguir identidades basándose en la proximidad en un espacio vectorial.

### 1. Preparación de Datos (Tripletes)
En lugar de clasificar imágenes en categorías fijas, utilizamos **tripletes**. Para cada paso, el modelo recibe:
*   **Anchor (Ancla)**: Rostro de la persona A.
*   **Positive (Positivo)**: Otro rostro diferente de la misma persona A.
*   **Negative (Negativo)**: Rostro de una persona B diferente.

### 2. Arquitectura: Red Base y Triplet Loss
*   **Red Base (Encoder)**: Es una CNN que actúa como un extractor de características, comprimiendo la imagen en un vector de 128 números (embedding).
*   **Triplet Loss**: Esta función de pérdida es el corazón del modelo. Su fórmula busca que:
    $$Distance(A, P) + Margin < Distance(A, N)$$
    Es decir, obliga a que las fotos de la misma persona estén mucho más cerca entre sí que las de personas diferentes.

### 3. Entrenamiento y Evaluación
*   El modelo no se entrena para predecir un nombre, sino para optimizar distancias.
*   **PCA (Principal Component Analysis)**: Como los humanos no podemos visualizar 128 dimensiones, usamos PCA para proyectar esos vectores a un plano 2D. Si el entrenamiento es exitoso, veremos que los puntos del mismo color (misma persona) tienden a agruparse en regiones específicas del gráfico.

  ## Conclusiones y Observaciones

Tras el desarrollo y entrenamiento del sistema de reconocimiento facial, se pueden extraer las siguientes conclusiones:

1. **Eficacia del Triplet Loss**: A diferencia de la red siamesa simple (que mostró una precisión cercana al azar del 50% en las primeras pruebas), el entrenamiento con tripletes permite un aprendizaje métrico más fino al comparar simultáneamente ejemplos positivos y negativos frente a un ancla.

2. **Agrupamiento en el Espacio Latente**: La visualización mediante **PCA** revela cómo el modelo intenta agrupar rostros de la misma identidad. Sin embargo, dada la complejidad del dataset LFW y el tamaño limitado de nuestra red base, los grupos pueden presentar solapamientos.

3. **Limitaciones Actuales**: El modelo se ha entrenado sobre un subconjunto reducido. Para obtener resultados de nivel producción, se recomendaría:
    *   **Data Augmentation**: Aumentar la variedad de los datos mediante rotaciones y cambios de iluminación.
    *   **Transfer Learning**: Utilizar modelos pre-entrenados como ResNet o FaceNet como 'red base' (backbone).
    *   **Hard Triplet Mining**: Seleccionar los tripletes más difíciles durante el entrenamiento para forzar al modelo a aprender características más distintivas.
