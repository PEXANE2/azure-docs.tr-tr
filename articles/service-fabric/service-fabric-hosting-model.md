---
title: Azure Hizmet Kumaşı barındırma modeli
description: Dağıtılmış bir Hizmet Dokusu hizmetinin yinelemeleri (veya örnekleri) ile hizmet barındırıcı işlemi arasındaki ilişkiyi açıklar.
author: harahma
ms.topic: conceptual
ms.date: 04/15/2017
ms.author: harahma
ms.openlocfilehash: 82bc5068be651b05eb24efa3b05e46c1e7c1e24d
ms.sourcegitcommit: fb23286d4769442631079c7ed5da1ed14afdd5fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81115049"
---
# <a name="azure-service-fabric-hosting-model"></a>Azure Hizmet Kumaşı barındırma modeli
Bu makalede, Azure Hizmet Kumaşı tarafından sağlanan uygulama barındırma modellerine genel bir bakış sağlanır ve **Paylaşılan İşlem** ve Özel **İşlem** modelleri arasındaki farkları açıklar. Dağıtılan bir uygulamanın Hizmet Dokusu düğümünde nasıl göründüğünü ve hizmetin yinelemeleri (veya örnekleri) ile hizmet barındırıcı işlemi arasındaki ilişkiyi açıklar.

Daha fazla devam etmeden önce, [Hizmet Kumaşı'nda Model'de][a1]açıklanan çeşitli kavramları ve ilişkileri anladığınızdan emin olun. 

> [!NOTE]
> Bu makalede, açıkça farklı bir şey anlamı olarak belirtilmedikçe:
>
> - *Yineleme,* hem devlet hizmetinin bir kopyasını hem de bir devletsiz hizmet örneğini ifade eder.
> - *CodePackage,* ServiceType'ı kaydeden bir *ServiceHost* işlemine eşdeğer olarak kabul edilir ve bu *ServiceType'ın*hizmetlerinin yinelemelerini barındıran bir işlemdir. *ServiceType*
>

Barındırma modelini anlamak için bir örnek üzerinden bakalım. *ServiceType* 'MyServiceType' olan bir *ApplicationType* 'MyAppType' olduğunu varsayalım. 'MyServiceType' bir *CodePackage* 'MyCodePackage' olan *ServicePackage* 'MyServicePackage' tarafından sağlanmaktadır. 'MyCodePackage' çalıştığında *ServiceType* 'MyServiceType' kaydeder.

Diyelim ki üç düğümlü bir kümemiz var ve bir *uygulama* dokusu **oluşturuyoruz:/App1** tipi 'MyAppType'. Bu uygulama **kumaş içinde:/App1**, biz bir hizmet **kumaş oluşturmak:/App1/ServiceA** türü 'MyServiceType'. Bu hizmetin iki bölümü (örneğin, **P1** ve **P2)** ve bölüm başına üç yineleme vardır. Aşağıdaki diyagram, bir düğüm üzerinde dağıtılır biter gibi bu uygulamanın görünümünü gösterir.


![Dağıtılan uygulamanın düğüm görünümünün diyagramı][node-view-one]


Service Fabric, her iki bölümden de kopyaları barındıran 'MyCodePackage'ı başlatan 'MyServicePackage'i etkinleştirdi. Kümedeki tüm düğümler aynı görünüme sahip, çünkü kümedeki düğüm sayısına eşit olacak şekilde bölüm başına yineleme sayısını seçtik. Başka bir hizmet oluşturalım, **kumaş:/App1/ServiceB**, uygulama **kumaş:/App1**. Bu hizmetin bir bölümü (örneğin, **P3)** ve bölüm başına üç yineleme vardır. Aşağıdaki diyagram düğümde yeni görünümü gösterir:


![Dağıtılan uygulamanın düğüm görünümünün diyagramı][node-view-two]


Service Fabric, 'MyServicePackage'ın mevcut etkinleştirmesinde hizmet kumaşının **P3** bölümü için yeni kopyayı **yerleştirdi:/App1/ServiceB.** Nwo. başka bir uygulama **kumaşı oluşturalım:/App2** tipi 'MyAppType'. Kumaş **Içinde:/App2,** bir servis **kumaşı oluşturmak:/App2/ServiceA**. Bu hizmetin iki bölümü **(P4** ve **P5)** ve bölüm başına üç yineleme vardır. Aşağıdaki diyagram yeni düğüm görünümünü gösterir:


