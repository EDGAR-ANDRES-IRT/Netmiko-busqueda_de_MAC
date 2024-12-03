# Netmiko-busqueda_de_MAC
Presenta un proceso de automatización para detectar y encontrar el puerto donde se localiza la dirección física (MAC) de un host en una red LAN con varios Switches mediante el uso de Netmiko. <br>

### Datos
Edgar Andres Hernandez Avila <br>
Conmutación y enrutamiento de redes <br>
M.C Eliud Bueno Moreno <br>
Universidad Politécnica de Durango

## Requisitos
Para utilzar el código es necesario tener la libreria "Netmiko" correctamente instalada. Se puede instalar mediante el comando 'pip install Netmiko'.<br>
Las configuración de la red LAN requiere:<br>
~ Interconexión entre switches mediante enlaces troncales.<br>
~ Las credenciales de usuario y constraseña deben ser iguales en todos los switches
~ Habilitar SSH en cada switch.<br>
~ Habilitar CDP RUN en cada switch.<br>
~ La existencia de una Vlan de administración para acceder a cada swtich por medio de SSH.<br>
~ Asignar una SVI dentro del mismo segmento de red para cada switch.<br>
~ La computadora donde se ejecutará el código, tenga configurada un IP válida dentro del segmento de administración y sea capaz de acceder a cada switch mediante SSH.<br>

## Modo de uso
1.- Al comienzo del programa, se declara un diccionario donde se ingresan los parametros de la conexión al switch que se desea conectar inicialmente.<br>
![image](https://github.com/user-attachments/assets/d868e75e-74f6-47ad-8462-7a7ab515de64)<br>
Ajustar los parámetros según sea necesario.<br>
<br>
2.- Enseguida, se encuentra una variable donde se declara la dirección MAC a encontrar. Ingresar en formato xxxx.xxxx.xxxx, en minúsculas.<br>
![image](https://github.com/user-attachments/assets/4c28b7a4-7003-4ded-9f18-75ed3f3ef920)<br>

## Explicación
El programa se conecta al dispositivo principal por SSH para enviar el comando 'show mac address-table', verifica en la salida de texto con la función '.find', si la MAC a buscar se encuentra en alguna interfaz. De no encontrar la dirección, retorna un -1 terminando el progama e imprimiendo un mensaje de 'MAC no encontrada'. En caso de contrario, retorna la interfaz (de manera abreviada) donde localizó la MAC.<br><br>
Enseguida, obtiene el nombre completo de la interfaz donde localizó la MAC. Para esto envía un 'show interface ----' con la interfaz del resultado de la función anterior. Retorna la primera palabra de la salida de texto (siempre es la interfaz). <br><br>
Ahora, envía el comando 'show cdp neighbors detail' donde se muestra información detallada de los vecinos CDP. Se recorre la salida en busca de la interfaz. <br>
  Si el programa encuentra la interfaz, regresa la IP de conexión hacia del dispositivo vecino.<br>
  En caso de no encontrar la interfaz, signifca que el host con la MAC se encuentra en tal interfaz, retornado -1 e imprimiendo un mesanje final "La MAC se encuentra en x puerto de x dispositivo".<br><br>

Si el programa continúa ejecutandose, crea un diccionaro para la siguiente conexión al dispositivo vecino con la IP resultante de la función anterior. Establece conexión, y repite el bucle hasta encontrar la MAC en un puerto sin vecinos CDP.<br>





