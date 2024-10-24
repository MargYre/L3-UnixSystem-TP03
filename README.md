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
nb_param="$#"
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

## Exercice : Lister les utilisateur
**for user in $(cat /etc/passwd); do echo $user; done permet preque de
parcourir les lignes du dit fichier. Cependant, quel est le problème ?**
Le fichier /etc/passwd est découper en fonction des espaces avec la commande. 

**Utilisation de cut**
```
#!/bin/bash
grep '^[^:]*:[^:]*:[1-9][0-9][0-9]:' /etc/passwd | cut -d':' -f1
```
**Utilisation de awk**
```
#!/bin/bash
awk -F: '($3 >= 100) && ($7 != "/sbin/nologin") { print $1 }' /etc/passwd
```
*souces: https://www.cyberciti.biz/faq/understanding-etcpasswd-file-format/
https://unix.stackexchange.com/questions/144705/use-cut-to-extract-a-list-from-etc-passwd
https://www.unix.com/unix-for-dummies-questions-and-answers/248183-extract-user-accounts-home-directory-etc-passwd.html*

## Exercice : Mon utilisateur existe t’il ?
```
#!/bin/bash
printf "arg1: %s |" "$1"
printf " arg2: %s\n" "$2"
#grep peter /etc/passwd #egrep to check more than one user
if [ $# -lt 2 ]; then
    echo "Please enter -login or -uid (followeb by a login or UID)"
    exit 1
fi
#login. Get username (login) 
if [ "$1" = "-login" ]; then
    get_logins=$(cut -d: -f1 /etc/passwd) #capture de sortie
    for user in $get_logins; do
        if [ "$user" = "$2" ]; then
            echo "user exists"
            uid=$(grep "^$user:" /etc/passwd | cut -d: -f3)
            printf "UID of the user:: %s\n" "$uid"
            exit 0
	fi
    done
    printf "User does not exist\n"
fi
#UID
if [ "$1" = "-uid" ]; then
    get_uids=$(cut -d: -f3 /etc/passwd) #$() = capture de sortie
    for uid in $get_uids; do
        if [ "$uid" = "$2" ]; then
            echo "user exists"
            printf "UID of the user: %s\n" "$uid"
            exit 0
	fi
    done
    printf "No user with UID %s\n" "$2"
fi
exit 1
```
*aide: https://stackoverflow.com/questions/14810684/check-whether-a-user-exists#:~:text=user%20infomation%20is%20stored%20in,%22no%20such%20user%22%20message.*

## Exercice : Creation utilisateur
```
#!/bin/bash
#check if running as root in a bash script
if [ "$EUID" -ne 0 ]; then
    echo "Please run as root.\n"
    exit 1
fi
#check args
if [ $# -lt 2 ]; then
    echo "Usage: $0 -login <username> | -uid <UID>\n"
    exit 1
fi
# Récup args
user_type=$1
user_value=$2
printf user_type: %s - user_value: %s\n "$user_type" "$user_value"
#Us read to ask questions to the user
read -p "Enter login (username) : " login
read -p "Enter last name : " nom
read -p "Enter first name : " prenom
read -p "UID : " custom_uid
read -p "GID : " custom_gid
read -p "Commentaires : " custom_comment
#Check if home directory exists
if [ -d "/home/$login" ]; then
    echo "Error : /home/$login already exists."
    exit 1
else
    echo "/home/$user_value not find. You can create a user."
fi
#Call script does_user_exist.sh
./does_user_exist.sh "-login" "$login"
# if user exist then exit else create user
if [ $? -eq 0 ]; then # $? = contient le code de retour (ou exit status) de la dernière commande exécutée.
    echo "User already exist"
    exit 1
else
    echo "User does not exist, let's create it"
    useradd -m -d "/home/$login" -u "$custom_uid" -g "$custom_gid" -c "$custom_comment" "$login"
    if [ $? -eq 0 ]; then
        echo "L'utilisateur $login a été créé avec succès."
    else
        echo "Erreur lors de la création de l'utilisateur."
        exit 1
    fi
fi
exit 0
```
*https://www.cyberciti.biz/tips/howto-write-shell-script-to-add-user.html
https://stackoverflow.com/questions/18215973/how-to-check-if-running-as-root-in-a-bash-script
https://www.malekal.com/la-commande-useradd-utilisations-et-exemples/*

## Exercice : lecture au clavier
**comment quitter more ?** touche q.
**comment avancer d’une ligne ?** touche entrée.
**comment avancer d’une page ?** touche espace.
**comment remonter d’une page ?** touche b.
**comment chercher une chaı̂ne de caractères ? Passer à l’occurence suivante ?** touche / suivi de la chaîne recherchée, puis n pour passer à l'occurrence suivante.

```
#!/bin/bash
if [ $# -lt 1 ]; then
    echo "Wrong arguments"
    echo "Usage: ./<script name> <repository>"
    exit 1
fi
for fichier in "$1"/*; do
    if file "$fichier" | grep -q 'text'; then #-q = grep ne produit aucune sortie, mais retourne 0 si le mot est trouvé
        read -p "Voulez-vous visualiser le fichier $fichier ? (y/n) " choix
        if [ "$choix" = "y" ]; then
            more "$fichier"
        fi
    fi
done
exit 0
```
## Exercice : appréciation
```
#!/bin/bash
while true; do
    read -p "Entrer une note : " note
    if [ "$note" = q ]; then
        echo "Vous avez quitté le programme."
        exit 0
    fi
    if ! [[ "$note" =~ ^[0-9]+$ ]]; then
        echo "Veuillez entrer un nombre valide."
        continue
    fi
    if (( note >= 16 && note <= 20 )); then
        echo "Très bien"
    elif (( note >= 14 && note < 16 )); then
        echo "Bien"
    elif (( note >= 12 && note < 14 )); then
        echo "Assez bien"
    elif (( note >= 10 && note < 12 )); then
        echo "Moyen"
    elif (( note < 10 )); then
        echo "Insuffisant"
    else
        echo "Note invalide, entrez un nombre entre 0 et 20."
    fi
done
echo "Normalement on quitte le programme uniquement avec la touche q."
return 1
```