# Challenge Lab: Build Your DB Server and Interact With Your DB

This lab is designed to reinforce the concept of leveraging an AWS-managed database instance for solving relational database needs. **Amazon Relational Database Service (Amazon RDS)** makes it easy to set up, operate, and scale a relational database in the cloud. It provides cost-efficient and resizable capacity while managing time-consuming database administration tasks, which allows you to focus on your applications and business. Amazon RDS provides you with six familiar database engines to choose from: Amazon Aurora, Oracle, Microsoft SQL Server, PostgreSQL, MySQL, and MariaDB.

## Learning Objectives

After completing this lab, you will be able to:
- Create an RDS instance
- Use the Amazon RDS Query Editor to query data.

## Duration

This lab takes approximately 45 minutes.


## Tu Desafío
Para completar el Desafío, realiza lo siguiente:

5. Lanza una instancia de base de datos Amazon RDS utilizando los motores de base de datos Amazon Aurora Provisioned o MySQL. Toma nota de las credenciales de la base de datos, ya que se necesitarán en los pasos siguientes. Ten en cuenta las siguientes restricciones del laboratorio:

 - Motor de base de datos: Los motores admitidos son Amazon Aurora o MySQL. Amazon Aurora Serverless no está disponible.
  
[![1.png](https://i.postimg.cc/X7TcmM2g/1.png)](https://postimg.cc/tYNxncG1)

 - Plantilla: Elige Dev/Test o la capa gratuita.

[![2.png](https://i.postimg.cc/RZKQzPyx/2.png)](https://postimg.cc/NKGr6xQd)

 - Disponibilidad y durabilidad: Evita crear una instancia en espera.

[![3.png](https://i.postimg.cc/rsDxZgxb/3.png)](https://postimg.cc/bSjZsx6H)

 - Tamaño de la instancia de la base de datos: Elige las clases con burst (db.t2 y db.t3) de tipo db.t*.micro a db.t*.medium.

[![4.png](https://i.postimg.cc/hG0LXXKC/4.png)](https://postimg.cc/B8XLyQLD)

 - Almacenamiento: Elige General Purpose SSD (gp2) de un tamaño de hasta 100 GB. El acceso a IOPS provisionados está restringido.

[![5.png](https://i.postimg.cc/wBz5xXYy/5.png)](https://postimg.cc/Xr2BQyJ4)
 
 - Amazon VPC: Utiliza el VPC del laboratorio.

[![6.png](https://i.postimg.cc/DZyqL7Z7/6.png)](https://postimg.cc/QBw9rG1z)
 
 - Grupo de seguridad: Incluye un grupo de seguridad que permita que LinuxServer se conecte a la instancia de RDS.

[![7.png](https://i.postimg.cc/pr38w8Bd/7.png)](https://postimg.cc/ZvFC3W2k)

 - Para MySQL, en Configuración adicional, habilita el monitoreo mejorado y deshabilita la opción.

[![8.png](https://i.postimg.cc/G27vj68G/8.png)](https://postimg.cc/47cKfL1x)

 - Opciones de compra: Se permiten instancias bajo demanda. Otras opciones de compra están desactivadas.

6. Haz clic en Detalles y luego en Mostrar.
7. Haz clic en Descargar PEM (para Linux o macOS) o Descargar PPK (para Windows) según tu sistema operativo local.
8. Toma nota de la dirección de LinuxServer.
9. Conéctate (SSH) al servidor LinuxServer utilizando los detalles que anotaste.
10. Instala un cliente MySQL y úsalo para conectarte a tu base de datos. Alguna información útil está disponible aquí.

[![9.png](https://i.postimg.cc/SRp6tcQf/9.png)](https://postimg.cc/cgXtvgnv)

11. Crea una tabla RESTART con las siguientes columnas. Captura la pantalla para la entrega.
      - ID de estudiante (Número)
      - Nombre del estudiante
      - Ciudad de reinicio
      - Fecha de graduación (Fecha y hora)
  
12. Inserta 10 filas de muestra en esta tabla. Captura la pantalla para la entrega.
13. Selecciona todas las filas de esta tabla. Captura la pantalla para la entrega.

[![10.png](https://i.postimg.cc/VLm9q9VY/10.png)](https://postimg.cc/V5Zb1twT)

14. Crea una tabla CLOUD_PRACTITIONER con las siguientes columnas. Captura la pantalla para la entrega.
      - ID de estudiante (Número)
      - Fecha de certificación (Fecha y hora)
  
15. Inserta 5 filas de muestra en esta tabla. Captura la pantalla para la entrega.
16. Selecciona todas las filas de esta tabla. Captura la pantalla para la entrega.

[![11.png](https://i.postimg.cc/BZxcP7NY/11.png)](https://postimg.cc/F1HJ2ZM3)

17. Realiza una unión interna entre las 2 tablas creadas anteriormente y muestra el ID de estudiante, el nombre del estudiante y la fecha de certificación. Captura la pantalla para la entrega.

[![12.png](https://i.postimg.cc/L5BtdhnK/12.png)](https://postimg.cc/8fcJMpRt)
