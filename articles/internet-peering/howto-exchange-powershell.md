---
title: PowerShell'i kullanarak Exchange eşleme oluşturma veya değiştirme
titleSuffix: Azure
description: PowerShell'i kullanarak Exchange eşleme oluşturma veya değiştirme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 4fd7d345e5efbe6e4e86e5bb410e2df4dd917047
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75774412"
---
# <a name="create-or-modify-an-exchange-peering-using-powershell"></a>PowerShell'i kullanarak Exchange eşleme oluşturma veya değiştirme

Bu makalede, PowerShell cmdlets ve Kaynak Yöneticisi dağıtım modelini kullanarak bir Microsoft Exchange bakan oluşturma nasıl açıklanmaktadır. Bu makalede, kaynağın durumunu nasıl denetlersiniz, güncelleştirebilirsiniz veya silip yok edebilirsiniz.

İsterseniz, [portalı](howto-exchange-portal.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [Önkoşullar](prerequisites.md) ve [Exchange bakış gözden geçirin.](walkthrough-exchange-all.md)
* Azure kaynaklarına dönüştürülmemiş microsoft ile exchange eşlemeniz varsa, [PowerShell'i kullanarak Azure kaynağına bakan eski bir Exchange'e](howto-legacy-exchange-powershell.md) bakın

### <a name="working-with-azure-powershell"></a>Azure PowerShell ile çalışma
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

* [PowerShell kullanarak Doğrudan bir eşleme oluşturma veya değiştirme](howto-direct-powershell.md)
* [PowerShell'i kullanarak eski bir Doğrudan bakışı Azure kaynağına dönüştürün](howto-legacy-direct-powershell.md)

## <a name="additional-resources"></a>Ek kaynaklar
Aşağıdaki komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamalarını alabilirsiniz:

```powershell
Get-Help Get-AzPeering -detailed
```

Daha fazla bilgi için [Internet'e bakan SSS'leri](faqs.md) ziyaret edin
