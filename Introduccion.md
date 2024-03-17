<h3>CLASIFICACION DE LAS REDES:</h3>

- Segun el modo en el que intercambian informacion, existen 2 tipos de redes:
    - Redes broadcast
        - Un solo canal de comunicacion compartido por todas las maquinas, los paquetes mandados por una maquina son recibidos por todas las maquinas
        - Hay que indicar el destinatario en los paquetes
    - Redes punto a punto
        - Muchas conexiones entre pares individuales de maquinas
        - La solucion para la comunicacion multiple es comunicar 2 equipos atravesando una maquina intermedia (routing)

- Segun la forma de conmutacion existen 2 tipos de redes:
    - Conmutacion de circuitos:
        - Posee un canal dedicado
        - Trabaja con reservas
        - Se cierra un circuito fisico
        - Ventajas:
            - Se conoce el retardo de la comunicacion
        - Desventajas:
            - Ineficiente
            - Los terminales estan obligados a transmitir a la misma velocidad
    - Conmutacion de paquetes
        - No posee canal dedicado
        - Se envia un paquete y 1 o varios intermediarios redirigen el paquete hasta que llega al destino
        - Sin reservas
        - Ventajas:
            - Eficiencia
            - Los terminales pueden transmisitr a diferentes velocidades
            - Puedes establecer una prioridad en los paquetes
        - Desventajas:
            - No fiable porque puedes perder paquetes
            - Tiemop de transmision acotado
            - Todos los paquetes deben tener toda su informacion para llegar a su destino

- Segun el tamaño, existen varios tipos de redes:
    - <b>PAN:</b> Redes de area personal
    - <b>LAN:</b> Redes de area local
    - <b>MAN:</b> Similares a las LAN pero mas grandes
    - <b>WAN:</b> Redes de area publicam, muy grandes