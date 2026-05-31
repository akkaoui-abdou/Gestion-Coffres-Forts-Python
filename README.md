# Exercice : Gestion de coffres-forts bancaires

## Contexte

Une banque dispose de plusieurs coffres-forts destinés au stockage d'actifs physiques. Chaque coffre possède une capacité maximale de stockage exprimée en kilogrammes. Le système doit être capable de gérer différents types d'actifs, chacun ayant un poids spécifique, tout en garantissant qu'aucun coffre ne dépasse sa capacité autorisée.

## Types d'actifs

Les actifs pouvant être stockés sont les suivants :

| Type d'actif    | Poids |
| --------------- | ----- |
| Espèce          | 1 kg  |
| Lingot d'argent | 2 kg  |
| Lingot d'or     | 3 kg  |

## Travail demandé

Développer une application permettant de :

1. Modéliser une banque contenant plusieurs coffres-forts.
2. Associer à chaque coffre une capacité maximale de stockage.
3. Stocker des actifs dans les coffres en respectant leur capacité disponible.
4. Calculer et afficher la capacité restante de chaque coffre après les opérations de stockage.

## Scénario à implémenter

1. Créer une banque contenant **trois coffres-forts** de capacités :

   * Coffre n°1 : 25 kg
   * Coffre n°2 : 15 kg
   * Coffre n°3 : 10 kg

2. Dans le coffre n°2 (15 kg), stocker :

   * 1 lingot d'or
   * 1 lingot d'argent
   * 1 espèce

3. Dans le coffre n°3 (10 kg), tenter de stocker :

   * 4 lingots d'or

4. Afficher la capacité résiduelle de chacun des trois coffres après l'ensemble des opérations.

## Résultat attendu

Le programme doit indiquer la capacité restante de chaque coffre en tenant compte des actifs effectivement stockés et des éventuelles contraintes de capacité.


## Description

Ce projet implémente un système simple de gestion de coffres-forts pour une banque.

La banque possède plusieurs coffres, chacun ayant une capacité maximale exprimée en kilogrammes. Les coffres peuvent contenir différents types d'actifs physiques, chaque actif ayant un poids spécifique.

L'objectif est de garantir qu'un coffre ne puisse jamais dépasser sa capacité maximale.

---

# Règles métier

## Coffres

Chaque coffre possède :

* Une capacité maximale.
* Une liste d'actifs stockés.
* Une capacité résiduelle calculée à partir du poids déjà occupé.

## Actifs

Trois types d'actifs existent :

| Actif           | Poids |
| --------------- | ----- |
| Espèce          | 1 kg  |
| Lingot d'argent | 2 kg  |
| Lingot d'or     | 3 kg  |

---

# Architecture du programme

Le programme est construit en utilisant la programmation orientée objet.

## Classe `Actif`

Classe de base représentant un actif.

```python
class Actif:
    def __init__(self, poids):
        self.poids = poids
```

Cette classe contient uniquement le poids de l'actif.

---

## Classe `Espece`

```python
class Espece(Actif):
    def __init__(self):
        super().__init__(1)
```

Une espèce pèse 1 kg.

---

## Classe `LingotArgent`

```python
class LingotArgent(Actif):
    def __init__(self):
        super().__init__(2)
```

Un lingot d'argent pèse 2 kg.

---

## Classe `LingotOr`

```python
class LingotOr(Actif):
    def __init__(self):
        super().__init__(3)
```

Un lingot d'or pèse 3 kg.

---

## Classe `Coffre`

Cette classe représente un coffre-fort.

```python
class Coffre:
```

### Attributs

* `capacite_max` : poids maximal autorisé.
* `actifs` : liste des actifs stockés.

### Méthodes

#### `poids_occupe()`

Calcule le poids total des actifs présents.

```python
def poids_occupe(self):
    return sum(actif.poids for actif in self.actifs)
```

Exemple :

Si le coffre contient :

* 1 lingot d'or = 3 kg
* 1 lingot d'argent = 2 kg

Alors :

```text
poids occupé = 5 kg
```

---

#### `capacite_residuelle()`

Retourne l'espace restant.

```python
def capacite_residuelle(self):
    return self.capacite_max - self.poids_occupe()
```

Exemple :

```text
Capacité maximale = 15 kg
Poids occupé = 6 kg

Capacité résiduelle = 9 kg
```

---

#### `stocker(actif)`

Ajoute un actif uniquement si le coffre dispose encore d'assez d'espace.

```python
def stocker(self, actif):
    if self.capacite_residuelle() >= actif.poids:
        self.actifs.append(actif)
        return True
    return False
```

Cette vérification évite tout dépassement de capacité.

---

## Classe `Banque`

La banque contient plusieurs coffres.

```python
class Banque:
```

### Attribut

```python
self.coffres
```

Liste de tous les coffres de la banque.

### Méthode

```python
ajouter_coffre()
```

Permet d'ajouter un coffre à la banque.

---

# Déroulement de l'exercice

## Étape 1 : Création des coffres

La banque possède trois coffres :

| Coffre   | Capacité |
| -------- | -------- |
| Coffre 1 | 25 kg    |
| Coffre 2 | 15 kg    |
| Coffre 3 | 10 kg    |

```python
coffre1 = Coffre(25)
coffre2 = Coffre(15)
coffre3 = Coffre(10)
```

---

## Étape 2 : Stockage dans le coffre de 15 kg

On ajoute :

* 1 lingot d'or = 3 kg
* 1 lingot d'argent = 2 kg
* 1 espèce = 1 kg

Calcul :

```text
3 + 2 + 1 = 6 kg
```

Capacité restante :

```text
15 - 6 = 9 kg
```

---

## Étape 3 : Stockage dans le coffre de 10 kg

On tente de stocker :

```text
4 lingots d'or
```

Poids demandé :

```text
4 × 3 = 12 kg
```

Or le coffre ne peut contenir que :

```text
10 kg
```

Le programme refuse automatiquement le 4ème lingot.

Poids réellement stocké :

```text
3 lingots × 3 kg = 9 kg
```

Capacité restante :

```text
10 - 9 = 1 kg
```

---

# Résultat final

## Coffre 1

```text
25 kg restants
```

Aucun actif n'a été stocké.

---

## Coffre 2

```text
15 - 6 = 9 kg restants
```

---

## Coffre 3

```text
10 - 9 = 1 kg restant
```

---

# Sortie attendue

```text
Coffre 1 : 25 kg restants
Coffre 2 : 9 kg restants
Coffre 3 : 1 kg restant
```

---

# Complexité

### Stockage d'un actif

```text
O(1)
```

L'ajout dans une liste est effectué en temps constant.

### Calcul du poids occupé

```text
O(n)
```

où `n` représente le nombre d'actifs présents dans le coffre.

---

# Améliorations possibles

* Ajouter un identifiant unique pour chaque coffre.
* Permettre le retrait d'actifs.
* Gérer plusieurs types de métaux précieux.
* Sauvegarder les données dans un fichier JSON.
* Ajouter des tests unitaires avec `pytest`.
* Afficher le contenu détaillé de chaque coffre.
