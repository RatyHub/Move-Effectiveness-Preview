# Move Effectiveness Preview

## Description

**FR :** Plugin PSDK qui affiche l'efficacité prévue d'une attaque directement dans l'interface de sélection des capacités pendant le combat.

Il peut fonctionner de deux façons :
- en affichage fixe sous la liste des capacités
- ou directement dans chaque bouton de capacité

Le script prend en charge les combats `1v1`, `2v2` et `3v3`, les attaques à cible unique, les attaques à choix de cible et les attaques multi-cibles.

**EN :** PSDK plugin that displays the expected move effectiveness directly in the move selection UI during battle.

It can work in two different ways:
- as a fixed preview shown under the move list
- or directly inside each move button

The script supports `1v1`, `2v2` and `3v3` battles, single-target moves, target-selection moves and multi-target moves.

## Behaviour

### FR

- En `DISPLAY_MODE = 1`, le script affiche un aperçu fixe avec un asset de fond et un texte.
- En `DISPLAY_MODE = 2`, le script affiche l'aperçu directement dans chaque bouton de capacité.
- En `DISPLAY_MODE = 0`, le script n'affiche rien du tout.
- Si une attaque est neutre, l'affichage dépend de `SHOW_WHEN_NEUTRAL`.
- En `1v1`, une attaque à cible unique affiche la cible prévue par défaut.
- En `2v2` et `3v3`, si PSDK ouvre réellement un choix de cible pour une attaque mono-cible, le script prévisualise tous les ennemis valides.
- L'ordre des multiplicateurs suit l'ordre réel des adversaires.
- En cas de valeurs différentes sur plusieurs cibles :
  - en `1v1` et `2v2`, le format est `Variable (x0.5 / x2)`;
  - en `3v3`, le format compact est `x0.5 / x2 / x4` pour gagner de la place.
- Les attaques sans aperçu utile n'affichent rien.

### EN

- In `DISPLAY_MODE = 1`, the script shows a fixed preview with a background asset and a text label.
- In `DISPLAY_MODE = 2`, the preview is shown directly inside each move button.
- In `DISPLAY_MODE = 0`, the script is fully disabled.
- If a move is neutral, its visibility depends on `SHOW_WHEN_NEUTRAL`.
- In `1v1`, a single-target move previews the default target chosen by PSDK.
- In `2v2` and `3v3`, if PSDK actually opens target selection for a single-target move, the script previews every valid enemy target.
- Multiplier order follows the real enemy order.
- When different targets produce different multipliers:
  - in `1v1` and `2v2`, the format is `Variable (x0.5 / x2)`;
  - in `3v3`, the compact format is `x0.5 / x2 / x4` to save space.
- Moves with no useful preview do not show anything.

## Installation

### FR

1. Téléchargez le fichier `move_effectiveness_preview.psdkplug`.
2. Placez-le dans le dossier `scripts` de votre projet PSDK.
3. Démarrez le jeu.

Le Plugin Manager de PSDK installera automatiquement le plugin.

### EN

1. Download the `move_effectiveness_preview.psdkplug` file.
2. Put it in your PSDK project's `scripts` folder.
3. Start the game.

PSDK Plugin Manager will automatically install the plugin.

## Asset Notes

### FR

Les deux assets de fond sont prévus comme des spritesheets verticales à 3 frames :

| Frame | Utilisation |
| --- | --- |
| 1 | efficacité inférieure à `x1` |
| 2 | `x1` ou cas mixte / variable |
| 3 | efficacité supérieure à `x1` |

### EN

Both background assets are expected to be vertical 3-frame spritesheets:

| Frame | Usage |
| --- | --- |
| 1 | effectiveness below `x1` |
| 2 | `x1` or mixed / variable case |
| 3 | effectiveness above `x1` |

## Configuration

### FR

Les constantes de configuration se trouvent au début du script :

`scripts/99998 Move Effectiveness Preview.rb`

#### Réglages généraux

| Constante | Description |
| --- | --- |
| `DISPLAY_MODE` | Mode d'affichage : `0` désactive le script, `1` affiche un preview fixe, `2` affiche un preview dans chaque bouton de capacité. |
| `SHOW_WHEN_NEUTRAL` | Indique si les cas neutres (`x1`) doivent être affichés. S'applique aux deux modes. |
| `TEXT_COLOR` | Couleur du texte utilisée pour l'aperçu dans les deux modes. |
| `VARIABLE_TEXT` | Label utilisé pour les cas variables en `1v1` et `2v2`. |

