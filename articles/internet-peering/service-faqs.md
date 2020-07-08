---
title: Eşleme hizmeti-SSS
titleSuffix: Azure
description: Eşleme hizmeti-SSS
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: reference
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 507e503b881df123ffc6694c53b0e9cc9b6a8872
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75775478"
---
# <a name="peering-service---faqs"></a>Eşleme hizmeti-SSS

Genel sorular için aşağıdaki bilgileri gözden geçirebilirsiniz.

**Eşleme hizmetini desteklemek için mevcut olan doğrudan eşlemesini kullanarak bir taşıyıcı olabilir mi?**

Evet, bir taşıyıcı, eşleme hizmetini desteklemek için mevcut PNı 'den yararlanabilir. Bir eşleme hizmeti PNı, HA desteği için bir çeşitliliğe sahiptir. Mevcut PNı 'lar zaten ayrımlılık içeriyorsa, yeni bir altyapı gerekmez. Mevcut PNı 'lar için ayrımlılık gerekiyorsa, genişletilebilir.

**Bir taşıyıcı, eşleme hizmetini desteklemek için Microsoft ile yeni bir doğrudan eşleme kullanabilir mi?**

Evet, bu da mümkündür. Microsoft, eşleme hizmetini desteklemek için yeni bir doğrudan eşleme oluşturmak üzere taşıyıcısı ile birlikte çalışır.  

**Eşleme hizmetini desteklemek için neden doğrudan eşleme gereksinimi var?**

Eşleme hizmeti 'nin arkasındaki birincil sürücülerden biri, iyi bağlanmış bir SP aracılığıyla Microsoft çevrimiçi hizmetler bağlantı sağlamaktır. PNı, her zaman Gbps aralığında ve bu nedenle, taşıyıcı ve Microsoft arasında yüksek aktarım hızı bağlantısı için temel bir bina bloğudur.

**Eşleme hizmetini desteklemek için bir doğrudan eşleme üzerinde ayrımlılık gereksinimleri nelerdir?**

Bir PNı, yerel artıklığı ve coğrafi yedekliliği desteklemelidir. Yerel artıklık, belirli bir eşleme sitesinde iki farklı yol kümesi olarak tanımlanır. Coğrafi yedeklilik, taşıyıcının birincil sitenin başarısız olması durumunda farklı bir Microsoft Edge sitesinde ek bağlantıya sahip olmasını gerektirir. Kısa başarısızlık süresi taşıyıcısı, trafiği yedekleme sitesi üzerinden yönlendirebilir.

**Taşıyıcı zaten SLA ve kurumsal sınıf Internet 'i sunuyor, bu teklif nasıl farklı?**

Bazı taşıyıcılar, ağın parçaları üzerinde SLA ve kurumsal sınıf Internet 'i sunmaktadır. Eşleme hizmeti ' nde, Microsoft, ağın Microsoft bölümünde SLA teklif trafiği sunmaktadır. Eşleme hizmeti müşteriyi seçerek uçtan uca SLA 'Sı alınır. Şirket içinden ISS ağı üzerinde Microsoft Edge 'e SLA 'lar ISS 'nin kapsamına eklenebilir. Microsoft Edge 'den son kullanıcılara Microsoft küresel ağı 'ndan SLA artık Microsoft tarafından kapsanıyor.

**Bir hizmet sağlayıcı zaten PNı kullanarak Microsoft ile eş alıyorsa, eşleme hizmetini desteklemek için ne tür değişiklikler gerekir?**

* Bir eşleme hizmeti kullanıcısını ve bunun trafiğini belirlemek için yazılım değişiklikleri. Eşleme hizmeti bağlantısı aracılığıyla kullanıcı trafiğinin en yakın Microsoft Edge üzerinden Exchange 'e yönlendirilmesi için yönlendirme ilkesi değişiklikleri gerekebilir.
* Bağlantının yerel artıklık ve coğrafi yedeklilik içerdiğinden emin olun.
