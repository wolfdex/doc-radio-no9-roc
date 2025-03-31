# Radio - Sendetools

Ein Überblick über Audio Tools und Hinweise zu deren Einrichtung, wie diese u.a. bei Radio No9 genutzt werden.
Bei Radio No9 wird überwiegend auf Linux und somit bevorzugt OpenSource gesetzt, 

Allgemein ist es Empfohlen, das Setup soweit es nicht anders erforderlich ist möglichst simpel zu halten.

Dieses Dokument soll erst einen allgemeinen Überblick schaffen, Konfigurationssbeispiele folgen zum Schluss.

-> Audio - Tools

ein Überblick zu Audio - Software unter Linux

-> Audio unter Linux - pulseaudio, pipewire, jack? 

Grundlagen zu Audio unter Linux

-> Audio - Setup(s) unter Linux

spezifische Setup(s) und skripte zur Umsetzung


---

## Audio - Tools

Ein Überblick zu Audioanwendungen

### Noisereduction

#### Cadmus
als eigenständige Anwendung bzw. AppImage

Bedienung relativ simpel, startet als Icon in Taskleiste. Nach Auswahl des Mic, steht Cadmus als virtuelles Mic in Anwendungen bereit.

Alles automatisch, auch keine weiteren Einstellungen möglich.

##### pipewire

 Es wird eine Kette von mehreren virtuellen Audio-Device erzeugt, welche dem normalen Anwender verborgen bleiben.

HW-MIC -> loopback -> Cadmus RAW Mic Redirect -> Cadmus Mic Sink -> Cadmus Denoised Mic
 
das von Cadmus erzeugte loopback dient als eingang für die Audio-Quelle (HW Mic)
das "Cadmus Denoised Mic" dient als Mic im gewünschten Programm

https://github.com/josh-richardson/cadmus

https://www.linux-magazin.de/news/cadmus-unterdrueckt-hintergrundgeraeusche-in-videokonferenzen/


#### NoiseTorch

Eigenständige Anwendung, welche ein virtuelles Mic erzeugt, basiert auf RNNoise.

Bedienung nicht ganz übersichtlich

##### pipewire

die Anwendung ist als einzelnes Device in Pipewire sichtbar.

https://github.com/lawl/NoiseTorch

#### RNNoise
alsa-rnnoise, rnnoise

als VST-Plugin in Anwendungen bzw. VST-Hosts wie Carla nutzbar

Parameter können an Bedarf angepasst 

agiert selbst nicht als virtuelles Mic und somit nicht als Mic-Quelle nutzbar, benötigt zusätzliche virtuelle Quelle (source/virtual) als nutzbares virtuelles Mic

##### pipewire

HW-Mic -> RNNoise ->  virtuelles Mic


#### EasyEffects

Kommt einem vereinfachten LV2/VST-Host sehr nahe. Kann einiges mehr als nur Noisereduction.
Durch Plugins wie Equalizer, Kompressor, Reverb, … .

Filter für jeweils eine Stereo-Ausgabe und Stereo-Eingabe Kette möglich.

Somit bereits umfangreiche Eingriffe in die Audiokette möglich ohne komplexere Tools verwenden zu müssen.

Leider keine eigenen Plugins frei integrierbar. Bietet für Ein und Ausgabe, jeweils nur ein Stereokanal.

Komplexere Audiosetups mit mehreren Quellen bzw. Ausgabegeräten erfordern zusätzliche Konfigurationen mit pipewire.


##### Erweiterungen
easyeffects-record
easyeffects-bundy01-presets
polybar-easyeffects-presets 

##### pipewire

erzeugt EasyEffectsSource (Mic) und EasyEffectsSink (Lautsprecher)

Hängt ausgewählte Filter innerhalb in die Kette ein, welche als solches auch in pipewire ersichtlich sind

Filter lassen sich grob innerhalb EasyEffects einstellen, jedoch nicht in Carla

Eingabegerät (Mic) -> loopdevice -> Filter … > EasyEffectsSourcer -> Ziel-Anwendung

Quell-Anwendung -> EasyEffectsSink-> Filter … >   loopdevice -> Ausgabegerät (Lautsprecher)

Mehrere Mics:

Eingabegerät (Mic 1) ->  | ->  Mixer   ->    loopdevice -> Filter … > EasyEffectsSourcer -> Ziel-Anwendung
Eingabegerät (Mic 2) ->  |  

### LV2/VST - Host
 
