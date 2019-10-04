---
title: Azure depolama 'dan verileri sorgulama-Azure HDInsight
description: Azure depolama 'dan verileri sorgulamayı ve analizin sonuçlarını depolamak için Azure Data Lake Storage öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 10/01/2019
ms.openlocfilehash: d934568f09e62ad8c1b472583cbfee79d2c837f6
ms.sourcegitcommit: f2d9d5133ec616857fb5adfb223df01ff0c96d0a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71936866"
---
# <a name="use-azure-storage-with-azure-hdinsight-clusters"></a>Azure HDInsight kümeleri ile Azure depolama kullanma

HDInsight kümesindeki verileri çözümlemek için verileri [Azure depolama](../storage/common/storage-introduction.md)'da, [Azure Data Lake Storage Gen 1](../data-lake-store/data-lake-store-overview.md)/[Azure Data Lake Storage Gen 2](../storage/blobs/data-lake-storage-introduction.md)' ye veya bir birleşimine kaydedebilirsiniz. Bu depolama seçenekleri, hesaplama için kullanılan HDInsight kümelerini Kullanıcı verilerini kaybetmeden güvenle silmenizi sağlar.

Apache Hadoop, varsayılan dosya sisteminin bir kavramını destekler. Varsayılan dosya sistemi varsayılan bir düzeni ve yetkiyi gösterir. Ayrıca, göreli yolları çözümlemek için de kullanılabilir. HDInsight kümesi oluşturma işlemi sırasında Azure depolama 'da bir blob kapsayıcısını varsayılan dosya sistemi olarak belirtebilir veya HDInsight 3,6 ' de, varsayılan dosyalar olarak Azure Storage veya Azure Data Lake Storage Gen 1/Azure Data Lake Storage Gen 2 ' yi seçebilirsiniz birkaç özel durum içeren sistem. Hem varsayılan hem de bağlı depolama alanı olarak Data Lake Storage Gen 1 kullanmanın desteklenebilirliği için bkz. [HDInsight kümesi Için kullanılabilirlik](./hdinsight-hadoop-use-data-lake-store.md#availability-for-hdinsight-clusters).

