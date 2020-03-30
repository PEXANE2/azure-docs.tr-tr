---
title: Azure Cosmos DB'ye Azure Akış Analizi çıktısı
description: Bu makalede, yorumlanmamış JSON verilerinde veri arşivleme ve düşük gecikme li sorgular için, JSON çıktısı için Azure Cosmos DB'ye çıktı kaydetmek için Azure Akış Analizi'nin nasıl kullanılacağı açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 02/2/2020
ms.custom: seodec18
ms.openlocfilehash: e58e36b3caa5a5ecd137cb9cb61dad7ddb95ff3a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79254449"
---
# <a name="azure-stream-analytics-output-to-azure-cosmos-db"></a>Azure Cosmos DB'ye Azure Akış Analizi çıktısı  
Azure Akış Analizi, JSON çıktısı için [Azure Cosmos DB'yi](https://azure.microsoft.com/services/documentdb/) hedefleyerek yapılandırılmamış JSON verilerinde veri arşivleme ve düşük gecikme li sorguları sağlar. Bu belge, bu yapılandırmayı uygulamak için bazı en iyi uygulamaları kapsar.

Azure Cosmos DB'ye aşina değilseniz, başlamak için [Azure Cosmos DB belgelerine](https://docs.microsoft.com/azure/cosmos-db/) bakın. 

> [!Note]
> Şu anda, Stream Analytics Azure Cosmos DB'ye yalnızca *SQL API*üzerinden bağlantı destekler.
> Diğer Azure Cosmos DB API'leri henüz desteklenmez. Akış Analizi'ni diğer API'lerle oluşturulan Azure Cosmos DB hesaplarına yönlendirerseniz, veriler düzgün depolanamayabilir. 

## <a name="basics-of-azure-cosmos-db-as-an-output-target"></a>Çıktı hedefi olarak Azure Cosmos DB'nin temelleri
Stream Analytics'teki Azure Cosmos DB çıkışı, Akış işleme sonuçlarınızı, Azure Cosmos DB kapsayıcılarınıza JSON çıktısı olarak yazmanızı sağlar. 

Akış Analizi veritabanınızda kapsayıcılar oluşturmaz. Bunun yerine, bunları önde oluşturmanızı gerektirir. Daha sonra Azure Cosmos DB kapsayıcılarının fatura maliyetlerini denetleyebilirsiniz. Ayrıca, [Azure Cosmos DB API'lerini](https://msdn.microsoft.com/library/azure/dn781481.aspx)kullanarak kapsayıcılarınızın performansını, tutarlılığını ve kapasitesini doğrudan ayarlayabilirsiniz.

> [!Note]
> Azure Cosmos DB güvenlik duvarınızdan izin verilen IP'ler listesine 0.0.0.0 eklemeniz gerekir.

Aşağıdaki bölümlerde Azure Cosmos DB için bazı kapsayıcı seçenekleri ayrıntılı olarak anlatılmaktadır.

## <a name="tuning-consistency-availability-and-latency"></a>Tutarlılık, kullanılabilirlik ve gecikme süresi
Azure Cosmos DB, uygulama gereksinimlerinizi karşılamak için veritabanında ve kapsayıcılarda ince ayar yapmanızı ve tutarlılık, kullanılabilirlik, gecikme süresi ve iş ortalığı arasında dengeler oluşturmanıza olanak tanır. 

Okuma ve yazma gecikmelerine karşı senaryonuzun hangi okuma tutarlılığı düzeylerine bağlı olarak, veritabanı hesabınızda bir tutarlılık düzeyi seçebilirsiniz. Kapsayıcıdaki İstek Birimlerini (RUS) ölçekleyerek iş elde etme yi artırabilirsiniz. 

Ayrıca varsayılan olarak, Azure Cosmos DB, her CRUD işleminde kapsayıcınıza eşzamanlı dizin oluşturma olanağı sağlar. Azure Cosmos DB'de yazma/okuma performansını denetlemek için kullanışlı bir seçenek daha budur. 

Daha fazla bilgi için [veritabanınızı değiştir ve sorgu tutarlılığı düzeyleri makalesini](../cosmos-db/consistency-levels.md) gözden geçirin.

## <a name="upserts-from-stream-analytics"></a>Stream Analytics'ten ekler
Azure Cosmos DB ile Akış Analizi tümleştirmesi, belirli bir **Belge Kimliği** sütununa bağlı olarak kapsayıcınıza kayıt eklemenize veya güncelleştirmenize olanak tanır. Bu da bir *upsert*denir.

Stream Analytics iyimser bir yükseltme yaklaşımı kullanır. Güncelleştirmeler yalnızca bir ekleme belgesi kimliği çakışmasında başarısız olduğunda gerçekleşir. 

Uyumluluk düzeyi 1.0 ile Stream Analytics bu güncelleştirmeyi bir PATCH işlemi olarak gerçekleştirir, böylece belgeye kısmi güncelleştirmeler sağlar. Akış Analizi yeni özellikler ekler veya varolan bir özelliği n aşamalı olarak değiştirir. Ancak, JSON belgenizdeki dizi özelliklerinin değerlerindeki değişiklikler tüm dizinin üzerine yazılmasıyla sonuçlanır. Diğer bir diğer, dizi birleştirilmiş değil. 

1.2 ile, belgeyi eklemek veya değiştirmek için yükseltme davranışı değiştirilir. Uyumluluk düzeyi 1.2 ile ilgili sonraki bölümde bu davranışı daha da açıklar.

Gelen JSON belgesinde varolan bir kimlik alanı varsa, bu alan otomatik olarak Azure Cosmos DB'deki **Belge Kimliği** sütunu olarak kullanılır. Sonraki tüm yazılar bu şekilde ele alınır ve bu durumlardan birine yol açan durumlar:

- Benzersiz iLikler eklemeye yol açar.
- Yinelenen Kimlikler ve **Kimlik** olarak ayarlanmış **Belge Kimliği,** yukarıya yol açar.
- Yinelenen Kimlikler ve **Belge Kimliği,** ilk belgeden sonra hataya yol açmaz.

Yinelenen kimliği olanlar da dahil olmak üzere *tüm* belgeleri kaydetmek istiyorsanız, sorgunuzdaki kimlik alanını yeniden adlandırın **(AS** anahtar sözcük tuşunu kullanarak). Azure Cosmos DB'nin kimlik alanını oluşturmasına veya kimliği başka bir sütunun değeriyle değiştirmesine izin verin **(AS** anahtar sözcük kullanarak veya **Belge Kimliği** ayarını kullanarak).

## <a name="data-partitioning-in-azure-cosmos-db"></a>Azure Cosmos DB'de veri bölümleme
Azure Cosmos DB, bölümleri iş yükünüzü temel alınarak otomatik olarak ölçeklendirin. Bu nedenle, verilerinizi bölümleme yaklaşımı olarak [sınırsız](../cosmos-db/partition-data.md) kapsayıcılar öneririz. Akış Analizi sınırsız kapsayıcılara yazdığında, önceki sorgu adımı veya giriş bölümleme şeması kadar paralel yazar kullanır.

> [!NOTE]
> Azure Akış Analizi, en üst düzeyde bölüm anahtarlarıyla yalnızca sınırsız kapsayıcıları destekler. Örneğin, `/region` desteklenir. İç içe bölüm tuşları `/region/name`(örneğin,) desteklenmez. 

Bölüm tuşu seçtiğiniz bağlı olarak, bu _uyarı_alabilirsiniz:

`CosmosDB Output contains multiple rows and just one row per partition key. If the output latency is higher than expected, consider choosing a partition key that contains at least several hundred records per partition key.`

Bir dizi farklı değere sahip ve iş yükünüzü bu değerler arasında eşit olarak dağıtmanıza olanak tanıyan bir bölüm anahtarı özelliği seçmek önemlidir. Doğal bir bölümleme yapı olarak, aynı bölüm anahtarını içeren istekler tek bir bölümün maksimum verimi ile sınırlıdır. 

Aynı bölüm anahtarına ait belgelerin depolama boyutu 10 GB ile sınırlıdır. İdeal bölüm anahtarı, sorgularınızda filtre olarak sık sık görünen ve çözümünüzün ölçeklenebilir olduğundan emin olmak için yeterli kardinalliğe sahip olan anahtardır.

Bölüm anahtarı, Azure Cosmos DB için depolanan yordamlar ve tetikleyicilerdeki hareketler için de sınırdır. Hareketlerde birlikte oluşan belgelerin aynı bölüm anahtar değerini paylaşması için bölüm anahtarını seçmelisiniz. Azure [Cosmos DB'de Bölümleme](../cosmos-db/partitioning-overview.md) makalesi, bölüm anahtarı seçme hakkında daha fazla ayrıntı verir.

Sabit Azure Cosmos DB kapsayıcıları için Stream Analytics, dolduktan sonra ölçeklendirmenin veya dışarı çıkmanın hiçbir yolu yoktur. Onlar 10 GB ve 10.000 RU / s ilik bir üst sınırı var. Verileri sabit bir kapsayıcıdan sınırsız bir kapsayıcıya geçirmek için (örneğin, en az 1.000 RU/s ve bir bölüm anahtarı olan bir tane), [veri geçiş aracını](../cosmos-db/import-data.md) veya [değişiklik akışı kitaplığını](../cosmos-db/change-feed.md)kullanın.

Birden çok sabit kapsayıcıya yazma yeteneği azat ediliyor. Stream Analytics işinizi ölçeklediğimiz için önermiyoruz.

## <a name="improved-throughput-with-compatibility-level-12"></a>Uyumluluk düzeyi 1.2 ile geliştirilmiş iş seviyesi
Uyumluluk düzeyi 1.2 ile Stream Analytics, Azure Cosmos DB'ye toplu olarak yazmak için yerel tümleştirmeyi destekler. Bu tümleştirme, azure cosmos DB'ye etkili bir şekilde yazmaolanağı sağlarken, iş ortasını en üst düzeye çıkarır ve azaltma isteklerini verimli bir şekilde ele alabilir. 

Geliştirilmiş yazma mekanizması, yükseltme davranışındaki farklılık nedeniyle yeni bir uyumluluk düzeyi altında kullanılabilir. 1.2'den önceki düzeylerde, yükseltme davranışı belgeyi eklemek veya birleştirmektir. 1.2 ile, belgeyi eklemek veya değiştirmek için yükseltme davranışı değiştirilir.

1,2'den önceki düzeyleriyle, Akış Analizi, azure Cosmos DB'ye bölüm anahtarı başına belgeleri toplu olarak eklemek için özel bir depolanmış yordam kullanır. Burada, bir toplu iş işlem olarak yazılır. Tek bir kayıt geçici bir hataya çarpsa (azaltma), tüm toplu işlem yeniden denenmek zorundadır. Bu nispeten yavaş bile makul azaltma ile senaryolar yapar.

Aşağıdaki örnekte, aynı Azure Etkinlik Hub'larından okunan iki özdeş Akış Analizi işi gösterilmektedir. Her iki Akış Analizi işi de bir geçiş sorgusuyla [tamamen bölümlere ayrılmıştır](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#embarrassingly-parallel-jobs) ve aynı Azure Cosmos DB kapsayıcılarına yazar. Soldaki ölçümler uyumluluk düzeyi 1.0 ile yapılandırılan iş vardır. Sağdaki ölçümler 1.2 ile yapılandırılır. Azure Cosmos DB kapsayıcının bölüm anahtarı, giriş olayından gelen benzersiz bir GUID'dir.

![Akış Analizi ölçümlerinin karşılaştırılması](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-3.png)

Etkinlik Hub'larında gelen olay oranı, Azure Cosmos DB kapsayıcılarının (20.000 RUs) kabul edilebilmek üzere yapılandırıldığından iki kat daha yüksektir, bu nedenle Azure Cosmos DB'de azaltma beklenmektedir. Ancak, 1.2 ile iş sürekli daha yüksek bir verim (dakikada çıkış olayları) ve daha düşük bir ortalama SU% kullanımı ile yazıyor. Çevrenizde, bu fark birkaç daha fazla faktöre bağlıdır. Bu etkenler arasında olay biçimi seçimi, giriş olayı/ileti boyutu, bölüm anahtarları ve sorgu yer almaktadır.

![Azure Cosmos DB ölçümleri karşılaştırması](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-2.png)

1.2 ile Stream Analytics, Azure Cosmos DB'deki kullanılabilir iş akışının yüzde 100'ünü kullanmada daha akıllıdır ve azaltma veya hız sınırlaması konusunda çok az yeniden gönderme yapılır. Bu, kapsayıcıda aynı anda çalışan sorgular gibi diğer iş yükleri için daha iyi bir deneyim sağlar. Akış Analizi'nin Azure Cosmos DB ile saniyede 1.000 ila 10.000 ileti için lavabo olarak nasıl ölçeklendirildirtiğini görmek istiyorsanız, [bu Azure örnek projesini](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-cosmosdb)deneyin.

Azure Cosmos DB çıktısının 1.0 ve 1.1 ile aynıdır. Azure Cosmos DB ile Stream Analytics'te uyumluluk düzeyi 1.2'yi kullanmanızı *şiddetle öneririz.*

## <a name="azure-cosmos-db-settings-for-json-output"></a>JSON çıkışı için Azure Cosmos DB ayarları

Stream Analytics'te Azure Cosmos DB'nin çıktı olarak kullanılması, bilgi için aşağıdaki istem'i oluşturur.

![Azure Cosmos DB çıkış akışı için bilgi alanları](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-1.png)

|Alan           | Açıklama|
|-------------   | -------------|
|Çıktı diğer adı    | Akış Analizi sorgunuzda bu çıktıya başvurmak için bir takma ad.|
|Abonelik    | Azure aboneliği.|
|Hesap Kimliği      | Azure Cosmos DB hesabının adı veya bitiş noktası URI.|
|Hesap anahtarı     | Azure Cosmos DB hesabının paylaşılan erişim anahtarı.|
|Database        | Azure Cosmos DB veritabanı adı.|
|Kapsayıcı adı | Kapsayıcı adı, örneğin `MyContainer`. Adlandırılmış `MyContainer` bir kapsayıcı olmalıdır.  |
|Belge Kimliği     | İsteğe bağlı. Ekleme veya güncelleştirme işlemlerinin temel alınması gereken benzersiz anahtar olarak kullanılan çıktı olaylarındaki sütun adı. Boş bırakırsanız, güncelleştirme seçeneği olmadan tüm olaylar eklenir.|

Azure Cosmos DB çıktısını yapılandırdıktan sonra, sorguda INTO [deyiminin](https://docs.microsoft.com/stream-analytics-query/into-azure-stream-analytics)hedefi olarak kullanabilirsiniz. Bir Azure Cosmos DB çıktısını bu şekilde kullanıyorsanız, [bir bölüm anahtarının açıkça ayarlanması gerekir.](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#partitions-in-sources-and-sinks) 

Çıktı kaydı, Azure Cosmos DB'deki bölüm anahtarından sonra adında niçin büyük/küçük harf duyarlı bir sütun içermelidir. Daha fazla paralelleştirme elde etmek için deyim, aynı sütunu kullanan bir [PARTITION BY yan tümcesi](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#embarrassingly-parallel-jobs) gerektirebilir.

Örnek bir sorgu aşağıda veda edebilirsiniz:

```SQL
    SELECT TollBoothId, PartitionId
    INTO CosmosDBOutput
    FROM Input1 PARTITION BY PartitionId
``` 

## <a name="error-handling-and-retries"></a>Hata işleme ve yeniden deneme

Akış Analizi etkinlikleri Azure Cosmos DB'ye gönderirken geçici bir hata, hizmet kullanılabilirliği veya azaltma gerçekleşirse, Stream Analytics işlemi başarıyla bitirmek için süresiz olarak yeniden çalışır. Ancak aşağıdaki hatalar için yeniden deneme girişiminde bulunmaz:

- Yetkisiz (HTTP hata kodu 401)
- NotFound (HTTP hata kodu 404)
- Yasak (HTTP hata kodu 403)
- BadRequest (HTTP hata kodu 400)
