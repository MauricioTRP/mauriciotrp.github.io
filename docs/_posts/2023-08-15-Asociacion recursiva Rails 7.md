---
layout: single
toc: true
title: "Asociación recursiva en Rails 7"
date: 2023-07-11
categories:
  - Rails 7
  - Active Model
---

Para crear una asociacion de tipo recursiva en Rails se considerará el ejemplo clasico trabajador - supervisor en una DB. Para esto construiremos un modelo usuario básico, y le asignaremos campo de rol y supervisor para crear la relación y consultar en base a roles.

## Creación del usuario
Simplemente se creará usando alguno de los generadores (modelo, migración o scaffold)

{% highlight bash %}
rails generate scaffold User name role:integer
{% endhighlight %}

## Asignación de rol en modelo
Para poder usar los helpers que trae el método `enum` debemos modificar el modelo `User`

{% highlight ruby %}
class User < ApplicationRecord
  enum role: { worker: 0, supervisor: 1 }
end
{% endhighlight %}

de esta forma podemos usar nuevos métodos `worker?`, `supervisor?` para consultar si un registro tiene alguno de estos roles o `worker!`, `supervisor!` para definir uno de estos roles en un registro.

## Creación de campo para asociación recursiva
Ahora crearemos un nuevo campo `supervisor` para referenciar al modelo usuario, y definir la relación de supervisión entre trabajadores de una empresa.

Para esto usamos la migración

{% highlight bash %}
rails generate migration addSupervisorToUser supervisor:references
{% endhighlight %}

Ahora en el archivo de migración creado deberemos definir cual es la tabla a la cual está apuntando

Por último, debemos indicar en el modelo las relaciones `belongs_to` y `has_many` para poder consultar las relaciones de supervisores y trabajadores existentes.

{% highlight ruby %}
class User < ApplicationRecord
  belongs_to :supervisor, optional: true, class_name: 'User'
  has_many :workers, class_name: 'User', foreign_key: 'supervisor_id'
end
{% endhighlight %}

En este caso se debe notar que la relación `has_many` apunta su llave foranea a `supervisor_id`, que es el campo donde un usuario define cual es el supervisor.

## Consultas desde la consola
Ahora que tienes definidas las relaciones podrás consultarlos en la consola, y cualquier parte de tu aplicación: 

### Obtener todos los supervisores
Si quieres obtener todos los supervisores basta que uses

{% highlight bash %}
User.supervisor
{% endhighlight %}

que te devolverá un "`array`" (`ActiveRecord_Relation`) con todos los supervidores.

### Obtener un listado de los trabajadores supervisados por un determinado supervisor

Asumiendo que elegiste un supervisor de la lista anterior, digamos 
`jefazo`, puedes consultar todos los trabajadores que este supervisa simplemente usando el mensaje `workers` sobre `jefazo` (definido en el modelo en pasos anteriores):

{% highlight bash %}
jefazo.workers
{% endhighlight %}

devolverá otro "`array`" (`ActiveRecord_Associations_CollectionProxy`) con los trabajadores que son supervizados por `jefazo`

### Obtener el supervisor de un trabajador

De la misma forma, si tienes un trabajador (digamos `espinita`), podrás saber quien es el supervisor usando el mensaje `supervisor` sobre `espinita`:

{% highlight bash %}
espinita.supervisor
{% endhighlight %}

Que devolverá una instancia de clase `User`, con el supervisor que supervise a `espinita`

### Otras lecturas

Si quieres profundizar más sobre este concepto a nivel de diseño de base de datos recomiendo leer el artículo [design patttern: recursive associations][recursive-associations]

 [recursive-associations]: https://web.csulb.edu/colleges/coe/cecs/dbdesign/dbdesign.php?page=recursive.php
