# Informe de diseño · Interconexión de redes multisede

El proyecto de Redes I plantea una red corporativa para un escenario académico basado en Facebook. La implementación en Cisco Packet Tracer conecta seis sedes. Las LAN se dividen por departamento, y la WAN permite intercambiar rutas entre sedes con enlaces alternativos. El modelo físico muestra dónde se ubican los equipos y por dónde discurre el cableado.

## Dimensionamiento de las sedes

El enunciado establece una estimación por sede y pide reservar un **25 % de crecimiento**. El equipo repartió esa capacidad entre cinco VLAN. Las cantidades por área son estimaciones de diseño del grupo. La fuente da totales por sede y no detalla una plantilla real por departamento.

| Sede | Función en el escenario | Hosts base | Mínimo con 25 % | Hosts previstos en el plan | Tamaño de las cinco VLAN |
| --- | --- | ---: | ---: | ---: | --- |
| Menlo Park | Sede central | 600 | 750 | 780 | Cinco `/24`, de 155 a 157 hosts por VLAN |
| Dublín | Oficina regional de Europa | 350 | 438 | 468 | Cinco `/25`, de 74 a 106 hosts por VLAN |
| Singapur | Oficina de Asia-Pacífico | 250 | 313 | 343 | Cinco `/25`, de 68 a 69 hosts por VLAN |
| São Paulo | Oficina de Latinoamérica | 200 | 250 | 280 | Cinco `/26`, de 55 a 57 hosts por VLAN |
| Prineville | Centro de datos | 100 | 125 | 155 | Cuatro `/26` y una `/27`, de 30 a 32 hosts por VLAN |
| Luleå | Centro de datos | 100 | 125 | 155 | Cuatro `/26` y una `/27`, de 30 a 32 hosts por VLAN |

El plan ordena las subredes de mayor a menor, asigna bloques alineados dentro de `3.0.0.0/8` y elige el menor número de bits de host que cumple `2^h − 2 ≥ demanda`. Restar dos deja fuera las direcciones de red y broadcast. Por ejemplo, una VLAN de Singapur con 69 hosts supera las 62 IP utilizables de un `/26`, de modo que recibe un `/25` con 126. São Paulo, con un máximo de 57 por VLAN, sí cabe en `/26`. Un enlace serial punto a punto requiere solo dos IP de interfaz. El `/30` proporciona esas dos IP en un bloque de cuatro.

Las dos redes `/27` ofrecen 30 direcciones utilizables. Después de reservar dos IP de router y una IP virtual HSRP, quedan como máximo 27 para otros equipos. El mínimo de 25 % se cubre como estimación agregada por sede, con este límite en las dos VLAN más ajustadas. Las 83 estaciones dibujadas representan puestos de la simulación. El direccionamiento reserva capacidad para el escenario completo. [Consultar las 30 VLAN y los 27 circuitos](vlsm.md).

## Segmentación y enrutamiento local

Las VLAN **10 Administración**, **20 Ingeniería**, **30 Soporte Técnico**, **40 Seguridad** y **50 Desarrollo** separan los dominios de broadcast. Los puertos de acceso asignan cada estación a su área. Los enlaces troncales transportan varias VLAN donde el recorrido lo requiere. Por ejemplo, el trayecto entre Soporte y el switch principal puede transportar también Desarrollo, mientras el enlace hacia el switch de Desarrollo se limita a esa VLAN y la VLAN nativa 99.

Cada router utiliza subinterfaces 802.1Q para proporcionar una interfaz de capa 3 a cada VLAN sobre un solo enlace físico. Con este esquema, el tráfico entre departamentos pasa por el router y las cinco subredes locales aparecen como **directamente conectadas**. El enunciado propone enrutamiento estático dentro de la sede. La simulación resuelve este caso con las redes conectadas de las subinterfaces, sin entradas `ip route` configuradas.

## WAN y continuidad de la puerta de enlace

