# CAPA DE ENLACE

<style>
h3{
    color: #93a2f2;
    font-weight: bold;
    font-size: 20px;
}
</style>
<h3>Se encarga de:</h3>

- Sincroniza las tramas, donde acaba y comienza cada bloque de información
- Manejar los errores de transmisión, siempre hay errores y se encarga de amortiguarlos
- Regular el flujo de datos, para adaptar las diferentes velocidades de transferencia de las máquinas
- Control de acceso al medio, se encarga del direccionamiento

<h3>Arquitectura IEEE</h3>

- Se subdivide en:
    - Subcapa LLC
        - Control de errores
        - Control de flujo
    - Subcapa MAC
        - Control de acceso al medio
        - Sincronización de tramas
- Estándares actuales:
    - Ethernet (IEEE 802.3)
    - Wi-Fi (IEEE 802.11)
        - Problemas de mac - inalambricas
            - Nodo oculto
                - Se da cuando dentro de una red un punto no se encuentra con el otro
            - Nodo expuesto
                - Ocurre cuando un dispositivo quiero transmitir, pero como otro dispotivo esta transmitiendo, no necesariamente al primero, cree equivocadamente que el canal esta ocupado

![IEEE estándares](IEEE_estandares.png)

<h3>ENTRAMADO</h3>

- Entramado se refiere a la conexión de 2 dispositivos para la comunicación a nivel de enlace
- Se subdivide en:
    - Capa física
        - 1 flujo de bit
        - Siempre tiene errores
    - Capa de enlace
        - Divide el flujo de bits en una serie de tramas
        - Añade información redundante
        - Se comprueba si es correcta en el endpoint
- Algunas soluciones:
    - Dejar espacio entre las tramas
        - Problemas de temporalización
    - Conteo de caracteres
        - Cuenta el número de caracteres de la trama, para enviar en la cabecera
        - Se puede perder el entramado si se produce un error de transmisión
        - No se utiliza actualmente
    - Banderas, con relleno de caracteres
        - Se establecen unos bytes especiales al principio y al final de la trama, que denominamos flags
        - El problema es que en la carga útil se encuentra la misma secuencia de bytes especiales FLAG
        - Otro problema es que se utiliza relleno de caracteres ESC, por lo que pueden encontrarse en la secuencia de datos
    - Banderas, con relleno de bit
        - Se utiliza la siguiente FLAG al principio y al final de la trama: 01111110
        - Un problema que tiene es si la bandera se encuentra en la carga útil
        - Otro problema es cuando se encuentran 5 1s en el flujo de bit, se inserta automáticamente un 0
    - Violación de codificación de la capa física
        - Solo aplicable a redes con un medio físico situacional
        - 1 alto-bajo
        - 0 bajo-alto
        - Utilizar el 1 y el 0 para delimitar la trama

<h1>LLC</h1>
<h3>CONTROL DE ERRORES</h3>

- La capa física siempre es susceptible al error en la transmisión de los bits
- Deteccion de errores
    - Existen 2 métodos de deteccion de errores:
        - FEC(Forward Error Correction): Recupera información original sobre la marcha
        - ARQ(Automatic Repeat Request): Detectar errores y solicitar la retransmisión de los datos
    - Se identifica con:
        - Bit de paridad
        - Codigo de redundancia ciclica CRC
- Correcion de errores
    - Para la correcion de errores:
        - Se añade información redundante para codificar la informacion
        - Hamming establece las bases de la teoria de codificacion, que son:
            - Codigos para la deteccion de errores
            - Codigos para la correcion de errores
- Retransmision
    - Se necesita retroalimentacion
    - El receptor envia tramas de control especiales
    - Existen 2 estrategias basicas
        - Parada y espera
            - Se envia una trama y se espera confirmacion del otro extremo
                - ACK: Confirmacion positiva
                - NACK: Confirmacion negativa
            - Es necesario introducir temporizadores
                - Si se pierde una trama:
                    - Se espera a que acabe el temporizador (Contador temporal) para evitar que se pierda el mensaje y se vuelve a enviar
                - Si se pierde una confirmacion:
                    - Se espera a que acabe el temporizador (Contador temporal)
                    - Llegan tramas duplicadas
            - Es necesario numerar las tramas (1 bit)
        - Flujo continuo
            - Mayor desafio
            - Se envian mas mensajes seguidas sin esperar confirmacion
            - Existen 2 estrategias a la hora de un error de transmision:
                - REJ: Rechazo simple:
                    - Se vuelven a enviar las tramas desde la trama que fallo
                - SREJ: Rechazo selectivo
                    - Se envian solo las tramas que fallo
