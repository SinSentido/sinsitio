---
title: "Instalar_postgre_odoo_fedora"
date: 2019-10-08T20:22:55+02:00
draft: false
---

#Añadir el repositorio
<pre>$ sudo rpm -Uvh https://yum.postgresql.org/11/fedora/fedora-30-x86_64/pgdg-fedora-repo-latest.noarch.rpm
[sudo] password for sinsentido: 
Recuperando https://yum.postgresql.org/11/fedora/fedora-30-x86_64/pgdg-fedora-repo-latest.noarch.rpm
advertencia:/var/tmp/rpm-tmp.GZQLBc: EncabezadoV4 DSA/SHA1 Signature, ID de clave 442df0f8: NOKEY
Verifying...                          ################################# [100%]
Preparando...                         ################################# [100%]
Actualizando / instalando...
   1:pgdg-fedora-repo-42.0-5          ################################# [100%]
</pre>

#Instalar postgre



#Comprobar la version instalada y verifiacar que se se ha instalado correctamente
<pre>psql --version
psql (PostgreSQL) 12.0
</pre>


#Inicializar postgre y crear sus archivos de configuración.
<pre>sudo /usr/pgsql-12/bin/postgresql-12-setup initdb
[sudo] password for sinsentido: 
Initializing database ... OK
</pre>

#Iniciamos el servidor postgreSQL
<pre>$ sudo systemctl start postgresql-12.service 
</pre>


#Configuramos el usuario para el postgre.
<pre>$ sudo -i -u postgres
</pre>

<pre>[postgres@pc157-8 ~]$ createuser --interactive
Ingrese el nombre del rol a agregar: sinsentido
¿Será el nuevo rol un superusuario? (s/n) s
</pre>

<pre>[postgres@pc157-8 ~]$ createdb sinsentido
</pre>

<pre>$ psql
psql (12.0)
Digite «help» para obtener ayuda.

sinsentido=&gt; 
</pre>


#Ahora que el servidor está funcionando y configurado vamos a instalar un gestor gráfico (pgAdmin
<pre>$ sudo dnf install pgadmin4
</pre>

#configurar
<pre>$ cd /etc/httpd/conf.d/
[sinsentido@pc157-8 conf.d]$ ls
autoindex.conf  mod_dnssd.conf  pgadmin4.conf.sample  php.conf  phpMyAdmin.conf  README  userdir.conf  welcome.conf
[sinsentido@pc157-8 conf.d]$ sudo cp pgadmin4.conf.sample ./pgadmin4.conf
[sudo] password for sinsentido: 
</pre>

#Crear los directorios de datos 

<pre>$ sudo mkdir -p /var/lib/pgadmin4/ /var/log/pgadmin4/
</pre>

#Crear directivas de inicio de sesion
<pre> sudo python3 /usr/lib/python3.7/site-packages/pgadmin4-web/setup.py
NOTE: Configuring authentication for SERVER mode.

Enter the email address and password to use for the initial pgAdmin user account:

Email address: josexp93@gmail.com
Password: 
Retype password:
pgAdmin 4 - Application Initialisation
======================================
</pre>


#Crear y configurar archivo de configuracion
<pre>$ cd /usr/lib/python3.7/site-packages/pgadmin4-web/
[sinsentido@pc157-8 pgadmin4-web]$ sudo nano config_local.py
</pre>

<pre>LOG_FILE = <font color="#4E9A06"><b>&apos;/var/log/pgadmin4/pgadmin4.log&apos;</b></font>
SQLITE_PATH = <font color="#4E9A06"><b>&apos;/var/lib/pgadmin4/pgadmin4.db&apos;</b></font>
SESSION_DB_PATH = <font color="#4E9A06"><b>&apos;/var/lib/pgadmin4/sessions&apos;</b></font>
STORAGE_DIR = <font color="#4E9A06"><b>&apos;/var/lib/pgadmin4/storage&apos;</b></font>

</pre>
