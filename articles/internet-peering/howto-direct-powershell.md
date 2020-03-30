---
title: PowerShell kullanarak Doğrudan bir eşleme oluşturma veya değiştirme
titleSuffix: Azure
description: PowerShell kullanarak Doğrudan bir eşleme oluşturma veya değiştirme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 69031550bdab1535213c78f81426fa76e8ea62ad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75774243"
---
# <a name="create-or-modify-a-direct-peering-using-powershell"></a>PowerShell kullanarak Doğrudan bir eşleme oluşturma veya değiştirme

Bu makalede, PowerShell cmdlets ve Kaynak Yöneticisi dağıtım modelini kullanarak Microsoft Direct bakışoluşturma nasıl açıklanmaktadır. Bu makalede, kaynağın durumunu nasıl denetlersiniz, güncelleştirebilirsiniz veya silip yok edebilirsiniz.

İsterseniz, [portalı](howto-direct-portal.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [Önkoşulları](prerequisites.md) ve [Doğrudan bakan gözden geçirin.](walkthrough-direct-all.md)
* Azure kaynaklarına dönüştürülmemiş olan Microsoft ile doğrudan bakışınız varsa, [PowerShell'i kullanarak eski bir Doğrudan Azure kaynağına dönüştür'e](howto-legacy-direct-powershell.md) bakın

### <a name="working-with-azure-powershell"></a>Azure PowerShell ile çalışma
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

* [PowerShell'i kullanarak Exchange eşleme sini oluşturun veya değiştirin.](howto-exchange-powershell.md)
* [PowerShell'i kullanarak Azure kaynağına bakan eski bir Exchange'i dönüştürün.](howto-legacy-exchange-powershell.md)

## <a name="additional-resources"></a>Ek kaynaklar
Aşağıdaki komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamalarını alabilirsiniz:

```powershell
Get-Help Get-AzPeering -detailed
```

Daha fazla bilgi için [Internet'e bakan SSS'leri](faqs.md) ziyaret edin
