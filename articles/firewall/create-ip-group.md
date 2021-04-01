---
title: Azure Güvenlik duvarında IP grupları oluşturma
description: IP grupları, Azure Güvenlik duvarı kuralları için IP adreslerini Grupve yönetmenize olanak sağlar.
services: firewall
author: vhorne
ms.service: firewall
ms.topic: how-to
ms.date: 06/23/2020
ms.author: victorh
ms.openlocfilehash: c53b35351108717c7b597e052a66e9902be5ec6c
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "93394535"
---
# <a name="create-ip-groups"></a>IP Grupları oluşturma

IP grupları, Azure Güvenlik duvarı kuralları için IP adreslerini Grupve yönetmenize olanak sağlar. Tek bir IP adresi, birden çok IP adresi veya bir veya daha fazla IP adresi aralığı olabilir.

## <a name="create-an-ip-group---azure-portal"></a>IP grubu oluşturma-Azure portal

Azure portal kullanarak bir IP grubu oluşturmak için:

1. Azure portal giriş sayfasında, **kaynak oluştur**' u seçin.
1. Arama kutusuna **IP grupları**' nı girin ve **IP grupları**' nı seçin.
1. **Oluştur**’u seçin.
1. Aboneliğinizi seçin.
1. Bir kaynak grubu seçin veya yeni bir tane oluşturun.
1. IP grubunuz için benzersiz bir ad girin ve ardından bir bölge seçin.
1. **İleri ' yi seçin: IP adresleri**.
1. Bir IP adresi, birden fazla IP adresi veya IP adresi aralığı yazın.

   IP adreslerini girmenin iki yolu vardır:
   - Bunları el ile girebilirsiniz
   - Bunları bir dosyadan içeri aktarabilirsiniz

   Bir dosyadan içeri aktarmak için **bir dosyadan Içeri aktar**' ı seçin. Dosyanızı kutuya sürükleyebilir veya **dosyalara gözatabiliriz**' ı seçebilirsiniz. Gerekirse, karşıya yüklenen IP adreslerinizi gözden geçirebilir ve düzenleyebilirsiniz.

   Bir IP adresi yazdığınızda, Portal, çakışan, yinelenen ve biçimlendirme sorunlarını kontrol etmek için onu doğrular.

1. İşiniz bittiğinde, **gözden geçir + oluştur**' u seçin.
1. **Oluştur**’u seçin.

## <a name="create-an-ip-group---azure-powershell"></a>IP grubu oluşturma-Azure PowerShell

Bu örnek, Azure PowerShell kullanarak bir adres öneki ve IP adresi olan bir IP grubu oluşturur:

```azurepowershell
$ipGroup = @{
    Name              = 'ipGroup'
    ResourceGroupName = 'ipGroupRG'
    Location          = 'West US'
    IpAddress         = @('10.0.0.0/24', '192.168.1.10') 
}

New-AzIpGroup @ipGroup
```

## <a name="create-an-ip-group---azure-cli"></a>IP grubu oluşturma-Azure CLı

Bu örnek, Azure CLı kullanarak adres ön eki ve IP adresi içeren bir IP grubu oluşturur:

```azurecli-interactive
az network ip-group create \
    --name ipGroup \ 
    --resource-group ipGroupRG \
    --location westus \
    --ip-addresses '10.0.0.0/24' '192.168.1.10'
```

## <a name="next-steps"></a>Sonraki adımlar

- [IP grupları hakkında daha fazla bilgi](ip-groups.md)
