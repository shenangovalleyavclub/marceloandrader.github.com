---
layout: post
title: "Dependencias de Aplicaciones CLI"
date: 2011-12-15 17:16
comments: true
categories: [software, cli]
---

Estas son mis aplicaciones preferidas, para obtener los comandos que m&aacute;s uso en la l&iacute;nea de comandos

{% codeblock comandos m&aacute;s usados lang:bash %}
history | awk '{CMD[$2]++;count++;}END { for (a in CMD)print CMD[a] " " CMD[a]/count*100 "% " a;}' | grep -v "./" | column -c3 -s " " -t | sort -nr | nl |  head -n20
{% endcodeblock %}

<!-- more -->

Resultado:
<pre>
     1	3194  33.7347%    git     # para trabajar con c&oacute;digo fuente
     2	1535  16.2125%    cd
     3	854   9.01986%    ls
     4	486   5.13308%    ssh	  # conexi&oacute;n a servidores
     5	195   2.05957%    cat
     6	193   2.03845%    sudo
     7	172   1.81665%    mate
     8	155   1.63709%    curl    # probar peticiones HTTP
     9	152   1.60541%    tail    # ver logs
    10	151   1.59485%    vim
    11	150   1.58428%    rake
    12	120   1.26743%    cap     # desplegar aplicaciones (capistrano)
    13	97    1.0245%     ping
    14	85    0.897761%   phpunit
    15	80    0.844951%   find
    16	79    0.83439%    rm
    17	79    0.83439%    mysql
    18	73    0.771018%   php
    19	68    0.718209%   irb
    20	61    0.644275%   mvim
</pre>


Pueden comentar el uso de su l&iacute;nea de comandos.
