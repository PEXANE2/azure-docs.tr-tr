---
title: Azure Cloud Shell'den Terraform ile dağıtın | Microsoft Dokümanlar
description: Azure Cloud Shell'den Terraform ile dağıtma
services: Azure
documentationcenter: ''
author: tomarchermsft
manager: routlaw
tags: azure-cloud-shell
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/15/2017
ms.author: tarcher
ms.openlocfilehash: 8bacadd8941131f608411e61cc15c120c1b2bc60
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79458163"
---
# <a name="deploy-with-terraform-from-bash-in-azure-cloud-shell"></a>Azure Cloud Shell'de Bash'ten Terraform ile dağıtın
Bu makale, [Terraform AzureRM sağlayıcısıyla](https://www.terraform.io/docs/providers/azurerm/index.html)bir kaynak grubu oluşturmanıza yardımcı olur.

[Hashicorp Terraform,](https://www.terraform.io/) API'leri, düzenlenecek, gözden geçirilecek ve sürümlendirilecek ekip üyeleri arasında paylaşılabilen bildirimsel yapılandırma dosyalarına kodlayan açık kaynak bir araçtır. Microsoft AzureRM sağlayıcısı, AzureRM API'leri aracılığıyla Azure Kaynak Yöneticisi tarafından desteklenen kaynaklarla etkileşim kurmak için kullanılır.

## <a name="automatic-authentication"></a>Otomatik kimlik doğrulama
Terraform varsayılan olarak Bulut Kabuğu'ndaki Bash'e yüklenir. Ayrıca Bulut Shell, Terraform Azure modülleri aracılığıyla kaynakları dağıtmak için varsayılan Azure CLI aboneliğinizi otomatik olarak doğrular.

Terraform, ayarlanan varsayılan Azure CLI aboneliğini kullanır. Varsayılan abonelikleri güncelleştirmek için çalıştırın:

```azurecli-interactive
az account set --subscription mySubscriptionName
```

## <a name="walkthrough"></a>Kılavuz
### <a name="launch-bash-in-cloud-shell"></a>Cloud Shell'de Bash'i başlatın
1. Bulut Shell'i tercih ettiğiniz konumdan başlatın
2. Tercih ettiğiniz aboneliğin ayarlı olduğunu doğrulayın

```azurecli-interactive
az account show
```

### <a name="create-a-terraform-template"></a>Terraform şablonu oluşturma
Tercih ettiğiniz metin düzenleyicisi ile main.tf adlı yeni bir Terraform şablonu oluşturun.

```
vim main.tf
```

Aşağıdaki kodu Bulut Kabuğu'na kopyalayın/yapıştırın.

```
resource "azurerm_resource_group" "myterraformgroup" {
    name = "myRgName"
    location = "West US"
}
```

Dosyanızı kaydedin ve metin düzenleyicinizden çıkın.

### <a name="terraform-init"></a>Terraform init
Çalıştırarak `terraform init`başlayın.

```
justin@Azure:~$ terraform init

Initializing provider plugins...

The following providers do not have any version constraints in configuration,
so the latest version was installed.

To prevent automatic upgrades to new major versions that may contain breaking
changes, it is recommended to add version = "..." constraints to the
corresponding provider blocks in configuration, with the constraint strings
suggested below.

* provider.azurerm: version = "~> 0.2"

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

[Terraform init komutu,](https://www.terraform.io/docs/commands/init.html) Terraform yapılandırma dosyalarını içeren bir çalışma dizinini başlatmak için kullanılır. Komut, `terraform init` yeni bir Terraform yapılandırması yazdıktan veya varolan bir yapılandırmayı sürüm denetiminden klonladıktan sonra çalıştırılması gereken ilk komuttur. Bu komutu birden çok kez çalıştırmak güvenlidir.

### <a name="terraform-plan"></a>Terraform plan
Terraform şablonu tarafından oluşturulacak kaynakları `terraform plan`önizleyin.

```
justin@Azure:~$ terraform plan
Refreshing Terraform state in-memory prior to plan...
The refreshed state will be used to calculate this plan, but will not be
persisted to local or remote state storage.


------------------------------------------------------------------------

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + azurerm_resource_group.demo
      id:       <computed>
      location: "westus"
      name:     "myRGName"
      tags.%:   <computed>


Plan: 1 to add, 0 to change, 0 to destroy.

------------------------------------------------------------------------

Note: You didn't specify an "-out" parameter to save this plan, so Terraform
can't guarantee that exactly these actions will be performed if
"terraform apply" is subsequently run.
```

[terraform plan komutu](https://www.terraform.io/docs/commands/plan.html), yürütme planı oluşturmak için kullanılır. Terraform açıkça devre dışı bırakılmadığı sürece bir yenileme gerçekleştirir ve yapılandırma dosyalarında belirtilen durumu elde etmek için hangi eylemlerin gerekli olduğunu belirler. Plan -out kullanılarak kaydedilebilir ve daha sonra sadece önceden planlanmış eylemlerin yürütülmesini sağlamak için terraform uygulamak için sağlanabilir.

### <a name="terraform-apply"></a>Terraform apply
Azure kaynaklarını ' `terraform apply`ile sağlama

```
justin@Azure:~$ terraform apply
azurerm_resource_group.demo: Creating...
  location: "" => "westus"
  name:     "" => "myRGName"
  tags.%:   "" => "<computed>"
azurerm_resource_group.demo: Creation complete after 0s (ID: /subscriptions/mySubIDmysub/resourceGroups/myRGName)

Apply complete! Resources: 1 added, 0 changed, 0 destroyed.
```

[Terraform uygulama komutu,](https://www.terraform.io/docs/commands/apply.html) yapılandırmanın istenilen durumuna ulaşmak için gereken değişiklikleri uygulamak için kullanılır.

### <a name="verify-deployment-with-azure-cli"></a>Azure CLI ile dağıtımı doğrula
Kaynağın sağlamayı başarada başarada olduğunu doğrulamak için çalıştırın. `az group show -n myRgName`

```azurecli-interactive
az group show -n myRgName
```

### <a name="clean-up-with-terraform-destroy"></a>Terraform destroy ile temizleyin
Terraform tarafından oluşturulan altyapıyı temizlemek için [Terraform yok komutu](https://www.terraform.io/docs/commands/destroy.html) ile oluşturulan kaynak grubunu temizleyin.

```
justin@Azure:~$ terraform destroy
azurerm_resource_group.demo: Refreshing state... (ID: /subscriptions/mySubID/resourceGroups/myRGName)

An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  - azurerm_resource_group.demo


Plan: 0 to add, 0 to change, 1 to destroy.

Do you really want to destroy?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

azurerm_resource_group.demo: Destroying... (ID: /subscriptions/mySubID/resourceGroups/myRGName)
azurerm_resource_group.demo: Still destroying... (ID: /subscriptions/mySubID/resourceGroups/myRGName, 10s elapsed)
azurerm_resource_group.demo: Still destroying... (ID: /subscriptions/mySubID/resourceGroups/myRGName, 20s elapsed)
azurerm_resource_group.demo: Still destroying... (ID: /subscriptions/mySubID/resourceGroups/myRGName, 30s elapsed)
azurerm_resource_group.demo: Still destroying... (ID: /subscriptions/mySubID/resourceGroups/myRGName, 40s elapsed)
azurerm_resource_group.demo: Destruction complete after 45s

Destroy complete! Resources: 1 destroyed.
```

Terraform aracılığıyla bir Azure kaynağını başarıyla oluşturdunuz. Cloud Shell hakkında bilgi edinmeye devam etmek için sonraki adımları ziyaret edin.

## <a name="next-steps"></a>Sonraki adımlar
[Terraform Azure sağlayıcısı hakkında bilgi edinin](https://www.terraform.io/docs/providers/azurerm/#)<br>
[Bulut Kabuğunda Bash hızlı başlat](quickstart.md)
