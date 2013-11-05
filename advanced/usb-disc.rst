.. include:: /links.txt

USB Speichermedien einbinden
============================

USB-Speichermedien wie externe Festplatten oder USB-Sticks lassen
sich an Routern die einen USB-Port verfügen einfach einbinden.

Notwendige Pakete installieren
------------------------------

Grundlegender USB-Suport muss bereits verfügbar sein. Dies sollte
jedoch auf den meisten geräten mit USB-Bereits der Fall sein. Wenn nicht
dann siehe `Basic USB Support im OpenWrt Wiki <http://wiki.openwrt.org/doc/howto/usb.essentials>`_.

Folgende Pakete müssen für USB-Speichermedien installiert werden,
siehe :ref:`packages`.

* kmod-usb-storage
* block-mount

.. warning::

   ``block-mount`` kollidiert zur Zeit mit Datenien, die bereits von
   ``busybox`` bereitgestellt werden. Es ist daher notwendig die beiden oben
   gennanten Pakete mit :command:`opkg install kmod-usb-storage block-mount --force_overwrite`
   zu installieren. Eine Installation über LuCI ist daher derzeit nicht möglich.

Je nach auf dem externen Medium verwendeten Dateisystem werden noch
``kmod-fs-*`` Pakete benötigt, die geläufigsten sind:

* kmod-fs-ext4 - für EXT2, EXT3 und EXT4
* kmod-fs-ntfs - für NTFS
* kmod-fs-reiserfs - Für ReiserFS
* kmod-fs-vfat - Für VFAT/FAT32

Wird kmod-fs-vfat verwendet dann muss zusätzlich auch noch eine ``Codepage``
installiert werden, diese befinden sich in den Paketen ``kmod-nls-cp*``. Es
ist empfehlenswert, diese Codepages zu installieren:

* kmod-nls-cp437
* kmod-nls-cp850
* kmod-nls-cp852

Ferner wird Support für Charsets benötigt:

* kmod-nls-iso8859-1
* kmod-nls-iso8859-15

.. _usb-automount:

Automatisches mounten aktivieren
--------------------------------

Nach Installation der benötigten Pakete sind zwei Laufwerke vorkonfiguriert aber
nicht aktiviert. Sie verhindern damit ein automatisches Einbinden von externen
Laufwerken ins Filesystem. Es ist empfehlenswert diese zunächst zu löschen::

  uci del fstab.@mount[0]
  uci del fstab.@swap[0]
  uci commit fstab

Per Default werden nun externe Laufwerke sobald sie angeschlossen wurden automatisch
an einen Mountpoint in :file:`/mnt/` gemounted, z.B. nach :file:`/mnt/sda1`.

Wenn das nicht funktioniert dann schaue im :ref:`show-syslog` nach ob dort
Fehlermeldungen sichtbar sind.

.. _usb-manualmount:

USB-Storage-Geräte konfigurieren und feste Mountpoints zuweisen
---------------------------------------------------------------

Für bessere Kontrolle darüber, wohin Geräte gemounted werden und die Möglichkeit
Mountoptionen anzugeben müssen USB-Storage-Geräte manuell konfiguriert werden.
Dies kann via LuCI oder auf der Shell des Routers geschehen.

.. _fstab-options:

Optionen für das Mounten externer Speichermedien
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Folgende Optionen können sowohl mit :ref:`LuCI <luci-fstab>` als
auch auf der :ref:`Shell <shell-fstab>` gesetzt werden, um das Einbinden
externer Medien zu kontrollieren:

.. tabularcolumns:: |p{4.5cm}|p{3cm}|p{7.5cm}|

