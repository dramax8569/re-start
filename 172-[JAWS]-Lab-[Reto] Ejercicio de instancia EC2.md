# Desafío de Laboratorio 1: Ejercicio de Instancia EC2

**Duración:**
El tiempo estimado para completar este laboratorio es de aproximadamente 55 minutos.

# Desafío de Laboratorio - Instancia Amazon EC2 y Servidor Web

**Su desafío es el siguiente:**

*Cree una instancia de Amazon Linux EC2 para ejecutar una aplicación web sencilla. Los requisitos generales son los siguientes:
    - Utilice la consola de AWS para iniciar la instancia.
    - Utilice una AMI de Amazon Linux y un tipo de instancia T3 con un tamaño inferior al mediano.
    - Lance la instancia en una nueva VPC y una nueva subred y asigne automáticamente su dirección IP pública.
    - Al crear la instancia, en los datos de usuario, instale e inicie el servicio httpd como su servidor web. Proporcione permiso de escritura a los usuarios en el directorio raíz de documentos del servidor web (/var/www/html).
    - Utilice un tipo de volumen SSD de uso general (gp2) para el volumen raíz.
    - Configure la instancia y cree los recursos necesarios para permitirle conectarse a ella mediante SSH.
    - Tome una captura de pantalla del registro del sistema de la instancia EC2 que muestre que el servicio httpd se instaló correctamente.

*Pruebe su servidor web mediante la implementación de la página web sencilla que aparece a continuación:
    - Abra una sesión SSH en la instancia de EC2.
    - Abra un editor de texto y copie y pegue el siguiente código HTML:

    ```
    html
    <!DOCTYPE html>
    <html>
    <body>
    <h1>Su-NOMBRE's re/Start Project Work</h1>
    <p>EC2 Instance Challenge Lab</p>
    </body>
    </html>
    ```

*Reemplace YOUR-NAME con su nombre y guarde el archivo como projects.html.

*Coloque este archivo en el directorio /var/www/html de su instancia EC2.

*Abra un navegador web y navegue hasta esta página web de ejemplo. Tome una captura de pantalla que muestre que la página se devolvió y se muestra correctamente.

*Envíe las capturas de pantalla que tomó a su instructor.

**Sugerencias:**

- Deberá crear un par de claves y descargar su archivo de clave privada para permitirle conectarse de forma remota a través de SSH a su instancia EC2.

- Deberá crear una gateway de Internet y configurar correctamente la tabla de enrutamiento de la subred en la VPC antes de lanzar la instancia.

- Asegúrese de que dispone de la configuración del grupo de seguridad adecuada. En particular, debe permitir el tráfico HTTP.

- Utilice la dirección IP pública de la instancia para acceder a su página web.

