---
title: Eşleme Hizmeti iş ortağı kılavuzu
titleSuffix: Azure
description: Eşleme Hizmeti iş ortağı kılavuzu
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: d626d98613bd5d988b599d0980c885d7f73bb958
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76720257"
---
# <a name="peering-service-partner-walkthrough"></a>Eşleme Hizmeti iş ortağı kılavuzu

Bu bölümde, Bir Sağlayıcının Doğrudan Bakma Hizmeti'ni etkinleştirmek için izlemesi gereken adımları açıklamaktadır.

## <a name="create-direct-peering-connection-for-peering-service"></a>Peering Service için Doğrudan eşleme bağlantısı oluşturma
Hizmet Sağlayıcılar, Bakma Hizmetini destekleyen yeni Doğrudan bakış oluşturma yaparak coğrafi erişimlerini genişletebilir. Bunu başarmak için,
1. Zaten değilse, Bir Peering Service iş ortağı olun
1. Portalı kullanarak [Doğrudan bir bakış oluşturmak veya değiştirmek](howto-direct-portal.md)için yönergeleri izleyin. Yüksek kullanılabilirlik gereksinimini karşıladığından emin olun.
1. Ardından, portalı [kullanarak Doğrudan bir bakışüzerinde Bakma Hizmetini Etkinleştirme](howto-peering-service-portal.md)adımlarını izleyin.

## <a name="use-legacy-direct-peering-connection-for-peering-service"></a>Peering Service için eski Doğrudan eşleme bağlantısını kullanma
Bakma Hizmetini desteklemek için kullanmak istediğiniz eski Doğrudan bakışlarınız varsa,
1. Zaten değilse, Bir Peering Service iş ortağı olun.
1. Portalı kullanarak eski bir doğrudan azure [kaynağına bakmayı dönüştürme yönergelerini izleyin.](howto-legacy-direct-portal.md) Gerekirse, yüksek kullanılabilirlik gereksinimini karşılamak için ek devreler sipariş edin.
1. Ardından, portalı [kullanarak Doğrudan bir bakışüzerinde Bakma Hizmetini Etkinleştirme](howto-peering-service-portal.md)adımlarını izleyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Epolitikası nabakma](https://peering.azurewebsites.net/peering)hakkında bilgi edinin.
* Microsoft'a doğrudan bakış ayarlama adımları hakkında bilgi edinmek için [Doğrudan bakan gözden geçirme yi](walkthrough-direct-all.md)izleyin.
* Exchange'in Microsoft ile eşlemeyi ayarlama adımları hakkında bilgi edinmek için [Exchange'in eşleme gözden geçirmesini](walkthrough-exchange-all.md)izleyin.