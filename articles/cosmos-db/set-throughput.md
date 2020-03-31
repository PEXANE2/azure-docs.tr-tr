---
title: Azure Cosmos kapsayıcıları ve veritabanlarında sağlama veri mişi
description: Azure Cosmos kapsayıcılarınız ve veritabanlarınız için sağlanan iş ortasını nasıl ayarlayamaize cereyiş yapılacağını öğrenin.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/12/2019
ms.openlocfilehash: e7a64776cba00a6840af70cecad5bf9c02b3f38e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79251979"
---
# <a name="provision-throughput-on-containers-and-databases"></a>Kapsayıcı ve veritabanlarına aktarım hızı sağlama

Azure Cosmos veritabanı, bir kapsayıcı kümesi nin yönetim birimidir. Veritabanı şema-agnostik kapsayıcılar kümesinden oluşur. Azure Cosmos kapsayıcısı, hem iş hem de depolama için ölçeklenebilirlik birimidir. Kapsayıcı, Azure bölgesindeki bir makine kümesine yatay olarak bölümlenir ve Azure Cosmos hesabınızla ilişkili tüm Azure bölgelerine dağıtılır.

Azure Cosmos DB ile, iki ayrıntılı olarak iş ortası sağlayabilirsiniz:
 
- Azure Cosmos kapsayıcıları
- Azure Cosmos veritabanları

## <a name="set-throughput-on-a-container"></a>Bir kapsayıcıda iş elde et  

Azure Cosmos kapsayıcısı üzerinde sağlanan iş, yalnızca bu kapsayıcı için ayrılmıştır. Kapsayıcı her zaman sağlanan iş elde edilir. Bir konteyner üzerinde sağlanan iş kolu, SLA'lar tarafından finansal olarak desteklenen bir kaptır. Bir kapsayıcıda iş ortası nasıl yapılandırılabildiğini öğrenmek için Azure [Cosmos kapsayıcıda Sağlama iş başına](how-to-provision-container-throughput.md)bakın.

Bir kapsayıcıüzerinde sağlanan iş bölümünü ayarlama en sık kullanılan seçenektir. [İstek Birimleri (RUs)](request-units.md)kullanarak herhangi bir miktarda iş elde etme sağlayarak bir konteyner için iş miktarını elastik olarak ölçeklendirebilirsiniz. 

Bir kapsayıcı için sağlanan üretim, fiziksel bölümleri arasında eşit olarak dağıtılır ve mantıksal bölümleri fiziksel bölümler arasında eşit olarak dağıtan iyi bir bölüm anahtarı varsayarsak, üretim de tüm bölümlerarasında eşit olarak dağıtılır kapsayıcının mantıksal bölümleri. Mantıksal bölümler için iş çıktısını seçerek belirtemezsiniz. Bir kapsayıcının bir veya daha fazla mantıksal bölümü fiziksel bir bölüm tarafından barındırıldığı için, fiziksel bölümler yalnızca kapsayıcıya aittir ve kapsayıcıda sağlanan iş elde etmeyi destekler. 

Mantıksal bir bölüm üzerinde çalışan iş yükü, bu mantıksal bölüme ayrılan iş kaynağından daha fazlasını tüketirse, işlemleriniz hız sınırlı olur. Fiyat sınırlaması oluştuğunda, tüm kapsayıcı için sağlanan iş ortasını artırabilir veya işlemleri yeniden deneyebilirsiniz. Bölümleme hakkında daha fazla bilgi için [Mantıksal bölümlere](partition-data.md)bakın.

Kapsayıcı için garantili performans istediğinizde, kapsayıcı parçalı olarak iş yerini yapılandırmanızı öneririz.

Aşağıdaki resim, fiziksel bir bölümün bir kapsayıcının bir veya daha fazla mantıksal bölümlerini nasıl barındırdığını gösterir:

![Fiziksel bölüm](./media/set-throughput/resource-partition.png)

## <a name="set-throughput-on-a-database"></a>Veri tabanında iş oluşturma yı ayarlama

