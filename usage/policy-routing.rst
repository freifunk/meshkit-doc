.. include:: /links.txt

.. _policy-routing:

Policy Routing
==============

Policy Routing ist nicht ganz einfach und im Idealfall musst du als Benutzer
darüber auch nicht viel wissen, da es einfach im Hintergrund dafür sorgt,
dass das Mesh-netzwerk richtig funktioniert. Dennoch sollen hier einige Grundlagen
des Policy Routings in der Meshkit Firmware erklärt werden, damit die
Möglichkeit besteht zu begreifen was hier passiert.

Was ist Policy Routing und wofür ist es gut?
--------------------------------------------

.. image:: /images/policy-routing.png
   :alt: Policy Routing
   :width: 40%
   :align: right

Das ``freifunk-policyrouting``-Paket ist standardmässig installiert und wird
benötigt, um Traffic je nach Herkunft unterschiedlich weiterleiten zu können.

**Eigener Traffic** soll über die eigene Internetleitung geschickt werden, während
Traffic aus dem Freifunknetz auch nur ins Freifunknetz weitergeleitet werden darf.
Als eigener Traffic ist per Default aller Traffic definiert, der entweder **lokal**,
also vom Freifunkrouter selbst generiert wurde oder der aus der Firewall-Zone ``LAN``
kommt. Als **Freifunktraffic** wird alles betrachtet, was das Node über ein
Interface erreicht hat, das zur Zone ``Freifunk`` gehört. Für eine Erklärung der Zonen
siehe: :ref:`net-zones`.

Das Bild verdeutlicht dies:

* Datenverkehr von dem am ``LAN`` angeschlossenen PC wird über die eigene Leitung
  verschickt, lokal vom Knoten selbst ausgehender Traffic ebenfalls
* Verkehr der über ``Freifunk`` hereinkommt (in diesem Fall über WLAN) darf das
  Node nur Richtung Freifunk verlassen. In diesem Fall wird dieser Verkehr
  ebenfalls wieder über WLAN ins Mesh weitergeschickt, um darüber sein
  Ziel zu erreichen.

.. container:: clearer

   .. no content here, this is just to clear the float from previous image

Konfiguration von Policy Routing
--------------------------------

.. warning::

   Diese Einstellungen sind komplex und es ist leicht Dinge kaputt
   zu machen. Ändere an den EInstellungen also nur etwas, wenn du genau
   weisst was du tust.

Mit Luci
^^^^^^^^

Policy Routing kann in LuCI unter
:menuselection:`Administration --> Freifunk --> Policy Routing`
konfiguriert werden. Das Paket das diese Funktion für LuCI
mitbringt ist ``luci-app-freifunk-policyrouting`` und ist in der
Regel schon installiert. Ansonsten muss es noch nachinstalliert werden,
siehe :ref:`packages`.

.. image:: /images/luci/policy-routing.jpg
   :alt: Policy Routing im LuCI Webinterface

.. hint::

   Will man auch Traffic aus der ``LAN``-Zone nur übers Freifunknetz
   schicken dann fügt wählt man bei :guilabel:`Firewallzonen` zusätzlich
   auch noch ``lan`` aus.

Auf der Shell
^^^^^^^^^^^^^

Die Konfiguration für das policy Routing befindet sich in
:file:`/etc/config/freifunk-policyrouting` und kann entweder direkt
bearbeitet werden oder mit :command:`uci` verändert werden.

.. code-block:: sh

   config 'settings' 'pr'
   	option 'strict' '1'
   	option 'fallback' '1'
   	option 'zones' 'freifunk'
   	option 'enable' '1'

Zum Bearbeiten mit :command:`uci`:

.. code-block:: sh

   uci set freifunk-policyrouting.pr.strict=1
   uci set freifunk-policyrouting.pr.fallback=1
   uci set freifunk-policyrouting.pr.enable=1
   uci set freifunk-policyrouting.pr.zones=freifunk
   uci commit freifunk-policyrouting.pr.zones