![Dağıtılan uygulamanın düğüm görünümünün diyagramı][node-view-three]


Service Fabric, 'MyCodePackage'ın yeni bir kopyasını başlatan 'MyServicePackage'ın yeni bir kopyasını etkinleştirir. Hizmet kumaşı her iki bölümden **kopyaları:/App2/ServiceA** **(P4** ve **P5)** bu yeni kopya 'MyCodePackage' yerleştirilir.

## <a name="shared-process-model"></a>Paylaşılan İşlem modeli
Önceki bölümde, Paylaşılan İşlem modeli olarak adlandırılan Service Fabric tarafından sağlanan varsayılan barındırma modeli açıklanmaktadır. Bu modelde, belirli bir uygulama için, belirli bir *ServicePackage'ın* yalnızca bir kopyası bir düğüm üzerinde etkinleştirilir (bu da içinde bulunan tüm *CodePackage'ları* başlatır). Belirli bir *ServiceType'ın* tüm hizmetlerinin tüm kopyaları *ServiceType'ı*kaydeden *CodePackage'a* yerleştirilir. Başka bir deyişle, belirli bir *ServiceType* düğümündeki tüm hizmetlerin yinelemeleri aynı işlemi paylaşır.

## <a name="exclusive-process-model"></a>Özel İşlem modeli
Service Fabric tarafından sağlanan diğer hosting modeli Exclusive Process modelidir. Bu modelde, belirli bir düğümüzerinde, her yineleme kendi özel işlem yaşıyor. Service Fabric *ServicePackage'ın* yeni bir kopyasını etkinleştirir (içinde bulunan tüm *CodePackages'ı* başlatır). Yinelemeler, yinelemenin ait olduğu hizmetin *ServiceType'ını* kaydeden *CodePackage'a* yerleştirilir. 

Service Fabric sürüm 5.6 veya daha yeni bir sürümünü kullanıyorsanız, bir hizmet oluşturduğunuz anda [(PowerShell][p1], [REST][r1]veya [FabricClient][c1]kullanarak) Özel İşlem modelini seçebilirsiniz. **ServicePackageActivationMode'u** 'ExclusiveProcess' olarak belirtin.

```powershell
PS C:\>New-ServiceFabricService -ApplicationName "fabric:/App1" -ServiceName "fabric:/App1/ServiceA" -ServiceTypeName "MyServiceType" -Stateless -PartitionSchemeSingleton -InstanceCount -1 -ServicePackageActivationMode "ExclusiveProcess"
```

```csharp
var serviceDescription = new StatelessServiceDescription
{
    ApplicationName = new Uri("fabric:/App1"),
    ServiceName = new Uri("fabric:/App1/ServiceA"),
    ServiceTypeName = "MyServiceType",
    PartitionSchemeDescription = new SingletonPartitionSchemeDescription(),
    InstanceCount = -1,
    ServicePackageActivationMode = ServicePackageActivationMode.ExclusiveProcess
};

var fabricClient = new FabricClient(clusterEndpoints);
await fabricClient.ServiceManager.CreateServiceAsync(serviceDescription);
```

Uygulama bildiriminizde varsayılan bir hizmet varsa, **ServicePackageActivationMode** özniteliğini belirterek Özel İşlem modelini seçebilirsiniz:

```xml
<DefaultServices>
  <Service Name="MyService" ServicePackageActivationMode="ExclusiveProcess">
    <StatelessService ServiceTypeName="MyServiceType" InstanceCount="1">
      <SingletonPartition/>
    </StatelessService>
  </Service>
</DefaultServices>
```
Şimdi başka bir hizmet oluşturalım, **kumaş:/App1/ServiceC**, uygulama **kumaş:/App1**. Bu hizmetin iki bölümü (örneğin, **P6** ve **P7)** ve bölüm başına üç yineleme vardır. **ServicePackageActivationMode'u** 'ExclusiveProcess' olarak ayarlarsınız. Aşağıdaki diyagram düğümde yeni bir görünüm gösterir:


![Dağıtılan uygulamanın düğüm görünümünün diyagramı][node-view-four]


Gördüğünüz gibi, Service Fabric 'MyServicePackage' (bölüm **P6** ve **P7**her çoğaltma için bir) iki yeni kopyaaktive. Service Fabric *CodePackage*kendi özel kopyası her çoğaltma yerleştirilir. Özel İşlem modelini kullandığınızda, belirli bir uygulama için, belirli bir *ServicePackage'ın* birden çok kopyası bir düğüm üzerinde etkin olabilir. Önceki örnekte, 'MyServicePackage' üç kopya kumaş için **etkin:/App1**. 'MyServicePackage' bu aktif kopyalarının her biri ile ilişkili bir **ServicePackageActivationId** vardır. Bu kimlik, uygulama dokusu içindeki kopyayı **tanımlar:/App1**.

Bir uygulama için yalnızca Paylaşılan İşlem modelini kullandığınızda, bir düğümüzerinde *ServicePackage'ın* yalnızca bir etkin kopyası vardır. ServicePackage bu aktivasyonu için **ServicePackageActivationId** boş bir dize. *ServicePackage* Bu durumda, örneğin, **kumaş:/App2**ile .

> [!NOTE]
>- Paylaşılan İşlem barındırma modeli **ServicePackageActivationMode** eşittir **SharedProcess**karşılık gelir. Bu varsayılan barındırma modelidir ve **ServicePackageActivationMode** hizmet oluşturma sırasında belirtilmesi gerekmez.
>
>- Exclusive Process hosting modeli **ServicePackageActivationMode** karşılık gelir **ExclusiveProcess**eşittir. Bu ayarı kullanmak için, hizmeti oluştururken açıkça belirtmeniz gerekir. 
>
>- Bir hizmetin barındırma modelini görüntülemek [için, hizmet açıklamasını][p2]sorgula ve **ServicePackageActivationMode**değerine bakın.
>
>

## <a name="work-with-a-deployed-service-package"></a>Dağıtılan bir servis paketiyle çalışma
Bir düğümdeki *ServicePackage* etkin bir kopyası dağıtılan [bir hizmet paketi][p3]olarak adlandırılır. Hizmet oluşturmak için Özel İşlem modelini kullandığınızda, belirli bir uygulama için aynı *ServicePackage*için birden çok dağıtılmış hizmet paketi olabilir. Dağıtılan bir hizmet paketine özgü işlemler gerçekleştirin, belirli bir dağıtılmış hizmet paketini tanımlamak için **ServicePackageActivationId'i** sağlamanız gerekir. Örneğin, dağıtılan bir hizmet [paketinin durumunu bildiriyorveya][p4] [dağıtılmış bir hizmet paketinin kod paketini yeniden başlatıyorsanız][p5]kimliği sağlayın.

Dağıtılan hizmet [paketlerinin][p3] listesini bir düğüm üzerinde sorgulayarak dağıtılan bir hizmet paketinin **ServicePackageActivationId'ini** öğrenebilirsiniz. [Dağıtılan hizmet türleri,][p6] [dağıtılan yinelemeler][p7]ve bir düğüm üzerinde [dağıtılan kod paketleri][p8] için sorgu yaparken, sorgu sonucu, ana dağıtılan hizmet paketinin **ServicePackageActivationId'ini** de içerir.

> [!NOTE]
>- Paylaşılan İşlem barındırma modeli altında, belirli bir düğümüzerinde, belirli bir uygulama için, bir *ServicePackage* yalnızca bir kopyası etkinleştirilir. *Boş dize*ye eşit bir **ServicePackageActivationId'e** sahiptir ve dağıtılan servis paketiyle ilgili işlemleri gerçekleştirirken belirtilmesi gerekmez. 
>
> - Özel İşlem barındırma modeli altında, belirli bir düğüm üzerinde, belirli bir uygulama için, bir *ServicePackage* bir veya daha fazla kopya etkin olabilir. Her etkinleştirme, dağıtılan servis paketiyle ilgili işlemleri gerçekleştirirken belirtilen *boş olmayan* **servicePackageActivationId'e**sahiptir. 
>
> - **ServicePackageActivationId** atlanırsa, varsayılan olarak *boş dize*için. Paylaşılan İşlem modeli altında etkinleştirilen dağıtılmış bir hizmet paketi varsa, işlem üzerinde gerçekleştirilir. Aksi takdirde işlem başarısız olur.
>
> - Bir kez sorgu yapmayın ve **ServicePackageActivationId**önbellek . Kimlik dinamik olarak oluşturulur ve çeşitli nedenlerle değişebilir. **ServicePackageActivationId**gerektiren bir işlemi gerçekleştirmeden önce, önce [dağıtılan hizmet paketlerinin][p3] listesini bir düğüm üzerinde sorgulamalısınız. Ardından, özgün işlemi gerçekleştirmek için sorgu sonucundaki **ServicePackageActivationId'i** kullanın.
>
>

