---
title: Portalı kullanarak Doğrudan eşleme üzerinde Eşleme Hizmeti’ni etkinleştirme
titleSuffix: Azure
description: Portalı kullanarak Doğrudan eşleme üzerinde Eşleme Hizmeti’ni etkinleştirme
services: internet-peering
author: derekolo
ms.service: internet-peering
ms.topic: article
ms.date: 3/18/2020
ms.author: derekol
ms.openlocfilehash: 87a65826a338f4098ae24c33939ea7f9d4475e36
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80129930"
---
# <a name="enable-peering-service-on-a-direct-peering-using-the-portal"></a>Portalı kullanarak Doğrudan eşleme üzerinde Eşleme Hizmeti’ni etkinleştirme

Bu makalede, portalı kullanarak Doğrudan bir bakışta [Peering Service](overview-peering-service.md) etkinleştirmek için nasıl açıklanır.

İsterseniz, [PowerShell](howto-peering-service-powershell.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [ön koşulları](prerequisites.md) gözden geçirin.
* Aboneliğinizde, PeerIng Service'i etkinleştirmek istediğiniz doğrudan bir bakış seçin. Eğer yoksa, ya eski doğrudan bakan dönüştürmek veya yeni bir Doğrudan bakan oluşturun.
    * Eski bir Doğrudan bakışı dönüştürmek [için, portalı kullanarak eski bir doğrudan](howto-legacy-direct-portal.md)bakışı Azure kaynağına dönüştür'teki yönergeleri izleyin.
    * Yeni bir Doğrudan eşleme oluşturmak için, Portalı kullanarak Doğrudan eşleme oluştur'daki yönergeleri izleyin [veya doğrudan bir eşleme'yi değiştirin.](howto-direct-portal.md)

## <a name="enable-peering-service-on-a-direct-peering"></a>Doğrudan eşleme üzerinde Eşleme Hizmeti’ni etkinleştir

### <a name="view-direct-peering"></a><a name= get></a>Doğrudan bakış görüntüleme
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

### <a name="enable-the-direct-peering-for-peering-service"></a><a name= get></a>Peering Service için Doğrudan eşlemesini etkinleştirme

Önceki adımda Doğrudan eşleme'yi açtıktan sonra, Bakma Hizmeti için etkinleştirin.
[!INCLUDE [peering-direct-modify](./includes/peering-service-direct-portal.md)]

## <a name="modify-a-direct-peering-connection"></a>Doğrudan eşleme bağlantısını değiştirme

Bağlantı ayarlarını değiştirmeniz gerekiyorsa, Oluştur'da **Doğrudan eşleme** bölümünü [değiştir'e bakın veya portalı kullanarak Doğrudan eşleme](howto-direct-portal.md)

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak Exchange eşlemi oluşturma veya değiştirme](howto-exchange-portal.md)
* [Portalı kullanarak eski bir Exchange eşlemesini bir Azure kaynağına dönüştürme](howto-legacy-exchange-portal.md)

## <a name="additional-resources"></a>Ek kaynaklar

Sık sorulan sorular için [Bkz. Akran Hizmeti SSSS.'](service-faqs.md)