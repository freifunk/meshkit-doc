.. _community_profiles:

Community Profile
=================

Jede Community, für die Meshkit Firmwareimages bauen soll, hat ein Community-
Profil, in dem einige Standardwerte (wie z.B. Kanal, ESSIDs usw.) stehen.

Diese Vorgaben werden entweder nur im Hintergrund benutzt oder, falls sie
durch den Benutzer veränderbar sein sollen, als Standardwerte in Meshkit
verwendet.

Für eine Erklärung der einzelnen Optionen siehe:
http://wiki.freifunk.net/Kamikaze/Profile

Alle Communityprofile befinden sich im
`LuCI Repository <https://github.com/openwrt/luci/tree/master/contrib/package/community-profiles/files/etc/config>`_


.. _community_profile_create:

Profil für eine neue Community erstellen
----------------------------------------

Um ein Profil für eine neue Community zu erstellen, schaue dir zunächst
die existierenden Communityprofile an. Kopiere dann eins der Profile
und passe es für die neue Community an. Danach erstelle ein Ticket oder einen
Pull Request auf https://github.com/openwrt/luci/issues, welcher das neue Profil
hinzufügt.
