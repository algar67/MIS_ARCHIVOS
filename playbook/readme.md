Hay que copiar la clave publica del nodo donde va a clonarse el repositorio.

Para saber la clave publica tenemos que ejecitar el comando  **cat ~/.ssh/id_rsa.pub**

y nos saldrá algo parecido a esto

ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQC/+/K4XFwRI3x2mdMzdKdaLPkUu1rnRwmaQ2Sc15JEpo4O7CGjhJ6Ak5tR7oFK6/AQ9U1ZccU0RR9ZliImvXfUwgtVta09p+Dcl2U8kyN7mnhEXgcMeKmCFei+5yvZm7fA7DXa8yhAXPT8f1E3nW7edv2tJHg0Gpr03KTdVVr+3XEPMbbcc5FlxVMdRSZLhZTnUab6AmFyvwJ2ofyp0K/44VnOTtz44jlXzkRfNI0iWFUYw5mvgiabFQdpkijv5vQmEb36h+MC66Q1a+t35gGZrv8HljngOqY7X1czgq3o7OqUCjPfQG8vP+73BI9OORkWc82wKbaEorNhKkvAJJ9IQJhDRpzNgZ5ez3pnMqxr0p46GVobBwrM8nPPaKmuVYCF/qfFqSYuNR6zlTuOeG5yKdyf8DW3BeCdZxXBXGO4R2ZDYIo6KonCE1OkjcX0NK3cNpPKsLn+E/p19Efj958fUm4kIKHYYkkf7wGosCCsSx8ejw7+1t33oNvPMLjjgIc= otrou@nodo2


Configuro el fichero .gitconfig de la siguiente manera ( si no esta lo creo)
(fichero nodo 1 en ~/.gitconfig       /home/otrou/.gitconfig)                           

````
[credential]
        helper = cache

[github]
    user = algar67
    token = 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
 ````


El codigo de un playbook en yaml para clonar un repositorio Git privado por SSH
 (git_ssh.yml)

````
---
- name: Clone de un repositorio GIT vía SSH
  hosts: nodo2
  tasks:
      - name: Clone del repositorio GIT
        git:
            repo: git@github.com:isotomor/thebridgecdptenero23.git # Aquí deberéis poner el repo que estamos usando
            dest: /home/otrou/gilocal33/thebridgecdptenero23
            accept_hostkey: true
````
