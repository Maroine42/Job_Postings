# Aperçu

Bienvenue dans mon analyse du marché de l'emploi dans le domaine de la data, avec un focus sur les postes de Data Scientist. Ce projet est né de mon désir de mieux comprendre et naviguer dans ce marché. Il explore les compétences les plus demandées et les mieux rémunérées afin d’identifier les opportunités optimales pour les Data Scientists.

Les données proviennent d'une [base de données publique sur Hugging Face](https://huggingface.co/datasets/lukebarousse/data_jobs), contenant des informations détaillées sur les intitulés de poste, les salaires, les localisations et les compétences essentielles. Grâce à une série de scripts Python, j’examine des questions clés telles que les compétences les plus demandées, les tendances salariales et l’intersection entre la demande et la rémunération dans le domaine de l'analyse de données.

Veuillez noter que les données présentées ici couvrent l’ensemble de l’année 2023.

# Les Questions

Voici les questions auxquelles je souhaite répondre dans mon projet :

    1.Quelles sont les compétences les plus demandées pour les trois rôles data les plus populaires en France ?

    2.Comment évoluent les compétences en demande pour les Data Scientists en France ?

    3.Quelle est la rémunération des emplois et des compétences pour les Data Scientists aux États-Unis ?

    4.Quelles sont les compétences optimales à apprendre pour les Data Scientists aux États-Unis ? (Forte demande ET haut salaire)


# Outils Utilisés


- Python : L'outil central de mon analyse, me permettant d'examiner les données et d'en extraire des insights essentiels. J'ai utilisé les bibliothèques suivantes :

    - Pandas : Pour l'analyse des données.
    - Matplotlib : Pour la visualisation des données.
    - Seaborn : Pour créer des visualisations plus avancées.

- Jupyter Notebooks : L'outil que j'ai utilisé pour exécuter mes scripts Python tout en intégrant mes notes et analyses.

- Visual Studio Code : Mon environnement de développement pour exécuter mes scripts Python.

- Git & GitHub : Essentiels pour le contrôle de version et le partage de mon code Python et de mes analyses, assurant la collaboration et le suivi du projet.

# Gestion des environnements avec Anaconda

Gérer efficacement les environnements est essentiel lorsqu'on travaille sur des projets d'analyse de données, car différentes bibliothèques et dépendances peuvent entraîner des conflits ou des problèmes de compatibilité. Tout au long de ce projet, j'ai utilisé Anaconda pour créer et gérer des environnements isolés, garantissant ainsi un flux de travail stable et reproductible.

Grâce à Anaconda, j'ai pu :

- Maintenir la compatibilité des dépendances : Les différentes bibliothèques nécessitent souvent des versions spécifiques de leurs dépendances. Avec les environnements Anaconda, j'ai pu installer et gérer les packages sans affecter mon installation Python globale.

- Assurer la reproductibilité : La création d’un environnement dédié m'a permis de documenter précisément les versions des packages utilisés, facilitant ainsi la reproduction des résultats et le partage du projet.

- Travailler avec plusieurs bibliothèques : Tout au long du projet, j'ai utilisé diverses bibliothèques comme pandas pour la manipulation des données, matplotlib et seaborn pour la visualisation, et scikit-learn pour le machine learning. Anaconda m’a permis d’installer et de basculer facilement entre ces outils selon les besoins.

- Éviter les conflits : Certaines bibliothèques nécessitent des versions spécifiques de dépendances qui peuvent ne pas être compatibles entre elles. En isolant mon projet dans un environnement Anaconda, j'ai pu éviter les conflits susceptibles de perturber mon travail.

# Préparation et nettoyage des données

Cette section décrit les étapes effectuées pour préparer les données à l’analyse, en garantissant leur précision et leur exploitabilité.

## Importation et nettoyage des données

Je commence par importer les bibliothèques nécessaires et charger le jeu de données, suivi des premières tâches de nettoyage afin d'assurer la qualité des données.

```python
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```

## Filtrer les offres en France

Pour concentrer mon analyse sur le marché de l’emploi en France, j’applique des filtres au jeu de données afin de ne retenir que les postes basés en France.

```python
df_FR = df[df['job_country'] == 'France']
```

# L'Analyse

