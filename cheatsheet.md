### What is Terraform ?

## Les blocks :

# terraform 
contient les paramètres, les providers requis pour provisionner les infras

# provider:
on utilise un block **required_providers**
pour chaque provider on définit la source et la version à utiliser:
      source  = "hashicorp/aws" | On va chercher dans la registry Terraform  "hashicorp/aws" -> registry.terraform.io/hashicorp/aws
      version = "~> 4.16" | parametre optionnel mais recommandé .

