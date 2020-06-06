---
title: Visual Studio App Center ve Azure hizmetleriyle bulutta uygulama verilerini depolayın, yönetin ve kalıcı hale getirin
description: Mobil uygulama verilerini bulutta depolamanıza, yönetmenize ve devam etmenize olanak sağlayan Visual Studio App Center gibi hizmetler hakkında bilgi edinin.
author: codemillmatt
ms.assetid: 12344321-0123-4678-8588-ccff02097224
ms.service: mobile-services
ms.topic: article
ms.date: 03/24/2020
ms.author: masoucou
ms.openlocfilehash: 724ad41bdafdba976939cbf9b7f88df2de6c6c18
ms.sourcegitcommit: 0a5bb9622ee6a20d96db07cc6dd45d8e23d5554a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84450671"
---
# <a name="store-sync-and-query-mobile-application-data-from-the-cloud"></a>Buluttan mobil uygulama verilerini depolayın, eşitleyin ve sorgulayın
Oluşturduğunuz uygulama türü ne olsun, büyük olasılıkla veri oluşturabilir ve işleyebilirsiniz. Uygulamanızın kullanıcılarınızın yüksek beklentileri vardır. Bunlar, uygulamanın her durumda hızlı ve sorunsuz çalışmasını ister. Çoğu uygulama birden çok cihazda da çalışır. Uygulamanıza bir masaüstü veya mobil cihazdan erişebilirsiniz. Birden çok kullanıcı aynı anda uygulamayı kullanabilir ve verilere anında ve gerçek zamanlı erişim elde etme beklentisiyle verileri paylaşabilir.

Uygulama kullanıcılarınız her zaman internet bağlantısına sahip olmaz. Uygulamalar, internet bağlantısı olmadan veya ile birlikte çalışmak üzere tasarlanmıştır ve beklenir. Geliştiriciler, uygulamaları için harika bir müşteri deneyimi sağlamak üzere verilerini buluta depolamak ve eşitlemek için doğru çözümü seçmelidir.

Microsoft, sunucuları alma, veritabanınızı seçme veya mümkün olduğunca zengin deneyim sağlamak için ölçek veya güvenlik konusunda endişelenmeye gerek duymanızı ortadan kaldıran çeşitli hizmetler sunar. Bu hizmetler, SQL veya NoSQL API 'Leri kullanarak uygulama verilerini bulutta depolamanıza olanak sağlayan harika bir geliştirici deneyimi sağlar. Ayrıca, tüm cihazlarda verileri eşitleyebilir ve ölçeklenebilir ve sağlam uygulamalar oluşturmaya yardımcı olmak için uygulamanın ağ bağlantısı olmadan veya olmadan çalışmasını sağlayabilirsiniz.

