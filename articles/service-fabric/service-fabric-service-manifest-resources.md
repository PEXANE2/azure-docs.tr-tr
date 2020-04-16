---
title: Servis Kumaşı hizmet bitiş noktalarını belirtme
description: HTTPS uç noktalarının nasıl ayarlanılmayı içeren bir hizmet bildiriminde uç nokta kaynakları nasıl açıklayınız?
ms.topic: conceptual
ms.date: 2/23/2018
ms.openlocfilehash: 88e71d15829e68bde635f5b4d40224b8fa914f40
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81417586"
---
# <a name="specify-resources-in-a-service-manifest"></a>Hizmet bildiriminde kaynakları belirtin
## <a name="overview"></a>Genel Bakış
Hizmet bildirimi, hizmet tarafından kullanılan kaynakların derlenen kodu değiştirmeden beyan edilmesine veya değiştirilmesine izin verir. Service Fabric, hizmet için uç nokta kaynaklarının yapılandırmasını destekler. Hizmet bildiriminde belirtilen kaynaklara erişim, uygulama bildirimindeki SecurityGroup aracılığıyla denetlenebilir. Kaynakların bildirimi, bu kaynakların dağıtım sırasında değiştirilmesine izin verir, bu da hizmetin yeni bir yapılandırma mekanizması getirmesi gerekmediği anlamına gelir. ServiceManifest.xml dosyası için şema tanımı Hizmet Kumaş SDK ve *c:\Program Files\Microsoft SDKs\Service Fabric\schemas\ServiceFabricServiceModel.xsd*araçları ile yüklenir.

## <a name="endpoints"></a>Uç Noktalar
Hizmet bildiriminde bir uç nokta kaynağı tanımlandığında, hizmet dokusu, bağlantı noktası açıkça belirtilmediğinde ayrılmış uygulama bağlantı noktası aralığından bağlantı noktaları atar. Örneğin, bu paragraftan sonra sağlanan bildirim parçacığında belirtilen son nokta *ServiceEndpoint1'e* bakın. Ayrıca, hizmetler bir kaynakta belirli bir bağlantı noktası da isteyebilir. Farklı küme düğümleri üzerinde çalışan hizmet yinelemelerine farklı bağlantı noktası numaraları atanırken, aynı düğümüzerinde çalışan bir hizmetin yinelemeleri bağlantı noktasını paylaşır. Hizmet yinelemeleri daha sonra çoğaltma ve istemci isteklerini dinlemek için gerektiğinde bu bağlantı noktalarını kullanabilir.

Bir https bitiş noktası belirten bir hizmeti etkinleştirdikten sonra, Service Fabric bağlantı noktasının erişim denetim girişini ayarlar, belirtilen sunucu sertifikasını bağlantı noktasına bağlar ve ayrıca hizmetin sertifikanın özel anahtarına izin olarak çalıştığını gösteren kimliği verir. Etkinleştirme akışı, Hizmet Kumaşı her başlatıldığında veya bir yükseltme yoluyla uygulamanın sertifika bildirimi değiştirildiğinde çağrılır. Bitiş noktası sertifikası da değişiklikler/yenilemeler için izlenir ve izinler gerektiğinde periyodik olarak yeniden uygulanır.

Hizmetin sona ermesi üzerine, Service Fabric bitiş noktası erişim denetimi girişini temizler ve sertifika bağlamayı kaldırır. Ancak, sertifikanın özel anahtarına uygulanan izinler temizlenmez.

> [!WARNING] 
> Tasarım gereği statik bağlantı noktaları ClusterManifest'te belirtilen uygulama bağlantı noktası aralığıyla çakışmamalıdır. Statik bir bağlantı noktası belirtirseniz, uygulama bağlantı noktası aralığının dışına atarsanız, aksi takdirde bağlantı noktası çakışmaları neden olur. Sürüm 6.5CU2 ile böyle bir çakışma tespit ettiğimizde bir **Sağlık Uyarısı** yayınlayacağız, ancak dağıtımın sevk edilen 6,5 davranışıyla senkronize olarak devam etmesine izin veririz. Ancak, uygulama dağıtımını sonraki büyük sürümlerden engelleyebiliriz.
>
> Sürüm 7.0 ile uygulama bağlantı noktası aralığı kullanımı nın HostingConfig ötesine geçtiğini tespit ettiğimizde bir **Sağlık Uyarısı** yayınlayacaktır::ApplicationPortExhaustThresholdPercentage (varsayılan %80).
>

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
    <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
    <Endpoint Name="ServiceEndpoint3" Protocol="https"/>
  </Endpoints>
