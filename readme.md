## Apuntes del bootcamp
***

Temas

1. Carpeta ssh para instrucciones de instalacion de ssh en ubuntu.<br><br>

2. Carpeta ansible para instrucciones de instalcion de ansible y su funcionamiento: <br><br>

# Ejercicio 1

Terminar la ejecución del ejercicio que ejecuta un fichero main.py.

# Ejercicio 2

El módulo File, permite realizar las operaciones que trabajan con archivos y directorios, así como la de asignar
permisos. Buscar algún ejemplo de cómo modificar la fecha de modificación y de acceso de un fichero a la fecha de hoy.

*Solución planteada*<br>
Ejecutamos  'ansible-playbook modify.yml'

El fichero modify.yml tiene el siguiente código

````yml
- name: Update modification and access time of given file
  hosts: nodo1
  vars:
    ruta: /home/otrou/borrara/micarpeta/fichero.txt
  tasks:
    - name: fecha de creacion y modificacion
      shell: "ls -l {{ ruta }}"
      register: info_file1
    - name: imprimir informacion del fichero antes de modificarlo
      debug:
        msg: "{{ info_file1.stdout }}"
    - name: Update modification and access time of given file
      ansible.builtin.file:
        path: "{{ ruta }}"
        state: file
        modification_time: now
        access_time: now
    - name: fecha de creacion y modificacion
      shell: "ls -l {{ ruta }}"
      register: info_file2
    - name: imprimir informacion del fichero despues de modificarlo
      debug:
        msg: "{{ info_file2.stdout }}
```` 

Y obtenemos esta salida.
![Imagen del resultado ejercicio2](./img/salida.png)

> Nota: Si consigues el ejemplo en forma de tarea de playbook, convertirlo a formato de comando de ansible. 
> 
> **ansible nodo1 -m ansible.builtin.shell -a "touch -a -m /home/otrou/borrara/micarpeta/fichero.txt"**




# Ejercicio 3 (opcional)

Conectar el repositorio de GitHub. 

     
         ```yml
---
- name: Clonar repositorio de Git en servidor remoto
  hosts: nodo1
  become: true

  vars:
    git_repo_url: https://github.com/tu-usuario/tu-repositorio.git
    git_branch: main
    git_dest_dir: /home/ubuntu/mi-proyecto

  tasks:
    - name: Instalar Git
      apt:
        name:
          - git
        state: present

    - name: Clonar repositorio de Git
      git:
        repo: "{{ git_repo_url }}"
        version: "{{ git_branch }}"
        dest: "{{ git_dest_dir }}"
     
````
         
        