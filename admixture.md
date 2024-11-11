

# Análisis de Estructura hallar número de clústers más probables y el grado de mezcla

Intentaremos visualizar la Admixture (mezcla genética) de nuestros datos, es decir graficar las recombinaciones genéticas dentro de grupo y a lo largo de varias generaciones.

## Convertimos los datos para la admixture
```
plink --vcf Spheniscus.vcf --recode 12 --out archivo2-ADMX --allow-extra-chr # convertir a formato plink (recode 12) admisible y reconocible para admixture 
```


## Se estima cuales están bajo desequilibrio de ligamiento para hacer el primer paso de Pruning (poda)
```
plink --file archivo2-ADMX --indep-pairwise 50 5 0.1 --allow-extra-chr #cualquier SNP que se correlacione con un r de 0.1 o mayor será eliminado
```

## Se extraen los snps que no presentan desequilibrio de ligamiento.  para hacer el secundo paso de Pruning 
```
plink --file archivo2-ADMX --extract plink.prune.in --recode --out archivo3-ADMX --allow-extra-chr 0
```

## Con este loop probaremos  distintos valores de K (cantidad de poblaciones ancestrales), con K={1 2 3 4 5 6 7 8 9 10}.
> +  El flag '--cv' permite evaluar varios K con una 'validación cruzada'. Calculan el error estandar de la validación cruzada para cada K, y el de menor valor indica que tiene mayor se considera el más probable.

```
for K in 1 2 3 4 5 6 7 8 9 10
do
admixture --cv archivo3-ADMX.ped $K -j8 | tee log${K}.out
done #loop que permite probar escenarios con diferentes valores de K
``` 
## Para observar los distintos K ocupamos el comando *grep*
```
grep -h CV *.out > ID_log_admix.txt
```
El K eligido de acuerdo con nuestros datos es 4 -> tenemos 4 poblaciones 

## Utilización de la herramienta pophelper para graficar los resultados:

http://pophelper.com/ 
Con esto, podemos visualisar los resultados de Admixture y eligir varias opciones gráficas como las colores, las leyendas de los Barplots, el ordenamiento de las poblaciones...
El gráfico obtenido es el siguiente:

![Capture d'écran 2024-11-11 032404](https://github.com/user-attachments/assets/3fc56ff7-622d-48cd-b9d1-5333c116fe9b)


