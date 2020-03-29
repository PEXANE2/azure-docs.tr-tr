---
title: Azure Cosmos DB otomatik pilot modunda iş artışı yla ilgili sık sorulan sorular
description: Azure Cosmos DB veritabanları ve kapsayıcıları için otomatik pilot modu hakkında sık sorulan sorular
author: deborahc
ms.author: dech
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/16/2019
ms.openlocfilehash: d2ad585cabefb9ea78b88e748e422c745c11d03f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75483316"
---
# <a name="frequently-asked-questions-about-provisioned-throughput-in-azure-cosmos-db-autopilot-mode-preview"></a>Azure Cosmos DB otomatik pilot modunda sağlanan iş ortası hakkında sık sorulan sorular (Önizleme)
Otomatik pilot modu ile Azure Cosmos DB, kapsayıcınızın veya veritabanınızın RU/s'lerini kullanıma göre otomatik olarak yönetir ve ölçeklendirebilir. Bu makalede, otomatik pilot modu hakkında sık sorulan soruları yanıtlar. 

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="is-autopilot-mode-supported-for-all-apis"></a>Tüm API'ler için otomatik pilot modu desteklenir mi?
Evet, otomatik pilot modu tüm API'ler için desteklenir: Core (SQL), Gremlin, Table, Cassandra ve ApI MongoDB için.

### <a name="is-autopilot-mode-supported-for-multi-master-accounts"></a>Çok yönlü hesaplar için otomatik pilot modu desteklenir mi?
Evet, otomatik pilot modu çok eski hesaplar için desteklenir. Maksimum RU/s Cosmos hesabına eklenen her bölgede mevcuttur. 

### <a name="what-is-the-pricing-for-autopilot"></a>Otomatik pilot için fiyatlandırma nedir?
Ayrıntılar için Azure Cosmos DB [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/) bakın. 

### <a name="how-do-i-enable-autopilot-for-my-containers-or-databases"></a>Kapsayıcılarım veya veritabanlarım için otomatik pilotu nasıl etkinleştirebilirim?
Azure portalı kullanılarak oluşturulan yeni kapsayıcılarda ve veritabanlarında otomatik pilot modu etkinleştirilebilir. 

### <a name="is-there-cli-or-sdk-support-to-create-containers-or-databases-with-autopilot-mode"></a>Otomatik pilot moduna sahip kapsayıcılar veya veritabanları oluşturmak için CLI veya SDK desteği var mı?
Şu anda, önizleme sürümünde yalnızca Azure portalından otomatik pilot moduna sahip kaynaklar oluşturabilirsiniz. CLI ve SDK desteği henüz mevcut değil.

