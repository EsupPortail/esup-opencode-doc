# esup-opencode-doc
# Installer opencode avec ILaaS et Albert API

Cette documentation fait suite à l'atelier IA OpenCode animé par Morgan B de l'université de Strasbourg dont voici le replay : https://videos.esup-portail.org/video/1973-atelier-ia-tech-9-utiliser-un-agent-de-code-en-local/

Pour reproduire l'atelier sur votre ordinateur, il vous faut une clé d'accès à un LLM (un modèle de langage). C'est elle qui permet à l'agent de code d'interroger le modèle. Dans l'enseignement supérieur, vous avez deux possibilités, et vous pouvez utiliser l'une, l'autre ou les deux :

- ILaaS : le service de l'enseignement supérieur et de la recherche, accessible avec votre compte universitaire ;
- Albert API : le service d'IA de l'État, géré par la DINUM.

Vous trouverez ci‑dessous les étapes pour obtenir votre clé, installer l'agent de code opencode et le configurer. Le fichier de configuration joint à ce mail (opencode.json) contient déjà les deux services, ainsi que des règles de sécurité.

## ⚠️ Règle n°1, avant tout

Lancez toujours opencode depuis le dossier de votre projet. Ne le lancez jamais depuis la racine de votre dossier personnel (~), ni depuis un projet Git sur lequel vous travaillez depuis 6 mois 😉

opencode peut lire, modifier et supprimer tout ce qui se trouve dans le dossier où vous le lancez. Pour vos premiers essais, créez un dossier de test. Pour un vrai projet, faites un commit ou une sauvegarde avant de commencer.

## 1. Récupérer votre clé
Il vous faut au moins une des deux clés :

- ILaaS : demandez votre clé individuelle avec votre compte universitaire sur https://www.ilaas.fr/demande-de-cles-ilaas/
- Albert API : connectez‑vous sur https://albert.playground.etalab.gouv.fr/keys, créez une nouvelle clé et copiez‑la tout de suite dans un endroit sûr.

_Vos clés sont personnelles : ne les partagez pas._

## 2. Installer opencode
Commandes officielles de https://opencode.ai :

- Mac / Linux :
`curl -fsSL https://opencode.ai/v2/install | bash`

- Mac avec Homebrew :
`brew install anomalyco/tap/opencode-v2`

- Windows (ou toute machine avec Node.js) :
`npm install -g @opencode/cli`

## 3. Mettre en place le fichier de configuration
Enregistrez le fichier opencode.json joint dans ce dossier (créez le dossier opencode s'il n'existe pas) :

- Mac / Linux : `~/.config/opencode/opencode.json`
- Windows : `C:\Users\<votre nom>\.config\opencode\opencode.json`

### Le modèle utilisé par défaut
C'est Mistral Small 4 119B (ILaaS). Si vous n'avez qu'une clé Albert, ou si vous préférez Albert API, ouvrez le fichier et remplacez la ligne :

`"model": "ilaas/mistral-small-4-119b",`

par :

`"model": "albert/qwen3-coder-30b-a3b-instruct",`

C'est un modèle d'Albert spécialisé dans le code. Vous pouvez aussi changer de modèle à tout moment dans opencode avec /models.

### Les règles de sécurité
Par défaut, opencode s'autorise tout, y compris à sortir du dossier du projet. Les règles du fichier changent ce comportement :

- opencode vous demande votre accord avant de lancer une commande, de modifier un fichier ou d'aller sur Internet ;
- il peut lancer sans demander quelques commandes qui ne font que lire (ls, git status, git diff, git log) ;
- il ne peut pas :
  - lire ou modifier des fichiers en dehors du dossier du projet ;
  - lire vos fichiers .env, qui contiennent souvent des mots de passe ;
  - lancer sudo, rm -rf, git push, git reset --hard ou d'autres commandes destructrices.

**Lisez vraiment chaque demande avant d'accepter** : ces règles sont un filet de sécurité, pas une garantie.

## 4. Enregistrer vos clés dans opencode
Dans le terminal, tapez :
'opencode auth login'

Pour une clé ILaaS : tapez 'ilaas' pour filtrer la liste, sélectionnez ILAAS, puis collez votre clé.
Pour une clé Albert : tapez 'albert', sélectionnez Albert API, puis collez votre clé.
Si vous avez les deux clés, lancez la commande deux fois.

## 5. Lancer opencode
```
cd chemin/vers/votre/projet
opencode
```

## En cas de problème
- opencode ne répond pas à vos messages : fermez toutes les fenêtres opencode, tapez `pkill -f "opencode serve"`, relancez opencode, puis tapez `/new` pour ouvrir une nouvelle session.
- Un modèle est refusé : votre clé n'y a peut‑être pas accès. Choisissez‑en un autre avec /models.
- Ne modifiez pas les lignes "reasoning": false et "variants" des modèles ILaaS : sans elles, opencode reçoit des réponses vides.
