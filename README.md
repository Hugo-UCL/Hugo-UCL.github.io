# Linguistic

# Summary

1. **Data acquisition**
   - Understand the data you will use during this project.

2. **Data tokenization**
   - Build and use a tokenizer according to the given instructions.

3. **Data labeling**
   - Complete a function to label the tokenized data.

4. **Compute metrics**
   - Complete a function to obtain the metrics used to evaluate the model.

5. **Training – validation loop**
   - Complete the loop used to train and evaluate the model.

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


- print("Label for question-context at index 4242:", (start_position, end_position)) : : (247, 250) (token index)
- print(train_dataset["offset_mapping"][4242][start_position]) :  [1077, 1078] (character index)
- print(train_dataset["offset_mapping"][4242][end_position]) : [1080, 1081] (character index)
- print(train_dataset["contexts_th"][4242])
- print("Rep tokenized",train_dataset["answers_th"][4242]) : 6.3%

'offset_mapping': [ None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, [0, 5], [6, 15], [16, 18], [19, 22], [23, 27], [28, 32], [32, 33], [34, 43], [44, 46], [47, 50], [51, 55], [56, 62], [62, 63], [64, 70], [71, 75], [76, 80], [81, 84], [85, 92], [92, 93], [94, 101], [102, 106], [107, 110], [111, 119], [120, 126], [127, 129], [130, 133], [134, 143], [144, 147], [148, 151], [152, 159], [159, 160], [161, 164], [165, 169], [170, 178], [179, 182], [183, 190], [191, 196], [197, 202], [203, 213], [214, 216], [217, 220], [221, 222], [222, 223], [223, 224], [224, 225], [226, 230], [231, 237], [237, 238], [239, 242], [243, 246], [247, 251], [252, 256], [257, 264], [265, 267], [268, 274], [275, 277], [278, 282], [283, 285], [286, 289], [290, 295], [295, 296], [296, 297], [298, 305], [306, 311], [312, 320], [321, 331], [332, 335], [336, 339], [340, 347], [348, 351], [351, 354], [355, 356], [356, 365], [365, 366], [367, 374], [374, 377], [377, 378], [379, 387], [387, 388], [389, 392], [393, 400], [400, 401], [401, 402], [403, 414], [415, 417], [418, 421], [422, 428], [429, 435], [435, 436], [437, 440], [441, 443], [444, 448], [449, 452], [453, 457], [458, 464], [464, 468], [469, 476], [477, 482], [483, 487], [488, 490], [491, 494], [495, 500], [500, 501], [502, 505], [506, 513], [514, 524], [525, 536], [537, 540], [541, 548], [548, 549], [549, 556], [557, 568], [569, 571], [572, 575], [576, 580], [581, 586], [586, 587], [588, 596], [597, 607], [608, 610], [611, 614], [615, 623], [624, 633], [634, 643], [644, 645], [645, 646], [646, 647], [647, 648], [648, 649], [649, 650], [650, 651], [652, 654], [655, 663], [664, 665], [665, 666], [666, 667], [667, 668], [668, 669], [669, 670], [670, 671], [671, 672], [672, 673], [674, 677], [678, 684], [685, 686], [686, 689], [689, 693], [693, 694], [695, 701], [702, 703], [703, 704], [704, 705], [705, 706], [706, 707], [707, 708], [708, 709], [709, 710], [711, 714], [715, 723], [724, 726], [727, 740], [741, 746], [747, 749], [749, 754], [754, 755], [755, 756], [757, 762], [763, 767], [768, 777], [778, 785], [786, 794], [795, 799], [800, 808], [809, 815], [816, 822], [823, 824], [824, 825], [825, 826], [826, 827], [827, 828], [829, 831], [832, 836], [837, 843], [844, 845], [845, 846], [846, 847], [847, 848], [848, 849], [850, 852], [853, 856], [857, 860], [861, 865], [866, 878], [879, 885], [886, 889], [890, 893], [894, 898], [899, 904], [905, 909], [910, 916], [917, 920], [921, 924], [925, 937], [938, 941], [942, 945], [946, 953], [954, 964], [964, 965], [966, 978], [978, 979], [980, 983], [984, 989], [989, 990], [990, 995], [996, 1003], [1004, 1015], [1016, 1025], [1026, 1030], [1031, 1034], [1035, 1039], [1040, 1044], [1045, 1048], [1049, 1060], [1061, 1066], [1066, 1067], [1068, 1070], [1071, 1075], [1075, 1076], [1077, 1078], [1078, 1079], [1079, 1080], [1080, 1081], [1082, 1084], [1085, 1088], [1089, 1093], [1094, 1098], [1099, 1102], [1103, 1105], [1106, 1113], [1114, 1123], [1123, 1124], [1125, 1129], [1130, 1136], [1137, 1142], [1142, 1143], [1143, 1149], [1149, 1150], [1151, 1157], [1158, 1160], [1161, 1167], [1168, 1174], [1175, 1177], [1178, 1186], [1186, 1187], [1188, 1202], [1203, 1205], [1206, 1210], [1211, 1217], [1217, 1218], [1219, 1220], [1221, 1230], [1231, 1240], [1241, 1243], [1243, 1244], [1244, 1247], [1248, 1254], [1254, 1255], [1255, 1262], [1263, 1264], [1264, 1268], [1268, 1271], [1271, 1273], [1273, 1274], [1275, 1276], [1276, 1283], [1283, 1284], [1285, 1286], [1286, 1287], [1287, 1288], [1288, 1289], [1290, 1292], [1293, 1297], [1297, 1299], [1299, 1301], [1301, 1302], [1303, 1304], [1304, 1310], [1310, 1311], [1312, 1315], [1315, 1316], [1316, 1317], [1318, 1320], [1321, 1329], [1330, 1332], [1333, 1334], [1334, 1337], [1337, 1341], [1341, 1342], [1343, 1349], [1349, 1350], [1351, 1356], [1357, 1360], [1361, 1365], [1366, 1370], [1371, 1373], [1374, 1378], [1379, 1383], [1384, 1386], [1387, 1390], [1391, 1398], [1399, 1406], [1407, 1417], [1418, 1425], [1426, 1431], [1431, 1432], [1433, 1438], [1438, 1439], [1440, 1443], [1444, 1449], [1449, 1450], [1451, 1455], [1456, 1464], [1465, 1467], [1468, 1474], [1474, 1475], [1476, 1483], [1484, 1488], [1489, 1491], [1492, 1493], [1493, 1494], [1494, 1495], [1495, 1496], [1497, 1499], [1500, 1503], [1504, 1508], [1508, 1509], [1509, 1510], [1511, 1521], [1521, 1522], [1523, 1526], [1527, 1535], [1536, 1537], [1537, 1538], [1538, 1539], [1539, 1540], [1540, 1541], [1542, 1550], [1550, 1551], [1552, 1556], [1557, 1560], [1561, 1568], [1569, 1578], [1579, 1584], [1585, 1591], [1592, 1597], [1598, 1600], [1601, 1602], [1602, 1603], [1603, 1604], [1604, 1605], [1605, 1606], [1607, 1615], [1616, 1618], [1619, 1629], None ],