.. list-table::
   :widths: 30 20 50
   :header-rows: 1

   * - Option LuCI
     - Option Shell
     - Beschreibung
   * - Aktiviert
     - enabled
     - Bestimmt ob dieses Gerät eingebunden wird
   * - Schnittstelle
     - device
     - Gerät/Partition das gemounted werden soll. Wenn :guilabel:`UUID` oder
       :guilabel:`Label` ebenfalls gesetzt sind, dann werden diese
       bevorzugt.
   * - Mountpunkt
     - target
     - Der Mountpunkt, an den das Gerät im dateisystem des Routers eingebunden
       werden soll. Übliche Praxis ist, Geräte unterhalb von :file:`/mnt/`
       einzuhängen.
   * - Dateisystem
     - fstype
     - Dateisystem auf dem Datenträger, der eingebunden werden soll, z.B. vfat,
       ext2, ext3, ext4 oder ntfs. Wird diese option weg bzw. leer gelassen,
       dann wird versucht den Typ des Dateisystems automatisch zu erkennen.
   * - UUID
     - uuid
     - :term:`UUID` des Gerätes oder der Partition, die eingehängt werden soll.
   * - Label
     - label
     - Label des Dateisystems, das eingehängt werden soll. Wird gleichzeitig eine
       :guilabel:`UUID` konfiguriert dann hat die UUID Vorrang.
   * - Mount-Optionen
     - options
     - Spezielle Mount-Optionen, die dem :command:`mount`-Kommando mitgegeben
       werden sollen. Für mögliche Optionen siehe:
       `mount Manpage <http://linux.die.net/man/8/mount>`_.

.. _luci-fstab:

Externe Medien in LuCI konfigurieren
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Externe Speichermedien und deren Mountpoints können in LuCI unter
:menuselection:`Administration --> System --> Einhängepunkte`
verwaltet werden.

.. image:: /images/luci/mount-points-add.png
   :alt: Übersicht Mountpoints konfigurieren in LuCI

Klicke direkt darunter auf :guilabel:`Hinzufügen`, um ein externes
Speichermedium manuell zu mounten. Es öffnet sich eine Seite,
auf der das Medium konfiguriert werden muss.

.. image:: /images/luci/mount-points-add-detail.png
   :alt: Mountpoints hinzufügen bzw. konfigurieren in LuCI

Wähle unter :guilabel:`Schnittstelle` ein Gerät aus. Alternativ kann das Gerät auch
im Tab :guilabel:`Erweiterte Einstellungen` anhand seiner :guilabel:`UUID` oder
seinem :guilabel:`Label` identifiziert werden. Wähle anschliessend unter
:guilabel:`Mountpunkt` den Mountpoint im Dateisystem des Routers aus. Unter
:guilabel:`Dateisystem` kann das Dateisystem das sich auf dem externen Medium
befindet ausgewählt werden. Wird dies leer gelassen dann wird versucht das
Dateisystem automatisch zu erkennen. Unter :guilabel:`Mount-Optionen` im Tab
:guilabel:`Erweiterte Einstellungen` können weitere Optionen für den
:command:`mount`-Befehl angegeben werden, der dafür zuständig ist, die
externen Medien ins Dateisystem einzubinden.

Für eine Erklärung der einzelnen Optionen siehe :ref:`fstab-options`.

.. _shell-fstab:

Externe Medien auf der Shell konfigurieren
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Die Einstellungen für externe Datenträger befinden sich in der Datei
:file:`/etc/config/fstab`. Für jeden Mountpunkt wird eine Sektion vom
Typ ``mount``::

  config mount 'myusbstick' 
  	option enabled '1'
  	option device '/dev/sda1'
  	option target '/mnt/myusbstick'

ALternativ können diese Einstellungen auch mit :command:`uci` gemacht werden::

  uci set fstab.myusbstick=mount
  uci set fstab.myusbstick.enabled=1
  uci set fstab.myusbstick.device="/dev/sda1"
  uci set fstab.myusbstick.target="/mnt/myusbstick"

Für eine Erklärung der einzelnen Optionen siehe :ref:`fstab-options`.


Weiterführende Links
--------------------

* `USB Storage im OpenWrt Wiki <http://wiki.openwrt.org/doc/howto/usb.storage>`_

