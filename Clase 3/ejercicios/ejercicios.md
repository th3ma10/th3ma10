#### Ejercicio 1: Verificar el Espacio Disponible en el Sistema

#### Ejercicio 2: Mostrar el Número de Inodos Utilizados y Disponibles

#### Ejercicio 3: Ver el Espacio Ocupado por el directorio /var

#### Ejercicio 4: Mostrar el Espacio Ocupado por Todos los Subdirectorios de /etc

#### Ejercicio 5: Identificar los 10 Archivos más Grandes en /var/log

#### Ejercicio 6: Mostrar el Espacio Ocupado por todos los archivos '.log' en /var/log

#### Ejercicio 7: Crear un Reporte del Espacio Ocupado por /var/log, sus subdirectorios, y los directorios de sus subdirectorios.



#Ejercicio 1: sudo df -h

#Ejercicio 2: sudo df -i

#Ejercicio 3: sudo du -sch /var

#Ejercicio 4: sudo du -h --max-depth-=1 /etc

#Ejercicio 5: sudo du -sm /var/log/* | sort -nr | head

#Ejercicio 6: sudo du -ch --max-depth=1 --max-depth=2 /var/log/*.log