Beschreibung der einzelnen Optionen:

.. list-table::
   :widths: 25 50 25
   :header-rows: 1

   * - Option
     - Beschreibung
     - Default
   * - strict
     - Wenn keine Route ins Internet über das Freifunknetz verfügbar
       ist dann würde Traffic aus ``Freifunk`` über die eigene Leitung
       geroutet. Der Wert 1 verhindert dies durch Einfügen einer Firewallregel
       die dies verbietet. Will man in diesem Fall erlauben den Traffic über
       das eigene Internet zu routen dann den Wert auf 0 setzen
     - 1
   * - fallback
     - Gibt an, ob bei Ausfall der eigenen Internetleitung der eigene
       Traffic über Freifunk ins Internet geroutet werden soll. Wird als
       Wert 1 gesetzt dann darf Freifunk als Fallback verwendet werden.
     - 0
   * - enable
     - 1 aktiviert Policy Routing, 0 deaktiviert es
     - 0
   * - zones
     - Zonen die für die die Policy Routing Regeln gelten sollen.
     -

Wurden alle Einstellungen gemacht/verändert dann muss Policy Routing noch mit

.. code-block:: sh

   /etc/init.d/freifunk-policyrouting restart

neu gestartet werden, damit diese Konfiguration aktiv wird.

.. hint::

   Will man auch Traffic aus der ``LAN``-Zone nur übers Freifunknetz
   schicken dann fügt man der Option ``zones`` noch ``lan`` hinzu, also
   zones="freifunk lan".

Infos zu Routingtabellen und zum Debuggen
-----------------------------------------

Zum Anzeigen und Verändern der Routingtabellen und -regeln wird
:command:`ip` verwendet. Wichtige Kommandos von ip sind in diesem
Zusammenhang:

.. list-table::
   :widths: 40 60
   :header-rows: 1

   * - Kommando
     - Aktion
   * - ip rule show
     - zeigt alle Regeln
   * - ip route show
     - zeigt alle Routen aus der ``main`` Routingtabelle
   * - ip route show table olsr
     - zeigt alle Routen aus der ``olsr`` Routingtabelle

Zunächst wollen wir uns für einen Überblick alle Regeln ansehen und geben
dazu auf der Shell des Routers :command:`ip rule show` ein. Als Ausgabe
erscheint auf einem Knoten auf dem Policyrouting für die Freifunkzone
aktiviert wurde:

.. code-block: sh

   0:		from all lookup local
   1000:	from all lookup olsr 
   2000:	from all lookup localnets 
   20000:	from all iif wlan0 lookup olsr-default 
   20000:	from all iif wlan0-1 lookup olsr-default 
   20001:	from all iif wlan0 unreachable
   20001:	from all iif wlan0-1 unreachable
   32766:	from all lookup main 
   32767:	from all lookup default 
   100000:	from all lookup olsr-default

Die Zahl in der ersten Spalte gibt die **Priorität** an. In dieser Reihenfolge
werden die Regeln durchlaufen. Danach folgen **Bedingungen** die zutreffen
müssen, damit ein Paket das weitergeleitet werden soll diese Regel benutzt.
Zum Beispiel meint ``from all iif wlan0-1`` von allen IPs, von denen Pakete
über die Schnittstelle ``wlan0-1`` empfangen wurden. Schliesslich folgt eine
Anweisung, wie mit dem Paket zu verfahren ist. ``unreachable`` besagt, wir brechen
hier ab wenn zuvor keine passende Route gefunden wurde. ``lookup`` dagegen
ist eine Anweisung, zu welcher Routingtabelle gesprungen werden soll um dort eine
passende Route  zu finden. Wird keine passende Route in dieser Tabelle gefunden
dann wird mit der nächsten regel fortgefahren.

Für Policy Routing sind folgende Routingtabellen von Bedeutung:

