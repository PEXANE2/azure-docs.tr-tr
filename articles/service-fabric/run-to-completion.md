---
title: Hizmet Kumaşında RunToCompletion semantik
description: Hizmet Kumaşında RunToCompletion semantikini açıklar.
author: shsha-msft
ms.topic: conceptual
ms.date: 03/11/2020
ms.author: shsha
ms.openlocfilehash: adf4b11412aa752144d4ed4fef06d2de1d76598d
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81431299"
---
# <a name="runtocompletion"></a>RunToCompletion

Sürüm 7.1 ile başlayarak, Service Fabric [kapsayıcılar][containers-introduction-link] ve [konuk çalıştırılabilir][guest-executables-introduction-link] uygulamalar için **RunToCompletion** semantik destekler. Bu anlamsal uygulamalar, bir görevi ve çıkışı tamamlayan, her zaman çalışan uygulamaları ve hizmetleri etkinleştirir.

Bu makaleye geçmeden önce Service Fabric uygulama modeli ve [Service Fabric hosting modelini][hosting-model-link]tanımanızı öneririz. [Service Fabric application model][application-model-link]

> [!NOTE]
> RunToCompletion semantik şu anda [Güvenilir Hizmetler][reliable-services-link] programlama modeli kullanılarak yazılmış hizmetler için desteklenmez.
 
