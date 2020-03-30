---
title: Kullanım için depolama seçeneklerini Azure HDInsight kümeleriyle karşılaştırın
description: Azure HDInsight ile depolama türlerine ve bunların nasıl çalıştığına genel bir bakış sağlar.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 98d71434ac9e3f712be0cbd8c505b7d5a537e7cc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79095537"
---
# <a name="compare-storage-options-for-use-with-azure-hdinsight-clusters"></a>Kullanım için depolama seçeneklerini Azure HDInsight kümeleriyle karşılaştırın

HDInsight kümeleri oluştururken birkaç farklı Azure depolama hizmeti arasında seçim yapabilirsiniz:

* Azure Storage
* Azure Data Lake Storage Gen2
* Azure Data Lake Storage Gen1

Bu makalede, bu depolama türleri ve benzersiz özellikleri genel bir bakış sağlar.

Aşağıdaki tablo, HDInsight'ın farklı sürümleriyle desteklenen Azure Depolama hizmetlerini özetleyilmiştir:

| Depolama hizmeti | Hesap türü | Ad Alanı Türü | Desteklenen hizmetler | Desteklenen performans katmanları | Desteklenen erişim katmanları | HDInsight Sürümü | Küme türü |
|---|---|---|---|---|---|---|---|
|Azure Data Lake Storage Gen2| Genel amaçlı V2 | Hiyerarşik (dosya sistemi) | Blob | Standart | Sıcak, Serin, Arşiv | 3.6+ | Kıvılcım 2.1 ve 2.2 hariç tüm|
|Azure Storage| Genel amaçlı V2 | Nesne | Blob | Standart | Sıcak, Serin, Arşiv | 3.6+ | Tümü |
|Azure Storage| Genel amaçlı V1 | Nesne | Blob | Standart | Yok | Tümü | Tümü |
|Azure Storage| Blob Depolama** | Nesne | Blok Blob | Standart | Sıcak, Serin, Arşiv | Tümü | Tümü |
|Azure Data Lake Storage Gen1| Yok | Hiyerarşik (dosya sistemi) | Yok | Yok | Yok | 3.6 Yalnızca | HBase hariç tüm |

**HDInsight kümeleri için yalnızca ikincil depolama hesapları BlobStorage türünde olabilir ve Sayfa Blob desteklenen bir depolama seçeneği değildir.

Azure Depolama hesap türleri hakkında daha fazla bilgi için Azure [depolama hesabına genel bakış](../storage/common/storage-account-overview.md)

Azure Depolama erişim katmanları hakkında daha fazla bilgi için Azure [Blob depolama alanı: Premium (önizleme), Sıcak, Cool ve Arşiv depolama katmanlarına](../storage/blobs/storage-blob-storage-tiers.md) bakın

Birincil ve isteğe bağlı ikincil depolama için farklı hizmet kombinasyonlarını kullanarak bir küme oluşturabilirsiniz. Aşağıdaki tablo, şu anda HDInsight'ta desteklenen küme depolama yapılandırmalarını özetleyecektir:

| HDInsight Sürümü | Birincil Depolama | İkincil Depolama | Destekleniyor |
|---|---|---|---|
| 3.6 & 4.0 | Genel Amaçlı V1 , Genel Amaçlı V2 | Genel Amaçlı V1 , Genel Amaçlı V2, BlobStorage(Blok Blobs) | Evet |
| 3.6 & 4.0 | Genel Amaçlı V1 , Genel Amaçlı V2 | Data Lake Storage Gen2 | Hayır |
| 3.6 & 4.0 | Veri Gölü Depolama Gen2* | Data Lake Storage Gen2 | Evet |
| 3.6 & 4.0 | Veri Gölü Depolama Gen2* | Genel Amaçlı V1 , Genel Amaçlı V2, BlobStorage(Blok Blobs) | Evet |
| 3.6 & 4.0 | Data Lake Storage Gen2 | Data Lake Storage Gen1 | Hayır |
| 3.6 | Data Lake Storage Gen1 | Data Lake Storage Gen1 | Evet |
| 3.6 | Data Lake Storage Gen1 | Genel Amaçlı V1 , Genel Amaçlı V2, BlobStorage(Blok Blobs) | Evet |
| 3.6 | Data Lake Storage Gen1 | Data Lake Storage Gen2 | Hayır |
| 4.0 | Data Lake Storage Gen1 | Herhangi biri | Hayır |
| 4.0 | Genel Amaçlı V1 , Genel Amaçlı V2 | Data Lake Storage Gen1 | Hayır |

