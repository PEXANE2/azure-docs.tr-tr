---
title: Azure Cosmos DB için Azure Stream Analytics çıkışı
description: Bu makalede, çıkış veri arşivleme ve düşük gecikme süreli sorgular yapılandırılmamış JSON verileri için bir JSON çıkışı için Azure Cosmos DB'ye kaydetmek için Azure Stream Analytics kullanmayı açıklar.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 02/2/2020
ms.custom: seodec18
ms.openlocfilehash: e58e36b3caa5a5ecd137cb9cb61dad7ddb95ff3a
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/04/2020
ms.locfileid: "76986997"
---
# <a name="azure-stream-analytics-output-to-azure-cosmos-db"></a>Azure Cosmos DB için Azure Stream Analytics çıkışı  
Azure Stream Analytics, veri arşivlemeyi ve yapılandırılmamış JSON verilerinde düşük gecikme süreli sorguları etkinleştirerek JSON çıkışı için [Azure Cosmos DB](https://azure.microsoft.com/services/documentdb/) hedefleyebilir. Bu belge, bu yapılandırmayı uygulamak için bazı en iyi uygulamaları kapsar.

Azure Cosmos DB hakkında bilgi sahibi değilseniz, kullanmaya başlamak için [Azure Cosmos DB belgelerine](https://docs.microsoft.com/azure/cosmos-db/) bakın. 

> [!Note]
> Şu anda Stream Analytics, yalnızca *SQL API 'si*aracılığıyla Azure Cosmos DB bağlantısını destekler.
> Diğer Azure Cosmos DB API henüz desteklenmiyor. Diğer API 'lerle oluşturulan Azure Cosmos DB hesaplara Stream Analytics işaret ediyorsanız, veriler düzgün şekilde depolanmayabilir. 

## <a name="basics-of-azure-cosmos-db-as-an-output-target"></a>Çıkış hedefi olarak Azure Cosmos DB temelleri
Stream Analytics Azure Cosmos DB çıktısı, akış işleme sonuçlarınızı Azure Cosmos DB kapsayıcılarınızın JSON çıktısı olarak yazılmasına izin verebilir. 

Stream Analytics, veritabanınızda kapsayıcı oluşturmaz. Bunun yerine, bunları baştan oluşturmanız gerekir. Daha sonra Azure Cosmos DB kapsayıcıların faturalandırma maliyetlerini kontrol edebilirsiniz. Ayrıca, [Azure Cosmos DB API 'lerini](https://msdn.microsoft.com/library/azure/dn781481.aspx)kullanarak doğrudan kapsayıcılarınızın performans, tutarlılık ve kapasitesini ayarlayabilirsiniz.

> [!Note]
> Azure Cosmos DB güvenlik duvarınızdan izin verilen IP 'Ler listesine 0.0.0.0 eklemeniz gerekir.

Aşağıdaki bölümler Azure Cosmos DB kapsayıcı seçeneklerinden bazılarını ayrıntılandırır.

## <a name="tuning-consistency-availability-and-latency"></a>Tutarlılık, kullanılabilirlik ve gecikme ayarlama
Uygulama gereksinimlerinizi karşılamak için, Azure Cosmos DB veritabanı ve kapsayıcıları ince ayarlamanıza ve tutarlılık, kullanılabilirlik, gecikme süresi ve aktarım hızı arasında denge sağlamanıza olanak tanır. 

Okuma tutarlılık düzeylerini bağlı olarak senaryo gereksinimlerinize yönelik Okuma ve yazma gecikme süresi, tutarlılık düzeyi veritabanı hesabınızı seçin. Kapsayıcıda Istek birimleri (ru) ölçeğini ölçeklendirerek aktarım hızını artırabilirsiniz. 

Ayrıca, Azure Cosmos DB, kapsayıcılarınızın her bir CRUD işleminde zaman uyumlu dizin oluşturmayı sağlar. Bu, Azure Cosmos DB 'de yazma/okuma performansını denetlemek için başka bir kullanışlı seçenektir. 

Daha fazla bilgi için, [veritabanınızı değiştirme ve tutarlılık düzeylerini sorgulama](../cosmos-db/consistency-levels.md) makalesini inceleyin.

## <a name="upserts-from-stream-analytics"></a>Stream Analytics'ten alınan upsert eder
Azure Cosmos DB tümleştirmesi Stream Analytics, belirli bir **belge kimliği** sütununa göre kapsayıcınıza kayıt eklemenize veya güncelleştirmenize olanak tanır. Bu da *upsert*olarak adlandırılır.

Stream Analytics iyimser bir yaklaşım kullanır. Güncelleştirmeler yalnızca bir ekleme bir belge KIMLIĞI çakışmasıyla başarısız olduğunda gerçekleşir. 

Uyumluluk düzeyi 1,0 ile, Stream Analytics bu güncelleştirmeyi bir düzeltme eki işlemi olarak gerçekleştirir, bu nedenle belgede Kısmi güncelleştirme yapılmasını sağlar. Stream Analytics yeni özellikler ekler veya var olan bir özelliği artımlı olarak değiştirir. Ancak, JSON belgenizdeki dizi özelliklerinin değerlerinde yapılan değişiklikler tüm dizinin üzerine yazılmasına neden olacak. Diğer bir deyişle, dizi birleştirilmez. 

1,2 ile, büyük bir davranış belgeyi eklemek veya değiştirmek için değiştirilir. Uyumluluk düzeyi 1,2 ile ilgili sonraki bölümde bu davranış açıklanmaktadır.

Gelen JSON belgesinde mevcut bir KIMLIK alanı varsa, bu alan Azure Cosmos DB **belge kimliği** sütunu olarak otomatik olarak kullanılır. Sonraki tüm yazmalar, bu durumlardan birine göre önde işlenir:

- Benzersiz kimlikler ekleme ile sonuçlanabilmektedir.
- Yinelenen kimlikler ve **belge kimliği** , **kimlik** lideri olarak upsert olarak ayarlanmıştır.
- Yinelenen kimlikler ve **belge kimliği** , ilk belgeden sonra hataya neden olarak ayarlanmadı.

Yinelenen KIMLIĞE sahip olanlar da dahil olmak üzere *Tüm* belgeleri kaydetmek ISTIYORSANıZ sorgunuzun ID alanını ( **as** anahtar sözcüğünü kullanarak) yeniden adlandırın. KIMLIK alanı Azure Cosmos DB oluşturma veya KIMLIĞI başka bir sütunun değeriyle değiştirme ( **as** anahtar sözcüğünü kullanarak veya **belge kimliği** ayarını kullanarak).

## <a name="data-partitioning-in-azure-cosmos-db"></a>Azure Cosmos DB veri bölümleme
Azure Cosmos DB, iş yükünüze göre bölümleri otomatik olarak ölçeklendirir. Bu nedenle, verilerinizi bölümlemeye yönelik yaklaşım olarak [sınırsız](../cosmos-db/partition-data.md) kapsayıcı önerilir. Stream Analytics sınırsız kapsayıcılara yazdığında, önceki sorgu adımı veya giriş bölümleme düzeni olarak çok sayıda paralel yazıcı kullanır.

> [!NOTE]
> Azure Stream Analytics en üst düzeyde yalnızca bölüm anahtarları olan sınırsız kapsayıcıları destekler. Örneğin, `/region` desteklenir. İç içe bölüm anahtarları (örneğin, `/region/name`) desteklenmez. 

Bölüm anahtarınız seçiminize bağlı olarak şu _uyarıyı_alabilirsiniz:

`CosmosDB Output contains multiple rows and just one row per partition key. If the output latency is higher than expected, consider choosing a partition key that contains at least several hundred records per partition key.`

Farklı değerlere sahip bir bölüm anahtarı özelliği seçmeniz ve iş yükünüzü bu değerler arasında eşit bir şekilde dağıtmanıza olanak sağlamak önemlidir. Doğal bir bölümlendirme yapıtı olarak, aynı bölüm anahtarını içeren istekler tek bir bölümün en yüksek aktarım hızı ile sınırlıdır. 

Aynı bölüm anahtarına ait olan belgelerin depolama boyutu 10 GB ile sınırlıdır. İdeal bölüm anahtarı, sorgularda filtre olarak sık görülen ve çözümünüzün ölçeklenebilir olduğundan emin olmak için yeterli kardinalite olan bir bölümdür.

Bölüm anahtarı Ayrıca, saklı yordamlar ve Azure Cosmos DB tetikleyicilerinde bulunan işlemler için de sınır olur. İşlem içinde birlikte oluşan belgelerin aynı bölüm anahtarı değerini paylaşması için bölüm anahtarını seçmeniz gerekir. Azure Cosmos DB bölümünde [bulunan](../cosmos-db/partitioning-overview.md) makale, bölüm anahtarı seçme hakkında daha fazla ayrıntı sağlar.

Sabit Azure Cosmos DB kapsayıcıları için, Stream Analytics, tamamen ölçeklendirilmesi veya bu kadar bir yönteme izin vermez. 10 GB ve 10.000 RU/sn 'lik bir üst sınıra sahiptir. Verileri sabit bir kapsayıcıdan sınırsız bir kapsayıcıya (örneğin, en az 1.000 RU/s ve bölüm anahtarıyla) geçirmek için [veri geçiş aracı](../cosmos-db/import-data.md) 'nı veya [değişiklik akışı kitaplığını](../cosmos-db/change-feed.md)kullanın.

Birden çok sabit kapsayıcıya yazma özelliği kullanım dışı bırakılıyor. Stream Analytics işinizi ölçeklendirmek için bunu önermiyoruz.

## <a name="improved-throughput-with-compatibility-level-12"></a>Uyumluluk düzeyi 1,2 ile iyileştirilmiş performans
Uyumluluk düzeyi 1,2 ile, Stream Analytics Azure Cosmos DB toplu olarak yazmak için yerel tümleştirmeyi destekler. Bu tümleştirme, işleme hızını en üst düzeye çıkararak ve daraltma isteklerini verimli bir şekilde işlerken Azure Cosmos DB için yazma özelliği sağlar 

Gelişmiş yazma mekanizması, büyük bir davranıştaki farklılık nedeniyle yeni bir uyumluluk düzeyi altında bulunabilir. 1,2 ' dan önceki düzeyler ile, büyük bir davranış belgeyi eklemek veya birleştirmek olur. 1,2 ile, büyük bir davranış belgeyi eklemek veya değiştirmek için değiştirilir.

1,2 ' dan önceki düzeyler sayesinde Stream Analytics, bölüm anahtarı başına belgeleri Azure Cosmos DB içine toplu olarak eklemek için özel bir saklı yordam kullanır. Burada bir Batch işlem olarak yazılır. Tek bir kayıt geçici bir hatayı (daraltma) azaltsa bile, tüm toplu işin yeniden denenmesi gerekir. Bu, senaryoları nispeten daha da düşük bir şekilde yavaş azaltarak kolaylaştırır.

Aşağıdaki örnekte aynı Azure Event Hubs girişinin okuduğu iki özdeş Stream Analytics işi gösterilmektedir. Stream Analytics işlerin her ikisi de bir PASSTHROUGH sorgusuyla [tamamen bölümlenir](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#embarrassingly-parallel-jobs) ve aynı Azure Cosmos DB kapsayıcılarına yazılır. Sol taraftaki ölçümler uyumluluk düzeyi 1,0 ile yapılandırılmış işten alınır. Sağdaki ölçümler 1,2 ile yapılandırılır. Azure Cosmos DB kapsayıcısının bölüm anahtarı, giriş olayından gelen benzersiz bir GUID 'dir.

![Stream Analytics ölçümlerinin karşılaştırması](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-3.png)

Event Hubs gelen olay hızı Azure Cosmos DB kapsayıcılardan (20.000 ru) en fazla iki kat daha yüksektir ve bu nedenle Azure Cosmos DB kısıtlama beklenir. Bununla birlikte, 1,2 ile iş düzenli olarak daha yüksek bir işleme (dakika başına çıktı olayları) ve düşük ortalama SU kullanımı ile bir yazma işlemi olur. Ortamınızda bu fark, daha fazla etkene göre farklılık gösterir. Bu faktörler, olay biçimi, giriş olayı/ileti boyutu, bölüm anahtarları ve sorgu seçimi içerir.

![Azure Cosmos DB ölçümleri karşılaştırması](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-2.png)

1,2 ile Stream Analytics, azaltma veya hız sınırlandırmasından çok az sayıda yeniden gönderme işlemi ile Azure Cosmos DB, kullanılabilir üretilen iş hızının yüzde 100 ' ü kullanarak daha akıllı bir işlem olur. Bu, kapsayıcıda aynı anda çalışan sorgular gibi diğer iş yükleri için daha iyi bir deneyim sağlar. Stream Analytics, saniyede 1.000 ile 10.000 ileti için bir havuz olarak Azure Cosmos DB nasıl ölçeklendirebileceğiniz hakkında bilgi edinmek istiyorsanız, [Bu Azure örnek projesini](https://github.com/Azure-Samples/streaming-at-scale/tree/master/eventhubs-streamanalytics-cosmosdb)deneyin.

Azure Cosmos DB çıkışının üretilen işi 1,0 ve 1,1 ile aynıdır. Uyumluluk düzeyi 1,2 ' i Azure Cosmos DB Stream Analytics kullanmanızı *kesinlikle öneririz* .

## <a name="azure-cosmos-db-settings-for-json-output"></a>JSON çıkışı için Azure Cosmos DB ayarları

Stream Analytics bir çıkış olarak Azure Cosmos DB kullanmak için aşağıdaki istem bilgileri oluşturulur.

![Azure Cosmos DB çıkış akışı için bilgi alanları](media/stream-analytics-documentdb-output/stream-analytics-documentdb-output-1.png)

|Alan           | Açıklama|
|-------------   | -------------|
|Çıkış diğer adı    | Stream Analytics sorgunuzda Bu çıktıya başvurmak için bir diğer ad.|
|Abonelik    | Azure aboneliği.|
|Hesap Kimliği      | Azure Cosmos DB hesabının adı veya uç nokta URI 'SI.|
|Hesap anahtarı     | Azure Cosmos DB hesabının paylaşılan erişim anahtarı.|
|Veritabanı        | Azure Cosmos DB veritabanı adı.|
|Kapsayıcı adı | Kapsayıcı adı, örneğin `MyContainer`. `MyContainer` adlı bir kapsayıcı var olmalıdır.  |
|Belge Kimliği     | İsteğe bağlı. Hangi ekleme veya güncelleştirme işlemleri dayanması benzersiz bir anahtar kullanılan çıkış olaylarındaki sütun adı. Boş bırakırsanız, tüm olaylar hiçbir güncelleştirme seçeneği olmadan eklenir.|

Azure Cosmos DB çıkışını yapılandırdıktan sonra, bunu sorgusunda bir [Into ifadesinin](https://docs.microsoft.com/stream-analytics-query/into-azure-stream-analytics)hedefi olarak kullanabilirsiniz. Bu şekilde bir Azure Cosmos DB çıkışı kullanırken, [bölüm anahtarının açıkça ayarlanması gerekir](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#partitions-in-sources-and-sinks). 

Çıkış kaydı, Azure Cosmos DB bölüm anahtarından sonra adlı büyük küçük harfe duyarlı bir sütun içermelidir. Daha büyük paralelleştirme elde etmek için, deyimi aynı sütunu kullanan bir [bölüm by yan tümcesi](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-parallelization#embarrassingly-parallel-jobs) gerektirebilir.

Örnek bir sorgu aşağıda verilmiştir:

```SQL
    SELECT TollBoothId, PartitionId
    INTO CosmosDBOutput
    FROM Input1 PARTITION BY PartitionId
``` 

## <a name="error-handling-and-retries"></a>Hata işleme ve yeniden deneme

Stream Analytics geçici bir hata, hizmet kullanılamaz veya daraltma işlemi, Azure Cosmos DB olay gönderirken, işlemi başarıyla tamamlamayı Stream Analytics. Ancak, aşağıdaki hatalarda yeniden deneme girişiminde yoktur:

- Yetkisiz (HTTP hata kodu 401)
- NotFound (HTTP hata kodu 404)
- Yasak (HTTP hata kodu 403)
- BadRequest (HTTP hata kodu 400)
