---
title: Azure Cosmos DB'de bekleme sırasında şifreleme
description: Azure Cosmos DB, bekleyen verilerin şifrelenmesi nasıl sağladığını ve nasıl uygulandığını öğrenin.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.author: sngun
ms.custom: seodec18
ms.openlocfilehash: f406f008e2c377b39deb8d151855ce7315616701
ms.sourcegitcommit: e42c778d38fd623f2ff8850bb6b1718cdb37309f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2019
ms.locfileid: "69616863"
---
# <a name="data-encryption-in-azure-cosmos-db"></a>Azure Cosmos DB veri şifreleme 

Bekleyen şifreleme gibi katı hal sürücülerine (SSD'ler), kalıcı depolama cihazlarında verilerin şifrelenmesi için yaygın olarak başvuran bir ifade ve sabit disk sürücülerinin (HDD'ler) ' dir. Cosmos DB, SSD'ler üzerinde birincil veritabanlarında depolar. Yedeklemeler ve medya ekler, HDD'ler ile genel olarak yedeklendiğinden Azure Blob storage'da depolanır. Cosmos DB için bekleyen şifrelemenin'ın yayınlanmasıyla birlikte, tüm veritabanları, medya ekler ve yedeklemeleri şifrelenir. Verileriniz artık (ağ üzerinden) aktarım sırasında şifrelenir ve rest (kalıcı depolama) uçtan uca şifreleme sağlar.

Cosmos DB bir PaaS hizmeti kullanımı çok kolay olduğu gibi. Cosmos DB'de depolanan tüm kullanıcı verileri, aktarım ve bekleme sırasında şifrelenir olduğundan, herhangi bir eylemde bulunmanız gerekmez. Bu başka bir yolu olan bekleyen şifreleme varsayılan olarak "açık" olduğu. Açma veya kapatma için denetim yoktur. Azure Cosmos DB, hesabın çalıştığı tüm bölgelerde AES-256 şifrelemesini kullanır. Karşılamaya devam ederken bu özelliği sunuyoruz. bizim [kullanılabilirliğini ve performansını SLA'lar](https://azure.microsoft.com/support/legal/sla/cosmos-db).

## <a name="implementation-of-encryption-at-rest-for-azure-cosmos-db"></a>Azure Cosmos DB için bekleyen şifrelemenin uygulaması

Bekleme sırasında şifreleme birkaç güvenli anahtar depolama sistemleri, şifreli ağları ve şifreleme API'leri dahil güvenlik teknolojileri kullanılarak uygulanır. Anahtarları Yönet sistemleriyle iletişim şifresini çözmek ve verileri işleyen sistemleri sahip. Şifrelenmiş verilerin depolanması ve anahtar yönetimini nasıl ayrılmış diyagramda gösterilmektedir. 

![Tasarım diyagramı](./media/database-encryption-at-rest/design-diagram.png)

Bir kullanıcı isteği temel akışı aşağıdaki gibidir:
- Kullanıcı veritabanı hesabı hazır hale getirdik ve yönetim Service kaynak sağlayıcısı için bir istek ile depolama anahtarlarını alınır.
- Cosmos DB bağlantı HTTPS ve güvenli aktarımı ile bir kullanıcı oluşturur. (SDK'ları ayrıntılarını soyut.)
- Kullanıcı daha önce oluşturulmuş bir güvenli bağlantı üzerinden depolanması için bir JSON belgesi gönderir.
- Kullanıcının dizin oluşturmayı devre dışı bırakılmış sürece JSON belgesini dizine alınır.
- JSON belge ve dizin verileri güvenli depolama yazılır.
- Düzenli olarak, veriler güvenli depolama alanından okuyun ve Azure şifreli Blob Store için yedeklenen.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="q-how-much-more-does-azure-storage-cost-if-storage-service-encryption-is-enabled"></a>S: Depolama Hizmeti Şifrelemesi etkinleştirilmişse Azure depolama maliyeti ne kadar fazla olur?
Y: Ek bir maliyet yoktur.

### <a name="q-who-manages-the-encryption-keys"></a>S: Şifreleme anahtarlarını kim yönetir?
Y: Anahtarlar Microsoft tarafından yönetilir.

### <a name="q-how-often-are-encryption-keys-rotated"></a>S: Şifreleme anahtarları ne sıklıkta döndürülür?
Y: Microsoft 'un Cosmos DB şifreleme anahtarı döndürme için bir iç kılavuz kümesi vardır. Özel yönergeleri yayımlanmaz. Microsoft, yayımlama [Security Development Lifecycle (SDL)](https://www.microsoft.com/sdl/default.aspx), iç Kılavuzu kümesini görülür ve geliştiriciler için yararlı en iyi yöntemler vardır.

### <a name="q-can-i-use-my-own-encryption-keys"></a>S: Kendi şifreleme anahtarlarımı kullanabilir miyim?
Y: Cosmos DB bir PaaS hizmetidir ve hizmetin kolayca kullanılmasını sağlamak için çok çalıştık. Bir uyumluluk gereksinimini PCI-DSS gibi toplantı için proxy soru olarak bu sorunun sıklıkla sorulması etmiş olabilirsiniz. Bu özellik oluşturmanın bir parçası olarak, Cosmos DB kullanan müşteriler anahtarları yönetmek zorunda kalmadan kendi gereksinimlerini karşıladığından emin olmak için Uyumluluk denetçiler birlikte çalıştık.

### <a name="q-what-regions-have-encryption-turned-on"></a>S: Şifreleme hangi bölgelerde açık?
Y: Tüm Azure Cosmos DB bölgelerinin tüm Kullanıcı verileri için şifrelenmesi açıktır.

### <a name="q-does-encryption-affect-the-performance-latency-and-throughput-slas"></a>S: Şifreleme, performans gecikmesini ve üretilen iş SLA 'larını etkiler mi?
Y: Artık mevcut ve yeni hesaplar için bekleyen şifreleme özelliğinin etkin olduğundan emin olmak için performans SLA 'Ları üzerinde hiçbir etkisi veya değişiklik yoktur. Hakkında daha fazla edinebilirsiniz [Cosmos DB için SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db) son garantileri görmek için sayfayı.

### <a name="q-does-the-local-emulator-support-encryption-at-rest"></a>S: Yerel öykünücü, bekleyen şifrelemeyi destekliyor mu?
Y: Öykünücü, tek başına bir geliştirme/test aracıdır ve yönetilen Cosmos DB hizmetinin kullandığı anahtar yönetim hizmetlerini kullanmaz. Bizim önerimiz burada hassas öykünücü test verilerini depolama sürücülerinde BitLocker etkinleştirmektir. [Öykünücü destekler varsayılan veri dizinine değiştirme](local-emulator.md) iyi bilinen bir konum kullanarak yanı sıra.

## <a name="next-steps"></a>Sonraki adımlar

Cosmos DB güvenliğe ve en son geliştirmelere genel bakış için bkz. [Azure Cosmos veritabanı güvenliği](database-security.md).
Microsoft sertifikaları hakkında daha fazla bilgi için bkz. [Azure Güven Merkezi](https://azure.microsoft.com/support/trust-center/).
