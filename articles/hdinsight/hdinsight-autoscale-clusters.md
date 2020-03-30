---
title: Azure HDInsight kümelerini otomatik olarak ölçeklendirin
description: Otomatik olarak Apache Hadoop ölçek kümeleri için Azure HDInsight Otomatik Ölçeklendirme özelliğini kullanın
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.custom: hdinsightactive
ms.date: 03/05/2020
ms.openlocfilehash: 68bc30d08d95fe8e3d20a8ecb7af6c9710951921
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78399722"
---
# <a name="automatically-scale-azure-hdinsight-clusters"></a>Azure HDInsight kümelerini otomatik olarak ölçeklendirin

> [!Important]
> Azure HDInsight Otomatik Ölçeklendirme özelliği, 7 Kasım 2019'da Spark ve Hadoop kümeleri için genel kullanılabilirlik için yayımlandı ve özelliğin önizleme sürümünde bulunmayan geliştirmeleri içeriyordu. 7 Kasım 2019'dan önce bir Kıvılcım kümesi oluşturduysanız ve kümenizde Otomatik Ölçeklendirme özelliğini kullanmak istiyorsanız, önerilen yol yeni bir küme oluşturmak ve yeni kümede Otomatik Ölçeklendirme'yi etkinleştirmektir.
>
> Etkileşimli Sorgu (LLAP) ve HBase kümeleri için otomatik ölçeklendirme hala önizlemededir. Otomatik ölçek yalnızca Spark, Hadoop, Interactive Query ve HBase kümelerinde kullanılabilir.

Azure HDInsight'ın küme Otomatik Ölçeklendirme özelliği, kümedeki işçi düğümlerinin sayısını otomatik olarak ölçeklendirin. Kümedeki diğer düğüm türleri şu anda ölçeklendirilemiyor.  Yeni bir HDInsight kümesinin oluşturulması sırasında, en az ve en fazla sayıda alt düğüm ayarlanabilir. Otomatik ölçeklendirme daha sonra analitik yükün kaynak gereksinimlerini izler ve alt düğüm sayısını yukarı veya aşağı ölçeklendirin. Bu özellik için ek ücret alınmaz.

## <a name="cluster-compatibility"></a>Küme uyumluluğu

Aşağıdaki tabloda, Otomatik Ölçek özelliğiyle uyumlu küme türleri ve sürümleri açıklanmaktadır.

| Sürüm | Spark | Hive | LLAP | HBase | Kafka | Storm | ML |
|---|---|---|---|---|---|---|---|
| ESP olmadan HDInsight 3.6 | Evet | Evet | Evet | Evet* | Hayır | Hayır | Hayır |
| ESP olmadan HDInsight 4.0 | Evet | Evet | Evet | Evet* | Hayır | Hayır | Hayır |
| ESP ile HDInsight 3.6 | Evet | Evet | Evet | Evet* | Hayır | Hayır | Hayır |
| ESP ile HDInsight 4.0 | Evet | Evet | Evet | Evet* | Hayır | Hayır | Hayır |

\*HBase kümeleri yalnızca yük tabanlı değil, zamanlama tabanlı ölçekleme için yapılandırılabilir.

## <a name="how-it-works"></a>Nasıl çalışır?

HDInsight kümeniz için yük tabanlı ölçekleme veya zamanlama tabanlı ölçekleme seçebilirsiniz. Yük tabanlı ölçeklendirme, en iyi CPU kullanımını sağlamak ve çalışan maliyeti en aza indirmek için kümenizdeki düğüm sayısını, ayarladığınız bir aralık içinde değiştirir.

Zamanlama tabanlı ölçekleme, belirli zamanlarda etkili olan koşullara bağlı olarak kümenizdeki düğüm sayısını değiştirir. Bu koşullar kümeyi istenilen sayıda düğüme ölçeklendirin.

### <a name="metrics-monitoring"></a>Ölçümler izleme

Otomatik ölçeklendirme kümeyi sürekli olarak izler ve aşağıdaki ölçümleri toplar:

|Ölçüm|Açıklama|
|---|---|
|Bekleyen Toplam İşlemci|Bekleyen tüm kapsayıcıların yürütülmesini başlatmak için gereken toplam çekirdek sayısı.|
|Toplam Bekleyen Bellek|Bekleyen tüm kapsayıcıların yürütülmesini başlatmak için gereken toplam bellek (MB'de).|
|Toplam Serbest Cpu|Etkin işçi düğümlerinde kullanılmayan tüm çekirdeklerin toplamı.|
|Toplam Ücretsiz Bellek|Etkin altdüğümdeki kullanılmayan belleğin toplamı (MB'de).|
|Düğüm Başına Kullanılan Bellek|İşçi düğümündeki yük. Üzerinde 10 GB bellek kullanılan bir işçi düğümü, 2 GB kullanılmış belleğe sahip bir işçiden daha fazla yük altında kabul edilir.|
|Düğüm başına Uygulama Üstat Sayısı|Bir işçi düğümüüzerinde çalışan Uygulama Yöneticisi (AM) kapsayıcılarının sayısı. İki kapsayıcısı barındıran bir işçi düğümü, sıfır kapsayıcıları barındıran bir işçi düğümünden daha önemli kabul edilir.|

Yukarıdaki ölçümler her 60 saniyede bir kontrol edilir. Otomatik ölçeklendirme, bu ölçümlere dayalı olarak ölçeklendirme ve küçültme kararları verir.

### <a name="load-based-scale-conditions"></a>Yük tabanlı ölçek koşulları

Aşağıdaki koşullar algılandığında, Otomatik Ölçek bir ölçek isteği yayımlar:

|Ölçeği büyütme|Küçült|
|---|---|
|Toplam bekleyen CPU 3 dakikadan fazla toplam ücretsiz CPU daha büyüktür.|Bekleyen toplam CPU, 10 dakikadan uzun bir süre boyunca toplam ücretsiz CPU'dan daha azdır.|
|Toplam bekleyen bellek, 3 dakikadan fazla toplam boş bellekten daha büyüktür.|Toplam bekleyen bellek, 10 dakikadan fazla toplam boş bellekten daha azdır.|

Ölçeklendirme için HDInsight hizmeti, geçerli CPU ve bellek gereksinimlerini karşılamak için kaç yeni alt düğüm gerektiğini hesaplar ve ardından gerekli sayıda düğüm eklemek için ölçeklendirme isteği verir.

Düğüm başına kapsayıcılarının sayısına ve geçerli CPU ve bellek gereksinimlerine bağlı olarak küçültme için, otomatik ölçeklendirme belirli sayıda düğümü kaldırmak için bir istek yayınlar. Hizmet ayrıca, geçerli iş yürütme dayalı kaldırma için aday hangi düğümleri algılar. Küçültme işlemi önce düğümleri devre dışı bırakıp kümeden kaldırır.

## <a name="get-started"></a>Kullanmaya başlayın

### <a name="create-a-cluster-with-load-based-autoscaling"></a>Yük tabanlı Otomatik ölçekleme ile bir küme oluşturma

Yük tabanlı ölçekleme yle Otomatik Ölçeklendirme özelliğini etkinleştirmek için, normal küme oluşturma işleminin bir parçası olarak aşağıdaki adımları tamamlayın:

1. Yapılandırma **+ fiyatlandırma** sekmesinde **otomatik ölçeklendir** onay kutusunu etkinleştir'i seçin.
1. **Otomatik Ölçek türü**altında Yük **tabanlı'yı** seçin.
1. Aşağıdaki özellikler için istenen değerleri girin:  

    * **İşçi düğümü**için **başdüğüm sayısı.**
    * İşçi düğümlerinin **Min** sayısı.
    * **Maksimum** işçi düğüm sayısı.

    ![İşçi düğümü yük tabanlı otomatik ölçeklendirmeyi etkinleştirme](./media/hdinsight-autoscale-clusters/azure-portal-cluster-configuration-pricing-autoscale.png)

İlk işçi düğüm sayısı minimum ve maksimum, dahil arasında olmalıdır. Bu değer, oluşturulduğunda kümenin başlangıç boyutunu tanımlar. En az sayıda çalışan düğüm üç veya daha fazla olarak ayarlanmalıdır. Kümenizi üçdüğümden daha az ölçekleme, yetersiz dosya çoğaltma nedeniyle kümenin güvenli modda sıkışıp kalmasına neden olabilir.  Daha fazla bilgi için bkz: [Güvenli modda takılıp kalma.](./hdinsight-scaling-best-practices.md#getting-stuck-in-safe-mode)

### <a name="create-a-cluster-with-schedule-based-autoscaling"></a>Zamanlama tabanlı Otomatik ölçekleme ile bir küme oluşturma

Zamanlama tabanlı ölçeklemeyle Otomatik Ölçeklendirme özelliğini etkinleştirmek için, normal küme oluşturma işleminin bir parçası olarak aşağıdaki adımları tamamlayın:

1. Yapılandırma **+ fiyatlandırma** sekmesinde otomatik **ölçeklendir** onay kutusunu etkinleştir'i işaretleyin.
1. Kümeyi ölçekleme sınırını kontrol eden **İşçi düğümü**için **düğüm sayısını** girin.
1. **Otomatik Ölçek türü**altında Zamanlama **tabanlı** seçeneği seçin.
1. **Otomatik Ölçek yapılandırma** penceresini açmak için **Yapıla'yı** seçin.
1. Saat dilimini seçin ve ardından **+ Ekle koşulunu** tıklatın
1. Yeni koşulun geçerli olması gereken haftanın günlerini seçin.
1. Koşulun etkili olması gereken zamanı ve kümenin ölçeklendirilmesi gereken düğüm sayısını değiştirin.
1. Gerekirse daha fazla koşul ekleyin.

    ![Alt düğüm çizelgesi tabanlı oluşturmayı etkinleştirme](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-schedule-creation.png)

Düğüm sayısı, koşul eklemeden önce girdiğiniz en fazla işçi düğümü sayısı ile 3 arasında olmalıdır.

### <a name="final-creation-steps"></a>Son oluşturma adımları

Hem yük tabanlı hem de zamanlama tabanlı ölçekleme için, **Düğüm boyutu**altındaki açılır listeden bir VM seçerek işçi düğümleri için VM türünü seçin. Her düğüm türü için VM türünü seçtikten sonra, tüm küme için tahmini maliyet aralığını görebilirsiniz. VM türlerini bütçenize uyacak şekilde ayarlayın.

![Alt düğüm çizelgesi tabanlı otomatik ölçek düğüm boyutunu etkinleştirme](./media/hdinsight-autoscale-clusters/azure-portal-cluster-configuration-pricing-vmsize.png)

Aboneliğinizin her bölge için bir kapasite kotası vardır. Baş düğümlerinizin toplam çekirdek sayısı ve maksimum işçi düğümü sayısı kapasite kotasını aşamaz. Ancak, bu kota yumuşak bir sınırdır; her zaman kolayca artmış almak için bir destek bileti oluşturabilirsiniz.

> [!Note]  
> Toplam çekirdek kota sınırını aşarsanız, 'bu bölgedeki kullanılabilir çekirdekleri aşan maksimum düğüm, lütfen başka bir bölge seçin veya kotayı artırmak için desteğe başvurun' diyen bir hata iletisi alırsınız.

Azure portalını kullanarak HDInsight küme oluşturma hakkında daha fazla bilgi için, [Azure portalını kullanarak HDInsight'ta Linux tabanlı kümeler oluştur'a](hdinsight-hadoop-create-linux-clusters-portal.md)bakın.  

### <a name="create-a-cluster-with-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu yla küme oluşturma

#### <a name="load-based-autoscaling"></a>Yük tabanlı otomatik ölçekleme

Azure Kaynak Yöneticisi şablonuna yük tabanlı Otomatik Ölçekleme `autoscale` ile, özellikleri `computeProfile`  >  `workernode` `minInstanceCount` olan bölüme bir düğüm `maxInstanceCount` ekleyerek ve aşağıdaki json snippet'inde gösterildiği gibi bir HDInsight kümesi oluşturabilirsiniz.

```json
{
  "name": "workernode",
  "targetInstanceCount": 4,
  "autoscale": {
      "capacity": {
          "minInstanceCount": 3,
          "maxInstanceCount": 10
      }
  },
  "hardwareProfile": {
      "vmSize": "Standard_D13_V2"
  },
  "osProfile": {
      "linuxOperatingSystemProfile": {
          "username": "[parameters('sshUserName')]",
          "password": "[parameters('sshPassword')]"
      }
  },
  "virtualNetworkProfile": null,
  "scriptActions": []
}
```

Kaynak Yöneticisi şablonları ile kümeoluşturma hakkında daha fazla bilgi için, [Kaynak Yöneticisi şablonlarını kullanarak HDInsight'ta Apache Hadoop kümeleri oluştur'a](hdinsight-hadoop-create-linux-clusters-arm-templates.md)bakın.  

#### <a name="schedule-based-autoscaling"></a>Zamanlama tabanlı otomatik ölçekleme

Bölüme `autoscale` bir düğüm `computeProfile`  >  ekleyerek, zamanlama tabanlı Bir Azure Kaynak Yöneticisi şablonu otomatik ölçekleme ile bir HDInsight kümesi oluşturabilirsiniz. `workernode` Düğüm, `autoscale` değişikliğin `recurrence` ne zaman `timezone` `schedule` gerçekleşeceğini açıklayan bir a içerir.

```json
{
  "autoscale": {
    "recurrence": {
      "timeZone": "Pacific Standard Time",
      "schedule": [
        {
          "days": [
            "Monday",
            "Tuesday",
            "Wednesday",
            "Thursday",
            "Friday"
          ],
          "timeAndCapacity": {
            "time": "11:00",
            "minInstanceCount": 10,
            "maxInstanceCount": 10
          }
        },
      ]
    }
  },
  "name": "workernode",
  "targetInstanceCount": 4,
}
```

### <a name="enable-and-disable-autoscale-for-a-running-cluster"></a>Çalışan bir küme için Otomatik Ölçek'i etkinleştirme ve devre dışı

#### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Çalışan kümede Otomatik Ölçeklendirme'yi etkinleştirmek için **Ayarlar**altında **Küme boyutunu** seçin. Ardından **otomatik ölçeklendir'i etkinleştir'i**seçin. İstediğiniz Otomatik Ölçekleme türünü seçin ve yük tabanlı veya zamanlama tabanlı ölçeklendirme seçeneklerini girin. Son olarak, **Kaydet'i**seçin.

![Alt düğüm çizelgesi tabanlı otomatik ölçeklendirme kümesini etkinleştirme](./media/hdinsight-autoscale-clusters/azure-portal-settings-autoscale.png)

#### <a name="using-the-rest-api"></a>REST API’sini kullanma

REST API'yi kullanarak çalışan bir kümede Otomatik Ölçeklendirme'yi etkinleştirmek veya devre dışı kakmak için, aşağıdaki kod snippet'inde gösterildiği gibi Otomatik Ölçek uç noktasına bir POST isteğinde bulunun:

```
https://management.azure.com/subscriptions/{subscription Id}/resourceGroups/{resourceGroup Name}/providers/Microsoft.HDInsight/clusters/{CLUSTERNAME}/roles/workernode/autoscale?api-version=2018-06-01-preview
```

İstek yükündeki uygun parametreleri kullanın. Aşağıdaki json yükü Otomatik Ölçeklendirmeyi etkinleştirmek için kullanılabilir. Otomatik Ölçeklendirmeyi devre dışı `{autoscale: null}` kullanabilirsiniz.

```json
{ autoscale: { capacity: { minInstanceCount: 3, maxInstanceCount: 2 } } }
```

Tüm yük parametrelerinin tam açıklaması için [yük tabanlı otomatik ölçeklendirmeyi etkinleştirme](#load-based-autoscaling) yle ilgili önceki bölüme bakın.

## <a name="best-practices"></a>En iyi uygulamalar

### <a name="choosing-load-based-or-schedule-based-scaling"></a>Yük tabanlı veya zamanlama tabanlı ölçekleme seçme

Hangi modu seçeceğiniz konusunda karar vermeden önce aşağıdaki faktörleri göz önünde bulundurun:

* Küme oluşturma sırasında Otomatik Ölçeklendirme'yi etkinleştirin.
* En az düğüm sayısı en az üç olmalıdır.
* Yük farkı: kümenin yükü belirli zamanlarda, belirli günlerde tutarlı bir desen izler. Değilse, yük tabanlı zamanlama daha iyi bir seçenektir.
* SLA gereksinimleri: Otomatik ölçeklendirme tahminyerine reaktiftir. Yükün artmaya başladığı ve kümenin hedef boyutunda olması gerektiği zaman arasında yeterli gecikme olacak mı? Sıkı SLA gereksinimleri varsa ve yük bilinen sabit bir desense, 'zamanlama tabanlı' daha iyi bir seçenektir.

### <a name="consider-the-latency-of-scale-up-or-scale-down-operations"></a>Küçültme veya küçültme işlemlerinin gecikmesini göz önünde bulundurun

Bir ölçekleme işleminin tamamlanması 10 ila 20 dakika sürebilir. Özelleştirilmiş bir zamanlama ayarlarken, bu gecikmeyi planlayın. Örneğin, küme boyutunun 09:00'da 20 olması gerekiyorsa, ölçekleme işleminin 09:00'da tamamlanması için zamanlama tetikleyicisini sabah 8:30 gibi daha erken bir zamana ayarlayın.

### <a name="preparation-for-scaling-down"></a>Ölçekleme için hazırlık

Küme ölçeklendirme işlemi sırasında, Otomatik ölçeklendirme hedef boyutu karşılamak için düğümleri devre dışı bırakmak olacaktır. Bu düğümlerde çalışan görevler varsa, Otomatik Ölçek görevler tamamlanana kadar bekler. Her alt düğüm de HDFS'de bir rol hizmet ettiği için, geçici veriler kalan düğümlere kaydırılır. Bu nedenle, kalan düğümlerde tüm geçici verileri barındıracak yeterli alan olduğundan emin olmalısınız.

Çalışan işler çalışmaya ve bitirmeye devam edecektir. Bekleyen işler, daha az kullanılabilir işçi düğümüyle normal olarak zamanlanmayı bekler.

### <a name="minimum-cluster-size"></a>Minimum küme boyutu

Kümenizi üç düğümden daha az boyutlandırmayın. Kümenizi üçdüğümden daha az ölçekleme, yetersiz dosya çoğaltma nedeniyle kümenin güvenli modda sıkışıp kalmasına neden olabilir.  Daha fazla bilgi için bkz: [Güvenli modda takılıp kalma.](./hdinsight-scaling-best-practices.md#getting-stuck-in-safe-mode)

## <a name="monitoring"></a>İzleme

### <a name="cluster-status"></a>Küme durumu

Azure portalında listelenen küme durumu, Otomatik Ölçeklendirme etkinliklerini izlemenize yardımcı olabilir.

![Alt düğüm yük tabanlı otomatik ölçek küme durumunu etkinleştirme](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-cluster-status.png)

Görebileceğiniz küme durum iletilerinin tümü aşağıdaki listede açıklanmıştır.

| Küme durumu | Açıklama |
|---|---|
| Çalışıyor | Küme normal çalışıyor. Önceki Otomatik Ölçeklendirme etkinliklerinin tümü başarıyla tamamlandı. |
| Güncelleştirme  | Küme Otomatik Ölçekyapılandırması güncelleştiriliyor.  |
| HDInsight yapılandırması  | Bir küme ölçeklendirme veya küçültme işlemi devam ediyor.  |
| Güncelleştirme Hatası  | HDInsight, Otomatik Ölçek yapılandırma güncelleştirmesi sırasında sorunlarla karşılaştı. Müşteriler güncelleştirmeyi yeniden denemeyi veya otomatik ölçeği devre dışı bırakmayı seçebilir.  |
| Hata  | Kümede bir sorun var ve kullanılabilir değil. Bu kümeyi silin ve yeni bir küme oluşturun.  |

Kümenizdeki geçerli düğüm sayısını görüntülemek için, kümeniz için Genel **Bakış** sayfasındaki **Küme boyutu** grafiğine gidin veya **Ayarlar**altında **Küme boyutunu** seçin.

### <a name="operation-history"></a>Operasyon geçmişi

Küme ölçümlerinin bir parçası olarak küme ölçeklendirme ve küçültme geçmişini görüntüleyebilirsiniz. Ayrıca, son gün, hafta veya diğer zaman dilimindeki tüm ölçekleme eylemlerini de listeleyebilirsiniz.

İzleme altında **Ölçümler'i**seçin. **Metrics** Ardından **Metrik** açılır kutusundan metrik ve **Etkin Çalışan Sayısı** **ekle'yi** seçin. Zaman aralığını değiştirmek için sağ üstteki düğmeyi seçin.

![İşçi düğümü zamanlama tabanlı otomatik ölçek ölçüm](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-chart-metric.png)

## <a name="next-steps"></a>Sonraki adımlar

En iyi [uygulamaları ölçeklemede](hdinsight-scaling-best-practices.md) kümeleri el ile ölçekleme için en iyi uygulamalar hakkında bilgi edinin
