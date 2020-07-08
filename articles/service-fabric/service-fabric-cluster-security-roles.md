---
title: 'Küme güvenliği Service Fabric: istemci rolleri'
description: Bu makalede, iki istemci rolü ve rollere sunulan izinler açıklanmaktadır.
ms.topic: conceptual
ms.date: 2/23/2018
ms.openlocfilehash: abca19e686d39338fcaa2e0b0c8126913135170b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "75451892"
---
# <a name="role-based-access-control-for-service-fabric-clients"></a>Service Fabric istemcileri için rol tabanlı erişim denetimi
Azure Service Fabric, bir Service Fabric kümesine bağlı istemciler için iki farklı erişim denetimi türünü destekler: yönetici ve Kullanıcı. Erişim denetimi, küme yöneticisinin farklı Kullanıcı grupları için belirli küme işlemlerine erişimi sınırlandırıp kümenin daha güvenli olmasını sağlar.  

**Yöneticiler** , yönetim özelliklerine (okuma/yazma özellikleri dahil) tam erişime sahiptir. Varsayılan olarak, **kullanıcıların** yalnızca yönetim özelliklerine okuma erişimi vardır (örneğin, sorgu özellikleri) ve uygulama ve Hizmetleri çözme özelliği vardır.

Her biri için ayrı sertifikalar sağlayarak küme oluşturma sırasında iki istemci rolünü (yönetici ve istemci) belirtirsiniz. Güvenli bir Service Fabric kümesi ayarlama hakkında daha fazla bilgi için bkz. [Service Fabric küme güvenliği](service-fabric-cluster-security.md) .

## <a name="default-access-control-settings"></a>Varsayılan erişim denetimi ayarları
Yönetici erişimi denetim türü tüm FabricClient API 'Lerine tam erişime sahiptir. Service Fabric kümesinde aşağıdaki işlemler dahil olmak üzere okuma ve yazma işlemleri gerçekleştirebilir:

### <a name="application-and-service-operations"></a>Uygulama ve hizmet işlemleri
* **CreateService**: hizmet oluşturma                             
* **Createservicefromtemplate**: şablondan hizmet oluşturma                             
* **Updateservice**: hizmet güncelleştirmeleri                             
* **DeleteService**: hizmet silme                             
* **Provisionapplicationtype**: uygulama türü sağlama                             
* **Createapplication**: uygulama oluşturma                               
* **DeleteApplication**: uygulama silme                             
* **Upgradeapplication**: uygulama yükseltmelerini başlatma veya kesme                             
* **Unprovisionapplicationtype**: uygulama türü sağlamayı kaldırma                             
* **MoveNextUpgradeDomain**: açık bir güncelleştirme etki alanı ile uygulama yükseltmeleri sürdürülüyor                             
* **Reportupgradehealth**: geçerli yükseltme ilerlemesiyle uygulama yükseltmeleri sürdürülüyor                             
* **Reporthegizlilik**: raporlama durumu                             
* **Predeploypackagetonode**: ön dağıtım API 'si                            
* **Codepackagecontrol**: kod paketlerini yeniden başlatma                             
* **Recoverpartition**: bir bölümü kurtarma                             
* **Recoverpartitions**: bölümleri kurtarma                             
* **Recoverservicepartitions**: hizmet bölümlerini kurtarma                             
* **Recoversystempartitions**: sistem hizmeti bölümlerini kurtarma                             