Bir Azure Cosmos veritabanında iş ortası sağlamayaptığınızda, iş ortası veritabanındaki tüm kapsayıcılar (paylaşılan veritabanı kapsayıcıları olarak adlandırılır) arasında paylaşılır. Veritabanında belirli kapsayıcılar üzerinde sağlanan bir iş girdisi belirtilmiş sayılsa da, bir özel durum dur. Kapsayıcıları arasında veritabanı düzeyinde sağlanan iş verisinin paylaşılması, bir dizi makinede bir veritabanı barındırmaya benzer. Veritabanıiçindeki tüm kapsayıcılar bir makinede kullanılabilen kaynakları paylaştığından, doğal olarak belirli bir kapsayıcıda öngörülebilir performans elde emmezsiniz. Veritabanında sağlanan iş ortasını nasıl yapılandırılabildiğini öğrenmek için azure [cosmos veritabanında sağlanan iş ortasını yapılandırın.](how-to-provision-database-throughput.md)

Azure Cosmos veritabanında iş elde etmek, söz konusu veritabanı için sağlanan iş ortasını her zaman aldığınıza garanti eder. Veritabanındaki tüm kapsayıcılar sağlanan iş ortasını paylaştığından, Azure Cosmos DB söz konusu veritabanındaki belirli bir kapsayıcı için öngörülebilir iş yapma garantisi sağlamaz. Belirli bir kapsayıcının alabileceği iş bölümünün aşağıdakibölüme bağlıdır:

* Konteyner sayısı.
* Çeşitli kapsayıcılar için bölüm anahtarları seçimi.
* İş yükünün kapsayıcıların çeşitli mantıksal bölümleri arasında dağılımı. 

İş parçacığının birden çok kapsayıcı arasında paylaşılmak istediğinizde bir veritabanında iş parçacığı yapılandırmanızı öneririz, ancak iş parçacığı belirli bir kapsayıcıya ayırmak istemiyoruz. 

Aşağıdaki örnekler, veritabanı düzeyinde iş verime sağlamanın nerede tercih edildiğigösterilmektedir:

* Bir veritabanının sağlanan iş verisini bir kapsayıcı kümesi arasında paylaşmak çok kiracılı bir uygulama için yararlıdır. Her kullanıcı farklı bir Azure Cosmos kapsayıcısı tarafından temsil edilebilir.

* Bir veritabanının sağlanan iş ortasını bir kapsayıcı kümesi arasında paylaşmak, Bir VM kümesinde veya şirket içi fiziksel sunuculardan Azure Cosmos DB'ye barındırılan MongoDB veya Cassandra gibi bir NoSQL veritabanını geçirdiğinizde yararlıdır. Azure Cosmos veritabanınızda yapılandırılan sağlanan sağlanan iş ortasını, MongoDB veya Cassandra kümenizin bilgi işlem kapasitesine göre mantıksal eşdeğer, ancak daha uygun maliyetli ve esnek olarak düşünün.  

Sağlanan iş bölümü ile bir veritabanı içinde oluşturulan tüm kapsayıcılar bir [bölüm anahtarı](partition-data.md)ile oluşturulmalıdır. Zaman içinde herhangi bir noktada, bir veritabanı içinde bir kapsayıcıya ayrılan iş bölümü, o kapsayıcının tüm mantıksal bölümlerine dağıtılır. Veritabanında yapılandırılan sağlanan iş başına sahip kapsayıcılarınız varsa, belirli bir kapsayıcıya veya mantıksal bir bölüme iş için seçerek uygulayamazsınız. 

Mantıksal bir bölümdeki iş yükü, belirli bir mantıksal bölüme ayrılan iş kaynağından daha fazla tüketirse, işlemleriniz hız sınırlıdır. Hız sınırlayıcı oluştuğunda, tüm veritabanının iş verisini artırabilir veya işlemleri yeniden deneyebilirsiniz. Bölümleme hakkında daha fazla bilgi için [Mantıksal bölümlere](partition-data.md)bakın.

