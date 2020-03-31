---
title: Azure Hizmet Veri Tos'undaki AMQP 1.0'a genel bakış
description: Azure Hizmet Veri Yolu'nun açık standart bir protokol olan Gelişmiş İleti Sıraya Protokolü'nü (AMQP) nasıl desteklediğini öğrenin.
services: service-bus-messaging
documentationcenter: .net
author: axisc
manager: timlt
editor: spelluru
ms.assetid: 0e8d19cc-de36-478e-84ae-e089bbc2d515
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 50d21cfe8136b9c794eae5104bbb34e28f7c1661
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76759321"
---
# <a name="amqp-10-support-in-service-bus"></a>Servis Veri Servisi'nde AMQP 1.0 desteği
Hem Azure Hizmet Veri Servisi bulut hizmeti hem de Windows Server için şirket içi Servis Veri Servisi [(Service Bus 1.1)](https://msdn.microsoft.com/library/dn282144.aspx) Gelişmiş İleti Kuyruk Protokolü (AMQP) 1.0'ı destekler. AMQP, açık standart bir protokol kullanarak çapraz platform, karma uygulamalar oluşturmanıza olanak tanır. Farklı diller ve çerçeveler kullanılarak oluşturulmuş ve farklı işletim sistemlerinde çalışan bileşenleri kullanarak uygulamalar oluşturabilirsiniz. Tüm bu bileşenler Servis Veri Servisi'ne bağlanabilir ve yapılandırılmış iş mesajlarını verimli ve tam bir doğrulukla sorunsuz bir şekilde değiştirebilir.

## <a name="introduction-what-is-amqp-10-and-why-is-it-important"></a>Giriş: AMQP 1.0 nedir ve neden önemlidir?
Geleneksel olarak, ileti yönelimli ara yazılım ürünleri istemci uygulamaları ve aracılar arasındaki iletişim için özel protokoller kullanılmıştır. Bu, belirli bir satıcının ileti aracısını seçtikten sonra, istemci uygulamalarınızı bu aracıya bağlamak için söz konusu satıcının kitaplıklarını kullanmanız gerektiği anlamına gelir. Bir uygulamayı farklı bir ürüne taşıma yapmak, bağlı tüm uygulamalarda kod değişiklikleri gerektirdiğinden, bu satıcıya bir dereceye kadar bağımlılık sağlar. 

Ayrıca, farklı satıcılardan mesajlaşma brokerları bağlamak zor. Bu genellikle iletileri bir sistemden diğerine taşımak ve özel ileti biçimleri arasında çeviri yapmak için uygulama düzeyinde köprüleme gerektirir. Bu yaygın bir gerekliliktir; örneğin, eski birbirinden farklı sistemlere yeni bir birleşik arabirim sağlamanız veya bir birleşmeden sonra BT sistemlerini tümleştirmeniz gerektiğinde.

Yazılım endüstrisi hızlı hareket eden bir işletmedir; yeni programlama dilleri ve uygulama çerçeveleri bazen şaşırtıcı bir hızda tanıtılır. Benzer şekilde, BT sistemlerinin gereksinimleri zaman içinde gelişir ve geliştiriciler en son platform özelliklerinden yararlanmak isterler. Ancak, bazen seçili ileti satıcısı bu platformları desteklemez. İleti protokolleri tescilli olduğundan, başkalarının bu yeni platformlar için kitaplık sağlaması mümkün değildir. Bu nedenle, ileti ürünlerini kullanmaya devam etmenizi sağlayan ağ geçitleri veya köprüler oluşturma gibi yaklaşımları kullanmanız gerekir.

Gelişmiş İleti Sıraya Protokolü (AMQP) 1.0'ın geliştirilmesi bu sorunlardan kaynaklandı. Bu JP Morgan Chase, kim, en finansal hizmet firmaları gibi, mesaj odaklı middleware ağır kullanıcılar kökenli. Amaç basitti: farklı diller, çerçeveler ve işletim sistemleri kullanılarak oluşturulmuş bileşenleri kullanarak ileti tabanlı uygulamalar oluşturmayı mümkün kılan açık standart bir mesajlaşma protokolü oluşturmak, hepsi de bir dizi türünün en iyisi bileşenleri kullanarak Tedarikçi.

## <a name="amqp-10-technical-features"></a>AMQP 1.0 teknik özellikleri
AMQP 1.0, sağlam, çapraz platform, mesajlaşma uygulamaları oluşturmak için kullanabileceğiniz verimli, güvenilir, kablolu düzeyde bir mesajlaşma protokolüdür. Protokolün basit bir amacı vardır: iki taraf arasında iletilerin güvenli, güvenilir ve verimli bir şekilde aktarılmasının mekaniğini tanımlamak. İletilerin kendileri, heterojen gönderenlerin ve alıcıların yapılandırılmış iş mesajlarını tam doğrulukla değiştirmelerini sağlayan taşınabilir bir veri gösterimi kullanılarak kodlanır. Aşağıda en önemli özelliklerin bir özeti verilmiştir:

* **Verimli**: AMQP 1.0, protokol yönergeleri ve üzerinden aktarılan iş iletileri için ikili kodlama kullanan bağlantı yönelimli bir protokoldür. Ağ ve bağlı bileşenlerin kullanımını en üst düzeye çıkarmak için gelişmiş akış kontrol şemaları içerir. Bununla birlikte, protokol verimlilik, esneklik ve birlikte çalışabilirlik arasında bir denge sağlamak üzere tasarlanmıştır.
* **Güvenilir**: AMQP 1.0 protokolü, iletilerin yangın ve unut'dan güvenilir, tam olarak bir kez kabul edilen teslimata kadar çeşitli güvenilirlik garantileriyle değiştirilmesine olanak tanır.
* **Esnek**: AMQP 1.0, farklı topolojileri desteklemek için kullanılabilecek esnek bir protokoldür. Aynı protokol istemciden müşteriye, istemciden brokera ve broker-broker iletişimleri için kullanılabilir.
* **Broker modeli bağımsız**: AMQP 1.0 belirtimi, bir broker tarafından kullanılan mesajlaşma modelinde herhangi bir gereksinim getirmez. Bu, mevcut mesajlaşma brokerlarına kolayca AMQP 1.0 desteği eklemenin mümkün olduğu anlamına gelir.

## <a name="amqp-10-is-a-standard-with-a-capital-s"></a>AMQP 1.0 bir Standarttır (büyük 'S')
AMQP 1.0, ISO ve IEC tarafından ISO/IEC 19464:2014 olarak onaylanmış uluslararası bir standarttır.

AMQP 1.0, 20'den fazla şirket, hem teknoloji tedarikçisi hem de son kullanıcı firmalarından oluşan bir çekirdek grup tarafından 2008 yılından beri geliştirilmektedir. Bu süre zarfında, kullanıcı firmaları gerçek iş gereksinimlerine katkıda bulunmuş ve teknoloji satıcıları bu gereksinimleri karşılamak için protokolü geliştirmiştir. Süreç boyunca, satıcılar kendi uygulamaları arasında birlikte çalışabilirlik doğrulamak için işbirliği atölyekatıldı.

Ekim 2011'de geliştirme çalışmaları, Yapılandırılmış Bilgi Standartlarını Geliştirme Örgütü (OASIS) bünyesindeki teknik bir komiteye ve Oasis AMQP 1.0 Standardı Ekim 2012'de yayımlandı. Standardın geliştirilmesi sırasında aşağıdaki firmalar teknik komiteye katılmıştır:

* **Teknoloji satıcıları**: Axway Software, Huawei Technologies, IIT Software, INETCO Systems, Kaazing, Microsoft, Mitre Corporation, Primeton Technologies, Progress Software, Red Hat, SITA, Software AG, Solace Systems, VMware, WSO2, Zenika.
* **Kullanıcı firmalar:** Bank of America, Credit Suisse, Deutsche Boerse, Goldman Sachs, JPMorgan Chase.

Açık standartların yaygın olarak belirtilen faydalarından bazıları şunlardır:

* Satıcı kilitleme şansı daha az
* Birlikte Çalışabilirlik
* Kitaplıkların ve takım ların geniş kullanılabilirliği
* Eskimelere karşı koruma
* Bilgili personelin durumu
* Daha düşük ve yönetilebilir risk

## <a name="amqp-10-and-service-bus"></a>AMQP 1.0 ve Servis Otobüsü
Azure Hizmet Veri Yolu'ndaki AMQP 1.0 desteği, artık etkin bir ikili protokol kullanarak çeşitli platformlardan Hizmet Veri Yolu'nu sıraya sokup aracılı mesajlaşma özelliklerini yayımlayabileceğiniz/abone olabileceğiniz anlamına gelir. Ayrıca, dillerin, çerçevelerin ve işletim sistemlerinin bir karışımını kullanarak oluşturulmuş bileşenlerden oluşan uygulamalar oluşturabilirsiniz.

Aşağıdaki şekil, Linux üzerinde çalışan Java istemcilerinin, standart Java İleti Hizmeti (JMS) API'si ve Windows'ta çalışan .NET istemcileri kullanılarak yazıldığı, AMQP 1.0'ı kullanarak Hizmet Veri Servisi aracılığıyla ileti alışverişinde bulunduğu bir örnek dağıtımı göstermektedir.

![][0]

**Şekil 1: Servis Veri Ve Servis Veri Çilesi ve AMQP 1.0 kullanarak platformlar arası mesajlaşmayı gösteren örnek dağıtım senaryosu**

Şu anda aşağıdaki istemci kitaplıklarının Hizmet Veri Servisi ile çalıştığı bilinmektedir:

| Dil | Kitaplık |
| --- | --- |
| Java |Apache Qpid Java İleti Hizmeti (JMS) istemcisi<br/>IIT Yazılım SwiftMQ Java istemcisi |
| C |Apaçi Qpid Proton-C |
| PHP |Apache Qpid Proton-PHP |
| Python |Apaçi Qpid Proton-Python |
| C# |AMQP .NET Lite |

**Şekil 2: AMQP 1.0 istemci kitaplıkları tablosu**

## <a name="summary"></a>Özet
* AMQP 1.0, çapraz platform, karma uygulamalar oluşturmak için kullanabileceğiniz açık ve güvenilir bir mesajlaşma protokolüdür. AMQP 1.0 bir OASIS standardıdır.
* AMQP 1.0 desteği artık Azure Servis Veri Yolunda'nın yanı sıra Windows Server için Servis Veri Servisi'nde de kullanılabilir (Service Bus 1.1). Fiyatlandırma, varolan protokollerle aynıdır.

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi edinmeye hazır mısınız? Aşağıdaki bağlantıları ziyaret edin:

* [AMQP ile .NET'ten Servis Veri Servisi Kullanma]
* [AMQP ile Java'dan Servis Veri Servisi Kullanma]
* [Azure Linux VM'de Apache Qpid Proton-C yükleme]
* [Windows Server için Hizmet Veri Servisi'nde AMQP]

[0]: ./media/service-bus-amqp-overview/service-bus-amqp-1.png
[AMQP ile .NET'ten Servis Veri Servisi Kullanma]: service-bus-amqp-dotnet.md
[AMQP ile Java'dan Servis Veri Servisi Kullanma]: service-bus-amqp-java.md
[Azure Linux VM'de Apache Qpid Proton-C yükleme]: service-bus-amqp-apache.md
[Windows Server için Hizmet Veri Servisi'nde AMQP]: https://msdn.microsoft.com/library/dn574799.aspx
