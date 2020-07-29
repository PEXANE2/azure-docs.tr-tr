---
title: Azure Service Fabric ölçümlerin birleştirmesi
description: Service Fabric ölçümler için bir strateji olarak birleştirme veya paketleme kullanma hakkında bilgi edinin. Bu teknik çok büyük hizmetler için yararlıdır.
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: bba459be4408f4a4bc438bb33b0570a91e84f2cd
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75563369"
---
# <a name="defragmentation-of-metrics-and-load-in-service-fabric"></a>Service Fabric ölçümler ve yük birleştirme
Kümedeki yük ölçümlerini yönetmek için Service Fabric kümesi Kaynak Yöneticisi varsayılan stratejisi, yükü dağıtmaktır. Düğümlerin eşit olarak kullanıldığından emin olmak, hem çekişmenin hem de harcanan kaynaklara yol açabilecek sıcak ve soğuk noktaları önler. İş yüklerini kümede dağıtmak, bir hatanın belirli bir iş yükünün büyük bir yüzdesine sahip olmamasını güvence altına almamasından dolayı hatalı çalışan hatalar açısından en güvenli seçenektir. 

Service Fabric kümesi Kaynak Yöneticisi, birleştirme olan yükü yönetmek için farklı bir strateji destekler. Birleştirme, bir ölçümün kullanımını küme genelinde dağıtmaya çalışmak yerine birleştirilir. Birleştirme, ölçüm yükünün ortalama standart sapmasını en aza indirmek yerine, varsayılan Dengeleme stratejisinin yalnızca bir Inversion 'si olur. Küme Kaynak Yöneticisi, bu dosyayı artırmayı dener.

## <a name="when-to-use-defragmentation"></a>Birleştirme ne zaman kullanılır?
Kümedeki yük dağıtımı, her düğümdeki kaynakların bazılarını kullanır. Bazı iş yükleri, bir düğümün en çok büyük bir kısmını veya tümünü tükettiği ve çok daha fazla hizmet oluşturur. Bu gibi durumlarda, herhangi bir düğümde çalıştırmak için yeterli alan olmadığından büyük iş yükleri oluşturulması mümkündür. Büyük iş yükleri Service Fabric bir sorun değildir; Bu durumda küme Kaynak Yöneticisi, bu büyük iş yüküne yer açmak için kümeyi yeniden düzenleme ihtiyacı olduğunu belirler. Ancak, iş yükünün kümede zamanlanmak üzere beklemesi gerekir.

Gezinmekte olan çok sayıda hizmet ve durum varsa, büyük iş yükünün kümeye yerleştirilmesi uzun zaman alabilir. Bu, kümedeki diğer iş yüklerinin de büyük olması ve bu nedenle yeniden düzenleme daha uzun sürmesi durumunda daha olasıdır. Service Fabric ekibi, bu senaryonun benzetimleri içinde oluşturma sürelerini ölçülüyor. Küme kullanımı %30 ile %50 arasında olduğundan, büyük hizmetler oluşturmanın çok daha uzun sürdüğü görüldü. Bu senaryoyu işlemek için, bir dengeleme stratejisi olarak birleştirme tanıtıldık. Büyük iş yükleri için, özellikle oluşturma zamanının önemli olduğu durumlar için birleştirme işlemi, bu yeni iş yüklerinin kümede zamanlanmasını gerçekten sağlıyoruz.

Birleştirme ölçümlerini, kümenin yükünü daha az düğüme yeniden yapılandırmayı yeniden denemek için Kaynak Yöneticisi kümeye sahip olacak şekilde yapılandırabilirsiniz. Bu, kümeyi yeniden oluşturmadan büyük hizmetler için neredeyse her zaman oda olmasını sağlamaya yardımcı olur. Kümeyi yeniden düzenlemeniz gerekmez, büyük iş yükleri hızla oluşturulmasına izin verir.

Çoğu kişi birleştirme gerektirmez. Hizmetler genellikle küçüktür, bu nedenle kümede yer alan bir oda bulmak zor değildir. Yeniden düzenleme yapılacağından, çoğu hizmetin küçük olduğu ve hızlı ve paralel şekilde taşınabileceği için bu hızlı bir şekilde devam edebilir. Ancak, büyük hizmetlerden sahipseniz ve bunların hızlı bir şekilde oluşturulmasını istiyorsanız birleştirme stratejisi sizin için olur. Daha sonra birleştirme kullanmanın avantajları tartışıyoruz. 

## <a name="defragmentation-tradeoffs"></a>Birleştirme avantajları
Başarısız olan düğümlerde daha fazla hizmet çalıştığından, birleştirme işlemi hatalara karşı canlılığını artırabilir. Küme, kümedeki kaynakların ayrılmış durumda tutulması gerektiğinden büyük iş yüklerinin oluşturulması için bekleyen maliyetleri de artırabilir.

