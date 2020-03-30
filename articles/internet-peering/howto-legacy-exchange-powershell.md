---
title: PowerShell'i kullanarak Azure kaynağına bakan eski bir Exchange'i dönüştürme
titleSuffix: Azure
description: PowerShell'i kullanarak Azure kaynağına bakan eski bir Exchange'i dönüştürme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: aa11f329cf0a0cb27d58b940b42731a2ec41c272
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75775400"
---
# <a name="convert-a-legacy-exchange-peering-to-azure-resource-using-powershell"></a>PowerShell'i kullanarak Azure kaynağına bakan eski bir Exchange'i dönüştürme

Bu makalede, PowerShell cmdlets kullanarak Azure kaynağına bakan varolan bir eski Exchange'in nasıl dönüştürülür.

İsterseniz, [portalı](howto-legacy-exchange-portal.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [Önkoşullar](prerequisites.md) ve [Exchange bakış gözden geçirin.](walkthrough-exchange-all.md)

### <a name="working-with-azure-powershell"></a>Azure PowerShell ile çalışma
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="convert-a-legacy-exchange-peering-to-azure-resource"></a>Eski bir Exchange eşlemeyi Azure kaynağına dönüştürme

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Azure hesabınızda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-powershell.md)]

### <a name="get-legacy-exchange-peering-for-conversion"></a><a name= get></a>Dönüşüm için eski Exchange'i ele alma
Aşağıda seattle bakan konumda eski Exchange bakan almak için örnek:

```powershell
$legacyPeering = Get-AzLegacyPeering -Kind Exchange -PeeringLocation "Seattle"
$legacyPeering
```

Yanıt aşağıdaki örneğe benzer:
```powershell
    Kind                     : Exchange
    PeeringLocation          : Seattle
    PeerAsn                  : 65000
    Connection               : ------------------------
    PeerSessionIPv4Address   : 10.21.31.100
    MicrosoftIPv4Address     : 10.21.31.50
    SessionStateV4           : Established
    MaxPrefixesAdvertisedV4  : 20000
    PeerSessionIPv6Address   : fe01::3e:100
    MicrosoftIPv6Address     : fe01::3e:50
    SessionStateV6           : Established
    MaxPrefixesAdvertisedV6  : 2000
    ConnectionState          : Active
```

### <a name="convert-legacy-peering"></a>Eski eşlemi dönüştürme
Aşağıdaki komut, eski Exchange'i Azure kaynağına dönüştürmek için kullanılabilir:

```powershell
$legacyPeering[0] | New-AzPeering `
    -Name "SeattleExchangePeering" `
    -ResourceGroupName "PeeringResourceGroup"

```

&nbsp;
> [!IMPORTANT] 
> Eski bakışları azure kaynağına dönüştürürken, değişikliklerin desteklenmediğini unutmayın&nbsp;

Aşağıda, uçuça sağlama başarıyla tamamlandığında örnek bir yanıt verilmiştir:

```powershell
    Name                     : SeattleExchangePeering
    Kind                     : Exchange
    Sku                      : Basic_Exchange_Free
    PeeringLocation          : Seattle
    PeerAsn                  : 65000
    Connection               : ------------------------
    PeerSessionIPv4Address   : 10.21.31.100
    MicrosoftIPv4Address     : 10.21.31.50
    SessionStateV4           : Established
    MaxPrefixesAdvertisedV4  : 20000
    PeerSessionIPv6Address   : fe01::3e:100
    MicrosoftIPv6Address     : fe01::3e:50
    SessionStateV6           : Established
    MaxPrefixesAdvertisedV6  : 2000
    ConnectionState          : Active
```
## <a name="additional-resources"></a>Ek kaynaklar
Aşağıdaki komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamalarını alabilirsiniz:

```powershell
Get-Help Get-AzPeering -detailed
```
Daha fazla bilgi için [Internet'e bakan SSS'leri](faqs.md) ziyaret edin

## <a name="next-steps"></a>Sonraki adımlar

* [PowerShell'i kullanarak Exchange eşleme oluşturma veya değiştirme](howto-exchange-powershell.md)
