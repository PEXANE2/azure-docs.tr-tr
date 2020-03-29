---
title: Azure Hizmet Kumaşı uygulamalarını ve hizmetlerini tanımlama
description: Hizmet Kumaşı uygulamalarını ve hizmetlerini tanımlamak için bildirimlerin nasıl kullanıldığını açıklar.
ms.topic: conceptual
ms.date: 8/12/2019
ms.openlocfilehash: 6014ef6a9b6ec810aafd5e5be96223b8ed92d576
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75349974"
---
# <a name="service-fabric-application-and-service-manifests"></a>Hizmet Kumaş uygulama ve hizmet bildirimleri
Bu makalede, Service Fabric uygulamalarının ve hizmetlerinin ApplicationManifest.xml ve ServiceManifest.xml dosyaları kullanılarak nasıl tanımlandığı ve sürüldüğü açıklanmaktadır.  Daha ayrıntılı örnekler için [uygulama ve hizmet bildirimi örneklerine](service-fabric-manifest-examples.md)bakın.  Bu manifesto dosyaları için XML şema [ServiceFabricServiceModel.xsd şema belgelerde](service-fabric-service-model-schema.md)belgelenmiştir.

> [!WARNING]
> Manifest XML dosyası şeması, alt öğelerin doğru sıralanmasına neden olur.  Kısmi bir geçici çözüm olarak, Visual Studio'da Hizmet Kumaşı bildirimlerinden herhangi birini yazarken veya değiştirirken "C:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd" adresini açın. Bu, alt öğelerin sırasını kontrol etmek için izin verir ve intelli-anlamda sağlar.

