## SAE3 – STM32L432KC Project

### English

#### Overview
SAE3 is an STM32CubeMX/STM32CubeIDE project targeting the STM32L432KC microcontroller (commonly used on the NUCLEO‑L432KC board). The repository contains the auto‑generated HAL setup and user code regions where application logic can be implemented.

#### SAÉ S3.1 – Real‑Time Audio Compression Effect
This project can run a real‑time dynamic range compression effect on the STM32 NUCLEO‑L432KC. It continuously analyzes the most recent input audio window of duration \(\Delta t = 1\,\text{s}\) to estimate the peak amplitude \(V_{amp}\). The applied gain \(A\) follows a piecewise law:

\[\displaystyle
A(V_{amp}) =
\begin{cases}
1, & \text{if } V_{amp} < V_{th} \\
a\,V_{amp} + b + \dfrac{c}{V_{amp}}, & \text{if } V_{th} \le V_{amp} < V_{max} \\
\dfrac{2V_{th}+\Delta V}{2V_{amp}}, & \text{if } V_{max} \le V_{amp}
\end{cases}
\]

where \(V_{max} = V_{th} + \Delta V\). For the demonstration settings specified in the brief: \(V_{th} = 1\,\text{V}\) and \(\Delta V = 2\,\text{V}\). Constants \(a\), \(b\), and \(c\) are chosen so that the gain is continuous at the region boundaries.

Key points:
- The compressor reduces gain only when the recent amplitude exceeds the threshold; quieter passages remain unchanged.
- Unlike distortion, the waveform shape is preserved while its amplitude range is controlled.

Technical notes:
- Hardware: NUCLEO‑L432KC (STM32L432KC)
- Sample rate: 32 kHz
- User controls: The specification anticipates making \(V_{th}\) and \(\Delta V\) adjustable (e.g., potentiometer or buttons) and displaying their values; the demo uses fixed values.

How to use (adapt to your environment):
- Open the project in STM32CubeIDE, build, and flash the board.
- Feed the processing board with the acquisition board output and route its output to the restitution board.
- Implement or adjust the compression routine inside USER CODE regions under `Core/Src` (e.g., the audio processing callback), and, if desired, map inputs to \(V_{th}\) and \(\Delta V\).

#### Key Components
- `SAE3.ioc`: STM32CubeMX configuration file (pins, clocks, middleware). Regenerate code from this file when configuration changes.
- `Core/`: Application sources (`Src/`) and headers (`Inc/`), including `main.c`, initialization code, and user code sections.
- `Drivers/`: ST HAL and CMSIS libraries used by the project.
- `Debug/`: IDE/debugger artifacts (may be re‑created by the IDE).
- `STM32L432KCUX_FLASH.ld`: Linker script used for building the firmware.

#### Prerequisites
- STM32CubeIDE (recommended) or another ARM GCC toolchain
- ST‑Link drivers (if flashing with ST‑Link)
- Optionally, STM32CubeMX (to modify `SAE3.ioc` and regenerate sources)
- Optional CLI tools: `STM32_Programmer_CLI` (from STM32CubeProgrammer) or `openocd`

#### Build & Flash
1) Using STM32CubeIDE
   - Open STM32CubeIDE → File → Open Projects from File System… → select the project root.
   - Ensure the target is STM32L432KC and build the project.
   - Connect the board via ST‑Link and click Debug/Run to flash.

2) Using CLI (example with STM32CubeProgrammer)
   - Build with your GCC ARM toolchain to produce `SAE3.hex` or `SAE3.bin`.
   - Flash (example):
     - `STM32_Programmer_CLI -c port=SWD -w SAE3.hex -v -rst`

#### Typical Project Flow
- Edit pin/clock/peripheral setup in `SAE3.ioc`.
- Generate code in STM32CubeMX (or through CubeIDE’s Device Configuration Tool).
- Add application logic under `Core/Src` and headers in `Core/Inc` inside the designated USER CODE sections.

#### Debugging
- Use STM32CubeIDE debugger (ST‑Link). Set breakpoints in `Core/Src/main.c` and other sources.
- Verify clock and peripheral init in `SystemClock_Config()` and `MX_*_Init()` functions.

#### Notes
- Avoid editing HAL files under `Drivers/` unless necessary; prefer USER CODE blocks in `Core/` files.
- If regeneration overwrites files, only code inside USER CODE sections persists.

---

### Français

#### Présentation
SAE3 est un projet STM32CubeMX/STM32CubeIDE pour le microcontrôleur STM32L432KC (souvent utilisé avec la carte NUCLEO‑L432KC). Le dépôt contient la configuration HAL auto‑générée et des zones de code utilisateur pour implémenter la logique applicative.

