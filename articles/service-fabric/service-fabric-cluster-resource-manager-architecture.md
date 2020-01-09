---
title: Kaynak Yöneticisi mimarisi
description: Azure Service Fabric Cluster Kaynak Yöneticisi hizmetiyle ilgili bir genel bakış ve mimari bilgiler.
author: masnider
ms.topic: conceptual
ms.date: 08/18/2017
ms.author: masnider
ms.openlocfilehash: 94ed906533d108081d620e9b183ecfee249d85ca
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/31/2019
ms.locfileid: "75551701"
---
# <a name="cluster-resource-manager-architecture-overview"></a>Küme Resource Manager mimarisine genel bakış
Service Fabric kümesi Kaynak Yöneticisi, kümede çalışan merkezi bir hizmettir. Özellikle kaynak tüketimine ve herhangi bir yerleştirme kuralına göre kümedeki hizmetlerin istenen durumunu yönetir. 

Kümenizdeki kaynakları yönetmek için Service Fabric kümesi Kaynak Yöneticisi birkaç bilgi parçasına sahip olmalıdır:

- Şu anda hangi hizmetleri var
- Her hizmetin geçerli (veya varsayılan) kaynak tüketimi 
- Kalan küme kapasitesi 
- Kümedeki düğümlerin kapasitesi 
- Her düğümde tüketilen kaynak miktarı

Belirli bir hizmetin kaynak tüketimi zaman içinde değişebilir ve hizmetler genellikle birden fazla kaynak türü hakkında bilgi alabilir. Farklı hizmetlerde, ölçülen gerçek fiziksel ve fiziksel kaynaklar olabilir. Hizmetler, bellek ve disk tüketimi gibi fiziksel ölçümleri izleyebilir. Daha yaygın olarak hizmetler, "WorkQueueDepth" veya "TotalRequests" gibi mantıksal ölçümler hakkında bilgi alabilir. Hem mantıksal hem de fiziksel ölçümler aynı kümede kullanılabilir. Ölçümler birçok hizmet arasında paylaşılabilir veya belirli bir hizmete özel olabilir.

## <a name="other-considerations"></a>Diğer konular
Kümenin sahipleri ve işleçleri, hizmet ve uygulama yazarlarından farklı olabilir veya en azından farklı hastaları takan kişilerle aynıdır. Uygulamanızı geliştirirken, ne gerektirdiği hakkında birkaç şeyi bilirsiniz. Kullanacağı kaynakların tahmini ve farklı hizmetlerin dağıtılması gerekir. Örneğin, web katmanının Internet 'e açık olan düğümlerde çalıştırılması gerekir, ancak veritabanı Hizmetleri ' nin olmaması gerekir. Diğer bir örnek olarak, Web Hizmetleri büyük olasılıkla CPU ve ağ tarafından kısıtlanıyor olsa da, veri katmanı Hizmetleri bellek ve disk tüketimine daha fazla önem vermez. Bununla birlikte, üretimde bu hizmet için canlı site olayını işleyen veya hizmette yükseltmeyi yöneten kişinin farklı bir işi vardır ve farklı araçlar gerekir. 

Küme ve hizmetler dinamiktir:

- Kümedeki düğümlerin sayısı büyüyebilir ve küçülebilir
- Farklı boyutlardaki ve türlerin düğümleri gelebilir ve şu şekilde olabilir
- Hizmetler, istenen kaynak ayırmaları ve yerleştirme kurallarını oluşturabilir, kaldırılabilir ve değiştirebilir
- Yükseltmeler veya diğer yönetim işlemleri altyapı düzeylerinde uygulama üzerinde küme aracılığıyla alabilir
- Her zaman bir başarısızlık meydana gelebilir.

## <a name="cluster-resource-manager-components-and-data-flow"></a>Küme Kaynak Yöneticisi bileşenleri ve veri akışı
Küme Kaynak Yöneticisi, her bir hizmetin gereksinimlerini ve bu hizmetlerin içindeki her bir hizmet nesnesine göre kaynak tüketimini izlemek zorunda. Küme Kaynak Yöneticisi iki kavramsal bölümden oluşur: her düğümde çalışan aracılar ve hataya dayanıklı bir hizmet. Her düğümdeki aracılar hizmetlerden gelen yükleme raporlarını izler, bunları toplar ve düzenli olarak raporlar. Küme Kaynak Yöneticisi hizmeti yerel aracılardan tüm bilgileri toplar ve geçerli yapılandırmasına göre yeniden çalışır.

Aşağıdaki diyagrama bakalım:

<center>

![Kaynak Dengeleyici mimarisi][Image1]
</center>

Çalışma zamanı sırasında oluşabilecek birçok değişiklik vardır. Örneğin, bazı hizmetlerin değişiklik tükettiği kaynak miktarını, bazı hizmetleri başarısız olduğunu ve bazı düğümlerin kümeye katılıp ayrılmaalım. Bir düğümdeki tüm değişiklikler toplanır ve Küme Kaynak Yöneticisi hizmetine (1, 2) yeniden toplanmış, çözümlendikleri ve depolandıklarında düzenli aralıklarla gönderilir. Hizmetin değişikliklere baktığı ve herhangi bir eylemin gerekli olup olmadığını belirlediği (3) her birkaç saniye. Örneğin, kümeye bazı boş düğümlerin eklendiğini fark edebilirsiniz. Sonuç olarak, bazı hizmetleri bu düğümlere taşımaya karar verir. Küme Kaynak Yöneticisi, belirli bir düğümün aşırı yüklenmiş olduğunu veya belirli hizmetlerin başarısız olduğunu ya da silindiğini, kaynakları başka bir yerde boşaltmasını de fark edebilirsiniz.

Aşağıdaki diyagrama göz atalım ve ileri ' ye bakabilirsiniz. Kümenin Kaynak Yöneticisi değişikliklerin gerekli olduğunu belirlediğini varsayalım. Gerekli değişiklikleri yapmak için diğer sistem hizmetleriyle (özellikle Yük Devretme Yöneticisi) eşgüdümünü sağlar. Ardından gerekli komutlar uygun düğümlere (4) gönderilir. Örneğin, Kaynak Yöneticisi Düğüm5 aşırı yüklenmiş olduğunu fark edelim ve bu nedenle B hizmetini Düğüm5 ' den Düğüm4 ' a taşımaya karar verdim. Yeniden yapılandırma (5) sonunda, küme şöyle görünür:

<center>

![Kaynak Dengeleyici mimarisi][Image2]
</center>

## <a name="next-steps"></a>Sonraki adımlar
- Küme Kaynak Yöneticisi, kümeyi açıklamak için birçok seçenek içerir. Bunlarla ilgili daha fazla bilgi edinmek için [Service Fabric kümesini açıklama](./service-fabric-cluster-resource-manager-cluster-description.md) konusunda bu makaleye göz atın
- Küme Kaynak Yöneticisi birincil görevleri, kümeyi yeniden dengeleyerek yerleştirme kurallarını zorunlu tutacaktır. Bu davranışları yapılandırma hakkında daha fazla bilgi için bkz. [Service Fabric kümenizin dengelenmesi](./service-fabric-cluster-resource-manager-balancing.md)

[Image1]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-1.png
[Image2]:./media/service-fabric-cluster-resource-manager-architecture/Service-Fabric-Resource-Manager-Architecture-Activity-2.png
