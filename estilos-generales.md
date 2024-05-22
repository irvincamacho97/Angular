**Estilos generales.**

Los estilo generales surgen de no repetir código *css*, de una mayor flexibilidad a cambios.

Imagina que tienes un proyecto angular donde los colores y estilos de botones, notificaciones, etiquetas, etc
lo repetiras N veces en distintas vistas de un proyeto.

Se tiene un Mock donde se tiene que registrar los datos un libro.

<img src="https://github.com/irvincamacho97/Angular/assets/132851888/7b419821-dd41-4380-ba44-e98c39faa8ba"> </img>

Nota como se tiene colores asignados dode te pediran que:

   - Cada botón de repuesta positiva cambiara el icono pero el *color* lo compatiran todas las vistas.
   - Cada botón de respuesta negativa cambiara el icono pero el *color* lo compatiran todas la vistas.
   - Cada etiqueta que acompaña un input tienen el mismo estilo y solo los obligatorios tendra un icono *.
   - Fondo de aplicación que tendran todas las vistas.

**Asignación de los estilo generales acorde a los requerimientos**

Al ya tener un proyecto *Angular* automaticamente se genera un archivo *style.css*, este archivo ya lo comparten todos los componentes que se 
crearan en un futuro.

![Captura de pantalla de 2024-05-22 17-32-28](https://github.com/irvincamacho97/Angular/assets/132851888/92fc3f26-ff7d-4662-b147-d14348a27263)

Primero en el archivo styles.css se declararan las variables de color a nivel raiz del proyecto, con el objetivo se priorizar los colores identificativos de la aplicación web

el código a ingresar es el siguiente:

```css

:root{
    --verde-base: #BBDB9B;
    --rojo-base: #9d2449;
    --blanco: #fff;
}

```
