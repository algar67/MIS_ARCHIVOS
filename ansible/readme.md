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

## Instalación de Ansible

Para este Bootcamp vamos a utilizar como **Nodo de control** una **MV con Ubuntu**. 

El único prerrequisito de Ansible es que el Nodo de control tenga instalado **Python 2** (versión 2.7) o **Python 3** (versiones 3.5 y superiores)

***No se puede utilizar Ansible desde Windows*** Hay dos productos Ansible: 

- Ansible community
- Ansible-core (más minimalista)

Instalaremos Ansible community

Sobre la MV que se acompaña para el Bootcamp, vamos a crear un clon que utilizaremos como Nodo Administrado. Luego la configuraremos más adelante en otro Hands-On

Después en la MV original, sobre un terminal: 

```bash
sudo -E add-apt-repository ppa:ansible/ansible

sudo apt update

# Solo ejecutar este comando si tenéis buena conexión de internet. 
sudo apt upgrade

sudo apt install ansible
```

Para otros linux

```bash
# Fedora:

sudo dnfinstallansible

# RHEL

sudo yuminstallansible

# CentOS

sudo yuminstallepel-release
sudo yuminstallansible
```

Ahora vamos a dar un nombre a nuestra máquina con Ansible:

```bash
sudo nmcli general hostname control
sudo systemctl restart systemd-hostnamed
reboot
```

También crearíamos una clave SSH con ssh-keygen.
- Nota: Aunque sabemos que meter una **passphrase** es más seguro, en estos ejemplo, para evitar que Ansible nos la pregunte cada vez que lanzamos una tarea, la dejaremos vacía.


Vamos a tomar ahora la máquina que clonamos. Y para hacer más realista la práctica, cambiaremos el nombre de la maquina, y nos preocuparemos de que tenga instalado SSH.
  - Antes de abrir el nodo de control, cambiamos el nombre a la que funcionará como nodo administrado:

```bash
sudo nmcli general hostname nodo1 

sudo systemctl restart systemd-hostnamed
```

Podríamos crear más nodos clonado nodo1 y realizando los cambios pertinentes en las ip de red.

También habría que copiar el fichero **authorized_keys** en cada nodo para que así el nodo de control tenga acceso.

Una vez que tengamos acceso por SSH a los nodos, podemos empezar lanzando los primeros comandos Ad-hoc

## El primer inventario

Ansible ***necesita identificar a quién se va a aplicar las tareas que definamos***, bien con comandos Ad - hoc o con Playbooks.

Para ello necesitamos crear un **Inventario**.

Podemos usar el ***inventario por defecto*** que se encuentra en **/etc/ansible/hosts**

O podemos crear uno, en forma de texto plano, que se utilizará sólo para las tareas que lo indiquen.


Para tener todo almacenado en una carpeta, vamos a crear en el Home la carpeta ansible 

````bash
mkdir ansible
cd ansible
````

- Con cualquier herramienta de edición (vi, vim, gedit, etc) podemos crear el primer inventario llamado **inven.txt**:

- Abrimos el fichero y en él, escribimos:
  - nodo1
  - O bien 192.168.174.102  (o la IP del nodo en cuestión)


Ahora, nuestra primera instrucción de ansible será comprobar que nodos tenemos en el inventario. En un terminal, escribimos:

````bash
ansible all -i inven.txt --list-hosts
````
- **all** significa que la operación --list-hosts se va a aplicar a todos los nodos que aparezcan en el inventario.
- **--list-hosts**  es la orden listar todos los hosts
- **-i inven.txt** indica el nombre del inventario que utilizaremos en la orden
  - Si se omite la opción -i , Ansible buscará hosts en el archivo de inventario /etc/ansible/hosts en su lugar.

La salida será algo así: 
- hosts (1):
  - nodo1


Si tuviéramos más de un nodo y quisiéramos agruparlos en categorías, etc en el ***fichero de inventario*** podríamos escribir algo parecido a esto:
````text
[pruebas] 
    nodo1

[produccion] 
    nodo2
````

Y ahora, si sólo quiero mostrar los hosts que tengo agrupados bajo el grupo producción: 
````bash
ansible produccion -i inven.txt --list-hosts
````

Si añadimos un nuevo nodo fuera de los grupos a nuestro inventario

````text
nodo3
[pruebas]
    nodo1
[produccion]
    nodo2
````

Ese nodo3 estaría dentro de un grupo especial que crea Ansible llamado ***ungrouped***. También existe otro grupo por defecto llamado **all**.
  - **Nota**: *cuidar que el nodo no agrupado esté al principio del fichero, porque si no, lo tomaría como si estuviera en el grupo produccion*
