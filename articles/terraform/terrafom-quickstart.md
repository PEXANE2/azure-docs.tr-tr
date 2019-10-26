---
title: Azure 'da Terrayform 'a giriş.
description: Azure 'da bir Azure Cosmos DB ve Azure Container Instances dağıtarak Terrayform ile çalışmaya başlayın.
services: terraform
author: neilpeterson
ms.service: azure
ms.topic: quickstart
ms.date: 09/20/2019
ms.author: nepeters
ms.openlocfilehash: 6f9b6a73e279ca5923e32c7c524f9a55e260526d
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/25/2019
ms.locfileid: "72924826"
---
# <a name="create-a-terraform-configuration-for-azure"></a>Azure için Terraform yapılandırması oluşturma

Bu örnekte, bir Teraform yapılandırması oluşturma ve bu yapılandırmayı Azure 'a dağıtma konusunda deneyim elde edersiniz. Tamamlandığında, bir Azure Cosmos DB örneği, bir Azure Container Instance ve bu iki kaynak arasında çalışacak bir uygulama dağıtmış olursunuz. Bu belge, önkoşul olarak önceden yüklenmiş olan Azure Cloud Shell tüm çalışmanın tamamlandığını varsayar. Kendi sisteminizdeki örnek aracılığıyla çalışmak isterseniz, [burada](../virtual-machines/linux/terraform-install-configure.md)bulunan yönergeleri kullanarak Terırform yükleyebilirsiniz.

## <a name="create-first-configuration"></a>İlk yapılandırma oluştur

Bu bölümde, bir Azure Cosmos DB örneği için yapılandırma oluşturacaksınız.

Azure Cloud Shell 'i açmak için **Şimdi deneyin** ' i seçin. Açıldığında, Cloud Shell kod düzenleyicisini açmak için `code .` girin.

```bash
code .
```

Aşağıdaki Terpform yapılandırmasında kopyalayıp yapıştırın.

Bu yapılandırma, bir Azure kaynak grubunu, rastgele bir tamsayıyı ve bir Azure Cosmos DB örneğini modeller. Rastgele tamsayı Cosmos DB örnek adında kullanılır. Çeşitli Cosmos DB ayarları da yapılandırılır. Cosmos DB Teraform yapılandırmalarının tüm listesi için bkz. [Cosmos DB terrayform başvurusu](https://www.terraform.io/docs/providers/azurerm/r/cosmosdb_account.html).

Bitince dosyayı `main.tf` olarak kaydedin. Bu işlem, kod düzenleyicisinin sağ üst kısmındaki üç nokta kullanılarak yapılabilir.

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

[Terrayform init](https://www.terraform.io/docs/commands/init.html) komutu çalışma dizinini başlatır. Yeni yapılandırmanın dağıtımına hazırlanmak için Cloud Shell terminalinde `terraform init` çalıştırın.

```bash
terraform init
```

[Terrayform plan](https://www.terraform.io/docs/commands/plan.html) komutu, yapılandırmanın düzgün şekilde biçimlendirildiğini doğrulamak ve hangi kaynakların oluşturulacağını, güncelleştirileceğini veya yok edildiğini görselleştirmek için kullanılabilir. Sonuçlar bir dosyada depolanabilir ve daha sonra yapılandırmayı uygulamak için kullanılabilir.

Yeni Teraform yapılandırmasını test etmek için `terraform plan` çalıştırın.

```bash
terraform plan --out plan.out
```

[Terrayform uygulamasını](https://www.terraform.io/docs/commands/apply.html) kullanarak yapılandırmayı uygulayın ve plan dosyasının adını belirtin. Bu komut, Azure aboneliğinizdeki kaynakları dağıtır.

```bash
terraform apply plan.out
```

İşiniz bittiğinde, kaynak grubunun oluşturulduğunu ve kaynak grubuna yerleştirilmiş bir Azure Cosmos DB örneği olduğunu görebilirsiniz.

## <a name="update-configuration"></a>Güncelleştirme yapılandırması

Yapılandırmayı bir Azure Container Instance içerecek şekilde güncelleştirin. Kapsayıcı Cosmos DB verileri okuyan ve yazan bir uygulamayı çalıştırır.

Aşağıdaki yapılandırmayı `main.tf` dosyasının altına kopyalayın. Bitince dosyayı kaydedin.

İki ortam değişkeni ayarlanır, `COSMOS_DB_ENDPOINT` ve `COSMOS_DB_MASTERKEY`. Bu değişkenler veritabanına erişmek için konum ve anahtarı tutar. Bu değişkenlerin değerleri, son adımda oluşturulan veritabanı örneğinden alınır. Bu işlem ilişkilendirme olarak bilinir. Terrayform ilişkilendirme hakkında daha fazla bilgi için bkz. [enterpolasyon sözdizimi](https://www.terraform.io/docs/configuration/interpolation.html).


Yapılandırma ayrıca kapsayıcı örneğinin tam etki alanı adını (FQDN) döndüren bir çıkış bloğu da içerir.

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

Güncelleştirilmiş planı oluşturmak ve yapılacak değişiklikleri görselleştirmek için `terraform plan` çalıştırın. Yapılandırmaya bir Azure Container Instance kaynağının eklendiğini görmeniz gerekir.

```bash
terraform plan --out plan.out
```

Son olarak, yapılandırmayı uygulamak için `terraform apply` çalıştırın.

```bash
terraform apply plan.out
```

İşlem tamamlandıktan sonra kapsayıcı örneği FQDN 'sine göz atın.

## <a name="test-application"></a>Uygulamayı test etme

Kapsayıcı örneğinin FQDN 'sine gidin. Her şey doğru şekilde yapılandırıldıysa, aşağıdaki uygulamayı görmeniz gerekir.

![Azure oy uygulaması](media/terraform-quickstart/azure-vote.jpg)

## <a name="clean-up-resources"></a>Kaynakları temizleme

İşiniz bittiğinde, Azure kaynakları ve kaynak grubu [terkform Destroy](https://www.terraform.io/docs/commands/destroy.html) komutu kullanılarak kaldırılabilir.

```bash
terraform destroy -auto-approve
```

## <a name="next-steps"></a>Sonraki adımlar

Bu örnekte, bir Terrayform yapılandırması oluşturdunuz, dağıtıldı ve yok edilir. Azure 'da Terrampaform kullanma hakkında daha fazla bilgi için bkz. Azure Teraform sağlayıcı belgeleri.

> [!div class="nextstepaction"]
> [Azure Terrayform sağlayıcısı](https://www.terraform.io/docs/providers/azurerm/)
