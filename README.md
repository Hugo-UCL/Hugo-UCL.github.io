# Linguistic

**Gestion des Tokens Overflow** : La fonction traite les cas où une paire question-context dépasse la longueur maximale autorisée. Elle conserve le mapping entre ces fragments débordants et les exemples originaux à l'aide de `sample_map`.

`overflow_to_sample_mapping`. Imaginons que nous ayons un dataset de deux phrases très longues nécessitant une tokenisation en plusieurs fragments. Voici les phrases et leurs fragments respectifs :

1. **Phrase 0 (index 0)** : "Le renard brun rapide saute par-dessus le chien paresseux."
   - Fragment 0.1 : "Le renard brun rapide"
   - Fragment 0.2 : "rapide saute par-dessus"
   - Fragment 0.3 : "par-dessus le chien"
   - Fragment 0.4 : "le chien paresseux."

2. **Phrase 1 (index 1)** : "La lune brillait clairement dans le ciel nocturne."
   - Fragment 1.1 : "La lune brillait"
   - Fragment 1.2 : "brillait clairement dans"
   - Fragment 1.3 : "dans le ciel"
   - Fragment 1.4 : "le ciel nocturne."

Avec `overflow_to_sample_mapping = [0, 0, 0, 0, 1, 1, 1, 1]`, le mapping est le suivant :

- `[0, 0, 0, 0]` indique que les fragments 0.1, 0.2, 0.3, et 0.4 appartiennent tous à la Phrase 0.
- `[1, 1, 1, 1]` indique que les fragments 1.1, 1.2, 1.3, et 1.4 appartiennent tous à la Phrase 1.


