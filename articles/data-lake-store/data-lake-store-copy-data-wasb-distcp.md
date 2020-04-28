---
title: DistCp kullanarak verileri ve Azure Data Lake Storage 1.
description: Azure depolama bloblarına/verilerine veri kopyalamak için DistCp aracını kullanın Azure Data Lake Storage 1.
author: twooley
ms.service: data-lake-store
ms.topic: conceptual
ms.date: 01/03/2020
ms.author: twooley
ms.openlocfilehash: 455e73ece2d46a508b3077c13c8106fe53beb4de
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75638842"
---
# <a name="use-distcp-to-copy-data-between-azure-storage-blobs-and-azure-data-lake-storage-gen1"></a>Azure depolama Blobları ve Azure Data Lake Storage 1. arasında veri kopyalamak için DistCp kullanma

> [!div class="op_single_selector"]
> * [DistCp kullanma](data-lake-store-copy-data-wasb-distcp.md)
> * [AdlCopy kullanma](data-lake-store-copy-data-azure-storage-blob.md)
>
>

Azure Data Lake Storage 1. erişimi olan bir HDInsight kümeniz varsa, verileri bir HDInsight küme depolama alanına (ıNB) Data Lake Storage 1. hesabına kopyalamak için DistCp gibi Hadoop ekosistem araçlarını kullanabilirsiniz. Bu makalede, DistCp aracının nasıl kullanılacağı gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

