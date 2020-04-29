---
title: Azure Cosmos kapsayıcılarında ve veritabanlarında üretilen iş sağlama
description: Azure Cosmos kapsayıcıları ve veritabanları için sağlanan aktarım hızını ayarlamayı öğrenin.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/12/2019
ms.openlocfilehash: e7a64776cba00a6840af70cecad5bf9c02b3f38e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79251979"
---
# <a name="provision-throughput-on-containers-and-databases"></a>Kapsayıcı ve veritabanlarına aktarım hızı sağlama

Azure Cosmos veritabanı, bir kapsayıcı kümesi için bir yönetim birimidir. Bir veritabanı, bir şema belirsiz kapsayıcılar kümesinden oluşur. Azure Cosmos kapsayıcısı, aktarım hızı ve depolama için ölçeklenebilirlik birimidir. Bir kapsayıcı, bir Azure bölgesindeki bir makine kümesi arasında yatay olarak bölümlenmiştir ve Azure Cosmos hesabınızla ilişkili tüm Azure bölgelerinde dağıtılır.

Azure Cosmos DB, aktarım hızını iki ayrı şekilde sağlayabilirsiniz:
 
- Azure Cosmos kapsayıcıları
- Azure Cosmos veritabanları

## <a name="set-throughput-on-a-container"></a>Bir kapsayıcıda üretilen işi ayarlama  

Azure Cosmos kapsayıcısı üzerinde sağlanan aktarım hızı, o kapsayıcı için özel olarak ayrılmıştır. Kapsayıcı, sağlanan aktarım hızını her zaman alır. Bir kapsayıcıda sağlanan aktarım hızı SLA 'Lar tarafından mali olarak desteklenir. Bir kapsayıcıda üretilen işi yapılandırma hakkında bilgi edinmek için bkz. [Azure Cosmos kapsayıcısında Işleme sağlama](how-to-provision-container-throughput.md).

Bir kapsayıcıda sağlanan aktarım hızını ayarlamak en sık kullanılan seçenektir. [Istek birimleri (ru)](request-units.md)kullanarak herhangi bir miktarda aktarım hızı sağlayarak bir kapsayıcı için ölçek işleme esnek sağlayabilirsiniz. 

Bir kapsayıcı için sağlanan aktarım hızı, fiziksel bölümleri arasında eşit olarak dağıtılır ve mantıksal bölümleri fiziksel bölümler arasında eşit bir şekilde dağıtan iyi bir bölüm anahtarı varsayıldığında, verimlilik aynı zamanda kapsayıcının tüm mantıksal bölümlerinde eşit olarak dağıtılır. Mantıksal bölümler için üretilen işi seçmeli olarak belirtemezsiniz. Bir kapsayıcının bir veya daha fazla mantıksal bölümü fiziksel bölüm tarafından barındırıldığından, fiziksel bölümler yalnızca kapsayıcıya aittir ve kapsayıcıda sağlanan aktarım hızını destekler. 

Bir mantıksal bölümde çalışan iş yükü, bu mantıksal bölüme ayrılan aktarım hızına göre daha fazla tüketir, işlemlerinizin hızı sınırlı olacaktır. Oran sınırlaması gerçekleştiğinde, tüm kapsayıcı için sağlanan aktarım hızını artırabilir ya da işlemleri yeniden deneyebilirsiniz. Bölümlendirme hakkında daha fazla bilgi için bkz. [mantıksal bölümler](partition-data.md).

Kapsayıcı için garantili performans istediğinizde, kapsayıcı ayrıntı düzeyinde aktarım hızı yapılandırmanızı öneririz.

Aşağıdaki görüntüde bir fiziksel bölümün bir kapsayıcının bir veya daha fazla mantıksal bölümünü nasıl barındırdığı gösterilmektedir:

![Fiziksel bölüm](./media/set-throughput/resource-partition.png)

## <a name="set-throughput-on-a-database"></a>Bir veritabanında üretilen işi ayarlama

Azure Cosmos veritabanında üretilen iş sağladığınızda, aktarım hızı veritabanında tüm kapsayıcılar (paylaşılan veritabanı kapsayıcıları olarak adlandırılır) arasında paylaşılır. Bir özel durum, veritabanındaki belirli kapsayıcılarda sağlanan bir üretilen işi belirtadınız. Veritabanı düzeyinde sağlanan aktarım hızını kapsayıcı genelinde paylaşmak, bir makine kümesinde bir veritabanını barındırmakla benzerdir. Bir veritabanı içindeki tüm kapsayıcılar bir makinede kullanılabilir kaynakları paylaştığından, doğal olarak herhangi bir kapsayıcı üzerinde öngörülebilir bir performans almaz. Bir veritabanında sağlanan üretilen işi yapılandırma hakkında bilgi edinmek için bkz. [Azure Cosmos veritabanında sağlanan aktarım hızını yapılandırma](how-to-provision-database-throughput.md).

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

