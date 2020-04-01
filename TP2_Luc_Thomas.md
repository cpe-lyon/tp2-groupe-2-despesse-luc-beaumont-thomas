# Exercice 1. Variables d’environnement

**1. Dans quels dossiers bash trouve-t-il les commandes tapées par l’utilisateur ?**

* /usr/local/sbin
* /usr/local/bin
* /usr/sbin
* /usr/bin
* /sbin
* /bin 

**2. Quelle variable d’environnement permet à la commande cd tapée sans argument de vous ramener dans votre répertoire personnel ?**

`$HOME`

**3. Explicitez le rôle des variables LANG, PWD, OLDPWD, SHELL et _.**

*	"LANG" contient la langue sélectionnée pour le système.
*	"PWD" contient le chemin du répertoire courant.
*	"OLDPWD" contient le chemin de l'ancien répertoire courant.
*	"SHELL" contient l'interpréteur de commande utilisateur défini dans "/etc/passwd".
*	"_" affiche l'emplacement de la commande printenv.

**4. Créez une variable locale MY_VAR (le contenu n’a pas d’importance). Vérifiez que la variable existe.**

Création de la variable locale:

`MY_VAR="Bonjour"`

Affichage de la valeur de la variable:

`echo $MY_VAR`

**5. Tapez ensuite la commande bash. Que fait-elle ? La variable MY_VAR existe-t-elle ? Expliquez. A la fin de cette question, tapez la commande exit pour revenir dans votre session initiale.**

La commande bash va lancer un nouvel interpréteur de commande (un shell sh amélioré).
La saisie de la commande echo $MY_VAR ne va donc rien donner puisque cette variable à été a été déclarée en tant que variable locale.

**6. Transformez MY_VAR en une variable d’environnement et recommencez la question précédente. Expliquez.**

Création de la variable d'environnement:

`export MY_VAR="Bonjour"`

Affichage du contenu de la variable:

`printenv MY_VAR`

La variable existe bien car elle est devenu une variable d'environnement, contrairement à la variable locale qui elle est connue du Shell courant et donc uniquement pour la session en cours.

**7. Créer la variable d’environnement NOMS ayant pour contenu vos noms de binômes séparés par un espace. Afficher la valeur de NOMS pour vérifier que l’affectation est correcte.**

Création de la variable d'environnement:

`export NOMS="Thomas Luc"`

Affichage du contenu de la variable:

`echo $NOMS`

L'affectation est correcte.

**8. Ecrivez une commande qui affiche ”Bonjour à vous deux, binôme1 binôme2 !” (où binôme1 et binôme2 sont vos deux noms) en utilisant la variable NOMS.**

`echo "Bonjour à vous deux, $NOMS`

**9. Quelle différence y a-t-il entre donner une valeur vide à une variable et l’utilisation de la commande unset ?**

`unset`: supprime la variable d'environnement du système.

"Donner une valeur vide à une variable": Ne la supprime pas mais laisse un contenu vide.

**10. Utilisez la commande echo pour écrire exactement la phrase : $HOME = chemin (où chemin est votre dossier personnel d’après bash)**

`echo '$HOME' = "$HOME"`

Guillemets simples: chaîne littérale --> le contenu n’est pas interprété

Guillemets doubles: chaîne interprétée

## Programmation Bash

**Vous enregistrerez vos scripts dans un dossier script que vous créerez dans votre répertoire personnel.
Tous les scripts sont bien entendu à tester.
Ajoutez le chemin vers script à votre PATH de manière permanente.**

# Exercice 2. Contrôle de mot de passe

**Écrivez un script testpwd.sh qui demande de saisir un mot de passe et vérifie s’il correspond ou non au contenu d’une variable PASSWORD dont le contenu est codé en dur dans le script. Le mot de passe saisi par l’utilisateur ne doit pas s’afficher.**

```
#!/bin/bash
PASS="motdepasse"

read -s -p "Entrez le mot de passe : " mdp

if [ $PASS = $mdp ]; then
	echo "Mot de passe OK!"
else 
	echo "Mot de passe érroné!"
fi
```

# Exercice 3. Expressions rationnelles

