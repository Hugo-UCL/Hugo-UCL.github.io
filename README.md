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


- print("Label for question-context at index 4242:", (start_position, end_position)) : : (247, 250) (token index)
- print(train_dataset["offset_mapping"][4242][start_position]) :  [1077, 1078] (character index)
- print(train_dataset["offset_mapping"][4242][end_position]) : [1080, 1081] (character index)
- print(train_dataset["contexts_th"][4242])
- print("Rep tokenized",train_dataset["answers_th"][4242]) : 6.3%

'offset_mapping': [ None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, None, [0, 5], [6, 15], [16, 18], [19, 22], [23, 27], [28, 32], [32, 33], [34, 43], [44, 46], [47, 50], [51, 55], [56, 62], [62, 63], [64, 70], [71, 75], [76, 80], [81, 84], [85, 92], [92, 93], [94, 101], [102, 106], [107, 110], [111, 119], [120, 126], [127, 129], [130, 133], [134, 143], [144, 147], [148, 151], [152, 159], [159, 160], [161, 164], [165, 169], [170, 178], [179, 182], [183, 190], [191, 196], [197, 202], [203, 213], [214, 216], [217, 220], [221, 222], [222, 223], [223, 224], [224, 225], [226, 230], [231, 237], [237, 238], [239, 242], [243, 246], [247, 251], [252, 256], [257, 264], [265, 267], [268, 274], [275, 277], [278, 282], [283, 285], [286, 289], [290, 295], [295, 296], [296, 297], [298, 305], [306, 311], [312, 320], [321, 331], [332, 335], [336, 339], [340, 347], [348, 351], [351, 354], [355, 356], [356, 365], [365, 366], [367, 374], [374, 377], [377, 378], [379, 387], [387, 388], [389, 392], [393, 400], [400, 401], [401, 402], [403, 414], [415, 417], [418, 421], [422, 428], [429, 435], [435, 436], [437, 440], [441, 443], [444, 448], [449, 452], [453, 457], [458, 464], [464, 468], [469, 476], [477, 482], [483, 487], [488, 490], [491, 494], [495, 500], [500, 501], [502, 505], [506, 513], [514, 524], [525, 536], [537, 540], [541, 548], [548, 549], [549, 556], [557, 568], [569, 571], [572, 575], [576, 580], [581, 586], [586, 587], [588, 596], [597, 607], [608, 610], [611, 614], [615, 623], [624, 633], [634, 643], [644, 645], [645, 646], [646, 647], [647, 648], [648, 649], [649, 650], [650, 651], [652, 654], [655, 663], [664, 665], [665, 666], [666, 667], [667, 668], [668, 669], [669, 670], [670, 671], [671, 672], [672, 673], [674, 677], [678, 684], [685, 686], [686, 689], [689, 693], [693, 694], [695, 701], [702, 703], [703, 704], [704, 705], [705, 706], [706, 707], [707, 708], [708, 709], [709, 710], [711, 714], [715, 723], [724, 726], [727, 740], [741, 746], [747, 749], [749, 754], [754, 755], [755, 756], [757, 762], [763, 767], [768, 777], [778, 785], [786, 794], [795, 799], [800, 808], [809, 815], [816, 822], [823, 824], [824, 825], [825, 826], [826, 827], [827, 828], [829, 831], [832, 836], [837, 843], [844, 845], [845, 846], [846, 847], [847, 848], [848, 849], [850, 852], [853, 856], [857, 860], [861, 865], [866, 878], [879, 885], [886, 889], [890, 893], [894, 898], [899, 904], [905, 909], [910, 916], [917, 920], [921, 924], [925, 937], [938, 941], [942, 945], [946, 953], [954, 964], [964, 965], [966, 978], [978, 979], [980, 983], [984, 989], [989, 990], [990, 995], [996, 1003], [1004, 1015], [1016, 1025], [1026, 1030], [1031, 1034], [1035, 1039], [1040, 1044], [1045, 1048], [1049, 1060], [1061, 1066], [1066, 1067], [1068, 1070], [1071, 1075], [1075, 1076], [1077, 1078], [1078, 1079], [1079, 1080], [1080, 1081], [1082, 1084], [1085, 1088], [1089, 1093], [1094, 1098], [1099, 1102], [1103, 1105], [1106, 1113], [1114, 1123], [1123, 1124], [1125, 1129], [1130, 1136], [1137, 1142], [1142, 1143], [1143, 1149], [1149, 1150], [1151, 1157], [1158, 1160], [1161, 1167], [1168, 1174], [1175, 1177], [1178, 1186], [1186, 1187], [1188, 1202], [1203, 1205], [1206, 1210], [1211, 1217], [1217, 1218], [1219, 1220], [1221, 1230], [1231, 1240], [1241, 1243], [1243, 1244], [1244, 1247], [1248, 1254], [1254, 1255], [1255, 1262], [1263, 1264], [1264, 1268], [1268, 1271], [1271, 1273], [1273, 1274], [1275, 1276], [1276, 1283], [1283, 1284], [1285, 1286], [1286, 1287], [1287, 1288], [1288, 1289], [1290, 1292], [1293, 1297], [1297, 1299], [1299, 1301], [1301, 1302], [1303, 1304], [1304, 1310], [1310, 1311], [1312, 1315], [1315, 1316], [1316, 1317], [1318, 1320], [1321, 1329], [1330, 1332], [1333, 1334], [1334, 1337], [1337, 1341], [1341, 1342], [1343, 1349], [1349, 1350], [1351, 1356], [1357, 1360], [1361, 1365], [1366, 1370], [1371, 1373], [1374, 1378], [1379, 1383], [1384, 1386], [1387, 1390], [1391, 1398], [1399, 1406], [1407, 1417], [1418, 1425], [1426, 1431], [1431, 1432], [1433, 1438], [1438, 1439], [1440, 1443], [1444, 1449], [1449, 1450], [1451, 1455], [1456, 1464], [1465, 1467], [1468, 1474], [1474, 1475], [1476, 1483], [1484, 1488], [1489, 1491], [1492, 1493], [1493, 1494], [1494, 1495], [1495, 1496], [1497, 1499], [1500, 1503], [1504, 1508], [1508, 1509], [1509, 1510], [1511, 1521], [1521, 1522], [1523, 1526], [1527, 1535], [1536, 1537], [1537, 1538], [1538, 1539], [1539, 1540], [1540, 1541], [1542, 1550], [1550, 1551], [1552, 1556], [1557, 1560], [1561, 1568], [1569, 1578], [1579, 1584], [1585, 1591], [1592, 1597], [1598, 1600], [1601, 1602], [1602, 1603], [1603, 1604], [1604, 1605], [1605, 1606], [1607, 1615], [1616, 1618], [1619, 1629], None ],

(247, 250) (token index) [1077, 1078], [1078, 1079], [1079, 1080], [1080, 1081]
# Finding the character 
print(text[1077],text[1078],text[1079],text[1080]) : 6.3% SAME GOOD
L'indice 5 est "exclusif", cela signifie que dans le tuple `[0, 5]` d'un `offset_mapping`, la position 5 n'est pas incluse dans le segment de texte représenté par ce tuple. Asian = `[0, 5]`