.. list-table::
   :widths: 30 70
   :header-rows: 1

   * - Tabelle
     - Beschreibung
   * - local
     - enthält lokale Hostrouten
   * - olsr
     - enthält alle von OLSR empfangenen Routen mit Ausnahme der
       Defaultroute
   * - olsr-default
     - enthält die von OLSR empfangene Defaultroute (sofern vorhanden)
   * - localnets
     - Enthält Routen zu direkt erreichbaren Netzwerken
   * - main
     - Dies ist die Hauptroutingtabelle von Linux. Hier steht unter
       anderem die eigene Defaultroute über den eigenen Internetzugang
       (sofern vorhanden).
   * - default
     - wird nicht verwendet

Zwei Beispiele sollen helfen zu verstehen, wie Traffic aus verschiedenen Zonen
anders behandelt wird:

Fluss von Traffic aus der LAN-Zone oder vom Router selbst
---------------------------------------------------------

Von einem an ``LAN`` angeschlossenen Rechner oder vom Node selbst soll
eine Verbindung mit einer Adresse im Internet aufgebaut werden. Daher muss
eine Defaultroute gefunden werden.

Zunächst werden die Regeln 0, 1000 und 2000 durchlaufen und dementsprechend die
Tabellen ``local``, ``olsr`` und ``localnets`` befragt. Hier wird jedoch keine
Defaultroute gefunden. Die Regeln mit den Prioritäten 20000 und 20001 werden
übersprungen, da die Bedingungen ``iif wlan0`` bzw. ``iif wlan0-1`` nicht
zutreffen. Die Regel mit der Priorität 32766 verweist nun auf die ``main``
Routingtabelle. Wird hier nun eine Defaultroute gefunden, dann benutzt das
Paket die gefundene Route. Wird keine gefunden suchen wir weiter. Die ``default``
Tabelle ist leer und wird daher übersprungen. Wurde ausgewählt dass Fallback übers
Freifunknetz möglich sein soll erreichen wir schliesslich die Regel mit der Priorität
100000, die die Anweisung enthält, in der Tabelle ``olsr-default`` nachzuschauen. Wird
dort eine defaultroute gefunden wird diese benutzt und das Paket übers Mesh geschickt.
Wenn nicht dann hatten wur kein Glück. In diesem Fall kennt das Node keine Route ins
Internet.

Fluss von Traffic aus der Freifunk-Zone
---------------------------------------

Ein über WLAN verbundener Rechner oder Access Point will eine Verbindung
mit einer Adresse im Internet aufbauen. Der Rechner ist über die Schnittstelle
``wlan0`` verbunden, die zur Zone ``Freifunk`` gehört. Es muss nun wiederum
eine Defaultroute gefunden werden.

Zunächst werden die Regeln 0, 1000 und 2000 durchlaufen und dementsprechend die
Tabellen ``local``, ``olsr`` und ``localnets`` befragt. Hier wird jedoch keine
Defaultroute gefunden. Eine der Regeln mit der Priorität 20000 trifft zu,
es wird also die Tabelle ``olsr-default`` befragt. Wird in dieser Tabelle
eine Defaultroute gefunden, dann wird diese benutzt. Wird keine gefunden geht
es weiter mit den Regeln mit Priorität 20001 die hier sind, weil die Option
``strict`` gewählt wurde. Die Suche nach einer Route wird hier durch die
Anweisung ``unreachable`` abgebrochen und dem Rechner mitgeteilt, dass der Knoten
keine defaultroute für ihn kennt. Hätte man nicht die Option ``strict``
gewählt dann würde als nächstes die Tabelle ``main`` befragt und dort die eigene
Defaultroute des knotens gefunden und benutzt.

Links
-----

* `Policy Routing im Freifunk Wiki <http://wiki.freifunk.net/Kamikaze/freifunk-policyrouting>`_
* `Quellcode von freifunk-policyrouting <http://luci.subsignal.org/trac/browser/luci/trunk/contrib/package/freifunk-policyrouting>`_
