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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76044928"
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