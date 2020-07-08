---
title: Küme performansını izleme-Azure HDInsight
description: Azure HDInsight 'ta Apache Hadoop kümelerinin sistem durumunu ve performansını izleme.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 03/09/2020
ms.openlocfilehash: 75ac5a7fc352f877573d79a004d8da761c6f1cef
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "79082889"
---
# <a name="monitor-cluster-performance-in-azure-hdinsight"></a>Azure HDInsight 'ta küme performansını izleme

HDInsight kümesinin sistem durumunu ve performansını izlemek, en iyi performansı ve kaynak kullanımını sürdürmek için gereklidir. İzleme, küme yapılandırma hatalarını ve Kullanıcı kodu sorunlarını tespit etmenize ve adreslamanıza yardımcı olabilir.

Aşağıdaki bölümlerde, kümelerinizde yükün nasıl izleneceği ve iyileştirgetirileceği, YARN kuyruklarınızın Apache Hadoop ve depolama azaltma sorunlarının algılanması açıklanır.

## <a name="monitor-cluster-load"></a>Küme yükünü izleme

Küme üzerinde yük, tüm düğümlerde eşit şekilde dağıtıldığında Hadoop kümeleri en iyi performansı sunabilir. Bu işlem, tek tek düğümlerdeki RAM, CPU veya disk kaynaklarıyla sınırlandırılmadan işleme görevlerinin çalıştırılmasını sağlar.

Kümenizin düğümlerine ve bunların yüklenmesine ilişkin üst düzey bir görünüm almak için, [ambarı Web Kullanıcı arabiriminde](hdinsight-hadoop-manage-ambari.md)oturum açın ve ardından **konaklar** sekmesini seçin. Konaklar tam etki alanı adlarına göre listelenir. Her konağın işletim durumu renkli bir sistem durumu göstergesi ile gösterilir:

| Renk | Açıklama |
| --- | --- |
| Red | Konaktaki en az bir ana bileşen çalışmıyor. Etkilenen bileşenleri listeleyen araç ipucunu görmek için üzerine gelin. |
| Orange | Konaktaki en az bir ikincil bileşen çalışmıyor. Etkilenen bileşenleri listeleyen araç ipucunu görmek için üzerine gelin. |
| Yellow | Ambarı sunucusu ana bilgisayardan 3 dakikadan uzun bir sinyal almadı. |
| Yeşil | Normal çalışma durumu. |

Ayrıca, her konak için çekirdek sayısını ve RAM miktarını ve disk kullanımını ve yük ortalamasını gösteren sütunları görürsünüz.

![Apache ambarı ana bilgisayarları sekmesine genel bakış](./media/hdinsight-key-scenarios-to-monitor/apache-ambari-hosts-tab.png)

Bu konakta ve bunların ölçümlerinde çalışan bileşenlere ayrıntılı bir bakış için konak adlarından herhangi birini seçin. Ölçümler, kullanılabilir CPU kullanımı, yükleme, disk kullanımı, bellek kullanımı, ağ kullanımı ve işlem sayısı gibi seçilebilir bir zaman çizelgesi olarak gösterilir.

![Apache ambarı ana bilgisayar ayrıntılarına genel bakış](./media/hdinsight-key-scenarios-to-monitor/apache-ambari-host-details.png)

Uyarıları ayarlama ve ölçümleri görüntüleme hakkındaki ayrıntılar için bkz. [Apache ambarı Web Kullanıcı arabirimini kullanarak HDInsight kümelerini yönetme](hdinsight-hadoop-manage-ambari.md) .

## <a name="yarn-queue-configuration"></a>YARN kuyruğu yapılandırması

Hadoop 'un dağıtılmış platformunda çalışan çeşitli hizmetleri vardır. YARN (ancak başka bir kaynak Negotiator), bu hizmetleri koordine eder ve tüm yükün küme genelinde eşit olarak dağıtıldığından emin olmak için küme kaynaklarını ayırır.

YARN, JobTracker, kaynak yönetimi ve iş zamanlama/izlemenin iki sorumlulukını iki Daemon 'ları: genel Kaynak Yöneticisi ve uygulama başına ApplicationMaster (Har) olarak böler.

