---
title: Kullanılabilir bir çalıştırılabilir'i Azure Hizmet Kumaşı'na dağıtma
description: Varolan bir uygulamayı hizmet kumaşı kümesine dağıtılabilmek için kullanılabilir konuk olarak nasıl paketleyebilirsiniz öğrenin.
ms.topic: conceptual
ms.date: 07/02/2017
ms.openlocfilehash: cdbc965d0e8ec4a8f42fbe438b8ac6ddfe05a1b3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75377115"
---
# <a name="package-and-deploy-an-existing-executable-to-service-fabric"></a>Mevcut bir yürütülebilir yazılımı Servis Kumaşı'na paketleyip dağıtın
Bir [konuk çalıştırılabilir](service-fabric-guest-executables-introduction.md)olarak mevcut bir yürütülebilir paketleme, bir Visual Studio proje şablonu kullanmak veya [uygulama paketini el ile oluşturmak](#manually)için seçebilirsiniz. Visual Studio kullanılarak, uygulama paketi yapısı ve bildirim dosyaları sizin için yeni proje şablonu tarafından oluşturulur.

> [!TIP]
> Mevcut bir Windows'u bir hizmete sığdırmanın en kolay yolu Visual Studio'yu ve Linux'u kullanarak Yeoman'ı kullanmaktır.
>

## <a name="use-visual-studio-to-package-and-deploy-an-existing-executable"></a>Varolan bir yürütülebilir paketi ve dağıtmak için Visual Studio'u kullanın
Visual Studio, hizmet kumaşı kümesine çalıştırılabilir bir konuk dağıtmanıza yardımcı olmak için bir Service Fabric hizmet şablonu sağlar.

1. **Dosya** > **Yeni Projesi'ni**seçin ve bir Hizmet Kumaşı uygulaması oluşturun.
2. Hizmet şablonu olarak **Konuk Çalıştırılabilir'i** seçin.
3. Yürütülebilir klasörü seçmek ve hizmeti oluşturmak için parametrelerin geri kalanını doldurmak için **Gözat'ı** tıklatın.
   * *Kod Paketi Davranışı*. Dosyanızın tüm içeriğini Visual Studio Project'e kopyalamak için ayarlanabilir, bu da yürütülebilir değişiklik yoksa yararlıdır. Yürütülebilir yapının değişmesini bekliyorsanız ve dinamik olarak yeni yapılar alma olanağı istiyorsanız, bunun yerine klasöre bağlanmayı seçebilirsiniz. Visual Studio'da uygulama projesini oluştururken bağlantılı klasörleri kullanabilirsiniz. Bu, proje içinden kaynak konuma bağlanır ve konuk tarafından çalıştırılabilir kaynağı hedefte güncelleştirilebilmenizi sağlar. Bu güncelleştirmeler, yapıdaki uygulama paketinin bir parçası haline gelir.
   * *Program,* hizmeti başlatmak için çalıştırılması gereken yürütülebilir belirtir.
   * *Bağımsız değişkenler,* yürütülebilir e aktarılması gereken bağımsız değişkenleri belirtir. Bu bağımsız değişkenler ile parametrelerin bir listesi olabilir.
   * *WorkingFolder,* başlatılacak işlemin çalışma dizinini belirtir. Üç değer belirtebilirsiniz:
     * `CodeBase`çalışma dizininin uygulama paketindeki (önceki dosya yapısında gösterilen diziliş)`Code` kod dizini olarak ayarlanacağını belirtir.
     * `CodePackage`çalışma dizininin uygulama paketinin köküne (önceki dosya yapısında`GuestService1Pkg` gösterildiği) ayarlanacağını belirtir.
     * `Work`dosyaların çalışma adı verilen bir alt dizine yerleştirildiğini belirtir.
4. Hizmetinize bir ad verin ve **Tamam**’a tıklayın.
5. Hizmetinizin iletişim için bir bitiş noktası olması gerekiyorsa, artık protokolü, bağlantı noktasını ve türü ServiceManifest.xml dosyasına ekleyebilirsiniz. Örneğin: `<Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" UriScheme="http" PathSuffix="myapp/" Type="Input" />`.
6. Artık paketi kullanabilir ve Visual Studio'da çözümü hata ayıklayarak yerel kümenize karşı eylem yayınlayabilirsiniz. Hazır olduğunuzda, uygulamayı uzak bir kümede yayımlayabilir veya kaynak denetimi için çözümü iade edebilirsiniz.
7. Hizmet Kumaş Explorer'da çalışan konuk çalıştırılabilir hizmetinizi nasıl göreceğinizi görmek için [çalışan uygulamanızı kontrol](#check-your-running-application) edin.

Örnek bir gözden geçirme için bkz. [Visual Studio'yu kullanarak ilk konuk çalıştırılabilir uygulamanızı oluşturun.](quickstart-guest-app.md)

## <a name="use-yeoman-to-package-and-deploy-an-existing-executable-on-linux"></a>Linux'ta mevcut bir yürütülebilir paketi ve dağıtmak için Yeoman'ı kullanın

Linux'ta çalıştırılabilir bir konuk oluşturma ve dağıtma yordamı, bir csharp veya java uygulaması dağıtmakla aynıdır.

1. Bir terminal penceresinde `yo azuresfguest` yazın.
2. Uygulamanızı adlandırın.
3. Hizmetinizi adlandırın ve yürütülebilir yol ve çağrılması gereken parametreleri içeren ayrıntıları sağlayın.

Yeoman, komut dosyalarını yükleme ve kaldırma ile birlikte uygun uygulama ve manifesto dosyalarıyla birlikte bir uygulama paketi oluşturur.

<a id="manually"></a>

## <a name="manually-package-and-deploy-an-existing-executable"></a>Varolan bir çalıştırılabilir'i el ile paketleyip dağıtın
Bir konuğun çalıştırılabilir el ile paketlenmesi işlemi aşağıdaki genel adımlara bağlıdır:

1. Paket dizin yapısını oluşturun.
2. Uygulamanın kodunu ve yapılandırma dosyalarını ekleyin.
3. Hizmet bildirimi dosyasını edin.
4. Uygulama bildirimi dosyasını edin.

<!--
>[AZURE.NOTE] We do provide a packaging tool that allows you to create the ApplicationPackage automatically. The tool is currently in preview. You can download it from [here](https://aka.ms/servicefabricpacktool).
-->

### <a name="create-the-package-directory-structure"></a>Paket dizin yapısını oluşturma
[Paket Azure Hizmet Kumaş Uygulaması'nda](https://docs.microsoft.com/azure/service-fabric/service-fabric-package-apps)açıklandığı gibi dizin yapısını oluşturarak başlayabilirsiniz.

### <a name="add-the-applications-code-and-configuration-files"></a>Uygulamanın kod ve yapılandırma dosyalarını ekleme
Dizin yapısını oluşturduktan sonra, uygulamanın kod ve yapılandırma dosyalarını kod ve config dizinleri altında ekleyebilirsiniz. Ayrıca kod veya config dizinleri altında ek dizinler veya alt dizinler oluşturabilirsiniz.

Service Fabric `xcopy` uygulama kök dizininin içeriğini yapar, bu nedenle iki üst dizin, kod ve ayar oluşturmak dışında kullanılacak önceden tanımlanmış bir yapı yoktur. (İsterseniz farklı adlar seçebilirsiniz. Daha fazla bilgi sonraki bölümde yer almaktadır.)

> [!NOTE]
> Uygulamanın ihtiyaç duyduğu tüm dosyaları ve bağımlılıkları eklediğinizden emin olun. Service Fabric, uygulama nın hizmetlerinin dağıtılacayacağı kümedeki bütün düğümlerde uygulama paketinin içeriğini kopyalar. Paket, uygulamanın çalışması gereken tüm kodu içermelidir. Bağımlılıkların zaten yüklü olduğunu varsaymayın.
>
>

### <a name="edit-the-service-manifest-file"></a>Hizmet bildirimi dosyasını edin
Bir sonraki adım, hizmet bildirimi dosyasını aşağıdaki bilgileri içerecek şekilde yeniden sağlamaktır:

* Hizmet türünün adı. Bu, Service Fabric'in bir hizmeti tanımlamak için kullandığı bir kimliktir.
* Uygulamayı başlatmak için kullanılacak komut (ExeHost).
* Uygulamayı ayarlamak için çalıştırılması gereken herhangi bir komut dosyası (SetupEntrypoint).

Aşağıda bir `ServiceManifest.xml` dosya örneği verilmiştir:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" Name="NodeApp" Version="1.0.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceTypes>
      <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true"/>
   </ServiceTypes>
   <CodePackage Name="code" Version="1.0.0.0">
      <SetupEntryPoint>
         <ExeHost>
             <Program>scripts\launchConfig.cmd</Program>
         </ExeHost>
      </SetupEntryPoint>
      <EntryPoint>
         <ExeHost>
            <Program>node.exe</Program>
            <Arguments>bin/www</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
         </ExeHost>
      </EntryPoint>
   </CodePackage>
   <Resources>
      <Endpoints>
         <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
   </Resources>
</ServiceManifest>
```

Aşağıdaki bölümler, dosyanın güncelleştirmeniz gereken farklı bölümlerinin üzerinden geçer.

#### <a name="update-servicetypes"></a>Hizmet Türlerini Güncelleştir
```xml
<ServiceTypes>
  <StatelessServiceType ServiceTypeName="NodeApp" UseImplicitHost="true" />
</ServiceTypes>
```

* İstediğiniz adı `ServiceTypeName`seçebilirsiniz. Değer, hizmeti tanımlamak `ApplicationManifest.xml` için dosyada kullanılır.
* `UseImplicitHost="true"` belirtin. Bu özellik Service Fabric'e hizmetin bağımsız bir uygulamaya dayandığını söyler, bu nedenle Hizmet Fabric'in tek yapması gereken bir işlem olarak başlatmak ve sağlığını izlemektir.

#### <a name="update-codepackage"></a>CodePackage'ı Güncelleştir
CodePackage öğesi, hizmet kodunun konumunu (ve sürümünü) belirtir.

```xml
<CodePackage Name="Code" Version="1.0.0.0">
```

Öğe, `Name` hizmet kodunu içeren uygulama paketinde dizinin adını belirtmek için kullanılır. `CodePackage`ayrıca `version` özniteliği vardır. Bu, kodun sürümünü belirtmek için kullanılabilir ve Hizmet Kumaşı'ndaki uygulama yaşam döngüsü yönetim altyapısını kullanarak hizmetin kodunu yükseltmek için de kullanılabilir.

#### <a name="optional-update-setupentrypoint"></a>İsteğe bağlı: SetupEntrypoint'i Güncelleştir
```xml
<SetupEntryPoint>
   <ExeHost>
       <Program>scripts\launchConfig.cmd</Program>
   </ExeHost>
</SetupEntryPoint>
```
SetupEntryPoint öğesi, hizmet kodu başlatılmadan önce yürütülmesi gereken yürütülebilir veya toplu iş dosyasını belirtmek için kullanılır. Bu isteğe bağlı bir adımdır, bu nedenle herhangi bir başlatma gerekli olduğunda dahil edilmesi gerekmez. SetupEntryPoint, hizmet her yeniden başlatıldığında yürütülür.

Yalnızca bir SetupEntryPoint olduğundan, uygulamanın kurulumu birden çok komut dosyası gerektiriyorsa, kurulum komut dosyalarının tek bir toplu iş dosyasında gruplandırılması gerekir. SetupEntryPoint her tür dosyayı çalıştırabilir: çalıştırılabilir dosyalar, toplu iş dosyaları ve PowerShell cmdlets. Daha fazla bilgi için Bkz. [KurulumEntryPoint'i Yapılandır.](service-fabric-application-runas-security.md)

Önceki örnekte, SetupEntryPoint kod dizininin `LaunchConfig.cmd` `scripts` alt dizininde bulunan (WorkingFolder öğesinin CodeBase olarak ayarlanmış olduğunu varsayarak) adlı bir toplu iş dosyası çalıştırır.

#### <a name="update-entrypoint"></a>Giriş Noktasını Güncelleştir
```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
  </ExeHost>
</EntryPoint>
```

Hizmet `EntryPoint` bildirimi dosyasındaki öğe, hizmetin nasıl başlatılsüreceğini belirtmek için kullanılır.

Öğe, `ExeHost` hizmeti başlatmak için kullanılması gereken yürütülebilir (ve bağımsız değişkenler) belirtir. Programın kod paketinin `IsExternalExecutable="true"` dışında `ExeHost` harici bir yürütülebilir olduğunu belirtmek için özniteliği isteğe bağlı olarak ekleyebilirsiniz. Örneğin, `<ExeHost IsExternalExecutable="true">`.

* `Program`hizmeti başlatması gereken yürütülebilir adı belirtir.
* `Arguments`yürütülebilir geçirilmelidir bağımsız değişkenleri belirtir. Bu bağımsız değişkenler ile parametrelerin bir listesi olabilir.
* `WorkingFolder`başlatılacak işlemin çalışma dizinini belirtir. Üç değer belirtebilirsiniz:
  * `CodeBase`çalışma dizininin uygulama paketindeki (önceki dosya yapısındaki dizini)`Code` kod dizini olarak ayarlanacağını belirtir.
  * `CodePackage`çalışma dizininin uygulama paketinin köküne (önceki dosya yapısında)`GuestService1Pkg` ayarlanacağını belirtir.
    * `Work`dosyaların çalışma adı verilen bir alt dizine yerleştirildiğini belirtir.

WorkingFolder, göreli yolların uygulama veya başlatma komut dosyaları tarafından kullanılabilmesi için doğru çalışma dizinini ayarlamak için yararlıdır.

#### <a name="update-endpoints-and-register-with-naming-service-for-communication"></a>Uç Noktaları güncelleştirin ve iletişim için Adlandırma Hizmetine kaydolun
```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000" Type="Input" />
</Endpoints>

```
Önceki örnekte, `Endpoint` öğe, uygulamanın dinleyebileceği uç noktaları belirtir. Bu örnekte, Node.js uygulaması bağlantı noktası 3000'de http'de dinlenir.

Ayrıca, diğer hizmetlerin bu hizmetin bitiş noktası adresini bulabilmeleri için Hizmet Kumaşı'ndan bu bitiş noktasını Adlandırma Hizmeti'ne yayımlamasını isteyebilirsiniz. Bu, konuk çalıştırılabilir hizmetler arasında iletişim kurabilmenizi sağlar.
Yayınlanan bitiş noktası adresi formdur. `UriScheme://IPAddressOrFQDN:Port/PathSuffix` `UriScheme`ve `PathSuffix` isteğe bağlı özniteliklerdir. `IPAddressOrFQDN`bu çalıştırılabilir alır düğümün IP adresi veya tam nitelikli etki alanı adıdır ve sizin için hesaplanır.

Aşağıdaki örnekte, hizmet dağıtıldıktan sonra, Service Fabric Explorer'da hizmet `http://10.1.4.92:3000/myapp/` örneği için yayınlanana benzer bir bitiş noktası görürsünüz. Ya da bu yerel bir `http://localhost:3000/myapp/`makineyse, görüyorsunuz.

```xml
<Endpoints>
   <Endpoint Name="NodeAppTypeEndpoint" Protocol="http" Port="3000"  UriScheme="http" PathSuffix="myapp/" Type="Input" />
</Endpoints>
```
Hizmetler arasında iletişim kurmak için bu adresleri [ters proxy](service-fabric-reverseproxy.md) ile kullanabilirsiniz.

### <a name="edit-the-application-manifest-file"></a>Uygulama bildirimi dosyasını edin
Dosyayı `Servicemanifest.xml` yapılandırdıktan sonra, doğru hizmet türü `ApplicationManifest.xml` ve adının kullanıldığından emin olmak için dosyada bazı değişiklikler yapmanız gerekir.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="NodeAppType" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
   </ServiceManifestImport>
</ApplicationManifest>
```

#### <a name="servicemanifestimport"></a>ServiceManifestImport
Öğede, `ServiceManifestImport` uygulamaya eklemek istediğiniz bir veya daha fazla hizmeti belirtebilirsiniz. `ServiceManifestName`Hizmetler, `ServiceManifest.xml` dosyanın bulunduğu dizinin adını belirten ile başvurulur.

```xml
<ServiceManifestImport>
  <ServiceManifestRef ServiceManifestName="NodeApp" ServiceManifestVersion="1.0.0.0" />
</ServiceManifestImport>
```

## <a name="set-up-logging"></a>Günlük günlüğe kaydetme
Konuk yürütülebilir ler için, uygulama ve yapılandırma komut dosyalarının herhangi bir hata gösterip gösterolmadığını öğrenmek için konsol günlüklerini görebilmek yararlıdır.
Konsol yeniden yönlendirme `ServiceManifest.xml` `ConsoleRedirection` öğesi kullanılarak dosyada yapılandırılabilir.

> [!WARNING]
> Konsol yeniden yönlendirme ilkesini üretimde dağıtılan bir uygulamada asla kullanmayın, çünkü bu uygulamanın başarısız olup olmadığını etkileyebilir. Bunu *yalnızca* yerel geliştirme ve hata ayıklama amacıyla kullanın.  
>
>

```xml
<EntryPoint>
  <ExeHost>
    <Program>node.exe</Program>
    <Arguments>bin/www</Arguments>
    <WorkingFolder>CodeBase</WorkingFolder>
    <ConsoleRedirection FileRetentionCount="5" FileMaxSizeInKb="2048"/>
  </ExeHost>
</EntryPoint>
```

`ConsoleRedirection`konsol çıktısını (hem stdout hem de stderr) çalışan bir dizine yönlendirmek için kullanılabilir. Bu, Hizmet Kumaşı kümesinde uygulamanın kurulumu veya yürütülmesi sırasında hata olmadığını doğrulama olanağı sağlar.

`FileRetentionCount`çalışma dizininde kaç dosyanın kaydedilebildiğini belirler. Örneğin, 5 değeri, önceki beş yürütmenin günlük dosyalarının çalışma dizininde depolanmış olduğu anlamına gelir.

`FileMaxSizeInKb`günlük dosyalarının maksimum boyutunu belirtir.

Günlük dosyaları hizmetin çalışma dizinlerinden birine kaydedilir. Dosyaların nerede bulunduğunu belirlemek için, hizmetin hangi düğümüzerinde çalıştığını ve hangi çalışma dizininin kullanıldığını belirlemek için Hizmet Kumaş ı Gezgini'ni kullanın. Bu işlem daha sonra bu makalede ele alınmıştır.

## <a name="deployment"></a>Dağıtım
Son [adım, uygulamanızı dağıtmaktır.](service-fabric-deploy-remove-applications.md) Aşağıdaki PowerShell komut dosyası, uygulamanızı yerel geliştirme kümesine nasıl dağıtılayınız ve yeni bir Service Fabric hizmeti başlatmayı gösterir.

```powershell

Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath 'C:\Dev\MultipleApplications' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'nodeapp'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'nodeapp'

New-ServiceFabricApplication -ApplicationName 'fabric:/nodeapp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0

New-ServiceFabricService -ApplicationName 'fabric:/nodeapp' -ServiceName 'fabric:/nodeapp/nodeappservice' -ServiceTypeName 'NodeApp' -Stateless -PartitionSchemeSingleton -InstanceCount 1

```

>[!TIP]
> [Paket büyükse](service-fabric-package-apps.md#compress-a-package) veya çok sayıda dosya varsa, görüntü deposuna kopyalamadan önce paketi sıkıştırın. [Burada](service-fabric-deploy-remove-applications.md#upload-the-application-package)daha fazla bilgi edinin .
>

Bir Service Fabric hizmeti çeşitli "yapılandırmalarda" dağıtılabilir. Örneğin, tek veya birden çok örnek olarak dağıtılabilir veya Hizmet Kumaşı kümesinin her düğümünde hizmetin bir örneği olacak şekilde dağıtılabilir.

`New-ServiceFabricService` Cmdlet'in `InstanceCount` parametresi, Hizmet Kumaşı kümesinde kaç hizmet örneği başlatılması gerektiğini belirtmek için kullanılır. Dağıttığınız uygulama türüne bağlı olarak deÄ `InstanceCount` eri ayarlayırabilirsiniz. En yaygın iki senaryo şunlardır:

* `InstanceCount = "1"`. Bu durumda, kümede hizmetin yalnızca bir örneği dağıtılır. Service Fabric'in zamanlayıcısı, hizmetin hangi düğümde dağıtılanacağını belirler.
* `InstanceCount ="-1"`. Bu durumda, hizmetin bir örneği Hizmet Kumaşı kümesindeki her düğüme dağıtılır. Sonuç, kümedeki her düğüm için hizmetin bir (ve yalnızca bir) örneğine sahip olmaktır.

Bu, ön uç uygulamaları (örneğin, bir REST bitiş noktası) için yararlı bir yapılandırmadır, çünkü istemci uygulamalarının bitiş noktasını kullanmak için kümedeki düğümlerden herhangi birisine "bağlanması" gerekir. Bu yapılandırma, örneğin Service Fabric kümesinin tüm düğümleri bir yük dengeleyicisine bağlandığında da kullanılabilir. İstemci trafiği daha sonra kümedeki tüm düğümlerde çalışan hizmete dağıtılabilir.

## <a name="check-your-running-application"></a>Çalışan uygulamanızı kontrol edin
Service Fabric Explorer'da, hizmetin çalıştığı düğümü tanımlayın. Bu örnekte, Düğüm1'de çalışır:

![Hizmetin çalıştığı düğüm](./media/service-fabric-deploy-existing-app/nodeappinsfx.png)

Düğüme gidin ve uygulamaya göz atarsanız, diskteki konumu da dahil olmak üzere temel düğüm bilgilerini görürsünüz.

![Diskteki konum](./media/service-fabric-deploy-existing-app/locationondisk2.png)

Sunucu Gezgini'ni kullanarak dizine göz atarsanız, aşağıdaki ekran görüntüsünde gösterildiği gibi çalışma dizinini ve hizmetin günlük klasörünü bulabilirsiniz: 

![Günlüğün konumu](./media/service-fabric-deploy-existing-app/loglocation.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, bir konuk çalıştırılabilir paketi ve Hizmet Kumaş dağıtmak nasıl öğrendim. İlgili bilgi ve görevler için aşağıdaki makalelere bakın.

* Paketleme aracının ön salıverilme bağlantısı da dahil olmak üzere, uygulanabilir bir konuk paketleme [ve dağıtma için örnek](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [REST kullanarak Adlandırma hizmeti aracılığıyla iletişim kuran iki konuk çalıştırılabilir (C# ve nodejs) örneği](https://github.com/Azure-Samples/service-fabric-containers)
* [Konuk tarafından yürütülebilir birden çok uygulama dağıtma](service-fabric-deploy-multiple-apps.md)
* [Visual Studio'yu kullanarak ilk Servis Kumaşı uygulamanızı oluşturun](service-fabric-tutorial-create-dotnet-app.md)
