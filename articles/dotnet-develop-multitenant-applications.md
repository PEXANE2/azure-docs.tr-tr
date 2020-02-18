---
title: Çok kiracılı web uygulaması kalıbı | Microsoft Docs
description: Azure 'da çok kiracılı bir Web uygulamasının nasıl uygulanacağını anlatan mimari genel bakışları ve tasarım düzenlerini bulun.
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
ms.openlocfilehash: d3e267eab056589ed38c436620dd0db185291da1
ms.sourcegitcommit: b8f2fee3b93436c44f021dff7abe28921da72a6d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2020
ms.locfileid: "77425910"
---
# <a name="multitenant-applications-in-azure"></a>Azure 'da çok kiracılı uygulamalar
Çok kiracılı bir uygulama, "ayrı kiracılardaki kullanıcıların uygulamayı kendi kendine yaptığı gibi görüntülemesine izin veren paylaşılan bir kaynaktır. Çok kiracılı bir uygulamaya kendisini hedefleyen tipik bir senaryo, farklı kiracılardan uygulamanın tüm kullanıcılarının Kullanıcı deneyimini özelleştirmek ve aksi takdirde aynı temel iş gereksinimlerine sahip olması olabilir. Office 365, Outlook.com ve visualstudio.com gibi büyük çok kiracılı uygulamalara örnek olarak verilebilir.

Bir uygulama sağlayıcısının perspektifinden, çok kiracılı avantajların avantajları çoğunlukla operasyonel ve maliyet verimliliği ile ilgilidir. Uygulamanızın bir sürümü birçok kiracının/müşterinin ihtiyaçlarını karşılayabilir, izleme, performans ayarlama, yazılım bakımı ve veri yedeklemeleri gibi sistem yönetimi görevlerinin birleştirmeye izin verir.

Aşağıda, bir sağlayıcının perspektifinden en önemli hedeflerin ve gereksinimlerin bir listesi verilmiştir.

* **Sağlama**: uygulama için yeni kiracılar sağlayabiliyor olmanız gerekir.  Çok sayıda kiracıya sahip çok müşterili uygulamalarda, genellikle self servis sağlamayı etkinleştirerek bu işlemin otomatikleştirilmesi gerekir.
* Bakım **: birden**çok kiracı tarafından kullanılırken uygulamayı yükseltebilmeniz ve başka bakım görevleri gerçekleştirmeniz gerekir.
* **İzleme**: herhangi bir sorunu belirlemek ve sorunlarını gidermek için her zaman uygulamayı izleyebilmelisiniz. Bu, her kiracının uygulamayı nasıl kullandığını izlemeyi içerir.

Uygun şekilde uygulanmış çok kiracılı bir uygulama, kullanıcılara aşağıdaki avantajları sağlar.

* **Yalıtım**: tek tek kiracıların etkinlikleri uygulamanın diğer kiracılar tarafından kullanımını etkilemez. Kiracılar birbirlerine ait verilere erişemez. Bu, uygulamanın dışlamalı olarak kullanıldığı halde kiracıya görünür.
* **Kullanılabilirlik**: tek kiracılar, UYGULAMANıN bir SLA 'da tanımlanan garantilerle sürekli kullanılabilir olmasını ister. Yine, diğer kiracıların etkinlikleri uygulamanın kullanılabilirliğini etkilememelidir.
* **Ölçeklenebilirlik**: uygulama ayrı kiracıların talebini karşılayacak şekilde ölçeklendirilir. Diğer kiracıların varlığı ve eylemleri uygulamanın performansını etkilememelidir.
* **Maliyetler**: çok kiracılı kaynak paylaşımını sağladığından, maliyetler ayrılmış, tek kiracılı bir uygulama çalıştırmaktan daha düşüktür.
* **Özelleştirme**. Uygulamayı, tek bir kiracının özellik ekleme veya kaldırma, renkleri ve logoları değiştirme ya da kendi kodunu veya betiğini ekleme gibi çeşitli yollarla özelleştirebilme özelliği.

