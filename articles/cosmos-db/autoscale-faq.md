---
title: Azure Cosmos DB 'de otomatik ölçeklendirme sağlanan işleme hakkında sık sorulan sorular
description: Azure Cosmos DB veritabanları ve kapsayıcılar için otomatik ölçeklendirme sağlanan aktarım hızı hakkında sık sorulan sorular
author: deborahc
ms.author: dech
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/10/2020
ms.openlocfilehash: ca4e79977132586c619f323015f9d915e04707f1
ms.sourcegitcommit: 0a5bb9622ee6a20d96db07cc6dd45d8e23d5554a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84449524"
---
# <a name="frequently-asked-questions-about-autoscale-provisioned-throughput-in-azure-cosmos-db"></a>Azure Cosmos DB 'de otomatik ölçeklendirme sağlanan aktarım hızı hakkında sık sorulan sorular

Otomatik ölçeklendirme sağlanan verimlilik sayesinde Azure Cosmos DB, kullanımı temel alarak veritabanınızın veya kapsayıcının RU/s 'sini otomatik olarak yönetip ölçeklendirecektir. Bu makalede, otomatik ölçeklendirme hakkında sıkça sorulan sorular yanıtlanmaktadır.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="what-is-the-difference-between-autopilot-and-autoscale-in-azure-cosmos-db"></a>Azure Cosmos DB içinde "Autopilot" ve "otomatik ölçeklendirme" arasındaki fark nedir?
"Otomatik ölçeklendirme" veya "otomatik ölçeklendirme sağlanan aktarım hızı", daha önce "Autopilot" olarak bilinen özellik için güncelleştirilmiş addır. Otomatik ölçeklendirmeyi geçerli yayınla, özel en büyük RU/sn ve programlı destek ayarlama özelliği de dahil olmak üzere yeni özellikler ekledik. 

### <a name="what-happens-to-databases-or-containers-created-in-the-previous-autopilot-tier-model"></a>Önceki Autopilot Katmanı modelinde oluşturulan veritabanlarına veya kapsayıcılara ne olur?
Önceki katman modeliyle oluşturulan kaynaklar, yeni otomatik ölçeklendirme özel en yüksek RU/s modeliyle otomatik olarak desteklenir. Katmanın üst sınırı, aynı ölçek aralığı ile sonuçlanan yeni en fazla RU/sn haline gelir. 

Örneğin, daha önce 400 ile 4000 RU/s arasında ölçeklendirilmiş bir katman seçtiyseniz veritabanı veya kapsayıcı, 400/s ' 4000 e kadar olan en fazla 4000 RU/sn 'ye sahip olarak gösterilir. Buradan, en fazla RU/sn 'yi iş yükünüze uyacak şekilde özel bir değere değiştirebilirsiniz. 

### <a name="how-quickly-will-autoscale-scale-up-and-down-based-on-spikes-in-traffic"></a>Trafikte ani artışlar temel alarak otomatik olarak ölçek ölçeği ne kadar hızlı bir şekilde ayarlanır?
Otomatik ölçeklendirme ile sistem, üretilen iş `T` `0.1 * Tmax` akışını temel alarak, ve aralığındaki aktarım HıZıNı (ru/s) yukarı veya aşağı ölçeklendirir `Tmax` . Ölçeklendirme otomatik ve anlık olduğundan, herhangi bir zamanda herhangi bir noktada, hiçbir gecikme olmadan sağlanmış olarak kullanabilirsiniz `Tmax` . 

