---
title: Azure Hizmet Kumaş konteyner uygulama manifesto örnekleri
description: Çok konteynerli Service Fabric uygulaması için uygulama ve hizmet bildirimi ayarlarını nasıl yapılandırıştırmayı öğrenin.
author: peterpogorski
ms.topic: conceptual
ms.date: 06/08/2018
ms.author: pepogors
ms.openlocfilehash: 5efdbd17db20e69ace33b379ddbb99b2c4a20e69
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79258401"
---
# <a name="multi-container-application-and-service-manifest-examples"></a>Birden çok kapsayıcılı uygulama ve hizmet bildirimi örnekleri
Aşağıda, çok konteynerli Service Fabric uygulaması için uygulama ve hizmet bildirimlerine örnekler verilmiştir. Bu örneklerin amacı, hangi ayarların kullanılabilip kullanılacağını göstermektir. Bu uygulama ve hizmet bildirimleri [Windows Server 2016 Kapsayıcı Örnek](https://github.com/Azure-Samples/service-fabric-containers/tree/master/Windows) bildirimlerine dayanır.

Aşağıdaki özellikler gösterilmiştir:

|Bildirim|Özellikler|
|---|---|
|[Uygulama bildirimi](#application-manifest)| [geçersiz kılma ortamı değişkenleri,](service-fabric-get-started-containers.md#configure-and-set-environment-variables) [yapılandırma kapsayıcı bağlantı noktası-to-host eşleme,](service-fabric-get-started-containers.md#configure-container-port-to-host-port-mapping-and-container-to-container-discovery) [yapılandırma kapsayıcı kayıt defteri kimlik doğrulaması,](service-fabric-get-started-containers.md#configure-container-repository-authentication) [kaynak yönetişim,](service-fabric-resource-governance.md) [küme yalıtım modu,](service-fabric-get-started-containers.md#configure-isolation-mode) [işletim sistemi yapıya özgü kapsayıcı görüntüleri belirtin](service-fabric-get-started-containers.md#specify-os-build-specific-container-images)| 
|[FrontEndService hizmet bildirimi](#frontendservice-service-manifest)| [ayar çevre değişkenleri](service-fabric-get-started-containers.md#configure-and-set-environment-variables), [bir bitiş noktası yapılandırma](service-fabric-get-started-containers.md#configure-communication), kapsayıcı komutları geçmek, bir kapsayıcı içine bir sertifika [alma](service-fabric-securing-containers.md)| 
|[BackEndService hizmet bildirimi](#backendservice-service-manifest)|[ayar ortamı değişkenleri,](service-fabric-get-started-containers.md#configure-and-set-environment-variables) [bir bitiş noktası yapılandırma,](service-fabric-get-started-containers.md#configure-communication) [birim sürücüsü yapılandırma](service-fabric-containers-volume-logging-drivers.md)| 

Bkz. [Uygulama bildirimi öğeleri,](#application-manifest-elements) [FrontEndService hizmet manifestosu öğeleri](#frontendservice-service-manifest-elements)ve Belirli XML öğeleri hakkında daha fazla bilgi için [BackEndService hizmet bildirimi öğeleri.](#backendservice-service-manifest-elements)

## <a name="application-manifest"></a>Uygulama bildirimi

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest ApplicationTypeName="Container.ApplicationType"
                     ApplicationTypeVersion="1.0.0"
                     xmlns="http://schemas.microsoft.com/2011/01/fabric"
                     xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                     xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <Parameters>
    <Parameter Name="BackEndService_InstanceCount" DefaultValue="-1" />
    <Parameter Name="FrontEndService_InstanceCount" DefaultValue="-1" />
    <Parameter Name="CpuCores" DefaultValue="2" />
    <Parameter Name="BlockIOWeight" DefaultValue="200" />
    <Parameter Name="MaximumIOBandwidth" DefaultValue="1024" />
    <Parameter Name="MemoryReservationInMB" DefaultValue="1024" />
    <Parameter Name="MemorySwapInMB" DefaultValue="4084"/>
    <Parameter Name="MaximumIOps" DefaultValue="20"/>
    <Parameter Name="MemoryFront" DefaultValue="4084" />
    <Parameter Name="MemoryBack" DefaultValue="2048" />
    <Parameter Name="CertThumbprint" DefaultValue=""/>
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="BackEndServicePkg" ServiceManifestVersion="1.0.0" />    
    
    <!-- Policies to be applied to the imported service manifest. -->
    <Policies>
      <!-- Set resource governance at the service package level. -->
      <ServicePackageResourceGovernancePolicy CpuCores="[CpuCores]" MemoryInMB="[MemoryFront]"/>

      <!-- Set resource governance at the code package level. -->
      <ResourceGovernancePolicy CodePackageRef="Code" CpuPercent="10" MemoryInMB="[MemoryFront]" BlockIOWeight="[BlockIOWeight]" MaximumIOBandwidth="[MaximumIOBandwidth]" MaximumIOps="[MaximumIOps]" MemoryReservationInMB="[MemoryReservationInMB]" MemorySwapInMB="[MemorySwapInMB]"/>
      
      <!-- Policies for activating container hosts. -->
      <ContainerHostPolicies CodePackageRef="Code" Isolation="process">
        
        <!-- Credentials for the repository hosting the container image.-->
        <RepositoryCredentials AccountName="sfsamples" Password="ENCRYPTED-PASSWORD" PasswordEncrypted="true"/>
        
        <!-- This binds the port the container is listening on (8905 in this sample) to an endpoint resource named "BackEndServiceTypeEndpoint", which is defined in the service manifest.  -->
        <PortBinding ContainerPort="8905" EndpointRef="BackEndServiceTypeEndpoint"/>
        
        <!-- Configure the Azure Files volume plugin.  Bind the source folder on the host VM or a remote share to the destination folder within the running container. -->
        <Volume Source="azfiles" Destination="c:\VolumeTest\Data" Driver="sfazurefile">
          <!-- Driver options to be passed to driver. The Azure Files volume plugin supports the following driver options:
            shareName (the Azure Files file share that provides the volume for the container), storageAccountName (the Azure storage account
            that contains the Azure Files file share), storageAccountKey (Access key for the Azure storage account that contains the Azure Files file share).
            These three driver options are required. -->
          <DriverOption Name="shareName" Value="" />
          <DriverOption Name="storageAccountName" Value="MY-STORAGE-ACCOUNT-NAME" />
          <DriverOption Name="storageAccountKey" Value="MY-STORAGE-ACCOUNT-KEY" />
        </Volume>
        
        <!-- Windows Server containers may not be compatible across different versions of the OS.  You can specify multiple OS images per container and tag 
        them with the build versions of the OS. Get the build version of the OS by running "winver" at a Windows command prompt. -->
        <ImageOverrides>
          <!-- If the underlying OS is build version 16299 (Windows Server version 1709), Service Fabric picks the container image tagged with Os="16299". -->
          <Image Name="sfsamples.azurecr.io/sfsamples/servicefabricbackendservice_1709" Os="16299" />
          
          <!-- An untagged container image is assumed to work across all versions of the OS and overrides the image specified in the service manifest. -->
          <Image Name="sfsamples.azurecr.io/sfsamples/servicefabricbackendservice_default" />          
        </ImageOverrides>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>

  <!-- Policies to be applied to the imported service manifest. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="FrontEndServicePkg" ServiceManifestVersion="1.0.0" />
    
    <!-- This enables you to provide different values for environment variables when creating a FrontEndService
         Theses environment variables are declared in the FrontEndServiceType service manifest-->
    <EnvironmentOverrides CodePackageRef="Code">
      <EnvironmentVariable Name="BackendServiceName" Value="Container.Application/BackEndService"/>
      <EnvironmentVariable Name="HttpGatewayPort" Value="19080"/>
      <EnvironmentVariable Name="IsContainer" Value="true"/>
    </EnvironmentOverrides>
    
    <!-- This policy maps the  port of the container (80) to the endpoint declared in the service, 
         FrontEndServiceTypeEndpoint which is exposed as port 80 on the host-->    
    <Policies>

      <!-- Set resource governance at the service package level. -->
      <ServicePackageResourceGovernancePolicy CpuCores="[CpuCores]" MemoryInMB="[MemoryBack]"/>

      <!-- Policies for activating container hosts. -->
      <ContainerHostPolicies CodePackageRef="Code" Isolation="process">

        <!-- Credentials for the repository hosting the container image.-->
        <RepositoryCredentials AccountName="sfsamples" Password="ENCRYPTED-PASSWORD" PasswordEncrypted="true"/>

        <!-- Binds an endpoint resource (declared in the service manifest) to the exposed container port. -->
        <PortBinding ContainerPort="80" EndpointRef="FrontEndServiceTypeEndpoint"/>

        <!-- Import a certificate into the container.  The certificate must be installed in the LocalMachine store of all the cluster nodes.
          When the application starts, the runtime reads the certificate and generates a PFX file and password (on Windows) or a PEM file (on Linux).
          The PFX file and password are accessible in the container using the Certificates_ServicePackageName_CodePackageName_CertName_PFX and 
          Certificates_ServicePackageName_CodePackageName_CertName_Password environment variables. The PEM file is accessible in the container using the 
          Certificates_ServicePackageName_CodePackageName_CertName_PEM and Certificates_ServicePackageName_CodePackageName_CertName_PrivateKey environment variables.-->
        <CertificateRef Name="MyCert1" X509StoreName="My" X509FindValue="[CertThumbprint]" />

        <!-- If the certificate is already in PFX or PEM form, you can create a data package inside your application and reference that certificate here. -->
        <CertificateRef Name="MyCert2" DataPackageRef="Data" DataPackageVersion="1.0.0" RelativePath="MyCert2.PFX" Password="ENCRYPTED-PASSWORD" IsPasswordEncrypted="true"/>
      </ContainerHostPolicies>
    </Policies>
  </ServiceManifestImport>
  
  <DefaultServices>
    <!-- The section below creates instances of service types, when an instance of this 
         application type is created. You can also create one or more instances of service type using the 
         ServiceFabric PowerShell module.
         
         The attribute ServiceTypeName below must match the name defined in the imported ServiceManifest.xml file. -->
        
    <Service Name="FrontEndService" >
      <StatelessService ServiceTypeName="FrontEndServiceType" InstanceCount="[FrontEndService_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
        <Service Name="BackEndService" ServicePackageActivationMode="ExclusiveProcess">
      <StatelessService ServiceTypeName="BackEndServiceType" InstanceCount="[BackEndService_InstanceCount]">
        <SingletonPartition />
      </StatelessService>
    </Service>
  </DefaultServices>
</ApplicationManifest>
```

## <a name="frontendservice-service-manifest"></a>FrontEndService hizmet bildirimi

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="FrontEndServicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="FrontEndServiceType" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ContainerHost>
        <!--The repo and image on https://hub.docker.com or Azure Container Registry. -->
        <ImageName>sfsamples.azurecr.io/sfsamples/servicefabricfrontendservice:v1</ImageName>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container or exe.  These variables are overridden in the application manifest. -->
    <EnvironmentVariables>
      <EnvironmentVariable Name="BackendServiceName" Value=""/>
      <EnvironmentVariable Name="HttpGatewayPort" Value=""/>
      <EnvironmentVariable Name="IsContainer" Value=""/>
    </EnvironmentVariables>
  </CodePackage>

  <!-- Config package is the contents of the Config directory under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />
  
  <!-- Data package is the contents of the Data directory under PackageRoot that contains an 
       independently-updateable and versioned static data that's consumed by the process at runtime. -->
  <DataPackage Name="Data" Version="1.0.0"/>

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the port on which to 
           listen. For a guest executable is used to register with the NamingService at its REST endpoint
           with http scheme and port 80 -->
      <Endpoint Name="FrontEndServiceTypeEndpoint" UriScheme="http" Port="80"/>
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="backendservice-service-manifest"></a>BackEndService hizmet bildirimi

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="BackEndServicePkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType.
         The UseImplicitHost attribute indicates this is a guest service. -->
    <StatelessServiceType ServiceTypeName="BackEndServiceType" UseImplicitHost="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ContainerHost>
        <!--The repo and image on https://hub.docker.com or Azure Container Registry. -->
        <ImageName>sfsamples.azurecr.io/sfsamples/servicefabricbackendservice:v1</ImageName>
        
        <!-- Pass comma delimited commands to your container. -->
        <Commands> dotnet, myproc.dll, 5 </Commands>
      </ContainerHost>
    </EntryPoint>
    <!-- Pass environment variables to your container. These variables are overridden in the application manifest. -->
    <EnvironmentVariables>
      <EnvironmentVariable Name="IsContainer" Value="true"/>
    </EnvironmentVariables>
  </CodePackage>

  <!-- Config package is the contents of the Config directory under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- This endpoint is used by the communication listener to obtain the host port on which to 
           listen. For a guest executable is used to register with the NamingService at its REST endpoint
           with http scheme. In this case since no port is specified, one is created and assigned dynamically
           to the service. This dynamically assigned host port is mapped to the container port (8905 in this sample),
            which was specified in the application manifest.-->
      <Endpoint Name="BackEndServiceTypeEndpoint" UriScheme="http" />
    </Endpoints>
  </Resources>
</ServiceManifest>
```

## <a name="application-manifest-elements"></a>Uygulama bildirim öğeleri
### <a name="applicationmanifest-element"></a>UygulamaManifesto Öğesi
Bildirimsel olarak uygulama türünü ve sürümünü açıklar. Bir veya daha fazla hizmet bildirimi, bir uygulama türü oluşturmak için başvurulan. Kurucu hizmetlerin yapılandırma ayarları parametreli uygulama ayarları kullanılarak geçersiz kılınabilir. Varsayılan hizmetler, hizmet şablonları, ilkeler, ilkeler, tanılama kurulumu ve sertifikalar da uygulama düzeyinde bildirilebilir. Daha fazla bilgi için Bkz. [ApplicationManifest Öğesi](service-fabric-service-model-schema-elements.md#ApplicationManifestElementApplicationManifestTypeComplexType)

### <a name="parameters-element"></a>Parametreler Elemanı
Bu uygulama bildiriminde kullanılan parametreleri bildirir. Bu parametrelerin değeri, uygulama anında kullanıldığında sağlanabilir ve uygulama veya hizmet yapılandırma ayarlarını geçersiz kılmak için kullanılabilir. Daha fazla bilgi için [bkz.](service-fabric-service-model-schema-elements.md#ParametersElementanonymouscomplexTypeComplexTypeDefinedInApplicationManifestTypecomplexType)

### <a name="parameter-element"></a>Parameter Öğesi
Bu bildirimde kullanılacak bir uygulama parametresi. Parametre değeri uygulama anında değişiklik yapabilir veya değer sağlanmazsa varsayılan değer kullanılır. Daha fazla bilgi için [Bkz. Parametre Öğesi](service-fabric-service-model-schema-elements.md#ParameterElementanonymouscomplexTypeComplexTypeDefinedInParameterselement)

### <a name="servicemanifestimport-element"></a>ServiceManifestImport Elemanı
Hizmet geliştiricisi tarafından oluşturulan bir hizmet bildirimi içe aktarım. Uygulamadaki her kurucu hizmet için bir hizmet bildirimi içilmelidir. Yapılandırma geçersiz kılar ve ilkeler hizmet bildirimi için bildirilebilir. Daha fazla bilgi için [ServiceManifestImport Element'e](service-fabric-service-model-schema-elements.md#ServiceManifestImportElementanonymouscomplexTypeComplexTypeDefinedInApplicationManifestTypecomplexType) bakın

### <a name="servicemanifestref-element"></a>ServiceManifestRef Öğesi
Referans olarak hizmet bildirimini içeri akteder. Şu anda hizmet bildirimi dosyası (ServiceManifest.xml) yapı paketinde bulunmalıdır. Daha fazla bilgi için [ServiceManifestRef Öğesi'ne](service-fabric-service-model-schema-elements.md#ServiceManifestRefElementServiceManifestRefTypeComplexTypeDefinedInServiceManifestImportelement) bakın

### <a name="policies-element"></a>İlkeler Öğesi
İçe aktarılan hizmet bildiriminde uygulanacak ilkeleri (bitiş noktası bağlama, paket paylaşımı, run-as ve güvenlik erişimi) açıklar. Daha fazla bilgi için [Bkz. İlkeler Öğesi](service-fabric-service-model-schema-elements.md#PoliciesElementServiceManifestImportPoliciesTypeComplexTypeDefinedInServiceManifestImportelement)

### <a name="servicepackageresourcegovernancepolicy-element"></a>ServicePackageResourceGovernancePolicy Öğesi
Tüm hizmet paketi düzeyinde uygulanan kaynak yönetim ilkesini tanımlar. Daha fazla bilgi için [ServicePackageResourceGovernancePolicy Öğesi'ne](service-fabric-service-model-schema-elements.md#ServicePackageResourceGovernancePolicyElementServicePackageResourceGovernancePolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInServicePackageTypecomplexType) bakın

### <a name="resourcegovernancepolicy-element"></a>Kaynak YönetişimPolitikası Öğesi
Kod paketi için kaynak sınırlarını belirtir. Daha fazla bilgi için [KaynakYönetişimPolitikası Öğesi'ne](service-fabric-service-model-schema-elements.md#ResourceGovernancePolicyElementResourceGovernancePolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInDigestedCodePackageelementDefinedInDigestedEndpointelement) bakın

### <a name="containerhostpolicies-element"></a>ContainerHostPolicies Öğesi
Kapsayıcı ana bilgisayarlarını etkinleştirme ilkelerini belirtir. Daha fazla bilgi için [ContainerHostPolicies Öğesi'ne](service-fabric-service-model-schema-elements.md#ContainerHostPoliciesElementContainerHostPoliciesTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInDigestedCodePackageelement) bakın

### <a name="repositorycredentials-element"></a>Depo Kimlik Bilgileri Öğesi
Görüntüleri çekmek için kapsayıcı görüntü deposu için kimlik bilgileri. Daha fazla bilgi için [Bkz. Depo Kimlik Bilgileri Öğesi](service-fabric-service-model-schema-elements.md#RepositoryCredentialsElementRepositoryCredentialsTypeComplexTypeDefinedInContainerHostPoliciesTypecomplexType)

### <a name="portbinding-element"></a>PortBağlama Elemanı
Açıktaki kapsayıcı bağlantı noktasına bağlanabilir hangi uç nokta kaynağı belirtir. Daha fazla bilgi için Bkz. [PortBinding Öğesi](service-fabric-service-model-schema-elements.md#PortBindingElementPortBindingTypeComplexTypeDefinedInServicePackageContainerPolicyTypecomplexTypeDefinedInContainerHostPoliciesTypecomplexType)

### <a name="volume-element"></a>Hacim Öğesi
Kapsayıcıya bağlanacak sesi belirtir. Daha fazla bilgi için [Bkz. Birim Öğesi](service-fabric-service-model-schema-elements.md#VolumeElementContainerVolumeTypeComplexTypeDefinedInContainerHostPoliciesTypecomplexType)

### <a name="driveroption-element"></a>DriverOption Öğesi
Sürücü seçenekleri sürücüye geçirilecek. Daha fazla bilgi için Bkz. [DriverOption Öğesi](service-fabric-service-model-schema-elements.md#DriverOptionElementDriverOptionTypeComplexTypeDefinedInContainerLoggingDriverTypecomplexTypeDefinedInContainerVolumeTypecomplexType)

### <a name="imageoverrides-element"></a>ImageOverrides Öğesi
Windows Server kapsayıcıları işletim sistemi farklı sürümlerinde uyumlu olmayabilir.  Kapsayıcı başına birden çok işletim sistemi görüntüsü belirtebilir ve bunları işletim sistemi oluşturma sürümleriyle etiketleyebilirsiniz. Windows komut isteminde "winver" çalıştırarak işletim sistemi yapı sürümünü alın. Temel işletim sistemi build sürüm 16299 (Windows Server sürüm 1709) ise, Service Fabric Os="16299" ile etiketlenmiş kapsayıcı görüntüsünü seçer. Etiketlenmemiş bir kapsayıcı görüntüsünün işletim sistemi'nin tüm sürümlerinde çalıştığı varsayılır ve hizmet bildiriminde belirtilen görüntüyü geçersiz kılar. Daha fazla bilgi için [ImageOverrides Öğesi'ne](service-fabric-service-model-schema-elements.md#ImageOverridesElementImageOverridesTypeComplexTypeDefinedInContainerHostPoliciesTypecomplexType) bakın

### <a name="image-element"></a>Görüntü Öğesi
Başlatılacak işletim sistemi yapı sürüm numarasına karşılık gelen konteyner görüntüsü. Os özniteliği belirtilmemişse, kapsayıcı görüntüsünün işletim sistemi'nin tüm sürümlerinde çalıştığı varsayılır ve hizmet bildiriminde belirtilen görüntüyü geçersiz kılar. Daha fazla bilgi için [Resim Öğesi'ne](service-fabric-service-model-schema-elements.md#ImageElementImageTypeComplexTypeDefinedInImageOverridesTypecomplexType) bakın

### <a name="environmentoverrides-element"></a>EnvironmentOverrides Öğesi
 Daha fazla bilgi için [EnvironmentOverrides Öğesi'ne](service-fabric-service-model-schema-elements.md#EnvironmentOverridesElementEnvironmentOverridesTypeComplexTypeDefinedInServiceManifestImportelement) bakın

### <a name="environmentvariable-element"></a>ÇevreDeğişken Öğesi
Çevre değişkeni. Daha fazla bilgi için [Bkz. EnvironmentVariable Element](service-fabric-service-model-schema-elements.md#EnvironmentVariableElementEnvironmentVariableOverrideTypeComplexTypeDefinedInEnvironmentOverridesTypecomplexType)

### <a name="certificateref-element"></a>CertificateRef Öğesi
Kapsayıcı ortamına maruz kalması gereken bir X509 sertifikası hakkındaki bilgileri belirtir. Sertifika, tüm küme düğümlerinin LocalMachine deposuna yüklenmelidir.
Uygulama başladığında, çalışma zamanı sertifikayı okur ve bir PFX dosyası ve parola (Windows'da) veya pem dosyası (Linux'ta) oluşturur.
PFX dosyası na ve parolaya Certificates_ServicePackageName_CodePackageName_CertName_PFX ve Certificates_ServicePackageName_CodePackageName_CertName_Password ortam değişkenleri kullanılarak kapsayıcıda erişilebilir. PEM dosyasına Certificates_ServicePackageName_CodePackageName_CertName_PEM ve Certificates_ServicePackageName_CodePackageName_CertName_PrivateKey ortam değişkenleri kullanılarak kapsayıcıda erişilebilir. Daha fazla bilgi için [Bkz. CertificateRef Öğesi](service-fabric-service-model-schema-elements.md#CertificateRefElementContainerCertificateTypeComplexTypeDefinedInContainerHostPoliciesTypecomplexType)

### <a name="defaultservices-element"></a>Varsayılan Hizmetler Öğesi
Bir uygulama bu uygulama türüne karşı anında oluşturulduğunda otomatik olarak oluşturulan hizmet örneklerini bildirir. Daha fazla bilgi için [DefaultServices Öğesi'ne](service-fabric-service-model-schema-elements.md#DefaultServicesElementDefaultServicesTypeComplexTypeDefinedInApplicationManifestTypecomplexTypeDefinedInApplicationInstanceTypecomplexType) bakın

### <a name="service-element"></a>Hizmet Öğesi
Uygulama anında oluşturulduğunda otomatik olarak oluşturulacak bir hizmeti bildirir. Daha fazla bilgi için [Hizmet Öğesi'ne](service-fabric-service-model-schema-elements.md#ServiceElementanonymouscomplexTypeComplexTypeDefinedInDefaultServicesTypecomplexType) bakın

### <a name="statelessservice-element"></a>StatelessService Öğesi
Devletsiz bir hizmeti tanımlar. Daha fazla bilgi için [StatelessService Öğesi'ne](service-fabric-service-model-schema-elements.md#StatelessServiceElementStatelessServiceTypeComplexTypeDefinedInServiceTemplatesTypecomplexTypeDefinedInServiceelement) bakın


## <a name="frontendservice-service-manifest-elements"></a>FrontEndService hizmet bildirim öğeleri
### <a name="servicemanifest-element"></a>ServiceManifest Öğesi
Bildirimsel olarak hizmet türünü ve sürümünü açıklar. Bir veya daha fazla hizmet türünü desteklemek için bir hizmet paketi oluşturan bağımsız olarak yükseltilebilir kodu, yapılandırmayı ve veri paketlerini listeler. Hizmetler türü, sistem durumu özellikleri ve yük dengeleme ölçümleri gibi kaynaklar, tanılama ayarları ve hizmet meta verileri de belirtilir. Daha fazla bilgi için [ServiceManifest Öğesi'ne](service-fabric-service-model-schema-elements.md#ServiceManifestElementServiceManifestTypeComplexType) bakın

### <a name="servicetypes-element"></a>ServiceTypes Öğesi
Bu bildirimde hangi hizmet türlerinin CodePackage tarafından destekleniyi tanımlar. Bir hizmet bu hizmet türlerinden birine karşı anında olduğunda, bu bildirimde beyan edilen tüm kod paketleri giriş noktaları çalıştırılarak etkinleştirilir. Hizmet türleri, kod paketi düzeyinde değil, bildirim düzeyinde bildirilir. Daha fazla bilgi için [ServiceTypes Öğesi'ne](service-fabric-service-model-schema-elements.md#ServiceTypesElementServiceAndServiceGroupTypesTypeComplexTypeDefinedInServiceManifestTypecomplexType) bakın

### <a name="statelessservicetype-element"></a>StatelessServiceType Öğesi
Devletsiz hizmet türünü açıklar. Daha fazla bilgi için Bkz. [StatelessServiceType Öğesi](service-fabric-service-model-schema-elements.md#StatelessServiceTypeElementStatelessServiceTypeTypeComplexTypeDefinedInServiceAndServiceGroupTypesTypecomplexTypeDefinedInServiceTypesTypecomplexType)

### <a name="codepackage-element"></a>CodePackage Öğesi
Tanımlı hizmet türünü destekleyen bir kod paketini açıklar. Bir hizmet bu hizmet türlerinden birine karşı anında olduğunda, bu bildirimde beyan edilen tüm kod paketleri giriş noktaları çalıştırılarak etkinleştirilir. Elde edilen işlemlerin, desteklenen hizmet türlerini çalışma zamanında kaydetmesi beklenir. Birden çok kod paketi olduğunda, sistem bildirilen hizmet türlerinden herhangi birini aradığında bunların tümü etkinleştirilir. Daha fazla bilgi için [CodePackage Öğesi'ne](service-fabric-service-model-schema-elements.md#CodePackageElementCodePackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedCodePackageelement) bakın

### <a name="entrypoint-element"></a>EntryPoint Öğesi
EntryPoint tarafından belirtilen yürütülebilir genellikle uzun süren hizmet ana bilgisayardır. Ayrı bir kurulum giriş noktasının varlığı, hizmet ana bilgisayarını uzun süreler boyunca yüksek ayrıcalıklarla çalıştırmak zorunda kalmamayı önler. EntryPoint tarafından belirtilen yürütülebilir kurulumEntryPoint çıktıktan sonra başarıyla çalıştırılır. Ortaya çıkan işlem izlenir ve yeniden başlatılır (SetupEntryPoint ile yeniden başlar) eğer hiç sona erer veya çöker. Daha fazla bilgi için [EntryPoint Öğesi'ne](service-fabric-service-model-schema-elements.md#EntryPointElementEntryPointDescriptionTypeComplexTypeDefinedInCodePackageTypecomplexType) bakın

### <a name="containerhost-element"></a>ContainerHost Öğesi
 Daha fazla bilgi için [ContainerHost Öğesi'ne](service-fabric-service-model-schema-elements.md#ContainerHostElementContainerHostEntryPointTypeComplexTypeDefinedInEntryPointDescriptionTypecomplexType) bakın

### <a name="imagename-element"></a>ImageName Öğesi
Repo ve resim [https://hub.docker.com](https://hub.docker.com) veya Azure Kapsayıcı Kayıt Defteri. Daha fazla bilgi için [ImageName Öğesi'ne](service-fabric-service-model-schema-elements.md#ImageNameElementxs:stringComplexTypeDefinedInContainerHostEntryPointTypecomplexType) bakın

### <a name="environmentvariables-element"></a>ÇevreDeğişkenler Öğesi
Ortam değişkenlerini kapsayıcınıza veya exe'nize aktarın.  Daha fazla bilgi için [ÇevreDeğişkenler Öğesi'ne](service-fabric-service-model-schema-elements.md#EnvironmentVariablesElementEnvironmentVariablesTypeComplexTypeDefinedInCodePackageTypecomplexType) bakın

### <a name="environmentvariable-element"></a>ÇevreDeğişken Öğesi
Çevre değişkeni. Daha fazla bilgi için [Bkz. EnvironmentVariable Element](service-fabric-service-model-schema-elements.md#EnvironmentVariableElementEnvironmentVariableOverrideTypeComplexTypeDefinedInEnvironmentOverridesTypecomplexType)

### <a name="configpackage-element"></a>ConfigPackage Elemanı
Settings.xml dosyasını içeren Ad özniteliği tarafından adlandırılan bir klasör bildirir. Bu dosya, işlemin çalışma zamanında okuyabileceği kullanıcı tanımlı, anahtar değeri çifti ayarlarının bölümlerini içerir. Yükseltme sırasında, yalnızca ConfigPackage sürümü değişmişse, çalıştırma işlemi yeniden başlatılamaz. Bunun yerine, geri arama, yapılandırma ayarlarının dinamik olarak yeniden yüklenebilmeleri için işlemin değiştiğini gösterir. Daha fazla bilgi için [Bkz. ConfigPackage Element](service-fabric-service-model-schema-elements.md#ConfigPackageElementConfigPackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedConfigPackageelement)

### <a name="datapackage-element"></a>DataPackage Öğesi
Statik veri dosyalarını içeren Ad özniteliği tarafından adlandırılan bir klasör bildirir. Service Fabric, hizmet bildiriminde listelenen veri paketlerinden herhangi biri yükseltildiğinde ana bilgisayar ve destek paketlerinde belirtilen tüm EX'leri ve DLLHOST'ları geri dönüştürür. Daha fazla bilgi için [Bkz. DataPackage Öğesi](service-fabric-service-model-schema-elements.md#DataPackageElementDataPackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedDataPackageelement)

### <a name="resources-element"></a>Kaynaklar Öğesi
Derlenmiş kodu değiştirmeden bildirilebilen ve hizmet dağıtıldığında değiştirilebilen bu hizmet tarafından kullanılan kaynakları açıklar. Bu kaynaklara erişim, uygulama bildiriminin İlkeler ve İlkeler bölümleri aracılığıyla denetlenir. Daha fazla bilgi için [Kaynaklar Öğesi'ne](service-fabric-service-model-schema-elements.md#ResourcesElementResourcesTypeComplexTypeDefinedInServiceManifestTypecomplexType) bakın

### <a name="endpoints-element"></a>Uç Nokta Öğesi
Hizmet için uç noktaları tanımlar. Daha fazla bilgi için Bkz. [Uç Noktaları Öğesi](service-fabric-service-model-schema-elements.md#EndpointsElementanonymouscomplexTypeComplexTypeDefinedInResourcesTypecomplexType)

### <a name="endpoint-element"></a>Bitiş Noktası Öğesi
Daha fazla bilgi için [Bkz. Endpoint Öğesi](service-fabric-service-model-schema-elements.md#EndpointElementEndpointOverrideTypeComplexTypeDefinedInEndpointselement)


## <a name="backendservice-service-manifest-elements"></a>BackEndService hizmet bildirimi öğeleri
### <a name="servicemanifest-element"></a>ServiceManifest Öğesi
Bildirimsel olarak hizmet türünü ve sürümünü açıklar. Bir veya daha fazla hizmet türünü desteklemek için bir hizmet paketi oluşturan bağımsız olarak yükseltilebilir kodu, yapılandırmayı ve veri paketlerini listeler. Hizmetler türü, sistem durumu özellikleri ve yük dengeleme ölçümleri gibi kaynaklar, tanılama ayarları ve hizmet meta verileri de belirtilir. Daha fazla bilgi için [ServiceManifest Öğesi'ne](service-fabric-service-model-schema-elements.md#ServiceManifestElementServiceManifestTypeComplexType) bakın

### <a name="servicetypes-element"></a>ServiceTypes Öğesi
Bu bildirimde hangi hizmet türlerinin CodePackage tarafından destekleniyi tanımlar. Bir hizmet bu hizmet türlerinden birine karşı anında olduğunda, bu bildirimde beyan edilen tüm kod paketleri giriş noktaları çalıştırılarak etkinleştirilir. Hizmet türleri, kod paketi düzeyinde değil, bildirim düzeyinde bildirilir. Daha fazla bilgi için [ServiceTypes Öğesi'ne](service-fabric-service-model-schema-elements.md#ServiceTypesElementServiceAndServiceGroupTypesTypeComplexTypeDefinedInServiceManifestTypecomplexType) bakın

### <a name="statelessservicetype-element"></a>StatelessServiceType Öğesi
Devletsiz hizmet türünü açıklar. Daha fazla bilgi için Bkz. [StatelessServiceType Öğesi](service-fabric-service-model-schema-elements.md#StatelessServiceTypeElementStatelessServiceTypeTypeComplexTypeDefinedInServiceAndServiceGroupTypesTypecomplexTypeDefinedInServiceTypesTypecomplexType)

### <a name="codepackage-element"></a>CodePackage Öğesi
Tanımlı hizmet türünü destekleyen bir kod paketini açıklar. Bir hizmet bu hizmet türlerinden birine karşı anında olduğunda, bu bildirimde beyan edilen tüm kod paketleri giriş noktaları çalıştırılarak etkinleştirilir. Elde edilen işlemlerin, desteklenen hizmet türlerini çalışma zamanında kaydetmesi beklenir. Birden çok kod paketi olduğunda, sistem bildirilen hizmet türlerinden herhangi birini aradığında bunların tümü etkinleştirilir. Daha fazla bilgi için [CodePackage Öğesi'ne](service-fabric-service-model-schema-elements.md#CodePackageElementCodePackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedCodePackageelement) bakın

### <a name="entrypoint-element"></a>EntryPoint Öğesi
EntryPoint tarafından belirtilen yürütülebilir genellikle uzun süren hizmet ana bilgisayardır. Ayrı bir kurulum giriş noktasının varlığı, hizmet ana bilgisayarını uzun süreler boyunca yüksek ayrıcalıklarla çalıştırmak zorunda kalmamayı önler. EntryPoint tarafından belirtilen yürütülebilir kurulumEntryPoint çıktıktan sonra başarıyla çalıştırılır. Ortaya çıkan işlem izlenir ve yeniden başlatılır (SetupEntryPoint ile yeniden başlar) eğer hiç sona erer veya çöker. Daha fazla bilgi için [EntryPoint Öğesi'ne](service-fabric-service-model-schema-elements.md#EntryPointElementEntryPointDescriptionTypeComplexTypeDefinedInCodePackageTypecomplexType) bakın

### <a name="containerhost-element"></a>ContainerHost Öğesi
Daha fazla bilgi için [ContainerHost Öğesi'ne](service-fabric-service-model-schema-elements.md#ContainerHostElementContainerHostEntryPointTypeComplexTypeDefinedInEntryPointDescriptionTypecomplexType) bakın

### <a name="imagename-element"></a>ImageName Öğesi
Repo ve resim [https://hub.docker.com](https://hub.docker.com) veya Azure Kapsayıcı Kayıt Defteri. Daha fazla bilgi için [ImageName Öğesi'ne](service-fabric-service-model-schema-elements.md#ImageNameElementxs:stringComplexTypeDefinedInContainerHostEntryPointTypecomplexType) bakın

### <a name="commands-element"></a>Commands Öğesi
Virgül delimited komut listesini kapsayıcıya geçirin. Daha fazla bilgi için [bkz.](service-fabric-service-model-schema-elements.md#CommandsElementxs:stringComplexTypeDefinedInContainerHostEntryPointTypecomplexType)

### <a name="environmentvariables-element"></a>ÇevreDeğişkenler Öğesi
Ortam değişkenlerini kapsayıcınıza veya exe'nize aktarın.  Daha fazla bilgi için [ÇevreDeğişkenler Öğesi'ne](service-fabric-service-model-schema-elements.md#EnvironmentVariablesElementEnvironmentVariablesTypeComplexTypeDefinedInCodePackageTypecomplexType) bakın

### <a name="environmentvariable-element"></a>ÇevreDeğişken Öğesi
Çevre değişkeni. Daha fazla bilgi için [Bkz. EnvironmentVariable Element](service-fabric-service-model-schema-elements.md#EnvironmentVariableElementEnvironmentVariableOverrideTypeComplexTypeDefinedInEnvironmentOverridesTypecomplexType)

### <a name="configpackage-element"></a>ConfigPackage Elemanı
Settings.xml dosyasını içeren Ad özniteliği tarafından adlandırılan bir klasör bildirir. Bu dosya, işlemin çalışma zamanında okuyabileceği kullanıcı tanımlı, anahtar değeri çifti ayarlarının bölümlerini içerir. Yükseltme sırasında, yalnızca ConfigPackage sürümü değişmişse, çalıştırma işlemi yeniden başlatılamaz. Bunun yerine, geri arama, yapılandırma ayarlarının dinamik olarak yeniden yüklenebilmeleri için işlemin değiştiğini gösterir. Daha fazla bilgi için [Bkz. ConfigPackage Element](service-fabric-service-model-schema-elements.md#ConfigPackageElementConfigPackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedConfigPackageelement)

### <a name="resources-element"></a>Kaynaklar Öğesi
Derlenmiş kodu değiştirmeden bildirilebilen ve hizmet dağıtıldığında değiştirilebilen bu hizmet tarafından kullanılan kaynakları açıklar. Bu kaynaklara erişim, uygulama bildiriminin İlkeler ve İlkeler bölümleri aracılığıyla denetlenir. Daha fazla bilgi için [Kaynaklar Öğesi'ne](service-fabric-service-model-schema-elements.md#ResourcesElementResourcesTypeComplexTypeDefinedInServiceManifestTypecomplexType) bakın

### <a name="endpoints-element"></a>Uç Nokta Öğesi
Hizmet için uç noktaları tanımlar. Daha fazla bilgi için Bkz. [Uç Noktaları Öğesi](service-fabric-service-model-schema-elements.md#EndpointsElementanonymouscomplexTypeComplexTypeDefinedInResourcesTypecomplexType)

### <a name="endpoint-element"></a>Bitiş Noktası Öğesi
 Daha fazla bilgi için [Bkz. Endpoint Öğesi](service-fabric-service-model-schema-elements.md#EndpointElementEndpointOverrideTypeComplexTypeDefinedInEndpointselement)

