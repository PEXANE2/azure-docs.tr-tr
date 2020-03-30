---
title: HDInsight kümesi için günlükleri yönetme - Azure HDInsight
description: HDInsight etkinlik günlüğü dosyalarının türlerini, boyutlarını ve bekletme ilkelerini belirleyin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 02/05/2020
ms.openlocfilehash: 8c3cbf4c18b32a94abfe95e77be768020b44fda6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79272311"
---
# <a name="manage-logs-for-an-hdinsight-cluster"></a>HDInsight kümesi için günlükleri yönetme

HDInsight kümesi çeşitli günlük dosyaları üretir. Örneğin, Apache Hadoop ve Apache Spark gibi ilgili hizmetler ayrıntılı iş yürütme günlükleri üretir. Günlük dosya yönetimi, sağlıklı bir HDInsight kümesini korumanın bir parçasıdır. Günlük arşivleme için yasal gereklilikler de olabilir.  Günlük dosyalarının sayısı ve boyutu nedeniyle, günlük depolama ve arşivleme en iyi duruma getirmek hizmet maliyet yönetimine yardımcı olur.

HDInsight küme günlüklerinin yönetilmesi, küme ortamının tüm yönleri hakkında bilgi saklamayı içerir. Bu bilgiler, ilişkili tüm Azure Hizmet günlüklerini, küme yapılandırmayı, iş yürütme bilgilerini, hata durumlarını ve gerektiğinde diğer verileri içerir.

HDInsight günlük yönetimindeki tipik adımlar şunlardır:

* Adım 1: Günlük tutma ilkelerini belirleme
* Adım 2: Küme hizmet sürümleri yapılandırma günlüklerini yönetme
* Adım 3: Küme iş yürütme günlüğü dosyalarını yönetme
* Adım 4: Tahmin günlüğü toplu depolama boyutları ve maliyetleri
* Adım 5: Günlük arşivi ilkelerini ve işlemlerini belirleme

## <a name="step-1-determine-log-retention-policies"></a>Adım 1: Günlük tutma ilkelerini belirleme

HDInsight küme günlüğü yönetim stratejisi oluşturmanın ilk adımı, iş senaryoları ve iş yürütme geçmişi depolama gereksinimleri hakkında bilgi toplamaktır.

### <a name="cluster-details"></a>Küme ayrıntıları

Aşağıdaki küme ayrıntıları, günlük yönetimi stratejinizde bilgi toplamaya yardımcı olur. Bu bilgileri belirli bir Azure hesabında oluşturduğunuz tüm HDInsight kümelerinden toplayın.

* Küme adı
* Küme bölgesi ve Azure kullanılabilirlik bölgesi
* Son durum değişikliğinin ayrıntılarını içeren küme durumu
* Ana, çekirdek ve görev düğümleri için belirtilen HDInsight örneklerinin türü ve sayısı