### <a name="cluster-operations"></a>Küm işlemleri
* **Provisionfabric**: MSI ve/veya küme bildirimi sağlama                             
* **Yükselmen Bric**: küme yükseltmeleri başlatılıyor                             
* **Unprovisionfabric**: MSI ve/veya küme bildirimi sağlama geri alınıyor                         
* **MoveNextFabricUpgradeDomain**: açık bir güncelleştirme etki alanı ile küme yükseltmeleri sürdürülüyor                             
* **Reportfabricupgradehealth**: geçerli yükseltme ilerlemesi ile küme yükseltmeleri sürdürülüyor                             
* **Starınfrastructuretask**: altyapı görevleri başlatılıyor                             
* **FinishInfrastructureTask**: altyapı görevleri bitiriliyor                             
* **InvokeInfrastructureCommand**: altyapı görevi yönetim komutları                              
* **ActivateNode**: bir düğümü etkinleştirme                             
* **DeactivateNode**: düğümü devre dışı bırakma                             
* **DeactivateNodesBatch**: birden çok düğümü devre dışı bırakma                             
* **Removenodedeetkinleştirmeleri**: birden çok düğümdeki devre dışı bırakma geri döndürülüyor                             
* **Getnodedeactivationstatus**: devre dışı bırakma durumu denetleniyor                             
* **NodeStateRemoved**: Raporlama düğümü durumu kaldırıldı                             
* **Reportfault**: raporlama hatası                             
* **Dosya içeriği**: görüntü deposu istemci dosyası aktarımı (dış küme)                             
* **Dosya indirme**: görüntü deposu istemci dosyası indirme başlatma (dış küme)                             
* **InternalList**: görüntü deposu istemci dosyası listeleme işlemi (iç)                             
* **Sil**: görüntü deposu istemcisi silme işlemi                              
* **Karşıya yükle**: görüntü deposu istemci yükleme işlemi                             
* **NodeControl**: düğümleri başlatma, durdurma ve yeniden başlatma                             
* **Movereperepcontrol**: çoğaltmaları bir düğümden diğerine taşıma                             

### <a name="miscellaneous-operations"></a>Çeşitli işlemler
* **Ping**: istemci ping işlemleri                             
* **Sorgu**: tüm sorgular izin verildi
* **Nameexists**: URI varlık denetimlerini adlandırma                             

Kullanıcı erişim denetimi türü, varsayılan olarak aşağıdaki işlemlerle sınırlıdır: 

* **Enumeratesubnames**: URI numaralandırması adlandırma                             
* **Enumerateproperties**: adlandırma özelliği numaralandırması                             
* **Propertyreadbatch**: Özellik okuma işlemleri adlandırma                             
* **GetServiceDescription**: uzun yoklama hizmeti bildirimleri ve okuma hizmeti açıklamaları                             
* **ResolveService**: şikayet tabanlı hizmet çözümlemesi                             
* **ResolveNameOwner**: adlandırma URI 'si sahibi çözümleniyor                             
* **ResolvePartition**: sistem hizmetlerini çözümleme                             
* **ServiceNotifications**: olay tabanlı hizmet bildirimleri                             
* **Getupgradestatus**: uygulama yükseltme durumu yoklanıyor                             
* **Getfabricupgradestatus**: küme yükseltme durumu yoklanıyor                             
* **InvokeInfrastructureQuery**: altyapı görevlerini sorgulama                             
* **Liste**: görüntü deposu istemci dosyası listeleme işlemi                             
* **Resetpartitionload**: yük devretme birimi için yükleme sıfırlanıyor                             
* **ToggleVerboseServicePlacementHealthReporting**: ayrıntılı hizmet yerleştirme sistem durumu raporlamasını değiştirme                             

Yönetici erişim denetimi, önceki işlemlere de erişebilir.

## <a name="changing-default-settings-for-client-roles"></a>İstemci rolleri için varsayılan ayarları değiştirme
Küme bildirim dosyasında, gerekirse istemciye yönetici özellikleri sağlayabilirsiniz. [Küme oluşturma](service-fabric-cluster-creation-via-portal.md)sırasında **doku ayarları** seçeneğine giderek Varsayılanları değiştirebilir ve **ad**, **yönetici**, **Kullanıcı**ve **değer** alanlarında önceki ayarları sağlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
[Service Fabric küme güvenliği](service-fabric-cluster-security.md)

[Service Fabric kümesi oluşturma](service-fabric-cluster-creation-via-portal.md)

