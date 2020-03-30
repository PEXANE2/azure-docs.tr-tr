---
title: Wingtips uygulamasına hoş geldiniz
description: Bulut ortamındaki Azure SQL Veritabanı için veritabanı kiralama modelleri ve örnek Wingtips SaaS uygulaması hakkında bilgi edinin.
keywords: sql veritabanı öğreticisi
services: sql-database
ms.service: sql-database
ms.subservice: scenario
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: stevestein
ms.author: sstein
ms.reviewer: billgib
ms.date: 01/25/2019
ms.openlocfilehash: 4e0b3afe51ac7c7a6b9213fcee79af57cbbd8197
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73818323"
---
# <a name="the-wingtip-tickets-saas-application"></a>Wingtip Biletleri SaaS uygulaması

Aynı *Wingtip Tickets* SaaS uygulaması üç numunenin her birinde uygulanmaktadır. Uygulama basit bir olay listesi ve bilet SaaS uygulaması küçük mekanları hedefleyen - tiyatrolar, kulüpler, vb. Her mekan uygulamanın kiracısıdır ve kendi verileri vardır: mekan ayrıntıları, etkinlik listeleri, müşteriler, bilet siparişleri, vb.  Uygulama, yönetim komut dosyaları ve öğreticilerle birlikte uçlardan uca bir SaaS senaryosunu sergiler. Buna kiracıların sağlanması, performansın izlenmesi ve yönetilmesi, şema yönetimi ve kiracı lar arası raporlama ve analiz dahildir.

## <a name="three-saas-application-and-tenancy-patterns"></a>Üç SaaS uygulama ve kira desenleri

Uygulamanın üç sürümü mevcuttur; her biri Azure SQL Veritabanı'nda farklı bir veritabanı kiradesi deseni inceler.  İlk kendi veritabanı ile kiracı başına bağımsız bir uygulama kullanır. İkinci kiracı başına bir veritabanı ile çok kiracılı bir uygulama kullanır. Üçüncü örnek, çok kiracılı veritabanlarını parçalanmış çok kiracılı bir uygulama kullanır.

![Üç kira düzeni][image-three-tenancy-patterns]

 Her örnek, uygulama kodunun yanı sıra bir dizi tasarım ve yönetim desenini inceleyen yönetim komut dosyalarını ve öğreticileri içerir.  Her örnek beş dakikadan daha kısa sürede dağıtılır.  Tasarım ve yönetim farklılıklarını karşılaştırabilmeniz için üçü de yan yana dağıtılabilir.

## <a name="standalone-application-per-tenant-pattern"></a>Kiracı deseni başına bağımsız uygulama

Kiracı deseni başına bağımsız uygulama, her kiracı için bir veritabanı ile tek bir kiracı uygulaması kullanır. Veritabanı da dahil olmak üzere her kiracının uygulaması ayrı bir Azure kaynak grubuna dağıtılır. Kaynak grubu, hizmet sağlayıcısının aboneliğinde veya kiracının aboneliğinde dağıtılabilir ve sağlayıcı tarafından kiracı adına yönetilebilir. Kiracı deseni başına bağımsız uygulama en büyük kiracı yalıtımını sağlar, ancak birden çok kiracı arasında kaynakları paylaşma fırsatı olmadığından genellikle en pahalı uygulamadır.  Bu desen, daha karmaşık olabilecek ve daha az sayıda kiracıya dağıtılan uygulamalar için uygundur.  Bağımsız dağıtımlarda, uygulama her kiracı için diğer desenlere göre daha kolay özelleştirilebilir.  

