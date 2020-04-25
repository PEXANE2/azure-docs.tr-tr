---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 09/18/2019
ms.author: cephalin
ms.openlocfilehash: b4234acb2ce32980a268e389cb31de9a57ed18e6
ms.sourcegitcommit: edccc241bc40b8b08f009baf29a5580bf53e220c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/24/2020
ms.locfileid: "82131929"
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
* Sanal Ağ tümleştirmesi *
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

> [!NOTE]
> Takas edilmemiş ayarlara uygulanan bazı uygulama ayarları da takas edilmez. Örneğin, Tanılama ayarları değiştirilmediğinden, `WEBSITE_HTTPLOGGING_RETENTION_DAYS` ve `DIAGNOSTICS_AZUREBLOBRETENTIONDAYS` gibi ilgili uygulama ayarları, yuva ayarı olarak gösterilmese bile, aynı zamanda takas edilmez.
>
