# Redirect (redirect:)

- Redirige al usuario a otra pagina (vista)
- Es una redirección real, del lado del cliente. Spring responde con un 302 (o 303) y un header Location, y es el navegador el que hace una nueva petición GET a esa URL. Como resultado sí ves el cambio de URL en la barra del navegador, y es un request completamente nuevo (se pierden los atributos del Model anterior).

# Forward (forward:)

- Es una redirección interna, del lado del servidor. 
- El servlet container simplemente transfiere el control a otra ruta sin que el navegador se entere: no hay un segundo round-trip HTTP, la URL en el navegador no cambia, y se conserva el mismo request (con sus atributos, parámetros, etc.).

- Se mantiene en la misma ruta


# RedirectAttributes