Los 27 circuitos seriales interconectan los 12 routers de la simulación. **RIPv2** anuncia las subredes entre sedes. La sumarización automática está deshabilitada para conservar los prefijos VLSM específicos. Al aprender rutas dinámicamente, los routers pueden recalcular el camino si un circuito deja de estar disponible y existe otra ruta. RIP usa el número de saltos como métrica, por lo que expresa distancia lógica y no capacidad del enlace.

**HSRPv2** atiende un problema distinto. Cada VLAN usa una IP virtual estable como puerta de enlace, compartida por dos routers con IP propias. El router preferido tiene prioridad 200 en los cinco grupos. Si el par cambia de rol, los hosts conservan su puerta de enlace configurada. La redundancia del gateway cubre la pérdida de un router o su uplink LAN, mientras el switch principal compartido sigue siendo un elemento común a ambos.

## PVST y EtherChannel

Los switches usan **PVST** para calcular árboles por VLAN. La VLAN 50 cruza el trayecto de Desarrollo y Soporte hacia el switch principal, así que el grupo estableció un orden de elección explícito en cinco sedes. Una prioridad menor es preferible en la elección del puente raíz.

| Sedes | Desarrollo | Soporte | Switch principal |
| --- | ---: | ---: | ---: |
| Dublín, Singapur, São Paulo, Prineville y Luleå | 0 | 4096 | 8192 |
| Menlo Park | Prioridad predeterminada | Prioridad predeterminada | Prioridad predeterminada |

Con ese orden, el switch de Desarrollo es el candidato preferente para la VLAN 50, seguido por Soporte y por el principal dentro de los tres equipos configurados. La elección se controla para esa VLAN. El diseño mantiene PVST en las demás VLAN.

En cada sede se configuró **LACP activo en ambos extremos** del `Port-channel 1`. Agrupa tres enlaces FastEthernet entre el switch principal (`Fa0/5–Fa0/7`) y el de Seguridad (`Fa0/4–Fa0/6`). Se eligió ese tramo para mantener un único enlace lógico entre ambos switches con mayor capacidad agregada y continuidad si queda un miembro operativo. STP trata el canal como una unidad. El mecanismo de distribución reparte tráfico entre miembros según el flujo, por lo que una sola conversación no obtiene necesariamente el ancho de banda agregado.

## DHCP, gestión y control de acceso

Los routers definen **un pool DHCP por VLAN** y anuncian la IP virtual HSRP como gateway. Así, una estación puede obtener IP, máscara y puerta de enlace de la subred a la que se conecta. Ambos routers de cada sede conservan pools para las mismas redes. HSRP comparte el gateway y los pools superpuestos requieren coordinación de asignaciones para operar como servidores DHCP redundantes.

Los routers configuran **SSH** para administración remota con usuario local, mientras los switches principales conservan **Telnet** para las prácticas del simulador. SSH protege la sesión de gestión. Telnet se limita al contexto de laboratorio. El enunciado también contempla **Port-Security** como control de las MAC en puertos de acceso. La simulación entregada segmenta los puertos por VLAN y carece de una política de límite de MAC configurada.

## Lectura de la topología

[Vista lógica general](../imagenes/logica-exportacion-archivo.png) · [Circuitos WAN](../imagenes/wan-interfaces.svg) · [Mapa físico](../imagenes/fisica-mapa.png)

La vista lógica explica cómo circula el tráfico entre VLAN y sedes. La física muestra la organización de ciudades, edificios, oficinas y armarios. En Dublín se puede seguir el recorrido desde la [ciudad](../imagenes/fisica-dublin-ciudad.png), pasando por [Administración](../imagenes/fisica-dublin-edificio.png), hasta el [rack](../imagenes/fisica-dublin-rack.png) que aloja el switch y los dos routers.

Fuentes de diseño del laboratorio · Enunciado «Proyecto de Interconexión de Redes – Empresa Facebook», plan VLSM del Grupo 3 y [simulación de Cisco Packet Tracer](../ProyectoVFinal.pkt).
