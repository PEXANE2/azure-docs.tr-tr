---
title: Veritabanı güvenliği - Azure Cosmos DB
description: Azure Cosmos DB'nin verileriniz için veritabanı koruması ve veri güvenliği sağladığını öğrenin.
author: markjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 03/10/2020
ms.author: mjbrown
ms.openlocfilehash: 345fc329df1c57cab7dd66c609bf3701fa3a6124
ms.sourcegitcommit: bc738d2986f9d9601921baf9dded778853489b16
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80619139"
---
# <a name="security-in-azure-cosmos-db---overview"></a>Azure Cosmos DB’de güvenlik - genel bakış

Bu makalede veritabanı ihlallerini önlemenize, algılamanıza ve yanıtlamanıza yardımcı olmak üzere Azure Cosmos DB tarafından sunulan veritabanı güvenliği en iyi yöntemleri ve önemli özellikleri açıklanır.

## <a name="whats-new-in-azure-cosmos-db-security"></a>Azure Cosmos DB güvenliğindeki yenilikler

Artık tüm Azure bölgelerinde Azure Cosmos DB'de depolanan belgeler ve yedeklemeler için artık istirahatte şifreleme kullanılabilir. Bu bölgelerdeki hem yeni hem de mevcut müşteriler için istirahatte şifreleme otomatik olarak uygulanır. Bir şey yapılandırmak için gerek yoktur; ve verilerinizin güvenli ve emniyetli olduğunu ve hazır da şifrenizi bilmenin avantajıyla, önceki gibi aynı büyük gecikme, iş kaynağı, kullanılabilirlik ve işlevselliği elde edeyim.

## <a name="how-do-i-secure-my-database"></a>Veritabanımı nasıl güvenli hale alarım?

Veri güvenliği, siz, müşteri ve veritabanı sağlayıcınız arasında paylaşılan bir sorumluluktur. Seçtiğiniz veritabanı sağlayıcısına bağlı olarak, taşıdığınız sorumluluk miktarı değişebilir. Şirket içi bir çözüm seçerseniz, son nokta korumasından donanımınızın fiziksel güvenliğine kadar her şeyi sağlamanız gerekir ki bu kolay bir iş değildir. Azure Cosmos DB gibi bir PaaS bulut veritabanı sağlayıcısı seçerseniz, ilgi alanınız önemli ölçüde küçülür. Microsoft'un Cloud Computing için [Paylaşılan Sorumlulukları](https://aka.ms/sharedresponsibility) teknik incelemesinden ödünç alınan aşağıdaki resim, Azure Cosmos DB gibi bir PaaS sağlayıcısıyla sorumluluğunuzun nasıl azaldığını gösterir.

![Müşteri ve veritabanı sağlayıcısı sorumlulukları](./media/database-security/nosql-database-security-responsibilities.png)

Önceki diyagram üst düzey bulut güvenlik bileşenlerini gösterir, ancak veritabanı çözümünüz için özel olarak hangi öğeler hakkında endişelenmeniz gerekir? Ve nasıl birbirleri ile çözümleri karşılaştırabilirsiniz?

Veritabanı sistemlerini karşılaştırmak için aşağıdaki gereksinimlerin denetim listesini öneririz:

- Ağ güvenliği ve güvenlik duvarı ayarları
- Kullanıcı kimlik doğrulaması ve ince taneli kullanıcı denetimleri
- Bölgesel hatalar için verileri küresel olarak çoğaltma becerisi
- Bir veri merkezinden diğerine başarısız olma becerisi
- Veri merkezi içinde yerel veri çoğaltma
- Otomatik veri yedeklemeleri
- Yedeklemelerden silinen verilerin geri yüklemesi
- Hassas verileri koruma ve yalıtma
- Saldırıların izlenmesi
- Saldırılara yanıt verme
- Veri yönetimi kısıtlamalarına uymak için verileri coğrafi olarak çitleme yeteneği
- Korumalı veri merkezlerinde sunucuların fiziksel koruması
- Sertifikalar

