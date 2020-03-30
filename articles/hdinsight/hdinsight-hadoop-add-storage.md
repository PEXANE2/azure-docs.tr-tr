---
title: HDInsight'a ek Azure Depolama hesapları ekleme
description: Varolan bir HDInsight kümesine nasıl ek Azure Depolama hesapları ekleyeceğinizi öğrenin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/21/2020
ms.openlocfilehash: 87eb04b7323186175195babf6a602fa12d25176f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78206716"
---
# <a name="add-additional-storage-accounts-to-hdinsight"></a>HDInsight'a ek depolama alanı hesapları ekleme

HDInsight'a ek Azure Depolama *hesapları* eklemek için komut dosyası eylemlerini nasıl kullanacağınızı öğrenin. Bu belgedeki adımlar, varolan bir HDInsight kümesine bir depolama *hesabı* ekler. Bu makale, depolama *hesapları* (varsayılan küme depolama hesabı için değil) için geçerlidir ve [Azure Veri Gölü Depolama Gen1](hdinsight-hadoop-use-data-lake-store.md) ve Azure Veri [Gölü Depolama Gen2](hdinsight-hadoop-use-data-lake-storage-gen2.md)gibi ek depolama için geçerli değildir.

> [!IMPORTANT]  
> Bu belgedeki bilgiler, oluşturulduktan sonra kümeye ek depolama hesabı(lar) eklemekle ilgilidir. Küme oluşturma sırasında depolama hesapları ekleme hakkında bilgi [için](hdinsight-hadoop-provision-linux-clusters.md)bkz.

## <a name="prerequisites"></a>Ön koşullar

