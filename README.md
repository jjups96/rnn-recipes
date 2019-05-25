# Etiquetando sin etiquetas explicitas.

## Intro
<div style="text-align: justify">
Este proyecto empezó con la idea de etiquetar texto, pero rápidamente me di cuenta que la parte más tediosa e importante del proyecto era encontrar una buena base de información con textos preprocesados y ya etiquetados. Fue entonces cuando me interese en en la idea de aprovechar las estructuras de información disponibles para así construir una aplicación de PLN sin datos explícitamente etiquetados. Para ello hacemos uso del dataset de recetas en <a href="https://www.kaggle.com/hugodarwood/epirecipes">ingles de kaggle</a>, al cual le aplicamos etiquetado de sequencias.
</div>

## Etiquetado de sequencias
<div style="text-align: justify">
En el aprendizaje automático, el etiquetado de secuencias es un tipo de tarea de reconocimiento de patrones que implica la asignación algorítmica de una etiqueta categórica a cada miembro de una secuencia de valores observados.

La mayoría de los algoritmos de etiquetado de secuencias "clásicos" son de naturaleza probabilística, confiando en la inferencia estadística para encontrar la mejor secuencia. Los modelos estadísticos más comunes en uso para el etiquetado de secuencias hacen una suposición de Markov, es decir, que la elección de la etiqueta para una palabra en particular depende directamente sólo de las etiquetas inmediatamente adyacentes; De ahí que el conjunto de etiquetas forme una cadena de Markov. Esto conduce naturalmente al modelo oculto de Markov (HMM), uno de los modelos estadísticos más comunes utilizados para el etiquetado de secuencias.

Por ello, el uso de LSTM debería ser una mejora considerable
</div>



