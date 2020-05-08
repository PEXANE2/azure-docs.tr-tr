---
title: Verilerinizi Azure Cosmos DB bir Cassandra API hesabına geçirme-öğretici
description: Bu öğreticide, Apache Cassandra 'dan verileri Azure Cosmos DB ' de bir Cassandra API hesabına kopyalamak için CQL copy komutunu & Spark 'ın nasıl kullanılacağını öğrenin
author: kanshiG
ms.author: govindk
ms.reviewer: sngun
ms.service: cosmos-db
ms.subservice: cosmosdb-cassandra
ms.topic: tutorial
ms.date: 12/03/2018
ms.custom: seodec18
ms.openlocfilehash: b450d8463c09647b8f87547afe9d714c4b8659c4
ms.sourcegitcommit: c535228f0b77eb7592697556b23c4e436ec29f96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82858568"
---
# <a name="tutorial-migrate-your-data-to-cassandra-api-account-in-azure-cosmos-db"></a>Öğretici: verilerinizi Azure Cosmos DB Cassandra API hesaba geçirin

Bir geliştirici olarak, şirket içinde veya bulutta çalışan mevcut Cassandra iş yüklerine sahip olabilirsiniz ve bunları Azure 'a geçirmek isteyebilirsiniz. Bu tür iş yüklerini Azure Cosmos DB bir Cassandra API hesabına geçirebilirsiniz. Bu öğretici, Apache Cassandra verilerinin Azure Cosmos DB Cassandra API hesaba geçirilmesi için kullanılabilecek farklı seçeneklere ilişkin yönergeler sağlar.

Bu öğretici aşağıdaki görevleri kapsar:

> [!div class="checklist"]
> * Geçiş planlaması
> * Geçiş için önkoşullar
> * cqlsh COPY komutunu kullanarak verileri geçirme
> * Spark'ı kullanarak verileri geçirme

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="prerequisites-for-migration"></a>Geçiş için önkoşullar

* **Verimlilik Ihtiyaçlarınızı tahmin edin:** Azure Cosmos DB Cassandra API hesabına veri geçirmeden önce, iş yükünüzün işleme ihtiyaçlarını tahmin etmeniz gerekir. Genel olarak, CRUD işlemlerine gereken ortalama aktarım hızıyla başlamanız ve ardından Ayıklama Dönüştürme Yükleme (ETL) için veya öngörülemeyen işlemler için gereken fazladan aktarım hızını eklemeniz önerilir. Geçişi planlamak için şu ayrıntılara ihtiyacınız vardır: 

  * **Mevcut veri boyutu veya tahmini veri boyutu:** Minimum veritabanı boyutunu ve aktarım hızı gereksinimini tanımlar. Yeni uygulama için veri boyutu tahmini yapıyorsanız, verilerin satırlara düzgün dağıtıldığını varsayabilir ve veri boyutuyla çarparak değeri tahmin edebilirsiniz. 

  * **Gerekli aktarım hızı:** Yaklaşık okuma (sorgu/al) ve yazma (güncelleştirme/silme/ekleme) işleme hızı. Bu değer hem gerekli istek birimlerini hem de eylemsizlik durumunda veri boyutu hesaplamak için gereklidir.  

  * **Şema:** Csqlsh aracılığıyla mevcut Cassandra kümenize bağlanın ve şemayı Cassandra 'dan dışarı aktarın: 

    ```bash
    cqlsh [IP] "-e DESC SCHEMA" > orig_schema.cql
    ```

    Mevcut iş yükünüzün gereksinimlerini tanımladıktan sonra, toplanan verimlilik gereksinimlerine göre bir Azure Cosmos hesabı, veritabanı ve kapsayıcılar oluşturmanız gerekir.  

  * **Bir işlem IÇIN ru ücreti belirleme:** Cassandra API tarafından desteklenen SDK 'Lardan birini kullanarak RUs 'yi belirleyebilirsiniz. Bu örnekte .NET sürümünün RU ücretleri gösterilmektedir.

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

* **Cassandra API hesapta tablo oluşturun:** Verileri geçirmeye başlamadan önce Azure portal veya cönsh 'ten tüm tablolarınızı önceden oluşturun. Veritabanı düzeyinde aktarım hızı olan bir Azure Cosmos hesabına geçiş yapıyorsanız, Azure Cosmos kapsayıcıları oluştururken bir bölüm anahtarı sağladığınızdan emin olun.