Paylaşılan bir iş verme veritabanındaki kapsayıcılar, bu veritabanına ayrılan iş veritabanını (RU/s) paylaşır. Veritabanında en az 400 RU/s ile en fazla dört kapsayıcınız olabilir. İlk dört sonra her yeni konteyner ek 100 RU / s minimum gerektirir. Örneğin, sekiz kapsayıcıiçeren paylaşılan bir iş veri tabanınız varsa, veritabanındaki minimum RU/s 800 RU/s olacaktır.

> [!NOTE]
> Şubat 2020'de, paylaşılan bir iş veri tabanında en fazla 25 kapsayıcıya sahip olmamızı sağlayan ve kapsayıcılar arasında iş verimi paylaşımını daha iyi sağlayan bir değişiklik başlattık. İlk 25 kapsayıcıdan sonra, veritabanına yalnızca veritabanının paylaşılan iş açısından ayrı olan [özel iş girdisi ile birlikte sağlanmış](#set-throughput-on-a-database-and-a-container)daha fazla kapsayıcı ekleyebilirsiniz.<br>
Azure Cosmos DB hesabınız zaten >=25 kapsayıcısı olan paylaşılan bir iş veri tabanı içeriyorsa, hesap ve aynı Azure aboneliğindeki diğer tüm hesaplar bu değişiklikten muaftır. Geri bildiriminiz veya sorularınız varsa lütfen [ürün desteğine başvurun.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) 

İş yüklerin, veritabanındaki tüm koleksiyonları silme ve yeniden oluşturmayı içeriyorsa, boş veritabanını bırakmanız ve koleksiyon oluşturmadan önce yeni bir veritabanını yeniden oluşturmanız önerilir. Aşağıdaki resim, fiziksel bir bölümün veritabanıiçindeki farklı kapsayıcılara ait bir veya daha fazla mantıksal bölümü nasıl barındırabileceğini gösterir:

![Fiziksel bölüm](./media/set-throughput/resource-partition2.png)

## <a name="set-throughput-on-a-database-and-a-container"></a>Bir veritabanı ve kapsayıcı üzerinde iş oluşturma ayarlama

İki modeli birleştirebilirsiniz. Hem veritabanında hem de kapsayıcıda iş başına izin verilir. Aşağıdaki örnek, bir Azure Cosmos veritabanı nda ve kapsayıcıda nasıl iş veritinin sağlanmaktadır:

* *"K"* RUs'un sağlanmış iş bölümüyle *Z* adında bir Azure Cosmos veritabanı oluşturabilirsiniz. 
* Ardından, veritabanı içinde *A*, *B*, *C*, *D*ve *E* adlı beş kapsayıcı oluşturun. B kapsayıcısını oluştururken, bu kapsayıcı seçeneği **için Özel Tedarik'i** etkinleştirdiğinizden emin olun ve bu kapsayıcıda sağlanan *"P"* RUs'u açıkça yapılandırın. Paylaşılan ve özel iş verisini yalnızca veritabanı ve kapsayıcı oluştururken yapılandırabileceğinizi unutmayın. 

   ![Elde iş oluşturmayı konteyner düzeyinde ayarlama](./media/set-throughput/coll-level-throughput.png)

* *"K"* RUs iş liği *A*, *C*, *D*ve *E*dört kap arasında paylaşılır. *A,* *C*, *D*veya *E* için kullanılabilir tam iş miktarı değişir. Her bir konteynerin iş bilgililiği için SLA'lar yoktur.
* *B* adlı *konteynerin "P"* RUs'u her zaman elde etmesi garanti edilir. SLA'lar tarafından da desteklenen bir yer.

> [!NOTE]
> Sağlanan iş verisine sahip bir kapsayıcı paylaşılan veritabanı kapsayıcısına dönüştürülemez. Tersine paylaşılan bir veritabanı kapsayıcısı özel bir iş verisine sahip olacak şekilde dönüştürülemez.

