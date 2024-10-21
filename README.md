# TP 03 : Shell bash
## Pourquoi utiliser bash ?
```bash
    root@serveur1:~# which bash
    /usr/bin/bash
```
#### notes:
- la variable $# contient le nombre de paramètres passés au script
- pour chaque entier i entre 1 et 9, la variable $i contient le i-ème paramètre
- la variable $@ contient la liste de tous les paramètres séparés par des espaces
- la variable $0 contient le nom du programme en cours d’exécution

### Exercice : paramètres

```
#!/bin/bash
var="Hello World"
nb_param="$#"
name_program="$0"
thirdh_param="$3"
list_param="$@"
computer_name="$(hostname)"
printf "%s\n" "Bonjour, vous avez rentré $nb_param de paramètres en paramètres"
printf "%s\n" "Le nom du script est $name_program"
printf "%s\n" "Le 3ème paramètres est $thirdh_param"
printf "%s\n" "Voici la listes des paramètres: $list_param"

```

*source: https://www.cyberciti.biz/faq/hello-world-bash-shell-script/*


### Exercice : argument type et droits
```
  GNU nano 7.2                     test-fichier.sh                              
#!/bin/bash
var="Hello World"
nb_param="$#"
name_program="$0"
thirdh_param="$3"
list_param="$@"
computer_name="$(hostname)"
my_file="$1"

if [ $# -eq 2 ];then
        echo "Plese enter 1 argument next time"
elif [ -f $my_file ];then
        echo "$my_file exists"
elif [ -d $my_file ];then
        echo "$my_file is a directory"
elif [ -r $my_file ];then
        echo "is readable"
elif [ -w $my_file ];then
        echo "is writteble"
elif [ -x $my_file ];then
        echo "is executable"
else
        echo "The file passed as parameter does not exit"
fi

printf "%s\n""name of the file passed as parameter: $my_file\n"
```
*source:https://www.baeldung.com/linux/bash-check-script-arguments*

### Exercice : Afficher le contenu d’un répertoire