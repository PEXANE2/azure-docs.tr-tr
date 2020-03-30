---
title: Küme performansını izleyin - Azure HDInsight
description: Azure HDInsight'ta Apache Hadoop kümelerinin sistem durumu ve performansı nasıl izlenir?
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 03/09/2020
ms.openlocfilehash: 75ac5a7fc352f877573d79a004d8da761c6f1cef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79082889"
---
# <a name="monitor-cluster-performance-in-azure-hdinsight"></a>Azure HDInsight'ta küme performansını izleme

Bir HDInsight kümesinin sistem durumunu ve performansını izlemek, optimum performans ve kaynak kullanımını sürdürmek için çok önemlidir. İzleme, küme yapılandırma hatalarını ve kullanıcı kodu sorunlarını algılamanıza ve gidermenize de yardımcı olabilir.

Aşağıdaki bölümlerde kümelerinizdeki yükün nasıl izlendiği ve optimize edilebildiği açıklanır, Apache Hadoop İplik kuyrukları ve depolama azaltma sorunlarını algılar.

## <a name="monitor-cluster-load"></a>Küme yükünü izleme

Hadoop kümeleri, kümeüzerindeki yük tüm düğümlere eşit olarak dağıtıldığında en iyi performansı sağlayabilir. Bu, işlem görevlerinin tek tek düğümlerde RAM, CPU veya disk kaynakları tarafından sınırlandırılmadan çalışmasını sağlar.

