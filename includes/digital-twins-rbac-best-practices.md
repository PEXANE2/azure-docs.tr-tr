---
title: include dosyası
description: include dosyası
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 08/12/2019
ms.author: v-adgera
ms.custom: include file
ms.openlocfilehash: fa88bf7f8db9b4fea63429dbfd18f84ee84ccda1
ms.sourcegitcommit: 5b76581fa8b5eaebcb06d7604a40672e7b557348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "69012317"
---
Rol tabanlı erişim denetimi, erişimi, izinleri ve rolleri yönetmeye yönelik devralma temelli bir güvenlik stratejisidir. Alt roller, üst rollerden izinleri devralınır. İzinler, üst rolden devralınmaksızın da atanabilir. Ayrıca, gerektiğinde bir rolü özelleştirmek için de atanabilir.

Örneğin, bir alan yöneticisinin belirtilen bir alana yönelik tüm işlemleri çalıştırmak için genel erişime ihtiyacı vardır. Erişim, alanın altındaki veya içindeki tüm düğümleri içerir. Cihaz yükleyicisinin, cihazlar ve sensörler için yalnızca *okuma* ve *güncelleştirme* izinlerine ihtiyacı bulunabilir.

Her durumda, roller tam olarak verilir ve en az ayrıcalık Ilkesi başına görevlerini yerine getirmek için *gereken erişimden daha fazla olmaz* . Bu ilkeye göre *yalnızca*bir kimlik verilir:

* İşini tamamlaması için gereken erişim miktarı.
* Bir rol, işini gerçekleştirme ile ilgili ve sınırlı.

>[!IMPORTANT]
> Her zaman en az ayrıcalık Ilkesini izleyin.

Aşağıdaki iki önemli rol tabanlı erişim denetimi uygulaması:

> [!div class="checklist"]
> * Her rolün doğru izinlere sahip olduğunu doğrulamak için rol atamalarını düzenli olarak denetleyin.
> * Bireyler rolleri veya atamaları değiştirirken rolleri ve atamaları temizleyin.