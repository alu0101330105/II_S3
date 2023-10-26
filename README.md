## Interfaces Inteligentes:
### Seminario 03

# Introducción a la programación de gráficos 3D.

- Raimon Mejías Hernández
(alu0101390161@ull.edu.es)

- Jorge González Delgado
(alu0101330105@ull.edu.es)

- Saúl Sosa Díaz
(alu0101404141@ull.edu.es)

## Ejercicio 01
### Pregunta:
¿Qué funciones se pueden usar en los scripts de Unity para llevar a cabo traslaciones, rotaciones y escalados?
### Respuesta:
Las transformaciones se pueden interpretar como cambios del sistema de referencia, los objetos se transforman multiplicando cada vértice por la matriz de transformación.
Ahora bien, para poder modificar las transformaciones en unity 
debemos hacer una distinción entre los objetos normales y los físicos.
Para mover los objetos sin físicas podemos actuar directamente en su transform pero para los físicos debemos usar el rigidbody.

- Objetos sin físicas:
  - Trasladar:
  `transform.translate(Vector3 translation);`
  - Rotar: 
  `transform.Rotate(Vector3 eulerAngles);`
  - Escalar:
  `transform.localScale = new Vector3(scale);`

- Objetos con físicas:
  - Trasladar: 
  - Hay varias formas de trasladar un objeto físico 
    - `Rigidbody.addForce(Vector3 translation);`
    - `Rigidbody.MovePosition(Vector3 position);`
  - Rotar: 
  `Rigidbody.AddTorque(Vector3 eulerAngles);`
  - Escalar:
  Los objetos físicos no se escalan directamente como los objetos de juego normales. El escalamiento no tiene un efecto directo en la física del objeto. En su lugar, si se necesita cambiar el tamaño de un objeto físico, se debe ajustar su collider para que coincida con el nuevo tamaño del objeto.

## Ejercicio 02	
### Pregunta: 

¿Cómo trasladamos la cámara 2 metros en cada uno de los ejes y luego la rotas 30º alrededor del eje Y?. Rota la cámara alrededor del eje Y 30ª y desplaza 2 metros en cada uno de los ejes. ¿Obtendrías el mismo resultado en ambos casos?. Justifica el resultado.

### Respuesta: 

Para trasladar la cámara en cada eje se debe utilizar el componente Transform de la misma, en ella el atributo Position indica la posición de la cámara en la escena. modificar el vector Position y asignarle los valores (2, 2, 2) movería la cámara 2 metros en cada eje. 

Rotar la cámara es muy similar a trasladarla, utilizando el atributo Rotation del Transform permite rotar la cámara en el eje indicado, para rotar la cámara en el eje Y se debe asignar el siguiente vector: (0, 30, 0). Rotando así la cámara en 30º.

Realizar la traslación y la rotación en diferente orden no modifica el resultado. En cualquiera de los dos casos la cámara estará en la posición (2, 2, 2) rotada un ángulo de 30º.
## Ejercicio 03

### Pregunta:

Sitúa la esfera de radio 1 en el campo de visión de la cámara y configura un volumen de vista que la recorte parcialmente.

### Respuesta:

Cuando colocamos la esfera de forma que el plano cercano (modificado para exagerar el fenómeno) se encuentre intersectando parcialmente con la esfera, encontramos el suceso que se conoce como “near plane clipping” que es cuando podemos ver a través de objetos opacos ya que es como si la cámara, se hubiese colado dentro de éste. Ese fenómeno se apoya en el hecho de que unity hace Culling (no renderizar caras de objetos que no se van a ver como el interior de una esfera) de los objetos de la escena de forma predeterminada. El resultado es similar a observar un eclipse solar, pero con una esfera con número finito de polígonos.

## Ejercicio 04
### Pregunta:
Sitúa la esfera de radio 1 en el campo de visión de la cámara y configura el volumen de vista para que la deje fuera de la vista.
### Respuesta:
Para ello debemos modificar la pirámide truncada de la cámara, es decir el frustum en unity para poder modificar el volumen de este cuerpo geométrico debemos modificar en la cámara los parámetros Clipping planes modificando el near y el far.

## Ejercicio 05
### Pregunta: 

¿Cómo puedes aumentar el ángulo de la cámara?. ¿Qué efecto tiene disminuir el ángulo de la cámara?.

### Respuesta:

Para aumentar el ángulo de la cámara en Unity se debe aumentar el campo Field Of  View(FOV) dentro del Componente Camera del objeto.

Aumentar el ángulo de la cámara implica aumentar el valor del FOV, permitiendo así observar una mayor cantidad del espacio pero distorsionando la vista. A mayor FOV mayor será el efecto de ojo de pez, mientras que a menor FOV menor será el espacio visible y aparecerá un efecto de Zoom 
## Ejercicio 06
### Pregunta:

Es correcta la siguiente afirmación: Para realizar la proyección al espacio 2D, en el inspector de la cámara, cambiaremos el valor de projection, asignándole el valor de orthographic

### Respuesta:
Si, es correcta, cuando seleccionamos el valor de projection a orthographic, la pirámide truncada que representaba el volumen de visión de la cámara, pasa a ser un prisma rectangular, que en conclusión permite recrear una imágen en 2D sin perspectiva, donde la profundidad de los objetos no altera su tamaño aparente en la imágen rasterizada resultante. Como dato curioso, si además de cambiar el modo de proyección de la cámara, orientamos el punto de vista del editor en el eje Z, y cambiamos la proyección del editor a orthographic, obtendremos el mismo resultado que se consigue con un espacio de trabajo en 2D por defecto.

