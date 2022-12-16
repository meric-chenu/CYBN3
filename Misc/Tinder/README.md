# Write-Up
> **title:** Tinder
>
> **category:** Misc
>
> **difficulty:** Facile
>
> **point:** 50
>
> **author:** Maestran
>
> **description:**
> Ca va être compliqué de match là : <br>
> **CYBN{([^[^a-z0-9A-X]|[^Z]])0{0}1{0}0{1}u{1}_(?#Ab^[1-4][5555-7-8-9-2]).(?<=4)[r-r]((((3_))))(?#[a-z]|[3-4]|[1-2])([t](h)[3](_))(?#.[a-Z0-5]...)t[r-r]u{1}((((3_))))r3g3x_(?#[a-z]|[3-4]|[1-2])([^[a-l]|[^n-z0-9A-X])0{0}1{0}4{1}st.(?<=3)r{1,10}}**

## Analyse du sujet
Pour les connaisseurs, nous voyons directement qu'il s'agit d'une regex. Nous voyons également que celle-ci est encadrée dans le flag CYBN{}. Il nous faut donc probablement supprimer des bouts de celle-ci qui sont inutiles.<br><br>
Commençons par ce bout de regex : **([^[^a-z0-9A-X]|[^Z]])**
l'élément **[^Z]** spécifié l'exclusion du caractère Z, nous pouvons donc supprimer cette partie. Ce qui revient à avoir **([^[^a-z0-9A-X]])** Or ici, nous pouvons avons n'importe quel lettre majuscule et chiffre puisque l'élément ^a-z permet d'exclure les lettres minuscules. Gardons le de côté.
Nous avons donc la regex suivante : **([^[0-9A-X]])0{0}1{0}0{1}u{1}_(?#Ab^[1-4][5555-7-8-9-2]).(?<=4)[r-r]((((3_))))(?#[a-z]|[3-4]|[1-2])([t](h)[3](_))(?#.[a-Z0-5]...)t[r-r]u{1}((((3_))))r3g3x_(?#[a-z]|[3-4]|[1-2])([^[a-l]|[^n-z0-9A-X])0{0}1{0}4{1}st.(?<=3)r{1,10}**<br>
Passons à l'élément **0{0}1{0}0{1}u{1}** qui nous donne 0u.<br>

Nous en sommes donc à : **[^[0-9A-X]])0u_(?#Ab^[1-4][5555-7-8-9-2]).(?<=4)[r-r]((((3_))))(?#[a-z]|[3-4]|[1-2])([t](h)[3](_))(?#.[a-Z0-5]...)t[r-r]u{1}((((3_))))r3g3x_(?#[a-z]|[3-4]|[1-2])([^[a-l]|[^n-z0-9A-X])0{0}1{0}4{1}st.(?<=3)r{1,10}**

Passons à l'élément **(?#Ab^[1-4][5555-7-8-9-2])** : "?#" permet de rendre en commentaire ce qui suit.<br><br>
Nous en sommes donc à : **[^[0-9A-X]])0u_.(?<=4)[r-r]((((3_))))(?#[a-z]|[3-4]|[1-2])([t](h)[3](_))(?#.[a-Z0-5]...)t[r-r]u{1}((((3_))))r3g3x_(?#[a-z]|[3-4]|[1-2])([^[a-l]|[^n-z0-9A-X])0{0}1{0}4{1}st.(?<=3)r{1,10}**<br>
Passons à la partie **.(?<=4)[r-r]((((3_))))** :  Le point spécifie n'importe quel caractère. Gardons le de côté. L'élément [r-r] peut se simplifier en "r". L'élément ((((3_)))) peut se transformer en 3_
Nous avons donc : **.(?<=4)r3_**<br>
Nous en sommes donc à : **[^[0-9A-X]])0u_.(?<=4)r3_(?#[a-z]|[3-4]|[1-2])([t](h)[3](_))(?#.[a-Z0-5]...)t[r-r]u{1}((((3_))))r3g3x_(?#[a-z]|[3-4]|[1-2])([^[a-l]|[^n-z0-9A-X])0{0}1{0}4{1}st.(?<=3)r{1,10}**

Prenons l'élément : **(?#[a-z]|[3-4]|[1-2])([t](h)[3](_))(?#.[a-Z0-5]...)**
L'élément **?#[a-z]|[3-4]|[1-2]** spécifié de prendre n'importe quelle lettre minuscule, ou un chiffre entre 3 et 4 ou un chiffre entre 1 et 2. Nous pouvons donc enlever ces éléments due à l'inexactitude des éléments.<br>
L'élement **([t](h)[3](_))** se simplifie en **th3_**<br>
L'élément **(?#.[a-Z0-5]...)** spécifie n'importe quel caractère, puis n'importe quelle lettre minuscule suivie d'un chiffre entre 0 et 5, suivit de n'importe quel caractère. Nous pouvons enlever cette partie.<br><br>
Nous en sommes donc à : **[^[0-9A-X]])0u_.(?<=4)r3_th3_t[r-r]u{1}((((3_))))r3g3x_(?#[a-z]|[3-4]|[1-2])([^[a-l]|[^n-z0-9A-X])0{0}1{0}4{1}st.(?<=3)r{1,10}**<br>
Passons à l'élément **t[r-r]u{1}((((3_))))** : nous pouvons le simplifier en **tru3_**<br><br>
Nous en sommes donc à : **[^[0-9A-X]])0u_.(?<=4)r3_th3_tru3_r3g3x_0{0}1{0}4{1}st.(?<=3)r{1,10}**

Prenons la dernière partie, c'est à dire **0{0}1{0}4{1}st.(?<=3)r{1,10}** : Nous avons 4st.(?<=3)r.  Nous pouvons enlever le point puisque nous n'avons pas besoin d'un caractère en plus. Nous voyons que nous avons le mot "4st3r". Si l'on ajoute un m devant, nous avons le mot "m4st3r" --> master. <br>

Nous avons donc le regex suivante : **[^[0-9A-X]])0u_.(?<=4)r3_th3_tru3_r3g3x_m4st3r**<br>
Lisant la regex, nous pouvons extraire le mot **Y0u_4r3_th3_tru3_r3g3x_m4st3r**.<br><br>
Le flag est donc : **CYBN{Y0u_4r3_th3_tru3_r3g3x_m4st3r}**

