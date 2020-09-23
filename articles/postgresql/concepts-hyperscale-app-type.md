---
title: Uygulama türünü belirleme-hiper ölçek (Citus)-PostgreSQL için Azure veritabanı
description: Etkin dağıtılmış veri modelleme için uygulamanızı tanımla
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 07/17/2020
ms.openlocfilehash: 92333857177d33307d6997bfcbdf79787d3ab127
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90895947"
---
# <a name="determining-application-type"></a>Uygulama türünü belirleme

Bir Hyperscale (Citus) sunucu grubunda etkili sorgular çalıştırmak, tabloların sunucular arasında düzgün bir şekilde dağıtılmasını gerektirir. Önerilen dağıtım, uygulama türüne ve sorgu desenlerine göre değişir.

Hiperscale üzerinde iyi çalışan çok sayıda uygulama vardır (Citus). Veri modellemesinin ilk adımı, uygulamanızın uygulamanıza ne kadar benzediğini belirlemektir.

## <a name="at-a-glance"></a>Bir bakışta

| Çok kiracılı uygulamalar                                 | Gerçek zamanlı uygulamalar                                |
|-----------------------------------------------------------|-------------------------------------------------------|
| Bazen şemada düzinelerce veya yüzlerce tablo          | Az sayıda tablo                                |
| Tek seferde bir kiracıyla (Şirket/mağaza) ilgili sorgular | Toplamalara sahip görece basit analiz sorguları |
| Web istemcilerine hizmet vermeye yönelik OLTP iş yükleri                    | Çoğunlukla sabit verilerin yüksek alım hacmi           |
| Kiracı başına analitik sorguları sunan OLAP iş yükleri   | Büyük olaylar tablosu etrafında genellikle ortalama            |

## <a name="examples-and-characteristics"></a>Örnekler ve Özellikler

**Çok kiracılı uygulama**

> Bunlar genellikle diğer şirketlere, hesaplara veya kuruluşlara sunan SaaS uygulamalardır. Birçok SaaS uygulaması, doğal olarak ilişkisel bir veritabanıdır. Düğümler arasında verilerin dağıtılacağı doğal bir boyutu vardır: kiracı kimliğine göre yalnızca parçalar \_ .
>
> Hiper ölçek (Citus), uygulamanızı yeniden mimarmadan milyonlarca kiracıya kadar kiracın ölçeğini ayarlamanıza olanak sağlar. Birleşimler, yabancı anahtar kısıtlamaları, işlemler, ACID ve tutarlılık gibi ihtiyacınız olan ilişkisel semantiğini koruyabilirsiniz.
>
> -   **Örnekler**: bir dijital pazarlama çözümü ya da bir satış Otomasyon Aracı gibi diğer işletmeler için mağazaların barındırdıkları Web siteleri.
> -   **Özellikler**: kiracılar genelinde bilgi birleştirmek yerine tek bir kiracıyla ilgili sorgular. Bu, Web istemcilerine hizmet sunmak için OLTP iş yüklerini ve kiracı başına analitik sorgulara hizmet veren OLAP iş yüklerini içerir. Veritabanı şemanızda onlarca veya yüzlerce tablo olması, çok kiracılı veri modeli için de bir göstergedir.
>
> Çok kiracılı bir uygulamanın hiper ölçeğe göre ölçeklendirilmesi (Citus), uygulama kodunda küçük değişiklikler yapılmasını da gerektirir. Ruby on rayları ve Docgo gibi popüler çerçeveler için destek sunuyoruz.

**Gerçek zamanlı analiz**

> Büyük paralellik gerektiren uygulamalar, hızlı sonuçlar için yüzlerce çekirdeği sayısal, istatistiksel veya saymaya yönelik olarak koordine.  Hiper ölçek (Citus), birden çok düğümde SQL sorguları oluşturup paralelleştirerek, bir saniyede milyarlarca kayıt arasında gerçek zamanlı sorgular gerçekleştirmeye olanak tanır.
>
> Gerçek zamanlı analiz veri modellerindeki tablolar genellikle Kullanıcı \_ kimliği, ana bilgisayar \_ kimliği veya cihaz kimliği gibi sütunlarla dağıtılır \_ .
>
> -   **Örnekler**: müşterilere yönelik ve alt saniyelik yanıt süreleri gerektiren analiz panoları.
> -   **Özellikler**: genellikle büyük bir cihaz, site veya Kullanıcı olay tablosu etrafında ortalama olan ve çoğunlukla sabit verilerin yüksek alım hacmi gerektiren birkaç tablo. Nispeten daha basit (ancak hesaplama açısından yoğun) analiz sorguları, çeşitli toplamalar ve gruplar içeriyor.

Durumunuz yukarıdaki duruma benziyorsa, bir sonraki adım, verilerinizi sunucu grubuna nasıl yerleştireceğinize karar vermeye yöneliktir. Veritabanı Yöneticisi \' seçimi dağıtım sütunları, performansı sağlamak için tipik sorguların erişim desenleriyle aynı olmalıdır.

## <a name="next-steps"></a>Sonraki adımlar

* Uygulamanızdaki tablolar için [bir dağıtım sütunu seçerek](concepts-hyperscale-choose-distribution-column.md) verileri verimli bir şekilde dağıtın
