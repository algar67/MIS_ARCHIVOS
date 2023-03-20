## Como crear superusaurio llamado otrou

Recordamos que si queremos cambiar el nombre del usuario de otra máquina, eliminando el anterior, una lista de comandos para ello puede ser esta:

**sudo adduser otrou**
  Se ingresaría la contraseña, etc. Después lo agregaríamos al grupo sudo

**sudo adduser otrou sudo**
  Estableceríamos la contraseña de root y después reiniciaríamos.

**sudo passwd root**
  Iniciaríamos con el nuevo usuario y accederíamos como root

**su root**
**passwd -l root**
  Reiniciamos de nuevo y volvemos a entrar con el nuevo usuario y eliminamos el anterior

**sudo deluser usuario**
  Por útlimo borramos el perfil y así se conserva el nuevo nombre asignado

**sudo rm -r /home/usuario**
