Aquí se pueden encontrar los códigos de los graficos hechos en R:

# **PCA (Principal Component Analysis)**
## Carga de las librerías y dependencia para la visualización
```{r}
library(ggplot2)
library(stringr)
```

## Eigenvalues
### Carga de los datos de eigenvalues: 
```{r}
Val <- read.table("C:\\UC\\2024_2\\Genómica de la Conservación\\Penguin2-LD-0.1.eigenval")
Val$PC <- c(1:10)  #en el código anterior eran 20 filas, ahora son 10
colnames(Val) <- c("percent","PC") #renombrar las columnas PC=Principal Component y percent su porcentaje de contribución en los datos
```
#### Agregamos la columna "clasificación" que diferencia a las 4 especies:
Agregamos la columna de clasificación según las tres primeras letras del ID:
```{r}
Vec$Classification <- substr(Vec$ID, 1, 3)
```

### Graficamos  los eigenvalues en Barplot o Histogramas 
```{r}
ggplot(Val[1:5,], aes(x=Val[1:5,]$PC, y=Val[1:5,]$percent), xlab = "PC") +
  geom_bar(stat = "identity", width = 0.5) +
  labs(y= "% variance", x = "PC")+
  lims(y=c(0,10)) +
  scale_y_continuous(breaks = c(0,2,4,6,8,10))
```

## Eigenvectors
### Cargar los datos de eigenvectors y cambiar ubicación
```{r}
Vec <- read.table("C:\\UC\\2024_2\\Genómica de la Conservación\\Penguin2-LD-0.1.eigenvec")  
Vec <- Vec[,(1:5), drop=FALSE] #guardar los 5 primeros vectores
colnames(Vec)<-c("ID","Species","PCA1","PCA2","PCA3") #renombrar las columnas
```
#### Agregamos nuevamente la columna "clasificación" que diferencia a las 4 especies:
Agregamos la columna de clasificación según las tres primeras letras del ID:
```{r}
Vec$Classification <- substr(Vec$ID, 1, 3)
```

## Representación grafica 
### Graficamos los Componentes 1 y 2, es decir los ejes PC1 y PC2
```{r}
p1 <- ggplot (Vec, aes(x= PCA1, y= PCA2, color = Classification))+  
  geom_point(size=4)+  
  theme_classic()+  
  labs(x="PC1 (4.44%)", y="PC2 (2.84%)") +  
  theme(axis.title.x = element_text(face="bold", vjust=0, size=rel(1.5))) +  
  theme(axis.title.y = element_text(face="bold", vjust=1.5, size=rel(1.5)))+  
  theme(panel.border = element_rect(colour="black", fill=NA, size=1)) 

p2 <- p1 + scale_colour_gradient(low = c("#CA3F3F","#64AD3F","#E8B547")) #creación de un gradiente de color para representar las especies
```

### Graficamos los Componentes 2 y 3, es decir PC3 en función de PC2:
```{r}
p3 <- ggplot (Vec, aes(x= PCA2, y= PCA3, color = Classification))+
  geom_point(size=4)+
  theme_gray()+
  labs(x="PC2 (1.99%)", y="PC3 (1.8%)") +
  theme(axis.title.x = element_text(face="bold", vjust=0, size=rel(1.5))) +
  theme(axis.title.y = element_text(face="bold", vjust=1.5, size=rel(1.5))) +
  theme(panel.border = element_rect(colour="black", fill=NA, size=1))

p4 <- p3 + scale_colour_gradient(low = c("#CA3F3F","#64AD3F","#E8B547")) #creación de un gradiente de color para representar las especies
```
![PCA](https://github.com/user-attachments/assets/766538c6-e644-404f-9c38-7f50c65c6c22)