#### Réglages du mode 1

| Constante | Description |
| --- | --- |
| `BACKGROUND_IMAGE` | Asset utilisé derrière le texte en mode 1. |
| `BACKGROUND_POSITION` | Position du fond du preview fixe en mode 1 : `[x, y]`. |

#### Réglages du mode 2

| Constante | Description |
| --- | --- |
| `MODE_2_BACKGROUND_IMAGE` | Asset utilisé derrière le texte en mode 2. |
| `MODE_2_BACKGROUND_OFFSET` | Décalage appliqué au fond du preview du mode 2 depuis sa position de base : `[x, y]`. |

Si vous souhaitez modifier ces constantes, la bonne pratique est de faire un monkey patch dans un script chargé après celui-ci, par exemple :

```ruby
module EffectivenessPreview
  remove_const(:DISPLAY_MODE)
  remove_const(:SHOW_WHEN_NEUTRAL)
  remove_const(:TEXT_COLOR)
  remove_const(:VARIABLE_TEXT)
  remove_const(:BACKGROUND_IMAGE)
  remove_const(:BACKGROUND_POSITION)
  remove_const(:MODE_2_BACKGROUND_IMAGE)
  remove_const(:MODE_2_BACKGROUND_OFFSET)

  DISPLAY_MODE = 2
  SHOW_WHEN_NEUTRAL = false
  TEXT_COLOR = 9
  VARIABLE_TEXT = 'Variable'
  BACKGROUND_IMAGE = 'battle/button_effectiveness_fixed_preview'
  BACKGROUND_POSITION = [2, 188]
  MODE_2_BACKGROUND_IMAGE = 'battle/button_effectiveness_skill_preview'
  MODE_2_BACKGROUND_OFFSET = [-3, 2]
end
```

### EN

The configuration constants are located at the top of the script:

`scripts/99998 Move Effectiveness Preview.rb`

#### General settings

| Constant | Description |
| --- | --- |
| `DISPLAY_MODE` | Display mode: `0` disables the script, `1` shows a fixed preview, `2` shows a preview inside each move button. |
| `SHOW_WHEN_NEUTRAL` | Tells whether neutral cases (`x1`) should be shown. Applies to both modes. |
| `TEXT_COLOR` | Text color used by the preview in both modes. |
| `VARIABLE_TEXT` | Label used for variable cases in `1v1` and `2v2`. |

#### Display mode 1 settings

| Constant | Description |
| --- | --- |
| `BACKGROUND_IMAGE` | Asset used behind the text in display mode 1. |
| `BACKGROUND_POSITION` | Position of the fixed preview background in mode 1: `[x, y]`. |

#### Display mode 2 settings

| Constant | Description |
| --- | --- |
| `MODE_2_BACKGROUND_IMAGE` | Asset used behind the text in display mode 2. |
| `MODE_2_BACKGROUND_OFFSET` | Offset applied to the mode 2 preview background from its default position: `[x, y]`. |

If you want to modify these constants, the recommended approach is to use a monkey patch in a script loaded after this one, for example:

```ruby
module EffectivenessPreview
  remove_const(:DISPLAY_MODE)
  remove_const(:SHOW_WHEN_NEUTRAL)
  remove_const(:TEXT_COLOR)
  remove_const(:VARIABLE_TEXT)
  remove_const(:BACKGROUND_IMAGE)
  remove_const(:BACKGROUND_POSITION)
  remove_const(:MODE_2_BACKGROUND_IMAGE)
  remove_const(:MODE_2_BACKGROUND_OFFSET)

  DISPLAY_MODE = 2
  SHOW_WHEN_NEUTRAL = false
  TEXT_COLOR = 9
  VARIABLE_TEXT = 'Variable'
  BACKGROUND_IMAGE = 'battle/button_effectiveness_fixed_preview'
  BACKGROUND_POSITION = [2, 188]
  MODE_2_BACKGROUND_IMAGE = 'battle/button_effectiveness_skill_preview'
  MODE_2_BACKGROUND_OFFSET = [-3, 2]
end
```
