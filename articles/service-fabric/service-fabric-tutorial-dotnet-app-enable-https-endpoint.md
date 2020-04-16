---
title: Kestrel kullanarak BIR HTTPS bitiş noktası ekleme
description: Bu öğreticide Kestrel kullanarak bir ASP.NET Core ön uç web hizmetine HTTPS uç noktası ekleme ve uygulamayı bir kümeye dağıtma işlemi hakkında bilgi alacaksınız.
ms.topic: tutorial
ms.date: 07/22/2019
ms.custom: mvc
ms.openlocfilehash: aafe2e7c89f6d4a90806378e9cf25c81f51feb60
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81411175"
---
# <a name="tutorial-add-an-https-endpoint-to-an-aspnet-core-web-api-front-end-service-using-kestrel"></a>Öğretici: Kestrel kullanarak bir ASP.NET Core Web API’si ön uç hizmetine HTTPS uç noktası ekleme

Bu öğretici, bir serinin üçüncü bölümüdür.  Service Fabric üzerinde çalışan bir ASP.NET Core hizmetinde HTTPS’yi etkinleştirme hakkında bilgi edineceksiniz. İşlemi tamamladığınızda, 443 numaralı bağlantı noktasını dinleyen HTTPS özellikli bir ASP.NET Core web ön ucu içeren bir oylama uygulamasına sahip olursunuz. Oylama uygulamasını [.NET Service Fabric uygulaması derleme](service-fabric-tutorial-deploy-app-to-party-cluster.md) bölümünde el ile oluşturmak istemiyorsanız, tamamlanmış uygulamanın [kaynak kodunu indirebilirsiniz](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/).

Serinin üçüncü bölümünde şunları öğrenirsiniz:

> [!div class="checklist"]
> * Hizmette bir HTTPS uç noktası tanımlama
> * Kestrel’i HTTPS kullanacak şekilde yapılandırma
> * TLS/SSL sertifikasını uzak küme düğümlerine yükleme
> * Sertifikanın özel anahtarına AĞ HİZMETİ erişimi verme
> * Azure yük dengeleyicide 443 numaralı bağlantı noktasını açma
> * Uygulamayı uzak kümeye dağıtma

