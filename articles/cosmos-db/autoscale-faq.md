---
title: Azure Cosmos DB 'de otomatik ölçeklendirme sağlanan işleme hakkında sık sorulan sorular
description: Azure Cosmos DB veritabanları ve kapsayıcılar için otomatik ölçeklendirme sağlanan aktarım hızı hakkında sık sorulan sorular
author: deborahc
ms.author: dech
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 04/28/2020
ms.openlocfilehash: fa4c2708f34a377a17914c7e6e5abdd709cbb5b1
ms.sourcegitcommit: e0330ef620103256d39ca1426f09dd5bb39cd075
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/05/2020
ms.locfileid: "82791791"
---
# <a name="frequently-asked-questions-about-autoscale-provisioned-throughput-in-azure-cosmos-db"></a>Azure Cosmos DB 'de otomatik ölçeklendirme sağlanan aktarım hızı hakkında sık sorulan sorular

Otomatik ölçeklendirme sağlanan aktarım hızı sayesinde, Azure Cosmos DB kapsayıcının veya veritabanınızın RU/s 'sini kullanıma göre otomatik olarak yönetip ölçeklendirecektir. Bu makalede, otomatik ölçeklendirme hakkında sıkça sorulan sorular yanıtlanmaktadır.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="is-autoscale-supported-for-all-apis"></a>Tüm API 'Ler için otomatik ölçeklendirme destekleniyor mu?
Evet, otomatik ölçeklendirme tüm API 'Ler için desteklenir: Core (SQL), Gremlin, Table, Cassandra ve MongoDB için API.

### <a name="is-autoscale-supported-for-multi-master-accounts"></a>Çoklu yönetici hesapları için otomatik ölçeklendirme destekleniyor mu?
Evet, otomatik ölçeklendirme, çok yöneticili hesaplar için desteklenir. En fazla RU/sn, Cosmos hesabına eklenen her bölgede kullanılabilir. 

### <a name="what-is-the-pricing-for-autoscale-"></a>Otomatik ölçeklendirme fiyatlandırması nedir?
Ayrıntılar için Azure Cosmos DB [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/) bakın. 

### <a name="how-do-i-enable-autoscale-for-my-containers-or-databases"></a>Kapsayıcılarım veya Veritabanlarım için otomatik ölçeklendirmeyi etkinleştirmek Nasıl yaparım??
Otomatik ölçeklendirme, Azure portal kullanılarak oluşturulan yeni kapsayıcılar ve veritabanları üzerinde etkinleştirilebilir.

### <a name="is-there-cli-or-sdk-support-to-create-containers-or-databases-with-autoscale-provisioned-throughput"></a>Otomatik ölçeklendirme sağlanmış iş aktarımına sahip kapsayıcılar veya veritabanları oluşturmak için CLı veya SDK desteği var mı?
Şu anda yalnızca Azure portal otomatik ölçek sağlanmış işleme içeren kaynaklar oluşturabilirsiniz. CLı ve SDK desteği henüz kullanılamamaktadır.

### <a name="can-i-enable-autoscale-on-an-existing-container-or-a-database"></a>Varolan bir kapsayıcıda veya veritabanında otomatik ölçeklendirmeyi etkinleştirebilir miyim?
Şu anda, oluşturma sırasında yeni kapsayıcılar ve veritabanlarında otomatik ölçeklendirmeyi etkinleştirebilirsiniz. Mevcut kapsayıcılar ve veritabanları için otomatik ölçeklendirmeyi etkinleştirme desteği henüz kullanılamamaktadır. [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md) veya [akışı Değiştir](change-feed.md)' i kullanarak, mevcut kapsayıcıları yeni bir kapsayıcıya geçirebilirsiniz. 

### <a name="can-i-turn-off-autoscale-on-a-container-or-database"></a>Bir kapsayıcı veya veritabanında otomatik ölçeklendirmeyi kapatabilir miyim?
Evet, sağlanan aktarım hızı için ' standart (El Ile) ' seçeneğine geçerek otomatik ölçeklendirmeyi devre dışı bırakabilirsiniz. Geçerli yayında, otomatik ölçeklendirmeyi standart sağlanan aktarım hızına geçtikten sonra aynı kaynak için otomatik ölçeklendirmeyi yeniden etkinleştiremezsiniz. 

### <a name="is-autoscale-supported-for-shared-throughput-databases"></a>Paylaşılan üretilen iş veritabanları için otomatik ölçeklendirme destekleniyor mu?
Evet, otomatik ölçeklendirme, paylaşılan üretilen iş veritabanları için desteklenir. Bu özelliği etkinleştirmek için veritabanını oluştururken otomatik ölçeklendirme ve **üretilen Iş sağlama** seçeneğini belirleyin. 

