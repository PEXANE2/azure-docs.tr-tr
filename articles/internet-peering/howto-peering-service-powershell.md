---
title: PowerShell'i kullanarak Azure Peering Hizmetini Doğrudan bakışlamada etkinleştirme
titleSuffix: Azure
description: PowerShell'i kullanarak Azure Peering Hizmetini Doğrudan bakışlamada etkinleştirme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: c029b822d00ce545e3623a6212421a55b2d1971f
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81686990"
---
# <a name="enable-azure-peering-service-on-a-direct-peering-by-using-powershell"></a>PowerShell'i kullanarak Azure Peering Hizmetini Doğrudan bakışlamada etkinleştirme

Bu makalede, PowerShell cmdlets ve Azure Kaynak Yöneticisi dağıtım modelini kullanarak Doğrudan bir bakışüzerinde Azure [Peering Hizmeti](overview-peering-service.md) etkinleştirmek için nasıl açıklanmaktadır.

İsterseniz, Azure [portalını](howto-peering-service-portal.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [ön koşulları](prerequisites.md) gözden geçirin.
* Aboneliğinizde, Peering Service'i etkinleştirmek istediğiniz doğrudan bir bakış seçin. Eğer yoksa, ya eski doğrudan bakan dönüştürmek veya yeni bir Doğrudan bakan oluşturmak:
    * Eski bir Doğrudan bakışlandırmayı dönüştürmek [için, PowerShell'i kullanarak bir Azure kaynağına doğrudan bakan eskiyi dönüştür'teki](howto-legacy-direct-powershell.md)yönergeleri izleyin.
    * Yeni bir Doğrudan eşleme oluşturmak için, PowerShell'i kullanarak Doğrudan bakış oluşturma yönergelerini izleyin [veya Doğrudan bir bakış oluşturma'yı değiştirin.](howto-direct-powershell.md)

### <a name="work-with-azure-powershell"></a>Azure PowerShell ile çalışın
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="enable-peering-service-on-a-direct-peering"></a>Doğrudan eşleme üzerinde Eşleme Hizmeti’ni etkinleştir

### <a name="view-direct-peering"></a><a name= get></a>Doğrudan bakış görüntüleme
[!INCLUDE [peering-direct-get](./includes/direct-powershell-get.md)]

### <a name="enable-the-direct-peering-for-peering-service"></a><a name= get></a>Peering Service için Doğrudan eşlemesini etkinleştirme

Önceki adımda Doğrudan bakma yı aldıktan sonra, Bakma Hizmeti için etkinleştirin.
[!INCLUDE [peering-direct-modify](./includes/peering-service-direct-powershell.md)]

## <a name="modify-a-direct-peering-connection"></a>Doğrudan eşleme bağlantısını değiştirme

Bağlantı ayarlarını değiştirmeniz gerekiyorsa, Create'deki "Doğrudan eşleme" bölümüne bakın [veya PowerShell'i kullanarak Doğrudan bir eşleme'yi değiştirin.](howto-direct-powershell.md)

## <a name="next-steps"></a>Sonraki adımlar

* [PowerShell'i kullanarak Exchange eşlemi oluşturma veya değiştirme](howto-exchange-powershell.md)
* [PowerShell'i kullanarak azure kaynağına bakan eski bir Exchange'i dönüştürme](howto-legacy-exchange-powershell.md)

## <a name="additional-resources"></a>Ek kaynaklar
Aşağıdaki komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamalarını alabilirsiniz:

```powershell
Get-Help Get-AzPeering -detailed
```

Sık sorulan sorular [için, Bakma Hizmeti SSS'si'ne](service-faqs.md)bakın.