---
title: DistCp kullanarak WASB'a ve WASB'den Azure Veri Gölü Depolama Gen1'e veri kopyalama
description: Azure Depolama kalıplarına gelen verileri Azure Veri Gölü Depolama Gen1'e kopyalamak için DistCp aracını kullanın
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 01/03/2020
ms.author: twooley
ms.openlocfilehash: 455e73ece2d46a508b3077c13c8106fe53beb4de
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75638842"
---
# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-azure-data-lake-storage-gen1"></a>Azure Depolama lekeleri ile Azure Veri Gölü Depolama Gen1 arasındaki verileri kopyalamak için DistCp'yi kullanın

> [!div class="op_single_selector"]
> * [DistCp kullanma](data-lake-store-copy-data-wasb-distcp.md)
> * [AdlCopy kullanma](data-lake-store-copy-data-azure-storage-blob.md)
>
>

Azure Veri Gölü Depolama Gen1'e erişimi olan bir HDInsight kümeniz varsa, verileri bir HDInsight küme depolama deposuna (WASB) kopyalamak için DistCp gibi Hadoop ekosistem araçlarını kullanarak Veri Gölü Depolama Gen1 hesabına aktarabilirsiniz. Bu makalede, DistCp aracının nasıl kullanılacağı gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Bir Azure Veri Gölü Depolama Gen1 hesabı.** Nasıl oluşturulacağına ilişkin talimatlar için [bkz.](data-lake-store-get-started-portal.md)
* Veri Gölü Depolama Gen1 hesabına erişim içeren **Azure HDInsight kümesi.** Bkz. [Veri Gölü Depolama Gen1 ile hdinsight kümesi oluşturun.](data-lake-store-hdinsight-hadoop-use-portal.md) Küme için Uzak Masaüstü'nü etkinleştirdiğinizden emin olun.

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a>HDInsight Linux kümesinden DistCp'yi kullanma

BIR HDInsight kümesi, farklı kaynaklardan gelen verileri bir HDInsight kümesine kopyalamak için kullanılabilen DistCp aracıyla birlikte gelir. HDInsight kümesini Data Lake Storage Gen1'i ek depolama alanı olarak kullanacak şekilde yapılandırıldıysanız, Verileri bir Data Lake Storage Gen1 hesabına ve ondan kopyalamak için Kutudan Kullanıma Sun'u kullanabilirsiniz. Bu bölümde, DistCp aracının nasıl kullanılacağına bakAcağız.

1. Masaüstünüzden kümeye bağlanmak için SSH'yi kullanın. Bkz. [Linux tabanlı HDInsight kümesine bağlanın.](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md) SSH komutları çalıştırın.

1. Azure Depolama bloblarına (WASB) erişip erişemeyeceğiniz doğrulayın. Şu komutu çalıştırın:

   ```
   hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/
   ```

   Çıktı, depolama blob içindekilerin bir listesini sağlar.

1. Benzer şekilde, veri gölü depolama gen1 hesabına kümeden erişip erişemeyeceğinidoğrulayın. Şu komutu çalıştırın:

   ```
   hdfs dfs -ls adl://<data_lake_storage_gen1_account>.azuredatalakestore.net:443/
   ```

    Çıktı, Veri Gölü Depolama Gen1 hesabındaki dosya ve klasörlerin bir listesini sağlar.

1. WASB'deki verileri Veri Gölü Depolama Gen1 hesabına kopyalamak için DistCp'yi kullanın.

   ```
   hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_storage_gen1_account>.azuredatalakestore.net:443/myfolder
   ```

    Komut, WASB'deki **/example/data/gutenberg/** klasörünün içeriğini Veri Gölü Depolama Gen1 hesabındaki **/myfolder'a** kopyalar.