Mobil uygulama verilerini bulutta yönetmek ve depolamak için aşağıdaki hizmetleri kullanın.

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) , genel olarak dağıtılmış, çok modelli bir veritabanı hizmetidir. Bu uygulamayı, Planet ölçekli uygulamalar oluşturmak için kullanabilirsiniz. Azure Cosmos DB ile, dünyanın her yerindeki Azure bölgesinde işleme ve depolamayı esnek ve bağımsız olarak ölçeklendirebilirsiniz. En sevdiğiniz API yüzeylerini kullanarak hızlı, tek basamaklı milisaniyelik veri erişiminin avantajlarından yararlanabilirsiniz. Bu yüzeyler SQL, MongoDB, Cassandra, tablolar veya Gremlin içerir. Azure Cosmos DB, üretilen iş, gecikme süresi, kullanılabilirlik ve tutarlılık için kapsamlı hizmet düzeyi sözleşmeleri (SLA 'Lar) sağlar.

**Önemli özellikler**
- SQL (Core) API, Cassandra API, MongoDB API, Gremlin API ve Tablo API'si dahil olmak üzere çok çeşitli API 'Leri destekler.
- Anahtar teslim küresel dağıtım, kullanıcılarınızın nerede olduğu her yerde verilerinizi çoğaltır. Kullanıcılarınız, kendilerine en yakın verilerin bir çoğaltmayla etkileşime geçebilir.
- Veritabanı altyapısı tamamen şema belirsiz olduğundan şema veya dizin yönetimi yok.
- Ortak bulutta 54 + bölgelerini içeren dünya çapındaki tüm Azure bölgelerinde kullanılabilir Azure Cosmos DB için ubititous bölgesel varlık varlığı.
- Tam olarak tanımlanmış, Azure Cosmos DB çok yöneticili çoğaltma protokolü, iyi tanımlanmış beş tutarlılık seçeneği sunacak şekilde dikkatle tasarlandığından, birden çok tutarlılık seçimi. Bu beş seçenek güçlü, sınırlanmış Eskime durumu, oturum, tutarlı ön ek ve nihai.
- okuma ve yazma işlemleri için% 99,999 kullanılabilirlik.
- Program aracılığıyla (veya Azure portal aracılığıyla), uygulamanızın bölgesel bir olağanüstü duruma göre tasarlandığından emin olmak için Azure Cosmos DB hesabınızın bölgesel yük devretmesini çağırın.
- Dünya çapındaki 99. yüzdede garantili düşük gecikme süresi.

**Başvurular**
- [Azure portal](https://portal.azure.com) 
- [Azure Cosmos DB belgeleri](/azure/cosmos-db/introduction)

## <a name="azure-sql-database"></a>Azure SQL Veritabanı
 [Azure SQL veritabanı](https://azure.microsoft.com/services/sql-database/) , genel amaçlı ilişkisel veritabanı yönetimli bir hizmettir. Azure buluttaki uygulamalar ve çözümler için yüksek düzeyde kullanılabilir ve yüksek performanslı bir veri depolama katmanı oluşturmak için bu özelliği kullanabilirsiniz.

**Önemli özellikler**
- **Elastik veritabanı modelleri ve araçları:** Bir elastik veritabanı sayesinde geliştiriciler ölçekleme için bir veritabanı grubu arasında kaynakları havuza alabilir. Bu kaynakları yönetici yönetiminde, iş olarak bir komut dosyası gönderebilirsiniz. SQL veritabanı daha sonra betiği veritabanları genelinde gerçekleştirir.
- **Yüksek performans:** Yüksek performanslı uygulamalar en son sürümden faydalanabilir. Bu, %25 daha fazla Premium veritabanı gücü sağlar.
- **Yedeklemeler, çoğaltma ve yüksek kullanılabilirlik:** Yerleşik çoğaltma ve veritabanı düzeyinde Microsoft tarafından desteklenen bir SLA, uygulama sürekliliği ve olağanüstü olaylara karşı koruma sağlar. Etkin coğrafi çoğaltma, "kurtarma" üzerinde tam denetim sağlayan, yük devretme ve self servis geri yükleme yapılandırmanızı sağlar. Veri geri yükleme, 35 güne kadar olan veri yedeklemelerinden edinilebilir.
- **Neredeyse sıfır bakım:** Otomatik yazılım, hizmetin bir parçasıdır. Yerleşik sistem çoğaltmaları, devralınan veri koruma, veritabanı çalışma süresi ve sistem kararlılığını sunmaya yardımcı olur. Sistem çoğaltmaları otomatik olarak yeni bilgisayarlara taşınır. Bunlar, eski olanlar başarısız olduğu için anında sağlanır.
- **Güvenlik:** Azure SQL veritabanı, kurumsal veya sektör-uygulanan uyumluluk ilkelerini karşılamak için bir güvenlik özellikleri portföyü sunar:
    - Denetim, geliştiricilere uyumlulukla ilgili görevleri gerçekleştirme ve etkinlikler hakkında bilgi elde etme olanağı sunar.
    - Geliştiriciler ve, Azure SQL veritabanı için satır düzeyinde güvenlik, dinamik veri maskeleme ve saydam veri şifrelemesi ile hassas verilere erişimi sınırlamaya yardımcı olması için veritabanı düzeyinde ilkeler uygulayabilir.
    - Azure SQL veritabanı, HIPAA BAA, ISO/ıEC 27001:2005, Fedrampa ve AB model yan tümceleri gibi başlıca Azure uyumluluk sertifikaları ve onaylarının bir parçası olarak önemli bulut denetleyicileri tarafından doğrulanır.

**Başvurular**
- [Azure portal](https://portal.azure.com) 
- [Azure SQL Veritabanı belgeleri](/azure/sql-database/) 
