# HANDOVER — Electrolab

Document de passation pour le projet **Electrolab** (site éducatif sur l'électricité et la mécanique pour Evan, 13 ans).

Dernière mise à jour : 2026-05-16

---

## 🌐 URL prod

- **Site en ligne :** https://haibi.github.io/Electricite-fun/
- **Déploiement :** GitHub Pages, branche `main`, dossier racine (`/`)
- Le déploiement se fait automatiquement à chaque `git push` sur `main` (1 à 2 min de latence).
- HTTPS forcé : oui.

---

## 📦 Repo GitHub

- **URL :** https://github.com/haibi/Electricite-fun
- **Visibilité :** publique
- **Propriétaire :** `haibi` (compte de Maximilien Haibi)
- **Branche principale :** `main`
- **Stack :** un seul fichier `index.html` auto-contenu (~4 290 lignes), zéro dépendance, zéro framework, HTML/CSS/JS pur.

### Identité Git locale (dans le dépôt)
```
user.name  = haibi
user.email = maximilien.haibi@yumea.fr
```

### Authentification
- `gh` CLI connecté en tant que `haibi` (keyring), scopes `gist`, `read:org`, `repo`.
- Le token n'est PAS stocké dans le repo.

---

## 🗄️ Base de données

**Aucune.** Le site est 100 % statique. Toute persistance se fait côté navigateur via :

| Clé localStorage         | Contenu                                        |
|--------------------------|------------------------------------------------|
| `elec_badges`            | Tableau des IDs de badges débloqués (14 max)   |
| `elec_missions`          | Tableau des IDs de missions complétées         |
| `elec_muted`             | `'1'` si le son est coupé                      |
| `meca_astres`            | Tableau des planètes déjà utilisées au lanceur |

Pas de backend, pas d'API serveur, pas d'appel externe au runtime (les questions de quiz sont hardcodées dans le JS).

---

## 👤 Comptes admin

**Aucun.** Pas d'authentification, pas de système d'utilisateur. Le site est ouvert à tous, anonyme.

Le seul "admin" est le propriétaire du repo GitHub (haibi) qui peut push sur `main`.

---

## ⚠️ Points d'attention

### 1. Merge Mécanik en cours, NON committé
- L'état actuel du fichier `index.html` contient l'intégration en cours du module **Mécanique** (lanceur de projectiles, atelier forces, quiz mécanique) dans l'app Electrolab.
- **Non committé** : `git status` montre `M index.html`.
- Structure ajoutée :
  - Sélecteur de matière `⚡ Électricité` / `🚀 Mécanique` au-dessus des onglets
  - Bloc `#subject-meca` avec 3 nouveaux onglets (Lanceur / Atelier Forces / Quiz méca)
  - +8 badges Mécanik dans l'objet `BADGES` (total 14)
  - JS Mécanik wrapped dans une IIFE pour éviter les conflits de noms (`mecaCurrentQuiz`, `QUESTIONS_MECA`, etc.)
  - CSS scoped via override des variables CSS sur `#subject-meca` (palette orange/bleu/vert au lieu de bleu/jaune/violet)
- **Pas encore testé en navigateur réel.** La syntaxe JS est validée (parse OK via `new Function(...)`).
- ⚠️ Le merge n'a pas été finalisé : il reste à vérifier le rendu visuel et l'interaction (canvas lanceur, pendule, ressort).

### 2. Tâche planifiée distante pour fix quiz
- Une routine cloud est programmée pour le **2026-05-17 à 06:35 Europe/Paris (04:35 UTC)**.
- ID : `trig_0172XYMEyP7Rin4Cr4rLhq9x`
- URL : https://claude.ai/code/routines/trig_0172XYMEyP7Rin4Cr4rLhq9x
- Mission : déboguer l'onglet Quiz Électricité (signalé non fonctionnel par l'utilisateur), tester en headless via jsdom, commit + push sur `main`.
- ⚠️ Cette routine cloud va probablement entrer en collision avec le merge Mécanik uncommitted local. Si le merge n'est pas committé/pushé avant 04:35 UTC, la routine partira sur l'état GitHub d'avant le merge.

