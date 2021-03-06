# SNA Reciprocity (Triads and Diads) and Transitivity 15_ACESSO G) O acesso geográfico a este serviço impede a realização de ativades em conjunto. (var13) 
Leonardo Martins  
17 de julho de 2016  
SNA Descritive Analysis from "Projeto Redes de Atenção às pessoas que consomem álcool e outras Drogas em Juiz de Fora-MG   Brazil"  - SNArRDJF

Here you can find a basic script to analysis data from SNArRDJF - this script was elaborated considering its use for orther matrix adjacency data from SNArRDJF - Here we are going to analyse:

# 15_ACESSO G) O acesso geográfico a este serviço impede a realização de ativades em conjunto. (var13) 

`#########################
`# Basic Preparation #####
`#########################

#Loading objects generated with previous script 

```r
rm(list = ls()) # removing previous objects to be sure that we don't have objects conflicts name
load("~/SNArRDJF/Robject/8_spanning_tree_var13.RData")
```
##Reload packages

```r
suppressMessages(library(RColorBrewer))
suppressMessages(library(car))
suppressMessages(library(xtable))
suppressMessages(library(igraph))
suppressMessages(library(miniCRAN))
suppressMessages(library(magrittr))
suppressMessages(library(keyplayer))
suppressMessages(library(dplyr))
suppressMessages(library(feather))
suppressMessages(library(visNetwork))
suppressMessages(library(knitr))
suppressMessages(library(DT))
```
##Adding phantom tools

```r
#In order to get dinamic javascript object install those ones. If you get problems installing go to Stackoverflow.com and type your error to discover what to do. In some cases the libraries need to be intalled in outside R libs.
#devtools::install_github("wch/webshot")
#webshot::install_phantomjs()
```
##Setting a random seed - this is a good strategy to keep the same graph pattern layout in a new report generation

```r
set.seed(123)
```

##Simplify Graph - removing loops and duble edges 

```r
#var13<-simplify(var13) #Simplify
```

#Reciprocity Default
Reciprocity Default - Proportion of mutual connections - probability that hte opposite counterpart of a directed graph is also included

```r
reciprocity(var13, mode="default")
```

```
## [1] 0.4154341
```

#Reciprocity Ratio
Reciprocity Ratio - Probability  of mutual connections between a vertex pair - if we know - probability that hte opposite counterpart of a directed graph is also included in the 

```r
reciprocity(var13, mode="ratio")
```

```
## [1] 0.2621753
```

#Dyad Census
A dyad consists of an unordered pair of actors and links that exist between two actors of the pair classified by mutal non-mutual and no connection in a directed graphs

Dyads are 2-subgraphs where a subgraph is a subset of actors taken from the complete set of network actors and all links
between them. See more here <http://file.scirp.org/pdf/SN_2013012915270187.pdf>

##Dyad Census 
Number of pairs with mutual connections "mut" and number of pairs with non-mutual connections "asym"

```r
dyad.census_var13<-dyad.census(var13)
```
##Mutual connections.

```r
dyad.census_var13_mut<-dyad.census_var13$mut
dyad.census_var13_mut
```

```
## [1] 323
```
##Non-mutual connections.

```r
dyad.census_var13_asym<-dyad.census_var13$asym
dyad.census_var13_asym
```

```
## [1] 909
```
##No connection between them.

```r
dyad.census_var13_null<-dyad.census_var13$null
dyad.census_var13_null
```

```
## [1] 16159
```
#Triad Census - Check this out in order to understand triad lables

The studies about transitivity in social networks led Holland and Leinhardt (1975) to propose that the local structure in social networks can be expressed by the triad census or triad count, the numbers of triads of any kinds.

You can see more here:
<http://www.stats.ox.ac.uk/~snijders/Trans_Triads_ha.pdf>


```r
#Triad Census 
tc_var13 <- triad.census(var13)

#Triad Census Label 
census_labels = c('T.003',
                  'T.012',
                  'T.102',
                  'T.021D',
                  'T.021U',
                  'T.021C',
                  'T.111D',
                  'T.111U',
                  'T.030T',
                  'T.030C',
                  'T.201',
                  'T.120D',
                  'T.120U',
                  'T.120C',
                  'T.210',
                  'T.300')

