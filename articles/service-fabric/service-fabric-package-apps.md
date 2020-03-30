---
title: Azure Hizmet Kumaşı uygulamasını paketle
description: Azure Hizmet Kumaşı uygulamasını paketleme ve kümeye dağıtıma nasıl hazırlanacağı hakkında bilgi edinin.
ms.topic: conceptual
ms.date: 2/23/2018
ms.openlocfilehash: 7c99eec28ac06ecf666d6dda1015f889841a5dbf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79258349"
---
# <a name="package-an-application"></a>Uygulamaları paketleme

Bu makalede, Hizmet Kumaşı uygulamasının nasıl paketlenebildiğini ve dağıtıma nasıl hazır hale getirilebildiğini açıklanmaktadır.

## <a name="package-layout"></a>Paket düzeni

Uygulama bildirimi, bir veya daha fazla hizmet bildirimi ve diğer gerekli paket dosyaları, Hizmet Kumaşı kümesine dağıtım için belirli bir düzende düzenlenmelidir. Bu makaledeki örnek bildirimlerin aşağıdaki dizin yapısında düzenlenmesi gerekir:

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

Klasörler, karşılık gelen her öğenin **Ad** öznitelikleriyle eşleşecek şekilde adlandırılır. Örneğin, hizmet **bildirimimyCodeA** ve **MyCodeB**adlarını içeren iki kod paketi içeriyorsa, aynı adlara sahip iki klasör her kod paketi için gerekli ikili dosyaları içerir.

## <a name="use-setupentrypoint"></a>SetupEntryPoint'i kullan

**SetupEntryPoint'i** kullanmak için tipik senaryolar, hizmet başlamadan önce çalıştırılabilir bir çalıştırma yapmanız veya yüksek ayrıcalıklara sahip bir işlem gerçekleştirmeniz gerektiği zamandır. Örnek:

* Hizmetin çalıştırılması gereken ortam değişkenlerini ayarlama ve başlatma. Yalnızca Service Fabric programlama modelleri aracılığıyla yazılmış yürütülebilir uygulamalarla sınırlı değildir. Örneğin, npm.exe bir düğüm.js uygulaması dağıtmak için yapılandırılan bazı ortam değişkenleri gerekir.
* Güvenlik sertifikaları yükleyerek erişim denetimini ayarlama.

**SetupEntryPoint'in**nasıl yapılandırılabildiğini hakkında daha fazla bilgi için [bkz.](service-fabric-application-runas-security.md)

<a id="Package-App"></a>

## <a name="configure"></a>Yapılandırma

### <a name="build-a-package-by-using-visual-studio"></a>Visual Studio'u kullanarak paket oluşturun

Uygulamanızı oluşturmak için Visual Studio'yu kullandıysanız, yukarıda açıklanan düzenle eşleşen bir paket oluşturmak için *Paket* komutunu kullanabilirsiniz.

Paket oluşturmak için *Solution Explorer'daki* uygulama projesini sağ tıklatın ve **Paket** komutunu seçin:

![Visual Studio ile uygulama paketleme][vs-package-command]

Ambalaj tamamlandığında, Paketin konumunu **Çıktı** penceresinde bulabilirsiniz. Uygulamanızı Visual Studio'da dağıttığınızda veya hata ayıkladiğinizde paketleme adımı otomatik olarak gerçekleşir.

### <a name="build-a-package-by-command-line"></a>Komut satırına göre paket oluşturma

Uygulamanızı programlı olarak paketlemek de `msbuild.exe`mümkündür. Kaputun altında, Visual Studio çıktı aynı şekilde çalışıyor.

```shell
D:\Temp> msbuild HelloWorld.sfproj /t:Package
```

## <a name="test-the-package"></a>Paketi test edin

[Test-ServiceFabricApplicationPackage](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) komutunu kullanarak PowerShell üzerinden paket yapısını yerel olarak doğrulayabilirsiniz.
Bu komut, bildirim ayrıştma sorunlarını denetler ve tüm başvuruları doğrular. Bu komut yalnızca paketteki dizinlerin ve dosyaların yapısal doğruluğunu doğrular.
Gerekli tüm dosyaların mevcut olup olmadığını denetlemenin ötesinde, kod veya veri paketi içeriğinin hiçbirini doğrulamaz.

```powershell
Test-ServiceFabricApplicationPackage .\MyApplicationType
```

```Output
False
Test-ServiceFabricApplicationPackage : The EntryPoint MySetup.bat is not found.
FileName: C:\Users\servicefabric\AppData\Local\Temp\TestApplicationPackage_7195781181\nrri205a.e2h\MyApplicationType\MyServiceManifest\ServiceManifest.xml
```

Bu hata, hizmet bildirimi **SetupEntryPoint'te** başvurulan *MySetup.bat* dosyasının kod paketinde eksik olduğunu gösterir. Eksik dosya eklendikten sonra, uygulama doğrulaması geçer:

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

