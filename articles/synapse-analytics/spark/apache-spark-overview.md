---
title: Azure Synapse Analytics'te Apache Spark nedir?
description: Bu makale, Azure Synapse Analytics'teki Apache Spark'a ve Spark'ı kullanabileceğiniz farklı senaryolara giriş sağlar.
services: synapse-analytics
author: euangMS
ms.service: synapse-analytics
ms.topic: overview
ms.subservice: ''
ms.date: 04/15/2020
ms.author: euang
ms.reviewer: euang
ms.openlocfilehash: 4155e419bb03613abad808ca09d84bcb6583291f
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81423652"
---
# <a name="what-is-apache-spark-in-azure-synapse-analytics"></a>Azure Synapse Analytics'te Apache Spark nedir

Apache Spark, büyük veri analitik uygulamaların performansını artırmak için bellek içi işlemeyi destekleyen paralel bir işleme çerçevesidir. Azure Synapse Analytics'teki Apache Spark, Microsoft'un Bulut'taki Apache Spark uygulamalarından biridir. Azure Synapse, Azure'da bir Kıvılcım havuzu (önizleme) oluşturmayı ve yapılandırmayı kolaylaştırır. Azure Synapse'deki Kıvılcım havuzları Azure Depolama ve Azure Veri Gölü Oluşturma 2 Depolama ile uyumludur. Böylece Azure'da depolanan verilerinizi işlemek için Azure Spark havuzlarını kullanabilirsiniz.

![Spark: birleşik çerçeve](./media/apache-spark-overview/spark-overview.png)

[!INCLUDE [preview](../includes/note-preview.md)]

## <a name="what-is-apache-spark"></a>Apache Kıvılcım nedir

Apache Spark, bellek içi küme hesaplaması için ilkel liksağlar. Bir Spark işi belleğe veri yükleyip önbelleğe alabilir ve verileri tekrarlayarak sorgulayabilir. Bellek içi bilgi işlem, disk tabanlı uygulamalardan çok daha hızlıdır. Spark, yerel koleksiyonlar gibi dağıtılmış veri kümelerini işlemenize izin vermek için birden çok programlama diliyle de tümleşir. Her şeyi harita olarak yapılandırmaya gerek olmadığı için işlem sayısı azalmış olur.

![Geleneksel MapReduce vs Spark](./media/apache-spark-overview/map-reduce-vs-spark.png)

Azure Synapse'deki Kıvılcım havuzları tam olarak yönetilen bir Spark hizmeti sunar. Synapse Analytics'te bir Kıvılcım havuzu oluşturmanın avantajları burada listelenmiştir.

