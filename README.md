
# Práctica 4

**Universidad de San Carlos de Guatemala**

**Facultad de Ingeniería**

**Escuela de Ciencias y Sistemas**

**Redes de Computadoras 1**

**Ing. Pedro Pablo Hernandez Ramirez**

**Aux. Andrés Alejandro Montufar Cordero**

**Angel Manuel Miranda Asturias 201807394**

# Manual de Configuración

## Configuración de la topología de red en GNS3

La topología de red que se realizó la práctica es la siguiente (Agregadas las IPs correspondientes):
![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/Topologia.png "Topología de Red")

Las direcciones de cada VLAN  estan detalladas en la tabla siguiente:

| VLAN | Dirección de Red | Primera Dirección Asignable | Última Dirección Asignable | Dirección de Broadcast |
|:----:|:----------------:|:---------------------------:|:--------------------------:|:----------------------:|
|  10  |  192.168.14.0/24 |         192.168.14.1        |       192.168.14.254       |     192.168.14.254     |
|  20  |  192.168.24.0/24 |         192.168.24.1        |       192.168.24.254       |     192.168.24.254     |

## Configurar modo Trunk

Para poder "pasar" las VLANs y algunas configuraciones dentro de los EthernetSwitches, se necesita de realizar ciertas configuraciones dentro de las interfaces de los mismos. Para estos se realiza la siguiente secuencia de comandos, luego de ingresar a la consola de estos.

```
    int range fa#/# - #     # Se ingresa a las interfaces que deseamos modificar como trunk
    speed 100
    duplex full
    switchport mode trunk
```

Para ver que intefaces deseamos modificar utilizamos el comando

```
sh int status
```

Y todas las interfaces que tengamos conectadas que deseemos configurar como trunk estarán ahí. Por ejemplo:

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/EthernetSwitches/Po/Po3/MostrarStatusES3Inicial.png "SH INT STATUS")

Para configurarlas realizamos lo que se muestra en las siguientes capturas de pantalla:

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/EthernetSwitches/Po/Po1/InicialDuplexFullSpeed100Trunk.png "Trunk ESW1")

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/EthernetSwitches/Po/Po2/InicialDuplexFullSpeed100TrunkES2.png "Trunk ESW2")

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/EthernetSwitches/Po/Po3/InicialDuplexFullSpeed100TrunkES3.png "Trunk ESW3")

## Configurar VTP

Para la configuración de VTP utilizamos como nombre de dominio y contraseña "redes1_201807394". El servidor estará en el ESW1 y los clientes serán los ESW2 y 3. Se utilizaron los comandos siguientes:

```
    conf t
    vtp domain redes1_201807394
    vtp password redes1_201807394
    vtp mode server/client/transparent  # Dependiendo del modo en que lo querramos
```

Las configuraciones por EthernetSwitch son las siguientes:

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/EthernetSwitches/VTP/VTPESW1.png "VTP ESW1")

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/EthernetSwitches/VTP/VTPESW2.png "VTP ESW2")

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/EthernetSwitches/VTP/VTPESW3.png "VTP ESW3")

## Configuración de VLANs

La configuración de VLANs se realizará únicamente en el EthernetSwitch que configuramos como servidor. Los comandos correspondientes para cada VLAN son los siguientes:

```
    conf t
    vlan #
    name NOMBRE
```

Donde # es un número distinto a 1 o 1002-1005 y NOMBRE es el nombre que le pondremos a nuestra VLAN. En nuestro caso, se realizó lo siguiente:

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/EthernetSwitches/VLANS/CrearVLAN.png "Crear VLANs")

Luego confirmaremos que estas VLANs se "transmitieron" a los demás Switches con el siguiente comando:

```
    sh vlan-switch
```

### ESW1

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/EthernetSwitches/VLANS/ConfirmarESW1.png "VLAN ESW1")

### ESW2

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/EthernetSwitches/VLANS/ConfirmarESW2.png "VLAN ESW2")

### ESW3

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/EthernetSwitches/VLANS/ConfirmarESW3.png "VLAN ESW3")