* HDInsight'ta hadoop kümesi. [Linux'ta HDInsight ile başlayın](./hadoop/apache-hadoop-linux-tutorial-get-started.md)bakın.
* Depolama hesabı adı ve anahtarı. Bkz. [Depolama hesabı erişim anahtarlarını yönet.](../storage/common/storage-account-keys-manage.md)
* PowerShell kullanıyorsanız, AZ modülügerekir.  Bkz. [Azure PowerShell'e Genel Bakış](https://docs.microsoft.com/powershell/azure/overview).

## <a name="how-it-works"></a>Nasıl çalışır?

İşlem sırasında komut dosyası aşağıdaki eylemleri gerçekleştirir:

* Depolama hesabı küme için çekirdek site.xml yapılandırmasında zaten varsa, komut dosyası çıkar ve başka işlem yapılmaz.

* Depolama hesabının var olduğunu ve anahtar kullanılarak erişilebileni doğrular.

* Küme kimlik bürünme sini kullanarak anahtarı şifreler.

* Depolama hesabını çekirdek site.xml dosyasına ekler.

* Durur ve [Apache Oozie](https://oozie.apache.org/)yeniden başlatın , [Apache Hadoop İplik](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html), [Apache Hadoop MapReduce2](https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html), ve [Apache Hadoop HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsUserGuide.html) hizmetleri. Bu hizmetlerin durdurulması ve başlatılması, yeni depolama hesabını kullanmalarına olanak tanır.

> [!WARNING]  
> HDInsight kümesinden farklı bir konumda bir depolama hesabı nın kullanılması desteklenmez.

## <a name="add-storage-account"></a>Depolama hesabı ekleme

Değişiklikleri aşağıdaki hususlarla uygulamak için [Komut Dosyası Eylemi'ni](hdinsight-hadoop-customize-cluster-linux.md#script-action-to-a-running-cluster) kullanın:

|Özellik | Değer |
|---|---|
|Bash script URI|`https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh`|
|Düğüm türü(ler)|Head|
|Parametreler|`ACCOUNTNAME``ACCOUNTKEY` `-p` bağlı)|

* `ACCOUNTNAME`HDInsight kümesine eklenecek depolama hesabının adıdır.
* `ACCOUNTKEY`için `ACCOUNTNAME`erişim anahtarıdır.
* `-p` isteğe bağlıdır. Belirtilmişse, anahtar şifrelenmez ve core-site.xml dosyasında düz metin olarak depolanır.

## <a name="verification"></a>Doğrulama

Azure portalında HDInsight kümesini görüntülerken, __Özellikler__ altında __Depolama Hesapları__ girişini seçmek, bu komut dosyası eylemi yle eklenen depolama hesaplarını görüntülemez. Azure PowerShell ve Azure CLI ek depolama hesabını da görüntülemez. Komut dosyası yalnızca küme nin yapılandırmasını `core-site.xml` değiştirir, çünkü depolama bilgileri görüntülenmez. Bu bilgiler, Azure yönetim API'lerini kullanarak küme bilgilerini alırken kullanılmaz.

Ek depolama kullanımını doğrulamak için aşağıda gösterilen yöntemlerden biri:

### <a name="powershell"></a>PowerShell

Komut dosyası, verilen kümeyle ilişkili Depolama Hesabı adını(lar) döndürecektir. Gerçek `CLUSTERNAME` küme adı ile değiştirin ve sonra komut dosyasını çalıştırın.

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

### <a name="apache-ambari"></a>Apaçi Ambari

1. Bir web tarayıcısından, `https://CLUSTERNAME.azurehdinsight.net`kümenizin adı nerede' `CLUSTERNAME` ye gidin.

1. **HDFS** > **Configs** > **Gelişmiş** > Özel çekirdek**sitesine**gidin.

1. Ile başlayan tuşları `fs.azure.account.key`gözlemleyin. Hesap adı, bu örnek resimde görüldüğü gibi anahtarın bir parçası olacaktır:

   ![Apache Ambari aracılığıyla doğrulama](./media/hdinsight-hadoop-add-storage/apache-ambari-verification.png)

## <a name="remove-storage-account"></a>Depolama hesabını kaldırma

1. Bir web tarayıcısından, `https://CLUSTERNAME.azurehdinsight.net`kümenizin adı nerede' `CLUSTERNAME` ye gidin.

1. **HDFS** > **Configs** > **Gelişmiş** > Özel çekirdek**sitesine**gidin.

1. Aşağıdaki tuşları kaldırın:
    * `fs.azure.account.key.<STORAGE_ACCOUNT_NAME>.blob.core.windows.net`
    * `fs.azure.account.keyprovider.<STORAGE_ACCOUNT_NAME>.blob.core.windows.net`

Bu tuşları kaldırıp yapılandırmayı kurtardıktan sonra Oozie, Yarn, MapReduce2, HDFS ve Hive'ı tek tek yeniden başlatmanız gerekir.

## <a name="known-issues"></a>Bilinen sorunlar

### <a name="storage-firewall"></a>Depolama güvenlik duvarı

**Seçili ağlardaki**Güvenlik Duvarları **ve sanal ağlar** kısıtlamalarıyla depolama hesabınızı güvenli hale getirmeyi seçerseniz, HDInsight'ın depolama hesabınıza erişebilmesi için **güvenilen Microsoft hizmetlerine izin verme** istisnasını etkinleştirdiğinden emin olun.

### <a name="unable-to-access-storage-after-changing-key"></a>Anahtarı değiştirdikten sonra depolama alanına erişilemiyor

Bir depolama hesabının anahtarını değiştirirseniz, HDInsight artık depolama hesabına erişemez. HDInsight küme için çekirdek site.xml anahtar önbelleğe alınmış bir kopyasını kullanır. Önbelleğe alınmış bu kopya, yeni anahtarla eşleşecek şekilde güncelleştirilmelidir.

Komut dosyası eylemini yeniden çalıştırmak, komut dosyası depolama hesabı için bir girişin zaten var olup olmadığını denetlemek için anahtarı __güncelleştirmez.__ Bir giriş zaten varsa, herhangi bir değişiklik yapmaz.

Bu sorunu aşmak için:  
1. Depolama hesabını kaldırın.
1. Depolama hesabı ekleyin.

> [!IMPORTANT]  
> Kümeye bağlı birincil depolama hesabının depolama anahtarının döndürülmesi desteklenmez.

### <a name="poor-performance"></a>Düşük performans

Depolama hesabı HDInsight kümesinden farklı bir bölgedeyse, düşük performansla karşılaşabilirsiniz. Farklı bir bölgedeki verilere erişmek, ağ trafiğini bölgesel Azure veri merkezinin dışına ve genel internet üzerinden gönderir ve bu da gecikmeye neden olabilir.

### <a name="additional-charges"></a>Ek ücretler

Depolama hesabı HDInsight kümesinden farklı bir bölgedeyse, Azure faturalandırmanızda ek çıkış ücretleri fark edebilirsiniz. Veriler bölgesel bir veri merkezinden ayrıldığında çıkış ücreti uygulanır. Bu ücret, trafik farklı bir bölgedeki başka bir Azure veri merkezine yönelik olsa bile uygulanır.

## <a name="next-steps"></a>Sonraki adımlar

Varolan bir HDInsight kümesine nasıl ek depolama hesapları ekleyeceğinizöğrenilir. Komut dosyası eylemleri hakkında daha fazla bilgi için, [komut dosyası eylemini kullanarak Linux tabanlı HDInsight kümelerini özelleştir'e](hdinsight-hadoop-customize-cluster-linux.md) bakın