*=Bu, küme erişimi için aynı yönetilen kimliği kullanmak üzere kurulum olduğu sürece, bir veya birden çok Veri Gölü Depolama Gen2 hesabı olabilir.

> [!Note] 
> Data Lake Storage Gen2 birincil depolama Spark 2.1 veya 2.2 kümeleri için desteklenmez. 

## <a name="use-azure-data-lake-storage-gen2-with-apache-hadoop-in-azure-hdinsight"></a>Azure HDInsight'ta Apache Hadoop ile Azure Veri Gölü Depolama Gen2'yi kullanma

Azure Veri Gölü Depolama Gen2, Azure Veri Gölü Depolama Gen1'in temel özelliklerini alır ve Azure Blob depolama alanına entegre eder. Bu özellikler arasında Hadoop, Azure Etkin Dizin (Azure AD) ve POSIX tabanlı erişim denetim listeleri (AçS) ile uyumlu bir dosya sistemi bulunur. Bu kombinasyon, Blob depolamanın katmanlama ve veri yaşam döngüsü yönetimini kullanırken Azure Veri Gölü Depolama Gen1'in performansından yararlanmanızı sağlar.

Azure Veri Gölü Depolama Gen2 hakkında daha fazla bilgi için [bkz.](../storage/blobs/data-lake-storage-introduction.md)

### <a name="core-functionality-of-azure-data-lake-storage-gen2"></a>Azure Veri Gölü Depolama Gen2'nin temel işlevselliği

* **Hadoop ile uyumlu erişim:** Azure Veri Gölü Depolama Gen2'de, hadoop Dağıtılmış Dosya Sistemi (HDFS) ile verileri olduğu gibi yönetebilir ve erişebilirsiniz. Azure Blob Dosya Sistemi (ABFS) sürücüsü, Azure HDInsight ve Azure Databricks dahil olmak üzere tüm Apache Hadoop ortamlarında kullanılabilir. Veri Gölü Depolama Gen2'de depolanan verilere erişmek için ABFS'yi kullanın.

* **POSIX izinlerinin bir üst kümesi:** Data Lake Gen2'nin güvenlik modeli, ACL ve POSIX izinlerinin yanı sıra Data Lake Storage Gen2'ye özgü bazı ekstra parçalılıkları destekler. Ayarlar yönetici araçları veya Apache Hive ve Apache Spark gibi çerçeveler aracılığıyla yapılandırılabilir.

* **Maliyet etkinliği:** Data Lake Storage Gen2 düşük maliyetli depolama kapasitesi ve işlemleri sunar. Azure Blob depolama yaşam döngüsü gibi özellikler, veriler yaşam döngüsü boyunca ilerlerken faturalandırma oranlarını ayarlayarak maliyetleri düşürmeye yardımcı olur.

* **Blob depolama araçları, çerçeveleri ve uygulamalarıyla uyumluluk:** Data Lake Storage Gen2, Blob depolama için çok çeşitli araçlar, çerçeveler ve uygulamalarla çalışmaya devam etmektedir.

* **Optimize edilmiş sürücü:** ABFS sürücüsü özellikle büyük veri analitiği için optimize edilebiyi tasarlar. İlgili REST API'leri dağıtılmış dosya sistemi (DFS) bitiş noktası, dfs.core.windows.net ile su yüzüne çıkar.

### <a name="whats-new-for-azure-data-lake-storage-gen-2"></a>Azure Veri Gölü Depolama Gen 2 için yenilikler

#### <a name="managed-identities-for-secure-file-access"></a>Güvenli dosya erişimi için yönetilen kimlikler

