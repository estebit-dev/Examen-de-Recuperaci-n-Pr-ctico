![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.001.png)
##
## **CARRERA**
## **ADMINISTRACIÓN DE INFRAESTRUCTURA Y PLATAFORMAS TECNOLÓGICAS**
## **MATERIA**
## **SISTEMAS OPERATIVOS I**
## **TEMA**
## **EXAMEN DE RECUPERACIÓN PRÁCTICO**
## **ESTUDIANTE**
## <a name="_epqsslws3b9m"></a>**ESTEBAN GEOVANNY MOLINA CHIRIBOGA**
##
<a name="_ar3t143pwcgq"></a>**

------------------------------
## **DOCENTE**
## **ING. JONNATHAN F. NIVICELA A.**





**Configurar el Servidor DNS con Bind**

sudo dnf install bind bind-utils -y

Editar el archivo de configuración de Bind

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.002.png)

Modificar las siguientes líneas para permitir consultas desde tu red local:

options {

`    `listen-on port 53 { 127.0.0.1; 192.168.1.19; };

`    `allow-query     { localhost; 192.168.1.0/24; };

`    `recursion yes;

};

Editar el archivo de configuración de zonas:

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.003.png)

zone "examensistemas.com" IN {

`    `type master;

`    `file "/var/named/examensistemas.com.zone";

};

Crear el archivo de zona:

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.004.png)

Agregar las líneas

$TTL 86400

@   IN  SOA  ns1.examensistemas.com. root.examensistemas.com. (

`        `2025022001  ; Serial

`        `3600        ; Refresh

`        `1800        ; Retry

`        `604800      ; Expire

`        `86400 )     ; Minimum TTL

`    `IN  NS  ns1.examensistemas.com.

ns1 IN  A   192.168.1.19

www IN  A   192.168.1.19

mail IN  A   192.168.1.19

@   IN  MX 10 mail.examensistemas.com.

Guardar y cambia los permisos:

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.005.png)

Verificar que el DNS responde correctamente:

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.006.png)





**Instalar y Configurar Postfix con Dovecot**

sudo dnf install postfix dovecot -y sudo systemctl enable --now postfix dovecot

Editar la configuración de Postfix:

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.007.png)

Modificar las líneas

myhostname = mail.examensistemas.com

mydomain = examensistemas.com

myorigin = $mydomain

inet\_interfaces = all

inet\_protocols = ipv4

mydestination = $myhostname, localhost.$mydomain, localhost, $mydomain

relay\_domains = 

home\_mailbox = Maildir/

Guardar y reinicia Postfix:

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.008.png)

Configurar Dovecot

Editar el archivo de configuración:

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.009.png)

Verificar la línea: protocols = imap pop3 lmtp

Editae el archivo de autenticación:

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.010.png)

Descomentar la línea: disable\_plaintext\_auth = no

Editar el archivo de configuración de correo:

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.011.png)

Verificar la línea: mail\_location = maildir:~/Maildir

Reiniciar Dovecot:

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.012.png)

Abrir Puertos en el Firewall

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.013.png)

**Creación de una nueva cuenta para un usuario y que el mismo puede acceder y enviar/recibir correos**

Agregar el usuario y contraseña

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.014.png)

Crear el buzon para “Juan” y configurar permisos

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.015.png)

Verificamos que la carpeta se creo correctamente

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.016.png)

**Verificacion de correo**

Primero una verificación que funcione el correo servidor

Enviar un correo

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.017.png)

Verificar que llego el correo

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.018.png)

Usamos mutt para leer el correo

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.019.png)

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.020.png)

**Prueba Cliente (Juan) – Servidor (molina)**

Configuramos Thunderbird Servidor

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.021.png)

Configuramos ThunderBird Cliente

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.022.png)

Enviamos un correo de prueba desde Juan

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.023.png)

Revisamos la bandeja en molina

Verificado.

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.024.png)

**Servicio de Apache**

Instalamos apache

sudo dnf install httpd -y

Habilitamos para que inicie con el sistema

sudo systemctl enable --now httpd

Verificamos que esté corriendo:

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.025.png)

Creamos un nuevo archivo de configuración para el sitio

sudo nano /etc/httpd/conf.d/examensistemas.com.conf

y agregamos lo siguiente:

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.026.png)

Creamos el directorio para el sitio web de prueba y cambiamos el propietario para evitar problemas de permisos

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.027.png)

Creamos una página de prueba:

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.028.png)

Editamos el archivo de configuración de la zona DNS

sudo nano /var/named/examensistemas.com.zone

Y agregamos las líneas señaladas

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.029.png)

Guardamos y reniciamos el DNS

sudo systemctl restart named

Desde Windows comprobamos que el DNS responde correctamente

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.030.png)

Reiniciamos Apache para aplicar cambios

sudo systemctl restart httpd

Accedemos desde el navegador

Funciona correctamente con el dominio 

![](Aspose.Words.f9c4e3e0-b69f-4247-bd63-362cef9a3465.031.png)




