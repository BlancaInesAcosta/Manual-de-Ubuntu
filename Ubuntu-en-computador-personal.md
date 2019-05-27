Contenido
Autor: Vladimir Támara Patiño
Actualizado: Daniel Cardona Nogueira

1. Configuraciones posibles
Si procesador soporta virtualización y computador tiene más de 4GB en RAM y en disco más de 160GB, emplear configuración 1,
Si procesador es de 64 bits pero el disco es de menos de 160GB o tiene menos de 4GB en RAM emplear configuración 2.
Si el procesador es de 32 bits emplear configuración 3.

1.1 Configuración 1
Ubuntu completo actualizado con cuentas administrativas para el usuario y si el usuario desea apoyo en actualizaciones con cuenta para la oficina de TI de nombre soporteti y clave conocida sólo por miembros de la Oficina de TI --por cambiar cuando haya cambios de personal.
Chromium en Ubuntu
Disco compartido (U:) configurado en Ubuntu en un marcador del gestor de archivos
Windows 10 (aprox. 50G) en máquina virtual ingresado al dominio. Carpetas compartidas: Descargas, Documentos y USBs (/media/usuario de Ubuntu).  Nombre de máquina según nemotecnia.
En los computadores con cuenta soporteti instalar paquete ssh para facilitar administración remota (particularmente actualizaciones).
Este documento se centra en este.

1.2 Configuración 2
Agregar un disco duro y poner Linux en el nuevo disco. Dejar Windows en primario con GRUB.
1.3 Configuración 3
Una partición con Linux
En el caso de computadores de biblioteca que ingrese sin clave a cuenta y que automáticamente abra navegador con pestaña única en Archivo de Prensa.
2. Preparativos (configuración 1)
Para la compra de equipos seguir recomendaciones de https://archivoprensa.cinep.org.co:12443/ti/front/knowbaseitem.form.php?id=107 
Si el computador viene con Windows OEM debe dejarse al menos un año para propósitos de garantía.  
Si el computador viene con UEFI y opera en modo UEFI deshabilitar arranque seguro --secure boot (necesario para que corra VirtualBox y sus módulos no firmados por Ubuntu 16.04).  Para esto al iniciar computador ingresar a la BIOS y buscar menú de secure boot, asegurarse de que esté deshabilitado y de que las llaves estén booradas.
Desde la BIOS habiltar virtualización.
Desde Windows 10 reducir partición de Windows: Herramientas del Sistema, Herramientas Administrativas.
Asignar una IP reservada en el DCHP del CINEP en grupo "A internet equipos No Dominio" y añadirla al grupo Linux de la regla SSH a Linux.  Si es en banco de datos la destinada para el usuario en cortafuegos y si se instalará Windows virtualizado en banco de datos reservar una IP en el DHCP.
3. Instalación (configuración 1)
Instalar preferiblemente desde USB (en la Oficina de TI esperamos tener varias memorias USB listas con medios de instalación de los sistemas que soportamos).
Arrancar desde USB con instalador (puede requerir ingresar al BIOS para reconfigurar orden de arranque) o presionar tecla al arranque para elegir arranque por USB.  Los medios recientes soportan bien arranque UEFI que es mejor si hay otros sistemas.
Desde el Ubuntu cuando de la opción de elegir espacio en disco escoger "Mas opciones" para controlar en detalle el uso de disco, achicar aún más partición de Windows (digamos 100G) --se puede hacer desde instalador de Ubuntu pero no desde herramienta de Windows).
En el espacio liberado crear 3 particiones: / de unos 100G para Ubuntu, área de intercambio del doble de RAM y el resto para /home donde quedarán datos de usuario.  Este particionado facilitará actualizaciones de emergencia donde se formatee y remplace / pero no se toquen datos de usuario.  grub en partción maestra del disco es buena opción.
Para el nombre del equipo y el nombre del usuario seguir las recomendaciones del documento de principios y polìtcas https://archivoprensa.cinep.org.co:12443/ti/front/knowbaseitem.form.php?id=13 
Una vez instalado configurar de requerirse el BIOS para que use GRUB en lugar del arranque de Windows.
4. Configuración y otros programas
Configurar red:
En el caso de CINEP asignar una IP fija en el documento de inventario de IPs de base de conocimiento y usarla. Usar puerta de enlace 192.168.1.15 y DNS 192.168.1.6
En el caso de Banco de datos poner ip fija asignada (la asignación en el momento la hace Vladimir), puerta de enlace 192.168.177.19, servidor DNS 192.168.177.19 con Rutas  Direccion: 192.168.1.0    Mask 255.255.255.0    Gateway 192.168.177.30  Metrica: 1500
Agregar cache de paquetes:
echo 'Acquire::http { Proxy "http://192.168.1.66:3142"; };' | sudo tee /etc/apt/apt.conf.d/01apt-cacher-ng-proxy
Actualizar paquetes disponibles en repositorios:
sudo apt-get update
Desde el HUD teclear IDIOMAS (o LANGUAGE), e instalar soporte completo de idiomas y de español en particular.
Instalar paquete ubuntu-restricted-extras:
sudo apt-get install ubuntu-restricted-extras
sCuando pida aceptar licencua EULA dar Aceptar y después confirmar con Si

