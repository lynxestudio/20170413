# Publicación de archivos con ASP.NET en Linux y Apache con HtmlInputFile Server Control y en Windows con FileUpload Server Control

Una de las operaciones más frecuentes de las aplicaciones web (Web applications) es la carga de archivos (file upload), que es la acción donde el usuario puede seleccionar un archivo de su computadora y mediante el uso de un formulario en una página web puede transferir el archivo desde el sistema de archivos local hacia el sistema de archivos del servidor donde se encuentre la página web.

ASP .NET tiene dos formas de hacer esta funcionalidad, una mediante un control HtmlInputFile la cual se ha soportado desde el Framework 1.1 y la otra mediante un control FileUpload la cual se soporta a partir del Framework 2.0. Para este mini tutorial mostraré dos ejemplos de la implementación de la publicación de documentos con ASP .NET, el primero utilizando el control HtmlInputFile y el segundo utilizando el control FileUpload.

Fig 1. El código ASP.NET del ejemplo con un control HtmlInputFile.



Fig 2. El código C# del ejemplo con un control HtmlInputFile.




En este segundo ejemplo muestro la implementación del FileUpload server control ASP.NET (este control se encuentra disponible desde la versión 2.0 del Framework) con el siguiente código.

Fig 3. El código ASP.NET del ejemplo con server control FileUpload.



Fig 4. El código C# del ejemplo con server control FileUpload.



Para ambos casos el archivo de configuración web.config es el mismo, lo que cambia en ambos casos es el valor de la llave publishPath dentro de las etiquetas

<appSettings>
, ya que para el Ejemplo 1.0 que lo ejecutaremos en Linux utilizando Apache y Mono.
Por lo que el valor se establece en /home/martin/public_html/Downloads como se muestra en el web.config a continuación:
Fig 5. Archivo Web.config para la implementación en Apache bajo Linux



En el caso del ejemplo 2.0 lo ejecutaremos en IIS 7.0 y Windows. Por lo que el valor se establece como se muestra en el web.config a continuación:


Fig 6. Archivo web.config para la implementación en IIS



Para asignar el valor de la variable publishPath en el ejemplo 1.0 con la siguiente línea:
    string publishPath = ConfigurationSettings.AppSettings["publishPath"];
y para el ejemplo 2.0 se asigna con la siguiente línea:

    string publishPath = System.Configuration.ConfigurationManager.AppSettings["publishPath"];
Antes de ejecutar la página en Linux debemos habilitar con los permisos de escritura al directorio donde se guardaran los archivos, para hacerlo en Linux siga los siguientes pasos:
El directorio donde se subiran los archivos debe tener los permisos de escritura para el grupo www.
Si estamos como usuarios normales, cambiamos a una cuenta con privilegios de superusuario y establecemos y establecemos el grupo del directorio (en este ejemplo /home/martin/public_html/Downloads) como www con el siguiente comando:
            # chgrp www Downloads
        
Ponemos permisos de escritura para el grupo con el siguiente comando:
            # chmod 775 Downloads
        
Estos comandos se muestran a continuación en la siguiente imagen.

Fig 7. Comandos para establecer los permisos del directorio



Ahora configuramos la Web Application dentro del archivo httpd.conf del servidor Apache como se muestra en la siguiente imagen:

Fig 8. Configuración de la Web Application en Apache



Para consultar como crear una aplicación Web en Apache revisar el siguiente documento: ASP.NET con Mono

Al ejecutar la página ASP.NET desde FireFox en Linux se mostrará como en las siguientes imágenes:

Fig 9. Seleccionando el archivo para publicar desde Firefox en Linux



Fig 10. Publicando el archivo desde Firefox en Linux



De la misma manera a como se hizo en el ejemplo anterior, antes de ejecutar la página en Windows debemos de habilitar con los permisos necesarios el directorio del servidor donde vamos a guardar los documentos, en este ejemplo la ruta configurada como directorio de publicación es C:\inetpub\wwwroot\dowloads, por lo que debemos seguir los siguientes pasos:
Hacer click derecho sobre el directorio, en la ventana Properties seleccionar al usuario IIS_IUSRS (el cuál es el usuario anónimo de IIS), presionar el botón “Edit” para abrir la ventana Permissions.
Fig 11. Seleccionando los permisos para el usuario de IIS


En la ventana de Permissions en la lista inferior Permissions for IIS_IUSRS seleccionar la casilla con el permiso Write
Fig 12. Estableciendo los permisos de escritura


Creamos la Web Application de forma que se muestre en IIS 7.0 como en la siguiente imagen:
Fig 13. Vista de la web application en IIS Manager



Al ejecutar la página ASP.NET desde Internet Explorer se verá como en las siguientes imágenes:
Fig 14. Seleccionando el archivo desde IE en Windows



Fig 15. Publicando el archivo desde IE en Windows



Un error frecuente tanto para la implementación con HtmlInputFile y FileUpload, es tratar de subir un archivo que tenga un tamaño mayor al máximo permitido por la petición al servidor Web (Request)
Fig 16. Excepción al exceder el límite máximo de la petición.




Para solucionar este error solo hay que agregar la siguiente línea al archivo de configuración, donde definimos el tamaño máximo en Kb del archivo que necesitamos publicar. Por ejemplo en el siguiente web.config se definió un tamaño máximo aproximado a 5 mbs (5120 kb).
Fig 17. El archivo web config estableciendo el tamaño máximo del archivo a publicar.




Para más información sobre este error consultar este enlace: HttpException Maximum Request Length.

Descarga el proyecto con HtmlInputFile (Framework 1.1)

Descarga el proyecto con FileUpload (Framework 2.0)
Descarga el código fuente inline del ejemplo con HtmlInputFile (Framework 1.1)

Descarga el código fuente inline del ejemplo con FileUpload (Framework 2.0)
