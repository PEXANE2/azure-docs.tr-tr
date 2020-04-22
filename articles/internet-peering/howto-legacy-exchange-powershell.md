---
title: PowerShell'i kullanarak azure kaynağına bakan eski bir Exchange'i dönüştürme
titleSuffix: Azure
description: PowerShell'i kullanarak azure kaynağına bakan eski bir Exchange'i dönüştürme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: eedf87548d62e05d4940911ed3dcd821077acb27
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81686786"
---
# <a name="convert-a-legacy-exchange-peering-to-an-azure-resource-by-using-powershell"></a>PowerShell'i kullanarak azure kaynağına bakan eski bir Exchange'i dönüştürme

Bu makalede, PowerShell cmdlets kullanarak bir Azure kaynağına bakan varolan bir eski Exchange'in nasıl dönüştürülür.

İsterseniz, Azure [portalını](howto-legacy-exchange-portal.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [ön koşulları](prerequisites.md) ve Exchange bakış [geçiş ini](walkthrough-exchange-all.md) gözden geçirin.

### <a name="work-with-azure-powershell"></a>Azure PowerShell ile çalışın
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="convert-a-legacy-exchange-peering-to-an-azure-resource"></a>Azure kaynağına bakan eski bir Exchange'i dönüştürme

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Azure hesabınızda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-powershell.md)]

### <a name="get-legacy-exchange-peering-for-conversion"></a><a name= get></a>Dönüşüm için eski Exchange eşlemealın
Bu örnek, Seattle'daki bakan konumuna bakan eski Exchange'in nasıl alındığını gösterir:

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
Bu komut, eski Exchange'i azure kaynağına dönüştürmek için kullanılabilir:

```powershell
$legacyPeering[0] | New-AzPeering `
    -Name "SeattleExchangePeering" `
    -ResourceGroupName "PeeringResourceGroup"

```

&nbsp;
> [!IMPORTANT] 
> Eski bir gözatlandırmayı bir Azure kaynağına dönüştürdüğünüzde, değişiklikler desteklenmez.
&nbsp;

Bu örnek yanıt, uçuça sağlamanın başarıyla ne zaman tamamlandığını gösterir:

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
Daha fazla bilgi için [Internet'e bakan SSS'lere](faqs.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [PowerShell'i kullanarak Exchange eşlemi oluşturma veya değiştirme](howto-exchange-powershell.md)
