# 🔥 FireStorm — Android CTF Challenge Writeup

Challenge de reverse engineering Android combinant analyse statique (JADX), instrumentation dynamique (Frida) et authentification Firebase pour récupérer un flag.

---

## 📋 Étapes du challenge

1. Préparation de l'environnement
2. Analyse statique avec JADX
3. Script Frida – Explication détaillée
4. Configuration Firebase extraite de strings.xml
5. Script Python pour l'authentification et la récupération du flag

---

## 🛠️ Réalisation

### Étape 1 — Analyse statique avec JADX-GUI

Ouverture du fichier `FireStorm.apk` dans JADX-GUI. On observe l'`AndroidManifest.xml` avec le package `com.pwnsec.firestorm` et les permissions réseau, ainsi que la classe `MainActivity` contenant la méthode `Password()`.

<img width="1915" height="1030" alt="img1" src="https://github.com/user-attachments/assets/8c79a98a-a846-4ee1-a3b0-a17bb395898d" />


---

### Étape 2 — Décompilation de la méthode `Password()` dans JADX

La méthode `Password()` est identifiée dans `MainActivity`. Elle construit le mot de passe en concatenant plusieurs substrings extraits de `strings.xml` (R.string.Friday_Night, R.string.Author, R.string.JustRandomString, etc.), puis appelle la fonction native `generateRandomString()` de la librairie `libfirestorm.so`.

<img width="1912" height="1030" alt="img2" src="https://github.com/user-attachments/assets/e9732aeb-c3f5-4752-bdf1-45d7dd3a261f" />


---

### Étape 3 — Script Frida et exécution

Création du fichier `frida_firestorm.js` et injection dans le processus de l'application via Frida. Le script appelle manuellement la méthode `Password()` sur une instance de `MainActivity` pour récupérer le mot de passe Firebase généré dynamiquement.

```bash
frida -U -p 4253 -l frida_firestorm.js
```

Le mot de passe Firebase récupéré est affiché dans la console :

```
[+] Mot de passe Firebase généré : C7_dotpsC7t7f_._In_i.IdttpaofoaIIdIdnndIfC
```

<img width="1067" height="756" alt="img3" src="https://github.com/user-attachments/assets/8bcb7dc9-d6ee-4b59-b139-175ba84b6de9" />


---

### Étape 4 — Authentification Firebase et récupération du flag

Utilisation du script Python `get_flag.py` avec le mot de passe obtenu via Frida pour s'authentifier sur Firebase avec l'email `TK757567@pwnsec.xyz`, puis lecture du flag depuis la Realtime Database.

```bash
python get_flag.py
```

<img width="1882" height="967" alt="img4" src="https://github.com/user-attachments/assets/c4a2a66e-a81c-4837-8fe6-0847e847294d" />


---

## 🏁 Flag

> Le flag est récupéré depuis la base de données Firebase après authentification réussie avec le mot de passe dynamique extrait via Frida.

---

## 🧰 Outils utilisés

| Outil | Usage |
|-------|-------|
| JADX-GUI | Décompilation de l'APK |
| Frida | Instrumentation dynamique |
| Android Emulator | Exécution de l'application |
| Python + pyrebase | Authentification Firebase |
| ADB | Communication avec l'émulateur |