Filter und Anwendungen für Audio kommen oft in Formaten wie VST,  LV2 und ähnlichen verbreitet werden. Üblicherweise werden diese als Plugins in komplexen Audioanwendungen (DAW) zur Musikproduktion genutzt und laufen nicht eigenständig. LV2 Hosts bzw. VST Hosts sind Anwendungen, welche diese Plugins aufnehmen und ausführen können, ohne zusätzlich eine große Audiosoftware verwenden zu müssen.
 
 Unter Linux gibt es bereits eine Vielfältige Auswahl an VST und LV2, die nativ laufen und über Paketmanager installiert werden können. Über eine VST-Wine-Bridge lassen sich auch VST für Windows nutzen, soweit diese kein DRM-Schutz haben. 

Mit den PlugIns sollte man es nicht übertreiben. Jedes benötigt zusätzliche Rechenleistung und erhöht die Latenz.


#### Carla

Carla ist ein Universalwerkzeug.

Mit diesem kann man das wireing von pipewire einsehen und ändern.

Zusätzlich dient Carla als Host für VST/LV2/DSSI
Über eine Wine-Bridge können sogar Windows VSTs (ohne DRM) genutzt werden

Virtuelle Racks und Setups lassen sich jederzeit nach Bedarf speichern und laden.

### pipewire - wireing

#### qpwgraph

Ein relativ simples Werkzeug zur Darstellung und zum Ändern von pipewire Verbindungen.

Konfigurationen lassen sich speichern und laden.

Kann selbst nicht als Host für Plugins dienen.


### Mischpult (Mixer)

#### Jack Mixer (jack_mixer)

als eigenständige Anwendung auszuführen und in Audiokette einzubinden. Nicht als Plugin verfügbar.

Es lassen sich nahezu beliebig viele Ein und Ausgabekanäle konfigurieren und als fertige Konfiguration speichern bzw. laden.


#### LSP Mixer x4/x8/x16

gibt es als Stereo und Mono Variante

Mixer als LV2 Plugin mehrere Eingänge auf einen Ausgang bringen

sehr praktisch in Carla


### Software zum Streamen

#### Mixxx - DJ Software
https://mixxx.org/

#### idjc
https://idjc.sourceforge.io/

#### OBS Studio
https://obsproject.com/de/download

### Audioformate

Einen guten praxisnahen Überblick zu Audioformaten bringt das folgende Video

38C3 - Everyone VS. MP3 - Audio Datei-Formate für DJs und co.
https://www.youtube.com/watch?v=bkz7zJqrJXo

Für das meiste tut es MP3 zum Senden und Auflegen. Damit kommen alle Hardware und Softwarelösungen klar.
Archiviert sollte im Original-Format bzw. Verlustfreie Formate wie AIFF oder FLAC genutzt werden.
Hat man ein Verlustbehaftetes in ein Verlustfreies Format für Archivierungszwecke umgewandelt, so sollte man dies kennzeichnen.
Es wird dadurch zwar nicht schlechter, nur auch nicht besser.

## empfohlene Konfigurationen


### Audiokette allgemein

in EasyEffects oder auch in LV2/VST-Hosts wie Carla

allgemein:
Quelle -> (Mixer) -> Limiter -> De-Esser -> Noisereduction (RNNoise, NoiseTorch) -> Calf Stereo -> virtual_source
-> (Mixer) -> Sendesoftware

variante 1 - Keep-It-Simple:
jede Quelle (Mic) in einen Mixer, von diesem in der Audiokette in Signalverarbeitung

Vorteil, eine einfache Audiokette aufsetzen, die für die meisten Anwendungszwecke passt und alles behandelt
es können unschöne Audioeffekte entstehen, die man eigentlich vermeiden will

  source1 + source2 + source3 -> source_mixer 
  -> limiter -> de-esser -> rnnoise -> calf-stereo -> virtual_source

variante 2 - erweitertes Setup:

für jede Quelle eine eigene Signalkette die jeweils in Mixer führt

Vorteil Eingangssignale lassen sich feiner steuern und abmischen
Ungewollte Effekte beim Mischen lassen sich durch zusätzliche Filter behandeln
erfordert mehr Rechenleistung mit Carla oder DAW in Live-Modus.

  Quelle -> limiter -> de-esser -> rnnoise -> calf-stereo ->  source_mixer -> jack_mixer aggregate_sources() -> virtual_source

Tipp: Vorabsetup

Mittels VirtualSink und VirtualSource lässt sich die Konfiguriation für mehrere Geräte, bereits aufsetzen, ohne dass diese immer present sein müssen,
Beispiel wäre Mobile-Setup, Desktop-Setup. Bei tatsächlichen Bedarf verbindet man die jeweilige Quelle / Ziel mit dem dazu gehörigen VirtualSink  / VirtualSource.