ordering = c('1',
                  '2',
                  '3',
                  '4',
                  '5',
                  '6',
                  '7',
                  '8',
                  '13',
                  '10',
                  '9',
                  '14',
                  '15',
                  '11',
                  '12',
                  '16')

#Saving in a dataframe for further studies
triad_df_var13 <- data.frame(census_labels,tc_var13)
write.csv(triad_df_var13, "~/SNArRDJF/Banco Redes R/var13_complet_triads.csv")
```
##Triad Census Types 
The following labels gives the 16 different triads for directed graphs. The coding refers to the numbers of mutual, asymmetric, and null dyads, with a further identifying letter: Up, Down, Cyclical, Transitive.

E.g., 1-2-0-D has *1* mutual, *2* asymmetric, *0* null dyads, and the *Down* orientation.

###Describing triads


```r
triad_df_var13
```

```
##    census_labels tc_var13
## 1          T.003   881836
## 2          T.012   120779
## 3          T.102    35919
## 4         T.021D     7289
## 5         T.021U     2663
## 6         T.021C     5120
## 7         T.111D     4210
## 8         T.111U     6506
## 9         T.030T      986
## 10        T.030C       74
## 11         T.201     4723
## 12        T.120D      379
## 13        T.120U      516
## 14        T.120C      372
## 15         T.210      812
## 16         T.300      261
```

###Triads Tables Recoding

```r
#Recoding different types of triads 
triad_df_var13$type[triad_df_var13$census_labels=="T.003"]<-"Vacuously Transitive"
triad_df_var13$type[triad_df_var13$census_labels=="T.012"]<-"Vacuously Transitive"
triad_df_var13$type[triad_df_var13$census_labels=="T.102"]<-"Vacuously Transitive"
triad_df_var13$type[triad_df_var13$census_labels=="T.021D"]<-"Vacuously Transitive"
triad_df_var13$type[triad_df_var13$census_labels=="T.021U"]<-"Vacuously Transitive"
triad_df_var13$type[triad_df_var13$census_labels=="T.021C"]<-"Intransitive"
triad_df_var13$type[triad_df_var13$census_labels=="T.111D"]<-"Intransitive"
triad_df_var13$type[triad_df_var13$census_labels=="T.111U"]<-"Intransitive"
triad_df_var13$type[triad_df_var13$census_labels=="T.030T"]<-"Transitive"
triad_df_var13$type[triad_df_var13$census_labels=="T.030C"]<-"Intransitive"
triad_df_var13$type[triad_df_var13$census_labels=="T.201"]<-"Intransitive"
triad_df_var13$type[triad_df_var13$census_labels=="T.120D"]<-"Transitive"
triad_df_var13$type[triad_df_var13$census_labels=="T.120U"]<-"Transitive"
triad_df_var13$type[triad_df_var13$census_labels=="T.120C"]<-"Intransitive"
triad_df_var13$type[triad_df_var13$census_labels=="T.210"]<-"Intransitive"
triad_df_var13$type[triad_df_var13$census_labels=="T.300"]<-"Transitive"
```

Graphical Ilustration 

![<http://knoesis.wright.edu/sites/default/files/icnc15.pdf>](figures/img.png)

###Triads Tables

```r
datatable(triad_df_var13)
```

<!--html_preserve--><div id="htmlwidget-fa290254df76dbe65af8" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-fa290254df76dbe65af8">{"x":{"filter":"none","data":[["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16"],["T.003","T.012","T.102","T.021D","T.021U","T.021C","T.111D","T.111U","T.030T","T.030C","T.201","T.120D","T.120U","T.120C","T.210","T.300"],[881836,120779,35919,7289,2663,5120,4210,6506,986,74,4723,379,516,372,812,261],["Vacuously Transitive","Vacuously Transitive","Vacuously Transitive","Vacuously Transitive","Vacuously Transitive","Intransitive","Intransitive","Intransitive","Transitive","Intransitive","Intransitive","Transitive","Transitive","Intransitive","Intransitive","Transitive"]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> \u003c/th>\n      <th>census_labels\u003c/th>\n      <th>tc_var13\u003c/th>\n      <th>type\u003c/th>\n    \u003c/tr>\n  \u003c/thead>\n\u003c/table>","options":{"columnDefs":[{"className":"dt-right","targets":2},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

#Transitivity - clustering coefficient

##Transitivity Global
Socialnetwork is treated as an undirected network global - ratio of triangles (direction disregarded) to connected triples.


```r
transitivity(var13, type="global")
```

```
## [1] 0.2505465
```

# Transitivity Local
Ratio of triangles to connected triples each vertex is part of.

```r
V(var13)$transitivity_local<-transitivity(var13, type="local") 
var13_transitivity_local<-transitivity(var13, type="local") #local - ratio of triangles to connected triples each vertex is part of.
```
#Descriptive Statistics for Local Transitivity by Vertex 

```r
summary(var13_transitivity_local[which(var13_transitivity_local != Inf)])
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
##  0.0000  0.2350  0.3333  0.3669  0.4682  1.0000
```

```r
sd(var13_transitivity_local[which(var13_transitivity_local != Inf)])
```

```
## [1] 0.2041274
```

#Barrat's Weighted Transitivity by Edges (Barrat's)

```r
V(var13)$transitivity_barrat<-transitivity(var13, weights=E(var13)$weight, type="barrat")

