---
title: Performans ayarlama-Hive Azure Data Lake Storage 1.
description: HDInsight ve Azure Data Lake Storage 1. Hive için performans ayarlama yönergeleri.
author: stewu
ms.service: data-lake-store
ms.topic: how-to
ms.date: 12/19/2016
ms.author: stewu
ms.openlocfilehash: 819e158ce2613441efdf2177d50fc8e7989bf68f
ms.sourcegitcommit: 374e47efb65f0ae510ad6c24a82e8abb5b57029e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2020
ms.locfileid: "85510930"
---
# <a name="performance-tuning-guidance-for-hive-on-hdinsight-and-azure-data-lake-storage-gen1"></a>HDInsight ve Azure Data Lake Storage 1. Hive için performans ayarlama Kılavuzu

Varsayılan ayarlar, birçok farklı kullanım durumunda iyi bir performans sağlamak üzere ayarlanmıştır.  G/ç yoğun sorgularda, Hive Azure Data Lake Storage 1. daha iyi performans sağlamak için ayarlanabilir.  

## <a name="prerequisites"></a>Ön koşullar

* **Bir Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Data Lake Storage 1. hesabı**. Bir oluşturma hakkında yönergeler için bkz. Azure Data Lake Storage 1. kullanmaya [başlama](data-lake-store-get-started-portal.md)
* Data Lake Storage 1. hesabına erişimi olan **Azure HDInsight kümesi** . Bkz. [Data Lake Storage 1. HDInsight kümesi oluşturma](data-lake-store-hdinsight-hadoop-use-portal.md). Küme için Uzak Masaüstü 'Nü etkinleştirdiğinizden emin olun.
* **HDInsight üzerinde Hive çalıştırma**.  HDInsight üzerinde Hive işleri çalıştırma hakkında bilgi edinmek için bkz. [HDInsight 'Ta Hive kullanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-use-hive)
* **Data Lake Storage 1. performans ayarlama yönergeleri**.  Genel performans kavramları için bkz. [Data Lake Storage 1. performans ayarlama Kılavuzu](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-performance-tuning-guidance)

## <a name="parameters"></a>Parametreler

Gelişmiş Data Lake Storage 1. performansını ayarlamaya yönelik en önemli ayarlar şunlardır:

* **Hive. tez. Container. size** : her görev tarafından kullanılan bellek miktarı

* **tez. Grouping. min-size** – her bir eşleştiricisindeki minimum boyut

* **tez. Grouping. Max-size** – her bir Eşleştiricideki en büyük boyut

* **hive.exec. Reducer. bytes. per. Reducer** – her Reducer boyutu

**Hive. tez. Container. size** -kapsayıcı boyutu, her görev için kullanılabilir bellek miktarını belirler.  Bu, Hive içindeki eşzamanlılık denetimi için ana giriştir.  

**tez. Grouping. min-size** – Bu parametre, her Eşleyici için en küçük boyutu ayarlamanıza olanak sağlar.  Tez 'nin seçtiği mapbir sayı bu parametrenin değerinden küçükse tez burada ayarlanan değeri kullanır.

**tez. Grouping. Max-size** – parametresi, her bir Eşleştiricideki en büyük boyutu ayarlamanıza olanak sağlar.  Tez 'nin seçtiği mapbir sayı bu parametrenin değerinden büyükse tez burada ayarlanan değeri kullanır.

**hive.exec. Reducer. bytes. per. Reducer** – Bu parametre her bir Reducer boyutunu ayarlar.  Varsayılan olarak, her Reducer 256 MB 'dir.  

## <a name="guidance"></a>Rehber

**hive.exec. Reducer. bytes. per. Reducer** – varsayılan değer sıkıştırılmamış olduğunda iyi sonuç verir.  Sıkıştırılmış veriler için Reducer boyutunu azaltmanız gerekir.  

**Hive. tez. Container. size ayarlayın** – her düğümde, bellek yarn. NodeManager. Resource. Memory-MB tarafından belirtilir ve varsayılan olarak HDI kümesi üzerinde doğru şekilde ayarlanmalıdır.  YARN 'de uygun belleği ayarlama hakkında daha fazla bilgi için bu [gönderisini](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom)inceleyin.

G/ç yoğunluklu iş yükleri, tez kapsayıcı boyutunu azaltarak daha paralellik avantajdan faydalanabilir. Bu, kullanıcıya eşzamanlılık artıran daha fazla kapsayıcı sağlar.  Ancak, bazı Hive sorguları önemli miktarda bellek (ör. Mapjoın) gerektirir.  Görev yeterli belleğe sahip değilse, çalışma zamanı sırasında yetersiz bellek özel durumu alacaksınız.  Bellek dışında özel durumlar alıyorsanız belleği artırmanız gerekir.   

Ya da paralellik çalıştıran, eşzamanlı görev sayısı toplam YARN bellekle sınırlı olacaktır.  YARN kapsayıcılarının sayısı, kaç tane eşzamanlı görevin çalıştırılacağını dikte eder.  Düğüm başına YARN belleği bulmak için, ambarı 'na gidebilirsiniz.  YARN 'ye gidin ve configs sekmesini görüntüleyin.  YARN belleği Bu pencerede görüntülenir.  

        Total YARN memory = nodes * YARN memory per node
        # of YARN containers = Total YARN memory / Tez container size
Data Lake Storage 1. kullanarak performansı iyileştirmeye yönelik anahtar, olabildiğince fazla eşzamanlılık artışı sağlar.  Tez, oluşturulması gereken görev sayısını otomatik olarak hesaplar, bu nedenle ayarlamanız gerekmez.   

## <a name="example-calculation"></a>Örnek hesaplama

8 düğümlü bir D14 kümeniz olduğunu varsayalım.  

    Total YARN memory = nodes * YARN memory per node
    Total YARN memory = 8 nodes * 96GB = 768GB
    # of YARN containers = 768GB / 3072MB = 256

## <a name="limitations"></a>Sınırlamalar

**Data Lake Storage 1. azaltma** 

Data Lake Storage 1. tarafından belirtilen bant genişliği sınırlarına ulaşırsanız, görev başarısızlıklarını görmek için başlangıç yapmanız gerekir. Bu, görev günlüklerinde azaltma hataları gözlemleyerek belirlenebilir.  Tez kapsayıcı boyutunu artırarak paralellik değerini azaltabilirsiniz.  İşiniz için daha fazla eşzamanlılık gerekiyorsa lütfen bizimle iletişime geçin.

Kısıtlanıyor olup olmadığınızı denetlemek için istemci tarafında hata ayıklama günlüğünü etkinleştirmeniz gerekir. Şunları yapabilirsiniz:

1. Aşağıdaki özelliği Hive config içindeki Log4J özelliklerine koyun. Bu işlem, ambarı görünümünden yapılabilir: Log4J. günlükçü. com. Microsoft. Azure. datalake. Store = DEBUG hata ayıkla yapılandırma için tüm düğümleri/hizmeti yeniden başlatın.

2. Daraldıysanız, Hive günlük dosyasında HTTP 429 hata kodunu görürsünüz. Hive günlük dosyası/t MP/ &lt; user &gt; /Hive.log konumunda

## <a name="further-information-on-hive-tuning"></a>Hive ayarlama hakkında daha fazla bilgi

Hive sorgularınızı ayarlamaya yardımcı olacak birkaç blog aşağıda verilmiştir:
* [HDInsight 'ta Hadoop için Hive sorgularını iyileştirme](https://azure.microsoft.com/documentation/articles/hdinsight-hadoop-optimize-hive-query/)
* [Hive sorgu performansı sorunlarını giderme](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/)
* [HDInsight 'ta en iyileştirme Hive ile Ignite konuşur](https://channel9.msdn.com/events/Machine-Learning-and-Data-Sciences-Conference/Data-Science-Summit-2016/MSDSS25)
