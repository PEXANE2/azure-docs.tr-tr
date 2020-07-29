---
title: Eşleme Hizmeti iş ortağı kılavuzu
titleSuffix: Azure
description: Eşleme Hizmeti iş ortağı kılavuzu
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: how-to
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 75ec1a4ededfea4f9b40461d69b6e16d947e6919
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84712488"
---
# <a name="peering-service-partner-walkthrough"></a>Eşleme Hizmeti iş ortağı kılavuzu

Bu bölümde, eşleme hizmeti için doğrudan eşlemeyi etkinleştirmek üzere sağlayıcının izlenmesi gereken adımlar açıklanmaktadır.

## <a name="create-direct-peering-connection-for-peering-service"></a>Eşleme hizmeti için doğrudan eşleme bağlantısı oluşturma
Hizmet sağlayıcıları, eşleme hizmetini destekleyen yeni bir doğrudan eşleme oluşturarak coğrafi erişimini genişletebilir. Bunu gerçekleştirmek için
1. Henüz yoksa bir eşleme hizmeti ortağı olun
1. [Portalı kullanarak doğrudan eşleme oluşturmak veya değiştirmek](howto-direct-portal.md)için yönergeleri izleyin. Yüksek kullanılabilirlik gereksinimini karşıladığından emin olun.
1. Ardından, [portalı kullanarak doğrudan eşleme üzerinde eşleme hizmetini etkinleştirmek](howto-peering-service-portal.md)için adımları izleyin.

## <a name="use-legacy-direct-peering-connection-for-peering-service"></a>Eşleme hizmeti için eski doğrudan eşleme bağlantısı kullan
Eşleme hizmetini desteklemek için kullanmak istediğiniz eski bir doğrudan eşlemeye sahipseniz,
1. Henüz yoksa bir eşleme hizmeti ortağı olun.
1. [Portalı kullanarak eski bir doğrudan eşlemeyi Azure kaynağına dönüştürmek](howto-legacy-direct-portal.md)için yönergeleri izleyin. Gerekirse, yüksek kullanılabilirlik gereksinimini karşılamak için ek devreler sıralayın.
1. Ardından, [portalı kullanarak doğrudan eşleme üzerinde eşleme hizmetini etkinleştirmek](howto-peering-service-portal.md)için adımları izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Eşleme ilkesi](https://peering.azurewebsites.net/peering)hakkında bilgi edinin.
* Microsoft ile doğrudan eşleme ayarlama adımları hakkında bilgi edinmek için [doğrudan eşleme](walkthrough-direct-all.md)Kılavuzu ' nu izleyin.
* Exchange eşlemesini Microsoft ile ayarlamaya yönelik adımlar hakkında bilgi edinmek için [Exchange eşleme](walkthrough-exchange-all.md)Kılavuzu ' nu izleyin.