# repositorioexamennoviembre
SRI - EXAMEN

# Crea un repositorio para contestar todo o exame.
Este repositorio ten que conter tódo-los ficheiros necesarios para xustifica-la túas respostas.

Contesta as seguintes preguntas, xustificándoas, nun README.md

#1. Explica métodos para 'abrir' unha consola/shell a un contenedor en execución.

-Para abrir la consola a un contenedor usaremos el comando:
```
'docker exec':
docker exec -it [nombre_contenedor]  
```
Al hacer esto se nos abrira una sesión interactiva con el contenedor


#2. No contenedor anterior (en execución), qué opciones tes que ter usado ó arrincalo para poder interactuar coas súas entradas e salidas

```
docker run -it [franco1]
```
#3. Cómo sería un ficheiro docker-compose para que dous contenedores se comuniquen entre si nunha rede só deles?


```
version '3.8'
	services
		contenedorU:
		image: alpine
		command: tall -f /dev/null
		networks:
			red privada:
			aliases:
				-alias_contenedorU
		contenedorI:
		image: alpine
		command: tall -f /dev/null
		networks:
			red privada:
			aliases:
				-alias_contenedorI
		networks:
			red_privada:
			driver: bridge
			
```

#4. Qué tes que engadir ó ficheiro anterior para que un contenedor teña unha IP fixa?

Si queremos asignarle la ip fija tendremos primero que modificar el .yml:

```
networks:
	red_privada:
	driver: bridge
	ipam
	config:
		- subnet: 172.28.0.0/16
```

Ahora asignamos la ip fija:
```
contenedorU
	networks:
		red_privada:
		ipv4_adress: 172.28.5.2
```

#5. Qué comando de consola podo usar para sabe-las ips dos contenedores anteriores?

Usaremos: 
```
docker inspect -f "
```

#6. Cál é a funcionalidade do apartado "ports" en docker compose?
```
Es usado mas que nada para el mapeo de puertos del contenedor a puertos del host.
```



#7. Para qué serve o rexistro CNAME? Pon un exemplo
```
El uso que se da es para la redirección de un dominio hacia otro distinto.
```
#8. Cómo podo facer para que a configuración dun contenedor DNS non se borre se creo outro contenedor?
```
Para no tener que perder la configuración del Dns es mejor tener como solución  el usar los volumenes persistentes
```

#9. Engade unha zoa tendaelectronica.int no teu docker DNS que teña:
- www á IP 172.16.0.1
- owncloud sexa un CNAME de www
- un rexistro de texto có contido "1234ASDF"
Comproba que todo funciona có comando "dig"
Mostra nos logs que o servicio funciona ben usando a saída da terminal ó levantar o compose ou có comando "docker logs [nomeContenedorOuID]"
(o apartado 9 realízase na máquina virtual)

--- Aca  tenemos el archivo yml configurado con los parametros dichos: ---

```
$TTL 3600
@    IN SOA ns.tendaelectronica.int. admin.tendaelectronica.int (
     2024111501 ; Serial
     3600       ; Refresh
     1000       ; Retry
     1209600    ; Expire
     3600 )     ; Minimun TTL
@    IN NS ns.tendaelectronica.int.
www  In A  172.16.0.1
owncloud IN CNAME www
@    IN TXT "1234ASDF"
     
		
```