### <a name="how-do-i-determine-what-rus-the-system-is-currently-scaled-to"></a>Sistemin Şu anda hangi RU/sn 'ye ölçeklenmekte olduğunu Nasıl yaparım?.
Sağlanan otomatik ölçeklendirme en fazla RU/sn ve sistemin ölçeklendiği geçerli üretilen işi (RU/s) izlemek için [Azure izleyici ölçümlerini](how-to-choose-offer.md#measure-and-monitor-your-usage) kullanın. 

### <a name="what-is-the-pricing-for-autoscale"></a>Otomatik ölçeklendirme fiyatlandırması nedir?
Her saat, `T` sistemin saat içinde ölçeklendirileceği en yüksek aktarım hızı için faturalandırılırsınız. Kaynağınız saat içinde istek olmadığında veya daha fazla ölçeklenmediği takdirde `0.1 * Tmax` , en az için faturalandırılırsınız `0.1 * Tmax` . Ayrıntılar için Azure Cosmos DB [fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/cosmos-db/) bakın. 

### <a name="how-does-autoscale-show-up-on-my-bill"></a>Otomatik ölçeklendirme, faturamda nasıl görünür?
Tek yöneticili hesaplarda 100 RU/sn başına otomatik ölçeklendirme oranı 1,5 x 'tir ve standart (el ile) sağlanan aktarım hızı hızıdır. Faturanızda, mevcut standart üretilen iş ölçümü ölçümünü görürsünüz. Bu ölçerin miktarı 1,5 ile çarpılacak. Örneğin, sistemin en yüksek RU/sn bir saat içinde olarak ölçeklendirildiğinde 6000 RU/sn ise, bu saat için ölçerin 60 * 1,5 = 90 birimi üzerinden faturalandırılırsınız.

Çoklu yönetici hesaplarında, 100 RU/sn başına otomatik ölçeklendirme oranı, standart (el ile) sağlanan çok yöneticili aktarım hızına göre aynıdır. Faturanızda var olan çok yöneticili ölçümü görürsünüz. Ücretler aynı olduğundan, otomatik ölçeklendirme kullanırsanız, standart aktarım hızı ile aynı miktarı görürsünüz.

### <a name="does-autoscale-work-with-reserved-capacity"></a>Otomatik ölçeklendirme ayrılmış kapasiteyle çalışıyor mu?
Evet. Tek yöneticili ayrılmış kapasite satın aldığınızda, otomatik ölçeklendirme kaynakları için rezervasyon indirimi, ölçüm kullanımınız için 1,5 * [belirtilen bölgenin oranının](../cost-management-billing/reservations/understand-cosmosdb-reservation-charges.md#reservation-discount-per-region)oranıyla uygulanır. 

Çoklu ana ayrılmış kapasite otomatik ölçeklendirme ve standart (el ile) sağlanan aktarım hızı için aynı şekilde geçerlidir. Bkz. [Azure Cosmos DB ayrılmış kapasite](cosmos-db-reserved-capacity.md)

### <a name="does-autoscale-work-with-free-tier"></a>Otomatik ölçeklendirme, ücretsiz katmanla mı çalışıyor?
Evet. Ücretsiz katmanda, bir kapsayıcıda otomatik ölçeklendirme işleme kullanabilirsiniz. Özel en büyük RU/s ile otomatik ölçeklendirme paylaşılan verimlilik veritabanları için destek henüz kullanılamamaktadır. [Ücretsiz katman faturalandırmasının nasıl otomatik ölçeklendirme ile çalıştığını](understand-your-bill.md#billing-examples-with-free-tier-accounts)görün.

### <a name="is-autoscale-supported-for-all-apis"></a>Tüm API 'Ler için otomatik ölçeklendirme destekleniyor mu?
Evet, otomatik ölçeklendirme tüm API 'Ler için desteklenir: Core (SQL), Gremlin, Table, Cassandra ve MongoDB için API.

### <a name="is-autoscale-supported-for-multi-master-accounts"></a>Çoklu yönetici hesapları için otomatik ölçeklendirme destekleniyor mu?
Evet. En fazla RU/sn, Azure Cosmos DB hesabına eklenen her bölgede kullanılabilir. 

### <a name="how-do-i-enable-autoscale-on-new-databases-or-containers"></a>Yeni veritabanları veya kapsayıcılar üzerinde otomatik ölçeklendirmeyi etkinleştirmek Nasıl yaparım? mı?
[Otomatik ölçeklendirmeyi etkinleştirme](how-to-provision-autoscale-throughput.md)hakkında bilgi için bu makaleye bakın.

### <a name="can-i-enable-autoscale-on-an-existing-database-or-a-container"></a>Varolan bir veritabanı veya kapsayıcıda otomatik ölçeklendirmeyi etkinleştirebilir miyim?
Evet. Ayrıca, gerektiğinde otomatik ölçeklendirme ve standart (el ile) sağlanan aktarım hızı arasında geçiş yapabilirsiniz. Şu anda tüm API 'Lerde, bu işlemleri yapmak için yalnızca [Azure Portal](how-to-provision-autoscale-throughput.md#enable-autoscale-on-existing-database-or-container) kullanabilirsiniz.

### <a name="how-does-the-migration-between-autoscale-and-standard-manual-provisioned-throughput-work"></a>Otomatik ölçeklendirme ve standart (el ile) sağlanan aktarım hızı arasındaki geçiş nasıl çalışır?
Kavramsal olarak, üretilen iş türünü değiştirmek iki aşamalı bir işlemdir. İlk olarak, aktarım hızı ayarlarını, otomatik ölçeklendirme veya el ile sağlanan aktarım hızı kullanacak şekilde değiştirmek için bir istek gönderirsiniz. Her iki durumda da sistem, geçerli üretilen iş ayarları ve depolama alanı temelinde bir ilk RU/sn değeri otomatik olarak belirlenir ve ayarlar. Bu adım sırasında, Kullanıcı tarafından sağlanmayan bir RU/s değeri kabul edilmez. Güncelleştirme tamamlandıktan sonra [ru/s](#can-i-change-the-max-rus-on-the-database-or-container) 'yi iş yükünüze uyacak şekilde değiştirebilirsiniz. 

**Standart (el ile) sağlanan aktarım hızını Otomatik ölçeklendirmeye geçirme**

Bir kapsayıcı için, ilk otomatik ölçeklendirmeyi tahmin etmek için aşağıdaki formülü kullanın: en ``MAX(4000, current manual provisioned RU/s, maximum RU/s ever provisioned / 10, storage in GB * 100)`` yakın 1000 ru/sn 'ye yuvarlanır. Gerçek ilk otomatik ölçeklendirme en fazla RU/sn, hesap yapılandırmanıza göre farklılık gösterebilir.

Örnek #1:10.000 RU/sn el ile sağlanan işleme ve 25 GB depolama alanı içeren bir kapsayıcınıza sahip olduğunuzu varsayalım. Otomatik ölçeklendirmeyi etkinleştirdiğinizde, ilk otomatik ölçeklendirme en fazla RU/sn olacaktır: 10.000 RU/s, bu da 1000-10.000 RU/s arasında ölçeklendirecektir. 

Örnek #2:50.000 RU/sn el ile sağlanan işleme ve 2500 GB depolama içeren bir kapsayıcınıza sahip olduğunuzu varsayalım. Otomatik ölçeklendirmeyi etkinleştirdiğinizde, ilk otomatik ölçeklendirme en fazla RU/sn olacaktır: 250.000 RU/s, bu da 25.000-250.000 RU/s arasında ölçeklendirecektir. 

**Otomatik ölçeklendirmeyi standart (el ile) sağlanan aktarım hızına geçirme**

Başlangıçtaki el ile sağlanan aktarım hızı, geçerli otomatik ölçeklendirme en yüksek RU/sn değerine eşit olacaktır. 

Örnek: 20.000 RU/sn 'nin en fazla RU/sn (2000-20.000 RU/s arasında ölçekleme) ile bir otomatik ölçeklendirme veritabanınız veya Kapsayıcınız olduğunu varsayalım. El ile sağlanan üretilen işi kullanmak için güncelleştirdiğinizde, ilk verimlilik 20.000 RU/sn olur. 

### <a name="is-there-azure-cli-or-powershell-support-to-manage-databases-or-containers-with-autoscale"></a>Otomatik ölçeklendirme ile veritabanlarını veya kapsayıcıları yönetmek için Azure CLı veya PowerShell desteği var mı?
Şu anda yalnızca Azure portal, .NET v3 SDK, Java v4 SDK ve Azure Resource Manager otomatik ölçeklendirmeyi kullanarak kaynak oluşturabilir ve yönetebilirsiniz. Azure CLı, PowerShell ve diğer SDK 'larda sunulan destek henüz kullanılamamaktadır.

### <a name="is-autoscale-supported-for-shared-throughput-databases"></a>Paylaşılan üretilen iş veritabanları için otomatik ölçeklendirme destekleniyor mu?
Evet, otomatik ölçeklendirme, paylaşılan üretilen iş veritabanları için desteklenir. Bu özelliği etkinleştirmek için veritabanını oluştururken otomatik ölçeklendirme ve **üretilen Iş sağlama** seçeneğini belirleyin. 

### <a name="what-is-the-number-of-allowed-containers-per-shared-throughput-database-when-autoscale-is-enabled"></a>Otomatik ölçeklendirme etkinken paylaşılan üretilen iş veritabanı başına izin verilen kapsayıcı sayısı nedir?
Azure Cosmos DB, otomatik ölçeklendirme veya standart (el ile) aktarım hızı içeren veritabanları için geçerli olan paylaşılan bir üretilen iş veritabanında en fazla 25 kapsayıcı zorlar. 

### <a name="what-is-the-impact-of-autoscale-on-database-consistency-level"></a>Otomatik ölçeklendirmeyi veritabanı tutarlılık düzeyinde etkisi nedir?
Veritabanının tutarlılık düzeyinde otomatik ölçeklendirmeyi bir etkisi yoktur.
Kullanılabilir tutarlılık düzeyleri hakkında daha fazla bilgi için [tutarlılık düzeyleri](consistency-levels.md) makalesine bakın.

### <a name="what-is-the-storage-limit-associated-with-each-max-rus-option"></a>Her bir maksimum RU/sn seçeneğiyle ilişkili depolama sınırı nedir?  
Her Max RU/sn için GB cinsinden depolama sınırı: en fazla RU/s/veritabanı veya kapsayıcı/100. Örneğin, en fazla RU/sn 20.000 RU/s ise, kaynak 200 GB depolama alanını destekleyebilir. Kullanılabilir en büyük RU/sn ve depolama seçenekleri için [Otomatik ölçeklendirme sınırları](provision-throughput-autoscale.md#autoscale-limits) makalesine bakın. 

### <a name="what-happens-if-i-exceed-the-storage-limit-associated-with-my-max-throughput"></a>En yüksek aktarım hızı ile ilişkili depolama sınırını aşarsam ne olur?
Veritabanının veya kapsayıcının en fazla üretilen işi ile ilişkili depolama sınırı aşılırsa, Azure Cosmos DB, bu depolama düzeyini destekleyebilen bir sonraki en yüksek RU/sn 'ye en fazla üretilen işi otomatik olarak arttırır.

Örneğin, 50.000 RU/sn 'nin (5000-50.000 RU/s arasında ölçekleme) en fazla RU/sn ile başlatırsanız, en fazla 500 GB veri saklayabilirsiniz. 500 GB 'ı aşarsanız (örneğin, depolama 600 GB ise), yeni en büyük RU/sn değeri 60.000 RU/sn olacaktır (6000-60.000 RU/s arasında ölçeklendirme).

### <a name="can-i-change-the-max-rus-on-the-database-or-container"></a>Veritabanı veya kapsayıcıda en büyük RU/sn 'yi değiştirebilir miyim? 
Evet. En fazla RU/sn 'yi değiştirme hakkında bu [makaleye](how-to-provision-autoscale-throughput.md) bakın. En fazla RU/sn 'yi, istenen değere bağlı olarak değiştirdiğinizde, bu işlem tamamlanması biraz zaman alabilir (seçili olan RU/s 'ye bağlı olarak 4-6 saat kadar sürebilir)

#### <a name="increasing-the-max-rus"></a>En büyük RU/sn 'yi artırma
En fazla ru/sn 'yi artırmak için bir istek gönderdiğinizde, hizmet, en yüksek ru/sn 'yi `Tmax` desteklemek için daha fazla kaynak sağlar. Bu işlem gerçekleşirken, mevcut iş yükünüz ve işlemler etkilenmez. Sistem, ' `0.1*Tmax` `Tmax` nin yeni ölçek aralığı `0.1*Tmax_new` hazırlanana kadar veritabanınızı veya kapsayıcınızı, önceki ile arasında ölçeklendirmeye devam edecektir `Tmax_new` .

#### <a name="lowering-the-max-rus"></a>En büyük RU/sn 'yi kısın
En büyük RU/s 'yi düşürürseniz, en küçük değer olarak ayarlayabilirsiniz: `MAX(4000, highest max RU/s ever provisioned / 10, current storage in GB * 100)` , en yakın 1000 ru/sn 'ye yuvarlanır. 

Örnek #1:20.000 RU/sn 'nin (2000-20.000 RU/sn) ve 50 GB depolama alanı için en fazla RU/sn içeren bir otomatik ölçeklendirme kapsayıcısına sahip olduğunuzu varsayalım. En düşük, en düşük değer: en büyük RU/s ' yi şu şekilde ayarlayabilirsiniz: MAX (4000, 20.000/10, **50 * 100**) = 5000 ru/s (500-5000 ru/s arasında ölçeklendirme).

Örnek #2:100.000 RU/sn ve 100 GB depolama alanı için en fazla RU/sn içeren bir otomatik ölçeklendirme kapsayıcısına sahip olduğunuzu varsayalım. Şimdi, en fazla RU/s 'yi 150.000 RU/sn 'ye ölçeklendirebilirsiniz (15.000-150.000 RU/s arasında ölçeklendirme). En düşük, en düşük değer şu anda en fazla RU/sn 'yi ayarlayabilirsiniz: MAX (4000, **150.000/10**, 100 * 100) = 15.000 ru/s (1500-15.000 ru/s arasında ölçeklendirme). 

Paylaşılan bir üretilen iş veritabanı için, en fazla RU/sn 'yi düşürürseniz, en düşük değer olan en küçük `MAX(4000, highest max RU/s ever provisioned / 10, current storage in GB * 100,  4000 + (MAX(Container count - 25, 0) * 1000))` 1000 ru/sn 'ye yuvarlanır.  

Yukarıdaki formüller ve örnekler, ayarlayabileceğiniz en düşük otomatik ölçeklendirme maksimum RU/sn ile ilgilidir ve `0.1 * Tmax` `Tmax` sistemin otomatik olarak ölçeklendirilmesi arasında ayrı olur. En fazla RU/sn ne olduğuna bakılmaksızın, sistem her zaman ile arasında ölçeklendirecektir `0.1 * Tmax` `Tmax` . 

### <a name="how-does-ttl-work-with-autoscale"></a>TTL otomatik ölçeklendirme ile nasıl çalışır?
Otomatik ölçeklendirme ile TTL işlemleri RU/s ölçeklendirmeyi etkilemez. TTL nedeniyle tüketilen tüm RUs, otomatik ölçeklendirme kapsayıcısının faturalandırılan RU/sn 'nin bir parçası değildir. 

Örneğin, 400 – 4000 RU/s ile bir otomatik ölçeklendirme kapsayıcısına sahip olduğunuzu varsayalım:
- Saat 1: T = 0: kapsayıcıda kullanım yok (TTL veya iş yükü isteği yok). Faturalanabilir RU/s, 400 RU/sn 'dir.
- Saat 1: T = 1: TTL etkin.
- Saat 1: T = 2: kapsayıcı, 1 saniye içinde 1000 RU kullanan istekleri almaya başlar. Ayrıca, olması gereken TTL 'nin 200 ' luk bir yanı da vardır. Faturalanabilir RU/s hala 1000 RU/sn 'dir. TTLs ne zaman meydana geldiğinde bağımsız olarak, otomatik ölçeklendirme ölçekleme mantığını etkilemez.

### <a name="what-is-the-mapping-between-the-max-rus-and-physical-partitions"></a>En büyük RU/s ve fiziksel bölümler arasındaki eşleme nedir?
En büyük RU/s 'yi ilk seçtiğinizde Azure Cosmos DB sağlayacak: en fazla RU/s/10.000 RU/s = fiziksel bölüm sayısı. Her [fiziksel bölüm](partition-data.md#physical-partitions) , 10.000 ru/sn ve 50 GB depolama alanı destekleyebilir. Depolama boyutu büyüdükçe Azure Cosmos DB, depolama artışını işlemek için bölümleri daha fazla fiziksel bölüm eklemek üzere otomatik olarak böler veya depolama [ilişkili sınırı aşarsa](#what-is-the-storage-limit-associated-with-each-max-rus-option)en büyük ru/sn 'yi arttırır. 

Veritabanının veya kapsayıcının en fazla RU/sn değeri, tüm fiziksel bölümlerde eşit olarak bölünür. Bu nedenle, tek bir fiziksel bölümün her biri için ölçeklenebilen toplam verimlilik: veritabanı veya kapsayıcı/# fiziksel bölüm için en fazla RU/sn. 

### <a name="what-happens-if-incoming-requests-exceed-the-max-rus-of-the-database-or-container"></a>Gelen istekler veritabanının veya kapsayıcının en büyük RU/sn boyutunu aşarsa ne olur?
Toplam kullanılan RU/s, veritabanının veya kapsayıcının en fazla RU/sn değerini aşarsa, en fazla RU/sn 'yi aşan istekler kısıtlanacaktır ve 429 durum kodu döndürür. %100 normalleştirilmiş kullanım üzerinden elde edilen istekler de kısıtlanacak. Normalleştirilmiş kullanım, tüm fiziksel bölümlerde RU/sn kullanımının en yüksek değeri olarak tanımlanır. Örneğin, en fazla aktarım hızı 20.000 RU/sn olduğunu ve her biri 10.000 RU/sn 'ye ölçeklendirebileceğiniz P_1 ve P_2 iki fiziksel bölüme sahip olduğunuzu varsayalım. Belirli bir saniye içinde P_1 6000 ru ve P_2 8000 ru kullanıyorsa, normalleştirilmiş kullanım en fazla (6000 RU/10.000 RU, 8000 RU/10.000 RU) = 0,8 olur.

> [!NOTE]
> Azure Cosmos DB istemci SDK 'Ları ve veri içe aktarma araçları (Azure Data Factory, toplu yürütücü kitaplığı) otomatik olarak 429s üzerinde yeniden denense de, zaman zaman 429s iyi durumda. Sürekli olarak yüksek sayıda 429s, en fazla RU/sn 'yi artırmanız veya bir [sık kullanılan bölüm](#autoscale-rate-limiting)için bölümleme stratejinizi gözden geçirmeniz gerektiğini gösteriyor olabilir.

### <a name="is-it-still-possible-to-see-429s-throttlingrate-limiting-when-autoscale-is-enabled"></a><a id="autoscale-rate-limiting"></a>Otomatik ölçeklendirme etkinleştirildiğinde 429s (daraltma/hız sınırlaması) görmeyi hala mümkün mü? 
Evet. 429s 'yi iki senaryoda görmek mümkündür. İlk olarak, genel olarak tüketilen RU/s, veritabanının veya kapsayıcının en fazla RU/sn boyutunu aşarsa, hizmet istekleri buna uygun şekilde azaltır. 

İkinci olarak, bir sıcak bölüm varsa (yani, diğer bölüm anahtarı değerleriyle karşılaştırılan daha yüksek bir istek miktarına sahip bir mantıksal bölüm anahtarı değeri), temeldeki fiziksel bölümün RU/s bütçesini aşması mümkündür. En iyi uygulama olarak, sık kullanılan bölümlerin önüne geçmek için, depolama ve aktarım hızı için eşit bir dağıtım ile sonuçlanan [iyi bir bölüm anahtarı seçin](partitioning-overview.md#choose-partitionkey) . 

Örneğin, 20.000 RU/s en fazla aktarım hızı seçeneğini belirlerseniz ve dört fiziksel bölümden oluşan 200 GB depolama alanı varsa, her fiziksel bölüm, 5000 RU/sn 'ye kadar hızla ölçeklendirilebilir. Belirli bir mantıksal bölüm anahtarında sık erişimli bir bölüm varsa, içindeki temeldeki fiziksel bölüm 5000 RU/s 'yi aşarsa, bu, %100 normalleştirilmiş kullanımı aşmaktadır.


## <a name="next-steps"></a>Sonraki adımlar

* [Azure Cosmos DB veritabanında veya kapsayıcıda otomatik ölçeklendirmeyi nasıl etkinleştireceğinizi](how-to-provision-autoscale-throughput.md)öğrenin.
* [Otomatik ölçeklendirme ile sağlanan aktarım hızının avantajları](provision-throughput-autoscale.md#benefits-of-autoscale)hakkında bilgi edinin.
* [Mantıksal ve fiziksel bölümler](partition-data.md)hakkında daha fazla bilgi edinin.
                        
