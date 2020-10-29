---
title: Azure Cosmos DB'ye giriş
description: Azure Cosmos DB hakkında bilgi edinin. Bu genel olarak dağıtılan çok modelli veritabanı; düşük gecikme süresi, esnek ölçeklenebilirlik, yüksek kullanılabilirlik için oluşturulmuştur ve NoSQL verileri için yerel destek sunar.
author: markjbrown
ms.author: mjbrown
ms.service: cosmos-db
ms.topic: overview
ms.date: 10/23/2020
ms.openlocfilehash: b691341fbaa5739639f81905a5fef335479694a7
ms.sourcegitcommit: dd45ae4fc54f8267cda2ddf4a92ccd123464d411
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/29/2020
ms.locfileid: "92927881"
---
# <a name="welcome-to-azure-cosmos-db"></a>Azure Cosmos DB’ye hoş geldiniz

Günümüzde uygulamaların çok kısa sürede yanıt vermesi ve her zaman açık olması beklenmektedir. Düşük gecikme süresi ve yüksek kullanılabilirlik düzeylerine erişmek için bu uygulamaların örneklerinin kullanıcılara yakın veri merkezlerine dağıtılması gerekir. Uygulamaların yoğun saatlerdeki büyük ölçekli kullanım değişikliklerine gerçek zamanlı olarak yanıt vermesi, hacmi sürekli artan verileri depolaması ve bu verileri birkaç milisaniye içinde kullanıcılara sunması gerekmektedir.

