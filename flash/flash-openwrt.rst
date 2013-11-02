.. include:: /links.txt

Router flashen auf dem bereits OpenWrt installiert ist
======================================================

Hier wird beschrieben, wie ein neues Meshkit Firmwareimage auf einen
Router geflasht werden kann, auf dem bereits eine auf OpenWrt basierende
Firmware läuft.

.. warning::

   Nach dem Flashen sollte als erstes das Passwort des Routers
   geändert werden, siehe :ref:`change-password`.

Eine Frage die immer wieder auftaucht ist: "Welches Image muss ich benutzen?"
Letztendlich verschafft hier nur ein Blick in die `Table of Hardware`_
Klarheit. Als Faustregel kann man jedoch sagen:

* Für **ar71xx** muss ein Image mit **sysupgrade** im Namen verwendet werden,
  wenn von einem bereits laufenden OpenWrt System aus geflasht wird.
* Für **brcm47xx** dagegen muss hier in der Regel ein Image mit der Endung
  **.trx** verwendet werden.

.. _flash-luci:

Firmware im LuCI Webinterface flashen
-------------------------------------

Bei den meisten Routern kann Über über das LuCI Webinterface einfach
eine neue Firmware installiert werden. Gehe dazu zu

:menuselection:`Administration --> System --> Backup / Firmware Update`

.. image:: /images/luci/firmware-upgrade.png
   :alt: Firmware Upgrade in LuCI

Entferne zunächst den Haken bei :guilabel:`Konfiguration sichern` (1).

.. hint::

  In den meisten Fällen ist es sinnvoll, die bestehende Konfiguration nicht zu
  sichern und ein komplett frisches Meshkit Firmwareimage zu flashen. Willst du
  dennoch deine schon bestehende konfiguration sichern, dann wähle beim Generieren
  des Images mit Meshkit die Option ``Keine Konfiguration`` (siehe
  :ref:`meshkit-step1-settings`) aus und lasse den Haken
  oben bei :guilabel:`Konfiguration sichern` stehen. Es kann dabei jedoch zu
  Problemen kommen, z.B. wenn deine bestehende Konfiguration veraltet ist und
  daher nicht mehr macht, was sie soll. 


Klicke anschliessend auf :guilabel:`Browse` (2) und wähle im sich öffnenden
Dialog das von Meshkit für deinen Router generierte Firmwareimage (siehe
:ref:`meshkit-fulldoc`) aus. Klicke anschliessend auf
:guilabel:`Firmware aktualisieren...` (3). 

Anschliessend will LuCI nocheinmal eine Bestätigung, dass man das Image
wirklich Flashen will. Im Fehlerfall wird eine Meldung über den Fehler
der aufgetreten ist ausgegeben. Normalerweise sollte aber die Aufforderung
zur Bestätigung des Flashvorgangs erscheinen:

.. image:: /images/luci/firmware-upgrade-verify.png
   :alt: Firmware Upgrade bestätigen in LuCI

Klicke hier auf :guilabel:`Fortfahren` um mit dem Flashvorgang zu
beginnen und folgenden Hinweis zu sehen:

.. image:: /images/luci/firmware-upgrade-flashing.png
   :alt: Firmware Upgrade läuft in LuCI

Beachte insbesondere den Hinweis, dass der Router während des Flashens
nicht vom Strom getrennt werden sollte.

Das dauert jetzt ein paar Minuten. Hat sich die IP-Adresse des Routers,
mit der du verbunden bist nicht geändert, dann sollte nach Beenden des
Flashvorgangs die Startseite des Routers neu geladen werden. Ist dies auch
nach längerer Zeit nicht der Fall, musst du evtl. die IP-Adresse auf deinem
Computer neu einstellen. Siehe dazu :ref:`connect`.


.. _flash-sysupgrade:

Firmware mit sysupgrade auf der Shell flashen
---------------------------------------------

Ist bereits Openwrt installiert, dann kann eine neue Firmware auch sehr einfach
über SSH (:ref:`connect-ssh`) mit :command:`sysupgrade` installiert werden.

Auf der Shell des Routers gibt man dafür ein::

  cd /tmp
  wget <link zum image>
  sysupgrade -n <heruntergeladenes image>


.. hint::

  Hat der Router keine eigene Internetverbindung und :command:`wget` kann
  das Image daher nicht direkt herunterladen, dann kann ein Image auch mit
  :command:`scp` nach :file:`/tmp` auf dem Router kopiert werden, siehe
  :ref:`scp`.


.. hint::

  Das :command:`sysupgrade` Kommando sichert durch die Angabe des "-n"
  Parameters keine Konfiguration. Das ist in der Regel gewollt, da es in den
  meisten Fällen sinnvoller ist, die bestehende Konfiguration nicht zu
  sichern und ein komplett frisches Meshkit Firmwareimage zu flashen. Willst du
  dennoch deine schon bestehende konfiguration sichern, dann wähle beim Generieren
  des Images mit Meshkit die Option ``Keine Konfiguration`` (siehe
  :ref:`meshkit-step1-settings`) aus und lasse den "-n" Parameter weg.
  Es kann dabei jedoch zu   Problemen kommen, z.B. wenn deine bestehende
  Konfiguration veraltet ist und   daher nicht mehr macht, was sie soll.

