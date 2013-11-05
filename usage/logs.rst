.. include:: /links.txt

.. _logs:

Logs anzeigen
=============

Auf dem Router gibt es zwei wichtige Logs, die wichtige Informationen
zum System geben können: ``System Log`` und ``Boot log``. Die Logs sind
insbesondere bei der Fehlersuche oft hilfreich.

.. _show-syslog:

System Log
----------

Das System Log enthält alle Ereignisse, deren Priorität mindestens so
hoch ist wie der eingestellte ``LogLevel``. Das System Log wird in einen
``Ring Buffer`` geschrieben, d.h. es steht nur eine begrenzte Anzahl an
Speicherplatz fürs Log zur Verfügung. Wird dieser überschritten, dann werden
die ältesten Log-Meldungen verworfen um Platz für neue zu schaffen.

Im **LuCI Webinterface** befindet sich das System Log unter
:menuselection:`Administration --> Status --> Systemprotokoll`.

Auf der Shell kann das System Log mit dem Kommando :command:`logread`
angezeigt werden.

.. _show-kernlog:

Kernel Log (Dmesg)
------------------

Das Log des Kernels kann in LuCI unter
:menuselection:`Administration --> Status --> Kernelprotokoll` angezeigt
werden.

Auf der Shell kann das Kernel-Log mit dem Kommando :command:`dmesg`
angezeigt werden.


.. log-configure:

Logging konfigurieren
=====================

Die Defaulteinstellungen für das Loggen von Ereignissen sind relativ
gut und müssen nicht angepasst werden. Dennoch ist es möglich die
Konfiguration der Loggingmechanismen an besondere Bedürfnisse
anzupassen.

.. _log-settings:

Folgende Einstellungen können gemacht werden:

.. tabularcolumns:: |p{4cm}|p{3cm}|p{8cm}|

.. list-table::
   :widths: 30 20 50
   :header-rows: 1

   * - Option LuCI
     - Option Shell
     - Beschreibung
   * - Größe des Systemprotokoll-Puffers
     - buffersize
     - Größe des ``Ring Buffer`` in kByte, in dem das Log gespeichert wird.
   * - Externer Protokollserver IP
     - log_ip
     - Optional: IP-Adresse eines externen Syslog-Servers, zu dem die Logeinträge
       geschickt werden
   * - Externer Protokollserver Port
     - log_port
     - Optional: Port des externen Syslog-Servers
   * - Protokolllevel
     - conloglevel
     - Logmeldungen müssen mindestens diese Priorität haben, um
       geloggt zu werden. Siehe :ref:`loglevel-syslog`.
   * - Cron Protokolllevel
     - cronloglevel
     - Cronmeldungen müssen mindestens diese Priorität haben, um geloggt zu
       werden. Siehe :ref:`loglevel-cron`.

Mögliche Loglevel
-----------------

Nachrichten werden nur geloggt, wenn deren Priorität mindestens dem
konfigurierten Level entspricht.

.. _loglevel-syslog:

Loglevel für Syslog-Meldungen
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Gibt an, ab welchem Level Meldungen von :term:`cron` ins Syslog
geschrieben werden.

.. tabularcolumns:: |p{3cm}|p{3cm}|p{2cm}|p{7cm}|

.. list-table::
   :widths: 15 25 10 50
   :header-rows: 1

   * - Level
     - in LuCI
     - numerisch
     - Beschreibung
   * - LOG_EMERG
     - Notfall
     - 1
     - Das System ist unbenutzbar
   * - LOG_ALERT
     - Alarm
     - 2
     - Es ist unbedingt ein sofortiges Eingreifen notwendig
   * - LOG_CRIT
     - Kritisch
     - 3
     - Kritische Warnungen
   * - LOG_ERR
     - Fehler
     - 4
     - Ein Fehler ist aufgetreten
   * - LOG_WARNING 
     - Warnung
     - 5
     - Eine Warnung
   * - LOG_NOTICE
     - Notiz
     - 6
     - Wichtige Hinweise anzeigen
   * - LOG_INFO
     - Info
     - 7
     - rein informelle Ausgaben
   * - LOG_DEBUG
     - DEBUG
     - 8
     - ALLE Meldungen werden ausgegeben

.. _loglevel-cron:

Loglevel für Cron-Meldungen
^^^^^^^^^^^^^^^^^^^^^^^^^^^


.. tabularcolumns:: |p{4cm}|p{3cm}|p{8cm}|

.. list-table::
   :widths: 30 10 60
   :header-rows: 1

   * - in LuCI
     - numerisch
     - Beschreibung
   * - Warnung
     - 9
     - Nur Fehler von Cron werden gelogged
   * - Normal
     - 8
     - Normales Logging von Cron-Programmaufrufen
   * - Debug
     - 5
     - Debug-Meldungen ausgeben


Logging konfigurieren in LuCI
-----------------------------

Gehe zu :menuselection:`Administration --> System --> System` und wechsle
im Abschnitt :guilabel:`Systemeigenschaften` zum Tab :guilabel:`Logging`.

.. image:: /images/luci/log-settings.png
   :alt: Log Einstellungen in LuCI

Zur Konfiguration der einzelnen Optionen siehe
:ref:`Einstellungen fürs Syslog <log-settings>`.

.. hint::

   Diese Einstellungen werden erst nach einem Neustart des Systems aktiv.

Logging konfigurieren auf der Shell
-----------------------------------

Die Konfiguration fürs die Logs befindet sich in :file:`/etc/config/system`
in der Sektion ``system``. Sie kann dort direkt bearbeitet werden. Alternativ
können einzelne Optionen auch mit :command:`uci` verändert werden::

  uci set system.system.buffersize=16
  uci set system.system.cronloglevel=9
  uci set system.system.conloglevel=8
  uci set system.system.log_ip='1.2.3.4'
  uci set system.system.log_port=514
  uci commit system

Zur Konfiguration der einzelnen Optionen siehe
:ref:`Einstellungen fürs Syslog <log-settings>`.


.. hint::

   Diese Einstellungen werden erst nach einem Neustart des Systems aktiv.
