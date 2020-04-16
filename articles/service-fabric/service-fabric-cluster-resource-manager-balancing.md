---
title: Azure Hizmet Kumaşı kümenizi dengeleyin
description: Service Fabric Cluster Resource Manager ile kümenizi dengelemeye giriş.
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: b6df25b525975f2d4fe6a02064e81f359a804c58
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81416262"
---
# <a name="balancing-your-service-fabric-cluster"></a>Servis kumaş kümenizi dengeleme
Service Fabric Cluster Kaynak Yöneticisi, düğüm veya hizmetlerin eklemelerine veya kaldırılmasına tepki olarak dinamik yük değişikliklerini destekler. Ayrıca kısıtlama ihlallerini otomatik olarak düzeltir ve kümeyi proaktif olarak yeniden dengeler. Ama bu eylemler ne sıklıkta yapılır ve onları ne tetikler?

Küme Kaynak Yöneticisi'nin gerçekleştirdiği üç farklı çalışma kategorisi vardır. Bunlar:

1. Yerleşim - bu aşamada eksik herhangi bir durum yinelemeleri veya stateless örnekleri yerleştirerek fırsatlar. Yerleşim, hem yeni hizmetleri hem de durum daki yinelemeleri veya başarısız olan durum suz örnekleri işlemeyi içerir. Çoğaltmaları veya örnekleri siler ve bırakarak burada ele alınır.
2. Kısıtlama Denetimleri – bu aşama, sistem deki farklı yerleşim kısıtlamalarının (kuralları) ihlallerini denetler ve düzeltir. Kurallara örnek olarak düğümlerin kapasiteyi aşmamasını ve bir hizmetin yerleşim kısıtlamalarının karşılanmasını sağlamak gibi örnekler verilebilir.
3. Dengeleme – bu aşama, farklı ölçümler için yapılandırılan istenilen denge düzeyine bağlı olarak yeniden dengelemenin gerekli olup olmadığını kontrol eder. Eğer öyleyse daha dengeli kümede bir düzenleme bulmaya çalışır.

## <a name="configuring-cluster-resource-manager-timers"></a>Küme Kaynak Yöneticisi Zamanlayıcılarını Yapılandırma
Dengeleme etrafında ki ilk denetim kümesi bir dizi zamanlayıcıdır. Bu zamanlayıcılar, Küme Kaynak Yöneticisi'nin kümeyi ne sıklıkta inceleyeceğini ve düzeltici eylemlerde denebildiğini yönetir.

Küme Kaynak Yöneticisi'nin yapabileceğiniz bu farklı düzeltme türlerinin her biri, sıklığını yöneten farklı bir zamanlayıcı tarafından denetlenir. Her zamanlayıcı yangınyaptığında, görev zamanlanır. Varsayılan olarak Kaynak Yöneticisi:

* durumunu tarar ve güncellemeleri uygular (bir düğümün aşağı olduğunu kaydetmek gibi) saniyenin 1/10'u
* her saniye yerleşim kontrol bayrağını ayarlar
* her saniye kısıtlama denetim bayrağını ayarlar
* her beş saniyede bir dengeleme bayrağını ayarlar

Bu zamanlayıcıları yöneten yapılandırma örnekleri aşağıda verilmiştir:

ClusterManifest.xml:

``` xml
        <Section Name="PlacementAndLoadBalancing">
            <Parameter Name="PLBRefreshGap" Value="0.1" />
            <Parameter Name="MinPlacementInterval" Value="1.0" />
            <Parameter Name="MinConstraintCheckInterval" Value="1.0" />
            <Parameter Name="MinLoadBalancingInterval" Value="5.0" />
        </Section>
```

Bağımsız dağıtımlar için ClusterConfig.json veya Azure barındırılan kümeler için Template.json aracılığıyla:

```json
"fabricSettings": [
  {
    "name": "PlacementAndLoadBalancing",
    "parameters": [
      {
          "name": "PLBRefreshGap",
          "value": "0.10"
      },
      {
          "name": "MinPlacementInterval",
          "value": "1.0"
      },
      {
          "name": "MinConstraintCheckInterval",
          "value": "1.0"
      },
      {
          "name": "MinLoadBalancingInterval",
          "value": "5.0"
      }
    ]
  }
]
```

