---
title: Cosmos DB ile Apache Spark & Apache Kafka - Azure HDInsight
description: Apache Kafka'dan gelen verileri okumak ve ardından Azure Cosmos DB'de depolamak için Apache Spark Structured Streaming'i nasıl kullanacağınızı öğrenin. Bu örnekte, HDInsight üzerinde Spark’tan bir Jupyter not defterini kullanarak verilerinizi akışla aktaracaksınız.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 11/18/2019
ms.openlocfilehash: 04faafca0811e60ded47d1e91a82054a1c1cdb25
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74406166"
---
# <a name="use-apache-spark-structured-streaming-with-apache-kafka-and-azure-cosmos-db"></a>Apache Kafka ve Azure Cosmos DB ile Apache Spark Yapılandırılmış Akış Kullanma

Azure HDInsight'ta [Apache Kafka'dan](https://kafka.apache.org/) gelen verileri okumak ve verileri Azure Cosmos DB'de depolamak için [Apache Spark](https://spark.apache.org/) [Structured Streaming'i](https://spark.apache.org/docs/latest/structured-streaming-programming-guide.html) nasıl kullanacağınızı öğrenin.

[Azure Cosmos DB,](https://azure.microsoft.com/services/cosmos-db/) dünya çapında dağıtılmış, çok modelli bir veritabanıdır. Bu örnekte bir SQL API veritabanı modeli kullanır. Daha fazla bilgi için [Azure Cosmos DB belgesine Hoş Geldiniz'e](../cosmos-db/introduction.md) bakın.

Spark yapılandırılmış akışı, Spark SQL üzerinde yerleşik bir akış işleme altyapısıdır. Bu altyapıyı kullanarak, statik veriler üzerinde toplu hesaplamayla aynı şekilde akış hesaplamalarını ifade edebilirsiniz. Yapılandırılmış Akış hakkında daha fazla bilgi için Apache.org'daki [Yapılandırılmış Akış Programlama Kılavuzu'na](https://spark.apache.org/docs/2.2.0/structured-streaming-programming-guide.html) bakın.

> [!IMPORTANT]  
> Bu örnekte HDInsight 3.6'da Spark 2.2 kullanılmıştır.
>
> Bu belgede yer alan adımlar hem HDInsight üzerinde Spark hem de HDInsight kümesinde Kafka içeren bir Azure kaynak grubu oluşturur. Bu kümelerin her ikisi de Spark kümesinin Kafka kümesiyle doğrudan iletişim kurmasına olanak tanıyan bir Azure Sanal Ağı içinde bulunur.
>
> Bu belgedeki adımları tamamladığınızda, aşırı ücretlerden kaçınmak için kümeleri silmeyi unutmayın.

## <a name="create-the-clusters"></a>Kümeleri oluşturma

HDInsight'taki Apache Kafka, Kafka brokerlarına halka açık internet üzerinden erişim sağlamaz. Kafka ile konuşan her şey, Kafka kümesindeki düğümlerle aynı Azure sanal ağında olmalıdır. Bu örnekte, hem Kafka hem de Kıvılcım kümeleri bir Azure sanal ağında bulunur. Aşağıdaki diyagram, kümeler arasındaki iletişimin nasıl aktığını gösterir:

![Bir Azure sanal ağında Spark ve Kafka kümeleri diyagramı](./media/apache-kafka-spark-structured-streaming-cosmosdb/apache-spark-kafka-vnet.png)

> [!NOTE]  
> Kafka hizmeti, sanal ağ içindeki iletişimle sınırlıdır. SSH ve Ambari gibi küme üzerindeki diğer hizmetlere internet üzerinden erişilebilir. HDInsight üzerinde kullanılabilir olan genel bağlantı noktaları hakkında daha fazla bilgi için bkz. [HDInsight Tarafından Kullanılan Bağlantı Noktaları ve URI’ler](hdinsight-hadoop-port-settings-for-services.md).

Bir Azure sanal ağı, Kafka ve Spark kümelerini el ile oluşturabiliyorken, Azure Kaynak Yöneticisi şablonu kullanmak daha kolaydır. Azure sanal ağıKafka ve Spark kümelerini Azure aboneliğinize dağıtmak için aşağıdaki adımları kullanın.

1. Aşağıdaki düğmeyi kullanarak Azure'da oturum açın ve şablonu Azure portalında açın.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fhdinsight-spark-scala-kafka-cosmosdb%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="./media/apache-kafka-spark-structured-streaming-cosmosdb/resource-manager-deploy.png" alt="Deploy to Azure"/>
    </a>

    Azure Kaynak Yöneticisi şablonu, bu proje için GitHub deposunda bulunur ([https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb)).

    Bu şablon aşağıdaki kaynakları oluşturur:

   * HDInsight 3.6 kümesi üzerinde bir Kafka.

   * HDInsight 3.6 kümesinde bir Kıvılcım.

   * HDInsight kümeleri içeren bir Azure Sanal Ağı. Şablon tarafından oluşturulan sanal ağ 10.0.0.0/16 adres alanını kullanır.

   * Azure Cosmos DB SQL API veritabanı.

    > [!IMPORTANT]  
    > Bu örnekte kullanılan yapılandırılmış akış not defteri, HDInsight 3.6 üzerinde Spark gerektirir. HDInsight üzerinde Spark’ın daha önceki bir sürümünü kullanıyorsanız, not defterini kullanırken hatalarla karşılaşırsınız.

1. **Özel dağıtım** bölümündeki girişleri doldurmak için aşağıdaki bilgileri kullanın:

    |Özellik |Değer |
    |---|---|
    |Abonelik|Azure aboneliğinizi seçin.|
    |Kaynak grubu|Bir grup oluşturun veya varolan bir grup seçin. Bu grup HDInsight kümesini içerir.|
    |Cosmos DB Hesap Adı|Bu değer Cosmos DB hesabının adı olarak kullanılır. Ad yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir. Ad, 3-31 karakter arası uzunlukta olmalıdır.|
    |Taban Küme Adı|Bu değer, Kıvılcım ve Kafka kümeleri için temel ad olarak kullanılır. Örneğin, **myhdi** girerken __kıvılcım-myhdi__ adında bir Kıvılcım kümesi ve **kafka-myhdi**adında bir Kafka kümesi oluşturur.|
    |Küme Sürümü|HDInsight küme sürümü. Bu örnek HDInsight 3.6 ile sınandı ve diğer küme türleri ile çalışmayabilir.|
    |Küme Oturum Açma Kullanıcı Adı|Kıvılcım ve Kafka kümelerinin yönetici kullanıcı adı.|
    |Küme Oturum Açma Parolası|Kıvılcım ve Kafka kümeleri için yönetici kullanıcı parolası.|
    |Ssh Kullanıcı Adı|SSH kullanıcı Kıvılcım ve Kafka kümeleri için oluşturmak için.|
    |Ssh Şifre|Kıvılcım ve Kafka kümeleri için SSH kullanıcısının şifresi.|

    ![HDInsight özel dağıtım değerleri](./media/apache-kafka-spark-structured-streaming-cosmosdb/hdi-custom-parameters.png)

1. **Hüküm ve Koşullar**’ı okuyun ve ardından **Yukarıda belirtilen hüküm ve koşulları kabul ediyorum**’u seçin.

1. Son olarak, **Satın Al'ı**seçin. Kümeleri, sanal ağı ve Cosmos DB hesabını oluşturmak 45 dakika kadar sürebilir.

## <a name="create-the-cosmos-db-database-and-collection"></a>Cosmos DB veritabanını ve koleksiyonunu oluşturma

Bu belgede kullanılan proje verileri Cosmos DB'de depolar. Kodu çalıştırmadan önce Cosmos DB örneğinde bir _veritabanı_ ve _koleksiyon_ oluşturmanız gerekir. Ayrıca belge bitiş noktasını ve Cosmos DB isteklerini doğrulamak için kullanılan _anahtarı_ da almanız gerekir.

Bunu yapmanın bir yolu [Azure CLI'yi](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)kullanmaktır. Aşağıdaki komut dosyası adlı `kafkadata` bir veritabanı `kafkacollection`ve bir koleksiyon adlı oluşturacak. Daha sonra birincil anahtarı döndürür.

```azurecli
#!/bin/bash

# Replace 'myresourcegroup' with the name of your resource group
resourceGroupName='myresourcegroup'
# Replace 'mycosmosaccount' with the name of your Cosmos DB account name
name='mycosmosaccount'

# WARNING: If you change the databaseName or collectionName
#          then you must update the values in the Jupyter notebook
databaseName='kafkadata'
collectionName='kafkacollection'

# Create the database
az cosmosdb sql database create --account-name $name --name $databaseName --resource-group $resourceGroupName

# Create the collection
az cosmosdb sql container create --account-name $name --database-name $databaseName --name $collectionName --partition-key-path "/my/path" --resource-group $resourceGroupName

# Get the endpoint
az cosmosdb show --name $name --resource-group $resourceGroupName --query documentEndpoint

# Get the primary key
az cosmosdb keys list --name $name --resource-group $resourceGroupName --type keys
```

Belge bitiş noktası ve birincil anahtar bilgileri aşağıdaki metne benzer:

```text
# endpoint
"https://mycosmosaccount.documents.azure.com:443/"
# key
"YqPXw3RP7TsJoBF5imkYR0QNA02IrreNAlkrUMkL8EW94YHs41bktBhIgWq4pqj6HCGYijQKMRkCTsSaKUO2pw=="
```

> [!IMPORTANT]  
> Jupyter Notebook'larda ihtiyaç duyulduğu ndan bitiş noktasını ve anahtar değerleri kaydedin.

## <a name="get-the-notebooks"></a>Defterleri alın

Bu belgede açıklanan örneğin kodu . [https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb](https://github.com/Azure-Samples/hdinsight-spark-scala-kafka-cosmosdb)

## <a name="upload-the-notebooks"></a>Not defterlerini karşıya yükleme

HDInsight kümesinde projedeki not defterlerini Spark'ınıza yüklemek için aşağıdaki adımları kullanın:

1. Web tarayıcınızdan Spark kümeniz üzerindeki Jupyter not defterine bağlanın. Aşağıdaki URL’de `CLUSTERNAME` değerini __Spark__ kümenizin adıyla değiştirin:

        https://CLUSTERNAME.azurehdinsight.net/jupyter

    Sorulduğunda, kümeyi oluştururken kullanılan küme kullanıcı adı (yönetici) ve parolasını girin.

2. Sayfanın sağ üst __kısmından, Stream-taxi-data-to-kafka.ipynb__ dosyasını kümeye yüklemek için __Yükle__ düğmesini kullanın. Karşıya yüklemeyi başlatmak için __Aç__’ı seçin.

3. Not defterleri __listesindeStream-taxi-data-to-kafka.ipynb__ girişini bulun ve yanındaki __Yükle__ düğmesini seçin.

4. __Kafka-to-Cosmos-DB.ipynb__ not defterini yüklemek için 1-3 adımlarını tekrarlayın.

## <a name="load-taxi-data-into-kafka"></a>Taksi verilerini Kafka'ya yükleyin

Dosyalar yüklendikten sonra, not defterini açmak __için Stream-taxi-data to-kafka.ipynb__ girişini seçin. Kafka'ya veri yüklemek için not defterindeki adımları izleyin.

## <a name="process-taxi-data-using-spark-structured-streaming"></a>Spark Structured Streaming kullanarak taksi verilerini işleme

[Jupyter Notebook](https://jupyter.org/) giriş sayfasından __Kafka-to-Cosmos-DB.ipynb__ girişini seçin. Kafka'dan veri akışı için not defterindeki adımları izleyin ve Spark Structured Streaming'i kullanarak Azure Cosmos DB'ye veri aktarın.

## <a name="next-steps"></a>Sonraki adımlar

Apache Spark Structured Streaming'i kullanmayı öğrendiğiniz için, Apache Spark, Apache Kafka ve Azure Cosmos DB ile çalışma hakkında daha fazla bilgi edinmek için aşağıdaki belgelere bakın:

* [Apache Kafka ile Apache Spark akışı (DStream) nasıl kullanılır.](hdinsight-apache-spark-with-kafka.md)
* [HDInsight'ta Jupyter Notebook ve Apache Spark ile başlayın](spark/apache-spark-jupyter-spark-sql.md)
* [Azure Cosmos DB’ye hoş geldiniz](../cosmos-db/introduction.md)
