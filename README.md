            ██████╗ ██╗███╗   ██╗ ██████╗ ███╗   ███╗███████╗██████╗ ██╗     ███████╗      ██╗
            ██╔══██╗██║████╗  ██║██╔════╝ ████╗ ████║██╔════╝██╔══██╗██║     ██╔════╝     ██╔╝
            ██████╔╝██║██╔██╗ ██║██║  ███╗██╔████╔██║█████╗  ██████╔╝██║     ███████╗    ██╔╝ 
            ██╔═══╝ ██║██║╚██╗██║██║   ██║██║╚██╔╝██║██╔══╝  ██╔═══╝ ██║     ╚════██║    ╚██╗ 
            ██║     ██║██║ ╚████║╚██████╔╝██║ ╚═╝ ██║███████╗██║     ███████╗███████║     ╚██╗
            ╚═╝     ╚═╝╚═╝  ╚═══╝ ╚═════╝ ╚═╝     ╚═╝╚══════╝╚═╝     ╚══════╝╚══════╝      ╚═╝



<details>
<summary>Castellano</summary>
<br>
> ! ESTE PROYECTO ESTA EN UNA FASE INCIAL, SEAN COMPASIVOS :P

  Sobre este proyecto:
-
PMP tiene como objetivo ser un recurso de aprendizaje de código abierto, que ayude aquellos que buscan ver cómo funcionan los protocolos dentro de un entorno controlado y seguro. Este proyecto no es perfecto, no está hecho por profesionales, soy un estudiante que se encontró una curva de aprendizaje bastante inclinada cuando comenzo en el mundillo de las redes, y que quiere probeer de herramientas a aquellos que vienen detrás :)
Si tienes experiencia en este tema y hay algo que crees que está mal, hazmelo saber, el feedback siempre se agradece; si quieres colaborar de forma activa, eres bienvenido, ve a la sección de cómo publicar cambios para saber más.

El proyecto incluye 4 sistemas autónomos: (Esta descripción está incompleta, se añadirá una explicación detallada en el futuro)
> **ISP 55:** Este es el más complejo de los 4, solo permite el tráfico ipv4 de forma predeterminada, pero implementa mpls y túneles 6rd para permitir el tráfico ipv6.
> **ISP 2000:** Estos son los nuevos en la industria, solo permiten ipv6 de forma predeterminada, pero se implementan túneles ip4ip6.
> **ISP 3000:** Al igual que ISP 55, solo permiten el tráfico ipv4 de forma predeterminada, pero usando 6pe, el tráfico ipv6 puede atravesarlo.
> **ISP 100:** Este ISP que tiene como objetivo simular el resto de Internet.

El color del punto final indica:
> **Amarillo:** Cliente con dirección ipv4 pública.
> **Verde:** Cliente con dirección ipv4 privada (CGNAT).
> **Rojo:** Cliente con rango Ipv6 asignado.
> - Círculo verde: Túnel a través de CGNAT.
> - Círculo amarillo: Túnel con salida publica.
>
> **Círculo azul**: Cliente conectado a ISP 3000 mediante PPPoE.

Otras características son:
>Servidor dhcp centralizado, rbgp dual (el servidor dhcp también actúa como rbgp) 2 vpn separados administrados por vrfs, ospf, ibgp y ebgp. (Probablemente me estoy perdiendo algo)

Primeros pasos:
-
En primer lugar, necesitas gns3, puedes instalarlo aquí y el archivo IBS.gns3project, las imágenes del firmware de los routers deberían estar incluidas, pero también puede encontrarlas aquí. También se recomienda wireshark, ya que se puede usar de forma nativa en gns3 para analizar el tráfico, puedes encontrarlo aquí.

Si esta es tu primera vez con gns3 y un proyecto de este tamaño, calma... pilla un café, y sientate comodo, gns3 se toma su tiempo para cargar y comenzar, así que no entres en pánico si no ves que las cosas se muevan al instante.. Gns3 tiene una arquitectura cliente-servidor, es por eso que quizás veas ventanas emergentes que intentan conectarse a un servidor en localhost.

Después de abrir el proyecto, cuando la ventana emergente de carga desaparezca, deberías estar listo para pulsar el botón de play (el verde en la parte superior izquierda) y los enlaces deberían comenzar a cambiar de rojo a verde. Lleva su tiempo, muchos protocolos que se ejecutan desde cold-start, así que espera unos minutos para asegurarte de que todo esté listo.

Pues ya tiene todas las interfaces con ips configuradas y sus tablas de rutas están llenas, es hora de jugar.
Primero puedes probar el camino más largo; desde una terminal de usuario final (haga doble clic en una PC verde, se debe abrir una terminal), escribe:

> **ping 50.0.0.2** (el servidor en la parte inferior)

Si no funciona, no te preocupes, ¿ves un mensaje "DDD" en el terminal de PC? Significa que el primer descubrimiento de dhcp falló, esto sucede porque el PC finaliza su configuración antes que el CPE, por lo que dhcp no se carga a tiempo. Para resolverlo solo escribe:
> **dhcp**

Si ahora ves DORA, significa que completó todos los pasos de descubrimiento, oferta, solicitud, reconocimiento, ahora puedes repetir el ping.
Si deseas analizar los paquetes en cualquier enlace (requiere wireshark), simplemente haz click derecho sobre él y haz clic en iniciar captura, debería aparecer una lupa sobre el enlace, recuerda que cerrar Wireshark no detiene la captura, si la lupa está ahí, la captura se está ejecutando y consumiendo recursos, recuarda esto, te salvará de crasheos inesperados.

