---
title: Azure Cosmos DB Autopilot modunda üretilen iş hakkında sık sorulan sorular
description: Azure Cosmos DB veritabanları ve kapsayıcılar için Autopilot modu hakkında sık sorulan sorular
author: deborahc
ms.author: dech
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/16/2019
ms.openlocfilehash: d2ad585cabefb9ea78b88e748e422c745c11d03f
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75483316"
---
# <a name="frequently-asked-questions-about-provisioned-throughput-in-azure-cosmos-db-autopilot-mode-preview"></a>Azure Cosmos DB Autopilot modundaki (Önizleme) sağlanan aktarım hızı hakkında sık sorulan sorular
Azure Cosmos DB, Autopilot modu ile kapsayıcının veya veritabanınızın RU/sn 'sini kullanıma göre otomatik olarak yönetip ölçeklendirecektir. Bu makale, Autopilot modu hakkında sık sorulan soruları yanıtlar. 

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="is-autopilot-mode-supported-for-all-apis"></a>Autopilot modu tüm API 'Ler için destekleniyor mu?
Evet, Autopilot modu tüm API 'Ler için desteklenir: Core (SQL), Gremlin, Table, Cassandra ve MongoDB için API.

### <a name="is-autopilot-mode-supported-for-multi-master-accounts"></a>Autopilot modu çok yöneticili hesaplar için destekleniyor mu?
Evet, Autopilot modu çok yöneticili hesaplar için desteklenir. En fazla RU/sn, Cosmos hesabına eklenen her bölgede kullanılabilir. 

### <a name="what-is-the-pricing-for-autopilot"></a>Autopilot için fiyatlandırma nedir?
Ayrıntılar için Azure Cosmos DB [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/) bakın. 

### <a name="how-do-i-enable-autopilot-for-my-containers-or-databases"></a>Autopilot veya Veritabanlarım için Nasıl yaparım? etkinleştirmek istiyor musunuz?
Autopilot modu, Azure portal kullanılarak oluşturulan yeni kapsayıcılar ve veritabanlarında etkinleştirilebilir. 

### <a name="is-there-cli-or-sdk-support-to-create-containers-or-databases-with-autopilot-mode"></a>Autopilot modu ile kapsayıcılar veya veritabanları oluşturmak için CLı veya SDK desteği var mı?
Şu anda önizleme sürümünde, yalnızca Azure portal Autopilot modu ile kaynak oluşturabilirsiniz. CLı ve SDK desteği henüz kullanılamamaktadır.

### <a name="can-i-enable-autopilot-on-an-existing-container-or-a-database"></a>Mevcut bir kapsayıcıda veya veritabanında Autopilot etkinleştirebilir miyim?
Şu anda, Autopilot ' yi oluştururken yeni kapsayıcılar ve veritabanları üzerinde etkin hale getirebilirsiniz. Mevcut kapsayıcılar ve veritabanları için Autopilot modunun etkinleştirilmesi desteği henüz kullanılamamaktadır. [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md) veya [akışı Değiştir](change-feed.md)' i kullanarak, mevcut kapsayıcıları yeni bir kapsayıcıya geçirebilirsiniz. 

### <a name="can-i-turn-off-autopilot-mode-on-a-container-or-database"></a>Autopilot modunu bir kapsayıcı veya veritabanında kapatabilir miyim?
Evet, sağlanan aktarım hızı için ' El Ile ' seçeneğine geçerek Autopilot kapatabilirsiniz. Önizleme sürümünde, Autopilot modundan el ile moduna geçtikten sonra aynı kaynak için Autopilot yeniden etkinleştirilemez. 

### <a name="is-autopilot-mode-supported-for-shared-throughput-databases"></a>Autopilot modu paylaşılan üretilen iş veritabanları için destekleniyor mu?
Evet, Autopilot modu paylaşılan üretilen iş veritabanları için desteklenir. Bu özelliği etkinleştirmek için veritabanını oluştururken Autopilot Mode ve **üretilen Iş sağlama** seçeneğini belirleyin. 

### <a name="what-is-the-number-of-allowed-collections-per-shared-throughput-database-when-autopilot-is-enabled"></a>Autopilot etkinleştirildiğinde paylaşılan üretilen iş veritabanı başına izin verilen koleksiyon sayısı nedir?
Autopilot modu etkinken paylaşılan üretilen iş veritabanları için, izin verilen koleksiyonların sayısı: MIN (25, en fazla RU/1000 veritabanı/). Örneğin, veritabanının en fazla üretilen işi 20.000 RU/sn ise, veritabanı en az (25, 20.000 RU/s/1000) = 20 koleksiyona sahip olabilir. 


