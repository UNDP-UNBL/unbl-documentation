# Ajouter vos propres métriques personnalisées à votre espace de travail

**Il s'agit d'une traduction générée par une intelligence artificielle qui peut contenir des erreurs.**

La plateforme publique UNBL propose actuellement onze métriques dynamiques par défaut (voir [« Quelles métriques dynamiques sont disponibles pour mon pays ? »](../unbl-public-platform/8_dynamic_metrics1.md)).

Les espaces de travail UNBL permettent aux utilisateurs de configurer leurs propres métriques personnalisées afin d'effectuer des calculs à la volée et d'afficher des statistiques zonales pour leurs zones d'intérêt, dérivées de leurs propres couches de données géospatiales téléchargées.

La configuration d'une métrique personnalisée dans un espace de travail suit 5 étapes. Chaque étape est décrite dans cette section.

## Étape 1 : Télécharger un lieu

Les métriques sont affichées sur UNBL après la sélection d'un lieu particulier qui définit la zone d'intérêt pour laquelle les statistiques zonales sont calculées. Il est donc nécessaire de télécharger un lieu dans votre espace de travail pour lequel vous souhaitez consulter la métrique personnalisée. Pour des étapes détaillées sur le téléchargement d'un lieu dans votre espace de travail, voir [« Comment ajouter des lieux ? »](5_add_places.md).

## Étape 2 : Télécharger une couche raster au format GeoTIFF

Pour créer une métrique personnalisée, il est nécessaire de télécharger une couche raster pour laquelle vous souhaitez consulter des statistiques zonales. Les espaces de travail UNBL prennent uniquement en charge les calculs de métriques à partir de couches téléchargées via l'option « Téléchargement de fichier GeoTIFF ». Pour des étapes détaillées sur le téléchargement d'une couche à l'aide de cette option, voir [« Comment télécharger des couches raster au format GeoTIFF ? »](6_add_data.md/#how-do-i-upload-raster-layers-in-geotiff-format).

Pour qu'une métrique personnalisée fonctionne correctement sur UNBL, il est nécessaire de tenir compte des prérequis techniques suivants pour les couches GeoTIFF téléchargées :

- Les GeoTIFF peuvent représenter tout type de données continues ou catégorielles, mais les valeurs de pixels doivent être de type entier ou flottant ;

- Il est recommandé que pour les données catégorielles, les GeoTIFF ne contiennent pas plus de 25 classes entières discrètes ; un nombre plus élevé de classes nuit à la lisibilité des graphiques de métriques ;

- Les statistiques zonales pour votre métrique personnalisée ne peuvent être calculées que pour les couches GeoTIFF dont la couverture de données recouvre le lieu téléchargé. Si le lieu sélectionné dans la vue cartographique UNBL ne chevauche pas spatialement la couche GeoTIFF configurée pour la métrique personnalisée, la métrique renverra un graphique de données vide.

- Il est possible de configurer une métrique de série temporelle qui montre les changements au fil du temps pour plusieurs couches GeoTIFF ; dans ces cas, toutes les couches GeoTIFF doivent avoir les mêmes valeurs d'attributs, telles que les plages de valeurs min/max pour les données continues, ou les définitions de catégories (c'est-à-dire la configuration de la légende) pour les données catégorielles.

