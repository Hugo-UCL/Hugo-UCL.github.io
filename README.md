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



**Utilisation de `sample_idx`**: Dans votre boucle `for`, vous parcourez chaque fragment tokenisé (représenté ici par `offset`). Pour chaque fragment, vous utilisez `sample_idx = sample_map[i]` pour obtenir l'index de l'entrée originale du dataset d'où ce fragment provient. `i` est l'index du fragment dans la tokenisation, et `sample_map[i]` est l'index de l'entrée correspondante dans le dataset original.

**Enregistrement du Contexte et de la Réponse Théorique** :
   - `contexts_th.append(contexts[sample_idx])` : Cette ligne ajoute le contexte correspondant à l'index `sample_idx` à la liste `contexts_th`. `contexts[sample_idx]` récupère le contexte de la paire question-contexte originale (avant la tokenisation et le découpage en fragments). `contexts_th` sera donc une liste de contextes associés à chaque fragment.




text = ("Asian Americans in New York City, according to the 2010 Census, number more than one million, "
        "greater than the combined totals of San Francisco and Los Angeles. New York contains the highest total Asian "
        "population of any U.S. city proper. The New York City borough of Queens is home to the state's largest Asian "
        "American population and the largest Andean (Colombian, Ecuadorian, Peruvian, and Bolivian) populations in the "
        "United States, and is also the most ethnically diverse urban area in the world. The Chinese population "
        "constitutes the fastest-growing nationality in New York State; multiple satellites of the original Manhattan "
        "Chinatown (紐約華埠), in Brooklyn (布鲁克林華埠), and around Flushing, Queens (法拉盛華埠), are thriving as traditionally "
        "urban enclaves, while also expanding rapidly eastward into suburban Nassau County (拿騷縣) on Long Island (長島), as "
        "the New York metropolitan region and New York State have become the top destinations for new Chinese immigrants, "
        "respectively, and large-scale Chinese immigration continues into New York City and surrounding areas. In 2012, "
        "6.3% of New York City was of Chinese ethnicity, with nearly three-fourths living in either Queens or Brooklyn, "
        "geographically on Long Island. A community numbering 20,000 Korean-Chinese (Chaoxianzu (Chinese: 朝鲜族) or "
        "Joseonjok (Hangul: 조선족)) is centered in Flushing, Queens, while New York City is also home to the largest Tibetan "
        "population outside China, India, and Nepal, also centered in Queens. Koreans made up 1.2% of the city's population, "
        "and Japanese 0.3%. Filipinos were the largest Southeast Asian ethnic group at 0.8%, followed by Vietnamese, who "
        "made up 0.2% of New York City's population in 2010. Indians are the largest South Asian group, comprising 2.4% "
        "of the city's population, with Bangladeshis and Pakistanis at 0.7% and 0.5%, respectively. Queens is the "
        "preferred borough of settlement for Asian Indians, Koreans, and Filipinos, as well as Malaysians and other "
        "Southeast Asians; while Brooklyn is receiving large numbers of both West Indian as well as Asian Indian immigrants.")


- print("Label for question-context at index 4242:", (start_position, end_position)) : : (247, 250)
- print(train_dataset["offset_mapping"][4242][start_position]) :  [1077, 1078] (character index)
- print(train_dataset["offset_mapping"][4242][end_position]) : [1080, 1081] (character index)
- print(train_dataset["contexts_th"][4242])
- print("Rep tokenized",train_dataset["answers_th"][4242]) : 6.3%


# Finding the character at index 1077
- char_at_1077 = text[1078]
- print(char_at_1077)
- ##print
- ##6
