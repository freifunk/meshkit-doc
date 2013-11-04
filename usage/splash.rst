.. _splash:

DHCP-Splash
===========

Der DHCP-Splash ist ein Captive Portal, mit dem Gästen im Freifunknetz beim
ersten Zugriff aufs Internet eine **Begrüssungsseite** angezeit werden kann.

Splash kann ausserdem die **Bandbreite im Up- und Download pro Nutzer limitieren**.
Zudem ist es möglich, einzelne Clients dauerhaft zu erlauben
(:ref:`Whitelist <whitelist>`) oder dauerhaft zu sperren
(:ref:`Blacklist <blacklist>`).

Gründe Splash einzusetzen sind

* Nutzer auf Freifunk aufmerksam machen und zum mitmachen auffordern
* Nutzer über Risiken aufklären (Netzwerk ist unverschlüsselt)
* Nutzungsbedingungen anzeigen und den Nutzer akzeptieren lassen. "Mach nix
  verbotenes". Dies ist in rechtlicher Hinsicht sinnvoll.
* Indem man durch den Splash eine regelmässige Zwangstrennung hat wird das
  Freifunknetz für Filesharer eher uninteressant.
* Hinweis auf den Knotenbetreiber
* Lokalitätsbezogene Informationen anzeigen
* Manche nennen den Splash auch "Nervseite". Indem reine Nutzer mit dem
  Splash genervt werden, sollen sie ermutigt werden, sich aktiv am Netz
  zu beteiligen.

Gegen einen Einsatz von Splash spricht

* Nutzer werden genervt. Man kann dies aber auch wie oben als Vorteil sehen.
* Aus technischen Gründen funktioniert nur eine Umleitung von HTTP-Requests.
  Daher können sowohl HTTPS als auch alle anderen Protokolle nicht aufs Internet
  zugreifen, solange der Splash nicht akzeptiert wurde indem zumindest einmal
  eine HTTP-Seite aufgerufen wird.

.. warning::

   Splash darf nicht andere aktive Teilnehmer/Knoten im Freifunknetz behindern.
   Daher Splash nur auf Schnittstellen betreiben, die nur für Clients (Gäste) benutzt
   werden.

.. _splash-status:

Splash Status anzeigen
----------------------

Statusinformationen zum Splash (verbundene Clients, Blacklist, Whitelist)
können entweder in LuCi oder auf der Shell angezeigt werden.

.. _splash-status-luci:

Splash Status in LuCI anzeigen
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Gehe zu :menuselection:`Administration --> Status --> Client-Splash`. Es werden
alle derzeit verbundenen Clients angezeigt:

.. image:: /images/luci/splash/splash-admin-status.png
   :alt: Splash Status in LuCI (Admin)

:guilabel:`Richtlinie` zeigt den aktuellen Status des Clients. Dieser kann dort
auch geändert werden.

.. tabularcolumns:: |p{5cm}|p{10cm}|

.. list-table::
   :widths: 25 75
   :header-rows: 1

   * - Richtlinie
     - Beschreibung
   * - erlaubt
     - Client ist auf der Whitelist.
   * - gesplasht
     - Client hat den Splash akzeptiert und ist freigeschalten
   * - vorübergehend geblockt
     - Lease des Clients entfernen. Er kann sich jedoch erneut
       freischalten.
   * - gesperrt
     - Client dauerhaft auf die Blacklist setzen. Er kann den
       Splash dann nicht mehr akzeptieren und bekommt stattdessen
       eine Meldung angezeit, dass er geblockt wurde.

.. hint::

   Statusinformationen zu Splash-Clients sind in teilweise anonymisierter
   Form auch im öffentlichen Teil des Webinterfaces sichtbar:
   :menuselection:`Freifunk --> Status --> Splash`.

.. hint::

   Mit dem Paket ``collectd-mod-splash-leases`` können Grafiken über
   verbundene Clients erstellt werden.

.. _splash-status-shell:

Splash Status auf der Shell anzeigen
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Informationen zu verbundenen Clients erhält man auf der Shell mit::

  luci-splash list



.. _splash-general-settings:

Allgemeine Einstellungen für Splash
-----------------------------------

Hier können die Freigabezeit, Up-/Downloadlimit und ein Weiterleitungsziel
eingerichtet werden.

.. tabularcolumns:: |p{4cm}|p{3cm}|p{8cm}|

.. list-table::
   :widths: 25 15 60
   :header-rows: 1

   * - Option LuCI
     - Option Shell
     - Beschreibung
   * - Freigabezeit
     - leasetime
     - Die Freigabezeit in Stunden. So lange kann der Gast das Netz benutzen,
       bevor er den Splash erneut akzeptieren muss.
   * - Ziel für Weiterleitung
     - redirect_url
     - Auf diese Seite wird der Nutzer nach Akzeptieren des Splashs
       weitergeleitet. Wird die Option leer gelassen, dann wird der Nutzer
       direkt auf die Seite weitergeleitet, auf die er ursprünglich
       zugreifen wollte.
   * - Upload-Begrenzung
     - limit_up
     - Upload-Limit in KByte pro Sekunde. Die Limitierung gilt pro Client.
       Ein Wert von 0 deaktiviert die Begrenzung. Clients die auf der
       :ref:`Whitelist <whitelist>` stehen sind von der Begrenzung ausgenommen. 
   * - Download-Begrenzung
     - limit_up
     - Download-Limit in KByte pro Sekunde. Die Limitierung gilt pro Client.
       Ein Wert von 0 deaktiviert die Begrenzung. Clients die auf der
       :ref:`Whitelist <whitelist>` stehen sind von der Begrenzung ausgenommen. 


In LuCI
^^^^^^^

Öffne :menuselection:`Administration --> Dienste --> Client-Splash`. Ganz oben
siehst du gleich die allgemeinen Einstellungen für Splash:

.. image:: /images/luci/splash/splash-settings-general.png
   :alt: Allgemeine Einstellungen von LuCI Splash

Auf der Shell
^^^^^^^^^^^^^

Um diese allgemeinen Einstellungen auf der Shell vorzunehmen kann entweder
:file:`/etc/config/luci_splash` direkt bearbeitet werden::

   config core 'general'
   	option leasetime '1'
   	option limit_up '20'
   	option limit_down '50'
   	option redirect_url 'http://www.freifunk.net'

oder die selben Einstellungen mit :command:`uci` gemacht werden::

  uci set luci_splash.general.leasetime=1
  uci set luci_splash.general.limit_up=20
  uci set luci_splash.general.limit_down=50
  uci set luci_splash.general.redirect_url='http://www.freifunk.net'
  uci commit luci_splash

Anchliessend muss Splash mit::

  /etc/init.d/luci_splash

neu gestartet werden damit die Änderungen wirksam werden.


.. _splash-interfaces:

Interfaces zum Splash hinzufügen
--------------------------------

Um ein Interface zum Splash hinzuzufügen (damit also Clients über
dieses Interface den Splash akzeptieren müssen), muss der Name des
Netzwerks sowie die Firewallzone zu der das Netzwerk gehört bekannt
sein.

Interface unter LuCI hinzufügen
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Öffne :menuselection:`Administration --> Dienste --> Client-Splash`. In
der Interface-Sektion sieht man bereits Konfigurierte Schnittstellen:

.. image:: /images/luci/splash/splash-interfaces.png
   :alt: Interface Einstellungen von LuCI Splash

Durch einen Klick auf :guilabel:`Hinzufügen` können weitere Schnittstellen
hinzugefügt werden.

Interface auf der Shell hinzufügen
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Interfaces die Splash benutzen soll werden konfigguriert in
:file:`/etc/config/luci_splash`. Um ein neues Interface zum Splash
hinzuzufügen kann dort direkt am Ende eine neue interface-Sektion
eingefügt werden::

  config iface 'wireless0custom'
  	option network 'wireless0custom'
  	option zone 'freifunk'

Alternativ kann dieser Eintrag auch mit :command:`uci` erstellt werden::

  uci set luci_splash.wireless0custom=iface
  uci set luci_splash.wireless0custom.network=wireless0custom
  uci set luci_splash.wireless0custom.zone=freifunk
  uci commit luci_splash

