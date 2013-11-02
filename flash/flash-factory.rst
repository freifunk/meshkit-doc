.. include:: /links.txt

Router flashen auf denen die Originalfirmware installiert ist
=============================================================

Hier wird beschrieben, wie ein Meshkit Firmwareimage auf einen
Router geflasht werden kann, auf dem noch die Originalfirmware
läuft.

.. warning::

   Nach dem Flashen sollte als erstes das Passwort des Routers
   geändert werden, siehe :ref:`change-password`.

Eine Frage die immer wieder auftaucht ist: "Welches Image muss ich benutzen?"
Letztendlich verschafft hier nur ein Blick in die `Table of Hardware`_
Klarheit. Als Faustregel kann man jedoch sagen:

* Für **ar71xx** muss ein Image mit **factory** im Namen verwendet werden,
  wenn auf dem Router noch die Originalfirmware läuft.
* Für **brcm47xx** dagegen muss hier in der Regel ein Image mit der Endung
  **.bin** verwendet werden.

Es gibt sehr viele Router die mit OpenWrt geflasht werden können. Dementsprechend
gibt es verschiedene Wege. Manche Router sind leicht zu flashen, z.B. übers
Webinterface der Originalfirmware, bei anderen muss man etwas mehr Aufwand
betreiben (z.B. Über serielle Konsole und/oder :term:`TFTP`). Aufschluss
darüber, wie ein bestimmtes Modell geflasht werden kann gibt, wie so oft,
die OpenWrt `Table of Hardware`_.

Firmware übers Webinterface der Originalfirmware flashen
--------------------------------------------------------

Das ist der einfachste Weg und funktioniert auf vielen Routern. Es kann an
dieser Stelle nicht für jedes Modell einzeln erklärt werden, die genau der
Router über die oroginale Weboberfläche geflasht werden kann. Das Prinzip ist
jedoch bei den meisten Routern ähnlich. Im Folgenden ein paar Beispiele von
beliebten Routern mit grosser Verbreitung. Wie immer lohnt auch ein Blick
in die `Table of Hardware`_, besonders für hier nicht aufgeführte Modelle.

Flashen von TP-Link Routern über das originale Webinterface
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

TP-Link Router  können leicht über das originale Webinterface geflasht werden.
Per Default haben sie auf den LAN-Schnittstellen entweder ``192.168.0.1`` oder
``192.168.1.1`` als IP-Adresse.

Zunächst musst der PC mit einem der LAN-Ports des Routers verbunden werden,
siehe dazu auch :ref:`connect-router`.

Rufe dann im Browser die Seite http://192.168.0.1 bzw
http://192.168.1.1 auf. Dort wirst du zunächst nach Benutzername und
Passwort gefragt, welches bei TP-Link beide **admin** sind.

Nach erfolgreichem Login sieht man die originale Weboberfläche des Routers.
Wähle hier 
:menuselection:`System Tools --> Firmware Upgrade` (1)

.. image:: /images/flash/tp-link/vendor-flash-select-firmware.png
   :alt: Auswahl der zu flashenden Firmware im TP-Link Webinterface

Danach wähle durch einen Klick auf :guilabel:`Browse` (2) die mit Meshkit
generierte (siehe :ref:`meshkit-fulldoc`) und auf den eigenen Rechner
heruntergeladene Firmware aus. Hier wird ein Image mit **factory** im Namen
benötigt. Klicke anschliessend auf :guilabel:`Upgrade` (3). Es kommt eine
Nachfrage, ob man sich wirklich sicher ist, die Firmware flashen zu wollen.

.. image:: /images/flash/tp-link/flash-confirm.png
   :alt: Bestätigung dass man wirklich Flashen will in der TP-Link Firmware

Nochmal überlegen... wollen wir natürlich. Also noch eben mit einem Klick
auf :guilabel:`OK` bestätigen um den Upgradevorgang zu starten:

.. image:: /images/flash/tp-link/flashing.png
   :alt: Flashvorgang läuft in der TP-Link Firmware

War der Flashvorgang erfolgreich, dann erscheint noch

.. image:: /images/flash/tp-link/flash-successful.png
   :alt: Flashvorgang war erfolgreich in der TP-Link Firmware

Gut. Die Meshkit Firmware ist nun installiert. OpenWrt verwendet per Default
``192.168.1.1`` als IP der LAN-Schnittstelle, der automatische Reload der
Seite funktioniert als nicht, wenn als IP zum Zugriff auf den Router
hier ``192.168.0.1`` verwendet wurde. Zum Verbinden mit dem frisch
geflashten Freifunkrouter siehe :ref:`connect-router`.
