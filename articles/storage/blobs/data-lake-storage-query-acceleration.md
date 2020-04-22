---
title: Azure Veri Gölü Depolama sorgu hızlandırma (önizleme)
description: Sorgu hızlandırma (önizleme), yalnızca bir işleme işlemi için gereken verileri alarak uygulamaların ve analiz çerçevelerinin veri işlemeyi önemli ölçüde optimize etmesini sağlayan Azure Veri Gölü Depolaması için yeni bir özelliktir.
author: normesta
ms.topic: conceptual
ms.author: normesta
ms.reviewer: jamesbak
ms.date: 04/21/2020
ms.service: storage
ms.subservice: data-lake-storage-gen2
ms.openlocfilehash: 191a3280075403c8c5b57c5ffca1c7707d1ddb11
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81771826"
---
# <a name="azure-data-lake-storage-query-acceleration-preview"></a>Azure Veri Gölü Depolama sorgu hızlandırma (önizleme)

Sorgu hızlandırma (önizleme), uygulamaların ve analiz çerçevelerinin yalnızca belirli bir işlemi gerçekleştirmek için gereksinim duydukları verileri alarak veri işlemeyi önemli ölçüde optimize etmesini sağlayan Azure Veri Gölü Depolaması için yeni bir özelliktir. Bu, depolanan veriler hakkında kritik bilgiler elde etmek için gereken zamanı ve işlem gücünü azaltır.

