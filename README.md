# Lab 1 Phase 2 — Terraform VPC from scratch

> Création d'un VPC AWS complet en Infrastructure as Code avec Terraform.
> Premier lab de la **Phase 2 — Terraform**.

[![Terraform](https://img.shields.io/badge/Terraform-v1.15.7-7B42BC?logo=terraform&logoColor=white)]()
[![AWS](https://img.shields.io/badge/AWS-VPC-FF9900?logo=amazonaws&logoColor=white)]()
[![Region](https://img.shields.io/badge/Region-eu--west--3-58A6FF)]()
[![Status](https://img.shields.io/badge/Status-Validated-3FB950)]()

---

## 🎯 Objectif

Reproduire en code Terraform l'infrastructure VPC créée manuellement en Phase 1, en appliquant les principes d'Infrastructure as Code.

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
├── main.tf          ← ressources AWS (provider, VPC, subnets, IGW, routing)
├── variables.tf     ← paramètres configurables
├── outputs.tf       ← valeurs exposées après apply
└── .gitignore       ← exclut .terraform/ et *.tfstate
```

## 🚀 Déploiement

📄 **[Lab1_Phase2_Terraform_Detaille.pdf](./Lab1_Phase2_Terraform_Detaille.pdf)**

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

## 📊 Outputs

```hcl
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
- [x] `terraform destroy` — 12 ressources supprimées

## 🎓 Compétences mises en pratique

- HCL (HashiCorp Configuration Language)
- Variables Terraform (string, list, default)
- Resources et références inter-ressources
- Méta-argument count avec length()
- Interpolation de variables dans les strings
- Outputs Terraform
- Workflow init → plan → apply → destroy
- .gitignore pour exclure le state et les providers

## ⚙️ Prérequis

```bash
# Terraform installé
terraform version  # >= 1.0.0

# AWS CLI configuré
aws configure
aws sts get-caller-identity
```

---

**Parcours :** BTS SIO SISR → Bachelor ESGI Cloud/Réseaux → Mastère
**Objectif :** Cloud Infrastructure / Platform Engineer
