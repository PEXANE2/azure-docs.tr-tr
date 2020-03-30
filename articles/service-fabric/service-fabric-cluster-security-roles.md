---
title: 'Service Fabric küme güvenliği: istemci rolleri'
description: Bu makalede, iki istemci rolü ve rollere sağlanan izinler açıklanmaktadır.
ms.topic: conceptual
ms.date: 2/23/2018
ms.openlocfilehash: abca19e686d39338fcaa2e0b0c8126913135170b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75451892"
---
# <a name="role-based-access-control-for-service-fabric-clients"></a>Service Fabric istemcileri için rol tabanlı erişim denetimi
Azure Hizmet Kumaşı, Bir Hizmet Kumaşı kümesine bağlı istemciler için iki farklı erişim denetim türünü destekler: yönetici ve kullanıcı. Erişim denetimi, küme yöneticisinin farklı kullanıcı grupları için belirli küme işlemlerine erişimi sınırlamasına olanak sağlayarak kümeyi daha güvenli hale getirir.  

**Yöneticiler** yönetim yeteneklerine (okuma/yazma özellikleri dahil) tam erişime sahiptir. Varsayılan olarak, **kullanıcılar** yalnızca yönetim yeteneklerine (örneğin, sorgu yetenekleri) ve uygulamaları ve hizmetleri çözümleme yeteneğine okuma erişimine sahiptir.

Küme oluşturma sırasında her biri için ayrı sertifikalar sağlayarak iki istemci rolü (yönetici ve istemci) belirtirsiniz. Güvenli bir Hizmet Kumaşı kümesi kurma yla ilgili ayrıntılar için [Service Fabric küme güvenliğine](service-fabric-cluster-security.md) bakın.

## <a name="default-access-control-settings"></a>Varsayılan erişim denetimi ayarları
Yönetici erişim denetim türü tüm FabricClient API'lerine tam erişime sahiptir. Aşağıdaki işlemler de dahil olmak üzere Service Fabric kümesinde okuma ve yazma işlemlerini gerçekleştirebilir:

### <a name="application-and-service-operations"></a>Uygulama ve servis işlemleri
* **CreateService**: hizmet oluşturma                             
* **CreateServiceFromTemplate**: şablondan hizmet oluşturma                             
* **UpdateService**: servis güncellemeleri                             
* **DeleteService**: hizmet silme                             
* **ProvisionApplicationType**: uygulama türü sağlama                             
* **CreateApplication**: uygulama oluşturma                               
* **DeleteApplication**: uygulama silme                             
* **UpgradeApplication**: uygulama yükseltmelerini başlatma veya kesintiye uğratma                             
* **UnprovisionApplicationType**: uygulama türü unprovisioning                             
* **MoveNextUpgradeDomain**: açık bir güncelleştirme etki alanı ile uygulama yükseltmeleri devam                             
* **ReportUpgradeHealth:** geçerli yükseltme ilerleme ile uygulama yükseltmeleri devam                             
* **ReportHealth**: raporlama sağlık                             
* **PredeployPackageToNode**: dağıtım öncesi API                            
* **CodePackageControl**: kod paketlerini yeniden başlatma                             
* **RecoverPartition**: bir bölümü kurtarma                             
* **RecoverPartitions**: bölümleri kurtarma                             
* **RecoverServicePartitions**: hizmet bölümlerini kurtarma                             
* **RecoverSystemPartitions**: sistem servis bölümlerini kurtarma                             

### <a name="cluster-operations"></a>Küm işlemleri
* **ProvisionFabric**: MSI ve/veya küme manifestosu sağlama                             
* **UpgradeFabric**: başlangıç küme yükseltmeleri                             
* **UnprovisionFabric**: MSI ve/veya küme depresör süzme                         
* **MoveNextFabricUpgradeDomain**: açık bir güncelleştirme etki alanı ile küme yükseltmeleri devam                             
* **ReportFabricUpgradeHealth:** geçerli yükseltme ilerleme ile küme yükseltmeleri devam                             
* **StartInfrastructureTask**: altyapı görevlerini başlatma                             
* **FinishInfrastructureTask**: altyapı görevlerini bitirme                             
* **InvokeInfrastructureCommand**: altyapı görev yönetimi komutları                              
* **ActivateNode**: düğümü etkinleştirme                             
* **DeactivateNode**: düğümü devre dışı bırakma                             
* **DeactivateNodesBatch**: birden fazla düğümü devre dışı bırakma                             
* **RemoveNodeDeactivations**: birden fazla düğümüzerinde devre dışı bırakma                             
* **GetNodeDeActivationStatus**: devre dışı bırakma durumunu denetleme                             
* **NodeStateRemoved**: raporlama düğümü durumu kaldırıldı                             
* **ReportFault**: raporlama hatası                             
* **FileContent**: image store istemci dosya aktarımı (kümedışında)                             
* **FileDownload**: image store istemci dosya indirme başlatma (küme için dış)                             
* **InternalList**: image store istemci dosya listesi işlemi (dahili)                             
* **Delete**: resim deposu istemci silme işlemi                              
* **Upload**: resim mağazası istemci yükleme işlemi                             
* **NodeControl**: düğümleri başlatma, durdurma ve yeniden başlatma                             
* **MoveReplicaControl**: kopyaları bir düğümden diğerine taşıma                             

### <a name="miscellaneous-operations"></a>Çeşitli operasyonlar
* **Ping**: istemci ping                             
* **Sorgu**: tüm sorgular izin
* **NameExists**: URI varoluş kontrollerini adlandırma                             

Kullanıcı erişim denetim türü varsayılan olarak aşağıdaki işlemlerle sınırlıdır: 

* **NumaralandırmaSubnames**: uri numaralandırma adlandırma                             
* **NumaralandırmaÖzellikleri**: özellik numaralandırma adlandırma                             
* **PropertyReadBatch**: adlandırma özelliği okuma işlemleri                             
* **GetServiceDescription**: uzun anket hizmet bildirimleri ve okuma hizmeti açıklamaları                             
* **ResolveService**: şikayet tabanlı hizmet çözünürlüğü                             
* **ResolveNameOwner**: URI sahibi adlandırma çözme                             
* **ResolvePartition**: sistem hizmetlerini çözme                             
* **ServiceBildirimler**: olay tabanlı hizmet bildirimleri                             
* **GetUpgradeStatus:** yoklama uygulaması yükseltme durumu                             
* **GetFabricUpgradeStatus:** yoklama küme yükseltme durumu                             
* **InvokeInfrastructureQuery**: altyapı görevlerini sorgulama                             
* **Liste**: resim deposu istemci dosya listesi işlemi                             
* **ResetPartitionLoad**: bir arıza ünitesi için yükleme sıfırlama                             
* **ToggleVerboseServicePlacementHealthReporting**: geçiş verbose hizmet yerleştirme sağlık raporlama                             

Yönetici erişim denetimi de önceki işlemlere erişimi vardır.

## <a name="changing-default-settings-for-client-roles"></a>İstemci rolleri için varsayılan ayarları değiştirme
Küme bildirimi dosyasında, gerekirse istemciye yönetici özellikleri sağlayabilirsiniz. [Küme oluşturma](service-fabric-cluster-creation-via-portal.md)sırasında **Kumaş Ayarları** seçeneğine giderek ve **ad**, **admin**, **kullanıcı**ve **değer** alanlarında önceki ayarları sağlayarak varsayılanları değiştirebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Servis Kumaş küme güvenliği](service-fabric-cluster-security.md)

[Hizmet Kumaş küme oluşturma](service-fabric-cluster-creation-via-portal.md)