## Ejercicio 07
### Pregunta:
Especifica las rotaciones que se han indicado en los ejercicios previos con la utilidad quaternion.

### Respuesta:
Más concretamente del ejercicio 2.  Se puede hacer con el siguiente script:
```cs
// Crear quaterniones para trasladar y rotar
Quaternion translationQuaternion = Quaternion.Euler(2, 2, 2); //
Quaternion rotationQuaternion = Quaternion.Euler(0, 30, 0);


// Aplicar las rotaciones
transform.rotation *= rotationQuaternion;
transform.position += translationQuaternion * Vector3.forward;
```

## Ejercicio 08
### Pregunta: 

¿Como puedes averiguar la matriz de proyección en perspectiva que se ha usado para proyectar la escena al último frame renderizado?

### Respuesta: 

El componente Camera tiene un atributo llamado `previousViewProjectionMatrix` que almacena la matriz de proyección en perspectiva que se ha usado para proyectar la escena al último frame.
## Ejercicio 09
### Pregunta:
¿Cómo puedes averiguar la matriz de proyección en perspectiva ortográfica que se ha usado para proyectar la escena al último frame renderizado?

### Respuesta:
Si estás trabajando con una cámara en proyección ortográfica, la forma de obtener la matriz de perspectiva utilizada en el último frame, es de la misma forma que si estas trabajando con proyección en perspectiva, accediendo al atributo `previousViewProjectionMatrix` del componente camera.
## Ejercicio 10
### Pregunta:
¿Cómo puedes obtener la matriz de transformación entre el sistema de coordenadas local y el mundial?.
### Respuesta:
Para obtener el las matrices de transformación entre el sistema de coordenadas local y el mundial se deben usar los siguientes métodos:
[transform.localToWorldMatrix](https://docs.unity3d.com/ScriptReference/Transform-localToWorldMatrix.html)
[transform.worldToLocalMatrix](https://docs.unity3d.com/ScriptReference/Transform-worldToLocalMatrix.html)

Estas matrices permiten transformar las coordenadas de un punto en el espacio local de un objeto al espacio mundial o viceversa.
## Ejercicio 11
### Pregunta:
¿Cómo puedes obtener la matriz para cambiar al sistema de referencia de vista?

### Respuesta:

Utilizando la propiedad publica del componente Camera denominado: `worldToCameraMatrix`


## Ejercicio 12
### Pregunta:
Especifica la matriz de la proyección usado en un instante de la ejecución del ejercicio 1 de la práctica 1.

### Respuesta:
Gracias al atributo `previousViewProjectionMatrix` obtenemos desde el componente camera la siguiente matriz de perspectiva:

$$
\begin{array}{cc} 
1.31011 & 0.00000 & 0.00000 & -11.08320\\
0.00000 & -2.74748 & 0.00000 & 8.90544\\
0.00000 & 0.00000 & -0.00040 & 0.20126\\
0.00000 & 0.00000 & 1.00000 & -2.95121\\
\end{array}
$$

## Ejercicio 13
### Pregunta:
Especifica la matriz de modelo y vista de la escena del ejercicio 1 de la práctica 1.
### Respuesta:
En Unity, se puede acceder a la matriz de transformación compuesta del objeto y la cámara utilizando el componente de Transform del objeto y la cámara. La matriz de transformación compuesta se calcula automáticamente y se combina la matriz Modelo y la matriz Vista. Puedes acceder a esta matriz de transformación a través de la propiedad: 
`Matrix4x4 modelViewMatrix = transform.localToWorldMatrix;` 

si mostramos su contenido veremos la siguiente matriz: 

$$
\begin{array}{cc} 
1.00000 & 0.00000 & 0.00000 & 4.08685\\
0.00000 & 1.00000 & 0.00000 & -0.53971\\
0.00000 & 0.00000 & 1.00000 & 2.66508\\
0.00000 & 0.00000 & 0.00000 & 1.00000\\
\end{array}
$$



## Ejercicio 14
### Pregunta:

Aplica una rotación en el start de uno de los objetos de la escena y muestra la matriz de cambio al sistema de referencias mundial.

### Respuesta:

Utilizando el siguiente script:
```cs
void Start()
{
    transform.Rotate(0, 45, 0);
    Debug.Log(transform.localToWorldMatrix);
}
```


Se puede obtener la matriz de cambio al sistema de referencias mundial:

$$
\begin{array}{cc} 
0.70711	& 0.00000	& 0.70711	& 0.00000\\
0.00000	& 1.00000	& 0.00000	& 0.00000\\
-0.70711 & 0.00000 & 0.70711 & -0.68600\\
0.00000 & 0.00000 & 0 & 1.00000\\
\end{array}
$$


## Ejercicio 15
### Pregunta:
¿Como puedes calcular las coordenadas del sistema de referencia de un objeto con las siguientes propiedades del Transform:?: 
Position (3, 1, 1), Rotation (45, 0, 45)

### Respuesta:
Para obtener los vectores X Y Z del sistema de coordenadas local del objeto (los ejes de coordenadas) podemos acceder a los siguientes atributos del transform del objeto:
- `transform.forward` Eje Z (azul)
- `transform.up` Eje Y (verde)
- `transform.right` Eje X (rojo)
