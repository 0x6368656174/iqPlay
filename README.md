iqPlay 
======

iqPlay - приложение для просмотра и манипуляции с видеофайлами, созданными программой iqScreenSpy. Так же iqPlay позволяет осуществлять манипуляции с файлами PlayDoc.bin СДВИ.

Установка iqPlay
----------------

Для установки iqPlay можно воспользоваться либо скриптом /sintez/sintez/bin/iqSetup, либо установить вручную, для этого необходимо выполнить следующие действия:

* Установить OpenCSW и настроить его для работы с локальным репозиторием
~~~~~~{bash}
    su
    cd /
    pkgadd -d http://web/opencsw/pkgutil-i386.pkg
    /usr/sfw/bin/wget http://web/iq/pkgutil.conf_localmirror.patch
    gpatch -p1 < pkgutil.conf_localmirror.patch
    rm pkgutil.conf_localmirror.patch
    /opt/csw/bin/pkgutil -U
~~~~~~
* Установить пакеты, от которых зависит iqPlay
~~~~~~{bash}
    su
    /opt/csw/bin/pkgutil -iy dialog sudo mplayer bash coreutils
~~~~~~
* Скачать iqPlay и iqFlashMount
~~~~~~{bash}
    su sintez
    cd /sintez/sintez/bin/
    /usr/sfw/bin/wget http://web/iq/bin/iqPlay
    chown sintez:syn iqPlay #Если забыли выйти из под root и скачали файл из под него
    chmod 755 iqPlay
    /usr/sfw/bin/wget http://web/iq/bin/iqFlashMount
    chown sintez:syn iqFlashMount #Если забыли выйти из под root и скачали файл из под него
    chmod 755 iqFlashMount
~~~~~~
* Скачать кастомную сборку dialog
~~~~~~{bash}
    su sintez
    cd /sintez/sintez/bin/
    /usr/sfw/bin/wget http://web/iq/bin/dialog
    chown sintez:syn dialog #Если забыли выйти из под root и скачали файл из под него
    chmod 755 dialog
~~~~~~
* Скачать файлы с хостами
~~~~~~~{bash}
    su sintez
    cd /sintez/sintez/.config/itQuasar
    /usr/sfw/bin/wget http://web/iq/config/iqPlayHostsRc.txt
    /usr/sfw/bin/wget http://web/iq/config/iqPlayHostsPivp.txt
    chown sintez:syn iqPlay* #Если забыли выйти из под root и скачали файл из под него
~~~~~~~
* Скачать файлы sudoers
~~~~~~~{bash}
    su
    cd /etc/opt/csw/sudoers.d/
    /usr/sfw/bin/wget http://web/iq/sudoers/mplayer
    chmod 640 mplayer
    /usr/sfw/bin/wget http://web/iq/sudoers/volmgt
    chmod 640 volmgt
~~~~~~~


Настройка iqPlay
----------------

iqPlay для вывода списка хостов использует файл /sintez/sintez/.config/itQuasar/iqPlayHosts.txt. При установке, выкачиваются два файла iqPlayHostsRc.txt и iqPlayHostsPivp.txt, для того, чтобы iqPlay знал какой из этих файлов использовать, а так же не копировать нужный файл вручную при каждом обновлении хостов, рекомендуется создать символическую ссылку на нужный файл, например для хостов РЦ:
~~~~~~~{bash}
    su sintez
    cd /sintez/sintez/.config/itQuasar
    ln -s /sintez/sintez/.config/itQuasar/iqPlayHostsRc.txt iqPlayHosts.txt
~~~~~~~

Обновление iqPlay
-----------------

Для обновления файлов хостов, необходимо отредактировать их на веб-сервере, а потом обновить iqPlay (рекомендуется использовать скрипт /sintez/sintez/bin/iqSetup, но можно и вручную выполнить пункт "Скачать файлы с хостами"). Файлы хостов хранятся на веб-сервере в папке /sd/web/htdocs/iq/config/. Они называются:
* iqPlayHostsRc.txt - для хостов РЦ
* iqPlayHostsPivp.txt - для хостов ПИВП

Для того, чтобы обновить файлы хостов ПИВП правильным решением будет обновление файла /sd/web/htdocs/iq/config/iqPlayHostsPivp.txt на веб-сервере РЦ, а потом синхронизировать файлы веб-сервера ПИВП с файлами веб-сервера РЦ. В этом случае файлы с хостами на обоих веб-серверах будут одинаковыми. Для синхронизации файлов веб-сервера ПИВП с файлами веб-сервера РЦ необходимо подключиться к веб-серверу ПИВП и выполнить на нем следующую команду:
~~~~~~~{bash}
	su
	/opt/csw/bin/rsync -av rsync://192.168.60.203:/iq /sd/web/htdocs/iq #Будьте внимательны со слешами в конце путей, они должны отсутствовать.
~~~~~~~
