---
title: "Depolama: Şirket içinde Apache Hadoop'u Azure HDInsight'a geçirin"
description: Şirket içi Hadoop kümelerini Azure HDInsight'a geçirmek için depolama nın en iyi uygulamalarını öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: ashishth
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 12/10/2019
ms.openlocfilehash: f19d4adad675cdf95f59aca0f752f46211b75e8f
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80436933"
---
# <a name="migrate-on-premises-apache-hadoop-clusters-to-azure-hdinsight"></a>Şirket içi Apache Hadoop kümelerini Azure HDInsight'a geçirin

Bu makalede, Azure HDInsight sistemlerinde veri depolama için öneriler verebilmiştir. Şirket içi Apache Hadoop sistemlerini Azure HDInsight'a geçirmede yardımcı olmak için en iyi uygulamaları sağlayan bir serinin parçasıdır.

## <a name="choose-right-storage-system-for-hdinsight-clusters"></a>HDInsight kümeleri için doğru depolama sistemini seçin

Şirket içi Apache Hadoop Dosya Sistemi (HDFS) dizin yapısı Azure Depolama veya Azure Veri Gölü Depolama'da yeniden oluşturulabilir. Daha sonra, kullanıcı verilerini kaybetmeden hesaplama için kullanılan HDInsight kümelerini güvenle silebilirsiniz. Her iki hizmet de hem varsayılan dosya sistemi hem de BIR HDInsight kümesi için ek bir dosya sistemi olarak kullanılabilir. HDInsight kümesi ve depolama hesabı aynı bölgede barındırılmalıdır.

### <a name="azure-storage"></a>Azure Storage

HDInsight kümeleri, Azure Depolama'daki blob kapsayıcısını varsayılan dosya sistemi veya ek dosya sistemi olarak kullanabilir.Standart katman depolama hesabı HDInsight kümeleriyle kullanılmak üzere desteklenir. Premier katmanı desteklenmez. Varsayılan Blob kapsayıcısı iş geçmişi ve iş günlükleri gibi kümeye özel bilgileri depolar.Birden çok küme için varsayılan dosya sistemi desteklenmez olarak bir blob kapsayıcı paylaşımı.

Oluşturma işleminde tanımlanan depolama hesapları ve ilgili anahtarları küme `%HADOOP_HOME%/conf/core-site.xml` düğümlerinde depolanır. Ayrıca Ambari UI HDFS yapılandırma "Özel çekirdek site" bölümü altında erişilebilir. Depolama hesabı anahtarı varsayılan olarak şifrelenir ve Hadoop daemons'a geçmeden önce anahtarların şifresini çözmek için özel bir şifre çözme komut dosyası kullanılır. Hive, MapReduce, Hadoop streaming ve Pig gibi işler, depolama hesaplarının ve meta verilerin açıklamasını taşır.

Azure Depolama coğrafi olarak çoğaltılabilir. Coğrafi çoğaltma coğrafi kurtarma ve veri artıklığı verse de, coğrafi olarak çoğaltılan konumun başarısız olması performansı ciddi şekilde etkiler ve ek maliyetlere neden olabilir. Öneri, coğrafi çoğaltmayı akıllıca ve yalnızca verilerin değeri ek maliyete değiyorsa seçmektir.

Aşağıdaki biçimlerden biri Azure Depolama'da depolanan verilere erişmek için kullanılabilir:

|Veri Erişim Biçimi |Açıklama |
|---|---|
|`wasb:///`|Şifrelenmemiş iletişimi kullanarak varsayılan depolamaya erişin.|
|`wasbs:///`|Şifreli iletişimi kullanarak varsayılan depolamaya erişin.|
|`wasb://<container-name>@<account-name>.blob.core.windows.net/`|Varsayılan olmayan bir depolama hesabıyla iletişim kurarken kullanılır. |

