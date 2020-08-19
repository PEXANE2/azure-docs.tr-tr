---
title: Azure Cosmos kapsayıcılarında ve veritabanlarında üretilen iş sağlama
description: Azure Cosmos kapsayıcıları ve veritabanları için sağlanan aktarım hızını ayarlamayı öğrenin.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/19/2020
ms.openlocfilehash: 00ed8f6ff9839c227f3d8a929a071834c5559226
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88605736"
---
# <a name="introduction-to-provisioned-throughput-in-azure-cosmos-db"></a>Azure Cosmos DB sağlanan üretilen iş hızına giriş

Azure Cosmos DB, veritabanlarınızda ve kapsayıcılarınızda sağlanan aktarım hızını ayarlamanıza olanak sağlar. İki tür sağlanan aktarım hızı, standart (el ile) veya otomatik ölçeklendirme. Bu makaleler, sağlanan iş üretiminin nasıl çalıştığına ilişkin bir genel bakış sunar. 

Azure Cosmos veritabanı bir dizi kapsayıcı için yönetim birimidir. Veritabanı şemadan bağımsız bir dizi kapsayıcıdan oluşur. Azure Cosmos kapsayıcısı hem aktarım hızı hem de depolama alanı için ölçeklenebilirlik birimidir. Bir kapsayıcı Azure bölgesinin içindeki bir dizi makine arasında dikey olarak bölümlenir ve Azure Cosmos hesabınızla ilişkilendirilmiş tüm Azure bölgelerine dağıtılır.

Azure Cosmos DB, aktarım hızını iki ayrı şekilde sağlayabilirsiniz:
 
- Azure Cosmos kapsayıcıları
- Azure Cosmos veritabanları

## <a name="set-throughput-on-a-container"></a>Bir kapsayıcıda üretilen işi ayarlama  

Azure Cosmos kapsayıcısı üzerinde sağlanan aktarım hızı, o kapsayıcı için özel olarak ayrılmıştır. Kapsayıcı, sağlanan aktarım hızını her zaman alır. Bir kapsayıcıda sağlanan aktarım hızı SLA 'Lar tarafından mali olarak desteklenir. Bir kapsayıcıda standart (el ile) üretilen iş üretimini yapılandırma hakkında bilgi edinmek için bkz. [Azure Cosmos kapsayıcısında sağlama işleme](how-to-provision-container-throughput.md). Bir kapsayıcıda otomatik ölçeklendirme verimini yapılandırma hakkında bilgi edinmek için bkz. [Otomatik ölçeklendirme Işleme sağlama](how-to-provision-autoscale-throughput.md).

Bir kapsayıcıda sağlanan aktarım hızını ayarlamak en sık kullanılan seçenektir. [Istek birimleri (ru)](request-units.md)kullanarak herhangi bir miktarda aktarım hızı sağlayarak bir kapsayıcı için ölçek işleme esnek sağlayabilirsiniz. 

Bir kapsayıcı için sağlanan aktarım hızı, fiziksel bölümleri arasında eşit olarak dağıtılır ve mantıksal bölümleri fiziksel bölümler arasında eşit bir şekilde dağıtan iyi bir bölüm anahtarı varsayıldığında, verimlilik aynı zamanda kapsayıcının tüm mantıksal bölümlerinde eşit olarak dağıtılır. Mantıksal bölümler için üretilen işi seçmeli olarak belirtemezsiniz. Bir kapsayıcının bir veya daha fazla mantıksal bölümü fiziksel bölüm tarafından barındırıldığından, fiziksel bölümler yalnızca kapsayıcıya aittir ve kapsayıcıda sağlanan aktarım hızını destekler. 

Bir mantıksal bölümde çalışan iş yükü, temeldeki fiziksel bölüme ayrılan aktarım hızına kıyasla daha fazla tüketir, işlemlerinizin hız sınırlı olması mümkündür. Bir mantıksal bölüm, diğer bölüm anahtarı değerlerinden daha fazla istek talep ediyorsa, _etkin bölüm_ olarak bilinen özellikler oluşur.

