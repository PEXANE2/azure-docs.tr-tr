---
title: Çok Kiracılı Web Uygulama Deseni | Microsoft Dokümanlar
description: Azure'da çok kiracılı bir web uygulamasının nasıl uygulanacağını açıklayan mimari genel bakışları ve tasarım desenlerini bulun.
services: ''
documentationcenter: .net
author: wadepickett
manager: wpickett
editor: ''
ms.assetid: 4f0281d2-1555-42b0-a99d-1222fade0b0f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 06/05/2015
ms.author: wpickett
ms.openlocfilehash: d1441ede9f448b3e6ffb0726c2ee92f192369e9a
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81481844"
---
# <a name="multitenant-applications-in-azure"></a>Azure’da Çok Müşterili Uygulamalar
Çok kiracılı uygulama, "ayrı kiracılardaki kullanıcıların" uygulamayı kendilerine aitmiş gibi görüntülemesine olanak tanıyan paylaşılan bir kaynaktır. Çok kiracılı bir uygulamaya kendini ödünç tipik bir senaryo, farklı kiracılardan gelen uygulamanın tüm kullanıcılarının kullanıcı deneyimini özelleştirmek isteyebileceği, ancak aksi takdirde aynı temel iş gereksinimlerine sahip olduğu senaryodur. Büyük çok kiracılı uygulamalara örnek olarak Office 365, Outlook.com ve visualstudio.com verilebilir.

Bir uygulama sağlayıcısının bakış açısından, çoklu hizmetin yararları çoğunlukla operasyonel ve maliyet verimliliği ile ilgilidir. Uygulamanızın bir sürümü, izleme, performans atonlaması, yazılım bakımı ve veri yedekleme gibi sistem yönetimi görevlerinin konsolidasyonuna olanak sağlayarak birçok kiracının/müşterinin gereksinimlerini karşılayabilir.

Aşağıdaki, sağlayıcının bakış açısından en önemli hedeflerin ve gereksinimlerin bir listesini sağlar.

* **Sağlama**: Uygulama için yeni kiracılar temin edebilmeniz gerekir.  Çok sayıda kiracıya sahip çok kiracılı uygulamalarda, self servis sağlama sağlayarak bu işlemi otomatikleştirmek genellikle gereklidir.
* **Sürdürülebilirlik**: Uygulamayı yükseltebilmeli ve birden çok kiracı kullanırken diğer bakım görevlerini gerçekleştirebilmelisin.
* **İzleme**: Herhangi bir sorunu tespit etmek ve sorunları gidermek için uygulamayı her zaman izleyebilmelisiniz. Bu, her kiracının uygulamayı nasıl kullandığını izlemeyi içerir.

Düzgün bir şekilde uygulanan çok kiracılı bir uygulama, kullanıcılara aşağıdaki avantajları sağlar.

* **İzolasyon**: Bireysel kiracıların faaliyetleri, uygulamanın diğer kiracılar tarafından kullanılmasını etkilemez. Kiracılar birbirlerinin verilerine erişemez. Kiracıya, uygulamayı özel olarak kullanıyormuş gibi görünüyor.
* **Kullanılabilirlik**: Bireysel kiracılar uygulamanın sürekli olarak kullanılabilir olmasını isterler, belki de bir SLA'da tanımlanan garantilerle. Yine, diğer kiracıların faaliyetleri uygulamanın kullanılabilirliğini etkilememelidir.
* **Ölçeklenebilirlik**: Bireysel kiracıların talebini karşılamak için uygulama ölçekler. Diğer kiracıların varlığı ve eylemleri uygulamanın performansını etkilememelidir.
* **Maliyetler**: Maliyetler, çok kiraya veda eden kaynakların paylaşılmasını sağladığından, özel, tek kiracılı bir uygulamayı çalıştırmaktan daha düşüktür.
* **Özelleştirilebilirlik**. Tek bir kiracı için uygulamayı, özellikleri ekleme veya kaldırma, renk ve logo değiştirme, hatta kendi kod larını veya komut dosyalarını ekleme gibi çeşitli şekillerde özelleştirme olanağı.

