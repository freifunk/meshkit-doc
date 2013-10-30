Kontakt
=======

Kontaktdaten können entweder im LuCI-Webinterface oder auf der Shell
geändert werden.

.. hint::

   Es ist empfehlenswert zumindest einen Nickname und eine gültige
   Emailadresse anzugeben, damit interessierte Nutzer oder andere
   Betreiber von Freifunkknoten Kontakt aufnehmen können.

LuCI
----

Zu finden unter :menuselection:`Administration --> Freifunk --> Kontakt`

Hier können Kontaktdaten wie ein **Pseudonym** (dein Nickname), **Name**, **Telefonnummer**,
**Email Adresse** und **Homepage** angegeben werden, die dann im öffentlichen Teil des
Webinterfaces unter :menuselection:`Freifunk --> Kontakt` angezeigt werden. Zudem
kann eine kurze Notiz hinterlegt werden.

.. image:: /images/luci/freifunk-contact.png

Auf der Shell
-------------

Entweder durch direktes Bearbeiten von :file:`/etc/config/freifunk`:


.. code-block:: sh

   config public 'contact'
           option phone '123456789'
           option note 'Vermasch mich noch heute! Dein Freifunk'
           option nickname 'freifunker'
           option name 'Hase'
           list homepage 'http://www.freifunk.net'
           option mail 'hase@example.org'

oder über :command:`uci` Kommandos:

.. code-block:: sh

   uci set freifunk.contact.phone=123456789
   uci set freifunk.contact.note=Vermasch mich noch heute! Dein Freifunk
   uci set freifunk.contact.nickname=freifunker
   uci set freifunk.contact.name=Hase
   uci set freifunk.contact.homepage=http://www.freifunk.net
   uci set freifunk.contact.mail=hase@example.org
   uci commit freifunk


