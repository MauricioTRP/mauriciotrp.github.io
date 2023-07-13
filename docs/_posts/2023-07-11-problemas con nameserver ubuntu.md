---
layout: single
title: "Problemas con nameserver en ubuntu"
date: 2023-07-11
categories: ubuntu SO
gallery:
  - url: /assets/images/resolv-conf.gif
---

Tienes problemas para acceder a un recurso web, podría ser que la configuración de servidores de nombres de dominio esté dando problemas, estás tratando de acceder a un recurso de `dominio.org` y en un dispositivo tienes problemas, pero en otro no dentro de la misma red, podemos agregar servidores de dominio en el archivo `/etc/resolv.conf` en OS basados en debian

{% highlight bash %}
# abrimos el archivo con permiso de superusuario

sudo nano /etc/resolv.conf
{% endhighlight %}

![resolv.conf]({{ site.url }}{{ site.baseurl }}/assets/images/resolv-conf.gif)

Al final de este archivo agregarás las IP de los servidores de nombre de dominio asociadas a Google y Cloudflare

Para hacer esto al final del archivo agregas:

{% highlight conf %}
nameserver 8.8.8.8
nameserver 8.8.4.4
nameserver 1.1.1.1
{% endhighlight %}

Así, en caso de no poder acceder a un sitio web porque no encuentra la IP asociada al nombre de dominio, probablemente ahora si la encuentres a través de estos servidores.