Oran sınırlaması gerçekleştiğinde, tüm kapsayıcı için sağlanan aktarım hızını artırabilir ya da işlemleri yeniden deneyebilirsiniz. Ayrıca, depolama ve istek birimini eşit bir şekilde dağıtan bir bölüm anahtarı seçtiğinizden emin olmanız gerekir. Bölümlendirme hakkında daha fazla bilgi için, bkz. [Azure Cosmos DB bölümlendirme ve yatay ölçeklendirme](partition-data.md).

Kapsayıcı için garantili performans istediğinizde, kapsayıcı ayrıntı düzeyinde aktarım hızı yapılandırmanızı öneririz.

Aşağıdaki görüntüde bir fiziksel bölümün bir kapsayıcının bir veya daha fazla mantıksal bölümünü nasıl barındırdığı gösterilmektedir:

:::image type="content" source="./media/set-throughput/resource-partition.png" alt-text="Fiziksel bölüm" border="false":::

## <a name="set-throughput-on-a-database"></a>Bir veritabanında üretilen işi ayarlama

> [!NOTE]
> Azure Cosmos veritabanında sağlama verimi, [müşteri tarafından yönetilen anahtarların](how-to-setup-cmk.md) etkinleştirildiği hesaplarda Şu anda mümkün değildir.

Azure Cosmos veritabanında üretilen iş sağladığınızda, aktarım hızı veritabanında tüm kapsayıcılar (paylaşılan veritabanı kapsayıcıları olarak adlandırılır) arasında paylaşılır. Tek istisna, veritabanındaki belirli kapsayıcılarda sağlanan bir aktarım hızı belirttiğinizde gerçekleşir. Veritabanı düzeyinde sağlanan aktarım hızını kapsayıcı genelinde paylaşmak, bir makine kümesinde bir veritabanını barındırmakla benzerdir. Bir veritabanı içindeki tüm kapsayıcılar bir makinede kullanılabilir kaynakları paylaştığından, doğal olarak herhangi bir kapsayıcı üzerinde öngörülebilir bir performans almaz. Bir veritabanında sağlanan üretilen işi yapılandırma hakkında bilgi edinmek için bkz. [Azure Cosmos veritabanında sağlanan aktarım hızını yapılandırma](how-to-provision-database-throughput.md). Bir veritabanında otomatik ölçeklendirme verimini yapılandırma hakkında bilgi edinmek için bkz. [Otomatik ölçeklendirme Işleme sağlama](how-to-provision-autoscale-throughput.md).

Azure Cosmos veritabanında üretilen iş oluşturma, bu veritabanı için her zaman sağlanan aktarım hızını almanızı garanti eder. Veritabanı içindeki tüm kapsayıcılar sağlanan üretilen işi paylaştığından, Azure Cosmos DB söz konusu veritabanındaki belirli bir kapsayıcı için öngörülebilir bir işlem garantisi sağlamaz. Belirli bir kapsayıcının alabileceği aktarım hızı bölümü şu şekilde değişir:

* Kapsayıcı sayısı.
* Çeşitli kapsayıcılar için bölüm anahtarlarının seçimi.
* Kapsayıcının çeşitli mantıksal bölümlerinin tamamında iş yükünün dağılımı. 

Aktarım hızını birden çok kapsayıcı genelinde paylaşmak istediğinizde, ancak belirli bir kapsayıcıya üretilen işi ayırmak istemediğinizde bir veritabanında üretilen işi yapılandırmanızı öneririz. 

Aşağıdaki örneklerde veritabanı düzeyinde üretilen işi sağlamak için nereye tercih edildiği gösterilmektedir:

* Bir kapsayıcı kümesi genelinde veritabanının sağlanan verimini paylaşmak çok kiracılı bir uygulama için yararlıdır. Her kullanıcı ayrı bir Azure Cosmos kapsayıcısı ile temsil edilebilir.

