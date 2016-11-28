# tunningSqlPlus

Cómo conseguir autocompletado en sqlplus

## Instalar rlwrap

~~~
# apt-get install rlwrap
~~~

## Crear el diccionario

Para crear el diccionario que usará sqlplus ejecutar este pequeño script:

~~~
#!/bin/bash
 
sqlplus -s system/hackme@hostname/bbdd << ! > sqlplus.dict
set head off pages 0 linesize 150 echo off feedback off verify off heading off
select object_name from all_objects where object_type in (
  'TABLE', 'VIEW', 'PACKAGE', 'PROCEDURE', 'FUNCTION'
);
!
~~~

## Ejecutar sqlplus, por ejemplo, así:

~~~
$ # -i: ignore case
$ # -f: nombre del diccionario
$ # -pgreen: prompt verde
$ rlwrap -if sqlplus.dict sqlplus scott/tiger -pgreen
~~~

## TIPS & TRICKS

### Añade a tus variables del entorno un alias como este:

~~~
oracle_dict='/path/to/your/dictionary/sqlplus.dict'
alias sql="rlwrap -if ${oracle_dict} -pgreen sqlplus"
~~~

### Parámetros interensantes para el prompt:

Crea un fichero login.sql como este en el directorio desde donde lanzas sqlplus:

~~~
set pagesize 50
set linesize 200
define_editor=/usr/bin/nvim
SET SQLPROMPT "_USER'@'_CONNECT_IDENTIFIER _DATE> "
~~~

![alt_tag](sqlplus.gif?raw_true "Demo")
