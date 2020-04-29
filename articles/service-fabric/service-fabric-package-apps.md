---
title: Azure Service Fabric uygulaması paketleme
description: Azure Service Fabric uygulaması paketleme ve bir kümeye dağıtım için hazırlanma hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 2/23/2018
ms.openlocfilehash: 7c99eec28ac06ecf666d6dda1015f889841a5dbf
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79258349"
---
# <a name="package-an-application"></a>Uygulamaları paketleme

Bu makalede bir Service Fabric uygulamasının nasıl paketleneceğini ve dağıtıma nasıl hazırlanılacağını açıklamaktadır.

## <a name="package-layout"></a>Paket düzeni

Uygulama bildirimi, bir veya daha fazla hizmet bildirimi ve diğer gerekli paket dosyaları Service Fabric bir kümeye dağıtım için belirli bir düzen halinde düzenlenmelidir. Bu makaledeki örnek bildirimlerin aşağıdaki dizin yapısında organize olması gerekir:

```
tree /f .\MyApplicationType
```

```Output
D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
```

Klasörler, karşılık gelen her öğenin **ad** öznitelikleriyle eşleşecek şekilde adlandırılır. Örneğin, hizmet bildirimi **Mycodea** ve **mycodeb**adlarıyla iki kod paketi içeriyorsa, aynı ada sahip iki klasör her kod paketi için gereken ikili dosyaları içerir.

## <a name="use-setupentrypoint"></a>SetupEntryPoint kullanma

**Setupentrypoint** kullanmanın tipik senaryoları, hizmet başlamadan önce bir yürütülebilir dosya çalıştırmanız veya yükseltilmiş ayrıcalıklarla bir işlem gerçekleştirmeniz gerekir. Örneğin:

* Hizmet yürütülebilir dosyasının ihtiyaç duyacağı ortam değişkenlerini ayarlama ve başlatma. Yalnızca Service Fabric programlama modelleriyle yazılmış yürütülebilir dosyalar ile sınırlı değildir. Örneğin, NPM. exe ' nin bir Node. js uygulamasını dağıtmak için yapılandırılmış bazı ortam değişkenlerine ihtiyacı vardır.
* Güvenlik sertifikaları yükleyerek erişim denetimini ayarlama.

**Setupentrypoint**yapılandırma hakkında daha fazla bilgi için bkz [. bir hizmet kurulumu giriş noktası için ilkeyi yapılandırma](service-fabric-application-runas-security.md)

<a id="Package-App"></a>

## <a name="configure"></a>Yapılandırma

### <a name="build-a-package-by-using-visual-studio"></a>Visual Studio 'Yu kullanarak paket oluşturma

Uygulamanızı oluşturmak için Visual Studio kullandıysanız, yukarıda açıklanan düzen ile eşleşen bir paket otomatik olarak oluşturmak için *Package* komutunu kullanabilirsiniz.

Bir paket oluşturmak için *Çözüm Gezgini* ' de uygulama projesine sağ tıklayın ve **paket** komutunu seçin:

![Visual Studio ile uygulama paketleme][vs-package-command]

Paketleme tamamlandığında, paketin konumunu **Çıkış** penceresinde bulabilirsiniz. Visual Studio 'da uygulamanızı dağıtırken veya hata ayıkladığınızda paketleme adımı otomatik olarak gerçekleşir.

### <a name="build-a-package-by-command-line"></a>Komut satırı ile paket oluşturma

Kullanarak `msbuild.exe`uygulamanızı programlı bir şekilde paketlemek da mümkündür. Aynı şekilde, Visual Studio bunu çalıştırırken çıktının aynı olması gerekir.

```shell
D:\Temp> msbuild HelloWorld.sfproj /t:Package
```

## <a name="test-the-package"></a>Paketi test etme

[Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) komutunu kullanarak, PowerShell aracılığıyla paket yapısını yerel olarak doğrulayabilirsiniz.
Bu komut, bildirim ayrıştırma sorunlarını denetler ve tüm başvuruları doğrular. Bu komut yalnızca paketteki dizinlerin ve dosyaların yapısal doğruluğunu doğrular.
Tüm gerekli dosyaların mevcut olup olmadığını kontrol eden hiçbir kod veya veri paketi içeriğini doğrulamaz.

```powershell
Test-ServiceFabricApplicationPackage .\MyApplicationType
```

```Output
False
Test-ServiceFabricApplicationPackage : The EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
```

Bu hata, kod paketinde **Setupentrypoint** hizmet bildiriminde başvurulan *MySetup. bat* dosyasının eksik olduğunu gösterir. Eksik dosya eklendikten sonra, uygulama doğrulaması geçirilir:

```
tree /f .\MyApplicationType
```

```Output
D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
```

```powershell
Test-ServiceFabricApplicationPackage .\MyApplicationType
```

```Output
True
```