* Bir sanal makine kümesinde veya şirket içi fiziksel sunuculardan Azure Cosmos DB için barındırılan MongoDB veya Cassandra gibi bir NoSQL veritabanını geçirdiğinizde, bir kapsayıcı kümesi genelinde bir veritabanının sağlanan aktarım hızını paylaşmanız yararlı olur. Azure Cosmos veritabanınızda yapılandırılan üretilen iş üretimini, MongoDB veya Cassandra kümenizin işlem kapasitesinden daha fazla maliyetli, daha düşük maliyetli ve elastik bir eşdeğer olarak düşünün.  

Sağlanan aktarım hızı ile bir veritabanı içinde oluşturulan tüm kapsayıcılar bir [bölüm anahtarıyla](partition-data.md)oluşturulmalıdır. Belirli bir zamanda, bir veritabanı içindeki bir kapsayıcıya ayrılan üretilen iş, o kapsayıcının tüm mantıksal bölümlerine dağıtılır. Bir veritabanında yapılandırılmış sağlanan aktarım hızını paylaşan kapsayıcılara sahipseniz, üretilen işi belirli bir kapsayıcıya veya mantıksal bölüme seçmeli olarak uygulayamazsınız. 

Mantıksal bir bölümdeki iş yükü, belirli bir mantıksal bölüme ayrılan aktarım hızına göre daha fazla tüketir, işlemlerinizin hızı sınırlıdır. Oran sınırlaması gerçekleştiğinde, veritabanının tamamı için üretilen işi artırabilir veya işlemleri yeniden deneyebilirsiniz. Bölümlendirme hakkında daha fazla bilgi için bkz. [mantıksal bölümler](partition-data.md).

Paylaşılan aktarım hızı veritabanındaki kapsayıcılar, söz konusu veritabanına ayrılmış olan aktarım hızını (RU/sn) paylaşır. Veritabanında en az 400 RU/sn hızında en çok dört kapsayıcınız olabilir. Standart (el ile) sağlanan aktarım hızı sayesinde, ilk dört değerden sonraki her yeni kapsayıcı için ek olarak 100 RU/sn gerekir. Örneğin sekiz kapsayıcılı bir paylaşılan aktarım hızı veritabanınız varsa, veritabanındaki en düşük RU/sn 800 RU/sn olacaktır. Otomatik ölçeklendirme sağlanan aktarım hızı sayesinde, bir veritabanında en fazla %4000 RU/sn (400-4000 RU/sn arasında ölçekleme) ile en fazla 25 kapsayıcı olabilir.

> [!NOTE]
> 2020 Şubat 'de, paylaşılan bir üretilen iş veritabanında en fazla 25 kapsayıcı kullanmanıza izin veren bir değişiklik geliştirdik ve bu değişiklikler, kapsayıcılar arasında üretilen iş aktarımını daha iyi bir şekilde olanaklı hale getirmiştir. İlk 25 kapsayıcıdan sonra veritabanına daha fazla kapsayıcı ekleyebilirsiniz ve bu, veritabanının paylaşılan iş verimini birbirinden ayrı olan [ayrılmış aktarım hızı ile sağlanırlar](#set-throughput-on-a-database-and-a-container).<br>
Azure Cosmos DB hesabınız zaten >= 25 kapsayıcılarıyla paylaşılan bir üretilen iş veritabanı içeriyorsa, hesap ve aynı Azure aboneliğindeki tüm diğer hesaplar bu değişiklikten muaf tutulur. Geri bildirim veya sorularınız varsa lütfen [ürün desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) . 

İş yükleriniz bir veritabanındaki tüm koleksiyonları silmeyi ve yeniden oluşturmayı içeriyorsa, boş veritabanını bırakıp koleksiyon oluşturmadan önce yeni bir veritabanı oluşturmanız önerilir. Aşağıdaki görüntüde bir fiziksel bölümün, bir veritabanı içindeki farklı kapsayıcılara ait bir veya daha fazla mantıksal bölümü nasıl barındıragösterdiği gösterilmektedir:

:::image type="content" source="./media/set-throughput/resource-partition2.png" alt-text="Fiziksel bölüm" border="false":::

## <a name="set-throughput-on-a-database-and-a-container"></a>Bir veritabanında ve kapsayıcıda üretilen işi ayarlama

