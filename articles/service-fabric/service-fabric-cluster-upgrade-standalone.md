---
title: Azure Service Fabric tek başına kümesini yükseltme
description: Azure Service Fabric tek başına kümesinin sürümünü veya yapılandırmasını yükseltme hakkında bilgi edinin.  T
ms.topic: conceptual
ms.date: 11/12/2018
ms.openlocfilehash: 519a9163d16fda2dd9fcf49cf22fe4ad4a272e09
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86260980"
---
# <a name="upgrading-and-updating-a-service-fabric-standalone-cluster"></a>Service Fabric tek başına Kümeyi yükseltme ve güncelleştirme

Tüm modern bir sistem için, ürününüzün uzun süreli başarısını sağlamak üzere yükselme için tasarlanmaya yönelik bir anahtar vardır. Azure Service Fabric tek başına kümesi, sahip olduğunuz bir kaynaktır. Bu makalede, nelerin yükseltileceğini veya güncelleştirileceğini açıklanmaktadır.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Kümenizde çalışan yapı sürümünü denetleme
Kümenizin her zaman [desteklenen bir Service Fabric sürümü](service-fabric-versions.md)çalıştırmasını sağlayın. Microsoft yeni bir Service Fabric sürümü duyurusu yaparken, önceki sürüm, duyuru tarihinden itibaren en az 60 gün sonra destek sonuna kadar işaretlenir. Yeni yayınlar [Service Fabric ekip blogundan](https://techcommunity.microsoft.com/t5/azure-service-fabric/bg-p/Service-Fabric)duyurulur. Yeni sürüm, bu noktada seçim yapmak için kullanılabilir.

Kümenizi, Microsoft tarafından yayımlandıklarında otomatik yapı yükseltmeleri alacak şekilde ayarlayabilir veya kümenizin açık olmasını istediğiniz desteklenen bir yapı sürümünü el ile seçebilirsiniz. Daha fazla bilgi için, [kümenizde çalışan Service Fabric sürümünü yükseltin](service-fabric-cluster-upgrade-windows-server.md).

## <a name="customize-configuration-settings"></a>Yapılandırma ayarlarını özelleştirme

Küme ve düğüm özelliklerinin güvenilirlik düzeyi gibiClusterConfig.jsdosya *üzerinde* birçok farklı [yapılandırma ayarı](service-fabric-cluster-manifest.md) ayarlanabilir.  Daha fazla bilgi edinmek için [tek başına kümenin yapılandırmasını yükseltin](service-fabric-cluster-config-upgrade-windows-server.md).  Diğer birçok gelişmiş ayar de özelleştirilebilir.  Daha fazla bilgi için [Service Fabric Cluster Fabric ayarlarını](service-fabric-cluster-fabric-settings.md)okuyun.

## <a name="define-node-properties"></a>Düğüm özelliklerini tanımla
Bazen bazı iş yüklerinin kümedeki belirli düğüm türlerinde çalışmasını sağlamak isteyebilirsiniz. Örneğin, bazı iş yükleri GPU 'Lar ya da SSD 'Ler gerektirebilir, diğerleri de olmayabilir. Kümedeki düğüm türlerinin her biri için, küme düğümlerine özel düğüm özellikleri ekleyebilirsiniz. Yerleştirme kısıtlamaları, bir veya daha fazla düğüm özelliği için seçim yapan ayrı hizmetlere eklenmiş deyimlerdir. Yerleştirme kısıtlamaları, hizmetlerin nerede çalışacağını tanımlar.

Yerleştirme kısıtlamalarının, düğüm özelliklerinin kullanımı ve bunların nasıl tanımlanacağı hakkında ayrıntılar için, [düğüm özelliklerini ve yerleştirme kısıtlamalarını](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints)okuyun.
 

## <a name="add-capacity-metrics"></a>Kapasite ölçümleri ekleme
Düğüm türlerinin her biri için, yük bildirmek üzere uygulamalarınızda kullanmak istediğiniz özel kapasite ölçümlerini ekleyebilirsiniz. Yük raporlamak için kapasite ölçümlerinin kullanımı hakkında daha fazla bilgi için, [küme](service-fabric-cluster-resource-manager-cluster-description.md) ve [ölçümleri ve yük](service-fabric-cluster-resource-manager-metrics.md)hakkındaki bilgileri Kaynak Yöneticisi Service Fabric kümesine bakın.

## <a name="patch-the-os-in-the-cluster-nodes"></a>Küme düğümlerinde işletim sistemini yama
Düzeltme Eki düzenleme uygulaması (POA), kapalı kalma süresi olmadan bir Service Fabric kümesinde işletim sistemi düzeltme eki uygulamayı otomatikleştiren Service Fabric bir uygulamadır. [Windows Için düzeltme eki düzenleme uygulaması](service-fabric-patch-orchestration-application.md) , her zaman kullanılabilir durumda tutulması sırasında düzeltme eklerini düzenli olarak yüklemek üzere kümenize dağıtılabilir. 


## <a name="next-steps"></a>Sonraki adımlar
* [Service Fabric kümesi doku ayarlarından](service-fabric-cluster-fabric-settings.md) bazılarını özelleştirmeyi öğrenin
* [Kümenizi ve dışarı ölçeklendirmeyi nasıl ölçeklentireceğinizi](service-fabric-cluster-scale-in-out.md) öğrenin
* [Uygulama yükseltmeleri](service-fabric-application-upgrade.md) hakkında bilgi edinin

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG
