---
title: Hizmet Kumaşı uygulamasının yükseltmesini yapılandırma
description: Microsoft Visual Studio'yu kullanarak Hizmet Kumaşı uygulamasını yükseltme ayarlarını nasıl yapılandırabilirsiniz öğrenin.
author: mikkelhegn
ms.topic: conceptual
ms.date: 06/29/2017
ms.author: mikhegn
ms.openlocfilehash: ef88a528dbb6d326e5cc742d14c27218eb7502f7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75464068"
---
# <a name="configure-the-upgrade-of-a-service-fabric-application-in-visual-studio"></a>Visual Studio'da Hizmet Kumaşı uygulamasının yükseltmesini yapılandırma
Azure Hizmet Dokusu için Visual Studio araçları, yerel veya uzak kümelere yayımlama için yükseltme desteği sağlar. Test ve hata ayıklama sırasında uygulamayı değiştirmek yerine uygulamanızı daha yeni bir sürüme yükseltmek istediğiniz üç senaryo vardır:

* Uygulama verileri yükseltme sırasında kaybolmaz.
* Yükseltme etki alanlarında yeterince hizmet örneği varsa, yükseltme sırasında herhangi bir hizmet kesintisi olmayacağından, kullanılabilirlik yüksek kalır.
* Testler yükseltilirken bir uygulamaya karşı çalıştırılabilir.

## <a name="parameters-needed-to-upgrade"></a>Yükseltmek için gereken parametreler
İki tür dağıtım arasından seçim yapabilirsiniz: düzenli veya yükseltme. Normal dağıtım kümedeki önceki dağıtım bilgilerini ve verilerini silerken, yükseltme dağıtımı onu korur. Visual Studio'da bir Service Fabric uygulamasını yükselttiyseniz, uygulama yükseltme parametreleri ve sistem durumu denetimi ilkeleri sağlamanız gerekir. Uygulama yükseltme parametreleri yükseltmenin denetimine yardımcı olurken, sistem durumu denetimi ilkeleri yükseltmenin başarılı olup olmadığını belirler. [Bkz. Hizmet Kumaşı uygulama yükseltmesi:](service-fabric-application-upgrade-parameters.md) daha fazla ayrıntı için yükseltme parametreleri.

Üç yükseltme modu vardır: *İzlenen,* *İzlenmeyen Otomatik*ve *UnmonitoredManual*.

* İzlenen yükseltme, yükseltme ve uygulama durumu denetimini otomatikleştirir.
* İzlenmeyen Otomatik yükseltme yükseltmeyi otomatikleştirir, ancak uygulama durumu denetimini atlar.
* İzlenmeyen Manuel yükseltme yaptığınızda, her yükseltme etki alanını el ile yükseltmeniz gerekir.

Her yükseltme modu farklı parametreler kümeleri gerektirir. Kullanılabilir yükseltme seçenekleri hakkında daha fazla bilgi edinmek için [Uygulama yükseltme parametrelerine](service-fabric-application-upgrade-parameters.md) bakın.

## <a name="upgrade-a-service-fabric-application-in-visual-studio"></a>Visual Studio'da Hizmet Kumaşı uygulamasını yükseltme
Bir Hizmet Kumaşı uygulamasını yükseltmek için Visual Studio Service Fabric araçlarını kullanıyorsanız, uygulama onay kutusunu **yükseltmeyi** işaretleyerek normal bir dağıtım yerine yükseltme olarak bir yayımlama işlemi belirtebilirsiniz.

### <a name="to-configure-the-upgrade-parameters"></a>Yükseltme parametrelerini yapılandırmak için
1. Onay kutusunun yanındaki **Ayarlar** düğmesini tıklatın. **Yükseltme Parametrelerini Edit** iletişim kutusu görüntülenir. **Yükseltme Parametrelerini Edit** iletişim kutusu, İzlenen, İzlenmeyen Otomatik ve İzlenmeyen Manuel yükseltme modlarını destekler.
2. Kullanmak istediğiniz yükseltme modunu seçin ve parametre ızgarasını doldurun.

    Her parametrenin varsayılan değerleri vardır. İsteğe bağlı parametre *DefaultServiceTypeHealthPolicy* karma tablo girişi alır. *DefaultServiceTypeHealthPolicy*için karma tablo giriş biçimine bir örnek aşağıda verilmiştir:

    ```
    @{ ConsiderWarningAsError = "false"; MaxPercentUnhealthyDeployedApplications = 0; MaxPercentUnhealthyServices = 0; MaxPercentUnhealthyPartitionsPerService = 0; MaxPercentUnhealthyReplicasPerPartition = 0 }
    ```

    *ServiceTypeHealthPolicyMap* aşağıdaki biçimde bir karma tablo girişi alır başka bir isteğe bağlı parametre:

    ```    
    @ {"ServiceTypeName" : "MaxPercentUnhealthyPartitionsPerService,MaxPercentUnhealthyReplicasPerPartition,MaxPercentUnhealthyServices"}
    ```

    İşte gerçek hayattan bir örnek:

    ```
    @{ "ServiceTypeName01" = "5,10,5"; "ServiceTypeName02" = "5,5,5" }
    ```
3. UnmonitoredManual yükseltme modunu seçerseniz, yükseltme işlemini devam ettirmek ve bitirmek için bir PowerShell konsolunu el ile başlatmanız gerekir. Hizmet [Kumaşı uygulama yükseltmesine bakın:](service-fabric-application-upgrade-advanced.md) Manuel yükseltmenin nasıl çalıştığını öğrenmek için gelişmiş konular.

## <a name="upgrade-an-application-by-using-powershell"></a>PowerShell kullanarak uygulamayı yükseltme
Service Fabric uygulamasını yükseltmek için PowerShell cmdlets'i kullanabilirsiniz. Ayrıntılı bilgi için [Service Fabric uygulama yükseltme öğretici](service-fabric-application-upgrade-tutorial.md) ve [Start-ServiceFabricApplicationUpgrade](https://docs.microsoft.com/powershell/module/servicefabric/start-servicefabricapplicationupgrade) bakın.

## <a name="specify-a-health-check-policy-in-the-application-manifest-file"></a>Uygulama bildirimi dosyasında bir sistem durumu denetimi ilkesi belirtin
Hizmet Kumaşı uygulamasındaki her hizmetin varsayılan değerleri geçersiz kılacak kendi sistem durumu ilkesi parametreleri olabilir. Bu parametre değerlerini uygulama bildirimi dosyasında sağlayabilirsiniz.

Aşağıdaki örnek, uygulama bildirimindeki her hizmet için benzersiz bir sistem durumu denetimi ilkesinin nasıl uygulanacağı gösterilmektedir.

```xml
<Policies>
    <HealthPolicy ConsiderWarningAsError="false" MaxPercentUnhealthyDeployedApplications="20">
        <DefaultServiceTypeHealthPolicy MaxPercentUnhealthyServices="20"               
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />
        <ServiceTypeHealthPolicy ServiceTypeName="ServiceTypeName1"
                MaxPercentUnhealthyServices="20"
                MaxPercentUnhealthyPartitionsPerService="20"
                MaxPercentUnhealthyReplicasPerPartition="20" />      
    </HealthPolicy>
</Policies>
```
## <a name="next-steps"></a>Sonraki adımlar
Bir uygulamayı yükseltme hakkında daha fazla bilgi için [Visual Studio'yu kullanarak uygulamayı yükseltme](service-fabric-application-upgrade-tutorial.md)konusuna bakın.