#### SAÉ S3 – Effet de Compression Audio en Temps Réel
Ce projet peut exécuter un effet de compression de la plage dynamique en temps réel sur la carte STM32 NUCLEO‑L432KC. Le système analyse en continu la fenêtre la plus récente du signal d’entrée de durée \(\Delta t = 1\,\text{s}\) pour estimer l’amplitude de crête \(V_{amp}\). Le gain appliqué \(A\) suit la loi par morceaux suivante :

\[\displaystyle
A(V_{amp}) =
\begin{cases}
1, & \text{si } V_{amp} < V_{th} \\
a\,V_{amp} + b + \dfrac{c}{V_{amp}}, & \text{si } V_{th} \le V_{amp} < V_{max} \\
\dfrac{2V_{th}+\Delta V}{2V_{amp}}, & \text{si } V_{max} \le V_{amp}
\end{cases}
\]

où \(V_{max} = V_{th} + \Delta V\). Pour la démonstration : \(V_{th} = 1\,\text{V}\) et \(\Delta V = 2\,\text{V}\). Les constantes \(a\), \(b\) et \(c\) sont choisies afin d’assurer la continuité du gain aux frontières.

Points clés :
- Le compresseur réduit le gain uniquement lorsque l’amplitude récente dépasse le seuil ; les passages faibles restent inchangés.
- Contrairement à la distorsion, la forme d’onde est préservée tandis que l’amplitude est contrôlée.

Notes techniques :
- Matériel : NUCLEO‑L432KC (STM32L432KC)
- Fréquence d’échantillonnage : 32 kHz
- Contrôles utilisateurs : Le cahier des charges prévoit que \(V_{th}\) et \(\Delta V\) puissent être variables (potentiomètre ou boutons) avec affichage ; la démo utilise des valeurs fixes.

Utilisation (à adapter à votre environnement) :
- Ouvrir le projet dans STM32CubeIDE, compiler et flasher la carte.
- Relier la carte de traitement à la carte d’acquisition (entrée) et à la carte de restitution (sortie).
- Implémenter/ajuster la routine de compression dans les zones USER CODE de `Core/Src` (ex. rappel de traitement audio) et, si besoin, lier des entrées à \(V_{th}\) et \(\Delta V\).

#### Composants clés
- `SAE3.ioc` : fichier de configuration STM32CubeMX (broches, horloges, middlewares). Régénérez le code après toute modification.
- `Core/` : sources (`Src/`) et en‑têtes (`Inc/`), incluant `main.c`, l’initialisation et les sections USER CODE.
- `Drivers/` : bibliothèques ST HAL et CMSIS utilisées par le projet.
- `Debug/` : artefacts de l’IDE/du débogueur (peuvent être recréés par l’IDE).
- `STM32L432KCUX_FLASH.ld` : script d’édition de liens utilisé pour la construction du firmware.

#### Prérequis
- STM32CubeIDE (recommandé) ou une toolchain GCC ARM
- Pilotes ST‑Link (si flash via ST‑Link)
- Optionnellement, STM32CubeMX (pour modifier `SAE3.ioc` et régénérer le code)
- Outils CLI optionnels : `STM32_Programmer_CLI` (STM32CubeProgrammer) ou `openocd`

#### Compilation & Flash
1) Avec STM32CubeIDE
   - Ouvrir STM32CubeIDE → File → Open Projects from File System… → sélectionner la racine du projet.
   - Vérifier la cible STM32L432KC puis compiler.
   - Connecter la carte en ST‑Link et cliquer Debug/Run pour flasher.

2) En ligne de commande (exemple STM32CubeProgrammer)
   - Compiler avec GCC ARM pour produire `SAE3.hex` ou `SAE3.bin`.
   - Flasher (exemple) :
     - `STM32_Programmer_CLI -c port=SWD -w SAE3.hex -v -rst`

#### Flux de travail typique
- Modifier la configuration broches/horloges/périphériques dans `SAE3.ioc`.
- Régénérer le code via STM32CubeMX (ou l’outil de configuration de CubeIDE).
- Ajouter la logique applicative dans `Core/Src` et les en‑têtes dans `Core/Inc` aux emplacements USER CODE.

#### Débogage
- Utiliser le débogueur STM32CubeIDE (ST‑Link). Placer des points d’arrêt dans `Core/Src/main.c` et autres fichiers.
- Vérifier l’initialisation de l’horloge et des périphériques dans `SystemClock_Config()` et les fonctions `MX_*_Init()`.

#### Remarques
- Éviter de modifier les fichiers HAL dans `Drivers/` sauf nécessité ; privilégier les blocs USER CODE dans `Core/`.
- En cas de régénération, seul le code dans les sections USER CODE est conservé.


