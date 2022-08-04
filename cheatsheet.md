# What is Terraform ?

## Les blocks :

### terraform 
contient les paramètres, les providers requis pour provisionner les infras
```
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }

  required_version = ">= 1.2.0"
}
```


### provider
on utilise un block **required_providers** :

```
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.16"
    }
  }
```

pour chaque provider on définit la source et la version à utiliser: 
* source  = "hashicorp/aws" # On va chercher dans la registry Terraform  "hashicorp/aws" -> registry.terraform.io/hashicorp/aws
* version = "~> 4.16" # parametre optionnel mais recommandé


On peut utiliser plusieurs provider ensemble.

### resource
Definit les composants de l'infra :
* physique
* virtuel ( ec2 par ex )
* logique ( une application heroku )

```
resource "aws_instance" "app_server" {
  ami           = "ami-830c94e3"
  instance_type = "t2.micro"

  tags = {
    Name = "ExampleAppServerInstance"
  }
}
```

Le bloc est construit de la manière suivante : 
`resource "resource type" "resource name"`.  
Le prefix du type correspond au nom du provider .  
Le resource type + le resource name = unique ID de la resource : `aws_instance.app_server`

On lui indique en argument la config de la ressource qu'on veut provisionner :
* ami : correspond à l'image utilisé pour lancer la ec2
* instance_type : correspond au type de vm utilisé

On peut utiliser un bloc tag en complément pour justement tagger la ressource.


## Les commandes
1. init :
* on initialise le repertoire avec `terraform init`
* télécharge et installe les providers définit dans le conf
* l'install se fait un dossier `.terraform`
* creation d'un fichier `.terraform.lock.hcl` qui specifie le provider et la version installée.

2. fmt / validate
* la commande `terraform fmt` permet de màj le formatage des fichiers de conf dans le dossier courant.
* on peut utiliser en complément la commande `terraform validate` qui valide que la syntaxe des fichiers est ok

3. apply
* présente le plan d'execution qui décrit les actions :
  * présente un `+` devant les ressources à créer et un `-` devant les actions à détruire , un `~` indique une modification.
  * applique la configuration

4. show
* inspecte l'état actuel du fichier `terraform.tfstate`

5. state
* permet de voir et de modifier les ressources dans le fichier state
* `terraform state [subcommand]` 
* Subcommands:
  * list                List resources in the state
  * mv                  Move an item in the state
  * pull                Pull current state and output to stdout
  * push                Update remote state from a local state file
  * replace-provider    Replace provider in the state
  * rm                  Remove instances from the state
  * show                Show a resource in the state

6. destroy
* détruit uniquement les ressources présentent dans le `terraform.tfstate`  
* `terraform destroy`

7. output
* permet d'afficher les valeurs défini dans le fichier `ouput.tf`

## Variable:
### les types de variables

* number
* string
* list : sequence de valeur de mm type
* map : table de recherche. clé - valeur . mm type
* set : collection de valeur unique non ordonnée de mm type

On définit des variables dans un fichier variables.tf :

```
variable "instance_name" {
  description = "Value of the Name tag for the EC2 instance"
  type        = string
  default     = "02_Ec2_example"
}
```
On appelle ensuite cette variables dans notre main.tf :
```
  tags = {
    Name = var.instance_name
  }
```
la valeur de notre clé "Name" sera donc la valeur par défaut à savoir **02_Ec2_example**

On peut également lors du lancement de la commande terraform ajouté un **-var** pour provisionner la variable : `terraform apply -var "instance_name=UnAutreNom"`


## Output
On définit dans un fichier `output.tf` les valeurs que l'on veut récuperer. 