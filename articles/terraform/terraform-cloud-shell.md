---
title: Öğretici-Teroyform için Azure Cloud Shell yapılandırma
description: Kimlik doğrulaması ve şablon yapılandırma adımlarını basitleştirmek için Terraform ile Azure Cloud Shell'i birlikte kullanın.
ms.service: terraform
author: tomarchermsft
ms.author: tarcher
ms.topic: tutorial
ms.date: 10/26/2019
ms.openlocfilehash: 693ed462fb1ba3dfed079e8ae97152732c771253
ms.sourcegitcommit: b1c94635078a53eb558d0eb276a5faca1020f835
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/27/2019
ms.locfileid: "72969589"
---
# <a name="tutorial-configure-azure-cloud-shell-for-terraform"></a>Öğretici: Terrayform için Azure Cloud Shell yapılandırma

Terkform, macOS, Windows veya Linux 'daki Bash komut satırından iyi çalışmaktadır. [Azure Cloud Shell](/azure/cloud-shell/overview) Bash deneyiminizdeki Teraform yapılandırmalarının çalıştırılması bazı benzersiz avantajlara sahiptir. Bu öğreticide, Cloud Shell kullanarak Azure 'a dağıtan Teraform betiklerinin nasıl yazılacağı gösterilmektedir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="automatic-credential-configuration"></a>Otomatik kimlik bilgisi yapılandırma

Terraform, Cloud Shell'de yüklüdür ve kullanıma hazırdır. Terraform betikleri Cloud Shell oturumu açıldığında otomatik olarak Azure kimlik doğrulamasından geçer ve ek yapılandırmaya gerek kalmadan altyapının yönetilmesini sağlar. Otomatik kimlik doğrulaması iki el ile işlemi atlar:
- Active Directory hizmet sorumlusu oluşturma.
- Azure Terrayform sağlayıcısı değişkenlerini yapılandırma.


## <a name="using-modules-and-providers"></a>Modülleri ve Sağlayıcıları kullanma

Azure Terrayform modülleri, Azure kaynaklarına erişmek ve bunları değiştirmek için kimlik bilgileri gerektirir. Cloud Shell ' de Terrayform modüllerini kullanmak için aşağıdaki kodu ekleyin:


```hcl
# Configure the Microsoft Azure Provider
provider "azurerm" {
}
```

Cloud Shell, `terraform` CLI komutlarından biri kullanıldığında `azurerm` sağlayıcısı için gerekli değerleri ortam değişkenleriyle iletir.

## <a name="other-cloud-shell-developer-tools"></a>Diğer Cloud Shell geliştirici araçları

Azure Depolama dosyaları ve kabuk durumları Cloud Shell oturumları arasında kalıcı olur. [Azure Depolama Gezgini](/azure/vs-azure-tools-storage-manage-with-storage-explorer)'ni kullanarak Cloud Shell'den yerel bilgisayarınızla dosya kopyalama ve yükleme işlemleri gerçekleştirebilirsiniz.

Cloud Shell'de bulunan Azure CLI, yapılandırma testi ve `terraform apply` veya `terraform destroy` işlemlerinin ardından kontrol gerçekleştirmek için kullanışlı bir araçtır.


## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Modül kayıt defteri kullanarak küçük bir VM kümesi oluşturma](terraform-create-vm-cluster-module.md)