* **Azure aboneliği**. Bkz. [Azure ücretsiz deneme sürümü edinme](https://azure.microsoft.com/pricing/free-trial/).
* **Azure Data Lake Storage 1. hesabı**. Bir oluşturma hakkında yönergeler için bkz. Azure Data Lake Storage 1. kullanmaya [başlama](data-lake-store-get-started-portal.md).
* Data Lake Storage 1. hesabına erişimi olan **Azure HDInsight kümesi** . Bkz. [Data Lake Storage 1. HDInsight kümesi oluşturma](data-lake-store-hdinsight-hadoop-use-portal.md). Küme için Uzak Masaüstü 'Nü etkinleştirdiğinizden emin olun.

## <a name="use-distcp-from-an-hdinsight-linux-cluster"></a>HDInsight Linux kümesinden DistCp kullanma

An HDInsight küme, farklı kaynaklardaki verileri bir HDInsight kümesine kopyalamak için kullanılabilen DistCp aracı ile gelir. HDInsight kümesini ek depolama alanı olarak Data Lake Storage 1. kullanacak şekilde yapılandırdıysanız, verileri bir Data Lake Storage 1. hesabına kopyalamak için DistCp hizmetinden yararlanabilirsiniz. Bu bölümde, DistCp aracının nasıl kullanılacağını inceleyeceğiz.

1. Masaüstünüzden, kümeye bağlanmak için SSH kullanın. Bkz. [Linux tabanlı HDInsight kümesine bağlanma](../hdinsight/hdinsight-hadoop-linux-use-ssh-unix.md). SSH isteminden komutları çalıştırın.

1. Azure depolama bloblarına (ıLB) erişip erişemeyeceğinizi doğrulayın. Şu komutu çalıştırın:

   ```
   hdfs dfs –ls wasb://<container_name>@<storage_account_name>.blob.core.windows.net/
   ```

   Çıktı, Depolama Blobu içindeki içeriklerin bir listesini sağlar.

1. Benzer şekilde, Data Lake Storage 1. hesabına kümeden erişip erişemeyeceğinizi doğrulayın. Şu komutu çalıştırın:

   ```
   hdfs dfs -ls adl://<data_lake_storage_gen1_account>.azuredatalakestore.net:443/
   ```

    Çıktı, Data Lake Storage 1. hesabındaki dosya ve klasörlerin bir listesini sağlar.

1. Verileri bir Data Lake Storage 1. hesabına kopyalamak için DistCp kullanın.

   ```
   hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_storage_gen1_account>.azuredatalakestore.net:443/myfolder
   ```

    Komut, **/example/Data/Gutenberg/** klasörünün içeriğini Data Lake Storage 1. hesabındaki **/myFolder** konumuna kopyalar.

1. Benzer şekilde, bir Data Lake Storage 1. hesabındaki verileri de Deb 'ye kopyalamak için DistCp kullanın.

   ```
   hadoop distcp adl://<data_lake_storage_gen1_account>.azuredatalakestore.net:443/myfolder wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg
   ```

    Komutu, Data Lake Storage 1. hesabındaki **/myFolder** içeriğini, **/example/Data/Gutenberg/** klasörüne kopyalar.

## <a name="performance-considerations-while-using-distcp"></a>DistCp kullanırken performans konuları

DistCp aracının en düşük ayrıntı düzeyi tek bir dosya olduğundan, en fazla eşzamanlı kopya sayısını ayarlamak, Data Lake Storage 1. karşı iyileştirmek için en önemli parametredir. Komut satırında maplıegt ('m ') parametresinin sayısını ayarlayarak eşzamanlı kopya sayısını denetleyebilirsiniz. Bu parametre, verileri kopyalamak için kullanılan en fazla Map, eşleme sayısını belirtir. Varsayılan değer 20 ' dir.

Örnek:

```
 hadoop distcp wasb://<container_name>@<storage_account_name>.blob.core.windows.net/example/data/gutenberg adl://<data_lake_storage_gen1_account>.azuredatalakestore.net:443/myfolder -m 100
```

### <a name="how-to-determine-the-number-of-mappers-to-use"></a>Kullanılacak mapbir eşleme sayısını belirleme

Aşağıda kullanabileceğiniz bazı yönergeler verilmiştir.

* **1. Adım: Toplam Yarn bellek belirleme** -ilk adım, distcp işini çalıştırdığınız kümede bulunan Yarn belleğini belirlemektir. Bu bilgiler, kümeyle ilişkili olan ambarı portalında kullanılabilir. Yarn ' ye gidin ve YARN belleğini görmek için **configs** sekmesini görüntüleyin. Toplam YARN belleğini almak için, düğüm başına YARN belleğini, kümenizde sahip olduğunuz düğümlerin sayısıyla çarpın.

* **2. Adım: mapcontroller sayısını hesaplama** -l DEĞERI, Yarn kapsayıcı boyutuna bölünen toplam Yarn bellek bölümüne eşittir. **m** YARN kapsayıcı boyutu bilgileri ayrıca, ambarı portalında da mevcuttur. YARN 'ye gidin ve **configs** sekmesini görüntüleyin. YARN kapsayıcı boyutu bu pencerede görüntülenir. Mapcontroller (**d**) sayısına ulaşacak denklem:

   `m = (number of nodes * YARN memory for each node) / YARN container size`

Örnek:

Kümede dört D14v2s düğümünüz olduğunu ve 10 farklı klasörden 10 TB veri aktarmak istediğinizi varsayalım. Klasörlerin her biri farklı miktarda veri içerir ve her klasör içindeki dosya boyutları farklıdır.

* Toplam YARN belleği-ambarı portalından, YARN belleğin bir D14 düğümü için 96 GB olduğunu belirlersiniz. Bu nedenle, dört düğümlü küme için toplam YARN bellek:

   `YARN memory = 4 * 96GB = 384GB`

* Mapliçların sayısı-ambarı portalından, D14 küme düğümü için YARN kapsayıcı boyutunun 3072 olduğunu belirlersiniz. Bu nedenle, mapa sayısı şu şekilde olur:

   `m = (4 nodes * 96GB) / 3072MB = 128 mappers`

Diğer uygulamalar bellek kullanıyorsa, DistCp için yalnızca kümenizin YARN bellek belleğinin bir kısmını kullanmayı tercih edebilirsiniz.

### <a name="copying-large-datasets"></a>Büyük veri kümelerini kopyalama

Taşınacak veri kümesinin boyutu büyükse (örneğin, > 1 TB) ya da birçok farklı klasör varsa, birden çok TCP işi kullanmayı göz önünde bulundurun. Büyük olasılıkla performans kazanımı yoktur, ancak iş başarısız olursa işin tamamı yerine yalnızca belirli bir işi yeniden başlatmanız gerekir.

### <a name="limitations"></a>Sınırlamalar

* DistCp, performansı iyileştirmek için boyut olarak benzer bir mapbir oluşturma girişiminde bulunur. Maprların sayısını artırmak performansı her zaman artırabilir.

* DistCp dosya başına yalnızca bir Eşleyici ile sınırlıdır. Bu nedenle, dosyalarınıza sahip olduğunuz için daha fazla mapbir sahip olmanız gerekmez. DistCp bir dosyaya yalnızca bir Eşleyici atayabildiğinden, bu, büyük dosyaları kopyalamak için kullanılabilecek eşzamanlılık miktarını sınırlandırır.

* Az sayıda büyük dosyanız varsa, daha olası eşzamanlılık sağlamak için bunları 256 MB dosya öbeklere ayırın.

* Bir Azure Blob depolama hesabından kopyalama yapıyorsanız, kopyalama işiniz BLOB depolama tarafında kısıtlanıyor olabilir. Bu, kopyalama işinizin performansını düşürür. Azure Blob depolama sınırları hakkında daha fazla bilgi edinmek için bkz. Azure [aboneliği ve hizmet sınırları](../azure-resource-manager/management/azure-subscription-service-limits.md)'Nda Azure depolama sınırları.

## <a name="see-also"></a>Ayrıca bkz.

* [Azure depolama Bloblarından veri kopyalama Data Lake Storage 1.](data-lake-store-copy-data-azure-storage-blob.md)
* [Data Lake Storage Gen1'de verilerin güvenliğini sağlama](data-lake-store-secure-data.md)
* [Data Lake Storage 1. ile Azure Data Lake Analytics kullanma](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight 'ı Data Lake Storage 1. ile kullanma](data-lake-store-hdinsight-hadoop-use-portal.md)