Azure HDInsight, Azure Veri Gölü Depolama Gen2'deki dosyalara küme erişimini güvence altına almak için yönetilen kimlikleri kullanır. Yönetilen kimlikler, Azure hizmetlerine otomatik olarak yönetilen kimlik bilgileri kümesi sağlayan Azure Etkin Dizin özelliğidir. Bu kimlik bilgileri, Etkin Dizin kimlik doğrulamasını destekleyen herhangi bir hizmetin kimliğini doğrulamak için kullanılabilir. Yönetilen kimlikleri kullanmak, kimlik bilgilerini kod veya yapılandırma dosyalarında depolamanızı gerektirmez.

Daha fazla bilgi için Azure [kaynakları için Yönetilen kimlikler'e](../active-directory/managed-identities-azure-resources/overview.md)bakın.

#### <a name="azure-blob-file-system-driver"></a>Azure Blob Dosya Sistemi sürücüsü

Apache Hadoop uygulamaları yerel disk depolama dan veri okumak ve yazmak için yerli bekliyoruz. ABFS gibi hadoop dosya sistemi sürücüsü, Hadoop uygulamalarının düzenli Hadoop dosya sistemi işlemlerini taklit ederek bulut depolama ile çalışmasını sağlar. Sürücü, uygulamadan alınan komutları gerçek bulut depolama platformunun anladığı işlemlere dönüştürür.

Daha önce Hadoop dosya sistemi sürücüsü, tüm dosya sistemi işlemlerini istemci tarafındaki Azure Depolama REST API çağrılarına dönüştürmüş ve ardından REST API'sini çağırmıştı. Ancak bu istemci tarafı dönüştürme, birden çok REST API çağrısıyla, bir dosyanın yeniden adlandırılması gibi tek bir dosya sistemi çalışması için çağrı yapılmasıyla sonuçlanır. ABFS, Hadoop dosya sistemi mantığının bir kısmını istemci tarafından sunucu tarafına taşımıştır. Azure Veri Gölü Depolama Gen2 API artık Blob API ile paralel olarak çalışır. Artık yaygın Hadoop dosya sistemi işlemleri tek bir REST API çağrısıyla yürütülebileceğinden, bu geçiş performansı artırır.

Daha fazla bilgi için [Bkz. Azure Blob Filesystem sürücüsü (ABFS): Hadoop için özel bir Azure Depolama sürücüsü.](../storage/blobs/data-lake-storage-abfs-driver.md)

#### <a name="uri-scheme-for-azure-data-lake-storage-gen-2"></a>Azure Veri Gölü Depolama Gen 2 için URI şeması 

Azure Veri Gölü Depolama Gen2, AZURE Depolama'daki dosyalara HDInsight'tan erişmek için yeni bir URI şeması kullanır:

`abfs://<FILE_SYSTEM_NAME>@<ACCOUNT_NAME>.dfs.core.windows.net/<PATH>`

URI şeması SSL şifreli erişim sağlar.

`<FILE_SYSTEM_NAME>`dosya sistemi Veri Gölü Depolama Gen2 yolunu tanımlar.

`<ACCOUNT_NAME>`Azure Depolama hesap adını tanımlar. Tam uygun etki alanı adı (FQDN) gereklidir.

`<PATH>`dosya veya dizin HDFS yol adıdır.

Değerler için `<FILE_SYSTEM_NAME>` `<ACCOUNT_NAME>` ve belirtilmemişse, varsayılan dosya sistemi kullanılır. Varsayılan dosya sistemindeki dosyalar için göreli bir yol veya mutlak bir yol kullanın. Örneğin, HDInsight kümeleriyle birlikte gelen `hadoop-mapreduce-examples.jar` dosya, aşağıdaki yollardan biri kullanılarak yönlendirilebilir:

```
abfs://myfilesystempath@myaccount.dfs.core.windows.net/example/jars/hadoop-mapreduce-examples.jar
abfs:///example/jars/hadoop-mapreduce-examples.jar /example/jars/hadoop-mapreduce-examples.jar
```

> [!Note]
> Dosya adı `hadoop-examples.jar` HDInsight sürümleri 2.1 ve 1.6 kümelerindedir. HDInsight dışındaki dosyalarla çalışırken, çoğu yardımcı programABFS biçimini tanımadığını, bunun yerine `example/jars/hadoop-mapreduce-examples.jar`.

Daha fazla bilgi için [bkz.](../storage/blobs/data-lake-storage-introduction-abfs-uri.md)

## <a name="azure-storage"></a>Azure Storage

Azure Depolama, HDInsight ile sorunsuz bir şekilde entegre olan güçlü bir genel amaçlı depolama çözümüdür. HDInsight, Azure Depolama’daki bir blob kapsayıcıyı kümenin varsayılan dosya sistemi olarak kullanabilir. HDFS arabirimi aracılığıyla, HDInsight'taki tüm bileşenler blob olarak depolanan yapılandırılmış veya yapılandırılmamış veriler üzerinde doğrudan çalışabilir.

HDInsight günlüklerini ve geçici dosyaları kendi iş verilerinizden yalıtmak için varsayılan küme depolamanız ve iş verileriniz için ayrı depolama kapsayıcıları kullanmanızı öneririz. Ayrıca, depolama maliyetini azaltmak için her kullanımdan sonra uygulama ve sistem günlükleri içeren varsayılan blob kapsayıcısını silmenizi öneririz. Kapsayıcıyı silmeden önce günlükleri aldığınızdan emin olun.

**Seçili ağlardaki**Güvenlik Duvarları **ve sanal ağlar** kısıtlamalarıyla depolama hesabınızı güvenli hale getirmeyi seçerseniz, HDInsight'ın depolama hesabınıza erişebilmesi için **güvenilen Microsoft hizmetlerine izin verme** istisnasını etkinleştirdiğinden emin olun.

### <a name="hdinsight-storage-architecture"></a>HDInsight depolama mimarisi

Aşağıdaki diyagram, Azure Depolama'nın HDInsight mimarisinin soyut bir görünümünü sağlar:

![HDInsight Depolama Mimarisi](./media/hdinsight-hadoop-compare-storage-options/storage-architecture.png "HDInsight Depolama Mimarisi")

HDInsight, işlem düğümlerine yerel olarak bağlı olan dağıtılmış dosya sistemine erişim imkanı sağlar. Bu dosya sistemine tam uygun URI kullanılarak erişilebilir, örneğin:

    hdfs://<namenodehost>/<path>

HDInsight aracılığıyla Azure Depolama'daki verilere de erişebilirsiniz. Söz dizimi aşağıdaki gibidir:

    wasb://<containername>@<accountname>.blob.core.windows.net/<path>

HDInsight kümelerine sahip bir Azure Depolama hesabı kullanırken aşağıdaki ilkeleri göz önünde bulundurun:

* **Bir kümeye bağlı depolama hesaplarındaki kapsayıcılar:** Oluşturma sırasında hesap adı ve anahtar kümeyle ilişkilendirildiğinden, bu kapsayıcılardaki blob'lara tam erişiminiz vardır.

* **Kümeye bağlı *olmayan* depolama hesaplarındaki ortak kapsayıcılar veya ortak lekeler:** Kaplarda ki lekeler için salt okunur izniniz var.
  
  > [!NOTE]  
  > Ortak kapsayıcılar, bu kapsayıcıda bulunan tüm lekelerin bir listesini almanızı ve kapsayıcı meta verilerini almanızı sağlar. Genel blob'lar, yalnızca tam URL'yi biliyorsanız blob erişiminize izin verir. Daha fazla bilgi için bkz. [Kapsayıcılara ve bloblara anonim okuma erişimini yönetme](../storage/blobs/storage-manage-access-to-resources.md).

* Bir **kümeye bağlı *olmayan* depolama hesaplarındaki özel kapsayıcılar:** WebHCat işlerini gönderirken depolama hesabını tanımlamadığınız sürece kapsayıcılarda bulunan lekelere erişemezsiniz. 

Oluşturma işleminde tanımlanan depolama hesapları ve bunların anahtarların %HADOOP_HOME%/conf/core-site.xml küme düğümlerinde depolanır. Varsayılan olarak, HDInsight çekirdek site.xml dosyasında tanımlanan depolama hesaplarını kullanır. Bu ayarı [Apache Ambari'yi](./hdinsight-hadoop-manage-ambari.md)kullanarak değiştirebilirsiniz.

Apache Hive, MapReduce, Apache Hadoop streaming ve Apache Pig gibi birden çok WebHCat işi, depolama hesaplarının ve meta verilerin açıklamasını taşıyabilir. (Bu şu anda depolama hesapları ile Pig için geçerlidir, ancak meta veriler için geçerli değildir.) Daha fazla bilgi için bkz: [Alternatif depolama hesapları ve metastore'lar içeren bir HDInsight kümesi kullanma.](https://social.technet.microsoft.com/wiki/contents/articles/23256.using-an-hdinsight-cluster-with-alternate-storage-accounts-and-metastores.aspx)

Bloblar yapılandırılmış ve yapılandırılmamış veriler için kullanılabilir. Blob kapsayıcıları verileri anahtar/değer çiftleri olarak depolar ve dizin hiyerarşisi yoktur. Ancak anahtar adı bir dizini yapısı içinde depolanmış gibi görünmesi için bir eğik çizgi karakteri ( / ) içerebilir. Örneğin, bir blob anahtarı olabilir `input/log1.txt`. Gerçek `input` bir dizin yoktur, ancak anahtar adındaki eğik çizgi karakteri nedeniyle anahtar bir dosya yoluna benzer.

### <a name="benefits-of-azure-storage"></a><a id="benefits"></a>Azure Depolamanın yararları
Bir araya gelebilen işlem kümeleri ve depolama kaynaklarının zımni performans maliyetleri vardır. Bu maliyetler, azure bölgesindeki depolama hesabı kaynaklarına yakın bir şekilde oluşturulan işlem kümeleri tarafından azaltılır. Bu bölgede, bilgi işlem düğümleri Verilere Azure Depolama'daki yüksek hızlı ağ üzerinden verimli bir şekilde erişebilir.

Verileri HDFS yerine Azure Depolama'da depoladiğinizde, aşağıdakiler gibi avantajlar elde elabilirsiniz:

* **Verileri yeniden kullanma ve paylaşma:** HDFS’deki veriler işlem kümesi içinde bulunur. Yalnızca işlem kümesi erişimi olan uygulamalar HDFS API'lerini kullanarak verileri kullanabilir. Buna karşılık Azure Depolama'daki verilere HDFS API'leri veya Blob depolama REST API'leri aracılığıyla erişilebilir. Bu düzenleme sayesinde, verileri üretmek ve tüketmek için daha büyük bir uygulama kümesi (diğer HDInsight kümeleri dahil) ve araçlar kullanılabilir.

* **Veri arşivleme:** Veriler Azure Depolama'da depolandığında, hesaplama için kullanılan HDInsight kümeleri kullanıcı verilerini kaybetmeden güvenli bir şekilde silinebilir.

* **Veri depolama maliyeti:** Bir bilgi işlem kümesinin maliyeti Azure Depolama'nın maliyetinden daha yüksek olduğundan, verileri uzun vadede DFS'de depolamak verileri Azure Depolama'da depolamaktan daha maliyetlidir. Ayrıca, verilerin her bilgi işlem kümesi oluşturma için yeniden yüklenmesi gerekmedığından, veri yükleme maliyetlerinden de tasarruf emiş oluyorsunuz.

* **Esnek ölçeklendirme:** HDFS size ölçeklendirilmiş dosya sistemi sağlamakla birlikte, ölçek, kümeniz için oluşturduğunuz düğüm sayısı tarafından belirlenir. Ölçeği değiştirmek, Azure Depolama'da otomatik olarak aldığınız elastik ölçekleme özelliklerine güvenmekten daha karmaşık olabilir.

* **Coğrafi çoğaltma:** Azure Depolama alanınız coğrafi olarak çoğaltılabilir. Coğrafi çoğaltma size coğrafi kurtarma ve veri artıklığı verse de, coğrafi olarak çoğaltılan konumun üzerinde bir hata performansınızı ciddi şekilde etkiler ve ek maliyetlere neden olabilir. Bu nedenle, yalnızca verilerin değeri ek maliyeti haklı gösteriyorsa, coğrafi çoğaltmayı dikkatli bir şekilde seçin.

Belirli MapReduce işleri ve paketleri, Azure Depolama'da depolamak istemediğiniz ara sonuçlar oluşturabilir. Bu durumda, verileri yerel HDFS'de depolamayı seçebilirsiniz. HDInsight, Kovan işlerinde ve diğer işlemlerde bu ara sonuçların birkaçı için DFS kullanır.

> [!NOTE]  
> Çoğu HDFS komutu (örneğin, `ls`, `copyFromLocal`ve `mkdir`) Azure Depolama'da beklendiği gibi çalışır. Yalnızca yerel HDFS uygulamasına özgü komutlar (DFS olarak adlandırılır) `fschk` ve `dfsadmin`Azure Depolama'da farklı davranışlar gösterir.

## <a name="overview-of-azure-data-lake-storage-gen1"></a>Azure Veri Gölü Depolama Gen1'e Genel Bakış

Azure Veri Gölü Depolama Gen1, büyük veri analitik iş yükleri için kurumsal çapında bir hiper ölçekli depodur. Azure Veri Gölü'nü kullanarak, operasyonel ve araştırmacı analizler için istediğiniz boyutta, türde ve yutma hızından verileri tek bir yerde yakalayabilirsiniz.

WebHDFS uyumlu REST API'lerini kullanarak Hadoop'tan (HDInsight kümesiyle kullanılabilir) Veri Gölü Depolama Gen1'e erişin. Data Lake Storage Gen1, depolanan veriler üzerinde analiz sağlamak üzere tasarlanmıştır ve veri analizi senaryolarında performans için ayarlanmıştır. Kutunun dışında, gerçek dünya kurumsal kullanım örnekleri için gerekli olan yetenekleri içerir. Bu özellikler arasında güvenlik, yönetilebilirlik, ölçeklenebilirlik, güvenilirlik ve kullanılabilirlik yer almaktadır.

Azure Veri Gölü Depolama Gen1 hakkında daha fazla bilgi için, [Azure Veri Gölü Depolama Gen1'e ayrıntılı Genel Bakış'a](../data-lake-store/data-lake-store-overview.md)bakın.

Veri Gölü Depolama Gen1'in temel özellikleri şunlardır.

### <a name="compatibility-with-hadoop"></a>Hadoop ile uyumluluk

Data Lake Storage Gen1, HDFS ile uyumlu ve Hadoop ekosistemi ile çalışan bir Apache Hadoop dosya sistemidir.  WebHDFS API'sini kullanan mevcut HDInsight uygulamalarınız veya hizmetleriniz Veri Gölü Depolama Gen1 ile kolayca tümleşebilir. Data Lake Storage Gen1, uygulamalar için WebHDFS uyumlu BIR REST arabirimini de ortaya çıkarır.

Data Lake Storage Gen1'de depolanan veriler MapReduce veya Hive gibi Hadoop analitik çerçeveleri kullanılarak kolayca analiz edilebilir. Azure HDInsight kümeleri, Veri Gölü Depolama Gen1'de depolanan verilere doğrudan erişmek için kullanılabilir ve yapılandırılabilir.

### <a name="unlimited-storage-petabyte-files"></a>Sınırsız depolama, petabayt boyutlu dosyalar

Data Lake Storage Gen1 sınırsız depolama sağlar ve analitik için çeşitli verileri depolamak için uygundur. Hesap boyutlarına, dosya boyutlarına veya veri gölünde depolanabilecek veri miktarına sınırlama getirmez. Tek tek dosyaların boyutu kilobaytlardan petabaytlara kadar değişebilir ve bu da Veri Gölü Depolama Gen1'i her türlü veriyi depolamak için mükemmel bir seçim haline getirir. Veriler, birden çok kopya oluşturarak uzun süre saklanır ve verilerin veri gölünde ne kadar süreyle depolanabileceği konusunda herhangi bir sınırlama yoktur.

### <a name="performance-tuning-for-big-data-analytics"></a>Büyük veri analitiği için performans alamı

Veri Gölü Depolama Gen1, büyük miktarda veriyi sorgulamak ve analiz etmek için büyük iş gücü gerektiren büyük ölçekli analitik sistemleri çalıştırmak üzere oluşturulmuştur. Veri gölü, bir dosyanın parçalarını birkaç ayrı depolama sunucusuna yayır. Verileri çözümlürken, bu kurulum dosya paralel olarak okunduğunda okuma iş buzunu iyileştirir.

### <a name="readiness-for-enterprise-highly-available-and-secure"></a>Kurumsal kullanıma hazır olma: Yüksek kullanılabilir ve güvenli

Data Lake Storage Gen1 endüstri standardı kullanılabilirlik ve güvenilirlik sağlar. Veri varlıkları dayanıklı olarak depolanır: gereksiz kopyalar beklenmeyen hatalara karşı korur. İşletmeler, çözümlerinde Veri Gölü Depolama Gen1'i mevcut veri platformlarının önemli bir parçası olarak kullanabilirler.

Data Lake Storage Gen1, depolanan veriler için kurumsal dereceli güvenlik de sağlar. Daha fazla bilgi için bkz: [Azure Veri Gölü Depolama Gen1'de veri güvenliğini sağlama.](#DataLakeStoreSecurity)

### <a name="flexible-data-structures"></a>Esnek veri yapıları

Veri Gölü Depolama Gen1, önceki dönüşümlere gerek kalmadan, verileri olduğu gibi yerel biçiminde depolayabilir. Veri Gölü Depolama Gen1, veriler yüklenmeden önce bir şema tanımlanmasını gerektirmez. Bireysel analitik çerçeve verileri yorumlar ve analiz sırasında bir şema tanımlar. Rasgele boyutve biçimlerde dosyaları depolayabildiği için, Veri Gölü Depolama Gen1 yapılandırılmış, yarı yapılandırılmış ve yapılandırılmamış verileri işleyebilir.

Veri için Veri Gölü Depolama Gen1 kapsayıcıları aslında klasörler ve dosyalardır. SDK'ları, Azure portalını ve Azure PowerShell'i kullanarak depolanan verileri kullanırsınız. Bu arabirimleri ve uygun kapsayıcıları kullanarak verilerinizi depoya koyduğunuz sürece, her türlü veriyi depolayabilirsiniz. Veri Gölü Depolama Gen1, depolanan veri türüne göre özel veri işleme yapmaz.

## <a name="data-security-in-data-lake-storage-gen1"></a><a name="DataLakeStoreSecurity"></a>Veri Gölü Depolama Gen1'de veri güvenliği
Veri Gölü Depolama Gen1, kimlik doğrulaması için Azure Etkin Dizini'ni kullanır ve verilerinize erişimi yönetmek için erişim denetim listelerini (AçS) kullanır.

| **Özellik** | **Açıklama** |
| --- | --- |
| Kimlik doğrulaması |Veri Gölü Depolama Gen1, Veri Gölü Depolama Gen1'de depolanan tüm veriler için kimlik ve erişim yönetimi için Azure Active Directory (Azure AD) ile tümleşir. Tümleştirme sayesinde, Veri Gölü Depolama Gen1 tüm Azure REKLAM özelliklerinden yararlanır. Bu özellikler arasında çok faktörlü kimlik doğrulama, Koşullu Erişim, rol tabanlı erişim denetimi, uygulama kullanım izleme, güvenlik izleme ve uyarı ve benzeri özellikler yer almaktadır. Data Lake Storage Gen1, REST arabirimi içinde kimlik doğrulama için OAuth 2.0 protokolünü destekler. [Azure Etkin Dizini kullanarak Azure Veri Gölü Depolama Gen1'deki Kimlik Doğrulama'ya](../data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md) bakın|
| Erişim denetimi |Data Lake Storage Gen1, WebHDFS protokolü tarafından ortaya çıkarılan POSIX tarzı izinleri destekleyerek erişim denetimi sağlar. ACL’ler kök klasörde, alt klasörlerde ve dosyalarda tek tek etkinleştirilebilir. ACD'lerin Veri Gölü Depolama Gen1 bağlamında nasıl çalıştığı hakkında daha fazla bilgi için Veri [Gölü Depolama Gen1'deki Erişim denetimine](../data-lake-store/data-lake-store-access-control.md)bakın. |
| Şifreleme |Veri Gölü Depolama Gen1 de hesapta depolanan veriler için şifreleme sağlar. Veri Gölü Depolama Gen1 hesabı oluştururken şifreleme ayarlarını belirtirsiniz. Verilerinizin şifrelenmesini seçebilir veya şifreleme yi tercih edebilirsiniz. Daha fazla bilgi için Bkz. [Veri Gölü Depolama Gen1'de Şifreleme.](../data-lake-store/data-lake-store-encryption.md) Şifrelemeyle ilgili yapılandırmanın nasıl sağlayabileceğine ilişkin talimatlar için [bkz.](../data-lake-store/data-lake-store-get-started-portal.md) |

Veri Gölü Depolama Gen1'de veri güvenliğini sağlama hakkında daha fazla bilgi edinmek için [bkz.](../data-lake-store/data-lake-store-secure-data.md)

## <a name="applications-that-are-compatible-with-data-lake-storage-gen1"></a>Veri Gölü Depolama Gen1 ile uyumlu uygulamalar
Data Lake Storage Gen1, Hadoop ekosistemindeki çoğu açık kaynak bileşeniile uyumludur. Ayrıca diğer Azure hizmetleriyle sorunsuz şekilde tümleştirilir.  Data Lake Storage Gen1'in hem açık kaynak bileşenleri hem de diğer Azure hizmetleriyle nasıl kullanAbileceği hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin.

* Data Lake Storage Gen1 ile birlikte çalışan açık kaynak uygulamalarının bir listesi için [Azure Veri Gölü Depolama Gen1 ile çalışan açık kaynak büyük veri uygulamalarına](../data-lake-store/data-lake-store-compatible-oss-other-applications.md) bakın.
* Daha geniş bir senaryo aralığı sağlamak için Veri Gölü Deposu Gen1'in diğer Azure hizmetleriyle nasıl kullanılacağını anlamak için [Azure Veri Gölü Deposu Gen1'i diğer](../data-lake-store/data-lake-store-integrate-with-other-services.md) Azure hizmetleriyle tümleştirme bölümüne bakın.
* Veri alma, veri işleme, veri indirme ve verileri görselleştirme gibi senaryolarda Veri Gölü Depolama Gen1'in nasıl kullanılacağını öğrenmek [için büyük veri gereksinimleri için Azure Veri Gölü Depolama Gen1'i](../data-lake-store/data-lake-store-data-scenarios.md) kullanma konusuna bakın.

## <a name="data-lake-storage-gen1-file-system-adl"></a>Veri Gölü Depolama Gen1 dosya sistemi (adl://)
Hadoop ortamlarında (HDInsight kümesiyle kullanılabilir) Yeni dosya sistemi olan AzureDataLakeFilesystem (adl://) aracılığıyla Veri Gölü Depolama Gen1'e erişebilirsiniz. adl:// kullanan uygulama ve hizmetlerin performansı, webhdfs'te şu anda bulunmayan şekillerde optimize edilebilir. Sonuç olarak, Veri Gölü Depolama Gen1'i kullandığınızda, önerilen adl:// kullanarak en iyi performansı elde etme veya WebHDFS API'sini doğrudan kullanmaya devam ederek varolan kodu koruma esnekliğine sahip olursunuz. Azure HDInsight, Veri Gölü Depolama Gen1'de en iyi performansı sağlamak için AzureDataLakeFilesystem'den tam olarak yararlanır.

Aşağıdakileri kullanarak Veri Gölü Depolama Gen1'deki verilerinize erişin:

`adl://<data_lake_storage_gen1_name>.azuredatalakestore.net`

Veri Gölü Depolama Gen1'deki verilere nasıl erişilisi hakkında daha fazla bilgi [için, depolanan verilerde kullanılabilen Eylemler'e](../data-lake-store/data-lake-store-get-started-portal.md#properties)bakın.



## <a name="next-steps"></a>Sonraki adımlar

* [Azure Veri Gölü Depolama Gen2'ye Giriş](../storage/blobs/data-lake-storage-introduction.md)
* [Azure Depolama’ya giriş](../storage/common/storage-introduction.md)