Paylaşılan bir üretilen iş veritabanındaki kapsayıcılar, bu veritabanına ayrılan üretilen işi (RU/s) paylaşır. Veritabanında en az 400 RU/sn olan en fazla dört kapsayıcı olabilir. İlk dördü sonrasında her yeni kapsayıcı, en az 100 RU/s için ek bir değer gerektirir. Örneğin, sekiz kapsayıcı içeren paylaşılan bir işleme veritabanınız varsa, veritabanında en az RU/sn 800 RU/sn olacaktır.

> [!NOTE]
> 2020 Şubat 'de, paylaşılan bir üretilen iş veritabanında en fazla 25 kapsayıcı kullanmanıza izin veren bir değişiklik geliştirdik ve bu değişiklikler, kapsayıcılar arasında üretilen iş aktarımını daha iyi bir şekilde olanaklı hale getirmiştir. İlk 25 kapsayıcıdan sonra veritabanına daha fazla kapsayıcı ekleyebilirsiniz ve bu, veritabanının paylaşılan iş verimini birbirinden ayrı olan [ayrılmış aktarım hızı ile sağlanırlar](#set-throughput-on-a-database-and-a-container).<br>
Azure Cosmos DB hesabınız zaten >= 25 kapsayıcılarıyla paylaşılan bir üretilen iş veritabanı içeriyorsa, hesap ve aynı Azure aboneliğindeki tüm diğer hesaplar bu değişiklikten muaf tutulur. Geri bildirim veya sorularınız varsa lütfen [ürün desteğine başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) . 

İş yükleriniz bir veritabanındaki tüm koleksiyonları silmeyi ve yeniden oluşturmayı içeriyorsa, boş veritabanını bırakıp koleksiyon oluşturmadan önce yeni bir veritabanı oluşturmanız önerilir. Aşağıdaki görüntüde bir fiziksel bölümün, bir veritabanı içindeki farklı kapsayıcılara ait bir veya daha fazla mantıksal bölümü nasıl barındıragösterdiği gösterilmektedir:

![Fiziksel bölüm](./media/set-throughput/resource-partition2.png)

## <a name="set-throughput-on-a-database-and-a-container"></a>Bir veritabanında ve kapsayıcıda üretilen işi ayarlama

İki modeli birleştirebilirsiniz. Hem veritabanında hem de kapsayıcıda sağlama aktarımına izin verilir. Aşağıdaki örnek, bir Azure Cosmos veritabanı ve bir kapsayıcı üzerinde üretilen iş sağlama işlemlerinin nasıl yapılacağını göstermektedir:

* *"K"* ru tarafından sağlanan iş hacmi ile *Z* adlı bir Azure Cosmos veritabanı oluşturabilirsiniz. 
* Sonra, veritabanı içinde *A*, *B*, *C*, *D*ve *E* adlı beş kapsayıcı oluşturun. B kapsayıcısını oluştururken, **Bu kapsayıcı seçeneği için adanmış üretilen iş üretimini sağlamayı** etkinleştirdiğinizden emin olun ve bu kapsayıcıda *"P"* ru sağlanan aktarım hızını açık olarak yapılandırın. Yalnızca veritabanı ve kapsayıcı oluştururken paylaşılan ve ayrılmış iş üretimini yapılandırabileceğinizi unutmayın. 

   ![Aktarım hızını kapsayıcı düzeyinde ayarlama](./media/set-throughput/coll-level-throughput.png)

* *"K"* Rus verimlilik, *A*, *C*, *D*ve *E*dört kapsayıcı genelinde paylaşılır. *Bir*, *C*, *D*veya *E* için kullanılabilir aktarım hızı miktarı farklılık gösterir. Her bir kapsayıcının verimlilik için SLA 'lar yoktur.
* *B* adlı kapsayıcı, *"P"* ru aktarım hızını her zaman almak için garanti edilir. SLA 'Lar tarafından desteklenir.

