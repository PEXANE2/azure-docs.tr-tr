---
title: Azure Hizmet Kumaşında Ölçümlerin Parçalanması
description: Service Fabric'teki ölçümler için bir strateji olarak parçalanma veya paketleme kullanma hakkında bilgi edinin. Bu teknik çok büyük hizmetler için yararlıdır.
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: bba459be4408f4a4bc438bb33b0570a91e84f2cd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75563369"
---
# <a name="defragmentation-of-metrics-and-load-in-service-fabric"></a>Servis Kumaşında ölçümlerin ve yükün parçalanması
Hizmet Kumaş Küme Kaynak Yöneticisi'nin kümedeki yük ölçümlerini yönetmek için varsayılan stratejisi yükü dağıtmaktır. Düğümlerin eşit olarak kullanılmasını sağlamak, hem çekişmeye hem de boşa giden kaynaklara yol açan sıcak ve soğuk noktalardan kaçınır. Kümedeki iş yüklerinin dağıtılması, bir hatanın belirli bir iş yükünün büyük bir yüzdesini almamasını sağladığından, hayatta kalan hatalar açısından da en güvenli durumdur. 

Service Fabric Cluster Kaynak Yöneticisi, parçalama olan yükü yönetmek için farklı bir stratejiyi destekler. Parçalanma, bir metrin kullanımını kümeye dağıtmaya çalışmak yerine birleştirilmiş olduğu anlamına gelir. Konsolidasyon, varsayılan dengeleme stratejisinin yalnızca bir ters çevirmesidir – metrik yükün ortalama standart sapmasını en aza indirmek yerine, Küme Kaynak Yöneticisi bunu artırmaya çalışır.

## <a name="when-to-use-defragmentation"></a>Parçalanma ne zaman kullanılır?
Kümedeki yükü dağıtmak, her düğümdeki bazı kaynakları tüketir. Bazı iş yükleri, olağanüstü büyük ve düğümün çoğunu veya tamamını tüketen hizmetler oluşturur. Bu gibi durumlarda, büyük iş yükleri oluşturulduğunda, herhangi bir düğümde bunları çalıştırmak için yeterli alan olmaması mümkündür. Büyük iş yükleri Hizmet Kumaşı'nda sorun değildir; Bu gibi durumlarda Küme Kaynak Yöneticisi, bu büyük iş yüküne yer açmak için kümeyi yeniden düzenlemesi gerektiğini belirler. Ancak, bu arada iş yükü kümede zamanlanması için beklemek zorunda.

Hareket etmek için çok sayıda hizmet ve durum varsa, büyük iş yükünün kümeye yerleştirilmesi uzun zaman alabilir. Kümedeki diğer iş yükleri de büyükse ve bu nedenle yeniden düzenlenmesi daha uzun sürüyorsa, bu durum daha olasıdır. Service Fabric ekibi bu senaryonun simülasyonlarında oluşturma sürelerini ölçtü. Küme kullanımı %30 ile %50 arasında yükselir gelmez büyük hizmetler oluşturmanın çok daha uzun sürdüğünü gördük. Bu senaryoyu işlemek için parçalanmayı dengeleme stratejisi olarak tanıttık. Büyük iş yükleri için, özellikle de oluşturma zamanının önemli olduğu iş yükleri için, parçalanmanın bu yeni iş yüklerinin kümede zamanlanmasına gerçekten yardımcı olduğunu bulduk.

Parçalanma ölçümlerini, hizmetlerin yükünü daha az düğüme sıkıştırmaya çalışacak şekilde Küme Kaynak Yöneticisi'ni proaktif olarak yapmak üzere yapılandırabilirsiniz. Bu, kümeyi yeniden düzenlemeden büyük hizmetler için hemen hemen her zaman yer olmasını sağlamaya yardımcı olur. Kümeyi yeniden düzenlemek zorunda olmamak, büyük iş yüklerinin hızla oluşturulmasına olanak tanır.

Çoğu insanın parçalanmaya ihtiyacı yoktur. Hizmetler genellikle küçüktür, bu nedenle kümede onlara yer bulmak zor değildir. Yeniden yapılanma mümkün olduğunda, çoğu hizmet küçük olduğundan ve hızlı ve paralel olarak taşınabildiği için, yine hızlı bir şekilde gider. Ancak, büyük hizmetleri varsa ve bunların hızlı bir şekilde oluşturulması gerekiyorsa, parçalanma stratejisi sizin içindir. Bir dahaki sefere parçalanmanın değiş tokuşunu tartışacağız. 