### Kep-It-Simple mit EasyEffects

Easy Effects mit seinen vorbereiteten Effekten übernimmt die Signalkette nach Variante 1.
Es kann nur einen Stereo-In mit dazugehöriger Audiokette und einen Stereo-Out mit dazugehöriger Audiokette verarbeiten. 
Dies sollte für die meisten ausreichen.

EasyEffects lässt sich später in Audio-Anwendungen direkt als Quelle bzw. Ziel auswählen.

Auch ist EasyEffects quasi Magnetisch und zieht bei entsprechender Konfiguration alle Geräte auf sich.
Will man das vermeiden, hängt man noch einen Eingangs-Mixer bzw. Ausgangs-Mixer dazu.

### Kep-It-Not-So-Simple mit LV2/VST-Host (Carla und andere LV2-Hosts)

im Grunde läuft hiert das Setup wie zuvor, nur dass die Signalverarbeitung nicht eine einzelne Anwendung (EasyEffects) die Konfiguration zenral übernimmt.

In Carla werden einzelne LV2/VST - Plugins geladen und manuell mittels pipewire verkettet.

Der Vorteil, man kann frei die Plugins auswählen und meist feiner einstellen und es sieht schöner aus.

Damit man die Signalkette auch in Anwendungen tatsächlich nutzen kann muss man für die Signalkette ein VirtualSink (Ausgabe) / VirtualSource (Eingabe) erstellen und an die entsprechende in der Signalkette hängen. Bei EasyEffects, macht dies das Programm bereits automatisch.

Auch hier ist es Vorteilhaft mittels VirtualSink und VirtualSource die Konfiguriation für alle angedachten Geräte, bereits aufsetzen, ohne dass diese immer present sein müssen. Beispiel wäre Mobile-Setup, Desktop-Setup. Bei tatsächlichen Bedarf verbindet man die jeweilige Quelle / Ziel mit dem dazu gehörigen VirtualSink  / VirtualSource.

Das fertige Carla Setup lässt sich speichern und jederzeit wieder nutzen.



## Audio unter Linux - pulseaudio, pipewire, jack? 

### pulseaudio
pulseaudio wurde durch das bessere pipewire ersetzt
mit **pipewire-pulse** gibt es eine Brücke, damit bisherige pulseaudio - Anwendungen weiterhin funktionieren

### pipewire

**pactl** entstammt zwar pulseaudio, funktioniert in verbindung mit **pipewire-pulse** weiterhin

**pw-cli** übernimmt alternativ die Aufgaben auf der Kommandozeile

#### Devices

Ob virtuell oder echte Hardware. Üblicherweise unterscheiden Audioanwendungen zwischen Geräten mit eingehenden und ausgehenden Signalen.
In Einzelfall ob gewollt oder nicht kann es passieren, dass einzelne Anwendungen keine Unterscheidung machen und alles verfügbare als Quelle und Ziel anzeigen. Daher empfiehlt sich immer auf eine sinnvolle Benennung zu achten.

Audioanwendungen wie Rauschunterdrückung oder auch Mixer sind oft nicht direkt auswählbar.

Mit virtuellen Geräten kann man solchen Lösungen in Anwendungen ein auswählbares Gerät für eine Signalkette bestimmen.

Ein weiterer praktischer Nutzen ist Signalketten für alle möglichen Geräte zu definieren. Will man das Signal eines Headset-Mic immer durch einen Limiter und Rauschunterdrückung laufen lassen. So baut man diese sich bereits mittels virtuellen Devices für das Headset auf, ohne dass es ständig angeschlossen sein muss. Erst, wenn man es tatsächlich benötigt und anschließt verbindet man das echte Hardware-Gerät mit dem dazu gehörigen virtuellen Gerät der Audiokette. Das geht in Pipewire oder mittels skript.


#####  Sink

Ein Sink funktioniert als Ausgabeziel wie ein Lautsprecher. Ist als solches auch in Audio-Anwendungen als Ziel auswählbar

```bash
pactl load-module module-null-sink media.class=Audio/Sink sink_name=OUTSinkStereo channel_map=stereo

```

#### (virtual) Source

Ein Source ist, wie der Name bereits sagt, eine Quelle. Diese funktionieren als Eingangssignal wie ein Mic. Diese sind in Anwendungen auch als Quelle auswählbar.

#### Virtual Source (Mic)
```bash

pactl load-module module-null-sink media.class=Audio/Source/Virtual sink_name=Source-InternalMic node.description="Source_InternalMic"
pactl load-module module-null-sink media.class=Audio/Source sink_name=Source-InternalMic node.description="Source_InternalMic"

```