Les utilisateurs souhaitant configurer des métriques personnalisées pour des données vectorielles de polygones doivent d'abord les convertir au format GeoTIFF raster à l'aide de techniques de rastérisation. Des exemples de techniques de rastérisation sont disponibles dans la documentation en ligne de [QGIS](https://docs.qgis.org/3.44/en/docs/user_manual/processing_algs/gdal/vectorconversion.html#rasterize-vector-to-raster), [PyGIS](https://pygis.io/docs/e_raster_rasterize.html) et [rdrr.io](https://rdrr.io/cran/terra/man/rasterize.html). Pour minimiser les erreurs dans les calculs de métriques pouvant être introduites par le processus de rastérisation des données vectorielles, tenez compte des points suivants :

- Si les données vectorielles ne contiennent que des noms d'attributs textuels, un nouveau champ entier doit être ajouté avant la rastérisation et un numéro unique doit être attribué à chaque classe pour les données catégorielles ; ce champ entier doit être utilisé pour attribuer des valeurs de pixels lors de la rastérisation ;

- Convertir les données vectorielles dans un système de référence de coordonnées projetées cohérent tel que WGS84 (EPSG : 4326) avant la rastérisation ;

- Les polygones superposés doivent être résolus par priorité de gravure de pixels lors de la rastérisation — par exemple, des approches « dernier tracé » ou « valeur la plus élevée gagne » ;

- Choisir une résolution raster appropriée qui tient compte des compromis entre la minimisation des erreurs de bordure lors de la conversion des limites de polygones en pixels et la minimisation de la taille du fichier (les couches téléchargées dans votre espace de travail UNBL via l'option « Téléchargement de fichier GeoTIFF » ne peuvent pas dépasser 1 000 Mo).

## Étape 3 : Créer une métrique

La création d'une métrique consiste à choisir les couches GeoTIFF de l'espace de travail qui doivent être utilisées pour calculer les statistiques zonales. Pour créer une métrique :

1.	Cliquez sur le bouton « Home » dans la page d'administration de votre espace de travail pour développer le menu déroulant. Sélectionnez « Metrics ».

2.	Cliquez sur le bouton « CREATE NEW METRIC » qui apparaît.

	![](images/en/image1-metrics.png)

3.	Dans la page de nouvelle métrique, renseignez les informations suivantes :

	a) *Title* : Le nom de votre métrique. Il doit décrire ce que montre le jeu de données pour lequel vous configurez la métrique. Il peut être identique à celui de la couche GeoTIFF téléchargée.

	b) *Metric slug* : Un slug est un identifiant unique pour la métrique dans votre espace de travail. Vous ne pouvez pas avoir plusieurs métriques avec le même slug dans votre espace de travail. Il ne doit contenir que des lettres, des chiffres et des tirets (« - »). Vous pouvez utiliser le bouton « GENERATE SLUG NAME » pour générer un identifiant unique basé sur le titre de métrique fourni.

	c) *Metric data source(s)* : Choisissez une couche GeoTIFF téléchargée dans votre espace de travail dans la liste déroulante. Pour les métriques personnalisées montrant l'évolution dans le temps, vous avez la possibilité de sélectionner plusieurs couches GeoTIFF à l'aide du bouton « ADD ADDITIONAL DATA SOURCE ». Sélectionnez cette option uniquement si vous disposez d'une série de couches GeoTIFF avec un schéma d'attributs cohérent, tel que des plages de valeurs min/max pour les données continues, ou des définitions de catégories (c'est-à-dire une configuration de légende) pour les données catégorielles.

	d) *Histogram bins* : Cette option apparaît comme un champ obligatoire si vous avez sélectionné une couche GeoTIFF avec une catégorie de données continues. La métrique utilisera un histogramme pour calculer les statistiques zonales des couches de données continues. Il est donc nécessaire de spécifier le nombre de classes que l'histogramme calculé pour la couche de données continues aura. Les classes sont également appelées intervalles. Elles divisent la plage de données numériques stockées dans la couche GeoTIFF en groupes de largeur égale. Choisissez un nombre qui crée un nombre adéquat d'intervalles de données pour la plage et la dispersion de vos données. Dans la plupart des cas, entre 5 et 20 classes est optimal, mais cela dépend de la plage de données spécifique.

	e) *Calculate for place types (optional)* : Vous pouvez éventuellement sélectionner le type de lieu pour lequel votre métrique personnalisée doit afficher des statistiques. Ceci est utile, par exemple, pour une métrique montrant l'eutrophisation côtière qui ne couvre que les lieux de la catégorie marine. Cependant, si la couche de données pour la métrique n'est pas conçue pour être limitée à une zone spécifique, ce champ doit rester vide.

	f)	Une fois tous les paramètres spécifiés, le bouton « SAVE AND VIEW DETAILS » s'affichera en bleu, à condition que toutes les informations saisies soient valides. Cliquez sur ce bouton pour configurer la métrique dans votre espace de travail.

	![](images/en/image2-metrics.png)

4. Dans la page de modification de métrique qui apparaît, activez le bouton « Published » pour publier la métrique.

## Étape 4 : Créer un widget

Une fois la métrique personnalisée configurée, il est nécessaire de créer un widget pour cette métrique. Le widget configure la manière dont les données de la métrique seront visualisées et les informations qu'elles afficheront dans la vue cartographique UNBL. Pour créer un widget :

