---
title: Visual Studio App Center ve Azure hizmetleriyle mobil uygulama geliştirmede farklı aşamalar
description: Mobil uygulama geliştirmeye ilişkin farklı aşamalar ve Visual Studio App Center gibi Microsoft hizmetlerinin yüksek sınıf bir mobil uygulama oluşturmanıza nasıl yardımcı olabileceğini öğrenin.
author: codemillmatt
ms.assetid: 12bbb070-9b3c-1234-5678-ccff02097224
ms.service: mobile-services
ms.topic: article
ms.date: 06/05/2020
ms.author: masoucou
ms.openlocfilehash: cd76ff64777892397aeb6152445652fae6f4ba24
ms.sourcegitcommit: f57fa5f3ce40647eda93f8be4b0ab0726d479bca
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/07/2020
ms.locfileid: "84485275"
---
# <a name="different-stages-in-mobile-application-development"></a>Mobil uygulama geliştirmede farklı aşamalar
Mobil uygulama oluşturmak birden çok aşamadan oluşur. Yüksek sınıf mobil uygulamalar oluşturmak için Microsoft Hizmetleri, araçları ve teknolojiden kolayca yararlanabilirsiniz.

## <a name="app-development"></a>Uygulama geliştirme
Hedef-C ve Java gibi dillerde yerel tek platformlu istemci uygulamaları oluşturabilirsiniz. Xamarin, doğal yerel ve Unity kullanarak platformlar arası uygulamalar oluşturabilirsiniz. Ayrıca, Cordova kullanarak karma uygulamalar oluşturabilirsiniz. İstemci uygulamaları oluşturmak için, bilgisayarlarda ve Mac 'lerde Visual Studio, Mac için Visual Studio veya Visual Studio Code gibi Ides ve kod düzenleyicilerinden yararlanabilirsiniz.

## <a name="plan-and-design-with-back-end-services"></a>Arka uç hizmetleriyle planlayın ve tasarlayın
 Microsoft, mobil uygulamanız için güvenli, ölçeklenebilir ve güvenilir bir arka uç ayarlamaya yönelik çeşitli hizmetler sunar. Kendi sunucularınızı sürdürmek zorunda kalmadan sunucusuz API 'Ler ve Tetikleyiciler oluşturabilirsiniz. Çeşitli uygulama oluşturma senaryolarını destekleyen kapsamlı ve kolay bir mobil arka uç oluşturmak için anında iletme bildirimleri, çevrimdışı veri eşitleme, kimlik doğrulama ve veri hizmetlerini kullanın. 
 
 Bir arka uç hizmetinin oluşturulması birçok gereksinimi vardır. Şunları yapmanız gerekir:
   - Cihazdaki veri depolamayı sınırlayın.
   - Verileri birden çok cihazda eşitler.
   - Uygulamanın ağ bağlantısı olmadan çalıştığından emin olun.
   - Kullanıcılara bildirimler gönderme.
   - Pil tüketimini azaltın.
   - Ölçeklenebilir, güvenilir ve güvenli bir arka uç oluşturun.
   - Sunucu bakımını otomatikleştirin.

## <a name="devops-and-continuous-monitoring"></a>DevOps ve sürekli izleme
Mobil ve bulut odaklı uygulamalar oluşturmak için, tüm platformlar için yerel derlemeler oluşturmak üzere sürekli tümleştirme (CI), sürekli teslim (CD) ve sürekli izleme hizmetlerinden yararlanın. Kullanıcı Arabirimi Otomasyonu testlerini binlerce fiziksel cihazda çalıştırabilir, beta test kanalları aracılığıyla sürümlerini otomatikleştirebilir veya doğrudan uygulama depolarına dağıtabilirsiniz. Ayrıca, uygulamaları için kullanım analizlerini, hata ve özel durum bilgilerini toplayabilirsiniz.

## <a name="additional-services"></a>Ek hizmetler
Birçok Azure hizmeti, oluşturulmakta olan uygulamanın türüne ve yapısına bağlı olarak tüketilebilir:
  - Hızlı sorgulama ile tüm iç veri türlerini aramak için Azure Bilişsel Arama.
  - Uygulamanızda yapay zeka kullanımı için Azure bilişsel hizmetler.
  - Uygulamanızda gerçek zamanlı iletişimi etkinleştirmek için Azure SignalR.
