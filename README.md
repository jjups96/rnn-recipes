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

## El conjunto de datos
<div style="text-align: justify">
Aunque el conjunto de datos consiste de información variada, como las calorías; como buscamos etiquetar ingredientes en recetas, solo necesitaremos las columnas de instrucciones e ingredientes. Si alguna vez han seguido una receta se pueden imaginar a dónde vamos. Como se dijo anteriormente lo que buscamos es aprovechar la estructura de las páginas web de recetas para crear un vocabulario y así etiquetar los ingredientes en las recetas.
 
Después de una tokenización, y obtención del vocabulario de recetas, pasamos a lo que no interesa: La estructura de los ingredientes.
</div>

### * 1 arreglo bien tokenizado y filtrado.
<div style="text-align: justify">
Una receta se guarda en un arreglo de cadenas, al concatener dichas cadenas obtenemos un parrafo. 
</div>
```
1. Place the stock, lentils, celery, carrot, thyme, and salt in a medium saucepan and bring to a boil. Reduce heat to low and simmer until the lentils are tender, about 30 minutes, depending on the lentils. (If they begin to dry out, add water as needed.) Remove and discard the thyme. Drain and transfer the mixture to a bowl; let cool. 2. Fold in the tomato, apple, lemon juice, and olive oil. Season with the pepper. 3. To assemble a wrap, place 1 lavash sheet on a clean work surface. Spread some of the lentil mixture on the end nearest you, leaving a 1-inch border. Top with several slices of turkey, then some of the lettuce. Roll up the lavash, slice crosswise, and serve. If using tortillas, spread the lentils in the center, top with the turkey and lettuce, and fold up the bottom, left side, and right side before rolling away from you.
```
<div style="text-align: justify">
Nuestros datos no estan etiquetados, pero podemos obetener algunos de los ingredientes que se mencionan en la receta usando el listado de ingredientes que acompana a la receta.
</div>
```
['4 cups low-sodium vegetable or chicken stock',
 '1 cup dried brown lentils',
 '1/2 cup dried French green lentils',
 '2 stalks celery, chopped',
 '1 large carrot, peeled and chopped',
 '1 sprig fresh thyme',
 '1 teaspoon kosher salt',
 '1 medium tomato, cored, seeded, and diced',
 '1 small Fuji apple, cored and diced',
 '1 tablespoon freshly squeezed lemon juice',
 '2 teaspoons extra-virgin olive oil',
 'Freshly ground black pepper to taste',
 '3 sheets whole-wheat lavash, cut in half crosswise, or 6 (12-inch) flour tortillas',
 '3/4 pound turkey breast, thinly sliced',
 '1/2 head Bibb lettuce']
```
<div style="text-align: justify">
Primero limpiamos el texto de caracteres no deseado.
</div>
```python
def _clean(text):
    text = text.replace("(", "")
    text = text.split("/")[0]
    return text
```
<div style="text-align: justify">
Para despues tokenizar la lista de ingredientes y despues filtrar las palabras que nos interesan, como los numeros y las stop words.
</div>
```python
def _filter(token):
    if len(token) < 2:
        return False
    if token.is_stop:
        return False
    if token.is_digit:
        return False
    if token.like_num:
        return False
    return True
```
<div style="text-align: justify">
Sin embargo, esto no es suficiente para solo etiquetar ingredientes y terminaremos etiquetando el lenguaje de cocina. Por ejemplo, nuestro filtro no hace distincion entre las palabras <b>pound turkey, breast, thinly, sliced</b>, por lo que al moento de etiquetar la receta tomara la sentencia: Top with several <b>slices</b> of <b>turkey</b>, then some of the <b>lettuce</b>. En lugar del deseado: Top with several slices of <b>turkey</b>, then some of the <b>lettuce</b> 
</div>
