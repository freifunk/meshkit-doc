.. include:: /links.txt

.. _shell:

Die Shell des Routers
=====================

Auf dem Freifunkrouter läuft ja bekanntlich ein `OpenWrt`_ System.
Hier gibt es viele der üblichen Kommandos die man aus der Linux/Unix
Welt kennt. Wer bereits Erfahrung mit der Shell (oder auf Deutsch:
Kommandozeile) auf unix-artigen Systemen hat wird sich auf OpenWrt
leicht zurechtfinden.

.. hint::

   Man muss die Shell nicht unbedingt verwenden. Vieles ist auch
   über die LuCI Weboberfläche zu bewerkstelligen. Einige Dinge lassen
   sich aber nur auf der Shell erledigen. Oder zumindest einfacher und
   schneller.


Im Folgenden sollen einige wichtige und nützliche Kommandos
vorgestellt werden.

.. hint::

   Die meisten Kommandos haben eine eingebaute Hilfe die angezeigt
   wird, wenn man dem Kommando ``-h`` oder ``--help`` anhängt.


Bewegen im Dateisystem
----------------------

Wie unter Linux üblich gibt es ein Rootverzeichnis, das :file:`/` heisst.
Alle weiteren Verzeichnisse sind Unterverzeichnisse davon.

Den Inhalt des aktuellen Ordners kann man mit :command:`ls`
anzeigen.

Um sich im Dateisystem zu bewegen wird :command:`cd` verwendet:

* :command:`cd ..` - Eine Ebene nach oben wechseln
* :command:`cd /` - Ins Rootverzeichnis wechseln
* :command:`cd <verzeichnis>` - Ins Verzeichnis <verzeichnis> wechseln, z.B.
* :command:`cd /etc/config` - wechselt ins Verzeichis :file:`/etc/config`


Dateien anzeigen und editieren
------------------------------

Standardmässig ist :command:`vi` als Editor auf dem Router installiert. Für Hilfe
zur Benutzung von :command:`vi` siehe:

`Dateien bearbeiten im OpenWrt Wiki <http://wiki.openwrt.org/de/doc/howto/user.beginner.cli#dateien.bearbeiten>`_

Als Alternative zu :command:`vi` ist auch :command:`nano` empfehlenswert. Dieser ist
nicht per Default installiert sondern muss nachinstalliert werden. Entweder direkt
von Meshkit mit ins Firmwareimage bauen lassen (siehe :ref:`meshkit-packages`)
oder nachinstallieren (:ref:`packages`).

Will man eine **Datei nur anzeigen**, dann kann hierfür auch :command:`cat`
verwendet werden, z.B. :command:`cat /etc/banner`.


Netzwerkprobleme debuggen
-------------------------

.. _command-nslookup:

nslookup
^^^^^^^^

Sagt uns ob die Namensauflösung für einen Rechner funktioniert.

Beispiel::

  root@freifunk:~# nslookup google.com
  Server:    127.0.0.1
  Address 1: 127.0.0.1 localhost
  
  Name:      google.com
  Address 1: 2a00:1450:4001:c02::65 fa-in-x65.1e100.net
  Address 2: 173.194.70.101 fa-in-f101.1e100.net

aber::

  root@freifunk:~# nslookup domaindieesnichtgibt.de
  Server:    127.0.0.1
  Address 1: 127.0.0.1 localhost
  
  nslookup: can't resolve 'domaindieesnichtgibt.de': Name or service not known

.. _command-ping:

ping und ping6
^^^^^^^^^^^^^^

Sendet ICMP echo requests an einen Rechner und zeigt die Paketlaufzeit,
sofern der ``angepingte`` Rechner antwortet.

Beispiel::

  root@freifunk:~# ping google.de
  PING google.de (173.194.32.255): 56 data bytes
  64 bytes from 173.194.32.255: seq=0 ttl=52 time=51.953 ms
  64 bytes from 173.194.32.255: seq=1 ttl=52 time=51.293 ms
  [...]

Für IPv6 verwendet man dementsprechend :command:`ping6`::

  root@freifunk:~# ping6 google.de
  PING google.de (2a00:1450:4016:803::1017): 56 data bytes
  64 bytes from 2a00:1450:4016:803::1017: seq=0 ttl=55 time=135.781 ms
  64 bytes from 2a00:1450:4016:803::1017: seq=1 ttl=55 time=132.964 ms

Bekommen wir eine Antwort vom Rechner, dann funktioniert die Kommunikation
mit ihm. Wenn nicht dann kann uns :ref:`command-traceroute` unter Umständen
sagen, wo das Problem liegt.

.. _command-traceroute:

traceroute und traceroute6
^^^^^^^^^^^^^^^^^^^^^^^^^^

Mit :command:`traceroute` kann die Route eines Pakets durchs Netz
verfolgt werden.

Beispiel::

  root@freifunk:~# traceroute openlab-indoor.ffa
  traceroute to openlab-indoor.ffa (10.11.0.20), 30 hops max, 38 byte packets
  1  kerberos.ffa (10.11.0.17)  4.165 ms  2.102 ms  1.234 ms
  2  mid1.hirsch.ffa (10.11.63.22)  66.282 ms  62.849 ms  61.505 ms
  3  openlab-balkon.ffa (10.11.0.21)  73.504 ms  94.929 ms  86.264 ms
  4  openlab-indoor.ffa (10.11.0.20)  97.610 ms  83.681 ms  78.941 ms

Hier wird die Route zu einem anderen Knoten im Mesh-Netzwerk. Das Paket
durchläuft die Router 1, 2, 3 um schliesslich bei 4 das Ziel zu erreichen.

Für IPv6 wird :command:`traceroute6` verwendet, das im Paket ``iputils-traceroute6``
enthalten ist und u.U. erst noch installiert werden muss, siehe :ref:`packages`.