### <a name="what-is-the-storage-limit-associated-with-each-max-rus-option"></a>Her bir maksimum RU/sn seçeneğiyle ilişkili depolama sınırı nedir?  
Her Max RU/sn için GB cinsinden depolama sınırı: en fazla RU/s/veritabanı veya kapsayıcı/100. Örneğin, en fazla RU/sn 20.000 RU/s ise, kaynak 200 GB depolama alanını destekleyebilir. Kullanılabilir en büyük RU/s ve depolama seçenekleri için [Autopilot limitleri](provision-throughput-autopilot.md#autopilot-limits) makalesine bakın. 

### <a name="what-happens-if-i-exceed-the-storage-limit-associated-with-my-max-throughput"></a>En yüksek aktarım hızı ile ilişkili depolama sınırını aşarsam ne olur?
Veritabanının veya kapsayıcının en fazla üretilen işi ile ilişkili depolama sınırı aşılırsa Azure Cosmos DB, en yüksek aktarım hızını, bu depolama düzeyini destekleyebilen bir sonraki en yüksek katmana otomatik olarak arttırır. Örneğin, 50 GB depolama sınırı olan 4000 RU/s en yüksek aktarım hızı seçeneğiyle bir veritabanının veya kapsayıcının sağlandığını varsayalım. Kaynağın depolaması 100 GB 'a artıyorsa, veritabanının veya kapsayıcının en fazla RU/sn değeri otomatik olarak 20.000 RU/sn 'ye yükselmiştir ve bu, 200 GB 'a kadar destek verebilir. 

### <a name="how-quickly-will-autopilot-scale-up-and-down-based-on-spikes-in-traffic"></a>Trafikte ani artışlar temelinde ölçeği ne kadar hızlı Autopilot?
Autopilot, gelen trafiğe göre en düşük ve en yüksek RU/sn aralığındaki RU/s ölçeğini anında. Faturalandırma, belirli bir saat içinde en yüksek RU/sn için ücretlendirildiğiniz 1 saatlik bir ayrıntı düzeyinde yapılır. 

### <a name="can-i-specify-a-custom-max-throughput-rus-value-for-autopilot-mode"></a>Autopilot modu için özel bir en fazla aktarım hızı (RU/s) değeri belirtebilir miyim?
Şu anda önizleme sürümü sırasında en fazla aktarım hızı (RU/s) için [dört seçenek](provision-throughput-autopilot.md#autopilot-limits) arasından seçim yapabilirsiniz.

### <a name="can-i-increase-the-max-rus-move-to-a-higher-tier-on-the-database-or-container"></a>Veritabanı veya kapsayıcıda en fazla RU/sn (daha yüksek bir katmana geçme) artırabilir miyim? 
Evet. Kapsayıcınız için **ölçek & ayarları** seçeneğinden veya veritabanınızın **Ölçek** seçeneğinde, Autopilot için daha yüksek bir en büyük ru/sn seçebilirsiniz. Bu, hizmet daha yüksek ölçeklendirmeyi destekleyecek daha fazla kaynak sağlarken, tamamlanması bir süre (genellikle 4-6 saat) sürebilen zaman uyumsuz bir genişleme işlemidir. 

### <a name="can-i-reduce-the-max-rus-move-to-a-lower-tier-on-the-database-or-container"></a>Veritabanı veya kapsayıcıda en fazla RU/sn (daha düşük bir katmana geçme) azaltabilir miyim?
Evet. Veritabanının veya kapsayıcının geçerli depolaması, ölçeklendirmek istediğiniz en fazla RU/s katmanıyla ilişkili [depolama sınırının](#what-is-the-storage-limit-associated-with-each-max-rus-option) altında olduğu sürece, bu katmana en fazla ru/sn 'yi azaltabilirsiniz. Örneğin, en fazla RU/sn olarak 20.000 RU/s seçtiyseniz, 50 GB 'den (4000 RU/sn ile ilişkili depolama sınırı) daha az depolama alanı varsa, en fazla RU/sn boyutunu 4000 RU/s olarak azaltabilirsiniz.

### <a name="what-is-the-mapping-between-the-max-rus-and-physical-partitions"></a>En büyük RU/s ve fiziksel bölümler arasındaki eşleme nedir?
En büyük RU/s 'yi ilk seçtiğinizde Azure Cosmos DB sağlayacak: en fazla RU/s/10.000 RU/s = fiziksel bölüm sayısı. Her [fiziksel bölüm](partition-data.md#physical-partitions) , en fazla 10.000 ru/sn ve 50 GB depolamayı destekleyebilir. Depolama boyutu büyüdükçe Azure Cosmos DB, depolama artışını işlemek için bölümleri daha fazla fiziksel bölüm eklemek üzere otomatik olarak böler veya depolama [ilişkili sınırı aşarsa](#what-is-the-storage-limit-associated-with-each-max-rus-option)en büyük ru/s katmanını arttırır. 

Veritabanının veya kapsayıcının en fazla RU/sn değeri, tüm fiziksel bölümlerde eşit olarak bölünür. Bu nedenle, tek bir fiziksel bölümün her biri için ölçeklenebilen toplam verimlilik: veritabanı veya kapsayıcı/# fiziksel bölüm için en fazla RU/sn. 

### <a name="what-happens-if-incoming-requests-exceed-the-max-rus-of-the-database-or-container"></a>Gelen istekler veritabanının veya kapsayıcının en büyük RU/sn boyutunu aşarsa ne olur?
Toplam tüketilen RU/sn kapsayıcının veya veritabanının en fazla RU/sn değerini aşarsa, en fazla RU/sn 'yi aşan istekler kısıtlanacaktır ve 429 durum kodu döndürür. %100 normalleştirilmiş kullanım üzerinden elde edilen istekler de kısıtlanacak. Normalleştirilmiş kullanım, tüm fiziksel bölümlerde RU/sn kullanımının en yüksek değeri olarak tanımlanır. Örneğin, en fazla aktarım hızı 20.000 RU/sn olduğunu ve her biri 10.000 RU/sn 'ye ölçeklendirebileceğiniz P_1 ve P_2 iki fiziksel bölüme sahip olduğunuzu varsayalım. Belirli bir saniye içinde P_1 6000 ru ve P_2 8000 ru kullanıyorsa, normalleştirilmiş kullanım en fazla (6000 RU/10.000 RU, 8000 RU/10.000 RU) = 0,8 olur.

> [!NOTE]
> Azure Cosmos DB istemci SDK 'Ları ve veri içe aktarma araçları (Azure Data Factory, toplu yürütücü kitaplığı) otomatik olarak 429s üzerinde yeniden denense de, zaman zaman 429s iyi durumda. Sürekli olarak yüksek sayıda 429s, en fazla RU/sn 'yi artırmanız veya bir [sık kullanılan bölüm](#is-it-still-possible-to-see-429s-throttlingrate-limiting-when-autopilot-is-enabled)için bölümleme stratejinizi gözden geçirmeniz gerektiğini gösteriyor olabilir.

### <a name="is-it-still-possible-to-see-429s-throttlingrate-limiting-when-autopilot-is-enabled"></a>Autopilot etkinleştirildiğinde 429s (daraltma/hız sınırlaması) görmeyi hala mümkün mü? 
Evet. 429s 'yi iki senaryoda görmek mümkündür. İlk olarak, kullanılan RU/s, kapsayıcının veya veritabanının en fazla RU/sn değerini aştığında hizmet, istekleri uygun şekilde azaltır. 

İkinci olarak, bir sıcak bölüm varsa (yani, diğer bölüm anahtarı değerleriyle karşılaştırılan daha yüksek bir istek miktarına sahip bir mantıksal bölüm anahtarı değeri), temeldeki fiziksel bölümün RU/s bütçesini aşması mümkündür. En iyi uygulama olarak, sık kullanılan bölümlerin önüne geçmek için, depolama ve aktarım hızı için eşit bir dağıtım ile sonuçlanan [iyi bir bölüm anahtarı seçin](partitioning-overview.md#choose-partitionkey) . 

Örneğin, 20.000 RU/s en fazla aktarım hızı seçeneğini belirlerseniz ve dört fiziksel bölümden oluşan 200 GB depolama alanı varsa, her fiziksel bölüm, 5000 RU/sn 'ye kadar hızla ölçeklendirilebilir. Belirli bir mantıksal bölüm anahtarında sık erişimli bir bölüm varsa, içindeki temeldeki fiziksel bölüm 5000 RU/s 'yi aşarsa, bu, %100 normalleştirilmiş kullanımı aşmaktadır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos kapsayıcısı veya veritabanında Autopilot 'yi etkinleştirmeyi](provision-throughput-autopilot.md#create-a-database-or-a-container-with-autopilot-mode)öğrenin.
* [Autopilot 'in avantajları](provision-throughput-autopilot.md#benefits-of-autopilot-mode)hakkında bilgi edinin.
* [Mantıksal ve fiziksel bölümler](partition-data.md)hakkında daha fazla bilgi edinin.