var13_transitivity_barrat<-transitivity(var13, weights=E(var13)$weight, type="barrat")
```

#Descriptive Statistics for Barrat Weighted Transitivity by Vertex 

```r
summary(var13_transitivity_barrat[which(var13_transitivity_barrat != Inf)])
```

```
##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
## -1.0000  0.4010  0.6364  0.6821  0.9524  3.0000
```

```r
sd(var13_transitivity_barrat[which(var13_transitivity_barrat != Inf)])
```

```
## [1] 0.568737
```

#Transitivity  Measures Dinamic Table

```r
#Getting  Measures

var13_transitivity_local<-transitivity(var13, type="local") %>% round(3)

var13_transitivity_barrat<-transitivity(var13, weights=E(var13)$weight, type="barrat") %>% round(3)

#Creating a datagrame of measures
var13_transitivity_df <- data.frame(var13_transitivity_local,var13_transitivity_barrat) %>% round(3)

#Adding type
var13_transitivity_df <-cbind(var13_transitivity_df, V(var13)$LABEL_COR)

#Adding names
names(var13_transitivity_df) <- c("Local", "Barrat's Weighted","Type")

#Ordering Variables
var13_transitivity_df<-var13_transitivity_df[c("Type", "Local", "Barrat's Weighted")]
```
## General tabel - DT 

```r
datatable(var13_transitivity_df, filter = 'top')
```

<!--html_preserve--><div id="htmlwidget-5ef0a3a691eb9839e7f2" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-5ef0a3a691eb9839e7f2">{"x":{"filter":"top","filterHTML":"<tr>\n  <td>\u003c/td>\n  <td data-type=\"factor\" style=\"vertical-align: top;\">\n    <div class=\"form-group has-feedback\" style=\"margin-bottom: auto;\">\n      <input type=\"search\" placeholder=\"All\" class=\"form-control\" style=\"width: 100%;\"/>\n      <span class=\"glyphicon glyphicon-remove-circle form-control-feedback\">\u003c/span>\n    \u003c/div>\n    <div style=\"width: 100%; display: none;\">\n      <select multiple=\"multiple\" style=\"width: 100%;\" data-options=\"[&quot;Acolhimento Institucional&quot;,&quot;Ajuda Mútua&quot;,&quot;Ambulatório de Saúde Mental&quot;,&quot;Assistência Hospitalar&quot;,&quot;CAPS&quot;,&quot;CAPSAD&quot;,&quot;Consultório na Rua&quot;,&quot;CRAS/CREAS&quot;,&quot;Entidades Assistênciais e Dependencia Química e CT&quot;,&quot;Entidades Socioassistenciais&quot;,&quot;Residência Terapeutica&quot;,&quot;UAPS&quot;,&quot;Urgência/Emergência&quot;]\">\u003c/select>\n    \u003c/div>\n  \u003c/td>\n  <td data-type=\"number\" style=\"vertical-align: top;\">\n    <div class=\"form-group has-feedback\" style=\"margin-bottom: auto;\">\n      <input type=\"search\" placeholder=\"All\" class=\"form-control\" style=\"width: 100%;\"/>\n      <span class=\"glyphicon glyphicon-remove-circle form-control-feedback\">\u003c/span>\n    \u003c/div>\n    <div style=\"display: none; position: absolute; width: 200px;\">\n      <div data-min=\"0\" data-max=\"1\" data-scale=\"3\">\u003c/div>\n      <span style=\"float: left;\">\u003c/span>\n      <span style=\"float: right;\">\u003c/span>\n    \u003c/div>\n  \u003c/td>\n  <td data-type=\"disabled\" style=\"vertical-align: top;\">\n    <div class=\"form-group has-feedback\" style=\"margin-bottom: auto;\">\n      <input type=\"search\" placeholder=\"All\" class=\"form-control\" style=\"width: 100%;\"/>\n      <span class=\"glyphicon glyphicon-remove-circle form-control-feedback\">\u003c/span>\n    \u003c/div>\n  \u003c/td>\n\u003c/tr>","data":[["1","2","3","4","5","6","7","8","9","10","11","12","13","14","15","16","17","18","19","20","21","22","23","24","25","26","27","28","29","30","31","32","33","34","35","36","37","38","39","40","41","42","43","44","45","46","47","48","49","50","51","52","53","54","55","56","57","58","59","60","61","62","63","64","65","66","67","68","69","70","71","72","73","74","75","76","77","78","79","80","81","82","83","84","85","86","87","88","89","90","91","92","93","94","95","96","97","98","99","100","101","102","103","104","105","106","107","108","109","110","111","112","113","114","115","116","117","118","119","120","121","122","123","124","125","126","127","128","129","130","131","132","133","134","135","136","137","138","139","140","141","142","143","144","145","146","147","148","149","150","151","152","153","154","155","156","157","158","159","160","161","162","163","164","165","166","167","168","169","170","171","172","173","174","175","176","177","178","179","180","181","182","183","184","185","186","187"],["Assistência Hospitalar","Ambulatório de Saúde Mental","CAPSAD","CRAS/CREAS","CRAS/CREAS","CRAS/CREAS","Assistência Hospitalar","Entidades Assistênciais e Dependencia Química e CT","Entidades Assistênciais e Dependencia Química e CT","Entidades Assistênciais e Dependencia Química e CT","CRAS/CREAS","CRAS/CREAS","Entidades Assistênciais e Dependencia Química e CT","Entidades Assistênciais e Dependencia Química e CT","UAPS","Residência Terapeutica","CRAS/CREAS","Urgência/Emergência","Entidades Socioassistenciais","Assistência Hospitalar","CAPS","Entidades Assistênciais e Dependencia Química e CT","CAPS","Ajuda Mútua","Entidades Socioassistenciais","Ajuda Mútua","Ajuda Mútua","CAPS","CRAS/CREAS","CRAS/CREAS","CRAS/CREAS","UAPS","Acolhimento Institucional","Ajuda Mútua","CRAS/CREAS","Entidades Assistênciais e Dependencia Química e CT","Entidades Assistênciais e Dependencia Química e CT","Residência Terapeutica","Residência Terapeutica","Entidades Socioassistenciais","Acolhimento Institucional","Consultório na Rua","Consultório na Rua","Entidades Assistênciais e Dependencia Química e CT","Entidades Socioassistenciais","CRAS/CREAS","Ajuda Mútua","Residência Terapeutica","Residência Terapeutica","Residência Terapeutica","Residência Terapeutica","Residência Terapeutica","Entidades Assistênciais e Dependencia Química e CT","UAPS","Ajuda Mútua","CRAS/CREAS","UAPS","Ajuda Mútua","Ajuda Mútua","Assistência Hospitalar","Ajuda Mútua","Entidades Assistênciais e Dependencia Química e CT","UAPS","UAPS","Entidades Assistênciais e Dependencia Química e CT","Entidades Assistênciais e Dependencia Química e CT","Ajuda Mútua","Entidades Socioassistenciais","Entidades Socioassistenciais","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","CRAS/CREAS","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","UAPS","Ajuda Mútua","Ajuda Mútua","UAPS","Entidades Assistênciais e Dependencia Química e CT","Entidades Assistênciais e Dependencia Química e CT","UAPS","UAPS","UAPS","Ajuda Mútua","Entidades Assistênciais e Dependencia Química e CT","Assistência Hospitalar","UAPS","Entidades Assistênciais e Dependencia Química e CT","Entidades Assistênciais e Dependencia Química e CT","Entidades Assistênciais e Dependencia Química e CT","UAPS","Assistência Hospitalar","Entidades Socioassistenciais","Entidades Socioassistenciais","Residência Terapeutica","Residência Terapeutica","UAPS","UAPS","UAPS","Residência Terapeutica","Residência Terapeutica","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","Entidades Socioassistenciais","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","UAPS","Entidades Socioassistenciais","CAPS","Entidades Socioassistenciais","UAPS","Acolhimento Institucional","UAPS","UAPS","UAPS","UAPS","UAPS","Ajuda Mútua","Ajuda Mútua","UAPS","UAPS","UAPS","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Ajuda Mútua","Assistência Hospitalar"],[0.063,0.074,0.038,0.201,0.242,0.157,0.055,0.182,0.363,0.41,0.191,0.308,0.283,0.296,0.345,0.561,0.134,0.167,0.197,0.317,0.242,0.236,0.297,0.098,0.429,0.4,0.2,0.136,0.22,0.392,0.35,0.232,0.279,0.3,0.551,0.242,0.277,0.321,0.429,0.189,0.249,0.402,0.398,0.281,0.591,0.161,0.2,0.424,0.424,0.424,0.424,0.424,0.4,0.358,0.084,0.336,0.238,0.286,0.131,0.23,0.255,0.1,0.253,0.436,0.235,null,0.667,0.242,0.242,0.2,0.381,0.182,0.197,0.308,0.242,0.242,0.297,0.285,0.214,0.214,0.214,0.214,0.182,0.194,0.118,0.2,0.4,0.333,0.202,0.464,0.227,0.644,0.306,0.25,0.183,0.564,0.333,0.267,0.361,0.244,0.333,0.288,0.667,0.667,0.444,0.372,0.533,0.667,0.667,0.308,0.282,0.219,0.297,0.417,0.333,0.4,0.22,0.231,0.242,0.234,0.418,0.345,0.357,0.267,0.357,0.536,0.5,0.429,0.571,0.714,0.3,0.286,0.393,0.6,0.429,0.319,0.5,0.667,0.489,0.619,0.473,0.571,0.333,0.533,0.333,0.269,0.208,0.762,0.286,0.333,0.5,0.333,0.5,0.333,0.236,0.3,0.667,0.833,1,0,0,0.4,0,0.333,1,1,0.5,1,1,0.5,0.5,1,0.667,0.4,0.5,0,0.5,1,0.5,0.667,0.5,0.5,0.467,0.4,0.5,null,null],[0.122,1.388,0.095,0.401,0.541,0.263,0.195,0.328,null,0.799,0.566,0.497,0.398,0.473,0.719,0.879,0.411,0.383,0.288,0.59,0.62,0.422,1.162,0.11,0.571,0.6,0.5,0.327,0.673,0.592,0.717,0.5,0.548,0.4,0.838,0.323,0.875,0.571,0.768,0.354,0.53,0.68,0.648,0.25,1,0.266,0.267,0.705,0.705,0.705,0.705,0.705,1,0.367,0.091,0.491,0.272,0.381,0.172,0.119,0.37,-1,0.538,null,0.941,null,0.667,0.587,-0.727,0.667,0.889,0.375,0.42,0.625,0,0.782,1.282,0.552,1,1,1,1,0.425,0.266,0.392,-0.85,0.635,0.417,0.41,0.19,1.545,0.75,0.375,1.071,-0.956,0.867,0.5,0.533,1.438,0.433,0.5,0.528,1,1,2.062,0.829,1.926,1,1,0.646,0.636,-0.696,0.612,2.625,1.611,0.85,-0.462,0.745,1,0.516,0.938,0.687,null,null,null,0.143,2,0.952,2.167,1,-0.5,2,0.878,1.04,0.905,0.675,1.81,1.194,0.852,3,0.885,1.111,0.792,1.16,0.5,0.518,0.438,1.167,0.571,0,null,0,null,0,0.425,0.667,1,1.667,2,0,0,0.5,0,0.5,1.333,1,0.667,1.333,1.333,0.667,0.667,1.333,0.833,0.5,0.667,0,0.667,1.333,0.667,1.333,1.333,1.333,0.6,0.625,0.667,null,null]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> \u003c/th>\n      <th>Type\u003c/th>\n      <th>Local\u003c/th>\n      <th>Barrat's Weighted\u003c/th>\n    \u003c/tr>\n  \u003c/thead>\n\u003c/table>","options":{"columnDefs":[{"className":"dt-right","targets":[2,3]},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false,"orderCellsTop":true}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

##Aggregating data from previous table - mean

```r
aggdata_mean <-aggregate(var13_transitivity_df, by=list(var13_transitivity_df$Type), FUN=mean, na.rm=TRUE)