- print("Label for question-context at index 4242:", (start_position, end_position)) : : (247, 250) (token index) soit [1077, 1078], [1078, 1079], [1079, 1080], [1080, 1081]
# Finding the character 
print(text[1077],text[1078],text[1079],text[1080]) : 6.3% SAME GOOD
L'indice 5 est "exclusif", cela signifie que dans le tuple `[0, 5]` d'un `offset_mapping`, la position 5 n'est pas incluse dans le segment de texte représenté par ce tuple. Asian = `[0, 5]`


In the early 1900s, James J. Hill of the Great Northern began promoting settlement in the Montana prairie to fill his trains with settlers and goods. Other railroads followed suit. In 1902, the Reclamation Act was passed, allowing irrigation projects to be built in Montana's eastern river valleys. In 1909, Congress passed the Enlarged Homestead Act that expanded the amount of free land from 160 to 320 acres (0.6 to 1.3 km2) per family and in 1912 reduced the time to "prove up" on a claim to three years. In 1916, the Stock-Raising Homestead Act allowed homesteads of 640 acres in areas unsuitable for irrigation.  This combination of advertising and changes in the Homestead Act drew tens of thousands of homesteaders, lured by free land, with World War I bringing particularly high wheat prices. In addition, Montana was going through a temporary period of higher-than-average precipitation. Homesteaders arriving in this period were known as "Honyockers", or "scissorbills." Though the word "honyocker", possibly derived from the ethnic slur "hunyak," was applied in a derisive manner at homesteaders as being "greenhorns", "new at his business" or "unprepared", the reality was that a majority of these new settlers had previous farming experience, though there were also many who did not.