Azure Cosmos DB modern uygulama geliştirmesi için tam olarak yönetilen bir NoSQL veritabanıdır. Tek basamaklı milisaniyelik yanıt süreleri ve otomatik ve anında ölçeklenebilirlik, herhangi bir ölçekte hız garantisi. İş sürekliliği, [SLA ile desteklenen](https://azure.microsoft.com/support/legal/sla/cosmos-db) kullanılabilirlik ve kurumsal sınıf güvenlik olanakları konusunda garanti edilir. Uygulama geliştirme, dünyanın herhangi bir yerindeki çok ana veri dağıtımı, popüler diller için açık kaynak API 'Leri ve SDK 'lar sayesinde daha hızlı ve daha üretken bir hale gelidir. Tam olarak yönetilen bir hizmet olarak Azure Cosmos DB, otomatik yönetim, güncelleştirmeler ve düzeltme eki uygulama sayesinde veritabanı yönetimini ellerinizi kapabir biçimde alır. Ayrıca, uygun maliyetli sunucusuz ve uygulama ihtiyaçlarına yanıt veren otomatik ölçeklendirme seçenekleriyle kapasite yönetimini de işler.

Azure aboneliği olmadan [ücretsiz Azure Cosmos DB deneyebilir](https://azure.microsoft.com/try/cosmosdb/) , ücretsiz ve taahhütlere sahip olabilirsiniz veya Ilk 400 ru/sn ve 5 GB depolama alanı ücretsiz bir hesap almak için [Azure Cosmos DB ücretsiz katmanını](optimize-dev-test.md#azure-cosmos-db-free-tier) kullanabilirsiniz.

> [!div class="nextstepaction"]
> [Azure Cosmos DB ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/)

:::image type="content" source="./media/introduction/azure-cosmos-db.png" alt-text="Azure Cosmos DB modern uygulama geliştirmesi için tam olarak yönetilen bir NoSQL veritabanıdır." border="false":::

## <a name="key-benefits"></a>Önemli Avantajlar

### <a name="guaranteed-speed-at-any-scale"></a>Herhangi bir ölçekte garantili hız

Paralellik SLA ile [desteklenen](https://azure.microsoft.com/support/legal/sla/cosmos-db) hızlı ve üretilen iş, hızlı genel erişim ve hızlı esneklik elde edin.

- Hızlı okuma ve yazma gecikmeleri ile küresel olarak, [SLA 'lar](https://azure.microsoft.com/support/legal/sla/cosmos-db) tarafından desteklenen işleme ve tutarlılık ile gerçek zamanlı erişim
- Bir düğmeye tıklayarak herhangi bir Azure bölgesine çok bölgeli yazma ve veri dağıtımı.
- Bağımsız olarak ve esnek ölçek depolama ve aktarım hızı, tüm Azure bölgelerinde (Dünya genelinde sınırsız sayıda ölçek için) öngörülemeyen trafik patları sırasında bile.

### <a name="simplified-application-development"></a>Basitleştirilmiş uygulama geliştirme

Açık kaynak API 'Leri, birden çok SDK, şemasız veri ve işlemsel veriler üzerinde hiçbir ETL analizi ile hızlı bir şekilde oluşturun.

- Azure Işlevleri, IoT Hub, AKS (Azure Kubernetes hizmeti), App Service ve daha fazlasını içeren modern (bulutta yerel) uygulama geliştirmede kullanılan anahtar Azure hizmetleriyle daha ayrıntılı bir şekilde tümleşir.
- Yerel Çekirdek (SQL) API, MongoDB için API, Cassandra API, Gremlin API ve Tablo API'si dahil olmak üzere birden çok veritabanı API 'si arasından seçim yapın.
- .NET, Java, Node.js ve Python için SDK 'lar ile tercih ettiğiniz dilleri kullanarak çekirdek (SQL) API üzerinde uygulamalar oluşturun. Ya da diğer veritabanı API 'Leri için istediğiniz sürücü seçiminizi yapın.
- Azure SYNAPSE Analytics ile Azure Cosmos DB depolanan neredeyse gerçek zamanlı işletimsel veriler üzerinde hiçbir ETL Analizi çalıştırın.
- Değişiklik akışı, veritabanı kapsayıcılarındaki değişiklikleri izlemenizi ve yönetmeyi ve Azure Işlevleri ile tetiklenen olaylar oluşturmayı kolaylaştırır.
- Azure Cosmos DB şema-daha az hizmeti, veri modelinden bağımsız olarak tüm verilerinizi otomatik olarak dizinleyebilir ve hızlı sorguları güçlendirme.

### <a name="mission-critical-ready"></a>Görev açısından kritik hazırlık

Her uygulama için iş sürekliliği,% 99,999 kullanılabilirlik ve kurumsal düzeyde güvenlik garantisi.

- Azure Cosmos DB, dünyanın her yerindeki endüstri lideri kullanılabilirliği dahil kapsamlı bir [SLA](https://azure.microsoft.com/support/legal/sla/cosmos-db) paketi sunar.
- Otomatik veri çoğaltmasıyla, verileri herhangi bir Azure bölgesine kolayca dağıtın. Güçlü tutarlılık kullanılırken, çok bölgeli yazma veya RPO 0 ile sıfır kapalı kalma süresinden yararlanın.
- Şirket içinde, kendinden yönetilen anahtarlarla Rest 'ten yararlanın.
- Rol tabanlı erişim denetimi, verilerinizi güvende tutar ve ince ayarlanmış denetim sağlar.

### <a name="fully-managed-and-cost-effective"></a>Tam olarak yönetilen ve ekonomik

Uygulamanızın ve TCO gereksinimleriniz ile eşleşen sunucusuz ve otomatik ölçeklendirme ile uçtan uca veritabanı yönetimi

- Tam olarak yönetilen veritabanı hizmeti. Otomatik, dokunmatik, bakım, düzeltme eki uygulama ve güncelleştirme, geliştiricilerin zaman ve para tasarrufu.
- Herhangi bir boyuttaki veya ölçekteki öngörülemeyen veya sporlı iş yükleri için uygun maliyetli seçenekler, geliştiricilerin kapasiteyi planlamak veya yönetmek zorunda kalmadan kolayca çalışmaya başlamanızı sağlar.
- Sunucusuz model, isteğe bağlı trafik yükünü yönetmek için çıkmanız gerekirse iş yükleri otomatik ve yanıt veren hizmeti sunar.
- Sağlanan üretilen işi otomatik olarak otomatik ölçeklendirme ve öngörülemeyen iş yükleri için kapasiteyi anında ölçeklendirirken [SLA 'ları](https://azure.microsoft.com/support/legal/sla/cosmos-db)koruyun.

## <a name="solutions-that-benefit-from-azure-cosmos-db"></a>Azure Cosmos DB'den yararlanan çözümler

Çok büyük miktarlarda veri, okuma ve yazma işlemlerini, çeşitli veriler için neredeyse gerçek zamanlı yanıt sürelerine sahip bir [Global ölçekte](distribute-data-globally.md) işlemek için gereken tüm [Web, mobil, oyun ve ıot uygulamaları](use-cases.md) , Cosmos DB [garantili yüksek kullanılabilirlik](https://azure.microsoft.com/support/legal/sla/cosmos-db/), yüksek aktarım hızı, düşük gecikme süresi ve ayarlanabilir tutarlılık avantajlarından yararlanır. Azure Cosmos DB [IoT ve telematik](use-cases.md#iot-and-telematics), [Perakende ve pazarlama](use-cases.md#retail-and-marketing), [oyun](use-cases.md#gaming) ve [Web ve mobil uygulamalar](use-cases.md#web-and-mobile-applications)oluşturmak için nasıl kullanılabileceğinizi öğrenin.

## <a name="next-steps"></a>Sonraki adımlar

Dört hızlı başlangıçtan biriyle Azure Cosmos DB kullanmaya başlayın:

- [Azure Cosmos DB SQL API’yi kullanmaya başlama](create-sql-api-dotnet.md)
- [MongoDB için Azure Cosmos DB API 'SI ile çalışmaya başlama](create-mongodb-nodejs.md)
- [Azure Cosmos DB Cassandra API’yi kullanmaya başlama](create-cassandra-dotnet.md)
- [Azure Cosmos DB Graph API’yi kullanmaya başlama](create-graph-dotnet.md)
- [Azure Cosmos DB Tablo API’yi kullanmaya başlama](create-table-dotnet.md)

> [!div class="nextstepaction"]
> [Azure Cosmos DB’yi ücretsiz deneyin](https://azure.microsoft.com/try/cosmosdb/)