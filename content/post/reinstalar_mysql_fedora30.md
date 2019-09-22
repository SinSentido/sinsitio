---
title: "Reinstalar mysql en Fedora 30"
date: 2019-09-22T00:03:51+02:00
draft: false
---
#Reinstalar mysql en Fedora 30

Para variar se me ha vuelto a olvidar la contraseña de root de mysql, y como he sido incapaz de cambiar la contraseña ya que no me funcionaba el comando mysqld_safe --skip-grant-tables & que es para iniciar mysql sin seguridad de manera que pudiera entrar a la cuenta de root y cambiar desde ahí la contraseña pues he tenido que reinstalarlo. Así que he decidido aprovechar para hacer un post sobre como eliminar correctamente y despues volver a instalar y configurar el servidor de mysql en fedora (quien me mandaría dejar debian, con lo apañaico que lo tenía todo ya).

##Desinstalar mysql

El primer paso que debemos hacer es eliminar los repositorios instalados en el equipo:

<pre>[sinsentido@localhost ~]$ sudo dnf remove mysql mysql-server
</pre>

Después tenemos que borrar todos los archivos de configuración del sistema. Generalmente podemos encontrarlos en /var/lib/mysql, pero en caso de que no los encontremos ahí podemos consultar la ruta de instalación en el archivo /etc/my.conf

<pre>[sinsentido@localhost ~]$ cat /etc/my.cnf | grep datadir
<font color="#CC0000"><b>datadir</b></font>=/var/lib/mysql
</pre>

<pre>[sinsentido@localhost ~]$ sudo rm -r /var/lib/mysql
</pre>

En mi caso lo he borrado porque no tengo nada importante ya que es una instalación que tengo para hacer cosas de clase. Pero si quisieramos conservar los archivos para hacer una copia de seguridad podemos renombrarlo en lugar de eliminarlo: 

<pre>[sinsentido@localhost ~]$ sudo mv /var/lib/mysql /var/lib/mysql_backup
</pre>

##Instalar mysql

Vale, ahora ya lo tenemos todo borrado y podemos empezar a reinstalar. Vamos a instalar el cliente y el servidor:

<pre>[sinsentido@localhost ~]$ sudo dnf install mysql mysql-server
</pre>

Y si todo corre sin errores ya tendremos instalado nuestro servidor mysql. 

##Configurar la nueva instalación

Lo primero que haremos una vez hayamos instalado mysql es iniciar el daemon y habilitarlo para que se inicie con el arranque del sistema.

<pre>[sinsentido@localhost ~]$ sudo systemctl start mysqld
[sinsentido@localhost ~]$ sudo systemctl enable mysqld
</pre>

Para los siguientes pasos necesitamos tener la contraseña que mysql crea por defecto para el usuario root. La podemos encontrar en /var/log/mysqld.log

<pre>[sinsentido@localhost ~]$ sudo cat /var/log/mysqld.log | grep password
</pre>

Una vez tenemos la contraseña por defecto entramos en mysql para cambiarla por una contraseña de nuestra elección con el siguiente comando:

<pre>[sinsentido@localhost ~]$ mysqladmin -u root -p password NuevaContraseña
Enter password: </pre>

Donde pone Enter password introducimos la contraseña que hemos obtenido anteriormente y donde pone "NuevaContraseña" escribimos nuestra contraseña. 

Hay que decir que mysql se ha vuelto tiquismiquis con la seguridad de las contraseñas y hay que cumplir bastantes condiciones.

Para finalizar probamos a entrar a mysql con la nueva contraseña y si lo hemos hecho bien debería ir correctamente.

<pre>[sinsentido@localhost ~]$ mysql -u root -p
Enter password: 
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 26
Server version: 8.0.17 MySQL Community Server - GPL

Copyright (c) 2000, 2019, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type &apos;help;&apos; or &apos;\h&apos; for help. Type &apos;\c&apos; to clear the current input statement.

mysql&gt; 
</pre>





