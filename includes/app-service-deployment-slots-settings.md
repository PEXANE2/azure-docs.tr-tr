---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 09/18/2019
ms.author: cephalin
ms.openlocfilehash: e00db06346b19ef85eb77626eb2ed169d2224b6c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "71129685"
---
Yapılandırmayı başka bir dağıtım yuvasından klonladiğinizde, klonlanan yapılandırma değiştirilebilir. Bazı yapılandırma öğeleri bir takas boyunca içeriği takip eder (yuvaya özgü değildir), diğer yapılandırma öğeleri ise bir takastan sonra aynı yuvada kalır (yuvaya özgü). Aşağıdaki listeler, yuvaları değiştirdiğinizde değişen ayarları gösterir.

**Değiştirilen ayarlar:**

* Çerçeve sürümü, 32/64-bit, web soketleri gibi genel ayarlar
* Uygulama ayarları (yuvaya yapışacak şekilde yapılandırılabilir)
* Bağlantı dizeleri (yuvaya yapışacak şekilde yapılandırılabilir)
* Işleyici eşlemeleri
* Ortak sertifikalar
* WebJobs içeriği
* Karma bağlantılar *
* Sanal ağ entegrasyonu *
* Hizmet bitiş noktaları *
* Azure İçerik Dağıtım Ağı *

Yıldız işaretiyle işaretlenmiş özellikler (*) değiştirilmeden planlanır. 

**Değiştirilen olmayan ayarlar:**

* Son noktaları yayımlama
* Özel etki alanı adları
* Halka açık olmayan sertifikalar ve TLS/SSL ayarları
* Ölçek ayarları
* Webİşler zamanlayıcıları
* IP kısıtlamaları
* Her Zaman Anın
* Tanılama günlüğü ayarları
* Orijinler arası kaynak paylaşımı (CORS)

> [!NOTE]
> Değiştirilmemiş ayarlariçin geçerli olan bazı uygulama ayarları da değiştirilmemiştir. Örneğin, tanılama günlüğü ayarları değiştirilemediğinden, `WEBSITE_HTTPLOGGING_RETENTION_DAYS` yuva `DIAGNOSTICS_AZUREBLOBRETENTIONDAYS` ayarları olarak görünmeseler bile ilgili uygulama ayarları gibi ve aynı zamanda değiştirilmeyecektir.
>
