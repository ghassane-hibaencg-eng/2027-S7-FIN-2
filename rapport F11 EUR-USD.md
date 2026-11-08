<div align="center">

<img src="figures/logo_uh1.png" alt="Université Hassan 1er" height="110"> &nbsp;&nbsp;&nbsp;&nbsp; <img src="figures/logo_encg.png" alt="ENCG Settat" height="110">

**Université Hassan 1er**
**École Nationale de Commerce et de Gestion Settat**

# Projet Data Science

## Volatilité du change euro-dollar : le passé permet-il de prévoir l'amplitude des variations à venir ?

<img src="figures/couverture.png" alt="Dollar - Euro" width="380">

**Réalisé par :** *(noms des étudiants)*

**Encadré par :** M. Abderrahim LARHLIMI

</div>

---

# Prévoir l'amplitude des variations du taux de change EUR/USD

## Table des matières

1. [Introduction et problème](#1-introduction-et-problème)
2. [Méthode](#2-méthode)
3. [Découverte : la volatilité est persistante](#3-découverte--la-volatilité-est-persistante)
4. [Résultat : le GARCH(1,1) est le meilleur modèle](#4-résultat--le-garch11-est-le-meilleur-modèle)
5. [Limites et pistes d'amélioration](#5-limites-et-pistes-damélioration)
6. [Conclusion](#conclusion)

---

## 1. Introduction et problème

Les variations passées du taux de change permettent-elles de prévoir l'amplitude des variations à venir ? La question est importante pour mesurer le risque de change et dimensionner une couverture. Le sens d'un mouvement (hausse ou baisse) est quasi imprévisible ; en revanche, l'ampleur des fluctuations pourrait garder de la mémoire. Le problème est donc de prévoir, un jour à l'avance, l'amplitude de la variation de l'EUR/USD et non sa direction.

Pour cela, on mesure l'amplitude par la valeur absolue du rendement $|r|$, qui sert de proxy de la volatilité, et on compare quatre approches : la volatilité historique moyenne (référence), une régression linéaire, un Gradient Boosting et un modèle GARCH(1,1).

## 2. Méthode

Les données utilisées proviennent de la Réserve fédérale américaine, à travers la base FRED (série DEXUSEU), et représentent le nombre de dollars pour un euro, relevé chaque jour ouvré du 4 janvier 1999 au 30 octobre 2026. Les 281 jours sans cotation, correspondant principalement aux jours fériés, sont supprimés, ce qui permet d'obtenir 6 978 rendements logarithmiques, calculés selon la formule :

$$r_t = 100 \times \ln\left(\frac{S_t}{S_{t-1}}\right)$$

Sur cette période, le taux de change varie entre 0,83 et 1,60 dollar, sans présence de valeurs aberrantes. Enfin, le rendement absolu moyen s'établit à 0,42 % par jour, avec un écart-type de 0,57 %.

<p align="center">
  <img src="figures/fig1_serie.png" alt="Taux de change, rendements et valeur absolue des rendements" width="85%">
</p>

*Figure 1 – Taux de change EUR/USD (panneau 1), rendement logarithmique quotidien en % (panneau 2) et valeur absolue du rendement $|r|$, proxy de la volatilité (panneau 3).*

### Lecture de la figure 1

- **Taux (panneau 1).** L'euro part d'environ 1,17 $ en 1999, avant de tomber à 0,83 $ vers 2000-2001, puis remonte jusqu'à 1,60 $ à la mi-2008, avant d'osciller entre 1,05 et 1,25 $ depuis 2015. Deux autres ruptures sont visibles : la baisse brutale de 2014-2015, de 1,39 à 1,05 $, ainsi que le passage sous la parité en 2022, avec un niveau d'environ 0,96 $. Ces variations importantes justifient l'utilisation des rendements plutôt que des prix.
- **Rendements (panneau 2).** Les rendements oscillent autour de zéro sans tendance particulière, mais leur amplitude varie au cours du temps. Elle est relativement faible entre 2005-2007, 2013-2014 et 2017-2019, tandis qu'elle devient beaucoup plus importante en 2008-2009. La volatilité n'est donc pas constante dans le temps.
- **Valeurs absolues des rendements (panneau 3).** Les valeurs élevées se regroupent sur certaines périodes, notamment en 2008-2009 (maximum de 4,6 %), puis en 2010-2012, 2015-2016, 2020 et 2022-2023. Une journée marquée par une forte variation tend ainsi à être suivie d'autres journées agitées : c'est le phénomène de regroupement de la volatilité.

## 3. Découverte : la volatilité est persistante

Si le passé aide à prévoir l'amplitude, c'est que la volatilité doit avoir de la mémoire. L'autocorrélation du rendement est presque nulle (0,008 au retard 1) : le passé ne dit rien du signe. Celle de $|r|$ est au contraire positive et décroît très lentement (environ 0,10 aux retards 2 à 10, 0,13 à 22 jours et encore 0,09 à 60 jours).

<p align="center">
  <img src="figures/fig2_autocorrelation.png" alt="Autocorrélation de r et de |r|" width="100%">
</p>

*Figure 2 – Autocorrélation de $r$ (gauche, quasi nulle) et de $|r|$ (droite, positive et persistante).*

Le graphique de gauche montre que les rendements ne présentent presque aucune autocorrélation, car la plupart des barres restent dans la bande de confiance : le rendement d'aujourd'hui ne permet pas de prévoir celui de demain. En revanche, le graphique de droite montre une autocorrélation positive des valeurs absolues des rendements, qui diminue lentement, même après 60 jours. Les fortes variations ont donc tendance à se regrouper, tout comme les périodes de faible volatilité.

Les régimes de marché le confirment, avec une volatilité annualisée de 16,8 % pendant la crise financière de 2008-2009 et de 10 % pendant la crise de la dette et la divergence entre la BCE et la Fed, contre 6 à 7 % entre 2017 et 2020 et depuis 2023. La volatilité conserve ainsi une mémoire relativement longue, ce qui justifie de prévoir les variations futures à partir des valeurs absolues des rendements passés.

## 4. Résultat : le GARCH(1,1) est le meilleur modèle

| Modèle                               | RMSE (%) | QLIKE   | RMSE/référence | Gain RMSE (%) |
|--------------------------------------|:--------:|:-------:|:--------------:|:-------------:|
| **GARCH(1,1)**                       | **0,3154** | **−0,5592** | **0,928**  | **7,21**      |
| Régression linéaire                  | 0,3173   | −0,5429 | 0,934          | 6,65          |
| Gradient boosting                    | 0,3173   | −0,5392 | 0,934          | 6,65          |
| Volatilité historique moyenne (réf.) | 0,3400   | −0,3669 | 1,000          | 0,00          |

*Tableau 1 – Performances sur le jeu de test 2015-2026 (plus bas = mieux).*

<p align="center">
  <img src="figures/fig3_metriques.png" alt="RMSE et QLIKE par modèle" width="100%">
</p>

*Figure 3 – Comparaison des modèles sur le jeu de test : RMSE (gauche) et QLIKE (droite). La ligne pointillée indique la référence.*

**Interprétation.** Les résultats montrent que les trois modèles intégrant les informations passées surpassent le modèle de référence fondé sur la volatilité historique moyenne. Le modèle GARCH(1,1) obtient les meilleures performances, avec un RMSE de 0,3154 et un QLIKE de −0,5592, soit une amélioration de 7,21 % du RMSE par rapport à la référence. La régression linéaire et le gradient boosting affichent des résultats quasiment identiques, ce qui suggère que la relation entre les variations passées et la volatilité future est relativement simple et ne nécessite pas forcément le recours à des modèles complexes. Ainsi, le GARCH(1,1) apparaît comme le modèle le plus performant et le plus adapté à la prévision de la volatilité. Sa supériorité est confirmée par le test de Diebold-Mariano ($p < 0{,}0001$) et reste valable dans les six situations étudiées, y compris lorsque la période du Covid est exclue.

### Volatilité prévue contre volatilité réalisée

<p align="center">
  <img src="figures/fig6_prevision.png" alt="Volatilité prévue contre volatilité réalisée" width="100%"><br><br>
  <img src="figures/fig6_zooms.png" alt="Zooms Covid-19 et 2022" width="100%">
</p>

*Figure 4 – Volatilité prévue la veille contre volatilité réalisée, 2015-2026, avec zooms sur le Covid-19 (2020) et sur 2022 (guerre en Ukraine et parité).*

Le graphique met en évidence la capacité des trois modèles à suivre l'évolution de la volatilité réalisée sur la période 2015-2026. Dans l'ensemble, les prévisions reproduisent correctement les principales tendances, notamment la hausse observée en 2020-2021, le retour à des niveaux plus faibles en 2024-2026 et les fluctuations enregistrées lors des périodes de crise.

Toutefois, certaines différences apparaissent selon les événements étudiés. Durant la période du Covid-19, les modèles sous-estiment le pic de volatilité : leurs prévisions restent autour de 0,25 %, alors que la volatilité réalisée atteint environ 0,77 % en mars 2020. De même, lors de la guerre en Ukraine en 2022, les prévisions tendent à sous-estimer les poussées de volatilité et à réagir avec un certain retard. Ces écarts montrent que, malgré leur capacité à anticiper les tendances générales, les modèles restent moins performants face aux chocs exceptionnels et aux changements brusques des conditions du marché.

## 5. Limites et pistes d'amélioration

- **Source :** relevé quotidien unique de la Fed, pas une clôture à heure fixe. C'est une source de bruit en plus.
- **Jours fériés :** un rendement calculé après un jour férié porte sur deux jours calendaires.
- **Régimes :** définis après coup, donc utilisables pour le diagnostic uniquement, jamais comme variable explicative.
- **Horizon :** un jour seulement.
- **Pistes :** un GARCH asymétrique (GJR, EGARCH) ou une régression HAR ; des variables externes (écart de taux BCE/Fed, VIX) ; des prévisions à 5 ou 22 jours, où le bruit pèse moins ; une volatilité réalisée intrajournalière, bien moins bruitée que $|r|$.

## Conclusion

Oui, mais avec des limites. Le passé aide à prévoir l'amplitude des variations de l'EUR/USD, et c'est statistiquement très solide. En revanche, il n'aide pas à prévoir leur sens.

1. **Source de la prévisibilité.** C'est la persistance de la volatilité (figure 2) : les périodes agitées et calmes durent. Une moyenne sur quelques semaines en capte l'essentiel.
2. **Gain réel mais modeste.** Environ 7 % de RMSE et 0,17 à 0,19 point de QLIKE par rapport à la moyenne historique (tableau 1 et figure 3). Le plafond de qualité de l'oracle est proche de la référence, ce qui limite tout gain.
3. **Choix du modèle.** Un modèle simple suffit : régression linéaire et boosting sont équivalents. Le GARCH(1,1) est le meilleur sur les deux métriques, mais son avantage est marginal.
4. **Chocs imprévisibles.** Les modèles sont les moins utiles quand la prévision compterait le plus (Covid, décisions de banques centrales), car ils réagissent après le choc (figure 4).
5. **Usage.** Utile pour dimensionner un risque ou une couverture, en particulier en période calme. Inutile pour parier sur la direction du change.
