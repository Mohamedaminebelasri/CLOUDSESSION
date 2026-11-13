# Contexte du projet Sawti-ASR (passation entre sessions Claude)

Lire ce fichier en entier avant de commencer. Il résume les décisions déjà prises.

## Fichiers de référence (dossier `projet_sawti/`)
- `prompt_projet.md` : description complète du projet (fiche du prof, contraintes,
  répartition du binôme, attentes). C'est la demande d'origine.
- `cahier_des_charges.tex` / `.pdf` : cahier des charges terminé (23 pages, LaTeX,
  plan en 8 parties imposé par le prof). Il fait foi pour les choix ci-dessous.

## Équipe
- Mohamed Amine Belasri et Yahya Amajane (ENSAM Meknès, filière IA, 4e année).
- Encadrants : Pr. Hosni, Pr. Menaoui.
- Rôles « Personne A » (données, augmentation, évaluation) et « Personne B »
  (fine-tuning, baseline, MLflow, automatisation) : pas encore attribués.

## Décisions prises
- **Données** : le corpus Sawti n'est pas public (volume à confirmer avec les
  encadrants). Plan B : datasets publics Atlas IA sur Hugging Face
  (`atlasia/DODa-audio-dataset`, MoulSot) et Casablanca (arXiv 2410.04527) pour
  le test en code-switching. Bruits : ESC-50 et NOISEX-92. Chiffres de MoulSot
  (~1 500 h dont ~80 h transcrites) annoncés par les auteurs, NON vérifiés.
  Licences et accès à vérifier.
- **Jeux de test** : propre, bruité (SNR 20/10/5/0 dB, bruits jamais vus à
  l'entraînement), code-switching.
- **Modèle** : Whisper tiny/base/small (GPU T4/P100 gratuits), code de langue
  arabe, fp16, gradient checkpointing, checkpoints toutes les 200 étapes.
- **Baseline** : Whisper zero-shot + full fine-tuning sans augmentation (version
  réduite si trop lourd pour un T4).
- **LoRA de départ** : q_proj, v_proj ; r = 16 (tests 8 et 32) ; alpha 32 ;
  dropout 0,05 ; lr 1e-3 (full FT : 1e-5) ; batch effectif 16.
- **Augmentation** : Audiomentations (bruit à SNR contrôlé, pitch ±2 demi-tons,
  vitesse 0,9–1,1) + SpecAugment.
- **Objectifs** : gain relatif de WER ≥ 10 % (bruité), ≥ 5 % (code-switching),
  perte ≤ 1 point de WER sur le propre. À ajuster après la baseline.
- **Expériences** : E0 zero-shot, E1 full FT sans aug (baseline), E2 full FT +
  aug, E3 LoRA sans aug, E4 LoRA + aug (r = 8/16/32), E5 variations de SNR.
- **Interface** : AUCUNE (retirée à la demande des étudiants). Utilisation par
  scripts + notebooks Kaggle/Colab, résultats dans MLflow.
- **Planning** : S1 à S16 (pas de dates). Voir la partie 7 du cahier des charges.

## Préférences de l'utilisateur (important)
- Répondre en français, simplement, avec des réponses courtes.
- Indiquer un niveau de confiance au début de chaque réponse.
- Ne jamais deviner : dire clairement quand une information est inconnue.
- Prévenir avant une tâche qui consomme beaucoup de tokens et attendre l'accord.
- Avancer étape par étape et attendre la validation entre les étapes.

## Prochaine étape demandée
Points 1 à 3 de `prompt_projet.md` : architecture globale, structure du dépôt
GitHub, planning 16 semaines. Puis, après validation, le code des modules.
