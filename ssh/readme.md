## Instalar servidor SSH

Para instalar el servicio SSH en un sistema Linux, puedes seguir estos pasos:

1. Abre una terminal en el sistema Linux y conecta el sistema a Internet.<br><br>

2. Ejecuta el siguiente comando para actualizar la lista de paquetes disponibles: 

   	**sudo apt-get update** <br><br>

3. Instala el paquete `openssh-server` utilizando el siguiente comando:

    	**sudo apt-get install openssh-server** <br><br>

4. El comando anterior instalará el servidor SSH y creará automáticamente un servicio systemd para iniciarlo y detenerlo. 


   Si necesitas iniciar el servicio manualmente, puedes usar el siguiente comando:
	   **sudo systemctl start ssh**


   Y para detener el servicio SSH, utiliza el siguiente comando:

   **sudo systemctl stop ssh** <br><br>


5. Asegúrate de que el servicio SSH se inicie automáticamente después del arranque utilizando el siguiente comando:

   **sudo systemctl enable ssh**


   Esto asegurará que el servicio SSH se inicie automáticamente cada vez que se inicie el sistema.

   Con estos pasos, deberías haber instalado el servicio SSH en tu sistema Linux y estarás listo para conectarte a él de forma segura mediante SSH.
   
   
   # INSTALAR SSH SERVIDOR Y SSH CLIENTE EN UBUNTU

## Para instalar el servidor SSH en Ubuntu, puedes seguir estos pasos:
        ------------------------

1.Abre una terminal en Ubuntu.

2.Ejecuta el siguiente comando para actualizar la lista de paquetes:
	**sudo apt-get update**

3.Ejecuta el siguiente comando para instalar el servidor SSH:
	**sudo apt-get install openssh-server**

4.Espera a que se complete la instalación.

## Para instalar el cliente SSH en Ubuntu, puedes seguir estos pasos:
        -----------------------

1.Abre una terminal en Ubuntu.

2.Ejecuta el siguiente comando para actualizar la lista de paquetes:
	**sudo apt-get update**

3.Ejecuta el siguiente comando para instalar el cliente SSH:
	**sudo apt-get install openssh-client**

4.Espera a que se complete la instalación.