Kısacası, yüksek ölçeklenebilir bir hizmet sağlamak için dikkate almalısınız birçok hususlar olmakla birlikte, birçok çok kiracı uygulamaları için ortak olan hedefleri ve gereksinimleri bir dizi de vardır. Bazıları belirli senaryolarda alakalı olmayabilir ve her senaryoda tek tek hedeflerin ve gereksinimlerin önemi farklılık gösterir. Çok kiracılı uygulamanın sağlayıcısı olarak, kiracının amaç ve gereksinimlerini karşılama, karlılık, faturalandırma, birden çok hizmet düzeyi, sağlama, bakım denetimi ve otomasyon gibi hedefleriniz ve gereksinimleriniz de olacaktır.

Çok kiracılı bir uygulamanın ek tasarım konuları hakkında daha fazla bilgi için [Azure'da Çok Kiracılı Uygulama Barındırma][Hosting a Multi-Tenant Application on Azure]bölümüne bakın. Çok kiracılı hizmet olarak yazılım (SaaS) veritabanı uygulamalarının ortak veri mimarisi düzenlerine ilişkin bilgi için bkz. [Azure SQL Veritabanı ile Çok Kiracılı SaaS Uygulamaları için Tasarım Düzenleri](sql-database/sql-database-design-patterns-multi-tenancy-saas-applications.md). 

Azure, çok kiracılı bir sistem tasarlarken karşılaşılan temel sorunları gidermenize olanak tanıyan birçok özellik sağlar.

**Yalıtım**

* TLS iletişimi olan veya olmayan Ev Sahibi Başlıklar tarafından Segment Web Sitesi Kiracıları
* Sorgu Parametrelerine Göre Web Sitesi Kiracılarını Segment
* İşçi Rollerinde Web Hizmetleri
  * Genellikle bir uygulamanın arka ucundaki verileri işleyen Alt Roller.
  * Genellikle uygulamalar için ön uç görevi yapan Web Rolleri.

**Depolama**

Azure SQL Veritabanı veya Azure Depolama hizmetleri gibi veri yönetimi, büyük miktarlarda yapılandırılmamış verilerin depolanması için hizmetler sağlayan Tablo hizmeti ve video, ses ve görüntüler gibi büyük miktarlarda yapılandırılmamış metin veya ikili veri depolamak için hizmetler sağlayan Blob hizmeti.

* SQL Veritabanı'nda kiracı başına SQL Server oturum açmalarında Çok Kiracılı Verilerin Güvenliğini Sağlama.
* Kapsayıcı düzeyi erişim ilkesi ni belirterek Uygulama Kaynakları için Azure Tabloları'nı kullanarak, paylaşılan erişim imzalarıyla korunan kaynaklar için yeni URL'ler vermek zorunda kalmadan izinleri ayarlama olanağınız olabilir.
* Uygulama Kaynakları için Azure Kuyrukları Azure kuyrukları genellikle kiracılar adına işlemeyi yönlendirmek için kullanılır, ancak sağlama veya yönetim için gereken işleri dağıtmak için de kullanılabilir.
* İşi paylaşılan bir hizmete iten Uygulama Kaynakları Için Hizmet Veri Hizmetleri Kuyrukları, her kiracının yalnızca bu kuyruğa itmek için yalnızca izinleri (ACS'den alınan taleplerden türetildiği gibi) sahip olduğu tek bir sıra kullanabilir, yalnızca hizmetteki alıcılar birden çok kiracıdan gelen verileri kuyruktan çekme iznine sahiptir.

**Bağlantı ve Güvenlik Hizmetleri**

* Azure Hizmet Veri Yolu, gelişmiş ölçek ve esneklik için ileti alışverişini gevşek bir şekilde değiştirmelerine olanak tanıyan uygulamalar arasında yer alan bir mesajlaşma altyapısıdır.

**Ağ Hizmetleri**

Azure, kimlik doğrulamayı destekleyen ve barındırılan uygulamalarınızın yönetilebilirliğini artıran çeşitli ağ hizmetleri sağlar. Bu hizmetler şunlardır:

* Azure Sanal Ağı, Azure'da sanal özel ağları (VPN'ler) sağlamanızı ve yönetmenize ve bunları şirket içi BT altyapısına güvenli bir şekilde bağlamanıza olanak tanır.
* Sanal Ağ Trafik Yöneticisi, ister aynı veri merkezinde ister dünyanın farklı veri merkezlerinde çalışıyor olsunlar, birden çok barındırılan Azure hizmetiüzerinden bakiye yüklemenize olanak tanır.
* Azure Active Directory (Azure AD), bulut uygulamalarınız için kimlik yönetimi ve erişim denetimi özellikleri sağlayan modern, REST tabanlı bir hizmettir. Azure Uygulama Kaynakları Için Reklam'ı kullanmak, kimlik doğrulama ve yetkilendirme özelliklerinin kodunuzdan çıkarılabilmesine izin verirken, kullanıcıların web uygulamalarınıza ve hizmetlerinize erişmelerini doğrulamanın ve yetkilendirmenin kolay bir yolunu sağlar.
* Azure Hizmet Veri Servisi, karmaşık güvenlik duvarı ve güvenlik altyapılarına gerek kalmadan Azure barındırılan uygulamalar ve şirket içi uygulamalar ve hizmetler arasındaki iletişim gibi dağıtılmış ve karma uygulamalar için güvenli bir ileti ve veri akışı olanağı sağlar. Son nokta olarak ortaya çıkan hizmetlere erişmek için Uygulama Kaynakları için Hizmet Veri Bölesini kullanmak kiracıya ait olabilir (örneğin, şirket içi gibi sistemin dışında barındırılan) veya kiracı için özel olarak sağlanan hizmetler olabilir (çünkü hassas, kiracıya özel veriler bunlar arasında seyahat eder).

**Kaynakların Sağlanması**

Azure, uygulama için yeni kiracılar sağlamanın çeşitli yollarını sağlar. Çok sayıda kiracıya sahip çok kiracılı uygulamalarda, self servis sağlama sağlayarak bu işlemi otomatikleştirmek genellikle gereklidir.

* Çalışan rolleri, kiracı kaynakları (örneğin, yeni bir kiracı kaydolur veya iptal eder), ölçüm kullanımı için ölçümler toplamak ve belirli bir zamanlamayı izleyerek veya önemli performans göstergelerinin eşiklerinin aşilmesine yanıt olarak ölçeği yönetmenize olanak sağlar. Bu aynı rol, güncelleştirmeleri ve çözümleri yükseltmeleri dışarı itmek için de kullanılabilir.
* Azure Blob'lar, bilgi işlem hizmeti Paketlerini, VHD görüntülerini ve diğer kaynakları korumak için kapsayıcı düzeyinde erişim ilkeleri sağlarken, yeni kiracılar için bilgi işlem veya önceden önbaşlatmalı depolama kaynakları sağlamak için kullanılabilir.
* Bir kiracı için SQL Veritabanı kaynaklarını sağlama seçenekleri şunlardır:
  
  * DDL komut dosyaları veya derlemeler içinde kaynak olarak gömülü.
  * SQL Server 2008 R2 DAC Paketleri programlı olarak dağıtıldı.
  * Ana başvuru veritabanından kopyalama.
  * Bir dosyadan yeni veritabanları sağlamak için veritabanı Alma ve Dışa Aktarma kullanma.

<!--links-->

[Hosting a Multi-Tenant Application on Azure]: https://msdn.microsoft.com/library/hh534480.aspx
[Designing Multitenant Applications on Azure]: https://msdn.microsoft.com/library/windowsazure/hh689716
