---
title: Verilerinizi Azure Cosmos DB- Tutorial'deki Bir Cassandra API hesabına geçirin
description: Bu eğitimde, Apache Cassandra'dan Azure Cosmos DB'deki Cassandra API hesabına veri kopyalamak için Spark'& CQL Copy komutunu nasıl kullanacağınızı öğrenin
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 12/03/2018
ms.custom: seodec18
Customer intent: As a developer, I want to migrate my existing Cassandra workloads to Azure Cosmos DB so that the overhead to manage resources, clusters, and garbage collection is automatically handled by Azure Cosmos DB.
ms.openlocfilehash: c754740369da6d0a8084b9b60ef178fb28e32f1b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75445682"
---
# <a name="tutorial-migrate-your-data-to-cassandra-api-account-in-azure-cosmos-db"></a>Öğretici: Verilerinizi Azure Cosmos DB'deki Cassandra API hesabına geçirin

Geliştirici olarak, şirket içinde veya bulutta çalışan mevcut Cassandra iş yükleriniz olabilir ve bunları Azure'a geçirmek isteyebilirsiniz. Bu tür iş yüklerini Azure Cosmos DB'deki Bir Cassandra API hesabına geçirebilirsiniz. Bu öğretici, Apache Cassandra verilerini Azure Cosmos DB'deki Cassandra API hesabına geçirmek için kullanılabilen farklı seçeneklerhakkında talimatlar sağlar.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Geçiş planlaması
> * Geçiş önkoşulları
> * cqlsh COPY komutunu kullanarak verileri geçirme
> * Spark'ı kullanarak verileri geçirme

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="prerequisites-for-migration"></a>Geçiş önkoşulları

* **Elde iş gereksinimlerinizi tahmin edin:** Azure Cosmos DB'deki Cassandra API hesabına veri geçirmeden önce, iş yükünüzün iş yükü ndeki iş ortası gereksinimlerini tahmin etmeniz gerekir. Genel olarak, CRUD işlemlerine gereken ortalama aktarım hızıyla başlamanız ve ardından Ayıklama Dönüştürme Yükleme (ETL) için veya öngörülemeyen işlemler için gereken fazladan aktarım hızını eklemeniz önerilir. Geçişi planlamak için şu ayrıntılara ihtiyacınız vardır: 

  * **Mevcut veri boyutu veya tahmini veri boyutu:** Minimum veritabanı boyutunu ve aktarım hızı gereksinimini tanımlar. Yeni uygulama için veri boyutu tahmini yapıyorsanız, verilerin satırlara düzgün dağıtıldığını varsayabilir ve veri boyutuyla çarparak değeri tahmin edebilirsiniz. 

  * **Gerekli iş bilgili lik:** Yaklaşık okuma (sorgu/alma) ve yazma (update/delete/insert) iş lenme oranı. Bu değer hem gerekli istek birimlerini hem de eylemsizlik durumunda veri boyutu hesaplamak için gereklidir.  

  * **Şema:** Mevcut Cassandra kümenize cqlsh üzerinden bağlanın ve şemayı Cassandra'dan aktarın: 

    ```bash
    cqlsh [IP] "-e DESC SCHEMA" > orig_schema.cql
    ```

    Varolan iş yükünüzün gereksinimlerini tanımladıktan sonra, toplanan iş yükü gereksinimlerine göre bir Azure Cosmos hesabı, veritabanı ve kapsayıcı oluşturmanız gerekir.  

  * **Bir işlem için RU ücretini belirleyin:** Cassandra API tarafından desteklenen SDK'lardan herhangi birini kullanarak RUs'ları belirleyebilirsiniz. Bu örnekte .NET sürümünün RU ücretleri gösterilmektedir.

    ```csharp
    var tableInsertStatement = table.Insert(sampleEntity);
    var insertResult = await tableInsertStatement.ExecuteAsync();

    foreach (string key in insertResult.Info.IncomingPayload)
      {
         byte[] valueInBytes = customPayload[key];
         double value = Encoding.UTF8.GetString(valueInBytes);
         Console.WriteLine($"CustomPayload:  {key}: {value}");
      }
    ```

