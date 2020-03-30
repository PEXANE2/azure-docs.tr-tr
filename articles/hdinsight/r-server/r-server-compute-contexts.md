---
title: HDInsight'ta ML Hizmetleri için içerik seçeneklerini bilgi işlem - Azure
description: HDInsight'ta ML Hizmetleri ile kullanıcıların kullanabileceği farklı bilgi işlem bağlamseçenekleri hakkında bilgi edinin
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 01/02/2020
ms.openlocfilehash: b67bd5b6310e1f8ce35dc14690757209ef62c9d7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75660265"
---
# <a name="compute-context-options-for-ml-services-on-hdinsight"></a>HDInsight'ta ML Hizmetleri için bilgi işlem bağlam ı seçenekleri

Azure HDInsight'taki ML Hizmetleri, bilgi işlem bağlamını ayarlayarak çağrıların nasıl yürütüldedildiğini denetler. Bu makalede, yürütmenin kenar düğümü veya HDInsight kümesinin çekirdekleri arasında paralelleştirilip paralelleştirilemeyeceğini belirtmek için kullanılabilen seçenekler sıralanır.

Bir kümenin kenar düğümü kümeye bağlanmak ve R komut dosyalarınızı çalıştırmak için uygun bir yer sağlar. Kenar düğümü ile, RevoScaler'ın paralelleştirilmiş dağıtılmış işlevlerini kenar düğümü sunucusunun çekirdekleri arasında çalıştırma seçeneğiniz vardır. RevoScaleR'ın Hadoop Harita Azaltma veya Apache Spark bilgi işlem bağlamlarını kullanarak kümenin düğümleri üzerinde de çalıştırabilirsiniz.

## <a name="ml-services-on-azure-hdinsight"></a>Azure HDInsight'ta ML Hizmetleri