Uygulamanızda [uygulama parametreleri](service-fabric-manage-multiple-environment-app-configuration.md) tanımlanmışsa, doğru doğrulama için [test-serviskumaşuygulama paketinden](/powershell/module/servicefabric/test-servicefabricapplicationpackage?view=azureservicefabricps) geçebilirsiniz.

Uygulamanın dağıtılanacağı kümeyi biliyorsanız, `ImageStoreConnectionString` parametrede geçmeniz önerilir. Bu durumda, paket, zaten kümede çalışan uygulamanın önceki sürümlerine karşı da doğrulanır. Örneğin, doğrulama, aynı sürüme sahip ancak farklı içeriğe sahip bir paketin zaten dağıtılıp dağıtılmadığını algılayabilir.  

Uygulama doğru bir şekilde paketlendikten ve doğrulamayı geçtikten sonra, daha hızlı dağıtım işlemleri için paketi sıkıştırmayı düşünün.

## <a name="compress-a-package"></a>Paketi sıkıştırma

Bir paket büyükse veya çok sayıda dosyavarsa, daha hızlı dağıtım için paketi sıkıştırabilirsiniz. Sıkıştırma dosya sayısını ve paket boyutunu azaltır.
Sıkıştırılmış bir uygulama paketi için, [uygulama paketinin yüklenmesi,](service-fabric-deploy-remove-applications.md#upload-the-application-package) özellikle sıkıştırma kopyanın bir parçası olarak yapılıyorsa, sıkıştırılmamış paketin yüklenmesine kıyasla daha uzun sürebilir. Sıkıştırma [ile,](service-fabric-deploy-remove-applications.md#unregister-an-application-type) [kayıt](service-fabric-deploy-remove-applications.md#register-the-application-package) ve uygulama türü kayıt açma daha hızlıdır.

Dağıtım mekanizması sıkıştırılmış ve sıkıştırılmamış paketler için aynıdır. Paket sıkıştırılırsa, küme görüntü deposunda bu şekilde depolanır ve uygulama çalıştırılmadan önce düğümüzerinde sıkıştırılmamıştır.
Sıkıştırma, geçerli Service Fabric paketinin yerini sıkıştırılmış sürümle değiştirir. Klasör yazma izinlerine izin vermelidir. Zaten sıkıştırılmış bir paketüzerinde sıkıştırma çalıştırma hiçbir değişiklik verir.

Powershell komutu [Copy-ServiceFabricApplicationPackage'ı](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) anahtarla `CompressPackage` çalıştırarak paketi sıkıştırabilirsiniz. Anahtarı kullanarak `UncompressPackage` paketi aynı komutla açabilirsiniz.

Aşağıdaki komut, paketi görüntü deposuna kopyalamadan sıkıştırır. Sıkıştırılmış bir paketi, `SkipCopy` bayrak olmadan [Copy-ServiceFabricApplicationPackage'ı](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) kullanarak gerektiğinde bir veya daha fazla Service Fabric kümesine kopyalayabilirsiniz.
Paket şimdi `code`, , `config`ve `data` paketleri için sıkıştırılmış dosyaları içerir. Birçok dahili işlem için gerekli olduğundan, uygulama bildirimi ve hizmet bildirimleri sıkıştırılmamaktadır. Örneğin, paket paylaşımı, uygulama türü adı ve belirli doğrulamalar için sürüm çıkarma tüm bildirimlere erişmek gerekir. Manifestoları zipping bu işlemleri verimsiz hale getirecek.

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

Alternatif olarak, [copy-serviceFabricApplicationPackage](/powershell/module/servicefabric/copy-servicefabricapplicationpackage?view=azureservicefabricps) ile paketi tek adımda sıkıştırıp kopyalayabilirsiniz.
Paket büyükse, hem paket sıkıştırma hem de kümeye yükleme için zaman tanımak için yeterince yüksek bir zaman dilimi sağlayın.

```powershell
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath .\MyApplicationType -ApplicationPackagePathInImageStore MyApplicationType -ImageStoreConnectionString fabric:ImageStore -CompressPackage -TimeoutSec 5400
```

Dahili olarak, Service Fabric doğrulama için uygulama paketleri için checksums bilgi verir. Sıkıştırma kullanırken, kontrol ler her paketin sıkıştırılmış sürümlerinde hesaplanır. Aynı uygulama paketinden yeni bir zip oluşturmak farklı kontrol ler oluşturur. Doğrulama hatalarını önlemek için [diff provisioning'i](service-fabric-application-upgrade-advanced.md)kullanın. Bu seçenekle, yeni sürüme değişmeyen paketleri eklemeyin. Bunun yerine, bunları doğrudan yeni hizmet bildiriminden başvurun.

Diff sağlama bir seçenek değilse ve paketleri eklemeniz gerekiyorsa, `code`checksum uyuşmazlığı önlemek için , `config`ve `data` paketler için yeni sürümler oluşturmanız gerekir. Önceki sürümün sıkıştırma kullanıp kullanmadığına bakılmaksızın, sıkıştırılmış paket kullanıldığında değişmemiş paketler için yeni sürümler oluşturmak gereklidir.

Paket artık doğru bir şekilde paketlenmiş, doğrulanmış ve sıkıştırılmış (gerekirse), bir veya daha fazla Service Fabric kümesine [dağıtıma](service-fabric-deploy-remove-applications.md) hazırdır.

### <a name="compress-packages-when-deploying-using-visual-studio"></a>Visual Studio'u kullanarak dağıtırken paketleri sıkıştırın

Visual Studio'ya, `CopyPackageParameters` yayımlama profilinize öğeyi ekleyerek paketleri dağıtımda `CompressPackage` sıkıştırmasını `true`ve özniteliği 'ne göre ayarlamasını söyleyebilirsiniz.

``` xml
    <PublishProfile xmlns="http://schemas.microsoft.com/2015/05/fabrictools">
        <ClusterConnectionParameters ConnectionEndpoint="mycluster.westus.cloudapp.azure.com" />
        <ApplicationParameterFile Path="..\ApplicationParameters\Cloud.xml" />
        <CopyPackageParameters CompressPackage="true"/>
    </PublishProfile>
```

## <a name="create-an-sfpkg"></a>Bir sfpkg oluşturma

Sürüm 6.1 ile başlayarak, Service Fabric harici bir mağazadan tedarik sağlar.
Bu seçenekle, uygulama paketinin görüntü deposuna kopyalanmasını gerekmez. Bunun yerine, bir `sfpkg` oluşturup harici bir mağazaya yükleyebilir, ardından indirme URI'yi Hizmet Kumaşı'na sağlayabilirken sağlayabilirsiniz. Aynı paket birden çok kümeye sağlanabilir. Dış mağazadan sağlama, paketi her kümeye kopyalamak için gereken zamandan tasarruf sağlar.

Dosya, `sfpkg` ilk başvuru paketini içeren ve ".sfpkg" uzantısı olan bir zip'tir.
Fermuarın içinde, uygulama paketi sıkıştırılabilir veya sıkıştırılamaz. Zip içinde uygulama paketinin sıkıştırma [kod,](service-fabric-package-apps.md#compress-a-package)config ve veri paketi düzeylerinde, daha önce belirtildiği gibi yapılır.

Bir `sfpkg`, özgün uygulama paketini içeren, sıkıştırılmış veya olmayan bir klasörle başlayın. Daha sonra, uzantısı ".sfpkg" ile klasörü zip için herhangi bir yardımcı program kullanın. Örneğin, [ZipFile.CreateFromDirectory](https://msdn.microsoft.com/library/hh485721(v=vs.110).aspx)kullanın.

```csharp
ZipFile.CreateFromDirectory(appPackageDirectoryPath, sfpkgFilePath);
```

Servis `sfpkg` Kumaşı dışında bant dışında harici mağazaya yüklenmesi gerekir. Harici depo, REST http veya https bitiş noktasını ortaya çıkaran herhangi bir mağaza olabilir. Sağlama sırasında Service Fabric, uygulama paketini `sfpkg` indirmek için bir GET işlemi yürütür, bu nedenle mağazanın paket için READ erişimine izin vermesi gerekir.

Paketi sağlamak için, indirme URI'sini ve uygulama türü bilgilerini gerektiren harici hükmü kullanın.

>[!NOTE]
> Görüntü deposu göreli yoluna dayalı sağlama `sfpkg` şu anda dosyaları desteklemiyor. Bu nedenle, görüntü `sfpkg` deposuna kopyalanmamalıdır.

## <a name="next-steps"></a>Sonraki adımlar

[Uygulamaları dağıtma ve kaldırma,][10] uygulama örneklerini yönetmek için PowerShell'in nasıl kullanılacağını açıklar

[Birden çok ortam için uygulama parametrelerinin yönetilmesi,][11] farklı uygulama örnekleri için parametrelerin ve ortam değişkenlerinin nasıl yapılandırılabildiğini açıklar.

[Uygulamanız için güvenlik ilkelerini yapılandırın,][12] erişimi kısıtlamak için güvenlik ilkeleri altında hizmetlerin nasıl çalıştırılacak yapılacağını açıklar.

<!--Image references-->
[vs-package-command]: ./media/service-fabric-package-apps/vs-package-command.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[10]: service-fabric-deploy-remove-applications.md
[11]: service-fabric-manage-multiple-environment-app-configuration.md
[12]: service-fabric-application-runas-security.md