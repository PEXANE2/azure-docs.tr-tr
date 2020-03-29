---
title: Azure Cosmos DB toplu yürütücü kitaplığına genel bakış
description: Toplu yürütme kitaplığı tarafından sunulan toplu alma ve toplu güncelleştirme API'leri aracılığıyla Azure Cosmos DB'de toplu işlemler gerçekleştirin.
author: tknandu
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: ramkris
ms.reviewer: sngun
ms.openlocfilehash: 9d335bcf6daf0b38e7a68ca2d40894dd64c93e40
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75442147"
---
# <a name="azure-cosmos-db-bulk-executor-library-overview"></a>Azure Cosmos DB toplu yürütücü kitaplığına genel bakış
 
Azure Cosmos DB hızlı, esnek ve küresel ölçekte dağıtılan bir veritabanı hizmeti olarak, esnek bir şekilde ölçeği genişleterek şunları desteklemek için tasarlanmıştır: 

* Büyük okuma ve yazma işlem hızı (saniyede milyonlarca işlem).  
* Öngörülebilir milisaniye düzeyinde bir gecikme süresiyle çok büyük (yüzlerce terabayt, hatta daha fazla) işlem verilerini depolama.  

Toplu yürütücü kitaplığı bu muazzam işlem hızı ve depolamadan yararlanmanıza yardımcı olur. Toplu yürütücü kitaplığı toplu içeri aktarma ve toplu güncelleştirme API'leri üzerinden Azure Cosmos DB'de toplu işlemler yapmanızı sağlar. Aşağıdaki bölümlerde toplu yürütücü kitaplığının özellikleri hakkında verilen bilgileri okuyabilirsiniz. 

> [!NOTE] 
> Şu anda, toplu yürütme kitaplığı alma ve güncelleştirme işlemlerini destekler ve bu kitaplık yalnızca Azure Cosmos DB SQL API ve Gremlin API hesapları tarafından desteklenir.
 
## <a name="key-features-of-the-bulk-executor-library"></a>Toplu yürütme kitaplığıntemel özellikleri  
 
* Bir kapsayıcıya ayrılan iş ortasını doyuramak için gereken istemci tarafı işlem kaynaklarını önemli ölçüde azaltır. Toplu alma API'sini kullanarak veri yazan tek bir iş parçacığı uygulaması, istemci makinenin CPU'unu doyururken verileri paralel olarak yazan çok iş parçacığı uygulamasıyla karşılaştırıldığında 10 kat daha fazla yazma iş parçacığı elde eder.  

* İstek, istek zaman zamanlarını ve diğer geçici özel durumları kitaplık içinde verimli bir şekilde işleyerek hız sınırlamasını işlemek için uygulama mantığı yazmanın sıkıcı görevlerini özetler.  

* Ölçeklendirmek için toplu işlemler gerçekleştiren uygulamalar için basitleştirilmiş bir mekanizma sağlar. Azure VM'de çalışan tek bir toplu yürütme örneği 500K RU/s'den fazla tüketebilir ve tek tek istemci VM'lere ek örnekler ekleyerek daha yüksek bir iş elde etme oranı elde edebilirsiniz.  
 
* Bir terabayttan daha fazla veriyi ölçeklendirilebilir bir mimari kullanarak bir saat içinde toplu olarak aktarabilir.  

* Azure Cosmos kapsayıcılarında varolan verileri düzeltme ekinde veyare olarak güncelleyebilir. 
 
## <a name="how-does-the-bulk-executor-operate"></a>Toplu uygulayıcı nasıl çalışır? 

Belgeleri almak veya güncelleştirmek için toplu bir işlem bir yığın varlıkla tetiklendiğinde, bunlar başlangıçta Azure Cosmos DB bölüm anahtar aralığına karşılık gelen kovalara karıştırılır. Bir bölüm anahtar aralığına karşılık gelen her kova içinde, bunlar mini gruplara ayrılır ve her mini toplu işlem sunucu tarafında işlenen bir yük olarak hareket eder. Toplu uygulayıcı kitaplığı, bu mini toplu iş birimlerinin hem bölüm anahtar aralıkları içinde hem de arasında eşzamanlı olarak yürütülmesi için optimizasyonlar oluşturmuştur. Aşağıdaki resim, toplu yürütmenin verileri farklı bölüm tuşlarına nasıl toplu olarak aktardığını gösterir:  

![Toplu uygulayıcı mimari](./media/bulk-executor-overview/bulk-executor-architecture.png)

Toplu yürütücü kitaplığı, koleksiyona ayrılan iş kısmını maksimum olarak kullandığından emin olmasını sağlar. Hız sınırlamalarını ve zaman aşımlarını verimli bir şekilde işlemek için her Azure Cosmos DB bölüm anahtar aralığı için [AIMD tarzı bir tıkanıklık kontrol mekanizması](https://tools.ietf.org/html/rfc5681) kullanır. 

## <a name="next-steps"></a>Sonraki Adımlar 
  
* [.NET](bulk-executor-dot-net.md) ve [Java'daki](bulk-executor-java.md)toplu yürütme kitaplığını tüketen örnek uygulamaları deneyerek daha fazla bilgi edinin.  
* [.NET](sql-api-sdk-bulk-executor-dot-net.md) ve [Java'daki](sql-api-sdk-bulk-executor-java.md)toplu uygulayıcı SDK bilgilerine ve sürüm notlarına göz atın.
* Toplu uygulayıcı kitaplığı, daha fazla bilgi edinmek için Azure [Cosmos DB Spark bağlayıcısı](spark-connector.md) makalesine bakın.  
* Toplu uygulayıcı kitaplığı, verileri kopyalamak için Azure Veri Fabrikası için [Azure Cosmos DB bağlayıcısının](https://aka.ms/bulkexecutor-adf-v2) yeni sürümüne de entegre edilmiştir.
