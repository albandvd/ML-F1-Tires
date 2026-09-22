# Parcours d'apprentissage : projet MLOps F1

Ce document liste les technos à apprendre pour mener notre projet MLOps sur la Formule 1 : un modèle de dégradation des pneus (v1), puis un simulateur d'undercut (v2). Pour chaque techno, il précise pourquoi elle est utile, le niveau à viser, qui mène, le temps estimé et où l'apprendre.

## Le principe : apprendre juste à temps

On apprend chaque techno **juste avant la soirée où on en a besoin**, pas des semaines à l'avance.

**Pourquoi :** ce qu'on apprend sans le pratiquer s'oublie vite. Apprendre la veille d'une soirée où on va s'en servir, c'est retenir en appliquant tout de suite. Cela évite aussi de s'épuiser sur Kubernetes alors qu'on n'a pas encore de modèle à déployer.

### Niveaux visés

Le code couleur reprend celui des gommes Pirelli :

| Niveau | Signification | Pourquoi cette distinction |
|---|---|---|
| 🔴 **À maîtriser** | On s'en sert sans arrêt, il faut être autonome. | Ce sont les fondations : une lacune ici ralentit toutes les soirées suivantes. |
| 🟡 **Savoir utiliser** | On s'en sert avec la documentation ouverte à côté. | C'est le niveau réaliste pour la plupart des outils : personne ne connaît MLflow ou Argo CD par cœur. |
| ⚪ **Survoler** | Savoir que ça existe et à quoi ça sert. | Certains outils servent une fois : y passer des heures serait du temps perdu. |

### Qui mène

Chaque techno a un **profil qui mène** : il l'apprend en profondeur et prend la tâche correspondante. L'autre profil l'apprend assez pour **relire la pull request** : c'est ce que compte la colonne de l'autre profil dans les tableaux.

**Pourquoi :** on avance plus vite en se répartissant l'effort, mais relire la PR de l'autre garantit que chacun comprend tout le projet. En entretien, les deux doivent pouvoir expliquer l'ensemble de la chaîne.

## Par quoi commencer

- [ ] **Tous les deux : pandas, puis les bases du machine learning.** Kaggle Learn, parcours « Pandas » puis « Intro to Machine Learning », environ 8 heures.
  *Pourquoi :* c'est notre lacune commune, et le bloc machine learning commence dès la soirée 4. Commencer maintenant évite d'arriver à cette soirée sans savoir ce qu'est un jeu de test.
- [ ] **Profil DevOps : l'outillage de la soirée 1.** uv, Ruff, pre-commit, Makefile, CI GitHub Actions minimale, environ 4 heures.
  *Pourquoi :* un dépôt bien outillé dès le départ évite de corriger des problèmes de format ou de dépendances pendant tout le projet.
- [ ] **Profil dev : FastF1 et pytest.** Exemples de la documentation FastF1 et guide de démarrage de pytest, environ 4 heures.
  *Pourquoi :* savoir charger une course avant la soirée 1 permet de consacrer cette soirée à explorer les données plutôt qu'à lire la documentation.

## Vue d'ensemble

Les dates supposent un démarrage le 21 septembre 2026, un rythme moyen de 1,5 soirée par semaine et 25 % de marge.

