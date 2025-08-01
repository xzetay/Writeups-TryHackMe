![1](./Pasted%20image%2020250616160151.png)

### NMAP

![2](./Pasted%20image%2020250616162920.png)

Nos podemos dar cuenta que los puertos 22, 80 y 443 están activos, por consiguiente buscaremos en un navegador como Firefox la IP.

### Examinar página web

![3](./Pasted%20image%2020250616164356.png)

Es un tipo de terminal en la cual hay comandos establecidos los cuales se pueden ejecutar y cada uno tiene un desenlace propio.

![4](./Pasted%20image%2020250616163552.png)

robots.txt = Sirve para decirle a los motores de búsqueda (como Google, Bing, etc.) qué partes de tu sitio web pueden o no pueden rastrear (indexar).

¿Para qué se usa exactamente?

1. Controlar el acceso de bots a tu sitio web.
2. Proteger áreas privadas o irrelevantes** de ser indexadas (como `/admin/`, `/cgi-bin/`, etc.).
3. Evitar sobrecargar el servidor con muchas peticiones automáticas.
4. Orientar a los bots hacia el archivo del mapa del sitio (`sitemap.xml`).

Se puede usar para ver rutas y directorios ocultos.

![5](./Pasted%20image%2020250616164356.png)

En este punto cuando los resultados de la búsqueda con robots.txt se me fue otorgada, empece a probar cada uno de ellos.

![6](./Pasted%20image%2020250616172641.png)

En el caso de /fsocity.dic se nos otorga un diccionario.

Y en el caso de /key-1-of-3.txt:

![7](./Pasted%20image%2020250616172823.png)

Ambos resultados se utilizarán posteriormente.
### Gobuster:

![8](./Pasted%20image%2020250616164716.png) 
![9](./Pasted%20image%2020250616164613.png)

Podemos visualizar que hay un wp-login.php el cual nos dirige a una página de logeo:

![10](./Pasted%20image%2020250616164803.png)

Usando Admin como Usuario y Admin y contraseña no se dará ningún resultado aparte del mensaje de Error.

![11](./Pasted%20image%2020250616172158.png)

Pero cuando agregaba nuevos usuarios para probar, el usuario Elliot dio un nuevo mensaje de error:

![12](./Pasted%20image%2020250616173025.png)

Con este mensaje se puede confirmar que el usuario vendría a ser Elliot, solo faltaría la contraseña.

Se usara BurpSite para interceptar el trpáfico de la máquina y ver las peticiones a más detalle.

![13](./Pasted%20image%2020250616171358.png)

Podemos darnos cuenta:

![14](./Pasted%20image%2020250616171630.png)

Aquí esta el segmento donde se especifica el Usuario y la Contraseña, con esto ya podremos utilizar Hydra para fuerza bruta.

### Hydra

Se usará Hydra para hallar la contraseña de Elliot

![15](./Pasted%20image%2020250616191551.png)

### Reverse Shell

Ahora estamos en el dashboard de Elliot:

![16](./Pasted%20image%2020250616180409.png)
 
En el apartado de Aparence logramos visualizar una opción la cual nos ayudará a subir una reverse shell php
 
![17](./Pasted%20image%2020250616180804.png)

Buscaremos una reverse shell para php y la copiaremos cambiando tanto nuestra IP como el Puerto.

![18](./Pasted%20image%2020250616181205.png)

Abriremos el puerto 1234 desde netcat para la reverse shell:

![19](./Pasted%20image%2020250616183646.png)

Esto aparecerá cuando lo que subamos sea exitoso:

![20](./Pasted%20image%2020250616182236.png)

Para ingresar con la reverse shell deberemos usar esa URL 

![21](./Pasted%20image%2020250616182254.png)

Ya estamos dentro!!!

![22](./Pasted%20image%2020250616183703.png)

Se logra visualizar en la carpeta home que hay 2 archivos y uno de ellos parece ser una contraseña

![23](./Pasted%20image%2020250616183921.png)

Efectivamente, es una hash en formato MD5.

![24](./Pasted%20image%2020250616184022.png)

### John the Ripper

Con john romperemos el hash.

![25](./Pasted%20image%2020250616184646.png)

Antes de continuar aremos que la shell sea más interactiva esto para cambiar de usuario a robot

![26](./Pasted%20image%2020250616184905.png)

Se ingresará al usuario robot con la contraseña ya hallada y luego hacer que nuestra shell sea más interactiva:

![27](./Pasted%20image%2020250616185238.png)

Ahora si podemos utilizar cat en el archivo key*

![28](./Pasted%20image%2020250616185603.png)

Ahora deberemos escalar al usuario root

Usando el comando find seguido de diferentes agregados podremos ver que binarios están configurados mal: find / -perm /6000 2>/dev/null | grep '/bin/'

![29](./Pasted%20image%2020250616190409.png)

Se busco permisos en el binario que se nos mostró, luego de ello buscamos en la página https://gtfobins.github.io/ nmap para ver como explotar ese binario:

![29](./Pasted%20image%2020250616190911.png)

Ahora solo pondremos la ruta de nmap y --interactive:

![29](./Pasted%20image%2020250616190741.png)

Somos root!!
Simplemente buscamos en el directorio root, listamos y listo:

![29](./Pasted%20image%2020250616191230.png)

---