- Conotrol de flujo
    - Es la estrategia que sse usa para que la entidad emisora no sobrecargue a la receptora
        <ol>
            <li> La receptora reserva memoria</li>
            <li> Se realiza el procesamiento de los datos</li>
            <li> Pasa los datos al nivel superior</li>
        </ol>
    - La memoria del receptor podria llenarse mientras procesa los datos anteriores
    - Existen 3 estregias para el control de flujo:
        - Parada espera:
            - Es muy poco eficiente
        - Parada brusca:
            - El receptor manda un mensaje de parada cuando se satura
            - Manda uno nuevo cuando no esta saturado
        - Ventanas deslizantes:
            - Tanto el receptor como el emisor reservan un espacio para el envio de tramas, cuando se envia una trama, la ventana aumenta, y cuando se recibe confirmacion se disminuye
            - Cada una de las tramas se etiquete con su CRC
            - Piggybacking: Estrategia en la que ademas de mandar la trama, se manda la trama que se esta esperando
            - HDLC:
                - Es un protocolo en el que se basan otros protocolos importantes
                - HDLC define:
                    - 3 tipos de estaciones:
                        - <b>Estacion primaria:</b> Las tramas se denominan ordenes, control el funcionamiento del enlace
                        - <b>Estacion secundaria:</b> Las tramas se denominan respuestas, funcionan bajo la estacion primaria
                        - <b>Estacion combinada:</b> Una mezcla de las otras 2
                    - 2 tipos de configuracion de enlace:
                        - <b>Configuracion no balanceada:</b> Formada por una estacion primaria y una o mas secundarias
                        - <b>Configuracion balanceada:</b> Formada por 2 estaciones secundarias
                    - 3 modos de operaciones:
                        - <b>Modo de respuesta normal (NRM):</b> La estacion primaria puede iniciar la transferencia de datos, pero la secundaria solo puede responder. Se usa en la no balanceada
                        - <b>Modo balanceado asincrono (ABM):</b> Cualquier estacion combinada podra iniciar la transmision sin necesidad de recibir permiso por parte de la otra. Es la mas utilizada
                        - <b>Modo de respuesta asincrono (ARM):</b> La estacion secundaria puede iniciar la transmision sin permiso explicito de la primaria. Se una en la no balanceada
                - Estructura de la trama:
                    - Campo delimitador
                        - Esta en los extremos de la trama y corresponde al patron 01111110
                        - Utiliza relleno de bits para solucionar el problema de que la estructura se encuentre dentro del contenido
                    - Campo de direccion:
                        - Identifica la estacion de recepcion, en las tramas solo existe la direccion destino no la origen. Ocupa 8 bits (1 byte)
                     - Campo de control:
                        - Identifica el tipo de trama, en base al primer o los 2 primeros bits.
                            - <b>Tramas de informacion (I):</b> Transportan informacion
                            - <b>Tramas de supervision (S):</b> Mecanismo para consentimiento de envio 
                            - <b>Tramas no numeradas (N):</b> Funciones no complementarias
                        - Un campo de control normal utiliza numeros de secuencia de 3 bits, de 8 que son; Se puede usar de forma ampliada usando 7 bits
                    - Campo de comprobacion de trama, codigo de redundancia ciclica (CRC):
                        - Puede llegar a ocupara 32 bits
                    - Campo de sondeo:
                        - Se utiliza dependiendo del contexto:
                            - Por ejemplo, en las tramas de respuesta, se fija a 1 para indentificar que es de tipo respuesta
                - Funcionamiento:
                    <ol>
                        <li>Se inicia la comunicacion</li>
                        <li>Se intercambian datos</li>
                        <li>Se termina la comunicacion</li>
                    </ol>
            - Otros protocolos:
                - <b>LAPB:</b>
                    - Procedimiento de acceso balanceado
                    - Modo balanceado asincrono
                    - Es un subconjuto de HDLC
                - <b>LAPD:</b>
                    - Procedimiento de acceso sobre canal
                    - Modo balanceado asincrono
                    - Recomendado para RDSI

    <h1>MAC</h1>
    <h3>CONTROL DE ACCESO AL MEDIO</h3>

    - Solo son necesarios en redes Broadcast
    - 2 tipos de estrategias:
        - <b>Estatica:</b> Asignacion fija de recursos a cada participante
            - Faciles de implementar
            - Poco adecuado para trafico a rafagas
            - Pueden ser:
                - MDF: Multiplexion por division en frecuencia
                - MDT: Multiplexion por division en tiempo
        - <b>Dinamica:</b> Asignacion dinamica de recurso a los participantes
            - Distribuye los recursos mas eficientemenete
            - Estrategias:
                - <b>Contienda</b>
                    - Entran en conflicto con el resto para el uso de recursos
                    - Una de ellas gana y puede transmitir
                    - Ejemplos:
                        - ALOHA
                            - ALOHA puro:
                                - Pueden transmitir datos en cualquier momento
                                - Las estaciones transmiten cuando tienen datos que transmitir
                                - Se producen colisiones
                                - Se retransmiten de forma aleatoria
                            - ALOHA ranurado:
                                - El tiempo se divide en ranuras de tamaño fijo
                                - Reduce el periodo vulnerable de la trama
                                - Se obliga a esperar el comienzo de una ranura para transmitir

                        - CSMA (Deteccion de portadora)
                            - Puede haber una colision si 2 estaciones transmiten a la vez, se puede dar el caso si una estaciones empieza a transmitir
                            - Tipos de criterios si el canal esta ocupado:
                                - CSMA persistente
                                    - Escucha constantemente hasta que esta libre y procede a transmitir
                                    - Es el que usa Ethernet
                                - CSMA no persistente
                                    - Escucha el canal de forma aleatoria, cuando ve que esta libre envia
                                - CSMA p persistente
                                    - Escucha contantemente, y cuando queda libre envia con probabilidad p
                                - CSMA con deteccion de colisiones
                                    - Permite abortar una transmision si hay una colision
                                        - Despues de abortar espera un tiempo aleatorio y transmite de nuevo
                                    - Periodos:
                                        - Transmision
                                        - Contencion
                                            - En este periodo ocurren las colisiones
                                        - Inactividad
                - <b>Reserva</b>
                    - Se pide una reserva de recursos
                    - Se reserva
                    - Espera su turno
                    - Ejemplo:
                        - Mapa de bits
                            - No tiene colisiones
                - <b>Asignacion</b>
                    - Se selecciona una unica estacion que puede utilizar el medio
                    - Tipos en base a quien decide la utilizacion quien usa el medio:
                    - No tienen colisiones
                        - Centralizada:
                            - Una maquina se encarga de tomas las decisiones
                        - Distribuida:
                            - Mas equitativa
                            - Funciona con un paso de testigos, quien lo posea puede transmitir
                    - Ejemplos:
                        - Token Ring
                        - Token Bus
                    - Problemas:
                        - Modelo de estacion, porque cuando se genera una trama, no hace nada hasta que se ha transmitido con exito
                        - Puede haber un canal unico, al ser un canal unico para todas las estaciones
                        - Pueden ocurrir colisiones, si 2 estaciones transmiten simultanemanete generan una o varias colisiones
                        - Tiempo continuo, no hay un reloj maestro
                        - Tiempo discreto, puede haber colisiones si hay mas de 1 trama por ranura slot
                        - Deteccion de portadora, una estacion puede detectar si el canal esta en uso
                        - Sin deteccion de portadora, la estacion no puede detectar si el canal esta en uso

