---
layout: single
title:  "Regexp en PSQL"
date:   2024-04-11 02:18:01 -0400
categories: jekyll rails
---

La entrada es breve y se actualizará en la marcha. 

## Expresiones regulares como restricción

Considerando que no queremos ingresar "basura" a nuestra DB, es que usaremos regexp para filtrar el posible ingreso de data basura. En este caso se considera el rut chileno con la expresión regular `/^(\d{1,3}(?:\.\d{1,3}){2}-[\dkK])$/gm` para su validación

```mermaid
classDiagram
    PERSONAS <|-- ESTUDIANTES : Inheritance

    class PERSONAS 
    PERSONAS :  SERIAL id
    PERSONAS :  String nombre
    PERSONAS :  String apellido
    PERSONAS :  String direccion
    PERSONAS :  String rut
    PERSONAS :  Date   fecha_nacimiento
    

    class ESTUDIANTES 
    ESTUDIANTES :  String carrera
    ESTUDIANTES :  String grupo
    ESTUDIANTES :  INT    grado
    

```

Con la instrucción `CHECK(rut ~ '^(\d{1,3}(?:\.\d{1,3}){2}-[\dkK])$')` podremos dar la validación en base a un regexp

```sql
CREATE TABLE personas (
id SERIAL PRIMARY KEY,
nombre VARCHAR(30) NOT NULL,
apellido VARCHAR(30),
direccion VARCHAR(50),
rut VARCHAR(20) NOT NULL UNIQUE CHECK(rut ~ '^(\d{1,3}(?:\.\d{1,3}){2}-[\dkK])$'),
fecha_nacimiento DATE
);
```

```sql
CREATE TABLE estudiantes (
carrera VARCHAR(50) NOT NULL,
GRUPO VARCHAR,
GRADO INT,
) INHERITS (personas);
```