#### loop device


pactl load-module module-loopback latency_msec=1


Ein Loop-Device kann man als Hilfsdevice zum einfacheren routen ansehen. Einige Anwendungen und Audiotreiber erzeugen solche solche automatisch.
Diese erscheinen üblicherweise weder als Eingans- noch als Ausgangsgerät in Anwendungen. Üblicherweise verwaltet es die erzeugende Anwendung selbst oder man muss direkt in Pipewire damit damit agieren.


```bash
pactl load-module module-loopback latency_msec=1 source=<input_source> sink=<output_sink>
```

alternativ pw-loopback

**pw-loopback** läuft als Annwendung und wird beendet beim schließen des Terminal. daher mit **nohup** oder **disdown** ausführen

```bash

nohup pw-loopback --capture-props='node.name=MyCaptureNode' --playback-props='node.name=MyPlaybackNode' &
```

oder 

```bash
pw-loopback --capture-props='node.name=MyCaptureNode' --playback-props='node.name=MyPlaybackNode' & disown
```


So praktisch diese sein können, so kann man diese sich mit pactl leider nicht frei benennen. Weshalb ich bei Bedarf ein normales Sink erzeuge und diese zur Unterscheidung etwas anders benenne. Nachteil, es erschein in Auswahlmenüs.

fake loop device
```bash
pactl load-module module-null-sink media.class=Audio/Sink sink_name=IN-Loop-InternalMic node.description="IN_LoopSink_InternalMic"
```


## Audio - Setup(s) unter Linux

#### installation pipewierwe
Nutzer der audio - Gruppe hinzufügen

```bash
$sudo usermod -aG audio USERNAME
```

Installation Pipewire (falls nicht vorhanden, Pakete nachinstallieren)

```bash
$sudo pacman -S pipewire pipewire-pulse pipewire-alsa pipewire-jack wireplumber
```

Enabling Pipewire & Session Manager (vorher Status abfragen, ggf. schon vorh.)

```bash
$systemctl --user enable --now pipewire.socket
$systemctl --user enable --now pipewire-pulse.socket
$systemctl --user enable --now wireplumber.service
```

Pipewire Check

inxi -A		(zeigt Audio Devices und genutzten Audio-Server an)
pw-top		(zeigt Auslastung von Pipewire an)

Folgende Einstellungen sollten in der Config vorgenommen werden:

```bash
sudo vim ~/.config/pipewire/jack.conf
```

``` 
    ...
    jack.properties = {

   node.latency = 128/48000
    ...
    jack.short-name = true
    ...
    jack.self-connect-mode = allow
    …
```


#### Tools


##### QPWGraph 
	qpwqraph ist eine GUI für die Pipes innerhalb von Pipewire
	(Leider noch ein wenig fehlerhaft beim abspeichern)
    
```bash
	$pacman -S qpwgraph
```

##### AJ-Snapshot 
	Command Line Tool um Patchbay auszulesen u. abzuspeichern und via Skript
	vorzuladen (als Unterstützung zu pqwgraph)
    
```bash
	$pacman -S aj-snapshot
```

#####  Jack-Mixer
	
	Jack Desktop GUI Mixer (lautstärke diverser Kanäle anpassen)

```bash
$pacman -S jack_mixer
```

#### Installation Radio Software - Internet DJ Console (IDJC)


Internet DJ Console (IDJC)

##### Abhängigkeiten:
(Tipp - checken ob bereits installiert: yay -Ss <Paketname>)

* python (> 3.7)			python --version
 pacman -S python

* python-mutagen		pacman -S python-mutagen

* PyGObject			pacman -S python-gobject

* GLib2				pacman -S glib2

##### audio environment:

* libshout-idjc			yay -S libshout-idjc

* Jack				(siehe oben pipewire-jack)

* libsndfile			pacman -S libsndfile

* libsamplerate			pacman -S libsamplerate

* mpg123			pacman -S mpg123


##### codecs:

* TwoLAME			pacman -S twolame

* Lame				pacman -S lame

* FFmpeg			pacman -S ffmpeg

* vorbis-tools			pacman -S libvorbis lib32-libvorbis

* FLAC				pacman -S flac

* Speex				pacman -S speex

* Opus				pacman -S opus



#### Kompilieren von libshout-idjc & idjc


TODO:
einbauen 
https://docs.google.com/document/d/1rWF8Kuv9Gm9wCiRDcV0Teb8U7l3KHjG45gMPYh4_IkQ/edit?tab=t.0#heading=h.hfi6kzoengc2 






