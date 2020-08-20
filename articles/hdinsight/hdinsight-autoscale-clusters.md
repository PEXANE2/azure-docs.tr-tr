---
title: Azure HDInsight kümelerini otomatik olarak ölçeklendirme
description: Ölçek kümelerini otomatik olarak Apache Hadoop için Azure HDInsight otomatik ölçeklendirme özelliğini kullanın
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: how-to
ms.custom: hdinsightactive,seoapr2020
ms.date: 04/29/2020
ms.openlocfilehash: 730df91d922c4bd6187748654f8184cfb7dc6ea0
ms.sourcegitcommit: cd0a1ae644b95dbd3aac4be295eb4ef811be9aaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88612716"
---
# <a name="automatically-scale-azure-hdinsight-clusters"></a>Azure HDInsight kümelerini otomatik olarak ölçeklendirme

Azure HDInsight 'ın ücretsiz otomatik ölçeklendirme özelliği, daha önce ayarlanan ölçütlere göre kümenizdeki çalışan düğümlerinin sayısını otomatik olarak artırabilir veya azaltabilir. Küme oluşturma sırasında en az ve en fazla düğüm sayısını ayarlarsınız, bir gün-saat zamanlaması veya belirli performans ölçümlerini kullanarak ölçekleme ölçütlerini oluşturun ve HDInsight platformu Rest 'i yapar.

## <a name="how-it-works"></a>Nasıl çalışır?

Otomatik Ölçeklendirme özelliği, ölçeklendirme olaylarını tetiklemek için iki tür koşul kullanır: çeşitli küme performansı ölçümleri ( *Yük tabanlı ölçeklendirme*olarak adlandırılır) ve zaman tabanlı tetikleyiciler ( *zamanlama tabanlı ölçeklendirme*olarak adlandırılır). Yük tabanlı ölçeklendirme, en iyi CPU kullanımını sağlamak ve çalışan maliyeti en aza indirmek için, kümenizdeki düğüm sayısını sizin ayarladığınız bir Aralık içinde değiştirir. Zamanlama tabanlı ölçeklendirme, belirli tarih ve saatlerle ilişkilendirdiğiniz işlemlere göre kümenizdeki düğüm sayısını değiştirir.

Aşağıdaki videoda, otomatik ölçeklendirmeyi çözen zorlukları ve HDInsight ile maliyetleri denetlemenize nasıl yardımcı olabilecek hakkında genel bir bakış sunulmaktadır.