Bugün Küme Kaynak Yöneticisi, bu eylemlerden yalnızca birini sırayla gerçekleştirir. Bu nedenle, bu zamanlayıcılara "minimum aralıklar" ve zamanlayıcılar "bayrak ayarlama" olarak patladığında alınan eylemleri ifade ediyoruz. Örneğin, Küme Kaynak Yöneticisi küme dengelemeden önce hizmet oluşturmak için bekleyen istekleri ilgilenir. Belirtilen varsayılan zaman aralıklarında görebileceğiniz gibi, Küme Kaynak Yöneticisi sık sık yapması gereken her şeyi tarar. Normalde bu, her adım sırasında yapılan değişiklik kümesinin küçük olduğu anlamına gelir. Sık sık küçük değişiklikler yapmak, kümede bir şeyler olduğunda Küme Kaynak Yöneticisi'nin yanıt vermesini sağlar. Aynı tür olayların çoğu aynı anda meydana gelme eğiliminde olduğundan varsayılan zamanlayıcılar bazı toplu iş sağlar. 

Örneğin, düğümler başarısız olduğunda, aynı anda tüm hata etki alanları bunu yapabilirsiniz. Tüm bu hatalar *PLBRefreshGap*sonra bir sonraki durum güncellemesi sırasında yakalanır. Düzeltmeler aşağıdaki yerleştirme, kısıtlama denetimi ve dengeleme çalıştırmaları sırasında belirlenir. Varsayılan olarak Küme Kaynak Yöneticisi kümedeki saatli değişiklikleri taramaz ve tüm değişiklikleri aynı anda ele almaya çalışır. Bunu yapmak, çalkalanma patlamalarına yol açar.

Küme Kaynak Yöneticisi de küme dengesiz olup olmadığını belirlemek için bazı ek bilgilere ihtiyacı var. Bunun için iki yapılandırma parçası daha var: *Dengeleme Eşikleri* ve *Aktivite Eşikleri.*

## <a name="balancing-thresholds"></a>Dengeleme eşikleri
Dengeleme Eşiği, yeniden dengelemeyi tetiklemek için ana denetimdir. Bir metrik için Dengeleme Eşiği bir _orandır._ En yüklü düğümdeki bir metnin yükü, en az yüklenen düğümdeki yük miktarına bölünürse, bu ölçümün *BalancingThreshold'u*aşarsa, küme dengesizdir. Sonuç olarak dengeleme küme kaynak yöneticisi denetler bir sonraki kez tetiklenir. *MinLoadBalancingInterval* zamanlayıcısı, Küme Kaynak Yöneticisi'nin yeniden dengeleme nin gerekli olup olmadığını ne sıklıkta denetlemesi gerektiğini tanımlar. Kontrol etmek bir şey olduğu anlamına gelmez. 

Dengeleme Eşikleri küme tanımının bir parçası olarak metrik bazında tanımlanır. Ölçümler hakkında daha fazla bilgi için [bu makaleye](service-fabric-cluster-resource-manager-metrics.md)göz atın.

ClusterManifest.xml

```xml
    <Section Name="MetricBalancingThresholds">
      <Parameter Name="MetricName1" Value="2"/>
      <Parameter Name="MetricName2" Value="3.5"/>
    </Section>
```

Bağımsız dağıtımlar için ClusterConfig.json veya Azure barındırılan kümeler için Template.json aracılığıyla:

```json
"fabricSettings": [
  {
    "name": "MetricBalancingThresholds",
    "parameters": [
      {
          "name": "MetricName1",
          "value": "2"
      },
      {
          "name": "MetricName2",
          "value": "3.5"
      }
    ]
  }
]
```

<center>

![Dengeleme Eşiği Örneği][Image1]
</center>

Bu örnekte, her hizmet bir metrik bir birim tüketiyor. En üstteki örnekte, bir düğümüzerindeki en büyük yük beştir ve en az ikidir. Bu ölçüm için dengeleme eşiğinin üç olduğunu varsayalım. Kümedeki oran 5/2 = 2,5 olduğundan ve bu üç tanımlı dengeleme eşiğinin altında olduğundan, küme dengelenir. Küme Kaynak Yöneticisi denetlediğinde dengeleme tetiklenmez.

Alttaki örnekte, bir düğümüzerindeki en büyük yük 10 iken, en az iki yük türve beş oran ile sonuçlanır. Beş, bu metrik için belirlenen üç dengeleme eşiğinin altından büyüktür. Sonuç olarak, dengeleme zamanlayıcısı bir dahaki sefere yeniden dengeleme çalışması zamanlanır. Böyle bir durumda bazı yük genellikle Düğüm3 dağıtılır. Hizmet Kumaş Küme Kaynak Yöneticisi açgözlü bir yaklaşım kullanmadığından, bazı yük düğüm2'ye de dağıtılabilir. 