## <a name="describe-a-service-in-servicemanifestxml"></a>ServiceManifest.xml'de bir hizmeti açıklayın
Hizmet bildirimi bildirimsel olarak hizmet türünü ve sürümünü tanımlar. Hizmet türü, sistem durumu özellikleri, yük dengeleme ölçümleri, hizmet ikilileri ve yapılandırma dosyaları gibi hizmet meta verilerini belirtir.  Başka bir şekilde, bir veya daha fazla hizmet türünü desteklemek için bir hizmet paketi oluşturan kodu, yapılandırmayı ve veri paketlerini açıklar. Bir hizmet bildirimi, bağımsız olarak sürülebilen birden çok kod, yapılandırma ve veri paketi içerebilir. Burada [Oylama örnek uygulamanın](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart) ASP.NET Core web ön uç hizmeti için bir hizmet manifestosu (ve burada bazı [daha ayrıntılı örnekler:](service-fabric-manifest-examples.md)

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="VotingWebPkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType. 
         This name must match the string used in RegisterServiceType call in Program.cs. -->
    <StatelessServiceType ServiceTypeName="VotingWebType" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>VotingWeb.exe</Program>
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is the contents of the Config directory under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="8080" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

**Sürüm** öznitelikleri yapılandırılmamış dizeleri ve sistem tarafından ayrıştırılmış değildir. Sürüm öznitelikleri yükseltmeleri için her bileşeni sürüm için kullanılır.

**ServiceTypes,** bu bildirimde hangi hizmet türlerinin **CodePackages** tarafından desteklenedildiğini bildirir. Bir hizmet bu hizmet türlerinden birine karşı anında olduğunda, bu bildirimde beyan edilen tüm kod paketleri giriş noktaları çalıştırılarak etkinleştirilir. Elde edilen işlemlerin, desteklenen hizmet türlerini çalışma zamanında kaydetmesi beklenir. Hizmet türleri, kod paketi düzeyinde değil, bildirim düzeyinde bildirilir. Bu nedenle, birden çok kod paketi olduğunda, sistem bildirilen hizmet türlerinden herhangi birini aradığında bunların tümü etkinleştirilir.

**EntryPoint** tarafından belirtilen yürütülebilir genellikle uzun süren hizmet ana bilgisayardır. **SetupEntryPoint,** başka bir giriş noktasından önce Hizmet Kumaşı (genellikle *LocalSystem* hesabı) ile aynı kimlik bilgileriyle çalışan ayrıcalıklı bir giriş noktasıdır.  Ayrı bir kurulum giriş noktasının varlığı, hizmet ana bilgisayarını uzun süreler boyunca yüksek ayrıcalıklarla çalıştırmak zorunda kalmamayı önler. **EntryPoint** tarafından belirtilen yürütülebilir **kurulumEntryPoint** çıktıktan sonra başarıyla çalıştırılır. İşlem sona ererse veya çökerse, ortaya çıkan işlem izlenir ve yeniden başlatılır **(SetupEntryPoint**ile yeniden başlar).  

**SetupEntryPoint'i** kullanmak için tipik senaryolar, hizmet başlamadan önce yürütülebilir bir çalıştırmayı çalıştırdığınız veya yüksek ayrıcalıklara sahip bir işlem gerçekleştirdiğiniz senaryolardır. Örnek:

* Hizmetin çalıştırılması gereken ortam değişkenlerini ayarlama ve başlatma. Bu yalnızca Service Fabric programlama modelleri aracılığıyla yazılmış yürütülebilir lerle sınırlı değildir. Örneğin, npm.exe bir düğüm.js uygulaması dağıtmak için yapılandırılan bazı ortam değişkenleri gerekir.
* Güvenlik sertifikaları yükleyerek erişim denetimini ayarlama.

SetupEntryPoint'in nasıl yapılandırılabildiğini hakkında daha fazla bilgi için [bkz.](service-fabric-application-runas-security.md)

**EnvironmentVariables** (önceki örnekte ayarlanmaz) bu kod paketi için ayarlanan ortam değişkenlerinin bir listesini sağlar. Ortam değişkenleri, farklı hizmet `ApplicationManifest.xml` örnekleri için farklı değerler sağlamak için geçersiz kılınabilir. 

**DataPackage** (önceki örnekte ayarlanmaz) çalışma zamanında işlem tarafından tüketilecek rasgele statik veriler içeren **Ad** özniteliği tarafından adlandırılan bir klasör bildirir.

**ConfigPackage,** **Ad** özniteliği tarafından adlandırılan ve *Ayarlar.xml* dosyası içeren bir klasör bildirir. Ayarlar dosyası, işlemin çalışma zamanında tekrar okuduğu kullanıcı tanımlı, anahtar değeri çifti ayarlarının bölümlerini içerir. Yükseltme sırasında, yalnızca **ConfigPackage** **sürümü** değişmişse, çalıştırma işlemi yeniden başlatılamaz. Bunun yerine, geri arama, yapılandırma ayarlarının dinamik olarak yeniden yüklenebilmeleri için işlemin değiştiğini gösterir. Burada örnek *settings.xml* dosyası:

```xml
<Settings xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Section Name="MyConfigurationSection">
    <Parameter Name="MySettingA" Value="Example1" />
    <Parameter Name="MySettingB" Value="Example2" />
  </Section>
</Settings>
```

Service Fabric Service **Endpoint,** Hizmet Kumaşı Kaynağının bir örneğidir. Bir Hizmet Kumaş Kaynağı derlenen kodu değiştirmeden beyan edilebilir/değiştirilebilir. Hizmet bildiriminde belirtilen Hizmet Dokusu Kaynaklarına erişim, uygulama bildirimindeki **SecurityGroup** aracılığıyla denetlenebilir. Hizmet bildiriminde bir Bitiş Noktası Kaynağı tanımlandığında, hizmet dokusu, bağlantı noktası açıkça belirtilmediğinde ayrılmış uygulama bağlantı noktası aralığından bağlantı noktaları atar. [Uç nokta kaynaklarını belirtme veya geçersiz kılma](service-fabric-service-manifest-resources.md)hakkında daha fazla bilgi edinin.

 
> [!WARNING]
> Tasarım gereği statik bağlantı noktaları ClusterManifest'te belirtilen uygulama bağlantı noktası aralığıyla çakışmamalıdır. Statik bir bağlantı noktası belirtirseniz, uygulama bağlantı noktası aralığının dışına atarsanız, aksi takdirde bağlantı noktası çakışmaları neden olur. Sürüm 6.5CU2 ile böyle bir çakışma tespit ettiğimizde bir **Sağlık Uyarısı** yayınlayacağız, ancak dağıtımın sevk edilen 6,5 davranışıyla senkronize olarak devam etmesine izin veririz. Ancak, uygulama dağıtımını sonraki büyük sürümlerden engelleyebiliriz.
>

<!--
For more information about other features supported by service manifests, refer to the following articles:

*TODO: LoadMetrics, PlacementConstraints, ServicePlacementPolicies
*TODO: Health properties
*TODO: Trace filters
*TODO: Configuration overrides
-->

## <a name="describe-an-application-in-applicationmanifestxml"></a>ApplicationManifest.xml'de bir uygulamayı açıklayın
Uygulama bildirimi bildirimsel olarak uygulama türü ve sürümünü açıklar. Kararlı adlar, bölümleme düzeni, örnek sayısı/çoğaltma faktörü, güvenlik/yalıtım ilkesi, yerleşim kısıtlamaları, yapılandırma geçersiz kılmaları ve kurucu hizmet türleri gibi hizmet bileşimi meta verilerini belirtir. Uygulamanın yerleştirildiği yük dengeleme etki alanları da açıklanmıştır.

Bu nedenle, bir uygulama bildirimi uygulama düzeyindeöğeleri açıklar ve bir veya daha fazla hizmet bildirimi bir uygulama türü oluşturmak için başvurur. İşte [Oylama örnek uygulama](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart) için uygulama bildirimi (ve burada bazı daha ayrıntılı [örnekler:](service-fabric-manifest-examples.md)

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VotingType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="VotingData_MinReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingData_PartitionCount" DefaultValue="1" />
    <Parameter Name="VotingData_TargetReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingWeb_InstanceCount" DefaultValue="-1" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingDataPkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
  </ServiceManifestImport>
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingWebPkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="VotingData">
      <StatefulService ServiceTypeName="VotingDataType" TargetReplicaSetSize="[VotingData_TargetReplicaSetSize]" MinReplicaSetSize="[VotingData_MinReplicaSetSize]">
        <UniformInt64Partition PartitionCount="[VotingData_PartitionCount]" LowKey="0" HighKey="25" />
      </StatefulService>
    </Service>
    <Service Name="VotingWeb" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="VotingWebType" InstanceCount="[VotingWeb_InstanceCount]">
        <SingletonPartition />
         <PlacementConstraints>(NodeType==NodeType0)</PlacementConstraints
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

Hizmet bildirimleri gibi, **Sürüm** öznitelikleri yapılandırılmamış dizeleri ve sistem tarafından ayrıştırılmış değildir. Sürüm öznitelikleri, yükseltmeler için her bileşeni sürümlemek için de kullanılır.

**Parametreler,** uygulama bildirimi boyunca kullanılan parametreleri tanımlar. Bu parametrelerin değerleri, uygulama anında kullanıma hazır olduğunda sağlanabilir ve uygulama veya hizmet yapılandırma ayarlarını geçersiz kılabilir.  Uygulama anında değer değiştirilmezse varsayılan parametre değeri kullanılır. Tek tek ortamlar için farklı uygulama ve hizmet parametrelerini nasıl koruyacağınızı öğrenmek [için, birden çok ortam için uygulama parametrelerini yönetme'ye](service-fabric-manage-multiple-environment-app-configuration.md)bakın.

**ServiceManifestImport,** bu uygulama türünü oluşturan hizmet bildirimlerine başvurular içerir. Bir uygulama bildirimi birden çok hizmet bildirimi içeriak içerebilir ve her biri bağımsız olarak sürülebilir. İçe aktarılan hizmet bildirimleri, bu uygulama türünde hangi hizmet türlerinin geçerli olduğunu belirler. ServiceManifestImport içinde, ServiceManifest.xml dosyalarındaki Ayarlar.xml ve ortam değişkenlerindeki yapılandırma değerlerini geçersiz kılarsınız. Son nokta bağlama, güvenlik ve erişim ve paket paylaşımı için **ilkeler** (önceki örnekte ayarlanmaz) ve paket paylaşımı içe aktarılan hizmet bildirimlerinde ayarlanabilir.  Daha fazla bilgi için, [uygulamanız için güvenlik ilkelerini yapılandır'a](service-fabric-application-runas-security.md)bakın.

**DefaultServices,** bir uygulama bu uygulama türüne karşı anında oluşturulduğunda otomatik olarak oluşturulan hizmet örneklerini bildirir. Varsayılan hizmetler sadece bir kolaylık tır ve oluşturulduktan sonra her açıdan normal hizmetler gibi olur. Bunlar, uygulama örneğindeki diğer hizmetlerle birlikte yükseltilir ve kaldırılabilir. Bir uygulama bildirimi birden çok varsayılan hizmet içerebilir.

**Sertifikalar** (önceki örnekte ayarlanmaz) https uç [noktalarını ayarlamak](service-fabric-service-manifest-resources.md#example-specifying-an-https-endpoint-for-your-service) veya [uygulama bildirimindeki sırları şifrelemek](service-fabric-application-secret-management.md)için kullanılan sertifikaları bildirir.

**Yerleşim Kısıtlamaları,** hizmetlerin nerede çalışması gerektiğini tanımlayan ifadelerdir. Bu ifadeler, bir veya daha fazla düğüm özelliği için seçtiğiniz tek tek hizmetlere eklenir. Daha fazla bilgi için [Bkz. Yerleşim kısıtlamaları ve düğüm özelliği sözdizimi](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-resource-manager-cluster-description#placement-constraints-and-node-property-syntax)

**İlkeler** (önceki örnekte ayarlanmaz) günlük koleksiyonunu, [varsayılan run-as,](service-fabric-application-runas-security.md) [sağlık](service-fabric-health-introduction.md#health-policies)ve hizmet(ler) Hizmet Kumaşı çalışma süresine erişimi olup olmadığını da dahil olmak üzere uygulama düzeyinde ayarlanması gereken [güvenlik erişim](service-fabric-application-runas-security.md) ilkelerini açıklar.

> [!NOTE] 
> Varsayılan olarak, Service Fabric uygulamaları, uygulamaya özgü istekleri kabul eden bir uç nokta ve Kumaş ve uygulamaya özgü dosyalar içeren ana bilgisayardaki dosya yollarını işaret eden ortam değişkenleri şeklinde Hizmet Kumaşı çalışma süresine erişebilir . Uygulama güvenilmeyen kodu barındırdığında (yani sahibi nin kimliği bilinmeyen veya uygulama sahibinin yürütülmesi için güvenli olmadığını bildiği kod) bu erişimi devre dışı bırakmayı düşünün. Daha fazla bilgi için, [Service Fabric güvenlik en iyi uygulamaları](service-fabric-best-practices-security.md#platform-isolation)bakın. 
>

**İlkeler** (önceki örnekte ayarlanmaz) hizmetleri çalıştırmak ve hizmet kaynaklarını [güvenli](service-fabric-application-runas-security.md)hale getirmek için gereken güvenlik ilkelerini (kullanıcılar veya gruplar) açıklar.  İlkeler **bölümlerinde** ilkelere başvurulmaktadır.





<!--
For more information about other features supported by application manifests, refer to the following articles:

*TODO: Security, Principals, RunAs, SecurityAccessPolicy
*TODO: Service Templates
-->



## <a name="next-steps"></a>Sonraki adımlar
- [Bir uygulamayı paketle](service-fabric-package-apps.md) ve dağıtmaya hazır hale getirin.
- [Uygulamaları dağıtın ve kaldırın.](service-fabric-deploy-remove-applications.md)
- [Parametreleri ve ortam değişkenlerini farklı uygulama örnekleri için yapılandırın.](service-fabric-manage-multiple-environment-app-configuration.md)
- [Uygulamanız için güvenlik ilkelerini yapılandırın.](service-fabric-application-runas-security.md)
- [Kurulum HTTPS uç noktaları](service-fabric-service-manifest-resources.md#example-specifying-an-https-endpoint-for-your-service).
- [Uygulama bildirimindeki sırları şifreleme](service-fabric-application-secret-management.md)

<!--Image references-->
[appmodel-diagram]: ./media/service-fabric-application-model/application-model.png
[cluster-imagestore-apptypes]: ./media/service-fabric-application-model/cluster-imagestore-apptypes.png
[cluster-application-instances]: media/service-fabric-application-model/cluster-application-instances.png