Instalar complementos completos de Libreoffice (por ejemplo que permiten guardar como dbf) con: sudo apt-get install libreoffice
Instalar chromium:  Desde dash Software y Actualizaciones.  En pestaña Otro Software marcar Socios de Canonical. Guardar y recargar. Después abrir Ubuntu Software teclear chromium y esperar al verlo elegir Instalar.
sudo apt-get install chromium-browser
Instalar chrome. 
Procedimiento 1 de https://www.profesionalreview.com/2016/04/25/instalar-chrome-50-ubuntu-16-04-lts/ 
sudo sh -c 'echo "deb http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google.list'
wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add
sudo apt-get update
sudo apt-get install google-chrome-stable
Si no es posible: Ir a a https://www.google.com/chrome/ Presionar "Descargar ahora", aceptar licencia, elegir paquete .deb. Guardar en Descargas.  Desde una terminal:
cd Descargas
sudo dpkg -i google-ch*
sudo apt-get install -f
Desde la terminal ejecutarlo con google-chrome-stable y una vez abra botón derecho sobre el ícono del lanzador y elegir "Mantener en el lanzador"
Instalar tipos de letra institucionales.
Como usuario en explorador de archivos ubicar en carpeta compartida el directorio "Institucional/Plantillas/MATERIAL INSTITUCIONAL/Fuentes" y copiar Frutiger y Myriad Pro. Después pegarlas por ejemplo en Descargas.  De allí de descargas volver a copiarlas en portapapeles.
Desde una terminal ejecutar "sudo nautilus /usr/share/fonts/truetype" y allí pegar las carpetas Frutiger y Myriad Pro.
Cerrar exploradores y ejecutar desde la terminal "sudo fc-cache -f -v" y probar que los tipos están disponibles y se ven bien por ejemplo desde LibreOffice Writer (Más en https://wiki.ubuntu.com/Fonts )
Cambiar nombre del equipo para que siga nemotecnia desde Configuración->Detalles
Instalar impresoras, buscando 192.168.1.21 y 192.168.1.22 y eligiendo JetDirect (que deja IP y puerto 9100) ha operado bien configuración automátca.
Instalar y ejecutar Fusion Inventory.  Ver instrucciones en sección 3.3 de https://archivoprensa.cinep.org.co:12443/ti/front/knowbaseitem.form.php?id=1
Si el computador lo usa un solo usuario explicarle que un administrador podría ver la información no cifrada que esté en su equipo (lo cual no es la política de la Oficina de TI), preguntarle si desea apoyo de la Oficina TI para soporte y actualizaciones de larga duración del computador en caso afirmativo crear cuenta administrador soporteti con clave compartida e instalar paquete ssh.
Por cada usuario que emplee el computador:
Crearle una cuenta siguiendo nemotecnica de usuarios 
Explicarle que el administrador podrá ver la información no cifrada que haya en su computador, pero esa no es la política de la Oficina de TI
Configurar desde nautilus acceso a directorio U: con usuario del dominio Windows
En el lanzador dejarle iconos de chromium y chrome
Con el usuario cambiar claves en Ubuntu, dominio Windows, LDAP para que coincidan.
Hacerle una capacitación básica
4.1 Windows en máquina virtual
Instalar VirtualBox (tras desactivar Secure Boot como se explicó).  Aunque hay paquete para ubuntu  mejor descargar de https://www.virtualbox.org/wiki/Linux_Downloads y extension pack de http://www.oracle.com/technetwork/es/server-storage/virtualbox/downloads/index.html#extpack  
La instalación hacerla con dpkg --install virtualbox...deb instalar librerías adicionales o apt -f install
Reconfigura con dpkg-reconfigure virtualbox para que recompile controladores.
Al crear maquina virtual asegurar de poner en configuración general. habilitar portapapeles bidireccional y arrastrar archivos bidireccional.
Desde la terminal ejecutarlo con virtualbox y una vez abra, pulsar botón derecho sobre el icono del lanzador y elegir "Mantener en el lanzador"
Copiar máquina virtual con Windows 10
scp "soporteti@192.168.1.66:VirtualBox\ VMs/w10clonada/w10clonada-disk1.vdi" ~
Esa máquina virtual ya tiene Windows 10, Office 2013, Karpesky, Chrome, Acrobat Reader y está en el dominio. 

En la configuración del adaptador de red de VirtualBox para esta máquna elegir Adaptador Puente -----para que quede en la red del computador físico.  Debe cambiarse en todo caso la configuración de red  (por omisión va con comodin 192.168.1.50) a: (1) si se unirá al dominio  reservar una IP de la red de empleados --en el documento de inventario-- y usarla

Configurar las carpetas compartidas de acuerdo a documento https://192.168.1.5/ti/front/knowbaseitem.form.php?id=42 (OJO: máximo 3)
Iniciar máquina virtual ingresar con cuenta local soporteti (administrador).  Poner la IP asignada.  Cambiarle el nombre para que siga nemotécnia desde Equipo->Propiedades.  Desde el mismo unirlo al domino con Id. de Red configurando el usuario del dominiuo que lo empleará.  Tras reiniciar con el usuario conectar el U: desde Explorador, Red botón derecho Conectar Unidad de Red.  Como Ruta funciona \\192.168.1.1\UsuariosCinep.  Actualizar bases de Karpesky.  NO correr Fusion (sólo para máquinas físicas).
 

4.2 Notas sobre preparación de imagen con Windows por reusar
Se creó de 50G dinámicos
Se instaló Windows 7 con los medios del CINEP e IP fija
Se activó abriendo puertos en cortafuegos (pedimos el favor a Jairo de documentar)
Se actualizó a Windows 10 (tuvo que hacerse 2 veces).
Se instaló Office 2013 (pedimos el favor a Jairo de documentar procedimiento)
Se instaló Karpesky con medios encontrados en el U: y llave 2015-2016 alli disponible
Se instaló chrome --tuvo que hacerse con IP fija y sin proxy
La imagen que se recomienda copiar en otros computadores es clonación de la instalada, sin entrelazar y del estado actual (que es más corta que copiando todas las instantaneas).
4.3 Conexión USB por medio de carpetas compartidas

• Iniciamos nuestra máquina virtual y en el menú horizontal superior, pulsa en "Devices" y, después, en "Install Guest Additions".

• Ahora, para hacer una carpeta compartida en VirtualBox vamos al "Menú de Inicio" de Windows y navegamos hasta "Mi Equipo" y pulsa en "Unidad de CD:VirtualBox Guest Additions". Posteriormente se abrirá un instalador que te irá guiando para que el programa se ubique en el equipo.

• Una vez que se hayan instalado las VirtualBox Guest Additions, para compartir una carpeta nos devolvemos a "Devices" y, después, pulsar en "Shared folder settings".Pincha en el icono de la carpeta con un signo más en verde.

• Ahora, podrás navegar por tu equipo "real" hasta escoger las carpetas HOME y MEDIA ubicadas en otras ubicaciones, se seleccionara la opción de hacerla permanente.

