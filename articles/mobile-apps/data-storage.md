---
title: Visual Studio App Center ve Azure hizmetleriyle uygulama verilerini bulutta depolama, yönetme ve bu bilgilerle devam edin
description: Görsel Stüdyo Uygulama Merkezi gibi mobil uygulama verilerini bulutta depolamanıza, yönetmenize ve devam ettirebilmenize izin veren hizmetler hakkında bilgi edinin.
author: codemillmatt
ms.assetid: 12344321-0123-4678-8588-ccff02097224
ms.service: vs-appcenter
ms.topic: article
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: 166847325fa9094136f1c2a143f1751420f05f66
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80240906"
---
# <a name="store-sync-and-query-mobile-application-data-from-the-cloud"></a>Mobil uygulama verilerini buluttan depolama, eşitleme ve sorgula
Ne tür bir uygulama oluşturursanız oluşturun, büyük olasılıkla veri oluşturacak ve işleyeceksiniz. Uygulamanızın kullanıcılarının beklentileri yüksek. Uygulamanın her koşulda hızlı ve sorunsuz çalışmasını isterler. Çoğu uygulama birden çok aygıtta da çalışır. Uygulamanıza bir masaüstü veya mobil cihazdan erişebilirsiniz. Birden çok kullanıcı uygulamayı aynı anda kullanabilir ve verilere anında ve gerçek zamanlı erişim beklentisiyle verileri paylaşabilir.

Uygulama kullanıcılarınız her zaman internet bağlantısına sahip olmaz. Uygulamalar tasarlanmış ve internet bağlantısı ile veya olmadan çalışması bekleniyor. Geliştiriciler, uygulamaları için mükemmel bir müşteri deneyimi sağlamak için verilerini depolamak ve bulutla senkronize etmek için doğru çözümü seçmelidir.

Microsoft, mümkün olduğunca zengin bir deneyim sağlamak için sunucuları döndürme, veritabanınızı seçme veya ölçek veya güvenlik konusunda endişelenme ihtiyacını ortadan kaldıran çeşitli hizmetler sunar. Bu hizmetler, SQL veya NoSQL API'lerini kullanarak uygulama verilerini bulutta depolamanızı sağlayan harika bir geliştirici deneyimi sağlar. Ayrıca tüm aygıtlarda verileri senkronize edebilir ve ölçeklenebilir ve sağlam uygulamalar oluşturmaya yardımcı olmak için uygulamanın ağ bağlantısı yla veya ağ bağlantısı olmadan çalışmasını sağlayabilirsiniz.

Mobil uygulama verilerini bulutta yönetmek ve depolamak için aşağıdaki hizmetleri kullanın.

## <a name="visual-studio-app-center"></a>Visual Studio App Center
[App Center Data](/appcenter/data/) bir veri yönetimi hizmetidir. Uygulamaların çevrimiçi ve çevrimdışı senaryolarda farklı aygıtlar ve platformlar arasında buluttaki uygulama verilerini yönetmesini, devam etmesini ve eşitlemesini sağlar. Azure Cosmos DB'nin üzerine inşa edilen App Center Data, kullanıcı tabanınız ve uygulama sayısı arttıkça ölçeklendirilir. Tüm verileriniz için düşük gecikme, yüksek kullanılabilirlik ve yüksek ölçeklenebilirlik sağlar.

**Önemli özellikler**
- Visual Studio App Center portalından yeni bir Azure Cosmos DB veritabanına veya mevcut bir Azure Cosmos DB veritabanına kolay sağlama.
- Uygulama verilerini kolayca depolamak, eşitlemek ve sorgulamak için NoSQL veritabanı desteği.
- Azure Cosmos DB'nin üzerine inşa edilen bu hizmet, Azure Cosmos DB tarafından sunulan temel özelliklerin çoğunu devralır ve iş gereksinimlerinizi karşılamak için küresel olarak ölçeklenebilir.
- Verileri aygıtlar arasında eşitlemek için çevrimiçi ve çevrimdışı eşitleme yetenekleri.
- Özel uygulama verilerini kolayca yönetmek için kullanabileceğiniz mobil istemci SDK'ları.
- iOS, Android, Xamarin ve React Native için platform desteği.

