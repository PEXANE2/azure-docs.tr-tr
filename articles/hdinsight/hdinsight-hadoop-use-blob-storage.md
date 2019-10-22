---
title: HDFS uyumlu Azure Depolama'da veri sorgulama - Azure HDInsight
description: Azure depolama 'dan verileri sorgulamayı ve analizin sonuçlarını depolamak için Azure Data Lake Storage öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/01/2019
ms.openlocfilehash: d934568f09e62ad8c1b472583cbfee79d2c837f6
ms.sourcegitcommit: e0e6663a2d6672a9d916d64d14d63633934d2952
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2019
ms.locfileid: "71936866"
---
# <a name="use-azure-storage-with-azure-hdinsight-clusters"></a>Azure HDInsight kümeleri ile Azure Depolama'yı kullanma

HDInsight kümesindeki verileri çözümlemek için, verileri [Azure depolama](../storage/common/storage-introduction.md)'da, [Azure Data Lake Storage gen 1](../data-lake-store/data-lake-store-overview.md) /[Azure Data Lake Storage Gen 2](../storage/blobs/data-lake-storage-introduction.md)veya bir kombinasyonda saklayabilirsiniz. Bu depolama seçenekleri, hesaplama için kullanılan HDInsight kümelerini Kullanıcı verilerini kaybetmeden güvenle silmenizi sağlar.

Apache Hadoop, varsayılan dosya sisteminin bir kavramını destekler. Varsayılan dosya sistemi varsayılan şema ve yetkilisi anlamına gelir. Bu göreceli yolları çözümlemek için de kullanılabilir. HDInsight kümesi oluşturma işlemi sırasında Azure depolama 'da bir blob kapsayıcısını varsayılan dosya sistemi olarak belirtebilir veya HDInsight 3,6 ' de, varsayılan dosyalar olarak Azure Storage veya Azure Data Lake Storage Gen 1/Azure Data Lake Storage Gen 2 ' yi seçebilirsiniz birkaç özel durum içeren sistem. Hem varsayılan hem de bağlı depolama alanı olarak Data Lake Storage Gen 1 kullanmanın desteklenebilirliği için bkz. [HDInsight kümesi Için kullanılabilirlik](./hdinsight-hadoop-use-data-lake-store.md#availability-for-hdinsight-clusters).

