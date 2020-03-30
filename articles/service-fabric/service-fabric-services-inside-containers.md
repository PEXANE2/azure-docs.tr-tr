---
title: Azure Hizmet Kumaşı hizmetlerinizi Windows'da kapsayıcı hale
description: Windows'da Hizmet Kumaşı Güvenilir Hizmetleri ve Güvenilir Aktörler hizmetlerinizi nasıl kaplaştırabileceğinizi öğrenin.
ms.topic: conceptual
ms.date: 5/23/2018
ms.author: anmola
ms.openlocfilehash: 9fe5980c13f655f8f30cc42771971a5015460420
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75466176"
---
# <a name="containerize-your-service-fabric-reliable-services-and-reliable-actors-on-windows"></a>Windows'da Service Fabric Reliable Services ve Reliable Actors kapsayıcıları

Service Fabric, Servis Kumaşı mikro hizmetlerinin (Güvenilir Hizmetler ve Güvenilir Aktör tabanlı hizmetler) konteynerleştirilmesini destekler. Daha fazla bilgi için [servis kumaş kapları](service-fabric-containers-overview.md)bakın.

Bu belge, hizmetinizin bir Windows kapsayıcısı içinde çalışmasını sağlamak için kılavuz sağlar.

> [!NOTE]
> Şu anda bu özellik yalnızca Windows için çalışır. Kapsayıcıları çalıştırmak için kümenin Windows Server 2016'da Kapsayıcılarla birlikte çalışıyor olması gerekir.

## <a name="steps-to-containerize-your-service-fabric-application"></a>Servis Kumaşı Uygulamanızı konteynerleştirme adımları

1. Service Fabric uygulamanızı Visual Studio'da açın.

2. Projenize sınıf [SFBinaryLoader.cs](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/code/SFBinaryLoaderForContainers/SFBinaryLoader.cs) ekleyin. Bu sınıftaki kod, bir kapsayıcının içinde çalışırken Servis Kumaşı çalışma zamanı ikililerini uygulamanızın içine doğru şekilde yüklemek için yardımcı dır.

3. Konteynerleştirmek istediğiniz her kod paketi için, program giriş noktasındaki yükleyiciyi başlattı. Program giriş noktası dosyanıza aşağıdaki kod snippet'inde gösterilen statik oluşturucuyu ekleyin.

   ```csharp
   namespace MyApplication
   {
      internal static class Program
      {
          static Program()
          {
              SFBinaryLoader.Initialize();
          }

          /// <summary>
          /// This is the entry point of the service host process.
          /// </summary>
          private static void Main()
          {
   ```