## <a name="guest-executable-and-container-applications"></a>Konuk çalıştırılabilir ve konteyner uygulamaları
Service [Fabric, konuk çalıştırılabilir][a2] ve [konteyner][a3] uygulamalarını, bağımsız hizmet veren, bağımsız hizmetler olarak ele almaktadır. *ServiceHost'ta* (bir işlem veya konteyner) Servis Kumaşı çalışma süresi yoktur. Bu hizmetler bağımsız olduğundan, *ServiceHost* başına yineleme sayısı bu hizmetler için geçerli değildir. Bu hizmetlerle birlikte kullanılan en yaygın yapılandırma, [InstanceCount'ın][c2] -1'e eşit olduğu tek bölümdür (kümenin her düğümünde çalışan hizmet kodunun bir kopyası). 

Bu hizmetler için varsayılan **ServicePackageActivationMode** **SharedProcess'dir,** bu durumda Service Fabric yalnızca belirli bir uygulama için bir düğüm üzerinde *ServicePackage'ın* bir kopyasını etkinleştirir.  Bu, hizmet kodunun yalnızca bir kopyasının düğüm çalıştıracağı anlamına gelir. Hizmet kodunuzu birden çok kopyasının bir düğümüzerinde çalışmasını istiyorsanız, hizmeti oluştururken **ServicePackageActivationMode'u** **ExclusiveProcess** olarak belirtin. Örneğin, *ServiceType'ın* birden çok hizmeti *(Service1* to *ServiceN)* oluşturduğunuzda *(ServiceManifest'te*belirtilir) veya hizmetiniz çok bölümlü olduğunda bunu yapabilirsiniz. 

## <a name="change-the-hosting-model-of-an-existing-service"></a>Varolan bir hizmetin barındırma modelini değiştirme
Şu anda, varolan bir hizmetin barındırma modelini Paylaşılan İşlem'den Özel İşleme (veya tam tersi) değiştiremezsiniz.

## <a name="choose-between-the-hosting-models"></a>Barındırma modelleri arasında seçim yapın
Hangi barındırma modelinin gereksinimlerinize en uygun olduğunu değerlendirmelisiniz. Paylaşılan İşlem modeli, daha az işlem ortaya çıktığı ve aynı işlemdeki birden çok yineleme bağlantı noktalarını paylaşabildiği için işletim sistemi kaynaklarını daha iyi kullanır. Ancak, yinelemelerden birinde hizmet ana bilgisayarını aşağı getirmesi gereken bir hata varsa, aynı işlemdeki diğer tüm yinelemeleri etkiler.

 Exclusive Process modeli, kendi sürecinde her yineleme ile daha iyi yalıtım sağlar. Yinelemelerden birinde hata varsa, diğer yinelemeleri etkilemez. Bu model, bağlantı noktası paylaşımının iletişim protokolü tarafından desteklenmediğini durumlar için yararlıdır. Bu çoğaltma düzeyinde kaynak yönetişim uygulamak için yeteneği kolaylaştırır. Ancak, Özel İşlem, düğümdeki her yineleme için bir işlem oluşturduğundan, daha fazla işletim sistemi kaynağı tüketir.

## <a name="exclusive-process-model-and-application-model-considerations"></a>Özel İşlem modeli ve uygulama modeli konuları
Çoğu uygulama için, *ServicePackage*başına bir *ServiceType* tutarak Hizmet Kumaşı'nda uygulamanızı modelleyebilirsiniz. 

Belirli durumlarda, Service Fabric ayrıca *ServicePackage* başına birden fazla *ServiceType* sağlar (ve bir *CodePackage* birden fazla *ServiceType*kaydedebilirsiniz). Bu yapılandırmaların yararlı olabileceği senaryolardan bazıları şunlardır:

- Daha az işlem oluşturarak ve işlem başına daha yüksek çoğaltma yoğunluğuna sahip olarak kaynak kullanımını en iyi duruma getirmek istiyorsunuz.
- Farklı *ServiceTypes* yinelemeler yüksek bir başlatma veya bellek maliyeti olan bazı ortak verileri paylaşmak gerekir.
- Ücretsiz bir hizmet teklifiniz var ve hizmetin tüm kopyalarını aynı işleme koyarak kaynak kullanımına bir sınır koymak istiyorsunuz.

Özel İşlem barındırma *modeli, ServicePackage*başına birden fazla *ServiceTypes* olan bir uygulama modeli ile tutarlı değildir. Bunun nedeni, *ServicePackage* başına birden çok *ServiceTypes* yinelemeler arasında daha yüksek kaynak paylaşımı elde etmek için tasarlanmış olmasıdır ve işlem başına daha yüksek çoğaltma yoğunluğu sağlar. Exclusive Process modeli farklı sonuçlar elde etmek için tasarlanmıştır.

Her *ServiceType'ı*kaydeden farklı bir *CodePackage* ile *ServicePackage*başına birden fazla *ServiceTypes* örneğini göz önünde bulundurun. İki *CodePackages*olan bir *ServicePackage* 'MultiTypeServicePackage' olduğunu varsayalım:

- *ServiceType* 'MyServiceTypeA'yı kaydeden 'MyCodePackageA'.
- *ServiceType* 'MyServiceTypeB'i kaydeden 'MyCodePackageB'.

Şimdi, diyelim ki bir uygulama, **kumaş oluşturmak:/SpecialApp**. Kumaş ın **Içinde:/SpecialApp**, Exclusive Process modeli ile aşağıdaki iki hizmeti oluşturuyoruz:

- Servis **kumaşı:/SpecialApp/ServiceA** tipi 'MyServiceTypeA', iki bölüm (örneğin, **P1** ve **P2)** ve bölüm başına üç yineleme ile.
- Servis **kumaş:/SpecialApp/ServiceB** tipi 'MyServiceTypeB', iki bölüm **(P3** ve **P4)** ve bölüm başına üç yineleme ile.

Belirli bir düğümde, her iki hizmetin de her biri iki yinelemeye sahiptir. Hizmetleri oluşturmak için Özel İşlem modelini kullandığımız için, Service Fabric her yineleme için 'MyServicePackage'ın yeni bir kopyasını etkinleştirir. 'MultiTypeServicePackage'ın her aktivasyonu 'MyCodePackageA' ve 'MyCodePackageB'nin bir kopyasını başlatır. Ancak, 'MyCodePackageA' veya 'MyCodePackageB' yalnızca biri ,'MultiTypeServicePackage' etkinleştirildiği çoğaltmayı barındırır. Aşağıdaki diyagram düğüm görünümünü gösterir:


![Dağıtılan uygulamanın düğüm görünümünün diyagramı][node-view-five]


Hizmet kumaş bölüm **P1** çoğaltma için 'MultiTypeServicePackage' aktivasyonu:/SpecialApp/ServiceA , 'MyCodePackageA' çoğaltma ev sahipliği yapıyor. **fabric:/SpecialApp/ServiceA** 'MyCodePackageB' çalışıyor. Benzer şekilde, hizmet kumaş bölüm **P3** çoğaltma için 'MultiTypeServicePackage' aktivasyonu:/SpecialApp/ServiceB , 'MyCodePackageB' çoğaltma ev sahipliği yapıyor. **fabric:/SpecialApp/ServiceB** 'MyCodePackageA' çalışıyor. Bu nedenle, *ServicePackage*başına *CodePackages* (farklı *ServiceTypes*kayıt) sayısı daha fazla, daha yüksek gereksiz kaynak kullanımı. 
 
 Ancak, hizmet **kumaşoluşturursak:/SpecialApp/ServiceA** ve **kumaş:/SpecialApp/ServiceB** ile Paylaşılan İşlem modeli, Service Fabric uygulama kumaşı için 'MultiTypeServicePackage'ın yalnızca bir kopyasını etkinleştirir:/SpecialApp. **fabric:/SpecialApp** 'MyCodePackageA' hizmet kumaşı için tüm kopyaları **barındırıyor:/SpecialApp/ServiceA.** 'MyCodePackageB' hizmet kumaşı için tüm kopyaları **barındırır:/SpecialApp/ServiceB.** Aşağıdaki diyagram, bu ayarda düğüm görünümünü gösterir: 


![Dağıtılan uygulamanın düğüm görünümünün diyagramı][node-view-six]


Önceki örnekte, 'MyCodePackageA' hem 'MyServiceTypeA' hem de 'MyServiceTypeB' kaydediyorsa ve 'MyCodePackageB' yoksa, gereksiz *CodePackage* running yok diye düşünebilirsiniz. Bu doğru olsa da, bu uygulama modeli Özel İşlem barındırma modeli ile uyumlu değildir. Amaç her yinelemeyi kendi özel işlemine koymaksa, her iki *ServiceTypes'ı* da aynı *CodePackage'dan*kaydetmeniz gerekmez. Bunun yerine, her *ServiceType'ı* kendi *ServicePackage'ına*koymanız yeterlidir.

### <a name="reliable-services-and-actor-forking-subprocesses"></a>Güvenilir Hizmetler ve Aktör forking alt süreçleri

Service Fabric, güvenilir hizmetleri ve daha sonra güvenilir aktörlerin alt süreçlerini desteklemez. Neden desteklenmeyen [CodePackageActivationContext](https://docs.microsoft.com/dotnet/api/system.fabric.codepackageactivationcontext?view=azure-dotnet) bir örnek desteklenmeyen bir alt işlem kaydetmek için kullanılamaz ve iptal belirteçleri sadece kayıtlı işlemlere gönderilir; alt işlemler üst işlem iptal jetonu aldıktan sonra kapanmadığında yükseltme hataları gibi her türlü soruna neden olabilir.

## <a name="next-steps"></a>Sonraki adımlar
[Bir uygulamayı paketle][a4] ve dağıtmaya hazır hale getirin.

[Uygulamaları dağıtın ve kaldırın.][a5] Bu makalede, uygulama örneklerini yönetmek için PowerShell nasıl kullanılacağı açıklanmaktadır.

<!--Image references-->
[node-view-one]: ./media/service-fabric-hosting-model/node-view-one.png
[node-view-two]: ./media/service-fabric-hosting-model/node-view-two.png
[node-view-three]: ./media/service-fabric-hosting-model/node-view-three.png
[node-view-four]: ./media/service-fabric-hosting-model/node-view-four.png
[node-view-five]: ./media/service-fabric-hosting-model/node-view-five.png
[node-view-six]: ./media/service-fabric-hosting-model/node-view-six.png

<!--Link references--In actual articles, you only need a single period before the slash-->
[a1]: service-fabric-application-model.md
[a2]: service-fabric-guest-executables-introduction.md
[a3]: service-fabric-containers-overview.md
[a4]: service-fabric-package-apps.md
[a5]: service-fabric-deploy-remove-applications.md

[r1]: https://docs.microsoft.com/rest/api/servicefabric/sfclient-api-createservice

[c1]: https://docs.microsoft.com/dotnet/api/system.fabric.fabricclient.servicemanagementclient.createserviceasync
[c2]: https://docs.microsoft.com/dotnet/api/system.fabric.description.statelessservicedescription.instancecount

[p1]: https://docs.microsoft.com/powershell/module/servicefabric/new-servicefabricservice
[p2]: https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricservicedescription
[p3]: https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedservicePackage
[p4]: https://docs.microsoft.com/powershell/module/servicefabric/send-servicefabricdeployedservicepackagehealthreport
[p5]: https://docs.microsoft.com/powershell/module/servicefabric/restart-servicefabricdeployedcodepackage
[p6]: https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedservicetype
[p7]: https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedreplica
[p8]: https://docs.microsoft.com/powershell/module/servicefabric/get-servicefabricdeployedcodepackage
