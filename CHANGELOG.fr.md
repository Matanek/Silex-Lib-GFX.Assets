# Historique

## [0.4.0] - 2026-11-08

- Adopte la déclaration `Result` unique : l’appel ordinaire rend la valeur
  ou arrête le programme sur erreur ; `try appel()` conserve le `Result`. Les
  anciens appels stockant un résultat et propagations deviennent explicites.
  Cette version requiert Silex 0.48.0 ou supérieur et les versions coordonnées
  de ses dépendances.
  Les variantes récupérables `try_*` et wrappers directs de même signature
  sont réunis sous le nom ordinaire. Les variantes booléennes, optionnelles ou
  natives gardent leur contrat.

- Le plugin Assets accepte les magasins hérités dans un Bundle et ne les libère pas à l’arrêt du contexte enfant.