| Özellik | Açıklama |
| --- | --- |
| Hız ve verimlilik |Kıvılcım örnekleri yaklaşık 2 dakika içinde 60'tan az düğüm için, 60'tan fazla düğüm için ise yaklaşık 5 dakika içinde başlar. Örnek, varsayılan olarak, bir not defteri bağlantısı tarafından canlı tutulmadığı sürece, yürütülen son işten 5 dakika sonra kapanır. |
| Oluşturma kolaylığı |Azure portalı, Azure PowerShell veya Synapse Analytics .NET SDK'yı kullanarak Birkaç dakika içinde Azure Synapse'de yeni bir Kıvılcım havuzu oluşturabilirsiniz. Bkz. [Synapse Analytics'teki Spark havuzları ile başlayın.](apache-spark-notebook-create-spark-use-sql.md) |
| Kullanım kolaylığı |Synapse [Analytics, Nteract'ten](https://nteract.io/)türetilen özel bir dizüstü bilgisayar içerir. Etkileşimli veri işleme ve görselleştirme için bu not defterlerini kullanabilirsiniz.|
| REST API'leri |Spark In Synapse Analytics, işleri uzaktan göndermek ve izlemek için REST API tabanlı bir Spark iş sunucusu olan [Apache Livy'yi](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)içerir. |
| Azure Veri Gölü Depolama Oluşturma 2 desteği| Azure Synapse'deki Kıvılcım havuzları, AZURE Veri Gölü Depolama Oluşturma Oluşturma 2'nin yanı sıra BLOB depolamayı da kullanabilir. Veri Gölü Depolama hakkında daha fazla bilgi için Azure [Veri Gölü Depolamasına Genel Bakış'a](../../data-lake-store/data-lake-store-overview.md)bakın. |
| Üçüncü taraf IDE’lerle tümleştirme | Azure Synapse, [Jetbrains'in IntelliJ IDEA'sı](https://www.jetbrains.com/idea/) için, uygulama oluşturmak ve Bir Kıvılcım havuzuna göndermek için yararlı olan bir IDE eklentisi sağlar. |
| Önceden yüklenmiş Anaconda kitaplıkları |Azure Synapse'deki kıvılcım havuzları, Önceden yüklenmiş Anaconda kitaplıklarıyla birlikte gelir. [Anaconda](https://docs.continuum.io/anaconda/) machine learning, veri analizi, görselleştirme vb. için 200’e yakın kitaplık sağlar. |
| Ölçeklenebilirlik | Azure Synapse havuzlarında bulunan Apache Spark Otomatik Ölçeketkinleştirilebilir, böylece havuzlar gerektiği gibi yukarı ve aşağı ölçeklenebilir. Ayrıca, tüm veriler Azure Depolama veya Veri Gölü Depolama'da depolandığından, Spark havuzları veri kaybı olmadan kapatılabilir. |

Azure Sinaps'taki Kıvılcım havuzları varsayılan olarak havuzlarda kullanılabilen aşağıdaki bileşenleri içerir.

- [Spark Core](https://spark.apache.org/docs/latest/). Spark Core, Spark SQL, GraphX ve MLlib içerir.
- [Anaconda](https://docs.continuum.io/anaconda/)
- [Apaçi Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)
- [Nteract dizüstü bilgisayar](https://nteract.io/)

## <a name="spark-pool-architecture"></a>Kıvılcım havuz mimarisi

Spark'ın Synapse Analytics'te nasıl çalıştığını anlayarak Spark'ın bileşenlerini anlamak kolaydır.

Spark uygulamaları, ana programınızdaki (sürücü programı olarak adlandırılır) SparkContext nesnesi tarafından koordine edilen bir havuzdaki bağımsız işlem kümeleri olarak çalıştırılır.

SparkContext, uygulamalar arasında kaynak ayıran küme yöneticisine bağlanabilir. Küme yöneticisi [Apache Hadoop İPN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html)olduğunu. Bağlandıktan sonra Spark, havuzdaki düğümler üzerinde, hesaplamaları çalıştıran ve uygulamanız için veri depolayan işlemler olan uygulayıcılar edinir. Ardından, uygulama kodunuzu (SparkContext’e geçirilen JAR veya Python dosyaları ile tanımlanır) yürütücülerinize gönderir. Son olarak SparkContext, yürütücülere çalıştırılacak görevleri gönderir.

SparkContext kullanıcının ana işlevini çalıştırıyor ve düğümler üzerinde çeşitli paralel işlemleri yürütür. Daha sonra SparkContext, işlemlerin sonuçlarını toplar. Düğümler dosya sisteminden ve dosya sisteminden veri okuyup yazar. Düğümler ayrıca, bellekteki verileri Esnek Dağıtılmış Veri Kümeleri (RDD'ler) olarak dönüştürdü.

SparkContext, Spark havuzuna bağlanır ve bir uygulamayı yönlendirilmiş bir grafiğe (DAG) dönüştürmekten sorumludur. Grafik, düğümler üzerinde bir yürütme işlemi içinde yürütülen tek tek görevlerden oluşur. Her uygulama, uygulama süresince devam eden ve görevleri birden fazla iş parçacığında çalıştıran kendi yürütücü işlemine sahiptir.

## <a name="apache-spark-in-synapse-analytics-use-cases"></a>Synapse Analytics'te Apache Spark kullanım örnekleri

Synapse Analytics'teki Spark havuzları aşağıdaki önemli senaryoları etkinleştirin:

### <a name="data-engineeringdata-preparation"></a>Veri Mühendisliği/Veri Hazırlama

Apache Spark, synapse Analytics'te daha değerli hale getirilebilmek ve daha sonra diğer hizmetler tarafından tüketilebilmek için büyük hacimli verilerin hazırlanmasını ve işlenmesini destekleyen birçok dil özelliği içerir. Bu, birden çok dil (C#, Scala, PySpark, Spark SQL) aracılığıyla etkinleştirilir ve işleme ve bağlantı için verilen kitaplıklar.

### <a name="machine-learning"></a>Machine Learning

Apache Spark, Synapse Analytics'teki bir Spark havuzundan kullanabileceğiniz Spark'ın üzerine inşa edilmiş bir makine öğrenimi kütüphanesi olan [MLlib](https://spark.apache.org/mllib/)ile birlikte gelir. Synapse Analytics'teki kıvılcım havuzları, makine öğrenimi de dahil olmak üzere veri bilimi için çeşitli paketleriçeren bir Python dağıtımı olan Anaconda'yı da içerir. Dizüstü bilgisayarlar için yerleşik destekle birleştirildiğinde, makine öğrenimi uygulamaları oluşturmak için bir ortama sahip siniz.

## <a name="where-do-i-start"></a>Nereden başlayacağım?

Synapse Analytics'te Apache Spark hakkında daha fazla bilgi edinmek için aşağıdaki makaleleri kullanın:

- [Hızlı başlangıç: Azure Synapse'de Bir Kıvılcım havuzu oluşturma](./apache-spark-notebook-create-spark-use-sql.md)
- [Öğretici: Apache Spark kullanarak makine öğrenimi](./apache-spark-machine-learning-mllib-notebook.md)
- [Apache Spark resmi belgeler](https://spark.apache.org/docs/latest/)

> [!NOTE]
> Bazı resmi Apache Spark belgeleri kıvılcım konsolu kullanarak dayanır, bu Azure Synapse Spark mevcut değildir, not defteri veya IntelliJ deneyimleri yerine kullanın

## <a name="next-steps"></a>Sonraki adımlar

Bu genel bakışta, Azure Synapse Analytics'te Apache Spark hakkında temel bir bilgi edinebilirsiniz. Azure Synapse Analytics'te Nasıl Bir Kıvılcım havuzu oluşturabilirsiniz öğrenmek için bir sonraki makaleye ilerleyin:

- [Azure Synapse'de Bir Kıvılcım havuzu oluşturma](./apache-spark-notebook-create-spark-use-sql.md)