Kaynak Yöneticisi, saf bir *Zamanlayıcı*olur ve yalnızca tüm rekabet eden uygulamalar arasında kullanılabilir kaynakları hızlar. Kaynak Yöneticisi, tüm kaynakların her zaman kullanıldığı, SLA 'Lar, kapasite garantisi vb. gibi çeşitli sabitler için optimize edilmesini sağlar. ApplicationMaster Kaynak Yöneticisi Kaynakları görüşür ve kapsayıcıları ve kaynak tüketimini yürütmek ve izlemek için NodeManager 'lar ile birlikte kullanılır.

Birden çok kiracı büyük bir kümeyi paylaşıyorsa, kümenin kaynakları için yarışmaya yer vardır. CapacityScheduler, istekleri sıraya alarak kaynak paylaşımında yardımcı olan takılabilir bir Zamanlayıcı 'dır. CapacityScheduler ayrıca kaynakların bir kuruluşun alt sıraları arasında paylaşıldığından, diğer uygulamaların sıralarının ücretsiz kaynakları kullanmasına izin verilmediğinden emin olmak için *hiyerarşik sıraları* destekler.

YARN bu sıralara kaynak ayırmamızı sağlar ve kullanılabilir kaynaklarınızın tümünün atanıp atanmadığını gösterir. Kuyruklarınız hakkındaki bilgileri görüntülemek için, ambarı Web Kullanıcı arabiriminde oturum açın ve sonra üstteki menüden **Yarn kuyruk yöneticisi** ' ni seçin.

![Apache ambarı YARN Kuyruk Yöneticisi](./media/hdinsight-key-scenarios-to-monitor/apache-yarn-queue-manager.png)

YARN kuyruğu Yöneticisi sayfası, her birine atanan kapasitenin yüzdesi ile birlikte sol taraftaki kuyrukların bir listesini gösterir.

![YARN kuyruğu Yöneticisi Ayrıntılar sayfası](./media/hdinsight-key-scenarios-to-monitor/yarn-queue-manager-details.png)

Kuyruklarınızı daha ayrıntılı bir şekilde görmek için, ambarı panosundan soldaki listeden **Yarn** hizmetini seçin. Sonra **hızlı bağlantılar** açılan menüsünde, etkin düğümünüzün altında **Kaynak Yöneticisi Kullanıcı arabirimi** ' ni seçin.

![Kaynak Yöneticisi UI menü bağlantıları](./media/hdinsight-key-scenarios-to-monitor/resource-manager-ui-menu-link.png)

Kaynak Yöneticisi Kullanıcı arabiriminde, sol taraftaki menüden **Zamanlayıcı** ' yı seçin. *Uygulama kuyrukları*altında kuyruklarınızın bir listesini görürsünüz. Burada, kuyruklarınızın her biri için kullanılan kapasiteyi, işlerin aralarında ne kadar iyi dağıtıldığını ve herhangi bir işin kaynak kısıtlamalı olup olmadığını görebilirsiniz.

![Apache HAdoop Kaynak Yöneticisi UI menüsü](./media/hdinsight-key-scenarios-to-monitor/resource-manager-ui-menu.png)

## <a name="storage-throttling"></a>Depolama alanı azaltma

Kümenin performans sorunu depolama düzeyinde meydana gelebilir. Bu tür bir performans, çalışan görevleriniz depolama hizmetinden daha fazla GÇ gönderiyorsa meydana gelmiş olan giriş/çıkış (g/ç) işlemlerini *engelleme* nedeniyle en sık kullanılan sorun sayısıdır. Bu engelleme, geçerli IOs işlenene kadar işlenmek üzere bekleyen bir GÇ istekleri kuyruğu oluşturur. Bloklar, fiziksel sınır olmayan *depolama alanı azaltmasından*kaynaklanır, ancak bir hizmet düzeyi SÖZLEŞMESI (SLA) tarafından depolama hizmeti tarafından uygulanan bir sınır değildir. Bu sınır, tek bir istemcinin veya kiracının hizmeti tekeline almasını sağlar. SLA, Azure Storage için saniyedeki IOs (ıOPS) sayısını sınırlar. Ayrıntılar için bkz. [Standart depolama hesapları Için ölçeklenebilirlik ve performans hedefleri](../storage/common/scalability-targets-standard-account.md).

