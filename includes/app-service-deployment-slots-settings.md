---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 06/18/2019
ms.author: cephalin
ms.openlocfilehash: 0691b1a531ffebbb2c368bdb37dd4d8025fb4a4e
ms.sourcegitcommit: 55e0c33b84f2579b7aad48a420a21141854bc9e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69623696"
---
Yapılandırmayı başka bir dağıtım yuvasından kopyaladığınızda, kopyalanmış yapılandırma düzenlenebilir olur. Bazı yapılandırma öğeleri bir takas (yuvaya özgü değil) içindeki içeriği izler, ancak diğer yapılandırma öğeleri bir değiştirme sonrasında aynı yuvada kalır (yuvaya özgü). Aşağıdaki listeler, yuvaları takas yaparken değişen ayarları gösterir.

**Takas edilen ayarlar**:

* Çerçeve sürümü, 32/64 bit, Web Yuvaları gibi genel ayarlar
* Uygulama ayarları (bir yuvayı kontrol etmek için yapılandırılabilir)
* Bağlantı dizeleri (bir yuvayı kontrol etmek için yapılandırılabilir)
* İşleyici eşlemeleri
* İzleme ve Tanılama ayarları
* Ortak sertifikalar
* WebJobs içeriği
* Karma bağlantılar *
* Sanal Ağ tümleştirmesi *
* Hizmet uç noktaları *
* Azure Content Delivery Network *

Bir yıldız işareti (*) ile işaretlenen özellikler, yuvaya yapışkan olarak yapılacak şekilde planlanmaktadır. 

**Takas olmayan ayarlar**:

* Yayımlama uç noktaları
* Özel etki alanı adları
* Özel sertifikalar ve SSL bağlamaları
* Ölçek ayarları
* WebJobs zamanlayıcılar
* IP kısıtlamaları
* Daima Açık
* Protokol ayarları (HTTPS, TLS sürümü, istemci sertifikaları)
* Tanılama günlüğü ayarları
* Çıkış noktaları arası kaynak paylaşımı (CORS)

<!-- VNET and hybrid connections not yet sticky to slot -->