</Resources>
```

Tek bir hizmet paketinde birden çok kod paketi varsa, kod paketinin **Bitiş Noktaları** bölümünde de başvurulması gerekir.  Örneğin, **ServiceEndpoint2a** ve **ServiceEndpoint2b** farklı kod paketlerine başvuran aynı hizmet paketinin uç noktalarıysa, her uç noktaya karşılık gelen kod paketi aşağıdaki gibi açıklığa kavuşturulur:

```xml
<Resources>
  <Endpoints>
    <Endpoint Name="ServiceEndpoint2a" Protocol="http" Port="802" CodePackageRef="Code1"/>
    <Endpoint Name="ServiceEndpoint2b" Protocol="http" Port="801" CodePackageRef="Code2"/>
  </Endpoints>
</Resources>
```

Config paket ayarları dosyasından (settings.xml) uç noktalarına başvuru hakkında daha fazla bilgi için [durumlu Güvenilir Hizmetleri Yapılandırmaya](service-fabric-reliable-services-configuration.md) bakın.

## <a name="example-specifying-an-http-endpoint-for-your-service"></a>Örnek: hizmetiniz için bir HTTP bitiş noktası belirtme
Aşağıdaki hizmet bildirimi, &lt;Kaynaklar&gt; öğesinde bir TCP uç noktası kaynağı ve iki HTTP uç noktası kaynağı tanımlar.

HTTP uç noktaları servis kumaşı tarafından otomatik olarak ACL'd'dir.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="Stateful1Pkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         This name must match the string used in the RegisterServiceType call in Program.cs. -->
    <StatefulServiceType ServiceTypeName="Stateful1Type" HasPersistedState="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>Stateful1.exe</Program>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is the contents of the Config directory under PackageRoot that contains an
       independently updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port number on which to
           listen. Note that if your service is partitioned, this port is shared with
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="ServiceEndpoint1" Protocol="http"/>
      <Endpoint Name="ServiceEndpoint2" Protocol="http" Port="80"/>
      <Endpoint Name="ServiceEndpoint3" Protocol="https"/>
      <Endpoint Name="ServiceEndpoint4" Protocol="https" Port="14023"/>

      <!-- This endpoint is used by the replicator for replicating the state of your service.
           This endpoint is configured through the ReplicatorSettings config section in the Settings.xml
           file under the ConfigPackage. -->
      <Endpoint Name="ReplicatorEndpoint" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="example-specifying-an-https-endpoint-for-your-service"></a>Örnek: hizmetiniz için bir HTTPS bitiş noktası belirtme
HTTPS protokolü sunucu kimlik doğrulaması sağlar ve istemci-sunucu iletişimini şifrelemek için de kullanılır. Hizmet Kumaşı hizmetinizde HTTPS'yi etkinleştirmek için, hizmet bildiriminin *Kaynaklar -> Bitiş Noktaları -> Bitiş Noktası* bölümünde, son nokta *ServiceEndpoint3*için daha önce gösterildiği gibi protokolü belirtin.

> [!NOTE]
> Uygulama yükseltmesırasında bir hizmetin protokolü değiştirilemez. Yükseltme sırasında değiştirilirse, bu bir kesme değişikliğidir.
> 

> [!WARNING] 
> HTTPS kullanırken, aynı düğüme dağıtılan farklı hizmet örnekleri (uygulamadan bağımsız) için aynı bağlantı noktasını ve sertifikayı kullanmayın. Farklı uygulama örneklerinde aynı bağlantı noktasını kullanarak iki farklı hizmetin yükseltilmesi bir yükseltme hatasına neden olur. Daha fazla bilgi için bkz: [HTTPS uç noktalarıyla birden çok uygulamayı yükseltme. ](service-fabric-application-upgrade.md#upgrading-multiple-applications-with-https-endpoints)
>

Burada bir HTTPS bitiş noktası için gerekli yapılandırmagösteren bir örnek ApplicationManifest olduğunu. Sunucu/bitiş noktası sertifikası parmak izi veya özortak adla beyan edilebilir ve bir değer sağlanmalıdır. EndpointRef ServiceManifest'teki EndpointResource'a bir başvurudur ve protokolü 'https' protokolüne ayarlanmış olmalıdır. Birden fazla EndpointCertificate ekleyebilirsiniz.  

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="Application1Type"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="Stateful1_MinReplicaSetSize" DefaultValue="3" />
    <Parameter Name="Stateful1_PartitionCount" DefaultValue="1" />
    <Parameter Name="Stateful1_TargetReplicaSetSize" DefaultValue="3" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion
       should match the Name and Version attributes of the ServiceManifest element defined in the
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateful1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <Policies>
      <EndpointBindingPolicy CertificateRef="SslCertByTP" EndpointRef="ServiceEndpoint3"/>
      <EndpointBindingPolicy CertificateRef="SslCertByCN" EndpointRef="ServiceEndpoint4"/>
    </Policies>
  </ServiceManifestImport>
  <DefaultServices>
    <!-- The section below creates instances of service types when an instance of this
         application type is created. You can also create one or more instances of service type by using the
         Service Fabric PowerShell module.

         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
    <Service Name="Stateful1">
      <StatefulService ServiceTypeName="Stateful1Type" TargetReplicaSetSize="[Stateful1_TargetReplicaSetSize]" MinReplicaSetSize="[Stateful1_ ]">
        <UniformInt64Partition PartitionCount="[Stateful1_PartitionCount]" LowKey="-9223372036854775808" HighKey="9223372036854775807" />
      </StatefulService>
    </Service>
  </DefaultServices>
  <Certificates>
    <EndpointCertificate Name="SslCertByTP" X509FindValue="FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF FF F0" X509StoreName="MY" />  
    <EndpointCertificate Name="SslCertByCN" X509FindType="FindBySubjectName" X509FindValue="ServiceFabric-EndpointCertificateBinding-Test" X509StoreName="MY" />  
  </Certificates>
</ApplicationManifest>
```