> [!NOTE]
> Sağlanan aktarım hızı olan bir kapsayıcı, paylaşılan veritabanı kapsayıcısına dönüştürülemez. Bunun tersine, paylaşılan bir veritabanı kapsayıcısının ayrılmış bir işleme sahip olacak şekilde dönüştürülemez.

## <a name="update-throughput-on-a-database-or-a-container"></a>Bir veritabanında veya kapsayıcıda üretilen işi güncelleştirme

Bir Azure Cosmos kapsayıcısı veya veritabanı oluşturduktan sonra, sağlanan aktarım hızını güncelleştirebilirsiniz. Veritabanı veya kapsayıcıda yapılandırabileceğiniz en fazla sağlanan aktarım hızı için bir sınır yoktur. [Sağlanan en düşük verimlilik](concepts-limits.md#storage-and-throughput) aşağıdaki etkenlere bağlıdır: 

* Kapsayıcıda depoladığınız en büyük veri boyutu
* Kapsayıcıda sağladığınız en fazla aktarım hızı
* Paylaşılan verimlilik içeren bir veritabanında bulunan Azure Cosmos kapsayıcılarının geçerli sayısı. 

SDK 'Ları kullanarak bir kapsayıcının veya veritabanının en düşük aktarım hızını alabilir veya Azure portal değeri görüntüleyebilirsiniz. .NET SDK kullanırken [Documentclient. Replaceoffsilinebilir Sync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.replaceofferasync?view=azure-dotnet) yöntemi sağlanan aktarım hızı değerini ölçeklendirmenize olanak tanır. Java SDK 'sını kullanırken, [Requestoptions. Setofferüretilen Iş verimlilik](sql-api-java-samples.md#offer-examples) yöntemi sağlanan aktarım hızı değerini ölçeklendirmenize olanak tanır. 

.NET SDK kullanırken [Documentclient. Readoffsilinebilir Sync](https://docs.microsoft.com/dotnet/api/microsoft.azure.documents.client.documentclient.readofferasync?view=azure-dotnet) yöntemi, bir kapsayıcının veya bir veritabanının en düşük verimini elde etmenizi sağlar. 

Bir kapsayıcının veya veritabanının sağlanan verimini istediğiniz zaman ölçeklendirebilirsiniz. Aktarım hızını artırmak için bir ölçeklendirme işlemi gerçekleştirildiğinde, sistem görevlerinin gerekli kaynakları sağlaması nedeniyle daha uzun zaman alabilir. Azure portal veya SDK 'Ları kullanarak program aracılığıyla ölçek işleminin durumunu kontrol edebilirsiniz. .NET SDK kullanırken, `DocumentClient.ReadOfferAsync` yöntemini kullanarak ölçeklendirme işleminin durumunu alabilirsiniz.

## <a name="comparison-of-models"></a>Modellerin karşılaştırması

|**Parametre**  |**Bir veritabanında sağlanan aktarım hızı**  |**Kapsayıcıda sağlanan aktarım hızı**|
|---------|---------|---------|
|Minimum ru |400 (ilk dört kapsayıcıdan sonra, her ek kapsayıcı için en az 100 ru/saniye gerekir.) |400|
|Kapsayıcı başına en az ru|100|400|
|Maksimum RUs|Sınırsız, veritabanında.|Sınırsız, kapsayıcıda.|
|RUs atanmış veya belirli bir kapsayıcıya kullanılabilir|Garanti yok. Belirli bir kapsayıcıya atanan RUs özelliklere bağlıdır. Özellikler, aktarım hızını, iş yükünün dağıtımını ve kapsayıcı sayısını paylaşan kapsayıcıların bölüm anahtarları seçeneği olabilir. |Kapsayıcıda yapılandırılan tüm RUs 'lar kapsayıcı için özel olarak ayrılmıştır.|
|Bir kapsayıcı için maksimum depolama alanı|Sayısız.|Sayısız.|
|Bir kapsayıcının mantıksal bölümü başına en fazla aktarım hızı|10.000 ru|10.000 ru|
|Kapsayıcının mantıksal bölümü başına en fazla depolama alanı (veri + dizin)|20 GB|20 GB|

## <a name="next-steps"></a>Sonraki adımlar

* [Mantıksal bölümler](partition-data.md)hakkında daha fazla bilgi edinin.
* [Azure Cosmos kapsayıcısında üretilen iş sağlama](how-to-provision-container-throughput.md)hakkında bilgi edinin.
* [Azure Cosmos veritabanında üretilen iş sağlama](how-to-provision-database-throughput.md)hakkında bilgi edinin.

