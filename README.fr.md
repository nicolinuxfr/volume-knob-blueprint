# Bouton de volume (Zigbee2MQTT)

🇬🇧 [English version](README.md)

[![Ouvrir votre instance Home Assistant et afficher la boîte de dialogue d'import du blueprint.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fnicolinuxfr%2Fvolume-knob-blueprint%2Fgh-pages%2Ffr%2Fvolume_knob.yaml)

**URL directe (copier-coller) :**
```
https://raw.githubusercontent.com/nicolinuxfr/volume-knob-blueprint/gh-pages/fr/volume_knob.yaml
```

Ce blueprint permet de contrôler un ou plusieurs lecteurs multimédia avec un bouton rotatif Zigbee2MQTT (ex. IKEA SYMFONISK Sound Controller). La rotation ajuste le volume avec accélération, et les clics peuvent être configurés indépendamment.

## Comment ça fonctionne

1. Le blueprint conserve le sélecteur d'appareil Home Assistant et écoute en priorité les déclencheurs d'appareil MQTT Zigbee2MQTT liés à l'appareil sélectionné.
2. Il écoute aussi les topics MQTT bruts `zigbee2mqtt/<friendly_name>` et `zigbee2mqtt/<friendly_name>/action` comme voie de secours, ce qui permet de conserver les détails de rotation quand ils sont disponibles.
3. **La rotation** ajuste le volume vers le haut ou le bas. Quand le payload MQTT brut est disponible, la vitesse de rotation est détectée via `action_step_size` et `action_rate`, puis traduite en nombre de répétitions (1 à 12 pas de volume par événement) pour une accélération fluide. Si seul le déclencheur d'appareil remonte, la rotation fonctionne quand même avec le pas de base.
4. **Les clics** (simple, double, appui long) sont chacun associés à une action configurable : couper/rétablir le son, allumer/éteindre, lecture/pause ou aucune action.
5. Les actions de volume et mute/lecture ne ciblent que les lecteurs multimédia actuellement disponibles (pas éteints, inconnus ou indisponibles). L'action allumer/éteindre cible tous les lecteurs configurés, pour pouvoir allumer un lecteur éteint.

## Options de configuration

| Input | Description | Défaut |
|---|---|---|
| Appareil Zigbee2MQTT | Le bouton rotatif Z2M à utiliser (sélectionné parmi vos appareils MQTT) | — |
| Lecteur(s) multimédia | Le ou les lecteurs multimédia à contrôler | — |
| Simple clic | Action lors d'un simple clic | Couper/rétablir le son |
| Double clic | Action lors d'un double clic | Lecture / Pause |
| Appui long | Action lors d'un appui long | Allumer/éteindre |

## Appareils compatibles

Ce blueprint fonctionne avec tout encodeur rotatif Zigbee2MQTT qui envoie des événements de rotation `brightness_step_up`/`brightness_step_down` et des événements de bouton `toggle`/`double`/`hue_move`. Appareils compatibles connus :

- IKEA SYMFONISK Sound Controller (E1744)
- Autres encodeurs rotatifs Z2M avec le même format de payload

## Limitations connues

- L'appareil Z2M est sélectionné via le sélecteur d'appareils HA (filtré sur les appareils MQTT). Les déclencheurs d'appareil MQTT sont le chemin principal. Le secours par topics MQTT bruts suppose toujours que le nom de l'appareil dans Home Assistant correspond au nom convivial Zigbee2MQTT.
- Zigbee2MQTT ne découvre les déclencheurs d'appareil MQTT qu'après qu'une action correspondante a été émise au moins une fois par l'appareil.
- La bascule du mute vérifie l'état de chaque lecteur individuellement. Si les lecteurs ont des états mute différents, chacun sera basculé indépendamment.
- La courbe d'accélération est calibrée pour l'IKEA SYMFONISK. D'autres boutons pourraient nécessiter des seuils différents.
