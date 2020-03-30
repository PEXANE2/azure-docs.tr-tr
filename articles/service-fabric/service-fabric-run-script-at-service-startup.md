---
title: Azure Hizmet Kumaşı hizmeti başladığında komut dosyası çalıştırma
description: Hizmet Kumaşı hizmet kurulum giriş noktası için bir ilkeyi nasıl yapılandıracağız ve hizmet başlangıç saatinde komut dosyası çalıştırmayı öğrenin.
author: athinanthny
ms.topic: conceptual
ms.date: 03/21/2018
ms.author: atsenthi
ms.openlocfilehash: a25f16f08ab8ae9564363f179d19d4b30c5315fa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75464279"
---
# <a name="run-a-service-startup-script-as-a-local-user-or-system-account"></a>Yerel kullanıcı veya sistem hesabı olarak bir hizmet başlangıcı komut dosyası çalıştırma
Hizmet Kumaşı hizmeti çalıştırılmadan önce bazı yapılandırma veya kurulum çalışmalarını çalıştırmak gerekebilir.  Örneğin, çevre değişkenlerini yapılandırma. Hizmet yürütülebilir hizmet için hizmet bildiriminde başlamadan önce çalışacak bir komut dosyası belirtebilirsiniz. Hizmet kurulum giriş noktası için bir RunAs ilkesi ni yapılandırarak, hangi hesabın çalıştırılabildiğini değiştirebilirsiniz.  Ayrı bir kurulum giriş noktası, yüksek ayrıcalıklı yapılandırmayı kısa bir süre için çalıştırmanızı sağlar, böylece hizmet ana bilgisayar çalıştırılabilir uzun süreler boyunca yüksek ayrıcalıklarla çalışmasına gerek yoktur.

Kurulum giriş noktası [(hizmet bildirimindeki](service-fabric-application-and-service-manifests.md)**SetupEntryPoint),** varsayılan olarak başka bir giriş noktasından önce Hizmet Dokusu (genellikle *NetworkService* hesabı) ile aynı kimlik bilgileriyle çalışan ayrıcalıklı bir giriş noktasıdır. **EntryPoint** tarafından belirtilen yürütülebilir genellikle uzun süren hizmet ana bilgisayardır. **EntryPoint** çalıştırılabilir **KurulumEntryPoint** çalıştırılabilir çıktıları başarıyla sonra çalıştırılır. Elde edilen işlem izlenir ve yeniden başlatılır ve sona ererse veya çökerse **SetupEntryPoint** ile yeniden başlar. 

## <a name="configure-the-service-setup-entry-point"></a>Hizmet kurulumu giriş noktasını yapılandırma
Aşağıda, kurulumGiriş **Noktası'nda**bir kurulum komut dosyası *MySetup.bat* belirten bir stateless hizmet için basit bir hizmet bildirimi örneğidir.  **Bağımsız değişkenler,** bağımsız değişkenleri çalıştırıldığında komut dosyasına geçirmek için kullanılır.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="MyStatelessServicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <Description>An example service manifest.</Description>
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="MyStatelessServiceType" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>MySetup.bat</Program>
        <Arguments>MyValue</Arguments>
        <WorkingFolder>Work</WorkingFolder>        
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>MyStatelessService.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpoint" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```
## <a name="configure-the-policy-for-a-service-setup-entry-point"></a>Hizmet kurulum giriş noktası için ilkeyi yapılandırma
Varsayılan olarak, hizmet kurulumu giriş noktası yürütülebilir Hizmet Kumaşı (genellikle *NetworkService* hesabı) ile aynı kimlik bilgileri altında çalışır.  Uygulama bildiriminde, başlangıç komut dosyasını yerel bir sistem hesabı veya yönetici hesabı altında çalıştırmak için güvenlik izinlerini değiştirebilirsiniz.

