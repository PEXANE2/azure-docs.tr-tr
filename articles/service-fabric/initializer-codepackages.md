---
title: Service Fabric Başlatıcı kod paketleri
description: Service Fabric içindeki Başlatıcı kod paketlerini açıklar.
author: shsha-msft
ms.topic: conceptual
ms.date: 03/10/2020
ms.author: shsha
ms.openlocfilehash: 8483e00f55d0dd49ba57db58b99b237ce0a169e5
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "81430636"
---
# <a name="initializer-codepackages"></a>Başlatıcı Kod Paketleri

Sürüm 7,1 ' den başlayarak, [kapsayıcılar][containers-introduction-link] ve [Konuk yürütülebilir][guest-executables-introduction-link] uygulamalar için **Başlatıcı kod paketlerini** destekler Service Fabric. Başlatıcı CodePackages, diğer kod paketleri yürütülmeye başlamadan önce ServicePackage kapsamında başlatma gerçekleştirme fırsatı sağlar. Bir ServicePackage ile olan ilişkisi, bir CodePackage için [Setupentrypoint][setup-entry-point-link] öğesine benzerdir.

Bu makaleye devam etmeden önce [Service Fabric uygulama modeli][application-model-link] ve [Service Fabric barındırma modeli][hosting-model-link]hakkında bilgi sahibi olmanız önerilir.

> [!NOTE]
> Başlatıcı kod paketleri şu anda [Reliable Services][reliable-services-link] programlama modeli kullanılarak yazılan hizmetler için desteklenmiyor.
 
## <a name="semantics"></a>Semantik

Bir başlatıcı CodePackage 'in başarıyla tamamlanması için çalıştırılması bekleniyor **(çıkış kodu 0)**. Başarısız bir başlatıcı CodePackage, başarıyla tamamlanana kadar yeniden başlatılır. Birden fazla Başlatıcı CodePackage 'e izin verilir ve bu, hizmet paketindeki diğer kod paketleri yürütülmeye başlamadan önce, **sırasıyla** **belirtilen sırada başarılı bir** şekilde **tamamlanmasıyla**yürütülür.

## <a name="specifying-initializer-codepackages"></a>Başlatıcı kod paketleri belirtme
ServiceManifest içinde **Başlatıcı** özniteliğini **true** olarak ayarlayarak bir CodePackage 'i başlatıcı olarak işaretleyebilirsiniz. Birden çok Başlatıcı kod paketi olduğunda, yürütme sırası **execorder** özniteliği aracılığıyla belirtilebilir. **Execorder** negatif olmayan bir tamsayı olmalıdır ve yalnızca başlatıcı kod paketleri için geçerlidir. **Execorder** 'ın daha düşük değerleriyle Başlatıcı kod paketleri önce yürütülür. Başlatıcı CodePackage için **Execorder** belirtilmemişse, varsayılan değer olan 0 varsayılır. Aynı **execorder** değeri Ile birlikte Başlatıcı kod paketlerinin göreli yürütme sırası belirtilmemiş.

Aşağıdaki ServiceManifest kod parçacığı, iki adet bir başlatıcı olarak işaretlenen üç kod paketini açıklar. Bu ServicePackage etkinleştirildiğinde *InitCodePackage0* , en düşük **execorder**değerine sahip olduğundan önce yürütülür. *InitCodePackage0*(çıkış kodu 0) başarıyla tamamlandığında, *InitCodePackage1* yürütülür. Son olarak, *InitCodePackage1*başarıyla tamamlandığında *workloadcodepackage* yürütülür.

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
## <a name="complete-example-using-initializer-codepackages"></a>Başlatıcı kod paketlerini kullanarak örneği doldurun

Başlatıcı kod paketleri 'ni kullanarak tüm bir örneğe bakalım.

> [!IMPORTANT]
> Aşağıdaki örnek, [Service Fabric ve Docker kullanarak Windows kapsayıcı uygulamaları][containers-getting-started-link]oluşturma konusunda benzerlik olduğunu varsayar.
>
> Bu örnek, mcr.microsoft.com/windows/nanoserver:1809 öğesine başvurur. Windows Server kapsayıcıları, bir konak işletim sisteminin tüm sürümleri arasında uyumlu değildir. Daha fazla bilgi için bkz. [Windows kapsayıcı sürümü uyumluluğu](https://docs.microsoft.com/virtualization/windowscontainers/deploy-containers/version-compatibility).

Aşağıdaki ServiceManifest.xml daha önce açıklanan ServiceManifest parçacığı üzerinde oluşturulur. *InitCodePackage0*, *InitCodePackage1* ve *workloadcodepackage* , kapsayıcıları temsil eden codepackages. Etkinleştirmeden sonra, *InitCodePackage0* önce yürütülür. Bir dosyaya ileti kaydeder ve çıkılıyor. Sonra, *InitCodePackage1* yürütülür ve ayrıca bir dosyaya ileti kaydeder ve çıkar. Son olarak, *Workloadcodepackage* yürütmeye başlar. Ayrıca bir iletiyi bir dosyaya kaydeder, **stdout** 'a dosyanın içeriğini çıkarır ve sonsuza kadar ping atar.

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

Aşağıdaki ApplicationManifest.xml, yukarıda açıklanan ServiceManifest.xml dayalı bir uygulamayı açıklar. Tüm kapsayıcılar için aynı **birim** bağlama olduğunu belirtir, yani **C:\workspace onhost** , her üç kapsayıcıda **c:\workspace oncontainer dosyasına** bağlanır. Net etkisi, tüm kapsayıcıların etkinleştirildikleri sırayla aynı günlük dosyasına yazmalarıdır.

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
ServicePackage başarıyla etkinleştirildikten sonra, **C:\WorkspaceOnHost\log.txt** içerikleri aşağıdaki gibi olmalıdır.

```console
C:\Users\test>type C:\WorkspaceOnHost\log.txt
Hi from InitCodePackage0.
Hi from InitCodePackage1.
Hi from WorkloadCodePackage.
```

## <a name="next-steps"></a>Sonraki adımlar

İlgili bilgiler için aşağıdaki makalelere bakın.

* [Service Fabric ve kapsayıcılar.][containers-introduction-link]
* [Service Fabric ve konuk yürütülebilir dosyaları.][guest-executables-introduction-link]

<!-- Links -->
[containers-introduction-link]: service-fabric-containers-overview.md
[containers-getting-started-link]: service-fabric-get-started-containers.md
[guest-executables-introduction-link]: service-fabric-guest-executables-introduction.md
[reliable-services-link]: service-fabric-reliable-services-introduction.md
[application-model-link]: service-fabric-application-model.md
[hosting-model-link]: service-fabric-hosting-model.md
[setup-entry-point-link]: service-fabric-run-script-at-service-startup.md

