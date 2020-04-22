---
title: PowerShell'i kullanarak Doğrudan bir eşleme oluşturma veya değiştirme
titleSuffix: Azure
description: PowerShell'i kullanarak Doğrudan bir eşleme oluşturma veya değiştirme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 7639499aaef8d479c2552849b2124e709c46fd36
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81680758"
---
# <a name="create-or-modify-a-direct-peering-by-using-powershell"></a>PowerShell'i kullanarak Doğrudan bir eşleme oluşturma veya değiştirme

Bu makalede, PowerShell cmdlets ve Azure Kaynak Yöneticisi dağıtım modelini kullanarak Microsoft Direct bakışlarının nasıl oluşturulutamamı açıklanmaktadır. Bu makalede, kaynağın durumunu nasıl denetlersiniz, güncelleştirebilirsiniz veya silip yok edebilirsiniz.

İsterseniz, Azure [portalını](howto-direct-portal.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [ön koşulları](prerequisites.md) ve Doğrudan bakan [gözden geçirin.](walkthrough-direct-all.md)
* Microsoft ile Azure kaynaklarına dönüştürülmemiş doğrudan bakış bağlantılarınız varsa, [powershell kullanarak bir Azure kaynağına](howto-legacy-direct-powershell.md)doğrudan bakış dönüştürme 'ye bakın.

### <a name="work-with-azure-powershell"></a>Azure PowerShell ile çalışın
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="create-and-provision-a-direct-peering"></a>Doğrudan bir bakış oluşturma ve sağlama

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Azure hesabınızda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-powershell.md)]

### <a name="get-the-list-of-supported-peering-locations-for-direct-peering"></a><a name=direct-location></a>Doğrudan eşleme için desteklenen eşleme konumlarının listesini alın
[!INCLUDE [direct-location](./includes/direct-powershell-create-location.md)]

### <a name="create-a-direct-peering"></a><a name=create></a>Doğrudan eşleme oluşturma
[!INCLUDE [direct-peering](./includes/direct-powershell-create-connection.md)]

### <a name="verify-direct-peering"></a><a name=get></a>Doğrudan bakmayı doğrula
[!INCLUDE [peering-direct-get](./includes/direct-powershell-get.md)]

## <a name="modify-a-direct-peering"></a><a name="modify"></a>Doğrudan eşleme değiştirme
[!INCLUDE [peering-direct-modify](./includes/direct-powershell-modify.md)]

## <a name="deprovision-a-direct-peering"></a><a name="delete"></a>Doğrudan bir bakış hükmünü deprovision
[!INCLUDE [peering-direct-delete](./includes/delete.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [PowerShell'i kullanarak Exchange eşlemi oluşturma veya değiştirme](howto-exchange-powershell.md)
* [PowerShell'i kullanarak azure kaynağına bakan eski bir Exchange'i dönüştürme](howto-legacy-exchange-powershell.md)

## <a name="additional-resources"></a>Ek kaynaklar
Aşağıdaki komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamalarını alabilirsiniz:

```powershell
Get-Help Get-AzPeering -detailed
```

Daha fazla bilgi için [Internet'e bakan SSS'lere](faqs.md)bakın.
