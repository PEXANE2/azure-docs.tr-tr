---
title: PowerShell kullanarak doğrudan eşleme üzerinde eşleme hizmetini etkinleştirme
titleSuffix: Azure
description: PowerShell kullanarak doğrudan eşleme üzerinde eşleme hizmetini etkinleştirme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: ac843fa5440f7ba085d33cd897bcd4a1722f77ac
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75774191"
---
# <a name="enable-peering-service-on-a-direct-peering-using-powershell"></a>PowerShell kullanarak doğrudan eşleme üzerinde eşleme hizmetini etkinleştirme

Bu makalede, PowerShell cmdlet 'lerini ve Kaynak Yöneticisi dağıtım modelini kullanarak doğrudan eşleme üzerinde [eşleme hizmeti](overview-peering-service.md) 'nin nasıl etkinleştirileceği açıklanır.

İsterseniz, [portalı](howto-peering-service-portal.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [önkoşulları](prerequisites.md) gözden geçirin.
* Aboneliğinizde eşleme hizmetini etkinleştirmek istediğiniz bir doğrudan eşleme seçin. Bir tane yoksa, eski bir doğrudan eşlemeyi dönüştürün veya yeni bir doğrudan eşleme oluşturun.
    * Eski bir doğrudan eşlemeyi dönüştürmek için [PowerShell kullanarak eski bir doğrudan eşlemeyi Azure kaynağına dönüştürme](howto-legacy-direct-powershell.md)bölümündeki yönergeleri izleyin.
    * Yeni bir doğrudan eşleme oluşturmak için [PowerShell kullanarak doğrudan eşleme oluşturma veya değiştirme](howto-direct-powershell.md)bölümündeki yönergeleri izleyin.

### <a name="working-with-azure-powershell"></a>Azure PowerShell ile çalışma
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="enable-peering-service-on-a-direct-peering"></a>Doğrudan eşleme üzerinde eşleme hizmetini etkinleştir

### <a name= get></a>Doğrudan eşlemeyi görüntüle
[!INCLUDE [peering-direct-get](./includes/direct-powershell-get.md)]

### <a name= get></a>Eşleme hizmeti için doğrudan eşlemeyi etkinleştir

Önceki adımda doğrudan eşleme aldıktan sonra, bunu eşleme hizmeti için etkinleştirin.
[!INCLUDE [peering-direct-modify](./includes/peering-service-direct-powershell.md)]

## <a name="modify-a-direct-peering-connection"></a>Doğrudan eşleme bağlantısını değiştirme

Bağlantı ayarlarını değiştirmeniz gerekiyorsa, [PowerShell kullanarak doğrudan eşleme oluşturma veya değiştirme](howto-direct-powershell.md) konusunun **doğrudan eşlemeyi değiştirme** bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [PowerShell kullanarak Exchange eşlemesi oluşturma veya değiştirme](howto-exchange-powershell.md)
* [PowerShell kullanarak eski bir Exchange eşlemesini Azure kaynağına dönüştürme](howto-legacy-exchange-powershell.md)

## <a name="additional-resources"></a>Ek kaynaklar
Aşağıdaki komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamaları alabilirsiniz:

```powershell
Get-Help Get-AzPeering -detailed
```

Sık sorulan sorular için bkz. [eşleme HIZMETI SSS](service-faqs.md).