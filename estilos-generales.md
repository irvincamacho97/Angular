**Estilos generales.**

Los estilos generales surgen de no repetir código *css*, de una mayor flexibilidad a cambios.

Imagina que tienes un proyecto angular donde los colores y estilos de botones, notificaciones, etiquetas, etc
lo repetirá N veces en distintas vistas de un proyecto.

Se tiene un Mock donde se tiene que registrar los datos de un libro.

<img src="https://github.com/irvincamacho97/Angular/assets/132851888/7b419821-dd41-4380-ba44-e98c39faa8ba"> </img>

Nota como se tiene colores asignados donde te pedirán que:

   - Cada botón de respuesta positiva cambiará el icono pero el *color* lo compartirán todas las vistas.
   - Cada botón de respuesta negativa cambiara el icono pero el *color* lo compartirán todas la vistas.
   - Cada etiqueta que acompaña un input tienen el mismo estilo y solo los obligatorios tendrá un icono *.
   - Fondo de aplicación que tendrán todas las vistas.

**Asignación de los estilo generales acorde a los requerimientos**

Al ya tener un proyecto *Angular* automáticamente se genera un archivo *style.css*, este archivo ya lo comparten todos los componentes que se 
crearán en un futuro.

![Captura de pantalla de 2024-05-22 17-32-28](https://github.com/irvincamacho97/Angular/assets/132851888/92fc3f26-ff7d-4662-b147-d14348a27263)

Primero en el archivo styles.css se declaran las variables de los colores nativos de lo que será la aplicación a nivel raíz del proyecto, con el objetivo se priorizar los colores identificativos de la aplicación web

el código a ingresar es el siguiente:

```css

:root{
    --verde-base: #BBDB9B;
    --rojo-base: #9d2449;
    --blanco: #fff;
}

```
Seguido se declara el estilo del botón con el siguiente código:

```css
.btn {
    border: none;
    font-size: 18px;
    border-radius:  3px;
    padding: 10px 25px;
}
```

El código anterior notarás que es un estilo genérico para todos lo botones que se le asignan *btn* para los botones positivos y negativos, se adentrará más.

Ahora declaramos los estilos para el botń positivo, lo estilo principales son:
   - Estilo genérico para toda la parte positiva.
   - Estilo *focus* para el momento de dar clic.
   - Estilo *hover* para el momento de sobreponer el indicar en el botón.

Dentro del estilo genérico se declara el color, el borde, color de letras o contenido y el color del borde, estas declaraciones serán con las variables root primeramente declaradas.

```css
    .btn-positivo{
    background-color: var(--verde-base);
    color: var(--blanco);
    border-color: var(--verde-base);
    }

```

Seguido la declaración del focus:
```css
.btn-positivo:focus{
    background-color: var(--verde-base);
    color: var(--blanco);
    border-color: var(--verde-base);
}

```

y por ultimo, el estilo de hover:

```css
.btn-positivo:hover{
    background-color: var(--verde-base);
    color: var(--blanco);
    border-color: var(--verde-base);
}
```

>Nota: se debe instalar bootstrap, para esto se ingresa @import "../node_modules/bootstrap/dist/css/bootstrap.min.css" en el mismo archivo y correr el comando npm i --force

**Solo queda comprobarlo consumiendolo en un html**

```html
            <div class="col-md-4">
                <button class="btn btn-positivo" 
                (click)="mapperToLibroRequest()">
                    Guardar
                </button>
            </div>

```
