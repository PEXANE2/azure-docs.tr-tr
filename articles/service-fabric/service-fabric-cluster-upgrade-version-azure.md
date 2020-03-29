---
title: Kümenin Azure Hizmet Kumaşı sürümünü yükseltme
description: Küme güncelleştirme modunu ayarlama, sertifikaları yükseltme, uygulama bağlantı noktaları ekleme, işletim sistemi yamaları yapma ve benzeri dahil olmak üzere Hizmet Kumaşı kümesini çalıştıran Hizmet Kumaşı kodunu ve/veya yapılandırmasını yükseltin. Yükseltmeler yapıldığında ne bekleyebilirsiniz?
ms.topic: conceptual
ms.date: 11/12/2018
ms.openlocfilehash: c3ffcbd4296385623ff5e2c1ee001c27598ff3fb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75451818"
---
# <a name="upgrade-the-service-fabric-version-of-a-cluster"></a>Kümenin Service Fabric sürümünü yükseltme

Herhangi bir modern sistem için, yükseltme için tasarım ürününüzün uzun vadeli başarı elde etmek için anahtardır. Azure Hizmet Kumaşı kümesi, sahip olduğunuz ancak kısmen Microsoft tarafından yönetilen bir kaynaktır. Bu makalede, Azure kümenizde çalışan Hizmet Dokusu sürümünün nasıl yükseltilen anlatılmaktadır.

Kümenizi, Microsoft tarafından yayımlandıkça otomatik kumaş yükseltmeleri alacak şekilde ayarlayabilir veya kümenizin üzerinde olmasını istediğiniz desteklenen kumaş sürümünü seçebilirsiniz.

Bunu, portalda "upgradeMode" küme yapılandırmasını ayarlayarak veya oluşturma sırasında veya daha sonra canlı bir kümede Kaynak Yöneticisi'ni kullanarak yaparsınız 

