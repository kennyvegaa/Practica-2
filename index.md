## Ejercicio 2
Equipo 3 : Noria Montes Itzel y Vega Alba Mónica Kendra. 

¿Cuáles son la propiedades númericas de un ruido blanco?

```markdown
DistribucionAsociada= "Normal"    #Normal, Poisson, LogNormal, Gamma, ChiCudrada, Exponencial
MediaAsociada= 0           #Valor númerico
CorrelacionAsociado= 0      #cor(e_i, e_j)= #Valor númerico
```
Simula una muestra con 355 ruidos blancos estandarizados 
```markdown
set.seed(1) 

RuidoBlanco= rnorm(355)
```
Gráfica los ruidos blancos, mediante una dispersión de datos uniendo cada observación por medio de una línea
```markdown
plot(RuidoBlanco, col = "turquoise4")
```
![1](https://user-images.githubusercontent.com/57273828/68147658-25c10380-ff00-11e9-8209-5c008f8d0d7e.png)

Los datos tienen la estructura típica de un ruido blanco
```markdown
PatronRuidoBlanco= "TRUE"      #TRUE (sí) FALSE (No)
```
Realiza un histrograma del ruido Blanco
```markdown
hist(rnorm(355), prob= T, col = "turquoise4")
```
![2](https://user-images.githubusercontent.com/57273828/68147702-3c675a80-ff00-11e9-8017-a93de7536a3b.png)

¿La gráfica es símetrica?
```markdown
Simetria= "TRUE"              #TRUE (sí) FALSE (No)
```
¿Viendo el histograma puedes asumir que tiene la distrubución que había asumido al inicio?
```markdown
HistogramaDistribucion= "TRUE" #TRUE (sí) FALSE (No)
```
Se puede asumir que todos los ruidos blancos están autocorrelacionadas entre si?
```markdown
Autocorrelacion= #TRUE (sí) FALSE (No)
```
COn el ruido blanco, genera una caminata aleatoria usando el sustitución hacía atrás
X(t)=X(t-1)+e(t)
X(t)=X(t-2)+e(t-1)+e(t)
Así hasta 
X(t)=e(1)+e(2)+...+e(t-1)+e(t)
```markdown
CaminataAleatoria=RuidoBlanco[1]
  for (i in 2:355) {
    CaminataAleatoria[i]= CaminataAleatoria[i-1]+ RuidoBlanco[i]
  }
    
plot(CaminataAleatoria, type = "l", col = "turquoise4")
```
![3](https://user-images.githubusercontent.com/57273828/68147722-51dc8480-ff00-11e9-9cee-18dfbdfd6dc3.png)

Con la caminata aleatoria propuesta obten la gráfica de autocorrelaciones
```markdown
acf(CaminataAleatoria)
```
![4](https://user-images.githubusercontent.com/57273828/68147752-66b91800-ff00-11e9-8d05-d04fd6551c02.png)

¿Se puede asumir que las observaciones son altamante correlacionadas entre sí?
```markdown
AutocorrelacionCaminata=   #TRUE (sí) FALSE (No)
```
Realiza la operación de diferencia con un lag de distancia.
Delta(x(t))=X(t)-X(t-1)
```markdown
DoritoCaminata= diff(CaminataAleatoria)
head(DoritoCaminata)
```
```markdown
[1]  0.1836433 -0.8356286  1.5952808  0.3295078 -0.8204684  0.4874291
```
Realiza la autocorrelación de las última diferencias.
```markdown
acf(DoritoCaminata)
```
![5](https://user-images.githubusercontent.com/57273828/68147810-7c2e4200-ff00-11e9-8e70-e6843ffc1312.png)

¿Se puede asumir que las observaciones diferenciadas son altamante correlacionadas entre sí?
```markdown
AutocorrelacionDorito= "FALSE" #TRUE (sí) FALSE (No)
```
La importancia de la diferenciación es que genera modelos que no son autocorrelacionados entre sí
Esto es importante para la realización de series de tiempo. Para ello haga una estimación a través del método de 
HoltWinters
```markdown
DoritoCaminata.ts= ts(DoritoCaminata, start=1990, frequency = 12)
```
Realice la estimación de HoltWinter con el modelo multiplicativo,con la serie anteriormente esctrita
```markdown
EstimacionDiferenciacion= HoltWinters(DoritoCaminata.ts, seasonal = "mult" )
plot(EstimacionDiferenciacion, col="turquoise4")
```
![6](https://user-images.githubusercontent.com/57273828/68147851-910ad580-ff00-11e9-9caa-5fc18c19e7ad.png)

Ahora haga un proyección de los datos para el siguiente periodo. (1)
```markdown
ProyeccionDiferenciacion = predict(EstimacionDiferenciacion, n.ahead = 1)
```
Considere el siguiente modelo:
```markdown
set.seed(1)
x=e=rnorm(250)
for (t in 2:250) x[t]=(equipo/10)*x[t-1]+e[t]
```
```markdown
print(x)
```
```markdown
  [1] -0.6264538107 -0.0042928190 -0.8369164581  1.3442058647  0.7327695312 -0.6006375247
  [7]  0.3072377950  0.8304960436  0.8249301647 -0.0579093377  1.4944083671  0.8381657466
 [13] -0.3697908566 -2.3256371442  0.4272397749  0.0832383235  0.0087812339  0.9464705809
 [19]  1.1051623694  0.9254500320  1.1966123812  1.1411200151  0.4169009879 -1.8642813995
 [25]  0.0605413280 -0.0379663411 -0.1671854090 -1.5209080066 -0.9344224571  0.1376148231
 [31]  1.3999639985  0.3172014722  0.4828320532  0.0910445754 -1.3497461842 -0.8199184186
 [37] -0.6402654793 -0.2513930405  1.0246074598  1.0705579864  0.1566437997 -0.2063685402
 [43]  0.6350528133  0.7471790427 -0.4646019817 -0.8468757515  0.1105192367  0.8016886955
 [49]  0.1281603965  0.9195558454  0.6739726340 -0.4098346031  0.2181693105 -1.0639123029
 [55]  1.1138500108  2.3145549018  0.3271449941 -0.9459911281  0.2859222890 -0.0492779172
 [61]  2.3868343854  0.6768103129  0.8927824563  0.2958368957 -0.6545221402 -0.0075643425
 [67] -1.8072279317  0.9233864821  0.4302692828  2.3016924552  1.1660172655 -0.3601412513
 [73]  0.5026839781 -0.7832924382 -1.4886211317 -0.1551401040 -0.4898339044 -0.1458448197
 [79]  0.0305878782 -0.5803445827 -0.7427721076 -0.3580102474  1.0706839223 -1.2023616237
 [85]  0.2332377005  0.4029216814  1.1839763417  0.0510089789  0.3853215036  0.3826952418
 [91] -0.4277114584  1.0795543685  1.4842689262  1.1454943274  1.9304817528  1.1376309514
 [97] -0.9353029230 -0.8538562911 -1.4807695022 -0.9176314871 -0.8956561234 -0.2265809639
[103] -0.9788959377 -0.1356400089 -0.6952766466  1.5587042754  1.1843187586  1.2654698571
[109]  0.7638263150  1.9113239750 -0.0623392614 -0.4803465088  1.2881782859 -0.2642428675
[115] -0.2866536039 -0.4788040106 -0.4636340717 -0.4182035245  0.3687272739 -0.0667123001
[121] -0.5259711521  1.1852474795  0.1409948353 -0.1372580794 -0.1413681650  0.6702558575
[127]  0.1275123531  0.0006195345 -0.6814746184 -0.5287126578 -0.0984533569 -0.6184304933
[133]  0.3459670446 -1.4146039684 -0.1178233297 -1.5717968225 -0.7725151736 -0.7600344565
[139] -0.8801051176 -0.3209283131 -2.0106379196  0.5733919361 -1.4929548554 -0.9114168581
[145] -1.3893451625 -1.1676225499  1.7368797806  0.5384595539 -1.1247626643 -1.9780343337
[151] -0.1432231988 -0.0615267924 -0.3365264123 -1.0303200711 -1.7965563315 -1.6141591961
[157]  0.5157810449 -0.4665323813 -1.5243865618  1.4119746539  0.8486927735  0.0159607311
[163]  1.0632712681  1.2054040318 -0.2576218387  2.1288159129  0.3836177437 -1.3094093271
[169] -0.5372224001  0.0463716192  2.3218898848  0.8023693333  0.6977096054  0.1321599463
[175] -0.2943528585 -0.1230318859  0.7507300399  2.3004640206  1.7175316449  1.7231678919
[181] -0.7143730540  0.7695836539  0.4507998998 -1.3320100591  0.1214197249 -0.1223286872
[187]  1.4278887058 -0.3377153879 -0.5315263703 -1.0855674085 -0.5027741840  0.2511795243
[193] -0.6563943158  0.6334548732 -1.0180463243 -1.3533983101  1.0351382138 -0.7053060012
[199]  0.2003829120 -0.3209611775  0.3131134864  1.7828073321  2.1214306331  0.3055213892
[205] -2.1935791185  1.8395878543  1.2189425230  0.9070100929  0.2587035047  0.5877194744
[211]  0.0119400105  0.4242766464 -0.2729637501 -1.4520970026  0.5522091667  1.6854077755
[217]  0.1968817634 -1.1942252266  0.2839737377  0.0404829844 -1.7210735115 -0.5141901938
[223] -0.7845573921 -0.5763357975 -1.3294731019  1.4042999774  0.0901579568 -1.5784660252
[229] -0.2763463688  0.1802717358 -0.9317451797 -3.1684442256 -1.5910149702  0.0932031448
[235] -0.0317623326 -0.1077074438  0.5285084955 -1.0279060899  0.7884052173  0.2311775369
[241]  0.7766639285  1.2671069133  0.6036124889 -0.6976238662  0.9536773961 -1.7140617260
[247] -1.0590092578 -0.5733734865 -0.3381330827  0.9190239840  0.3726735735  0.5030511649
[253]  0.0597730315 -0.2322855985  0.6357870561  1.3098072414 -2.0661016112  0.0411612652
[259]  0.3853146095 -0.3261318029
```
```markdown
plot(x, type = "l")
```
![7](https://user-images.githubusercontent.com/57273828/68147891-a2ec7880-ff00-11e9-958c-3793815ebedd.png)

Realice la autocorrelacion del modelo simulado
```markdown
acf(x)
```
![8](https://user-images.githubusercontent.com/57273828/68147940-b4ce1b80-ff00-11e9-88b7-fac1ffc3bb95.png)

Los valores simulados son altamente correlacionados
```markdown
CorrelacionAR= "FALSE" #TRUE (sí) FALSE (No)
```
¿Se puede realizar un modelo autorregresivo?
```markdown
Requesitos= "TRUE" #TRUE (sí) FALSE (No)
```
ajusta un modelo AR(p) (autorregresivo de p grados) usando el método mle
```markdown
ModeloAR= ar(x, method = "mle")
```
El modelo AR(P) que mejor se ajusta a los datos es de orden:
```markdown
OrdenP= ModeloAR$order
print(OrdenP)
```
```markdown
[1] 1
```
El valor del o de los coeficientes es :(usando código )
```markdown
Coeficiente= ModeloAR$ar
print(Coeficiente)
```
```markdown
[1] 0.2426139
```
Con esa información poryecte la información 10 periodos más. Recuerde que el modelo AR(p) es: AR(P)=x(t)=B1*X(t-1)+B2*X(t-2)+...+Bp*X(t-p)+E
```markdown
ProyeccionAR= predict(ModeloAR, n.ahead = 10)
n=length(x)
estimado=0
for(i in 1:10){
  x[n+i]=x[n+i-1]*Coeficiente+rnorm(1)
  estimado[i]=x[n+i]
}
ProyeccionAR=estimado
plot(x, type = "l")
lines(x=251:260, estimado, col="red")
```
![9](https://user-images.githubusercontent.com/57273828/68147988-c9aaaf00-ff00-11e9-9a24-301ace5d1493.png)
