---
title: PowerShell kullanarak eski bir Exchange eşlemesini Azure kaynağına dönüştürme
titleSuffix: Azure
description: PowerShell kullanarak eski bir Exchange eşlemesini Azure kaynağına dönüştürme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: how-to
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 2abd05243f30a32d8fe49f12bf4ee2ad09855002
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84700000"
---
# <a name="convert-a-legacy-exchange-peering-to-an-azure-resource-by-using-powershell"></a>PowerShell kullanarak eski bir Exchange eşlemesini Azure kaynağına dönüştürme

Bu makalede, PowerShell cmdlet 'lerini kullanarak mevcut bir eski Exchange eşlemesinin bir Azure kaynağına nasıl dönüştürüleceği açıklanır.

İsterseniz, Azure [portalını](howto-legacy-exchange-portal.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [önkoşulları](prerequisites.md) ve [Exchange eşleme](walkthrough-exchange-all.md) Kılavuzu ' nu gözden geçirin.

### <a name="work-with-azure-powershell"></a>Azure PowerShell çalışın
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="convert-a-legacy-exchange-peering-to-an-azure-resource"></a>Eski bir Exchange eşlemesini Azure kaynağına dönüştürme

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Azure hesabınızda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-powershell.md)]

### <a name="get-legacy-exchange-peering-for-conversion"></a><a name= get></a>Dönüştürme için eski Exchange eşlemesini al
Bu örnek, Seattle eşleme konumunda eski Exchange eşlemesinin nasıl alınacağını gösterir:

```powershell
$legacyPeering = Get-AzLegacyPeering -Kind Exchange -PeeringLocation "Seattle"
$legacyPeering
```

Yanıt aşağıdaki örneğe benzer şekilde görünür:
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

### <a name="convert-legacy-peering"></a>Eski eşlemeyi Dönüştür
Bu komut, eski Exchange eşlemesini bir Azure kaynağına dönüştürmek için kullanılabilir:

```powershell
$legacyPeering[0] | New-AzPeering `
    -Name "SeattleExchangePeering" `
    -ResourceGroupName "PeeringResourceGroup"

```

&nbsp;
> [!IMPORTANT] 
> Eski eşlemeyi bir Azure kaynağına dönüştürdüğünüzde, değişiklikler desteklenmez.
&nbsp;

Bu örnek yanıt, uçtan uca sağlama başarıyla tamamlandığında gösterilmektedir:

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
Daha fazla bilgi için bkz. [Internet eşlemesi SSS](faqs.md).

## <a name="next-steps"></a>Sonraki adımlar

* [PowerShell kullanarak bir Exchange eşlemesi oluşturma veya değiştirme](howto-exchange-powershell.md)