Kısaca, yüksek düzeyde ölçeklenebilir bir hizmet sağlamak için göz önünde bulundurmanız gereken birçok önemli işlem olmakla kalmaz, çok sayıda çok kiracılı uygulamalarda ortak olan birçok hedef ve gereksinim de vardır. Bazıları belirli senaryolarla ilgili olmayabilir ve tek tek hedeflerin ve gereksinimlerin önemi her senaryoya göre farklılık gösterir. Çok müşterili uygulamanın bir sağlayıcısı olarak, kiracının amaçlarını ve gereksinimlerini, kârlılığı, faturalandırmayı, birden çok hizmet düzeyini, sağlamayı, bakım izlemeyi ve Otomasyonu toplantısı gibi hedefler ve gereksinimler de vardır.

Çok kiracılı bir uygulamayla ilgili ek tasarım konuları hakkında daha fazla bilgi için bkz. [Azure 'Da çok kiracılı bir uygulama barındırma][Hosting a Multi-Tenant Application on Azure]. Çok kiracılı hizmet olarak yazılım (SaaS) veritabanı uygulamalarının ortak veri mimarisi düzenlerine ilişkin bilgi için bkz. [Azure SQL Veritabanı ile Çok Kiracılı SaaS Uygulamaları için Tasarım Düzenleri](sql-database/sql-database-design-patterns-multi-tenancy-saas-applications.md). 

Azure, çok kiracılı bir sistem tasarlarken karşılaşılan önemli sorunları ele almak için birçok özellik sağlar.

**Yalıtımı**

* SSL iletişimi olan veya olmayan konak üstbilgileri ile Web sitesi kiracılarını segment
* Sorgu parametrelerine göre segment Web sitesi kiracılar
* Çalışan rollerinde Web Hizmetleri
  * Genellikle bir uygulamanın arka ucunda verileri işleyen çalışan rolleri.
  * Genellikle uygulamalar için ön uç olarak davranan Web rolleri.

**Depolama**

Azure SQL veritabanı gibi veri yönetimi veya tablo hizmeti gibi Azure depolama hizmetleri, büyük miktarlarda yapılandırılmamış verileri depolama hizmetleri ve BLOB hizmeti gibi çok sayıda yapılandırılmamış metin depolamak için hizmetler sağlar video, ses ve görüntü gibi ikili veriler.

* SQL veritabanı kiracı başına SQL Server oturum açma işlemleri için çok kiracılı verilerin güvenliğini sağlama.
* Bir kapsayıcı düzeyi erişim ilkesi belirterek uygulama kaynakları için Azure tabloları 'nı kullanarak, paylaşılan erişim imzaları ile korunan kaynaklar için yeni URL 'ler vermek zorunda kalmadan izinleri ayarlama olanağına sahip olabilirsiniz.
* Uygulama kaynakları için Azure Kuyrukları Azure kuyrukları, kiracıların adına işlem yapmak için yaygın olarak kullanılır, ancak sağlama veya yönetim için gereken işleri dağıtmak için de kullanılabilir.
* İşleri paylaşılan bir hizmete ileten uygulama kaynakları için kuyruklar Service Bus, her bir kiracı göndericisinin yalnızca, yalnızca hizmetten gelen alıcıların bu kuyruğa göndermeleri durumunda (ACS 'den çıkarılan taleplerden türetilen) yalnızca izinlere sahip olduğu tek bir kuyruk kullanabilirsiniz. birden çok kiracıdan gelen verileri kuyruktan çekme iznidir.

**Bağlantı ve güvenlik hizmetleri**

* Azure Service Bus, uygulamalar arasında yer alan bir mesajlaşma altyapısı, gelişmiş ölçek ve esneklik için gevşek olarak bağlanmış bir şekilde ileti alışverişi yapmasına olanak tanır.

**Ağ Hizmetleri**

Azure, kimlik doğrulamasını destekleyen birkaç ağ hizmeti sağlar ve barındırılan uygulamalarınızın yönetilebilirliğini geliştirir. Bu hizmetler şunları içerir:

* Azure sanal ağ, Azure 'da sanal özel ağları (VPN 'Ler) sağlamanıza ve yönetmenize olanak sağlar ve bunları şirket içi BT altyapısına güvenle bağlayın.
* Sanal ağ Traffic Manager, aynı veri merkezinde veya dünyanın dört bir yanındaki farklı veri merkezlerinde çalışıyor olsun, birden çok barındırılan Azure hizmeti arasında gelen trafiğin yükünü dengelemenize olanak tanır.
* Azure Active Directory (Azure AD), bulut uygulamalarınız için kimlik yönetimi ve erişim denetimi özellikleri sağlayan modern, REST tabanlı bir hizmettir. Uygulama kaynakları için Azure AD 'nin kullanılması, kimlik doğrulama ve yetkilendirme özelliklerinin kodunuzun dışına yayılmasına izin verirken Web uygulamalarınıza ve hizmetlerinize erişim kazanmak için kolay bir şekilde kimlik doğrulaması ve yetkilendirme yapmanızı sağlar.
* Azure Service Bus, Azure 'da barındırılan uygulamalar ve şirket içi uygulamalar ile hizmetler arasında, karmaşık güvenlik duvarı ve güvenlik gerektirmeden iletişim gibi, dağıtılmış ve karma uygulamalar için güvenli bir mesajlaşma ve veri akışı özelliği sağlar altyapılarını. Uç noktalar olarak gösterilen hizmetlere erişmek için uygulama kaynakları için Service Bus geçişi kullanmak, kiracıya ait olabilir (örneğin, şirket içi gibi sistem dışında barındırılan) veya bunlar özel olarak kiracı için sağlanmış hizmetler olabilir (çünkü hassas, kiracıya özgü veriler bunlar arasında dolaşır).

**Kaynakları sağlama**

Azure, uygulama için yeni kiracılar sağlamaya yönelik çeşitli yollar sağlar. Çok sayıda kiracıya sahip çok müşterili uygulamalarda, genellikle self servis sağlamayı etkinleştirerek bu işlemin otomatikleştirilmesi gerekir.

* Çalışan rolleri, kiracı kaynaklarını (örneğin, yeni bir kiracı kaydolduğunda veya iptal edildiğinde) sağlar, ölçüm kullanımı için ölçümleri toplar ve belirli bir zamanlamaya göre veya önemli performans eşiklerinin kesişmesine yanıt olarak ölçeklendirmeyi yönetebilir göstergelerinin. Bu rol, çözüme güncelleştirme ve yükseltmeleri göndermek için de kullanılabilir.
* Azure Blob 'Ları, işlem hizmeti paketlerini, VHD görüntülerini ve diğer kaynakları korumak için kapsayıcı düzeyinde erişim ilkeleri sağlarken yeni kiracılar için işlem veya önceden başlatılmış depolama kaynakları sağlamak üzere kullanılabilir.
* Bir kiracı için SQL veritabanı kaynaklarını sağlama seçenekleri şunlardır:
  
  * Betikler içindeki DDL 'ler veya derlemeler içinde kaynak olarak katıştırılmış.
  * SQL Server 2008 R2 DAC paketleri programlı olarak dağıtıldı.
  * Ana başvuru veritabanından kopyalama.
  * Bir dosyadan yeni veritabanları sağlamak için veritabanı Içeri ve dışarı aktarma kullanma.

<!--links-->

[Hosting a Multi-Tenant Application on Azure]: https://msdn.microsoft.com/library/hh534480.aspx
[Designing Multitenant Applications on Azure]: https://msdn.microsoft.com/library/windowsazure/hh689716