Bu makalede Azure Depolama'nın HDInsight kümeleri ile nasıl çalıştığı hakkında bilgi edinebilirsiniz. Data Lake Storage Gen 1 ' in HDInsight kümeleri ile nasıl çalıştığını öğrenmek için bkz. [Azure HDInsight kümeleri ile Azure Data Lake Storage kullanma](hdinsight-hadoop-use-data-lake-store.md). HDInsight kümesi oluşturma hakkında daha fazla bilgi için bkz. [HDInsight 'ta Apache Hadoop kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md).

Azure depolama, HDInsight ile sorunsuz bir şekilde tümleşen, sağlam ve genel amaçlı bir depolama çözümüdür. HDInsight, Azure Depolama’daki bir blob kapsayıcıyı kümenin varsayılan dosya sistemi olarak kullanabilir. Hadoop dağıtılmış dosya sistemi (HDFS) arabirimi aracılığıyla, HDInsight’taki bileşenler kümesinin tümü blob olarak depolanan yapılandırılmış veya yapılandırılmamış veriler üzerinde doğrudan çalışabilir.

> [!IMPORTANT]  
> Depolama hesabı türü **blobstorage** yalnızca HDInsight kümeleri için ikincil depolama alanı olarak kullanılabilir.

| Depolama hesabı türü | Desteklenen hizmetler | Desteklenen performans katmanları | Desteklenen erişim katmanları |
|----------------------|--------------------|-----------------------------|------------------------|
| StorageV2 (genel amaçlı v2)  | Blob     | Standart                    | Sık erişimli, seyrek erişimli ve arşiv \*   |
| Depolama (genel amaçlı v1)   | Blob     | Standart                    | Yok                    |
| BlobStorage                    | Blob     | Standart                    | Sık erişimli, seyrek erişimli ve arşiv \*   |

İş verilerini depolamak için varsayılan blob kapsayıcısını kullanmanızı önermiyoruz. Depolama maliyetini azaltmak için blob kapsayıcısının her kullanımdan sonra silinmesi iyi bir uygulamadır. Varsayılan kapsayıcı, uygulama ve sistem günlükleri içerir. Kapsayıcıyı silmeden önce günlükleri aldığınızdan emin olun.

Birden çok küme için varsayılan dosya sistemi olarak bir blob kapsayıcısının paylaşılması desteklenmez.

> [!NOTE]  
> Arşiv erişim katmanı, birkaç saat alma gecikmesi olan ve HDInsight ile kullanım için önerilmeyen bir çevrimdışı katmandır. Daha fazla bilgi için bkz. [Arşiv erişim katmanı](../storage/blobs/storage-blob-storage-tiers.md#archive-access-tier).

Depolama hesabınızı **Seçili ağlardaki** **güvenlik duvarları ve sanal ağlar** kısıtlamalarına göre güvenli hale getirmek isterseniz, HDInsight 'ın depolama verilerinize erişebilmesi Için **güvenilir Microsoft hizmetlerine izin ver...** özel durumunu etkinleştirdiğinizden emin olun. hesabı.

## <a name="hdinsight-storage-architecture"></a>HDInsight depolama mimarisi

Aşağıdaki diyagram, Azure Depolama ile kullanılan HDInsight depolama mimarisine ilişkin bir özet görünüm sağlar:

![Hadoop kümeleri, blob depolamaya erişmek ve veri depolamak için, bu API kullanır](./media/hdinsight-hadoop-use-blob-storage/storage-architecture.png "HDInsight depolama mimarisi")

HDInsight, işlem düğümlerine yerel olarak bağlı olan dağıtılmış dosya sistemine erişim imkanı sağlar. Bu dosya sistemine tam uygun URI kullanılarak erişilebilir, örneğin:

    hdfs://<namenodehost>/<path>

Ayrıca HDInsight, Azure Depolama'da depolanan verilere erişebilmenizi de sağlar. Söz dizimi aşağıdaki gibidir:

    wasbs://<containername>@<accountname>.blob.core.windows.net/<path>

HDInsight kümeleriyle Azure Depolama hesabını kullanırken dikkat etmeniz gereken bazı noktalar mevcuttur.

* **Bir kümeye bağlı depolama hesaplarındaki kapsayıcılar:** Oluşturma sırasında hesap adı ve anahtar kümeyle ilişkilendirildiğinden, bu kapsayıcılardaki blob'lara tam erişiminiz vardır.

* **Bir kümeye bağlı OLMAYAN depolama hesaplarındaki genel kapsayıcılar veya genel blob’lar:** Kapsayıcılardaki blob’lara salt okunur iznine sahipsiniz.
  
> [!NOTE]  
> Genel kapsayıcılar, bu kapsayıcıda bulunan tüm blob’ların bir listesini ve kapsayıcı meta verilerini almanıza olanak tanır. Genel blob'lar, yalnızca tam URL'yi biliyorsanız blob erişiminize izin verir. Daha fazla bilgi için bkz. [kapsayıcılara ve bloblara erişimi yönetme](../storage/blobs/storage-manage-access-to-resources.md).

* **Bir kümeye bağlı olmayan depolama hesaplarındaki özel kapsayıcılar:** Web Hcat işleri gönderdiğinizde depolama hesabını tanımlamadığınız müddetçe kapsayıcılardaki bloblara erişemezsiniz. Bu, bu makalenin sonraki bölümlerinde açıklanmıştır.

Oluşturma işleminde tanımlanan depolama hesapları ve anahtarları küme düğümlerinde `%HADOOP_HOME%/conf/core-site.xml` depolanır. HDInsight’ın varsayılan davranışı core-site.xml dosyasında tanımlanan depolama hesaplarını kullanmaktır. Bu ayarı, [Apache ambarı](./hdinsight-hadoop-manage-ambari.md)'nı kullanarak değiştirebilirsiniz.

Apache Hive, MapReduce, Apache Hadoop streaming ve Apache Pig dahil olmak üzere birden çok WebHCat işi, depolama hesaplarının ve meta verilerin bir açıklamasını taşıyabilir. (Bu, şu anda depolama hesapları olan Pig için çalışıyor, ancak meta veriler için değil.) Daha fazla bilgi için bkz. [alternatif depolama hesaplarıyla HDInsight kümesi ve Metastores kullanma](https://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

Bloblar yapılandırılmış ve yapılandırılmamış veriler için kullanılabilir. Blob kapsayıcıları, verileri anahtar/değer çiftleri olarak depolar ve dizin hiyerarşisi yoktur. Ancak, bir dosyayı dizin yapısında depolanmış gibi göstermek için anahtar adında eğik çizgi karakteri (/) kullanılabilir. Örneğin, bir blob'un anahtarı *input/log1.txt* şeklinde olabilir. Gerçek *giriş* dizini yoktur, ancak anahtar adında eğik çizgi karakteri bulunması nedeniyle, bir dosya yolu görünümüne sahiptir.

## <a id="benefits"></a>Azure Depolamanın yararları

İşlem kümelerinin ve depolama kaynaklarının birlikte bulunmamasının zımni performans maliyeti, işlem kümelerinin Azure bölgesindeki depolama hesabı kaynaklarına yakın şekilde oluşturulduğu şekilde, yüksek hızlı ağın ise Azure Storage içindeki verilere erişmek için işlem düğümleri.

Verileri HDFS yerine Azure Depolama’da depolamanın çeşitli avantajları vardır:

* **Verileri yeniden kullanma ve paylaşma:** HDFS’deki veriler işlem kümesi içinde bulunur. Yalnızca işlem kümesi erişimi olan uygulamalar HDFS API'lerini kullanarak verileri kullanabilir. Azure depolama 'daki verilere, bir, bir,,, bir, bir,, bir [,, bir](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API),, bir,, bir Bu nedenle, daha büyük uygulama kümeleri (diğer HDInsight kümeleri dahil olmak üzere) ve araçları veri üretmek ve kullanmak için kullanılabilir.

* **Veri arşivleme:** Verileri Azure depolamada depolamak, hesaplama için kullanılan HDInsight kümelerinin kullanıcı verilerini kaybetmeden güvenle silinmesini sağlar.

* **Veri depolama maliyeti:** Verileri DFS uzun süreli depolamak verileri Azure depolamada depolamaktan daha maliyetlidir; çünkü işlem kümesinin maliyeti Azure depolama maliyetinden daha fazladır. Ayrıca, verilerin her işlem kümesi üretimi için yeniden yüklenmesi gerektiğinden, veri yükleme maliyetlerini de kaydediyorsunuz.

* **Esnek ölçeklendirme:** HDFS size ölçeklendirilmiş dosya sistemi sağlamakla birlikte, ölçek, kümeniz için oluşturduğunuz düğüm sayısı tarafından belirlenir. Ölçeği değiştirmek, Azure depolamada otomatik olarak aldığınız esnek ölçeklendirme özelliklere bağlı kalmaktan daha karmaşık bir işlem haline gelebilir.

* **Coğrafi çoğaltma:** Azure depolamanız coğrafi olarak çoğaltılabilir. Bu, size coğrafi kurtarma ve veri yedekliği sağlamakla birlikte, coğrafi olarak çoğaltılan bir konuma yük devretmek performansınızı ciddi bir şekilde etkiler ve ek ücrete neden olabilir. Bu nedenle, coğrafi çoğaltmayı akıllıca ve yalnızca verilerin değeri ek maliyetlere değer durumdaysa tercih etmenizi öneririz.

Bazı MapReduce işleri ve paketleri gerçekte Azure depolamada depolamak istemediğiniz ara sonuçlar oluşturabilir. Bu durumda, verileri yerel HDFS’de depolamak üzere seçebilirsiniz. Aslında, HDInsight Hive işleri ve diğer işlemlerdeki bu ara sonuçların bazıları için DFS kullanır.

> [!NOTE]  
> Çoğu bir `ls` (örneğin, `copyFromLocal` ve `mkdir`) hala beklendiği gibi çalışır. Yalnızca yerel bir bu uygulamaya özgü komutlar (DFS olarak adlandırılır), `fschk` ve `dfsadmin` gibi Azure depolamada farklı davranışlar gösterir.

## <a name="address-files-in-azure-storage"></a>Azure depolamada dosyaları adresleme

HDInsight’ta Azure depolamadaki dosyalara erişmek için URI şeması aşağıdaki gibidir:

```config
wasbs://<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>
```

URI şeması şifrelenmemiş erişim (ile *wasb:* öneki ile) ve SSL şifreli erişim (*wasbs* ile) şifrelenmemiş erişim sağlar. Azure’da aynı bölgede bulunan verilere erişirken dahi mümkün olduğunda *wasbs* kullanmanızı öneririz.

@No__t_0, Azure depolama 'daki blob kapsayıcısının adını tanımlar.
@No__t_0, Azure depolama hesabı adını tanımlar. Tam uygun etki alanı adı (FQDN) gereklidir.

Ne `<BlobStorageContainerName>` ne de `<StorageAccountName>` belirtilmemişse, varsayılan dosya sistemi kullanılır. Varsayılan dosya sistemindeki dosyalar için göreli bir yol veya mutlak bir yol kullanabilirsiniz. Örneğin, HDInsight kümeleriyle gelen *hadoop mapreduce examples.jar* dosyasına aşağıdakilerden birini kullanarak başvurulabilir:

```config
wasbs://mycontainer@myaccount.blob.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
wasbs:///example/jars/hadoop-mapreduce-examples.jar
/example/jars/hadoop-mapreduce-examples.jar
```

> [!NOTE]  
> Dosya adı, HDInsight sürüm 2,1 ve 1,6 kümelerinde `hadoop-examples.jar`.

Yol, dosya veya dizin \ yol adı ' dır. Azure depolama 'daki kapsayıcılar anahtar-değer depoları olduğundan, doğru hiyerarşik dosya sistemi yoktur. Bir blob anahtarındaki bir eğik çizgi karakteri (/) dizin ayırıcı olarak yorumlanır. Örneğin, *hadoop mapreduce examples.jar* için blob adı aşağıdaki gibidir:

```bash
example/jars/hadoop-mapreduce-examples.jar
```

> [!NOTE]  
> HDInsight dışındaki blob'larla çalışırken, yardımcı programların çoğu WASB biçimini tanımaz ve bunun yerine, `example/jars/hadoop-mapreduce-examples.jar` gibi temel yol biçimi gibi bekler.

## <a name="blob-containers"></a>Blob kapsayıcıları

Blob 'ları kullanmak için önce bir [Azure depolama hesabı](../storage/common/storage-create-storage-account.md)oluşturursunuz. Bunun bir parçası olarak depolama hesabının oluşturulduğu Azure bölgesini belirtirsiniz. Küme ve depolama hesabının aynı bölgede barındırılması gerekir. Hive meta veri deposu SQL Server veritabanı ve Apache Oozie metaser SQL Server veritabanı aynı bölgede de bulunmalıdır.

Nerede olursa olsun, oluşturduğunuz her blob Azure Storage hesabınızdaki bir kapsayıcıya aittir. Bu kapsayıcı HDInsight dışında oluşturulmuş bir blob veya bir HDInsight kümesi için oluşturulan bir kapsayıcı olabilir.

Varsayılan Blob kapsayıcısı iş geçmişi ve iş günlükleri gibi kümeye özel bilgileri depolar. Varsayılan Blob kapsayıcısını birden çok HDInsight kümesiyle paylaşmayın. Bu durum iş geçmişinin bozulmasına neden olabilir. Her küme için farklı bir kapsayıcı kullanmanız ve paylaşılan verileri varsayılan depolama hesabı yerine tüm ilgili kümelerin dağıtımında belirtilen bağlı bir depolama hesabına yerleştirmeniz önerilir. Bağlı depolama hesaplarını yapılandırma hakkında daha fazla bilgi için bkz. [HDInsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md). Ancak özgün HDInsight kümesi silindikten sonra varsayılan depolama kapsayıcısını yeniden kullanabilirsiniz. HBase kümeleri için, silinmiş bir HBase kümesi tarafından kullanılan varsayılan blob kapsayıcısını kullanarak yeni bir HBase kümesi oluşturarak HBase tablo şemasını ve verilerini tutabilirsiniz.

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

## <a name="interacting-with-azure-storage"></a>Azure depolama ile etkileşim kurma

Microsoft, Azure depolama ile çalışmak için aşağıdaki araçları sağlar:

| Araç | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Azure portalda](../storage/blobs/storage-quickstart-blobs-portal.md) |✔ |✔ |✔ |
| [Azure CLI](../storage/blobs/storage-quickstart-blobs-cli.md) |✔ |✔ |✔ |
| [Azure PowerShell](../storage/blobs/storage-quickstart-blobs-powershell.md) | | |✔ |
| [AzCopy](../storage/common/storage-use-azcopy-v10.md) |✔ | |✔ |

## <a name="use-additional-storage-accounts"></a>Ek depolama hesaplarını kullanma

HDInsight kümesi oluştururken ilişkilendirmek istediğiniz Azure Depolama hesabını belirtirsiniz. Bu depolama hesabına ek olarak, oluşturma işlemi sırasında veya bir küme oluşturulduktan sonra aynı Azure aboneliğinden veya farklı Azure aboneliklerinden başka depolama hesapları ekleyebilirsiniz. Ek depolama hesapları ekleme hakkında yönergeler için bkz. [HDInsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md).

> [!WARNING]  
> HDInsight kümesinden farklı bir konumda ek depolama hesabının kullanılması desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede HDFS ile uyumlu Azure Depolama'yı HDInsight ile nasıl kullanacağınızı öğrendiniz. Bu, ölçeklenebilir, uzun vadeli, arşivlemeli veri edinme çözümleri oluşturmanıza ve depolanan yapılandırılmış ve yapılandırılmamış verilerdeki bilgilerin kilidini açmak için HDInsight kullanmanıza olanak sağlar.

Daha fazla bilgi için bkz.

* [Azure HDInsight 'ı kullanmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Azure Data Lake Storage kullanmaya başlayın](../data-lake-store/data-lake-store-get-started-portal.md)
* [HDInsight 'a veri yükleme](hdinsight-upload-data.md)
* [HDInsight ile Apache Hive kullanma](hadoop/hdinsight-use-hive.md)
* [HDInsight ile Apache Pig kullanma](hadoop/hdinsight-use-pig.md)
* [HDInsight ile verilere erişimi kısıtlamak için Azure depolama paylaşılan erişim Imzalarını kullanma](hdinsight-storage-sharedaccesssignature-permissions.md)
* [Azure HDInsight kümeleriyle Azure Data Lake Storage 2. Nesil hizmetini kullanma](hdinsight-hadoop-use-data-lake-storage-gen2.md)
