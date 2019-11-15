---
title: Öğretici-Teroyform için Azure Cloud Shell yapılandırma
description: Kimlik doğrulaması ve şablon yapılandırma adımlarını basitleştirmek için Terraform ile Azure Cloud Shell'i birlikte kullanın.
ms.service: terraform
author: tomarchermsft
ms.author: tarcher
ms.topic: tutorial
ms.date: 10/26/2019
ms.openlocfilehash: 1259d5004bd547e33f65571333b6d0721d1253c0
ms.sourcegitcommit: a22cb7e641c6187315f0c6de9eb3734895d31b9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/14/2019
ms.locfileid: "74078719"
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
}
```

Cloud Shell, `terraform` CLı komutlarından herhangi birini kullandığınızda `azurerm` sağlayıcısı için gerekli değerleri ortam değişkenleri aracılığıyla geçirir.

## <a name="other-cloud-shell-developer-tools"></a>Diğer Cloud Shell geliştirici araçları

Azure Depolama dosyaları ve kabuk durumları Cloud Shell oturumları arasında kalıcı olur. Dosyaları yerel bilgisayarınızdan Cloud Shell kopyalamak ve karşıya yüklemek için [Azure Depolama Gezgini](/azure/vs-azure-tools-storage-manage-with-storage-explorer) kullanın.

Azure CLı Cloud Shell ' de kullanılabilir ve `terraform apply` veya `terraform destroy` bittikten sonra, yapılandırmaların test edilmesi ve işinizi denetlemesi için harika bir araçtır.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Modül kayıt defteri kullanarak küçük bir VM kümesi oluşturma](terraform-create-vm-cluster-module.md)
