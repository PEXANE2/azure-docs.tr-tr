---
title: Hizmet Kumaşında Başharf KodlarıPaketler
description: Hizmet Kumaşında Initializer CodePackages açıklar.
author: shsha-msft
ms.topic: conceptual
ms.date: 03/10/2020
ms.author: shsha
ms.openlocfilehash: 8483e00f55d0dd49ba57db58b99b237ce0a169e5
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81430636"
---
# <a name="initializer-codepackages"></a>Baş harf koduPaketleri

7.1 sürümüyle başlayan Service Fabric, [konteynerler][containers-introduction-link] ve [konuk çalıştırılabilir][guest-executables-introduction-link] uygulamalar için **Initializer CodePackages'ı** destekler. Initializer CodePackages, diğer CodePackage'lar yürütmeye başlamadan önce ServicePackage kapsamında başlatma işlemleri gerçekleştirme olanağı sağlar. Bir ServicePackage ile olan [ilişkileri, SetupEntryPoint'in][setup-entry-point-link] CodePackage için ne olduğuna benzer.

Bu makaleye geçmeden önce Service Fabric uygulama modeli ve [Service Fabric hosting modelini][hosting-model-link]tanımanızı öneririz. [Service Fabric application model][application-model-link]

> [!NOTE]
> Initializer CodePackages şu anda [Güvenilir Hizmetler][reliable-services-link] programlama modeli kullanılarak yazılmış hizmetler için desteklenmez.
 
## <a name="semantics"></a>Semantiği

Bir Initializer CodePackage başarılı **tamamlanması (çıkış kodu 0)** çalışması bekleniyor. Başarısız bir Initializer CodePackage başarıyla tamamlanana kadar yeniden başlatılır. Birden çok Başlatılan CodePackages izin verilir ve **başarılı tamamlanması**için yürütülür , **sırayla**, ServicePackage diğer CodePackages yürütme başlamadan önce **belirli bir sırada.**

## <a name="specifying-initializer-codepackages"></a>Initializer CodePackages belirtme
ServiceManifest'te **Initializer** özniteliğini doğru olarak ayarlayarak Bir CodePackage'ı **Initializer** olarak işaretleyebilirsiniz. Birden çok Initializer CodePackages olduğunda, yürütme sıraları **ExecOrder** özniteliği ile belirtilebilir. **ExecOrder** negatif olmayan bir tamsayı olmalıdır ve yalnızca Initializer CodePackages için geçerlidir. **ExecOrder'ın** daha düşük değerlerine sahip Initializer CodePackages önce yürütülür. **ExecOrder** bir Initializer CodePackage için belirtilmemişse, varsayılan değeri 0 olarak kabul edilir. **ExecOrder** aynı değere sahip Initializer CodePackages göreli yürütme sırası belirtilmemiştir.

Aşağıdaki ServiceManifest snippet üç CodePackages iki Initializers olarak işaretlenmiş açıklar. Bu ServicePackage etkinleştirildiğinde, **ExecOrder**en düşük değerine sahip olduğundan *InitCodePackage0* ilk yürütülür. *InitCodePackage0'ın*başarılı bir şekilde tamamlanmasıüzerine (çıkış kodu 0) *InitCodePackage1* yürütülür. Son olarak, *InitCodePackage1'in*başarıyla tamamlanması *üzerine, WorkloadCodePackage* yürütülür.

```xml
<CodePackage Name="InitCodePackage0" Version="1.0" Initializer="true" ExecOrder="0">
  ...
</CodePackage>

<CodePackage Name="InitCodePackage1" Version="1.0" Initializer="true" ExecOrder="1">
  ...
</CodePackage>

<CodePackage Name="WorkloadCodePackage" Version="1.0">
  ...
</CodePackage>
```
## <a name="complete-example-using-initializer-codepackages"></a>Initializer CodePackages kullanarak tam örnek

Initializer CodePackages kullanarak tam bir örnek bakalım.

> [!IMPORTANT]
> Aşağıdaki örnek, Service Fabric [ve Docker kullanarak Windows kapsayıcı uygulamaları][containers-getting-started-link]oluşturma aşinalık varsayar.
>
> Bu örnek, mcr.microsoft.com/windows/nanoserver:1809 başvurur. Windows Server kapsayıcıları bir ana bilgisayar işletim sistemi tüm sürümlerinde uyumlu değildir. Daha fazla bilgi için [Windows Kapsayıcı Sürüm Uyumluluğu'na](https://docs.microsoft.com/virtualization/windowscontainers/deploy-containers/version-compatibility)bakın.

Aşağıdaki ServiceManifest.xml daha önce açıklanan ServiceManifest snippet üzerine inşa edin. *InitCodePackage0*, *InitCodePackage1* ve *WorkloadCodePackage* kapsayıcıları temsil eden CodePackages'tir. Etkinleştirme üzerine, *InitCodePackage0* ilk yürütülür. Bir dosyaya ileti kaydeder ve çıkar. Ardından, *InitCodePackage1* yürütülür ve bir dosyaya ileti günlüğe kaydeder ve çıkar. Son olarak, *WorkloadCodePackage* yürütme başlar. Ayrıca bir dosyaya bir ileti günlükleri, **stdout** için dosyanın içeriğini çıktıları ve sonra sonsuza kadar ping.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ServiceManifest Name="WindowsInitCodePackageServicePackage" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <Description>Windows Init CodePackage Service</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="WindowsInitCodePackageServiceType"  UseImplicitHost="true"/>
  </ServiceTypes>
  <CodePackage Name="InitCodePackage0" Version="1.0" Initializer="true" ExecOrder="0">
    <EntryPoint>
      <ContainerHost>
        <ImageName>mcr.microsoft.com/windows/nanoserver:1809</ImageName>
        <Commands>/c,echo Hi from InitCodePackage0. &gt; C:\WorkspaceOnContainer\log.txt</Commands>
        <EntryPoint>cmd</EntryPoint>
      </ContainerHost>
    </EntryPoint>
  </CodePackage>

  <CodePackage Name="InitCodePackage1" Version="1.0" Initializer="true" ExecOrder="1">
    <EntryPoint>
      <ContainerHost>
        <ImageName>mcr.microsoft.com/windows/nanoserver:1809</ImageName>
        <Commands>/c,echo Hi from InitCodePackage1. &gt;&gt; C:\WorkspaceOnContainer\log.txt</Commands>
        <EntryPoint>cmd</EntryPoint>
      </ContainerHost>
    </EntryPoint>
  </CodePackage>

  <CodePackage Name="WorkloadCodePackage" Version="1.0">
    <EntryPoint>
      <ContainerHost>
        <ImageName>mcr.microsoft.com/windows/nanoserver:1809</ImageName>
        <Commands>/c,echo Hi from WorkloadCodePackage. &gt;&gt; C:\WorkspaceOnContainer\log.txt &amp;&amp; type C:\WorkspaceOnContainer\log.txt &amp;&amp; ping -t 127.0.0.1 &gt; nul</Commands>
        <EntryPoint>cmd</EntryPoint>
      </ContainerHost>
    </EntryPoint>
  </CodePackage>
</ServiceManifest>
```

Aşağıdaki ApplicationManifest.xml yukarıda açıklanan ServiceManifest.xml dayalı bir uygulama açıklar. Tüm kapsayıcılar için aynı **Birim** montaj belirtir unutmayın, yani **C:\WorkspaceOnHost** **C monte edilir:\WorkspaceOnContainer** her üç kapsayıcıüzerinde. Net etkisi, tüm kapsayıcıların etkinleştirildikleri sırada aynı günlük dosyasına yazmalarıdır.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ApplicationManifest ApplicationTypeName="WindowsInitCodePackageApplicationType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">

  <Description>Windows Init CodePackage Application</Description>

  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="WindowsInitCodePackageServicePackage" ServiceManifestVersion="1.0"/>
    <Policies>
      <ContainerHostPolicies CodePackageRef="InitCodePackage0" ContainersRetentionCount="2" RunInteractive="true">
        <Volume Source="C:\WorkspaceOnHost" Destination="C:\WorkspaceOnContainer" IsReadOnly="false" />
      </ContainerHostPolicies>

     <ContainerHostPolicies CodePackageRef="InitCodePackage1" ContainersRetentionCount="2" RunInteractive="true">
        <Volume Source="C:\WorkspaceOnHost" Destination="C:\WorkspaceOnContainer" IsReadOnly="false" />
      </ContainerHostPolicies>

      <ContainerHostPolicies CodePackageRef="WorkloadCodePackage" ContainersRetentionCount="2" RunInteractive="true">
        <Volume Source="C:\WorkspaceOnHost" Destination="C:\WorkspaceOnContainer" IsReadOnly="false" />
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>

  <DefaultServices>
    <Service Name="WindowsInitCodePackageService" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="WindowsInitCodePackageServiceType" InstanceCount="1">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```
ServicePackage başarıyla etkinleştirildikten sonra, **C:\WorkspaceOnHost\log.txt** içeriği aşağıdaki olmalıdır.

```console
C:\Users\test>type C:\WorkspaceOnHost\log.txt
Hi from InitCodePackage0.
Hi from InitCodePackage1.
Hi from WorkloadCodePackage.
```

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
[setup-entry-point-link]: service-fabric-run-script-at-service-startup.md

