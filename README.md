# Taller alineamientos y creación árboles

Dentro de este documento se encuentra el análisis correspondiente a lo requerido para el taller número 2.

## Pasos previos a la realización de los árboles

Previo a la obtención de los árboles, las secuencias dadas fueron curadas usando el programa Geneious. Estas correspondían a secuencias de los genes citocromo oxidasa 1 (COI) y a ITS. En las secuencias iniciales se encontraban una secuencia forward y una reverse, de las cuales se obtenía al final una secuencia consenso. Luego de obtener estas secuencias consenso, se realizó un [BLAST](https://blast.ncbi.nlm.nih.gov/Blast.cgi) con los archivos que contenían las secuencias consenso. Cabe resaltar que los archivos de las secuencias consenso fueron separados de acuerdo al gen al que pertenecían, es decir, que se creó una carpeta para las secuencias consenso de COI y otra para las secuencias de ITS.

El análisis con BLAST fue un paso importante previo a la construcción de los árboles. Gracias a los resultados de este algoritmo, se determinaron los grupos que iban a ser usados como grupos externos en la construcción de los árboles, por lo que sus respectivas secuencias fueron pegadas dentro del archivo de secuencias consenso. Se realizó una corrida de BLAST para cada uno de los genes presentados en este taller, y fueron tomados 5 grupos externos que correspondían a los primeros 5 resultados del análisis, exceptuando cuando había resultados que correspondían a un mismo organismo. Sin embargo, en el caso de ITS se hizo una excepción, ya que todos los resultados del BLAST correspondían a un mismo organismo, por lo que los 5 grupos externos seleccionados, a pesar de que son secuencias diferentes, corresponden a un mismo organismo.

Con las secuencias de los outgroups contenidas dentro de las secuencias consenso de cada uno de los genes, se procedió a realizar el alineamiento usando dos algoritmos: [ClustalW](http://www.clustal.org/) y [MUSCLE](https://www.ebi.ac.uk/Tools/msa/muscle/). Ninguno de los criterios presentados por los algoritmos fue modificado a excepción del archivo de salida, donde se seleccionó en ambos casos un archivo de salida en formato FASTA. Los archivos de texto plano con las secuencias se encuentran en ese [link](https://github.com/malvaradol/taller_2/tree/master/secuencias_originales).

## Análisis árboles

Luego de obtener los alineamientos con los algoritmos mencionados anteriormente, se procedió a realizar la construcción de los árboles. Para este fin, se utilizó el programa [IQTree](http://www.iqtree.org/). Luego de obtener los árboles correspondientes a cada gen y a cada algoritmo utilizado (dos genes, con dos árboles por método. En total cuatro árboles), se utilizó el programa [FigTree](https://github.com/rambaut/figtree/) para realizar el análisis y la modifiación de todos los árboles obtenidos. En las siguientes secciones se presentan los árboles obtenidos y un breve análisis general de los resultados obtenidos.

### Árboles con ClustalW

Tal como se explicó en la sección anterior, se obtuvieron dos árboles con este algoritmo, uno para COI y otro para ITS. Los resultados fueron los siguientes:

[Árbol obtenido con ClustalW de las secuencias del gen COI](https://github.com/malvaradol/taller_2/blob/master/alineamientos_COI_outgroups_clustal.fasta.treefile.pdf).

[Árbol obtenido con ClustalW de las secuencias del gen ITS](https://github.com/malvaradol/taller_2/blob/master/alineamientos_ITS_outgroups_clustal.fasta.treefile.pdf)

### Árboles con MUSCLE

Los árboles obtenidos con el algoritmo MUSCLE se encuentran a continuación:

[Árbol obtenido con MUSCLE de las secuencias del gen COI](https://github.com/malvaradol/taller_2/blob/master/alineamientos_COI_outgroups_muscle.fasta.treefile.pdf).

[Árbol obtenido con MUSCLE de las secuencias del gen ITS](https://github.com/malvaradol/taller_2/blob/master/alineamientos_ITS_outgroups_muscle.fasta.treefile.pdf).

### Análisis de resultados

Al comparar los resultados de los árboles obtenidos para cada gen con los diferentes algoritmos, encontramos que hay diferencias en cuanto a la organización de las ramas de los grupos externos y de las secuencias consenso. Estas diferencias pueden ser explicadas en base a las diferencias de los cálculos realizados por los algoritmos basados en la matriz de distancias, donde vemos que MUSCLE es un algoritmo mucho más exhaustivo, y genera dos matrices de distancias, una para generar un primer resultado, y la otra matriz que se usa para refinar el primer resultado obtenido. Este fenómeno causado por las matrices de distancia también pueden afectar la escala que se genera en cada una de las gráficas, donde se puede apreciar que en todos los árboles las tasas de sustitución por cada 100 nucleótidos son diferentes.

Por otro lado, y apartando la distinción entre algoritmos, encontramos que en todos los árboles hay dos grupos monofiléticos grandes, donde uno está conformado por los grupos externos, y el otro por los grupos internos o las secuencias de interés del taller. Las organizaciones internas de las ramas varía entre algoritmos, pero la estructura de grupos monofiléticos es conservada a traves de todos los árboles.

### Explicación breve funcionamiento algoritmos

Clustal

El algoritmo original de clustal tiene tres pasos principales. El primer paso consta de realizar el cálculo de todas las similitudes de las secuencias por pares, que luego se usan para construir una matriz de distancias. Luego de obtener tal matriz, esta es usada para la construcción de un dendograma o árbol guía, de donde se obtendrán unos agrupamientos iniciales. Gracias a estos agrupamientos obtenidos con el dendograma, será posible realizar el alineamiento múltiple de las secuencias de manera pareada, siguiendo el orden de los agrupamientos del dendograma. Finalmente se obtendrán los alineamientos múltiples calculados por el algoritmo. Cabe resaltar que ClustalW introdujo cambios debido a que el algoritmo Clustal inicial hacia ciertos supuestos que no eran biológicamente realistas como: la introducción del uso de una suma de bases ponderada, el uso de penalizaciones por gaps variables, y el uso de neighbor-joining en vez de UPGMA para la construcción del árbol filogenético o dendograma base.

MUSCLE

Hay tres pasos principales dentro del algoritmo MUSCLE. El primer paso corresponde a la generación de un alineamiento progresivo de las secuencias. La distancia k-meros para cada par de secuencias input es calculada, y una matriz es construida a partir de esto. La matriz es usada para construir el árbol UPGMA. Del árbol, el alineamiento progresivo es construido siguiendo el orden de ramificación del árbol. En cada hoja (taxón actual o punta de la rama) se construye un perfil a partir de las secuencias que fueron introducidas. Los nodos en el árbol son recorridos en orden de prefijo (descendientes antes de su ancestro). En cada nodo interno, un alineamiento pareado es construido de los dos perfiles de los descendientes, dandole un nuevo perfil que es asignado a ese nodo. Esto produce un alineamiento múltiple de secuencias

REFERENCIAS

https://pdfs.semanticscholar.org/7be2/08bb025d696aecedca2da8100d13b64a06f1.pdf

El archivo en forma de documento se puede encontrar [aquí](https://github.com/malvaradol/taller_2/blob/master/explicacionalgoritmos.odt).
