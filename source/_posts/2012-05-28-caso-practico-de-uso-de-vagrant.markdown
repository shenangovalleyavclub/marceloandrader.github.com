---
layout: post
title: "Caso pr&aacute;ctico de uso de vagrant"
date: 2012-05-28 17:01
comments: true
categories: software, desarrollo
---
Para este ejemplo levantaremos un sistema linux (ubuntu) 
con apache como ejemplo del uso de vagrant.

##Pre-Requisitos
Instalar VirtualBox [Descargar VirtualBox](http://www.virtualbox.org/wiki/Downloads)
vagrant soporta las versiones 4.0.x y 4.1.x

##Instalando Vagrant
Descargar el paquete adecuado para su sistema operativo [Descargar Vagrant](http://downloads.vagrantup.com/tags/v1.0.3)
e instalarlo.

<!-- more -->

##Probando la instalaci&oacute;n
Abrir una terminal y ejecutar :

{% codeblock check vagrant lang:bash %}
$ vagrant
Usage: vagrant -v -h command <args>

    -v, --version                    Print the version and exit.
    -h, --help                       Print this help.
  ...
$ vagrant --version
Vagrant version 1.0.3
{% endcodeblock %}


##Descargando una m&aacute;quina base (Base Box)
Vagrant funciona a base de m&aacute;quinas base que son
distribuciones de cualquier sistema operativo ligeramente
modificadas para que vagrant pueda ejecutarse en ellas,
normalmente instalar ruby, chef.

{% codeblock add base box lang:bash %}
$ vagrant box add lucid32 http://files.vagrantup.com/lucid32.box
{% endcodeblock %}

Lo que hace este comando es agregar una m&aacute;quina base, es decir
el archivo vmdk (el disco duro de la m&aacute;quina de virtual box)
junto con otros archivos de configuraci&oacute;n a un directorio
general: ``$HOME/.vagrant.d/boxes``

La idea es que a partir de este se pueden crear muchos ambientes
usando el mismo archivo descargado. Como en el ejemplo se descarga
un ubuntu lucid de 32 bits, al crear otra m&aacute;quina virtual
con vagrant, esta no se modifica solo se copia y se inicia la
instalaci&oacute;n de paquetes, de esta forma si algo va mal se puede
destruir la m&aacute;quina virtual y volver a regenerarla sin 
problemas y sin descargar nuevamente la m&aacute;quina base.

##Creando la primera m&aacute;quina virtual
Los ambientes virtuales normalmente estan atados a un proyecto 
espec&iacute;fico en el que se este trabajando, por lo cual el siguiente
comando debe ejecutarse dentro del directorio del proyecto.

{% codeblock init vagrant lang:bash %}
$ vagrant init lucid32
{% endcodeblock %}

Este comando crear&aacute; un archivo ``Vagrantfile`` en el cual
esta la configuraci&oacute;n de la m&aacute;quina por ejemplo:

* Cu&aacute;l es el box base, y su url
* Qu&eacute; tipo de interfaz de red usa y cual IP usar
* Puertos redirigidos (forward ports) para acceder a puertos dentro del ambiente virtual
* Directorios compartidos (por defecto siempre se comparte /vagrant dentro del ambiente a la m&aacute;quina host en el directorio
donde se encuentra el Vagrantfile
* El tipo de provisionador (puppet, chef o propio) esto es el 
sistema que se usar&aacute; para instalar los paquetes

##Iniciando el ambiente virtual
Este comando toma la configuraci&oacute;n del proyecto en Vagrantfile
e inicia una m&aacute;quina virtual en virtual box, y la provisiona
dependiendo del sistema usado, en este momento no lo hemos definido todav&iacute;a
y solo iniciar&aacute; la m&aacute;quina sin ning&uacute;n paquete
extra instalado.

{% codeblock vagrant up lang:bash %}
$ vagrant up
[default] Importing base box 'lucid32'...
Guest Additions Version: 4.1.0
VirtualBox Version: 4.1.4
[default] Matching MAC address for NAT networking...
[default] Clearing any previously set forwarded ports...
[default] Forwarding ports...
[default] -- 22 => 2222 (adapter 1)
[default] Creating shared folders metadata...
[default] Clearing any previously set network interfaces...
[default] Booting VM...
[default] Waiting for VM to boot. This can take a few minutes.
[default] VM booted and ready for use!
[default] Mounting shared folders...
[default] -- v-root: /vagrant
{% endcodeblock %}

Ok la m&aacute;quina est&aacute; levantada y ejecut&aacute;ndose
para ingresar podemos hacer ssh a esta:

{% codeblock vagrant ssh lang:bash %}
$ vagrant ssh
Linux lucid32 2.6.32-33-generic #70-Ubuntu SMP Thu Jul 7 21:09:46 UTC 2011 i686 GNU/Linux
Ubuntu 10.04.3 LTS

Welcome to Ubuntu!
 * Documentation:  https://help.ubuntu.com/
Last login: Thu Jul 21 13:07:53 2011 from 10.0.2.2
vagrant@lucid32:~$ 
{% endcodeblock %}


##Provisionando la nueva m&aacute;quina
Una vez que la m&aacute;quina se esta ejecutando no tiene
nada instalado, por lo que ahora lo que se requiere hacer es 
definir la forma en la que deseamos instalar el software base necesario,
recordemos que la idea de usar vagrant es que podemos utilizar
la misma receta en diferentes ambientes de desarrollo obteniendo
siempre los mismos resultados.

Para este caso usaremos [chef-solo](http://wiki.opscode.com/display/chef/Chef+Solo)

Chef permite escribir las recetas de instalaci&oacute;n de paquetes
usando un DSL escrito en ruby.

Para activar chef, abrimos Vagrantfile y agregamos al final antes del 
&uacute;ltimo ``end`` lo siguiente:

{% codeblock chef enabled lang:ruby %}
  # Enable and configure the chef solo provisioner
  config.vm.provision :chef_solo do |chef|
    # En este caso descargamos nuestro libro de recetas desde el web
	  # hay otras opciones para mantener las recetas dentro del proyecto
    chef.recipe_url = "http://files.vagrantup.com/getting_started/cookbooks.tar.gz"

    # Ejecuta la receta vagrant_main
    # la cual hace toda la magia
    chef.add_recipe("vagrant_main")
  end
{% endcodeblock %}

Y para ejecutar esta receta se necesita lanzar el comando:

{% codeblock vagrant provision lang:bash %}
$ vagrant provision
[default] Running provisioner: Vagrant::Provisioners::ChefSolo...
[default] Generating chef JSON and uploading...
[default] Running chef-solo...
stdin: is not a tty
[Mon, 28 May 2012 16:09:24 -0700] INFO: *** Chef 0.10.2 ***
[Mon, 28 May 2012 16:09:25 -0700] INFO: Setting the run_list to ["recipe[vagrant_main]"] from JSON
[Mon, 28 May 2012 16:09:25 -0700] INFO: Run List is [recipe[vagrant_main]]
[Mon, 28 May 2012 16:09:25 -0700] INFO: Run List expands to [vagrant_main]
[Mon, 28 May 2012 16:09:25 -0700] INFO: Starting Chef Run for lucid32
[Mon, 28 May 2012 16:09:25 -0700] INFO: Processing execute[apt-get update] action run (apt::default line 20)
[Mon, 28 May 2012 16:09:25 -0700] INFO: execute[apt-get update] sh(apt-get update)
...
[Mon, 28 May 2012 16:16:15 -0700] INFO: Processing service[apache2] action restart (apache2::default line 189)
[Mon, 28 May 2012 16:16:17 -0700] INFO: service[apache2] restarted
[Mon, 28 May 2012 16:16:17 -0700] INFO: Chef Run complete in 411.460964 seconds
[Mon, 28 May 2012 16:16:17 -0700] INFO: Running report handlers
[Mon, 28 May 2012 16:16:17 -0700] INFO: Report handlers complete
{% endcodeblock %}

##Redireccionar un puerto interno

Para poder acceder desde afuera del ambiente debemos modificar los puertos
redireccionados.

Para lo cual editamos ``Vagrantfile``

{% codeblock chef enabled lang:ruby %}
  # Agregar esto cerca de la configuraci&oacute;n de puertos comentada
  config.vm.forward_port 80, 4567
{% endcodeblock %}

Para hacer que se tome en cuenta este nuevo puerto se debe recargar la m&aacute;quina

{% codeblock vagrant reload lang:bash %}
$ vagrant reload
[default] Attempting graceful shutdown of VM...
[default] Clearing any previously set forwarded ports...
[default] Forwarding ports...
[default] -- 22 => 2222 (adapter 1)
[default] -- 80 => 4567 (adapter 1)
[default] Creating shared folders metadata...
[default] Clearing any previously set network interfaces...
[default] Booting VM...
[default] Waiting for VM to boot. This can take a few minutes.
{% endcodeblock %}

##Crear un archivo index.html para ver que est&aacute; funcionando

Abrir tu editor favorito y colocar lo siguiente:

{% codeblock index html lang:html %}
<h1>Hello from a Vagrant VM</h1>
{% endcodeblock %}

Ir al navegador y abrir [localhost:4567](http://localhost:4567)

Debe poder ver lo siguiente:

{% img /images/posts/vagrant-working.png Pagina web funcionando en vagrant %}


## Eso es todo
Si necesitan ayuda en algun punto que no lo trate con mucha explicaci&oacute;n
no duden en escribir en los comentarios.
