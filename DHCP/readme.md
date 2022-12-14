Primero crearemos el archivos dokcer-compose.yml con la imagen del servidor DHCP y mapeamos la carpeta "data" a una carpeta local que queramos, en mi caso tambien la llamaré "data". Además haremos que esté conectada a la interfaz fisica del host con el parametro "network_mode: host"

~~~
version: '3.9'
services:
    dhcpd:
      image: networkboot/dhcpd
      container_name: asir-dhcp
      network_mode: host
      volumes:
        - ./data:/data
~~~

Ahora dentro de la carpeta "data" crearemos un archivo llamado dhcp.comf para configurar las IPs y las redes. El archivo contendrá la siguiente configuracion:

~~~
# dhcpd.conf
#
# Configuración de la red de la Unidad de Imagen.
#
#
# El rango de direcciones 192.168.1.1-10 está reservada
# para los servidores y ordenadores con IP fija.
#

subnet 10.0.9.0 netmask 255.255.255.0 {

  range 10.0.9.200 10.0.9.202;

  option domain-name-servers 8.8.8.8, 193.146.96.2, 193.146.96.3;
  option domain-name "asir.tv";
  option routers 10.0.9.254;
  option subnet-mask 255.255.255.0;
  option broadcast-address 10.0.9.255;

  default-lease-time 86400;
  max-lease-time 172800;

  group {

     default-lease-time 604800;
     max-lease-time 691200;

     host apache {
         hardware ethernet 00:10:5a:f1:35:87;
         fixed-address 10.0.9.37;
     }

  }
}
~~~

