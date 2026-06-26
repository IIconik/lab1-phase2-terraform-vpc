# Lab 1 — Phase 2 — Terraform VPC + Remote State

> Création d'un VPC AWS complet en Infrastructure as Code avec Terraform et configuration du remote state S3.
> Premier lab de la **Phase 2 — Terraform**.

[![Terraform](https://img.shields.io/badge/Terraform-v1.15.7-7B42BC?logo=terraform&logoColor=white)]()
[![AWS](https://img.shields.io/badge/AWS-VPC%20%2B%20S3-FF9900?logo=amazonaws&logoColor=white)]()
[![Region](https://img.shields.io/badge/Region-eu--west--3-58A6FF)]()
[![Status](https://img.shields.io/badge/Status-Validated-3FB950)]()

---

## 🎯 Objectif

Reproduire en code Terraform le VPC créé manuellement en Phase 1, et configurer un remote state S3 pour permettre le travail en équipe.

## 🏗️ Infrastructure déployée

```
VPC 10.0.0.0/16
├── Internet Gateway
├── Subnet PUBLIC  10.0.1.0/24 (eu-west-3a) ─── rt-public (0.0.0.0/0 → IGW)
├── Subnet PUBLIC  10.0.2.0/24 (eu-west-3b) ─── rt-public
├── Subnet PRIVÉ   10.0.3.0/24 (eu-west-3a) ─── rt-private
└── Subnet PRIVÉ   10.0.4.0/24 (eu-west-3b) ─── rt-private
```

**12 ressources Terraform** — 3 fichiers de code, 2 commandes.

## 📁 Structure du projet

```
lab1-phase2-terraform-vpc/
├── main.tf          ← provider + VPC + subnets + IGW + route tables + backend S3
├── variables.tf     ← paramètres configurables
├── outputs.tf       ← valeurs exposées après apply
└── .gitignore       ← exclut .terraform/ et *.tfstate
```

## 🔧 Remote State S3

Le state Terraform est stocké dans S3 pour permettre le travail en équipe :

```hcl
backend "s3" {
  bucket       = "terraform-state-nathan-2701"
  key          = "phase2/lab1/terraform.tfstate"
  region       = "eu-west-3"
  use_lockfile = true
  encrypt      = true
}
```

## 📄 Procédures

📄 **[Lab1_Phase2_Terraform_Detaille.pdf](./Lab1_Phase2_Terraform_Detaille.pdf)** — VPC en Terraform (cours + procédure)

📄 **[Proc_Remote_State_S3.pdf](./Proc_Remote_State_S3.pdf)** — Remote State S3 (cours + procédure)

## 🚀 Déploiement

```bash
# Initialiser Terraform
terraform init

# Vérifier ce qui sera créé
terraform plan

# Déployer
terraform apply

# Supprimer
terraform destroy
```

## 📊 Outputs après apply

```
igw_id             = "igw-0544fa6e0c0e72cef"
private_subnet_ids = ["subnet-0290a7736fb59582e", "subnet-0233813e67fc8e042"]
public_subnet_ids  = ["subnet-0d2608853e4837a93", "subnet-074026e378eb0c759"]
vpc_id             = "vpc-007ba14b76c14488b"
```

## ✅ Validation

- [x] `terraform init` — Provider AWS v5.100.0 installé
- [x] `terraform plan` — 12 to add, 0 to change, 0 to destroy
- [x] `terraform apply` — 12 ressources créées, outputs affichés
- [x] Vérification console AWS — vpc-phase2-lab1 visible
- [x] Remote state visible dans S3 après apply
- [x] Verrou `use_lockfile = true` fonctionnel
- [x] `terraform destroy` — 12 ressources supprimées proprement

## 🔍 Concepts clés appris

| Concept | Explication |
|---------|-------------|
| **HCL** | Langage de configuration HashiCorp |
| **variable** | Paramètre configurable, défini une fois, utilisé partout |
| **resource** | Ressource AWS à créer (aws_vpc, aws_subnet...) |
| **count** | Crée N ressources identiques sans dupliquer le code |
| **output** | Valeur affichée après apply, partageable entre modules |
| **backend s3** | State stocké dans S3, partageable en équipe |
| **use_lockfile** | Verrou S3 — empêche les apply simultanés |

## ⚙️ Prérequis

```bash
terraform version  # >= 1.0.0
aws sts get-caller-identity  # terraform-user configuré
```

## 🎓 Compétences mises en pratique

- Infrastructure as Code (IaC) avec Terraform
- HCL : variables, resources, outputs, interpolation
- Méta-argument count avec length()
- Workflow init → plan → apply → destroy
- Remote state S3 avec verrou
- Migration state local → remote

---

**Parcours :** BTS SIO SISR → Bachelor ESGI Cloud/Réseaux → Mastère
**Objectif :** Cloud Infrastructure / Platform Engineer
