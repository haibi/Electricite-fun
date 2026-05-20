# HANDOVER — Electrolab + Mécanik + Système Solaire

Document de passation pour le portail éducatif d'Evan (13 ans).
3 apps indépendantes dans un seul repo, navigation via le menu `+`.

Dernière mise à jour : 2026-05-20

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
- **Stack :** 4 fichiers HTML auto-contenus, zéro dépendance, zéro framework, HTML/CSS/JS pur.

| Fichier          | App                     | Lignes |
|------------------|-------------------------|--------|
| `index.html`     | ⚡ Electrolab (Élec)    | ~2 980 |
| `mecanique.html` | 🚀 Mécanik              | ~2 160 |
| `solaire.html`   | 🪐 Système Solaire      | ~3 460 |
| `solaire-quiz.html` | 🪐 Quiz Solaire      | ~1 060 |
| `laser.html`     | 🔴 Laser Maze           | ~1 250 |
| `reflex.html`    | ⚡ Reflex               | ~1 350 |
| `volcan.html`    | 🌋 Volcan               | ~1 520 |
| `callab.html`    | 🧪 CalLab               | ~1 430 |
| `machine.html`   | ⚙️ Réac Lab             | ~2 500 |

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

### 1. Quiz : questions hardcodées
- Le quiz Électricité contient 24 questions hardcodées dans `const QUESTIONS = [...]` (dans `index.html`).
- Le quiz Mécanique contient 24 questions hardcodées dans `const QUESTIONS_MECA = [...]` (dans `mecanique.html`).
- Le quiz Système Solaire est dans `solaire-quiz.html` avec ses propres questions.
- Aucun appel API au runtime.

### 2. Audio Web Audio API
- L'objet global `Sound` est défini dans `index.html` (Electrolab) : moteur, buzzer, fusible POP, grésillement, arc électrique.
- Dans `mecanique.html` : `Sound.whoosh`, `Sound.impact`, `Sound.chime`, `Sound.snap` sont implémentés.
- Chaque page gère son propre contexte audio.

### 3. App switcher (`+`)
Le bouton `+` dans la topbar de chaque page pointe vers les autres apps :
- `index.html` ↔ `mecanique.html` ↔ `solaire.html`
- Les liens sont relatifs et fonctionnent en local et en prod GitHub Pages.

### 4. Idée "mur à casser" — NE PAS RÉIMPLÉMENTER sans refonte
Une fonctionnalité "mur de briques à casser" dans le lanceur Mécanik a été testée puis abandonnée (mai 2026).
Problèmes : balle disproportionnée vs briques, cassage trop facile, mur reconstruit auto à chaque tir.
→ Voir le fichier memory `idea_electrolab_mur_a_casser.md` pour le détail.

---

## 🧩 Architecture courte

```
Electricite-fun/
├── index.html          (~2 980 lignes) — ⚡ Electrolab
│   ├── <style> Variables CSS (bleu électrique / jaune néon / violet)
│   ├── Topbar + App switcher (+)
│   ├── Subject switcher ⚡ Élec / 🚀 Méca (lien vers mecanique.html)
│   └── <script>
│       ├── Fond étoilé canvas
│       ├── Badges (14), Modal, Toast
│       ├── LABO (drag&drop, calcul circuit)
│       ├── COMPARATEUR (SVG + canvas chart)
│       ├── QUIZ ÉLEC (24 Q hardcodées)
│       └── MISSIONS (8 défis progressifs)
│
├── mecanique.html      (~2 160 lignes) — 🚀 Mécanik
│   ├── Lanceur de projectiles (canvas, physique parabolique, 8 astres)
│   ├── Atelier Forces (pendule, ressort)
│   ├── Quiz Mécanique (24 Q hardcodées)
│   └── Badges Mécanik (8)
│
├── solaire.html        (~3 460 lignes) — 🪐 Système Solaire
│   ├── Simulation orbites planétaires (canvas animé)
│   ├── Fiches planètes (pop-up au clic)
│   └── Lien vers solaire-quiz.html
│
├── solaire-quiz.html   (~1 060 lignes) — 🪐 Quiz Solaire
│   └── QCM sur le système solaire
│
├── favicon-meca.svg    — Favicon SVG fusée (pour mecanique.html)
├── HANDOVER.md         — Ce fichier
└── README.md           (si présent)
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
223592c  Revert : suppression du mode mur à casser (retour à l'état précédent)
8f3f161  Doc: lien Système Solaire pointe vers la page locale solaire.html
cedbfa4  Update HANDOVER : architecture 3 apps (Electrolab + Mécanik + Système Solaire)
8e3043f  Intégration Système Solaire en 3ᵉ app (solaire.html + solaire-quiz.html)
1b01435  Refactor : Electrolab et Mécanik en pages séparées (multi-pages)
a060848  Fix : pendule visible au 1er affichage + objets du lanceur différenciés
ff05099  Intégration Mécanik dans Electrolab (1 seul site, 2 matières)
```

---

## ✅ Checklist de reprise

Si tu reprends ce projet à froid :

1. [ ] `git status` → vérifier qu'il n'y a pas de modifications non committées
2. [ ] Lancer un serveur local (`python -m http.server 8765`)
3. [ ] Ouvrir http://localhost:8765 et tester :
   - [ ] ⚡ Electrolab : Labo (drag&drop), Comparateur, Quiz Élec, Missions
   - [ ] 🚀 Mécanik (via `+` ou direct mecanique.html) : Lanceur, Atelier Forces, Quiz Méca
   - [ ] 🪐 Système Solaire (via `+` ou direct solaire.html) : orbites, fiches, quiz solaire
4. [ ] Corriger ce qui ne marche pas
5. [ ] Commit + push → GitHub Pages se déploie en 1–2 min
6. [ ] Vérifier la prod : https://haibi.github.io/Electricite-fun/