1.	Cliquez sur le bouton « Home » dans la page d'administration de votre espace de travail pour développer le menu déroulant. Sélectionnez « Widgets ».

2.	Cliquez sur le bouton « CREATE NEW WIDGET » qui apparaît.

	![](images/en/image3-metrics.png)

3. Dans la page de nouveau widget, renseignez les informations suivantes :

	a) *Title* : Idéalement, le nom du widget doit être identique à celui de la métrique configurée à l'étape 3. Cela associe clairement le widget à sa métrique.

	b) *Widget slug* : Un slug est un identifiant unique pour le widget dans votre espace de travail. Il ne doit contenir que des lettres, des chiffres et des tirets (« - »). Vous pouvez utiliser le bouton « GENERATE SLUG NAME » pour générer un identifiant unique basé sur le titre de métrique fourni. Idéalement, le slug du widget doit correspondre au slug de la métrique de l'étape 3.

	c) *Description (optional)* : Créez une courte description pour votre widget. Il doit s'agir d'une description générale des données que votre métrique affiche sur UNBL. Ce champ est optionnel.

	d) *Metric* : Choisissez la métrique créée à l'étape 3 à associer au widget.

	e) *Widget Layer(s)* : Ce champ spécifie la couche de données qui peut être visualisée dans la vue cartographique UNBL avec la métrique. Il est automatiquement rempli avec la couche GeoTIFF associée à la métrique choisie. Il est possible de choisir des couches supplémentaires dans le menu déroulant. Cependant, cela n'est pas recommandé, sauf si des couches supplémentaires existent dans l'espace de travail qui ne sont pas utilisées pour calculer la métrique mais sont utiles pour ajouter des informations géospatiales contextuelles.

	f) *Widget Chart* : Détermine le type de graphique qui sera utilisé pour visualiser les statistiques de la métrique pour votre lieu. Le tableau ci-dessous donne un aperçu des types de graphiques disponibles en fonction du type de widget, qui est automatiquement détecté selon a) si la couche GeoTIFF présente des données catégorielles (classes discrètes) ou continues (plage de valeurs numériques), et b) si une seule couche ou plusieurs couches (métrique de série temporelle) ont été choisies pour la métrique.

	<div style="display: flex; justify-content: center;">
	<table style="border-collapse: collapse;">
    <thead>
      <tr>
        <th style="min-width: 200px; border: 1px solid #ccc;"></th>
        <th style="text-align: center; font-size: 1.0rem; white-space: nowrap; border: 1px solid #ccc;"><strong>Données continues</strong></th>
        <th style="text-align: center; font-size: 1.0rem; white-space: nowrap; border: 1px solid #ccc;"><strong>Données catégorielles</strong></th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td style="font-size: 1.0rem; border: 1px solid #ccc;"><strong>Couche unique</strong></td>
        <td style="text-align: center; border: 1px solid #ccc;">Histogramme<br>![](images/en/image4-metrics.png){ width="350" style="display:inline-block"}</td>
        <td style="text-align: center; border: 1px solid #ccc;">Diagramme circulaire<br>![](images/en/image5-metrics.png){ width="250" style="display:inline-block"}<br>Diagramme en barres<br>![](images/en/image6-metrics.png){ width="250" style="display:inline-block"}</td>
      </tr>
      <tr>
        <td style="font-size: 1.0rem; border: 1px solid #ccc;"><strong>Série temporelle</strong></td>
        <td style="text-align: center; border: 1px solid #ccc;">Graphique linéaire<br>![](images/en/image7-metrics.png){ width="250" style="display:inline-block"}<br>Graphique en aires<br>![](images/en/image8-metrics.png){ width="250" style="display:inline-block"}</td>
        <td style="text-align: center; border: 1px solid #ccc;">Graphique en aires<br>![](images/en/image9-metrics.png){ width="350" style="display:inline-block"}</td>
      </tr>
    </tbody>
	</table>
	</div>

	| Type de graphique du widget | Affichage des données |
	| :----------: | :----------: |
	| Histogramme | Sépare la plage numérique des données en intervalles de largeur égale, appelés classes. Le nombre de classes affichées correspond au nombre de classes configurées dans la page de métrique. L'axe X affiche la variable mesurée dans les données, et l'axe Y affiche la superficie mesurée en km<sup>2</sup> de la zone d'intérêt.
	| Graphique en aires | Pour les données continues, sépare également la plage numérique en classes correspondant au nombre de classes configurées dans la page de métrique. Pour les données catégorielles, des classes distinctes sont utilisées. L'axe X affiche le temps, et l'axe Y affiche le pourcentage de la superficie totale de la zone d'intérêt.
	| Diagramme circulaire | Mesure la couverture proportionnelle de chaque classe catégorielle dans la zone d'intérêt et affiche les classes en secteurs s'additionnant à 100 %.
	| Diagramme en barres | Affiche chaque classe catégorielle sous forme de barre, l'axe X affichant les données mesurées et l'axe Y affichant la superficie mesurée en km<sup>2</sup> de la zone d'intérêt.
	| Graphique linéaire | Représente la variation de la moyenne entre différents jeux de données continues dans une métrique de série temporelle. L'axe X affiche le temps, et l'axe Y affiche la valeur moyenne mesurée de chaque jeu de données.

	f)i)	*Widget summary (optional)* : Crée un résumé des statistiques clés de la métrique qui sera affiché par le widget. La liste des champs de résumé disponibles fournit des paramètres pouvant être utilisés dans le texte du résumé. Un exemple de résumé de widget pour une métrique de série temporelle utilisant trois couches montrant l'Indice de modification humaine sur trois périodes est présenté ci-dessous.

	![](images/en/image10-metrics.png)

	Lorsque cette métrique et son widget associé sont actifs sur UNBL, les champs de résumé dans le texte sont automatiquement remplis avec les paramètres nécessaires, comme illustré ci-dessous.

	![](images/en/image11-metrics.png){style="width:550px !important" }

	Par ailleurs, le résumé du widget pour une métrique à couche unique de l'Indice de modification humaine utiliserait les champs de résumé suivants :

	- {location} : le nom du lieu actuel
	- {areaKm2} : la superficie cartographiée en kilomètres carrés
	- {mean} : la valeur moyenne en tant que nombre brut

	Le résumé du widget ressemblerait donc à quelque chose comme :

		"In {location}, {areaKm2} square kilometers had a mean HMI score of {mean} in 2022."

	![](images/en/image12-metrics.png){style="width:550px !important" }

	f)ii) Pour les métriques créées à l'aide d'une couche catégorielle, une option de basculement supplémentaire est disponible, appelée *Use layer categories*.

	L'option est activée par défaut et spécifie que le graphique de widget catégoriel doit utiliser les mêmes catégories de couche que celles configurées dans la couche raster associée sur UNBL. Si les utilisateurs souhaitent utiliser des catégories de graphique de widget différentes de celles spécifiées dans la couche raster associée, ils doivent désactiver l'option *Use layer categories*. Cela invitera l'utilisateur à spécifier une liste exhaustive de catégories à utiliser dans le graphique, chaque catégorie nécessitant les paramètres suivants :

	- *Label* : Le nom de la catégorie

	- *Colour* : Un sélecteur de couleur pour choisir la couleur à utiliser pour afficher la catégorie associée dans le graphique du widget

	- *Values* : Numéro(s) unique(s) dans la couche de données sous-jacente désignant la catégorie configurée. L'utilisateur peut spécifier plusieurs numéros pour la même catégorie.

	![](images/en/image12_5-metrics.png)

	g)	*X-Axis Label (optional)* : Pour les types de métriques avec histogramme, graphique linéaire ou graphique en aires, il est possible de spécifier un libellé pour l'axe X. Le libellé doit correspondre à la variable de données affichée. Ce champ est optionnel.

	h)	*X-Axis Unit (optional)* : Pour les types de métriques avec histogramme, graphique linéaire ou graphique en aires, il est possible de spécifier les unités de la variable de données. Ce champ est optionnel.

	i)	Une fois tous les paramètres spécifiés, le bouton « SAVE AND VIEW DETAILS » s'affichera en bleu, à condition que toutes les informations saisies soient valides. Cliquez sur ce bouton pour créer le widget.

	![](images/en/image13-metrics.png)

