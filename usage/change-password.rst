Passwort ändern
===============

Setzt voraus: :ref:`connect`

Im Webinterface
---------------

Das Passwort kann unter :menuselection:`Administration --> System --> Administration`
geändert werden.

.. image:: /images/luci/change-password.jpg

Mit SSH
-------

Auf dem Shell des Routers :command:`passwd` eingeben. Dann das neue Passwort 
eingeben und noch einmal bestätigen.

.. code-block:: sh

   root@freifunk:~# passwd
   Changing password for root
   New password: 
   Retype password: 
   Password for root changed by root
   root@freifunk:~#

