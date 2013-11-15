---
layout: post
title: "Aplicaciones Web del lado del Cliente con MVVM"
date: 2013-02-19 11:13
comments: true
categories: desarrollo, javascript, framework
---

Aplicaciones web con alta carga de l&oacute;gica del lado cliente
han comenzado a popularizarse, la idea es que mucho de lo que antes
se hac&iacute;a en el servidor, ahora se lo puede hacer en el cliente (browser)
y dejar al servidor solo como un API de acceso a la persistencia de 
los datos.

<!-- more -->

Ejemplos de este tipo de aplicaciones web son: Gmail, Twitter, Facebook, etc.

En estos escenarios librer&iacute;as como jQuery se queda corto
para organizar la estructura de la aplicaci&oacute;n, jQuery es
un gran aliado en la lucha contra la estandarizaci&oacute;n de
llamadas de bajo nivel de cada browser como por ejemplo el acceso
al DOM, y realizar cambios sencillos en la p&aacute;gina,
pero para organizar l&oacute;gica no, e inmediatamente comienza
a sentirse un c&oacute;digo dif&iacute;cil de mantener en el futuro.

Durante los pasados 2 a&ntilde;os he vivido esta transici&oacute;n de
tratar de usar jQuery para todo, a usar una nueva librer&iacute;a que
me ha permitido hacer mucho m&aacute;s y tener un c&oacute;digo m&aacute;s
limpio, la librer&iacute;a de la que les hablo es [knockout](http://knockoutjs.com/).

Esta librer&iacute;a de javascript tiene estas principales caracter&iacute;sticas:

- _Declarative Bindings_: Esto permite enlazar los elementos del DOM
usando un atributo especial de html5 (`data-bind`), se puede enlazar
desde eventos en el elemento, hasta cambios de estilos manejados por
evaluaci&oacute;n de c&oacute;digo javascript.

- _Automatic UI Refresh_: En esta librer&iacute;a los models son simples
objetos javascript a los cuales se puede atar v&iacute;a `data-bind` los
valores de estos para autom&aacute;ticamente desplegar en la vista sin tener
que codificar estos cambios, esto en mi experiencia es lo mejor de knockout
porque ahorra mucho c&oacute;digo repetitivo.

- _Dependendy Tracking_: Cuando se definen los modelos en objetos javascript
se utiliza ciertas funciones que permiten enlazar propiedades compuestas
por otras propiedades o atributos calculados.

- _Templating_: En combinaci&oacute;n con el refresco del UI autom&aacute;tico
permite definir como desplegar datos usando simple html m&aacute;s etiquetas
de template como `foreach` of `if` para crear el html final.

Los tutoriales en la p&aacute;gina son muy explicativos [learn.knockoutjs.com](http://learn.knockoutjs.com/)
y la [documentaci&oacute;n](http://knockoutjs.com/documentation/introduction.html) es bastante completa.

Este cambio de librer&iacute;a requiere un poco de cambio en la forma de pensar
para crear una soluci&oacute;n m&aacute;s robusta, lo bueno de este cambio es
que adicional a tener un mejor c&oacute;digo he comenzado a escribir tests
para mis clases javascript, lo que antes era imposible con jQuery simple.

Les dejo con un ejemplo de como usar `ko.observable` y `ko.observableArray` son
las funciones basicas de KO que permiten hacer uso de todo su potencial.

`ko.observable` permite rastrear los cambios a una variable simple, una vez creada
una variable con esta funci&oacute;n, se puede usar para realizar el refresco autom&aacute;tico
de la interfaz, crear atributos calculados, etc.

`ko.observableArray` permite rastrear los cambios a una colecci&oacute;n de objetos
cuando se agregan/remueven.

{% jsfiddle A9y7W %}