Linux kümeleri için **MY** mağazası varsayılan olarak **/var/lib/sfcerts**klasörüne aktarılır.


## <a name="overriding-endpoints-in-servicemanifestxml"></a>ServiceManifest.xml'de Uç Noktaları Geçersiz Kılma

ApplicationManifest'te ConfigOverrides bölümüne kardeş olacak bir ResourceOverrides bölümü ekleyin. Bu bölümde, Hizmet bildiriminde belirtilen kaynaklar bölümünde Uç Noktalar bölümü için geçersiz kılma belirtebilirsiniz. Geçersiz uç noktaları çalışma süresi 5.7.217/SDK 2.7.217 ve üzeri desteklenir.

ApplicationParametrelerini kullanarak ServiceManifest'te EndPoint'i geçersiz kılmak için ApplicationManifest'i aşağıdaki gibi değiştirin:

ServiceManifestImport bölümünde yeni bir bölüm "ResourceOveroverrides" ekleyin.

```xml
<ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="Stateless1Pkg" ServiceManifestVersion="1.0.0" />
    <ConfigOverrides />
    <ResourceOverrides>
      <Endpoints>
        <Endpoint Name="ServiceEndpoint" Port="[Port]" Protocol="[Protocol]" Type="[Type]" />
        <Endpoint Name="ServiceEndpoint1" Port="[Port1]" Protocol="[Protocol1] "/>
      </Endpoints>
    </ResourceOverrides>
        <Policies>
           <EndpointBindingPolicy CertificateRef="SslCertByTP" EndpointRef="ServiceEndpoint"/>
        </Policies>
  </ServiceManifestImport>
```

Aşağıdaki Parametrelerekle:

```xml
  <Parameters>
    <Parameter Name="Port" DefaultValue="" />
    <Parameter Name="Protocol" DefaultValue="" />
    <Parameter Name="Type" DefaultValue="" />
    <Parameter Name="Port1" DefaultValue="" />
    <Parameter Name="Protocol1" DefaultValue="" />
  </Parameters>
```

Uygulamayı dağıtırken bu değerleri ApplicationParameters olarak geçirebilirsiniz.  Örneğin:

```powershell
PS C:\> New-ServiceFabricApplication -ApplicationName fabric:/myapp -ApplicationTypeName "AppType" -ApplicationTypeVersion "1.0.0" -ApplicationParameter @{Port='1001'; Protocol='https'; Type='Input'; Port1='2001'; Protocol='http'}
```

Not: ApplicationParameters için sağlanan değerler boşsa, ilgili EndPointName için ServiceManifest'te sağlanan varsayılan değere geri döneriz.

Örneğin:

ServiceManifest'te belirttiğiniz

```xml
  <Resources>
    <Endpoints>
      <Endpoint Name="ServiceEndpoint1" Protocol="tcp"/>
    </Endpoints>
  </Resources>
```

Uygulama parametreleri için Port1 ve Protocol1 değeri boş veya boştur. Bağlantı noktası hala ServiceFabric tarafından kararlaştırıldı. Ve Protokol tcp olacaktır.

Yanlış bir değer belirttiğinizi varsayalım. Port için olduğu gibi bir int yerine "Foo" dize değeri belirtti.  Yeni HizmetFabricApplication komutu bir hata ile başarısız olur : 'ServiceEndpoint1' özniteliği 'Port1' bölümünde 'ResourceOverrides' geçersiz dir adı ile geçersizdir. Belirtilen değer 'Foo' ve gerekli 'int'.
