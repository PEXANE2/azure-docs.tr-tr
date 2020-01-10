---
title: MongoDB kullanan bir Node. js uygulaması dağıtma
description: Azure Service Fabric kümesine dağıtmak için birden çok Konuk yürütülebilir dosya paketleme hakkında izlenecek yol
author: mikkelhegn
ms.topic: conceptual
ms.date: 02/23/2018
ms.author: mikhegn
ms.openlocfilehash: 4538efc8a2426fc20dd20d1a85edaf6f76bfc649
ms.sourcegitcommit: 003e73f8eea1e3e9df248d55c65348779c79b1d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/02/2020
ms.locfileid: "75614477"
---
# <a name="deploy-multiple-guest-executables"></a>Konuk tarafından yürütülebilir birden çok uygulama dağıtma
Bu makalede birden çok Konuk yürütülebilirin nasıl paketlenecek ve Azure Service Fabric dağıtılacağı gösterilmektedir. Tek bir Service Fabric paketi oluşturmak ve dağıtmak için [Service Fabric bir konuk yürütülebilir dosyası dağıtmayı](service-fabric-deploy-existing-app.md)okuyun.

Bu izlenecek yol, veri deposu olarak MongoDB kullanan Node. js ön ucuna sahip bir uygulamanın nasıl dağıtılacağını gösterir, ancak adımları başka bir uygulama üzerinde bağımlılıkları olan herhangi bir uygulamaya uygulayabilirsiniz.   

Birden çok Konuk yürütülebilir dosya içeren uygulama paketini oluşturmak için Visual Studio 'Yu kullanabilirsiniz. [Mevcut bir uygulamayı paketlemek için bkz. Visual Studio 'Yu kullanma](service-fabric-deploy-existing-app.md). İlk Konuk yürütülebilirini ekledikten sonra, uygulama projesine sağ tıklayın ve **Add-> yeni Service Fabric hizmetini** seçerek ikinci Konuk yürütülebilir projesini çözüme ekleyin. Note: kaynağı Visual Studio projesinde bağlamayı seçerseniz, Visual Studio çözümünü oluşturun, uygulama paketinizin kaynaktaki değişikliklerle güncel olduğundan emin olur. 

