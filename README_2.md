# Gestion de Coffres-Forts en Python

## Objectif

L'objectif de cet exercice est de modéliser un système bancaire capable de gérer plusieurs coffres-forts.

Chaque coffre possède une capacité maximale de stockage exprimée en kilogrammes.

La banque doit être capable de stocker différents types d'actifs physiques :

| Actif           | Poids |
| --------------- | ----- |
| Espèce          | 1 kg  |
| Lingot d'argent | 2 kg  |
| Lingot d'or     | 3 kg  |

Le programme doit :

1. Créer une banque contenant 3 coffres.
2. Affecter les capacités suivantes :

   * Coffre 1 : 25 kg
   * Coffre 2 : 15 kg
   * Coffre 3 : 10 kg
3. Stocker :

   * 1 lingot d'or
   * 1 lingot d'argent
   * 1 espèce

   dans le coffre de 15 kg.
4. Tenter de stocker 4 lingots d'or dans le coffre de 10 kg.
5. Afficher la capacité résiduelle de chaque coffre.

---

# Choix de conception

Le programme utilise la programmation orientée objet (POO).

Les concepts métiers sont représentés par des classes :

* `Actif`
* `Espece`
* `LingotArgent`
* `LingotOr`
* `Coffre`
* `Banque`

Cette approche permet de facilement ajouter de nouveaux types d'actifs ou de nouveaux comportements dans le futur.

---

# Code complet

```python
# ==========================================
# Classe de base représentant un actif
# ==========================================

class Actif:
    def __init__(self, poids):
        self.poids = poids


# ==========================================
# Types d'actifs
# ==========================================

class Espece(Actif):
    def __init__(self):
        super().__init__(1)


class LingotArgent(Actif):
    def __init__(self):
        super().__init__(2)


class LingotOr(Actif):
    def __init__(self):
        super().__init__(3)


# ==========================================
# Coffre-fort
# ==========================================

class Coffre:
    def __init__(self, capacite_max):
        self.capacite_max = capacite_max
        self.actifs = []

    def poids_occupe(self):
        """
        Retourne le poids total actuellement stocké.
        """
        return sum(actif.poids for actif in self.actifs)

    def capacite_residuelle(self):
        """
        Retourne l'espace restant disponible.
        """
        return self.capacite_max - self.poids_occupe()

    def stocker(self, actif):
        """
        Stocke un actif uniquement si la capacité le permet.
        """
        if self.capacite_residuelle() >= actif.poids:
            self.actifs.append(actif)
            return True

        return False


# ==========================================
# Banque
# ==========================================

class Banque:
    def __init__(self):
        self.coffres = []

    def ajouter_coffre(self, coffre):
        self.coffres.append(coffre)


# ==========================================
# Programme principal
# ==========================================

# Création de la banque
banque = Banque()

# Création des 3 coffres
coffre1 = Coffre(25)
coffre2 = Coffre(15)
coffre3 = Coffre(10)

# Ajout des coffres à la banque
banque.ajouter_coffre(coffre1)
banque.ajouter_coffre(coffre2)
banque.ajouter_coffre(coffre3)

# ------------------------------------------
# Stockage dans le coffre de 15 kg
# ------------------------------------------

coffre2.stocker(LingotOr())
coffre2.stocker(LingotArgent())
coffre2.stocker(Espece())

# ------------------------------------------
# Tentative de stockage de 4 lingots d'or
# dans le coffre de 10 kg
# ------------------------------------------

for i in range(4):
    succes = coffre3.stocker(LingotOr())

    if not succes:
        print(
            f"Lingot d'or n°{i + 1} refusé : "
            "capacité insuffisante."
        )

# ------------------------------------------
# Affichage des capacités résiduelles
# ------------------------------------------

for numero, coffre in enumerate(banque.coffres, start=1):
    print(
        f"Coffre {numero} : "
        f"{coffre.capacite_residuelle()} kg restants"
    )
```

---

# Explication détaillée

## 1. Création des actifs

La classe `Actif` est une classe générique contenant uniquement le poids.

```python
class Actif:
    def __init__(self, poids):
        self.poids = poids
```

Les autres classes héritent de cette classe :

```python
class LingotOr(Actif):
    def __init__(self):
        super().__init__(3)
```

Cela signifie qu'un lingot d'or possède automatiquement un poids de 3 kg.

---

## 2. Gestion d'un coffre

Chaque coffre possède :

```python
self.capacite_max
```

qui représente sa capacité maximale.

et :

```python
self.actifs
```

qui contient les actifs stockés.

---

## 3. Calcul du poids occupé

```python
def poids_occupe(self):
    return sum(actif.poids for actif in self.actifs)
```

Exemple :

```text
1 lingot d'or = 3 kg
1 lingot d'argent = 2 kg
1 espèce = 1 kg

Poids total = 6 kg
```

---

## 4. Calcul de la capacité restante

```python
def capacite_residuelle(self):
    return self.capacite_max - self.poids_occupe()
```

Exemple :

```text
Capacité = 15 kg
Poids occupé = 6 kg

15 - 6 = 9 kg
```

---

## 5. Contrôle de dépassement

Avant chaque stockage :

```python
if self.capacite_residuelle() >= actif.poids:
```

Le programme vérifie que l'actif peut être ajouté.

Ainsi, aucun coffre ne peut dépasser sa capacité maximale.

---

# Vérification des résultats

## Coffre 1

Aucun actif stocké.

```text
25 - 0 = 25 kg
```

Capacité restante :

```text
25 kg
```

---

## Coffre 2

Contient :

```text
1 lingot d'or      = 3 kg
1 lingot d'argent  = 2 kg
1 espèce           = 1 kg
```

Total :

```text
6 kg
```

Capacité restante :

```text
15 - 6 = 9 kg
```

---

## Coffre 3

Tentative :

```text
4 lingots d'or
```

Poids demandé :

```text
4 × 3 = 12 kg
```

Capacité disponible :

```text
10 kg
```

Le programme accepte seulement :

```text
3 lingots d'or = 9 kg
```

Le quatrième est refusé.

Capacité restante :

```text
10 - 9 = 1 kg
```

---

# Sortie obtenue

```text
Lingot d'or n°4 refusé : capacité insuffisante.
Coffre 1 : 25 kg restants
Coffre 2 : 9 kg restants
Coffre 3 : 1 kg restants
```

---

# Complexité

| Opération                        | Complexité |
| -------------------------------- | ---------- |
| Stockage d'un actif              | O(1)       |
| Ajout d'un coffre                | O(1)       |
| Calcul du poids occupé           | O(n)       |
| Calcul de la capacité résiduelle | O(n)       |

où `n` représente le nombre d'actifs présents dans un coffre.

---

# Conclusion

Cette solution applique les principes de la programmation orientée objet pour modéliser une banque, ses coffres-forts et les différents actifs stockés. Elle garantit le respect des contraintes de capacité et fournit un système facilement extensible pour de futurs besoins.