## <a name="runtocompletion-semantics-and-specification"></a>RunToCompletion semantik ve belirtim
RunToCompletion semantik [ServiceManifest'i alırken][application-and-service-manifests-link] **Yürütme Politikası** olarak belirtilebilir. Belirtilen ilke ServiceManifest'i oluşturan tüm CodePackages tarafından devredilir. Aşağıdaki ApplicationManifest.xml snippet bir örnek sağlar.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="RunToCompletionServicePackage" ServiceManifestVersion="1.0"/>
  <Policies>
    <ExecutionPolicy Type="RunToCompletion" Restart="OnFailure"/>
  </Policies>
</ServiceManifestImport>
```
**ExecutionPolicy** aşağıdaki iki öznitelik sağlar:
* **Tür:** **RunToCompletion** şu anda bu öznitelik için izin verilen tek değerdir.
* **Yeniden başlatın:** Bu öznitelik, ServicePackage'ı oluşturan CodePackages'a uygulanan yeniden başlatma ilkesini hata durumunda belirtir. Sıfır olmayan bir **çıkış koduyla** çıkan bir CodePackage başarısız olmuş kabul edilir. Bu öznitelik için izin verilen değerler **OnFailure** ve **Asla** **OnFailure** varsayılan olarak vardır.

**OnFailure**olarak ayarlanmış yeniden başlatma ilkesi yle, herhangi bir CodePackage başarısız olursa **(sıfır olmayan çıkış kodu)**, tekrarlanan hatalar arasında geri dönüşler ile yeniden başlatılır. Herhangi bir CodePackage başarısız **olursa, Asla**olarak ayarlanmış yeniden başlatma ilkesi yle, DağıtılanServicePackage'in dağıtım durumu **Başarısız** olarak işaretlenir, ancak diğer CodePackage'ların yürütmeye devam etmesine izin verilir. ServicePackage'ı oluşturan tüm CodePackage başarılı bir şekilde tamamlanması için çalıştırılırsa **(çıkış kodu 0)** DeployedServicePackage'ın dağıtım durumu **RanToCompletion**olarak işaretlenir. 

## <a name="complete-example-using-runtocompletion-semantics"></a>RunToCompletion semantikkullanarak tam bir örnek

RunToCompletion semantik kullanarak tam bir örnek bakalım.

> [!IMPORTANT]
> Aşağıdaki örnek, Service Fabric [ve Docker kullanarak Windows kapsayıcı uygulamaları][containers-getting-started-link]oluşturma aşinalık varsayar.
>
> Bu örnek, mcr.microsoft.com/windows/nanoserver:1809 başvurur. Windows Server kapsayıcıları bir ana bilgisayar işletim sistemi tüm sürümlerinde uyumlu değildir. Daha fazla bilgi için [Windows Kapsayıcı Sürüm Uyumluluğu'na](https://docs.microsoft.com/virtualization/windowscontainers/deploy-containers/version-compatibility)bakın.

Aşağıdaki ServiceManifest.xml, kapsayıcıları temsil eden iki CodePackage'dan oluşan bir ServicePackage'ı tanımlar. *RunToCompletionCodePackage1* sadece **stdout** ve çıkışları için bir mesaj günlükleri. *RunToCompletionCodePackage2* bir süre için loopback adresi pings ve sonra **0,** **1** veya **2**bir çıkış kodu ile çıkar .

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ServiceManifest Name="WindowsRunToCompletionServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>Windows RunToCompletion Service</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="WindowsRunToCompletionServiceType"  UseImplicitHost="true"/>
  </ServiceTypes>
  <CodePackage Name="RunToCompletionCodePackage1" Version="1.0">
    <EntryPoint>
      <ContainerHost>
        <ImageName>mcr.microsoft.com/windows/nanoserver:1809</ImageName>
        <Commands>/c,echo Hi from RunToCompletionCodePackage1 &amp;&amp; exit 0</Commands>
        <EntryPoint>cmd</EntryPoint>
      </ContainerHost>
    </EntryPoint>
  </CodePackage>

  <CodePackage Name="RunToCompletionCodePackage2" Version="1.0">
    <EntryPoint>
      <ContainerHost>
        <ImageName>mcr.microsoft.com/windows/nanoserver:1809</ImageName>
        <Commands>/v,/c,ping 127.0.0.1 &amp;&amp; set /a exitCode=%random% % 3 &amp;&amp; exit !exitCode!</Commands>
        <EntryPoint>cmd</EntryPoint>
      </ContainerHost>
    </EntryPoint>
  </CodePackage>
</ServiceManifest>
```

Aşağıdaki ApplicationManifest.xml yukarıda açıklanan ServiceManifest.xml dayalı bir uygulama açıklar. **Bu OnFailure**bir yeniden başlatma ilkesi ile *WindowsRunToCompletionServicePackage* için **RunToCompletion** **ExecutionPolicy** belirtir. *WindowsRunToCompletionServicePackage*etkinleştirilmesi üzerine, kurucu CodePackages başlatılacaktır. *RunToCompletionCodePackage1* ilk etkinleştirme de başarılı bir şekilde çıkmalıdır. Ancak, *RunToCompletionCodePackage2* başarısız olabilir **(sıfır olmayan çıkış kodu)** bu durumda yeniden başlatma ilkesi **OnFailure**olduğu için yeniden başlatılır .

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ApplicationManifest ApplicationTypeName="WindowsRunToCompletionApplicationType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

  <Description>Windows RunToCompletion Application</Description>

  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="WindowsRunToCompletionServicePackage" ServiceManifestVersion="1.0"/>
    <Policies>
      <ExecutionPolicy Type="RunToCompletion" Restart="OnFailure"/>
    </Policies>
  </ServiceManifestImport>

  <DefaultServices>
    <Service Name="WindowsRunToCompletionService" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="WindowsRunToCompletionServiceType" InstanceCount="1">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```
## <a name="querying-deployment-status-of-a-deployedservicepackage"></a>Dağıtılmış ServicePackage'ın dağıtım durumunu sorgulama
Dağıtılan ServicePackage'ın dağıtım durumu PowerShell'den [Get-ServiceFabricDeployedServicePackage][deployed-service-package-link] kullanılarak veya [FabricClient][fabric-client-link] API [GetDeployedServicePackageListAsync (String, Uri, String)][deployed-service-package-fabricclient-link] kullanılarak C#'dan sorgulanabilir

## <a name="considerations-when-using-runtocompletion-semantics"></a>RunToCompletion semantikkullanırken dikkat edilmesi gerekenler

Geçerli RunToCompletion desteği için aşağıdaki noktalar belirtilmelidir.
* Bu semantik ler yalnızca [kapsayıcılar][containers-introduction-link] ve [konuk çalıştırılabilir][guest-executables-introduction-link] uygulamalar için desteklenir.
* RunToCompletion semantikiçeren uygulamalar için yükseltme senaryolarına izin verilmez. Kullanıcılar gerekirse bu tür uygulamaları silmeli ve yeniden oluşturmalıdır.
* Failover olayları, CodePackages'ın başarılı bir şekilde tamamlandıktan sonra, aynı düğümde veya kümenin diğer düğümlerinde yeniden yürütülmesine neden olabilir. Başarısız olaylara örnek olarak düğüm yeniden başlatılır ve düğümdeki Servis Kumaşı çalışma zamanı yükseltmeleri verilebilir.

## <a name="next-steps"></a>Sonraki adımlar

İlgili bilgiler için aşağıdaki makalelere bakın.

* [Servis Kumaş ve konteynerler.][containers-introduction-link]
* [Servis Kumaş ve konuk icra.][guest-executables-introduction-link]

<!-- Links -->
[containers-introduction-link]: service-fabric-containers-overview.md
[containers-getting-started-link]: service-fabric-get-started-containers.md
[guest-executables-introduction-link]: service-fabric-guest-executables-introduction.md
[reliable-services-link]: service-fabric-reliable-services-introduction.md
[application-model-link]: service-fabric-application-model.md
[hosting-model-link]: service-fabric-hosting-model.md
[application-and-service-manifests-link]: service-fabric-application-and-service-manifests.md
[setup-entry-point-link]: service-fabric-run-script-at-service-startup.md
[deployed-service-package-working-with-link]: service-fabric-hosting-model.md#work-with-a-deployed-service-package
[deployed-code-package-link]: https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedcodepackage
[deployed-service-package-link]: https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedservicePackage
[fabric-client-link]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient
[deployed-service-package-fabricclient-link]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.queryclient.getdeployedservicepackagelistasync

