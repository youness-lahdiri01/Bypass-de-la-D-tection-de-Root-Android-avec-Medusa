# Bypass de la Détection de Root Android avec Medusa

## Objectif

L’objectif de ce lab est d’utiliser l’outil **Medusa** (basé sur Frida) pour contourner les mécanismes de détection de root dans une application Android.

---

## Environnement

* Émulateur Android (Android 11)
* Frida installé (client + frida-server)
* Medusa cloné depuis GitHub
* Application cible : `owasp.mstg.uncrackable3`

---

## Étape 1 — Vérification de Frida

```bash
frida-ps -U
```

Résultat attendu : liste des processus Android.

---

## Étape 2 — Lancement de Medusa

```bash
python medusa.py
```

Sélection du device :

```text
emulator-5554
```
<img width="1101" height="630" alt="Screenshot 2026-04-26 170356" src="https://github.com/user-attachments/assets/b88f3694-c4ec-46d0-b1ed-660f9ba3bf10" />

---

## Étape 3 — Recherche des modules

```bash
search root
```

Modules trouvés :

* root_detection/universal_root_detection_bypass
* root_detection/rootbeer_detection_bypass

---
<img width="563" height="270" alt="Screenshot 2026-04-26 172923" src="https://github.com/user-attachments/assets/a864c25d-e47e-4376-8f90-e958fa4ada72" />

## Étape 4 — Activation du module

```bash
use root_detection/universal_root_detection_bypass
```

---

## Étape 5 — Lancement de l’application

```bash
run -f owasp.mstg.uncrackable3
```
<img width="1919" height="851" alt="Screenshot 2026-04-26 174014" src="https://github.com/user-attachments/assets/fdaf7323-ed8a-498b-bd97-6ed963d6bd28" />

---

## Résultats obtenus

Logs observés :

```text
Bypass return value for binary: su
Bypass test-keys check
Bypass return value for binary: Superuser.apk
[!] overwriting is debugger connected
```

Ces logs confirment que :

* Les vérifications de fichiers root sont bypassées
* Les checks système (test-keys) sont interceptés
* Les appels liés au root sont modifiés

---

## Comportement de l’application

```text
Session is detached due to: process-terminated
```

L’application se ferme automatiquement après l’injection.

---

## Analyse

Le bypass de root est **réussi**, mais l’application implémente des protections supplémentaires :

* Détection de debugger
* Détection d’instrumentation (Frida)
* Vérifications natives

Ces mécanismes entraînent la terminaison du processus.

---

## Conclusion

Medusa permet de contourner efficacement les mécanismes de détection de root.

Cependant, pour des applications sécurisées comme **UnCrackable Level 3**, des techniques supplémentaires sont nécessaires pour bypass :

* Anti-debug
* Anti-Frida
* Protections natives
<img width="1918" height="999" alt="Screenshot 2026-04-26 185110" src="https://github.com/user-attachments/assets/477c012b-8ddd-4181-bc30-8ed53a5f9798" />

---

## Remarque

Dans ce lab, l’objectif principal (bypass root avec Medusa) est atteint et validé par les logs.

---

## Commandes utilisées

```bash
frida-ps -U
python medusa.py
search root
use root_detection/universal_root_detection_bypass
run -f owasp.mstg.uncrackable3
```

---

## Auteur

Youness Lahdiri
