---
title: PeerIng Service - SSS
titleSuffix: Azure
description: PeerIng Service - SSS
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: reference
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: 507e503b881df123ffc6694c53b0e9cc9b6a8872
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75775478"
---
# <a name="peering-service---faqs"></a>Bakma Hizmeti - SSS

Genel sorular için aşağıdaki bilgileri inceleyebilirsiniz.

**Bir operatör, Eşleme Hizmetini desteklemek için microsoft ile varolan Doğrudan bakışlarını kullanabilir mi?**

Evet, bir taşıyıcı, Peering Service'i desteklemek için mevcut PNI'sini kullanabilir. Bir Peering Service PNI HA desteklemek için çeşitlilik gerektirir. Mevcut PNI zaten çeşitlilik varsa, o zaman yeni bir altyapı gereklidir. Mevcut PNI çeşitliliğe ihtiyaç varsa, o zaman artırılabilir.

**Bir operatör, Peering Service'i desteklemek için Microsoft ile yeni Doğrudan bakışlama kullanabilir mi?**

Evet, bu da mümkün. Microsoft, Karşılama Hizmetini desteklemek için yeni Direct peering oluşturmak için Carrier ile birlikte çalışacaktır.  

**Doğrudan bakma, Bakma Hizmetini desteklemek için neden bir gerekliliktir?**

Peering Service'in arkasındaki birincil sürücülerden biri, iyi bağlanmış bir SP aracılığıyla Microsoft çevrimiçi hizmetlerine bağlantı sağlamaktır. PNI her zaman Gbps aralığında ve dolayısıyla taşıyıcı ve Microsoft arasında yüksek verim bağlantısı için temel bir yapı taşı bulunmaktadır.

**Doğrudan Bakma Hizmetini desteklemenin çeşitlilik gereksinimleri nelerdir?**

Bir PNI yerel artıklık ve coğrafi artıklık desteklemelidir. Yerel artıklık, belirli bir izleme sitesinde iki farklı yol kümesi olarak tanımlanır. Coğrafi artıklık, birincil sitenin başarısız olması durumunda Taşıyıcı'nın farklı bir Microsoft kenar sitesinde ek bağlantıya sahip olması gerekir. Kısa arıza süresi için taşıyıcı yedekleme sitesi üzerinden trafik yönlendirebilirsiniz.

**Taşıyıcı zaten SLA ve kurumsal sınıf Internet sunuyor, nasıl bu teklif farklı?**

Bazı operatörler ağın kendi tarafında SLA ve kurumsal sınıf Internet sunuyoruz. Peering Service'de Microsoft, SLA'ya ağın Microsoft bölümünde teklif trafiği sunacaktır. Peering Service müşteri seçerek uçlardan uca SLA alırsınız. IsS ağındaki Microsoft kenarına kendi sitesinden SLA ISS tarafından karşılanabilir. Microsoft Global Network'teki SLA'da Microsoft edge'den son kullanıcılara uygulama artık Microsoft tarafından karşılanıyor.

**Bir hizmet sağlayıcısı PNI kullanarak Microsoft ile zaten eşeşse, Peering Service'i desteklemek için ne tür değişiklikler gerekir?**

* Bir Peering Service kullanıcısını ve trafiğini tanımlamak için yazılım değişiklikleri. Gözetmen Hizmet bağlantısı aracılığıyla bir kullanıcının trafiğini en yakın Microsoft kenarında değiştirmek için yönlendirme ilkesi değişiklikleri gerektirebilir.
* Bağlantının yerel artıklığa ve coğrafi artıklığa sahip olduğundan emin olun.
