---
title: Portalı kullanarak doğrudan eşleme üzerinde eşleme hizmetini etkinleştirme
titleSuffix: Azure
description: Portalı kullanarak doğrudan eşleme üzerinde eşleme hizmetini etkinleştirme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: d6b67c42ef8a5ba5ae98894775d1f56cee39ba8b
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75774984"
---
# <a name="enable-peering-service-on-a-direct-peering-using-the-portal"></a>Portalı kullanarak doğrudan eşleme üzerinde eşleme hizmetini etkinleştirme

Bu makalede, portalı kullanarak doğrudan eşleme üzerinde [eşleme hizmeti](overview-peering-service.md) 'nin nasıl etkinleştirileceği açıklanır.

İsterseniz, [PowerShell](howto-peering-service-powershell.md)'i kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [önkoşulları](prerequisites.md) gözden geçirin.
* Aboneliğinizde eşleme hizmetini etkinleştirmek istediğiniz bir doğrudan eşleme seçin. Bir tane yoksa, eski bir doğrudan eşlemeyi dönüştürün veya yeni bir doğrudan eşleme oluşturun.
    * Eski bir doğrudan eşlemeyi dönüştürmek için [portalı kullanarak eski bir doğrudan eşlemeyi Azure kaynağına dönüştürme](howto-legacy-direct-portal.md)bölümündeki yönergeleri izleyin.
    * Yeni bir doğrudan eşleme oluşturmak için [portalı kullanarak doğrudan eşleme oluşturma veya değiştirme](howto-direct-portal.md)bölümündeki yönergeleri izleyin.

## <a name="enable-peering-service-on-a-direct-peering"></a>Doğrudan eşleme üzerinde eşleme hizmetini etkinleştir

### <a name= get></a>Doğrudan eşlemeyi görüntüle
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

### <a name= get></a>Eşleme hizmeti için doğrudan eşlemeyi etkinleştir

Önceki adımda doğrudan eşleme 'yı açtıktan sonra, eşleme hizmeti için etkinleştirin.
[!INCLUDE [peering-direct-modify](./includes/peering-service-direct-portal.md)]

## <a name="modify-a-direct-peering-connection"></a>Doğrudan eşleme bağlantısını değiştirme

Bağlantı ayarlarını değiştirmeniz gerekiyorsa, [portalı kullanarak doğrudan eşleme oluşturma veya değiştirme](howto-direct-portal.md) konusunun **doğrudan eşlemeyi değiştirme** bölümüne bakın

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak Exchange eşlemesi oluşturma veya değiştirme](howto-exchange-portal.md)
* [Portalı kullanarak eski bir Exchange eşlemesini Azure kaynağına dönüştürme](howto-legacy-exchange-portal.md)

## <a name="additional-resources"></a>Ek kaynaklar

Sık sorulan sorular için bkz. [eşleme HIZMETI SSS](service-faqs.md).