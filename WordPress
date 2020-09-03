provider "aws" {
  region = "ap-south-1"
  access_key = "your access_key"
  secret_key = "your secret_key"
}

provider "kubernetes" {
  config_context_cluster  = "minikube"
}
resource "kubernetes_deployment" "wordpress" {
  metadata {
    name = "wordpress"
    labels = {
      test = "MyExampleApp"
    }
  }
  spec {
    replicas = 1
       strategy {
            type = "RollingUpdate"
        }
    selector {
       match_labels = {
                type = "cms"
                env = "prod"
            }
    }
    template {
      metadata {
      labels ={
               type = "cms"
               env = "prod"
                }
      }
      spec {
        container {
          image = "wordpress"
          name  = "wordpress"
          port{
            container_port = 80
          }
          }
        }
      }
    }
  }
resource "kubernetes_service" "Nodeport" {
  depends_on=[kubernetes_deployment.wordpress]
  metadata {
    name = "terraform-example"
  }
    spec {
        type = "NodePort"
        selector = {
          type = "cms"
        }
        port {
            port = 80
            target_port = 80
            protocol = "TCP"
        }
    }
}
