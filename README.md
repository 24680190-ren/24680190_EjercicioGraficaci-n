# 24680190_EjercicioGraficaci-n
<br>Crearemos una figura en 2D en un software llamdo Blender,para poder realizar el siguiente ejercicio debemos instalar el programa, así que será lo primero que hagamos.
para instalar Blender se necesita ir a la pagina oficial y desde la pastaña download descargar la versión más adecuada para tu sistema operativo.</br>
<img width="719" height="343" alt="image" src="https://github.com/user-attachments/assets/2d2f52f4-333f-4c65-8fbc-75cddfcecf95" />
<img width="1149" height="569" alt="image" src="https://github.com/user-attachments/assets/bdf935da-d50e-4850-9512-16033b5f2b41" />
<br>Debes esperar un poco luego de que haya terminado la descarga, ejecutalo y solo da clic en next. Cuando termine en la pantalla pricipal ve al apartado de **Scripting**, dentro de este colocaremos todo el codigo.</br>
<img width="2427" height="2037" alt="image" src="https://github.com/user-attachments/assets/7bd4036a-cc9b-4173-a29a-489f1d60ae8c" />
<br>Luego de esto podemos inicar con el código:</br>
```python
import bpy
import math
```
Estas lineas de código exportamos las librerias necesarias para llevar a cabo el programa:
+ by:libreria de blender bastante impottante ya que sin esta python no sabria las palabras reservadas de esta.
+ math: La necesitamos para usar funciones trigonométricas (seno y coseno) y el valor de $\pi$, esenciales para dibujar círculos y polígonos.
1. Definiremos la clase **crear_poligono_2d**en la que se encontraran los atributos de nuestro objeto (nombre,lados,radio)
<br>**malla:** La malla es la estructura básica que define la forma de casi cualquier objeto . Es el linezo en el que trazaremos nuetra figura, así que es de suma imprtancia craerla.</br>
<br>**objeto:** Es como un "contenedor" o una caja transparente. Tiene información sobre la posición, rotación y escala en el mundo.</br>
2. Para que el objeto sea visible agregamos **bpy.context.collection.objects.link(objeto)**
3. Seguimos con la definición de los vertices y aristas 
```python
def crear_poligono_2d(nombre, lados, radio):
    # Crear una nueva malla y un nuevo objeto
    malla = bpy.data.meshes.new(nombre)
    objeto = bpy.data.objects.new(nombre, malla)
    
    # Vincular el objeto a la escena actual
    bpy.context.collection.objects.link(objeto)
    
    vertices = []
    aristas = []
```
Dentro de un for vamos a determinar los vertices
1. Multiplicamos por dos pi  **(math.pi)** por i y lo dividimos entre los lados, asigando un valor  a la variable angulo.
2. Cordenada en x: multiplicamos radio por seno **(math.cos)** del angulo antes calaculado.
3. Cordenada en y: multiplicamos radio por seno **(math.sin)** del angulo antes calaculado.
```python
# Cálculo de vértices usando coordenadas polares a cartesianas
    for i in range(lados):
        angulo = 2 * math.pi * i / lados
        x = radio * math.cos(angulo)
        y = radio * math.sin(angulo)
        vertices.append((x, y, 0)) # Z = 0 para mantenerlo en 2D
```
Con el **for** anterior ya tenemos los veritces flotando en el espacio, así que ahora les daremos una forma.
Ya tenemos creados los puntos (anticipando que seran 6 ya que será un hexagono) para unir los vertices necesitamos dibujar lineas entres estos.
1. El ciclo como mencionamos se repetira 6 veces pero iniciara de 0-5
2. En una lista vamos a guardar la pareja de puntos que deben conectarse. __(El append significa añadir)__
3. Para unir lo spuntos es de suma importancia agregar __(i + 1) % lados__ ya que como se inicia en 0 en la ultima vuelta de ciclo intentara unir (5,6) y el punto 6 no existe, entonces al agregar % lados nos da como ultimo punto (5,0)
4. __malla.from_pydata(...)__:Esta es la línea más potente del script. Es el momento donde pasamos de "números en Python" a "objetos en Blender".Dentro de esta se encuentra la lista de vértices, aristas es la lisat de conexiones de puntos.
```python
# Definir las conexiones (aristas) entre los vértices
    for i in range(lados):
        aristas.append((i, (i + 1) % lados))
        
    # Cargar los datos en la malla
    malla.from_pydata(vertices, aristas, [])
    malla.update()
```
<br>La primer linea de codigo nos ayuda a limpiar la pantalla, ya que al iniciar podemos observar un cubo, luz y otros elementos que al crear uno nuevo nos podrian generar algun problema.</br>
<br>En el caso contrario esta la linea del  delete nos permite que al tocar la tecla x o suprimir se elimine el elemento seleccionado.</br>
Al final observamos como llamar a la funcion encargada de ejecutar el codigo, asi mismo en esta se determinan los valores dle radio, nombre de la figura y el número de lados. 

```python
# Limpiar la escena antes de empezar
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.delete()
#llamad a la función
crear_poligono_2d("Poligono2D", lados=6, radio=5)
```
A continuación se muestra el codigo completo: 
```python
import bpy
import math

def crear_poligono_2d(nombre, lados, radio):
    # Crear una nueva malla y un nuevo objeto
    malla = bpy.data.meshes.new(nombre)
    objeto = bpy.data.objects.new(nombre, malla)
    
    # Vincular el objeto a la escena actual
    bpy.context.collection.objects.link(objeto)
    
    vertices = []
    aristas = []
    
    # Cálculo de vértices usando coordenadas polares a cartesianas
    for i in range(lados):
        angulo = 2 * math.pi * i / lados
        x = radio * math.cos(angulo)
        y = radio * math.sin(angulo)
        vertices.append((x, y, 0)) # Z = 0 para mantenerlo en 2D
        
    # Definir las conexiones (aristas) entre los vértices
    for i in range(lados):
        aristas.append((i, (i + 1) % lados))
        
    # Cargar los datos en la malla
    malla.from_pydata(vertices, aristas, [])
    malla.update()

# Limpiar la escena antes de empezar
bpy.ops.object.select_all(action='SELECT')
bpy.ops.object.delete()

# Llamada a la función: Un hexágono de radio 5
crear_poligono_2d("Poligono2D", lados=6, radio=5)
```
Vizualización del codigo ejecutado 