İki modeli birleştirebilirsiniz. Hem veritabanında hem de kapsayıcıda sağlama aktarımına izin verilir. Aşağıdaki örnek, bir Azure Cosmos veritabanı ve bir kapsayıcı üzerinde standart (el ile) sağlanan işleme sağlamayı gösterir:

* *"K"* ru 'nin sağlanan standart (el ile) üretilen iş hızına sahip *Z* adlı bir Azure Cosmos veritabanı oluşturabilirsiniz. 
* Sonra, veritabanı içinde *A*, *B*, *C*, *D*ve *E* adlı beş kapsayıcı oluşturun. B kapsayıcısını oluştururken, **Bu kapsayıcı seçeneği için adanmış üretilen iş üretimini sağlamayı** etkinleştirdiğinizden emin olun ve bu kapsayıcıda *"P"* ru sağlanan aktarım hızını açık olarak yapılandırın. Yalnızca veritabanı ve kapsayıcı oluştururken paylaşılan ve ayrılmış iş üretimini yapılandırabileceğinizi unutmayın. 

   :::image type="content" source="./media/set-throughput/coll-level-throughput.png" alt-text="Aktarım hızını kapsayıcı düzeyinde ayarlama":::

* *"K"* Rus verimlilik, *A*, *C*, *D*ve *E*dört kapsayıcı genelinde paylaşılır. *Bir*, *C*, *D*veya *E* için kullanılabilir aktarım hızı miktarı farklılık gösterir. Her bir kapsayıcının verimlilik için SLA 'lar yoktur.
* *B* adlı kapsayıcı, *"P"* ru aktarım hızını her zaman almak için garanti edilir. SLA 'Lar tarafından desteklenir.

> [!NOTE]
> Sağlanan aktarım hızı olan bir kapsayıcı, paylaşılan veritabanı kapsayıcısına dönüştürülemez. Bunun tersine, paylaşılan bir veritabanı kapsayıcısının ayrılmış bir işleme sahip olacak şekilde dönüştürülemez.

## <a name="update-throughput-on-a-database-or-a-container"></a>Bir veritabanında veya kapsayıcıda üretilen işi güncelleştirme

Bir Azure Cosmos kapsayıcısı veya veritabanı oluşturduktan sonra, sağlanan aktarım hızını güncelleştirebilirsiniz. Veritabanı veya kapsayıcıda yapılandırabileceğiniz en fazla sağlanan aktarım hızı için bir sınır yoktur. 

