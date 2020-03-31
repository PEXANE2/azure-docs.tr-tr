---
title: PowerShell ile Azure Hizmet Kumaşı dağıtımı
description: Azure Hizmet Kumaşı'ndaki uygulamaları kaldırma ve dağıtma ve bu eylemlerin Powershell'de nasıl gerçekleştirilebildiğini öğrenin.
ms.topic: conceptual
ms.date: 01/19/2018
ms.openlocfilehash: e3fdd194f2949f1246e991968e02b3278f33f7db
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282516"
---
# <a name="deploy-and-remove-applications-using-powershell"></a>PowerShell kullanarak uygulamaları dağıtma ve kaldırma

> [!div class="op_single_selector"]
> * [Kaynak Yöneticisi](service-fabric-application-arm-resource.md)
> * [Powershell](service-fabric-deploy-remove-applications.md)
> * [Service Fabric CLI](service-fabric-application-lifecycle-sfctl.md)
> * [FabricClient API’leri](service-fabric-deploy-remove-applications-fabricclient.md)

<br/>

Bir [uygulama türü paketlendikten][10]sonra, azure hizmet kumaşı kümesine dağıtıma hazırdır. Dağıtım aşağıdaki üç adımı içerir:

1. Uygulama paketini resim mağazasına yükleyin.
2. Uygulama türünü görüntü deposu göreli yoluna kaydedin.
3. Uygulama örneğini oluşturun.

Dağıtılan uygulama artık gerekli olmadığında, uygulama örneğini ve uygulama türünü silebilirsiniz. Bir uygulamayı kümeden tamamen kaldırmak için aşağıdaki adımlar oluşur:

1. Çalışan uygulama örneğini kaldırın (veya silin.
2. Artık ihtiyacınız yoksa uygulama türünü kayıt dışı edin.
3. Uygulama paketini görüntü deposundan kaldırın.

Visual Studio'yu yerel geliştirme kümenizde uygulamaları dağıtmak ve hata ayıklamak için kullanıyorsanız, önceki tüm adımlar bir PowerShell komut dosyası aracılığıyla otomatik olarak işlenir.  Bu komut dosyası, uygulama projesinin *Komut Dosyaları* klasöründe bulunur. Bu makalede, Visual Studio dışında aynı işlemleri gerçekleştirebilirsiniz böylece bu komut dosyası ne yaptığını arka plan sağlar. 

Bir uygulamayı dağıtmanın başka bir yolu da dış hükmü kullanmaktır. Uygulama paketi [ `sfpkg` olarak paketlenebilir](service-fabric-package-apps.md#create-an-sfpkg) ve harici bir mağazaya yüklenebilir. Bu durumda, resim deposuna yükleme gerekli değildir. Dağıtım aşağıdaki adımlara ihtiyaç duyar:

1. Harici `sfpkg` bir mağazaya yükleyin. Harici depo, REST http veya https bitiş noktasını ortaya çıkaran herhangi bir mağaza olabilir.
2. Harici indirme URI ve uygulama türü bilgilerini kullanarak uygulama türünü kaydedin.
2. Uygulama örneğini oluşturun.

Temizleme için uygulama örneklerini kaldırın ve uygulama türünü kaldırın. Paket görüntü deposuna kopyalanmadığından, temizleyecek geçici bir konum yoktur. Harici mağazadan tedarik Service Fabric sürüm 6.1'den başlayarak kullanılabilir.

>[!NOTE]
> Visual Studio şu anda dış hükmü desteklemiyor.

 

## <a name="connect-to-the-cluster"></a>Kümeye bağlanma

Bu makalede powershell komutlarını çalıştırmadan önce, Service Fabric kümesine bağlanmak için her zaman [Connect-ServiceFabricCluster'ı](/powershell/module/servicefabric/connect-servicefabriccluster?view=azureservicefabricps) kullanmaya başlayın. Yerel geliştirme kümesine bağlanmak için aşağıdakileri çalıştırın:

```powershell
Connect-ServiceFabricCluster
```

Azure Active Directory, X509 sertifikaları veya Windows Active Directory kullanılarak güvenli leştirilmiş uzak bir kümeye veya [kümeye](service-fabric-connect-to-secure-cluster.md)bağlanma örnekleri için bkz.

## <a name="upload-the-application-package"></a>Uygulama paketini yükleyin

Uygulama paketinin yüklenmesi, paketi dahili Service Fabric bileşenleri tarafından erişilebilen bir konuma koyar.
Başvuru paketini yerel olarak doğrulamak istiyorsanız, [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) cmdlet'i kullanın.

[Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) komutu uygulama paketini küme görüntü deposuna yükler.

Visual Studio 2015'te *MyApplication* adında bir uygulama oluşturduğunuzu ve paketlediğinizi varsayalım. Varsayılan olarak, ApplicationManifest.xml listelenen uygulama türü adı "MyApplicationType" dır.  Gerekli uygulama bildirimini, hizmet bildirimlerini ve kod/config/veri paketlerini içeren uygulama paketi *C:\Users\<kullanıcı adı\>\Belgeler\Visual Studio 2015\Projeler\MyApplication\MyApplication\pkg\Hata Ayıklama*. 

Aşağıdaki komut, uygulama paketinin içeriğini listeler:

```powershell
$path = 'C:\Users\<user\>\Documents\Visual Studio 2015\Projects\MyApplication\MyApplication\pkg\Debug'
tree /f $path
```

```Output
Folder PATH listing for volume OSDisk
Volume serial number is 0459-2393
C:\USERS\USER\DOCUMENTS\VISUAL STUDIO 2015\PROJECTS\MYAPPLICATION\MYAPPLICATION\PKG\DEBUG
│   ApplicationManifest.xml
│
└───Stateless1Pkg
    │   ServiceManifest.xml
    │
    ├───Code
    │       Microsoft.ServiceFabric.Data.dll
    │       Microsoft.ServiceFabric.Data.Interfaces.dll
    │       Microsoft.ServiceFabric.Internal.dll
    │       Microsoft.ServiceFabric.Internal.Strings.dll
    │       Microsoft.ServiceFabric.Services.dll
    │       ServiceFabricServiceModel.dll
    │       Stateless1.exe
    │       Stateless1.exe.config
    │       Stateless1.pdb
    │       System.Fabric.dll
    │       System.Fabric.Strings.dll
    │
    └───Config
            Settings.xml
```

Uygulama paketi büyükse ve/veya çok sayıda dosyavarsa, [sıkıştırabilirsiniz.](service-fabric-package-apps.md#compress-a-package) Sıkıştırma, dosyaların boyutunu ve sayısını azaltır.
Bunun yan etkisi, uygulama türünü kaydetmenin ve geri almanın daha hızlı olmasıdır. Yükleme süresi şu anda daha yavaş olabilir, özellikle paketi sıkıştırma kattığınızda. 

Bir paketi sıkıştırmak için aynı [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) komutunu kullanın. Sıkıştırma, yüklemeden ayrı olarak, `SkipCopy` bayrağı kullanarak veya yükleme işlemiyle birlikte yapılabilir. Sıkıştırılmış bir pakete sıkıştırma uygulamak op değildir.
Sıkıştırılmış bir paketi açmak için, anahtarla aynı [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) komutunu `UncompressPackage` kullanın.

Aşağıdaki cmdlet, paketi görüntü deposuna kopyalamadan sıkıştırır. Paket şimdi `Code` ve `Config` paketler için sıkıştırılmış dosyaları içerir. Birçok dahili işlem (paket paylaşımı, uygulama türü adı ve belirli doğrulamalar için sürüm çıkarma gibi) için gerekli olduğundan, uygulama ve hizmet bildirimleri sıkıştırılmadı. Manifestoları zipping bu işlemleri verimsiz hale getirecek.

```powershell
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -CompressPackage -SkipCopy
tree /f $path
```

```Output
Folder PATH listing for volume OSDisk
Volume serial number is 0459-2393
C:\USERS\USER\DOCUMENTS\VISUAL STUDIO 2015\PROJECTS\MYAPPLICATION\MYAPPLICATION\PKG\DEBUG
|   ApplicationManifest.xml
|
└───Stateless1Pkg
       Code.zip
       Config.zip
       ServiceManifest.xml
```

Büyük uygulama paketleri için sıkıştırma zaman alır. En iyi sonuçları elde etmek için hızlı bir SSD sürücüsü kullanın. Sıkıştırma süreleri ve sıkıştırılmış paketin boyutu da paket içeriğine göre farklılık gösterir.
Örneğin, sıkıştırma süresiyle birlikte ilk ve sıkıştırılmış paket boyutunu gösteren bazı paketler için sıkıştırma istatistikleri aşağıda verilmiştir.

|Başlangıç boyutu (MB)|Dosya sayısı|Sıkıştırma Süresi|Sıkıştırılmış paket boyutu (MB)|
|----------------:|---------:|---------------:|---------------------------:|
|100|100|00:00:03.3547592|60|
|512|100|00:00:16.3850303|307|
|1024|500|00:00:32.5907950|615|
|2048|1000|00:01:04.3775554|1231|
|5012|100|00:02:45.2951288|3074|

Bir paket sıkıştırıldıktan sonra, gerektiğinde bir veya birden çok Service Fabric kümelerine yüklenebilir. Dağıtım mekanizması sıkıştırılmış ve sıkıştırılmamış paketler için aynıdır. Sıkıştırılmış paketler küme görüntü deposunda bu şekilde depolanır. Paketler, uygulama çalıştırılmadan önce düğümüzerinde sıkıştırılmamıştır.


Aşağıdaki örnek, paketi görüntü deposuna, "MyApplicationV1" adlı bir klasöre yükler:

```powershell
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -ApplicationPackagePathInImageStore MyApplicationV1 -TimeoutSec 1800
```

*-ApplicationPackagePathInImageStore* parametresini belirtmezseniz, uygulama paketi görüntü deposundaki "Hata Ayıklama" klasörüne kopyalanır.

>[!NOTE]
>**Copy-ServiceFabricApplicationPackage,** PowerShell oturumu bir Service Fabric kümesine bağlıysa, uygun görüntü deposu bağlantı dizesini otomatik olarak algılar. 5.6'dan eski Hizmet Kumaşı sürümleriiçin **-ImageStoreConnectionString** bağımsız değişkeni açıkça sağlanmalıdır.
>
>```powershell
>PS C:\> Copy-ServiceFabricApplicationPackage -ApplicationPackagePath $path -ApplicationPackagePathInImageStore MyApplicationV1 -ImageStoreConnectionString (Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)) -TimeoutSec 1800
>```
>
>Service Fabric SDK PowerShell modülünün bir parçası olan **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, görüntü deposu bağlantı dizesini almak için kullanılır.  SDK modüllerini almak için çalıştırın:
>
>```powershell
>Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
>```
>
>Bkz. Görüntü deposu ve görüntü deposu bağlantı dizesi hakkında ek bilgiler için [görüntü deposu bağlantı dizesini anlayın.](service-fabric-image-store-connection-string.md)
>
>
>

Bir paketi yüklemek için gereken süre, birden çok etkene bağlı olarak değişir. Bu etkenlerden bazıları paketteki dosya sayısı, paket boyutu ve dosya boyutlarıdır. Kaynak makine ile Service Fabric kümesi arasındaki ağ hızı da yükleme süresini etkiler. [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) için varsayılan zaman dilimi 30 dakikadır.
Açıklanan etkenlere bağlı olarak, zaman anına kadar artırmanız gerekebilir. Paketi kopyalama çağrısında sıkıştırıyorsanız, sıkıştırma süresini de göz önünde bulundurmanız gerekir.



## <a name="register-the-application-package"></a>Başvuru paketini kaydedin

Uygulama bildiriminde beyan edilen uygulama türü ve sürümü, başvuru paketi kaydedildiğinde kullanıma hazır hale gelir. Sistem, önceki adımda yüklenen paketi okur, paketi doğrular, paket içeriğini işler ve işlenen paketi dahili bir sistem konumuna kopyalar.  

Uygulama türünü kümeye kaydetmek ve dağıtımiçin kullanılabilir hale getirmek için [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) cmdlet'i çalıştırın:

### <a name="register-the-application-package-copied-to-image-store"></a>Görüntü deposuna kopyalanan başvuru paketini kaydetme

Bir paket daha önce görüntü deposuna kopyalandığında, kayıt işlemi görüntü deposundaki göreli yolu belirtir.

```powershell
Register-ServiceFabricApplicationType -ApplicationPathInImageStore MyApplicationV1
```

```Output
Register application type succeeded
```

"MyApplicationV1", uygulama paketinin bulunduğu resim deposundaki klasördür. "MyApplicationType" adı ve "1.0.0" (her ikisi de uygulama bildiriminde bulunur) sürümü olan uygulama türü artık kümeye kaydedilir.

### <a name="register-the-application-package-copied-to-an-external-store"></a>Harici bir mağazaya kopyalanan başvuru paketini kaydetme

Service Fabric sürüm 6.1'den başlayarak, hüküm paketin harici bir mağazadan indirilmelerini destekler. İndirme URI, [ `sfpkg` uygulama paketinin](service-fabric-package-apps.md#create-an-sfpkg) HTTP veya HTTPS protokolleri kullanılarak indirilebileceği uygulama paketine giden yolu temsil eder. Paket daha önce bu dış konuma yüklenmiş olmalıdır. Hizmet Kumaşı'nın dosyayı indirebilmesi için URI READ erişimine izin vermelidir. Dosya `sfpkg` ".sfpkg" uzantısı olmalıdır. Provizyon işlemi, uygulama bildiriminde bulunduğu gibi uygulama türü bilgilerini içermelidir.

```powershell
Register-ServiceFabricApplicationType -ApplicationPackageDownloadUri "https://sftestresources.blob.core.windows.net:443/sfpkgholder/MyAppPackage.sfpkg" -ApplicationTypeName MyApp -ApplicationTypeVersion V1 -Async
```

[Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) komutu, ancak sistem başvuru paketini başarıyla kaydettikten sonra geri döner. Kaydın ne kadar süreceği, uygulama paketinin boyutuna ve içeriğine bağlıdır. Gerekirse, **-TimeoutSec** parametresi daha uzun bir zaman aşımı sağlamak için kullanılabilir (varsayılan zaman aşımı 60 saniyedir).

Büyük bir uygulama paketiniz varsa veya zaman ayarı yaşıyorsanız- **Async** parametresini kullanın. Küme kayıt komutunu kabul ettiğinde komut geri döner. Kayıt işlemi gerektiği gibi devam ediyor.
[Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) komutu, uygulama türü sürümlerini ve bunların kayıt durumlarını listeler. Bu komutu, kaydın ne zaman yapılacağını belirlemek için kullanabilirsiniz.

```powershell
Get-ServiceFabricApplicationType
```

```Output
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="remove-an-application-package-from-the-image-store"></a>Uygulama paketini görüntü deposundan kaldırma

Bir paket görüntü deposuna kopyalanmışsa, uygulama başarıyla kaydedildikten sonra paketi geçici konumdan kaldırmanız gerekir. Uygulama paketlerini görüntü deposundan silerse sistem kaynaklarını boşaltAr. Kullanılmayan uygulama paketlerinin tutulması disk depolamayı tüketir ve uygulama performansı sorunlarına yol açar.

```powershell
Remove-ServiceFabricApplicationPackage -ApplicationPackagePathInImageStore MyApplicationV1
```

## <a name="create-the-application"></a>Uygulama oluşturma

[Yeni ServisKumaş Uygulama](/powershell/module/servicefabric/new-servicefabricapplication?view=azureservicefabricps) cmdlet'ini kullanarak başarıyla kaydedilmiş herhangi bir uygulama türü sürümünden bir uygulamayı anında alabilirsiniz. Her uygulamanın adı *"kumaş:"* şemasıyla başlamalı ve her uygulama örneği için benzersiz olmalıdır. Hedef uygulama türünün uygulama bildiriminde tanımlanan varsayılan hizmetler de oluşturulur.

```powershell
New-ServiceFabricApplication fabric:/MyApp MyApplicationType 1.0.0
```

```Output
ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
ApplicationParameters  : {}
```

Kayıtlı bir uygulama türünün belirli bir sürümü için birden çok uygulama örneği oluşturulabilir. Her uygulama örneği, kendi çalışma dizini ve süreci ile, izole çalışır.

Kümede hangi adlandırılmış uygulama ve hizmetlerin çalıştığını görmek için [Get-ServiceFabricApplication ve Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricapplication) cmdlets'i çalıştırın: [Get-ServiceFabricService](/powershell/module/servicefabric/get-servicefabricservice?view=azureservicefabricps)

```powershell
Get-ServiceFabricApplication  
```

```Output
ApplicationName        : fabric:/MyApp
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
ApplicationStatus      : Ready
HealthState            : Ok
ApplicationParameters  : {}
```

```powershell
Get-ServiceFabricApplication | Get-ServiceFabricService
```

```Output
ServiceName            : fabric:/MyApp/Stateless1
ServiceKind            : Stateless
ServiceTypeName        : Stateless1Type
IsServiceGroup         : False
ServiceManifestVersion : 1.0.0
ServiceStatus          : Active
HealthState            : Ok
```

## <a name="remove-an-application"></a>Uygulamayı kaldırma

Bir uygulama örneğine artık gerek kalmadığında, [Remove-ServiceFabricApplication](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) cmdlet'i kullanarak kalıcı olarak adıyla kaldırabilirsiniz. [Remove-ServiceFabricApplication,](/powershell/module/servicefabric/remove-servicefabricapplication?view=azureservicefabricps) uygulamaya ait tüm hizmetleri de otomatik olarak kaldırır ve tüm hizmet durumunu kalıcı olarak kaldırır. 

> [!WARNING]
> Bu işlem geri alınamaz ve uygulama durumu kurtarılamaz.

```powershell
Remove-ServiceFabricApplication fabric:/MyApp
```

```Output
Confirm
Continue with this operation?
[Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"):
Remove application instance succeeded
```

```powershell
Get-ServiceFabricApplication
```

## <a name="unregister-an-application-type"></a>Uygulama türünü kayıt dışı

Bir uygulama türünün belirli bir sürümüne artık gerek yoksa, [Kayıt Dışı-HizmetKumaşUygulamaTürü](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) cmdlet'ini kullanarak uygulama türünü silmeniz gerekir. Kullanılmayan uygulama türlerinin kaydedilmesi, uygulama türü dosyalarını kaldırarak görüntü deposu tarafından kullanılan depolama alanını serbest bırakır. Bir uygulama türünü kayıt dışı kaldırmak, görüntü deposuna kopya kullanılıyorsa, görüntü deposuna kopyalanan uygulama paketini geçici konuma kaldırmaz. Hiçbir uygulama karşı anında ve bekleyen uygulama yükseltmeleri başvurulan sürece bir uygulama türü kayıtsız olabilir.

Kümede şu anda kayıtlı olan uygulama türlerini görmek için [Get-ServiceFabricApplicationType'ı](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) çalıştırın:

```powershell
Get-ServiceFabricApplicationType
```

```Output
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

Kayıt [Dışı-HizmetKumaşUygulama Türü](/powershell/module/servicefabric/unregister-servicefabricapplicationtype?view=azureservicefabricps) belirli bir uygulama türü kayıt dışı çalıştırın:

```powershell
Unregister-ServiceFabricApplicationType MyApplicationType 1.0.0
```

## <a name="troubleshooting"></a>Sorun giderme

### <a name="copy-servicefabricapplicationpackage-asks-for-an-imagestoreconnectionstring"></a>Copy-ServiceFabricApplicationPackage bir ImageStoreConnectionString ister

Hizmet Kumaşı SDK ortamı zaten doğru varsayılanları ayarlamış olmalıdır. Ancak gerekirse, tüm komutlar için ImageStoreConnectionString, Service Fabric kümesinin kullandığı değerle eşleşmelidir. ImageStoreConnectionString'i küme manifestosunda, [Get-ServiceFabricClusterManifest](/powershell/module/servicefabric/get-servicefabricclustermanifest?view=azureservicefabricps) ve Get-ImageStoreStringFromClusterManifest komutları kullanılarak alınan aşağıdakileri bulabilirsiniz:

```powershell
Get-ImageStoreConnectionStringFromClusterManifest(Get-ServiceFabricClusterManifest)
```

Service Fabric SDK PowerShell modülünün bir parçası olan **Get-ImageStoreConnectionStringFromClusterManifest** cmdlet, görüntü deposu bağlantı dizesini almak için kullanılır.  SDK modüllerini almak için çalıştırın:

```powershell
Import-Module "$ENV:ProgramFiles\Microsoft SDKs\Service Fabric\Tools\PSModule\ServiceFabricSDK\ServiceFabricSDK.psm1"
```

ImageStoreConnectionString küme manifestosunda bulunur:

```xml
<ClusterManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Name="Server-Default-SingleNode" Version="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">

    [...]

    <Section Name="Management">
      <Parameter Name="ImageStoreConnectionString" Value="file:D:\ServiceFabric\Data\ImageStore" />
    </Section>

    [...]
```

Bkz. Görüntü deposu ve görüntü deposu bağlantı dizesi hakkında ek bilgiler için [görüntü deposu bağlantı dizesini anlayın.](service-fabric-image-store-connection-string.md)

### <a name="deploy-large-application-package"></a>Büyük uygulama paketini dağıtma

Sorun: [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) büyük bir uygulama paketi (GB sırası) için zaman dışarı.
Deneyin:
- [Parametreli Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) komutu için `TimeoutSec` daha büyük bir zaman arası belirtin. Varsayılan olarak, zaman ası 30 dakikadır.
- Kaynak makineniz ve kümeniz arasındaki ağ bağlantısını denetleyin. Bağlantı yavaşsa, daha iyi ağ bağlantısına sahip bir makine kullanmayı düşünün.
İstemci makine kümeden başka bir bölgedeyse, istemci makinesini kümeyle daha yakın veya aynı bölgede kullanmayı düşünün.
- Harici azaltma isabet olup olmadığını kontrol edin. Örneğin, görüntü deposu azure depolamayı kullanacak şekilde yapılandırıldığında, yükleme azaltılabilir.

Sorun: Yükleme paketi başarıyla tamamlandı, ancak [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) kez dışarı. Deneyin:
- Resim [deposuna kopyalamadan](service-fabric-package-apps.md#compress-a-package) önce paketi sıkıştırın.
Sıkıştırma, dosya boyutunu ve sayısını azaltır ve bu da Hizmet Kumaşı'nın gerçekleştirmesi gereken trafik ve çalışma miktarını azaltır. Yükleme işlemi daha yavaş olabilir (özellikle sıkıştırma süresini de eklerseniz), ancak uygulama türünü kaydedip kaydını kaldırsanız daha hızlıdır.
- Parametreli [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) için `TimeoutSec` daha büyük bir zaman arası belirtin.
- `Async` [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps)için anahtarı belirtin. Küme komutu kabul ettiğinde komut döner ve başvuru türünün kaydı eş senkronize olarak devam eder. Bu nedenle, bu durumda daha yüksek bir zaman anına belirtmeye gerek yoktur. [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) komutu tüm başarıyla kaydedilmiş başvuru türü sürümlerini ve bunların kayıt durumlarını listeler. Bu komutu, kaydın ne zaman yapılacağını belirlemek için kullanabilirsiniz.

```powershell
Get-ServiceFabricApplicationType
```

```Output
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

### <a name="deploy-application-package-with-many-files"></a>Birçok dosyaiçeren uygulama paketini dağıtma

Sorun: [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) birçok dosya (binlerce sipariş) ile bir uygulama paketi için kez dışarı.
Deneyin:
- Resim [deposuna kopyalamadan](service-fabric-package-apps.md#compress-a-package) önce paketi sıkıştırın. Sıkıştırma dosya sayısını azaltır.
- Parametreli [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps) için `TimeoutSec` daha büyük bir zaman arası belirtin.
- `Async` [Register-ServiceFabricApplicationType](/powershell/module/servicefabric/register-servicefabricapplicationtype?view=azureservicefabricps)için anahtarı belirtin. Küme komutu kabul ettiğinde komut döner ve başvuru türünün kaydı eş senkronize olarak devam eder.
Bu nedenle, bu durumda daha yüksek bir zaman anına belirtmeye gerek yoktur. [Get-ServiceFabricApplicationType](/powershell/module/servicefabric/get-servicefabricapplicationtype?view=azureservicefabricps) komutu tüm başarıyla kaydedilmiş başvuru türü sürümlerini ve bunların kayıt durumlarını listeler. Bu komutu, kaydın ne zaman yapılacağını belirlemek için kullanabilirsiniz.

```powershell
Get-ServiceFabricApplicationType
```

```Output
ApplicationTypeName    : MyApplicationType
ApplicationTypeVersion : 1.0.0
Status                 : Available
DefaultParameters      : { "Stateless1_InstanceCount" = "-1" }
```

## <a name="next-steps"></a>Sonraki adımlar

[Uygulamaları paketleme](service-fabric-package-apps.md)

[Hizmet Kumaş uygulama yükseltme](service-fabric-application-upgrade.md)

[Hizmet Kumaş sağlık giriş](service-fabric-health-introduction.md)

[Bir Service Fabric hizmetini tanılama ve sorun giderme](service-fabric-diagnostics-how-to-monitor-and-diagnose-services-locally.md)

[Hizmet Kumaşı'nda bir uygulama modeli](service-fabric-application-model.md)

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-package-apps.md
[11]: service-fabric-application-upgrade.md