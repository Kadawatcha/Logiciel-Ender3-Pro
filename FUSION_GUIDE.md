# Guide de Fusion et Configuration : Marlin 2.1.x + BigTreeTech SKR Mini E3 V1.2 (Creality Ender 3 Pro)

Ce document détaille les étapes complètes pour recréer ce firmware personnalisé à partir des sources officielles de **Marlin** et des configurations spécifiques pour la carte **BigTreeTech (BTT) SKR Mini E3 V1.2** sur **Creality Ender 3 Pro**.

---

## 📌 Contexte du Matériel

- **Imprimante :** Creality Ender 3 Pro
- **Carte mère :** BigTreeTech SKR Mini E3 V1.2 (MCU STM32F103RC, drivers TMC2209 intégrés)
- **Écran :** Écran d'origine Creality (`CR10_STOCKDISPLAY`)
- **Environnement PlatformIO :** `STM32F103RC_btt`

---

## 🛠️ Outils Requis

1. **[Visual Studio Code](https://code.visualstudio.com/)**
2. Extensions VS Code :
   - **PlatformIO IDE** (obligatoire)
   - **Auto Build Marlin** (optionnel mais recommandé)
3. **Git** (pour cloner les dépôts)

---

## 📥 Étape 1 : Récupérer les sources et la configuration de base

1. **Cloner Marlin officiel :**
   ```bash
   git clone -b 2.1.2.5 https://github.com/MarlinFirmware/Marlin.git
   ```

2. **Récupérer la configuration BTT SKR Mini E3 1.2 :**
   Rendez-vous sur le dépôt officiel des configurations d'exemples :  
   👉 **[Configurations Ender-3 Pro / BigTreeTech SKR Mini E3 1.2](https://github.com/MarlinFirmware/Configurations/tree/import-2.1.x/config/examples/Creality/Ender-3%20Pro/BigTreeTech%20SKR%20Mini%20E3%201.2)**

   Téléchargez les fichiers suivants :
   - `Configuration.h`
   - `Configuration_adv.h`
   - *(Optionnel)* `_Bootscreen.h` et `_Statusscreen.h`

3. **Placer les fichiers :**  
   Copiez ces fichiers directement dans le sous-dossier `Marlin/` de votre projet Marlin.

---

## ⚙️ Étape 2 : Configurations Clés dans le Code

### 1. Définir l'environnement dans `platformio.ini`
À la racine du projet, dans le fichier `platformio.ini` :
```ini
default_envs = STM32F103RC_btt
```

### 2. Paramètres de la carte dans `Marlin/Configuration.h`
```cpp
#define MOTHERBOARD BOARD_BTT_SKR_MINI_E3_V1_2
#define SERIAL_PORT 1
#define SERIAL_PORT_2 -1
#define BAUDRATE 115200
#define CUSTOM_MACHINE_NAME "Ender 3 Pro"
```

### 3. Drivers moteurs (TMC2209 intégrés)
```cpp
#define X_DRIVER_TYPE  TMC2209
#define Y_DRIVER_TYPE  TMC2209
#define Z_DRIVER_TYPE  TMC2209
#define E0_DRIVER_TYPE TMC2209
```

### 4. Dimensions du plateau et écran
```cpp
#define X_BED_SIZE 220
#define Y_BED_SIZE 220
#define Z_MAX_POS 250

#define CR10_STOCKDISPLAY
```

---

## 🚀 Étape 3 : Personnalisations spécifiques à ce dépôt

Plusieurs améliorations ont été intégrées dans ce fork :
1. **Babystepping activé** (`Marlin/Configuration_adv.h`) :
   - Permet d'ajuster finement la hauteur de buse (BabyStep Z) en temps réel pendant l'impression de la première couche.
2. **Préchauffages personnalisés** (`Marlin/Configuration.h`) :
   - Profils de températures adaptés pour : **PLA**, **PETG**, **TPU** et **ABS**.
3. **Gestion des bips / Son** (`Marlin/Configuration_adv.h`) :
   - Personnalisation sonore et suppression des bips intempestifs.
4. **Signature de version** (`Marlin/Version.h`, ligne 31) :
   - Personnalisation de l'affichage de démarrage (`SHORT_BUILD_VERSION "5.2 - Kad"`).

---

## 🔨 Étape 4 : Compilation et Flash

1. Ouvrir le dossier du projet dans **VS Code**.
2. Laisser **PlatformIO** télécharger les toolchains et bibliothèques STM32 (un redémarrage de VS Code peut être nécessaire si c'est la première installation).
3. Compiler le firmware :
   - Ouvrir l'onglet **PlatformIO** (icône alien à gauche) ➡️ `STM32F103RC_btt` ➡️ **General** ➡️ **Build** (ou cliquer sur la coche `✓` en bas de la fenêtre).
4. Une fois la compilation terminée (`SUCCESS`) :
   - Le fichier binaire se situe dans : `.pio/build/STM32F103RC_btt/firmware.bin`.
5. **Flasher l'imprimante :**
   - Prendre une carte MicroSD (formatée en **FAT32**, taille d'allocation de 4096 octets).
   - Copier le fichier `firmware.bin` à la racine de la carte.
   - Éteindre l'Ender 3 Pro.
   - Insérer la carte MicroSD, puis allumer l'imprimante.
   - L'écran reste bleu environ 10 à 20 secondes pendant le flash, puis Marlin démarre.
   - Vous devriez voir le message personnalisé (`5.2 - Kad`) après le logo Ender.
   - Suivez la section ci-dessous pour valider et enregistrer la configuration.

---

## 💡 Pour aller plus loin : Enregistrer les paramètres dans l'EEPROM

Pour que l'imprimante conserve définitivement tous les réglages et initialisations :

1. Directement sur l'écran de l'Ender 3 Pro après le premier démarrage :
   - Rendez-vous dans : **Configuration** ➡️ **Enregistrer config.**.
2. Un message de confirmation s'affichera sur l'écran d'accueil.
3. Le fichier sur la carte MicroSD est alors renommé en `FIRMWARE.CUR` attestant que la configuration a bien été enregistrée et le flash validé.
4. Éteignez l'imprimante, retirez la carte MicroSD (ou supprimez le fichier de la carte pour éviter de reflasher par erreur).
5. Rallumez : votre configuration personnalisée est prête et opérationnelle !

