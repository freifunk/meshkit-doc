.. _scp:

Dateien vom/zum Router kopieren
===============================

Setzt voraus: :ref:`connect`

Will man Dateien vom/zum Router kopieren, dann ist dies mit SCP
möglich, das Dateien über SSH bzw. das SCP Protokoll kopiert.

Von Linux aus
-------------

Mit scp
^^^^^^^

Das :command:`scp`-Kommand ist auf den meisten Distributionen bereits
installiert.

Um eine **Datei zum Router zu kopieren** gibt man in der Kommandozeile des
Linux-Systems folgendes ein:

.. code-block:: sh

   scp <Datei> root@<ip-des-routers>:/tmp

Dadurch wird die Datei nun, nachdem man sofern benötigt noch das Passwort eingegeben hat,
in den :file:`/tmp`-ordner des Routers kopiert. Will man ganze Verzeichnisse
rekursiv kopieren dann benutzt man für scp den "-r" Parameter, also z.B.

.. code-block:: sh

   scp -r <Verzeichnis> root@<ip-des-routers>:/tmp

Um eine Datei vom Router auf den eigenen Linux-Rechner zu kopieren werden
einfach die Parameter umgedreht. Um zum Beispiel die Datei
:file:`/etc/config/wireless` zu sichern kann folgendes KOmmando verwendet
werden:

.. code-block:: sh

   scp root@<ip-des-routers>:/etc/config/wireless .

Dies kopiert die Datei in den aktuellen Ordner. Sollen ganze Verzeichnisse
gesichert werden dann ebenfalls wieder den "-r"-Parameter verwenden.

.. hint::

   Diese Methode kann auch verwendet werden, um Dateien von einem Freifunkrouter
   zu einem anderen zu kopieren.


Mit fish
^^^^^^^^

In vielen Dateimanagern kann auf entfernte Dateisysteme zugegriffen werden,
indem man in der Adresszeile die Adresse **fish://<ip-des-routers>** eingibt.

Von Windows aus
---------------

Unter Windows kann `WinSCP <http://winscp.net/eng/docs/lang:de>`_ verwendet
werden, um Dateien vom/zum Freifunkrouter zu kopieren.
