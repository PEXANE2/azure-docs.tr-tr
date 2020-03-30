---
title: MongoDB kullanan bir Düğüm.js uygulaması dağıtma
description: Azure Hizmet Kumaşı kümesine dağıtmak için birden çok konuk çalıştırılabilirin nasıl paketleneceğe ilişkin gözden geçirme
author: mikkelhegn
ms.topic: conceptual
ms.date: 02/23/2018
ms.author: mikhegn
ms.openlocfilehash: 4538efc8a2426fc20dd20d1a85edaf6f76bfc649
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75614477"
---
# <a name="deploy-multiple-guest-executables"></a>Konuk tarafından yürütülebilir birden çok uygulama dağıtma
Bu makalede, azure hizmet kumaşına birden çok konuk çalıştırılabilir insanın nasıl paketleyip dağıtılacağa yorumlanmaktadır. Tek bir Service Fabric paketi oluşturmak ve dağıtmak [için Service Fabric'e çalıştırılabilir bir konuğun nasıl dağıtılabildiğini](service-fabric-deploy-existing-app.md)okuyun.

Bu izlik, veri deposu olarak MongoDB kullanan Bir Düğüm.js ön ucu olan bir uygulamanın nasıl dağıtılanacağı gösterse de, adımları başka bir uygulamaya bağımlı olan herhangi bir uygulamaya uygulayabilirsiniz.   

Birden çok konuk çalıştırılabilir içeren uygulama paketini oluşturmak için Visual Studio'yu kullanabilirsiniz. Bkz. [Varolan bir uygulamayı paketlemek için Visual Studio'yu kullanma.](service-fabric-deploy-existing-app.md) İlk konuk çalıştırılabilir ekledikten sonra, uygulama projesine sağ tıklayın ve çözüme ikinci konuk çalıştırılabilir projeeklemek için **Add->Yeni Hizmet Kumaşı hizmetini** seçin. Not: Visual Studio çözümlerini oluşturarak Visual Studio projesindeki kaynağı bağlamayı seçerseniz, uygulama paketinizin kaynaktaki değişikliklerle güncel olduğundan emin olursunuz. 

