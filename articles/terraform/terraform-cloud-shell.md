---
title: Öğretici - Terraform için Azure Bulut Kabuğunu Yapılandır
description: Bu eğitimde, kimlik doğrulama ve şablon yapılandırmasını basitleştirmek için Azure Cloud Shell ile Terraform'u kullanırsınız.
keywords: masmavi devops terraform bulut kabuk
ms.topic: tutorial
ms.date: 03/09/2020
ms.openlocfilehash: 3a9db1143ba07b549a271d53d610e0a4853467c6
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78945344"
---
# <a name="tutorial-configure-azure-cloud-shell-for-terraform"></a>Öğretici: Terraform için Azure Bulut Kabuğunu Yapılandırın

Terraform, macOS, Windows veya Linux'taki Bir Bash komut satırından iyi çalışır. Terraform yapılandırmalarınızı [Azure Cloud Shell'in](/azure/cloud-shell/overview) Bash deneyiminde çalıştırmak için bazı benzersiz avantajlar vardır. Bu öğretici, Cloud Shell'i kullanarak Azure'a dağıtılabilen Terraform komut dosyalarının nasıl yazılabildiğini gösterir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Otomatik kimlik bilgisi yapılandırma

Terraform, Cloud Shell'de yüklüdür ve kullanıma hazırdır. Terraform komut dosyaları, ek yapılandırma olmadan altyapıyı yönetmek için Cloud Shell'e giriş yaptığında Azure ile kimlik doğrulaması sağlar. Otomatik kimlik doğrulama iki el ile işlemi atlar:
- Active Directory hizmet ilkesi oluşturma
- Azure Terraform sağlayıcı değişkenlerini yapılandırma


## <a name="use-modules-and-providers"></a>Modülleri ve sağlayıcıları kullanma

Azure Terraform modülleri, Azure kaynaklarına erişmek ve değiştirmek için kimlik bilgileri gerektirir. Cloud Shell'de Terraform modüllerini kullanmak için aşağıdaki kodu ekleyin:


```hcl
# Configure the Microsoft Azure Provider
provider "azurerm" {
    # The "feature" block is required for AzureRM provider 2.x. 
    # If you are using version 1.x, the "features" block is not allowed.
    version = "~>2.0"
    features {}
}
```

Bulut Kabuğu, `azurerm` `terraform` CLI komutlarından herhangi birini kullandığınızda çevre değişkenleri aracılığıyla sağlayıcı için gerekli değerleri geçirir.

## <a name="other-cloud-shell-developer-tools"></a>Diğer Cloud Shell geliştirici araçları

Azure Depolama dosyaları ve kabuk durumları Cloud Shell oturumları arasında kalıcı olur. Dosyaları yerel bilgisayarınızdan Bulut Kabuğu'na kopyalamak ve yüklemek için [Azure Depolama Gezgini'ni](/azure/vs-azure-tools-storage-manage-with-storage-explorer) kullanın.

Azure CLI, Cloud Shell'de kullanılabilir ve yapılandırmaları test etmek `terraform apply` ve `terraform destroy` çalışmanızı veya bitişinizden sonra kontrol etmek için harika bir araçtır.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Modül Kayıt Defterini kullanarak küçük bir VM kümesi oluşturma](terraform-create-vm-cluster-module.md)