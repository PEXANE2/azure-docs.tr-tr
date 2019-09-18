---
title: 'Hata ayıklama Apache Hadoop: Günlükleri görüntüleme ve hata iletilerini yorumlama-Azure HDInsight'
description: PowerShell kullanarak HDInsight yönetirken alacağınız hata iletileri ve kurtarmak için uygulayabileceğiniz adımlar hakkında bilgi edinin.
ms.reviewer: jasonh
author: ashishthaps
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/14/2017
ms.author: ashishth
ms.openlocfilehash: 8ad2bdd0f12abad08515f0314b9c03cc971127cb
ms.sourcegitcommit: ca359c0c2dd7a0229f73ba11a690e3384d198f40
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71059209"
---
# <a name="analyze-apache-hadoop-logs-in-azure-hdinsight"></a>Azure HDInsight 'ta Apache Hadoop günlüklerini analiz etme

Azure HDInsight 'taki her Apache Hadoop kümesi, varsayılan dosya sistemi olarak kullanılan bir Azure depolama hesabına sahiptir. Depolama hesabına varsayılan depolama hesabı denir. Küme, günlüklerini depolamak için varsayılan depolama hesabındaki Azure Tablo depolama ve BLOB depolama alanını kullanır.  Kümenizin varsayılan depolama hesabını bulmak için bkz. [HDInsight 'ta Apache Hadoop kümelerini yönetme](../hdinsight-administer-use-portal-linux.md#find-the-storage-accounts). Günlükler, küme silindikten sonra bile depolama hesabında kalır.

## <a name="logs-written-to-azure-tables"></a>Azure tablolarına yazılan Günlükler

Azure tablolarına yazılan Günlükler, bir HDInsight kümesiyle neler olduğunu gösteren bir öngörü düzeyi sağlar.

Bir HDInsight kümesi oluşturduğunuzda, varsayılan tablo depolamada Linux tabanlı kümeler için altı tablo otomatik olarak oluşturulur:

* hdinsightagentlog
* syslog
* daemonlog
* hadoopservicelog
* ambariserverlog
* ambariagentlog

Tablo dosya adları **u\<clustername > ddmonyyyıyathhmmsssss\<TableName >** .

Bu tablolar aşağıdaki alanları içerir:

* ClusterDnsName
* ComponentName
* EventTimestamp
* Ana bilgisayar
* MALoggingHash
* `Message`
* N
* Ön IO zaman damgası
* Role
* RowIndex
* Kiracı
* TIMESTAMP
* TraceLevel

### <a name="tools-for-accessing-the-logs"></a>Günlüklere erişim araçları
Bu tablolardaki verilere erişmek için kullanabileceğiniz birçok araç vardır:

* Visual Studio
* Azure Depolama Gezgini
* Excel için Power Query

#### <a name="use-power-query-for-excel"></a>Excel için Power Query kullanma
Power Query, [Excel için Microsoft Power Query](https://www.microsoft.com/en-us/download/details.aspx?id=39379)yüklenebilir. Sistem gereksinimleri için indirme sayfasına bakın.

**Hizmet günlüğünü açmak ve analiz etmek için Power Query kullanma**

1. **Microsoft Excel**'i açın.
2. **Power Query** menüsünde **Azure**' a ve ardından **Microsoft Azure Tablo depolama**' ya tıklayın.
   
    ![HDInsight Hadoop Excel PowerQuery Azure Tablo depolama 'yı açma](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-using-excel-power-query-open.png)
3. Depolama hesabı adını, kısa ad veya FQDN girin.
4. Depolama hesabı anahtarını girin. Tabloların bir listesini görürsünüz:
   
    ![Azure Tablo depolamada depolanan HDInsight Hadoop günlükleri](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-table-names.png)
5. **Gezgin** bölmesinde hadoopservicelog tablosuna sağ tıklayın ve **Düzenle**' yi seçin. Dört sütun görürsünüz. İsteğe bağlı olarak, **bölüm anahtarını**, **satır anahtarını**ve **zaman damgası** sütunlarını seçerek ve Şeritteki seçeneklerden **sütunları kaldır** ' a tıklayarak silin.
6. Excel elektronik tablosuna aktarmak istediğiniz sütunları seçmek için Içerik sütunundaki Genişlet simgesine tıklayın. Bu gösterim için TraceLevel ve ComponentName ' i seçtim: Bu, hangi bileşenlere sorun olduğu hakkında bazı temel bilgiler verebilir.
   
    ![HDInsight Hadoop günlükleri sütunları seç Excel](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-using-excel-power-query-filter.png "HDInsight Hadoop günlükleri sütunları seç Excel")
7. Verileri içeri aktarmak için **Tamam** ' ı tıklatın.
8. **TraceLevel**, role ve **ComponentName** sütunlarını seçin ve ardından şeritte **grupla** ' ya tıklayın.
9. Gruplandırma ölçütü iletişim kutusunda **Tamam** ' a tıklayın.
10. \* * Uygula & Kapat * * öğesine tıklayın.

Artık gerektiğinde filtrelemek ve sıralamak için Excel 'i kullanabilirsiniz. Oluşan sorunları gidermek için diğer sütunları (örneğin, Ileti) dahil etmek isteyebilirsiniz, ancak yukarıda açıklanan sütunları seçip gruplandırmak, Hadoop hizmetlerinde neler olduğunu belirten bir resim sunar. Aynı fikir Setuplog ve hadoopinstalllog tablolarına uygulanabilir.

#### <a name="use-visual-studio"></a>Visual Studio'yu kullanma
**Visual Studio kullanma**

1. Visual Studio'yu açın.
2. **Görünüm** menüsünde, **bulut Gezgini**' ne tıklayın. Ya da **CTRL +\, CTRL + X**tuşlarına tıklamanız yeterlidir.
3. **Bulut Gezgini**' nden **kaynak türleri**' ni seçin.  Diğer kullanılabilir seçeneği **kaynak gruplarıdır**.
4. **Depolama hesapları**, kümenizin varsayılan depolama hesabı ve ardından **Tablolar**' ı genişletin.
5. **Hadoopservicelog**çift tıklayın.
6. Bir filtre ekleyin. Örneğin:
   
        TraceLevel eq 'ERROR'
   
    ![HDInsight Hadoop günlükleri sütunları seçme vs](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-visual-studio-filter.png "HDInsight Hadoop günlükleri sütunları seçme vs")
   
    Filtre oluşturma hakkında daha fazla bilgi için bkz. [Tablo Tasarımcısı Için Filtre dizeleri](../../vs-azure-tools-table-designer-construct-filter-strings.md)oluşturma.

## <a name="logs-written-to-azure-blob-storage"></a>Azure Blob depolamaya yazılan Günlükler
Azure tablolarına yazılan Günlükler, bir HDInsight kümesiyle neler olduğunu gösteren bir öngörü düzeyi sağlar. Ancak, bu tablolar, ne zaman ortaya çıkabilecek sorunları ele alırken faydalı olabilecek görev düzeyi günlükleri sağlamaz. Bu sonraki ayrıntı düzeyini sağlamak için HDInsight kümeleri, Temptaton aracılığıyla gönderilen herhangi bir iş için BLOB depolama hesabınıza görev günlükleri yazacak şekilde yapılandırılmıştır. Bu şekilde, bu, kümeye RDP/komut satırı erişimi aracılığıyla gönderilen işleri değil, Microsoft Azure PowerShell cmdlet 'leri veya .NET Iş gönderme API 'Leri kullanılarak gönderilen işlerin ortalamadığını gösterir. 

Günlükleri görüntülemek için bkz. [Linux tabanlı HDInsight 'TA YARN uygulama günlüklerine erişim Apache Hadoop](../hdinsight-hadoop-access-yarn-app-logs-linux.md).


Uygulama günlükleri hakkında daha fazla bilgi için bkz. [Apache Hadoop YARN 'de Kullanıcı günlüğü yönetimini ve erişimi basitleştirme](https://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/).


## <a name="view-cluster-health-and-job-logs"></a>Küme durumunu ve iş günlüklerini görüntüleme
### <a name="access-the-ambari-ui"></a>Ambarı Kullanıcı arabirimine erişin
Azure portal, küme bölmesini açmak için bir HDInsight küme adına tıklayın. Küme bölmesinden **Pano**' ya tıklayın.

![HDInsight başlatma kümesi panosu](./media/apache-hadoop-debug-jobs/hdi-debug-launch-dashboard.png)


### <a name="access-the-yarn-ui"></a>Yarn Kullanıcı arabirimine erişin
Azure portal, küme bölmesini açmak için bir HDInsight küme adına tıklayın. Küme bölmesinden **Pano**' ya tıklayın. İstendiğinde, Küme Yöneticisi kimlik bilgilerini girin. Ambarı ' nda, sol taraftaki hizmetler listesinden **Yarn** ' yi seçin. Görüntülenen sayfada **hızlı bağlantılar**' ı seçin ve ardından etkin baş düğüm girişini ve Kaynak Yöneticisi Kullanıcı arabirimini seçin.

YARN Kullanıcı arabirimini kullanarak şunları yapabilirsiniz:

* **Küme durumunu Al**. Sol bölmeden **küme**' ı genişletin ve **hakkında**' yı tıklatın. Bu durum, ayrılan toplam bellek, kullanılan çekirdekler, küme kaynak yöneticisi, küme sürümü vb. gibi küme durumu ayrıntılarını sunar.
  
    ![HDInsight başlatma kümesi Pano Yarn](./media/apache-hadoop-debug-jobs/hdi-debug-yarn-cluster-state.png "HDInsight başlatma kümesi Pano Yarn")
* **Düğüm durumunu Al**. Sol bölmeden **küme**' ı genişletin ve **düğümler**' e tıklayın. Bu, kümedeki tüm düğümleri, her düğümün HTTP adresini, her bir düğüme ayrılan kaynakları vb. listeler.
* **İş durumunu izleme**. Sol bölmeden **küme**' ı genişletin ve ardından kümedeki tüm işleri listelemek için **uygulamalar** ' a tıklayın. İşlerin belirli bir durumda (örneğin, yeni, gönderilen, çalışan, vb.) görünmesini istiyorsanız, **uygulamalar**altında uygun bağlantıya tıklayın. İşle ilgili daha fazla bilgi edinmek için iş adına tıklayabilirsiniz. Örneğin, çıkış, Günlükler vb. dahil olmak üzere iş hakkında daha fazla bilgi edinebilirsiniz.

### <a name="access-the-hbase-ui"></a>HBase Kullanıcı arabirimine erişme
Azure portal, küme bölmesini açmak için bir HDInsight HBase küme adına tıklayın. Küme bölmesinden **Pano**' ya tıklayın. İstendiğinde, Küme Yöneticisi kimlik bilgilerini girin. Ambarı ' nda, hizmetler listesinden HBase ' i seçin. Sayfanın üst kısmındaki **hızlı bağlantılar** ' ı seçin, etkin Zookeeper düğümü bağlantısına gelin ve HBase Master Kullanıcı arabirimi ' ne tıklayın.

## <a name="hdinsight-error-codes"></a>HDInsight hata kodları
Bu bölümde yer alan hata iletileri, Azure HDInsight 'ta Hadoop kullanıcılarının, Azure PowerShell kullanarak hizmeti yönetirken karşılaşabilecekleri olası hata koşullarını anlamalarına ve bu kullanıcılara alınabilecek adımları dikkate almanıza yardımcı olmak için verilmiştir hatayı kurtarmak için.

Bu hata iletilerinin bazıları HDInsight kümelerini yönetmek için kullanıldığında Azure portal de görülebilir. Ancak karşılaşabileceğiniz diğer hata iletileri, bu bağlamda olası düzeltme eylemleriyle ilgili kısıtlamalar nedeniyle daha ayrıntılı bir şekilde yapılır. Diğer hata iletileri, azaltma 'nın belirgin olduğu bağlamlarda sağlanır. 

### <a id="AtLeastOneSqlMetastoreMustBeProvided"></a>Atleastonessınmetastoremustbesaðlanan
* **Açıklama**: Hive ve Oozie meta veri ayarlarını kullanmak için lütfen en az bir bileşen için Azure SQL veritabanı ayrıntılarını sağlayın.
* **Risk azaltma**: Kullanıcının geçerli bir SQL Azure meta deposu sağlaması ve isteği yeniden denemesi gerekir.  

### <a id="AzureRegionNotSupported"></a>AzureRegionNotSupported
* **Açıklama**: Bölge *Nameofyourregion*içinde oluşturulamadı. Geçerli bir HDInsight bölgesi kullanın ve isteği yeniden deneyin.
* **Risk azaltma**: Müşterinin Şu anda bunları destekleyen küme bölgesini oluşturması gerekir: Güneydoğu Asya, Batı Avrupa, Kuzey Avrupa, Doğu ABD veya Batı ABD.  

### <a id="ClusterContainerRecordNotFound"></a>ClusterContainerRecordNotFound
* **Açıklama**: Sunucu istenen küme kaydını bulamadı.  
* **Risk azaltma**: İşlemi yeniden deneyin.

### <a id="ClusterDnsNameInvalidReservedWord"></a>Clusterdnsnameınvalidreservedword
* **Açıklama**: Küme DNS adı, *kullanıcıadı* geçersiz. Adın, alfasayısal karakterlerle başladığı ve bittiği ve yalnızca '-' özel karakterini içerebildiği için lütfen emin olun  
* **Risk azaltma**: Kümeniz için, alfasayısal karakterlerle başlayan ve biten ve '-' Dash dışında özel karakter içermeyen ve işlemi yeniden denemeden önce geçerli bir DNS adı kullandığınızdan emin olun.

### <a id="ClusterNameUnavailable"></a>ClusterNameUnavailable
* **Açıklama**: *Clustername* adlı küme adı kullanılamıyor. Lütfen başka bir ad seçin.  
* **Risk azaltma**: Kullanıcı benzersiz olan bir clustername belirtmeli ve yok ve yeniden dene. Kullanıcı Portalı kullanıyorsa, oluşturma adımları sırasında bir küme adı zaten kullanılıyorsa Kullanıcı arabirimi onlara bildirim gönderir.

### <a id="ClusterPasswordInvalid"></a>Clusterpasswordgeçersiz
* **Açıklama**: Küme parolası geçersiz. Parola en az 10 karakter uzunluğunda olmalı ve boşluk olmadan en az bir sayı, büyük harf, küçük harf ve özel karakter içermeli ve bunun bir parçası olarak Kullanıcı adını içermemelidir.  
* **Risk azaltma**: Geçerli bir küme parolası sağlayın ve işlemi yeniden deneyin.

### <a id="ClusterUserNameInvalid"></a>Clusterusernamegeçersiz
* **Açıklama**: Küme Kullanıcı adı geçersiz. Lütfen Kullanıcı adının özel karakterler veya boşluklar içermediğinden emin olun.  
* **Risk azaltma**: Geçerli bir küme Kullanıcı adı sağlayın ve işlemi yeniden deneyin.

### <a id="ClusterUserNameInvalidReservedWord"></a>Clusterusernameınvalidreservedword
* **Açıklama**: Küme DNS adı *Yourdnsclustername* geçersiz. Adın, alfasayısal karakterlerle başladığı ve bittiği ve yalnızca '-' özel karakterini içerebildiği için lütfen emin olun  
* **Risk azaltma**: Geçerli bir DNS kümesi Kullanıcı adı girip işlemi yeniden deneyin.

### <a id="ContainerNameMisMatchWithDnsName"></a>ContainerNameMisMatchWithDnsName
* **Açıklama**: URI 'niz içindeki kapsayıcı *adı ve DNS* *adı istek* gövdesinde aynı olmalıdır.  
* **Risk azaltma**: Kapsayıcı adınızın ve DNS adınızın aynı olduğundan emin olun ve işlemi yeniden deneyin.

### <a id="DataNodeDefinitionNotFound"></a>Davtanodedefinitionnotfound
* **Açıklama**: Geçersiz küme yapılandırması. Düğüm boyutunda hiçbir veri düğümü tanımı bulunamıyor.  
* **Risk azaltma**: İşlemi yeniden deneyin.

### <a id="DeploymentDeletionFailure"></a>DeploymentDeletionFailure
* **Açıklama**: Küme için dağıtımın silinmesi başarısız oldu  
* **Risk azaltma**: Silme işlemini yeniden deneyin.

### <a id="DnsMappingNotFound"></a>DnsMappingNotFound
* **Açıklama**: Hizmet yapılandırma hatası. Gerekli DNS eşleme bilgileri bulunamadı.  
* **Risk azaltma**: Kümeyi silin ve yeni bir küme oluşturun.

### <a id="DuplicateClusterContainerRequest"></a>DuplicateClusterContainerRequest
* **Açıklama**: Yinelenen küme kapsayıcısı oluşturma girişimi. *Nameofyourcontainer* için kayıt var, ancak ETags eşleşmiyor.
* **Risk azaltma**: Kapsayıcı için benzersiz bir ad sağlayın ve oluşturma işlemini yeniden deneyin.

### <a id="DuplicateClusterInHostedService"></a>DuplicateClusterInHostedService
* **Açıklama**: Barındırılan hizmet *Nameofyourhostedservice* zaten bir küme içeriyor. Barındırılan bir hizmet birden çok küme içeremez  
* **Risk azaltma**: Kümeyi başka bir barındırılan hizmette barındırın.

### <a id="FailureToUpdateDeploymentStatus"></a>FailureToUpdateDeploymentStatus
* **Açıklama**: Sunucu, küme dağıtımının durumunu güncelleştiremedi.  
* **Risk azaltma**: İşlemi yeniden deneyin. Bu birden çok kez oluşursa, CSS ile iletişim kurun.

### <a id="HdiRestoreClusterAltered"></a>HdiRestoreClusterAltered
* **Açıklama**: Bakım parçası olarak *clustername* adlı küme silindi. Lütfen kümeyi yeniden oluşturun.
* **Risk azaltma**: Kümeyi yeniden oluşturun.

### <a id="HeadNodeConfigNotFound"></a>HeadNodeConfigNotFound
* **Açıklama**: Geçersiz küme yapılandırması. Düğüm boyutlarında gerekli baş düğüm yapılandırması bulunamadı.
* **Risk azaltma**: İşlemi yeniden deneyin.

### <a id="HostedServiceCreationFailure"></a>HostedServiceCreationFailure
* **Açıklama**: Barındırılan hizmet *Nameofyourhostedservice*oluşturulamıyor. Lütfen isteği yeniden deneyin.  
* **Risk azaltma**: İsteği yeniden deneyin.

### <a id="HostedServiceHasProductionDeployment"></a>Hostedservicehasüretim dağıtımı
* **Açıklama**: Barındırılan hizmet *Nameofyourhostedservice* zaten bir üretim dağıtımına sahip. Barındırılan bir hizmet birden fazla üretim dağıtımı içeremez. Farklı bir küme adına sahip isteği yeniden deneyin.
* **Risk azaltma**: Farklı bir küme adı kullanın ve isteği yeniden deneyin.

### <a id="HostedServiceNotFound"></a>HostedServiceNotFound
* **Açıklama**: Küme için barındırılan hizmet *Nameofyourhostedservice* bulunamadı.  
* **Risk azaltma**: Küme hata durumundaysa, silin ve sonra yeniden deneyin.

### <a id="HostedServiceWithNoDeployment"></a>HostedServiceWithNoDeployment
* **Açıklama**: Barındırılan hizmet *Nameofyourhostedservice* 'in ilişkili bir dağıtımı yok.  
* **Risk azaltma**: Küme hata durumundaysa, silin ve sonra yeniden deneyin.

### <a id="InsufficientResourcesCores"></a>InsufficientResourcesCores
* **Açıklama**: SubscriptionID *depolamamadı* , *kümesiz*küme oluşturmak için çekirdekleri içermez. Gerekli: *resourcesrequired*, kullanılabilir: *resourcesavailable*.  
* **Risk azaltma**: Aboneliğinizdeki kaynakları boşaltın veya aboneliğin kullanabildiği kaynakları artırın ve kümeyi yeniden oluşturmayı deneyin.

### <a id="InsufficientResourcesHostedServices"></a>InsufficientResourcesHostedServices
* **Açıklama**: Abonelik KIMLIĞI *,* bir Hostedservice 'in *clustername*kümesini oluşturmak için yeni bir hostedservice kotasına sahip değildir.  
* **Risk azaltma**: Aboneliğinizdeki kaynakları boşaltın veya aboneliğin kullanabildiği kaynakları artırın ve kümeyi yeniden oluşturmayı deneyin.

### <a id="InternalErrorRetryRequest"></a>Internalerrorretryrequest
* **Açıklama**: Sunucu bir iç hatayla karşılaştı. Lütfen isteği yeniden deneyin.  
* **Risk azaltma**: İsteği yeniden deneyin.

### <a id="InvalidAzureStorageLocation"></a>Invalidazurestoraygelocation
* **Açıklama**: Azure depolama konumu *Dataregionname* geçerli bir konum değil. Bölgenin doğru olduğundan emin olun ve isteği yeniden deneyin.
* **Risk azaltma**: HDInsight 'ı destekleyen bir depolama konumu seçin, kümenizin birlikte bulunduğundan emin olun ve işlemi yeniden deneyin.

### <a id="InvalidNodeSizeForDataNode"></a>Invalidnodesizefordadtanode
* **Açıklama**: Veri düğümleri için VM boyutu geçersiz. Tüm veri düğümlerinde yalnızca ' büyük VM ' boyutu desteklenir.  
* **Risk azaltma**: Veri düğümü için desteklenen düğüm boyutunu belirtin ve işlemi yeniden deneyin.

### <a id="InvalidNodeSizeForHeadNode"></a>Invalidnodesizeforheadnode
* **Açıklama**: Baş düğüm için VM boyutu geçersiz. Baş düğüm için yalnızca ' Extrabüyük VM ' boyutu desteklenir.  
* **Risk azaltma**: Baş düğüm için desteklenen düğüm boyutunu belirtin ve işlemi yeniden deneyin

### <a id="InvalidRightsForDeploymentDeletion"></a>Invalidrightsfordeploymentsilinmeye
* **Açıklama**: Kullanılan abonelik *kimliği,* *kümesiz*küme için silme işlemini yürütmek için yeterli izinlere sahip değil.  
* **Risk azaltma**: Küme hata durumundaysa, bırakın ve sonra yeniden deneyin.  

### <a id="InvalidStorageAccountBlobContainerName"></a>InvalidStorageAccountBlobContainerName
* **Açıklama**: Dış depolama hesabı blobu *kapsayıcı adı geçersiz* . Adın bir harfle başladığı ve yalnızca küçük harf, rakam ve tire içerdiğinden emin olun.  
* **Risk azaltma**: Geçerli bir depolama hesabı blob kapsayıcısı adı belirtip işlemi yeniden deneyin.

### <a id="InvalidStorageAccountConfigurationSecretKey"></a>Invalidstorageaccountconfigurationsecretkey
* **Açıklama**: Dış depolama hesabının yapılandırması, gizli anahtar ayrıntılarının ayarlanması için *storageAccountName* gereklidir.  
* **Risk azaltma**: Depolama hesabı için geçerli bir gizli anahtar belirtip işlemi yeniden deneyin.

### <a id="InvalidVersionHeaderFormat"></a>Invalidversionheaderformat
* **Açıklama**: Sürüm üst bilgisi *etkisürümno* geçerli bir YYYY-AA-GG biçiminde değil.  
* **Risk azaltma**: Sürüm üst bilgisi için geçerli bir biçim belirtip isteği yeniden deneyin.

### <a id="MoreThanOneHeadNode"></a>Daha fazla yer alan
* **Açıklama**: Geçersiz küme yapılandırması. Birden fazla baş düğüm yapılandırması bulundu.  
* **Risk azaltma**: Yapılandırmayı yalnızca bir baş düğüm belirtilmiş olacak şekilde düzenleyin.

### <a id="OperationTimedOutRetryRequest"></a>OperationTimedOutRetryRequest
* **Açıklama**: İşlem, izin verilen süre veya en fazla yeniden deneme denemesi içinde tamamlanamadı. Lütfen isteği yeniden deneyin.  
* **Risk azaltma**: İsteği yeniden deneyin.

### <a id="ParameterNullOrEmpty"></a>ParameterNullOrEmpty
* **Açıklama**: *Etkiparametresi* null veya boş olamaz.  
* **Risk azaltma**: Parametre için geçerli bir değer belirtin.

### <a id="PreClusterCreationValidationFailure"></a>Prelustercreationvalidationfailure
* **Açıklama**: Küme oluşturma isteği girişlerinin bir veya daha fazlası geçerli değil. Giriş değerlerinin doğru olduğundan emin olun ve isteği yeniden deneyin.  
* **Risk azaltma**: Giriş değerlerinin doğru olduğundan emin olun ve isteği yeniden deneyin.

### <a id="RegionCapabilityNotAvailable"></a>RegionCapabilityNotAvailable
* **Açıklama**: Bölge özelliği, *Yourregionname* ve abonelik ID *yoursubscriptionıd*için kullanılamaz.  
* **Risk azaltma**: HDInsight kümelerini destekleyen bir bölge belirtin. Genel olarak desteklenen bölgeler şunlardır: Güneydoğu Asya, Batı Avrupa, Kuzey Avrupa, Doğu ABD veya Batı ABD.

### <a id="StorageAccountNotColocated"></a>Storageaccountnotbirlikte
* **Açıklama**: *Yourstorageaccountname* depolama hesabı *currentregionname*bölgesidir. Bu, kendi *Clusterregionname*küme bölgesiyle aynı olmalıdır.  
* **Risk azaltma**: Kümenizin bulunduğu bölgede bir depolama hesabı belirtin ya da verileriniz zaten depolama hesabında bulunuyorsa, mevcut depolama hesabıyla aynı bölgede yeni bir küme oluşturun. Portalı kullanıyorsanız, Kullanıcı arabirimi bu sorunu önceden bilgilendirir.

### <a id="SubscriptionIdNotActive"></a>Subscriptionıdnotactive
* **Açıklama**: Belirtilen abonelik KIMLIĞI, *SubscriptionID* etkin değil.  
* **Risk azaltma**: Aboneliğinizi yeniden etkinleştirin veya yeni bir geçerli abonelik alın.

### <a id="SubscriptionIdNotFound"></a>Subscriptionıdnotfound
* **Açıklama**: *ABONELIK kimliği bulunamadı* .  
* **Risk azaltma**: Abonelik KIMLIĞINIZIN geçerli olup olmadığını denetleyin ve işlemi yeniden deneyin.

### <a id="UnableToResolveDNS"></a>UnableToResolveDNS
* **Açıklama**: DNS *Yourdnsurl 'si*çözümlenemiyor. Lütfen blob uç noktasının tam URL 'sinin sağlandığından emin olun.  
* **Risk azaltma**: Geçerli bir blob URL 'SI sağlayın. URL, *http://* ile başlayıp *. com*ile biten dahil olmak üzere tam olarak geçerli olmalıdır.

### <a id="UnableToVerifyLocationOfResource"></a>UnableToVerifyLocationOfResource
* **Açıklama**: *Dnsurl 'sinin*kaynak konumu doğrulanamıyor. Lütfen blob uç noktasının tam URL 'sinin sağlandığından emin olun.  
* **Risk azaltma**: Geçerli bir blob URL 'SI sağlayın. URL, *http://* ile başlayıp *. com*ile biten dahil olmak üzere tam olarak geçerli olmalıdır.

### <a id="VersionCapabilityNotAvailable"></a>VersionCapabilityNotAvailable
* **Açıklama**: Sürüm özelliği, sürüm tarafından *Belirtilmediedversion* ve *abonelik kimliği için*kullanılabilir değil.  
* **Risk azaltma**: Kullanılabilir bir sürüm seçin ve işlemi yeniden deneyin.

### <a id="VersionNotSupported"></a>VersionNotSupported
* **Açıklama**: Version *Belirtilmediversion* desteklenmiyor.
* **Risk azaltma**: Desteklenen bir sürüm seçin ve işlemi yeniden deneyin.

### <a id="VersionNotSupportedInRegion"></a>VersionNotSupportedInRegion
* **Açıklama**: Version *belirtilme* sürümü, *belirtilmedii*Azure bölgesi 'nde kullanılamaz.  
* **Risk azaltma**: Belirtilen bölgede desteklenen bir sürüm seçin ve işlemi yeniden deneyin.

### <a id="WasbAccountConfigNotFound"></a>' Te Baccountconfignotfound
* **Açıklama**: Geçersiz küme yapılandırması. Dış hesaplarda gerekli olan% s hesap yapılandırması bulunamadı.  
* **Risk azaltma**: Hesabın var olduğunu ve yapılandırmada düzgün bir şekilde belirtildiğinden emin olun ve işlemi yeniden deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Linux tabanlı HDInsight 'ta Apache Hadoop Hizmetleri için yığın dökümlerini etkinleştirme](../hdinsight-hadoop-collect-debug-heap-dump-linux.md)
* [HDInsight kümelerini Apache Ambari Web arabiriminden yönetme](../hdinsight-hadoop-manage-ambari.md)
