---
title: include dosyası
description: include dosyası
services: digital-twins
author: kingdomofends
ms.service: digital-twins
ms.topic: include
ms.date: 09/30/2019
ms.author: v-adgera
ms.custom: include file
ms.openlocfilehash: 1c8237ce4e8b758395567e939c1ed4bda1f297ff
ms.sourcegitcommit: 7c2dba9bd9ef700b1ea4799260f0ad7ee919ff3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71827656"
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