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
source  = "hashicorp/aws" # On va chercher dans la registry Terraform  "hashicorp/aws" -> registry.terraform.io/hashicorp/aws
version = "~> 4.16" # parametre optionnel mais recommandé


On peut utiliser plusieurs provider ensemble.

### resource
Definit les composants de l'infra :
* physique
* virtuel ( ec2 par ex )
* logique ( une application heroku )