names(aggdata_mean) <- c("Group","Type","Local(M)", "Barrat's Weighted(M)")
  
#Removing Type variable
aggdata_mean<-aggdata_mean[,-c(2)]
```
##Aggregating data from previous table - sd

```r
aggdata_sd <-aggregate(var13_transitivity_df, by=list(var13_transitivity_df$Type), FUN=sd, na.rm=TRUE) 

names(aggdata_sd) <- c("Group","Type","Local(SD)", "Barrat's Weighted(SD)")

#Removing Type variable
aggdata_sd<-aggdata_sd[,-c(2)]
```
##Merging mean and standart deviation

```r
total_table <- merge(aggdata_mean,aggdata_sd,by="Group")

#Rounding
Group<-total_table[,c(1)] #Keeping group
total_table<-total_table[,-c(1)] %>% round(3) #Rouding
total_table<-cbind(Group,total_table) #Binding toghter

#Organizing Variabels
total_table<-total_table[c("Group","Local(M)","Local(SD)", "Barrat's Weighted(M)","Barrat's Weighted(SD)")]
```
##Final table with round - Transitivity

```r
datatable(total_table, filter = 'top')
```

<!--html_preserve--><div id="htmlwidget-97710429a9035fc6cc1a" style="width:100%;height:auto;" class="datatables html-widget"></div>
<script type="application/json" data-for="htmlwidget-97710429a9035fc6cc1a">{"x":{"filter":"top","filterHTML":"<tr>\n  <td>\u003c/td>\n  <td data-type=\"factor\" style=\"vertical-align: top;\">\n    <div class=\"form-group has-feedback\" style=\"margin-bottom: auto;\">\n      <input type=\"search\" placeholder=\"All\" class=\"form-control\" style=\"width: 100%;\"/>\n      <span class=\"glyphicon glyphicon-remove-circle form-control-feedback\">\u003c/span>\n    \u003c/div>\n    <div style=\"width: 100%; display: none;\">\n      <select multiple=\"multiple\" style=\"width: 100%;\" data-options=\"[&quot;Acolhimento Institucional&quot;,&quot;Ajuda Mútua&quot;,&quot;Ambulatório de Saúde Mental&quot;,&quot;Assistência Hospitalar&quot;,&quot;CAPS&quot;,&quot;CAPSAD&quot;,&quot;Consultório na Rua&quot;,&quot;CRAS/CREAS&quot;,&quot;Entidades Assistênciais e Dependencia Química e CT&quot;,&quot;Entidades Socioassistenciais&quot;,&quot;Residência Terapeutica&quot;,&quot;UAPS&quot;,&quot;Urgência/Emergência&quot;]\">\u003c/select>\n    \u003c/div>\n  \u003c/td>\n  <td data-type=\"number\" style=\"vertical-align: top;\">\n    <div class=\"form-group has-feedback\" style=\"margin-bottom: auto;\">\n      <input type=\"search\" placeholder=\"All\" class=\"form-control\" style=\"width: 100%;\"/>\n      <span class=\"glyphicon glyphicon-remove-circle form-control-feedback\">\u003c/span>\n    \u003c/div>\n    <div style=\"display: none; position: absolute; width: 200px;\">\n      <div data-min=\"0.038\" data-max=\"0.508\" data-scale=\"3\">\u003c/div>\n      <span style=\"float: left;\">\u003c/span>\n      <span style=\"float: right;\">\u003c/span>\n    \u003c/div>\n  \u003c/td>\n  <td data-type=\"number\" style=\"vertical-align: top;\">\n    <div class=\"form-group has-feedback\" style=\"margin-bottom: auto;\">\n      <input type=\"search\" placeholder=\"All\" class=\"form-control\" style=\"width: 100%;\"/>\n      <span class=\"glyphicon glyphicon-remove-circle form-control-feedback\">\u003c/span>\n    \u003c/div>\n    <div style=\"display: none; position: absolute; width: 200px;\">\n      <div data-min=\"0.003\" data-max=\"0.296\" data-scale=\"3\">\u003c/div>\n      <span style=\"float: left;\">\u003c/span>\n      <span style=\"float: right;\">\u003c/span>\n    \u003c/div>\n  \u003c/td>\n  <td data-type=\"disabled\" style=\"vertical-align: top;\">\n    <div class=\"form-group has-feedback\" style=\"margin-bottom: auto;\">\n      <input type=\"search\" placeholder=\"All\" class=\"form-control\" style=\"width: 100%;\"/>\n      <span class=\"glyphicon glyphicon-remove-circle form-control-feedback\">\u003c/span>\n    \u003c/div>\n  \u003c/td>\n  <td data-type=\"number\" style=\"vertical-align: top;\">\n    <div class=\"form-group has-feedback\" style=\"margin-bottom: auto;\">\n      <input type=\"search\" placeholder=\"All\" class=\"form-control\" style=\"width: 100%;\"/>\n      <span class=\"glyphicon glyphicon-remove-circle form-control-feedback\">\u003c/span>\n    \u003c/div>\n    <div style=\"display: none; position: absolute; width: 200px;\">\n      <div data-min=\"0.021\" data-max=\"0.427\" data-scale=\"3\">\u003c/div>\n      <span style=\"float: left;\">\u003c/span>\n      <span style=\"float: right;\">\u003c/span>\n    \u003c/div>\n  \u003c/td>\n\u003c/tr>","data":[["1","2","3","4","5","6","7","8","9","10","11","12","13"],["Acolhimento Institucional","Ajuda Mútua","Ambulatório de Saúde Mental","Assistência Hospitalar","CAPS","CAPSAD","Consultório na Rua","CRAS/CREAS","Entidades Assistênciais e Dependencia Química e CT","Entidades Socioassistenciais","Residência Terapeutica","UAPS","Urgência/Emergência"],[0.271,0.418,0.074,0.193,0.236,0.038,0.4,0.271,0.306,0.309,0.508,0.391,0.167],[0.02,0.296,null,0.108,0.07,null,0.003,0.117,0.102,0.12,0.128,0.17,null],[0.55,0.666,1.388,0.422,0.657,0.095,0.664,0.524,null,0.43,0.812,null,0.383],[0.021,0.416,null,0.37,0.358,null,0.023,0.166,null,0.427,0.155,null,null]],"container":"<table class=\"display\">\n  <thead>\n    <tr>\n      <th> \u003c/th>\n      <th>Group\u003c/th>\n      <th>Local(M)\u003c/th>\n      <th>Local(SD)\u003c/th>\n      <th>Barrat's Weighted(M)\u003c/th>\n      <th>Barrat's Weighted(SD)\u003c/th>\n    \u003c/tr>\n  \u003c/thead>\n\u003c/table>","options":{"columnDefs":[{"className":"dt-right","targets":[2,3,4,5]},{"orderable":false,"targets":0}],"order":[],"autoWidth":false,"orderClasses":false,"orderCellsTop":true}},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->

#Saving objects with new variables and changes

```r
save.image("~/SNArRDJF/Robject/9_dyad_triad_var13.RData") 
```