## 1. Quelles sont les compétences les plus demandées pour les 3 rôles data les plus populaires en France ?

Pour identifier les compétences les plus demandées pour les trois rôles data les plus populaires, j’ai filtré les postes en fonction de leur popularité, puis extrait les cinq compétences principales associées à ces trois rôles. Cette requête met en avant les intitulés de poste les plus courants ainsi que leurs compétences clés, indiquant les compétences sur lesquelles me concentrer en fonction du rôle que je vise.

# Visualisation des données

```python
fig, ax = plt.subplots(len(job_titles), 1)


for i, job_title in enumerate(job_titles):
    df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)[::-1]
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r')

plt.show()
```

## Résultats

![Vis Skills](Visualisations/Skill%20Demand/Skill_Demand_Likelihood.png)

*Graphique en barres visualisant les salaires des 3 principaux rôles de données et leurs 5 compétences principales associées à chacun.*

## Insights 

- SQL est une compétence très demandée dans tous les rôles : SQL apparaît fréquemment dans les offres d’emploi pour les Data Scientists (45 %), les Data Engineers (49 %) et les Data Analysts (33 %). Cela indique que SQL est une compétence essentielle pour les professionnels des métiers de la data en France.

- Python est une compétence clé pour les Data Engineers et les Data Scientists : Python est fortement requis pour les Data Engineers (57 %) et les Data Scientists (67 %). Cela suggère que Python est indispensable pour les rôles impliquant un traitement et une analyse avancée des données.

- Les plateformes cloud sont importantes pour les Data Engineers : Des compétences liées aux plateformes cloud comme AWS (27 %) et Spark (35 %) sont particulièrement recherchées pour les Data Engineers. Cela reflète l'importance croissante du cloud computing dans les tâches d’ingénierie des données.

## 2. Comment évoluent les compétences en demande pour les Data Scientists en France ?

Pour analyser l’évolution des compétences demandées en 2023 pour les Data Scientists, j’ai filtré les offres de Data Scientist et regroupé les compétences par mois de publication des annonces. Cela m’a permis d’identifier les cinq compétences les plus populaires chaque mois, montrant ainsi leur évolution tout au long de l’année 2023.

## Visualisation des données

```python
fig, ax = plt.subplots(len(job_titles), 1)

from matplotlib.ticker import PercentFormatter

df_plot = df_DA_US_percent.iloc[:, :5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')

plt.gca().yaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.show()
```

## Results

![skill_trend](Visualisations/Skill%20Trend/Skill_Trend.png)

*Graphique en barres visualisant les compétences les plus tendances pour les Data Scientists en France en 2023.*

## Insights 

- Python est la compétence la plus demandée : Python est constamment la compétence la plus mentionnée dans les offres d’emploi pour les Data Scientists tout au long de l’année, soulignant son importance cruciale dans le domaine.

- SQL et Git sont également très valorisés : SQL et Git apparaissent fréquemment dans les offres, ce qui suggère que la manipulation des données et le contrôle de version sont des compétences essentielles pour les Data Scientists en France.

- R et Spark ont une demande modérée : Bien que R et Spark soient mentionnés dans les offres d'emploi, leur fréquence est inférieure à celle de Python, SQL et Git. Cela indique qu’ils restent importants, mais ne sont pas aussi universellement requis que les trois compétences principales.


# 3. Quelle est la rémunération des emplois et compétences pour les Data Scientists aux États-Unis ?

Pour cette section, j’ai choisi de concentrer mon analyse sur les États-Unis, car la majorité des offres d'emploi de la base de données proviennent de ce pays.

Afin d’identifier les rôles et compétences les mieux rémunérés, j’ai filtré les offres pour ne retenir que celles basées aux États-Unis et analysé leur salaire médian. Mais avant cela, j’ai étudié la distribution des salaires pour les principaux postes data tels que Data Scientist et Data Engineer, afin d’obtenir une idée des emplois les mieux payés.


## Visualisation des données


```python
sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)

ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```

## Results

![skill_dist](Visualisations/Salary%20Analysis/Salary_Distribution.png)

*Diagramme en boîte visualisant les distributions salariales des 6 principaux titres de postes en data.*



## Insights