Comandos básicos del terminal ios:
-
Si deseas profundizar, debe saber cómo usar el terminal ios, los aspectos más básicos son:
> **show ip interface brief** o **sh ip int b**
> **sh ip route**

Entrando al modo de configuración dglobal:
> **configurar terminal** o **conf t**

Pero la palabra clave más importante es "?", escribe "?" después de cualquier comando y obtendrá todas las opciones que puedes usar, usa ***enter*** y ***space*** para avanzar.

Revisar la configuración del router:
-
Para ver la configuración de cualquier router con el proyecto parado, puedes hacer click derecho en el router deseado e ir a "edit config". Si el proyecto se está ejecutando, también puedes escribir en el terminal:
> **show run** o **sh run**

y usa ***enter*** y ***space*** para avanzar.

Ttrabajando en:
-
- Permitir que los clientes tengan diferentes rangos de ipv6 asignados (lo que permite a las empresas tener rangos más grandes), implica la configuración de un túnel 6rd a una anycast en los border router.


TO-DO:
-
- Servidores DNS


Cómo publicar cambios:
-
(Trabajando en ello)
</details>

<details>
<summary>English</summary>
<br>
> ! THIS PROJECT IS IN A REALLY EARLY STAGE, PLEASE BE COMPASSIONATE :P

About this project:
- 
PMP aims to be an opensource learning resource, that helps new people to see how protocols operates inside a controlled and secure environment. This project is not perfect, is not made by professionals, I'm an student myself that found a fairly steep learning curve when I wanted to start in the world of networks, so I wanted to help others.
  
If you have experience in this topic, and there is something you think is wrong, please let me know, feedback is great; also if you want to collaborate, you are welcome, go to the how to publish changes section to know more.

The project includes 4 autonomous system: (This description is work in progress, an in depth explanation will be added)
> **ISP 55:** This is the more complex one, it only allow ipv4 traffic by default, but implements mpls and 6rd tunnels to allow ipv6 traffic. 
> **ISP 2000:** This are the new guys, they only allow ipv6 by default, but ip4ip6 tunnels are deployed.
> **ISP 3000:** Like ISP 55, they only allow ipv4 traffic by default, but using 6pe tunnels ipv6 traffic can go throw.
> **ISP 100:** This is a dual stack isp that aims to simulate the rest of internet.

The end-point color indicates:
> **Yellow:** Client with public ipv4 address.
> **Green:** Client with private ipv4 address (CGNAT).
> **Red:** Client with Ipv6 range assigned.
> - Green circle: Tunnel throw cgnat.
> - Yellow circle: Public tunnel.
> 
> **Blue circle**: Client connected to ISP 3000 throw PPPoE. 

Other features are:
>Cgnat on green endpoints, centraliced dhcp server, dual rbgp (the dhcp server also act as rbgp), PPPoE that allows ClientPPP to acces the ISP 3000 network, 2 separated vpns managed by vrfs, ospf, ibgp and ebgp. (Probably Im missing something)

Start walking around:
-
First of all, you need gns3, you can install it here and the IBS.gns3project, the router's firmware images should be included, but you can find them here too. Also wireshark is recomended, since it can be used natively in gns3 to analyze traffic, you can find it here.

If this is your first time with gns3 and a project of this size, stay calm... btw go take a coffee, it takes its time to load and start, so don't panic if you don't see things doing things instantly. Gns3 has a client-server arquitecture, thats why you see popups trying to connect to a server in localhost.

After opening the project, when the loading popup is gone, you should be ready to hit the play button (the green one on top left) and links should start to change from red to green. It takes a while, many protocols running from cold start, so give it a few minutes to make sure everything its ready.

So you have all interfaces with ips configured and your route tables are filled, its time to play.
First you can test the longest path; from an end user terminal (double click on a green PC, a terminal should open), write :

>  **ping 50.0.0.2** (the server at the bottom)

If it doesn't work, no worries, do you see a "DDD" message on the PC terminal? It means the first dhcp discovery failed, this happens due to the PC ending its configuration before the CPE does, so dhcp is not working on time. To solve it just write:
> **dhcp**

If you now see DORA, it means it compleated all the discovery, offer, request, ack steps, now you can repeated the ping.

If you want to analyze the packets in any link (wireshark required), just right click on it and click on start capture, a lens should appear over the link, remember that closing wireshark doest stop stop the capture, if the lens is there, wireshark is running and consuming resources, take care of that.

Basic ios terminal commands:
-
If you want to dig in, you need to now how to use the ios terminal, the most basics are:
> **show ip interface brief** or **sh ip in b**
> **show ip route**

Entering global config mode:
> **configure terminal** or **conf t**

But the most important keyword is "?", write "?" after any command and you will get all the options you have, use ***enter*** and ***space*** to advance. 

Review router configuration:
-
To check the configuration of any router with the project not running, you can right click on the desired router and go to edit config. If the project is running, you also can write:
> **show run**

and use ***enter*** and ***space*** to advance.

Currently working on:
-
- Allow clients to have different ipv6 ranges assigned (allowing companies to have bigger ranges), involves 6rd tunnel configuration with anycast to bot border routers.


TO-DO:
-
- DNS servers


How to publish changes:
-
(Working on this)
</details>
