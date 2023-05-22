## DESCRIPCIÓN


Se disponen de **3 roles**:

- **prometheus**: realiza una instalación del servicio prometheus mediante la descarga y utilizacion del paquete de github de Prometheus (https://github.com/prometheus/prometheus/releases/) y la posterior creación de este servicio.

- **node_exporter**: realiza una instalación del servicio node_exporter mediante la descarga y utilizacion del paquete de github de node_exporter (https://github.com/prometheus/node_exporter/releases/) y la posterior creacion de este servicio.

- **añadir_nodo**: Este rol agrega los nodos con node_exporter al servidor prometheus mediante la edición del fichero de configuración de prometheus. En concreto añade las lineas con los targets de los hosts que se desean añadir.


En cada rol existe un README.md donde se explica particularmente cada uno de ellos más especificamente.




## INVENTORY


Para la correcta ejecución de estos roles se debe tener en el inventario un grupo llamado 'NODE_EXPORTER' con los hosts donde se instalará node_exporter y que posteriormente se agregarán al servidor Prometheus. Este grupo de inventario es necesario para la ejecución del rol 'añadir_nodo' ya que el rol lo utiliza para identificar los hosts a agregar al servidor prometheus.


 Ejemplo de inventario:

		[PROMETHEUS]
		server

		[NODE_EXPORTER]
		nodo1
		nodo2
		nodo3




## PLAYBOOK


Ejemplo de playbook.yml para la ejecución de estos roles:


        - name: Instalación node_exporter
          hosts: NODE_EXPORTER
          become: true
          roles:
            - role: node_exporter

        - name: Instalación y configuracion Prometheus
          hosts: PROMETHEUS
          become: true
          roles:
            - role: prometheus
            - role: añadir_nodo       


    
El rol 'añadir_nodo' se puede usar individualemente más adelante para agregar nuevos nodos de node_exporter al servidor prometheus. Unicamente, se debe añadir en el grupo 'NODE_EXPORTER' el hostname o ip address de este nuevo nodo a incluir.