4. Dans la page de modification de widget qui apparaît, activez le bouton « Published » pour publier le widget.

## Étape 5 : Créer un tableau de bord

Un tableau de bord est l'interface utilisateur qui affiche la métrique et le widget associé dans la vue cartographique UNBL lors de la sélection d'un lieu pour consulter les métriques. Il est important de noter que les utilisateurs peuvent créer autant de métriques et de widgets qu'ils le souhaitent, mais ils peuvent tous être placés dans le même tableau de bord. Il est donc nécessaire de créer un seul tableau de bord pour toutes les métriques qu'un utilisateur souhaite configurer. Si votre espace de travail ne contient pas encore de tableau de bord, suivez ces étapes pour en créer un :

1.	Cliquez sur le bouton « Home » dans la page d'administration de votre espace de travail pour développer le menu déroulant. Sélectionnez « Dashboards ».

2.	Cliquez sur le bouton « CREATE NEW DASHBOARD » qui apparaît.

	![](images/en/image14-metrics.png)

3. Dans la page de nouveau tableau de bord, renseignez les informations suivantes :

	a)	*Title* : Le tableau de bord doit avoir un nom qui définit clairement un groupe thématique pour les métriques personnalisées qui y sont liées. Par exemple, un tableau de bord peut contenir toutes les métriques personnalisées définies par un utilisateur particulier et s'appeler « Custom metrics by *user x* ». Ou bien un tableau de bord peut contenir des métriques destinées à être consultées pour un lieu spécifique, par exemple « Custom metrics for *country x* ».

	b)	*Dashboard slug* : Un slug est un identifiant unique pour le tableau de bord dans votre espace de travail. Vous ne pouvez pas avoir plusieurs tableaux de bord avec le même slug dans votre espace de travail. Il ne doit contenir que des lettres, des chiffres et des tirets (« - »). Vous pouvez utiliser le bouton « GENERATE SLUG NAME » pour générer un identifiant unique basé sur le titre de tableau de bord fourni.

	c)	*Dashboard description* : Vous pouvez fournir une brève description du groupe de métriques ici. Par exemple, « This dashboard contains custom metrics defined by *user x*. » Ce champ est optionnel.

	d)	*Included Widgets* : Choisissez le(s) widget(s) à inclure dans ce tableau de bord.

	e)	Une fois tous les paramètres spécifiés, le bouton « SAVE AND VIEW DETAILS » s'affichera en bleu, à condition que toutes les informations saisies soient valides. Cliquez sur ce bouton pour créer le tableau de bord.

	![](images/en/image15-metrics.png)

