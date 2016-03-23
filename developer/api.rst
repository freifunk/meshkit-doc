API
===

Meshkit hat eine API, mit der sich einige Statusinformationen abfragen lassen.
Ausserdem können Firmwareimages mit der API erstellt werden.

Alle API-URLs sind erreichbar unter http://<url>/api/json/<api>

status
------
Gibt den aktuellen Status von Meshkit aus, z.B.

.. code-block:: json

  {"status": true, "queuedimg": 0, "memfree": "494", "totalimg": 585, "successimg": 505, "failedimg": 80, "loadavg": "2.06, 1.66, 1.13", "memused": "508"}

targets
-------

Liste verfügbarer Targets, z.B.:

.. code-block:: json

  ["ar71xx-generic-attitude_adjustment-35817", "atheros-attitude_adjustment-35864", "brcm47xx-attitude_adjustment-35817", "brcm63xx-attitude_adjustment-35864", "x86-generic-attitude_adjustment-35864", "x86-kvm_guest-attitude_adjustment-35864"]

buildstatus
-----------

Informationen zu einem speziellen Build anfragen. Dies benötigt zwingend eine id
und passenden Random String (rand).

Beispiel-Aufruf: http://<url>/api/json/buildstatus?id=1234&rand=36a419d3bd7ad460d16b4773cc3b5d5a

Antwort 

.. code-block:: json

  {
    "status": 0,
    "files": [
        "openwrt-ar71xx-generic-tl-wr1043nd-v1-squashfs-sysupgrade.bin",
        "md5sums",
        "openwrt-ar71xx-generic-vmlinux.elf",
        "openwrt-ar71xx-generic-vmlinux.bin",
        "openwrt-ar71xx-generic-vmlinux.gz",
        "openwrt-ar71xx-generic-root.squashfs-64k",
        "build.log",
        "openwrt-ar71xx-generic-vmlinux-lzma.elf",
        "openwrt-ar71xx-generic-root.squashfs",
        "openwrt-ar71xx-generic-tl-wr1043nd-v1-squashfs-factory.bin",
        "openwrt-ar71xx-generic-uImage-lzma.bin",
        "openwrt-ar71xx-generic-vmlinux.lzma",
        "openwrt-ar71xx-generic-uImage-gzip.bin",
        "openwrt-ar71xx-generic-rootfs.tar.gz"
    ],
    "queued": 0,
    "downloaddir": "http://meshkit.freifunk.net/images//36a419d3bd7ad460d16b4773cc3b5d5a/bin/"
  }

Definierte Werte für status: 0: Sucessful 1: Queued, 2: Failed


buildimage
----------

Einen neuen Auftrag zum erstellen eines Firmwareimages erstellen.

Dies benötigt zwingend die Angabe eines targets. 

Beispiel-Aufruf: http://<url>/api/json/buildimage?target=ar71xx-generic-attitude_adjustment-35817&profile=TLWR1043&community=augsburg&noconf=1

Antwort:

.. code-block:: json

  {
    "rand": "af851246c99b82ac8b3cf3fbe30ff100",
    "errors": {},
    "id": 586
  }

Im Fehlerfall, z.B. bei Angabe ungültiger Variablen, wird ein Dictionary mit
Fehlermeldungen zurückgegeben.