- Les postes seniors offrent des salaires plus élevés : Les postes de Senior Data Scientist et Senior Data Engineer affichent des distributions salariales plus élevées que leurs équivalents non seniors. Cela montre que l’expérience et l’ancienneté ont un impact significatif sur le potentiel de rémunération dans les métiers de la data.

- Les Data Scientists gagnent plus que les Data Engineers : La distribution salariale des Data Scientists est généralement supérieure à celle des Data Engineers, suggérant que les rôles de Data Scientist peuvent offrir une meilleure rémunération en moyenne.

- Les Data Scientists ont la fourchette salariale la plus basse : Les Data Scientists, qu’ils soient seniors ou non, présentent la fourchette salariale la plus basse parmi les rôles analysés. Cela reflète le caractère relativement accessible de ces postes par rapport à des rôles plus spécialisés comme Data Engineer.

## Compétences les mieux rémunérées et les plus demandées pour les Data Scientists

Ensuite, j’ai affiné mon analyse en me concentrant uniquement sur les rôles de Data Scientist. J’ai examiné les compétences les mieux rémunérées ainsi que celles les plus demandées. Pour illustrer ces résultats, j’ai utilisé deux graphiques en barres.

## Visualisation des données

```python
fig, ax = plt.subplots(2, 1)  

# Top 10 Highest Paid Skills for Data Scientists
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')

# Top 10 Most In-Demand Skills for Data Scientistsr')
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')

plt.show()
```

## Résultats

Voici un aperçu des compétences les mieux rémunérées et les plus demandées pour les Data Scientists aux États-Unis :

![high_paid](Visualisations/Salary%20Analysis/Salary_Skillls_In_Demand.png)

*Graphiques en barres séparés visualisant les compétences les mieux rémunérées et les compétences les plus demandées pour les Data Scientists aux États-Unis.*


## Insights

- Les outils spécialisés commandent des salaires plus élevés : Des compétences comme Asana, Airtable et Watson sont associées à des salaires médians plus élevés, ce qui indique que l'utilisation d'outils spécialisés ou de niche peut considérablement augmenter le potentiel de rémunération des Data Scientists aux États-Unis.

- Les compétences techniques de base sont les plus demandées : Des compétences comme TensorFlow, Spark, SQL et Python sont très demandées, ce qui reflète leur importance fondamentale dans le travail quotidien des Data Scientists. Malgré leur forte demande, ces compétences ne sont pas toujours associées aux salaires les plus élevés.

- Disparité entre les compétences les mieux rémunérées et les plus demandées : Il existe un écart notable entre les compétences les mieux rémunérées et les plus demandées. Cela suggère que, bien que certaines compétences spécialisées puissent entraîner des salaires plus élevés, les compétences techniques de base restent essentielles pour la majorité des rôles de Data Scientist.


# 4. Quelles sont les compétences les plus optimales à apprendre pour les Data Scientists ?

Pour identifier les compétences les plus optimales à apprendre (celles qui sont à la fois les mieux rémunérées et les plus demandées), j'ai calculé le pourcentage de demande pour chaque compétence ainsi que le salaire médian associé. Cela permet de repérer facilement les compétences les plus avantageuses à acquérir.

Ensuite, pour approfondir l’analyse, nous allons visualiser les différentes technologies dans le graphique. Nous ajouterons des étiquettes colorées en fonction de la technologie (par exemple, {Programmation : Python}).

## Visualisation des données

```python
from matplotlib.ticker import PercentFormatter

# Create a scatter plot
scatter = sns.scatterplot(
    data=df_DA_skills_tech_high_demand,
    x='skill_percent',
    y='median_salary',
    hue='technology',  # Color by technology
    palette='bright',  # Use a bright palette for distinct colors
    legend='full'  # Ensure the legend is shown
)
plt.show()
```

## Résultats

![opt_skills](Visualisations/Optimal%20Skills/Optimal_Skills.png)

*Nuage de points visualisant les compétences les plus optimales (bien rémunérées et très demandées) pour les Data Scientists aux États-Unis avec des étiquettes de couleur pour la technologie.*


## Insights

- TensorFlow et Spark sont des compétences très valorisées : TensorFlow et Spark figurent parmi les compétences les plus optimales pour les Data Scientists, avec un pourcentage important d'annonces d'emploi demandant ces compétences. Cela souligne leur importance dans le traitement avancé des données et les tâches d'apprentissage automatique.

