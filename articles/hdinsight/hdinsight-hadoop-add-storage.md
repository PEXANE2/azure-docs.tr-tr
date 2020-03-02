---
title: HDInsight 'a ek Azure Storage hesapları ekleme
description: Mevcut bir HDInsight kümesine daha fazla Azure depolama hesabı eklemeyi öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/21/2020
ms.openlocfilehash: 87eb04b7323186175195babf6a602fa12d25176f
ms.sourcegitcommit: 1fa2bf6d3d91d9eaff4d083015e2175984c686da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/01/2020
ms.locfileid: "78206716"
---
# <a name="add-additional-storage-accounts-to-hdinsight"></a>HDInsight 'a ek depolama hesapları ekleme

HDInsight 'a ek Azure depolama *hesapları* eklemek için betik eylemlerinin nasıl kullanılacağını öğrenin. Bu belgedeki adımlar, var olan bir HDInsight kümesine bir depolama *hesabı* ekler. Bu makale, depolama *hesapları* (varsayılan küme depolama hesabı değil) için geçerlidir ve [Azure Data Lake Storage 1.](hdinsight-hadoop-use-data-lake-store.md) ve [Azure Data Lake Storage 2.](hdinsight-hadoop-use-data-lake-storage-gen2.md)gibi ek depolama alanı değildir.