* **Aktarım hızını artırma:** Veri geçişinizin süresi, Azure Cosmos DB'deki tablolar için sağladığınız aktarım hızı miktarına bağlıdır. Geçiş süresince aktarım hızını artırın. Daha yüksek aktarım hızı ile, hız sınırlamayı önleyebilir ve daha kısa sürede geçişi tamamlayabilirsiniz. Geçişi tamamladıktan sonra maliyet tasarrufu sağlamak için aktarım hızını azaltın. Ayrıca, Azure Cosmos hesabının kaynak veritabanınız ile aynı bölgede olması önerilir. 

* **TLS 'Yi etkinleştir:** Azure Cosmos DB katı güvenlik gereksinimleri ve standartları vardır. Hesabınızla etkileşim kurarken TLS ' i etkinleştirdiğinizden emin olun. CQL 'i SSH ile birlikte kullandığınızda TLS bilgilerini sağlama seçeneğiniz vardır.

## <a name="options-to-migrate-data"></a>Verileri geçirme seçenekleri

Verileri mevcut Cassandra iş yüklerinden Azure Cosmos DB'ye taşırken şu seçenekleri kullanabilirsiniz:

* [cqlsh COPY komutunu kullanma](#migrate-data-using-cqlsh-copy-command)  
* [Spark'ı kullanma](#migrate-data-using-spark) 

## <a name="migrate-data-using-cqlsh-copy-command"></a>cqlsh COPY komutunu kullanarak verileri geçirme

[CQL Copy komutu](https://cassandra.apache.org/doc/latest/tools/cqlsh.html#cqlsh) , Azure Cosmos DB Cassandra API hesabına yerel verileri kopyalamak için kullanılır. Verileri kopyalamak için aşağıdaki adımları kullanın:

1. Cassandra API hesabınızın bağlantı dizesi bilgilerini alın:

   * [Azure Portal](https://portal.azure.com)oturum açın ve Azure Cosmos hesabınıza gidin.

   * cqlsh'den Cassandra API hesabınıza bağlanmak için ihtiyacınız olan tüm bilgilerin yer aldığı **Bağlantı Dizesi** bölmesini açın.

2. Portaldan bağlantı bilgilerini kullanarak cqhsh oturumu açın.

3. Yerel verileri Cassandra API hesabına kopyalamak için CQL COPY komutunu kullanın.

   ```bash
   COPY exampleks.tablename FROM filefolderx/*.csv 
   ```

## <a name="migrate-data-using-spark"></a>Spark'ı kullanarak verileri geçirme 

Spark ile Cassandra API hesabına veri geçirmek için aşağıdaki adımları kullanın:

- [Azure Databricks kümesi](cassandra-spark-databricks.md) veya [HDInsight kümesi](cassandra-spark-hdinsight.md) sağlama 

- [Tablo kopyalama işlemini](cassandra-spark-table-copy-ops.md) kullanarak verileri hedef Cassandra API uç noktaya taşıma 

Azure sanal makinelerinde veya başka bir bulutta mevcut bir kümede bulunan verileriniz varsa Spark işleri kullanılarak verilerin geçirilmesi önerilen bir seçenektir. Bu seçenek, Spark 'ın bir kez veya normal giriş için bir aracı olarak ayarlanmasını gerektirir. Bu geçişi, şirket içi ve Azure arasında Azure ExpressRoute bağlantısını kullanarak hızlandırabilirsiniz. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında, kaynak grubunu, Azure Cosmos hesabını ve ilgili tüm kaynakları silebilirsiniz. Bunu yapmak için, sanal makine için kaynak grubunu seçin, **Sil**' i seçin ve ardından silinecek kaynak grubunun adını onaylayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, Azure Cosmos DB Cassandra API hesabınıza verilerinizi nasıl geçirebileceğiniz öğrendiniz. Artık diğer Azure Cosmos DB kavramları hakkında bilgi edinmek için aşağıdaki makaleye devam edebilirsiniz:

> [!div class="nextstepaction"]
> [Azure Cosmos DB'deki ayarlanabilir tutarlılık düzeyleri](../cosmos-db/consistency-levels.md)


