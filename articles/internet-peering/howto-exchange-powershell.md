---
title: PowerShell'i kullanarak Exchange eşlemi oluşturma veya değiştirme
titleSuffix: Azure
description: PowerShell'i kullanarak Exchange eşlemi oluşturma veya değiştirme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 23c905f148da614c7785b61b76abed191206cd90
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81678659"
---
# <a name="create-or-modify-an-exchange-peering-by-using-powershell"></a>PowerShell'i kullanarak Exchange eşlemi oluşturma veya değiştirme

Bu makalede, PowerShell cmdlets ve Kaynak Yöneticisi dağıtım modelini kullanarak bir Microsoft Exchange bakan oluşturma nasıl açıklanmaktadır. Bu makalede, kaynağın durumunu nasıl denetlersiniz, güncelleştirebilirsiniz veya silip yok edebilirsiniz.

İsterseniz, Azure [portalını](howto-exchange-portal.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [ön koşulları](prerequisites.md) ve Exchange bakış [geçiş ini](walkthrough-exchange-all.md) gözden geçirin.
* Microsoft ile Azure kaynaklarına dönüştürülmemiş Exchange eşlemeleri zaten varsa, [PowerShell'i kullanarak azure kaynağına bakan eski bir Exchange'i dönüştür'e](howto-legacy-exchange-powershell.md)bakın.

### <a name="work-with-azure-powershell"></a>Azure PowerShell ile çalışın
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="create-and-provision-an-exchange-peering"></a>Exchange eşleme oluşturma ve sağlama

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Azure hesabınızda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-powershell.md)]

### <a name="get-the-list-of-supported-peering-locations-for-exchange-peering"></a><a name=exchange-location></a>Exchange eşleme için desteklenen eşleme konumlarının listesini alın
[!INCLUDE [exchange-location](./includes/exchange-powershell-create-location.md)]

### <a name="create-an-exchange-peering"></a><a name=create></a>Exchange eşleme oluşturma
[!INCLUDE [exchange-peering](./includes/exchange-powershell-create-connection.md)]

### <a name="get-exchange-peering"></a><a name=get></a>Exchange'in eşlenemelerini alın
[!INCLUDE [peering-exchange-get](./includes/exchange-powershell-get.md)]

## <a name="modify-an-exchange-peering"></a><a name="modify"></a>Exchange eşleme sini değiştirme
[!INCLUDE [peering-exchange-modify](./includes/exchange-powershell-modify.md)]

## <a name="deprovision-an-exchange-peering"></a><a name=delete></a>Exchange'e bakmayı deprovision

[!INCLUDE [peering-exchange-delete](./includes/delete.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [PowerShell'i kullanarak Doğrudan bir eşleme oluşturma veya değiştirme](howto-direct-powershell.md)
* [PowerShell'i kullanarak eski doğrudan bir Azure kaynağına doğrudan bakış dönüştürme](howto-legacy-direct-powershell.md)

## <a name="additional-resources"></a>Ek kaynaklar
Aşağıdaki komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamalarını alabilirsiniz:

```powershell
Get-Help Get-AzPeering -detailed
```

Daha fazla bilgi için [Internet'e bakan SSS'lere](faqs.md)bakın.
