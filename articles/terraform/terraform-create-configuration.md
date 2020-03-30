---
title: Quickstart - Azure için Terraform yapılandırması oluşturma
description: Azure Cosmos DB ve Azure Kapsayıcı Örnekleri'ni dağıtarak Azure'da Terraform ile başlayın.
ms.topic: quickstart
ms.date: 10/26/2019
ms.openlocfilehash: 92f66fcfeac69b66eb8cdb0dc58b7a3171d45e10
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77470030"
---
# <a name="quickstart-create-a-terraform-configuration-for-azure"></a>Hızlı başlatma: Azure için bir Terraform yapılandırması oluşturma

Bu hızlı başlangıçta, Terraform'u kullanarak Azure kaynakları oluşturursunuz. Bu makaledeki adımlar, aşağıdaki kaynakları oluşturmada size yol verebmaktadır:

> [!div class="checklist"]
> * Azure Cosmos DB örneği
> * Azure Container Örneği
> * Bu iki kaynak ta çalışan uygulama

## <a name="create-first-configuration"></a>İlk yapılandırmayı oluşturma

Bu bölümde, bir Azure Cosmos DB örneği için yapılandırma oluşturursunuz.

1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.

1. Azure Bulut Kabuğu'nu açın.

1. Cloud Shell düzenleyicisini başlatın:

    ```bash
    code main.tf
    ```

