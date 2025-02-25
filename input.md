![](./image1.png){width="6.5in" height="0.8972222222222223in"}

## 

## **CARRERA**

## **ADMINISTRACIÓN DE INFRAESTRUCTURA Y PLATAFORMAS TECNOLÓGICAS**

## **MATERIA**

## **SISTEMAS OPERATIVOS I**

## **TEMA**

## **EXAMEN DE RECUPERACIÓN PRÁCTICO**

## **ESTUDIANTE**

## **ESTEBAN GEOVANNY MOLINA CHIRIBOGA**

## 

## ** **

## **DOCENTE**

## **ING. JONNATHAN F. NIVICELA A.**

**Configurar el Servidor DNS con Bind**

sudo dnf install bind bind-utils -y

Editar el archivo de configuración de Bind

![](./image2.png){width="5.115297462817148in"
height="0.19794400699912512in"}

Modificar las siguientes líneas para permitir consultas desde tu red
local:

options {

listen-on port 53 { 127.0.0.1; 192.168.1.19; };

allow-query { localhost; 192.168.1.0/24; };

recursion yes;

};

Editar el archivo de configuración de zonas:

![](./image3.png){width="5.354913604549432in"
height="0.19794400699912512in"}

zone \"examensistemas.com\" IN {

type master;

file \"/var/named/examensistemas.com.zone\";

};

Crear el archivo de zona:

![](./image4.png){width="6.011255468066492in"
height="0.20836286089238845in"}

Agregar las líneas

\$TTL 86400

@ IN SOA ns1.examensistemas.com. root.examensistemas.com. (

2025022001 ; Serial

3600 ; Refresh

1800 ; Retry

604800 ; Expire

86400 ) ; Minimum TTL

IN NS ns1.examensistemas.com.

ns1 IN A 192.168.1.19

www IN A 192.168.1.19

mail IN A 192.168.1.19

@ IN MX 10 mail.examensistemas.com.

Guardar y cambia los permisos:

![](./image5.png){width="6.5in" height="0.8409722222222222in"}

Verificar que el DNS responde correctamente:

![](./image6.png){width="6.5in" height="4.44375in"}

**Instalar y Configurar Postfix con Dovecot**

sudo dnf install postfix dovecot -y sudo systemctl enable \--now postfix
dovecot

Editar la configuración de Postfix:

![](./image7.png){width="4.427701224846894in"
height="0.2500349956255468in"}

Modificar las líneas

myhostname = mail.examensistemas.com

mydomain = examensistemas.com

myorigin = \$mydomain

inet_interfaces = all

inet_protocols = ipv4

mydestination = \$myhostname, localhost.\$mydomain, localhost,
\$mydomain

relay_domains =

home_mailbox = Maildir/

Guardar y reinicia Postfix:

![](./image8.png){width="4.3964468503937in"
height="0.1771084864391951in"}

Configurar Dovecot

Editar el archivo de configuración:

![](./image9.png){width="4.834008092738408in"
height="0.2396172353455818in"}

Verificar la línea: protocols = imap pop3 lmtp

Editae el archivo de autenticación:

![](./image10.png){width="5.396586832895888in"
height="0.21878062117235345in"}

Descomentar la línea: disable_plaintext_auth = no

Editar el archivo de configuración de correo:

![](./image11.png){width="5.9070745844269466in"
height="0.16668963254593175in"}

Verificar la línea: mail_location = maildir:\~/Maildir

Reiniciar Dovecot:

![](./image12.png){width="4.490209973753281in"
height="0.1875262467191601in"}

Abrir Puertos en el Firewall

![](./image13.png){width="6.000837707786527in"
height="1.9481889763779527in"}

**Creación de una nueva cuenta para un usuario y que el mismo puede
acceder y enviar/recibir correos**

Agregar el usuario y contraseña

![](./image14.png){width="6.188363954505687in"
height="1.2814293525809275in"}

Crear el buzon para "Juan" y configurar permisos

![](./image15.png){width="6.5in" height="0.5013888888888889in"}

Verificamos que la carpeta se creo correctamente

![](./image16.png){width="4.948606736657918in"
height="0.8959580052493439in"}

**Verificacion de correo**

Primero una verificación que funcione el correo servidor

Enviar un correo

![](./image17.png){width="6.5in" height="0.2902777777777778in"}

Verificar que llego el correo

![](./image18.png){width="6.5in" height="0.6597222222222222in"}

Usamos mutt para leer el correo

![](./image19.png){width="4.542300962379702in"
height="0.531324365704287in"}

![](./image20.png){width="6.5in" height="1.9597222222222221in"}

**Prueba Cliente (Juan) -- Servidor (molina)**

Configuramos Thunderbird Servidor

![](./image21.png){width="6.304180883639545in"
height="3.771732283464567in"}

Configuramos ThunderBird Cliente

![](./image22.png){width="6.5in" height="3.654166666666667in"}

Enviamos un correo de prueba desde Juan

![](./image23.png){width="6.5in" height="0.3402777777777778in"}

Revisamos la bandeja en molina

Verificado.

![](./image24.png){width="6.5in" height="2.654166666666667in"}

**Servicio de Apache**

Instalamos apache

sudo dnf install httpd -y

Habilitamos para que inicie con el sistema

sudo systemctl enable \--now httpd

Verificamos que esté corriendo:

![](./image25.png){width="5.8396522309711285in"
height="3.3141272965879267in"}

Creamos un nuevo archivo de configuración para el sitio

sudo nano /etc/httpd/conf.d/examensistemas.com.conf

y agregamos lo siguiente:

![](./image26.png){width="5.745814741907261in"
height="2.8900951443569554in"}

Creamos el directorio para el sitio web de prueba y cambiamos el
propietario para evitar problemas de permisos

![](./image27.png){width="6.386307961504812in"
height="0.5000699912510936in"}

Creamos una página de prueba:

![](./image28.png){width="6.5in" height="0.3909722222222222in"}

Editamos el archivo de configuración de la zona DNS

sudo nano /var/named/examensistemas.com.zone

Y agregamos las líneas señaladas

![](./image29.png){width="6.5in" height="2.995138888888889in"}

Guardamos y reniciamos el DNS

sudo systemctl restart named

Desde Windows comprobamos que el DNS responde correctamente

![](./image30.png){width="4.300582895888014in"
height="2.3188615485564306in"}

Reiniciamos Apache para aplicar cambios

sudo systemctl restart httpd

Accedemos desde el navegador

Funciona correctamente con el dominio

![](./image31.png){width="6.5in" height="2.341666666666667in"}
