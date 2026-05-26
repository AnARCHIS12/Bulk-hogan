# 📦 BulkHogan – Mistral Bulk Processor (Free tier)

**Demo en ligne :** https://liteclaw.web-4.art/

---

## 🎯 But du site

**BulkHogan** est un outil web révolutionnaire conçu pour **envoyer 100 questions d'un coup** à l'API Mistral AI et **automatiser les réponses** de manière entièrement autonome.

### Fini la perte de temps !
Plus besoin de copier-coller chaque question individuellement dans un chatbot. BulkHogan traite automatiquement des listes entières de questions, une par une, avec rotation intelligente des modèles AI, et stocke toutes les réponses localement dans votre navigateur.

---

## ✨ Pourquoi c'est très pratique

| Avant BulkHogan | Avec BulkHogan |
|----------------|----------------|
| ❌ Une question à la fois | ✅ 100+ questions en batch |
| ❌ Copier-coller manuel | ✅ Import automatique depuis fichier |
| ❌ Attendre devant l'écran | ✅ Traitement autonome en arrière-plan |
| ❌ Une réponse générique pour plusieurs questions | ✅ Une réponse précise par question |
| ❌ Perte des historiques | ✅ Stockage local persistant (IndexedDB) |
| ❌ Un seul modèle AI | ✅ Rotation automatique entre 19 modèles Mistral |

### Les avantages du mode "Bulk" (traitement par lots)

1. **Plus besoin d'être devant l'ordinateur** : Lancez le traitement et quittez votre poste. BulkHogan travaille pour vous.

2. **Une réponse unique par question** : Contrairement aux prompts classiques où vous demandez "liste-moi 10 idées" en une seule requête (et obtenez une réponse groupée), ici chaque question reçoit sa propre réponse dédiée, avec le contexte complet et toute l'attention du modèle.

3. **Optimisation des tokens** : Chaque question bénéficie du quota complet de tokens (1024 tokens de réponse configurés), garantissant des réponses détaillées et précises.

4. **Rotation des modèles** : Si un modèle est saturé ou lent, BulkHogan alterne automatiquement entre les modèles disponibles (Codestral, Mistral-Large, Ministral, etc.).