1. Benzer şekilde, verileri Bir Veri Gölü Depolama Gen1 hesabındaki WASB'ye kopyalamak için DistCp'yi kullanın.

   ```
   hadoop distcp adl://<data_lake_storage_gen1_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg
   ```

    Komut, Veri Gölü Depolama Gen1 hesabındaki **/myfolder'ın** içeriğini WASB'daki **/example/data/gutenberg/** klasörüne kopyalar.

## <a name="performance-considerations-while-using-distcp"></a>DistCp kullanırken performans hususları

DistCp aracının en düşük parçalı lık tek bir dosya olduğundan, en fazla eşzamanlı kopya sayısını ayarlamak, onu Veri Gölü Depolama Gen1'e karşı en iyi duruma getirmek için en önemli parametredir. Komut satırındaki mappers ('m') parametresini ayarlayarak eşzamanlı kopya sayısını kontrol edebilirsiniz. Bu parametre, verileri kopyalamak için kullanılan en fazla haritacı sayısını belirtir. Varsayılan değer 20'dir.

Örnek:

```
 hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_storage_gen1_account>.azuredatalakestore.net:443/myfolder -m 100
```

### <a name="how-to-determine-the-number-of-mappers-to-use"></a>Kullanılacak haritaperistanların sayısı nasıl belirlenir?

Aşağıda kullanabileceğiniz bazı yönergeler verilmiştir.

* **Adım 1: Toplam İplik belleği belirleme** - İlk adım, DistCp işini çalıştırdığınız kümeiçin kullanılabilir İplik belleği belirlemektir. Bu bilgiler kümeyle ilişkili Ambari portalında mevcuttur. İplik'e gidin ve İplik belleği görmek için **Configs** sekmesini görüntüleyin. Toplam İplik belleği almak için, düğüm başına İplik belleği kümenizdeki düğüm sayısıyla çarpın.

* **Adım 2: Mappers sayısını hesaplamak** - **m** değeri YARN konteyner boyutuna bölünen toplam İPLik bellek bölümüne eşittir. İplik konteyner boyutu bilgileri Ambari portalında da mevcuttur. İPLik'e gidin ve **Configs** sekmesini görüntüleyin. İplik kapsayıcı boyutu bu pencerede görüntülenir. Mappers sayısına ulaşmak için denklem (**m**) :

   `m = (number of nodes * YARN memory for each node) / YARN container size`

Örnek:

Kümede dört D14v2 düğümü olduğunu ve 10 farklı klasörden 10 TB veri aktarmak istediğinizi varsayalım. Klasörlerin her biri farklı miktarda veri içerir ve her klasördeki dosya boyutları farklıdır.

* Toplam İplik belleği - Ambari portalından, İplik belleği bir D14 düğümü için 96 GB olduğunu belirlersiniz. Yani, dört düğüm kümesi için toplam İplik belleği:

   `YARN memory = 4 * 96GB = 384GB`

* Haritacı sayısı - Ambari portalından, İplik konteyner boyutunun D14 küme düğümü için 3072 olduğunu belirlersiniz. Yani, mappers sayısı:

   `m = (4 nodes * 96GB) / 3072MB = 128 mappers`

Diğer uygulamalar bellek kullanıyorsa, kümenizin İplik belleği yalnızca bir bölümünü DistCp için kullanmayı seçebilirsiniz.

### <a name="copying-large-datasets"></a>Büyük veri kümelerini kopyalama

Taşınacak veri kümesinin boyutu büyükse (örneğin, 1 TB >) veya çok sayıda farklı klasörünüz varsa, birden çok DistCp işi kullanmayı düşünün. Büyük olasılıkla hiçbir performans kazancı yoktur, ancak herhangi bir iş başarısız olursa, tüm iş yerine yalnızca belirli bir işi yeniden başlatmanız gerekir, böylece işleri dağıtır.

### <a name="limitations"></a>Sınırlamalar

* DistCp, performansı optimize etmek için boyutu benzer harita oluşturucular oluşturmaya çalışır. Haritapers sayısını artırmak her zaman performansı artırmak olmayabilir.

* DistCp dosya başına yalnızca bir mapper ile sınırlıdır. Bu nedenle, dosyalarınız olduğundan daha fazla harita oluşturucunuz olmamalıdır. DistCp bir dosyaya yalnızca bir mapper atayabileceğinden, bu büyük dosyaları kopyalamak için kullanılabilecek eşzamanlılık miktarını sınırlar.

* Az sayıda büyük dosyanız varsa, daha fazla potansiyel eşzamanlılık sağlamak için bunları 256 MB'lık dosya yığınlarına bölün.

* Bir Azure Blob depolama hesabından kopyalıyorsanız, kopyalama işiniz Blob depolama tarafında daraltılmış olabilir. Bu, kopyalama işinizin performansını düşürür. Azure Blob depolama alanının sınırları hakkında daha fazla bilgi edinmek için [Azure abonelik ve hizmet sınırlarında](../azure-resource-manager/management/azure-subscription-service-limits.md)Azure Depolama sınırlarına bakın.

## <a name="see-also"></a>Ayrıca bkz.

* [Verileri Azure Depolama lekelerinden Veri Gölü Depolama Gen1'e kopyalama](data-lake-store-copy-data-azure-storage-blob.md)
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Veri Gölü Depolama Gen1 ile Azure Veri Gölü Analizini Kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Veri Gölü Depolama Gen1 ile Azure HDInsight'ı kullanın](data-lake-store-hdinsight-hadoop-use-portal.md)