[Standart depolama hesapları için ölçeklenebilirlik hedefleri,](../../storage/common/scalability-targets-standard-account.md) Azure Depolama hesaplarının geçerli sınırlarını listeler. Uygulamanın gereksinimleri tek bir depolama hesabının ölçeklenebilirlik hedeflerini aşarsa, uygulama birden çok depolama hesabı kullanmak ve ardından veri nesnelerini bu depolama hesapları arasında bölmek için oluşturulabilir.

[Azure Depolama Analizi,](../../storage/storage-analytics.md) tüm depolama hizmetleri için ölçümler sağlar ve Azure portalı grafikler aracılığıyla görselleştirilecek toplama ölçümleri yapılandırılabilir. Depolama kaynak ölçümleri için eşiklere ne zaman ulaşıldığını bildirmek için uyarılar oluşturulabilir.

Azure Depolama, bir uygulama veya başka bir depolama hesabı kullanıcısı tarafından yanlışlıkla değiştirildiğinde veya silindiğinde verileri kurtarmaya yardımcı olmak [için blob nesneleri için yumuşak silme](../../storage/blobs/storage-blob-soft-delete.md) sunar.

[Blob anlık görüntüleri](https://docs.microsoft.com/rest/api/storageservices/creating-a-snapshot-of-a-blob)oluşturabilirsiniz. Anlık görüntü, bir noktada alınan bir lekenin salt okunur halidir ve bir lekeyi yedeklemenin bir yolunu sağlar. Anlık görüntü oluşturulduktan sonra okunabilir, kopyalanabilir veya silinebilir, ancak değiştirilemez.

> [!Note]
> "Wasbs" sertifikasına sahip olmayan şirket içi Hadoop Dağıtımları'nın eski sürümleri için Java güven deposuna aktarılmaları gerekir.

Sertifikaları Java güven deposuna almak için aşağıdaki yöntemler kullanılabilir:

Azure Blob TLS/SSL sertifikasını bir dosyaya indirin

```bash
echo -n | openssl s_client -connect <storage-account>.blob.core.windows.net:443 | sed -ne '/-BEGIN CERTIFICATE-/,/-END CERTIFICATE-/p' > Azure_Storage.cer
```

Yukarıdaki dosyayı tüm düğümlerde Java güven deposuna aktarma

```bash
keytool -import -trustcacerts -keystore /path/to/jre/lib/security/cacerts -storepass changeit -noprompt -alias blobtrust -file Azure_Storage.cer
```

Eklenen sertifikanın güven deposunda olduğunu doğrulama

```bash
keytool -list -v -keystore /path/to/jre/lib/security/cacerts
```

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure HDInsight kümeleriyle Azure Depolama'yı kullanma](../hdinsight-hadoop-use-blob-storage.md)
- [Standart depolama hesapları için ölçeklenebilirlik hedefleri](../../storage/common/scalability-targets-standard-account.md)
- [Blob depolama için ölçeklenebilirlik ve performans hedefleri](../../storage/blobs/scalability-targets.md)
- [Microsoft Azure Depolama Performansı ve Ölçeklenebilirlik Onay Listesi](../../storage/common/storage-performance-checklist.md)
- [Microsoft Azure Depolama izleme, tanılama ve sorun giderme](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md)
- [Azure portalında depolama hesabını izleme](../../storage/common/storage-monitor-storage-account.md)

### <a name="azure-data-lake-storage-gen1"></a>Azure Data Lake Storage Gen1

Azure Veri Gölü Depolama, HDFS ve POSIX stili erişim denetim modelini uygular. İnce taneli erişim denetimi için AAD ile birinci sınıf tümleştirme sağlar. Depolayabildiği verilerin boyutu veya büyük ölçüde paralel analitik çalıştırma yeteneği için hiçbir sınır yoktur.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure portalını kullanarak Veri Gölü Depolama ile HDInsight kümeleri oluşturun](../../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md)
- [Azure HDInsight kümeleriyle Veri Gölü Depolama'yı kullanma](../hdinsight-hadoop-use-data-lake-store.md)

### <a name="azure-data-lake-storage-gen2"></a>Azure Data Lake Storage Gen2

Azure Veri Gölü Depolama Gen2 en son depolama teklifidir. İlk nesil Azure Veri Gölü Depolama'nın temel özelliklerini, doğrudan Azure Blob Depolama'ya entegre edilmiş Hadoop uyumlu dosya sistemi bitiş noktasıyla birleşir. Bu geliştirme, nesne depolamanın ölçek ve maliyet avantajlarını, genellikle yalnızca şirket içi dosya sistemleriyle ilişkili güvenilirlik ve performansla birleştirir.

ADLS Gen 2, [Azure Blob depolama](../../storage/blobs/storage-blobs-introduction.md) alanının üzerine inşa edilmiştir ve hem dosya sistemi hem de nesne depolama paradigmalarını kullanarak verilerle arayüz oluşturanıza olanak tanır. Dosya sistemi semantikleri, dosya düzeyi güvenliği ve ölçek gibi [Azure Veri Gölü Depolama Gen1'inin](../../data-lake-store/index.yml)özellikleri düşük maliyetli, katmanlı depolama, yüksek kullanılabilirlik/olağanüstü durum kurtarma özellikleri ve Azure [Blob depolamadan](../../storage/blobs/storage-blobs-introduction.md)büyük bir SDK/araç ekosistemi yle birleştirilir. Veri Gölü Depolama Gen2'de, analitik iş yükleri için optimize edilmiş bir dosya sistemi arabiriminin avantajlarını eklerken nesne depolamanın tüm nitelikleri kalır.

Veri Gölü Depolama Gen2'nin temel bir özelliği, nesneleri/dosyaları performant veri erişimi için dizinler hiyerarşisi içinde düzenleyen Blob depolama hizmetine [hiyerarşik](../../storage/data-lake-storage/namespace.md) bir ad alanı eklenmesidir.Hiyerarşik yapı, dizin adını öneki paylaşan tüm nesneleri sıralayıp işlemek yerine dizini yeniden adlandırma veya silme gibi işlemleri dizinde tek atomik meta veri işlemleri olarak etkinleştirir.

Geçmişte, bulut tabanlı analitik performans, yönetim ve güvenlik alanlarında uzlaşmak zorundaydı. Azure Veri Gölü Depolama (ADLS) Gen2'nin temel özellikleri aşağıdaki gibidir:

- **Hadoop uyumlu erişim**: Azure Data Lake Storage Gen2, [Hadoop Dağıtılmış Dosya Sistemi (HDFS)](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsDesign.html)ile olduğu gibi verileri yönetmenize ve erişmenize olanak tanır. Yeni [ABFS sürücüsü,](../../storage/data-lake-storage/abfs-driver.md)  [Azure HDInsight'a](../index.yml)dahil olan tüm Apache Hadoop ortamlarında kullanılabilir. Bu sürücü, Veri Gölü Depolama Gen2'de depolanan verilere erişmenizi sağlar.

- **POSIX izinlerinin bir üst kümesi**: Data Lake Gen2'nin güvenlik modeli, Data Lake Storage Gen2'ye özgü bazı ekstra parçalılıkla birlikte ACL ve POSIX izinlerini tam olarak destekler. Ayarlar yönetici araçları veya Hive ve Spark gibi çerçeveler aracılığıyla yapılandırılabilir.

- **Uygun maliyetli**: Data Lake Storage Gen2 düşük maliyetli depolama kapasitesi ve işlemlerine sahiptir. Veriler tüm yaşam döngüsü boyunca geçiş yaptığından, faturalandırma oranları [Azure Blob depolama yaşam döngüsü](../../storage/common/storage-lifecycle-management-concepts.md)gibi yerleşik özellikler aracılığıyla maliyetleri en aza indirmek için değişir.

- **Blob depolama araçları, çerçeveleri ve uygulamalarıyla çalışır**: Data Lake Storage Gen2, Blob depolama için bugün var olan çok çeşitli araçlar, çerçeveler ve uygulamalarla çalışmaya devam etmektedir.

- **Optimize edilmiş sürücü**: Azure Blob Filesystem sürücüsü (ABFS), özellikle büyük veri analitiği için [optimize edilebilmektedir.](../../storage/data-lake-storage/abfs-driver.md)  İlgili REST API'leri dfs bitiş noktası üzerinden su yüzüne çıkar, dfs.core.windows.net.

Aşağıdaki biçimlerden biri ADLS Gen2'de depolanan verilere erişmek için kullanılabilir:
- `abfs:///`: Küme için varsayılan Veri Gölü Depolama'ya erişin.
- `abfs://file_system@account_name.dfs.core.windows.net`: Varsayılan olmayan bir Veri Gölü Depolama ile iletişim kurarken kullanılır.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Veri Gölü Depolama Gen2'ye Giriş](../../storage/data-lake-storage/introduction.md)
- [Azure Blob Filesystem sürücüsü (ABFS.md)](../../storage/data-lake-storage/abfs-driver.md)
- [Azure HDInsight kümeleriyle Azure Veri Gölü Depolama Gen2'yi kullanma](../hdinsight-hadoop-use-data-lake-storage-gen2.md)

## <a name="secure-azure-storage-keys-within-on-premises-hadoop-cluster-configuration"></a>Şirket içi Hadoop küme yapılandırması içinde Azure Depolama anahtarlarını güvenli hale

Hadoop yapılandırma dosyalarına eklenen Azure Depolama anahtarları, şirket içi HDFS ve Azure Blob depolama arasında bağlantı kurar. Bu anahtarlar Hadoop kimlik bilgisi sağlayıcısı çerçevesi ile şifreleyerek korunabilir. Şifrelendikten sonra, güvenli bir şekilde saklanabilir ve erişilebilir.

**Kimlik bilgilerini sağlamak için:**

```bash
hadoop credential create fs.azure.account.key.account.blob.core.windows.net -value <storage key> -provider jceks://hdfs@headnode.xx.internal.cloudapp.net/path/to/jceks/file
```

**Yukarıdaki sağlayıcı yolunu core-site.xml'e veya ambari yapılandırmasına özel çekirdek sitesi altında eklemek için:**

```xml
<property>
    <name>hadoop.security.credential.provider.path</name>
        <value>
        jceks://hdfs@headnode.xx.internal.cloudapp.net/path/to/jceks
        </value>
    <description>Path to interrogate for protected credentials.</description>
</property>
```

> [!Note]
> Sağlayıcı yolu özelliği, core-site.xml'de anahtar depolamak yerine distcp komut satırına aşağıdaki gibi eklenebilir:

```bash
hadoop distcp -D hadoop.security.credential.provider.path=jceks://hdfs@headnode.xx.internal.cloudapp.net/path/to/jceks /user/user1/ wasb:<//yourcontainer@youraccount.blob.core.windows.net/>user1
```

## <a name="restrict-azure-storage-data-access-using-sas"></a>SAS kullanarak Azure Depolama veri erişimini kısıtlama

HDInsight varsayılan olarak kümeyle ilişkili Azure Depolama hesaplarındaki verilere tam erişime sahiptir. Blob kapsayıcısı üzerindeki Paylaşılan Erişim İmzaları (SAS), kullanıcılara verilere salt okunur erişim sağlamak gibi verilere erişimi kısıtlamak için kullanılabilir.

### <a name="using-the-sas-token-created-with-python"></a>Python ile oluşturulan SAS belirteci kullanma

1. [SASToken.py](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature/blob/master/Python/SASToken.py) dosyasını açın ve aşağıdaki değerleri değiştirin:

    |Belirteç Özelliği|Açıklama|
    |---|---|
    |policy_name|Oluşturulacak depolanan ilke için kullanılacak ad.|
    |storage_account_name|Depolama hesabınızın adı.|
    |storage_account_key|Depolama hesabının anahtarı.|
    |storage_container_name|Erişimi kısıtlamak istediğiniz depolama hesabındaki kapsayıcı.|
    |example_file_path|Kapsayıcıya yüklenen bir dosyaya giden yol.|

2. SASToken.py dosyası `ContainerPermissions.READ + ContainerPermissions.LIST` izinlerle birlikte gelir ve kullanım durumuna göre ayarlanabilir.

3. Komut dosyasını aşağıdaki gibi çalıştırın:`python SASToken.py`

4. Komut dosyası tamamlandığında aşağıdaki metne benzer SAS belirteci görüntüler:`sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14`

5. Paylaşılan Erişim İmzası içeren bir kapsayıcıya erişimi sınırlamak için Ambari HDFS Configs Advanced Custom core-site Add özelliği altında küme için çekirdek site yapılandırmasına özel bir giriş ekleyin.

6. **Anahtar** ve **Değer** alanları için aşağıdaki değerleri kullanın:

    **Anahtar** `fs.azure.sas.YOURCONTAINER.YOURACCOUNT.blob.core.windows.net` : **Değer**: Python uygulaması tarafından döndürülen SAS KEY from 4 yukarıda.

7. Bu anahtarı ve değeri kaydetmek için **Ekle** düğmesini tıklatın, ardından yapılandırma değişikliklerini kaydetmek için **Kaydet** düğmesini tıklatın. İstendiğinde, değişikliğin açıklamasını ekleyin (örneğin SAS depolama alanı erişimi ekleme) ve sonra **Kaydet'i**tıklatın.

8. Ambari web UI'sinde, soldaki listeden HDFS'yi seçin ve ardından sağdaki Hizmet Eylemleri'nden **Etkilenen Tümünü Yeniden Başlat'ı** seçin. İstendiğinde, **Tümünü Yeniden Başlat'ı Doğrula'yı**seçin.

9. MapReduce2 ve YARN için bu işlemi tekrarlayın.

Azure'da SAS Belirteçleri kullanımı hakkında hatırlanması gereken üç önemli şey vardır:

1. SAS belirteçleri "READ + LIST" izinleriyle oluşturulduğunda, Bu SAS belirteciyle Blob kapsayıcısına erişen kullanıcılar verileri "yazıp silemez" olmayacaktır. Bu SAS belirteci ile Blob kapsayıcıerişen ve bir yazma veya `"This request is not authorized to perform this operation"`silme işlemi deneyin kullanıcılar, gibi bir ileti alacaksınız .

2. SAS belirteçleri izinlerle `READ + LIST + WRITE` oluşturulduğunda (yalnızca kısıtlamak `DELETE` `hadoop fs -put` için), önce `\_COPYING\_` bir dosyaya yazıp sonra dosyayı yeniden adlandırmayı deneyin gibi komutlar. Bu HDFS işlemi `copy+delete` WASB için bir eşler. İzin `DELETE` verilmediğinden, "koymak" başarısız olur. İşlem, `\_COPYING\_` bazı eşzamanlılık denetimi sağlamak amacıyla tasarlanmış bir Hadoop özelliğidir. Şu anda sadece "DELETE" işlemini "YAZ" işlemlerini etkilemeden kısıtlamanın bir yolu yoktur.

3. Ne yazık ki, hadoop kimlik bilgileri sağlayıcısı ve şifre çözme anahtar sağlayıcısı (ShellDecryptionKeyProvider) şu anda SAS belirteçleri ile çalışmıyor ve bu yüzden şu anda görünürlükten korunamaz.

Daha fazla bilgi için, [HDInsight'taki verilere erişimi kısıtlamak için Azure Depolama Paylaşılan Erişim İmzalarını Kullan'a](../hdinsight-storage-sharedaccesssignature-permissions.md)bakın.

## <a name="use-data-encryption-and-replication"></a>Veri şifreleme ve çoğaltma kullanma

Azure Depolama'ya yazılan tüm veriler [Depolama Hizmeti Şifrelemesi (SSE)](../../storage/common/storage-service-encryption.md)kullanılarak otomatik olarak şifrelenir. Azure Depolama hesabındaki veriler yüksek kullanılabilirlik için her zaman çoğaltılır.Bir depolama hesabı oluşturduğunuzda, aşağıdaki çoğaltma seçeneklerinden birini seçebilirsiniz:

- [Yerel olarak yedekli depolama (LRS)](../../storage/common/storage-redundancy-lrs.md)
- [Bölge yedekli depolama (ZRS)](../../storage/common/storage-redundancy-zrs.md)
- [Coğrafi olarak yedekli depolama (GRS)](../../storage/common/storage-redundancy-grs.md)
- [Okuma erişimli coğrafi olarak yedekli depolama (RA-GRS)](../../storage/common/storage-redundancy.md)

Azure Veri Gölü Depolama sıcağa yerel olarak yedekli depolama (LRS) sağlar, ancak kritik verileri, olağanüstü durum kurtarma planının gereksinimlerine uygun bir sıklıkta başka bir bölgedeki başka bir Veri Gölü Depolama hesabına da kopyalamanız gerekir. [ADLCopy,](../../data-lake-store/data-lake-store-copy-data-azure-storage-blob.md) [DistCp,](https://hadoop.apache.org/docs/current/hadoop-distcp/DistCp.html) [Azure PowerShell](../../data-lake-store/data-lake-store-get-started-powershell.md)veya [Azure Veri Fabrikası](../../data-factory/connector-azure-data-lake-store.md)gibi verileri kopyalamak için farklı yöntemler vardır.Ayrıca, yanlışlıkla silinmesini önlemek için Veri Gölü Depolama hesabıiçin erişim ilkelerini zorlamak önerilir.

Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Azure Depolama çoğaltma](../../storage/common/storage-redundancy.md)
- [Azure Veri Gölü Depolama (ADLS) için olağanüstü durum kılavuzu](../../data-lake-store/data-lake-store-disaster-recovery-guidance.md)

## <a name="attach-additional-azure-storage-accounts-to-cluster"></a>Kümeye ek Azure Depolama hesapları ekleme

HDInsight oluşturma işlemi sırasında, varsayılan dosya sistemi olarak bir Azure Depolama hesabı veya Azure Veri Gölü Depolama hesabı seçilir. Bu varsayılan depolama hesabına ek olarak, küme oluşturma işlemi sırasında veya küme oluşturulduktan sonra aynı Azure aboneliğinden veya farklı Azure aboneliklerinden ek depolama hesapları eklenebilir.

Ek depolama hesabı aşağıdaki yollarla bir eklenebilir:
- Ambari HDFS Config Advanced Custom core-site Depolama Hesap Adı ve anahtar hizmetleri yeniden başlatma
- Depolama hesabı adını ve anahtarını geçirerek [Komut Dosyası eylemini](../hdinsight-hadoop-add-storage.md) kullanma

> [!Note]
> Geçerli kullanım durumlarında, Azure depolama alanının sınırları [Azure Desteği'ne](https://azure.microsoft.com/support/faq/)yapılan bir istekle artırılabilir.

Daha fazla bilgi için bkz. [HDInsight’a başka depolama hesapları ekleme](../hdinsight-hadoop-add-storage.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu serinin bir sonraki makalesini okuyun: [Azure HDInsight Hadoop geçişi için şirket içi veri geçişi için en iyi uygulamalar.](apache-hadoop-on-premises-migration-best-practices-data-migration.md)
