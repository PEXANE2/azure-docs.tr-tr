---
title: PowerShell kullanarak eski bir Exchange eşlemesini Azure kaynağına dönüştürme
titleSuffix: Azure
description: PowerShell kullanarak eski bir Exchange eşlemesini Azure kaynağına dönüştürme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: aa11f329cf0a0cb27d58b940b42731a2ec41c272
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75775400"
---
# <a name="convert-a-legacy-exchange-peering-to-azure-resource-using-powershell"></a>PowerShell kullanarak eski bir Exchange eşlemesini Azure kaynağına dönüştürme

Bu makalede, PowerShell cmdlet 'lerini kullanarak mevcut bir eski Exchange eşlemesinin Azure kaynağına nasıl dönüştürüleceği açıklanır.

İsterseniz, [portalı](howto-legacy-exchange-portal.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [önkoşulları](prerequisites.md) ve [Exchange eşleme](walkthrough-exchange-all.md) kılavuzunu gözden geçirin.

### <a name="working-with-azure-powershell"></a>Azure PowerShell ile çalışma
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="convert-a-legacy-exchange-peering-to-azure-resource"></a>Eski bir Exchange eşlemesini Azure kaynağına Dönüştür

### <a name="sign-in-to-your-azure-account-and-select-your-subscription"></a>Azure hesabınızda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-powershell.md)]

### <a name= get></a>Dönüştürme için eski Exchange eşlemesini al
Aşağıda, Seattle eşleme konumunda eski Exchange eşlemesini almak için örnek verilmiştir:

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

### <a name="convert-legacy-peering"></a>Eski eşlemeyi Dönüştür
Aşağıdaki komut, eski Exchange eşlemesini Azure kaynağına dönüştürmek için kullanılabilir:

```powershell
$legacyPeering[0] | New-AzPeering `
    -Name "SeattleExchangePeering" `
    -ResourceGroupName "PeeringResourceGroup"

```

&nbsp;
> [!IMPORTANT] 
> Eski eşlemeyi Azure kaynağına dönüştürürken değişiklikler desteklenmediğini unutmayın &nbsp;

Aşağıda, uçtan uca sağlama başarıyla tamamlandığında örnek bir yanıt verilmiştir:

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
Aşağıdaki komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamaları alabilirsiniz:

```powershell
Get-Help Get-AzPeering -detailed
```
Daha fazla bilgi için [Internet eşlemesi SSS](faqs.md) ' yi ziyaret edin

## <a name="next-steps"></a>Sonraki adımlar

* [PowerShell kullanarak bir Exchange eşlemesi oluşturma veya değiştirme](howto-exchange-powershell.md)
