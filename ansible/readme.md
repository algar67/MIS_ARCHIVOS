# Instalacion de ansible

Hay que copiar la clave pública del nodo control de Ansible a los nodos complementarios.

Para eso debes generar una clave SSH en el nodo control de Ansible, si aún no la has generado. 

Puedes generarla con el siguiente comando: **ssh-keygen -t rsa**

Esto generará una clave pública y una clave privada en la ubicación por defecto (~/.ssh/id_rsa).

Copiar la clave pública a los nodos complementarios con el siguiente comando: 

**ssh-copy-id usuario@nodo** 

Reemplaza "usuario" con tu nombre de usuario en el nodo complementario y "nodo" con el nombre de host o dirección IP del nodo complementario.

Por ejemplo, si el usuario es "jdoe" y el nodo complementario se llama "nodo1.example.com", el comando sería: ssh-copy-id jdoe@nodo1.example.com.

El comando ssh-copy-id copiará automáticamente la clave pública a la carpeta ~/.ssh/authorized_keys en el nodo complementario y ajustará los permisos adecuados para asegurar que sólo el propietario de la clave pueda acceder a ella.

Repite el paso 2 para cada nodo complementario al que desees copiar la clave pública. Asegúrate de cambiar el nombre de host o dirección IP y el usuario para cada nodo complementario.

Con estos pasos, deberías haber copiado la clave pública del nodo control de Ansible a los nodos complementarios y estarás listo para conectarte a ellos de forma segura utilizando SSH.

<br><br><br>

¿Desde que directorio se pueden lanzar los comandos de Ansible?

En general, los comandos de Ansible se ejecutan desde el directorio raíz del proyecto, donde se encuentran los archivos de configuración (como `ansible.cfg` y `inventory`) y los playbooks.

Por ejemplo, si tienes una estructura de proyecto como la siguiente:

ansible-project/
├── ansible.cfg
├── inventory
└── playbooks/
├── webserver.yaml
├── database.yaml
└── common.yaml

Para ejecutar uno de los playbooks, desde la línea de comandos, debes cambiar al directorio `ansible-project` y ejecutar el comando, como por ejemplo:

cd ansible-project
**ansible-playbook playbooks/webserver.yaml**<br>
En este caso, el comando ansible-playbook se está ejecutando desde el directorio raíz del proyecto ansible-project, donde se encuentran los archivos de configuración y los playbooks.

Ten en cuenta que, si utilizas rutas relativas en tus playbooks o tareas, el directorio base será relativo a la ubicación del archivo en sí, y no al directorio desde el que se ejecuta el comando.
