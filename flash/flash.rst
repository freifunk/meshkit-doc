.. include:: /links.txt

Router auf dem bereits OpenWrt installiert ist
==============================================

Über LuCI
---------

Über das Webinterface kann einfach eine neue Firmware installiert werden.
<TODO: Screenshots, Erklärung>

Sysupgrade
----------

Ist bereits Openwrt installiert, dann kann eine neue Firmware sehr einfach
über SSH installiert werden. Auf der Shell des Routers gibt man dafür ein:

.. code-block:: sh

  cd /tmp
  wget <link zum image>
  sysupgrade -n <heruntergeladenes image>

Dieses Kommando sichert durch den "-n" Parameter keine Konfiguration. 