## Configuración y Creación de Port-Channel

Los port-channel sirven para "salvar" la red si alguno de las conexiones "extras" que tenemos muere por alguna razón. También sirven para evitar ciclos infinitos dentro de la red. Para crearlos utilizaremos los comandos siguientes.

```
    conf t
    int range f#/# - #
    channel-group # mode on
```

Los # son números. En el caso de "int range..." son para configurar o "juntar" esas interfaces en un mismo Port-Channel. Y en el último es un número que nosotros le asignamos.

Se realizó de la siguiente manera por cada Port-Channel y ESW:

### Port-Channel ESW1

#### Po1

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/EthernetSwitches/Po/Po1/Po1ESW1.png "Po1 ESW1")

#### Po2

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/EthernetSwitches/Po/Po1/Po2ESW1.png "Po2 ESW1")

### Port-Channel ESW2

#### Po1

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/EthernetSwitches/Po/Po2/Po1ESW2.png "Po1 ESW2")

#### Po2

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/EthernetSwitches/Po/Po2/Po3ESW2.png "Po3 ESW2")

### Port-Channel ESW3

#### Po1

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/EthernetSwitches/Po/Po3/Po2ESW3.png "Po2 ESW3")

#### Po2

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/EthernetSwitches/Po/Po3/Po3ESW3.png "Po3 ESW3")

## Verificar root y puertos bloqueados

Para ver cual es la ruta principal dentro de nuestra topología, es necesario saber que Switch es el root. Para esto utilizamos el comando siguiente:

```
    sh spanning-tree brief
```

Con este comando nos mostrará a detalle qué Port-channels están bloqueados y que Switch es el root. En nuestro caso el Port-channel 3 estaba bloqueado en el ESW3 y el Switch root es el ESW1. A continuación las capturas de pantalla: 

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/EthernetSwitches/Root/BriefESW1_1.png "Sh1 ESW1")

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/EthernetSwitches/Root/BriefESW1_2.png "Sh2 ESW1")

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/EthernetSwitches/Root/BriefESW2_1.png "Sh1 ESW2")

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/EthernetSwitches/Root/BriefESW2_2.png "Sh2 ESW2")

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/EthernetSwitches/Root/BriefESW3_1.png "Sh1 ESW3")

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/EthernetSwitches/Root/BriefESW3_2.png "Sh2 ESW3")

Notese que en el ESW1 hay una línea que nos dice: ""

## Configuración de subinterfacez del Router

Nuestra topología cuenta con un router. En este se hará uso de subinterfaces. Que estas son para tener las VLANs dentro de una sola interfaz. Para la  configuración de cada subinterfaz se utilizaron los siguientes comandos:

```
    conf t
    int f#/#.##
    encapsulation dot1q NUMEROVLAN
    ip address IP MASCARADERED
```

Los resultados son los siguientes:

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/Router/ConfSubInterfaces.png "Subinterfaz 1")

Cabe mencionar que en el comando de IP de este screenshot me confundí. La IP que le asigne al final es la siguiente: __192.168.14.254__ y la __192.168.24.254__.

Guardamos los cambios y ya tenemos casi lista nustra red. Falta un único paso.

## Configuración Switches Normales

Los Switch normales también debemos de colocar sus interfaces que se comunican con los ESW en modo trunk. Además que se necesita asignar las VLANs correspondientes a cada puerto, para que cada computadora pertenezca a una. Las configuraciones son las siguientes:

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/Switch1.png "Switch 1")

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/Switch2.png "Switch 2")

# Captura de paquetes

Para la captura de paquetes se siguió el mismo procedimiento de la última práctica realizada en GNS3. El resultado es el siguiente:

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/Paquetes/Paquetes.png "Paquetes")

# Ruta Principal

La ruta principal de los paquetes se dibujo en base a lo calculado en el antepenúltimo punto del manual de configuración. La ruta es la siguiente tomando el ejemplo de envío de paquetes de TinyLinux a la PC2:

![alt-text](https://github.com/ManuelMiranda99/-REDES1-Practica4_201807394/blob/main/Imgs/Camino.png "Camino")