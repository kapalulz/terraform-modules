 #cd .. < projectA > main.tf >
 
 provider "aws" {
  region     = "eu-north-1"
  access_key = "AKIAZYSWY6T2AQLJDHFC"
  secret_key = "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"
}

/*
module "vpc-default" {
  source = "../modules/aws_network"
}*/

module "vpc-dev" {
  #source               = "../modules/aws_network"
  source               = "git@github.com:kapalulz/terraform-modules.git//aws_network"
  env                  = "development"
  vpc_cidr             = "10.100.0.0/16"
  public_subnet_cidrs  = ["10.100.1.0/24", "10.100.2.0/24"]
  private_subnet_cidrs = []
}

module "vpc-prod" {
  #source               = "../modules/aws_network"
  source               = "git@github.com:kapalulz/terraform-modules.git//aws_network"
  env                  = "production"
  vpc_cidr             = "10.100.0.0/16"
  public_subnet_cidrs  = ["10.100.1.0/24", "10.100.2.0/24", "10.100.3.0/24"]
  private_subnet_cidrs = ["10.100.11.0/24", "10.100.22.0/24", "10.100.33.0/24"]
}

module "vpc-test" {
  #source               = "../modules/aws_network"
  source               = "git@github.com:kapalulz/terraform-modules.git//aws_network"
  env                  = "production"
  vpc_cidr             = "10.100.0.0/16"
  public_subnet_cidrs  = ["10.100.1.0/24", "10.100.2.0/24"]
  private_subnet_cidrs = ["10.100.11.0/24", "10.100.22.0/24"]
}


#===========================================

output "prod_public_subnet_ids" {
  value = module.vpc-prod.public_subnet_ids
}

output "prod_private_subnet_ids" {
  value = module.vpc-prod.private_subnet_ids
}


output "dev_public_subnet_ids" {
  value = module.vpc-dev.public_subnet_ids
}

output "dev_private_subnet_ids" {
  value = module.vpc-dev.private_subnet_ids
}