### <a name="can-i-enable-autopilot-on-an-existing-container-or-a-database"></a>Varolan bir kapsayıcıda veya veritabanında otomatik pilotu etkinleştirebilir miyim?
Şu anda, bunları oluştururken yeni kapsayıcılar ve veritabanları üzerinde otomatik pilot etkinleştirebilirsiniz. Varolan kapsayıcılarda ve veritabanlarında otomatik pilot modunu etkinleştirmek için destek henüz kullanılamıyor. Varolan kapsayıcıları [Azure Veri Fabrikası'nı](../data-factory/connector-azure-cosmos-db.md) kullanarak yeni bir kapsayıcıya geçirebilirsiniz veya [özet akışını değiştirebilirsiniz.](change-feed.md) 

### <a name="can-i-turn-off-autopilot-mode-on-a-container-or-database"></a>Kapsayıcı veya veritabanında otomatik pilot modunu kapatabilir miyim?
Evet, sağlanan iş için 'Manuel' seçeneğine geçerek otomatik pilotu kapatabilirsiniz. Önizleme sürümünde, otomatik pilot modundan manuel moda geçtikten sonra, aynı kaynak için otomatik pilotu yeniden etkinleştiremezsiniz. 

### <a name="is-autopilot-mode-supported-for-shared-throughput-databases"></a>Paylaşılan iş veri tabanları için otomatik pilot modu desteklendi mi?
Evet, otomatik pilot modu paylaşılan iş veri tabanları için desteklenir. Bu özelliği etkinleştirmek için, veritabanını oluştururken otomatik pilot modunu ve **Provision iş yaratma** seçeneğini seçin. 

### <a name="what-is-the-number-of-allowed-collections-per-shared-throughput-database-when-autopilot-is-enabled"></a>Otomatik pilot etkinleştirildiğinde paylaşılan iş bölümü veritabanı başına izin verilen koleksiyon sayısı nedir?
Otomatik pilot modu etkinleştirilmiş paylaşılan iş veri tabanları için izin verilen koleksiyon sayısı şudur: MIN(25, Max RU/s/ veritabanı / 1000). Örneğin, veritabanının maksimum iş bölümü 20.000 RU/s ise, veritabanında MIN(25, 20.000 RU/s / 1000) = 20 koleksiyon olabilir. 


### <a name="what-is-the-storage-limit-associated-with-each-max-rus-option"></a>Her maksimum RU/s seçeneğiyle ilişkili depolama sınırı nedir?  
Her maksimum RU/s için GB depolama sınırı: Max RU / veritabanı veya konteyner / 100. Örneğin, maksimum RU/s 20.000 RU/s ise, kaynak 200 GB depolama alanını destekleyebilir. Kullanılabilir maksimum RU/s ve depolama seçenekleri için [otomatik pilot sınırlayan](provision-throughput-autopilot.md#autopilot-limits) makaleye bakın. 

### <a name="what-happens-if-i-exceed-the-storage-limit-associated-with-my-max-throughput"></a>Maksimum iş tonumla ilişkili depolama sınırını aşarsam ne olur?
Veritabanı veya kapsayıcının maksimum iş bölümüyle ilişkili depolama sınırı aşılırsa, Azure Cosmos DB maksimum iş verisini otomatik olarak bu depolama düzeyini desteklenebilen bir sonraki en yüksek katmana artırır. Örneğin, bir veritabanı nın veya kapsayıcının, depolama sınırı 50 GB olan 4000 RU/s maksimum iş bölümü seçeneğiyle sağlanmış olduğunu varsayalım. Kaynağın depolanması 100 GB'a çıkarsa, veritabanının veya kapsayıcının maksimum RU/s's'u otomatik olarak 20.000 RU/s'ye yükseltilir ve bu da 200 GB'a kadar desteklenebilir. 

### <a name="how-quickly-will-autopilot-scale-up-and-down-based-on-spikes-in-traffic"></a>Otomatik pilot, trafikteki ani artışlara göre ne kadar hızlı ölçeklenecek?
Otomatik pilot, gelen trafiğe bağlı olarak, minimum ve maksimum RU/s aralığında ru/s'leri anında ölçeklendirecek veya küçültecektir. Faturalandırma, belirli bir saatte en yüksek RU/s için ücretlendirildiğiniz 1 saatlik bir parçalı lıkta yapılır. 

### <a name="can-i-specify-a-custom-max-throughput-rus-value-for-autopilot-mode"></a>Otomatik pilot modu için özel bir max iş artışı (RU/s) değeri belirtebilir miyim?
Şu anda, önizleme sürümü sırasında, maksimum iş elde etmek için [dört seçenek](provision-throughput-autopilot.md#autopilot-limits) (RU/s) arasında seçim yapabilirsiniz.

### <a name="can-i-increase-the-max-rus-move-to-a-higher-tier-on-the-database-or-container"></a>Veritabanında veya kapsayıcıda maksimum RU/s'yi (daha yüksek bir katmana taşıyın) artırabilir miyim? 
Evet. Kapsayıcınız için **Ölçek & Ayarları** seçeneğinden veya veritabanınız için **Ölçekle** seçeneğinden otomatik pilot için daha yüksek bir maksimum RU/s seçebilirsiniz. Bu, hizmet daha yüksek ölçeği desteklemek için daha fazla kaynak sağlarken, tamamlanması bazen (genellikle RU/s'ye bağlı olarak 4-6 saat) zaman alabilecek bir eşzamanlı ölçeklendirme işlemidir. 

### <a name="can-i-reduce-the-max-rus-move-to-a-lower-tier-on-the-database-or-container"></a>Veritabanıveya kapsayıcıdaki maksimum RU/s'yi (daha düşük bir katmana taşıma) azaltabilir miyim?
Evet. Veritabanının veya kapsayıcının geçerli depolama alanı, küçültmek istediğiniz maksimum RU/s katmanıyla ilişkili [depolama sınırının](#what-is-the-storage-limit-associated-with-each-max-rus-option) altında olduğu sürece, maksimum RU/s'yi bu katmana indirebilirsiniz. Örneğin, maksimum RU/s olarak 20.000 RU/s seçtiyseniz, 50 GB'dan az depolama alanınız varsa (4000 RU/s ile ilişkili depolama sınırı) maksimum RU/s'yi 4000 RU/s'ye küçültebilirsiniz.

### <a name="what-is-the-mapping-between-the-max-rus-and-physical-partitions"></a>Maksimum RU/s ve fiziksel bölümler arasındaki haritalama nedir?
Max RU/s'yi ilk seçtiğinizde, Azure Cosmos DB aşağıdakileri sağlayacaktır: Max RU/s / 10,000 RU/s = # fiziksel bölümler. Her [fiziksel bölüm](partition-data.md#physical-partitions) 10.000 RU/s ve 50 GB depolama alanını destekleyebilir. Depolama boyutu büyüdükçe, Azure Cosmos DB depolama artışını işlemek için daha fazla fiziksel bölüm eklemek için bölümleri otomatik olarak böler veya depolama ilişkili [sınırı aşarsa](#what-is-the-storage-limit-associated-with-each-max-rus-option)maksimum RU/s katmanını artırır. 

Veritabanının veya kapsayıcının Max RU/s's'u tüm fiziksel bölümlere eşit olarak bölünür. Yani, herhangi bir fiziksel bölüm ölçeklendirebilirsiniz toplam iş bölümü: Max RU / veritabanı veya konteyner / # fiziksel bölümler. 

### <a name="what-happens-if-incoming-requests-exceed-the-max-rus-of-the-database-or-container"></a>Gelen istekler veritabanının veya kapsayıcının maksimum RU/s'sini aşarsa ne olur?
Genel tüketilen RU/s kapsayıcıveya veritabanının maksimum RU/s'sini aşarsa, maksimum RU/s'yi aşan istekler daraltılacak ve 429 durum kodunu döndürecektir. %100'den fazla normalleştirilmiş kullanımla sonuçlanan talepler de azaltılır. Normalleştirilmiş kullanım, tüm fiziksel bölümlerde RU/s kullanımının maksimumu olarak tanımlanır. Örneğin, maksimum iş bünüzün 20.000 RU/s olduğunu ve her biri 10.000 RU/s'ye ölçekleme yapabilen P_1 ve P_2 iki fiziksel bölüme sahip olduğunuzu varsayalım. Belirli bir saniyede, P_1 6000 US ve P_2 8000 RUs kullandıysa, normalleştirilmiş kullanım MAX(6000 RU / 10.000 RU, 8000 RU / 10.000 RU) = 0.8'dir.

> [!NOTE]
> Azure Cosmos DB istemci SDK'ları ve veri alma araçları (Azure Veri Fabrikası, toplu yürütme kitaplığı) 429'larda otomatik olarak yeniden çalışır, bu nedenle ara sıra 429'lar iyidir. 429s sürekli yüksek sayıda max RU / s artırmak veya sıcak bir [bölüm](#is-it-still-possible-to-see-429s-throttlingrate-limiting-when-autopilot-is-enabled)için bölümleme stratejisini gözden geçirmek gerektiğini gösterebilir.

### <a name="is-it-still-possible-to-see-429s-throttlingrate-limiting-when-autopilot-is-enabled"></a>Otomatik pilot etkinleştirildiğinde 429'ları (azaltma/hız sınırlaması) görmek hala mümkün mü? 
Evet. İki senaryoda 429'ları görmek mümkündür. İlk olarak, genel tüketilen RU/s kapsayıcıveya veritabanının maksimum RU/s'sini aştığında, hizmet istekleri buna göre daraltacaktır. 

İkinci olarak, sıcak bir bölüm varsa, yani diğer bölüm anahtar değerlerine kıyasla orantısız olarak daha yüksek istek miktarına sahip mantıksal bir bölüm anahtar değeri varsa, temel fiziksel bölümün RU/s bütçesini aşması mümkündür. En iyi uygulama olarak, sıcak bölümleri önlemek için, hem depolama ve iş bölümü eşit bir dağıtım sonuçlanır [iyi bir bölüm anahtarı seçin.](partitioning-overview.md#choose-partitionkey) 

Örneğin, 20.000 RU/s maksimum iş verme seçeneğini seçerseniz ve dört fiziksel bölümiçeren 200 GB depolama alanına sahipseniz, her fiziksel bölüm 5000 RU/s'ye kadar otomatik olarak ölçeklendirilebilir. Belirli bir mantıksal bölüm anahtarında sıcak bir bölüm varsa, altta yatan fiziksel bölüm 5000 RU/s'yi aştığında, yani %100 normalleştirilmiş kullanımı aştığında 429'ları görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos kapsayıcıveya veritabanında otomatik pilotu](provision-throughput-autopilot.md#create-a-database-or-a-container-with-autopilot-mode)nasıl etkinleştirin öğrenin.
* [Otomatik pilotun yararları](provision-throughput-autopilot.md#benefits-of-autopilot-mode)hakkında bilgi edinin.
* [Mantıksal ve fiziksel bölümler](partition-data.md)hakkında daha fazla bilgi edinin.