````bash
ansible ungrouped -i inven.txt --list-hosts
````

También se pueden crear grupos cuyo contenido sean otros grupos (children):

````text
[desarrollo] 
  nodo3
[pruebas] 
  nodo1
[produccion] 
  nodo2
[dev:children]
  desarrollo 
  pruebas
[prod:children] 
  produccion
````

La forma de llamarlos es similar a las anteriores: 

````bash
ansible dev -i inven.txt --list-hosts

ansible prod -i inven.txt --list-hosts
````

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


### Crear listas y diccionarios

#### Listas
- En Ansible se pueden utilizar listas (arrays) y diccionarios (pares clave:valor) para definir variables de varios valores. Por ejemplo, si queremos definir una serie de números de puerto:

```yaml
vars:
  port_nums: [21,22,23,25,80,443]
```

Por influencia de Python, los arrays se enmarcan entre [ ]

- Otra forma sería en forma lista según YAML:

````yaml
vars:

port_nums:
  - 21
  - 22
  - 23
  - 25
  - 80
  - 443
````

Otras formas de lanzar o ver elemento en Ansible 

- Muestra el último elemento
```yaml
port_nums[-1]
```
- Mostrar desde el tercero hasta el último:
```yaml
port_nums[2:]
```  

- Mostrar desde el tercero hasta el penúltimo: •
````yaml
port_nums[2:-1]
````  

Otra forma sería en forma lista según YAML:

````yaml
vars:
  port_nums:
    - 21
    - 22
    - 23
    - 25
    - 80
    - 443
````

Para imprimir todos los valores de **port_nums**: 

- **{{ port_nums }}**

Si queremos un elemento específico de la lista: 
- **{{ port_nums[0] }}**

Con el número 0 entre corchetes queremos indicar que hacemos referencia al primer elemento de la lista. (Igual que se hace en Python)

#### Diccionarios

Respecto a los diccionarios, aquí se muestra un ejemplo de definición del diccionario **usuarios**:

- {‘clave1’: valor, ‘clave2’: valor}

````yaml
vars:
  usuarios:
    juan:
      username: juan 
      uid: 1122
      shell: /bin/bash
    ana:
      username: ana 
      uid: 2233
      shell: /bin/sh
````

Sin embargo, para los diccionarios hay dos formas de acceder a sus elementos:

````yaml
usuarios['juan']['shell']
# -> /bin/bash

#ó

{{usuarios.juan.shell}}
````

### Incluir variables externas

Al igual que se puede importar (o incluir) tareas en un libro de jugadas. También se puede hacer lo mismo con las variables. Es decir, en un playbook, se puede incluir variables definidas en un archivo externo: **variables.yml**

Ahora, para incluirlo en un playbook, sólo hay que utilizar la palabra clave **vars_files** y colocar el nombre del fichero como valor:

```yaml
---
- name: variables externas
  hosts: nodo1
  # Aquí incluimos variables externas
  vars_files: variables.yml
  tasks:
    - name: Mostrar el segundo elemento de port_nums 
      debug:
        msg: El puerto SSH es {{port_nums[1] }}
    - name: Mostrar el uid de Juan
      debug:  
        msg: El UIDde Juan es {{usuarios.juan.uid }}
```

**vars_files** preprocesa y carga las variables al comienzo del playbook. Si lo que queremos es que se carguen a medida que van realizándose tareas (modo dinámico) utilizaremos **include_vars** así

```yaml
\---
- name: variables externas 
  hosts: nodo1 
  tasks:
  - name: Mostrar el segundo elemento de port_nums 
    debug:
      msg: El puerto SSH es cualquiera
  - name: Carga de las variables 
      include_vars: variables.yml
  - name: Mostrar el uid de Juan
    debug:
      msg: El UIDde Juan es {{usuarios.juan.uid }}
```

### Obtener información del usuario. 
Ansible permite solicitar información en tiempo de ejecución al usuario.
Se utiliza vars_prompt y espera a que el usuario introduzca la información y pulse ENTER:

````yaml
---
- name: saludo
  hosts: nodo1
  vars_prompt:
    - name: nombre
      prompt: ¿Cuál es su nombre?
      private: no
  tasks:
    - name: saludamos al usuario
      debug:
        msg: Hola {{nombre }}
````

El determinar **private: no**, es necesario si queremos ver qué estamos escribiendo, porque de lo contrario, private es yes y pasará como con las contraseñas.