Aşağıdaki diyagramda, biri birleştirilmiş diğeri olmayan iki kümenin görsel temsili verilmiştir. 

<center>

![Dengeli ve birleştirilmiş kümeleri karşılaştırma][Image1]
</center>

Dengeli durumda, en büyük hizmet nesnelerinden birini yerleştirmek için gereken taşıma sayısını göz önünde bulundurun. Birleştirilmiş kümede, büyük iş yükü diğer hizmetlerin taşınmasını beklemek zorunda kalmadan dört veya beş düğümlere yerleştirilebiliyor.

## <a name="defragmentation-pros-and-cons"></a>Birleştirme uzmanları ve dezavantajları
Bu nedenle bu diğer kavramsal kavramların anlamı nedir? İşte göz önünde bulundurmanız gereken hızlı bir tablo:

| Birleştirme uzmanları | Birleştirme dezavantajlarını |
| --- | --- |
| Büyük hizmetlerin daha hızlı oluşturulmasını sağlar |Yoğunlaştırıcı daha az düğüme yüklenir, çekişmeyi artırır |
| Oluşturma sırasında daha düşük veri hareketine izin vermez |Başarısızlıklar daha fazla hizmeti etkileyebilir ve daha fazla dalgalanma neden olabilir |
| Gereksinimler ve geri kazanma alanının zengin açıklamasına izin verir |Daha karmaşık genel kaynak yönetimi yapılandırması |

Aynı kümede birleştirilmiş ve normal ölçümleri karıştırabilirsiniz. Küme Kaynak Yöneticisi birleştirme ölçümlerini diğerlerini yayırken mümkün olduğunca birleştirmeye çalışır. Birleştirme ve dengeleme stratejilerini karıştırma sonuçları, aşağıdakiler de dahil olmak üzere çeşitli etkenlere bağlıdır:
  - Dengeleme ölçümleri sayısı ile birleştirme ölçümleri sayısı
  - Herhangi bir hizmetin her iki ölçüm türünü kullanıp kullanmadığını belirtir 
  - ölçüm ağırlıkları
  - geçerli ölçüm yükleri
  
Gerekli yapılandırmanın tam olarak belirlenmesi için deneme gereklidir. Üretim ortamında birleştirme ölçümlerini etkinleştirmeden önce iş yüklerinizin kapsamlı bir şekilde ölçülmenizi öneririz. Bu, özellikle de aynı hizmet içindeki birleştirme ve dengeli ölçümler karıştırılması durumunda geçerlidir. 

## <a name="configuring-defragmentation-metrics"></a>Birleştirme ölçümlerini yapılandırma
Birleştirme ölçümlerini yapılandırmak, kümede küresel bir karardır ve tek tek ölçümler birleştirme için seçilebilir. Aşağıdaki yapılandırma parçacıkları, birleştirme için ölçümlerin nasıl yapılandırılacağını gösterir. Bu durumda, "Metric1" bir birleştirme ölçümü olarak yapılandırılmıştır, ancak "Metric2" normal şekilde dengeolmaya devam edecektir. 

ClusterManifest.xml:

```xml
<Section Name="DefragmentationMetrics">
    <Parameter Name="Metric1" Value="true" />
    <Parameter Name="Metric2" Value="false" />
</Section>
```

Tek başına dağıtımlar veya Azure 'da barındırılan kümeler için Template.jsClusterConfig.jsaracılığıyla:

```json
"fabricSettings": [
  {
    "name": "DefragmentationMetrics",
    "parameters": [
      {
          "name": "Metric1",
          "value": "true"
      },
      {
          "name": "Metric2",
          "value": "false"
      }
    ]
  }
]
```


## <a name="next-steps"></a>Sonraki adımlar
- Küme Kaynak Yöneticisi, kümeyi açıklamak için Man seçeneklerine sahiptir. Bunlarla ilgili daha fazla bilgi edinmek için [Service Fabric kümesini açıklama](service-fabric-cluster-resource-manager-cluster-description.md) konusunda bu makaleye göz atın
- Ölçümler, Service Fabric küme kaynağı Yöneticisi 'nin kümedeki tüketimi ve kapasiteyi nasıl yönettiğini açıklamaktadır. Ölçümler ve bunların nasıl yapılandırılacağı hakkında daha fazla bilgi edinmek için [Bu makaleye](service-fabric-cluster-resource-manager-metrics.md) göz atın

[Image1]:./media/service-fabric-cluster-resource-manager-defragmentation-metrics/balancing-defrag-compared.png
