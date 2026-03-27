# Bouton de volume (Zigbee2MQTT)

🇬🇧 [English version](README.md)

[![Ouvrir votre instance Home Assistant et afficher la boîte de dialogue d'import du blueprint.](https://my.home-assistant.io/badges/blueprint_import.svg)](https://my.home-assistant.io/redirect/blueprint_import/?blueprint_url=https%3A%2F%2Fraw.githubusercontent.com%2Fnicolinuxfr%2Fvolume-knob-blueprint%2Fgh-pages%2Ffr%2Fvolume_knob.yaml)

**URL directe (copier-coller) :**
```
https://raw.githubusercontent.com/nicolinuxfr/volume-knob-blueprint/gh-pages/fr/volume_knob.yaml
```

Ce blueprint permet de contrôler un ou plusieurs lecteurs multimédia avec un bouton rotatif Zigbee2MQTT (ex. IKEA SYMFONISK Sound Controller). La rotation ajuste le volume avec accélération, et les clics peuvent être configurés indépendamment.

## Comment ça fonctionne

1. Le blueprint utilise uniquement le sélecteur d'appareil Home Assistant.
2. Les déclenchements passent par les événements d'action MQTT exposés par l'appareil sélectionné.
3. **La rotation** ajuste le volume vers le haut ou le bas. La vitesse de rotation est lue depuis les capteurs `action_step_size` et `action_rate` du même device, puis traduite en nombre de répétitions (1 à 12 pas de volume par événement) pour une accélération fluide.
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

- Le device sélectionné doit exposer des événements d'action MQTT ainsi que les capteurs `action_step_size` et `action_rate`.
- La bascule du mute vérifie l'état de chaque lecteur individuellement. Si les lecteurs ont des états mute différents, chacun sera basculé indépendamment.
- La courbe d'accélération est calibrée pour l'IKEA SYMFONISK. D'autres boutons pourraient nécessiter des seuils différents.