> [!NOTE]
> Sorgu hızlandırma özelliği genel önizlemededir ve Kanada Orta ve Fransa Orta bölgelerinde kullanılabilir. Sınırlamaları gözden geçirmek için [Bilinen sorunlar](data-lake-storage-known-issues.md) makalesine bakın. Önizlemeye kaydolmak için [bu forma](https://aka.ms/adls/qa-preview-signup)bakın.  

## <a name="overview"></a>Genel Bakış

Sorgu hızlandırma, uygulamaların verilerin diskten okunduğu anda satır ve sütunlara filtre uygulamasına olanak tanıyan filtreleme *yüklemlerini* ve *sütun projeksiyonlarını* kabul eder. Yalnızca bir yüklemin koşullarını karşılayan veriler ağ üzerinden uygulamaya aktarılır. Bu, ağ gecikmesi ve işlem maliyetini azaltır.  

Sorgu hızlandırma isteğinde satır filtresi yüklemlerini ve sütun projeksiyonlarını belirtmek için SQL'i kullanabilirsiniz. İstek yalnızca bir dosyayı işler. Bu nedenle, sql'in birleştirmeler ve toplu olarak gruplandırma gibi gelişmiş ilişkisel özellikleri desteklenmez. Sorgu hızlandırma, CSV ve JSON biçimlendirilmiş verileri her isteğe giriş olarak destekler.

Sorgu hızlandırma özelliği, Veri Gölü Depolama (üzerinde hiyerarşik ad alanı etkinleştirilmiş depolama hesapları) ile sınırlı değildir. Sorgu hızlandırma, üzerinde hiyerarşik ad alanı **don't** olmayan depolama hesaplarındaki lekelerle tamamen uyumludur. Bu, depolama hesaplarında zaten blob olarak depoladığınız verileri işlediğinizde ağ gecikmesi ve işlem maliyetlerinde de aynı düşüşü elde edebileceğiniz anlamına gelir.

İstemci uygulamasında sorgu ivmesinin nasıl kullanılacağına bir örnek olarak, [Bkz. Azure Veri Gölü Depolama sorgu ivmesini kullanarak verileri filtreleyin.](data-lake-storage-query-acceleration-how-to.md)

## <a name="data-flow"></a>Veri akışı

Aşağıdaki diyagram, tipik bir uygulamanın verileri işlemek için sorgu ivmesini nasıl kullandığını göstermektedir.

> [!div class="mx-imgBorder"]
> ![Sorgu hızlandırma ya da hızlanma genel bakış](./media/data-lake-storage-query-acceleration/query-acceleration.png)

1. İstemci uygulaması yüklemleri ve sütun projeksiyonları belirterek dosya verilerini ister.

2. Sorgu hızlandırma, belirtilen SQL sorgusunu ayrıştırır ve çalışmayı ayrıştırMak ve verileri filtrelemek için dağıtır.

3. İşlemciler diskteki verileri okur, verileri uygun biçimi kullanarak ayrıştırır ve ardından belirtilen yüklemleri ve sütun projeksiyonlarını uygulayarak verileri filtreler.

4. Sorgu hızlandırma istemci uygulamasına geri akışı için yanıt parçaları birleştirir.

5. İstemci uygulaması akışlı yanıtı alır ve ayrışdırır. Uygulamanın ek verileri filtrelemesi gerekmez ve istenen hesaplamaveya dönüşümü doğrudan uygulayabilir.

## <a name="better-performance-at-a-lower-cost"></a>Daha düşük maliyetle daha iyi performans

Sorgu hızlandırma, uygulamanız tarafından aktarılan ve işlenen veri miktarını azaltarak performansı en iyi duruma getirin.

Toplanan bir değeri hesaplamak için, uygulamalar genellikle bir dosyadan **tüm** verileri alır ve sonra verileri yerel olarak işleyip filtreler. Analitik iş yüklerine ilişkin giriş/çıktı desenlerinin analizi, uygulamaların belirli bir hesaplamayı gerçekleştirmek için genellikle okudukları verilerin yalnızca %20'sini gerektirdiğini ortaya koymaktadır. Bu istatistik [bölüm budama](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-optimize-hive-query#hive-partitioning)gibi teknikler uyguladıktan sonra bile doğrudur. Bu, bu verilerin %80'inin gereksiz yere ağ üzerinden aktarıldığı, ayrıştırıldığı ve uygulamalar tarafından filtreedildiği anlamına gelir. Bu desen, temelde gereksiz verileri kaldırmak için tasarlanmış, önemli bir işlem maliyeti ne tabi tutar.  

Azure, hem iş hem de gecikme süresi açısından sektör lideri bir ağa sahip olsa da, gereksiz yere bu ağ üzerinden veri aktarımı uygulama performansı için hala maliyetlidir. Depolama isteği sırasında istenmeyen verileri filtreleyerek sorgu hızlandırma bu maliyeti ortadan kaldırır.

Ayrıca, gereksiz verileri ayrıştırmak ve filtrelemek için gereken CPU yükü, uygulamanızın çalışmasını yapabilmesi için daha fazla sayıda ve daha büyük VM'ler sağlamasını gerektirir. Bu işlem yükünü sorgu hızlandırmaya aktararak, uygulamalar önemli maliyet tasarrufları gerçekleştirebilir.

## <a name="applications-that-can-benefit-from-query-acceleration"></a>Sorgu hızlandırmadan yararlanabilecek uygulamalar

Sorgu hızlandırma, dağıtılmış analiz çerçeveleri ve veri işleme uygulamaları için tasarlanmıştır. 

Apache Spark ve Apache Hive gibi dağıtılmış analiz çerçeveleri, çerçeve içinde bir depolama soyutlama katmanı içerir. Bu altyapılar, kullanıcı sorguları için en uygun sorgu planını belirlerken temel G/Ç hizmetinin yetenekleri hakkında bilgi edinebilen sorgu optimize edicileri de içerir. Bu çerçeveler sorgu ivmesini tümleştirmeye başlıyor. Sonuç olarak, bu çerçevelerin kullanıcıları sorgularda değişiklik yapmak zorunda kalmadan geliştirilmiş sorgu gecikmesi ve daha düşük toplam sahip olma maliyeti görür. 

Sorgu hızlandırma da veri işleme uygulamaları için tasarlanmıştır. Bu tür uygulamalar genellikle, yerleşik dağıtılmış analitik çerçevelerini her zaman kullanmamalarından doğrudan analitik öngörülere yol açmayacak büyük ölçekli veri dönüşümleri gerçekleştirir. Bu uygulamalar genellikle temel depolama hizmetiyle daha doğrudan bir ilişkiye sahiptir, böylece sorgu hızlandırma gibi özelliklerden doğrudan yararlanabilirler. 

Bir uygulamanın sorgu ivmesini nasıl tümleştirebileceğine bir örnek olarak, [Bkz. Azure Veri Gölü Depolama sorgu ivmesini kullanarak verileri filtreleyin.](data-lake-storage-query-acceleration-how-to.md)

## <a name="pricing"></a>Fiyatlandırma

Azure Veri Gölü Depolama hizmetindeki artan bilgi işlem yükü nedeniyle, sorgu ivmesini kullanmak için fiyatlandırma modeli normal Azure Veri Gölü Depolama işlem modelinden farklıdır. Sorgu hızlandırma, taranan veri miktarı için bir maliyet ve arayanın döndürülen veri miktarı için bir maliyet yükler.

Faturalandırma modelindeki değişikliğe rağmen, Sorgu hızlandırmanın fiyatlandırma modeli, çok daha pahalı VM maliyetlerindeki azalma göz önüne alındığında, iş yükünün toplam sahip olma maliyetini düşürmek üzere tasarlanmıştır.

## <a name="next-steps"></a>Sonraki adımlar

- [Sorgu hızlandırma kayıt formu](https://aka.ms/adls/qa-preview-signup)    
- [Azure Veri Gölü Depolama sorgu ivmesi kullanarak verileri filtreleme](data-lake-storage-query-acceleration-how-to.md)
- [Sorgu hızlandırma SQL dil başvurusu (önizleme)](query-acceleration-sql-reference.md)
- Sorgu ivmesi REST API başvurusu