> [!VIDEO https://www.youtube.com/embed/UlZcDGGFlZ0?WT.mc_id=dataexposed-c9-niner]

### <a name="choosing-load-based-or-schedule-based-scaling"></a>Yük tabanlı veya zamanlama tabanlı ölçeklendirmeyi seçme

Bir ölçeklendirme türü seçerken aşağıdaki faktörleri göz önünde bulundurun:

* Yük sapması: küme yükünün belirli saatlerde, belirli günlerde tutarlı bir düzende izlenmesi mi? Aksi takdirde, yük tabanlı zamanlama daha iyi bir seçenektir.
* SLA gereksinimleri: otomatik ölçeklendirme ölçeği, tahmine dayalı yerine reaktif. Yükün ne zaman artmaya başladığı ve kümenin hedef boyutunda olması gereken süre arasında yeterli gecikme olur mu? Katı SLA gereksinimleri varsa ve yük sabit bir bilinen örüntü ise, ' zamanlama tabanlı ' daha iyi bir seçenektir.

### <a name="cluster-metrics"></a>Küme ölçümleri

Otomatik ölçeklendirme, kümeyi sürekli izler ve aşağıdaki ölçümleri toplar:

|Ölçüm|Açıklama|
|---|---|
|Toplam bekleyen CPU|Tüm bekleyen kapsayıcıları yürütmeye başlamak için gereken toplam çekirdek sayısı.|
|Toplam bekleyen bellek|Tüm bekleyen kapsayıcıların yürütülmesini başlatmak için gereken toplam bellek (MB cinsinden).|
|Toplam boş CPU|Etkin çalışan düğümlerinde kullanılmayan tüm çekirdekler toplamı.|
|Toplam boş bellek|Etkin çalışan düğümlerinde kullanılmayan belleğin (MB cinsinden) toplamı.|
|Düğüm başına kullanılan bellek|Çalışan düğümündeki yük. 10 GB belleğin kullanıldığı bir çalışan düğümü, 2 GB kullanılan bellekle çalışan bir çalışandan daha fazla yük altında kabul edilir.|
|Düğüm başına uygulama ana sayısı|Çalışan düğümünde çalışan uygulama ana (Har) kapsayıcıları sayısı. İki har kapsayıcıyı barındıran bir çalışan düğümü, sıfır har kapsayıcıları barındıran bir çalışan düğümünden daha önemli kabul edilir.|

Yukarıdaki ölçümler her 60 saniyede bir denetlenir. Bu ölçülerden herhangi birini kullanarak kümeniz için ölçeklendirme işlemleri ayarlayabilirsiniz.

### <a name="load-based-scale-conditions"></a>Yük tabanlı ölçek koşulları

Aşağıdaki koşullar algılandığında otomatik ölçeklendirme bir ölçek isteği ister:

|Ölçeği büyütme|Ölçeği azalt|
|---|---|
|Toplam bekleyen CPU, 3 dakikadan uzun bir süre boyunca toplam boş CPU 'dan fazla.|Toplam bekleyen CPU, 10 dakikadan uzun bir süre için toplam boş CPU 'dan daha az.|
|Toplam bekleyen bellek, 3 dakikadan uzun bir süre boyunca toplam boş bellekten fazla.|Toplam bekleyen bellek, 10 dakikadan uzun bir süre boyunca toplam boş bellekten daha düşüktür.|

Ölçek artırma için, otomatik ölçeklendirme gereken sayıda düğüm eklemek için bir ölçek isteği verir. Ölçek artırma, geçerli CPU ve bellek gereksinimlerini karşılamak için kaç yeni çalışan düğümünün gerekli olduğunu temel alır.

Ölçek azaltma için, otomatik ölçeklendirme belirli sayıda düğümü kaldırma isteği verir. Ölçek azaltma, düğüm başına düşen saat kapsayıcısı sayısına bağlıdır. Ve geçerli CPU ve bellek gereksinimleri. Hizmet ayrıca hangi düğümlerin geçerli iş yürütmeye göre kaldırılması gerektiğini de algılar. Ölçeği azaltma işlemi, önce düğümleri yeniden komisyonlar ve sonra kümeden kaldırır.

### <a name="cluster-compatibility"></a>Küme uyumluluğu

> [!Important]
> Azure HDInsight Otomatik Ölçeklendirme özelliği, Spark ve Hadoop kümeleri için 7 Kasım 2019’da genel kullanıma sunulmuştu ve özelliğin önizleme sürümünde sağlanmayan geliştirmeler içeriyordu. 7 Kasım 2019’dan önce Spark kümesi oluşturduysanız ve kümenizde Otomatik Ölçeklendirme özelliğini kullanmak istiyorsanız, yeni küme oluşturma ve yeni kümede Otomatik Ölçeklendirme’yi etkinleştirme yolunu izlemeniz önerilir.
>
> Interactive Query (LLAP) ve HBase kümeleri için otomatik ölçeklendirme hala önizleme aşamasındadır. Otomatik ölçeklendirme yalnızca Spark, Hadoop, Interactive Query ve HBase kümelerinde kullanılabilir.

Aşağıdaki tablo, otomatik ölçeklendirme özelliğiyle uyumlu küme türlerini ve sürümlerini açıklamaktadır.

| Sürüm | Spark | Hive | LLAP | HBase | Kafka | Fırtına | ML |
|---|---|---|---|---|---|---|---|
| ESP olmadan HDInsight 3,6 | Yes | Yes | Yes | Evet* | Hayır | Hayır | Hayır |
| ESP olmadan HDInsight 4,0 | Yes | Yes | Yes | Evet* | Hayır | Hayır | Hayır |
| HDInsight 3,6, ESP ile | Yes | Yes | Yes | Evet* | Hayır | Hayır | Hayır |
| HDInsight 4,0, ESP ile | Yes | Yes | Yes | Evet* | Hayır | Hayır | Hayır |

\* HBase kümeleri, yük tabanlı değil yalnızca zamanlama tabanlı ölçeklendirme için yapılandırılabilir.

## <a name="get-started"></a>başlarken

### <a name="create-a-cluster-with-load-based-autoscaling"></a>Yük tabanlı otomatik ölçeklendirme ile küme oluşturma

Otomatik ölçeklendirme özelliğini yük tabanlı ölçeklendirmeyle etkinleştirmek için, normal küme oluşturma sürecinin bir parçası olarak aşağıdaki adımları izleyin:

1. **Yapılandırma + fiyatlandırma** sekmesinde **Otomatik ölçeklendirmeyi etkinleştir** onay kutusunu seçin.
1. **Otomatik ölçeklendirme türü**altında **Yük tabanlı** ' i seçin.
1. Aşağıdaki özellikler için amaçlanan değerleri girin:  

    * **Çalışan düğümü**için Ilk **düğüm sayısı** .
    * **En az** çalışan düğüm sayısı.
    * Çalışan düğüm sayısı **üst sınırı** .

    ![Çalışan düğümü yük tabanlı otomatik ölçeklendirmeyi etkinleştir](./media/hdinsight-autoscale-clusters/azure-portal-cluster-configuration-pricing-autoscale.png)

Çalışan düğümlerinin ilk sayısı, dahil en düşük ve en yüksek arasında olmalıdır. Bu değer, oluşturulduğu sırada kümenin başlangıç boyutunu tanımlar. Çalışan düğümlerinin minimum sayısı üç veya daha fazla olarak ayarlanmalıdır. Kümenizin üçten az düğüme ölçeklendirilmesi, yetersiz dosya çoğaltma nedeniyle güvenli modda takılmasına neden olabilir.  Daha fazla bilgi için bkz. [güvenli modda takılme](./hdinsight-scaling-best-practices.md#getting-stuck-in-safe-mode).

### <a name="create-a-cluster-with-schedule-based-autoscaling"></a>Zamanlama tabanlı otomatik ölçeklendirme ile küme oluşturma

Otomatik ölçeklendirme özelliğini zamanlama tabanlı ölçeklendirmeyle etkinleştirmek için, normal küme oluşturma sürecinin bir parçası olarak aşağıdaki adımları izleyin:

1. **Yapılandırma + fiyatlandırma** sekmesinde **Otomatik ölçeklendirmeyi etkinleştir** onay kutusunu işaretleyin.
1. **Çalışan düğümü**Için **düğüm sayısını** girin ve bu, kümenin ölçeğini ölçekleme sınırını denetler.
1. **Otomatik ölçeklendirme türü**altında **zamanlama tabanlı** seçeneğini belirleyin.
1. **Otomatik ölçeklendirme yapılandırma** penceresini açmak için **Yapılandır** ' ı seçin.
1. Saat diliminizi seçin ve **+ Koşul Ekle** ' ye tıklayın.
1. Yeni koşulun uygulanacağı haftanın günlerini seçin.
1. Koşulun etkili olması gereken süreyi ve kümenin ölçeklendirilmesi gereken düğüm sayısını düzenleyin.
1. Gerekirse daha fazla koşul ekleyin.

    ![Çalışan düğümü zamanlama tabanlı oluşturmayı etkinleştir](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-schedule-creation.png)

Düğüm sayısı 3 ile koşul eklemeden önce girmiş olduğunuz çalışan düğümü sayısının üst sınırı olmalıdır.

### <a name="final-creation-steps"></a>Son oluşturma adımları

**Düğüm boyutu**altında aşağı açılan LISTEDEN bir VM seçerek çalışan DÜĞÜMLERI için VM türünü seçin. Her düğüm türü için VM türünü seçtikten sonra, tüm küme için tahmini maliyet aralığını görebilirsiniz. VM türlerini bütçenize uyacak şekilde ayarlayın.

![Çalışan düğümü zamanlama tabanlı otomatik ölçeklendirme düğümü boyutunu etkinleştir](./media/hdinsight-autoscale-clusters/azure-portal-cluster-configuration-pricing-vmsize.png)

Aboneliğinizin her bölge için bir kapasite kotası vardır. Baş düğümlerinizin toplam çekirdek sayısı ve en yüksek çalışan düğümleri kapasite kotasını aşamaz. Ancak, bu kota bir hafif limit; her zaman kolayca arttırılabilmeniz için bir destek bileti oluşturabilirsiniz.

> [!Note]  
> Toplam çekirdek kota sınırını aşarsanız, ' en fazla düğüm bu bölgedeki kullanılabilir çekirdekleri aştığından bir hata iletisi alırsınız. lütfen başka bir bölge seçin veya kotayı artırmak için desteğe başvurun. '

Azure portal kullanarak HDInsight kümesi oluşturma hakkında daha fazla bilgi için [Azure Portal kullanarak HDInsight 'Ta Linux tabanlı kümeler oluşturma](hdinsight-hadoop-create-linux-clusters-portal.md)konusuna bakın.  

### <a name="create-a-cluster-with-a-resource-manager-template"></a>Kaynak Yöneticisi şablonuyla küme oluşturma

#### <a name="load-based-autoscaling"></a>Yük tabanlı otomatik ölçeklendirme

`autoscale` `computeProfile`  >  `workernode` Özellikler ile bölüme bir düğüm ekleyerek `minInstanceCount` ve `maxInstanceCount` aşağıdaki JSON kod parçacığında gösterildiği gibi, Azure Resource Manager şablonu yük tabanlı otomatik ölçeklendirmeyle bir HDInsight kümesi oluşturabilirsiniz. Tüm Resource Manager şablonu için bkz. [hızlı başlangıç şablonu: Loadbased otomatik ölçeklendirme özelliği etkinken Spark kümesi dağıtma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-autoscale-loadbased).

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

#### <a name="schedule-based-autoscaling"></a>Zamanlama tabanlı otomatik ölçeklendirme

Bölüme bir düğüm ekleyerek bir Azure Resource Manager şablonu zamanlama tabanlı otomatik ölçeklendirmeyle bir HDInsight kümesi oluşturabilirsiniz `autoscale` `computeProfile`  >  `workernode` . `autoscale`Düğüm, `recurrence` ' a sahip olan `timezone` ve `schedule` değişikliğin ne zaman gerçekleştireceğinizi açıklayan bir içerir. Tam bir Resource Manager şablonu için bkz. [zamanlama tabanlı otomatik ölçeklendirme etkinken Spark kümesini dağıtma](https://github.com/Azure/azure-quickstart-templates/tree/master/101-hdinsight-autoscale-schedulebased).

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
        }
      ]
    }
  },
  "name": "workernode",
  "targetInstanceCount": 4
}
```

### <a name="enable-and-disable-autoscale-for-a-running-cluster"></a>Çalışan bir küme için otomatik ölçeklendirmeyi etkinleştirme ve devre dışı bırakma

#### <a name="using-the-azure-portal"></a>Azure portalını kullanma

Çalışan bir kümede otomatik ölçeklendirmeyi etkinleştirmek için **Ayarlar**altında **küme boyutu** ' nu seçin. Sonra **Otomatik ölçeklendirmeyi etkinleştir**' i seçin. İstediğiniz otomatik ölçeklendirme türünü seçin ve yük tabanlı veya zamanlama tabanlı ölçeklendirme seçeneklerini girin. Son olarak **Kaydet**' i seçin.

![Çalışan düğümü zamanlama tabanlı otomatik ölçeklendirmeyi etkinleştirme kümesi](./media/hdinsight-autoscale-clusters/azure-portal-settings-autoscale.png)

#### <a name="using-the-rest-api"></a>REST API’sini kullanma

REST API kullanarak çalışan bir kümede otomatik ölçeklendirmeyi etkinleştirmek veya devre dışı bırakmak için, otomatik ölçeklendirme uç noktasına bir POST isteği oluşturun:

```
https://management.azure.com/subscriptions/{subscription Id}/resourceGroups/{resourceGroup Name}/providers/Microsoft.HDInsight/clusters/{CLUSTERNAME}/roles/workernode/autoscale?api-version=2018-06-01-preview
```

İstek yükünde uygun parametreleri kullanın. Aşağıdaki JSON yükü otomatik ölçeklendirmeyi etkinleştirmek için kullanılabilir. `{autoscale: null}`Otomatik ölçeklendirmeyi devre dışı bırakmak için yükü kullanın.

```json
{ "autoscale": { "capacity": { "minInstanceCount": 3, "maxInstanceCount": 5 } } }
```

Tüm yük parametrelerinin tam açıklaması için [Yük tabanlı otomatik ölçeklendirmeyi etkinleştirme](#load-based-autoscaling) konusundaki önceki bölüme bakın.

## <a name="monitoring-autoscale-activities"></a>Otomatik ölçeklendirme etkinliklerini izleme

### <a name="cluster-status"></a>Küme durumu

Azure portal listelenen küme durumu, otomatik ölçeklendirme etkinliklerini izlemenize yardımcı olabilir.

![Çalışan düğümü yük tabanlı otomatik ölçeklendirme kümesi durumunu etkinleştir](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-cluster-status.png)

Görebileceğiniz tüm küme durumu iletileri aşağıdaki listede açıklanmıştır.

| Küme durumu | Açıklama |
|---|---|
| Çalışma | Küme normal şekilde çalışıyor. Önceki otomatik ölçeklendirme etkinliklerinin tümü başarıyla tamamlandı. |
| Bilen  | Küme otomatik ölçeklendirme yapılandırması güncelleştiriliyor.  |
| HDInsight yapılandırması  | Bir küme ölçeği artırma veya genişletme işlemi devam ediyor.  |
| Güncelleştirme hatası  | HDInsight, otomatik ölçeklendirme yapılandırma güncelleştirmesi sırasında sorunlar karşıladı. Müşteriler güncelleştirmeyi yeniden denemeyi seçebilir veya otomatik ölçeklendirmeyi devre dışı bırakabilir.  |
| Hata  | Kümede bir sorun var ve kullanılabilir değil. Bu kümeyi silip yeni bir tane oluşturun.  |

Kümenizdeki düğümlerin geçerli sayısını görüntülemek için, kümenizin **genel bakış** sayfasında **küme boyutu** grafiğine gidin. Veya **Ayarlar**altında **küme boyutu** ' nu seçin.

### <a name="operation-history"></a>İşlem geçmişi

Küme ölçümlerinin bir parçası olarak küme ölçeği artırma ve ölçeği azaltma geçmişini görüntüleyebilirsiniz. Ayrıca, geçen gün, hafta veya başka bir süre içinde tüm ölçekleme eylemlerini de listeleyebilirsiniz.

**İzleme**altında **ölçümler** ' i seçin. Ardından **ölçüm** açılan kutusundan ölçüm ve **etkin çalışan sayısı** **Ekle** ' yi seçin. Zaman aralığını değiştirmek için sağ üstteki düğmeyi seçin.

![Çalışan düğümü zamanlama tabanlı otomatik ölçeklendirme ölçümünü etkinleştir](./media/hdinsight-autoscale-clusters/hdinsight-autoscale-clusters-chart-metric.png)

## <a name="other-considerations"></a>Diğer önemli noktalar

### <a name="consider-the-latency-of-scale-up-or-scale-down-operations"></a>Ölçek artırma veya azaltma işlemlerinin gecikme süresini göz önünde bulundurun

Ölçeklendirme işleminin tamamlanması 10 ila 20 dakika sürebilir. Özelleştirilmiş bir zamanlama ayarlarken bu gecikmeyi planlayın. Örneğin, küme boyutunun 9:00 ' de 20 olması gerekiyorsa, ölçeklendirme işleminin 9:00 "ile tamamlanmasını sağlamak için zamanlama tetikleyicisini 8:30 olarak daha önceki bir zamana ayarlayın.

### <a name="preparation-for-scaling-down"></a>Ölçeği azaltma hazırlığı

Küme ölçeklendirme işlemi sırasında, otomatik ölçeklendirme, hedef boyutunu karşılamak için düğümlerin yetkisini alınır. Görevler bu düğümlerde çalışıyorsa, otomatik ölçeklendirme, görevler tamamlanana kadar bekler. Her çalışan düğüm da bir rol görevi görüyor olduğundan, geçici veriler kalan düğümlere kaydıralınacaktır. Bu nedenle, tüm geçici verileri barındırmak için kalan düğümlerde yeterli alan olduğundan emin olun.

Çalışan işler devam edecektir. Bekleyen işler, daha az kullanılabilir çalışan düğümü ile zamanlamaya göre bekleyecektir.

### <a name="minimum-cluster-size"></a>En düşük küme boyutu

Kümenizi üçten az düğüme ölçeklendirmeyin. Kümenizin üçten az düğüme ölçeklendirilmesi, yetersiz dosya çoğaltma nedeniyle güvenli modda takılmasına neden olabilir.  Daha fazla bilgi için bkz. [güvenli modda takılme](./hdinsight-scaling-best-practices.md#getting-stuck-in-safe-mode).

### <a name="llap-daemons-count"></a>LLAP Daemon 'ları sayısı

Otomatik Ölçeklendirme özelliği etkinleştirilmiş LLAP kümelerinde, otomatik ölçeklendirme yukarı/aşağı olayı, LLAP Daemon 'ları sayısının etkin çalışan düğümü sayısına göre ölçeğini/ölçeğini de ölçeklendirir. Ancak Daemon 'ları sayısında bu değişiklik, ambarı 'nda **num_llap_nodes** config 'de kalıcı değildir. Hive hizmetleri el ile yeniden başlatılırsa, ambarı 'ndaki yapılandırmaya göre LLAP Daemon 'ları sayısı sıfırlanır.

Aşağıdaki senaryoyu ele alalım:
1. 3 çalışan düğümü ile bir LLAP otomatik ölçeklendirme etkin kümesi oluşturulur ve yük tabanlı otomatik ölçeklendirme, en düşük çalışan düğümleri 3 ve en fazla çalışan düğümü 10 olarak etkinleştirilir.
2. LLAP Daemon 'ları Count config, LLAP yapılandırmasına ve ambarı 'na göre 3 çalışan düğümü ile oluşturulduğundan 3 ' e sahiptir.
3. Küme üzerinde yük olması nedeniyle bir otomatik ölçeklendirme işlemi tetiklenir, küme artık 10 düğüme ölçeklendirilir.
4. Düzenli aralıklarla çalışan otomatik ölçeklendirme denetimi, LLAP Daemon 'ları Count 'un 3 olduğunu, ancak etkin çalışan düğümü sayısının 10 olduğunu, otomatik ölçeklendirme işleminin de LLAP Daemon sayısını 10 olarak artıracaktır, ancak bu değişiklik, ambarı yapılandırması-num_llap_nodes içinde kalıcı olmaz.
5. Otomatik ölçeklendirme artık devre dışı.
6. Kümede artık 10 çalışan düğümü ve 10 LLAP Daemon 'ları vardır.
7. LLAP hizmeti el ile yeniden başlatıldı.
8. Yeniden başlatma sırasında, LLAP yapılandırmasındaki num_llap_nodes yapılandırmasını denetler ve değeri 3 olarak fark eder, bu nedenle Daemon 'ları 3 örneğini alır, ancak çalışan düğümü sayısı 10 ' dur. Artık iki ile arasında bir uyumsuzluk var.

Bu durumda, geçerli etkin çalışan düğüm sayısı ile eşleşecek şekilde **Gelişmiş Hive-Interactive-env altında num_llap_node yapılandırmasını (HIVE LLAP cini çalıştırmak için olan düğüm sayısı)** el ile değiştirmemiz gerekir.

**Not**

Otomatik ölçeklendirme olayları, ambarı 'nda bulunan **en fazla toplam eşzamanlı sorgu** olan Hive yapılandırmasını değiştirmez. Yani, Hive Server 2 etkileşimli hizmeti, **LLAP Daemon 'ları sayısı, yük/zamanlamaya göre ölçeği yukarı ve aşağı ölçeklense bile, her zaman herhangi bir zamanda yalnızca verilen sayıda eşzamanlı sorgu işleyebilir**. Genel öneri, bu yapılandırmayı el ile müdahale edilmesi için en yoğun kullanım senaryosuna göre ayarlamak olacaktır. Ancak, en **az sayıda çalışan düğümü belirtilen sayıda tez AMS (maksimum eşzamanlı sorgu yapılandırmasına eşit) barındıramazsa, biri en fazla toplam eşzamanlı sorgu yapılandırması için yüksek bir değer ayarlamanın başarısız olabileceğini** unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

[Ölçek kılavuzlarından](hdinsight-scaling-best-practices.md) el ile ölçek ölçekleme yönergeleri hakkında bilgi edinin