GitHub [.../Microsoft/WingtipTicketsSaaS-StandaloneApp'taki][github-code-for-wingtip-sa] [öğreticilere][docs-tutorials-for-wingtip-sa] ve kodlara göz atın.

## <a name="database-per-tenant-pattern"></a>Kiracı deseni başına veritabanı

Kiracı deseni başına veritabanı, kiracı yalıtımı ile ilgili olan ve paylaşılan kaynakların maliyet etkin kullanımına olanak tanıyan merkezi leştirilmiş bir hizmeti çalıştırmak isteyen hizmet sağlayıcılar için etkilidir. Her mekan veya kiracı için bir veritabanı oluşturulur ve tüm veritabanları merkezi olarak yönetilir. Veritabanları, kiracıların öngörülemeyen iş yükü desenlerinden yararlanan uygun maliyetli ve kolay performans yönetimi sağlamak için elastik havuzlarda barındırılabilir. Katalog veritabanı, kiracılar ve veritabanları arasındaki eşlemi tutar. Bu eşleme, uygulamaya verimli bağlantı yönetimi sağlayan [Elastik Veritabanı İstemci Kitaplığı'nın](sql-database-elastic-database-client-library.md)parçalı harita yönetimi özellikleri kullanılarak yönetilir.

GitHub [.../Microsoft/WingtipTicketsSaaS-DbPerTenant'daki][github-code-for-wingtip-dpt] [öğreticilere][docs-tutorials-for-wingtip-dpt] ve kodlara göz atın.

## <a name="sharded-multi-tenant-database-pattern"></a>Kırık çok kiracıveritabanı deseni

Çok kiracılı veritabanları, kiracı başına daha düşük maliyet arayan hizmet sağlayıcılar için etkilidir ve kiracı yalıtımı azalır. Bu desen, çok sayıda kiracının tek bir veritabanına paketlenmesine ve kiracı başına maliyeti düşürmesine olanak tanır. Sonsuz ölçek lere yakın, kiracıları birden çok veritabanlarıarasında parçalayarak mümkündür. Katalog veritabanı, kiracıları veritabanlarıyla eşler.  

Bu desen, veritabanında birden çok kiracıyla maliyet için optimizasyon yapabileceğiniz veya kendi veritabanında tek bir kiracıyla yalıtım için optimize edebileceğiniz *karma* bir modelde de izin verir. Seçim, kiracı nın sağlanması veya daha sonra, uygulama üzerinde hiçbir etkisi olmadan, kiracı tarafından kiracı bazında yapılabilir.  Bu model, kiracı gruplarına farklı davranılması gerektiğinde etkili bir şekilde kullanılabilir. Örneğin, düşük maliyetli kiracılar paylaşılan veritabanlarına atanabilirken, premium kiracılar kendi veritabanlarına atanabilir. 

GitHub [.../Microsoft/WingtipTicketsSaaS-MultiTenantDb'daki][github-code-for-wingtip-mt] [öğreticilere][docs-tutorials-for-wingtip-mt] ve kodlara göz atın.

## <a name="next-steps"></a>Sonraki adımlar

#### <a name="conceptual-descriptions"></a>Kavramsal açıklamalar

- Uygulama kira desenleri daha ayrıntılı bir açıklama [Multi-kiracı SaaS veritabanı kira desenleri][saas-tenancy-app-design-patterns-md] mevcuttur

#### <a name="tutorials-and-code"></a>Öğreticiler ve kod

- Kiracı başına bağımsız uygulama:
    - [Bağımsız uygulama için Öğreticiler][docs-tutorials-for-wingtip-sa].
    - [Tek başına uygulama için kod, GitHub üzerinde][github-code-for-wingtip-sa].

- Kiracı başına veritabanı:
    - [Kiracı başına veritabanı için Öğreticiler][docs-tutorials-for-wingtip-dpt].
    - [GitHub'da kiracı başına veritabanı kodu.][github-code-for-wingtip-dpt]

- Kırık çok kiracı:
    - [Kırık çok kiracı için Öğreticiler][docs-tutorials-for-wingtip-mt].
    - [GitHub'da kırık çok kiracı için kod.][github-code-for-wingtip-mt]



<!-- Image references. -->

[image-three-tenancy-patterns]: media/saas-tenancy-welcome-wingtip-tickets-app/three-tenancy-patterns.png "Üç kira düzeni."

<!-- Docs.ms.com references. -->

[saas-tenancy-app-design-patterns-md]: saas-tenancy-app-design-patterns.md

<!-- WWWeb http references. -->

[docs-tutorials-for-wingtip-sa]: https://aka.ms/wingtipticketssaas-sa
[github-code-for-wingtip-sa]: https://github.com/Microsoft/WingtipTicketsSaaS-StandaloneApp

[docs-tutorials-for-wingtip-dpt]: https://aka.ms/wingtipticketssaas-dpt
[github-code-for-wingtip-dpt]: https://github.com/Microsoft/WingtipTicketsSaaS-DbPerTenant

[docs-tutorials-for-wingtip-mt]: https://aka.ms/wingtipticketssaas-mt
[github-code-for-wingtip-mt]: https://github.com/Microsoft/WingtipTicketsSaaS-MultiTenantDb