### <a name="what-is-the-number-of-allowed-collections-per-shared-throughput-database-when-autoscale-is-enabled"></a>Otomatik ölçeklendirme etkinleştirildiğinde, paylaşılan üretilen iş veritabanı başına izin verilen koleksiyon sayısı nedir?
Otomatik Ölçeklendirme özelliği etkinken paylaşılan üretilen iş veritabanları için izin verilen koleksiyon sayısı: en az (25, veritabanı/1000 ' lik en fazla RU/) olur. Örneğin, veritabanının en fazla üretilen işi 20.000 RU/sn ise, veritabanı en az (25, 20.000 RU/s/1000) = 20 koleksiyona sahip olabilir. 


### <a name="what-is-the-storage-limit-associated-with-each-max-rus-option"></a>Her bir maksimum RU/sn seçeneğiyle ilişkili depolama sınırı nedir?  
Her Max RU/sn için GB cinsinden depolama sınırı: en fazla RU/s/veritabanı veya kapsayıcı/100. Örneğin, en fazla RU/sn 20.000 RU/s ise, kaynak 200 GB depolama alanını destekleyebilir. Kullanılabilir en büyük RU/sn ve depolama seçenekleri için [Otomatik ölçeklendirme sınırları](provision-throughput-autoscale.md#autoscale-limits) makalesine bakın. 

### <a name="what-happens-if-i-exceed-the-storage-limit-associated-with-my-max-throughput"></a>En yüksek aktarım hızı ile ilişkili depolama sınırını aşarsam ne olur?
Veritabanının veya kapsayıcının en fazla üretilen işi ile ilişkili depolama sınırı aşılırsa Azure Cosmos DB, en yüksek aktarım hızını, bu depolama düzeyini destekleyebilen bir sonraki en yüksek katmana otomatik olarak arttırır. Örneğin, 50 GB depolama sınırı olan 4000 RU/s en yüksek aktarım hızı seçeneğiyle bir veritabanının veya kapsayıcının sağlandığını varsayalım. Kaynağın depolaması 100 GB 'a artıyorsa, veritabanının veya kapsayıcının en fazla RU/sn değeri otomatik olarak 20.000 RU/sn 'ye yükselmiştir ve bu, 200 GB 'a kadar destek verebilir. 

### <a name="how-quickly-will-autoscale-up-and-down-based-on-spikes-in-traffic"></a>Trafikte ani artışlar temel alınarak ne kadar hızlı bir şekilde yukarı ve aşağı ayarlanır?
Otomatik ölçeklendirme ile, en düşük ve en yüksek RU/sn aralığında, gelen trafiğe göre ölçeği büyütme veya ölçeği azaltma anında sağlayabilirsiniz. Faturalandırma, belirli bir saat içinde en yüksek RU/sn için ücretlendirildiğiniz 1 saatlik bir ayrıntı düzeyinde yapılır.

### <a name="can-i-specify-a-custom-max-throughput-rus-value-for-autoscale"></a>Otomatik ölçeklendirme için bir özel en fazla aktarım hızı (RU/s) değeri belirtebilir miyim?
Şu anda en fazla aktarım hızı (RU/s) için [dört seçenek](provision-throughput-autoscale.md#autoscale-limits) arasından seçim yapabilirsiniz.

### <a name="can-i-increase-the-max-rus-move-to-a-higher-tier-on-the-database-or-container"></a>Veritabanı veya kapsayıcıda en fazla RU/sn (daha yüksek bir katmana geçme) artırabilir miyim? 
Evet. Kapsayıcınız için **ölçek & ayarları** seçeneğinden veya veritabanınız için **Ölçek** seçeneğinde, otomatik ölçeklendirme için daha yüksek bir en fazla ru/sn seçebilirsiniz. Bu, hizmet daha yüksek ölçeklendirmeyi destekleyecek daha fazla kaynak sağlarken, tamamlanması bir süre (genellikle 4-6 saat) sürebilen zaman uyumsuz bir genişleme işlemidir. 

### <a name="can-i-reduce-the-max-rus-move-to-a-lower-tier-on-the-database-or-container"></a>Veritabanı veya kapsayıcıda en fazla RU/sn (daha düşük bir katmana geçme) azaltabilir miyim?
Evet. Veritabanının veya kapsayıcının geçerli depolaması, ölçeklendirmek istediğiniz en fazla RU/s katmanıyla ilişkili [depolama sınırının](#what-is-the-storage-limit-associated-with-each-max-rus-option) altında olduğu sürece, bu katmana en fazla ru/sn 'yi azaltabilirsiniz. Örneğin, en fazla RU/sn olarak 20.000 RU/s seçtiyseniz, 50 GB 'den (4000 RU/sn ile ilişkili depolama sınırı) daha az depolama alanı varsa, en fazla RU/sn boyutunu 4000 RU/s olarak azaltabilirsiniz.

### <a name="what-is-the-mapping-between-the-max-rus-and-physical-partitions"></a>En büyük RU/s ve fiziksel bölümler arasındaki eşleme nedir?
En büyük RU/s 'yi ilk seçtiğinizde Azure Cosmos DB sağlayacak: en fazla RU/s/10.000 RU/s = fiziksel bölüm sayısı. Her [fiziksel bölüm](partition-data.md#physical-partitions) , en fazla 10.000 ru/sn ve 50 GB depolamayı destekleyebilir. Depolama boyutu büyüdükçe Azure Cosmos DB, depolama artışını işlemek için bölümleri daha fazla fiziksel bölüm eklemek üzere otomatik olarak böler veya depolama [ilişkili sınırı aşarsa](#what-is-the-storage-limit-associated-with-each-max-rus-option)en büyük ru/s katmanını arttırır. 

Veritabanının veya kapsayıcının en fazla RU/sn değeri, tüm fiziksel bölümlerde eşit olarak bölünür. Bu nedenle, tek bir fiziksel bölümün her biri için ölçeklenebilen toplam verimlilik: veritabanı veya kapsayıcı/# fiziksel bölüm için en fazla RU/sn. 

### <a name="what-happens-if-incoming-requests-exceed-the-max-rus-of-the-database-or-container"></a>Gelen istekler veritabanının veya kapsayıcının en büyük RU/sn boyutunu aşarsa ne olur?
Toplam tüketilen RU/sn kapsayıcının veya veritabanının en fazla RU/sn değerini aşarsa, en fazla RU/sn 'yi aşan istekler kısıtlanacaktır ve 429 durum kodu döndürür. %100 normalleştirilmiş kullanım üzerinden elde edilen istekler de kısıtlanacak. Normalleştirilmiş kullanım, tüm fiziksel bölümlerde RU/sn kullanımının en yüksek değeri olarak tanımlanır. Örneğin, en fazla aktarım hızı 20.000 RU/sn olduğunu ve her biri 10.000 RU/sn 'ye ölçeklendirebileceğiniz P_1 ve P_2 iki fiziksel bölüme sahip olduğunuzu varsayalım. Belirli bir saniye içinde P_1 6000 ru ve P_2 8000 ru kullanıyorsa, normalleştirilmiş kullanım en fazla (6000 RU/10.000 RU, 8000 RU/10.000 RU) = 0,8 olur.

> [!NOTE]
> Azure Cosmos DB istemci SDK 'Ları ve veri içe aktarma araçları (Azure Data Factory, toplu yürütücü kitaplığı) otomatik olarak 429s üzerinde yeniden denense de, zaman zaman 429s iyi durumda. Sürekli olarak yüksek sayıda 429s, en fazla RU/sn 'yi artırmanız veya bir [sık kullanılan bölüm](#autoscale-rate-limiting)için bölümleme stratejinizi gözden geçirmeniz gerektiğini gösteriyor olabilir.

### <a name="is-it-still-possible-to-see-429s-throttlingrate-limiting-when-autoscale-is-enabled"></a><a id="autoscale-rate-limiting"></a>Otomatik ölçeklendirme etkinleştirildiğinde 429s (daraltma/hız sınırlaması) görmeyi hala mümkün mü? 
Evet. 429s 'yi iki senaryoda görmek mümkündür. İlk olarak, kullanılan RU/s, kapsayıcının veya veritabanının en fazla RU/sn değerini aştığında hizmet, istekleri uygun şekilde azaltır. 

İkinci olarak, bir sıcak bölüm varsa (yani, diğer bölüm anahtarı değerleriyle karşılaştırılan daha yüksek bir istek miktarına sahip bir mantıksal bölüm anahtarı değeri), temeldeki fiziksel bölümün RU/s bütçesini aşması mümkündür. En iyi uygulama olarak, sık kullanılan bölümlerin önüne geçmek için, depolama ve aktarım hızı için eşit bir dağıtım ile sonuçlanan [iyi bir bölüm anahtarı seçin](partitioning-overview.md#choose-partitionkey) . 

Örneğin, 20.000 RU/s en fazla aktarım hızı seçeneğini belirlerseniz ve dört fiziksel bölümden oluşan 200 GB depolama alanı varsa, her fiziksel bölüm, 5000 RU/sn 'ye kadar hızla ölçeklendirilebilir. Belirli bir mantıksal bölüm anahtarında sık erişimli bir bölüm varsa, içindeki temeldeki fiziksel bölüm 5000 RU/s 'yi aşarsa, bu, %100 normalleştirilmiş kullanımı aşmaktadır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos kapsayıcısı veya veritabanında otomatik ölçeklendirmeyi etkinleştirmeyi](provision-throughput-autoscale.md#create-db-container-autoscale)öğrenin.
* [Otomatik ölçeklendirme ile sağlanan aktarım hızının avantajları ](provision-throughput-autoscale.md#autoscale-benefits)hakkında bilgi edinin.
* [Mantıksal ve fiziksel bölümler](partition-data.md)hakkında daha fazla bilgi edinin.