<h1>TCP/IP</h1>
<H3>PROTOCOLOS DE NIVEL DE ENALCE TCP/IP</H3>

- ETHERNET
    - Sincronizacion de relojes de todas las estaciones
    - Identifican de forma univica el hardware de las estaciones
- PPP
    - Identifican de forma univica el hardware de las estaciones
    - Lo necesita internet para
        - Trafico encaminador a encaminador
        - Trafico usuario domestico
    - Permite la negociaciones de direcciones IP
    - Autenticacion
- SLIP



        

<h3>TIPOS DE MENSAJES DE HDLC</h3>

<ul>
    <li>NO NUMERADOS</li>
        <ol>
            <li>
                SAMB: Inicia la conexión
            </li>
            <li>
                DISC: Desconexión
            </li>
            <li>
                UA: Confirmas conexión o desconexión
            </li>
            <li>
                DM: Rechaza la conexion
            </li>
        </ol>
    <li>NUMERADOS</li>
        <ol>
            <li>
                SREJ: Rechazo selectivo
            </li>
            <li>
                REJ: Rechazo simple
            </li>
            <li>
                RR: Receive ready, se puede usar con flag
            </li>
                <ul>
                    <li>
                        RRP: Pregunta, que obliga a la respuesta
                    </li>
                    <li>
                        RRR: Respueta, obligada por la pregunta
                    </li>
                </ul>
            <li>
                RNR: Receive not ready
            </li>
        </ol>
</ul>
