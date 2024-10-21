# TP 03 : Shell bash
```
ssh root@localhost -p 2222
```
## Exercice : paramètres
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
## Exercice : vérification du nombre de paramètres
```
#!/bin/bash
var="Hello World"
nb_param="$#"
name_program="$0"
thirdh_param="$3"
list_param="$@"
computer_name="$(hostname)"
CONCAT="$1"
CONCAT+="$2"
if [ $# -eq 3 ];then
        echo "Plese enter 2 arguments next time"
        exit 1
fi
printf "%s\n""Concaténation: $CONCAT\n"
exit 0
```
## Exercice : argument type et droits
```
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
        exit 1
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
exit 0
```
## Exercice : Afficher le contenu d’un répertoire
```
#!/bin/bash
var="Hello World"
nb_param="$#"
name_program="$0"
thirdh_param="$3"
list_param="$@"
computer_name="$(hostname)"
my_file="$1"

if [ $# -lt 1 ];then
        echo "Plese enter 1 argument next time"
        exit 1
elif [ ! -d $my_file ];then
        echo "Please enter a directory"
        exit 1
fi
echo "$my_file is a directory"

echo "####### Fichiers dans $my_file"
for entry in "$my_file"/*;do
        if [ -f "$entry" ]; then
                echo "$entry"
        fi
done
echo "####### Répertoires dans $my_file"
for entry in "$my_file"/*; do
        if [ -d "$entry" ]; then
        echo "$entry"
        fi
done
exit 0
```
*aide: https://stackoverflow.com/questions/2437452/how-to-get-the-list-of-files-in-a-directory-in-a-shell-script*