[None, None, None, None, None, None, None, None, None, None, None, [0, 2], [3, 6], [7, 12], [13, 18], [18, 19], [20, 25], [26, 27], [27, 28], [29, 33], [34, 36], [37, 40], [41, 46], [47, 55], [56, 61], [62, 71], [72, 82], [83, 85], [86, 89], [90, 97], [98, 105], [106, 108], [109, 113], [114, 117], [118, 124], [125, 129], [130, 138], [139, 142], [143, 148], [148, 149], [150, 155], [156, 165], [166, 174], [175, 179], [179, 180], [181, 183], [184, 188], [188, 189], [190, 193], [194, 196], [196, 205], [206, 209], [210, 213], [214, 220], [220, 221], [222, 230], [231, 241], [242, 250], [251, 253], [254, 256], [257, 262], [263, 265], [266, 273], [273, 274], [274, 275], [276, 283], [284, 289], [290, 297], [297, 298], [299, 301], [302, 306], [306, 307], [308, 316], [317, 323], [324, 327], [328, 330], [330, 333], [333, 336], [337, 346], [347, 350], [351, 355], [356, 364], [365, 368], [369, 375], [376, 378], [379, 383], [384, 388], [389, 393], [394, 397], [398, 400], [401, 404], [405, 410], [411, 412], [412, 413], [413, 414], [414, 415], [416, 418], [419, 420], [420, 421], [421, 422], [423, 425], [425, 426], [426, 427], [428, 431], [432, 438], [439, 442], [443, 445], [446, 450], [451, 458], [459, 462], [463, 467], [468, 470], [471, 472], [472, 477], [478, 480], [480, 481], [482, 484], [485, 486], [487, 492], [493, 495], [496, 501], [502, 507], [507, 508], [509, 511], [512, 516], [516, 517], [518, 521], [522, 527], [527, 528], [528, 531], [531, 535], [536, 545], [546, 549], [550, 557], [558, 567], [567, 568], [569, 571], [572, 575], [576, 581], [582, 584], [585, 590], [591, 593], [593, 596], [596, 601], [602, 605], [606, 616], [616, 617], [619, 623], [624, 635], [636, 638], [639, 650], [651, 654], [655, 662], [663, 665], [666, 669], [670, 679], [680, 683], [684, 688], [689, 693], [694, 696], [697, 706], [707, 709], [710, 719], [719, 722], [722, 723], [724, 728], [728, 729], [730, 732], [733, 737], [738, 742], [742, 743], [744, 748], [749, 754], [755, 758], [759, 760], [761, 769], [770, 782], [783, 787], [788, 793], [794, 800], [800, 801], [802, 804], [805, 813], [813, 814], [815, 822], [823, 826], [827, 832], [833, 840], [841, 842], [843, 852], [853, 859], [860, 862], [863, 869], [869, 870], [870, 874], [874, 875], [875, 882], [883, 896], [896, 897], [898, 907], [907, 910], [911, 919], [920, 922], [923, 927], [928, 934], [935, 939], [940, 945], [946, 948], [949, 950], [950, 953], [953, 955], [955, 960], [960, 961], [961, 962], [963, 965], [966, 967], [967, 968], [968, 971], [971, 974], [974, 978], [978, 979], [979, 980], [980, 981], [982, 988], [989, 992], [993, 997], [998, 999], [999, 1001], [1001, 1003], [1003, 1006], [1006, 1008], [1008, 1009], [1009, 1010], [1011, 1019], [1020, 1027], [1028, 1032], [1033, 1036], [1037, 1043], [1044, 1045], [1045, 1047], [1047, 1048], [1049, 1050], [1050, 1051], [1051, 1053], [1053, 1055], [1055, 1056], [1056, 1057], [1057, 1058], [1059, 1062], [1063, 1070], [1071, 1073], [1074, 1075], [1076, 1079], [1079, 1082], [1082, 1084], [1085, 1091], [1092, 1094], [1095, 1104], [1104, 1107], [1108, 1110], [1111, 1116], [1117, 1118], [1118, 1123], [1123, 1127], [1127, 1128], [1128, 1129], [1129, 1130], [1131, 1132], [1132, 1135], [1136, 1138], [1139, 1142], [1143, 1151], [1151, 1152], [1153, 1155], [1156, 1157], [1157, 1159], [1159, 1160], [1160, 1162], [1162, 1165], [1165, 1167], [1167, 1168], [1168, 1169], [1170, 1173], [1174, 1181], [1182, 1185], [1186, 1190], [1191, 1192], [1193, 1201], [1202, 1204], [1205, 1210], [1211, 1214], [1215, 1223], [1224, 1227], [1228, 1236], [1237, 1244], [1245, 1255], [1255, 1256], [1257, 1263], [1264, 1269], [1270, 1274], [1275, 1279], [1280, 1284], [1285, 1288], [1289, 1292], [1293, 1296], [1296, 1297], None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None]


