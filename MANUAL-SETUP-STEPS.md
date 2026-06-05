# Samsung Root Extended - Manual Setup Reference

Release-Stand: v1.3.0 / Manager 1.3.0

Diese Datei dokumentiert die Schritte, die bisher nicht vollstaendig durch die Module selbst erledigt werden konnten.
Sie dient als Vorlage fuer die spaetere Automationslogik im Manager.

## 1. Nach frischem Flash vorbereiten

Diese Schritte sind system-/UI-abhaengig und koennen nicht zuverlaessig von einem Magisk- oder LSPosed-Modul erledigt werden:

```powershell
adb devices
```

Manuell am Geraet:

- Android-Ersteinrichtung minimal abschliessen.
- Entwickleroptionen aktivieren.
- USB-Debugging aktivieren.
- USB-Debugging-RSA-Abfrage bestaetigen.
- Magisk-App oeffnen und eventuell geforderte Zusatzinstallation/Update abschliessen.
- Zygisk in Magisk aktivieren oder kompatibles Standalone-Zygisk nutzen.

Danach ist ein Reboot sinnvoll.

## 2. Root und Magisk pruefen

```powershell
adb devices
adb shell su -c id
adb shell su -c magisk -v
```

Erwartung:

```text
uid=0(root)
```

Wenn `su` nicht funktioniert, muss Magisk zuerst manuell fertig eingerichtet werden.

## 3. LSPosed installieren

LSPosed ist eine externe Voraussetzung und gehoert nicht zu den eigenen vier Samsung-Root-Extended-Modulen.

```powershell
adb push "LSPosed-v1.11.0-7209-zygisk-release.zip" /sdcard/Download/
adb shell su -c magisk --install-module /sdcard/Download/LSPosed-v1.11.0-7209-zygisk-release.zip
```

Danach rebooten.

```powershell
adb install -r "LSPosedManager.apk"
adb shell pm path org.lsposed.manager
adb shell su -c "ps -A | grep lspd"
```

## 4. Eigene Magisk-Module installieren

Main Profile Spoofer:

```powershell
adb push "Samsung-Root-Extended-Main-Profile-Spoofer.zip" /sdcard/Download/
adb shell su -c magisk --install-module /sdcard/Download/Samsung-Root-Extended-Main-Profile-Spoofer.zip
```

Work Profile Bypass:

```powershell
adb push "Samsung-Root-Extended-Work-Profile-Bypass.zip" /sdcard/Download/
adb shell su -c magisk --install-module /sdcard/Download/Samsung-Root-Extended-Work-Profile-Bypass.zip
```

Danach rebooten.

Pruefen:

```powershell
adb shell su -c "ls /data/adb/modules"
```

Erwartete eigene Magisk-Module:

```text
main_profile_spoofer_generic
work_profile_bypass
```

## 5. Eigene LSPosed-APK-Module installieren

```powershell
adb install -r "Samsung-Root-Extended-Work-Profile-Spoofer.apk"
adb install -r "Samsung-Root-Extended-Work-Profile-Isolation.apk"
```

Pruefen:

```powershell
adb shell pm path local.niklas26903.workprofilespoofer
adb shell pm path local.niklas26903.workprofileisolation
```

## 6. Arbeitsprofil erzeugen, falls es fehlt

Dieser Schritt kann je nach ROM/Framework fehlschlagen. Der Bypass muss vorher aktiv sein.

```powershell
adb shell pm list users
adb shell pm create-user --profileOf 0 --managed --for-testing NiklasWork
adb shell pm list users
```

Den erzeugten Work-Profile-User merken, z. B. `10`.

## 7. Profiltrennung per Settings haerten

Beispiel mit Work-Profile-User `10`:

```powershell
adb shell su -c "settings put secure --user 0 managed_profile_contact_remote_search 0"
adb shell su -c "settings put secure --user 10 managed_profile_contact_remote_search 0"
adb shell su -c "settings put secure --user 0 cross_profile_calendar_enabled 0"
adb shell su -c "settings put secure --user 10 cross_profile_calendar_enabled 0"
```

Pruefen:

```powershell
adb shell su -c "settings get secure --user 0 managed_profile_contact_remote_search"
adb shell su -c "settings get secure --user 10 managed_profile_contact_remote_search"
adb shell su -c "settings get secure --user 0 cross_profile_calendar_enabled"
adb shell su -c "settings get secure --user 10 cross_profile_calendar_enabled"
```

Alle Werte sollen `0` sein.

## 8. Spoof-Konfigurationen schreiben

Diese Schritte kann der Manager bereits direkt ueber die UI erledigen.

Main Profile Config:

```text
/data/adb/profile-spoofer/main-profile.conf
```

Work Profile Config:

```text
/sdcard/ProfileSpoofer/work-profile.properties
```

Nach Aenderung der Profile ist ein Reboot sinnvoll.

## 9. LSPosed-Scopes setzen

Dieser Schritt ist bisher der wichtigste manuelle Restpunkt, falls der Manager die LSPosed-Konfiguration noch nicht automatisch schreiben soll.

Module:

- `local.niklas26903.workprofilespoofer`
- `local.niklas26903.workprofileisolation`

Mindest-Scopes:

```text
com.google.android.gms
com.google.android.gsf
com.google.android.gsf.login
com.android.vending
com.google.android.googlequicksearchbox
```

Optionale Scopes:

```text
com.android.chrome
com.google.android.apps.maps
com.google.android.gm
com.google.android.apps.docs
com.google.android.youtube
```

Danach rebooten oder betroffene Apps hart stoppen.

## 10. Endpruefung

```powershell
adb shell su -c "ls /data/adb/modules"
adb shell pm path local.niklas26903.workprofilespoofer
adb shell pm path local.niklas26903.workprofileisolation
adb shell pm path org.lsposed.manager
adb shell su -c "ps -A | grep lspd"
adb shell pm list users
adb shell settings get global device_name
adb shell getprop ro.product.model
```

Erwartet:

- Root funktioniert.
- Magisk funktioniert.
- Zygisk oder kompatibles Standalone-Zygisk ist aktiv.
- LSPosed/lspd laeuft.
- `main_profile_spoofer_generic` ist installiert.
- `work_profile_bypass` ist installiert.
- `local.niklas26903.workprofilespoofer` ist installiert.
- `local.niklas26903.workprofileisolation` ist installiert.
- Arbeitsprofil existiert.
- Profiltrennungs-Settings sind gesetzt.