Bu makalede, Azure Storage 'ın HDInsight kümeleri ile nasıl çalıştığı hakkında bilgi edineceksiniz. Data Lake Storage Gen 1 ' in HDInsight kümeleri ile nasıl çalıştığını öğrenmek için bkz. [Azure HDInsight kümeleri ile Azure Data Lake Storage kullanma](hdinsight-hadoop-use-data-lake-store.md). HDInsight kümesi oluşturma hakkında daha fazla bilgi için bkz. [HDInsight 'ta Apache Hadoop kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md).

Azure depolama, HDInsight ile sorunsuz bir şekilde tümleşen sağlam, genel amaçlı bir depolama çözümüdür. HDInsight, Azure depolama 'da küme için varsayılan dosya sistemi olarak bir blob kapsayıcısı kullanabilir. Hadoop Dağıtılmış dosya sistemi (ISE) arabirimi aracılığıyla, HDInsight 'taki tüm bileşen kümesi, blob olarak depolanan yapılandırılmış veya yapılandırılmamış veriler üzerinde doğrudan çalışabilir.

> [!IMPORTANT]  
> Depolama hesabı türü **blobstorage** yalnızca HDInsight kümeleri için ikincil depolama alanı olarak kullanılabilir.

| Depolama hesabı türü | Desteklenen hizmetler | Desteklenen performans katmanları | Desteklenen erişim katmanları |
|----------------------|--------------------|-----------------------------|------------------------|
| StorageV2 (genel amaçlı v2)  | Bun     | Stand                    | Sık erişimli, seyrek erişimli arşiv @ no__t-0   |
| Depolama (genel amaçlı v1)   | Bun     | Stand                    | Yok                    |
| BlobStorage                    | Bun     | Stand                    | Sık erişimli, seyrek erişimli arşiv @ no__t-0   |

İş verilerini depolamak için varsayılan blob kapsayıcısını kullanmanızı önermiyoruz. Depolama maliyetini azaltmak için her bir kullanım sonrasında varsayılan blob kapsayıcısını silmek iyi bir uygulamadır. Varsayılan kapsayıcı, uygulama ve sistem günlükleri içerir. Kapsayıcıyı silmeden önce günlükleri aldığınızdan emin olun.

Birden çok küme için varsayılan dosya sistemi olarak bir blob kapsayıcısının paylaşılması desteklenmez.

> [!NOTE]  
> Arşiv erişim katmanı, birkaç saat alma gecikmesi olan ve HDInsight ile kullanım için önerilmeyen bir çevrimdışı katmandır. Daha fazla bilgi için bkz. [Arşiv erişim katmanı](../storage/blobs/storage-blob-storage-tiers.md#archive-access-tier).

Depolama hesabınızı **Seçili ağlardaki** **güvenlik duvarları ve sanal ağlar** kısıtlamalarına göre güvenli hale getirmek isterseniz, HDInsight 'ın depolama verilerinize erişebilmesi Için **güvenilir Microsoft hizmetlerine izin ver...** özel durumunu etkinleştirdiğinizden emin olun. hesabı.

## <a name="hdinsight-storage-architecture"></a>HDInsight depolama mimarisi

Aşağıdaki diyagramda Azure Storage kullanarak HDInsight depolama mimarisinin soyut bir görünümü sunulmaktadır:

![Hadoop kümeleri, blob Storage HDInsight depolama mimarisi 'ne erişmek ve veri depolamak için, bu API kullanır](./media/hdinsight-hadoop-use-blob-storage/storage-architecture.png "")

HDInsight, işlem düğümlerine yerel olarak bağlı olan dağıtılmış dosya sistemine erişim sağlar. Bu dosya sistemine tam URI kullanılarak erişilebilir, örneğin:

    hdfs://<namenodehost>/<path>

Ayrıca HDInsight, Azure depolama 'da depolanan verilere erişmenizi sağlar. Sözdizimi şöyledir:

    wasbs://<containername>@<accountname>.blob.core.windows.net/<path>

HDInsight kümeleri ile Azure depolama hesabı kullanırken bazı konular aşağıda verilmiştir.

* **Bir kümeye bağlı depolama hesaplarındaki kapsayıcılar:** Hesap adı ve anahtarı oluşturma sırasında kümeyle ilişkili olduğundan, bu kapsayıcılardaki bloblara tam erişiminiz vardır.

* **Bir kümeye bağlı olmayan depolama hesaplarındaki genel kapsayıcılar veya genel Bloblar:** Kapsayıcılarda bloblara salt okuma izninizin olması gerekir.
  
> [!NOTE]  
> Ortak kapsayıcılar, bu kapsayıcıda kullanılabilen tüm Blobların bir listesini almanızı ve kapsayıcı meta verilerini almanızı sağlar. Genel Bloblar yalnızca tam URL 'YI biliyorsanız bloblara erişmenizi sağlar. Daha fazla bilgi için bkz. [kapsayıcılara ve bloblara erişimi yönetme](../storage/blobs/storage-manage-access-to-resources.md).

* **Bir kümeye bağlı olmayan depolama hesaplarındaki özel kapsayıcılar:** Web Hcat işleri gönderdiğinizde depolama hesabını tanımlamadığınız müddetçe kapsayıcılardaki bloblara erişemezsiniz. Bu makalenin ilerleyen kısımlarında açıklanmaktadır.

Oluşturma işleminde tanımlanan depolama hesapları ve anahtarları küme düğümlerinde `%HADOOP_HOME%/conf/core-site.xml` ' da depolanır. HDInsight 'ın varsayılan davranışı, Core-site. xml dosyasında tanımlanan depolama hesaplarını kullanmaktır. Bu ayarı, [Apache ambarı](./hdinsight-hadoop-manage-ambari.md)'nı kullanarak değiştirebilirsiniz.

Apache Hive, MapReduce, Apache Hadoop streaming ve Apache Pig dahil olmak üzere birden çok WebHCat işi, depolama hesaplarının ve meta verilerin bir açıklamasını taşıyabilir. (Bu, şu anda depolama hesapları olan Pig için çalışıyor, ancak meta veriler için değil.) Daha fazla bilgi için bkz. [alternatif depolama hesaplarıyla HDInsight kümesi ve Metastores kullanma](https://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx).

Blob 'lar, yapılandırılmış ve yapılandırılmamış veriler için kullanılabilir. Blob kapsayıcıları, verileri anahtar/değer çiftleri olarak depolar ve dizin hiyerarşisi yoktur. Ancak, bir dosyanın dizin yapısında depolanmış gibi görünmesi için anahtar adında eğik çizgi karakteri (/) kullanılabilir. Örneğin, bir Blobun anahtarı *Input/log1. txt*olabilir. Gerçek *giriş* dizini yok, ancak anahtar adında eğik çizgi karakterinin varlığı nedeniyle dosya yolunun görünümü var.

## <a id="benefits"></a>Azure depolama 'nın avantajları

İşlem kümelerinin ve depolama kaynaklarının birlikte bulunmamasının zımni performans maliyeti, işlem kümelerinin Azure bölgesindeki depolama hesabı kaynaklarına yakın şekilde oluşturulduğu şekilde, yüksek hızlı ağın ise Azure Storage içindeki verilere erişmek için işlem düğümleri.

Verileri, Azure depolama 'da şu şekilde depolamaya ilişkin birkaç avantaj vardır:

* **Verilerin yeniden kullanımı ve paylaşılması:** Bilgisayar \ ' daki veriler, işlem kümesi içinde bulunur. Yalnızca bilgi işlem kümesine erişimi olan uygulamalar, bu verileri, bu verileri,,,, bu verileri de, bu verileri Azure depolama 'daki verilere, bir, bir,,, bir, bir,, bir [,, bir](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API),, bir,, bir Bu nedenle, daha büyük bir uygulamalar kümesi (diğer HDInsight kümeleri dahil) ve araçları verileri oluşturmak ve kullanmak için kullanılabilir.

* **Veri arşivleme:** Verileri Azure Storage 'da depolamak, hesaplama için kullanılan HDInsight kümelerinin Kullanıcı verilerini kaybetmeden güvenle silinmesini sağlar.

* **Veri depolama maliyeti:** Verilerin DFS 'de uzun süreli depolanması, verileri Azure Storage 'da depolamaktan daha maliyetlidir çünkü bir işlem kümesinin maliyeti Azure Storage maliyetinden daha yüksektir. Ayrıca, verilerin her işlem kümesi üretimi için yeniden yüklenmesi gerektiğinden, veri yükleme maliyetlerini de kaydediyorsunuz.

* **Elastik genişleme:** Her ne kadar ölçekli bir dosya sistemi sağlar, ancak ölçek kümeniz için oluşturduğunuz düğüm sayısına göre belirlenir. Ölçeği değiştirmek, Azure depolama 'da otomatik olarak aldığınız esnek ölçeklendirme özelliklerine bağlı olandan daha karmaşık bir süreç olabilir.

* **Coğrafi çoğaltma:** Azure depolama, coğrafi olarak çoğaltılabilir. Bu, coğrafi kurtarma ve veri yedekliliği sağlasa da, coğrafi olarak çoğaltılan konuma yönelik bir yük devretme performansı önemli ölçüde etkiler ve ek ücret ödemeniz gerekebilir. Bu nedenle, Coğrafi çoğaltmayı tam olarak ve yalnızca verilerin değeri ek maliyete değolursa tercih edilir.

Belirli MapReduce işleri ve paketleri, gerçekten Azure Storage 'da depolamak istemediğiniz ara sonuçlar oluşturabilir. Bu durumda, verileri yerel olarak depolamayı seçebilirsiniz. Aslında, HDInsight Hive işlerinde ve diğer işlemlerde bu ara sonuçlardan bazıları için DFS 'yi kullanır.

> [!NOTE]  
> En çok sayıda komut (örneğin, `ls`, `copyFromLocal` ve `mkdir`) yine de beklendiği gibi çalışır. Yalnızca yerel bir IBU uygulamaya özgü olan (DFS olarak adlandırılır), `fschk` ve `dfsadmin` gibi komutlar Azure depolamada farklı davranışlar gösterir.

## <a name="address-files-in-azure-storage"></a>Azure Storage 'daki adres dosyaları

HDInsight 'tan Azure Storage 'daki dosyalara erişmek için URI şeması:

```config
wasbs://<BlobStorageContainerName>@<StorageAccountName>.blob.core.windows.net/<path>
```

URI şeması, şifrelenmemiş erişim (yani, SGB *:* öneki ile) ve SSL şifreli erişimi ( *GB*'ler ile) sağlar. Azure 'da aynı bölgenin içinde yer alan verilere erişirken bile, mümkün *olan yerlerde fazla* kullanım yapmanızı öneririz.

@No__t-0, Azure depolama 'daki blob kapsayıcısının adını tanımlar.
@No__t-0, Azure depolama hesabı adını tanımlar. Tam nitelikli etki alanı adı (FQDN) gereklidir.

@No__t-0 veya `<StorageAccountName>` belirtilmediyse, varsayılan dosya sistemi kullanılır. Varsayılan dosya sistemindeki dosyalar için göreli bir yol veya mutlak bir yol kullanabilirsiniz. Örneğin, HDInsight kümeleriyle birlikte gelen *Hadoop MapReduce Examples. jar* dosyası, aşağıdakilerden biri kullanılarak başvurulabilirler:

```config
wasbs://mycontainer@myaccount.blob.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
wasbs:///example/jars/hadoop-mapreduce-examples.jar
/example/jars/hadoop-mapreduce-examples.jar
```

> [!NOTE]  
> HDInsight sürümleri 2,1 ve 1,6 kümelerinde dosya adı `hadoop-examples.jar` ' dır.

Yol, dosya veya dizin \ yol adı ' dır. Azure depolama 'daki kapsayıcılar anahtar-değer depoları olduğundan, doğru hiyerarşik dosya sistemi yoktur. Blob anahtarının içindeki eğik çizgi karakteri (/) dizin ayırıcı olarak yorumlanır. Örneğin, *Hadoop MapReduce Examples. jar* için blob adı:

```bash
example/jars/hadoop-mapreduce-examples.jar
```

> [!NOTE]  
> HDInsight dışında bloblarla çalışırken, çoğu yardımcı program, UBB biçimini tanımaz ve bunun yerine `example/jars/hadoop-mapreduce-examples.jar` gibi temel bir yol biçimi bekler.

## <a name="blob-containers"></a>Blob kapsayıcıları

Blob 'ları kullanmak için önce bir [Azure depolama hesabı](../storage/common/storage-create-storage-account.md)oluşturursunuz. Bunun bir parçası olarak, depolama hesabının oluşturulduğu bir Azure bölgesi belirtirsiniz. Kümenin ve depolama hesabının aynı bölgede barındırılması gerekir. Hive meta veri deposu SQL Server veritabanı ve Apache Oozie metaser SQL Server veritabanı aynı bölgede de bulunmalıdır.

Nerede olursa olsun oluşturduğunuz her blob, Azure Depolama hesabınızdaki bir kapsayıcıya aittir. Bu kapsayıcı, HDInsight dışında oluşturulmuş mevcut bir BLOB olabilir veya bir HDInsight kümesi için oluşturulan bir kapsayıcı olabilir.

Varsayılan blob kapsayıcısı, iş geçmişi ve Günlükler gibi kümeyle özel bilgileri depolar. Birden çok HDInsight kümesi ile varsayılan bir blob kapsayıcısı paylaşmayın. Bu işlem, iş geçmişini bozabilir. Her küme için farklı bir kapsayıcı kullanmanız ve paylaşılan verileri varsayılan depolama hesabı yerine tüm ilgili kümelerin dağıtımında belirtilen bağlı bir depolama hesabına yerleştirmeniz önerilir. Bağlı depolama hesaplarını yapılandırma hakkında daha fazla bilgi için bkz. [HDInsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md). Ancak, orijinal HDInsight kümesi silindikten sonra varsayılan bir depolama kapsayıcısını yeniden kullanabilirsiniz. HBase kümeleri için, silinmiş bir HBase kümesi tarafından kullanılan varsayılan blob kapsayıcısını kullanarak yeni bir HBase kümesi oluşturarak HBase tablo şemasını ve verilerini tutabilirsiniz.

[!INCLUDE [secure-transfer-enabled-storage-account](../../includes/hdinsight-secure-transfer.md)]

## <a name="interacting-with-azure-storage"></a>Azure depolama ile etkileşim kurma

Microsoft, Azure depolama ile çalışmak için aşağıdaki araçları sağlar:

| Aracı | Linux | OS X | Windows |
| --- |:---:|:---:|:---:|
| [Azure portal](../storage/blobs/storage-quickstart-blobs-portal.md) |✔ |✔ |✔ |
| [Azure CLı](../storage/blobs/storage-quickstart-blobs-cli.md) |✔ |✔ |✔ |
| [Azure PowerShell](../storage/blobs/storage-quickstart-blobs-powershell.md) | | |✔ |
| [AzCopy](../storage/common/storage-use-azcopy-v10.md) |✔ | |✔ |

## <a name="use-additional-storage-accounts"></a>Ek depolama hesapları kullanma

HDInsight kümesi oluştururken ilişkilendirmek istediğiniz Azure Depolama hesabını belirtirsiniz. Bu depolama hesabına ek olarak, oluşturma işlemi sırasında veya bir küme oluşturulduktan sonra aynı Azure aboneliğinden veya farklı Azure aboneliklerinden ek depolama hesapları ekleyebilirsiniz. Ek depolama hesapları ekleme hakkında yönergeler için bkz. [HDInsight kümeleri oluşturma](hdinsight-hadoop-provision-linux-clusters.md).

> [!WARNING]  
> HDInsight kümesinden farklı bir konumda ek depolama hesabı kullanılması desteklenmez.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure depolama 'yı HDInsight ile nasıl kullanacağınızı öğrendiniz. Bu, ölçeklenebilir, uzun süreli, veri alma çözümleri oluşturmanıza ve depolanan yapılandırılmış ve yapılandırılmamış veriler içindeki bilgilerin kilidini açmak için HDInsight kullanmanıza olanak sağlar.

Daha fazla bilgi için bkz.:

* [Azure HDInsight 'ı kullanmaya başlama](hadoop/apache-hadoop-linux-tutorial-get-started.md)
* [Azure Data Lake Storage kullanmaya başlayın](../data-lake-store/data-lake-store-get-started-portal.md)
* [HDInsight 'a veri yükleme](hdinsight-upload-data.md)
* [HDInsight ile Apache Hive kullanma](hadoop/hdinsight-use-hive.md)
* [HDInsight ile Apache Pig kullanma](hadoop/hdinsight-use-pig.md)
* [HDInsight ile verilere erişimi kısıtlamak için Azure depolama paylaşılan erişim Imzalarını kullanma](hdinsight-storage-sharedaccesssignature-permissions.md)
* [Azure HDInsight kümeleri ile Azure Data Lake Storage 2. kullanma](hdinsight-hadoop-use-data-lake-storage-gen2.md)