Label for question-context at index 1203: (16, 19)
- print(train_dataset["offset_mapping"][1203][start_position_1203]) : [20, 25]
- print(train_dataset["offset_mapping"][1203][end_position_1203]) : [29, 33]
Questiobn tokenized 57342802d058e614000b6a40
Rep tokenized James J. Hill



# part 2

Inference (1)
Consider the model trained on three epochs (the result of Question 3) and use it to predict the answer on a novel question+context pair(not in the squad dataset):

Question: "How can I use the fine-tuned SQuAD model to answer a new question given a context?"

Context: "The SQuAD model is a question-answering model that has been fine-tuned on the Stanford Question Answering Dataset (SQuAD). It can be used to answer new questions given a context. The context is a piece of text that contains the information needed to answer the question. To use the model, you need to pass the question and the context to the model's prediction function. The model will then return the answer to the question based on the information in the context."


# Fonctionement d'un modèle de question réponse
```python
train_dataloader = DataLoader(
    train_set,
    collate_fn=default_data_collator,
    shuffle=True,
    batch_size=8
)

val_dataloader = DataLoader(
    validation_set,
    collate_fn=default_data_collator,
    batch_size=8
)
```
```python
import torch

for epoch in range(num_train_epochs):
    # Training

    # This instruction sets the model to training mode (as opposed to evaluation mode)
    model.train()

    for step, batch in enumerate(train_dataloader):

        # Get ouputs and loss from the model
        outputs = model(**batch)
        loss = outputs.loss

        # Backpropagation
        #############################################
        #############accelerate part#################
        accelerator.backward(loss)
        #############end of accelerate part##########
        #############################################

        # TODO: Update model parameters (i.e. weights and biases of the model) by performing an optimization step
        # Hint: look at optimizer
        optimizer.step()

        # TODO: Update learning rate
        # Hint: look at lr_scheduler
        lr_scheduler.step()

        # TODO: Zero gradients to prevent gradient accumulation
        optimizer.zero_grad()

        # (Optional, if you want a progress bar)
        # progress_bar.update(1)

    # Evaluation

    # This instruction sets the model to evaluation mode (as opposed to training mode)
    model.eval()

    # This initializes lists to store the predicted logits for start and end positions
    start_logits = []
    end_logits = []


    for batch in val_dataloader:
        with torch.no_grad():
            outputs = model(**batch)

        start_logits.append(accelerator.gather(outputs.start_logits).cpu().numpy())
        end_logits.append(accelerator.gather(outputs.end_logits).cpu().numpy())

    start_logits = np.concatenate(start_logits)
    end_logits = np.concatenate(end_logits)
    start_logits = start_logits[: len(validation_dataset)]
    end_logits = end_logits[: len(validation_dataset)]


    metrics = compute_metrics(
        start_logits, end_logits, validation_dataset
    )
    print(f"epoch {epoch}:", metrics)
```

**Entraînement** :

`model.train()` met le modèle en mode entraînement. Cela affecte certaines couches du modèle, comme les couches de dropout ou de normalisation par batch, pour qu'elles se comportent différemment pendant l'entraînement par rapport à l'évaluation.


1.  Calcul des Sorties et de la Perte :
   - `outputs = model(**batch)` passe les données actuelles (batch) au modèle pour obtenir les prédictions (outputs).
   - `loss = outputs.loss` calcule la perte (loss), qui mesure l'erreur entre les prédictions du modèle et les réponses attendues.

2.  Rétropropagation : `accelerator.backward(loss)` effectue la rétropropagation de la perte à travers le modèle. Cela calcule les gradients de la perte par rapport à chaque paramètre du modèle. `accelerator` peut être un outil pour optimiser cette opération sur plusieurs GPU ou pour d'autres améliorations de performance.

3. Mise à Jour des Paramètres du Modèle : `optimizer.step()` met à jour les paramètres (poids et biais) du modèle en fonction des gradients calculés. C'est ce qu'on appelle une étape d'optimisation.

4. Mise à Jour du Taux d'Apprentissage : `lr_scheduler.step()` ajuste le taux d'apprentissage, souvent pour le diminuer au fil du temps, ce qui peut aider à améliorer la précision de l'entraînement.

