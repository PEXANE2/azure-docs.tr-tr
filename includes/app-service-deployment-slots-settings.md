---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 09/18/2019
ms.author: cephalin
ms.openlocfilehash: 7458f6868d7fbee72b55ad002148691a113c269d
ms.sourcegitcommit: 66479d7e55449b78ee587df14babb6321f7d1757
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/15/2020
ms.locfileid: "97532062"
---
Yapılandırmayı başka bir dağıtım yuvasından kopyaladığınızda, kopyalanmış yapılandırma düzenlenebilir olur. Bazı yapılandırma öğeleri bir takas (yuvaya özgü değil) içindeki içeriği izler, ancak diğer yapılandırma öğeleri bir değiştirme sonrasında aynı yuvada kalır (yuvaya özgü). Aşağıdaki listeler, yuvaları takas yaparken değişen ayarları gösterir.

**Takas edilen ayarlar**:

* Çerçeve sürümü, 32/64 bit, Web Yuvaları gibi genel ayarlar
* Uygulama ayarları (bir yuvayı kontrol etmek için yapılandırılabilir)
* Bağlantı dizeleri (bir yuvayı kontrol etmek için yapılandırılabilir)
* İşleyici eşlemeleri
* Ortak sertifikalar
* WebJobs içeriği
* Karma bağlantılar *
* Hizmet uç noktaları *
* Azure Content Delivery Network *

Yıldız işareti (*) ile işaretlenen özellikler, takas edilmemiş olarak planlanmaktadır. 

**Takas olmayan ayarlar**:

* Yayımlama uç noktaları
* Özel etki alanı adları
* Genel olmayan sertifikalar ve TLS/SSL ayarları
* Ölçek ayarları
* WebJobs zamanlayıcılar
* IP kısıtlamaları
* Her zaman açık
* Tanılama ayarları
* Çıkış noktaları arası kaynak paylaşımı (CORS)
* Sanal ağ tümleştirmesi

> [!NOTE]
> Bu ayarları değiştirilebilir yapmak için uygulamanın her yuvasında uygulama ayarını ekleyin `WEBSITE_OVERRIDE_PRESERVE_DEFAULT_STICKY_SLOT_SETTINGS` ve değerini veya olarak ayarlayın `0` `false` . Bu ayarlar tamamen değiştirilebilir veya hiç yok. Başkalarının değil, yalnızca bazı ayarları deitirilebilir hale getirebilirsiniz.

> Takas edilmemiş ayarlara uygulanan bazı uygulama ayarları da takas edilmez. Örneğin, Tanılama ayarları değiştirilmediğinden, ve gibi ilgili uygulama ayarları, `WEBSITE_HTTPLOGGING_RETENTION_DAYS` `DIAGNOSTICS_AZUREBLOBRETENTIONDAYS` yuva ayarı olarak gösterilmese bile, aynı zamanda takas edilmez.
>