## <a name="samples"></a>Örnekler
* [Uygulanabilir bir konuğu paketlemek ve dağıtmak için örnek](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [REST kullanarak Adlandırma hizmeti aracılığıyla iletişim kuran iki konuk çalıştırılabilir (C# ve nodejs) örneği](https://github.com/Azure-Samples/service-fabric-containers)

## <a name="manually-package-the-multiple-guest-executable-application"></a>Birden çok konuk çalıştırılabilir uygulamayı el ile paketle
Alternatif olarak, uygun çalıştırılabilir el ile paketleyebilirsiniz. Ayrıntılar için, [bkz.](service-fabric-deploy-existing-app.md#manually-package-and-deploy-an-existing-executable)

### <a name="packaging-the-nodejs-application"></a>Düğüm.js uygulamasının paketlenmesi
Bu makalede, Node.js Hizmet Kumaş kümesindeki düğümlere yüklü olmadığını varsayar. Sonuç olarak, paketlemeden önce düğüm uygulamanızın kök dizinine Düğüm.exe eklemeniz gerekir. Node.js uygulamasının dizin yapısı (Express web framework ve Jade şablon uyrağı kullanılarak) aşağıdakine benzer olmalıdır:

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

Bir sonraki adım olarak, Düğüm.js uygulaması için bir uygulama paketi oluşturursunuz. Aşağıdaki kod, Düğüm.js uygulamasını içeren bir Service Fabric uygulama paketi oluşturur.

```
.\ServiceFabricAppPackageUtil.exe /source:'[yourdirectory]\MyNodeApplication' /target:'[yourtargetdirectory] /appname:NodeService /exe:'node.exe' /ma:'bin/www' /AppType:NodeAppType
```

Aşağıda kullanılan parametrelerin bir açıklaması verilmiştir:

* **/kaynak,** paketlemesi gereken uygulama dizinine işaret ederler.
* **/hedef,** paketin oluşturulması gereken dizini tanımlar. Bu dizin kaynak dizinden farklı olmalıdır.
* **/appname,** varolan uygulamanın uygulama adını tanımlar. Bunun Servis Kumaşı uygulama adı için değil, bildirimdeki servis adına çevrildiğini anlamak önemlidir.
* **/exe,** Service Fabric'in bu durumda `node.exe`başlatması gereken yürütülebilir liği tanımlar.
* **/ma,** yürütülebilir başlatmak için kullanılan bağımsız değişkeni tanımlar. Node.js yüklü olmadığından, Service Fabric'in node.js web sunucusunu `node.exe bin/www`çalıştırarak başlatması gerekir.  `/ma:'bin/www'`node.exe `bin/www` için argüman olarak kullanmak için ambalaj aracı söyler.
* **/AppType,** Service Fabric uygulama türü adını tanımlar.

/hedef parametresinde belirtilen dizine göz atarsanız, aracın aşağıda gösterildiği gibi tam işlevli bir Service Fabric paketi oluşturduğunu görebilirsiniz:

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
Oluşturulan ServiceManifest.xml şimdi aşağıdaki kod snippet gösterildiği gibi, Node.js web sunucusu nasıl başlatılması gerektiğini açıklayan bir bölüm vardır:

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
Bu örnekte, Node.js web sunucusu port 3000'i dinler, bu nedenle ServiceManifest.xml dosyasındaki bitiş noktası bilgilerini aşağıda gösterildiği gibi güncelleştirmeniz gerekir.   

```xml
<Resources>
      <Endpoints>
         <Endpoint Name="NodeServiceEndpoint" Protocol="http" Port="3000" Type="Input" />
      </Endpoints>
</Resources>
```
### <a name="packaging-the-mongodb-application"></a>MongoDB uygulamasının paketlenmesi
Artık Node.js uygulamasını paketlediğinize göre, MongoDB'yi paketleyebilirsiniz. Daha önce de belirtildiği gibi, şimdi geçmek adımları Node.js ve MongoDB özgü değildir. Aslında, tek bir Hizmet Kumaş uygulaması olarak birlikte paketlenecek olan tüm uygulamalar için geçerlidir.  

MongoDB'yi paketlemek için Mongod.exe ve Mongo.exe'yi paketlediğinden emin olmak istersiniz. Her iki ikili de `bin` MongoDB yükleme dizininizdeki dizinde yer alır. Dizin yapısı aşağıdakine benzer.

```
|-- MongoDB
    |-- bin
        |-- mongod.exe
        |-- mongo.exe
        |-- anybinary.exe
```
Service Fabric'in MongoDB'yi aşağıdakine benzer bir komutla başlatması gerekir, bu nedenle MongoDB'yi paketlerken `/ma` parametreyi kullanmanız gerekir.

```
mongod.exe --dbpath [path to data]
```
> [!NOTE]
> Düğümün yerel dizinine MongoDB veri dizini koyarsanız, düğüm hatası durumunda veriler korunmuyor. Veri kaybını önlemek için dayanıklı depolama alanı kullanmalı veya MongoDB yineleme kümesi uygulamalısınız.  
>
>

PowerShell veya komut kabuğunda, paketleme aracını aşağıdaki parametrelerle çalıştırıyoruz:

```
.\ServiceFabricAppPackageUtil.exe /source: [yourdirectory]\MongoDB' /target:'[yourtargetdirectory]' /appname:MongoDB /exe:'bin\mongod.exe' /ma:'--dbpath [path to data]' /AppType:NodeAppType
```

Hizmet Kumaşı uygulama paketinize MongoDB eklemek için, /hedef parametrenin Node.js uygulamasıyla birlikte uygulama bildirimini içeren aynı dizine işaret ettiğinden emin olmanız gerekir. Ayrıca aynı ApplicationType adını kullandığınızdan emin olmanız gerekir.

Dizine göz atalım ve aracın ne yarattığını inceleyelim.

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
Gördüğünüz gibi, araç MongoDB ikililerini içeren dizine yeni bir klasör, MongoDB ekledi. Dosyayı `ApplicationManifest.xml` açarsanız, paketin artık hem Node.js uygulamasını hem de MongoDB'yi içerdiğini görebilirsiniz. Aşağıdaki kod, uygulama bildiriminin içeriğini gösterir.

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

### <a name="publishing-the-application"></a>Uygulamayı yayımlama
Son adım, aşağıdaki PowerShell komut dosyalarını kullanarak uygulamayı yerel Service Fabric kümesine yayımlamaktır:

```
Connect-ServiceFabricCluster localhost:19000

Write-Host 'Copying application package...'
Copy-ServiceFabricApplicationPackage -ApplicationPackagePath '[yourtargetdirectory]' -ImageStoreConnectionString 'file:C:\SfDevCluster\Data\ImageStoreShare' -ApplicationPackagePathInImageStore 'NodeAppType'

Write-Host 'Registering application type...'
Register-ServiceFabricApplicationType -ApplicationPathInImageStore 'NodeAppType'

New-ServiceFabricApplication -ApplicationName 'fabric:/NodeApp' -ApplicationTypeName 'NodeAppType' -ApplicationTypeVersion 1.0  
```

Uygulama yerel kümeye başarıyla yayımlandıktan sonra, Node.js uygulamasının hizmet bildirimine girdiğimiz bağlantı noktasındaki Düğüm.js uygulamasına\/erişebilirsiniz -- örneğin http: /localhost:3000.

Bu eğitimde, mevcut iki uygulamayı tek bir Service Fabric uygulaması olarak kolayca nasıl paketlendireceğinizi görmüş olabilirsiniz. Yüksek kullanılabilirlik ve sistem tümleştirmesi gibi Bazı Service Fabric özelliklerinden yararlanabilmesi için servis kumaşına nasıl dağıtacağınıda da öğrendiniz.

## <a name="adding-more-guest-executables-to-an-existing-application-using-yeoman-on-linux"></a>Linux'ta Yeoman'ı kullanarak mevcut bir uygulamaya daha fazla konuk çalıştırılabilir ekleme

`yo` kullanılarak oluşturulmuş bir uygulamaya başka bir hizmet eklemek için aşağıdaki adımları uygulayın: 
1. Dizini mevcut uygulamanın kök dizinine değiştirin.  Örneğin Yeoman tarafından oluşturulan uygulama `MyApplication` ise `cd ~/YeomanSamples/MyApplication` olacaktır.
2. Çalıştırın `yo azuresfguest:AddService` ve gerekli ayrıntıları sağlayın.

## <a name="next-steps"></a>Sonraki adımlar
* [Servis Kumaşı ve konteynerlerle](service-fabric-containers-overview.md) konteynerdağıtma hakkında bilgi edinin
* [Uygulanabilir bir konuğu paketlemek ve dağıtmak için örnek](https://github.com/Azure-Samples/service-fabric-dotnet-getting-started)
* [REST kullanarak Adlandırma hizmeti aracılığıyla iletişim kuran iki konuk çalıştırılabilir (C# ve nodejs) örneği](https://github.com/Azure-Samples/service-fabric-containers)
