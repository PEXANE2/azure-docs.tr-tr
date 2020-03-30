---
title: Azure Cosmos DB'de Cassandra API ile elastik ölçeklendirme
description: Azure Cosmos DB Cassandra API hesabını ölçeklendirmek için kullanılabilen seçenekler ve avantajları/dezavantajları hakkında bilgi edinin
author: TheovanKraay
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 01/13/2020
ms.author: thvankra
ms.openlocfilehash: 10d81de48c0d8f56c7c3fd26e3fd82a8c3df84c6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79474688"
---
# <a name="elastically-scale-an-azure-cosmos-db-cassandra-api-account"></a>Azure Cosmos DB Cassandra API hesabını elastik olarak ölçeklendirin

Cassandra için Azure Cosmos DB API'nin elastik doğasını keşfetmek için çeşitli seçenekler vardır. Azure Cosmos DB'de nasıl etkili bir şekilde ölçeklendirilebildiğini anlamak için, sisteminizdeki performans taleplerini hesaba katmak için doğru miktarda istek biriminin (RU/s) nasıl sağlandığını anlamak önemlidir. İstek birimleri hakkında daha fazla bilgi edinmek için [istek birimleri](request-units.md) makalesine bakın. 

Cassandra API için [,NET ve Java SDK'larını](https://docs.microsoft.com/azure/cosmos-db/find-request-unit-charge#cassandra-api)kullanarak tek tek sorgular için İstek Birimi ücretini alabilirsiniz. Bu, hizmette sağlamanız gereken RU/s miktarını belirlemede yararlıdır.

![Veritabanı işlemleri İstek Birimlerini tüketir](./media/request-units/request-units.png)

## <a name="handling-rate-limiting-429-errors"></a>İşlem hızı sınırlayıcı (429 hata)

Azure Cosmos DB, istemciler sizin sağladığınız tutardan daha fazla kaynak (RU/s) tüketirse, fiyat sınırlı (429) hataları döndürür. Azure Cosmos DB'deki Cassandra API, bu özel durumları Cassandra yerel protokolündeki aşırı yüklenen hatalara çevirir. 

Sisteminiz gecikmeye duyarlı değilse, yeniden denemeler kullanarak işlenecek iş verme oranını işlemek için yeterli olabilir. Java'daki [Cassandra yeniden deneme ilkesi](https://docs.datastax.com/en/developer/java-driver/4.4/manual/core/retries/) için Azure [Cosmos DB uzantısını](https://github.com/Azure/azure-cosmos-cassandra-extensions) kullanarak hızı saydam bir şekilde nasıl işleyeceğiniz için [Java kodu örneğine](https://github.com/Azure-Samples/azure-cosmos-cassandra-java-retry-sample) bakın. Hız sınırlamasını işlemek için [Kıvılcım uzantısını](https://mvnrepository.com/artifact/com.microsoft.azure.cosmosdb/azure-cosmos-cassandra-spark-helper) da kullanabilirsiniz.

## <a name="manage-scaling"></a>Ölçekleme yönetme

Gecikme yi en aza indirmeniz gerekiyorsa, Cassandra API'sinde ölçek yönetimi ve üretim (RUs) sağlama seçenekleri vardır:

* [Azure portalını kullanarak el ile](#use-azure-portal)
* [Kontrol düzlemi özelliklerini kullanarak programlanabilir](#use-control-plane)
* [Belirli bir SDK ile CQL komutları kullanılarak programlanabilir](#use-cql-queries)
* [Otomatik pilot kullanarak dinamik olarak](#use-autopilot)

Aşağıdaki bölümlerde her yaklaşımın avantajları ve dezavantajları açıklayınız. Daha sonra sisteminizin ölçekleme gereksinimlerini, genel maliyet ve çözümünüzün verimlilik gereksinimlerini dengelemek için en iyi stratejiye karar verebilirsiniz.

## <a name="use-the-azure-portal"></a><a id="use-azure-portal"></a>Azure portal’ı kullanma

Azure portalını kullanarak Azure Cosmos DB Cassandra API hesabındaki kaynakları ölçeklendirebilirsiniz. Daha fazla bilgi için, [kapsayıcılar ve veritabanları üzerinde Provizyon iş parçacığı](set-throughput.md)makalesine bakın. Bu makalede, Azure portalında [veritabanı](set-throughput.md#set-throughput-on-a-database) veya [kapsayıcı](set-throughput.md#set-throughput-on-a-container) düzeyinde iş parçacığı ayarı göreli yararları açıklanmaktadır. Bu makalelerde belirtilen "veritabanı" ve "kapsayıcı" terimleri, Cassandra API için sırasıyla "keyspace" ve "tablo" ile eşleşmektedir.

Bu yöntemin avantajı, veritabanında üretim kapasitesini yönetmek için basit bir anahtar teslim yoludur. Ancak, dezavantajı birçok durumda, ölçekleme yaklaşımınızın hem uygun maliyetli hem de yüksek performanslı olması için belirli otomasyon düzeylerini gerektirebilir. Sonraki bölümlerde ilgili senaryolar ve yöntemler açıklanın.

## <a name="use-the-control-plane"></a><a id="use-control-plane"></a>Kontrol düzlemini kullanma

Azure Cosmos DB'nin Cassandra için sunduğu API, çeşitli kontrol düzlemi özelliklerimizi kullanarak iş ortamını programlı olarak ayarlama olanağı sağlar. Kılavuz ve örnekler için [Azure Kaynak Yöneticisi,](manage-cassandra-with-resource-manager.md) [Powershell](powershell-samples-cassandra.md)ve [Azure CLI](cli-samples-cassandra.md) makalelerini görün.

Bu yöntemin avantajı, en yüksek etkinlik veya düşük etkinlik dönemleri için hesap lamak için bir zamanlayıcıya dayalı kaynakların ölçekleme yukarı veya aşağı otomatikleştirebilirsiniz. Azure İşlevlerini ve Powershell'i kullanarak bunu nasıl başarabilirsiniz [alada](https://github.com/Azure-Samples/azure-cosmos-throughput-scheduler) örneğimize bir göz atın.

Bu yaklaşımın dezavantajı, öngörülemeyen değişen ölçek gereksinimlerine gerçek zamanlı olarak yanıt verememeniz olabilir. Bunun yerine, sisteminizdeki uygulama bağlamından, istemci/SDK düzeyinde veya [Otomatik Pilot'u](provision-throughput-autopilot.md)kullanmanız gerekebilir.

## <a name="use-cql-queries-with-a-specific-sdk"></a><a id="use-cql-queries"></a>Belirli bir SDK ile CQL sorgularını kullanma

Verilen veritabanı veya kapsayıcı için [CQL ALTER komutlarını](cassandra-support.md#keyspace-and-table-options) çalıştırarak sistemi kodla dinamik olarak ölçeklendirebilirsiniz.

Bu yaklaşımın avantajı, ölçek gereksinimlerine dinamik olarak ve uygulamanıza uygun özel bir şekilde yanıt vermenize olanak sağlamasıdır. Bu yaklaşımla, standart RU/s ücretlerinden ve oranlarından hala yararlanabilirsiniz. Sisteminizin ölçek gereksinimleri çoğunlukla tahmin edilebilirse (%70 civarı veya daha fazla), CQL ile SDK kullanmak Otomatik Pilot kullanmaktan daha uygun maliyetli bir otomatik ölçeklendirme yöntemi olabilir. Bu yaklaşımın dezavantajı, oran sınırlamagecikmesini artırabiliriken yeniden denemelerin uygulanmasının oldukça karmaşık olmasıdır.

## <a name="use-autopilot"></a><a id="use-autopilot"></a>Otomatik Pilot Kullanma

İş ortasını sağlamanın manuel veya programlı bir şekilde sağlanmasına ek olarak, Azure kozmosun kaplarını Otomatik Pilot modunda da yapılandırabilirsiniz. Otomatik pilot modu, SLA'lardan ödün vermeden belirtilen RU aralıkları dahilinde tüketim ihtiyaçlarınıza otomatik olarak ve anında ölçeklendirecektir. Daha fazla bilgi edinmek için [otomatik pilot modu makalesinde Azure Cosmos kapsayıcıları ve veritabanları oluştur'a](provision-throughput-autopilot.md) bakın.

Bu yaklaşımın avantajı, sisteminizdeki ölçekleme gereksinimlerini yönetmenin en kolay yolu olmasıdır. **Yapılandırılmış RU aralıkları içinde**fiyat sınırlaması uygulamamayı garanti eder. Dezavantajı, sisteminizde ölçekleme gereksinimleri öngörülebilir ise, Otomatik Pilot ısmarlama kontrol düzlemi veya Yukarıda belirtilen SDK düzeyi yaklaşımları kullanarak daha ölçekleme ihtiyaçlarını işleme daha az maliyet-etkin bir yol olabilir.

## <a name="next-steps"></a>Sonraki adımlar

- Java uygulaması kullanarak [Cassandra API hesabı, veritabanı ve tablo oluşturmaya](create-cassandra-api-account-java.md) başlama
