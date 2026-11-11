# Guide de Fusion et Configuration : Marlin 2.1.x + BigTreeTech SKR Mini E3 V1.2 (Creality Ender 3 Pro)

Ce document détaille les étapes complètes pour recréer ce dépôt à partir des sources officielles de **Marlin** et des configurations spécifiques fournies par **BigTreeTech (BTT)** pour la **Creality Ender 3 Pro**.

---

## 📌 Contexte du Matériel

- **Imprimante :** Creality Ender 3 Pro
- **Carte mère :** BigTreeTech SKR Mini E3 V1.2 (MCU STM32F103RC, drivers TMC2209 intégrés)
- **Écran :** Écran d'origine Creality (CR10_STOCKDISPLAY)
- **Environnement PlatformIO :** `STM32F103RC_btt`

---

## 🛠️ Outils Requis

1. **[Visual Studio Code](https://code.visualstudio.com/)**
2. Extensions VS Code obligatoires :
   - **PlatformIO IDE**
   - *(Optionnel mais recommandé)* **Auto Build Marlin**
3. **Git** pour récupérer les sources.

---

## 📥 Étape 1 : Récupérer les dépôts nécessaires

### Marlin Officiel + Exemples de Configurations Marlin

#### Option 1 : Téléchargement direct (le plus simple)
Rendez-vous directement sur le lien GitHub du dossier :  
👉 **[Configurations Ender-3 Pro / BigTreeTech SKR Mini E3 1.2](https://github.com/MarlinFirmware/Configurations/tree/import-2.1.x/config/examples/Creality/Ender-3%20Pro/BigTreeTech%20SKR%20Mini%20E3%201.2)**  
Téléchargez les fichiers :
- `Configuration.h`
- `Configuration_adv.h`
- *(Optionnel)* `_Bootscreen.h` et `_Statusscreen.h`  
Puis copiez-les directement dans le dossier `Marlin/Marlin/` de votre projet

---

## ⚙️ Étape 2 : Configurations Clés dans le Code

### 1. Définir l'environnement dans `platformio.ini`
À la racine du projet, dans le fichier `platformio.ini` :
```ini
default_envs = STM32F103RC_btt
```

### 2. Définir la carte mère dans `Marlin/Configuration.h`
```cpp
#define MOTHERBOARD BOARD_BTT_SKR_MINI_E3_V1_2
#define SERIAL_PORT 1
#define SERIAL_PORT_2 -1
#define BAUDRATE 115200
#define CUSTOM_MACHINE_NAME "Ender 3 Pro"
```

### 3. Drivers de moteurs pas-à-pas (TMC2209 intégrés)
Dans `Marlin/Configuration.h` :
```cpp
#define X_DRIVER_TYPE  TMC2209
#define Y_DRIVER_TYPE  TMC2209
#define Z_DRIVER_TYPE  TMC2209
#define E0_DRIVER_TYPE TMC2209
```

### 4. Dimensions du plateau
```cpp
#define X_BED_SIZE 220
#define Y_BED_SIZE 220
#define Z_MAX_POS 250
```

### 5. Écran LCD
```cpp
#define CR10_STOCKDISPLAY
```

---

## 🚀 Étape 3 : Personnalisations spécifiques à ce dépôt

Dans ce projet, plusieurs améliorations et réglages personnalisés ont été apportés :
1. **Babystepping activé** : Permet de régler la hauteur de buse (BabyStep Z) en direct pendant la première couche.
2. **Préchauffages personnalisés** (`Marlin/Configuration.h`) :
   - Profils configurés pour : **PLA**, **PETG**, **TPU** et **ABS**.
3. **Gestion des bips / Son** (`Marlin/Configuration_adv.h`) :
   - Personnalisation sonore et désactivation des sons superflus.

---

## 🔨 Étape 4 : Compilation et Flash

1. Ouvrir le dossier du projet dans **VS Code**.
2. Attendre que **PlatformIO** installe les toolchains et bibliothèques STM32. (il faudra surement relancer VS Code)
3. Cliquer sur l'icône **PlatformIO** (tête d'alien à gauche) puis :
   - `STM32F103RC_btt` ➡️ **General** ➡️ **Build** (ou raccourci en bas : coche `✓`).
4. Une fois la compilation réussie (`SUCCESS`) :
   - Le firmware compilé se trouve dans : `.pio/build/STM32F103RC_btt/firmware.bin`.
5. **Flasher l'imprimante :**
   - Prendre une carte MicroSD (formatée en FAT32, allocation 4096 octets).
   - Copier le fichier `firmware.bin` à la racine de la carte.
   - Éteindre l'Ender 3 Pro, insérer la carte MicroSD, puis allumer l'imprimante.
   - L'écran reste bleu environ 10 à 20 secondes pendant le flash, puis Marlin démarre.
   - Vous devrez appercevoir un changement au démarrage après le logo ender : " XX - kad " modifiable dans Version.h mligne 31
   - Voir "pour aller plus loin" pour enregistrer définitivement la configuration ou alors retirez la carte et redemarez l'imprimante pour 
   


## Pour aller plus loin...

Les modifications apportés vous ont plus ? content pour vous ; )
dans ce cas n'hésitez pas a e,registrer les parametres du logiciel :
      - a partir de la dernière étape du 4.5 allez dans parametres avancés puis ""Enregistrer config."
      - SUr l'écran d'accueil un message devrai s'afficher 
      - retirez la carte microSd
      - au prochain démarage vous aurez ma version custom 
   