> [!IMPORTANT]  
> Bu belgedeki bilgiler, bir kümeye oluşturulduktan sonra ek depolama hesapları ekleme hakkında bilgi içerir. Küme oluşturma sırasında depolama hesapları ekleme hakkında daha fazla bilgi için bkz. [HDInsight 'ta Apache Hadoop, Apache Spark, Apache Kafka ve daha fazlası ile kümeleri ayarlama](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="prerequisites"></a>Önkoşullar

* HDInsight üzerinde bir Hadoop kümesi. Bkz. [Linux 'Ta HDInsight kullanmaya başlama](./hadoop/apache-hadoop-linux-tutorial-get-started.md).
* Depolama hesabı adı ve anahtarı. Bkz. [depolama hesabı erişim anahtarlarını yönetme](../storage/common/storage-account-keys-manage.md).
* PowerShell kullanıyorsanız AZ Module gerekecektir.  Bkz. [Azure PowerShell genel bakış](https://docs.microsoft.com/powershell/azure/overview).

## <a name="how-it-works"></a>Nasıl çalışır?

İşlem sırasında betik aşağıdaki eylemleri gerçekleştirir:

* Depolama hesabı, küme için Core-site. xml yapılandırmasında zaten varsa, komut dosyası çıkar ve başka hiçbir eylem gerçekleştirilmez.

* Depolama hesabının var olduğunu ve anahtar kullanılarak erişilebilir olduğunu doğrular.

* Küme kimlik bilgilerini kullanarak anahtarı şifreler.

* Core-site. xml dosyasına depolama hesabı ekler.

* [Apache Oozie](https://oozie.apache.org/), [Apache Hadoop yarn](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html), [Apache Hadoop MapReduce2](https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html)ve [Apache Hadoop](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsUserGuide.html) mı hizmetlerini durdurup yeniden başlatır. Bu hizmetleri durdurup başlatmak, yeni depolama hesabını kullanmalarına izin verir.

> [!WARNING]  
> HDInsight kümesinden farklı bir konumda depolama hesabı kullanılması desteklenmez.

## <a name="add-storage-account"></a>Depolama hesabı ekleme

Değişiklikleri aşağıdaki noktalara uygulamak için [betik eylemi](hdinsight-hadoop-customize-cluster-linux.md#script-action-to-a-running-cluster) kullanın:

|Özellik | Değer |
|---|---|
|Bash betiği URI 'SI|`https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh`|
|Düğüm türleri|Head|
|Parametreler|`ACCOUNTNAME` `ACCOUNTKEY` `-p` (isteğe bağlı)|

* `ACCOUNTNAME`, HDInsight kümesine eklenecek depolama hesabının adıdır.
* `ACCOUNTKEY`, `ACCOUNTNAME`için erişim anahtarıdır.
* `-p` isteğe bağlıdır. Belirtilmişse, anahtar şifrelenmez ve Core-site. xml dosyasında düz metin olarak depolanır.

## <a name="verification"></a>Doğrulama

Azure portal HDInsight kümesi görüntülenirken, __Özellikler__ altında __depolama hesapları__ girdisi seçildiğinde bu betik eylemiyle eklenen depolama hesapları gösterilmez. Azure PowerShell ve Azure CLı ek depolama hesabını görüntülemez. Betik yalnızca kümenin `core-site.xml` yapılandırmasını değiştirdiği için depolama bilgileri gösterilmez. Bu bilgiler, Azure Yönetim API 'Leri kullanılarak küme bilgileri alınırken kullanılmaz.

Ek depolama alanını doğrulamak için aşağıda gösterilen yöntemlerden birini kullanın:

### <a name="powershell"></a>PowerShell

Betik, belirtilen kümeyle ilişkili depolama hesabı adlarını döndürür. `CLUSTERNAME` gerçek küme adıyla değiştirin ve ardından betiği çalıştırın.

```powershell
# Update values
$clusterName = "CLUSTERNAME"

$creds = Get-Credential -UserName "admin" -Message "Enter the cluster login credentials"

$clusterName = $clusterName.ToLower();

# getting service_config_version
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_service_config_versions/HDFS" `
    -Credential $creds -UseBasicParsing
$respObj = ConvertFrom-Json $resp.Content

$configVersion=$respObj.Clusters.desired_service_config_versions.HDFS.service_config_version

$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=$configVersion" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content

# extract account names
$value = ($respObj.items.configurations | Where type -EQ "core-site").properties | Get-Member -membertype properties | Where Name -Like "fs.azure.account.key.*"
foreach ($name in $value ) { $name.Name.Split(".")[4]}
```

### <a name="apache-ambari"></a>Apache ambarı

1. Bir Web tarayıcısından `https://CLUSTERNAME.azurehdinsight.net`' a gidin; burada `CLUSTERNAME` kümenizin adıdır.

1. **Gelişmiş** > **özel çekirdek sitesine** >  > **config** 'ler ' **e gidin.**

1. `fs.azure.account.key`ile başlayan anahtarları gözlemleyin. Hesap adı bu örnek görüntüde görüldüğü gibi anahtarın bir parçası olacaktır:

   ![Apache ambarı aracılığıyla doğrulama](./media/hdinsight-hadoop-add-storage/apache-ambari-verification.png)

## <a name="remove-storage-account"></a>Depolama hesabını kaldır

1. Bir Web tarayıcısından `https://CLUSTERNAME.azurehdinsight.net`' a gidin; burada `CLUSTERNAME` kümenizin adıdır.

1. **Gelişmiş** > **özel çekirdek sitesine** >  > **config** 'ler ' **e gidin.**

1. Aşağıdaki anahtarları kaldırın:
    * `fs.azure.account.key.<STORAGE_ACCOUNT_NAME>.blob.core.windows.net`
    * `fs.azure.account.keyprovider.<STORAGE_ACCOUNT_NAME>.blob.core.windows.net`

Bu anahtarları kaldırdıktan ve yapılandırmayı kaydettikten sonra Oozie, Yarn, MapReduce2,, ve Hive tek tek yeniden başlatmanız gerekir.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="storage-firewall"></a>Depolama güvenlik duvarı

Depolama hesabınızı **Seçili ağlardaki** **güvenlik duvarları ve sanal ağlar** kısıtlamalarına göre güvenli hale getirmek isterseniz, HDInsight 'ın depolama hesabınıza erişebilmesi Için **güvenilir Microsoft hizmetlerine izin ver...** özel durumunu etkinleştirdiğinizden emin olun.

### <a name="unable-to-access-storage-after-changing-key"></a>Anahtar değiştirildikten sonra depolamaya erişilemiyor

Bir depolama hesabı için anahtarı değiştirirseniz, HDInsight artık depolama hesabına erişemez. HDInsight, küme için Core-site. xml dosyasında önbelleğe alınmış anahtarın bir kopyasını kullanır. Bu önbelleğe alınan kopyanın yeni anahtarla eşleşecek şekilde güncellenmesi gerekir.

Betik eylemi yeniden çalıştırıldığında anahtar, depolama hesabı için bir girişin zaten mevcut olup olmadığını kontrol eder. Zaten bir giriş varsa, hiçbir değişiklik yapmaz.

Bu sorunu geçici olarak çözmek için:  
1. Depolama hesabını kaldırın.
1. Depolama hesabını ekleyin.

> [!IMPORTANT]  
> Bir kümeye bağlı birincil depolama hesabı için depolama anahtarı döndürülmesi desteklenmez.

### <a name="poor-performance"></a>Düşük performans

Depolama hesabı HDInsight kümesinden farklı bir bölgedeyse, düşük performansla karşılaşabilirsiniz. Farklı bir bölgedeki verilere erişmek, bölgesel Azure veri merkezi dışında ve genel İnternet üzerinden ağ trafiği gönderir ve bu da gecikme süresi ortaya çıkarabilir.

### <a name="additional-charges"></a>Ek ücretler

Depolama hesabı HDInsight kümesinden farklı bir bölgedeyse Azure faturamda ek çıkış ücretleri görebilirsiniz. Veriler bölgesel bir veri merkezinden ayrıldığında çıkış ücreti uygulanır. Bu ücret, trafik farklı bir bölgedeki başka bir Azure veri merkezine gidiyor olsa bile uygulanır.

## <a name="next-steps"></a>Sonraki adımlar

Var olan bir HDInsight kümesine daha fazla depolama hesabı eklemeyi öğrendiniz. Betik eylemleri hakkında daha fazla bilgi için bkz. [betik eylemi kullanarak Linux tabanlı HDInsight kümelerini özelleştirme](hdinsight-hadoop-customize-cluster-linux.md)