[Azure HDInsight'taki ML Hizmetleri,](r-server-overview.md) R tabanlı analitik için en son özellikleri sağlar. [Azure Blob](../../storage/common/storage-introduction.md "Azure Blob depolama") depolama hesabınızda, Veri Gölü Deposu'nda veya yerel Linux dosya sisteminizde apache Hadoop HDFS kapsayıcısında depolanan verileri kullanabilir. ML Hizmetleri açık kaynak R üzerine kurulduğundan, oluşturduğunuz R tabanlı uygulamalar 8000+ açık kaynak R paketinden herhangi birini uygulayabilir. Ayrıca, Microsoft'un ML Hizmetleri ile birlikte verilen büyük veri analiz paketi [RevoScaleR'deki](https://docs.microsoft.com/machine-learning-server/r-reference/revoscaler/revoscaler)rutinleri de kullanabilirler.  

## <a name="compute-contexts-for-an-edge-node"></a>Kenar düğümü için bağlamları hesaplama

Genel olarak, kenar düğümünde ML Hizmetleri kümesinde çalışan bir R komut dosyası, bu düğümüzerinde R yorumlayıcısı içinde çalışır. Özel durumlar, RevoScaleR işlevini çağıran adımlardır. RevoScaleR çağrıları, RevoScaleR bilgi işlem bağlamını nasıl ayarladığınıza göre belirlenen bir bilgi işlem ortamında çalışır.  R komut dosyanızı kenar düğümünden çalıştırdığınızda, bilgi işlem bağlamının olası değerleri şunlardır:

- yerel sıralı (*yerel*)
- yerel paralel (*localpar*)
- Harita Küçültme
- Spark

*Yerel* ve *yerel par* seçenekleri yalnızca **rxExec** çağrılarının nasıl yürütüldedildiği konusunda farklılık gösterir. Her ikisi de revoScaler **numCoresToUse** seçeneğinin kullanımı yoluyla aksi belirtilmedikçe, mevcut tüm çekirdekler `rxOptions(numCoresToUse=6)`arasında paralel bir şekilde diğer rx-fonksiyon çağrıları yürütmek, örneğin . Paralel yürütme seçenekleri en iyi performansı sunar.

Aşağıdaki tablo, çağrıların nasıl yürütüldedildiğini ayarlamak için çeşitli bilgi işlem bağlamı seçeneklerini özetler:

| İşlem bağlamı  | Nasıl ayarlanır                      | Yürütme bağlamı                        |
| ---------------- | ------------------------------- | ---------------------------------------- |
| Yerel sıralı | rxSetComputeContext('yerel')    | Seri olarak çalıştırılan rxExec çağrıları dışında kenar düğümü sunucusunun çekirdekleri arasında paralelleştirilmiş yürütme |
| Yerel paralellik   | rxSetComputeContext('localpar') | Kenar düğümü sunucusunun çekirdekleri arasında paralelleştirilmiş yürütme |
| Spark            | RxSpark()                       | HDI kümesinin düğümleri arasında Kıvılcım üzerinden paralelleştirilmiş dağıtılmış yürütme |
| Harita Küçültme       | RxHadoopMR()                    | HDI kümesinin düğümleri arasında Harita Küçültme ile paralelleştirilmiş dağıtılmış yürütme |

## <a name="guidelines-for-deciding-on-a-compute-context"></a>Bilgi işlem bağlamında karar verme yönergeleri

Paralelleştirilmiş yürütme sağlayan seçtiğiniz üç seçenekten hangisi, analitik çalışmanızın niteliğine, verilerinizin boyutuna ve konumuna bağlıdır. Hangi bilgi işlem bağlamında kullanılacağını söyleyen basit bir formül yoktur. Ancak, doğru seçimi yapmanıza veya en azından bir kıyaslama çalıştırmadan önce seçimlerinizi daraltmanıza yardımcı olabilecek bazı yol gösterici ilkeler vardır. Bu yol gösterici ilkeler şunlardır:

- Yerel Linux dosya sistemi HDFS'den daha hızlıdır.
- Veriler yerelse ve XDF'deyse tekrarlanan analizler daha hızlıdır.
- Bir metin veri kaynağından az miktarda veri akışı tercih edilir. Veri miktarı daha büyükse, çözümlemeden önce XDF'ye dönüştürün.
- Verileri çözümleme için kenar düğümüne kopyalama veya akış yükü, çok büyük miktarda veri için yönetilemez hale gelir.
- ApacheSpark Hadoop analiz için Map Reduce daha hızlıdır.

Bu ilkeler göz önüne alındığında, aşağıdaki bölümlerde bir bilgi işlem bağlamı seçmek için başparmak bazı genel kurallar sunuyoruz.

### <a name="local"></a>Yerel

- Çözümleneniz gereken veri miktarı küçükse ve tekrarlanan çözümleme gerektirmiyorsa, *yerel* veya *yerel par*kullanarak doğrudan analiz rutinine aktarın.
- Çözümleneniz gereken veri miktarı küçük veya orta ölçekliyse ve tekrarlanan analiz gerektiriyorsa, yerel dosya sistemine kopyalayın, XDF'ye aktarın ve *yerel* veya *yerel par*üzerinden analiz edin.

### <a name="apache-spark"></a>Apache Spark

- Çözümlenenden büyük veri miktarı büyükse, **rxHiveData** veya **RxParquetData**kullanarak bir Spark DataFrame'e veya HDFS'deki XDF'ye (depolama sorun olmadığı sürece) aktarın ve Spark bilgi işlem bağlamını kullanarak analiz edin.

### <a name="apache-hadoop-map-reduce"></a>Apache Hadoop Harita Azalt

- Genellikle daha yavaş olduğundan, Yalnızca Spark bilgi işlem bağlamında aşılmaz bir sorunla karşılaşırsanız, Harita Yıkbilgini azaltma bağlamını kullanın.  

## <a name="inline-help-on-rxsetcomputecontext"></a>rxSetComputeContext'ta satır satır başına yardım
Daha fazla bilgi ve RevoScaleR bilgi işlem bağlamları örnekleri için, rxSetComputeContext yönteminde R satır altı yardım bakın, örneğin:

    > ?rxSetComputeContext

Machine [Learning Server belgelerinde](https://docs.microsoft.com/machine-learning-server/) [Dağıtılmış bilgi işlem özetine](https://docs.microsoft.com/machine-learning-server/r/how-to-revoscaler-distributed-computing) de bakabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, yürütmenin kenar düğümü veya HDInsight kümesinin çekirdekleri arasında paralelleştirilip paralelleştirilmediğini belirtmek için kullanılabilen seçenekleri öğrendiniz. HDInsight kümeleriyle ML Hizmetlerini nasıl kullanacağınız hakkında daha fazla bilgi edinmek için aşağıdaki konulara bakın:

- [Apache Hadoop için ML Hizmetlerine Genel Bakış](r-server-overview.md)
- [HDInsight'ta ML Hizmetleri için Azure Depolama seçenekleri](r-server-storage.md)