4. Dans la page de modification de tableau de bord qui apparaît, activez le bouton « Published » pour publier le tableau de bord.

!!!note
	Si vous disposez déjà d'un tableau de bord et souhaitez y ajouter des widgets nouvellement configurés, vous pouvez modifier votre tableau de bord existant et ajouter des widgets inclus en cliquant sur l'icône de crayon du tableau de bord dans la liste des entrées de tableau de bord qui apparaissent dans la page d'administration après avoir choisi « Dashboards » dans le menu déroulant.

## Consultation des tableaux de bord et des widgets

Pour consulter vos métriques personnalisées :

1. Dans la vue cartographique UNBL, assurez-vous que votre espace de travail est activé.

	![](images/en/image16-metrics.png){style="width:600px !important" }

2. Sélectionnez le lieu pour lequel vous souhaitez consulter des métriques personnalisées dans l'onglet « PLACES ».

	![](images/en/image17-metrics.png){style="width:400px !important" }

3. Si la plateforme publique UNBL et/ou d'autres espaces de travail sont actifs en même temps que votre propre espace de travail, vous devrez peut-être sélectionner le tableau de bord contenant vos métriques personnalisées. Dans ce cas, un menu déroulant apparaîtra avec une liste de tableaux de bord et les espaces de travail associés de chaque tableau de bord. Sélectionnez votre tableau de bord dans le menu déroulant.

	!!!note
		Si les métriques personnalisées de votre tableau de bord ne chevauchent pas le lieu activé, votre tableau de bord n'apparaîtra pas dans le menu déroulant.

	![](images/en/image18-metrics.png){style="width:400px !important" }

4. Vous pouvez maintenant consulter les métriques personnalisées que vous avez configurées pour votre couche GeoTIFF et votre lieu choisis. Toutes les fonctionnalités des métriques dynamiques par défaut d'UNBL sont disponibles pour vos métriques personnalisées, notamment le basculement de la couche associée, la consultation d'informations et le téléchargement de données de métriques aux formats .csv, .tsv et .json.

	![](images/en/image19-metrics.png)