Ve her ne kadar açık gibi görünse [de,](https://thehackernews.com/2017/01/mongodb-database-security.html) son büyük ölçekli veritabanı ihlalleri aşağıdaki gereksinimlerin basit ama kritik önemini bize hatırlatıyor:

- Güncel tutulan yamalı sunucular
- Varsayılan/TLS şifrelemesi ile HTTPS
- Güçlü parolalara sahip yönetim hesapları

## <a name="how-does-azure-cosmos-db-secure-my-database"></a>Azure Cosmos DB veritabanımı nasıl güvence altına alar

Önceki listeye geri bakalım - Azure Cosmos DB bu güvenlik gereksinimlerinden kaç tanesini sağlar? Her birini.

Her birini ayrıntılı olarak inceleyelim.

|Güvenlik gereksinimi|Azure Cosmos DB'nin güvenlik yaklaşımı|
|---|---|
|Ağ güvenliği|IP güvenlik duvarı kullanmak, veritabanınızı güvenli hale getirmek için ilk koruma katmanıdır. Azure Cosmos DB, gelen güvenlik duvarı desteği için ilke odaklı IP tabanlı erişim denetimlerini destekler. IP tabanlı erişim denetimleri geleneksel veritabanı sistemleri tarafından kullanılan güvenlik duvarı kurallarına benzer, ancak Azure Cosmos veritabanı hesabına yalnızca onaylanmış bir makine kümesinden veya bulut hizmetleriyle erişilebilmeleri için genişletilir. [Azure Cosmos DB güvenlik duvarı destek](firewall-support.md) makalesinde daha fazla bilgi edinin.<br><br>Azure Cosmos DB, belirli bir IP adresini (168.61.48.0), ip aralığını (168.61.48.0/8) ve IP ve aralıkkombinasyonlarını etkinleştirmenizi sağlar. <br><br>Bu izin verilen listenin dışındaki makinelerden kaynaklanan tüm istekler Azure Cosmos DB tarafından engellenir. Onaylanan makinelerden ve bulut hizmetlerinden gelen istekler, kaynaklara erişim denetimi verilabilmesi için kimlik doğrulama işlemini tamamlamalıdır.<br><br> Ağ yalıtımı elde etmek ve Azure Cosmos DB kaynaklarınızı genel Internet'ten korumak için [sanal ağ hizmeti etiketlerini](../virtual-network/service-tags-overview.md) kullanabilirsiniz. Güvenlik kuralları oluştururken belirli IP adresleri yerine hizmet etiketlerini kullanın. Bir kuralın uygun kaynak veya hedef alanında hizmet etiketi adını (örneğin AzureCosmosDB) belirterek, ilgili hizmetin trafiğine izin verebilir veya reddedebilirsiniz.|
|Yetkilendirme|Azure Cosmos DB, yetkilendirme için karma tabanlı ileti kimlik doğrulama kodu (HMAC) kullanır. <br><br>Her istek gizli hesap anahtarı kullanılarak karma hale edilir ve sonraki base-64 kodlanmış karma her çağrıyla birlikte Azure Cosmos DB'ye gönderilir. İsteği doğrulamak için Azure Cosmos DB hizmeti karma oluşturmak için doğru gizli anahtarı ve özellikleri kullanır, ardından değeri istekteki yle karşılaştırır. İki değer eşleşirse, işlem başarıyla yetkilendirilir ve istek işlenir, aksi takdirde bir yetkilendirme hatası olur ve istek reddedilir.<br><br>[Bir ana anahtar](secure-access-to-data.md#master-keys)veya belge gibi bir kaynağa ince taneli erişimsağlayan bir kaynak [belirteci](secure-access-to-data.md#resource-tokens) kullanabilirsiniz.<br><br>[Azure Cosmos DB kaynaklarına erişimi güvence altına alma](secure-access-to-data.md)hakkında daha fazla bilgi edinin.|
|Kullanıcılar ve izinler|Hesabın ana anahtarını kullanarak, veritabanı başına kullanıcı kaynakları ve izin kaynakları oluşturabilirsiniz. Kaynak belirteci veritabanındaki bir izinle ilişkilidir ve kullanıcının veritabanındaki bir uygulama kaynağına erişimi olup olmadığını (okuma-yazma, salt okuma veya erişim yok) belirler. Uygulama kaynakları kapsayıcı, belgeler, ekler, depolanan yordamlar, tetikleyiciler ve UDF'leri içerir. Kaynak belirteci daha sonra kimlik doğrulama sırasında kaynağa erişimi sağlamak veya reddetmek için kullanılır.<br><br>[Azure Cosmos DB kaynaklarına erişimi güvence altına alma](secure-access-to-data.md)hakkında daha fazla bilgi edinin.|
|Etkin dizin tümleştirmesi (RBAC)| Azure portalındaki Access denetimi (IAM) kullanarak Cosmos hesabına, veritabanına, kapsayıcıya ve tekliflere (iş tünemi) erişimi sağlayabilir veya kısıtlayabilirsiniz. IAM rol tabanlı erişim denetimi sağlar ve Active Directory ile tümleştirir. Bireyler ve gruplar için yerleşik rolleri veya özel rolleri kullanabilirsiniz. Daha fazla bilgi için [Active Directory tümleştirme](role-based-access-control.md) makalesine bakın.|
|Genel çoğaltma|Azure Cosmos DB, tek bir düğmeyi tıklatarak verilerinizi Azure'un dünya çapındaki veri merkezlerinden herhangi birine çoğaltmanızı sağlayan anahtar teslim küresel dağıtım sunar. Genel çoğaltma, tüm dünyada küresel olarak ölçeklendirmenize ve verilerinize düşük gecikmeli erişim sağlamanıza olanak tanır.<br><br>Güvenlik bağlamında, genel çoğaltma bölgesel hatalara karşı veri koruması sağlar.<br><br>[Verileri küresel olarak dağıtma](distribute-data-globally.md) bölümünde daha fazlasını öğrenin.|
|Bölgesel yük devretme|Verilerinizi birden fazla veri merkezinde çoğalttınızsa, bölgesel bir veri merkezi çevrimdışı olursa Azure Cosmos DB işlemlerinizi otomatik olarak devraşar. Verilerinizin çoğaltıldığı bölgeleri kullanarak öncelikle başarısız bölgelerin listesini oluşturabilirsiniz. <br><br>[Azure Cosmos DB'deki Bölgesel Arızalar hakkında](high-availability.md)daha fazla bilgi edinin.|
|Yerel çoğaltma|Azure Cosmos DB, tek bir veri merkezi içinde bile yüksek kullanılabilirlik için verileri otomatik olarak çoğaltır ve size [tutarlılık düzeyleri](consistency-levels.md)seçeneği sunar. Bu çoğaltma, tüm tek bölge hesapları ve tüm çok bölgeli hesaplar için rahat tutarlılığa sahip %99,99 [kullanılabilirlik SLA'sını](https://azure.microsoft.com/support/legal/sla/cosmos-db) ve tüm çok bölgeli veritabanı hesaplarında %99,999 okuma kullanılabilirliğini garanti eder.|
|Otomatik çevrimiçi yedeklemeler|Azure Cosmos veritabanları düzenli olarak yedeklenir ve coğrafi yedek deposunda depolanır. <br><br>[Azure Cosmos DB ile Otomatik çevrimiçi yedekleme ve geri yükleme](../synapse-analytics/sql-data-warehouse/backup-and-restore.md)hakkında daha fazla bilgi edinin.|
|Silinen verileri geri yükleme|Otomatik çevrimiçi yedeklemeler, olaydan 30 gün sonrasına kadar yanlışlıkla silmiş olabileceğiniz verileri kurtarmak için kullanılabilir. <br><br>[Azure Cosmos DB ile Otomatik çevrimiçi yedekleme ve geri yükleme hakkında](../synapse-analytics/sql-data-warehouse/backup-and-restore.md) daha fazla bilgi edinin|
|Hassas verileri koruma ve yalıtma|Yeniliklerde listelenen bölgelerdeki tüm veriler? şimdi istirahatte şifrelenir.<br><br>Kişisel veriler ve diğer gizli veriler belirli bir kapsayıcıya yalıtılabilir ve okuma-yazma veya salt okunur erişim belirli kullanıcılarla sınırlı olabilir.|
|Saldırılar için monitör|Denetim [günlüğü ve etkinlik günlüklerini](logging.md)kullanarak, normal ve anormal etkinlik için hesabınızı izleyebilirsiniz. Kaynaklarınızda hangi işlemlerin gerçekleştirilmediğini, işlemi kimin başlattığını, işlemin ne zaman gerçekleştiğini, işlemin durumunu ve bu tabloyu izleyen ekran görüntüsünde gösterildiği gibi çok daha fazlasını görüntüleyebilirsiniz.|
|Saldırılara yanıt verme|Olası bir saldırıyı bildirmek için Azure desteğiyle iletişime geçtikten sonra, 5 adımlık bir olay yanıt süreci başlatıldı. 5 adımlı işlemin amacı, bir sorun algılandıktan ve bir soruşturma başlatıldıktan sonra normal hizmet güvenliğini ve işlemlerini mümkün olan en kısa sürede geri yüklemektir.<br><br>[Bulutta Microsoft Azure Güvenlik Yanıtı](https://aka.ms/securityresponsepaper)hakkında daha fazla bilgi edinin.|
|Geo-eskrim|Azure Cosmos DB, egemen bölgeler için veri yönetimi sağlar (örneğin, Almanya, Çin, ABD Gov).|
|Korumalı tesisler|Azure Cosmos DB'deki veriler Azure'un korumalı veri merkezlerinde SSD'lerde depolanır.<br><br>[Microsoft küresel veri merkezlerinde](https://www.microsoft.com/en-us/cloud-platform/global-datacenters) daha fazla bilgi edinin|
|HTTPS/SSL/TLS şifreleme|Azure Cosmos DB'ye tüm bağlantılar HTTPS'yi destekler. Azure Cosmos DB, TLS 1.2'yi de destekler.<br>En az TLS sürümü sunucu tarafı zorlamak mümkündür. Bunu yapmak için [azurecosmosdbtls@service.microsoft.com](mailto:azurecosmosdbtls@service.microsoft.com)lütfen .|
|Bekleme sırasında şifreleme|Azure Cosmos DB'de depolanan tüm veriler istirahatte şifrelenir. [Azure Cosmos DB şifreleme](./database-encryption-at-rest.md) hakkında daha fazla bilgi edinin|
|Yamalı sunucular|Azure Cosmos DB, yönetilen bir veritabanı olarak, sunucuları otomatik olarak sizin için yapılan sunucuları yönetme ve yama gereksinimini ortadan kaldırır.|
|Güçlü parolalara sahip yönetim hesapları|Bu gereksinimden bahsetmemiz gerektiğine inanmak bile zor, ancak bazı rakiplerimizin aksine, Azure Cosmos DB'de şifresi olmayan bir idari hesaba sahip olmak imkansızdır.<br><br> TLS ve HMAC gizli tabanlı kimlik doğrulama sıyrık üzerinden güvenlik varsayılan olarak pişirilir.|
|Güvenlik ve veri koruma sertifikaları| En güncel sertifikalar listesi için genel Azure [Uyumluluk sitesine](https://www.microsoft.com/en-us/trustcenter/compliance/complianceofferings) ve tüm sertifikalara sahip en son [Azure Uyumluluk Belgesi'ne](https://gallery.technet.microsoft.com/Overview-of-Azure-c1be3942) bakın (Cosmos'u arayın). Daha odaklı bir okuma için 25 Nisan 2018 gönderisine göz atın [Azure #CosmosDB: Güvenli, özel, SOCS 1/2 Type 2, HITRUST, PCI DSS Level 1, ISO 27001, HIPAA, FedRAMP High ve daha birçok larını içeren uyumlu.

Aşağıdaki ekran görüntüsü, hesabınızı izlemek için denetim günlüğünü ve ![etkinlik günlüklerini nasıl kullanabileceğinizi gösterir: Azure Cosmos DB için etkinlik günlükleri](./media/database-security/nosql-database-security-application-logging.png)

## <a name="next-steps"></a>Sonraki adımlar

Ana anahtarlar ve kaynak belirteçleri hakkında daha fazla bilgi için [bkz.](secure-access-to-data.md)

Denetim günlüğü hakkında daha fazla bilgi için [Azure Cosmos DB tanı günlüğü'ne](logging.md)bakın.

Microsoft sertifikaları hakkında daha fazla bilgi için [Azure Güven Merkezi'ne](https://azure.microsoft.com/support/trust-center/)bakın.