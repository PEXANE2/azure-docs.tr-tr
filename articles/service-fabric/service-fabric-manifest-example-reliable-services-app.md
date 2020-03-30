---
title: Güvenilir hizmetler uygulaması manifesto örnekleri
description: Güvenilir bir hizmet Hizmet Kumaş ı uygulaması için uygulama ve hizmet bildirimi ayarlarını nasıl yapılandırabileceğinizi öğrenin.
author: peterpogorski
ms.topic: conceptual
ms.date: 06/11/2018
ms.author: pepogors
ms.openlocfilehash: f40e54f5260f827f0b18c833d23d1f57b5ebc3a3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79282360"
---
# <a name="reliable-services-application-and-service-manifest-examples"></a>Güvenilir hizmetler uygulaması ve hizmet bildirimi örnekleri
Aşağıda, ASP.NET Core web ön uç ve durum lu bir arka uç olan bir Hizmet Kumaşı uygulaması için uygulama ve hizmet bildirimlerine örnekler verilmiştir. Bu örneklerin amacı, hangi ayarların kullanılabilip kullanılacağını göstermektir. Bu uygulama ve hizmet bildirimleri [Servis Kumaşı .NET Quickstart](https://github.com/Azure-Samples/service-fabric-dotnet-quickstart/) bildirimlerine dayanır.

Aşağıdaki özellikler gösterilmiştir:

|Bildirim|Özellikler|
|---|---|
|[Uygulama bildirimi](#application-manifest)| [kaynak yönetişim,](service-fabric-resource-governance.md) [yerel yönetici hesabı olarak bir hizmet çalıştırmak,](service-fabric-application-runas-security.md)tüm hizmet kodu paketleri için varsayılan bir [ilke uygulamak,](service-fabric-application-runas-security.md#apply-a-default-policy-to-all-service-code-packages) [kullanıcı ve grup ilkeleri oluşturmak,](service-fabric-application-runas-security.md)hizmet örnekleri arasında bir veri paketi paylaşmak, [hizmet bitiş noktaları geçersiz kılmak](service-fabric-service-manifest-resources.md#overriding-endpoints-in-servicemanifestxml)| 
|FrontEndService hizmet bildirimi| [Hizmet başlangıç komut dosyası çalıştırın,](service-fabric-run-script-at-service-startup.md) [bir HTTPS bitiş noktası tanımlayın](service-fabric-tutorial-dotnet-app-enable-https-endpoint.md#define-an-https-endpoint-in-the-service-manifest) | 
|BackEndService hizmet bildirimi| [Config paketini](service-fabric-application-and-service-manifests.md) [bildirin, veri paketi bildirin,](service-fabric-application-and-service-manifests.md) [bitiş noktasını yapılandırın](service-fabric-service-manifest-resources.md)| 

Bkz. [Uygulama bildirimi öğeleri,](#application-manifest-elements) [VotingWeb hizmeti bildirim öğeleri](#votingweb-service-manifest-elements)ve VotingData hizmeti belirli XML öğeleri hakkında daha fazla bilgi için öğeleri [gösterir.](#votingdata-service-manifest-elements)

## <a name="application-manifest"></a>Uygulama bildirimi

```xml
<?xml version="1.0" encoding="utf-8"?>
<ApplicationManifest xmlns:xsd="https://www.w3.org/2001/XMLSchema" xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance" ApplicationTypeName="VotingType" ApplicationTypeVersion="1.0.0" xmlns="http://schemas.microsoft.com/2011/01/fabric">
  <Parameters>
    <Parameter Name="VotingData_MinReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingData_PartitionCount" DefaultValue="1" />
    <Parameter Name="VotingData_TargetReplicaSetSize" DefaultValue="3" />
    <Parameter Name="VotingWeb_InstanceCount" DefaultValue="-1" />
    <Parameter Name="CpuCores" DefaultValue="2" />
    <Parameter Name="Memory" DefaultValue="4084" />
    <Parameter Name="BlockIOWeight" DefaultValue="200" />
    <Parameter Name="MaximumIOBandwidth" DefaultValue="1024" />
    <Parameter Name="MemoryReservationInMB" DefaultValue="1024" />
    <Parameter Name="MemorySwapInMB" DefaultValue="4084"/>
    <Parameter Name="Port" DefaultValue="8081" />
    <Parameter Name="Protocol" DefaultValue="tcp" />
    <Parameter Name="Type" DefaultValue="internal" />
  </Parameters>
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingDataPkg" ServiceManifestVersion="1.0.0" />
    <!-- Override endpoints declared in the service manifest. -->
    <ResourceOverrides>
      <Endpoints>
        <Endpoint Name="DataEndpoint" Port="[Port]" Protocol="[Protocol]" Type="[Type]" />
      </Endpoints>
    </ResourceOverrides>

    <!-- Policies to be applied to the imported service manifest. -->
    <Policies>
      
      <!-- Set resource governance at the service package level. -->
      <ServicePackageResourceGovernancePolicy CpuCores="[CpuCores]" MemoryInMB="[Memory]"/>

      <!-- Set resource governance at the code package level. -->
      <ResourceGovernancePolicy CodePackageRef="Code" CpuPercent="10" MemoryInMB="[Memory]" BlockIOWeight="[BlockIOWeight]" 
                                MaximumIOBandwidth="[MaximumIOBandwidth]" MaximumIOps="[MaximumIOps]" MemoryReservationInMB="[MemoryReservationInMB]" 
                                MemorySwapInMB="[MemorySwapInMB]"/>

      <!-- Share the data package across multiple instances of the VotingData service-->
      <PackageSharingPolicy PackageRef="Data"/>

      <!-- Give read rights on the "DataEndpoint" endpoint to the Customer2 account.-->
      <SecurityAccessPolicy GrantRights="Read" PrincipalRef="Customer2" ResourceRef="DataEndpoint" ResourceType="Endpoint"/>         
    </Policies>
  </ServiceManifestImport>
  
  <!-- Import the ServiceManifest from the ServicePackage. The ServiceManifestName and ServiceManifestVersion 
       should match the Name and Version attributes of the ServiceManifest element defined in the 
       ServiceManifest.xml file. -->
  <ServiceManifestImport>
    <ServiceManifestRef ServiceManifestName="VotingWebPkg" ServiceManifestVersion="1.0.0" />
    
    <!-- Policies to be applied to the imported service manifest. -->
    <Policies>
      <!-- Run the setup entry point (defined in the imported service manifest) as the SetupAdminUser account 
      (declared in the following Principals section). -->
      <RunAsPolicy CodePackageRef="Code" UserRef="SetupAdminUser" EntryPointType="Setup" />
      
    </Policies>

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
      </StatelessService>
    </Service>
  </DefaultServices>
  <!-- Define users and groups required to run the services and access resources.  Principals are used in the Policies section(s). -->
  <Principals>
    <!-- Declare a set of groups as security principals, which can be referenced in policies. Groups are useful if there are multiple users 
    for different service entry points and they need to have certain common privileges that are available at the group level. -->
    <Groups>
      <!-- Create a group that has administrator privileges. -->
      <Group Name="LocalAdminGroup">
        <Membership>
          <SystemGroup Name="Administrators" />
        </Membership>
      </Group>
    </Groups>
    <Users>
      <!-- Declare a user and add the user to the Administrators system group. The SetupAdminUser account is used to run the 
      setup entry point of the VotingWebPkg code package (described in the preceding Policies section).-->
      <User Name="SetupAdminUser">
        <MemberOf>
          <SystemGroup Name="Administrators" />
        </MemberOf>
      </User>
      <!-- Create a user. Local user accounts are created on the machines where the application is deployed. By default, these accounts 
      do not have the same names as those specified here. Instead, they are dynamically generated and have random passwords. -->
      <User Name="Customer1" >
        <MemberOf>
          <!-- Add the user to the local administrators group.-->
          <Group NameRef="LocalAdminGroup" />
        </MemberOf>
      </User>
      <!-- Create a user as a local user with the specified account name and password. Local user accounts are created on the machines 
      where the application is deployed. -->
      <User Name="Customer2" AccountType="LocalUser" AccountName="Customer1" Password="MyPassword">
        <MemberOf>
          <!-- Add the user to the local administrators group.-->
          <Group NameRef="LocalAdminGroup" />
        </MemberOf>
      </User>
      <!-- Create a user as NetworkService. -->
      <User Name="MyDefaultAccount" AccountType="NetworkService" />      
    </Users>
  </Principals>
  <!-- Policies applied at the application level. -->
  <Policies>
    <!-- Specify a default user account for all code packages that don’t have a specific RunAsPolicy defined in 
    the ServiceManifestImport section(s). -->
    <DefaultRunAsPolicy UserRef="MyDefaultAccount" />
    
  </Policies>
</ApplicationManifest>

```

## <a name="votingweb-service-manifest"></a>OylamaWeb hizmet bildirimi

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
    <!-- A privileged entry point that by default runs with the same credentials as Service Fabric (typically the NetworkService account) before 
    any other entry point. Use the setup entry point to set system environment variables, give the account running the service (NETWORK SERVICE, by default) 
    access to a certificate's private key, or perform other setup tasks. In the application manifest, you can change the security permissions to run the startup script 
    under a local system account or an administrator account.  -->
    <SetupEntryPoint>
      <ExeHost>
        <!-- The setup script to run. -->
        <Program>Setup.bat</Program>
        <!-- Pass arguments to the script when it runs.-->
        <Arguments>MyValue</Arguments>
        <!-- The working directory for the process in the code package on the node where the application is deployed. CodePackage sets the working directory to be 
        the root of the code package regardless of where the EXE is defined in the code package directory. This is where the processes can write the data. Writing data 
        in the code package or code base is not recommended as those folders could be shared between different application instances and may get deleted.-->
        <WorkingFolder>CodePackage</WorkingFolder>
        <!-- Warning! Do not use console redirection in a production application, only use it for local development and debugging. Redirects console output from the startup
        script to an output file in the application folder called "log" on the cluster node where the application is deployed and run. Also set the number of output files
        to retain and the maximum file size (in KB). -->
        <ConsoleRedirection FileRetentionCount="10" FileMaxSizeInKb="20480"/>
      </ExeHost>
    </SetupEntryPoint>
    <EntryPoint>
      <ExeHost>
        <Program>VotingWeb.exe</Program>
        <!-- The working directory for the process in the code package on the node where the application is deployed. CodePackage sets the working directory to be 
        the root of the code package regardless of where the EXE is defined in the code package directory. This is where the processes can write the data. Writing data 
        in the code package or code base is not recommended as those folders could be shared between different application instances and may get deleted.-->
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Config package is the contents of the Config directory under PackageRoot that contains an 
       independently-updateable and versioned set of custom configuration settings for your service. -->
  <ConfigPackage Name="Config" Version="1.0.0" />

  <Resources>
    <Endpoints>
      <!-- Configure a HTTPS endpoint on port 443. This endpoint is used by the communication listener to obtain the port on which to 
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Protocol="https" Name="EndpointHttps" Type="Input" Port="443" />
    </Endpoints>
  </Resources>
</ServiceManifest>

```

## <a name="votingdata-service-manifest"></a>VotingData hizmet bildirimi

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceManifest Name="VotingDataPkg"
                 Version="1.0.0"
                 xmlns="http://schemas.microsoft.com/2011/01/fabric"
                 xmlns:xsd="https://www.w3.org/2001/XMLSchema"
                 xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance">
  <ServiceTypes>
    <!-- This is the name of your ServiceType. 
         This name must match the string used in RegisterServiceType call in Program.cs. -->
    <StatefulServiceType ServiceTypeName="VotingDataType"  HasPersistedState="true" />
  </ServiceTypes>

  <!-- Code package is your service executable. -->
  <CodePackage Name="Code" Version="1.0.0">
    <EntryPoint>
      <ExeHost>
        <Program>VotingData.exe</Program>
        <!-- The working directory for the process in the code package on the node where the application is deployed. CodePackage sets the working directory to be 
        the root of the code package regardless of where the EXE is defined in the code package directory. This is where the processes can write the data. Writing data 
        in the code package or code base is not recommended as those folders could be shared between different application instances and may get deleted.-->
        <WorkingFolder>CodePackage</WorkingFolder>
      </ExeHost>
    </EntryPoint>
  </CodePackage>

  <!-- Declares a folder, named by the Name attribute, under PackageRoot that contains a Settings.xml file. This file contains sections of user-defined, 
  key-value pair settings that the process can read back at run time. During an upgrade, if only the ConfigPackage version has changed, 
  then the running process is not restarted. Instead, a callback notifies the process that configuration settings have changed so they can be reloaded dynamically. -->
  <ConfigPackage Name="Config" Version="1.0.0" />
  
  <!-- Declares a folder, named by the Name attribute, under PackageRoot which contains static data files to be consumed by the process at run time. -->
  <DataPackage Name="Data" Version="1.0.0"/>

  <Resources>
    <Endpoints>
      <!-- Define an internal (used for intra-application communication) TCP endpoint. Since no port is specified, one is created and assigned dynamically
           to the service.-->
      <Endpoint Name="DataEndpoint" Protocol="tcp" Type="Internal" />
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

### <a name="resourceoverrides-element"></a>ResourceOverrides Öğesi
Hizmet bildirimi kaynaklarında bildirilen uç noktalar için kaynak geçersiz kılmaları belirtir. Daha fazla bilgi için [ResourceOverrides Öğesi'ne](service-fabric-service-model-schema-elements.md#ResourceOverridesElementResourceOverridesTypeComplexTypeDefinedInServiceManifestImportelement) bakın

### <a name="endpoints-element"></a>Uç Nokta Öğesi
Geçersiz kılınacak uç nokta(lar). Daha fazla bilgi için Bkz. [Uç Noktaları Öğesi](service-fabric-service-model-schema-elements.md#EndpointsElementanonymouscomplexTypeComplexTypeDefinedInResourceOverridesTypecomplexType)

### <a name="endpoint-element"></a>Bitiş Noktası Öğesi
Hizmet bildiriminde bildirilen bitiş noktası geçersiz kılınacak. Daha fazla bilgi için [Bkz. Endpoint Öğesi](service-fabric-service-model-schema-elements.md#EndpointElementEndpointOverrideTypeComplexTypeDefinedInEndpointselement)

### <a name="policies-element"></a>İlkeler Öğesi
İçe aktarılan hizmet bildiriminde uygulanacak ilkeleri (bitiş noktası bağlama, paket paylaşımı, run-as ve güvenlik erişimi) açıklar. Daha fazla bilgi için [Bkz. İlkeler Öğesi](service-fabric-service-model-schema-elements.md#PoliciesElementServiceManifestImportPoliciesTypeComplexTypeDefinedInServiceManifestImportelement)

### <a name="servicepackageresourcegovernancepolicy-element"></a>ServicePackageResourceGovernancePolicy Öğesi
Tüm hizmet paketi düzeyinde uygulanan kaynak yönetim ilkesini tanımlar. Daha fazla bilgi için [ServicePackageResourceGovernancePolicy Öğesi'ne](service-fabric-service-model-schema-elements.md#ServicePackageResourceGovernancePolicyElementServicePackageResourceGovernancePolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInServicePackageTypecomplexType) bakın

### <a name="resourcegovernancepolicy-element"></a>Kaynak YönetişimPolitikası Öğesi
Bir kod paketi için kaynak sınırlarını belirtir. Daha fazla bilgi için [KaynakYönetişimPolitikası Öğesi'ne](service-fabric-service-model-schema-elements.md#ResourceGovernancePolicyElementResourceGovernancePolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInDigestedCodePackageelementDefinedInDigestedEndpointelement) bakın

### <a name="packagesharingpolicy-element"></a>PackageSharingPolicy Öğesi
Bir kodun, config veya veri paketinin aynı hizmet türündeki hizmet örnekleri arasında paylaşılması gerektiğini gösterir. Daha fazla bilgi için [PackageSharingPolicy Öğesi'ne](service-fabric-service-model-schema-elements.md#PackageSharingPolicyElementPackageSharingPolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexType) bakın

### <a name="securityaccesspolicy-element"></a>SecurityAccessİlköğesi
Hizmet bildiriminde tanımlanan bir kaynak (bitiş noktası gibi) anaparaya erişim izinleri verir. Genellikle, güvenlik risklerini en aza indirmek için hizmetlerin farklı kaynaklara erişimini denetlemek ve kısıtlamak çok yararlıdır. Uygulama, farklı geliştiriciler tarafından geliştirilen bir pazar dan hizmet koleksiyonundan inşa edildiğinde bu özellikle önemlidir. Daha fazla bilgi için [SecurityAccessPolicy Öğesi'ne](service-fabric-service-model-schema-elements.md#SecurityAccessPolicyElementSecurityAccessPolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInSecurityAccessPolicieselementDefinedInDigestedEndpointelement) bakın

### <a name="runaspolicy-element"></a>RunasPolicy Öğesi
Yerel kullanıcı veya yerel sistem hesabını bir hizmet kodu paketinin olarak çalışacağını belirtir. Etki alanı hesapları, Azure Active Directory'nin kullanılabildiği Windows Server dağıtımlarında desteklenir. Varsayılan olarak, uygulamalar Fabric.exe işleminin altında çalıştığı hesap altında çalışır. Uygulamalar, Müdürler bölümünde beyan edilmesi gereken diğer hesaplar olarak da çalıştırılabilir. Bir hizmete RunAs ilkesi uygularsanız ve hizmet bildirimi HTTP protokolü ile uç nokta kaynaklarını bildirirse, bu uç noktalara tahsis edilen bağlantı noktalarının RunAs için doğru erişim denetimi olduğundan emin olmak için bir SecurityAccessPolicy belirtmeniz gerekir hizmetin altında çalıştığı kullanıcı hesabı. HTTPS bitiş noktası için, istemciye dönmek için sertifikanın adını belirtmek için bir EndpointBindingPolicy tanımlamanız da gerekir. Daha fazla bilgi için [RunAsPolicy Öğesi'ne](service-fabric-service-model-schema-elements.md#RunAsPolicyElementRunAsPolicyTypeComplexTypeDefinedInServiceManifestImportPoliciesTypecomplexTypeDefinedInDigestedCodePackageelement) bakın

### <a name="defaultservices-element"></a>Varsayılan Hizmetler Öğesi
Bir uygulama bu uygulama türüne karşı anında oluşturulduğunda otomatik olarak oluşturulan hizmet örneklerini bildirir. Daha fazla bilgi için [DefaultServices Öğesi'ne](service-fabric-service-model-schema-elements.md#DefaultServicesElementDefaultServicesTypeComplexTypeDefinedInApplicationManifestTypecomplexTypeDefinedInApplicationInstanceTypecomplexType) bakın

### <a name="service-element"></a>Hizmet Öğesi
Uygulama anında oluşturulduğunda otomatik olarak oluşturulacak bir hizmeti bildirir. Daha fazla bilgi için [Hizmet Öğesi'ne](service-fabric-service-model-schema-elements.md#ServiceElementanonymouscomplexTypeComplexTypeDefinedInDefaultServicesTypecomplexType) bakın

### <a name="statefulservice-element"></a>StatefulService Öğesi
Devlet hizmeti tanımlar. Daha fazla bilgi için [StatefulService Öğesi'ne](service-fabric-service-model-schema-elements.md#StatefulServiceElementStatefulServiceTypeComplexTypeDefinedInServiceTemplatesTypecomplexTypeDefinedInServiceelement) bakın

### <a name="statelessservice-element"></a>StatelessService Öğesi
Devletsiz bir hizmeti tanımlar. Daha fazla bilgi için [StatelessService Öğesi'ne](service-fabric-service-model-schema-elements.md#StatelessServiceElementStatelessServiceTypeComplexTypeDefinedInServiceTemplatesTypecomplexTypeDefinedInServiceelement) bakın

### <a name="principals-element"></a>İlkeler Öğesi
Bu uygulamanın hizmetleri çalıştırması ve kaynakları güvenli hale getirmek için gereken güvenlik ilkelerini (kullanıcılar, gruplar) açıklar. İlkeler bölümlerinde ilkelere başvurulmaktadır. Daha fazla bilgi için [Bkz. İlkeler Öğesi](service-fabric-service-model-schema-elements.md#PrincipalsElementSecurityPrincipalsTypeComplexTypeDefinedInApplicationManifestTypecomplexTypeDefinedInEnvironmentTypecomplexType)

### <a name="groups-element"></a>Groups Öğesi
İlkelerde başvurulabilecek bir grup kümesini güvenlik ilkeleri olarak bildirir. Gruplar, farklı hizmet giriş noktaları için birden çok kullanıcı varsa ve grup düzeyinde kullanılabilen belirli ortak ayrıcalıklara sahip olmaları gerekiyorsa yararlıdır. Daha fazla bilgi için [Gruplar Öğesi'ne](service-fabric-service-model-schema-elements.md#GroupsElementanonymouscomplexTypeComplexTypeDefinedInSecurityPrincipalsTypecomplexType) bakın

### <a name="group-element"></a>Group Öğesi
Bir grubu ilkelerde başvurulan bir güvenlik ilkesi olarak bildirir. Daha fazla bilgi için [Grup Öğesi'ne](service-fabric-service-model-schema-elements.md#GroupElementanonymouscomplexTypeComplexTypeDefinedInGroupselement) bakın

### <a name="membership-element"></a>Üyelik Öğesi
 Daha fazla bilgi için [Üyelik Öğesi'ne](service-fabric-service-model-schema-elements.md#MembershipElementanonymouscomplexTypeComplexTypeDefinedInGroupelement) bakın

### <a name="systemgroup-element"></a>SystemGroup Elemanı
 Daha fazla bilgi için [SystemGroup Öğesi'ne](service-fabric-service-model-schema-elements.md#SystemGroupElementanonymouscomplexTypeComplexTypeDefinedInMembershipelement) bakın

### <a name="users-element"></a>Kullanıcılar Öğesi
İlkelerde başvurulan bir kullanıcı kümesini güvenlik ilkeleri olarak bildirir. Daha fazla bilgi için [Bkz. Kullanıcılar Öğesi](service-fabric-service-model-schema-elements.md#UsersElementanonymouscomplexTypeComplexTypeDefinedInSecurityPrincipalsTypecomplexType)

### <a name="user-element"></a>Kullanıcı Öğesi
Bir kullanıcıyı ilkelerde başvurulan bir güvenlik ilkesi olarak bildirir. Daha fazla bilgi için [Bkz. Kullanıcı Öğesi](service-fabric-service-model-schema-elements.md#UserElementanonymouscomplexTypeComplexTypeDefinedInUserselement)

### <a name="memberof-element"></a>Üye Eleman
Kullanıcılar varolan herhangi bir üyelik grubuna eklenebilir, böylece söz le üyelik grubunun tüm özellikleri ve güvenlik ayarları devralınabilir. Üyelik grubu, farklı hizmetler veya aynı hizmet (farklı bir makinede) tarafından erişilmesi gereken dış kaynakları güvence altına almak için kullanılabilir. Daha fazla bilgi için [Üye Öğesi'ne](service-fabric-service-model-schema-elements.md#MemberOfElementanonymouscomplexTypeComplexTypeDefinedInUserelement) bakın

### <a name="systemgroup-element"></a>SystemGroup Elemanı
Kullanıcıyı ekleyecek sistem grubu.  Sistem grubu Gruplar bölümünde tanımlanmalıdır. Daha fazla bilgi için [SystemGroup Öğesi'ne](service-fabric-service-model-schema-elements.md#SystemGroupElementanonymouscomplexTypeComplexTypeDefinedInMemberOfelement) bakın

### <a name="group-element"></a>Group Öğesi
Kullanıcıyı ekleyecek grup.  Grup Gruplar bölümünde tanımlanmalıdır. Daha fazla bilgi için [Grup Öğesi'ne](service-fabric-service-model-schema-elements.md#GroupElementanonymouscomplexTypeComplexTypeDefinedInMemberOfelement) bakın

### <a name="policies-element"></a>İlkeler Öğesi
Uygulama düzeyinde uygulanacak ilkeleri (günlük toplama, varsayılan run-as, sistem durumu ve güvenlik erişimi) açıklar. Daha fazla bilgi için [Bkz. İlkeler Öğesi](service-fabric-service-model-schema-elements.md#PoliciesElementApplicationPoliciesTypeComplexTypeDefinedInApplicationManifestTypecomplexTypeDefinedInEnvironmentTypecomplexType)

### <a name="defaultrunaspolicy-element"></a>DefaultRunasPolicy Öğesi
ServiceManifestImport bölümünde tanımlanan belirli bir RunAsPolicy'si olmayan tüm hizmet kodu paketleri için varsayılan bir kullanıcı hesabı belirtin. Daha fazla bilgi için [DefaultRunAsPolicy Öğesi'ne](service-fabric-service-model-schema-elements.md#DefaultRunAsPolicyElementanonymouscomplexTypeComplexTypeDefinedInApplicationPoliciesTypecomplexType) bakın




## <a name="votingweb-service-manifest-elements"></a>OylamaWeb hizmeti bildirim öğeleri
### <a name="servicemanifest-element"></a>ServiceManifest Öğesi
Bildirimsel olarak hizmet türünü ve sürümünü açıklar. Bir veya daha fazla hizmet türünü desteklemek için bir hizmet paketi oluşturan bağımsız olarak yükseltilebilir kodu, yapılandırmayı ve veri paketlerini listeler. Hizmetler türü, sistem durumu özellikleri ve yük dengeleme ölçümleri gibi kaynaklar, tanılama ayarları ve hizmet meta verileri de belirtilir. Daha fazla bilgi için [ServiceManifest Öğesi'ne](service-fabric-service-model-schema-elements.md#ServiceManifestElementServiceManifestTypeComplexType) bakın

### <a name="servicetypes-element"></a>ServiceTypes Öğesi
Bu bildirimde hangi hizmet türlerinin CodePackage tarafından destekleniyi tanımlar. Bir hizmet bu hizmet türlerinden birine karşı anında olduğunda, bu bildirimde beyan edilen tüm kod paketleri giriş noktaları çalıştırılarak etkinleştirilir. Hizmet türleri, kod paketi düzeyinde değil, bildirim düzeyinde bildirilir. Daha fazla bilgi için [ServiceTypes Öğesi'ne](service-fabric-service-model-schema-elements.md#ServiceTypesElementServiceAndServiceGroupTypesTypeComplexTypeDefinedInServiceManifestTypecomplexType) bakın

### <a name="statelessservicetype-element"></a>StatelessServiceType Öğesi
Devletsiz hizmet türünü açıklar. Daha fazla bilgi için Bkz. [StatelessServiceType Öğesi](service-fabric-service-model-schema-elements.md#StatelessServiceTypeElementStatelessServiceTypeTypeComplexTypeDefinedInServiceAndServiceGroupTypesTypecomplexTypeDefinedInServiceTypesTypecomplexType)

### <a name="codepackage-element"></a>CodePackage Öğesi
Tanımlı hizmet türünü destekleyen bir kod paketini açıklar. Bir hizmet bu hizmet türlerinden birine karşı anında olduğunda, bu bildirimde beyan edilen tüm kod paketleri giriş noktaları çalıştırılarak etkinleştirilir. Elde edilen işlemlerin, desteklenen hizmet türlerini çalışma zamanında kaydetmesi beklenir. Birden çok kod paketi olduğunda, sistem bildirilen hizmet türlerinden herhangi birini aradığında bunların tümü etkinleştirilir. Daha fazla bilgi için [CodePackage Öğesi'ne](service-fabric-service-model-schema-elements.md#CodePackageElementCodePackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedCodePackageelement) bakın

### <a name="setupentrypoint-element"></a>SetupEntryPoint Öğesi
Varsayılan olarak, başka bir giriş noktasından önce Hizmet Kumaşı (genellikle NETWORKSERVICE hesabı) ile aynı kimlik bilgileriyle çalışan ayrıcalıklı bir giriş noktası. EntryPoint tarafından belirtilen yürütülebilir genellikle uzun süren hizmet ana bilgisayardır. Ayrı bir kurulum giriş noktasının varlığı, hizmet ana bilgisayarını uzun süreler boyunca yüksek ayrıcalıklarla çalıştırmak zorunda kalmamayı önler. Daha fazla bilgi için [Bkz. SetupEntryPoint Öğesi](service-fabric-service-model-schema-elements.md#SetupEntryPointElementanonymouscomplexTypeComplexTypeDefinedInCodePackageTypecomplexType)

### <a name="exehost-element"></a>ExeHost Öğesi
 Daha fazla bilgi için [ExeHost Öğesi'ne](service-fabric-service-model-schema-elements.md#ExeHostElementExeHostEntryPointTypeComplexTypeDefinedInSetupEntryPointelement) bakın

### <a name="program-element"></a>Program Öğesi
Çalıştırılabilir ad.  Örneğin, "MySetup.bat" veya "MyServiceHost.exe". Daha fazla bilgi için [Program Öğesi'ne](service-fabric-service-model-schema-elements.md#ProgramElementxs:stringComplexTypeDefinedInExeHostEntryPointTypecomplexType) bakın

### <a name="arguments-element"></a>Bağımsız Değişkenler Öğesi
 Daha fazla bilgi için [Bağımsız Değişkenler Öğesi'ne](service-fabric-service-model-schema-elements.md#ArgumentsElementxs:stringComplexTypeDefinedInExeHostEntryPointTypecomplexType) bakın

### <a name="workingfolder-element"></a>WorkingFolder Öğesi
Uygulamanın dağıtıldığı küme düğümündeki kod paketindeki işlem için çalışma dizini. Üç değer belirtebilirsiniz: İş (varsayılan), CodePackage veya CodeBase. CodeBase, çalışma dizininin EXE'nin kod paketinde tanımlandığı dizine ayarlandığını belirtir. CodePackage, EXE'nin kod paketi dizininde nerede tanımlandıklarına bakılmaksızın çalışma dizini kod paketinin kökü olarak ayarlar. Çalışma, çalışma dizini düğümüzerinde oluşturulan benzersiz bir klasöre ayarlar.  Bu klasör tüm uygulama örneği için aynıdır. Varsayılan olarak, uygulamadaki tüm işlemlerin çalışma dizini uygulama çalışma klasörüne ayarlanır. Bu, işlemlerin verileri yazabileceği yerdir. Bu klasörler farklı uygulama örnekleri arasında paylaşılabileceğinden ve silinebileceğinden, kod paketine veya kod tabanına veri yazma önerilmez. Daha fazla bilgi için [Bkz. WorkingFolder Öğesi](service-fabric-service-model-schema-elements.md#WorkingFolderElementanonymouscomplexTypeComplexTypeDefinedInExeHostEntryPointTypecomplexType)

### <a name="consoleredirection-element"></a>ConsoleRedirection Öğesi

> [!WARNING]
> Bir üretim uygulamasında konsol yönlendirmesi kullanmayın, yalnızca yerel geliştirme ve hata ayıklama için kullanın. Konsol çıktısını başlangıç komut dosyasından uygulama nın dağıtıldığı ve çalıştırıldığı küme düğümünde "günlük" adı verilen uygulama klasöründeki bir çıktı dosyasına yönlendirir. Daha fazla bilgi için [ConsoleRedirection Öğesi'ne](service-fabric-service-model-schema-elements.md#ConsoleRedirectionElementanonymouscomplexTypeComplexTypeDefinedInExeHostEntryPointTypecomplexType) bakın

### <a name="entrypoint-element"></a>EntryPoint Öğesi
EntryPoint tarafından belirtilen yürütülebilir genellikle uzun süren hizmet ana bilgisayardır. Ayrı bir kurulum giriş noktasının varlığı, hizmet ana bilgisayarını uzun süreler boyunca yüksek ayrıcalıklarla çalıştırmak zorunda kalmamayı önler. EntryPoint tarafından belirtilen yürütülebilir kurulumEntryPoint çıktıktan sonra başarıyla çalıştırılır. Ortaya çıkan işlem izlenir ve yeniden başlatılır (SetupEntryPoint ile yeniden başlar) eğer hiç sona erer veya çöker. Daha fazla bilgi için [EntryPoint Öğesi'ne](service-fabric-service-model-schema-elements.md#EntryPointElementEntryPointDescriptionTypeComplexTypeDefinedInCodePackageTypecomplexType) bakın

### <a name="exehost-element"></a>ExeHost Öğesi
 Daha fazla bilgi için [ExeHost Öğesi'ne](service-fabric-service-model-schema-elements.md#ExeHostElementanonymouscomplexTypeComplexTypeDefinedInEntryPointDescriptionTypecomplexType) bakın

### <a name="configpackage-element"></a>ConfigPackage Elemanı
PackageRoot'un altında, Ayarlar.xml dosyası içeren Ad özniteliği tarafından adlandırılan bir klasör bildirir. Bu dosya, işlemin çalışma zamanında okuyabileceği kullanıcı tanımlı, anahtar değeri çifti ayarlarının bölümlerini içerir. Yükseltme sırasında, yalnızca ConfigPackage sürümü değişmişse, çalıştırma işlemi yeniden başlatılamaz. Bunun yerine, geri arama, yapılandırma ayarlarının dinamik olarak yeniden yüklenebilmeleri için işlemin değiştiğini gösterir. Daha fazla bilgi için [Bkz. ConfigPackage Element](service-fabric-service-model-schema-elements.md#ConfigPackageElementConfigPackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedConfigPackageelement)

### <a name="resources-element"></a>Kaynaklar Öğesi
Derlenmiş kodu değiştirmeden bildirilebilen ve hizmet dağıtıldığında değiştirilebilen bu hizmet tarafından kullanılan kaynakları açıklar. Bu kaynaklara erişim, uygulama bildiriminin İlkeler ve İlkeler bölümleri aracılığıyla denetlenir. Daha fazla bilgi için [Kaynaklar Öğesi'ne](service-fabric-service-model-schema-elements.md#ResourcesElementResourcesTypeComplexTypeDefinedInServiceManifestTypecomplexType) bakın

### <a name="endpoints-element"></a>Uç Nokta Öğesi
Hizmet için uç noktaları tanımlar. Daha fazla bilgi için Bkz. [Uç Noktaları Öğesi](service-fabric-service-model-schema-elements.md#EndpointsElementanonymouscomplexTypeComplexTypeDefinedInResourcesTypecomplexType)

### <a name="endpoint-element"></a>Bitiş Noktası Öğesi
Hizmet bildiriminde bildirilen bitiş noktası geçersiz kılınacak. Daha fazla bilgi için [Bkz. Endpoint Öğesi](service-fabric-service-model-schema-elements.md#EndpointElementEndpointOverrideTypeComplexTypeDefinedInEndpointselement)



## <a name="votingdata-service-manifest-elements"></a>VotingData hizmeti bildirim öğeleri
### <a name="servicemanifest-element"></a>ServiceManifest Öğesi
Bildirimsel olarak hizmet türünü ve sürümünü açıklar. Bir veya daha fazla hizmet türünü desteklemek için bir hizmet paketi oluşturan bağımsız olarak yükseltilebilir kodu, yapılandırmayı ve veri paketlerini listeler. Hizmetler türü, sistem durumu özellikleri ve yük dengeleme ölçümleri gibi kaynaklar, tanılama ayarları ve hizmet meta verileri de belirtilir. Daha fazla bilgi için [ServiceManifest Öğesi'ne](service-fabric-service-model-schema-elements.md#ServiceManifestElementServiceManifestTypeComplexType) bakın

### <a name="servicetypes-element"></a>ServiceTypes Öğesi
Bu bildirimde hangi hizmet türlerinin CodePackage tarafından destekleniyi tanımlar. Bir hizmet bu hizmet türlerinden birine karşı anında olduğunda, bu bildirimde beyan edilen tüm kod paketleri giriş noktaları çalıştırılarak etkinleştirilir. Hizmet türleri, kod paketi düzeyinde değil, bildirim düzeyinde bildirilir. Daha fazla bilgi için [ServiceTypes Öğesi'ne](service-fabric-service-model-schema-elements.md#ServiceTypesElementServiceAndServiceGroupTypesTypeComplexTypeDefinedInServiceManifestTypecomplexType) bakın

### <a name="statefulservicetype-element"></a>StatefulServiceType Öğesi
Devlet hizmeti türünü açıklar. Daha fazla bilgi için [Bkz. StatefulServiceType Öğesi](service-fabric-service-model-schema-elements.md#StatefulServiceTypeElementStatefulServiceTypeTypeComplexTypeDefinedInServiceAndServiceGroupTypesTypecomplexTypeDefinedInServiceTypesTypecomplexType)

### <a name="codepackage-element"></a>CodePackage Öğesi
Tanımlı hizmet türünü destekleyen bir kod paketini açıklar. Bir hizmet bu hizmet türlerinden birine karşı anında olduğunda, bu bildirimde beyan edilen tüm kod paketleri giriş noktaları çalıştırılarak etkinleştirilir. Elde edilen işlemlerin, desteklenen hizmet türlerini çalışma zamanında kaydetmesi beklenir. Birden çok kod paketi olduğunda, sistem bildirilen hizmet türlerinden herhangi birini aradığında bunların tümü etkinleştirilir. Daha fazla bilgi için [CodePackage Öğesi'ne](service-fabric-service-model-schema-elements.md#CodePackageElementCodePackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedCodePackageelement) bakın

### <a name="entrypoint-element"></a>EntryPoint Öğesi
EntryPoint tarafından belirtilen yürütülebilir genellikle uzun süren hizmet ana bilgisayardır. Ayrı bir kurulum giriş noktasının varlığı, hizmet ana bilgisayarını uzun süreler boyunca yüksek ayrıcalıklarla çalıştırmak zorunda kalmamayı önler. EntryPoint tarafından belirtilen yürütülebilir kurulumEntryPoint çıktıktan sonra başarıyla çalıştırılır. Ortaya çıkan işlem izlenir ve yeniden başlatılır (SetupEntryPoint ile yeniden başlar) eğer hiç sona erer veya çöker. Daha fazla bilgi için [EntryPoint Öğesi'ne](service-fabric-service-model-schema-elements.md#EntryPointElementEntryPointDescriptionTypeComplexTypeDefinedInCodePackageTypecomplexType) bakın

### <a name="exehost-element"></a>ExeHost Öğesi
 Daha fazla bilgi için [ExeHost Öğesi'ne](service-fabric-service-model-schema-elements.md#ExeHostElementanonymouscomplexTypeComplexTypeDefinedInEntryPointDescriptionTypecomplexType) bakın

### <a name="program-element"></a>Program Öğesi
Çalıştırılabilir ad.  Örneğin, "MySetup.bat" veya "MyServiceHost.exe". Daha fazla bilgi için [Program Öğesi'ne](service-fabric-service-model-schema-elements.md#ProgramElementxs:stringComplexTypeDefinedInExeHostEntryPointTypecomplexType) bakın

### <a name="workingfolder-element"></a>WorkingFolder Öğesi
Uygulamanın dağıtıldığı küme düğümündeki kod paketindeki işlem için çalışma dizini. Üç değer belirtebilirsiniz: İş (varsayılan), CodePackage veya CodeBase. CodeBase, çalışma dizininin EXE'nin kod paketinde tanımlandığı dizine ayarlandığını belirtir. CodePackage, EXE'nin kod paketi dizininde nerede tanımlandıklarına bakılmaksızın çalışma dizini kod paketinin kökü olarak ayarlar. Çalışma, çalışma dizini düğümüzerinde oluşturulan benzersiz bir klasöre ayarlar.  Bu klasör tüm uygulama örneği için aynıdır. Varsayılan olarak, uygulamadaki tüm işlemlerin çalışma dizini uygulama çalışma klasörüne ayarlanır. Bu, işlemlerin verileri yazabileceği yerdir. Bu klasörler farklı uygulama örnekleri arasında paylaşılabileceğinden ve silinebileceğinden, kod paketine veya kod tabanına veri yazma önerilmez. Daha fazla bilgi için [Bkz. WorkingFolder Öğesi](service-fabric-service-model-schema-elements.md#WorkingFolderElementanonymouscomplexTypeComplexTypeDefinedInExeHostEntryPointTypecomplexType)

### <a name="configpackage-element"></a>ConfigPackage Elemanı
PackageRoot'un altında, Ayarlar.xml dosyası içeren Ad özniteliği tarafından adlandırılan bir klasör bildirir. Bu dosya, işlemin çalışma zamanında okuyabileceği kullanıcı tanımlı, anahtar değeri çifti ayarlarının bölümlerini içerir. Yükseltme sırasında, yalnızca ConfigPackage sürümü değişmişse, çalıştırma işlemi yeniden başlatılamaz. Bunun yerine, geri arama, yapılandırma ayarlarının dinamik olarak yeniden yüklenebilmeleri için işlemin değiştiğini gösterir. Daha fazla bilgi için [Bkz. ConfigPackage Element](service-fabric-service-model-schema-elements.md#ConfigPackageElementConfigPackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedConfigPackageelement)

### <a name="datapackage-element"></a>DataPackage Öğesi
PackageRoot'un altında, çalışma sırasında işlem tarafından tüketilecek statik veri dosyalarını içeren Ad özniteliği tarafından adlandırılan bir klasör bildirir. Service Fabric, hizmet bildiriminde listelenen veri paketlerinden herhangi biri yükseltildiğinde ana bilgisayar ve destek paketlerinde belirtilen tüm EX'leri ve DLLHOST'ları geri dönüştürür. Daha fazla bilgi için [Bkz. DataPackage Öğesi](service-fabric-service-model-schema-elements.md#DataPackageElementDataPackageTypeComplexTypeDefinedInServiceManifestTypecomplexTypeDefinedInDigestedDataPackageelement)

### <a name="resources-element"></a>Kaynaklar Öğesi
Derlenmiş kodu değiştirmeden bildirilebilen ve hizmet dağıtıldığında değiştirilebilen bu hizmet tarafından kullanılan kaynakları açıklar. Bu kaynaklara erişim, uygulama bildiriminin İlkeler ve İlkeler bölümleri aracılığıyla denetlenir. Daha fazla bilgi için [Kaynaklar Öğesi'ne](service-fabric-service-model-schema-elements.md#ResourcesElementResourcesTypeComplexTypeDefinedInServiceManifestTypecomplexType) bakın

### <a name="endpoints-element"></a>Uç Nokta Öğesi
Hizmet için uç noktaları tanımlar. Daha fazla bilgi için Bkz. [Uç Noktaları Öğesi](service-fabric-service-model-schema-elements.md#EndpointsElementanonymouscomplexTypeComplexTypeDefinedInResourcesTypecomplexType)

### <a name="endpoint-element"></a>Bitiş Noktası Öğesi
Hizmet bildiriminde bildirilen bitiş noktası geçersiz kılınacak. Daha fazla bilgi için [Bkz. Endpoint Öğesi](service-fabric-service-model-schema-elements.md#EndpointElementEndpointOverrideTypeComplexTypeDefinedInEndpointselement)

