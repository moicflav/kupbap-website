# CLAUDE.md — Site Web KUPBAP

## En début de session
- **Invoquer le skill `frontend-design`** avant toute modification ou création de code frontend.

## Serveur local
- URL : `http://localhost:3000`
- Lancer avec : `node serve.mjs` depuis le dossier `Site web Kupbap creation/`
- Flavien partage ses captures d'écran manuellement — pas de screenshot automatique.

## Stack
- HTML statique (3 pages : `index.html`, `menu.html`, `contact.html`)
- CSS et JS écrits directement dans chaque fichier HTML (balises `<style>` et `<script>`)
- Tailwind CSS via CDN + CSS custom pour les composants spécifiques
- Polices : Intro Rust (self-hosted WOFF2, titres) + DM Sans (Google Fonts, corps)

## Brand Assets — toujours vérifier avant de coder
- **Logo :** `Kupbap_brand_assets/Logo-Kupbap-1 (1).png`
- **Police display :** `Kupbap_brand_assets/Fonts/IntroRust.woff2`
- **Images menu :** `Kupbap_brand_assets/images_menu/`
- **Bannières hero :** `Kupbap_brand_assets/Bannière/`
- **Photos Koreo :** `Kupbap_brand_assets/koreo/`
- Ne jamais utiliser de placeholders si un asset réel existe.

## Hiérarchie des couleurs
Les couleurs **primaires** (doivent dominer visuellement) :
- Bleu `#12448E` — sections de fond, overlays, CTA principal
- Rouge `#FF3131` — accents, mots clés, bouton commander
- Crème `#F8F4E3` — fond des sections contenu (menu, avis…)

Les couleurs **secondaires** (viennent en support, jamais dominantes) :
- Blanc — cartes sur fond crème, éléments flottants
- Noir chaud `#1A1A2E` — sections de transition, texte
- Or `#E8A317` — prix, étoiles, détails décoratifs