5. **Gestion intelligente des erreurs** : 
   - Détection des doublons (une question déjà traitée est ignorée)
   - Gestion automatique des rate limits (pause de 6 secondes en cas d'erreur 429)
   - Reprise possible après interruption

---

## 📖 Mode d'emploi

### Étape 1 : Obtenir votre clé API Mistral (gratuite)

1. Rendez-vous sur [console.mistral.ai](https://console.mistral.ai/)
2. Créez un compte gratuit (ou connectez-vous)
3. Allez dans la section **"API Keys"**
4. Cliquez sur **"Create new key"**
5. Copiez votre clé (elle ressemble à : `5qaRTjWrezar7y6yy66gbH8Rake`)
6. **Important** : Le free tier offre jusqu'à 2000 appels/mois gratuitement !

### Étape 2 : Configurer BulkHogan

1. **Entrez votre clé API** dans le champ prévu
2. Cliquez sur **"💾 Sauvegarder la clé"** (elle est stockée localement dans votre navigateur)
3. **Sélectionnez les modèles** que vous souhaitez utiliser (cochez les cases)
   - Par défaut, les 3 premiers modèles sont sélectionnés
   - Vous pouvez en cocher plusieurs pour activer la rotation

4. **(Optionnel)** Ajoutez un **pré-prompt système** pour guider les réponses :
   ```
   Exemple : Tu es un assistant précis et concis. Réponds toujours en français, max 100 mots.
   ```

### Étape 3 : Préparer vos questions

Deux méthodes :

**Méthode A – Saisie manuelle :**
```
Quelle est la capitale de la France ?
Explique le théorème de Pythagore
Donne une recette de pancakes
Qui a écrit Les Misérables ?
...
```

**Méthode B – Import de fichier :**
- Créez un fichier `.txt` ou `.csv` avec une question par ligne
- Cliquez sur **"Choisir un fichier"** et sélectionnez-le

### Étape 4 : Lancer le traitement

1. Cliquez sur **"🚀 Lancer le traitement"**
2. Observez la progression :
   - Barre de progression en temps réel
   - Nombre de questions traitées / total
   - Question en cours d'exécution
3. Les réponses s'affichent automatiquement dans le tableau ci-dessous

### Étape 5 : Récupérer vos résultats

- Toutes les réponses sont stockées dans le **tableau d'historique**
- Chaque ligne contient : numéro, question, réponse, modèle utilisé, date
- Vous pouvez **supprimer** des entrées individuellement
- Bouton **"🗑️ Effacer tout"** pour reset complet

---

## ⚠️ Pourquoi ça ne marche pas en local (fichier direct)

### Le problème CORS

Lorsque vous ouvrez `index.html` directement depuis votre disque dur (en double-cliquant dessus, URL en `file://`), les navigateurs bloquent les requêtes vers l'API Mistral pour des raisons de sécurité. C'est la politique **CORS (Cross-Origin Resource Sharing)**.

**Erreur typique :**
```
Access to fetch at 'https://api.mistral.ai/...' from origin 'null' has been blocked by CORS policy.
```

### Solution : Hébergement sur serveur requis

Pour fonctionner, BulkHogan doit être accessible via :
- `http://localhost:8000` (serveur local)
- `https://votre-site.com` (hébergement web)

**Hébergements gratuits recommandés :**
- [GitHub Pages](https://pages.github.com/)
- [Netlify](https://www.netlify.com/)
- [Vercel](https://vercel.com/)
- [Cloudflare Pages](https://pages.cloudflare.com/)

---

## 🔧 Comment faire marcher en local sans serveur ni Python

Si vous voulez absolument utiliser BulkHogan **sans hébergement**, voici les modifications à apporter au code :

### Option 1 : Utiliser un proxy CORS

Ajoutez un proxy CORS gratuit devant l'API Mistral. Modifiez la ligne 602 dans `index.html` :

**Avant :**
```javascript
const response = await fetch('https://api.mistral.ai/v1/chat/completions', {
```

**Après :**
```javascript
const response = await fetch('https://cors-anywhere.herokuapp.com/https://api.mistral.ai/v1/chat/completions', {
```

⚠️ **Attention** : Les proxies publics ne sont pas recommandés pour des clés API sensibles.

### Option 2 : Extension navigateur CORS

Installez une extension qui désactive CORS temporairement :
- Chrome : "Allow CORS: Access-Control-Allow-Origin"
- Firefox : "CORS Everywhere"

**Procédure :**
1. Installez l'extension
2. Activez-la avant d'ouvrir `index.html`
3. Double-cliquez sur le fichier HTML
4. BulkHogan fonctionnera normalement

### Option 3 : Serveur HTTP minimal (1 commande)

**Sur Windows (PowerShell) :**
```powershell
python -m http.server 8000
```

**Sur Mac/Linux :**
```bash
python3 -m http.server 8000
```

Puis ouvrez : `http://localhost:8000`

### Option 4 : Version desktop avec Electron

Transformez BulkHogan en application desktop :

1. Installez Node.js
2. Créez un projet Electron
3. Emballez le HTML dans une fenêtre Electron (pas de restrictions CORS)

---

## 📚 Cas d'usage

### 1. ✍️ Écriture de roman

**Scénario :** Vous écrivez un roman et avez besoin de développer 50 scènes.

**Questions en bulk :**
```
Décris une scène d'ouverture où le héros découvre un mystérieux artefact
Écris un dialogue tendu entre le protagoniste et son mentor
Développe une scène de poursuite dans les rues de Paris la nuit
Crée une scène émotionnelle de réconciliation entre deux personnages
...
```

**Résultat :** 50 scènes détaillées, prêtes à être assemblées ou adaptées.

---

### 2. 📋 Liste de questions / FAQ

**Scénario :** Vous créez une FAQ pour votre produit.

**Workflow intelligent :**

**Étape 1 – Génération de la liste :**
Posez UNE question initiale :
```
Génère-moi une liste de 50 questions fréquentes qu'un client pourrait poser sur un service de livraison de repas à domicile. Une question par ligne, sans numérotation.
```

**Étape 2 – Réinjection :**
- Copiez la liste générée
- Collez-la dans le champ de questions de BulkHogan
- Lancez le traitement en masse

**Résultat :** 50 questions + 50 réponses détaillées pour votre FAQ !

---

### 3. 🎓 Contenu éducatif

- Quiz de révision (100 questions sur un chapitre)
- Fiches de vocabulaire (traductions, définitions)
- Exercices corrigés (maths, physique, langues)

---

### 4. 📊 Analyse de données textuelles

- Sentiment analysis sur 100 avis clients
- Catégorisation de tickets support
- Extraction d'informations depuis des documents

---

## 💡 Idées pour modifier le code et créer d'autres outils

Le code source de BulkHogan est **100% modifiable**. Voici comment le transformer :

### 1. 📖 Outil d'écriture de livre scientifique

**Modifications :**
- Changez le pré-prompt par défaut :
  ```
  Tu es un expert en vulgarisation scientifique. Explique les concepts avec précision, utilise des analogies, cite des sources quand pertinent. Structure en : Introduction → Explication → Exemple → Conclusion.
  ```
- Augmentez `max_tokens` à 2048 pour des réponses plus longues
- Ajoutez un champ "Niveau de complexité" (débutant, intermédiaire, expert)

**Usage :** Générer des chapitres entiers de livres scientifiques.

---

### 2. 📕 Manuel d'utilisation / Documentation technique

**Modifications :**
- Pré-prompt :
  ```
  Tu es un rédacteur technique senior. Produis des instructions claires, étape par étape, avec des avertissements de sécurité quand nécessaire. Format : Titre → Prérequis → Étapes → Dépannage.
  ```
- Ajoutez un upload de screenshot (nécessite backend)
- Export des réponses en Markdown ou PDF

**Usage :** Documentation produit, guides utilisateur, tutoriels.

---

### 3. 🌍 Guide de voyage par pays

**Modifications :**
- Pré-prompt :
  ```
  Tu es un guide de voyage expert. Pour chaque ville/quartier, décris : attractions principales, restaurants recommandés, transports, budget moyen, conseils de sécurité, meilleures saisons.
  ```
- Structurez les questions : "Guide complet de [Ville], [Pays]"

**Usage :** Créer des guides de voyage personnalisés pour 50+ destinations.

---

### 4. 📝 Générateur de contenu SEO

**Modifications :**
- Pré-prompt orienté SEO (mots-clés, structure H1/H2/H3, meta descriptions)
- Ajoutez un champ "Mot-clé principal"
- Export en format WordPress/HTML

**Usage :** Produire 100 articles de blog optimisés SEO.

---

### 5. 🎭 Créateur de personnages pour jeux de rôle

**Modifications :**
- Pré-prompt :
  ```
  Tu es un maître du jeu créatif. Pour chaque personnage, génère : Nom, Âge, Apparence, Personnalité, Background, Compétences, Faiblesses, Objectifs, Secrets.
  ```

**Usage :** Peupler vos campagnes D&D ou univers de fiction.

---

## 🚀 30 autres idées pour monétisation, influenceurs TikTok, activistes humanistes

### 📱 Pour les Influenceurs TikTok / YouTube

| # | Idée | Description |
|---|------|-------------|
| 1 | **Générateur de scripts viraux** | 100 hooks accrocheurs + scripts complets pour vidéos TikTok |
| 2 | **Idées de contenu illimitées** | 365 idées de vidéos classées par catégorie |
| 3 | **Réponses aux commentaires** | Auto-générer des réponses personnalisées à 500+ commentaires |
| 4 | **Descriptions optimisées** | 100 descriptions YouTube avec timestamps et CTAs |
| 5 | **Titres clickbait éthiques** | 50 variantes de titres A/B testés |
| 6 | **Hashtags par niche** | Listes de hashtags trending par catégorie |
| 7 | **Collaboration pitches** | Emails/messages personnalisés pour 100 influenceurs cibles |
| 8 | **Analyse de tendances** | Résumé de 50 trends actuelles avec angles de contenu |
| 9 | **Transcriptions enrichies** | Transformer 20 vidéos en articles de blog |
| 10 | **Calendrier éditorial** | 90 jours de contenu planifié jour par jour |

---

### 💰 Pour la Monétisation / Business en ligne

| # | Idée | Description |
|---|------|-------------|
| 11 | **Fiches produits e-commerce** | 500 fiches produits optimisées SEO + descriptions persuasives |
| 12 | **Emails de relance panier** | 30 variantes d'emails anti-abandon de panier |
| 13 | **Pages de vente** | Structures complètes pour 20 landing pages différentes |
| 14 | **Réponses aux avis clients** | 100 réponses personnalisées (positives et négatives) |
| 15 | **Scripts de webinaire** | Webinaires complets de 45min sur 10 sujets différents |
| 16 | **Offres promotionnelles** | 50 idées de promos avec copywriting inclus |
| 17 | **FAQ sectorielles** | FAQ complètes pour 10 niches différentes |
| 18 | **Guides d'achat comparatifs** | 20 guides "Meilleur X pour Y" avec tableaux comparatifs |
| 19 | **Posts LinkedIn B2B** | 100 posts professionnels engageants |
| 20 | **Pitch decks investisseurs** | Structures narratives pour 10 types de startups |

---

### 🌍 Pour les Activistes Humanistes / ONG

| # | Idée | Description |
|---|------|-------------|
| 21 | **Campagnes de sensibilisation** | 50 messages percutants sur le changement climatique |
| 22 | **Lettres aux élus** | Modèles personnalisables pour 30 représentants politiques |
| 23 | **Dossiers de presse** | Communiqués pour 20 causes différentes |
| 24 | **Posts réseaux sociaux engagés** | 100 posts Instagram/Twitter avec visuels décrits |
| 25 | **Scripts de vidéos virales** | 20 scénarios de vidéos choc pour sensibiliser |
| 26 | **Arguments de débat** | 50 arguments sourcés pour défendre une cause |
| 27 | **Newsletters donateurs** | 12 newsletters mensuelles pour fidéliser les donateurs |
| 28 | **Fiches pédagogiques** | Matériel éducatif pour 30 ateliers de sensibilisation |
| 29 | **Appels aux dons** | 30 variantes d'appels émotionnels et rationnels |
| 30 | **Partenariats entreprises** | Proposals personnalisés pour 50 entreprises cibles |

---

### 🎁 Bonus : Idées transverses

| # | Idée | Public cible |
|---|------|--------------|
| 31 | **Générateur de noms de marque** | Entrepreneurs |
| 32 | **Créateur de slogans publicitaires** | Marketeurs |
| 33 | **Rédacteur de CV et lettres de motivation** | Demandeurs d'emploi |
| 34 | **Coach de développement personnel** | Particuliers |
| 35 | **Générateur de recettes de cuisine** | Food bloggers |
| 36 | **Créateur d'histoires pour enfants** | Parents/enseignants |
| 37 | **Analyste de contrats juridiques** | Avocats/PME |
| 38 | **Traducteur contextuel** | Voyageurs/entreprises |
| 39 | **Générateur de code commenté** | Développeurs juniors |
| 40 | **Créateur de quiz interactifs** | Formateurs/enseignants |

---

## 🛠️ Personnalisation avancée du code

### Modifier la rotation des modèles

Dans la section `VALID_MODELS` (ligne 269), vous pouvez :
- Ajouter de nouveaux modèles Mistral
- Retirer ceux qui ne vous intéressent pas
- Changer l'ordre de priorité

### Ajuster les paramètres de requête

Lignes 608-613 :
```javascript
{
    model: model,
    messages: messages,
    temperature: 0.7,      // Créativité (0 = précis, 1 = créatif)
    max_tokens: 1024       // Longueur max de réponse
}
```

**Recommandations :**
- `temperature: 0.3` → Réponses factuelles, techniques
- `temperature: 0.7` → Équilibré (défaut)
- `temperature: 0.9` → Créatif, stories, brainstorming
- `max_tokens: 2048` → Pour des réponses très longues

### Ajouter un export des résultats

Actuellement les données sont dans IndexedDB. Pour exporter en CSV, ajoutez cette fonction :

```javascript
function exportToCSV() {
    getAllResponses().then(items => {
        let csv = 'Question,Réponse,Modèle,Date\n';
        items.forEach(item => {
            csv += `"${item.question.replace(/"/g, '""')}","${item.answer.replace(/"/g, '""')}",${item.model},${item.date}\n`;
        });
        const blob = new Blob([csv], { type: 'text/csv' });
        const url = URL.createObjectURL(blob);
        const a = document.createElement('a');
        a.href = url;
        a.download = 'bulkhogan_export.csv';
        a.click();
    });
}
```

---

## 📞 Support & Contribution

- **Demo :** https://liteclaw.web-4.art/
- **Licence :** Code ouvert, modifiable à volonté
- **API Mistral :** https://mistral.ai/

---

## ⚡ En résumé

**BulkHogan** transforme la façon dont vous interagissez avec l'IA :
- ✅ **Gain de temps massif** : Traitez 100 questions en quelques minutes
- ✅ **Qualité supérieure** : Une réponse dédiée par question
- ✅ **Autonomie totale** : Lancez et oubliez
- ✅ **Gratuit** : Utilise le free tier de Mistral (2000 appels/mois)
- ✅ **Flexible** : Adaptable à tous vos besoins

**Prêt à booster votre productivité ?** Copiez ce code, hébergez-le, et commencez à traiter vos questions en masse dès aujourd'hui ! 🚀

---

*Développé avec ❤️ pour la communauté*