5.  Réinitialisation des Gradients :`optimizer.zero_grad()` réinitialise les gradients accumulés. Cela empêche l'accumulation de gradients d'un lot à l'autre, ce qui pourrait mener à des erreurs dans l'entraînement.




**Evaluation** :
Après avoir activé le mode évaluation avec `model.eval()`:

```python
for batch in val_dataloader:
    with torch.no_grad():
        outputs = model(**batch)
```

Ici, `model(**batch)` est l'endroit où le modèle est utilisé. Pour chaque lot (`batch`) de données de l'ensemble de validation (`val_dataloader`), le modèle fait des prédictions. `with torch.no_grad()` est utilisé pour désactiver le calcul des gradients, car pendant l'évaluation, on ne veut pas que le modèle modifie ses poids, ce qui est essentiel pendant la phase d'entraînement.

```python
 for batch in val_dataloader:
     with torch.no_grad():
         outputs = model(**batch)

     start_logits.append(accelerator.gather(outputs.start_logits).cpu().numpy())
     end_logits.append(accelerator.gather(outputs.end_logits).cpu().numpy())
```


Pour chaque question posée au modèle, il examine le texte de contexte fourni et calcule un ensemble de logits pour le début et un autre ensemble pour la fin de la réponse potentielle. Le modèle produit ces logits en analysant le texte, en tenant compte de la manière dont les mots sont utilisés et reliés les uns aux autres. Au cours de ces calculs, il s'appuie sur ses couches internes, basées sur l'architecture BERT, et, au terme de ce processus, produit les logits correspondants.

Dans un modèle de réponse aux questions, il y aura typiquement deux ensembles de logits : un pour les positions de début potentielles de la réponse dans le texte, et un autre pour les positions de fin.  Dans notre cas, `outputs.start_logits` et `outputs.end_logits`.



**Création de `Dataset` pour Hugging Face** :
   - Lorsque `.map` est appliqué avec `batched=True`, il traite les données en utilisant des lots et retourne un nouvel objet `Dataset` avec les modifications apportées.
   - L'utilisation de `remove_columns=dataset.column_names` dans `.map` supprime les colonnes originales, ne conservant que les nouvelles colonnes ajoutées ou modifiées par la fonction `labeling_dataset`.
cette approche est spécifiquement conçue pour traiter de grands ensembles de données où vous avez plusieurs exemples (comme des questions et des contextes) que vous voulez transformer en même temps.



Le tokenizer de Hugging Face, lorsqu'il est utilisé pour traiter une paire de séquences (comme une question et un contexte dans le cas d'un modèle de réponse aux questions), combine ces deux séquences en une seule entrée pour le modèle. Cela signifie que les données tokenisées ne contiendront pas de champs distincts pour la "question" et le "contexte". Au lieu de cela, elles sont fusionnées en une seule séquence, avec des tokens spéciaux (comme `[SEP]` dans BERT et ses variantes) pour marquer la séparation entre les deux.

Voici pourquoi et comment cela fonctionne :

1. **Fusion de la Question et du Contexte** :
   - Lors de la tokenisation de la paire question-contexte, le tokenizer fusionne ces deux éléments en une seule séquence pour créer un format approprié pour le modèle de réponse aux questions.
   - Par exemple, dans le cas de BERT et de ses variantes, la séquence résultante serait quelque chose comme `[CLS] question [SEP] contexte [SEP]`.

2. **Pas de Champ 'question' dans `tokenized_data`** :
   - C'est pourquoi vous ne verrez pas de champ distinct pour la "question" dans `tokenized_data`. Au lieu de cela, vous obtenez des champs comme `input_ids` et `attention_mask`, qui représentent la séquence combinée de la question et du contexte.

3. **Utilisation des Données Tokenisées** :
   - Les `input_ids` sont des identifiants de tokens qui représentent la question et le contexte. Les modèles de réponse aux questions utilisent cette séquence combinée pour comprendre la question dans le contexte donné et trouver la réponse appropriée.
   - Les autres champs comme `attention_mask` et `offset_mapping` sont utilisés pour gérer les aspects tels que les tokens de padding et le mappage des tokens aux positions d'origine dans le texte.

Si vous avez besoin d'identifier séparément la question et le contexte après la tokenisation, vous devrez utiliser les marqueurs spéciaux (comme `[SEP]`) ou `offset_mapping` pour déterminer où se termine la question et où commence le contexte dans les `input_ids`. Cependant, pour la plupart des modèles de réponse aux questions, cette distinction n'est pas nécessaire une fois les données tokenisées, car le modèle traite la séquence combinée dans son ensemble.

