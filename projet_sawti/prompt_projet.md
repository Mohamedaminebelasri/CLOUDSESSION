# Contexte
Je suis étudiant en 4e année ingénieur (filière IA, ENSAM Meknès). Avec mon binôme,
nous réalisons pendant UN SEMESTRE (~16 semaines) le projet d'initiation suivant.
Ressources : 2 personnes, GPU gratuits uniquement (Kaggle et Google Colab, T4/P100).
Objectif secondaire : un projet de qualité professionnelle, publiable sur GitHub
et Hugging Face, qui valorise notre CV d'ingénieur IA.

# Fiche officielle du projet (fournie par le prof)
- Titre : Sawti-ASR-FineTuner & Data-Augmenter (Pipeline ASR & MLOps Audio)
- Thème : Plateforme Sawti (Continuité, Arabizi et Code-Switching)
- Encadrants : Pr. Hosni, Pr. Menaoui
- Contexte : L'adaptation des modèles acoustiques fondationnels aux spécificités de la
  Darija marocaine et au code-switching (français-arabe) requiert une chaîne MLOps
  robuste, capable d'appliquer des techniques d'augmentation de données synthétiques
  pour surmonter la rareté des corpus annotés dans des conditions acoustiques réelles.
- Travail demandé : Concevoir un pipeline MLOps automatisé de réentraînement et
  d'augmentation audio pour modèles ASR :
  1. Augmentation spectrale et acoustique (SpecAugment, injection de bruits,
     variations de pitch/vitesse)
  2. Fine-tuning efficace de modèles acoustiques via LoRA/PEFT
  3. Évaluation de la robustesse face au code-switching
- Baseline à battre : full fine-tuning standard du modèle ASR, sans augmentation
  de données ni PEFT.
- Données : corpus audio Sawti enrichis par Atlas IA + bruits ESC-50 / NOISEX.
- Stack : PyTorch, Hugging Face Transformers, Audiomentations, Librosa, MLflow.
- Livrables : pipeline de fine-tuning et d'augmentation MLOps documenté, avec
  registre de modèles.
- Métrique : maximiser le gain relatif de WER sur des enregistrements bruités et
  en situation de code-switching.

# Contraintes et points à vérifier
- GPU Kaggle/Colab : sessions limitées → modèles Whisper tiny/base/small, fp16,
  gradient checkpointing, checkpoints fréquents pour reprendre après coupure.
  (Les quotas exacts de Kaggle/Colab sont à vérifier.)
- Le volume réel de données Darija annotées (Sawti / Atlas IA) est encore INCONNU :
  prévoir un plan B avec des datasets publics de Darija ou d'arabe dialectal sur
  Hugging Face (à identifier et vérifier, ne pas inventer de noms).
- La baseline full fine-tuning peut être trop lourde pour un T4 : proposer une
  version comparable et faisable (modèle plus petit, sous-ensemble des données).

# Répartition du binôme
- Personne A : données (collecte, nettoyage, split train/val/test), pipeline
  d'augmentation, scripts d'évaluation WER/CER (global, bruité, code-switching).
- Personne B : fine-tuning LoRA/PEFT, baseline, suivi MLflow, registre de modèles,
  automatisation du pipeline de réentraînement.

# Ce que j'attends de toi
1. Une architecture globale du projet (schéma des modules et flux de données).
2. Une structure de dépôt GitHub propre (dossiers, configs, scripts, notebooks,
   README, requirements).
3. Un planning sur 16 semaines avec jalons et livrables, réparti entre A et B.
4. Pour chaque module, les choix techniques justifiés et le code de départ :
   - chargement et préparation des données (format HF datasets)
   - augmentation (Audiomentations + SpecAugment) avec niveaux de bruit (SNR)
   - baseline : Whisper zero-shot + full fine-tuning
   - fine-tuning LoRA (PEFT) : modules ciblés, rang, hyperparamètres de départ
   - évaluation : WER/CER sur 3 jeux de test (propre, bruité, code-switching),
     gain relatif de WER vs baseline
   - MLflow : tracking des expériences + Model Registry
5. Un plan d'expériences (ablations : sans/avec augmentation, LoRA vs full FT,
   différents rangs LoRA, différents SNR) et le format du tableau de résultats.
6. Les risques principaux et leurs solutions de repli.
7. Ce qu'il faut publier à la fin (modèle sur Hugging Face, README, démo Gradio)
   pour maximiser la valeur CV.

Procède étape par étape : commence par les points 1 à 3, puis attends ma validation
avant de générer le code des modules. Si une information te manque ou si tu n'es
pas sûr, dis-le explicitement au lieu de deviner.
