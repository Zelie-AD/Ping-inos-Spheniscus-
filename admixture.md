

# Análisis de Estructura hallar número de clústers más probables y el grado de mezcla


## Convertir a formato plink (recode 12) admisible y reconocible para admixture 
```
plink --vcf Spheniscus.vcf --recode 12 --out archivo2-ADMX --allow-extra-chr
```


## Pruning, primer paso. Se estima cuales están bajo desequilibrio de ligamiento. Aquí cualquier SNP que se correlacione con un r de 0.1 o mayor será eliminado
```
plink --file archivo2-ADMX --indep-pairwise 50 5 0.1 --allow-extra-chr
```

## Pruning, segundo paso. Se extraen los snps que no presentan desequilibrio de ligamiento.
```
plink --file archivo2-ADMX --extract plink.prune.in --recode --out archivo3-ADMX --allow-extra-chr 0
```

## Con este loop probaremos  distintos valores de K (cantidad de poblaciones ancestrales), con K={1 2 3 4 5 6 7 8 9 10}.
> +  El flag '--cv' permite evaluar varios K con una 'validación cruzada'. Calculan el error estandar de la validacion cruzada para cada K, y el de menor valor indica que tiene mayor se considera el más probable.

```
for K in 1 2 3 4 5 6 7 8 9 10
do
admixture --cv archivo3-ADMX.ped $K -j8 | tee log${K}.out
done
``` 
## Para observar los distintos K ocupamos el comando *grep*
```
grep -h CV *.out > ID_log_admix.txt
```

Para graficar los resultados ocuparemos una herramienta en línea llamada pophelper:

http://pophelper.com/