<center>

![Dengeleme Eşik Örnek Eylemleri][Image2]
</center>

> [!NOTE]
> "Dengeleme" kümenizdeki yükü yönetmek için iki farklı strateji işler. Küme Kaynak Yöneticisi'nin kullandığı varsayılan strateji, kümedeki düğümler arasında yük dağıtmaktır. Diğer strateji [parçalanma.](service-fabric-cluster-resource-manager-defragmentation-metrics.md) Parçalanma aynı dengeleme çalışması sırasında gerçekleştirilir. Dengeleme ve parçalanma stratejileri aynı küme içinde farklı ölçümler için kullanılabilir. Bir hizmette hem dengeleme hem de parçalanma ölçümleri olabilir. Parçalanma ölçümleri için, kümedeki yüklerin oranı dengeleme eşiğinin _altında_ olduğunda yeniden dengelemeyi tetikler. 
>

Dengeleme eşiğinin altına inmek açık bir hedef değildir. Dengeleme Eşikleri sadece bir *tetikleyicidir.* Küme Kaynak Yöneticisi çalışır dengeleme yaparken, varsa hangi iyileştirmeleri yapabileceğinizi belirler. Dengeleme araması başladı diye bir şey hareket anlamına gelmez. Bazen küme dengesiz ama düzeltmek için çok kısıtlı. Alternatif olarak, iyileştirmeler çok [pahalı](service-fabric-cluster-resource-manager-movement-cost.md)hareketleri gerektirir).

## <a name="activity-thresholds"></a>Etkinlik eşikleri
Bazen düğümler göreceli olarak dengesiz olsa da, kümedeki *toplam* yük miktarı düşüktür. Yük eksikliği geçici bir daldırma olabilir, ya da küme yeni ve sadece bootstrapped almak çünkü. Her iki durumda da, elde edilecek çok az şey olduğundan kümeyi dengelemek için zaman harcamak istemeyebilirsiniz. Küme dengeleme yapıldıysa, büyük *bir mutlak* fark yaratmadan şeyleri hareket ettirebilmek için ağ ve hesaplama kaynaklarını harcarsınız. Gereksiz hamleleri önlemek için Etkinlik Eşikleri olarak bilinen başka bir denetim vardır. Etkinlik Eşikleri, etkinlik için bazı mutlak alt sınır belirtmenize olanak tanır. Düğüm bu eşiğin üzerinde değilse, Dengeleme Eşiği karşılansa bile dengeleme tetiklenmez.

Bu ölçüm için üç dengeleme eşiğimizi koruduğumuzu söyleyebiliriz. Ayrıca 1536 aktivite eşiği olduğunu da varsayalım. İlk durumda, küme Dengeleme Eşiği başına dengesiz iken hiçbir düğüm bu Aktivite Eşiği karşılar, bu yüzden hiçbir şey olmaz. Alttaki örnekte, Düğüm1 Etkinlik Eşiğinin üzerindedir. Hem Dengeleme Eşiği hem de metrik için Etkinlik Eşiği aşıldığından, dengeleme zamanlanır. Örnek olarak, aşağıdaki diyagrama bakalım: 

<center>

![Etkinlik Eşik Örneği][Image3]
</center>

Dengeleme Eşikleri gibi, Etkinlik Eşikleri de küme tanımı ile metrik başına tanımlanır:

ClusterManifest.xml

``` xml
    <Section Name="MetricActivityThresholds">
      <Parameter Name="Memory" Value="1536"/>
    </Section>
```

Bağımsız dağıtımlar için ClusterConfig.json veya Azure barındırılan kümeler için Template.json aracılığıyla:

```json
"fabricSettings": [
  {
    "name": "MetricActivityThresholds",
    "parameters": [
      {
          "name": "Memory",
          "value": "1536"
      }
    ]
  }
]
```

Dengeleme ve etkinlik eşikleri belirli bir metriğe bağlıdır - dengeleme yalnızca aynı metrik için hem Dengeleme Eşiği hem de Etkinlik Eşiği aşıldığında tetiklenir.

> [!NOTE]
> Belirtilmediğinde, bir metnin Dengeleme Eşiği 1 ve Etkinlik Eşiği 0'dır. Bu, Küme Kaynak Yöneticisi'nin bu ölçümü belirli bir yük için mükemmel bir şekilde dengede tutmaya çalışacağı anlamına gelir. Özel ölçümler kullanıyorsanız, ölçümleriniz için kendi dengeleme ve etkinlik eşiklerinizi açıkça tanımlamanız önerilir. 
>

