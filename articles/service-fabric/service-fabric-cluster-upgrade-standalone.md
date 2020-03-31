---
title: Azure Hizmet Kumaşı bağımsız kümeyi yükseltme
description: Azure Hizmet Kumaşı bağımsız kümesinin sürümünü veya yapılandırmasını yükseltme hakkında bilgi edinin.  T
ms.topic: conceptual
ms.date: 11/12/2018
ms.openlocfilehash: 6da9b4c6890895141ecc419382f05f667614fb31
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75451833"
---
# <a name="upgrading-and-updating-a-service-fabric-standalone-cluster"></a>Service Fabric bağımsız kümeyi yükseltme ve güncelleştirme

Herhangi bir modern sistem için, yükseltme için tasarım ürününüzün uzun vadeli başarı elde etmek için anahtardır. Azure Hizmet Kumaşı bağımsız küme, sahip olduğunuz bir kaynaktır. Bu makalede, yükseltilebilir veya güncelleştirilebilenler açıklanmaktadır.

## <a name="controlling-the-fabric-version-that-runs-on-your-cluster"></a>Kümenizde çalışan kumaş sürümünü denetleme
Kümenizin her zaman [desteklenen](service-fabric-versions.md)bir Service Fabric sürümünü çalıştırdığından emin olun. Microsoft, Service Fabric'in yeni bir sürümünüduyurduğunda, önceki sürüm, duyuru tarihinden itibaren en az 60 gün sonra desteğin sona erdirilme üzere işaretlenir. Yeni sürümler [Service Fabric takım blogunda](https://blogs.msdn.microsoft.com/azureservicefabric/)duyurulur. Yeni sürüm bu noktada seçmek için kullanılabilir.

Kümenizi, Microsoft tarafından yayımlandıkları için otomatik kumaş yükseltmeleri alacak şekilde ayarlayabilir veya kümenizin üzerinde olmasını istediğiniz desteklenen kumaş sürümünü el ile seçebilirsiniz. Daha fazla bilgi [için, kümenizde çalışan Hizmet Kumaşı sürümünü yükselt'i](service-fabric-cluster-upgrade-windows-server.md)okuyun.

## <a name="customize-configuration-settings"></a>Yapılandırma ayarlarını özelleştirme

*ClusterConfig.json* dosyasında küme ve düğüm özelliklerinin güvenilirlik düzeyi gibi birçok farklı [yapılandırma ayarı](service-fabric-cluster-manifest.md) ayarlanabilir.  Daha fazla bilgi için [bağımsız bir kümenin yapılandırmasını yükselt'i](service-fabric-cluster-config-upgrade-windows-server.md)okuyun.  Diğer birçok, daha gelişmiş, ayarları da özelleştirilebilir.  Daha fazla bilgi için [Service Fabric küme kumaş ayarlarını](service-fabric-cluster-fabric-settings.md)okuyun.

## <a name="define-node-properties"></a>Düğüm özelliklerini tanımlama
Bazen belirli iş yüklerinin kümedeki belirli düğüm türlerinde çalıştığından emin olmak isteyebilirsiniz. Örneğin, bazı iş yükleri GPU'lar veya SSD'ler gerektirebilirken, diğerleri gerekmeyebilir. Kümedeki düğüm türlerinin her biri için küme düğümlerine özel düğüm özellikleri ekleyebilirsiniz. Yerleşim kısıtlamaları, bir veya daha fazla düğüm özelliği için seçen tek tek hizmetlere eklenen deyimlerdir. Yerleşim kısıtlamaları, hizmetlerin nerede çalışması gerektiğini tanımlar.

Yerleşim kısıtlamaları, düğüm özellikleri ve bunları nasıl tanımlayacakayrıntıları için [düğüm özelliklerini ve yerleşim kısıtlamalarını](service-fabric-cluster-resource-manager-cluster-description.md#node-properties-and-placement-constraints)okuyun.
 

## <a name="add-capacity-metrics"></a>Kapasite ölçümleri ekleme
Düğüm türlerinin her biri için, yüklemeyi bildirmek için uygulamalarınızda kullanmak istediğiniz özel kapasite ölçümleri ekleyebilirsiniz. Yükü bildirmek için kapasite ölçümlerinin kullanımı yla ilgili ayrıntılar için, Kümenizi ve [Ölçümlerinizi](service-fabric-cluster-resource-manager-metrics.md)ve [Yüklemenizi Açıklayan](service-fabric-cluster-resource-manager-cluster-description.md) Hizmet Kumaş Kümesi Kaynak Yöneticisi Belgelerine bakın.

## <a name="patch-the-os-in-the-cluster-nodes"></a>Küme düğümlerinde işletim sistemi yama
Yama orkestrasyon uygulaması (POA), bir Servis Kumaşı kümesinde çalışma sistemi yamasını kesinti olmadan otomatikleştiren bir Service Fabric uygulamasıdır. [Windows için Patch Orchestration Application,](service-fabric-patch-orchestration-application.md) hizmetleri her zaman kullanılabilir tutarken yamaları düzenlenmiş bir şekilde yüklemek için kümenizde dağıtılabilir. 


## <a name="next-steps"></a>Sonraki adımlar
* Bazı hizmet kumaş küme [kumaş ayarlarını](service-fabric-cluster-fabric-settings.md) özelleştirmek öğrenin
* [Kümenizi nasıl ölçeklendirecek ve dışarı çıkarmayı](service-fabric-cluster-scale-up-down.md) öğrenin
* Uygulama [yükseltmeleri](service-fabric-application-upgrade.md) hakkında bilgi edinin

<!--Image references-->
[CertificateUpgrade]: ./media/service-fabric-cluster-upgrade/CertificateUpgrade2.png
[AddingProbes]: ./media/service-fabric-cluster-upgrade/addingProbes2.PNG
[AddingLBRules]: ./media/service-fabric-cluster-upgrade/addingLBRules.png
[HealthPolices]: ./media/service-fabric-cluster-upgrade/Manage_AutomodeWadvSettings.PNG
[ARMUpgradeMode]: ./media/service-fabric-cluster-upgrade/ARMUpgradeMode.PNG
[Create_Manualmode]: ./media/service-fabric-cluster-upgrade/Create_Manualmode.PNG
[Manage_Automaticmode]: ./media/service-fabric-cluster-upgrade/Manage_Automaticmode.PNG