Azure Storage kullanıyorsanız, azaltma dahil olmak üzere depolama ile ilgili sorunları izleme hakkında bilgi için bkz. [izleyici, tanılama ve sorun giderme Microsoft Azure depolama](https://docs.microsoft.com/azure/storage/storage-monitoring-diagnosing-troubleshooting).

Kümenizin yedekleme deposu Azure Data Lake Storage (ADLS) ise, azaltma bilgileriniz büyük olasılıkla bant genişliği limitlerinin nedenidir. Bu durumda azaltma, görev günlüklerinde azaltma hataları gözlemleyerek belirlenebilir. ADLS için, bu makalelerde uygun hizmet için daraltma bölümüne bakın:

* [HDInsight ve Azure Data Lake Storage Apache Hive için performans ayarlama Kılavuzu](../data-lake-store/data-lake-store-performance-tuning-hive.md)
* [HDInsight ve Azure Data Lake Storage MapReduce için performans ayarlama Kılavuzu](../data-lake-store/data-lake-store-performance-tuning-mapreduce.md)
* [HDInsight ve Azure Data Lake Storage Apache Storm için performans ayarlama Kılavuzu](../data-lake-store/data-lake-store-performance-tuning-storm.md)

## <a name="troubleshoot-sluggish-node-performance"></a>Yavaş düğüm performansının sorunlarını giderme

Bazı durumlarda, kümede yetersiz disk alanı nedeniyle ağır bir durum oluşabilir. Şu adımlarla araştırın:

1. Düğümlerin her birine bağlanmak için [SSH komutunu](./hdinsight-hadoop-linux-use-ssh-unix.md) kullanın.

1. Aşağıdaki komutlardan birini çalıştırarak disk kullanımını kontrol edin:

    ```bash
    df -h
    du -h --max-depth=1 / | sort -h
    ```

1. Çıktıyı gözden geçirin ve `mnt` klasörde veya diğer klasörlerde büyük dosyaların varlığını denetleyin. Genellikle, `usercache` ve `appcache` (mnt/Resource/Hadoop/Yarn/yerel/kullanıcıönbelleği/Hive/APPCACHE/) klasörleri büyük dosyalar içerir.

1. Büyük dosyalar varsa, geçerli bir iş dosyanın büyümesi veya başarısız olan bir önceki iş bu soruna katkıda bulunabilir. Bu davranışın geçerli bir işin neden olup olmadığını denetlemek için aşağıdaki komutu çalıştırın:

    ```bash
    sudo du -h --max-depth=1 /mnt/resource/hadoop/yarn/local/usercache/hive/appcache/
    ```

1. Bu komut belirli bir işi gösteriyorsa, aşağıdaki gibi bir komut kullanarak işi sonlandırmayı tercih edebilirsiniz:

    ```bash
    yarn application -kill -applicationId <application_id>
    ```

    `application_id`Uygulama kimliğiyle değiştirin. Belirli bir iş belirtilmemişse, sonraki adıma gidin.

1. Yukarıdaki komut tamamlandıktan sonra veya belirli bir iş belirtilmemişse, aşağıdakine benzer bir komut çalıştırarak belirlediğiniz büyük dosyaları silin:

    ```bash
    rm -rf filecache usercache
    ```

Disk alanı sorunlarıyla ilgili daha fazla bilgi için bkz. [disk yetersiz alanı](./hadoop/hdinsight-troubleshoot-out-disk-space.md).

> [!NOTE]  
> Korumak istediğiniz büyük dosyalarınız varsa ancak düşük disk alanı sorununa katkıda bulunursa, HDInsight kümenizi ölçeklendirmeniz ve hizmetlerinizi yeniden başlatmanız gerekir. Bu yordamı tamamladıktan ve birkaç dakika bekledikten sonra, depolamanın serbest olduğunu ve düğümün olağan performansının geri yüklendiğini fark edeceksiniz.

## <a name="next-steps"></a>Sonraki adımlar

Kümelerinizi sorun giderme ve izleme hakkında daha fazla bilgi için aşağıdaki bağlantıları ziyaret edin:

* [HDInsight günlüklerini çözümleme](hdinsight-debug-jobs.md)
* [Apache Hadoop YARN günlükleri ile uygulama hatalarını ayıklama](hdinsight-hadoop-access-yarn-app-logs-linux.md)
* [Linux tabanlı HDInsight 'ta Apache Hadoop Hizmetleri için yığın dökümlerini etkinleştirme](hdinsight-hadoop-collect-debug-heap-dump-linux.md)