## <a name="defragmentation-tradeoffs"></a>Parçalanma dengeleri
Arıza, başarısız düğümlerde daha fazla hizmet çalıştırdığından, hataların etkisini artırabilir. Kümedeki kaynakların büyük iş yüklerinin oluşturulmasını beklemek üzere yedekte tutulması gerektiğinden, parçalanma maliyetleri de artırabilir.

Aşağıdaki diyagram, biri parçalanmış, diğeri parçalanamayan iki kümenin görsel bir gösterimini verir. 

<center>

![Dengeli ve Parçalanmış Kümeleri Karşılaştırma][Image1]
</center>

Dengeli durumda, en büyük hizmet nesnelerinden birini yerleştirmek için gerekli olacak hareketlerin sayısını düşünün. Parçalanmış kümede, büyük iş yükü başka hizmetlerin taşınmasını beklemek zorunda kalmadan dört veya beş düğüme yerleştirilebilir.

## <a name="defragmentation-pros-and-cons"></a>Parçalanma artıları ve eksileri
Peki bu diğer kavramsal dengeler nelerdir? Burada düşünmek şeyler hızlı bir tablo:

| Parçalanma Artıları | Parçalanma Cons |
| --- | --- |
| Büyük hizmetlerin daha hızlı oluşturulmasını sağlar |Yükü daha az düğüme yoğunlar, çekişmeyi arttırır |
| Oluşturma sırasında daha düşük veri hareketi sağlar |Hatalar daha fazla hizmeti etkileyebilir ve daha fazla karmaşaya neden olabilir |
| Gereksinimleri ve alan ıslahı zengin açıklamasısağlar |Daha karmaşık genel Kaynak Yönetimi yapılandırması |

Parçalanmış ve normal ölçümleri aynı kümede karıştırabilirsiniz. Küme Kaynak Yöneticisi, parçalanma ölçümlerini mümkün olduğunca birleştirirken diğerlerini de yaymaya çalışır. Parçalanma ve dengeleme stratejilerinin karıştırılması nın sonuçları aşağıdakiler dahil olmak üzere çeşitli faktörlere bağlıdır:
  - dengeleme ölçümlerinin sayısı ile parçalanma ölçümlerinin sayısı
  - Herhangi bir hizmetin her iki tür de ölçüm kullanıp kullanmadığı 
  - metrik ağırlıklar
  - geçerli metrik yükler
  
Tam yapılandırmagerekli belirlemek için deneme gereklidir. Üretimde parçalanma ölçümlerini etkinleştirmeden önce iş yüklerinizin ayrıntılı olarak ölçülmesini öneririz. Bu, özellikle parçalanma ve dengeli ölçümleri aynı hizmet içinde karıştırırken geçerlidir. 

## <a name="configuring-defragmentation-metrics"></a>Parçalanma ölçümlerinin yapılandırılması
Parçalanma ölçümleri yapılandırma kümede genel bir karardır ve parçalanma için tek tek ölçümler seçilebilir. Aşağıdaki config parçacıkları parçalanma için ölçümleri nasıl yapılandıracağını gösterir. Bu durumda, "Metric1" parçalanma ölçütü olarak yapılandırılırken, "Metric2" normal olarak dengelenmeye devam edecektir. 

ClusterManifest.xml:

```xml
<Section Name="DefragmentationMetrics">
    <Parameter Name="Metric1" Value="true" />
    <Parameter Name="Metric2" Value="false" />
</Section>
```

Bağımsız dağıtımlar için ClusterConfig.json veya Azure barındırılan kümeler için Template.json aracılığıyla:

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
- Küme Kaynak Yöneticisi kümeyi açıklamak için adam seçenekleri vardır. Onlar hakkında daha fazla bilgi edinmek için, [Bir Hizmet Kumaş kümesi açıklayan](service-fabric-cluster-resource-manager-cluster-description.md) bu makaleye göz atın
- Ölçümler, Service Fabric Cluster Resource Manger'ın kümedeki tüketimi ve kapasiteyi nasıl yönettiğidir. Ölçümler ve bunları nasıl yapılandıracağınız hakkında daha fazla bilgi edinmek için [bu makaleye](service-fabric-cluster-resource-manager-metrics.md) göz atın

[Image1]:./media/service-fabric-cluster-resource-manager-defragmentation-metrics/balancing-defrag-compared.png