**Règle de fond des sections (dans l'ordre sur la page) :**
Hero (bleu) → Menu (crème) → Avis (crème) → Kpop (noir) → Infos Pratiques (bleu) → Footer (noir profond)

## Règles de code
- N'animer que `transform` et `opacity` — jamais `transition-all`
- Chaque élément cliquable doit avoir un état hover
- Ne pas ajouter de sections ou fonctionnalités non demandées par Flavien

## Principe responsive & spacing — OBLIGATOIRE sur toutes les pages
**Le contenu prime. Les sections s'adaptent. L'espacement est un système cohérent, pas des valeurs inventées au cas par cas.**

### Layout
1. **Jamais de `height` fixe sur une section** — toujours `min-height` ou `height: auto`.
2. **`min-height: 100vh; min-height: 100svh;`** pour les sections plein écran (ordre obligatoire : `100vh` fallback, `100svh` en second).
3. **Aucun élément hors champ** à aucun breakpoint. La section grandit ou le layout se réorganise.
4. **Layouts absolus → fallback grille CSS** sous les breakpoints critiques.
5. **Boutons en colonne sur mobile** à ≤768px — `flex-direction: column; align-items: stretch`.
6. **Jamais de `overflow: hidden` sur une section** sauf besoin explicite (clipper une animation). Préférer `overflow-x: hidden` sur le `body`.

### Système de spacing (échelle unique pour tout le site)
7. **Tous les paddings et gaps référencent la même échelle.** Ne jamais inventer une valeur de padding arbitraire. Si une valeur n'existe pas dans l'échelle, utiliser la plus proche :
   ```css
   --space-sm:  clamp(1.25rem, 2.5vh, 2rem);    /* gaps internes, marges entre éléments */
   --space-md:  clamp(2rem,    4vh,   3rem);     /* espacement moyen, padding horizontal mobile */
   --space-lg:  clamp(3rem,    6vh,   5rem);     /* padding vertical de section standard */
   --space-xl:  clamp(4.5rem,  9vh,   7rem);     /* padding vertical de grande section */
   ```
8. **Le spacing ne change pas quand le contenu change.** Si un texte ou un bouton est ajouté/supprimé, les paddings restent — la section grandit automatiquement. Ne jamais réduire le padding pour "compenser" du contenu ajouté.
9. **Rythme homogène sur toute la page** — les espacements entre label → titre → sous-titre → boutons doivent être cohérents d'une section à l'autre. Utiliser les mêmes tokens partout.
10. **`clamp()` pour tous les `font-size`** — `clamp(min, fluid-vw, max)`.

### Symétrie navbar (spécifique KUPBAP — hauteur navbar ≈ 4rem)
11. Le gap visuel entre le bas de la navbar et le premier texte = le gap entre le dernier élément et le bas de la section :
    ```css
    --section-gap: clamp(2rem, 5vh, 3.5rem);
    padding-top: calc(4rem + var(--section-gap));
    padding-bottom: var(--section-gap);
    ```
    En paysage mobile (`orientation: landscape` + `max-height: 500px`) : `--section-gap: 1.25rem`.

## Uniformité des boutons (OBLIGATOIRE)
Tous les boutons du site, toutes pages confondues, doivent être **strictement uniformes**. Référence de chaque type :

- **Bouton principal** `.btn-primary` — fond rouge `#FF3131`, texte blanc, `border-radius: 4px`, police DM Sans 600, `font-size: 0.88rem`, `letter-spacing: 0.05em`, `text-transform: uppercase`
- **Bouton ghost** `.btn-ghost` — fond transparent, bordure `rgba(255,255,255,0.35)`, texte blanc, mêmes dimensions que `.btn-primary`
- **Bouton navbar Commander** `.btn-nav` — fond rouge `#FF3131`, `border-radius: 8px`, DM Sans 600, `font-size: 0.78rem`
- **Bouton navbar Réserver** `.btn-nav-outline` — transparent, bordure blanche/bleue selon fond, mêmes dimensions que `.btn-nav`
- **Bouton bleu plein** (contact card) — fond `var(--blue)`, texte blanc, `border-radius: 8px`, DM Sans 600

Règles :
- Ne jamais créer un nouveau style de bouton sans vérifier qu'un existant ne convient pas
- Toute modification d'un type de bouton doit être répercutée sur **toutes les pages**
- Pas de `inline style` pour les boutons — utiliser les classes existantes

## Page de référence
- **`index.html` est la page de référence** pour tout le site — c'est toujours la base depuis laquelle partir pour lire les styles, composants et comportements existants
- Pour toute modification ou création : lire `index.html` en premier, puis propager vers `menu.html` et `contact.html` si nécessaire

## Synchronisation navbar & footer (OBLIGATOIRE)
- Toute modification sur la navbar ou le footer (couleurs, effets, taille de police, espacement, liens, comportement scroll, etc.) doit être **immédiatement répercutée sur toutes les pages** : `index.html`, `menu.html`, `contact.html`
- Ne jamais modifier la navbar ou le footer d'une seule page sans mettre les autres à jour dans le même échange
- Pages actives : `index.html`, `menu.html`, `contact.html` (3 pages — `notre-histoire.html` supprimée)

## Déploiement
- **Hébergement : Vercel** (statique, gratuit, HTTPS, CDN mondial)
- **Domaine :** `kupbap.fr` (DNS pointés vers Vercel)
- **Branche de production : `main`** — chaque push sur `main` déclenche un déploiement automatique en production

## Workflow de modification (après mise en ligne)
- **Toujours travailler sur la branche `dev`**, jamais directement sur `main`
- Vercel génère automatiquement une URL de prévisualisation pour chaque push sur `dev` (ex: `kupbap-dev.vercel.app`)
- Flavien valide visuellement sur l'URL de prévisualisation
- Une fois validé : merge `dev` → `main` → déploiement en production
- **Exception :** si Flavien demande explicitement un déploiement direct en production, on peut pusher sur `main` directement

## Checklist avant premier déploiement Vercel
- [ ] Supprimer anciens dossiers `Menu-ete-2026-francais/` et `Menu-ete-2026-anglais/`
- [ ] Convertir images PNG de `images_menu/` en WebP
- [ ] Renommer fichiers avec espaces/accents (ex: `Logo-Kupbap-1 (1).png`)
- [ ] Retirer `serve.mjs` du repo