| Phase | À apprendre avant | Date approximative | Dev | DevOps |
|---|---|---|---|---|
| [P0 : outils du dépôt](#p0--outils-du-dépôt) | Soirée 1 | 21 septembre | 5 h | 6,5 h |
| [P1 : données](#p1--données) | Soirées 2 et 3 | Fin septembre | 8,5 h | 9 h |
| [P2 : machine learning](#p2--machine-learning) | Soirée 4 | Vers le 8 octobre | 14,5 h | 13,5 h |
| [P3 : API et images](#p3--api-et-images) | Soirées 8 et 9 | Fin octobre | 7 h | 6 h |
| [P4 : Kubernetes et GitOps](#p4--kubernetes-et-gitops) | Soirées 10 à 12 | Mi-novembre | 11,5 h | 9 h |
| [P5 : suivi et démo](#p5--suivi-et-démo) | Soirées 13 à 15 | Fin novembre | 6 h | 8,5 h |
| [P6 : v2 undercut](#p6--v2-undercut) | Soirée 17 | Fin décembre | 2,5 h | 3 h |
| **Total** | | | **≈ 55 h** | **≈ 56 h** |

Soit environ **3 h 30 par semaine** en moyenne sur quatre mois, avec des pics avant les phases 2 et 4. Une partie peut se faire pendant la première heure de la soirée concernée. Si c'est trop, on étale le planning plutôt que de sauter l'apprentissage.

> **Suivi :** les cases à cocher ne sont pas cliquables dans un README. Pour suivre sa progression, copiez la liste d'une phase dans une issue GitHub : les cases y deviennent cliquables.

---

## P0 : outils du dépôt

*Avant la soirée 1.* Le profil DevOps connaît déjà une partie de ces outils : il les met en place, le profil dev apprend à s'en servir au quotidien.

| Techno | Niveau | Qui mène | Dev | DevOps | Ressources |
|---|---|---|---|---|---|
| Git et pull requests | 🔴 | Les deux | 1 h | 1 h | [Pull requests (FR)](https://docs.github.com/fr/pull-requests) |
| uv | 🟡 | DevOps | 0,5 h | 1 h | [Documentation uv](https://docs.astral.sh/uv/) |
| Ruff et pre-commit | 🟡 | DevOps | 0,5 h | 1 h | [Ruff](https://docs.astral.sh/ruff/), [pre-commit](https://pre-commit.com/) |
| Makefile | ⚪ | DevOps | 0,25 h | 0,5 h | En pratiquant |
| pytest | 🟡 | Dev | 2 h | 1 h | [pytest : Get started](https://docs.pytest.org/en/stable/getting-started.html) |
| GitHub Actions (CI de base) | 🟡 | DevOps | 0,5 h | 2 h | [GitHub Actions (FR)](https://docs.github.com/fr/actions) |

**Pourquoi chaque point est utile**

- [ ] **Git et pull requests.** Notre règle est « une tâche = une branche = une PR relue par l'autre ». La relecture de code est aussi la meilleure façon d'apprendre du travail de l'autre, et c'est une pratique attendue dans toute équipe.
- [ ] **uv.** Il installe les dépendances et fige leurs versions exactes dans `uv.lock`. Sans cela, le projet peut marcher chez l'un et casser chez l'autre, ou dans le conteneur, à cause d'une version différente d'une bibliothèque.
- [ ] **Ruff et pre-commit.** Ruff formate et vérifie le code, pre-commit le lance automatiquement avant chaque commit. Les relectures portent ainsi sur le fond, pas sur des espaces ou des imports inutilisés.
- [ ] **Makefile.** Il donne des commandes communes (`make up`, `make test`, `make train`). Personne n'a à se souvenir de longues commandes, et le README reste simple.
- [ ] **pytest.** Les tests vérifient que l'ingestion, les filtres de tours et l'API font ce qu'on attend. Ils permettent de modifier le code sans peur de casser quelque chose sans le voir.
- [ ] **GitHub Actions.** La CI lance le lint et les tests sur chaque PR. Une PR qui casse quelque chose est repérée avant la fusion, pas trois soirées plus tard.

---

## P1 : données

*Avant les soirées 2 et 3.* pandas est la techno la plus utilisée du projet : tout le temps investi ici sera rentabilisé.

| Techno | Niveau | Qui mène | Dev | DevOps | Ressources |
|---|---|---|---|---|---|
| pandas | 🔴 | Les deux | 4 h | 5 h | [Kaggle Learn : Pandas](https://www.kaggle.com/learn) |
| FastF1 | 🟡 | Dev | 2 h | 1 h | [Documentation FastF1](https://docs.fastf1.dev/) |
| Parquet | ⚪ | Les deux | 0,5 h | 0,5 h | En pratiquant |
| Pandera | 🟡 | Dev | 1 h | 0,5 h | [Pandera](https://pandera.readthedocs.io/) |
| Docker Compose | 🟡 | DevOps | 1 h | 2 h | [Docker Compose](https://docs.docker.com/compose/) |

**Pourquoi chaque point est utile**

- [ ] **pandas.** Toute la préparation des données passe par lui : filtrer les tours propres, regrouper par relais, calculer la cible, joindre la météo au bon tour avec `merge_asof`. En machine learning, on passe plus de temps à préparer les données qu'à entraîner des modèles.
- [ ] **FastF1.** C'est notre source de données : il récupère les tours, les pneus et la météo, et met tout en cache. Bien connaître ses colonnes (`TyreLife`, `Stint`, `TrackStatus`…) évite de construire le modèle sur des tours faussés par une voiture de sécurité ou un passage au stand.
- [ ] **Parquet.** Ce format de fichier est rapide, compact et conserve les types de colonnes, contrairement au CSV. Il suffit de savoir le lire et l'écrire avec pandas.
- [ ] **Pandera.** Il vérifie que les données respectent un schéma : colonnes présentes, types corrects, valeurs plausibles. Si FastF1 change un format un jour, le pipeline s'arrête avec une erreur claire au lieu d'entraîner silencieusement un mauvais modèle.
- [ ] **Docker Compose.** Il lance PostgreSQL, MinIO et MLflow en local avec une seule commande. Chacun a exactement le même environnement, et c'est une étape intermédiaire idéale avant Kubernetes.

---

## P2 : machine learning

*Avant la soirée 4, priorité n° 1.* C'est notre plus grosse marche : on commence dès maintenant, en parallèle des phases 0 et 1.

| Techno | Niveau | Qui mène | Dev | DevOps | Ressources |
|---|---|---|---|---|---|
| Notions de base | 🔴 | Les deux | 4 h | 4 h | [Kaggle Learn](https://www.kaggle.com/learn), [Google ML Crash Course](https://developers.google.com/machine-learning/crash-course) |
| scikit-learn | 🔴 | Les deux | 3,5 h | 3,5 h | [Kaggle Learn](https://www.kaggle.com/learn), [validation croisée](https://scikit-learn.org/stable/modules/cross_validation.html) |
| MLflow : suivi des essais | 🟡 | Les deux | 1,5 h | 1,5 h | [MLOps Zoomcamp](https://github.com/DataTalksClub/mlops-zoomcamp), [MLflow](https://mlflow.org/docs/latest/) |
| Arbres de décision et XGBoost | 🟡 | Les deux | 3 h | 3 h | [ML Zoomcamp](https://github.com/DataTalksClub/machine-learning-zoomcamp) |
| Optuna et SHAP | ⚪ | Dev | 2 h | 0,5 h | [Optuna](https://optuna.org/), [SHAP](https://shap.readthedocs.io/) |
| Registre MLflow et alias | 🟡 | DevOps | 0,5 h | 1 h | [MLflow](https://mlflow.org/docs/latest/) |

**Pourquoi chaque point est utile**

- [ ] **Notions de base.** Apprentissage supervisé, régression, séparation entraînement/test, surapprentissage, MAE (définitions dans le [lexique](#lexique)). Sans elles, on peut faire tourner un modèle sans savoir s'il est bon, ni pourquoi.
- [ ] **scikit-learn.** C'est la boîte à outils standard du ML classique. Les `Pipeline` évitent d'appliquer des transformations différentes à l'entraînement et en production. La validation croisée par groupe (`GroupKFold`) nous protège du piège n° 1 du projet : tester le modèle sur des tours d'un Grand Prix qu'il a déjà vu.
- [ ] **MLflow : suivi des essais.** Il enregistre chaque essai avec ses paramètres, ses scores et le modèle produit. Au dixième essai, on sait encore lequel était le meilleur et comment le reproduire.
- [ ] **Arbres de décision et XGBoost.** XGBoost est souvent le meilleur modèle sur des données tabulaires comme les nôtres. Comprendre les arbres de décision permet de savoir ce qu'il fait, et donc de le régler et de l'expliquer.
- [ ] **Optuna et SHAP.** Optuna cherche automatiquement les meilleurs réglages du modèle. SHAP montre quelles variables pèsent dans chaque prédiction : si l'âge des pneus ne ressort pas, c'est le signe d'un problème dans les données.
- [ ] **Registre MLflow et alias.** Le registre versionne les modèles, et l'alias `champion` désigne celui qui est en production. L'API charge toujours « le champion » : changer de modèle ne demande aucune modification de code.

---

## P3 : API et images

*Avant les soirées 8 et 9.* Le profil dev mène l'API, le profil DevOps mène l'image et la chaîne de publication.

| Techno | Niveau | Qui mène | Dev | DevOps | Ressources |
|---|---|---|---|---|---|
| FastAPI et Pydantic | 🟡 | Dev | 4 h | 1 h | [Tutoriel FastAPI](https://fastapi.tiangolo.com/tutorial/) |
| SQLAlchemy | ⚪ | Dev | 1,5 h | 0,5 h | [Tutoriel SQLAlchemy](https://docs.sqlalchemy.org/en/20/tutorial/) |
| Docker avancé | 🔴 | DevOps | 1 h | 2 h | [Docker : Get started](https://docs.docker.com/get-started/) |
| GHCR, tags SHA et Trivy | 🟡 | DevOps | 0,5 h | 2,5 h | [GitHub Actions (FR)](https://docs.github.com/fr/actions), [Trivy](https://trivy.dev/) |

**Pourquoi chaque point est utile**

- [ ] **FastAPI et Pydantic.** FastAPI expose le modèle sous forme d'API web ; Pydantic valide les entrées. Une requête avec une gomme inconnue ou une température négative est rejetée avec un message clair au lieu de produire une prédiction absurde. La documentation interactive générée automatiquement est aussi idéale pour une démo.
- [ ] **SQLAlchemy.** Il enregistre chaque prédiction dans PostgreSQL. Cet historique est indispensable pour surveiller le modèle et comparer plus tard ses prédictions à la réalité.
- [ ] **Docker avancé.** Construction en plusieurs étapes, utilisateur non root, image légère. Une image plus petite se télécharge et démarre plus vite, et une image qui ne tourne pas en root limite les dégâts en cas de faille.
- [ ] **GHCR, tags SHA et Trivy.** Taguer chaque image avec le SHA du commit permet de savoir exactement quel code tourne et de revenir en arrière, ce que le tag `latest` interdit. Trivy bloque la publication si l'image contient une vulnérabilité critique.

---

## P4 : Kubernetes et GitOps

*Avant les soirées 10 à 12.* C'est la deuxième grosse marche, surtout pour le profil dev. C'est lui qui mène la soirée 10 : c'est l'échange de rôles le plus formateur du projet.

| Techno | Niveau | Qui mène | Dev | DevOps | Ressources |
|---|---|---|---|---|---|
| Bases de Kubernetes | 🔴 | Dev | 8 h | 2 h | [Tutoriels Kubernetes (FR)](https://kubernetes.io/fr/docs/tutorials/), [ML Zoomcamp](https://github.com/DataTalksClub/machine-learning-zoomcamp), [Blog de Stéphane Robert (FR)](https://blog.stephane-robert.info/) |
| kind | 🟡 | DevOps | 0,5 h | 1 h | [kind : quick start](https://kind.sigs.k8s.io/docs/user/quick-start/) |
| Kustomize | 🟡 | DevOps | 1 h | 2 h | [Kustomize](https://kubernetes.io/docs/tasks/manage-kubernetes-objects/kustomization/) |
| Argo CD | 🟡 | DevOps | 1 h | 3 h | [Argo CD : getting started](https://argo-cd.readthedocs.io/en/stable/getting_started/) |
| CronJobs | 🟡 | Les deux | 1 h | 1 h | [CronJob Kubernetes](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/) |

**Pourquoi chaque point est utile**

- [ ] **Bases de Kubernetes.** Pod, Deployment, Service, ConfigMap, Secret, volume persistant, et savoir lire `kubectl logs` et `kubectl describe`. C'est ce qui fait tourner toute notre chaîne ; sans ces bases, chaque erreur de déploiement devient un mystère.
- [ ] **kind.** Il crée un vrai cluster Kubernetes sur nos machines, gratuitement. On apprend dans des conditions proches de la production sans payer de cloud.
- [ ] **Kustomize.** Il gère une base commune de manifestes et des variantes par environnement, sans dupliquer les fichiers. C'est intégré à `kubectl`, donc rien à installer.
- [ ] **Argo CD.** Il applique le GitOps : le dépôt Git décrit ce qui doit tourner, et Argo CD met le cluster en conformité. Chaque déploiement passe par une PR relue, donc tout est tracé et un retour en arrière revient à annuler un commit.
- [ ] **CronJobs.** Ils lancent l'ingestion et le réentraînement automatiquement après chaque Grand Prix. C'est ce qui transforme un modèle entraîné une fois en système qui se met à jour tout seul.

---

## P5 : suivi et démo

*Avant les soirées 13 à 15.* Deux formes de suivi : la santé du service (Prometheus, Grafana) et la santé du modèle (Evidently).

| Techno | Niveau | Qui mène | Dev | DevOps | Ressources |
|---|---|---|---|---|---|
| Prometheus et PromQL | 🟡 | DevOps | 1 h | 3 h | [Prometheus : getting started](https://prometheus.io/docs/prometheus/latest/getting_started/) |
| Grafana et chart Helm | 🟡 | DevOps | 0,5 h | 3 h | [Charts prometheus-community](https://github.com/prometheus-community/helm-charts), [Tutoriels Grafana](https://grafana.com/tutorials/) |
| Evidently | 🟡 | Dev | 3 h | 1 h | [Cours Evidently](https://learn.evidentlyai.com/), [documentation actuelle](https://docs.evidentlyai.com/) |
| Streamlit | ⚪ | Les deux | 1,5 h | 1,5 h | [Streamlit : get started](https://docs.streamlit.io/get-started) |

**Pourquoi chaque point est utile**

- [ ] **Prometheus et PromQL.** Prometheus collecte les métriques de l'API : latence, erreurs, nombre de requêtes. PromQL, son langage de requête, permet de répondre à « l'API ralentit-elle depuis le dernier déploiement ? ».
- [ ] **Grafana et chart Helm.** Grafana affiche ces métriques dans un tableau de bord. Le chart Helm `kube-prometheus-stack` installe Prometheus et Grafana d'un coup : on apprend à utiliser Helm pour installer des outils tiers, sans avoir à écrire ses propres charts.
- [ ] **Evidently.** Un service peut répondre vite et sans erreur tout en donnant de mauvaises prédictions. Evidently détecte la dérive des données : si un Grand Prix ne ressemble pas aux données d'entraînement (chaleur extrême, nouveau circuit), on le sait avant de faire confiance aux prédictions. Attention : le code du cours date de 2023 et l'API d'Evidently a beaucoup changé, suivez la documentation actuelle pour le code.
- [ ] **Streamlit.** Il permet de construire la démo « et si » en Python pur, sans HTML ni JavaScript. C'est ce que verront les gens à qui on présente le projet : autant qu'elle soit parlante.

---

## P6 : v2 undercut

*Avant la soirée 17.* Peu de nouvelles technos : l'effort porte sur la logique de simulation.

| Techno | Niveau | Qui mène | Dev | DevOps | Ressources |
|---|---|---|---|---|---|
| API REST d'OpenF1 | ⚪ | DevOps | 0,5 h | 1 h | [Documentation OpenF1](https://openf1.org/docs/) |
| Simulation tour par tour | 🟡 | Les deux | 2 h | 2 h | En pratiquant |

**Pourquoi chaque point est utile**

- [ ] **API REST d'OpenF1.** Son endpoint `/pit` donne directement la durée des arrêts au stand. Il sert à vérifier nos propres calculs de temps perdu, qui sont au cœur de l'undercut.
- [ ] **Simulation tour par tour.** Le simulateur avance tour par tour en utilisant le modèle de la v1 pour comparer deux stratégies. Le tester sur des cas calculables à la main garantit qu'il fait ce qu'on croit avant de le confronter aux vraies courses.

---

## Ce qu'on n'apprend pas maintenant

Ces sujets reviennent souvent dans les offres d'emploi, mais les ajouter maintenant ralentirait le projet sans nous faire progresser sur l'essentiel.

| Sujet | Pourquoi on attend |
|---|---|
| Écrire ses propres charts Helm | Kustomize suffit pour notre application ; Helm sert seulement à installer Prometheus et Grafana. |
| Terraform et le cloud (AWS, GCP, Azure) | Tout tourne sur nos machines, gratuitement. À envisager après la v2. |
| Orchestrateurs (Prefect, Airflow) | Les CronJobs Kubernetes suffisent pour un pipeline déclenché une fois par Grand Prix. |
| Deep learning et vision (PyTorch, YOLO) | Utile pour une éventuelle phase vidéo, pas pour des données tabulaires. |
| Feature store, service mesh, multi-cluster | Des outils pour les grandes équipes, surdimensionnés pour un projet à deux. |
| Les maths détaillées des algorithmes | Comprendre l'intuition suffit pour avancer ; le détail viendra si le sujet nous passionne. |

## Lexique

| Notion | Définition |
|---|---|
| Cible | Ce que le modèle doit prédire. Ici, l'écart de temps au tour par rapport à la référence du relais. |
| Variable (feature) | Une information donnée au modèle pour prédire la cible : âge des pneus, gomme, température de piste… |
| Régression | Prédire un nombre (une durée, un prix), par opposition à la classification, qui prédit une catégorie. |
| Modèle de référence (baseline) | Un modèle volontairement simple. Si un modèle compliqué ne fait pas mieux, il ne sert à rien. |
| MAE | Erreur absolue moyenne : de combien de secondes le modèle se trompe en moyenne. Plus c'est bas, mieux c'est. |
| Validation croisée par groupe | On teste le modèle sur des Grands Prix qu'il n'a jamais vus, en gardant chaque GP entier d'un seul côté du découpage. |
| Fuite de données | Le modèle a accès, pendant l'entraînement, à une information qu'il n'aura pas au moment de prédire. Ses scores sont alors trop beaux pour être vrais. |
| Surapprentissage | Le modèle apprend par cœur les données d'entraînement et se trompe sur les nouvelles. |
| Hyperparamètres | Les réglages du modèle choisis avant l'entraînement, comme la profondeur des arbres de XGBoost. |
| SHAP | Une méthode qui mesure la contribution de chaque variable à une prédiction. |
| Dérive des données | Les données reçues en production ne ressemblent plus à celles de l'entraînement. |
| Champion / challenger | Le modèle en production (champion) n'est remplacé par un nouveau (challenger) que si celui-ci fait mieux sur les mêmes données de test. |

---

*Parcours établi le 16 septembre 2026. Les durées sont des estimations pour des débutants en ML ; ajustez-les après les premières phases.*