**Başvurular**
- [Visual Studio App Center ile kaydolun](https://appcenter.ms/signup?utm_source=Mobile%20Development%20Docs&utm_medium=Azure&utm_campaign=New%20azure%20docs)
- [App Center Data ile başlayın](/appcenter/data/getting-started)

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure Cosmos DB,](https://azure.microsoft.com/services/cosmos-db/) dünya çapında dağıtılmış, çok modelli bir veritabanı hizmetidir. Gezegen ölçekli uygulamalar oluşturmak için kullanabilirsiniz. Azure Cosmos DB ile, dünya çapında ki birçok Azure bölgesinde elastik ve bağımsız olarak ölçeklendirebilirsiniz. En sevdiğiniz API yüzeylerini kullanarak hızlı, tek basamaklı milisaniyelik veri erişiminden yararlanabilirsiniz. Bu yüzeyler SQL, MongoDB, Cassandra, Tablolar veya Gremlin içerir. Azure Cosmos DB, iş ortalığı, gecikme süresi, kullanılabilirlik ve tutarlılık için kapsamlı hizmet düzeyi anlaşmaları (SLA'lar) benzersiz bir şekilde sağlar.

**Önemli özellikler**
- SQL (Core) API, Cassandra API, MongoDB API, Gremlin API ve Table API'yi içeren çok çeşitli API'leri destekler.
- Anahtar teslimi küresel dağıtım, kullanıcılarınızın olduğu her yerde verilerinizi çoğaltır. Kullanıcılarınız kendilerine en yakın verilerin bir kopyasıyla etkileşimkurabilir.
- Veritabanı altyapısı tamamen şema agnostik olduğu için şema veya dizin yönetimi yok.
- Azure Cosmos DB,genel bulutta 54'ten fazla bölgeyi içeren tüm Azure bölgelerinde kullanılabildiği için her yerde bölgesel varlık mevcuttur.
- Azure Cosmos DB'nin çok açılı çoğaltma protokolü iyi tanımlanmış beş tutarlılık seçeneği sunmak üzere özenle tasarlandığından, tam olarak tanımlanmış, birden çok tutarlılık seçeneği. Bu beş seçenek güçlü, sınırlı bayatlık, oturum, tutarlı önek ve nihai.
- Hem okuma hem de yazma için %99,999 kullanılabilirlik.
- Programlı olarak (veya Azure portalı üzerinden) Uygulamanızın bölgesel bir felakete dayanacak şekilde tasarlandığından emin olmak için Azure Cosmos DB hesabınızın bölgesel olarak başarısız olmasını sağlar.
- Dünya çapında yüzde 99'da düşük gecikme garantisi.

**Başvurular**
- [Azure portalında](https://portal.azure.com) 
- [Azure Cosmos DB belgeleri](/azure/cosmos-db/introduction)

## <a name="azure-sql-database"></a>Azure SQL Veritabanı
 [Azure SQL Veritabanı,](https://azure.microsoft.com/services/sql-database/) genel amaçlı ilişkisel veritabanı yönetilen bir hizmettir. Azure bulutundaki uygulamalar ve çözümler için yüksek kullanılabilir ve yüksek performanslı bir veri depolama katmanı oluşturmak için kullanabilirsiniz.

**Önemli özellikler**
- **Elastik veritabanı modelleri ve araçları:** Elastik bir veritabanı yla, geliştiriciler kaynakları ölçekleme için bir veritabanı grubu arasında bir araya getirebilir. Bu kaynakları yönetimsel olarak yönetmek için bir komut dosyası nı iş olarak gönderirsiniz. SQL veritabanı daha sonra komut dosyasını veritabanları arasında gerçekleştirir.
- **Yüksek performans:** Yüksek iş elde uygulamaları en son sürümden yararlanabilir. %25 daha fazla premium veritabanı gücü sağlar.
- **Yedeklemeler, çoğaltma ve yüksek kullanılabilirlik:** Yerleşik çoğaltma ve veritabanı düzeyinde Microsoft destekli SLA, uygulama sürekliliği ve yıkıcı olaylara karşı koruma sağlar. Etkin coğrafi çoğaltma, "oops kurtarma" üzerinde tam kontrol sağlayan failover ve self-servis geri yükleme yapılandırmanızı sağlar. Veri geri yüklemesi, 35 güne kadar olan veri yedeklemelerinden elde edilebilir.
- **Sıfıra yakın bakım:** Otomatik yazılım hizmetin bir parçasıdır. Yerleşik sistem yinelemeleri, doğal veri koruması, veritabanı çalışma süresi ve sistem kararlılığı sağlamaya yardımcı olur. Sistem yinelemeleri otomatik olarak yeni bilgisayarlara taşınır. Eskiler başarısız olduğu için anında temin edilirler.
- **Güvenlik:** Azure SQL Veritabanı, kuruluş veya endüstri tarafından zorunlu kılınan uyumluluk ilkelerini karşılamak için bir güvenlik özellikleri portföyü sunar:
    - Denetim, geliştiricilere uyumlulukla ilgili görevleri gerçekleştirme ve etkinlikler hakkında bilgi edinme olanağı sağlar.
    - Geliştiriciler ve BT, Azure SQL Veritabanı için satır düzeyinde güvenlik, dinamik veri maskeleme ve saydam veri şifrelemeile hassas verilere erişimi sınırlamaya yardımcı olmak için veritabanı düzeyinde ilkeler uygulayabilir.
    - Azure SQL Veritabanı, HIPAA BAA, ISO/IEC 27001:2005, FedRAMP ve AB Model Clauses gibi önemli Azure uyumluluk sertifikaları ve onaylarının kapsamının bir parçası olarak önemli bulut denetçileri tarafından doğrulanır.

**Başvurular**
- [Azure portalında](https://portal.azure.com) 
- [Azure SQL Veritabanı belgeleri](/azure/sql-database/) 