4. Projenizi oluşturun ve [paketle.](service-fabric-package-apps.md#Package-App) Bir paket oluşturmak ve oluşturmak için Solution Explorer'daki uygulama projesini sağ tıklatın ve **Paket** komutunu seçin.

5. Konteynerlemeniz gereken her kod paketi için PowerShell komut dosyası [CreateDockerPackage.ps1'i](https://github.com/Azure/service-fabric-scripts-and-templates/blob/master/scripts/CodePackageToDockerPackage/CreateDockerPackage.ps1)çalıştırın. Kullanımı aşağıdaki gibidir:

    Tam .NET
      ```powershell
        $codePackagePath = 'Path to the code package to containerize.'
        $dockerPackageOutputDirectoryPath = 'Output path for the generated docker folder.'
        $applicationExeName = 'Name of the Code package executable.'
        CreateDockerPackage.ps1 -CodePackageDirectoryPath $codePackagePath -DockerPackageOutputDirectoryPath $dockerPackageOutputDirectoryPath -ApplicationExeName $applicationExeName
      ```
    .NET Core
      ```powershell
        $codePackagePath = 'Path to the code package to containerize.'
        $dockerPackageOutputDirectoryPath = 'Output path for the generated docker folder.'
        $dotnetCoreDllName = 'Name of the Code package dotnet Core Dll.'
        CreateDockerPackage.ps1 -CodePackageDirectoryPath $codePackagePath -DockerPackageOutputDirectoryPath $dockerPackageOutputDirectoryPath -DotnetCoreDllName $dotnetCoreDllName
      ```
      Komut dosyası, $dockerPackageOutputDirectoryPath Docker yapıtlarının bulunduğu bir klasör oluşturur. Oluşturulan Dockerfile'ı `expose` herhangi bir bağlantı noktasıyla değiştirin, kurulum komut dosyalarını çalıştırın ve benzeri. ihtiyaçlarınıza göre.

6. Daha sonra Docker konteyner paketinizi [oluşturup](service-fabric-get-started-containers.md#Build-Containers) deponuza [itmeniz](service-fabric-get-started-containers.md#Push-Containers) gerekir.

7. Kapsayıcı resminizi, depo bilgilerinizi, kayıt defteri kimlik doğrulamasını ve bağlantı noktası eşlemesini eklemek için ApplicationManifest.xml ve ServiceManifest.xml'i değiştirin. Bildirimleri değiştirmek için azure [hizmeti kumaş kapsayıcı uygulaması oluştur'a](service-fabric-get-started-containers.md)bakın. Hizmet bildirimindeki kod paketi tanımının ilgili kapsayıcı görüntüsüyle değiştirilmesi gerekir. EntryPoint'i ContainerHost türüyle değiştirdiğinden emin olun.

   ```xml
   <!-- Code package is your service executable. -->
   <CodePackage Name="Code" Version="1.0.0">
   <EntryPoint>
    <!-- Follow this link for more information about deploying Windows containers to Service Fabric: https://aka.ms/sfguestcontainers -->
    <ContainerHost>
      <ImageName>myregistry.azurecr.io/samples/helloworldapp</ImageName>
    </ContainerHost>
   </EntryPoint>
   <!-- Pass environment variables to your container: -->
   </CodePackage>
   ```

8. Çoğalıcınız ve hizmet bitiş noktanız için bağlantı noktası-anabilgisayar eşlecisini ekleyin. Her iki bağlantı noktası da Service Fabric tarafından çalışma zamanında atanır, ContainerPort eşleme için atanan bağlantı noktasını kullanmak üzere sıfıra ayarlanır.

   ```xml
   <Policies>
   <ContainerHostPolicies CodePackageRef="Code">
    <PortBinding ContainerPort="0" EndpointRef="ServiceEndpoint"/>
    <PortBinding ContainerPort="0" EndpointRef="ReplicatorEndpoint"/>
   </ContainerHostPolicies>
   </Policies>
   ```

9. Kapsayıcı yalıtım modunu yapılandırmak için [bkz.]( https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-containers#configure-isolation-mode) Windows, kapsayıcılar için iki yalıtım modunu destekler: İşlem ve Hyper-V. Aşağıdaki parçacıklar, uygulama bildirimi dosyasında yalıtım modunun nasıl belirtildiğini gösterir.

   ```xml
   <Policies>
   <ContainerHostPolicies CodePackageRef="Code" Isolation="process">
   ...
   </ContainerHostPolicies>
   </Policies>
   ```
   ```xml
   <Policies>
   <ContainerHostPolicies CodePackageRef="Code" Isolation="hyperv">
   ...
   </ContainerHostPolicies>
   </Policies>
   ```

> [!NOTE] 
> Varsayılan olarak, Service Fabric uygulamaları, uygulamaya özgü istekleri kabul eden bir bitiş noktası şeklinde Hizmet Kumaşı çalışma süresine erişebilir. Uygulama güvenilmeyen kodu barındırdığında bu erişimi devre dışı bırakmayı düşünün. Daha fazla bilgi için, [Service Fabric güvenlik en iyi uygulamaları](service-fabric-best-practices-security.md#platform-isolation)bakın. Hizmet Dokusu çalışma süresine erişimi devre dışı katmak için, uygulama bildiriminin içe aktarılan hizmet bildirimine karşılık gelen İlkeler bölümünde aşağıdaki ayarı aşağıdaki gibi ekleyin:
>
```xml
  <Policies>
      <ServiceFabricRuntimeAccessPolicy RemoveServiceFabricRuntimeAccess="true"/>
  </Policies>
```
>

10. Bu uygulamayı sınamak için, sürüm 5.7 veya daha yüksek çalışan bir kümeye dağıtmanız gerekir. Çalışma zamanı sürümleri 6.1 veya daha düşük için, bu önizleme özelliğini etkinleştirmek için küme ayarlarını değiştirmeniz ve güncelleştirmeniz gerekir. Sonraki gösterilen ayarı eklemek için bu [makalede](service-fabric-cluster-fabric-settings.md) adımları izleyin.
    ```
      {
        "name": "Hosting",
        "parameters": [
          {
            "name": "FabricContainerAppsEnabled",
            "value": "true"
          }
        ]
      }
    ```

11. Sonraki düzenlenen uygulama paketini bu kümeye [dağıtın.](service-fabric-deploy-remove-applications.md)

Artık kümenizi çalıştıran kapsayıcıbir Service Fabric uygulamasına sahip olmalısınız.

## <a name="next-steps"></a>Sonraki adımlar
* [Service Fabric’te kapsayıcı](service-fabric-get-started-containers.md) çalıştırma hakkında daha fazla bilgi edinin.
* Service Fabric [uygulama yaşam döngüsü](service-fabric-application-lifecycle.md) hakkında bilgi edinin.
