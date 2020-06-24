---
title: Azure portal kullanarak doğrudan eşleme üzerinde Azure eşleme hizmetini etkinleştirin
titleSuffix: Azure
description: Azure portal kullanarak doğrudan eşleme üzerinde Azure eşleme hizmetini etkinleştirin
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: how-to
ms.date: 3/18/2020
ms.author: derekol
ms.openlocfilehash: a52e6038b622c004dc0d133394cd4f53600b2935
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84700051"
---
# <a name="enable-azure-peering-service-on-a-direct-peering-by-using-the-azure-portal"></a>Azure portal kullanarak doğrudan eşleme üzerinde Azure eşleme hizmetini etkinleştirin

Bu makalede, Azure portal kullanarak doğrudan eşleme üzerinde Azure [eşleme hizmeti](overview-peering-service.md) 'nin nasıl etkinleştirileceği açıklanır.

İsterseniz, [PowerShell](howto-peering-service-powershell.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [önkoşulları](prerequisites.md) gözden geçirin.
* Aboneliğinizde, eşleme hizmetini etkinleştirmek istediğiniz bir doğrudan eşleme seçin. Bir tane yoksa, eski bir doğrudan eşlemeyi dönüştürün veya yeni bir doğrudan eşleme oluşturun:
    * Eski bir doğrudan eşlemeyi dönüştürmek için [portalı kullanarak eski bir doğrudan eşlemeyi Azure kaynağına dönüştürme](howto-legacy-direct-portal.md)bölümündeki yönergeleri izleyin.
    * Yeni bir doğrudan eşleme oluşturmak için [portalı kullanarak doğrudan eşleme oluşturma veya değiştirme](howto-direct-portal.md)bölümündeki yönergeleri izleyin.

## <a name="enable-peering-service-on-a-direct-peering"></a>Doğrudan eşleme üzerinde Eşleme Hizmeti’ni etkinleştir

### <a name="view-direct-peering"></a><a name= get></a>Doğrudan eşlemeyi görüntüle
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

### <a name="enable-the-direct-peering-for-peering-service"></a><a name= get></a>Eşleme hizmeti için doğrudan eşlemeyi etkinleştir

Önceki adımda bir doğrudan eşleme açtıktan sonra, bunu eşleme hizmeti için etkinleştirin.
[!INCLUDE [peering-direct-modify](./includes/peering-service-direct-portal.md)]

## <a name="modify-a-direct-peering-connection"></a>Doğrudan eşleme bağlantısını değiştirme

Bağlantı ayarlarını değiştirmek için, [portalı kullanarak doğrudan eşleme oluşturma veya değiştirme](howto-direct-portal.md)konusunun "doğrudan eşlemeyi değiştirme" bölümüne bakın.

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak Exchange eşlemesi oluşturma veya değiştirme](howto-exchange-portal.md)
* [Portalı kullanarak eski bir Exchange eşlemesini Azure kaynağına dönüştürme](howto-legacy-exchange-portal.md)

## <a name="additional-resources"></a>Ek kaynaklar

Sık sorulan sorular için bkz. [eşleme hizmeti hakkında SSS](service-faqs.md).