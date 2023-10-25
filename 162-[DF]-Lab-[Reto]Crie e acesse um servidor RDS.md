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
- Plantilla: Elige Dev/Test o la capa gratuita.
- Disponibilidad y durabilidad: Evita crear una instancia en espera.
- Tamaño de la instancia de la base de datos: Elige las clases con burst (db.t2 y db.t3) de tipo db.t*.micro a db.t*.medium.
- Almacenamiento: Elige General Purpose SSD (gp2) de un tamaño de hasta 100 GB. El acceso a IOPS provisionados está restringido.
- Amazon VPC: Utiliza el VPC del laboratorio.
- Grupo de seguridad: Incluye un grupo de seguridad que permita que LinuxServer se conecte a la instancia de RDS.
- Para MySQL, en Configuración adicional, habilita el monitoreo mejorado y deshabilita la opción.
- Opciones de compra: Se permiten instancias bajo demanda. Otras opciones de compra están desactivadas.

6. Haz clic en Detalles y luego en Mostrar.
7. Haz clic en Descargar PEM (para Linux o macOS) o Descargar PPK (para Windows) según tu sistema operativo local.
8. Toma nota de la dirección de LinuxServer.
9. Conéctate (SSH) al servidor LinuxServer utilizando los detalles que anotaste.
10. Instala un cliente MySQL y úsalo para conectarte a tu base de datos. Alguna información útil está disponible aquí.
11. Crea una tabla RESTART con las siguientes columnas. Captura la pantalla para la entrega.

- ID de estudiante (Número)
- Nombre del estudiante
- Ciudad de reinicio
- Fecha de graduación (Fecha y hora)
  
12. Inserta 10 filas de muestra en esta tabla. Captura la pantalla para la entrega.
13. Selecciona todas las filas de esta tabla. Captura la pantalla para la entrega.
14. Crea una tabla CLOUD_PRACTITIONER con las siguientes columnas. Captura la pantalla para la entrega.

- ID de estudiante (Número)
- Fecha de certificación (Fecha y hora)
  
15. Inserta 5 filas de muestra en esta tabla. Captura la pantalla para la entrega.
16. Selecciona todas las filas de esta tabla. Captura la pantalla para la entrega.
17. Realiza una unión interna entre las 2 tablas creadas anteriormente y muestra el ID de estudiante, el nombre del estudiante y la fecha de certificación. Captura la pantalla para la entrega.