1. Bu adımdaki yapılandırma birkaç Azure kaynağını modeller. Bu kaynaklar arasında bir Azure kaynak grubu ve bir Azure Cosmos DB örneği yer alır. Benzersiz bir Cosmos DB örnek adı oluşturmak için rasgele bir tamsayı kullanılır. Birkaç Cosmos DB ayarları da yapılandırılmıştır. Daha fazla bilgi için [Cosmos DB Terraform referansına](https://www.terraform.io/docs/providers/azurerm/r/cosmosdb_account.html)bakın. Aşağıdaki Terraform yapılandırmasını kopyalayıp düzenleyiciye yapıştırın:

    ```hcl
    resource "azurerm_resource_group" "vote-resource-group" {
      name     = "vote-resource-group"
      location = "westus"
    }

    resource "random_integer" "ri" {
      min = 10000
      max = 99999
    }

    resource "azurerm_cosmosdb_account" "vote-cosmos-db" {
      name                = "tfex-cosmos-db-${random_integer.ri.result}"
      location            = azurerm_resource_group.vote-resource-group.location
      resource_group_name = azurerm_resource_group.vote-resource-group.name
      offer_type          = "Standard"
      kind                = "GlobalDocumentDB"

      consistency_policy {
        consistency_level       = "BoundedStaleness"
        max_interval_in_seconds = 10
        max_staleness_prefix    = 200
      }

      geo_location {
        location          = "westus"
        failover_priority = 0
      }
    }
    ```

1. Dosyayı kaydedin (**&lt;Ctrl>S)** ve düzenleyiciden çıkın (**&lt;Ctrl>Q**).

## <a name="run-the-configuration"></a>Yapılandırmayı çalıştırma

Bu bölümde, yapılandırmayı çalıştırmak için birkaç Terraform komutu kullanırsınız.

1. [Terraform init](https://www.terraform.io/docs/commands/init.html) komutu çalışma dizinini başharfletir. Cloud Shell'de aşağıdaki komutu çalıştırın:

    ```bash
    terraform init
    ```

1. [Terraform planı](https://www.terraform.io/docs/commands/plan.html) komutu yapılandırma sözdizimini doğrulamak için kullanılabilir. `-out` Parametre sonuçları bir dosyaya yönlendirir. Çıktı dosyası daha sonra yapılandırmayı uygulamak için kullanılabilir. Cloud Shell'de aşağıdaki komutu çalıştırın:

    ```bash
    terraform plan --out plan.out
    ```

1. [Terraform uygulama](https://www.terraform.io/docs/commands/apply.html) komutu yapılandırmayı uygulamak için kullanılır. Önceki adımdaki çıktı dosyası belirtilir. Bu komut, Azure kaynaklarının oluşturulmasına neden olur. Cloud Shell'de aşağıdaki komutu çalıştırın:

    ```bash
    terraform apply plan.out
    ```

1. Azure portalındaki sonuçları doğrulamak için yeni kaynak grubuna göz atın. Yeni Azure Cosmos DB örneği yeni kaynak grubundadır.

## <a name="update-configuration"></a>Yapılandırmayı güncelleştirme

Bu bölümde, yapılandırmanın bir Azure Kapsayıcı Örneği içerecek şekilde nasıl güncelleştirileceği gösterilmektedir. Kapsayıcı, Cosmos DB'ye veri okuyan ve yazan bir uygulama çalıştırır.

1. Cloud Shell düzenleyicisini başlatın:

    ```bash
    code main.tf
    ```

1. Bu adımdaki yapılandırma iki ortam `COSMOS_DB_ENDPOINT` değişkeni ayarlar: ve `COSMOS_DB_MASTERKEY`. Bu değişkenler veritabanına erişmek için konumu ve anahtarı tutar. Bu değişkenlerin değerleri önceki adımda oluşturulan veritabanı örneğinden elde edilir. Bu işlem enterpolasyon olarak bilinir. Terraform enterpolasyonu hakkında daha fazla bilgi edinmek için [Enterpolasyon Sözdizimi'ne](https://www.terraform.io/docs/configuration/interpolation.html)bakın. Yapılandırma, kapsayıcı örneğinin tam nitelikli etki alanı adını (FQDN) döndüren bir çıktı bloğu da içerir. Aşağıdaki kodu kopyalayıp düzenleyiciye yapıştırın:

    ```hcl
    resource "azurerm_container_group" "vote-aci" {
      name                = "vote-aci"
      location            = azurerm_resource_group.vote-resource-group.location
      resource_group_name = azurerm_resource_group.vote-resource-group.name
      ip_address_type     = "public"
      dns_name_label      = "vote-aci"
      os_type             = "linux"

      container {
        name   = "vote-aci"
        image  = "microsoft/azure-vote-front:cosmosdb"
        cpu    = "0.5"
        memory = "1.5"
        ports {
          port     = 80
          protocol = "TCP"
        }

        secure_environment_variables = {
          "COSMOS_DB_ENDPOINT"  = azurerm_cosmosdb_account.vote-cosmos-db.endpoint
          "COSMOS_DB_MASTERKEY" = azurerm_cosmosdb_account.vote-cosmos-db.primary_master_key
          "TITLE"               = "Azure Voting App"
          "VOTE1VALUE"          = "Cats"
          "VOTE2VALUE"          = "Dogs"
        }
      }
    }

    output "dns" {
      value = azurerm_container_group.vote-aci.fqdn
    }
    ```

1. Dosyayı kaydedin (**&lt;Ctrl>S)** ve düzenleyiciden çıkın (**&lt;Ctrl>Q**).

1. Önceki bölümde yaptığınız gibi, yapılacak değişiklikleri görsel olarak aşağıdaki komutu çalıştırın:

    ```bash
    terraform plan --out plan.out
    ```

1. Yapılandırmayı `terraform apply` uygulamak için komutu çalıştırın.

    ```bash
    terraform apply plan.out
    ```

1. Kapsayıcı örneği FQDN not olun.

## <a name="test-application"></a>Uygulamayı test etme

Uygulamayı sınamak için, kapsayıcı örneğinin FQDN'sine gidin. Aşağıdaki çıktıya benzer sonuçlar görmeniz gerekir:

![Azure oy uygulaması](media/terraform-quickstart/azure-vote.jpg)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, bu makalede oluşturulan kaynakları silin.

Bu öğreticide oluşturulan Azure kaynaklarını kaldırmak için [terraform yok](https://www.terraform.io/docs/commands/destroy.html) komutunu çalıştırın:

```bash
terraform destroy -auto-approve
```

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure kaynaklarını sağlamak için Terraform'u yükleyin ve yapılandırın.](terraform-install-configure.md)