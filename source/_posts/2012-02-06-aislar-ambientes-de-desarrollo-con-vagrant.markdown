---
layout: post
title: "Aislar ambientes de desarrollo con Vagrant"
date: 2012-02-06 18:00
comments: true
categories: software, desarrollo
---

Si desarrollas en un solo tipo de 'stack' de desarrollo lo m&aacute;s 
com&uacute;n es instalar este stack en tu m&aacute;quina principal
y desarrollar directamente en el sistema operativo de tu preferencia.

En mi caso uso Mac OS y para desarrollar sitios web simples con el 'stack' tipo
XAMP simplemente activaba apache y php que vienen preinstalados e instalaba 
mysql copiando los binarios descargados desde la p&aacute;gina web.

Nada mal para la mayor&iacute;a de sitios que hab&aacute; desarrollado hasta ahora, 
pero los problemas comienzan a darse cuando ya no solo desarrollas en un 
solo lenguaje en el lado del servidor.

<!-- more -->

En los &uacute;ltimos a&ntilde;os he comenzado a desarrollar en ruby on rails 
as&iacute; que esto complica un poco el panorama de mantener todo dentro del 
mismo sistema operativo pero hay herramientas que fueron creadas para 
ayudarte en esto, como [rvm](http://beginrescueend.com/rvm/) o
[rbenv](https://github.com/sstephenson/rbenv).

El problema real comienza cuando necesitas duplicar un bug que solo sucede
en producci&oacute;n con versiones espec&iacute;ficas del software base instalado, para 
lo cual necesitas mantener instaladas diferentes versiones de php, ruby, mysql,
tener tambi&eacute;n redis, realizar pruebas de nuevos lenguajes como nodejs
tener diferentes servidores web como nginx o lighttpd.

Tambi&eacute;n es un problema tener varios clientes, cada uno con un stack
diferente de despliegue, cada uno con versiones diferentes de compiladores,
interpretes, etc.

Buscando la mejor manera de manejar este problema encontr&eacute; un software 
que me ha ayudado mucho, su nombre es [Vagrant](http://vagrantup.com/).

La soluci&oacute;n que propone este software es mantener un proyecto completamente
independiente de otros proyectos a trav&eacute;s de m&aacute;quinas virtuales, para lo
cual depende de [VirtualBox](https://www.virtualbox.org/) y facilita la 
construcci&oacute;n de est&aacute;s m&aacute;quinas automatizando las instalaciones con [puppet](http://puppetlabs.com/)
o [chef](http://www.opscode.com/chef/).

Estuve probando este sistema en los &uacute;ltimos proyectos y me ha encantado, 
aparte de vagrant que ayuda en la gesti&oacute;n de las m&aacute;quinas virtuales,
el que m&aacute;s se luce es chef, la idea es que puedes tener toda una 'receta'
para instalar todo el software necesario para que se ejecute tu aplicaci&oacute;n
sobre el sistema operativo que ejecutas en producci&oacute;n, si lo usas
desde el primer momento del proyecto crear tu sistema de producci&oacute;n
es tan f&aacute;cil como ejecutar un comando.

En un post siguiente les contar&eacute; un poco m&aacute;s como se configura vagrant
y como se crea una receta b&aacute;sica con chef.