- Les compétences en programmation de base sont essentielles : Des compétences comme Python et SQL sont cruciales et apparaissent dans un grand pourcentage d'annonces d'emploi. Cela met en évidence l'importance des compétences solides en programmation et en manipulation des données dans le domaine de la Data Science.

- Les outils cloud et d'analyse prennent de l'importance : Les technologies cloud (par exemple, AWS) et les outils d'analyse (par exemple, Tableau, Excel) deviennent de plus en plus pertinents, reflétant la demande croissante de Data Scientists maîtrisant l'informatique en nuage et la visualisation des données.


# Ce que j'ai appris

Tout au long de ce projet, j'ai approfondi ma compréhension du marché de l'emploi pour les Data Scientists et j'ai amélioré mes compétences techniques en Python, en particulier dans la manipulation et la visualisation des données. Voici quelques points spécifiques que j'ai appris :

- Utilisation avancée de Python : L'utilisation de bibliothèques comme Pandas pour la manipulation des données, Seaborn et Matplotlib pour la visualisation des données, ainsi que d'autres bibliothèques m'a permis de réaliser des tâches d'analyse de données complexes de manière plus efficace.

- Importance du nettoyage des données : J'ai appris que le nettoyage et la préparation des données sont essentiels avant toute analyse, afin de garantir l'exactitude des informations extraites des données.

- Analyse stratégique des compétences : Le projet a mis en évidence l'importance d'aligner ses compétences avec la demande du marché. Comprendre la relation entre la demande de compétences, les salaires et la disponibilité des emplois permet de mieux planifier sa carrière dans le secteur technologique.


# Insights

Ce projet a fourni plusieurs enseignements généraux sur le marché de l'emploi pour les Data Scientists :

- Les compétences techniques fondamentales sont essentielles : Des compétences comme SQL, Python et les outils de visualisation des données (par exemple, Tableau, Excel) sont indispensables pour les Data Scientists. Ces compétences sont fréquemment mentionnées dans les annonces d'emploi et sont cruciales pour la manipulation, l'analyse et la présentation des données.

- L'expérience et la spécialisation augmentent le potentiel de rémunération : Les postes seniors, tels que Senior Data Scientist, commandent des salaires plus élevés que les postes d'entrée. De plus, des compétences spécialisées dans des outils comme Asana, Airtable ou des plateformes d'analytique avancée peuvent encore augmenter le potentiel de rémunération.

- La montée en puissance du cloud et de l'analyse avancée : La demande pour la maîtrise des technologies cloud (par exemple, AWS) et des outils d'analyse avancée (par exemple, TensorFlow, Spark) est en forte croissance. Cette tendance indique que les Data Scientists capables de tirer parti de ces technologies auront un avantage compétitif sur le marché de l'emploi.


# Les défis rencontrés


Ce projet n'a pas été sans défis, mais il a offert de bonnes opportunités d'apprentissage :


- Incohérences dans les données : Gérer les données manquantes ou incohérentes nécessite une attention particulière et des techniques de nettoyage des données rigoureuses pour garantir l'intégrité de l'analyse.

- Visualisation complexe des données : Concevoir des représentations visuelles efficaces de jeux de données complexes a été un défi, mais c'était crucial pour transmettre clairement et de manière convaincante les résultats de l'analyse.

- Trouver l'équilibre entre la largeur et la profondeur : Décider de la profondeur d'analyse à adopter tout en maintenant une vue d'ensemble du paysage des données a nécessité un équilibre constant pour garantir une couverture complète sans se perdre dans les détails.


# Conclusion

Cette exploration du marché de l'emploi des Data Scientists a été extrêmement informative, mettant en lumière les compétences clés et les tendances qui façonnent ce domaine en constante évolution. Les informations que j'ai recueillies renforcent ma compréhension et offrent des conseils pratiques à ceux qui souhaitent faire progresser leur carrière dans l'analyse de données. À mesure que le marché continue d'évoluer, une analyse continue sera essentielle pour rester en tête dans le domaine de l'analyse des données. Ce projet constitue une bonne base pour de futures explorations et souligne l'importance de l'apprentissage continu et de l'adaptation dans le domaine des données.