## <a name="balancing-services-together"></a>Hizmetleri birlikte dengeleme
Kümenin dengesiz olup olmadığı küme çapında bir karardır. Ancak, bunu düzeltme nin yolu, tek tek hizmet yinelemelerini ve örnekleri hareket ettiriyor. Bu mantıklı, değil mi? Bellek tek bir düğümüzerinde yığılmışsa, birden çok yineleme veya örnek katkıda bulunabilir. Dengesizliği gidermek için, dengesiz ölçümü kullanan durum lu yinelemelerin veya durumsuz örneklerin taşınması gerektirebilir.

Bazen olsa da, kendisi dengesiz değildi bir hizmet (daha önce yerel ve küresel ağırlıkların tartışma hatırlıyorum) taşınır alır. Tüm bu hizmet ölçümleri dengelenmişken bir hizmet neden taşınsın ki? Bir örnek görelim:

- Dört hizmet, Service1, Service2, Service3 ve Service4 olduğunu varsayalım. 
- Service1 ölçümleri Metrik1 ve Metric2'yi raporlar. 
- Service2 ölçümleri Metrik2 ve Metric3'e bildirir. 
- Service3 ölçümleri Metrik3 ve Metric4'i raporlar.
- Service4 raporları metrik Metric99. 

Burada nereye varıyoruz, bir zincir var! Gerçekten dört bağımsız hizmetimiz yok, ilgili üç hizmetimiz var ve biri kendi başına kapalı.

<center>

![Hizmetleri Birlikte Dengeleme][Image4]
</center>

Bu zincir nedeniyle, 1-4 ölçümlerindeki bir dengesizliğin 1-3 hizmetlerine ait yinelemelerin veya örneklerin hareket etmesini niçin mümkün olduğu açıktır. Ayrıca, Metrik 1, 2 veya 3'teki bir dengesizlik Service4'te hareketlere neden olamaz. Service4'e ait kopyaları veya örnekleri hareket ettiren 1-3 metrik dengesini etkilemek için kesinlikle hiçbir şey yapamayacağından, hiçbir anlamı olmaz.

Küme Kaynak Yöneticisi hangi hizmetlerin ilişkili olduğunu otomatik olarak bulur. Hizmetlerin ölçümlerini ekleme, kaldırma veya değiştirme ilişkileri etkileyebilir. Örneğin, iki dengeleme hizmeti arasında Service2, Metric2'yi kaldırmak için güncelleştirilmiş olabilir. Bu, Service1 ve Service2 arasındaki zinciri kırar. Şimdi, ilgili hizmetlerden oluşan iki grup yerine üç tane vardır:

<center>

![Hizmetleri Birlikte Dengeleme][Image5]
</center>

## <a name="next-steps"></a>Sonraki adımlar
* Ölçümler, Service Fabric Cluster Resource Manger'ın kümedeki tüketimi ve kapasiteyi nasıl yönettiğidir. Ölçümler ve bunları nasıl yapılandıracağınız hakkında daha fazla bilgi edinmek için [bu makaleye](service-fabric-cluster-resource-manager-metrics.md) göz atın
* Hareket Maliyeti, Küme Kaynak Yöneticisi'ne belirli hizmetlerin taşınmasının diğerlerinden daha pahalı olduğunu bildiren bir yoldur. Hareket maliyeti hakkında daha fazla şey için [bu makaleye](service-fabric-cluster-resource-manager-movement-cost.md) bakın
* Küme Kaynak Yöneticisi kümedeki karmaşayı yavaşlatmak için yapılandırabileceğiniz birkaç azaltma vardır. Normalde gerekli değildir, ama onlara ihtiyacınız [olursa,](service-fabric-cluster-resource-manager-advanced-throttling.md) bunları burada öğrenebilirsin.
* Küme Kaynak Yöneticisi alt kümelendirmeyi tanıyabilir ve işleyebilir (bazen yerleşim kısıtlamaları ve dengeleme kullandığınızda ortaya çıkan bir durum). Alt kümelemenin dengelemeyi nasıl etkileyebileceğini ve bununla nasıl başa çıkabileceğinizi öğrenmek için [buraya](cluster-resource-manager-subclustering.md) bakın

[Image1]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resrouce-manager-balancing-thresholds.png
[Image2]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-threshold-triggered-results.png
[Image3]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-activity-thresholds.png
[Image4]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together1.png
[Image5]:./media/service-fabric-cluster-resource-manager-balancing/cluster-resource-manager-balancing-services-together2.png
