---
title: Azure Cosmos DB'de işletim yerinde şifreleme
description: Azure Cosmos DB'nin verilerin şifrelemesini nasıl sağladığını ve nasıl uygulandığını öğrenin.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/23/2019
ms.custom: seodec18
ms.openlocfilehash: db20388b5277e000ffe7055e9840742d6af7788e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80062595"
---
# <a name="data-encryption-in-azure-cosmos-db"></a>Azure Cosmos DB'de veri şifreleme 

Şifreleme, katı hal sürücüleri (SSD'ler) ve sabit disk sürücüleri (HDD'ler) gibi geçici olmayan depolama aygıtlarında genellikle verilerin şifrelemesini ifade eden bir ifadedir. Cosmos DB birincil veritabanlarını SSD'lerde saklar. Medya ekleri ve yedekleri, genellikle HDD'ler tarafından yedeklenen Azure Blob depolama alanında depolanır. Cosmos DB için şifreleme nin serbest bırakılması ile tüm veritabanlarınız, medya ekleriniz ve yedeklemeleriniz şifrelenir. Verileriniz artık aktarım (ağ üzerinden) ve istirahatte (geçici olmayan depolama) şifrelenir ve bu da size uçuça şifreleme sağlar.

Bir PaaS hizmeti olarak Cosmos DB'nin kullanımı çok kolaydır. Cosmos DB'de depolanan tüm kullanıcı verileri istirahatte ve taşıma sırasında şifrelendiği için herhangi bir işlem yapmak zorunda değilsiniz. Bunu ifade etmenin başka bir yolu da şifrelemenin varsayılan olarak "açıktır". Kapatmak veya açmak için denetim yok. Azure Cosmos DB, hesabın çalıştığı tüm bölgelerde AES-256 şifrelemesi kullanır. Biz [kullanılabilirlik ve performans SLA'lar](https://azure.microsoft.com/support/legal/sla/cosmos-db)karşılamak için devam ederken biz bu özelliği sağlar.

## <a name="implementation-of-encryption-at-rest-for-azure-cosmos-db"></a>Azure Cosmos DB için şifreleme uygulaması

Şifreleme, güvenli anahtar depolama sistemleri, şifreli ağlar ve şifreleme API'leri de dahil olmak üzere bir dizi güvenlik teknolojisi kullanılarak uygulanır. Verilerin şifresini çözen ve işleyen sistemler, anahtarları yöneten sistemlerle iletişim kurmak zorunda. Diyagram, şifrelenmiş verilerin depolanması ve anahtarların yönetiminin nasıl ayrıldığını gösterir. 

![Tasarım diyagramı](./media/database-encryption-at-rest/design-diagram.png)

Kullanıcı isteğinin temel akışı aşağıdaki gibidir:
- Kullanıcı veritabanı hesabı hazır hale getirilir ve depolama anahtarları Yönetim Hizmeti Kaynak Sağlayıcısı'na bir istek yoluyla alınır.
- Kullanıcı, HTTPS/güvenli taşıma aracı yla Cosmos DB bağlantısı oluşturur. (SDK'lar ayrıntıları özetler.)
- Kullanıcı, önceden oluşturulmuş güvenli bağlantı üzerinden depolanacak bir JSON belgesi gönderir.
- Kullanıcı dizin oluşturmayı kapatmadıkça JSON belgesi dizine eklenir.
- Hem JSON belge hem de dizin verileri güvenli depolama için yazılır.
- Veriler düzenli aralıklarla güvenli depolamadan okunur ve Azure Şifrelenmiş Blob Mağazası'na yedeklenir.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="q-how-much-more-does-azure-storage-cost-if-storage-service-encryption-is-enabled"></a>S: Depolama Hizmeti Şifrelemesi etkinse Azure Depolama daha ne kadar tutar?
C: Ek bir ücret yoktur.

### <a name="q-who-manages-the-encryption-keys"></a>S: Şifreleme anahtarlarını kim yönetir?
C: Anahtarlar Microsoft tarafından yönetilir.

### <a name="q-how-often-are-encryption-keys-rotated"></a>S: Şifreleme anahtarları ne sıklıkta döndürülür?
C: Microsoft' un şifreleme anahtarı döndürmesi için cosmos DB' nin izlediği bir dizi dahili yönergesi vardır. Belirli yönergeler yayınlanmaz. Microsoft, dahili kılavuzun bir alt kümesi olarak görülen ve geliştiriciler için yararlı en iyi uygulamalara sahip olan [Güvenlik Geliştirme Yaşam Döngüsü'nü (SDL)](https://www.microsoft.com/sdl/default.aspx)yayımlar.

### <a name="q-can-i-use-my-own-encryption-keys"></a>S: Kendi şifreleme anahtarlarımı kullanabilir miyim?
C: Evet şimdi bu özellik yeni kozmos hesapları için kullanılabilir ve bu hesap oluşturma sırasında yapılmalıdır. Daha fazla bilgi için lütfen [Müşteri tarafından yönetilen Keys](https://docs.microsoft.com/azure/cosmos-db/how-to-setup-cmk) belgesini inceleyin.

### <a name="q-what-regions-have-encryption-turned-on"></a>S: Şifreleme hangi bölgeleri açtı?
C: Tüm Azure Cosmos DB bölgeleri tüm kullanıcı verileri için şifrelemeyi açtı.

### <a name="q-does-encryption-affect-the-performance-latency-and-throughput-slas"></a>S: Şifreleme performans gecikmesini ve iş sonu SLA'larını etkiler mi?
C: Tüm varolan ve yeni hesaplar için şifrelemenin etkin olması nedeniyle, performans SLA'larında herhangi bir etki veya değişiklik yoktur. En son garantileri görmek [için Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db) için SLA'yı daha fazla okuyabilirsiniz.

### <a name="q-does-the-local-emulator-support-encryption-at-rest"></a>S: Yerel emülatör şifrelemeyi istirahatte destekliyor mu?
C: Emülatör bağımsız bir dev/test aracıdır ve yönetilen Cosmos DB hizmetinin kullandığı anahtar yönetim hizmetlerini kullanmaz. Tavsiyemiz, hassas emülatör test verilerini depoladığınız sürücülerde BitLocker'ı etkinleştirmektir. [Emülatör, varsayılan veri dizinini değiştirmenin](local-emulator.md) yanı sıra iyi bilinen bir konum kullanmayı da destekler.

## <a name="next-steps"></a>Sonraki adımlar

Cosmos DB güvenliğine ve en son geliştirmelere genel bakış için [Azure Cosmos veritabanı güvenliğine](database-security.md)bakın.
Microsoft sertifikaları hakkında daha fazla bilgi için [Azure Güven Merkezi'ne](https://azure.microsoft.com/support/trust-center/)bakın.
