# AnalisisDA

Este script en Python hace un análisis de los usuarios dados de alta en un directorio activo. Para ello previamente se deben haber volcado los usuarios a un fichero .csv. El resultado es un fichero Excel donde cada pestaña contendrá los usuarios que cumplen determinadas reglas.


# Directorios

- **Code**: Contiene el código del script
- **Config**: Contiene un fichero con nombres de usuarios genéricos para identificar el uso de estos usuarios en el directorio activo.
	> **Nota**: Este fichero no está completo.
	
- **Input**: En esta carpeta se deben introducir el/los archivos .csv que se quieren analizar
- **Output**: En esta carpeta se almacenarán el/los ficheros Excel (.xlsx) con los resultados.

# Input

Para obtener el fichero .csv con el listado completo de usuarios dados de alta en el directorio activo junto con sus  atributos que servirá como input al script, se debe ejecutar en el sistema a analizar el siguiente comando:
```
csvde -f <<nombrefichero>>.csv -r ObjectClass=user
```
En la carpeta input se pueden colocar tantos .csv como se quiera, por cada uno de ellos se generará un fichero de output con el mismo nombre que el de input pero con extensión .xlsx

# Output

El fichero de output consiste en un fichero Excel (.xlsx) con 10 pestañas, en cada una de ellas se listaran los usuarios que cumplan una serie de condiciones:
- **Usuarios**: Todos los usuarios junto con sus atributos.
- **Deshabilitados-Bloqueados**: Usuarios que se encuentran deshabilitados o bloqueados.
- **Contraseña no requerida**: Usuarios a los que no se les requiere contraseña para hacer login. (userAccountControl flag: UF_PASSWD_NOTREQD)
- **Contraseña no expira**: Usuarios a los que no les expira nunca la contraseña. (userAccountControl flag: UF_DONT_EXPIRE_PASSWD)
- **Usuarios inactivos**: Usuarios que no han hecho login en los últimos 150 días (aprox. 5 meses). Esta variable se puede editar en el código modificando el valor de la variable *inactivityTime*.
-  **Nunca han accedido**: Usuarios que están dados de alta en el directorio activo pero que sin embargo nunca han hecho login.
- **No cambio contraseña**: Usuarios que no han cambiado la contraseña en un tiempo superior al definido en la política de contraseñas (por defecto establecido a 42 días). Esta variable se puede editar en el código modificando el valor de la variable *maxPasswordAge*.
- **Acceso y no cambio pass**: Usuarios que no han cambiado la contraseña en un tiempo superior al definido en la política de contraseñas (por defecto establecido a 42 días) pero que si que han hecho login en este periodo, y por tanto están incumpliendo la política de contraseñas definida.
- **Usuarios administradores**: Usuarios que son administradores del dominio. 
- **Usuarios Genéricos**: Usuarios con un nombre de usuario susceptible de ser genérico. Esta última pestaña requiere trabajo manual ya que los resultados se basan en el contenido del fichero *config/common-usernames.txt*, el cual se debe ir complentando.

> **Nota**: Se pueden consultar todos los userAccountControl flags y su significado [aquí](http://www.selfadsi.org/ads-attributes/user-userAccountControl.htm)
