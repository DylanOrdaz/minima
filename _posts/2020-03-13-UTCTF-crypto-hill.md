---
layout: post
title: "UTCTF Hill crypto"
categories: crypto
author:

meta: 
---

## Hill-Crypto

Descripción:

{% highlight bash %}

I found these characters on top of a hill covered with algae ... bruh I can't figure it out can you help  me?  

wznqca{d4uqop0fk_q1nwofDbzg_eu}

{% endhighlight %}

En la descripción del reto nos dice la palabra clave “Hill”, y en este caso buscamos en Google, si hay algún cifrado tal cual, descubrimos que si lo hay y que se llama “Hill cipher”.

Primero que nada hay que ver como funciona el cifrado Hill.

La formula del cifrado Hill es la siguiente.


![mxp][mxp]

En el cual

- M es la llave secreta, en este caso una matriz de 2x2
- P es el grupo de valores del texto sin cifrar, este numero se obtiene del abecedario en el cual A=0 y Z=25
- C es el grupo de de valores cifrados en modulo 26

¿Por qué modulo 26? Bueno esto se debe a que son 26 letras en el abecedario, y los resultados de los calculos que se van a realizar llegaran a ser mayores a 26, por eso es mod 26 para que los resultados esten dentro del rango del abecedario.

Bueno ya que conocemos que significa cada cosa, necesitamos hacer unas cuantas operaciones.

No conocemos la llave secreta la cual es necesaría para poder descifrar el mensaje, necesitamos saber los valores de dicha llave.

Para eso multiplicamos la matriz inversa de P en ambos lados, como en la siguiente imagen.

![mxpxp][mxpxp]

Al hacer las operaciones el resultado sería el siguiente

![m][m]

Ya tenemos que M (llave secreta) es igual a la matriz inversa de P (texto sin cifrar) multiplicado por C (valores cifrados)

El formato de la flag es "utflag{ }", el mismo ctf nos lo hace saber, por lo tanto podemos concluir que el siguiente texto es la flag codificada 
"wznqca{d4uqop0fk_q1nwofDbzg_eu}".

Desconocemos que siginifica el texto que esta entre las llaves, pero las primeras 5 palabras "wznqca" son "utflag", gracias a esto sabemos que es una matriz de 2x2 debido a que son solamente 5 letras. El tipo de ataque que se hará es conocido como "plaintext attack".

Vamos a comenzar con esto, sabemos en el cifrado Hill las letras son sustituidas por números del 0 al 25, entonces comencemos  A = 0, B = 1, C = 2, D = 3, E = 4, ..... 


Texto cifrado         | Num. en el alfabeto | Texto sin cifrar      | Num. en el alfabeto
--------------------- | --------------------- | --------------------- | ---------------------
w                     | 22                    | u                     | 20
z                     | 25                    | t                     | 19
n                     | 13                    | f                     | 5
q                     | 16                    | l                     | 11
c                     | 2                     | a                     | 0
a                     | 0                     | g                     | 6


Ya que entendimos lo más dificil es hora de sustituir los valores en la formula anterior.

Usaremos las letras "wznq" y "utfl" las sustituiremos por los números.

![vC][vC]

![vP][vP]

Pero los valores de P todavía no estan completamente terminados ya que debemos de tener la matriz inversa, entonces procedemos hacer la matriz inversa de P.


![pIN][pIN]

Pero todavía no esta completo y esto es debido a que los valores que aparecen ahí no son los mismo que los del abecedario entonces necesitamos encontrar el mod 26 de la matriz inversa de P.

![pINmod][pINmod]

Ahora si ya tenemos el valor definitivo de P, lo que procede es multiplicarlo con C para poder obtener la llave secreta M.

Hacemos el procedimiento que sería el siguiente

![resM][resM]


Ya tenemos M, pero para poder descifrar el mensaje se debe de tener la matriz inversa de M, la formula sería la siguiente.

![pFin][pFin]

La matriz inversa de M sería la siguiente

![kInv][kInv]

Y ahora ya que tenemos finalmente la matriz inversa de M podemos empezar a multiplicarla por C que es el mensaje cifrado para ir descifrando el mensaje.

Texto cifrado         | Num. en el alfabeto    | Llave 
--------------------- | ---------------------  | ----------
w                     | 22                     | 13
z                     | 25                     | 3
n                     | 13                     | 6
q                     | 16                     | 21                    


![mat1][mat1]


Texto cifrado         | Num. en el alfabeto    | Llave 
--------------------- | ---------------------  | ----------
c                     | 2                      | 13
a                     | 0                      | 3
d                     | 3                      | 6
u                     | 20                     | 21


![mat2][mat2]


Texto cifrado         | Num. en el alfabeto     
--------------------- | ---------------------   
q                     | 16                      
o                     | 14                      
p                     | 15                      
f                     | 5                     


![mat3][mat3]


Texto cifrado         | Num. en el alfabeto     
--------------------- | ---------------------   
k                     | 10                      
q                     | 16                      
n                     | 13                      
w                     | 22       


![mat4][mat4]


Texto cifrado         | Num. en el alfabeto     
--------------------- | ---------------------   
o                     | 14                      
f                     | 5                      
D                     | 3                      
b                     | 1                     


![mat5][mat5]


Texto cifrado         | Num. en el alfabeto     
--------------------- | ---------------------   
z                     | 25                      
g                     | 6                      
e                     | 4                      
u                     | 20                     


![mat6][mat6]


Traducimos los valores de las matrices a letras: "utflagdngerusyphertextqq"

Y la flag sería:

{% highlight bash %}

utflag {d4nger0us_c1pherText_qq}

{% endhighlight %}

[mxp]:{{site.baseurl}}/images/utctf/mxp.png
[mxpxp]:{{site.baseurl}}/images/utctf/mxpxp.png
[m]:{{site.baseurl}}/images/utctf/m.png
[vC]:{{site.baseurl}}/images/utctf/vC.png
[vP]:{{site.baseurl}}/images/utctf/vP.png
[pIN]:{{site.baseurl}}/images/utctf/pIN.png
[pINmod]:{{site.baseurl}}/images/utctf/pINmod.png
[resM]:{{site.baseurl}}/images/utctf/resM.png
[pFin]:{{site.baseurl}}/images/utctf/pFin.png
[kInv]:{{site.baseurl}}/images/utctf/kInv.png
[mat1]:{{site.baseurl}}/images/utctf/mat1.png
[mat2]:{{site.baseurl}}/images/utctf/mat2.png
[mat3]:{{site.baseurl}}/images/utctf/mat3.png
[mat4]:{{site.baseurl}}/images/utctf/mat4.png
[mat5]:{{site.baseurl}}/images/utctf/mat5.png
[mat6]:{{site.baseurl}}/images/utctf/mat6.png