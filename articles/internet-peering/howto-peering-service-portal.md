---
title: Azure portalını kullanarak Doğrudan bakışlamada Azure Bakış Hizmetini etkinleştirme
titleSuffix: Azure
description: Azure portalını kullanarak Doğrudan bakışlamada Azure Bakış Hizmetini etkinleştirme
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: article
ms.date: 3/18/2020
ms.author: derekol
ms.openlocfilehash: 625a17e5acff00f78c5a19725653eec629936f87
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81687036"
---
# <a name="enable-azure-peering-service-on-a-direct-peering-by-using-the-azure-portal"></a>Azure portalını kullanarak Doğrudan bakışlamada Azure Bakış Hizmetini etkinleştirme

Bu makalede, Azure portalını kullanarak Doğrudan bakışda Azure [Bakış Hizmeti'nin](overview-peering-service.md) nasıl etkinleştirilen açıklanmaktadır.

İsterseniz, [PowerShell](howto-peering-service-powershell.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [ön koşulları](prerequisites.md) gözden geçirin.
* Aboneliğinizde, Peering Service'i etkinleştirmek istediğiniz doğrudan bir bakış seçin. Eğer yoksa, ya eski doğrudan bakan dönüştürmek veya yeni bir Doğrudan bakan oluşturmak:
    * Eski bir Doğrudan bakışı dönüştürmek [için, portalı kullanarak bir Azure kaynağına doğrudan bakan bir eskiyi dönüştür'teki](howto-legacy-direct-portal.md)yönergeleri izleyin.
    * Yeni bir Doğrudan eşleme oluşturmak için, Portalı kullanarak Doğrudan bakış oluşturma yönergelerini izleyin [veya Doğrudan bir bakış oluşturma'yı değiştirin.](howto-direct-portal.md)

## <a name="enable-peering-service-on-a-direct-peering"></a>Doğrudan eşleme üzerinde Eşleme Hizmeti’ni etkinleştir

### <a name="view-direct-peering"></a><a name= get></a>Doğrudan bakış görüntüleme
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

### <a name="enable-the-direct-peering-for-peering-service"></a><a name= get></a>Peering Service için Doğrudan eşlemesini etkinleştirme

Önceki adımda Doğrudan bir eşleçaçtıktan sonra, Onu Bakma Hizmeti için etkinleştirin.
[!INCLUDE [peering-direct-modify](./includes/peering-service-direct-portal.md)]

## <a name="modify-a-direct-peering-connection"></a>Doğrudan eşleme bağlantısını değiştirme

Bağlantı ayarlarını değiştirmek için, Oluştur'daki "Doğrudan eşleme" bölümüne bakın [veya portalı kullanarak Doğrudan bir eşleçle değiştirin.](howto-direct-portal.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak Exchange eşlemi oluşturma veya değiştirme](howto-exchange-portal.md)
* [Portalı kullanarak azure kaynağına bakan eski bir Exchange'i dönüştürme](howto-legacy-exchange-portal.md)

## <a name="additional-resources"></a>Ek kaynaklar

Sık sorulan sorular [için, Bakma Hizmeti SSS'si'ne](service-faqs.md)bakın.