Bir veritabanı veya kapsayıcının [En düşük sağlanan verimini](concepts-limits.md#storage-and-database-operations) tahmin etmek için, en yüksek değeri bulun:

* 400 RU/sn 
* GB cinsinden geçerli depolama alanı * 10 RU/sn
* Veritabanı veya kapsayıcı/100 üzerinde sağlanan en yüksek RU/sn
* Kapsayıcı sayısı * 100 RU/sn (yalnızca paylaşılan üretilen iş veritabanı)

Gerçek minimum RU/sn, hesap yapılandırmanıza göre farklılık gösterebilir. [Azure izleyici ölçümlerini](monitor-cosmos-db.md#view-operation-level-metrics-for-azure-cosmos-db) , kaynak üzerinde sağlanan üretilen Iş (ru/s) ve depolamanın geçmişini görüntülemek için kullanabilirsiniz.

SDK 'Ları kullanarak bir kapsayıcının veya veritabanının en düşük aktarım hızını alabilir veya Azure portal değeri görüntüleyebilirsiniz. .NET SDK kullanırken [Documentclient. Replaceoffsilinebilir Sync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.replaceofferasync?view=azure-dotnet) yöntemi sağlanan aktarım hızı değerini ölçeklendirmenize olanak tanır. Java SDK 'sını kullanırken, [Requestoptions. Setofferüretilen Iş verimlilik](sql-api-java-sdk-samples.md) yöntemi sağlanan aktarım hızı değerini ölçeklendirmenize olanak tanır. 

.NET SDK kullanırken [Documentclient. Readoffsilinebilir Sync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.readofferasync?view=azure-dotnet) yöntemi, bir kapsayıcının veya bir veritabanının en düşük verimini elde etmenizi sağlar. 

Bir kapsayıcının veya veritabanının sağlanan verimini istediğiniz zaman ölçeklendirebilirsiniz. Aktarım hızını artırmak için bir ölçeklendirme işlemi gerçekleştirildiğinde, sistem görevlerinin gerekli kaynakları sağlaması nedeniyle daha uzun zaman alabilir. Azure portal veya SDK 'Ları kullanarak program aracılığıyla ölçek işleminin durumunu kontrol edebilirsiniz. .NET SDK kullanırken, yöntemini kullanarak ölçeklendirme işleminin durumunu alabilirsiniz `DocumentClient.ReadOfferAsync` .

## <a name="comparison-of-models"></a>Modellerin karşılaştırması
Bu tabloda bir veritabanında ve bir kapsayıcı üzerinde standart (el ile) aktarım hızı sağlama arasındaki bir karşılaştırma gösterilmektedir. 

|**Parametre**  |**Bir veritabanında standart (el ile) aktarım hızı**  |**Kapsayıcıda standart (el ile) aktarım hızı**|**Veritabanında otomatik ölçeklendirme işleme** | **Kapsayıcıda otomatik ölçeklendirme işleme**|
|---------|---------|---------|---------|---------|
|Giriş noktası (en az RU/sn) |400 RU/sn. İlk dört kapsayıcının ardından, her ek kapsayıcı için en az 100 RU/sn gerekir</li> |400| 400-4000 RU/s arasında otomatik ölçeklendirme. Kapsayıcı başına en az RU/sn olmayan en fazla 25 kapsayıcı olabilir</li> | 400-4000 RU/s arasında otomatik ölçeklendirme.|
|Kapsayıcı başına en az RU/sn|100|400|--|400-4000 RU/s arasında otomatik ölçeklendirme|
|Maksimum RUs|Sınırsız, veritabanında.|Sınırsız, kapsayıcıda.|Sınırsız, veritabanında.|Sınırsız, kapsayıcıda.
|RUs atanmış veya belirli bir kapsayıcıya kullanılabilir|Garanti yok. Belirli bir kapsayıcıya atanan RUs özelliklere bağlıdır. Özellikler, aktarım hızını, iş yükünün dağıtımını ve kapsayıcı sayısını paylaşan kapsayıcıların bölüm anahtarları seçeneği olabilir. |Kapsayıcıda yapılandırılan tüm RUs 'lar kapsayıcı için özel olarak ayrılmıştır.|Garanti yok. Belirli bir kapsayıcıya atanan RUs özelliklere bağlıdır. Özellikler, aktarım hızını, iş yükünün dağıtımını ve kapsayıcı sayısını paylaşan kapsayıcıların bölüm anahtarları seçeneği olabilir. |Kapsayıcıda yapılandırılan tüm RUs 'lar kapsayıcı için özel olarak ayrılmıştır.|
|Bir kapsayıcı için maksimum depolama alanı|Sayısız.|Sınırsız|Sınırsız|Sınırsız|
|Bir kapsayıcının mantıksal bölümü başına en fazla aktarım hızı|10.000 RU/sn|10.000 RU/sn|10.000 RU/sn|10.000 RU/sn|
|Kapsayıcının mantıksal bölümü başına en fazla depolama alanı (veri + dizin)|20 GB|20 GB|20 GB|20 GB|

## <a name="next-steps"></a>Sonraki adımlar

* [Mantıksal bölümler](partition-data.md)hakkında daha fazla bilgi edinin.
* [Azure Cosmos kapsayıcısında standart (el ile) sağlama](how-to-provision-container-throughput.md)hakkında bilgi edinin.
* [Azure Cosmos veritabanında standart (el ile) aktarım hızı sağlamayı](how-to-provision-database-throughput.md)öğrenin.
* [Azure Cosmos veritabanı veya kapsayıcısı üzerinde otomatik ölçeklendirme işleme sağlamayı](how-to-provision-autoscale-throughput.md)öğrenin.
