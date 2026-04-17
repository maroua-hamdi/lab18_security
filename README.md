# 🔥 FireStorm CTF – Android Reverse Engineering Lab

![Android](https://img.shields.io/badge/Platform-Android-green)
![Frida](https://img.shields.io/badge/Tool-Frida-blue)
![Reverse Engineering](https://img.shields.io/badge/Domain-Reverse--Engineering-red)

---

## 📌 Description

Ce lab consiste à analyser une application Android afin de récupérer un **flag caché**.  
L’application repose sur plusieurs techniques :

- Analyse statique avec JADX
- Code natif (JNI)
- Instrumentation dynamique avec Frida
- Authentification Firebase

---

## 🎯 Objectif

Récupérer le **mot de passe Firebase généré dynamiquement**, puis accéder à la base de données pour obtenir le flag.

---

# 🧪 Étape 1 : Analyse statique (JADX)

Analyse de l’APK `FireStorm.apk` avec JADX.

### 📌 Informations trouvées :

- Package : `com.pwnsec.firestorm`
- Classe principale : `MainActivity`
- Méthode critique : `Password()`

---

### 📸 AndroidManifest

![AndroidManifest](images/manifest.png)

📍 Emplacement :
/images/manifest.png

---

### 📸 Méthode Password()

![Password Method](images/password_method.png)

📍 Emplacement :
/images/password_method.png

---

## 🧠 Analyse de la méthode Password()

La méthode `Password()` :

```java
public String Password() {
    StringBuilder sb = new StringBuilder();
    String string = getString(R.string.Friday_Night);
    String string2 = getString(R.string.Author);
    String string3 = getString(R.string.JustRandomString);
    String string4 = getString(R.string.URL);
    String string5 = getString(R.string.IDKMaybethepasswordpassowrd);
    String string6 = getString(R.string.Token);

    sb.append(string.substring(5, 9));
    sb.append(string4.substring(1, 6));
    sb.append(string2.substring(2, 6));
    sb.append(string5.substring(5, 8));
    sb.append(string3);
    sb.append(string6.substring(18, 26));

    return generateRandomString(String.valueOf(sb));
}