Bu öğretici dizisinde şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * [.NET Service Fabric uygulaması oluşturma](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * [Uygulamayı uzak kümeye dağıtma](service-fabric-tutorial-deploy-app-to-party-cluster.md)
> * Bir ASP.NET Core ön uç hizmetine HTTPS uç noktası ekleme
> * [Azure Pipelines kullanarak CI/CD yapılandırma](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)
> * [Uygulama için izleme ve tanılamayı ayarlama](service-fabric-tutorial-monitoring-aspnet.md)


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiye başlamadan önce:

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun
* [Visual Studio 2019](https://www.visualstudio.com/) sürümünü 15.5 veya sonraki sürümlerle **Azure geliştirme** ve ASP.NET ve **web geliştirme** iş yükleriyle yükleyin.
* [Servis Kumaşı SDK'yı yükleyin](service-fabric-get-started.md)

## <a name="obtain-a-certificate-or-create-a-self-signed-development-certificate"></a>Sertifika edinme veya otomatik olarak imzalanan geliştirme sertifikası oluşturma

Üretim uygulamaları için [bir sertifika yetkilisinden (CA)](https://wikipedia.org/wiki/Certificate_authority) alınan bir sertifikayı kullanın. Geliştirme ve test için otomatik olarak imzalanan bir sertifika oluşturup bunu kullanabilirsiniz. Service Fabric SDK’sı, otomatik olarak imzalanan bir sertifika oluşturup `Cert:\LocalMachine\My` sertifika deposuna aktaran *CertSetup.ps1* betiğini sağlar. Yönetici olarak bir komut istemi açın ve "CN=mytestcert" öznesi olan bir sertifika oluşturmak için aşağıdaki komutu çalıştırın:

```powershell
PS C:\program files\microsoft sdks\service fabric\clustersetup\secure> .\CertSetup.ps1 -Install -CertSubjectName CN=mytestcert
```

Zaten bir sertifika PFX dosyanız varsa sertifikayı `Cert:\LocalMachine\My` sertifika deposuna aktarmak için aşağıdaki komutu çalıştırın:

```powershell

PS C:\mycertificates> Import-PfxCertificate -FilePath .\mysslcertificate.pfx -CertStoreLocation Cert:\LocalMachine\My -Password (ConvertTo-SecureString "!Passw0rd321" -AsPlainText -Force)


   PSParentPath: Microsoft.PowerShell.Security\Certificate::LocalMachine\My

Thumbprint                                Subject
----------                                -------
3B138D84C077C292579BA35E4410634E164075CD  CN=zwin7fh14scd.westus.cloudapp.azure.com
```

## <a name="define-an-https-endpoint-in-the-service-manifest"></a>Hizmet bildiriminde bir HTTPS uç noktası tanımlama

Visual Studio’yu **yönetici** olarak başlatın ve Oylama çözümünü açın. Çözüm Explorer, açık *VotingWeb / PackageRoot / ServiceManifest.xml*. Hizmet bildirimi, hizmet uç noktalarını tanımlar.  **Uç Noktalar** bölümünü bulun ve var olan "ServiceEndpoint" uç noktasını düzenleyin.  Adı "EndpointHttps" olarak değiştirin, protokolü *https*, türü *Girdi*, bağlantı noktasını ise *443* olarak ayarlayın.  Yaptığınız değişiklikleri kaydedin.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="VotingWebPkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="VotingWebType" />
  </ServiceTypes>

  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>VotingWeb.exe</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <Endpoint Protocol="https" Name="EndpointHttps" Type="Input" Port="443" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="configure-kestrel-to-use-https"></a>Kestrel’i HTTPS kullanacak şekilde yapılandırma

Çözüm Gezgini'nde *VotingWeb/VotingWeb.cs* dosyasını açın.  Kestrel’i HTTPS kullanacak şekilde yapılandırın ve `Cert:\LocalMachine\My` deposunda sertifikayı arayın. Deyimleri kullanarak aşağıdakileri ekleyin:

```csharp
using System.Net;
using Microsoft.Extensions.Configuration;
using System.Security.Cryptography.X509Certificates;
```

`ServiceInstanceListener` değerini yeni *EndpointHttps* uç noktasını kullanacak ve 443 numaralı bağlantı noktasını dinleyecek şekilde güncelleştirin. Web ana bilgisayarını Kestrel sunucusunu kullanacak şekilde yapılandırırken Kestrel'i tüm ağ arabirimlerinde IPv6 adreslerini dinleyecek şekilde yapılandırmanız gerekir: `opt.Listen(IPAddress.IPv6Any, port, listenOptions => {...}`.

```csharp
new ServiceInstanceListener(
serviceContext =>
    new KestrelCommunicationListener(
        serviceContext,
        "EndpointHttps",
        (url, listener) =>
        {
            ServiceEventSource.Current.ServiceMessage(serviceContext, $"Starting Kestrel on {url}");

            return new WebHostBuilder()
                .UseKestrel(opt =>
                {
                    int port = serviceContext.CodePackageActivationContext.GetEndpoint("EndpointHttps").Port;
                    opt.Listen(IPAddress.IPv6Any, port, listenOptions =>
                    {
                        listenOptions.UseHttps(GetHttpsCertificateFromStore());
                        listenOptions.NoDelay = true;
                    });
                })
                .ConfigureAppConfiguration((builderContext, config) =>
                {
                    config.AddJsonFile("appsettings.json", optional: false, reloadOnChange: true);
                })

                .ConfigureServices(
                    services => services
                        .AddSingleton<HttpClient>(new HttpClient())
                        .AddSingleton<FabricClient>(new FabricClient())
                        .AddSingleton<StatelessServiceContext>(serviceContext))
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseStartup<Startup>()
                .UseServiceFabricIntegration(listener, ServiceFabricIntegrationOptions.None)
                .UseUrls(url)
                .Build();
        }))
```

Ayrıca, Kestrel’in konuyu kullanarak sertifikayı `Cert:\LocalMachine\My` deposunda bulabilmesi için aşağıdaki yöntemi ekleyin.  

Önceki&lt;PowerShell komutuyla kendi imzalı bir sertifika oluşturduysanız veya sertifikanızın CN'sini kullanıyorsanız " your_CN_value"&gt;ile "mytestcert" ile değiştirin.
Yerel dağıtım `localhost` söz konusu olduğunda kimlik doğrulama özel durumlarını önlemek için "CN=localhost" kullanılmasının tercih edildiğini unutmayın.

```csharp
private X509Certificate2 FindMatchingCertificateBySubject(string subjectCommonName)
{
    using (var store = new X509Store(StoreName.My, StoreLocation.LocalMachine))
    {
        store.Open(OpenFlags.OpenExistingOnly | OpenFlags.ReadOnly);
        var certCollection = store.Certificates;
        var matchingCerts = new X509Certificate2Collection();
    
    foreach (var enumeratedCert in certCollection)
    {
      if (StringComparer.OrdinalIgnoreCase.Equals(subjectCommonName, enumeratedCert.GetNameInfo(X509NameType.SimpleName, forIssuer: false))
        && DateTime.Now < enumeratedCert.NotAfter
        && DateTime.Now >= enumeratedCert.NotBefore)
        {
          matchingCerts.Add(enumeratedCert);
        }
    }

        if (matchingCerts.Count == 0)
    {
        throw new Exception($"Could not find a match for a certificate with subject 'CN={subjectCommonName}'.");
    }
        
        return matchingCerts[0];
    }
}


```

## <a name="grant-network-service-access-to-the-certificates-private-key"></a>SERTIFIKANIN özel anahtarına NETWORK SERVICE erişimi ver

Önceki adımlardan birinde sertifikayı geliştirme bilgisayarındaki `Cert:\LocalMachine\My` deposuna aktarmıştınız.  Şimdi, hizmeti çalıştıran hesaba açıkça (NETWORK SERVICE, varsayılan olarak) sertifikanın özel anahtarına erişim verin. Bu adımı el ile (certlm.msc aracını kullanarak) yapabilirsiniz, ancak hizmet bildiriminin **SetupEntryPoint'inde** [bir başlangıç komut dosyası yapılandırarak](service-fabric-run-script-at-service-startup.md) bir PowerShell komut dosyasını otomatik olarak çalıştırmak daha iyidir.

>[!NOTE]
> Service Fabric, son nokta sertifikalarını parmak izine veya özne ortak adına göre bildirmeyi destekler. Bu durumda, çalışma zamanı bağlamayı ve ACL sertifikanın hizmetin olarak çalıştırdığı kimlik için özel anahtarını ayarlar. Çalışma süresi ayrıca değişiklikler/yenilemeler için sertifikayı izler ve ilgili özel anahtarı buna göre yeniden acl olarak yineler.

### <a name="configure-the-service-setup-entry-point"></a>Hizmet kurulumu giriş noktasını yapılandırma

Çözüm Explorer, açık *VotingWeb / PackageRoot / ServiceManifest.xml*.  **CodePackage** bölümünde **SetupEntryPoint** düğümünü ve sonra bir **ExeHost** düğümünü ekleyin.  **ExeHost** düğümünde **Program** ayarını "Setup.bat" ve **WorkingFolder** ayarını "CodePackage" olarak belirleyin.  VotingWeb hizmeti başlatıldığında, VotingWeb.exe başlatılmadan önce CodePackage klasöründe Setup.bat betiği yürütülür.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="VotingWebPkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <StatelessServiceType ServiceTypeName="VotingWebType" />
  </ServiceTypes>

  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>Setup.bat</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </SetupEntryPoint>

    <EntryPoint>
      <ExeHost>
        <Program>VotingWeb.exe</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <Endpoint Protocol="https" Name="EndpointHttps" Type="Input" Port="443" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

### <a name="add-the-batch-and-powershell-setup-scripts"></a>Toplu işi ve PowerShell kurulum betiklerini ekleme

PowerShell’i **SetupEntryPoint** noktasından çalıştırmak için PowerShell.exe dosyasını bir PowerShell dosyasını işaret eden toplu iş dosyasında çalıştırabilirsiniz. İlk olarak, toplu iş dosyasını hizmet projesine ekleyin.  Çözüm Gezgini’nde **VotingWeb**’e sağ tıklayıp **Ekle**->**Yeni Öğe**’yi seçin ve "Setup.bat" adlı yeni bir dosya ekleyin.  *Setup.bat* dosyasını düzenleyin ve aşağıdaki komutu ekleyin:

```bat
powershell.exe -ExecutionPolicy Bypass -Command ".\SetCertAccess.ps1"
```

*Setup.bat* dosya özelliklerini, **Çıkış Dizinine Kopyala** değerini "Daha yeniyse kopyala" olarak ayarlayacak şekilde değiştirin.

![Dosya özelliklerini ayarlama][image1]

Çözüm Gezgini’nde **VotingWeb**’e sağ tıklayıp **Ekle**->**Yeni Öğe**’yi seçin ve "SetCertAccess.ps1" adlı yeni bir dosya ekleyin.  *SetCertAccess.ps1* dosyasını düzenleyin ve aşağıdaki betiği ekleyin:

```powershell
$subject="mytestcert"
$userGroup="NETWORK SERVICE"

Write-Host "Checking permissions to certificate $subject.." -ForegroundColor DarkCyan

$cert = (gci Cert:\LocalMachine\My\ | where { $_.Subject.Contains($subject) })[-1]

if ($cert -eq $null)
{
    $message="Certificate with subject:"+$subject+" does not exist at Cert:\LocalMachine\My\"
    Write-Host $message -ForegroundColor Red
    exit 1;
}elseif($cert.HasPrivateKey -eq $false){
    $message="Certificate with subject:"+$subject+" does not have a private key"
    Write-Host $message -ForegroundColor Red
    exit 1;
}else
{
    $keyName=$cert.PrivateKey.CspKeyContainerInfo.UniqueKeyContainerName

    $keyPath = "C:\ProgramData\Microsoft\Crypto\RSA\MachineKeys\"
    $fullPath=$keyPath+$keyName
    $acl=(Get-Item $fullPath).GetAccessControl('Access')


    $hasPermissionsAlready = ($acl.Access | where {$_.IdentityReference.Value.Contains($userGroup.ToUpperInvariant()) -and $_.FileSystemRights -eq [System.Security.AccessControl.FileSystemRights]::FullControl}).Count -eq 1

    if ($hasPermissionsAlready){
        Write-Host "Account $userGroup already has permissions to certificate '$subject'." -ForegroundColor Green
        return $false;
    } else {
        Write-Host "Need add permissions to '$subject' certificate..." -ForegroundColor DarkYellow

        $permission=$userGroup,"Full","Allow"
        $accessRule=new-object System.Security.AccessControl.FileSystemAccessRule $permission
        $acl.AddAccessRule($accessRule)
        Set-Acl $fullPath $acl

        Write-Output "Permissions were added"

        return $true;
    }
}

```

*SetCertAccess.ps1* dosya özelliklerini, **Çıkış Dizinine Kopyala** değerini "Daha yeniyse kopyala" olarak ayarlayacak şekilde değiştirin.

### <a name="run-the-setup-script-as-a-local-administrator"></a>Kurulum betiğini yerel yönetici olarak çalıştırma

Varsayılan olarak, hizmet kurulumu giriş noktası yürütülebilir dosyası, Service Fabric ile aynı kimlik bilgileri altında (genellikle NetworkService hesabı) çalışır. *SetCertAccess.ps1* için yönetici ayrıcalıkları gerekir. Uygulama bildiriminde güvenlik izinlerini bir yerel yönetici hesabı altındaki başlangıç betiğini çalıştıracak şekilde değiştirebilirsiniz.

Çözüm Gezgini’nde *Voting/ApplicationPackageRoot/ApplicationManifest.xml* dosyasını açın. İlk olarak bir **Sorumlular** bölümü oluşturun ve yeni bir kullanıcı ekleyin (örneğin, "SetupAdminUser". SetupAdminUser kullanıcı hesabını Administrators sistem grubuna ekleyin.
Sonra, VotingWebPkg **ServiceManifestImport** bölümünde bir **RunAsPolicy** yapılandırarak SetupAdminUser sorumlusunu kurulum giriş noktasına uygulayın. Bu ilke, Service Fabric’e Setup.bat dosyasının SetupAdminUser olarak (yönetici ayrıcalıklarıyla) çalıştığını belirtir.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VotingType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="VotingData_MinReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingData_PartitionCount" DefaultValue="1" />
    <Parameter Name="VotingData_TargetReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingWeb_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingDataPkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
  </ServiceManifestImport>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingWebPkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <Service Name="VotingData">
      <StatefulService ServiceTypeName="VotingDataType" TargetReplicaSetSize="[VotingData_TargetReplicaSetSize]" MinReplicaSetSize="[VotingData_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[VotingData_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    <Service Name="VotingWeb" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="VotingWebType" InstanceCount="[VotingWeb_InstanceCount]">
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

## <a name="run-the-application-locally"></a>Uygulamayı yerel olarak çalıştırma

Solution Explorer'da **Oylama** uygulamasını seçin ve **Uygulama URL** \/özelliğini "https: /localhost:443" olarak ayarlayın.

Tüm dosyaları kaydedin ve F5’e basarak uygulamayı yerel olarak çalıştırın.  Uygulama dağıtıldıktan sonra https:\//localhost:443 adresine bir web tarayıcısı açılır. Otomatik olarak imzalanan bir sertifika kullanıyorsanız, bilgisayarınızın bu web sitesinin güvenliğine güvenmediğini bildiren bir uyarı görürsünüz.  Web sayfasına devam edin.

![Oylama uygulaması][image2]

## <a name="install-certificate-on-cluster-nodes"></a>Küme düğümlerine sertifika yükleme

Uygulamayı Azure'a dağıtmadan önce, sertifikayı `Cert:\LocalMachine\My` tüm uzak küme düğümlerinin deposuna yükleyin.  Hizmetler kümenin farklı düğümlerine taşınabilir.  Ön uç web hizmeti bir küme düğümünde başladığında, başlangıç betiği sertifikayı arar ve erişim izinlerini yapılandırır.

İlk olarak, sertifikayı bir PFX dosyasına aktarın. certlm.msc uygulamasını açın ve **Kişisel**>**Sertifikalar'a**gidin.  *Mytestcert* sertifikasına sağ tıklayın ve **Tüm Görevler**>**Dışa Aktarma'yı**seçin.

![Sertifikayı dışarı aktarma][image4]

Dışarı aktarma sihirbazında **Evet, özel anahtarı dışarı aktar** seçeneğini belirleyin ve Kişisel Bilgi Değişimi (PFX) biçimini seçin.  Dosyayı *C:\Users\sfuser\votingappcert.pfx* konumuna aktarın.

Ardından, [bu sağlanan Powershell komut dosyalarını](./scripts/service-fabric-powershell-add-application-certificate.md)kullanarak sertifikayı uzak kümeye yükleyin.

> [!Warning]
> Geliştirme ve test uygulamaları için otomatik olarak imzalanan bir sertifika yeterlidir. Üretim uygulamaları için otomatik olarak imzalanan sertifika yerine [bir sertifika yetkilisinden (CA)](https://wikipedia.org/wiki/Certificate_authority) sertifikayı kullanın.

## <a name="open-port-443-in-the-azure-load-balancer"></a>Azure yük dengeleyicide 443 numaralı bağlantı noktasını açma

Henüz açık değilse yük dengeleyicide 443 numaralı bağlantı noktasını açın.

```powershell
$probename = "AppPortProbe6"
$rulename="AppPortLBRule6"
$RGname="voting_RG"
$port=443

# Get the load balancer resource
$resource = Get-AzResource | Where {$_.ResourceGroupName –eq $RGname -and $_.ResourceType -eq "Microsoft.Network/loadBalancers"}
$slb = Get-AzLoadBalancer -Name $resource.Name -ResourceGroupName $RGname

# Add a new probe configuration to the load balancer
$slb | Add-AzLoadBalancerProbeConfig -Name $probename -Protocol Tcp -Port $port -IntervalInSeconds 15 -ProbeCount 2

# Add rule configuration to the load balancer
$probe = Get-AzLoadBalancerProbeConfig -Name $probename -LoadBalancer $slb
$slb | Add-AzLoadBalancerRuleConfig -Name $rulename -BackendAddressPool $slb.BackendAddressPools[0] -FrontendIpConfiguration $slb.FrontendIpConfigurations[0] -Probe $probe -Protocol Tcp -FrontendPort $port -BackendPort $port

# Set the goal state for the load balancer
$slb | Set-AzLoadBalancer
```

## <a name="deploy-the-application-to-azure"></a>Uygulamayı Azure’a dağıtma

Tüm dosyaları kaydedin, Hata Ayıklama’dan Yayın’a geçin ve F6’ya basarak yeniden oluşturun.  Çözüm Gezgini’nde **Oylama**’ya sağ tıklayın ve **Yayımla**’yı seçin. [Bir kümeye uygulama dağıtma](service-fabric-tutorial-deploy-app-to-party-cluster.md) bölümünde oluşturulan kümenin bağlantı uç noktasını veya başka bir kümeyi seçin.  Uygulamayı uzak kümede yayımlamak için **Yayımla**’ya tıklayın.

Uygulama dağıtıldığında, bir web tarayıcısı [https://mycluster.region.cloudapp.azure.com:443](https://mycluster.region.cloudapp.azure.com:443) açın ve (kümenizin bağlantı bitiş noktasıyla URL'yi güncelleyin). Otomatik olarak imzalanan bir sertifika kullanıyorsanız, bilgisayarınızın bu web sitesinin güvenliğine güvenmediğini bildiren bir uyarı görürsünüz.  Web sayfasına devam edin.

![Oylama uygulaması][image3]

## <a name="next-steps"></a>Sonraki adımlar

Öğreticinin bu bölümünde, şunların nasıl yapıldığını öğrendiniz:

> [!div class="checklist"]
> * Hizmette bir HTTPS uç noktası tanımlama
> * Kestrel’i HTTPS kullanacak şekilde yapılandırma
> * TLS/SSL sertifikasını uzak küme düğümlerine yükleme
> * Sertifikanın özel anahtarına AĞ HİZMETİ erişimi verme
> * Azure yük dengeleyicide 443 numaralı bağlantı noktasını açma
> * Uygulamayı uzak kümeye dağıtma

Sonraki öğreticiye ilerleyin:
> [!div class="nextstepaction"]
> [Azure Pipelines kullanarak CI/CD yapılandırma](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)

[image1]: ./media/service-fabric-tutorial-dotnet-app-enable-https-endpoint/SetupBatProperties.png
[image2]: ./media/service-fabric-tutorial-dotnet-app-enable-https-endpoint/VotingAppLocal.png
[image3]: ./media/service-fabric-tutorial-dotnet-app-enable-https-endpoint/VotingAppAzure.png
[image4]: ./media/service-fabric-tutorial-dotnet-app-enable-https-endpoint/ExportCert.png
