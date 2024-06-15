# Биоинформатика Проект. Индивидуальная часть. Leishmania Enrietti
### Порошин Илья Группа 2

### Краткое описание
Leishmania enriettii — вид жгутиконосных паразитических протистов рода Leishmania. Естественная инфекция этим паразитом изредка регистрируется в Бразилии только у домашних морских свинок. Они непатогенны для человека. Он демонстрирует высокую фенотипическую пластичность, будучи способным заражать различных позвоночных хозяев и переносчиков

### Коротко о файлах:
- Папка [ncbi_dataset](/ncbi_dataset/data) содержит в себе файлы скачанные с NCBI: полный геном, аннотации генов, белков и тп
- Файл [dnaSeq.txt](./dnaSeq.txt) содержит в себе полную геномную последовательность в чистом виде (одна большая строка нуклеотидов)
- Файл [epigenetic_genes_table.csv](./epigenetic_genes_table.csv) содержит таблицу генов из семейств ответственных за эпигенетику
- Файл [hmmer_out.csv](./hmmer_out.csv) содержит вывод hmmer'a переведенный в формат csv
- Файл [text_predictions.txt](./text_predictions.txt) содержит предикты z днк от zdnabert
- Файл [zhunt_results_filtered.csv](./zhunt_results_filtered.csv) содержит предикты z днк от zhunt

### 1) Таблица Семейств и генов (это все-все найденные. [первоначальный файл](./epigenetic_genes_table.csv))

|Family|Genes|
|------|-----|
|Histone deacetylase domain|KAG5469763.1, KAG5478271.1, KAG5480156.1, KAG5480302.1, KAG5485144.1|
|Class II histone deacetylase complex subunits 2 and 3|KAG5469797.1, KAG5477611.1|
|Histone methylation protein DOT1|KAG5469805.1, KAG5480781.1, KAG5485174.1|
|Histone chaperone Rttp106-like|KAG5470830.1, KAG5473945.1|
|Glutamine rich N terminal domain of histone deacetylase 4|KAG5470894.1|
|Histone methyltransferase Tudor domain 1|KAG5473941.1|
|Histone acetyltransferase subunit NuA4|KAG5476762.1|
|"6-O-methylguanine DNA methyltransferase, DNA binding domain"|KAG5476764.1|
|ASF1 like histone chaperone|KAG5480641.1, KAG5482449.1|
|Histone deacetylase complex subunit SAP130 C-terminus|KAG5485115.1|

### 2) Таблица с попаданиями z-dna и квадруплексов

| Участок | Число квадруплексов | Доля квадруплексов | Число предсказаний Zhunt (10k) | Доля предсказаний Zhunt(10k) | Число предсказаний Zhunt (1k) | Доля предсказаний Zhunt(1k) | Число предсказаний ZDNABERT | Доля предсказаний ZDNABERT |
|---------|---------------------|--------------------|--------------------------------|------------------------------|-------------------------------|-----------------------------|-----------------------------|----------------------------|
|Exons|196|0.044|4781|0.257|19351|0.327|44259|0.43|
|Introns|-|-|-|-|-|-|-|-|
|Promoters|1812|0.41|6250|0.336|17722|0.299|23303|0.227|
|Downstream|347|0.078|976|0.052|3150|0.053|4755|0.046|
|Intergenic|2119|0.48|6923|0.37|20252|0.34|32868|0.32|
|Genes|269|0.06|5123|0.27|20413|0.345|45685|0.44|

## Код:

### Подготовка Hmmer'ом
https://colab.research.google.com/drive/1BFKGx1rUWv6WnSrDJv6U7dZGd0oZdH_G#scrollTo=yIURD4UEsP9L

### Zhunt
```py
!gcc zhunt3-alan.c -lm -o zhunt3
!./zhunt3 12 8 12 GCA_017916305.1_LU_Lenr_1.0_genomic.fna

import pandas as pd

# Чтение результатов ZHUNT
results_file = "/content/GCA_017916305.1_LU_Lenr_1.0_genomic.fna.Z-SCORE"
columns = ["Position", "Z-score"]
df = pd.read_csv(results_file, sep="\t", names=columns)
df = df[df]
# Выводим первые несколько строк таблицы
print(df.head())

# Сохранение результатов в CSV
df.to_csv("zhunt_results.csv", index=False)

f = open("zhunt_results.csv", 'r')
out_f = open("zhunt_results_filtered.csv", 'w')

for line in f:
  if len(line.split()) < 6:
    out_f.write(line)
    continue

  z_score = float(line.split()[5])
  if (z_score > 1000):
    out_f.write(line)
```

### Аналитика
[Основные расчеты по z-dna и квадруплексам в колабе](https://colab.research.google.com/drive/1Dy8Ya2R6eeVx73GLKnE_5-9ydi8wYrGB?usp=sharing)
