# ⏱️ Lab 16 : Maîtriser les Services dans une Application Android

## 📋 Présentation du Projet
Ce laboratoire (Lab 16) vise à comprendre le fonctionnement et le cycle de vie des **Services** sous Android. L'objectif principal est de créer une application de "Chronomètre" fonctionnant en arrière-plan et communicant avec l'interface utilisateur. L'application est développée entièrement en Java.

---

## 🎯 Objectifs Pédagogiques
- Utiliser un **Foreground Service** pour exécuter une tâche prolongée de manière fiable (obligatoire depuis Android 8.0).
- Créer une **notification persistante** (`Ongoing Notification`) avec un `NotificationChannel` pour informer l'utilisateur de l'état du chronomètre.
- Implémenter un **Bound Service** avec la classe `Binder` pour permettre à l'Activity (`MainActivity`) de communiquer avec le Service en temps réel.
- Maîtriser le cycle de vie (`onCreate`, `onStartCommand`, `onBind`, `onDestroy`).
- Utiliser un `ScheduledExecutorService` pour incrémenter le temps de manière asynchrone sans bloquer le thread principal.

---

## 🛠️ Architecture du Code

### 1. ChronometreService (Foreground & Bound Service)
Le service est le cœur du projet.
- **`onStartCommand`** : Démarre le chronomètre. Utilise `startForeground` avec une notification persistante pour s'assurer que le système Android ne tue pas le service inopinément. Retourne `START_STICKY` pour redémarrer automatiquement en cas d'arrêt forcé.
- **`LocalBinder`** : Permet de renvoyer l'instance du `ChronometreService` à `MainActivity` lors de l'appel à `bindService`.
- **Incrémentation** : Un `ScheduledExecutorService` incrémente le chronomètre chaque seconde et met à jour la notification avec `NotificationManager.notify()`.

### 2. MainActivity
- Se connecte au Service via un objet `ServiceConnection`.
- Démarre le service via `startForegroundService` (ou `startService` pour API < 26) et établit la liaison avec `bindService`.
- Permet à l'utilisateur de démarrer ou d'arrêter le service avec l'envoi d'un `Intent` comportant l'action `"STOP"`.

### 3. Permissions et Manifeste
Les autorisations et déclarations requises ont été configurées dans `AndroidManifest.xml` :
- `<uses-permission android:name="android.permission.POST_NOTIFICATIONS"/>` (pour Android 13+).
- `<uses-permission android:name="android.permission.FOREGROUND_SERVICE"/>`
- `<uses-permission android:name="android.permission.FOREGROUND_SERVICE_DATA_SYNC"/>` (requis pour API 34+).
- Le service est déclaré avec `android:foregroundServiceType="dataSync"` et `android:exported="false"` pour des raisons de sécurité.

---

## 🚀 Utilisation de l'Application
1. Lancer l'application.
2. Cliquer sur **DÉMARRER SERVICE**.
   - Le compteur tourne.
   - Une notification persistante apparaît dans le tiroir de notifications avec le temps écoulé en direct.
3. Fermer l'application (en la balayant des tâches récentes).
   - Le service continue de tourner et la notification se met à jour en arrière-plan.
4. Relancer l'application et cliquer sur **ARRÊTER SERVICE**.
   - Le service est arrêté.
   - La notification disparaît.
   - Le chronomètre repasse à `00:00`.

---
**Développé par :** Laasri Mahmoud  
**Cadre :** Programmation Mobile : Android avec Java