---
title: 'Performansı ayarlama: Hive, HDInsight & Azure Data Lake Storage 2. | Microsoft Docs'
description: Hive performansı ayarlama yönergelerini Azure Data Lake Storage 2..
author: normesta
ms.subservice: data-lake-storage-gen2
ms.service: storage
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: normesta
ms.reviewer: stewu
ms.openlocfilehash: 66042568cede364c16302fbd85751de4113bbe0f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "74327578"
---
# <a name="tune-performance-hive-hdinsight--azure-data-lake-storage-gen2"></a>Performansı ayarlama: Hive, HDInsight & Azure Data Lake Storage 2.

Varsayılan ayarlar, birçok farklı kullanım durumunda iyi bir performans sağlamak üzere ayarlanmıştır.  G/ç yoğun sorgularda, Hive Azure Data Lake Storage 2. daha iyi performans sağlamak için ayarlanabilir.  

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Data Lake Storage 2. hesabı**. Bir oluşturma hakkında yönergeler için bkz [. hızlı başlangıç: Azure Data Lake Storage 2. depolama hesabı oluşturma](data-lake-storage-quickstart-create-account.md)
* Data Lake Storage 2. hesabına erişimi olan **Azure HDInsight kümesi** . Bkz. [Azure HDInsight kümeleri ile Azure Data Lake Storage 2. kullanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-use-data-lake-storage-gen2)
* **HDInsight üzerinde Hive çalıştırma**.  HDInsight üzerinde Hive işleri çalıştırma hakkında bilgi edinmek için bkz. [HDInsight 'Ta Hive kullanma](https://docs.microsoft.com/azure/hdinsight/hdinsight-use-hive)
* **Data Lake Storage 2. performans ayarlama yönergeleri**.  Genel performans kavramları için bkz. [Data Lake Storage 2. performans ayarlama Kılavuzu](data-lake-storage-performance-tuning-guidance.md)

## <a name="parameters"></a>Parametreler

Gelişmiş Data Lake Storage 2. performansını ayarlamaya yönelik en önemli ayarlar şunlardır:

* **Hive. tez. Container. size** : her görev tarafından kullanılan bellek miktarı

* **tez. Grouping. min-size** – her bir eşleştiricisindeki minimum boyut

* **tez. Grouping. Max-size** – her bir Eşleştiricideki en büyük boyut

* **Hive. Exec. Reducer. bytes. per. Reducer** – her Reducer için boyut

**Hive. tez. Container. size** -kapsayıcı boyutu, her görev için kullanılabilir bellek miktarını belirler.  Bu, Hive içindeki eşzamanlılık denetimi için ana giriştir.  

**tez. Grouping. min-size** – Bu parametre, her Eşleyici için en küçük boyutu ayarlamanıza olanak sağlar.  Tez 'nin seçtiği mapbir sayı bu parametrenin değerinden küçükse tez burada ayarlanan değeri kullanır.

**tez. Grouping. Max-size** – parametresi, her bir Eşleştiricideki en büyük boyutu ayarlamanıza olanak sağlar.  Tez 'nin seçtiği mapbir sayı bu parametrenin değerinden büyükse tez burada ayarlanan değeri kullanır.

**Hive. Exec. Reducer. bytes. per. Reducer** – Bu parametre her bir Reducer boyutunu ayarlar.  Varsayılan olarak, her Reducer 256 MB 'dir.  

## <a name="guidance"></a>Rehber

**Hive. Exec. Reducer. bytes. per. Reducer** – varsayılan değer sıkıştırılandığınızda iyi sonuç verir.  Sıkıştırılmış veriler için Reducer boyutunu azaltmanız gerekir.  

**Hive. tez. Container. size ayarlayın** – her düğümde, bellek yarn. NodeManager. Resource. Memory-MB tarafından belirtilir ve varsayılan olarak HDI kümesi üzerinde doğru şekilde ayarlanmalıdır.  YARN 'de uygun belleği ayarlama hakkında daha fazla bilgi için bu [gönderisini](https://docs.microsoft.com/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom)inceleyin.

G/ç yoğunluklu iş yükleri, tez kapsayıcı boyutunu azaltarak daha paralellik avantajdan faydalanabilir. Bu, kullanıcıya eşzamanlılık artıran daha fazla kapsayıcı sağlar.  Ancak, bazı Hive sorguları önemli miktarda bellek (ör. Mapjoın) gerektirir.  Görev yeterli belleğe sahip değilse, çalışma zamanı sırasında yetersiz bellek özel durumu alacaksınız.  Bellek dışında özel durumlar alıyorsanız belleği artırmanız gerekir.   

Ya da paralellik çalıştıran, eşzamanlı görev sayısı toplam YARN bellekle sınırlı olacaktır.  YARN kapsayıcılarının sayısı, kaç tane eşzamanlı görevin çalıştırılacağını dikte eder.  Düğüm başına YARN belleği bulmak için, ambarı 'na gidebilirsiniz.  YARN 'ye gidin ve configs sekmesini görüntüleyin.  YARN belleği Bu pencerede görüntülenir.  

        Total YARN memory = nodes * YARN memory per node
        # of YARN containers = Total YARN memory / Tez container size
Data Lake Storage 2. kullanarak performansı iyileştirmeye yönelik anahtar, olabildiğince fazla eşzamanlılık artışı sağlar.  Tez, oluşturulması gereken görev sayısını otomatik olarak hesaplar, bu nedenle ayarlamanız gerekmez.   

## <a name="example-calculation"></a>Örnek hesaplama

8 düğümlü bir D14 kümeniz olduğunu varsayalım.  

    Total YARN memory = nodes * YARN memory per node
    Total YARN memory = 8 nodes * 96GB = 768GB
    # of YARN containers = 768GB / 3072MB = 256

## <a name="further-information-on-hive-tuning"></a>Hive ayarlama hakkında daha fazla bilgi

Hive sorgularınızı ayarlamaya yardımcı olacak birkaç blog aşağıda verilmiştir:
* [HDInsight 'ta Hadoop için Hive sorgularını iyileştirme](https://azure.microsoft.com/documentation/articles/hdinsight-hadoop-optimize-hive-query/)
* [Hive sorgu performansı sorunlarını giderme](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/)
* [HDInsight 'ta en iyileştirme Hive ile Ignite konuşur](https://channel9.msdn.com/events/Machine-Learning-and-Data-Sciences-Conference/Data-Science-Summit-2016/MSDSS25)