### 3. Dépôt Mecanique-fun orphelin
- Un dépôt séparé `github.com/haibi/Mecanique-fun` existe encore (créé puis intégré dans Electrolab).
- Le dossier local `C:\Users\maxim\Desktop\Programme\Mecanique-fun\` existe aussi.
- Le lien vers ce dépôt a été retiré du sélecteur d'apps `+` dans Electrolab.
- L'utilisateur a indiqué vouloir consolider sur un seul repo. Suppression manuelle requise (non effectuée ici car instruction "Ne supprime rien").

### 4. Liens externes depuis le app switcher
Dans le bouton `+` de la topbar Electrolab :
- ⚡ Electrolab (current, non-cliquable)
- 🪐 Système Solaire → https://haibi.github.io/Systeme-solaire/ (même onglet)

### 5. Quiz : questions hardcodées
- Le quiz Électricité contient 24 questions hardcodées dans `const QUESTIONS = [...]`.
- Le quiz Mécanique contient 24 questions hardcodées dans `const QUESTIONS_MECA = [...]` (à l'intérieur de l'IIFE Mécanik).
- Aucun appel API au runtime (la version originale utilisait l'API Anthropic, retirée à la demande de l'utilisateur).

### 6. Audio Web Audio API
- L'objet global `Sound` est défini dans Electrolab pour : moteur (rumble), buzzer, fusible POP, grésillement, arc électrique.
- Les helpers Mécanik (`Sound.whoosh`, `Sound.impact`, `Sound.chime`, `Sound.snap`) sont **référencés mais non implémentés** dans le merge actuel. Les appels sont protégés par `if (Sound.whoosh) ...` donc no-op silencieux. À implémenter si on veut le son du tir, de l'impact, etc.

### 7. Préférences utilisateur (memory)
Memory file utilisateur : `C:\Users\maxim\.claude\projects\C--Users-maxim-Desktop-Programme-Site-de-Chlo-\memory\MEMORY.md`. Contient deux entrées :
- Feedback "ne pas écraser un index.html existant sans demander"
- Référence projet `kajiro-formation`

---

## 🧩 Architecture courte

```
index.html (~4 290 lignes)
├── <head>
│   ├── <title> ⚡ Electrolab – Evan
│   ├── <link rel="icon"> favicon SVG inline
│   └── <style> (~1 250 lignes)
│       ├── Variables CSS (bleu électrique / jaune néon / violet)
│       ├── Topbar, tabs, panels
│       ├── Composants électriques (LED, ampoule, moteur, buzzer, diode, fusible)
│       ├── Comparateur (SVG série/parallèle)
│       ├── Quiz (diff-cards, choices, feedback)
│       ├── Modal, toast, app switcher
│       ├── Subject switcher (⚡ / 🚀)
│       └── Mécanik : lanceur, planètes, pendule, ressort
├── <body>
│   ├── <canvas id="stars"> fond étoilé
│   ├── <header> logo + sélecteur apps + sons + badges
│   ├── <nav class="subject-switcher"> ⚡ Élec / 🚀 Méca
│   ├── <div id="subject-elec">
│   │   ├── nav.tabs : Labo / Comparateur / Quiz
│   │   └── main : 3 sections (view-lab, view-cmp, view-quiz)
│   ├── <div id="subject-meca">
│   │   ├── nav.tabs : Lanceur / Atelier Forces / Quiz
│   │   └── main : 3 sections (view-launcher, view-forces, view-meca-quiz)
│   ├── modals (.modal-backdrop, props popover, toast)
│   └── <script> (~2 200 lignes)
│       ├── 1.  Fond étoilé canvas
│       ├── 1b. App switcher
│       ├── 2.  Tabs (scoped per subject-block)
│       ├── 2b. Subject switcher
│       ├── 3.  Badges (14 au total)
│       ├── 4.  Modal
│       ├── 5.  Fiches pédagogiques composants
│       ├── 5b. Explication des mesures U/I/P/R
│       ├── 5c. Système Audio Web Audio API
│       ├── 5d. Tips pédagogiques (grillage)
│       ├── 6.  LABO (drag&drop, calcul circuit)
│       ├── 7.  COMPARATEUR (SVG + canvas chart)
│       ├── 8.  QUIZ ÉLEC (24 Q hardcodées)
│       ├── 9.  MISSIONS (8 défis progressifs)
│       └── 10. MÉCANIK (IIFE : lanceur, pendule, ressort, quiz méca)
```

---

## 🔧 Comment relancer en local

```bash
# Depuis la racine du repo (C:/Users/maxim/Desktop/Programme/Electricite-fun)
python -m http.server 8765
# Puis ouvrir http://localhost:8765
```

Ou utiliser la config preview Claude Code existante :
- `.claude/launch.json` (au niveau du worktree parent) définit un serveur `electrolab` sur port 8765.

---

## 📜 Historique git récent

```
4b69510  App switcher : ajout de Mécanik (forces & mouvement)
ecdc7d2  Ajout favicon SVG (éclair jaune sur fond cosmique)
acde46f  App switcher : ouvre les liens dans la même fenêtre
edb9a2f  Ajout app switcher (+) dans la topbar
176c3ee  Refactor mesures + Diode + Mesures locales + Mode missions
4e9753b  Ajout bulles pédagogiques + sons
52def77  Initial commit: Electrolab — site éducatif électricité pour Evan
```

---

## ✅ Checklist de reprise

Si tu reprends ce projet à froid :

1. [ ] `git status` → voir le merge Mécanik uncommitted
2. [ ] Lancer un serveur local (`python -m http.server 8765`)
3. [ ] Ouvrir http://localhost:8765 et tester :
   - [ ] L'onglet ⚡ Électricité fonctionne comme avant (Labo, Comparateur, Quiz)
   - [ ] Le switch vers 🚀 Mécanique affiche les 3 nouveaux onglets
   - [ ] Le lanceur dessine quelque chose et anime un tir
   - [ ] Le pendule oscille
   - [ ] Le ressort s'étire
   - [ ] Le quiz Mécanique pose une question et accepte une réponse
4. [ ] Corriger ce qui ne marche pas (canvas qui ne se dimensionnent pas avant le premier switch, par exemple)
5. [ ] Commit + push
6. [ ] Vérifier la prod : https://haibi.github.io/Electricite-fun/
7. [ ] Décider du sort du dépôt orphelin `Mecanique-fun`
