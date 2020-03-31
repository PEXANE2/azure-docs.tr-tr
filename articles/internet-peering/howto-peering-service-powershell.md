---
title: PowerShell'i kullanarak Doğrudan bir eşlemede Peering Service'i etkinleştirin
titleSuffix: Azure
description: PowerShell'i kullanarak Doğrudan bir eşlemede Peering Service'i etkinleştirin
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: ac843fa5440f7ba085d33cd897bcd4a1722f77ac
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75774191"
---
# <a name="enable-peering-service-on-a-direct-peering-using-powershell"></a>PowerShell'i kullanarak Doğrudan bir eşlemede Peering Service'i etkinleştirin

Bu makalede, PowerShell cmdlets ve Kaynak Yöneticisi dağıtım modelini kullanarak Doğrudan bir bakışüzerinde [Peering Service](overview-peering-service.md) etkinleştirmek için nasıl açıklanır.

İsterseniz, [portalı](howto-peering-service-portal.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [ön koşulları](prerequisites.md) gözden geçirin.
* Aboneliğinizde, PeerIng Service'i etkinleştirmek istediğiniz doğrudan bir bakış seçin. Eğer yoksa, ya eski doğrudan bakan dönüştürmek veya yeni bir Doğrudan bakan oluşturun.
    * Eski bir Doğrudan bakışlandırmayı dönüştürmek [için, PowerShell'i kullanarak eski bir doğrudan](howto-legacy-direct-powershell.md)bakışı Azure kaynağına dönüştür'teki yönergeleri izleyin.
    * Yeni bir Doğrudan eşleme oluşturmak için [PowerShell'i kullanarak Doğrudan eşleme oluştur'daki yönergeleri izleyin veya doğrudan bir eşleme oluşturun'i değiştirin.](howto-direct-powershell.md)

### <a name="working-with-azure-powershell"></a>Azure PowerShell ile çalışma
[!INCLUDE [CloudShell](./includes/cloudshell-powershell-about.md)]

## <a name="enable-peering-service-on-a-direct-peering"></a>Doğrudan eşleme üzerinde Eşleme Hizmeti’ni etkinleştir

### <a name="view-direct-peering"></a><a name= get></a>Doğrudan bakış görüntüleme
[!INCLUDE [peering-direct-get](./includes/direct-powershell-get.md)]

### <a name="enable-the-direct-peering-for-peering-service"></a><a name= get></a>Peering Service için Doğrudan eşlemesini etkinleştirme

Önceki adımda Doğrudan bakma aldıktan sonra, Peering Service için etkinleştirin.
[!INCLUDE [peering-direct-modify](./includes/peering-service-direct-powershell.md)]

## <a name="modify-a-direct-peering-connection"></a>Doğrudan eşleme bağlantısını değiştirme

Bağlantı ayarlarını değiştirmeniz gerekiyorsa, Create'deki **Doğrudan eşleme** bölümünü [değiştir'e bakın veya PowerShell'i kullanarak Doğrudan eşleme](howto-direct-powershell.md) bölümünü değiştirin

## <a name="next-steps"></a>Sonraki adımlar

* [PowerShell kullanarak Exchange eşleme oluşturma veya değiştirme](howto-exchange-powershell.md)
* [PowerShell'i kullanarak Azure kaynağına bakan eski bir Exchange'i dönüştürme](howto-legacy-exchange-powershell.md)

## <a name="additional-resources"></a>Ek kaynaklar
Aşağıdaki komutu çalıştırarak tüm parametrelerin ayrıntılı açıklamalarını alabilirsiniz:

```powershell
Get-Help Get-AzPeering -detailed
```

Sık sorulan sorular için [Bkz. Akran Hizmeti SSSS.'](service-faqs.md)