Uygulamanızda tanımlanmış [uygulama parametreleri](service-fabric-manage-multiple-environment-app-configuration.md) varsa, doğru doğrulama Için bunları [Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) içinde geçirebilirsiniz.

Uygulamanın dağıtılacağı kümeyi biliyorsanız, `ImageStoreConnectionString` parametresinde geçiş yapmanız önerilir. Bu durumda, paket, zaten kümede çalışmakta olan uygulamanın önceki sürümleriyle da onaylanır. Örneğin, doğrulama aynı sürüme sahip ancak farklı içeriğe sahip bir paketin zaten dağıtılıp dağıtılmadığını algılayabilir.  

Uygulama doğru şekilde paketlendikten ve doğrulamayı geçtiğinde, daha hızlı dağıtım işlemleri için paketi sıkıştırmayı göz önünde bulundurun.

## <a name="compress-a-package"></a>Bir paketi sıkıştır

Bir paket büyükse veya çok sayıda dosya olduğunda daha hızlı dağıtım için sıkıştırabilmeniz gerekir. Sıkıştırma, dosya sayısını ve paket boyutunu azaltır.
Sıkıştırılmış bir uygulama paketi için, özellikle de bir kopyanın parçası olarak sıkıştırma yapıldığında, [uygulama paketini karşıya yüklemek](service-fabric-deploy-remove-applications.md#upload-the-application-package) sıkıştırılmamış paketi karşıya yüklemeye kıyasla daha uzun sürebilir. Sıkıştırma ile uygulama türünü [kaydetme](service-fabric-deploy-remove-applications.md#register-the-application-package) ve [kaydını kaldırma](service-fabric-deploy-remove-applications.md#unregister-an-application-type) daha hızlıdır.

Dağıtım mekanizması, sıkıştırılmış ve sıkıştırılmamış paketler için aynıdır. Paket sıkıştırılmışsa, küme görüntü deposunda olduğu gibi depolanır ve uygulama çalıştırılmadan önce düğüm üzerinde sıkıştırılmamış olur.
Sıkıştırma geçerli Service Fabric paketinin sıkıştırılmış sürümle yerini almıştır. Klasör yazma izinlerine izin vermelidir. Zaten sıkıştırılmış bir pakette sıkıştırma çalıştırmak hiçbir değişiklik vermez.

Anahtar ile `CompressPackage` [Copy-Servicefabricapplicationpackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) PowerShell komutunu çalıştırarak bir paketi sıkıştırabilirsiniz. Anahtarı kullanarak `UncompressPackage` paketin sıkıştırmasını aynı komutla açabilirsiniz.

Aşağıdaki komut, paketi görüntü deposuna kopyalamadan sıkıştırır. Bir veya daha fazla Service Fabric kümesine, `SkipCopy` bayrak olmadan [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) kullanarak, bir veya daha fazla kümeye kopyalayabilirsiniz.
Paket artık `code`, `config`, ve `data` paketleri için daraltılmış dosyalar içeriyor. Birçok iç işlem için gerekli olduklarından, uygulama bildirimi ve hizmet bildirimleri sıkıştırıldı. Örneğin, belirli doğrulamaları için paket paylaşımı, uygulama türü adı ve sürüm ayıklama tüm bildirimlere erişmesi gerekir. Bildirimleri sıkıştırma işlemi bu işlemleri verimsiz hale getirir.

```
tree /f .\MyApplicationType
```

```Output
D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
    │   ServiceManifest.xml
    │
    ├───MyCode
    │       MyServiceHost.exe
    │       MySetup.bat
    │
    ├───MyConfig
    │       Settings.xml
    │
    └───MyData
            init.dat
```

```powershell
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -CompressPackage -SkipCopy
tree /f .\MyApplicationType
```

```Output
D:\TEMP\MYAPPLICATIONTYPE
│   ApplicationManifest.xml
│
└───MyServiceManifest
       ServiceManifest.xml
       MyCode.zip
       MyConfig.zip
       MyData.zip

```

Alternatif olarak, [Copy-ServiceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) ile paketi tek bir adımda sıkıştırabilir ve kopyalayabilirsiniz.
Paket büyükse, hem Paket sıkıştırması hem de kümeye yükleme için zamana izin vermek üzere yeterince yüksek bir zaman aşımı sağlayın.

```powershell
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -ApplicationPackagePathInImageStore MyApplicationType -ImageStoreConnectionString fabric:ImageStore -CompressPackage -TimeoutSec 5400
```

Dahili olarak, Service Fabric doğrulama için uygulama paketleri için sağlama toplamlarını hesaplar. Sıkıştırma kullanılırken, sağlama toplamı her bir paketin daraltılmış sürümlerinde hesaplanır. Aynı uygulama paketinden yeni bir ZIP oluşturulması farklı sağlama toplamı oluşturur. Doğrulama hatalarını engellemek için, [fark sağlamayı](service-fabric-application-upgrade-advanced.md)kullanın. Bu seçenekle, değiştirilmemiş paketleri yeni sürüme eklemeyin. Bunun yerine, doğrudan yeni hizmet bildiriminden başvuru yapın.

Fark sağlama bir seçenek değilse ve paketleri dahil etmeniz gerekiyorsa, sağlama toplamı uyuşmazlığından kaçınmak için `code`, `config`, ve `data` paketleri için yeni sürümler oluşturun. Önceki sürümün sıkıştırma kullanıp kullanmadığını bağımsız olarak, sıkıştırılmış bir paket kullanıldığında, değiştirilmemiş paketler için yeni sürümler oluşturulması gerekir.

Paket artık doğru bir şekilde paketlendi, doğruladı ve sıkıştırılmış (gerekliyse), bu nedenle bir veya daha fazla Service Fabric kümesine [dağıtım](service-fabric-deploy-remove-applications.md) için hazırdır.

### <a name="compress-packages-when-deploying-using-visual-studio"></a>Visual Studio kullanarak dağıtım yaparken paketleri sıkıştır

Yayımlama profilinize `CopyPackageParameters` öğe ekleyerek ve `CompressPackage` özniteliğini olarak `true`ayarlayarak, Visual Studio 'yu dağıtımdaki paketleri sıkıştırmak üzere söyleyebilirsiniz.

``` xml
    <PublishProfile xmlns="http://schemas.microsoft.com/2015/05/fabrictools">
        <ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com" />
        <ApplicationParameterFile Path="..\ApplicationParameters\Cloud.xml" />
        <CopyPackageParameters CompressPackage="true"/>
    </PublishProfile>
```

## <a name="create-an-sfpkg"></a>Bir sfpkg oluşturma

Sürüm 6,1 ' den başlayarak, Service Fabric bir dış depodan sağlama yapılmasına izin verir.
Bu seçenekle, uygulama paketinin görüntü deposuna kopyalanması gerekmez. Bunun yerine, oluşturup bir `sfpkg` dış depoya yükleyebilir ve sonra sağlama sırasında Service Fabric IÇIN indirme URI 'sini sağlayabilirsiniz. Aynı paket birden çok kümeye sağlanabilir. Dış depodan sağlama, paketi her kümeye kopyalamak için gereken süreyi kaydeder.

`sfpkg` Dosya, ilk uygulama paketini içeren ve ". sfpkg" uzantısına sahip bir zip dosyasıdır.
ZIP içinde, uygulama paketi sıkıştırılabilir veya sıkıştırılmamış olabilir. ZIP içindeki uygulama paketinin sıkıştırması, [daha önce belirtildiği](service-fabric-package-apps.md#compress-a-package)gibi kod, yapılandırma ve veri paketi düzeylerinde yapılır.

Oluşturmak için `sfpkg`, sıkıştırılmış veya olmayan özgün uygulama paketini içeren bir klasör ile başlayın. Sonra, ". sfpkg" uzantısına sahip klasörü ZIP için herhangi bir yardımcı programı kullanın. Örneğin, [ZipFile. CreateFromDirectory](https://msdn.microsoft.com/library/hh485721(v=vs.110).aspx)kullanın.

```csharp
ZipFile.CreateFromDirectory(appPackageDirectoryPath, sfpkgFilePath);
```

`sfpkg` Service Fabric dışında, bant dışı dış depoya yüklenmelidir. Dış depo, REST http veya HTTPS uç noktası sunan herhangi bir mağaza olabilir. Sağlama sırasında, Service Fabric `sfpkg` uygulama paketini indirmek IÇIN bir get işlemi yürütür, bu nedenle mağaza paket için okuma erişimine izin vermelidir.

Paketi sağlamak için, indirme URI 'SI ve uygulama türü bilgilerini gerektiren harici sağlama kullanın.

>[!NOTE]
> Görüntü deposu göreli yolu temelinde sağlama Şu anda dosyaları desteklemiyor `sfpkg` . `sfpkg` Bu nedenle, görüntü deposuna kopyalanmamalıdır.

## <a name="next-steps"></a>Sonraki adımlar

[Uygulamaları dağıtma ve kaldırma][10] uygulama örneklerini yönetmek için PowerShell 'in nasıl kullanılacağını açıklar

[Birden çok ortamda uygulama parametrelerinin yönetilmesi][11] , farklı uygulama örnekleri için parametrelerin ve ortam değişkenlerinin nasıl yapılandırılacağını açıklar.

[Uygulamanız için güvenlik Ilkelerini yapılandırma][12] erişimi kısıtlamak için güvenlik ilkeleri altında hizmetlerin nasıl çalıştırılacağını açıklar.

<!--Image references-->
[vs-package-command]: ./media/service-fabric-package-apps/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md