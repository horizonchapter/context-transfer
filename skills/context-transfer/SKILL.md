---
name: "context-transfer"
description: "Passation complète vers un nouveau chat, projet ou session Cowork : transfère tout le contexte sans perte. À utiliser sur « context transfer », « passation », « relais », « fin de chat », « nouveau chat »."
---

# Context transfer (relais complet)

But : le prochain Claude repart UNIQUEMENT de ce que tu écris maintenant. Il n'aura ni cette conversation, ni la mémoire, ni la liste de tâches. Il doit reprendre exactement là où on s'est arrêté, sans refaire ce qui est fait, sans rien sauter, sans reposer de question. Transfert complet : on compacte, on ne perd rien. N'avance PAS la tâche pendant la passation : écris seulement la passation.

Inspiré du relais du mod Jauge+ (dépôt mods-claude-code), en version skill utilisable partout : chat, projet, Cowork, Claude Code.

## Déclencheurs
« context transfer », « passation », « relais », « résumé pour un nouveau chat », « fin de chat », « reprise ». Proposer en une ligne quand la conversation devient très longue, change de sujet ou approche d'une limite. Pour recevoir une passation : voir « Mode reprise ».

## Choisir le mode
- **Mode fichiers** (Cowork ou Claude Code avec dossier de projet accessible, ou si l'utilisateur le demande) : écrire STATE.md et DECISIONS.md dans le dossier du projet.
- **Mode bloc** (chat sans accès fichiers) : produire le même contenu dans UN bloc de code à copier-coller. Mêmes sections, même exigence.

## Règles de fidélité (non négociables)
1. Reprends à l'identique : chiffres, noms, liens, chemins exacts, commandes, code utile, textes validés, et les mots exacts de l'utilisateur entre guillemets pour ses demandes et consignes.
2. Rien d'important ne disparaît : demandes ouvertes, décisions + pourquoi, choix écartés + pourquoi (pour ne pas les reproposer), pièges + contournement.
3. Sépare : DÉCIDÉ par l'utilisateur / PROPOSÉ par moi non validé / HYPOTHÈSE.
4. Étiquette chaque chiffre important : VÉRIFIÉ (source datée) / ESTIMÉ / HYPOTHÈSE / INCONNU.
5. Supprime seulement : politesses, répétitions, essais ratés sans enseignement, explications déjà comprises, ancien historique déjà archivé.
6. Jamais de clé, mot de passe, jeton, .env, numéro de carte : écris « [secret retiré] » et où le retrouver.
7. Léger mais complet : environ 150 lignes au plus (180 = plafond dur, 24 000 caractères), phrases courtes, chemins plutôt que contenu, pas de code recopié, pas de répétition entre sections, minimum utile 600 caractères. Si le CLAUDE.md du projet impose une limite plus stricte, signale le conflit à l'utilisateur au lieu de perdre du contenu.

## Mode fichiers : procédure
1. **Sauvegarde d'abord** : copie l'ancien STATE.md et DECISIONS.md dans `.passations/AAAA-MM-JJ_HHhMM/` (STATE-avant.md, DECISIONS-avant.md). Ne jamais écraser une sauvegarde existante. Si STATE.md n'existe pas, crée-le.
2. **Réécris STATE.md** (on remplace, on n'empile) dans cet ordre exact :
   - `## Reprise exacte` (le plus important, sois précis)
     - Tâche en cours : ce qu'on faisait et pour quelle demande.
     - Plan : étapes une par ligne, [x] faite, [>] en cours, [ ] à faire (recopie la liste de tâches si elle existe).
     - Dernière action : la toute dernière chose faite (fichier modifié, commande) et son résultat exact (réussite, ou erreur et son message).
     - Fichiers touchés : chemin exact — ce qui y est fait — ce qui reste à y faire. Signale tout fichier laissé à moitié modifié.
     - Prochaine action : UNE action concrète (fichier, commande), résultat attendu, comment vérifier. Puis les deux suivantes.
     - Ne pas refaire : ce qui est déjà fait et vérifié.
   - `## Objectif` : le but du projet et de cette session, 2 à 3 phrases.
   - `## Où on en est` : 3 lignes (ce qui marche, ce qui ne marche pas encore, ce qui bloque).
   - `## Fait` : ce qui sert encore pour la suite, avec la preuve (test passé, fichier créé).
   - `## Demandes de l'utilisateur` : demandes encore ouvertes avec ses mots exacts entre guillemets ; ses préférences et sa façon de travailler.
   - `## Décidé` : chaque décision avec son pourquoi + ce qui a été écarté et pourquoi.
   - `## Pièges déjà rencontrés` : erreurs, impasses, contournement trouvé.
   - `## Fichiers, liens et chiffres clés` : chemins, liens, commandes utiles ; chiffres étiquetés.
   - `## Outils et connecteurs` : ce qui a été fait ou reste à faire dans chaque outil (Notion, Shopify, Drive…), sans identifiants.
   - `## Attend une validation` : tout ce qui ne doit pas se faire sans l'accord explicite de l'utilisateur (publication, envoi, paiement, suppression, push, déploiement, hausse de budget pub).
   - `## Questions ouvertes`
   Garde ce qui reste vrai de l'ancien STATE.md, corrige ce qui a changé, retire l'obsolète. Aucune section vide.
3. **DECISIONS.md** : ajoute à la fin (jamais d'effacement ; crée-le si absent) une ligne par décision de la conversation : `AAAA-MM-JJ — décision — pourquoi`.
4. **Contrôle mécanique** : toutes les sections ci-dessus présentes et non vides ; taille dans les limites ; aucun secret. Corrige avant de continuer.
5. **Contrôle à froid** : fais lire STATE.md et la fin de DECISIONS.md (40 dernières lignes) par un lecteur NEUF qui n'a rien vu (sous-agent via l'outil Agent si disponible, sinon relis-les toi-même en jouant strictement le prochain Claude). Il répond exactement :
   - `<compris>` 3 lignes : où on en est, ce qui vient d'être fait, la prochaine action exacte `</compris>`
   - `<manques>` ce qui l'empêcherait de reprendre sans se tromper (question à poser avant d'agir, contradiction, fichier ou étape flou, décision sans pourquoi), ou `AUCUN` `</manques>`
   Ne signale que ce qui gêne vraiment la reprise, pas le style.
   Si des manques : corrige STATE.md à la bonne section sans rallonger le reste, puis recontrôle. Maximum 2 tours de complément. Au-delà, ajoute à STATE.md une section `## Points flous signalés au relais` (liste des manques restants, « à vérifier dans les fichiers ou à demander à l'utilisateur ») et passe quand même le relais.
6. **Archive** : enregistre dans le dossier `.passations/...` : controle.md (blocs « ## Contrôle N » avec compris + manques), puis STATE-apres.md et DECISIONS-apres.md (copies finales). Seul le DERNIER bloc de controle.md doit être cohérent avec le STATE.md final. Si un des 5 fichiers manque, c'est un échec : dis-le, ne relance pas seul.
7. **Ne modifie aucun autre fichier**, ne lance rien d'autre. N'écris PAS la ligne de journal de fin de session pendant le relais.
8. Termine par exactement : `Relais prêt`, puis une ligne : où sont les fichiers et comment reprendre (nouvelle session dans le même dossier, ou nouveau chat dans le même projet).

## Mode bloc : procédure
Produis dans UN seul bloc de code les mêmes sections que STATE.md (ordre identique), plus une section finale `## Décisions à ajouter à DECISIONS.md` (lignes `AAAA-MM-JJ — décision — pourquoi`). Fais le contrôle mécanique et le contrôle à froid (en jouant le prochain Claude) AVANT de livrer, et corrige. Termine le bloc par :

```
## Consigne pour le nouveau chat
Lis cette passation en entier, applique le mode reprise du skill context-transfer, résume-la en 3 lignes pour confirmer, puis continue à « Prochaine action ». Ne refais pas « Ne pas refaire », ne redemande pas ce qui est décidé ici.
```

Puis une ligne : ouvre un nouveau chat dans le même projet (instructions et fichiers conservés), colle le bloc, rejoins à nouveau les fichiers cités s'ils ne sont pas dans le projet. Termine par `Relais prêt`.

## Mode reprise (nouveau chat ou session qui reçoit une passation)
La conversation précédente a été effacée volontairement ; tout est dans STATE.md / le bloc.
1. Lis STATE.md en entier (ou le bloc), puis la fin de DECISIONS.md.
2. Avant de modifier quoi que ce soit, vérifie l'état réel : les « Fichiers touchés » existent et sont dans l'état décrit (relis les passages concernés) ; relance la vérification indiquée seulement si elle n'a aucun effet de bord. Si l'état réel contredit STATE.md, dis-le et fie-toi à l'état réel.
3. S'il y a un plan, recrée la liste de tâches à l'identique (faites cochées, étape en cours en cours).
4. Écris 3 lignes : où on en est, ce qui vient d'être fait, la prochaine action exacte. Traite d'abord les « Points flous signalés au relais » s'il y en a.
5. Continue à « Prochaine action », sans refaire « Ne pas refaire ». Si l'action figure dans « Attend une validation » : demande l'accord avant.
6. Si STATE.md contredit ce que dit l'utilisateur, signale-le.

## Rappels
- Règle du bouton : tu prépares, l'utilisateur valide. La passation ne déclenche aucune publication, envoi, paiement ni suppression.
- Un échec ou un écart : le dire à l'utilisateur, ne pas relancer le relais tout seul.

## Si l'utilisateur dit « fin de session »
Après le relais, ajoute une ligne à son journal de bord (fichier ou base Notion) s'il en tient un, comme le prévoit son CLAUDE.md.
