---
title: include dosyası
description: include dosyası
services: digital-twins
ms.author: alinast
author: alinamstanciu
manager: bertvanhoof
ms.service: digital-twins
ms.topic: include
ms.date: 01/15/2020
ms.custom: include file
ms.openlocfilehash: 893beb0800af0eece4d69e727e427c3e92b79121
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76044928"
---
Rol tabanlı erişim denetimi, erişimi, izinleri ve rolleri yönetmek için devralmaya dayalı bir güvenlik stratejisidir. Azalan roller, izinleri üst rollerden devralır. İzinler, bir üst rolden devralınmadan da atanabilir. Ayrıca, gerektiğinde bir rolü özelleştirmek için atanabilirler.

Örneğin, bir Uzay Yöneticisi'nin belirli bir alan için tüm işlemleri çalıştırmak için genel erişime ihtiyacı olabilir. Erişim, alanın altındaki veya içindeki tüm düğümleri içerir. Aygıt Yükleyicisi'nin aygıtlar ve sensörler için yalnızca *okuma* ve *güncelleştirme* izinleri gerekebilir.

Her durumda, roller tam olarak verilir ve en az ayrıcalık ilkesi ne olursa olsun görevlerini yerine getirmek için *gereken erişimden daha fazla değil.* Bu ilkeye göre, bir kimlik *sadece*verilir:

* İşini tamamlamak için gereken erişim miktarı.
* İşini yürütmekle sınırlı ve uygun bir rol.

>[!IMPORTANT]
> Her zaman En Az Ayrıcalık Prensibi'ne uyun.

İzaltılması gereken diğer iki önemli rol tabanlı erişim denetimi uygulaması:

> [!div class="checklist"]
> * Her rolün doğru izinlere sahip olduğunu doğrulamak için rol atamalarını düzenli aralıklarla denetler.
> * Bireyler rolleri veya atamaları değiştirdiğinde rolleri ve atamaları temizleyin.