## <a name="samples"></a>Örnekler
* [Konuk yürütülebilir dosyası paketleme ve dağıtma örneği](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [REST kullanarak adlandırma hizmeti üzerinden iletişimC# kuran iki konuk yürütülebilir dosya (ve NodeJS) örneği](https://github.com/Azure-Samples/service-fabric-containers)

## <a name="manually-package-the-multiple-guest-executable-application"></a>Birden çok Konuk yürütülebilir uygulamasını el ile paketleme
Alternatif olarak, Konuk yürütülebilir dosyasını el ile paketleyebilir. Ayrıntılar için bkz. [el ile paketleme ve mevcut bir yürütülebiliri dağıtma](service-fabric-deploy-existing-app.md#manually-package-and-deploy-an-existing-executable).

### <a name="packaging-the-nodejs-application"></a>Node. js uygulamasını paketleme
Bu makalede, Node. js ' nin Service Fabric kümesindeki düğümlerde yüklü olmadığı varsayılır. Sonuç olarak, paketlemeden önce Node. exe ' yi düğüm uygulamanızın kök dizinine eklemeniz gerekir. Node. js uygulamasının dizin yapısı (Express Web çerçevesi ve Jade şablon altyapısı kullanılarak) aşağıdaki gibi görünmelidir:

```
|-- NodeApplication
    |-- bin
        |-- www
    |-- node_modules
        |-- .bin
        |-- express
        |-- jade
        |-- etc.
    |-- public
        |-- images
        |-- etc.
    |-- routes
        |-- index.js
        |-- users.js
    |-- views
        |-- index.jade
        |-- etc.
    |-- app.js
    |-- package.json
    |-- node.exe
```

Sonraki adım olarak, Node. js uygulaması için bir uygulama paketi oluşturursunuz. Aşağıdaki kod, Node. js uygulamasını içeren bir Service Fabric uygulama paketi oluşturur.

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

Kullanılan parametrelerin açıklaması aşağıda verilmiştir:

* **/Source** , paketlenmesi gereken uygulamanın dizinine işaret eder.
* **/target** , paketin oluşturulması gereken dizini tanımlar. Bu dizin, kaynak dizinden farklı olmalıdır.
* **/appname** mevcut uygulamanın uygulama adını tanımlar. Bu, Service Fabric uygulama adına değil, bildirimde hizmet adına çevrilip dönüştürmediğinden emin olmak önemlidir.
* **/exe** Service Fabric başlatılması beklenen yürütülebilir dosyayı tanımlar, bu durumda `node.exe`.
* **/ma** , yürütülebilir dosyayı başlatmak için kullanılan bağımsız değişkeni tanımlar. Node. js yüklü olmadığından, Service Fabric `node.exe bin/www`yürüterek Node. js web sunucusunu başlatması gerekir.  `/ma:'bin/www'` paketleme aracına Node. exe için bağımsız değişken olarak `bin/www` kullanmasını söyler.
* **/Apptype** Service Fabric uygulama türü adını tanımlar.

/Target parametresinde belirtilen dizine gözattığınızda, aracın aşağıda gösterildiği gibi tam olarak çalışan bir Service Fabric paketi oluşturduğunu görebilirsiniz:

```
|--[yourtargetdirectory]
    |-- NodeApplication
        |-- C
              |-- bin
              |-- data
              |-- node_modules
              |-- public
              |-- routes
              |-- views
              |-- app.js
              |-- package.json
              |-- node.exe
        |-- config
              |--Settings.xml
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```
Oluşturulan ServiceManifest. xml artık, aşağıdaki kod parçacığında gösterildiği gibi node. js web sunucusunun nasıl başlatılması gerektiğini açıklayan bir bölüme sahiptir:

```xml
<CodePackage Name="C" Version="1.0">
    <EntryPoint>
        <ExeHost>
            <Program>node.exe</Program>
            <Arguments>'bin/www'</Arguments>
            <WorkingFolder>CodePackage</WorkingFolder>
        </ExeHost>
    </EntryPoint>
</CodePackage>
```
Bu örnekte, Node. js web sunucusu 3000 numaralı bağlantı noktasını dinler, bu nedenle ServiceManifest. xml dosyasındaki uç nokta bilgilerini aşağıda gösterildiği gibi güncelleştirmeniz gerekir.   

```xml
<Resources>
      <Endpoints>
         <Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
### <a name="packaging-the-mongodb-application"></a>MongoDB uygulamasını paketleme
Node. js uygulamasını paketlediğinizden, artık devam edebilir ve MongoDB 'yi paketleyebilirsiniz. Daha önce bahsedildiği gibi, bundan sonra gittiğiniz adımlar Node. js ve MongoDB 'ye özgü değildir. Aslında, tek bir Service Fabric uygulaması olarak birlikte paketlenmesi amaçlanan tüm uygulamalar için geçerlidir.  

MongoDB 'yi paketlemek için, Mongod. exe ve Mongo. exe ' yi paketlediğinizden emin olmak istersiniz. Her iki ikili dosya da MongoDB Yükleme dizininizin `bin` dizininde bulunur. Dizin yapısı aşağıdakine benzer şekilde görünür.

```
|-- MongoDB
    |-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
Service Fabric MongoDB 'yi aşağıda gösterilene benzer bir komutla başlatması gerekir, bu nedenle MongoDB paketleme sırasında `/ma` parametresini kullanmanız gerekir.

```
mongod.exe --dbpath [path to data]
```
> [!NOTE]
> MongoDB veri dizinini düğümün yerel dizinine yerleştirirseniz, veriler düğüm hatası durumunda korunmaz. Veri kaybını engellemek için dayanıklı depolama kullanmanız veya bir MongoDB çoğaltma kümesi uygulamanız gerekir.  
>
>

PowerShell veya komut kabuğu 'nda paketleme aracını aşağıdaki parametrelerle çalıştırdık:

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path to data]' /AppType:NodeAppType
```

Service Fabric uygulama paketinize MongoDB eklemek için/target parametresinin, Node. js uygulamasıyla birlikte uygulama bildirimini zaten içeren dizine işaret ettiğini doğrulayın. Aynı ApplicationType adını kullandığınızdan de emin olmanız gerekir.

Dizine gözatalım ve aracın ne yaptığını incelemektir.

```
|--[yourtargetdirectory]
    |-- MyNodeApplication
    |-- MongoDB
        |-- C
            |--bin
                |-- mongod.exe
                |-- mongo.exe
                |-- etc.
        |-- config
            |--Settings.xml
        |-- ServiceManifest.xml
    |-- ApplicationManifest.xml
```
Gördüğünüz gibi, araç MongoDB ikili dosyalarını içeren dizine yeni bir klasörünü MongoDB ekledi. `ApplicationManifest.xml` dosyasını açarsanız, paketin artık Node. js uygulamasını ve MongoDB 'yi içerdiğini görebilirsiniz. Aşağıdaki kod, uygulama bildiriminin içeriğini gösterir.

```xml
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="MyNodeApp" ApplicationTypeVersion="1.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="MongoDB" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <ServiceManifestImport>
      <ServiceManifestRef ServiceManifestName="NodeService" ServiceManifestVersion="1.0" />
   </ServiceManifestImport>
   <DefaultServices>
      <Service Name="MongoDBService">
         <StatelessService ServiceTypeName="MongoDB">
            <SingletonPartition />
         </StatelessService>
      </Service>
      <Service Name="NodeServiceService">
         <StatelessService ServiceTypeName="NodeService">
            <SingletonPartition />
         </StatelessService>
      </Service>
   </DefaultServices>
</ApplicationManifest>  
```

### <a name="publishing-the-application"></a>Uygulama yayımlama
Son adım, aşağıdaki PowerShell betiklerini kullanarak uygulamayı yerel Service Fabric kümesine yayımlamaktır:

```
Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath '[yourtargetdirectory]' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'NodeAppType'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'NodeAppType'

New-ServiceFabricApplication -ApplicationName 'fabric:/NodeApp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0  
```

Uygulama yerel kümeye başarıyla yayımlandıktan sonra Node. js uygulamasına Node. js uygulamasının hizmet bildiriminde girdiğimiz bağlantı noktasındaki Node. js uygulamasına erişebilirsiniz; Örneğin, http:\//localhost: 3000.

Bu öğreticide, iki mevcut uygulamayı tek bir Service Fabric uygulaması olarak nasıl kolayca paketlemeyi gördünüz. Ayrıca, yüksek kullanılabilirlik ve sistem durumu sistemi tümleştirmesi gibi bazı Service Fabric özelliklerden yararlanabilmesi için bunu Service Fabric dağıtmayı öğrendiniz.

## <a name="adding-more-guest-executables-to-an-existing-application-using-yeoman-on-linux"></a>Linux üzerinde Yeumman kullanarak mevcut bir uygulamaya daha fazla konuk yürütülebilir dosya ekleme

`yo` kullanılarak oluşturulmuş bir uygulamaya başka bir hizmet eklemek için aşağıdaki adımları uygulayın: 
1. Dizini mevcut uygulamanın kök dizinine değiştirin.  Örneğin Yeoman tarafından oluşturulan uygulama `MyApplication` ise `cd ~/YeomanSamples/MyApplication` olacaktır.
2. `yo azuresfguest:AddService` çalıştırın ve gerekli ayrıntıları sağlayın.

## <a name="next-steps"></a>Sonraki adımlar
* [Service Fabric ve kapsayıcılarla kapsayıcıları dağıtmaya genel bakış](service-fabric-containers-overview.md) hakkında bilgi edinin
* [Konuk yürütülebilir dosyası paketleme ve dağıtma örneği](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [REST kullanarak adlandırma hizmeti üzerinden iletişimC# kuran iki konuk yürütülebilir dosya (ve NodeJS) örneği](https://github.com/Azure-Samples/service-fabric-containers)