* **Gerekli aktarım hızını ayırma** Gereksinimleriniz arttıkça Azure Cosmos DB depolamayı ve aktarım hızını otomatik olarak ölçeklendirilebilir. Aktarım hızı gereksinimlerinizi tahmin etmek için [Azure Cosmos DB istek birimi hesaplayıcısını](https://www.documentdb.com/capacityplanner) kullanabilirsiniz. 

* **Cassandra API hesabında tablolar oluşturun:** Verileri geçişyapmaya başlamadan önce, tüm tablolarınızı Azure portalından veya cqlsh'ten önceden oluşturun. Veritabanı düzeyi iş düzeyi verime sahip bir Azure Cosmos hesabına geçiş yapıyorsunuz, Azure Cosmos kapsayıcılarını oluştururken bir bölüm anahtarı sağladığından emin olun.

* **Aktarım hızını artırma:** Veri geçişinizin süresi, Azure Cosmos DB'deki tablolar için sağladığınız aktarım hızı miktarına bağlıdır. Geçiş süresince aktarım hızını artırın. Daha yüksek aktarım hızı ile, hız sınırlamayı önleyebilir ve daha kısa sürede geçişi tamamlayabilirsiniz. Geçişi tamamladıktan sonra maliyet tasarrufu sağlamak için aktarım hızını azaltın. Azure Cosmos hesabının kaynak veritabanınızla aynı bölgede olması da önerilir. 

* **SSL’yi etkinleştirme:** Azure Cosmos DB sıkı güvenlik gereksinimleri ve standartlarına sahiptir. Hesabınız ile etkileşim kurarken SSL’yi etkinleştirdiğinizden emin olun. SSH ile CQL kullandığınızda, SSL bilgilerini sağlama seçeneğine sahip olursunuz.

## <a name="options-to-migrate-data"></a>Verileri geçirme seçenekleri

Verileri mevcut Cassandra iş yüklerinden Azure Cosmos DB'ye taşırken şu seçenekleri kullanabilirsiniz:

* [cqlsh COPY komutunu kullanma](#migrate-data-using-cqlsh-copy-command)  
* [Spark'ı kullanma](#migrate-data-using-spark) 

## <a name="migrate-data-using-cqlsh-copy-command"></a>cqlsh COPY komutunu kullanarak verileri geçirme

[CQL COPY komutu,](https://cassandra.apache.org/doc/latest/tools/cqlsh.html#cqlsh) yerel verileri Azure Cosmos DB'deki Cassandra API hesabına kopyalamak için kullanılır. Verileri kopyalamak için aşağıdaki adımları kullanın:

1. Cassandra API hesabınızın bağlantı dizesi bilgilerini alın:

   * [Azure portalında](https://portal.azure.com)oturum açın ve Azure Cosmos hesabınıza gidin.

   * cqlsh'den Cassandra API hesabınıza bağlanmak için ihtiyacınız olan tüm bilgilerin yer aldığı **Bağlantı Dizesi** bölmesini açın.

2. Portaldan bağlantı bilgilerini kullanarak cqhsh oturumu açın.

3. Yerel verileri Cassandra API hesabına kopyalamak için CQL COPY komutunu kullanın.

   ```bash
   COPY exampleks.tablename FROM filefolderx/*.csv 
   ```

## <a name="migrate-data-using-spark"></a>Spark'ı kullanarak verileri geçirme 

Verileri Spark ile Cassandra API hesabına geçirmek için aşağıdaki adımları kullanın:

- Azure [Databricks kümesini](cassandra-spark-databricks.md) veya [HDInsight kümesini](cassandra-spark-hdinsight.md) sağlama 

- [Tablo kopyalama işlemini](cassandra-spark-table-copy-ops.md) kullanarak verileri hedef Cassandra API bitiş noktasına taşıma 

Azure sanal makinelerinde veya başka bir bulutta varolan bir kümede bulunan verileriniz varsa, Spark işlerini kullanarak verileri geçirmeniz önerilir bir seçenektir. Bu seçenek, Spark'ın bir kez veya düzenli olarak yutulması için aracı olarak ayarlanmasını gerektirir. Şirket içi ve Azure arasındaki Azure ExpressRoute bağlantısını kullanarak bu geçişi hızlandırabilirsiniz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulmadığında, kaynak grubunu, Azure Cosmos hesabını ve ilgili tüm kaynakları silebilirsiniz. Bunu yapmak için, sanal makine için kaynak grubunu seçin, **Sil'i**seçin ve ardından silmek için kaynak grubunun adını onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, verilerinizi Azure Cosmos DB'deki Cassandra API hesabına nasıl geçirtileceğinizi öğrendiniz. Artık diğer Azure Cosmos DB kavramları hakkında bilgi edinmek için aşağıdaki makaleye geçebilirsiniz:

> [!div class="nextstepaction"]
> [Azure Cosmos DB'deki ayarlanabilir tutarlılık düzeyleri](../cosmos-db/consistency-levels.md)