**Ecrivez un script qui prend un paramètre et utilise la fonction suivante pour vérifier que ce paramètre est un nombre réel :
function is_number()
**
```
#!/bin/bash
  
function is_number()
{
  re='^[+-]?[0-9]+([.][0-9]+)?$'
  if ! [[ $1 =~ $re ]] ; then
    return 1
else return 0
fi }

for param in $*
do
        is_number $param
        if [ $? = 0 ]
        then
                echo "$param est un nombre réel !"
        else
                echo "Erreur : $param n'est pas un nombre réel !"
        fi
done
```


# Exercice 4. Contrôle d’utilisateur

**Écrivez un script qui vérifie l’existence d’un utilisateur dont le nom est donné en paramètre du script. Si le script est appelé sans nom d’utilisateur, il affiche le message : ”Utilisation : nom_du_script nom_utilisateur”, où nom_du_script est le nom de votre script récupéré automatiquement (si vous changez le nom de votre script, le message doit changer automatiquement)**

```
#!/bin/bash
  
if [ $# != 1 ]
then
        echo "Utilisation : $0 nom_utilisateur"
else
        res=$(grep $1 /etc/passwd)

        if [ -n "$res" ]
        then
                echo "L'ulisateur $1 existe."
        else
                echo "L'utilisateur n'existe pas."
        fi
fi
```
# Exercice 5. Factorielle

**Écrivez un programme qui calcule la factorielle d’un entier naturel passé en paramètre (on supposera que l’utilisateur saisit toujours un entier naturel).**

```
#!/bin/bash

res=1
for i in $(seq 1 $1)
do
	res=$(($res*$i))
done
echo $res
```
# Exercice 6. Le juste prix

**Écrivez un script qui génère un nombre aléatoire entre 1 et 1000 et demande à l’utilisateur de le deviner. Le programme écrira ”C’est plus !”, ”C’est moins !” ou ”Gagné !” selon les cas (vous utiliserez $RANDOM).**

```
#!/bin/bash
prix=$(((RANDOM%1000)+1))

devine=-1

while [ $devine -ne $prix ]
do
read devine
if [ $devine -gt $prix ]; then
  echo 'C'est moins !'
fi
if [ $devine -lt $prix ]; then
  echo 'C'est plus !'
fi
done

echo 'Gagné !'

```

# Exercice 7. Statistiques

**1. Écrivez un script qui prend en paramètres trois entiers (entre -100 et +100) et affiche le min, le max et la moyenne. Vous pouvez réutiliser la fonction de l’exercice 3 pour vous assurer que les paramètres sont bien des entiers.**

**2. Généralisez le programme à un nombre quelconque de paramètres (pensez à SHIFT)**

**3. Modifiez votre programme pour que les notes ne soient plus données en paramètres, mais saisies et stockées au fur et à mesure dans un tableau.**

```
#!/bin/bash
function is_number()
{
	re='^[+-]?[0-9]+([.][0-9]+)?$'
	if ! [[ $1 =~ $re ]] ; then
		return 1
	else
		return 0
	fi
}

sum=0
count=0
min=$1
max=$1

while (("$#")); do
    is_number $1
    if [[ $? = 1 ]]; then
        echo Un des paramètres n\'est pas un nombre
        exit
    fi
    sum=$((sum + $1))
    count=$((count + 1))
    if [[ $1 -gt $max ]]; then
        max=$1
    elif [[ $1 -lt $min ]]; then
        min=$1
    fi
    shift
done

echo Min: $min
echo Max: $max
printf 'Moyenne: %.2f\n' $(echo "$sum / $count" | bc -l)

#!/bin/bash
function is_number()
{
	re='^[+-]?[0-9]+([.][0-9]+)?$'
	if ! [[ $1 =~ $re ]] ; then
		return 1
	else
		return 0
	fi
}

values=()
input="0"
index=0

while [ -n "$input" ]; do
    read -p "Entrez un nombre: " input

    if [ -n "$input" ]; then
        is_number $input
        if [[ $? = 1 ]]; then
            echo Ce n\'est pas un nombre !
        else
            values[$index]=$input
            index=$(( $index + 1 ))
        fi
    fi
done

sum=0
min=${values[0]}
max=${values[0]}

for n in ${values[@]}; do
    sum=$((sum + $n))
    count=$((count + 1))

    if [[ $n -gt $max ]]; then
        max=$n
    elif [[ $n -lt $min ]]; then
        min=$n
    fi
done

echo Min: $min
echo Max: $max
printf 'Moyenne: %.2f\n' $(echo "$sum / $index" | bc -l)

```
