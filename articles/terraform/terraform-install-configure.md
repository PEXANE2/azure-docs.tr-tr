---
title: Quickstart - Azure kaynaklarını sağlamak için Terraform'u yükleme ve yapılandırma
description: Bu quicstart'ta, Azure kaynakları oluşturmak için Terraform'u yükleyip yapılandırırsınız
keywords: azure devops terraform yükleme yapılandırma
ms.topic: quickstart
ms.date: 03/09/2020
ms.openlocfilehash: 82635f59ec8165add2046a230a040b06f89d9898
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78943514"
---
# <a name="quickstart-install-and-configure-terraform-to-provision-azure-resources"></a>Hızlı başlatma: Azure kaynaklarını sağlamak için Terraform'u yükleme ve yapılandırma
 
Terraform, basit bir [cezbedici dil](https://www.terraform.io/docs/configuration/syntax.html)kullanarak bulut altyapısını tanımlamak, önizlemek ve dağıtmak için kolay bir yol sağlar. Bu makalede, Azure'da kaynakları sağlamak için Terraform'u kullanmak için gerekli adımlar açıklanmaktadır.

Azure ile Terraform'u nasıl kullanacağınız hakkında daha fazla bilgi edinmek için [Terraform Hub'ı](/azure/terraform)ziyaret edin.
> [!NOTE]
> Terraform'a özel destek için lütfen topluluk kanallarından birini kullanarak Terraform'a doğrudan ulaşın:
>
>    * Topluluk portalının [Terraform bölümü](https://discuss.hashicorp.com/c/terraform-core) sorular, kullanım örnekleri ve yararlı desenler içerir.
>
>    * Sağlayıcıyla ilgili sorularınız için lütfen topluluk portalının [Terraform Sağlayıcıları](https://discuss.hashicorp.com/c/terraform-providers) bölümünü ziyaret edin.



[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Terraform varsayılan olarak Bulut [Kabuğu'na](/azure/terraform/terraform-cloud-shell)yüklenir. Terraform'u yerel olarak yüklemeyi seçerseniz, bir sonraki adımı tamamlayın, aksi takdirde [Azure'a Terraform erişimini ayarlamaya](#set-up-terraform-access-to-azure)devam edin.

## <a name="install-terraform"></a>Terraform'u Yükleyin

Terraform'u yüklemek için işletim sisteminiz için uygun paketi ayrı bir yükleme dizinine [indirin.](https://www.terraform.io/downloads.html) Karşıdan yükleme, genel bir yol tanımlamanız gereken tek bir yürütülebilir dosya içerir. Linux ve Mac'te yolu nasıl ayarlayabilmek için [bu web sayfasına](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux)gidin. Windows'da yolu nasıl ayarlayabildiğini anlatan talimatlar için [bu web sayfasına](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows)gidin.

Yol yapılandırmanızı komutla doğrulayın. `terraform` Aşağıdaki örnek çıktıda olduğu gibi kullanılabilir Terraform seçeneklerinin listesi gösterilir:

```console
azureuser@Azure:~$ terraform
Usage: terraform [--version] [--help] <command> [args]
```

## <a name="set-up-terraform-access-to-azure"></a>Azure'a Terraform erişimi ayarlama

Terraform'un Azure'a kaynak sağlamasına olanak sağlamak için bir [Azure REKLAM hizmeti ilkesi](/cli/azure/create-an-azure-service-principal-azure-cli)oluşturun. Hizmet sorumlusu, Azure aboneliğinizde kaynak sağlama için Terraform komut dosyalarınızı verir.

Birden çok Azure aboneliğiniz varsa, abonelik kimliği ve kiracı kimliği değerlerinin listesini almak için önce hesabınızı [az hesap listesiyle](/cli/azure/account#az-account-list) sorgula:

```azurecli-interactive
az account list --query "[].{name:name, subscriptionId:id, tenantId:tenantId}"
```

Seçili bir aboneliği kullanmak için, bu oturumun aboneliğini [az hesap kümesiyle](/cli/azure/account#az-account-set)ayarlayın. `SUBSCRIPTION_ID` Kullanmak istediğiniz abonelikten döndürülen `id` alanın değerini tutacak ortam değişkenini ayarlayın:

```azurecli-interactive
az account set --subscription="${SUBSCRIPTION_ID}"
```

Artık Terraform ile kullanmak üzere bir hizmet prensibi oluşturabilirsiniz. [Az reklam sp create-for-rbac'ı](/cli/azure/ad/sp#az-ad-sp-create-for-rbac)kullanın ve aboneliğinizin *kapsamını* aşağıdaki gibi ayarlayın:

```azurecli-interactive
az ad sp create-for-rbac --role="Contributor" --scopes="/subscriptions/${SUBSCRIPTION_ID}"
```

Sizin `appId` `password`, `sp_name`, `tenant` , ve döndürülür. Bir not `appId` edin `password`ve.

## <a name="configure-terraform-environment-variables"></a>Terraform ortam değişkenlerini yapılandırma

Terraform'u Azure AD hizmet asıllığınızı kullanacak şekilde yapılandırmak için, azure [Terraform modülleri](https://registry.terraform.io/modules/Azure)tarafından kullanılan aşağıdaki ortam değişkenlerini ayarlayın. Azure geneli dışında bir Azure bulutuyla çalışıyorsanız ortamı da ayarlayabilirsiniz.

- `ARM_SUBSCRIPTION_ID`
- `ARM_CLIENT_ID`
- `ARM_CLIENT_SECRET`
- `ARM_TENANT_ID`
- `ARM_ENVIRONMENT`

Bu değişkenleri ayarlamak için aşağıdaki örnek kabuk komut dosyasını kullanabilirsiniz:

```bash
#!/bin/sh
echo "Setting environment variables for Terraform"
export ARM_SUBSCRIPTION_ID=your_subscription_id
export ARM_CLIENT_ID=your_appId
export ARM_CLIENT_SECRET=your_password
export ARM_TENANT_ID=your_tenant_id

# Not needed for public, required for usgovernment, german, china
export ARM_ENVIRONMENT=public
```

## <a name="run-a-sample-script"></a>Örnek komut dosyası çalıştırma

Boş bir `test.tf` dizin ve yapıştırın aşağıdaki komut dosyasında bir dosya oluşturun.

```hcl
provider "azurerm" {
  # The "feature" block is required for AzureRM provider 2.x. 
  # If you are using version 1.x, the "features" block is not allowed.
  version = "~>2.0"
  features {}
}
resource "azurerm_resource_group" "rg" {
        name = "testResourceGroup"
        location = "westus"
}
```

Dosyayı kaydedin ve Terraform dağıtımını başlatmayı önarayın. Bu adım, bir Azure kaynak grubu oluşturmak için gereken Azure modüllerini karşıdan yükler.

```bash
terraform init
```

Çıktı aşağıdaki örneğe benzer:

```console
* provider.azurerm: version = "~> 0.3"

Terraform has been successfully initialized!
```

Terraform komut dosyası tarafından tamamlanacak eylemleri `terraform plan`' ile önizleyebilirsiniz. Kaynak grubu oluşturmaya hazır olduğunuzda, Terraform planınızı aşağıdaki gibi uygulayın:

```bash
terraform apply
```

Çıktı aşağıdaki örneğe benzer:

```console
An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  + azurerm_resource_group.rg
      id:       <computed>
      location: "westus"
      name:     "testResourceGroup"
      tags.%:   <computed>

azurerm_resource_group.rg: Creating...
  location: "" => "westus"
  name:     "" => "testResourceGroup"
  tags.%:   "" => "<computed>"
azurerm_resource_group.rg: Creation complete after 1s
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure kimlik bilgilerini yapılandırmak ve Azure aboneliğinizde kaynak oluşturmaya başlamak için Terraform'u yükledin veya Bulut Bulut'u kullandınız. Azure'da daha eksiksiz bir Terraform dağıtımı oluşturmak için aşağıdaki makaleye bakın:

> [!div class="nextstepaction"]
> [Terraform ile Azure VM oluşturma](terraform-create-complete-vm.md)