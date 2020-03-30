---
title: Azure HDInsight kümesinde yavaş veya başarısız bir işi giderme
description: Azure HDInsight kümesinde yavaş veya başarısız bir işi tanılayın ve sorun giderin.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: troubleshooting
ms.date: 08/15/2019
ms.openlocfilehash: be991b63784a2c72a51bfbdc8506f3b4695ed6c7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75895311"
---
# <a name="troubleshoot-a-slow-or-failing-job-on-a-hdinsight-cluster"></a>HDInsight kümesinde yavaş veya başarısız olan bir işin sorunlarını giderme

HDInsight kümesindeki bir uygulama işleme verisi yavaş çalışıyorsa veya bir hata koduyla başarısız oluyorsa, birkaç sorun giderme seçeneğiniz vardır. İşlerinizin çalışması beklenenden daha uzun sürüyorsa veya genel olarak yavaş yanıt süreleri görüyorsanız, kümenin çalıştığı hizmetler gibi kümenizden yukarı akışta hatalar olabilir. Ancak, bu yavaşlamaların en yaygın nedeni yetersiz ölçekleme olduğunu. Yeni bir HDInsight kümesi oluşturduğunuzda, uygun [sanal makine boyutlarını](hdinsight-component-versioning.md#default-node-configuration-and-virtual-machine-sizes-for-clusters)seçin.

Yavaş veya başarısız bir kümeyi tanılamak için, ilişkili Azure Hizmetleri, küme yapılandırması ve iş yürütme bilgileri gibi ortamın tüm yönleri hakkında bilgi toplayın. Yararlı bir tanılama başka bir kümeüzerinde hata durumunu çoğaltmak için denemektir.

* Adım 1: Sorun hakkında veri toplayın.
* Adım 2: HDInsight küme ortamını doğrulayın.
* Adım 3: Kümenizin durumunu görüntüleyin.
* Adım 4: Ortam yığınını ve sürümleri gözden geçirin.
* Adım 5: Küme günlüğü dosyalarını inceleyin.
* Adım 6: Yapılandırma ayarlarını kontrol edin.
* Adım 7: Farklı bir kümedeki hatayı yeniden üretin.

## <a name="step-1-gather-data-about-the-issue"></a>Adım 1: Sorun hakkında veri toplama

HDInsight, kümelerle ilgili sorunları tanımlamak ve sorun gidermek için kullanabileceğiniz birçok araç sağlar. Aşağıdaki adımlar bu araçlar da size yol gösterir ve sorunu saptamak için öneriler sağlar.

### <a name="identify-the-problem"></a>Soruyu tanımlama

Sorunu belirlemeye yardımcı olmak için aşağıdaki soruları göz önünde bulundurun:

* Ne olmasını bekliyordum ki? Onun yerine ne oldu?
* Sürecin çalışması ne kadar sürdü? Ne kadar sürmeliydi?
* Görevlerim bu kümede her zaman yavaş mı çalışır? Farklı bir kümede daha hızlı mı koştular?
* Bu sorun ilk ne zaman oluştu? O zamandan beri ne sıklıkta oldu?
* Küme yapılandırmamda bir değişiklik oldu mu?

### <a name="cluster-details"></a>Küme ayrıntıları

Önemli küme bilgileri şunları içerir:

* Küme adı.
* Küme bölgesi - [bölge kesintileri](https://azure.microsoft.com/status/)için kontrol edin.
* HDInsight küme türü ve sürümü.
* Baş ve alt düğümler için belirtilen HDInsight örneklerinin türü ve sayısı.

Azure portalı şu bilgileri sağlayabilir:

![HDInsight Azure portalı Bilgileri](./media/hdinsight-troubleshoot-failed-cluster/hdi-azure-portal-info.png)

[Azure CLI'yi](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest)de kullanabilirsiniz:

```azurecli
az hdinsight list --resource-group <ResourceGroup>
az hdinsight show --resource-group <ResourceGroup> --name <ClusterName>
```

Başka bir seçenek PowerShell kullanıyor. Daha fazla bilgi için Azure [PowerShell ile HDInsight'ta Apache Hadoop kümelerini yönet'e](hdinsight-administer-use-powershell.md)bakın.

## <a name="step-2-validate-the-hdinsight-cluster-environment"></a>Adım 2: HDInsight küme ortamını doğrulama

Her HDInsight kümesi çeşitli Azure hizmetlerine ve Apache HBase ve Apache Spark gibi açık kaynaklı yazılımlara dayanır. HDInsight kümeleri, Azure Sanal Ağlar gibi diğer Azure hizmetlerini de arayabilir.  Küme hatası, kümenizdeki çalışan hizmetlerden herhangi biri veya harici bir hizmet ten kaynaklanabilir.  Küme hizmeti yapılandırma değişikliği kümenin başarısız olması yla da ilgili olabilir.

### <a name="service-details"></a>Servis detayları

* Açık kaynak kitaplık sürüm sürümlerini denetleyin.
* Azure [Hizmet Kesintileri](https://azure.microsoft.com/status/)olup yok.  
* Azure Hizmeti kullanım sınırlarını denetleyin. 
* Azure Sanal Ağ alt ağ yapılandırmasını kontrol edin.  

### <a name="view-cluster-configuration-settings-with-the-ambari-ui"></a>Ambari UI ile küme yapılandırma ayarlarını görüntüleme

Apache Ambari, bir WEB Kullanıcı Arabirimi ve REST API ile bir HDInsight kümesinin yönetimini ve izlenmesini sağlar. Ambari, Linux tabanlı HDInsight kümelerine dahildir. Azure portalı HDInsight sayfasında **Küme Panosu** bölmesini seçin.  Ambari UI'yi açmak için **HDInsight küme panosu** bölmesini seçin ve küme giriş kimlik bilgilerini girin.  

![Apache Ambari panoya genel bakış](./media/hdinsight-troubleshoot-failed-cluster/apache-ambari-overview.png)

Hizmet görünümlerinin listesini açmak için Azure portalı sayfasında **Ambari Görünümleri'ni** seçin.  Bu liste, hangi kitaplıkların yüklendiğine bağlıdır. Örneğin, İplik Sıra Yöneticisi, Kovan Görünümü ve Tez Görünümü'ni görebilirsiniz.  Yapılandırma ve hizmet bilgilerini görmek için bir hizmet bağlantısı seçin.

#### <a name="check-for-azure-service-outages"></a>Azure hizmet kesintilerini denetleme

HDInsight, çeşitli Azure hizmetlerine dayanır. Azure HDInsight'ta sanal sunucular çalıştırır, verileri ve komut dosyalarını Azure Blob depolama veya Azure Veri Gölü Depolama'da depolar ve Azure Tablo depolamasında günlük dosyalarını dizinler. Bu hizmetlerde meydana gelebilecek kesintiler, nadir de olsa, HDInsight'ta sorunlara neden olabilir. Kümenizde beklenmeyen yavaşlamalar veya hatalar varsa, [Azure Durum Panosu'nu](https://azure.microsoft.com/status/)kontrol edin. Her hizmetin durumu bölgeye göre listelenir. İlgili hizmetler için kümenizin bölgesini ve bölgelerini kontrol edin.

#### <a name="check-azure-service-usage-limits"></a>Azure hizmet kullanım sınırlarını denetleme

Büyük bir küme başlatıyorsanız veya aynı anda birden çok küme başlattıysanız, bir Azure hizmet sınırını aştıysanız küme başarısız olabilir. Hizmet sınırları, Azure aboneliğinize bağlı olarak değişir. Daha fazla bilgi için bkz. [Azure aboneliği ve hizmet limitleri, kotalar ve kısıtlamalar](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits).
Microsoft'tan Kullanılabilir HDInsight kaynaklarının (VM çekirdekleri ve VM örnekleri gibi) sayısını [Kaynak Yöneticisi çekirdek kota artış isteğiyle](https://docs.microsoft.com/azure/azure-portal/supportability/resource-manager-core-quotas-request)artırmasını isteyebilirsiniz.

#### <a name="check-the-release-version"></a>Sürüm sürümünü kontrol edin

Küme sürümünü en son HDInsight sürümüyle karşılaştırın. Her HDInsight sürümü yeni uygulamalar, özellikler, düzeltmeler ve hata düzeltmeleri gibi geliştirmeleri içerir. Kümenizi etkileyen sorun en son sürüm sürümünde giderilmiş olabilir. Mümkünse, HDInsight'ın en son sürümünü ve Apache HBase, Apache Spark ve diğerleri gibi ilişkili kitaplıkları kullanarak kümenizi yeniden çalıştırın.

#### <a name="restart-your-cluster-services"></a>Küme hizmetlerinizi yeniden başlatın

Kümenizde yavaşlamalar yaşıyorsanız, Ambari UI veya Azure Klasik CLI aracılığıyla hizmetlerinizi yeniden başlatmayı düşünün. Küme geçici hatalar yaşıyor olabilir ve yeniden başlatma ortamınızı stabilize etmek ve büyük olasılıkla performansı artırmak için en hızlı yoldur.

## <a name="step-3-view-your-clusters-health"></a>Adım 3: Kümenizin durumunu görüntüleyin

HDInsight kümeleri, sanal makine örneklerinde çalışan farklı türde düğümlerden oluşur. Her düğüm kaynak açlığı, ağ bağlantısı sorunları ve kümeyi yavaşlatabilecek diğer sorunlar için izlenebilir. Her küme iki baş düğümü içerir ve çoğu küme türü alt ve kenar düğümlerinin bir birleşimini içerir. 

Her küme türünün kullandığı çeşitli düğümlerin açıklaması için [bkz.](hdinsight-hadoop-provision-linux-clusters.md)

Aşağıdaki bölümlerde, her düğümün ve genel kümenin durumunun nasıl denetlenir.

### <a name="get-a-snapshot-of-the-cluster-health-using-the-ambari-ui-dashboard"></a>Ambari UI panosunu kullanarak küme durumunun anlık görüntüsünü alın

[Ambari Kullanıcı Arabirimi panosu](#view-cluster-configuration-settings-with-the-ambari-ui) (`https://<clustername>.azurehdinsight.net`) çalışma süresi, bellek, ağ ve CPU kullanımı, HDFS disk kullanımı vb. gibi küme durumu hakkında genel bir bakış sağlar. Kaynakları ana bilgisayar düzeyinde görüntülemek için Ambari'nin Hosts bölümünü kullanın. Ayrıca hizmetleri durdurup yeniden başlatabilirsiniz.

### <a name="check-your-webhcat-service"></a>WebHCat hizmetinizi kontrol edin

Apache Hive, Apache Pig veya Apache Sqoop işleri başarısız için ortak bir senaryo [WebHCat](hdinsight-hadoop-templeton-webhcat-debug-errors.md) (veya *Templeton)* hizmeti ile bir başarısızlık. WebHCat Hive, Pig, Scoop ve MapReduce gibi uzak iş yürütme için bir REST arabirimidir. WebHCat, iş gönderme isteklerini Apache Hadoop İPLik uygulamalarına çevirir ve İPLik uygulama durumundan türetilen bir durumu döndürür.  Aşağıdaki bölümlerde ortak WebHCat HTTP durum kodları açıklanır.

#### <a name="badgateway-502-status-code"></a>BadGateway (502 durum kodu)

Bu kod, ağ geçidi düğümlerinden gelen genel bir iletidir ve en yaygın hata durum kodlarıdır. Bunun olası bir nedeni, WebHCat hizmetinin etkin kafa düğümünde aşağıda olmasıdır. Bu olasılığı denetlemek için aşağıdaki CURL komutunu kullanın:

```bash
curl -u admin:{HTTP PASSWD} https://{CLUSTERNAME}.azurehdinsight.net/templeton/v1/status?user.name=admin
```

Ambari, WebHCat hizmetinin çöktüklerini gösteren bir uyarı görüntüler. WebHCat hizmetini ana bilgisayarda yeniden başlatarak yeniden başlatmayı deneyebilirsiniz.

![Apache Ambari WebHCat Server'ı Yeniden Başlat](./media/hdinsight-troubleshoot-failed-cluster/restart-webhcat-server.png)

Bir WebHCat sunucusu hala açmıyorsa, işlem günlüğünü hata iletileri için denetleyin. Daha ayrıntılı bilgi için `stderr` `stdout` düğümde başvurulan dosyaları ve dosyaları kontrol edin.

#### <a name="webhcat-times-out"></a>WebHCat kez dışarı

BIR HDInsight Ağ Geçidi, iki dakikadan uzun `502 BadGateway`süren yanıtları zamanlayarak geri döndü. WebHCat, iş durumları için İplik hizmetlerini sorgular ve YARN'ın yanıt vermesi iki dakikadan uzun sürerse, bu istek zaman aşımına neden olabilir.

Bu durumda, dizindeki aşağıdaki günlükleri gözden geçirin: `/var/log/webhcat`

* **webhcat.log** hangi sunucu günlükleri yazıyor log4j günlük
* **webhcat-console.log** başlatıldığında sunucunun stdout olduğunu
* **webhcat-console-error.log** sunucu sürecinin stderr olduğunu

> [!NOTE]  
> Her `webhcat.log` biri günlük olarak yuvarlanır ve kod adlı `webhcat.log.YYYY-MM-DD`dosyalar oluşturur. Araştırdığınız zaman aralığı için uygun dosyayı seçin.

Aşağıdaki bölümlerde WebHCat zaman ekmelerinin bazı olası nedenleri açıklanır.

##### <a name="webhcat-level-timeout"></a>WebHCat seviye zaman

WebHCat yük altındayken, 10'dan fazla açık soketle yeni soket bağlantıları kurmak daha uzun sürer ve bu da zaman aşımına neden olabilir. WebHCat'e ve WebHCat'ten `netstat` gelen ağ bağlantılarını listelemek için geçerli etkin başlıkta kullanın:

```bash
netstat | grep 30111
```

30111 bağlantı noktası WebHCat dinler. Açık soket sayısı 10'dan az olmalıdır.

Açık soket yoksa, önceki komut bir sonuç üretmez. Templeton kadar olup olmadığını kontrol etmek ve port 30111 dinlerken, kullanın:

```bash
netstat -l | grep 30111
```

##### <a name="yarn-level-timeout"></a>İplik seviyesi zaman

Templeton, İPN'i işleri yürütmeye çağırır ve Templeton ve YARN arasındaki iletişim zaman alabilen bir zaman alabına neden olabilir.

İplik düzeyinde, iki tür zaman aşışı vardır:

1. Bir İplik işi göndermek bir zaman alacaktır yeterince uzun sürebilir.

    Günlük dosyasını `/var/log/webhcat/webhcat.log` açıp "sıralanmış iş" araması yaptığınızda, yürütme süresinin aşırı uzun olduğu (>2000 ms) birden çok giriş görebilir ve girişler artan bekleme sürelerini gösterir.

    Yeni işlerin gönderilme oranı eski işlerin tamamlanma hızından daha yüksek olduğundan, sıraya giren işlerin zamanı artmaya devam eder. İPLik belleği %100 kullanıldıktan sonra, *iş başlatıcı sıcağı* *artık varsayılan kuyruktan*kapasite ödünç alamaz. Bu nedenle, artık yeni işler joblauncher sıraya kabul edilebilir. Bu davranış, bekleme süresinin daha uzun ve daha uzun olmasına neden olarak, genellikle diğerleri tarafından izlenen bir zaman aşımı hatasına neden olabilir.

    Aşağıdaki resimde, fazla kullanılan %714,4'te iş başlatıcısı kuyruğu gösterilmektedir. Bu, ödünç almak için varsayılan sırada hala serbest kapasite olduğu sürece kabul edilebilir. Ancak, küme tam olarak kullanıldığında ve İplik belleği %100 kapasitede olduğunda, yeni işler beklenmelidir ve bu da zaman alabilmelere neden olur.

    ![HDInsight İş başlatıcısı sıra görünümü](./media/hdinsight-troubleshoot-failed-cluster/hdi-job-launcher-queue.png)

    Bu sorunu çözmenin iki yolu vardır: ya gönderilen yeni işlerin hızını azaltmak veya kümeyi ölçekleyerek eski işlerin tüketim hızını artırmak.

2. İplik işleme zaman aparatları neden olabilir, uzun zaman alabilir.

    * Tüm işleri listele: Bu, zaman alan bir aramadır. Bu çağrı, UYGULAMALARı İPN ResourceManager'dan doğrular ve tamamlanan her uygulama için IPN JobHistoryServer'dan durum alır. Daha fazla sayıda iş ile, bu arama zaman dışarı olabilir.

    * Yedi günden eski işleri listeleyin: HDInsight YARN JobHistoryServer, tamamlanan iş`mapreduce.jobhistory.max-age-ms` bilgilerini yedi gün (değer) olarak tutacak şekilde yapılandırılır. Tasfiye işleri sayısallandırmaya çalışmak zaman alakart ait.

Bu sorunları tanılamak için:

1. Sorun gidermek için UTC zaman aralığını belirleme
2. Uygun `webhcat.log` dosya(lar) seçin
3. Bu süre zarfında WARN ve ERROR iletilerini arayın

#### <a name="other-webhcat-failures"></a>Diğer WebHCat hataları

1. HTTP durum kodu 500

    WebHCat'in 500 döndürdüğü çoğu durumda, hata iletisi hatayla ilgili ayrıntıları içerir. Aksi takdirde, `webhcat.log` WARN ve ERROR iletilerini arayın.

2. İş hataları

    WebHCat ile etkileşimlerin başarılı olduğu, ancak işlerin başarısız olduğu durumlar olabilir.

    Templeton gibi `stderr` iş konsolu `statusdir`çıktı toplar , genellikle sorun giderme için yararlıdır. `stderr`gerçek sorgunun İplik uygulama tanımlayıcısını içerir.

## <a name="step-4-review-the-environment-stack-and-versions"></a>Adım 4: Ortam yığınını ve sürümlerigözden geçirme

Ambari UI **Stack ve Sürüm** sayfası küme hizmetleri yapılandırması ve hizmet sürüm geçmişi hakkında bilgi sağlar.  Yanlış Hadoop hizmet kitaplığı sürümleri küme hatası nedeni olabilir.  Ambari UI'de **Yönetici** menüsünü seçin ve ardından **Yığınlar ve Sürümler'i**seçin.  Hizmet sürümü bilgilerini görmek için sayfadaki **Sürümler** sekmesini seçin:

![Apache Ambari Yığını ve Sürümleri](./media/hdinsight-troubleshoot-failed-cluster/ambari-stack-versions.png)

## <a name="step-5-examine-the-log-files"></a>Adım 5: Günlük dosyalarını inceleyin

HDInsight kümesini oluşturan birçok hizmet ve bileşenden oluşturulan birçok günlük türü vardır. [WebHCat günlük dosyaları](#check-your-webhcat-service) daha önce açıklanmıştır. Aşağıdaki bölümlerde açıklandığı gibi, kümenizdeki sorunları daraltmak için araştırabileceğiniz birkaç yararlı günlük dosyası vardır.

* HDInsight kümeleri, çoğu gönderilen işleri çalıştırmakla görevli birkaç düğümden oluşur. İşler aynı anda çalışır, ancak günlük dosyaları yalnızca sonuçları doğrusal olarak görüntüleyebilir. HDInsight, ilk olarak tamamlayamayan diğer görevleri sonlandırmak için yeni görevler yürütür. Tüm bu etkinlik `stderr` `syslog` ve dosyalara kaydedilir.

* Komut dosyası eylem günlüğü dosyaları, kümenizin oluşturma işlemi sırasında hataları veya beklenmeyen yapılandırma değişikliklerini gösterir.

* Hadoop adım günlükleri, hatalar içeren bir adımın parçası olarak başlatılan Hadoop işlerini tanımlar.

### <a name="check-the-script-action-logs"></a>Komut dosyası eylem günlüklerini denetleme

HDInsight [komut dosyası eylemleri,](hdinsight-hadoop-customize-cluster-linux.md) kümedeki komut dosyalarını el ile veya belirtildiğinde çalıştırın. Örneğin, komut dosyası eylemleri kümeye ek yazılım yüklemek veya varsayılan değerlerden yapılandırma ayarlarını değiştirmek için kullanılabilir. Komut dosyası eylem günlüklerinin denetlemesi, küme kurulumu ve yapılandırma sırasında oluşan hatalar hakkında bilgi sağlayabilir.  Ambari UI'deki **işletim e-i** hizmetini seçerek veya varsayılan depolama hesabından günlüklere erişerek komut dosyası eyleminin durumunu görüntüleyebilirsiniz.

Komut dosyası eylem günlükleri `\STORAGE_ACCOUNT_NAME\DEFAULT_CONTAINER_NAME\custom-scriptaction-logs\CLUSTER_NAME\DATE` dizinde yer eder.

### <a name="view-hdinsight-logs-using-ambari-quick-links"></a>Ambari Hızlı Linkleri kullanarak HDInsight günlüklerini görüntüleyin

HDInsight Ambari UI, bir dizi **Hızlı Bağlantı** bölümü içerir.  HDInsight kümenizde belirli bir hizmetin günlük bağlantılarına erişmek için kümeniz için Ambari UI'yi açın ve ardından soldaki listeden servis bağlantısını seçin. Hızlı **Bağlantılar** açılır düşüşünü, ardından ilgi çekici HDInsight düğümünü seçin ve ilişkili günlüğüne ilişkin bağlantıyı seçin.

Örneğin, HDFS günlükleri için:

![Ambari Hızlı Linkler Oturum Dosyaları için](./media/hdinsight-troubleshoot-failed-cluster/apache-ambari-quick-links.png)

### <a name="view-hadoop-generated-log-files"></a>Hadoop tarafından oluşturulan günlük dosyalarını görüntüleme

HDInsight kümesi, Azure tablolarına ve Azure Blob depolama alanına yazılmış günlükler oluşturur. İplik kendi yürütme günlüklerini oluşturur. Daha fazla bilgi için, [bir HDInsight kümesi için günlükleri yönet'e](hdinsight-log-management.md#access-the-hadoop-log-files)bakın.

### <a name="review-heap-dumps"></a>Yığın dökümlerini gözden geçirin

Yığın dökümleri, o zaman değişkenlerin değerleri de dahil olmak üzere uygulamanın belleğinin anlık görüntüsünü içerir ve bu da çalışma zamanında oluşan sorunları tanılamada yararlıdır. Daha fazla bilgi için, [Linux tabanlı HDInsight'taki Apache Hadoop hizmetleri için yığın dökümlerini etkinleştir mesuliye](hdinsight-hadoop-collect-debug-heap-dump-linux.md)bakın.

## <a name="step-6-check-configuration-settings"></a>Adım 6: Yapılandırma ayarlarını kontrol edin

HDInsight kümeleri Hadoop, Hive, HBase gibi ilgili hizmetler için varsayılan ayarlarla önceden yapılandırılmıştır. Kümenin türüne, donanım yapılandırmasına, düğüm sayısına, çalıştırdığınız iş türlerine ve birlikte çalıştığınız verilere (ve bu verilerin nasıl işlendiğine) bağlı olarak yapılandırmanızı optimize etmeniz gerekebilir.

Çoğu senaryo için performans yapılandırmalarını en iyi duruma getirme hakkında ayrıntılı talimatlar için, [Apache Ambari ile küme yapılandırmalarını optimize etme](hdinsight-changing-configs-via-ambari.md)'ye bakın. Spark'ı kullanırken performans [için Apache Spark işlerini optimize edin.](spark/apache-spark-perf.md) 

## <a name="step-7-reproduce-the-failure-on-a-different-cluster"></a>Adım 7: Farklı bir kümedeki hatayı yeniden oluşturma

Küme hatasının kaynağını tanılamaya yardımcı olmak için, aynı yapılandırmayla yeni bir küme başlatın ve başarısız olan işin adımlarını tek tek yeniden gönderin. Bir sonraki adımı işlemeden önce her adımın sonuçlarını kontrol edin. Bu yöntem, tek bir başarısız adımı düzeltme ve yeniden çalıştırma fırsatı verir. Bu yöntem, giriş verilerinizi yalnızca bir kez yükleme avantajına sahiptir.

1. Başarısız kümeyle aynı yapılandırmaya sahip yeni bir test kümesi oluşturun.
2. İlk iş adımını test kümesine gönderin.
3. Adım işleme tamamlandığında, adım günlüğü dosyalarındaki hataları denetleyin. Test kümesinin ana düğümüne bağlanın ve günlük dosyalarını buradan görüntüleyin. Adım günlüğü dosyaları yalnızca adım bir süre çalıştığından, bittikten veya başarısız olduktan sonra görünür.
4. İlk adım başarılı olduysa, bir sonraki adımı çalıştırın. Hatalar varsa, günlük dosyalarındaki hatayı araştırın. Kodunuzda bir hataysa, düzeltmeyi yapın ve adımı yeniden çalıştırın.
5. Tüm adımlar hatasız çalışana kadar devam edin.
6. Test kümesini hata ayıklamayı bitirdiğinizde, silin.

## <a name="next-steps"></a>Sonraki adımlar

* [HDInsight kümelerini Apache Ambari Web arabiriminden yönetme](hdinsight-hadoop-manage-ambari.md)
* [HDInsight Günlüklerini Analiz Edin](hdinsight-debug-jobs.md)
* [Linux tabanlı HDInsight'ta Apache Hadoop YARN uygulama işaretine erişin](hdinsight-hadoop-access-yarn-app-logs-linux.md)
* [Linux tabanlı HDInsight'ta Apache Hadoop hizmetleri için yığın dökümlerini etkinleştirin](hdinsight-hadoop-collect-debug-heap-dump-linux.md)
* [HDInsight'ta Apache Spark kümesi için Bilinen Sorunlar](hdinsight-apache-spark-known-issues.md)