## <a name="update-throughput-on-a-database-or-a-container"></a>Veri tabanında veya kapsayıcıda iş verici güncelleştirme

Bir Azure Cosmos kapsayıcısı veya veritabanı oluşturduktan sonra, sağlanan iş ortasını güncelleştirebilirsiniz. Veritabanında veya kapsayıcıda yapılandırabileceğiniz maksimum sağlanan iş verimi için bir sınır yoktur. Minimum [sağlanan iş başına](concepts-limits.md#storage-and-throughput) şu etkenler bağlıdır: 

* Kapsayıcıda depoladığınız maksimum veri boyutu
* Konteyner üzerinde şimdiye kadar sağladığınız maksimum verim
* Paylaşılan iş ortası içeren bir veritabanında bulunan geçerli Azure Cosmos kapsayıcı sayısı. 

SDK'ları kullanarak bir kapsayıcının veya veritabanının en az iş verisini alabilir veya Azure portalındaki değeri görüntüleyebilirsiniz. .NET SDK'yı kullanırken [DocumentClient.ReplaceOfferAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.replaceofferasync?view=azure-dotnet) yöntemi, sağlanan üretim değerini ölçeklendirmenize olanak tanır. Java SDK'yı kullanırken, [RequestOptions.setOfferThroughput](sql-api-java-samples.md#offer-examples) yöntemi, sağlanan üretim değerini ölçeklendirmenize olanak tanır. 

.NET SDK'yı kullanırken [DocumentClient.ReadOfferAsync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.readofferasync?view=azure-dotnet) yöntemi, bir kapsayıcının veya veritabanının minimum iş verialımını almanızı sağlar. 

Bir kapsayıcının veya veritabanının sağlanan iş verisini istediğiniz zaman ölçeklendirebilirsiniz. İş bilgililiği artırmak için bir ölçek işlemi gerçekleştirildiğinde, gerekli kaynakların sağlanması sistem görevleri nedeniyle daha uzun zaman alabilir. Azure portalında ölçek işleminin durumunu veya SDK'ları kullanarak programlı bir şekilde denetleyebilirsiniz. .Net SDK'yı kullanırken, `DocumentClient.ReadOfferAsync` yöntemi kullanarak ölçek işleminin durumunu alabilirsiniz.

## <a name="comparison-of-models"></a>Modellerin karşılaştırılması

|**Parametre**  |**Bir veritabanında sağlanan iş mesuliyonu**  |**Bir konteyner üzerinde sağlanan iş mesuliyonu**|
|---------|---------|---------|
|Minimum RUs |400 (İlk dört kaptan sonra, her ek konteyner saniyede en az 100 RUs gerektirir.) |400|
|Konteyner başına minimum RUs|100|400|
|Maksimum RUs|Sınırsız, veritabanında.|Sınırsız, konteynırın üzerinde.|
|Belirli bir kapsayıcıya atanan veya kullanılabilen RUs'lar|Garantisi yok. Belirli bir kapsayıcıya atanan RUs'lar özelliklerine bağlıdır. Özellikler, iş yükünü, iş yükünün dağılımını ve kapsayıcı sayısını paylaşan kapsayıcıların bölüm anahtarlarının seçimi olabilir. |Konteyner üzerinde yapılandırılan tüm RUS'lar sadece konteyner için ayrılmıştır.|
|Bir kapsayıcı için maksimum depolama alanı|Sınırsız.|Sınırsız.|
|Bir kapsayıcının mantıksal bölümü başına maksimum verim|10K RUs|10K RUs|
|Bir kapsayıcının mantıksal bölümü başına maksimum depolama (veri + dizin)|20 GB|20 GB|

## <a name="next-steps"></a>Sonraki adımlar

* [Mantıksal bölümler](partition-data.md)hakkında daha fazla bilgi edinin.
* [Azure Cosmos kapsayıcısına nasıl iş veriş sağlamayı](how-to-provision-container-throughput.md)öğrenin.
* [Azure Cosmos veritabanında iş ortası sağlama](how-to-provision-database-throughput.md)yı öğrenin.