> [!NOTE]
> Kümenizin her zaman desteklenen bir kumaş sürümünü çalıştırdığından emin olun. Hizmet dokusunun yeni bir sürümünüduyurduğumuz gibi, önceki sürüm, bu tarihten itibaren en az 60 gün sonra desteğin sona ermesi için işaretlenir. Yeni sürümler [hizmet kumaş takım blogunda](https://blogs.msdn.microsoft.com/azureservicefabric/)duyurulur. Yeni sürüm daha sonra seçmek için kullanılabilir. 
> 
> 

Kümenizin çalıştırdığı sürümün sona ermesinden 14 gün önce, kümenizi uyarı durumu durumuna sokan bir sistem durumu oluşturulur. Desteklenen kumaş sürümüne yükseltilene kadar küme uyarı durumunda kalır.

## <a name="set-the-upgrade-mode-in-the-azure-portal"></a>Azure portalında yükseltme modunu ayarlama
Küme oluştururken kümeyi otomatik veya el ile ayarlayabilirsiniz.

![Create_Manualmode][Create_Manualmode]

Canlı kümede yken, yönetme deneyimini kullanarak kümeyi otomatik veya el ile ayarlayabilirsiniz. 

### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-portal"></a>Portal üzerinden Manuel modolarak ayarlanmış bir kümedeki yeni sürüme yükseltme.
Yeni bir sürüme yükseltmek için tek yapmanız gereken açılır yerden kullanılabilir sürümü seçmek ve kaydetmektir. Kumaş yükseltmesi otomatik olarak devreye girer. Küme sistem durumu ilkeleri (düğüm durumu ve kümede çalışan tüm uygulamaların sistem durumu birleşimi) yükseltme sırasında yapıştırılır.

Küme sistem durumu ilkeleri karşılanmazsa, yükseltme geri alınır. Bu özel sistem durumu ilkelerini nasıl ayarlayıştığı hakkında daha fazla bilgi almak için bu belgeyi kızıntıyı kaydırın. 

Geri almayla sonuçlanan sorunları giderdikten sonra, önceki yle aynı adımları izleyerek yükseltmeyi yeniden başlatmanız gerekir.

![Manage_Automaticmode][Manage_Automaticmode]

## <a name="set-the-upgrade-mode-using-a-resource-manager-template"></a>Kaynak Yöneticisi şablonunu kullanarak yükseltme modunu ayarlama
Microsoft.ServiceFabric/clusters kaynak tanımına "upgradeMode" yapılandırmasını ekleyin ve "clusterCodeVersion"u aşağıda gösterildiği gibi desteklenen kumaş sürümlerinden birine ayarlayın ve ardından şablonu dağıtın. "upgradeMode" için geçerli değerler "Manuel" veya "Otomatik"

![ARMUpgradeMode][ARMUpgradeMode]

### <a name="upgrading-to-a-new-version-on-a-cluster-that-is-set-to-manual-mode-via-a-resource-manager-template"></a>Kaynak Yöneticisi şablonu aracılığıyla El Ile moduna ayarlanmış bir kümedeki yeni sürüme yükseltme.
Küme El Ile modundayken, yeni bir sürüme yükseltmek için "clusterCodeVersion"u desteklenen bir sürüme çevirin ve dağıtın. Şablonun dağıtımı, Kumaş yükseltme tekmeler otomatik olarak başladı olur. Küme sistem durumu ilkeleri (düğüm durumu ve kümede çalışan tüm uygulamaların sistem durumu birleşimi) yükseltme sırasında yapıştırılır.

Küme sistem durumu ilkeleri karşılanmazsa, yükseltme geri alınır.  

Geri almayla sonuçlanan sorunları giderdikten sonra, önceki yle aynı adımları izleyerek yükseltmeyi yeniden başlatmanız gerekir.

## <a name="set-custom-health-polices-for-upgrades"></a>Yükseltmeler için özel sağlık polisleri ayarlayın
Kumaş yükseltmesi için özel sağlık polisleri belirtebilirsiniz. Kümenizi Otomatik kumaş yükseltmelerine ayarladıysanız, bu ilkeler otomatik [kumaş yükseltmelerinin Faz-1'ine](service-fabric-cluster-upgrade.md#fabric-upgrade-behavior-during-automatic-upgrades)uygulanır.
Kümenizi El ile kumaş yükseltmeleri için ayarladıysanız, bu ilkeler kümenizdeki kumaş yükseltmesini başlatmak için sistemi tetikleyen yeni bir sürümü her seçtiğinizde uygulanır. İlkeleri geçersiz kılmazsanız, varsayılanlar kullanılır.

Gelişmiş yükseltme ayarlarını seçerek özel sistem durumu ilkelerini belirtebilir veya "kumaş yükseltme" bıçağı altındaki geçerli ayarları gözden geçirebilirsiniz. Nasıl yapılacağını aşağıdaki resmi gözden geçirin. 

![Özel sistem durumu ilkelerini yönetme][HealthPolices]

## <a name="list-all-available-versions-for-all-environments-for-a-given-subscription"></a>Belirli bir abonelik için tüm ortamlar için tüm kullanılabilir sürümleri listele
Aşağıdaki komutu çalıştırın ve buna benzer bir çıktı almalısınız.

"supportExpiryUtc" belirli bir sürümün süresinin ne zaman sona erdiğini veya süresinin dolduğunu söyler. En son sürüm geçerli bir tarih yok - sadece son kullanma tarihi henüz ayarlanmış olmadığı anlamına gelir "9999-12-31T23:59:59.99999999" bir değere sahiptir.

```REST
GET https://<endpoint>/subscriptions/{{subscriptionId}}/providers/Microsoft.ServiceFabric/locations/{{location}}/clusterVersions?api-version=2016-09-01

Example: https://management.azure.com/subscriptions/1857f442-3bce-4b96-ad95-627f76437a67/providers/Microsoft.ServiceFabric/locations/eastus/clusterVersions?api-version=2016-09-01

Output:
{
                  "value": [
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/5.0.1427.9490",
                      "name": "5.0.1427.9490",
                      "type": "Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.0.1427.9490",
                        "supportExpiryUtc": "2016-11-26T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.0.1427.9490",
                      "name": "5.1.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "5.1.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Windows"
                      }
                    },
                    {
                      "id": "subscriptions/35349203-a0b3-405e-8a23-9f1450984307/providers/Microsoft.ServiceFabric/environments/Windows/clusterVersions/4.4.1427.9490",
                      "name": "4.4.1427.9490",
                      "type": " Microsoft.ServiceFabric/environments/clusterVersions",
                      "properties": {
                        "codeVersion": "4.4.1427.9490",
                        "supportExpiryUtc": "9999-12-31T23:59:59.9999999",
                        "environment": "Linux"
                      }
                    }
                  ]
                }
```

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