Kümenizin düğümlerine ve yüklemelerine üst düzey bir bakış elde etmek için [Ambari Web UI'de](hdinsight-hadoop-manage-ambari.md)oturum açın ve ardından **Hosts** sekmesini seçin. Ev sahipleriniz tam nitelikli alan adlarına göre listelenir. Her ev sahibinin çalışma durumu renkli bir sistem durumu göstergesiyle gösterilir:

| Renk | Açıklama |
| --- | --- |
| Red | Ana bilgisayardaki en az bir ana bileşen aşağıda. Etkilenen bileşenleri listeleyen bir araç ucunu görmek için havada yat. |
| Orange | Ana bilgisayardaki en az bir ikincil bileşen aşağıda. Etkilenen bileşenleri listeleyen bir araç ucunu görmek için havada yat. |
| Yellow | Ambari Server 3 dakikadan uzun süredir ev sahibinden sinyal almamıştır. |
| Yeşil | Normal çalışma durumu. |

Ayrıca, her ana bilgisayar için çekirdek sayısını ve RAM miktarını, disk kullanımını ve yükleme ortalamasını gösteren sütunlar da görürsünüz.

![Apache Ambari sekme genel bakış ev sahipliği yapıyor](./media/hdinsight-key-scenarios-to-monitor/apache-ambari-hosts-tab.png)

Ana bilgisayarda çalışan bileşenlere ve bunların ölçümlerine ayrıntılı bir bakış için ana bilgisayar adlarından birini seçin. Ölçümler, CPU kullanımı, yük, disk kullanımı, bellek kullanımı, ağ kullanımı ve işlem sayısının seçilebilir bir zaman çizelgesi olarak gösterilir.

![Apache Ambari ev sahibi ayrıntılarına genel bakış](./media/hdinsight-key-scenarios-to-monitor/apache-ambari-host-details.png)

Uyarıları ayarlama ve ölçümleri görüntüleme yle ilgili ayrıntılar için [Apache Ambari Web UI'yi kullanarak HDInsight kümelerini yönet'e](hdinsight-hadoop-manage-ambari.md) bakın.

## <a name="yarn-queue-configuration"></a>İplik sıra konfigürasyonu

Hadoop dağıtılmış platformu nda çeşitli hizmetler sunmaktadır. YARN (Yet Another Resource Negotiator) bu hizmetleri koordine eder ve herhangi bir yükün küme boyunca eşit olarak dağıtıldığından emin olmak için küme kaynakları ayırır.

YARN, JobTracker'ın iki sorumluluklarını, kaynak yönetimini ve iş planlama/izlemeyi iki daemon'a böler: küresel kaynak yöneticisi ve uygulama başına ApplicationMaster (AM).

Kaynak Yöneticisi saf bir *zamanlayıcıdır*ve yalnızca rakip tüm uygulamalar arasında kullanılabilir kaynakları tahkim eder. Kaynak Yöneticisi, SLA'lar, kapasite garantileri vb. gibi çeşitli sabitleri optimize ederek tüm kaynakların her zaman kullanımda olmasını sağlar. ApplicationMaster, Kaynak Yöneticisi'nden gelen kaynakları görüşür ve kapsayıcıları ve kaynak tüketimini yürütmek ve izlemek için NodeManager(ler) ile birlikte çalışır.

Birden çok kiracı büyük bir kümeyi paylaştığında, kümenin kaynakları için rekabet vardır. CapacityScheduler, istekleri sıraya alarak kaynak paylaşımına yardımcı olan takılabilir bir zamanlayıcıdır. CapacityScheduler, diğer uygulamaların kuyruklarının boş kaynakları kullanmasına izin verilmeden önce kaynakların bir kuruluşun alt sıraları arasında paylaşılmasını sağlamak için *hiyerarşik kuyrukları* da destekler.

YARN, bu kuyruklara kaynak ayırmamıza olanak tanır ve tüm kullanılabilir kaynaklarınızın atanıp atanmadığını gösterir. Kuyruklarınız hakkındaki bilgileri görüntülemek için Ambari Web Kullanıcı Aracı'nda oturum açın ve üst menüden **İPN Sıra Yöneticisi'ni** seçin.

![Apache Ambari İplik Kuyruk Müdürü](./media/hdinsight-key-scenarios-to-monitor/apache-yarn-queue-manager.png)

İPLik Sıra Yöneticisi sayfası, her birine atanan kapasite yüzdesi ile birlikte soldaki kuyruklarınızın listesini gösterir.

![İplik Sıra Yöneticisi ayrıntıları sayfası](./media/hdinsight-key-scenarios-to-monitor/yarn-queue-manager-details.png)

Ambari panosundan kuyruklarınıza daha ayrıntılı bir göz atmak için soldaki listeden **İplik** hizmetini seçin. Ardından **Hızlı Bağlantılar** açılır menüsünün altında, etkin düğümünuzun altındaki Kaynak **Yöneticisi UI'sini** seçin.

![Kaynak Yöneticisi UI menü bağlantıları](./media/hdinsight-key-scenarios-to-monitor/resource-manager-ui-menu-link.png)

Kaynak Yöneticisi UI'sinde, sol menüden **Zamanlayıcı'yı** seçin. *Uygulama Kuyrukları*altında kuyruklarınızın listesini görürsünüz. Burada, her bir kuyruk için kullanılan kapasiteyi, işlerin aralarında ne kadar iyi dağıtıldığını ve herhangi bir işin kaynak kısıtlamalı olup olmadığını görebilirsiniz.

![Apache HAdoop Kaynak Yöneticisi UI menüsü](./media/hdinsight-key-scenarios-to-monitor/resource-manager-ui-menu.png)

## <a name="storage-throttling"></a>Depolama azaltma

Bir kümenin performans darboğazı depolama düzeyinde gerçekleşebilir. Bu tür bir darboğaz, *blocking* en sık, çalışan görevleriniz depolama hizmetinin işleyebilir daha fazla IO gönderdiğinde meydana gelen giriş/çıkış (IO) işlemlerini engellemesidir. Bu engelleme, geçerli IO'lar işlenene kadar işlenmeyi bekleyen bir IO isteği kuyruğu oluşturur. Bloklar, fiziksel bir sınır değil, bir hizmet düzeyi sözleşmesi (SLA) tarafından depolama hizmeti tarafından dayatılan bir sınır olan *depolama azaltma*nedeniyledir. Bu sınır, tek bir istemcinin veya kiracının hizmeti tekeline alamamasını sağlar. SLA, Azure Depolama için saniyede iOs (IOPS) sayısını sınırlar - ayrıntılar [için, standart depolama hesapları için ölçeklenebilirlik ve performans hedeflerine](../storage/common/scalability-targets-standard-account.md)bakın.

Azure Depolama'yı kullanıyorsanız, azaltma da dahil olmak üzere depolama yla ilgili sorunları izlemek için [bkz.](https://docs.microsoft.com/azure/storage/storage-monitoring-diagnosing-troubleshooting)

Kümenizin destek deposu Azure Veri Gölü Depolama (ADLS) ise, azaltma nız büyük olasılıkla bant genişliği sınırları nedeniyle dir. Bu durumda azaltma, görev günlüklerinde azaltma hataları gözlemleyerek tanımlanabilir. ADLS için, bu makalelerde uygun hizmet için azaltma bölümüne bakın:

* [HDInsight ve Azure Veri Gölü Depolama'da Apache Hive için performans alamı kılavuzu](../data-lake-store/data-lake-store-performance-tuning-hive.md)
* [HDInsight ve Azure Veri Gölü Depolama'da MapReduce için performans ayarı kılavuzu](../data-lake-store/data-lake-store-performance-tuning-mapreduce.md)
* [HDInsight ve Azure Veri Gölü Depolama'da Apache Storm için performans alamı kılavuzu](../data-lake-store/data-lake-store-performance-tuning-storm.md)

## <a name="troubleshoot-sluggish-node-performance"></a>Yavaş düğüm performansını giderme

Bazı durumlarda, kümedeki düşük disk alanı nedeniyle halsizlik oluşabilir. Şu adımlarla araştırın:

1. Düğümlerin her birine bağlanmak için [ssh komutunu](./hdinsight-hadoop-linux-use-ssh-unix.md) kullanın.

1. Aşağıdaki komutlardan birini çalıştırarak disk kullanımını denetleyin:

    ```bash
    df -h
    du -h --max-depth=1 / | sort -h
    ```

1. Çıktıyı gözden geçirin ve `mnt` klasörde veya diğer klasörlerde büyük dosyaların varlığını denetleyin. Genellikle, `usercache`ve `appcache` (mnt/resource/hadoop/ipn/local/usercache/hive/appcache/) klasörleri büyük dosyalar içerir.

1. Büyük dosyalar varsa, geçerli bir iş dosya büyümesine neden oluyor veya başarısız bir önceki iş bu soruna katkıda bulunmuş olabilir. Bu davranışın geçerli bir iş tarafından neden olup olmadığını denetlemek için aşağıdaki komutu çalıştırın:

    ```bash
    sudo du -h --max-depth=1 /mnt/resource/hadoop/yarn/local/usercache/hive/appcache/
    ```

1. Bu komut belirli bir işi gösteriyorsa, aşağıdakilere benzeyen bir komut kullanarak işi sonlandırmayı seçebilirsiniz:

    ```bash
    yarn application -kill -applicationId <application_id>
    ```

    Uygulama `application_id` kimliğiyle değiştirin. Belirli bir iş belirtilmezse, bir sonraki adıma geçin.

1. Yukarıdaki komut tamamlandıktan sonra veya belirli bir iş belirtilmediyse, aşağıdakilere benzeyen bir komut çalıştırarak tanımladığınız büyük dosyaları silin:

    ```bash
    rm -rf filecache usercache
    ```

Disk alanı sorunlarıyla ilgili daha fazla bilgi için disk [alanı dışında'na](./hadoop/hdinsight-troubleshoot-out-disk-space.md)bakın.

> [!NOTE]  
> Tutmak istediğiniz ancak düşük disk alanı sorununa katkıda bulunan büyük dosyalarınız varsa, HDInsight kümenizi ölçeklendirmeniz ve hizmetlerinizi yeniden başlatmanız gerekir. Bu yordamı tamamladıktan ve birkaç dakika bekledikten sonra, depolama alanının boşaltılmış olduğunu ve düğümün olağan performansının geri yüklenir olduğunu fark edeceksiniz.

## <a name="next-steps"></a>Sonraki adımlar

Sorun giderme ve kümelerinizi izleme hakkında daha fazla bilgi için aşağıdaki bağlantıları ziyaret edin:

* [HDInsight günlüklerini çözümleme](hdinsight-debug-jobs.md)
* [Apache Hadoop İPlik günlükleri ile hata ayıklama uygulamaları](hdinsight-hadoop-access-yarn-app-logs-linux.md)
* [Linux tabanlı HDInsight'ta Apache Hadoop hizmetleri için yığın dökümlerini etkinleştirin](hdinsight-hadoop-collect-debug-heap-dump-linux.md)
