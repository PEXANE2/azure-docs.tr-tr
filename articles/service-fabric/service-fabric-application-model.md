---
title: Azure Hizmet Kumaşı uygulama modeli
description: Uygulama ve hizmet bildirim dosyalarını kullanarak Azure Hizmet Dokusu'ndaki uygulamaları ve hizmetleri modelleme ve açıklama.
ms.topic: conceptual
ms.date: 2/23/2018
ms.openlocfilehash: 7179686b7d4ef2df267cb95ece8f83d5fb7682b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75551888"
---
# <a name="model-an-application-in-service-fabric"></a>Hizmet Kumaşı'nda bir uygulama modeli
Bu makalede, Azure Hizmet Dokusu uygulama modeline ve bir uygulama nın ve hizmetin bildirim dosyaları aracılığıyla nasıl tanımlanacak larına genel bir bakış sunulmaktadır.

## <a name="understand-the-application-model"></a>Uygulama modelini anlama
Uygulama, belirli bir işlevi veya işlevleri gerçekleştiren kurucu hizmetler topluluğudur. Bir hizmet tam ve bağımsız bir işlev gerçekleştirir ve diğer hizmetlerden bağımsız olarak başlayıp çalıştırılabilir.  Hizmet kod, yapılandırma ve verilerden oluşur. Her hizmet için kod yürütülebilir ikililerden oluşur, yapılandırma çalışma zamanında yüklenebilen hizmet ayarlarından oluşur ve veriler hizmet tarafından tüketilecek rasgele statik verilerden oluşur. Bu hiyerarşik uygulama modelindeki her bileşen bağımsız olarak sürülebilir ve yükseltilebilir.

![Servis Kumaşı uygulama modeli][appmodel-diagram]

Uygulama türü, bir uygulamanın kategorize sidir ve bir hizmet türü paketinden oluşur. Hizmet türü, bir hizmetin sınıflandırılmasıdır. Kategorilere ayırma farklı ayarlara ve yapılandırmalara sahip olabilir, ancak temel işlevsellik aynı kalır. Bir hizmetörnekleri, aynı hizmet türünün farklı hizmet yapılandırma varyasyonlarıdır.  

Uygulama ve hizmetlerin sınıfları (veya "türleri"), XML dosyaları (uygulama bildirimleri ve hizmet bildirimleri) aracılığıyla açıklanır.  Bildirimler uygulamaları ve hizmetleri açıklar ve uygulamaların kümenin görüntü deposundan anlık olarak alınabileceği şablonlardır.  Bildirimler Uygulama ve hizmet bildirimlerinde ayrıntılı olarak ele [alınmıştır.](service-fabric-application-and-service-manifests.md) ServiceManifest.xml ve ApplicationManifest.xml dosyası için şema tanımı Hizmet Kumaş SDK ve c araçları ile *yüklenir:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*. XML şema [ServiceFabricServiceModel.xsd şema belgelerinde](service-fabric-service-model-schema.md)belgelenmiştir.

Farklı uygulama örneklerinin kodu, aynı Hizmet Kumaş düğümü tarafından barındırıldığında bile ayrı işlemler olarak çalıştırılan. Ayrıca, her uygulama örneğinin yaşam döngüsü bağımsız olarak yönetilebilir (örneğin, yükseltilmiş). Aşağıdaki diyagram, uygulama türlerinin kod, yapılandırma ve veri paketlerinden oluşan hizmet türlerinden nasıl oluştuğunu gösterir. Diyagramı basitleştirmek için, her hizmet türü `ServiceType4` bu paket türlerinin bazılarını veya tümlerini içerse de yalnızca kod/config/veri paketleri gösterilir.

![Servis Kumaş ı uygulama türleri ve servis türleri][cluster-imagestore-apptypes]

Kümede etkin bir hizmet türünün bir veya daha fazla örneği olabilir. Örneğin, durum lu hizmet örnekleri veya yinelemeler, kümedeki farklı düğümlerde bulunan yinelemeler arasındaki durumu çoğaltarak yüksek güvenilirlik elde eder. Çoğaltma, kümedeki bir düğüm başarısız olsa bile hizmetin kullanılabilir olması için artıklık sağlar. [Bölümlenmiş bir hizmet,](service-fabric-concepts-partitioning.md) durumunu (ve bu duruma erişim desenlerini) kümedeki düğümler arasında daha da böler.

Aşağıdaki diyagram, uygulamalar ve hizmet örnekleri, bölümler ve yinelemeler arasındaki ilişkiyi gösterir.

![Bir hizmet içindeki bölümler ve yinelemeler][cluster-application-instances]

> [!TIP]
> Clusteradresiniz&lt;&gt;:19080/Explorerhttp://da bulunan Service Fabric Explorer aracını kullanarak bir kümedeki uygulamaların düzenini görüntüleyebilirsiniz. Daha fazla bilgi için [Service Fabric Explorer ile kümenizi Görselleştirme'ye](service-fabric-visualizing-your-cluster.md)bakın.
> 
> 


## <a name="next-steps"></a>Sonraki adımlar
- Uygulama [ölçeklenebilirliği](service-fabric-concepts-scalability.md)hakkında bilgi edinin.
- Hizmet [durumu,](service-fabric-concepts-state.md) [bölümleme](service-fabric-concepts-partitioning.md)ve [kullanılabilirlik](service-fabric-availability-services.md)hakkında bilgi edinin.
- [Uygulama ve hizmet bildirimlerinde](service-fabric-application-and-service-manifests.md)uygulamaların ve hizmetlerin nasıl tanımlandığı hakkında bilgi edinin.
- [Uygulama barındırma modelleri,](service-fabric-hosting-model.md) dağıtılan bir hizmetin yinelemeleri (veya örnekleri) ile hizmet barındırıcı işlemi arasındaki ilişkiyi açıklar.

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png