### <a name="configure-the-policy-by-using-a-local-system-account"></a>Yerel bir sistem hesabı kullanarak ilkeyi yapılandırma
Aşağıdaki uygulama bildirimi örneği, hizmet kurulum giriş noktasının kullanıcı yöneticisi hesabı (SetupAdminUser) altında çalışacak şekilde nasıl yapılandırılabildiğini gösterir.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="MyStatelessService_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyStatelessServicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <Service Name="MyStatelessService" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="MyStatelessServiceType" InstanceCount="[MyStatelessService_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
  <Principals>
    <Users>
      <User Name="SetupAdminUser">
        <MemberOf>
          <SystemGroup Name="Administrators" />
        </MemberOf>
      </User>
    </Users>
  </Principals>
</ApplicationManifest>
```

İlk olarak, SetupAdminUser gibi kullanıcı adında niçin bir **Müdürler** bölümü oluşturun. SetupAdminUser kullanıcı hesabı Yöneticiler sistem grubunun bir üyesidir.

Ardından, **ServiceManifestImport** bölümü altında, bu ilkeyi **SetupEntryPoint'e**uygulamak için bir ilke yapılandırın. Bu ilke, Service Fabric'e **MySetup.bat** dosyası çalıştırıldığında SetupAdminUser (yönetici ayrıcalıkları ile) olarak çalışması gerektiğini söyler. Ana giriş noktasına bir ilke *uygulamadığınız* için, **MyServiceHost.exe'deki** kod sistem **NetworkService** hesabı altında çalışır. Bu, tüm hizmet giriş noktalarının çalıştırılan varsayılan hesabıdır.

### <a name="configure-the-policy-by-using-local-system-accounts"></a>Yerel sistem hesaplarını kullanarak ilkeyi yapılandırma
Genellikle, başlangıç komut dosyasını yönetici hesabı yerine yerel bir sistem hesabı kullanarak çalıştırmak tercih edilir. Yöneticiler grubunun bir üyesi olarak RunAs ilkesini çalıştırmak genellikle iyi çalışmaz, çünkü bilgisayarlarvarsayılan olarak Kullanıcı Erişim Denetimi (UAC) etkinleştirilmiştir. Bu gibi durumlarda, öneri, Yöneticiler grubuna eklenen yerel bir kullanıcı yerine SetupEntryPoint'i LocalSystem olarak çalıştırmaktır. Aşağıdaki örnek, SetupEntryPoint'in LocalSystem olarak çalışacak şekilde ayarını gösterir:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyApplicationType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="MyStatelessService_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="MyStatelessServicePkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
         <RunAsPolicy CodePackageRef="Code" UserRef="SetupLocalSystem" EntryPointType="Setup" />
      </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <Service Name="MyStatelessService" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="MyStatelessServiceType" InstanceCount="[MyStatelessService_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
  <Principals>
      <Users>
         <User Name="SetupLocalSystem" AccountType="LocalSystem" />
      </Users>
   </Principals>
</ApplicationManifest>
```

> [!NOTE]
> Linux kümeleri için, bir hizmeti veya kurulum giriş noktasını **root**olarak çalıştırmak **için, AccountType'ı** **LocalSystem**olarak belirtebilirsiniz.

## <a name="run-a-script-from-the-setup-entry-point"></a>Kurulum giriş noktasından komut dosyası çalıştırma
Şimdi, yönetici ayrıcalıkları altında çalıştırmak için projeye bir başlangıç komut dosyası ekleyin. 

Visual Studio'da, servis projesini sağ tıklatın ve *MySetup.bat*adlı yeni bir dosya ekleyin.

Ardından, *MySetup.bat* dosyasının servis paketine dahil olduğundan emin olun. Varsayılan olarak, öyle değildir. Dosyayı seçin, bağlam menüsünü almak için sağ tıklatın ve **Özellikler'i**seçin. Özellikler iletişim kutusunda, Çıktı **Dizinine** Kopyala'nın **daha yeniyse Kopyala**olarak ayarlı olduğundan emin olun. Aşağıdaki ekran görüntüsüne bakın.

![SetupEntryPoint toplu dosya için Visual Studio CopyToOutput][image1]

Şimdi *MySetup.bat* dosyasını düzenle ve aşağıdaki komutları ekleyin bir sistem ortamı değişkeni ayarlayın ve bir metin dosyası çıktı:

```
REM Set a system environment variable. This requires administrator privilege
setx -m TestVariable %*
echo System TestVariable set to > out.txt
echo %TestVariable% >> out.txt

REM To delete this system variable us
REM REG delete "HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Session Manager\Environment" /v TestVariable /f
```

Ardından, çözümü yerel bir geliştirme kümesine oluşturun ve dağıtın. Hizmet başladıktan sonra, [Service Fabric Explorer'da](service-fabric-visualizing-your-cluster.md)gösterildiği gibi, MySetup.bat dosyasının iki şekilde başarılı olduğunu görebilirsiniz. PowerShell komut istemini açın ve yazın:

```
PS C:\ [Environment]::GetEnvironmentVariable("TestVariable","Machine")
MyValue
```

Ardından, hizmetin dağıtıldığı ve [Hizmet Kumaş Explorer'da](service-fabric-visualizing-your-cluster.md)başlatıldığı düğümün adını not edin. Örneğin, Düğüm 2. Ardından, **TestVariable**değerini gösteren out.txt dosyasını bulmak için uygulama örneği çalışma klasörüne gidin. Örneğin, bu hizmet Düğüm 2'ye dağıtıldıysa, **MyApplicationType**için bu yola gidebilirsiniz:

```
C:\SfDevCluster\Data\_App\Node.2\MyApplicationType_App\work\out.txt
```

## <a name="run-powershell-commands-from-a-setup-entry-point"></a>PowerShell komutlarını kurulum giriş noktasından çalıştırma
PowerShell'i **SetupEntryPoint** noktasından çalıştırmak için **PowerShell.exe'yi powershell** dosyasına işaret eden bir toplu iş dosyasında çalıştırabilirsiniz. İlk olarak, hizmet projesine bir PowerShell dosyası ekleyin --örneğin, **MySetup.ps1**. Dosyanın hizmet paketine eklenmesi için daha yeni özellik varsa *Kopya'yı* ayarlamayı unutmayın. Aşağıdaki örnek, **TestVariable**adlı bir sistem ortamı değişkeni ayarlayan MySetup.ps1 adlı bir PowerShell dosyasını başlatan örnek bir toplu iş dosyasını gösterir.

MySetup.bat bir PowerShell dosyası başlatmak için:

```
powershell.exe -ExecutionPolicy Bypass -Command ".\MySetup.ps1"
```

PowerShell dosyasında, bir sistem ortamı değişkeni ayarlamak için aşağıdakileri ekleyin:

```
[Environment]::SetEnvironmentVariable("TestVariable", "MyValue", "Machine")
[Environment]::GetEnvironmentVariable("TestVariable","Machine") > out.txt
```

> [!NOTE]
> Varsayılan olarak, toplu iş dosyası çalıştığında, dosyalar için **çalışma** adı verilen uygulama klasörüne bakar. Bu durumda, MySetup.bat çalıştığında, bu uygulama **kodu paketi** klasörü olan aynı klasörde MySetup.ps1 dosyasını bulmak istiyorum. Bu klasörü değiştirmek için çalışma klasörünü ayarlayın:
> 
> 

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    </ExeHost>
</SetupEntryPoint>
```

## <a name="debug-a-startup-script-locally-using-console-redirection"></a>Konsol yeniden yönlendirmeyi kullanarak bir başlangıç komut dosyasının hatasını yerel olarak
Bazen, bir kurulum komut dosyası çalıştıran konsol çıktısını görmek hata ayıklama amacıyla yararlıdır. Çıktıyı bir dosyaya yazan hizmet bildirimindeki kurulum giriş noktasına konsol yönlendirme ilkesi ayarlayabilirsiniz. Dosya çıktısı, uygulamanın dağıtıldığı ve çalıştırıldığı küme düğümünde **günlük** adı verilen uygulama klasörüne yazılır. 

> [!WARNING]
> Konsol yeniden yönlendirme ilkesini üretimde dağıtılan bir uygulamada asla kullanmayın, çünkü bu uygulamanın başarısız olup olmadığını etkileyebilir. Bunu *yalnızca* yerel geliştirme ve hata ayıklama amacıyla kullanın.  
> 
> 

Aşağıdaki hizmet bildirimi örneği, konsolun yeniden yönünü FileReCount değeriyle ayarlamayı gösterir:

```xml
<SetupEntryPoint>
    <ExeHost>
    <Program>MySetup.bat</Program>
    <WorkingFolder>CodePackage</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="10"/>
    </ExeHost>
</SetupEntryPoint>
```

Şimdi Bir **Echo** komutu yazmak için MySetup.ps1 dosyasını değiştirirseniz, bu hata ayıklama amacıyla çıktı dosyasına yazacaktır:

```
Echo "Test console redirection which writes to the application log folder on the node that the application is deployed to"
```

> [!WARNING]
> Komut dosyanızı hata ayıklamasonra, hemen bu konsol yeniden yönlendirme ilkesini kaldırın.



<!--Every topic should have next steps and links to the next logical set of content to keep the customer engaged-->
## <a name="next-steps"></a>Sonraki adımlar
* [Uygulama ve hizmet güvenliği hakkında bilgi edinin](service-fabric-application-and-service-security.md)
* [Uygulama modelini anlama](service-fabric-application-model.md)
* [Hizmet bildiriminde kaynakları belirtin](service-fabric-service-manifest-resources.md)
* [Uygulama dağıtma](service-fabric-deploy-remove-applications.md)

[image1]: ./media/service-fabric-application-runas-security/copy-to-output.png
