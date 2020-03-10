---
title: Öğretici-Teroyform için Azure Cloud Shell yapılandırma
description: Bu öğreticide, kimlik doğrulama ve şablon yapılandırmasını basitleştirmek için Azure Cloud Shell ile Terrayform kullanırsınız.
keywords: Azure DevOps terrayform Cloud Shell
ms.topic: tutorial
ms.date: 03/09/2020
ms.openlocfilehash: 3a9db1143ba07b549a271d53d610e0a4853467c6
ms.sourcegitcommit: 8f4d54218f9b3dccc2a701ffcacf608bbcd393a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/09/2020
ms.locfileid: "78945344"
---
# <a name="tutorial-configure-azure-cloud-shell-for-terraform"></a>Öğretici: Terrayform için Azure Cloud Shell yapılandırma

Terkform, macOS, Windows veya Linux 'daki Bash komut satırından iyi çalışmaktadır. [Azure Cloud Shell](/azure/cloud-shell/overview) Bash deneyiminizdeki Teraform yapılandırmalarının çalıştırılması bazı benzersiz avantajlara sahiptir. Bu öğreticide, Cloud Shell kullanarak Azure 'a dağıtım yapan Terrayform betikleri yazma gösterilmektedir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Otomatik kimlik bilgisi yapılandırma

Terraform, Cloud Shell'de yüklüdür ve kullanıma hazırdır. Terrayform betikleri, ek bir yapılandırma olmadan altyapıyı yönetmek için Cloud Shell oturum açtığında Azure ile kimlik doğrular. Otomatik kimlik doğrulaması iki el ile işlemi atlar:
- Active Directory hizmet sorumlusu oluşturma
- Azure Terrayform sağlayıcısı değişkenlerini yapılandırma


## <a name="use-modules-and-providers"></a>Modülleri ve sağlayıcıları kullanma

Azure Terrayform modülleri, Azure kaynaklarına erişmek ve bunları değiştirmek için kimlik bilgileri gerektirir. Cloud Shell ' de Terrayform modüllerini kullanmak için aşağıdaki kodu ekleyin:


```hcl
# Configure the Microsoft Azure Provider
provider "azurerm" {
    # The "feature" block is required for AzureRM provider 2.x. 
    # If you are using version 1.x, the "features" block is not allowed.
    version = "~>2.0"
    features {}
}
```

Cloud Shell, `terraform` CLı komutlarından herhangi birini kullandığınızda `azurerm` sağlayıcısı için gerekli değerleri ortam değişkenleri aracılığıyla geçirir.

## <a name="other-cloud-shell-developer-tools"></a>Diğer Cloud Shell geliştirici araçları

Azure Depolama dosyaları ve kabuk durumları Cloud Shell oturumları arasında kalıcı olur. Dosyaları yerel bilgisayarınızdan Cloud Shell kopyalamak ve karşıya yüklemek için [Azure Depolama Gezgini](/azure/vs-azure-tools-storage-manage-with-storage-explorer) kullanın.

Azure CLı Cloud Shell ' de kullanılabilir ve `terraform apply` veya `terraform destroy` bittikten sonra, yapılandırmaların test edilmesi ve işinizi denetlemesi için harika bir araçtır.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Modül kayıt defteri kullanarak küçük bir VM kümesi oluşturma](terraform-create-vm-cluster-module.md)