In beiden Fällen muss anschliessend der Splash mit
:command:`/etc/init.d/luci_splash restart` neu gestartet
werden, damit die Änderungen wirksam werden.


.. _whitelist:

Whitelist - Clients dauerhaft erlauben
--------------------------------------

Clients deren :term:`MAC`-Adresse auf der Whitelist steht werden dauerhaft
freigeschaltet, d.h. sie müssen nicht den Splash akzeptieren bevor sie
ins Internet dürfen. Ausserdem unterliegen sie nicht dem Bandbreitenlimit
für normale Clients falls die Bandbreite für diese limitiert wird (siehe
:ref:`splash-general-settings`).

Um Clients zur Whitelist hinzuzufügen wird deren :term:`MAC`-Adresse benötigt.
Clients die verbunden sind können auch anhand ihrer IP-Adresse zur Whitelist
hinzugefügt werden.

Clients whitelisten in LuCI
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Clients können auf der Statusseite von Splash (siehe
:ref:`splash-status-luci` gewhitelistet werden.

Alternativ ist dies auch möglich über die Splash Einstellungen unter
:menuselection:`Administration --> Dienste --> Client-Splash`.

.. image:: /images/luci/splash/splash-whitelist.png
   :alt: Clients whitelisten in LuCI


Clients whitelisten auf der Shell
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Um Clients auf der Shell auf die Whitelist zu setzen::

  luci-splash whitelist 00:11:22:33:44:55

Ist der Client verbunden und die IP-Adresse bekannt (siehe
:ref:`splash-status-shell`) dann kann der Client auch anhand der IP
freigeschalten werden::

  luci-splash whitelist 1.2.3.4


.. _blacklist:

Blacklist - Clients dauerhaft sperren
-------------------------------------

Clients deren :term:`MAC`-Adresse auf der Blacklist steht werden dauerhaft
gesperrt, d.h. sie können den Splash nicht mehr akzeptieren und bekommen
stattdessen eine Hinweisseite, dass sie geblockt wurden.

Um Clients zur Blacklist hinzuzufügen wird deren :term:`MAC`-Adresse benötigt.
Clients die verbunden sind können auch anhand ihrer IP-Adresse zur Blacklist
hinzugefügt werden.

Clients blacklisten in LuCI
^^^^^^^^^^^^^^^^^^^^^^^^^^^

Clients können auf der Statusseite von Splash (siehe
:ref:`splash-status-luci` geblacklistet werden.

Alternativ ist dies auch möglich über die Splash Einstellungen unter
:menuselection:`Administration --> Dienste --> Client-Splash`.

.. image:: /images/luci/splash/splash-blacklist.png
   :alt: Clients blacklisten in LuCI


Clients blacklisten auf der Shell
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Um Clients auf der Shell auf die Blacklist zu setzen::

  luci-splash blacklist 00:11:22:33:44:55

Ist der Client verbunden und die IP-Adresse bekannt (siehe
:ref:`splash-status-shell`) dann kann der Client auch anhand der IP
gesperrt werden::

  luci-splash blacklist 1.2.3.4

Bestimmte Ziele allgemein erlauben
----------------------------------

Einzelne Zielrechner bzw -netzwerke können generell erlaubt werden. Verbindungen
zu diesen sind immer möglich, es muss nicht zuerst der Splash erlaubt werden.

.. hint::

   Zugriff auf die im Communityprofil definierte **Community-Homepage** (siehe
   :ref:`community_profiles`)  wird automatisch erlaubt. Der Server, der diese
   Webseite bereitstellt muss hier also nicht extra eingetragen werden.

In LuCI
^^^^^^^

Gehe zu :menuselection:`Administration --> Dienste --> Client-Splash`. Dort kannst
du unter :guilabel:`Erlaubte Rechner/Netzwerke` durch einen Klick auf :guilabel:`Hinzufügen`
Ziel-Netzwerke hinzufügen, die vom Splashvorgang ausgenommen sein sollen.

.. image:: /images/luci/splash/splash-allowed-networks.png
   :alt: Zielrechner und -netzwerke generell erlauben in LuCI

Soll nur ein einzelnen Zielrechner erlaubt werden, dann reicht es, dessen IP-Adresse
anzugeben. Um ein ganzes Ziel-Netzwerk zu erlauben ist zusätzlich die Angabe einer
:term:`Netzmaske` in der dotted decimal Schreibweise (z.B. 255.255.255.0) notwendig.

Speichere deine Änderungen mit :guilabel:`Speichern & Anwenden`.

Auf der Shell
^^^^^^^^^^^^^

Die Einstellungen sind in der Datei :file:`/etc/config/luci_splash` gespeichert.
Um ein weiteres Ziel-Netzwerk, wir wollen es hier ``allowednet`` nennen, zu erlauben,
füge in diese Datei eine neue Sektion ein::

  config subnet 'allowednet'
  	option ipaddr '1.2.3.4'
  	option netmask '255.255.255.255'

Alternativ geht das auch mit :command:`uci`::

  uci set luci_splash.allowednet=subnet
  uci set luci_splash.allowednet.ipaddr=1.2.3.4
  uci set luci_splash.allowednet.netmask=255.255.255.255

In beiden Fällen muss anschliessend der Splash mit
:command:`/etc/init.d/luci_splash restart` neu gestartet
werden, damit die Änderungen wirksam werden.


Splash-Seite individualisieren
------------------------------

Es ist möglich, die Splash-Seite, die Benutzern angezeigt wird nach eigenen
Vorstellungen anzupassen. So können z.B. ein prominenterer Hinweis auf den Betreiber
oder Sponsor des Freifunkknotens eingefügt oder lokalitätsbezogene Informationen
angezeigt werden.

Es kann entweder die komplette Splash-Seite ersetzt werden oder nur eigener
Text in die Standardseite eingefügt werden.

Die Texte sollten in gültigem HTML geschrieben sein. Es können einige Marker
verwendet werden, die bei der Ausgabe ersetzt werden:

.. tabularcolumns:: |p{5cm}|p{10cm}|

.. list-table::
   :widths: 25 75
   :header-rows: 1

   * - Marker
     - Beschreibung
   * - ###COMMUNITY###
     - Name der Community
   * - ###COMMUNITY_URL###
     - URL zur Webseite der Community
   * - ###CONTACTURL###
     - URL zur lokalen Seite mit Kontaktinformationen
   * - ###LEASETIME###
     - Freigabezeit
   * - ###LIMIT###
     - Hinweis auf UP- und Downloadlimitierung
   * - ###ACCEPT###
     - Einbinden der :guilabel:`Akzeptieren` und :guilabel:`Ablehnen` Buttons

Splash Seite anpassen in LuCI
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Die Splash-Seite kann im LuCI-Webinterface unter
:menuselection:`Administration --> Dienste --> Client-Splash --> Splash-Text`
angepasst werden.

Soll der **komplette Text des Splash** angepasst werden dann gib ihn im oberen
Textfeld :guilabel:`Bearbeiten des kompletten Splash-Textes` ein. Es ist wichtig,
den Marker ###ACCEPT### einzufügen damit die Buttons angezeigt werden können.

Willst du **eigenen Text zum Splash hinzufügen** dann benutze das untere
Textfeld :guilabel:`Einbinden von eigenem Text in die Default-Splashseite`.

Splash Seite anpassen auf der Shell
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Um den **kompletten Text des Splash** zu ersetzen bearbeite die Datei
:file:`/usr/lib/luci-splash/splashtext.html` und füge dort deinen eigenen Text
(gültiges HTML) ein.

Um nur **eigenen Text zusätzlich zum Standardtext** des Splashs anzuzeigen bearbeite
:file:`/usr/lib/luci-splash/splashtextinclude.html` und füge dort deinen
eigenen HTML-Text ein.

Splash dauerhaft deaktivieren
-----------------------------

Auf der von Meshkit installierten Firmware wird Splash üblicherweise automatisch
installiert und eingerichtet. Will man den Splash dauerhaft deaktivieren
geht das mit::

  /etc/init.d/luci-splash stop
  /etc/init.d/luci-splash disable