Bu üst düzey bilgilerin çoğunu Azure portalını kullanarak alabilirsiniz.  Alternatif olarak, HDInsight kümeniz hakkında bilgi almak için [Azure CLI'yi](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) kullanabilirsiniz:

```azurecli
az hdinsight list --resource-group <ResourceGroup>
az hdinsight show --resource-group <ResourceGroup> --name <ClusterName>
```

Bu bilgileri görüntülemek için PowerShell'i de kullanabilirsiniz.  Daha fazla bilgi için [Azure PowerShell'i kullanarak HDInsight'taki Apache Manage Hadoop kümelerine](hdinsight-administer-use-powershell.md)bakın.

### <a name="understand-the-workloads-running-on-your-clusters"></a>Kümelerinizde çalışan iş yüklerini anlama

Her tür için uygun günlük günlüğü stratejileri tasarlamak için HDInsight kümenizde çalışan iş yükü türlerini anlamak önemlidir.

* İş yükleri deneysel (geliştirme veya test gibi) veya üretim kalitesi mi?
* Üretim kalitesindeki iş yükleri normalde ne sıklıkta çalışır?
* İş yüklerinden herhangi biri kaynak yoğun ve/veya uzun süreli mi?
* İş yüklerinden herhangi biri, birden çok günlük türünün üretildiği karmaşık bir Hadoop hizmetleri kümesi kullanıyor mu?
* İş yüklerinden herhangi birinin düzenleyici yürütme soyu gereksinimleriyle ilişkili var mı?

### <a name="example-log-retention-patterns-and-practices"></a>Örnek günlük bekletme desenleri ve uygulamaları

* Her günlük girişine bir tanımlayıcı ekleyerek veya diğer teknikler aracılığıyla veri soyu izlemeyi sürdürmeyi düşünün. Bu, verilerin ve işlemin özgün kaynağını izlemenize ve tutarlılığını ve geçerliliğini anlamak için verileri her aşamada izlemenize olanak tanır.

* Kümeden veya birden fazla kümeden günlükleri nasıl toplayabileceğinizi ve bunları denetim, izleme, planlama ve uyarı gibi amaçlarla nasıl harmanladığınızı düşünün. Günlük dosyalarına düzenli olarak erişmek ve indirmek için özel bir çözüm kullanabilir ve pano ekranı sağlamak için bunları birleştirebilir ve çözümleyebilirsiniz. Güvenlik veya hata algılama için uyarı için ek özellikler de ekleyebilirsiniz. Bu yardımcı programları PowerShell, HDInsight SDK'ları veya Azure klasik dağıtım modeline erişen kodu kullanarak oluşturabilirsiniz.

* Bir izleme çözüm veya hizmet yararlı bir yarar olup olmayacağını düşünün. Microsoft System Center bir [HDInsight yönetim paketi](https://www.microsoft.com/download/details.aspx?id=42521)sağlar. Günlükleri toplamak ve merkezileştirmek için Apache Chukwa ve Ganglia gibi üçüncü taraf araçlarını da kullanabilirsiniz. Birçok şirket Hadoop tabanlı büyük veri çözümlerini izlemek için hizmetler sunar, örneğin: Centerity, Compuware APM, Sematext SPM ve Zettaset Orchestrator.

## <a name="step-2-manage-cluster-service-versions-and-view-logs"></a>Adım 2: Küme hizmet sürümlerini yönetme ve günlükleri görüntüleme

Tipik bir HDInsight kümesi çeşitli hizmetler ve açık kaynak yazılım paketleri (Apache HBase, Apache Spark ve benzeri gibi) kullanır. Biyoinformatik gibi bazı iş yükleri için, iş yürütme günlüklerine ek olarak hizmet yapılandırma günlüğü geçmişini korumanız gerekebilir.

### <a name="view-cluster-configuration-settings-with-the-ambari-ui"></a>Ambari UI ile küme yapılandırma ayarlarını görüntüleme

Apache Ambari, bir web Kullanıcı Arabirimi ve REST API sağlayarak bir HDInsight kümesinin yönetimini, yapılandırmasını ve izlenmesini kolaylaştırır. Ambari, Linux tabanlı HDInsight kümelerine dahildir. Küme **Panoları** bağlantı sayfasını açmak için Azure portalı HDInsight sayfasındaki **Küme** Panosu bölmesini seçin.  Ardından, Ambari UI'yi açmak için **HDInsight küme panosu bölmesini** seçin.  Küme giriş kimlik bilgileriniz istenir.

Hizmet görünümlerinin bir listesini açmak için HDInsight için Azure portalı sayfasındaki **Ambari Görünümler** bölmesini seçin.  Bu liste, hangi kitaplıkları yüklediğinize bağlı olarak değişir.  Örneğin, İplik Sıra Yöneticisi, Kovan Görünümü ve Tez Görünümü'ni görebilirsiniz.  Yapılandırma ve hizmet bilgilerini görmek için herhangi bir hizmet bağlantısını seçin.  Ambari UI **Yığını ve Sürüm** sayfası küme hizmetlerinin yapılandırma ve hizmet sürüm geçmişi hakkında bilgi sağlar. Ambari UI'nin bu bölümüne gitmek için **Yönetici** menüsünü ve ardından **Yığınlar ve Sürümler'i**seçin.  Hizmet sürümü bilgilerini görmek için **Sürümler** sekmesini seçin.

![Apache Ambari admin Yığını ve Sürümleri](./media/hdinsight-log-management/ambari-stack-versions.png)

Ambari UI'yi kullanarak, kümedeki belirli bir ana bilgisayarda (veya düğümde) çalışan herhangi bir (veya tümü) hizmet için yapılandırmayı indirebilirsiniz.  **Hosts** menüsünü, ardından ilgi alanı için bağlantıyı seçin. Bu ana bilgisayar sayfasında, **Ana Bilgisayar Eylemleri** düğmesini seçin ve ardından **İstemci Configs'ı indirin.**

![Apache Ambari indir host istemci configs](./media/hdinsight-log-management/download-client-configs.png)

### <a name="view-the-script-action-logs"></a>Komut dosyası eylem günlüklerini görüntüleme

HDInsight [komut dosyası eylemleri,](hdinsight-hadoop-customize-cluster-linux.md) komut dosyalarını el ile veya belirtildiği nde bir kümeüzerinde çalıştırın. Örneğin, komut dosyası eylemleri kümeye ek yazılım yüklemek veya varsayılan değerlerden yapılandırma ayarlarını değiştirmek için kullanılabilir. Komut dosyası eylem günlükleri, kümenin kurulumu sırasında oluşan hatalar ve küme performansını ve kullanılabilirliğini etkileyebilecek yapılandırma ayarlarının değişikliklerini anlamanızı sağlayabilir.  Komut dosyası eyleminin durumunu görmek için Ambari UI'nizdeki **operasyonlar** düğmesini seçin veya varsayılan depolama hesabındaki durum günlüklerine erişin. Depolama günlükleri ' `/STORAGE_ACCOUNT_NAME/DEFAULT_CONTAINER_NAME/custom-scriptaction-logs/CLUSTER_NAME/DATE`den temin edilebilir.

### <a name="view-ambari-alerts-status-logs"></a>Ambari'yi görüntüle durum günlüklerini uyarır

Apache Ambari uyarı durumu `ambari-alerts.log`değişiklikleri yazıyor . Tam `/var/log/ambari-server/ambari-alerts.log`yol. Günlük için hata ayıklamayı etkinleştirmek için, Değiştir'deki `/etc/ambari-server/conf/log4j.properties.` bir özelliği değiştirin ve aşağıdakilerden aşağıdakileri `# Log alert state changes` girebilirsiniz:

```
log4j.logger.alerts=INFO,alerts

to

log4j.logger.alerts=DEBUG,alerts
```

## <a name="step-3-manage-the-cluster-job-execution-log-files"></a>Adım 3: Küme iş yürütme günlüğü dosyalarını yönetme

Bir sonraki adım, çeşitli hizmetler için iş yürütme günlüğü dosyalarını gözden geçirmektir.  Hizmetler Apache HBase, Apache Spark ve diğerleri içerebilir. Hadoop kümesi çok sayıda ayrıntılı günlük üretir, bu nedenle hangi günlüklerin yararlı olduğunu (ve hangilerinin yararlı olmadığını) belirleme zaman alabilir.  Günlük sisteminin anlaşılması, günlük dosyalarının hedefli yönetimi için önemlidir.  Aşağıdaki resim bir örnek günlük dosyasıdır.

![HDInsight örnek günlük dosyası örnek çıktı](./media/hdinsight-log-management/hdi-log-file-example.png)

### <a name="access-the-hadoop-log-files"></a>Hadoop günlük dosyalarına erişin

HDInsight günlük dosyalarını hem küme dosyası sisteminde hem de Azure Depolama'da saklar. Kümeye [bir SSH](hdinsight-hadoop-linux-use-ssh-unix.md) bağlantısı açarak ve dosya sistemine göz atarak veya uzak kafa düğümü sunucusundaki Hadoop İplik Durum portalını kullanarak kümedeki günlük dosyalarını inceleyebilirsiniz. Azure Depolama'daki günlük dosyalarını, Azure Depolama'dan verilere erişebilen ve veri indirebilen araçlardan herhangi birini kullanarak inceleyebilirsiniz. Örnekler [AzCopy,](../storage/common/storage-use-azcopy.md) [CloudXplorer](https://clumsyleaf.com/products/cloudxplorer)ve Visual Studio Server Explorer vardır. Azure blob depolamasındaki verilere erişmek için PowerShell ve Azure Depolama İstemci kitaplıklarını veya Azure .NET SDK'larını da kullanabilirsiniz.

Hadoop kümedeki çeşitli düğümlerde *görev girişimleri* olarak işlerin çalışmasını çalıştırır. HDInsight, ilk olarak tamamlanmadı diğer görev girişimlerini sonlandırmak için spekülatif görev denemeleri başlatabilir. Bu, denetleyiciye, stderr'a ve syslog günlük dosyalarına anında günlüğe kaydedilen önemli bir etkinlik oluşturur. Buna ek olarak, birden çok görev denemesi aynı anda çalışır, ancak bir günlük dosyası yalnızca sonuçları doğruya görüntüleyebilir.

#### <a name="hdinsight-logs-written-to-azure-blob-storage"></a>Azure Blob depolama alanına yazılan HDInsight günlükleri

HDInsight kümeleri, Azure PowerShell cmdlets veya .NET iş gönderme API'leri kullanılarak gönderilen tüm iş için görev günlüklerini Azure Blob depolama hesabına yazmak üzere yapılandırılır.  İşleri Kümeye SSH aracılığıyla gönderirseniz, yürütme günlüğü bilgileri önceki bölümde belirtildiği gibi Azure Tabloları'nda depolanır.

HDInsight tarafından oluşturulan çekirdek günlük dosyalarına ek olarak, YARN gibi yüklü hizmetler de iş yürütme günlüğü dosyaları oluşturur.  Günlük dosyalarının sayısı ve türü yüklenen hizmetlere bağlıdır.  Ortak hizmetler Apache HBase, Apache Spark ve benzeri vardır.  Kümenizde bulunan genel günlük dosyalarını anlamak için her hizmetin iş günlüğü yürütme dosyalarını araştırın.  Her hizmetin kendi benzersiz günlük yöntemleri ve günlük dosyalarını depolamak için konumları vardır.  Örnek olarak, en yaygın hizmet günlüğü dosyalarına (YARN'dan) erişmeye yönelik ayrıntılar aşağıdaki bölümde ele alınmıştır.

### <a name="hdinsight-logs-generated-by-yarn"></a>YARN tarafından oluşturulan HDInsight günlükleri

YARN, bir alt düğümdeki tüm kapsayıcılar arasında günlükleri toplar ve bu günlükleri çalışan düğüm başına birleştirilmiş günlük dosyası olarak depolar. Bu günlük, uygulama bittikten sonra varsayılan dosya sisteminde depolanır. Uygulamanız yüzlerce veya binlerce kapsayıcı kullanabilir, ancak tek bir alt düğümüzerinde çalıştırılabilen tüm kapsayıcılar için günlükler her zaman tek bir dosyaya toplanır. Uygulamanız tarafından kullanılan işçi düğümü başına yalnızca bir günlük vardır. GÜNLÜK toplama hdinsight kümeleri sürüm 3.0 ve üzeri varsayılan olarak etkinleştirilir. Toplanan günlükler küme için varsayılan depolama bulunur.

```
/app-logs/<user>/logs/<applicationId>
```

Toplanan günlükler, kapsayıcı tarafından dizine eklenmiş bir TFile ikili biçiminde yazıldığından doğrudan okunamıyor. Bu günlükleri uygulamalar veya ilgi çekici kapsayıcılar için düz metin olarak görüntülemek için İPLik ResourceManager günlüklerini veya CLI araçlarını kullanın.

#### <a name="yarn-cli-tools"></a>İplik CLI araçları

İPLIK CLI araçlarını kullanmak için öncelikle SSH kullanarak HDInsight kümesine bağlanmanız gerekir. Bu `<applicationId>`komutları çalıştırırken , , `<user-who-started-the-application>` `<containerId>`ve `<worker-node-address>` bilgileri belirtin. Günlükleri aşağıdaki komutlardan biriyle düz metin olarak görüntüleyebilirsiniz:

```bash
yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application>
yarn logs -applicationId <applicationId> -appOwner <user-who-started-the-application> -containerId <containerId> -nodeAddress <worker-node-address>
```

#### <a name="yarn-resourcemanager-ui"></a>İplik Kaynakları Müdürü UI

İplik ResourceManager UI küme başlığı düğümü üzerinde çalışır ve Ambari web UI üzerinden erişilir. İplik günlüklerini görüntülemek için aşağıdaki adımları kullanın:

1. Bir web tarayıcısında `https://CLUSTERNAME.azurehdinsight.net` sayfasına gidin. CLUSTERNAME değerini HDInsight kümenizin adıyla değiştirin.
2. Soldaki hizmetler listesinden YARN'ı seçin.
3. Hızlı Bağlantılar açılır bölümünden küme kafası düğümlerinden birini seçin ve ardından **ResourceManager günlüklerini**seçin. İplik günlüklerine bağlantı listesi sunulur.

## <a name="step-4-forecast-log-volume-storage-sizes-and-costs"></a>Adım 4: Tahmin günlüğü toplu depolama boyutları ve maliyetleri

Önceki adımları tamamladıktan sonra, HDInsight kümenizin ürettiği günlük dosyalarının türleri ve birimleri hakkında bir anlayışa sahip siniz.

Ardından, belirli bir süre içinde anahtar günlük depolama konumlarındaki günlük verilerinin hacmini çözümle. Örneğin, 30-60-90 günlük dönemlerde hacim ve büyüme analiz edebilirsiniz.  Bu bilgileri bir elektronik tabloya kaydedin veya Visual Studio, Azure Depolama Gezgini veya Excel için Güç Sorgusu gibi diğer araçları kullanın. Daha fazla bilgi için [hdinsight günlüklerini analiz](hdinsight-debug-jobs.md)edin.  

Artık anahtar günlükleri için bir günlük yönetimi stratejisi oluşturmak için yeterli bilgiye sahipsiniz.  Hem günlük boyutu büyümesini hem de günlük depolama Azure hizmet maliyetlerini tahmin etmek için elektronik tablonuzu (veya seçtiğiniz aracı) kullanın.  İncelediğiniz günlükler kümesi için günlük bekletme gereksinimlerini de göz önünde bulundurun.  Artık, hangi günlük dosyalarının silinebileceğini (varsa) ve hangi günlüklerin daha ucuz azure depolama alanına saklanıp arşivleneceğini belirledikten sonra gelecekteki günlük depolama maliyetlerini yeniden tahmin edebilirsiniz.

## <a name="step-5-determine-log-archive-policies-and-processes"></a>Adım 5: Günlük arşivi ilkelerini ve işlemlerini belirleme

Hangi günlük dosyalarının silinebileceğini belirledikten sonra, belirli bir süre sonra günlük dosyalarını otomatik olarak silmek için birçok Hadoop hizmetinde günlük parametrelerini ayarlayabilirsiniz.

Belirli günlük dosyaları için daha düşük fiyatlı günlük dosyası arşivleme yaklaşımını kullanabilirsiniz. Azure Kaynak Yöneticisi etkinlik günlükleri için, Bu yaklaşımı Azure portalını kullanarak keşfedebilirsiniz.  HDInsight örneğiniz için Azure portalındaki Etkinlik **Günlüğü** bağlantısını seçerek Kaynak Yöneticisi günlüklerinin arşivlemesini ayarlayın.  Etkinlik Günlüğü arama sayfasının üst kısmında, **Dışa** Aktarma etkinlik günlüğü bölmesini açmak için **Dışa** Aktar menüsü öğesini seçin.  Aboneliği, bölgeyi, depolama hesabına dışa aktarılıp dışa aktarılmayacağı ve günlüklerin kaç gün tutulup tutulmayacağı oturumu doldurun. Aynı bölmede, bir olay merkezine dışa aktarılıp dışa aktarılmayacağınızı da belirtebilirsiniz.

![Azure portalı dışa aktarma etkinlik günlüğü önizlemesi](./media/hdinsight-log-management/hdi-export-log-files.png)

Alternatif olarak, PowerShell ile günlük arşivleme komut dosyası olabilir.  Örneğin PowerShell komut dosyası için, [Azure Şişirme Depolama'da Azure Otomasyongünlüklerini Arşivle'ye](https://gallery.technet.microsoft.com/scriptcenter/Archive-Azure-Automation-898a1aa8)bakın.

### <a name="accessing-azure-storage-metrics"></a>Azure Depolama ölçümlerine erişim

Azure Depolama, günlük depolama işlemleri ve erişim için yapılandırılabilir. Bu çok ayrıntılı günlükleri kapasite izleme ve planlama ve depolama isteklerini denetlemek için kullanabilirsiniz. Günlüğe kaydedilmiş bilgiler, çözümlerinizin performansını izlemenize ve ince ayar yapmanızı sağlayan gecikme ayrıntıları içerir.
Bir HDInsight kümesinin verilerini tutan Azure Depolama için oluşturulan günlük dosyalarını incelemek için Hadoop için .NET SDK'yı kullanabilirsiniz.

### <a name="control-the-size-and-number-of-backup-indexes-for-old-log-files"></a>Eski günlük dosyaları için yedekleme dizinlerinin boyutunu ve sayısını denetleme

Tutulan günlük dosyalarının boyutunu ve sayısını denetlemek için aşağıdaki `RollingFileAppender`özellikleri ayarlayın:

* `maxFileSize`dosyanın üstünde ki kritik boyutudur. Varsayılan değer 10 MB'dır.
* `maxBackupIndex`oluşturulacak yedekleme dosyalarının sayısını belirtir, varsayılan 1.

### <a name="other-log-management-techniques"></a>Diğer günlük yönetimi teknikleri

Disk alanının tükenmesini önlemek için, günlük dosyalarının işlenmesini yönetmek için [logrotate](https://linux.die.net/man/8/logrotate) gibi bazı işletim sistemi araçlarını kullanabilirsiniz. Günlük dosyalarını `logrotate` sıkıştırarak ve eskilerini kaldırarak günlük olarak çalışacak şekilde yapılandırabilirsiniz. Yaklaşımınız, günlük dosyalarının yerel düğümlerde ne kadar süreyle sakkalacağınız gibi gereksinimlerinize bağlıdır.  

Çıkış günlüğü boyutunu büyük ölçüde artıran bir veya daha fazla hizmet için HATA Ayıklama günlüğünün etkin olup olmadığını da denetleyebilirsiniz.  

Tüm düğümlerden tek bir merkezi konuma günlük leri toplamak için, Solr'daki tüm günlük girişlerini yutmak gibi bir veri akışı oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight için İzleme ve Günlük Uygulaması](https://msdn.microsoft.com/library/dn749790.aspx)
* [Linux tabanlı HDInsight'ta Apache Hadoop YARN uygulama günlüklerine erişin](hdinsight-hadoop-access-yarn-app-logs-linux.md)
* [Çeşitli Apache Hadoop bileşenleri için günlük dosyalarının boyutunu denetleme](https://community.hortonworks.com/articles/8882/how-to-control-size-of-log-files-for-various-hdp-c.html)
