---
title: Bulut Hizmeti modeli ve paketi nedir | Microsoft Dokümanlar
description: Azure'da bulut hizmeti modelini (.csdef, .cscfg) ve paketi (.cspkg) açıklar
services: cloud-services
author: tanmaygore
ms.service: cloud-services
ms.topic: article
ms.date: 07/05/2017
ms.author: tagore
ms.openlocfilehash: 32603f4ab33e020245861e5dc66d2ade545fa627
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247494"
---
# <a name="what-is-the-cloud-service-model-and-how-do-i-package-it"></a>Bulut Hizmeti modeli nedir ve nasıl paketlenirim?
Bir bulut hizmeti üç bileşenden oluşturulur, hizmet tanımı *(.csdef)*, hizmet config *(.cscfg)* ve bir hizmet paketi *(.cspkg)*. **ServiceDefinition.csdef** ve **ServiceConfig.cscfg** dosyaları xml tabanlıdır ve bulut hizmetinin yapısını ve nasıl yapılandırıldığı açıklanır; topluca model denir. **ServicePackage.cspkg** **ServiceDefinition.csdef** oluşturulan ve diğer şeylerin yanı sıra, gerekli tüm ikili tabanlı bağımlılıkları içeren bir zip dosyasıdır. Azure hem **ServicePackage.cspkg** hem de **ServiceConfig.cscfg'den**bir bulut hizmeti oluşturur.

Bulut hizmeti Azure'da çalıştırıldığında **ServiceConfig.cscfg** dosyası üzerinden yeniden yapılandırabilirsiniz, ancak tanımı değiştiremezsiniz.

## <a name="what-would-you-like-to-know-more-about"></a>Ne hakkında daha fazla şey bilmek istersiniz?
* [ServiceDefinition.csdef](#csdef) ve [ServiceConfig.cscfg](#cscfg) dosyaları hakkında daha fazla şey bilmek istiyorum.
* Ben zaten bu konuda biliyorum, bana ne yapılandırabilirsiniz [bazı örnekler](#next-steps) verin.
* [ServicePackage.cspkg](#cspkg)oluşturmak istiyorum.
* Visual Studio kullanıyorum ve istiyorum ...
  * [Bulut hizmeti oluşturma][vs_create]
  * [Varolan bir bulut hizmetini yeniden yapılandırma][vs_reconfigure]
  * [Bulut Hizmeti projesini dağıtma][vs_deploy]
  * [Bulut hizmeti örneğine uzak masaüstü][remotedesktop]

<a name="csdef"></a>

## <a name="servicedefinitioncsdef"></a>ServiceDefinition.csdef
**ServiceDefinition.csdef** dosyası, bir bulut hizmetini yapılandırmak için Azure tarafından kullanılan ayarları belirtir. [Azure Hizmet Tanımı Şeması (.csdef Dosyası),](/previous-versions/azure/reference/ee758711(v=azure.100)) hizmet tanımı dosyası için izin verilebilen biçimi sağlar. Aşağıdaki örnekte, Web ve Çalışan rolleri için tanımlanabilecek ayarlar gösterilmektedir:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ServiceDefinition name="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceDefinition">
  <WebRole name="WebRole1" vmsize="Medium">
    <Sites>
      <Site name="Web">
        <Bindings>
          <Binding name="HttpIn" endpointName="HttpIn" />
        </Bindings>
      </Site>
    </Sites>
    <Endpoints>
      <InputEndpoint name="HttpIn" protocol="http" port="80" />
      <InternalEndpoint name="InternalHttpIn" protocol="http" />
    </Endpoints>
    <Certificates>
      <Certificate name="Certificate1" storeLocation="LocalMachine" storeName="My" />
    </Certificates>
    <Imports>
      <Import moduleName="Connect" />
      <Import moduleName="Diagnostics" />
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <LocalResources>
      <LocalStorage name="localStoreOne" sizeInMB="10" />
      <LocalStorage name="localStoreTwo" sizeInMB="10" cleanOnRoleRecycle="false" />
    </LocalResources>
    <Startup>
      <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" />
    </Startup>
  </WebRole>

  <WorkerRole name="WorkerRole1">
    <ConfigurationSettings>
      <Setting name="DiagnosticsConnectionString" />
    </ConfigurationSettings>
    <Imports>
      <Import moduleName="RemoteAccess" />
      <Import moduleName="RemoteForwarder" />
    </Imports>
    <Endpoints>
      <InputEndpoint name="Endpoint1" protocol="tcp" port="10000" />
      <InternalEndpoint name="Endpoint2" protocol="tcp" />
    </Endpoints>
  </WorkerRole>
</ServiceDefinition>
```

Burada kullanılan XML şemasını daha iyi anlamak için [Hizmet Tanımı Şeması'na](/previous-versions/azure/reference/ee758711(v=azure.100)) başvurabilirsiniz, ancak burada bazı öğelerin hızlı bir açıklaması aşağıda verilmiştir:

**Siteler**  
IIS7'de barındırılan web sitelerinin veya web uygulamalarının tanımlarını içerir.

**GirişSon noktalar**  
Bulut hizmetiyle iletişim kurmak için kullanılan uç noktaların tanımlarını içerir.

**InternalEndpoints**  
Rol örnekleri tarafından birbirleriyle iletişim kurmak için kullanılan uç noktaların tanımlarını içerir.

**Configurationsettings**  
Belirli bir rolün özellikleri için ayar tanımlarını içerir.

**Sertifikalar**  
Bir rol için gerekli olan sertifikaların tanımlarını içerir. Önceki kod örneği, Azure Connect yapılandırması için kullanılan bir sertifikayı gösterir.

**Yerel Kaynaklar**  
Yerel depolama kaynaklarının tanımlarını içerir. Yerel depolama kaynağı, bir rolün bir örneğinin çalıştırıldığı sanal makinenin dosya sisteminde ayrılmış bir dizinidir.

**Ithalat**  
İçe aktarılan modüllerin tanımlarını içerir. Önceki kod örneği Uzak Masaüstü Bağlantısı ve Azure Bağlantısı modüllerini gösterir.

**Başlangıç**  
Rol başladığında çalıştırılabilen görevleri içerir. Görevler .cmd veya çalıştırılabilir bir dosyada tanımlanır.

<a name="cscfg"></a>

## <a name="serviceconfigurationcscfg"></a>ServiceConfiguration.cscfg
Bulut hizmetiniz için ayarların yapılandırması **ServiceConfiguration.cscfg** dosyasındaki değerlere göre belirlenir. Bu dosyadaki her rol için dağıtmak istediğiniz örnek sayısını belirtirsiniz. Hizmet tanımı dosyasında tanımladığınız yapılandırma ayarlarının değerleri hizmet yapılandırma dosyasına eklenir. Bulut hizmetiyle ilişkili tüm yönetim sertifikalarının parmak izleri de dosyaya eklenir. [Azure Hizmet Yapılandırma Şeması (.cscfg Dosyası),](/previous-versions/azure/reference/ee758710(v=azure.100)) bir hizmet yapılandırma dosyası için izin verilebilen biçimi sağlar.

Hizmet yapılandırma dosyası uygulamayla birlikte paketlenmez, ancak ayrı bir dosya olarak Azure'a yüklenir ve bulut hizmetini yapılandırmak için kullanılır. Bulut hizmetinizi yeniden dağıtmadan yeni bir hizmet yapılandırma dosyası yükleyebilirsiniz. Bulut hizmeti çalışırken bulut hizmetinin yapılandırma değerleri değiştirilebilir. Aşağıdaki örnek, Web ve Çalışan rolleri için tanımlanabilecek yapılandırma ayarlarını gösterir:

```xml
<?xml version="1.0"?>
<ServiceConfiguration serviceName="MyServiceName" xmlns="http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration">
  <Role name="WebRole1">
    <Instances count="2" />
    <ConfigurationSettings>
      <Setting name="SettingName" value="SettingValue" />
    </ConfigurationSettings>

    <Certificates>
      <Certificate name="CertificateName" thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
      <Certificate name="Microsoft.WindowsAzure.Plugins.RemoteAccess.PasswordEncryption"
         thumbprint="CertThumbprint" thumbprintAlgorithm="sha1" />
    </Certificates>
  </Role>
</ServiceConfiguration>
```

Burada kullanılan XML şemasını daha iyi anlamak için [Hizmet Yapılandırma Şeması'na](/previous-versions/azure/reference/ee758710(v=azure.100)) başvurabilirsiniz, ancak burada öğelerin hızlı bir açıklaması aşağıda veda edebilirsiniz:

**Örnekler**  
Rol için çalışan örneklerin sayısını yapılandırır. Bulut hizmetinizin yükseltmeler sırasında kullanılamaz hale gelmesini önlemek için, web'e bakan rollerinizin birden fazla örneğini dağıtmanız önerilir. Birden fazla örnek dağıtarak, bir hizmet için iki veya daha fazla rol örneği dağıtıldığında Internet'e dönük roller için %99,95 dış bağlantı garantisi veren [Azure İşlem Hizmeti Düzeyi Sözleşmesi'ndeki (SLA)](https://azure.microsoft.com/support/legal/sla/)yönergelere bağlı kalırsınız.

**Configurationsettings**  
Bir rol için çalışan örneklerin ayarlarını yapılandırır. Öğelerin `<Setting>` adı, hizmet tanımı dosyasındaki ayar tanımlarıyla eşleşmelidir.

**Sertifikalar**  
Hizmet tarafından kullanılan sertifikaları yapılandırır. Önceki kod örneği, RemoteAccess modülü için sertifikanın nasıl tanımlandığını gösterir. *Parmak izi* özniteliğinin değeri, kullanılacak sertifikanın parmak izine ayarlanmalıdır.

<p/>

> [!NOTE]
> Sertifikanın parmak izi, bir metin düzenleyicisi kullanılarak yapılandırma dosyasına eklenebilir. Veya, değer Visual Studio rolü **Özellikleri** sayfasının **Sertifikalar** sekmesine eklenebilir.
> 
> 

## <a name="defining-ports-for-role-instances"></a>Rol örnekleri için bağlantı noktalarını tanımlama
Azure, bir web rolüne yalnızca bir giriş noktası sağlar. Tüm trafik tek bir IP adresi üzerinden oluşur anlamına gelir. Web merkezi üstbilgisini isteği doğru konuma yönlendirecek şekilde yapılandırarak web bağlantı noktasını paylaşacak şekilde yapılandırabilirsiniz. Uygulamalarınızı IP adresindeki tanınmış bağlantı noktalarını dinleyecek şekilde yapılandırabilirsiniz.

Aşağıdaki örnek, bir web sitesi ve web uygulaması yla web rolü yapılandırmasını gösterir. Web sitesi bağlantı noktası 80'deki varsayılan giriş konumu olarak yapılandırılır ve web uygulamaları "mail.mysite.cloudapp.net" olarak adlandırılan alternatif bir ana bilgisayar üstbilgisinden istek alacak şekilde yapılandırılır.

```xml
<WebRole>
  <ConfigurationSettings>
    <Setting name="DiagnosticsConnectionString" />
  </ConfigurationSettings>
  <Endpoints>
    <InputEndpoint name="HttpIn" protocol="http" port="80" />
    <InputEndpoint name="Https" protocol="https" port="443" certificate="SSL"/>
    <InputEndpoint name="NetTcp" protocol="tcp" port="808" certificate="SSL"/>
  </Endpoints>
  <LocalResources>
    <LocalStorage name="Sites" cleanOnRoleRecycle="true" sizeInMB="100" />
  </LocalResources>
  <Site name="Mysite" packageDir="Sites\Mysite">
    <Bindings>
      <Binding name="http" endpointName="HttpIn" />
      <Binding name="https" endpointName="Https" />
      <Binding name="tcp" endpointName="NetTcp" />
    </Bindings>
  </Site>
  <Site name="MailSite" packageDir="MailSite">
    <Bindings>
      <Binding name="mail" endpointName="HttpIn" hostHeader="mail.mysite.cloudapp.net" />
    </Bindings>
    <VirtualDirectory name="artifacts" />
    <VirtualApplication name="storageproxy">
      <VirtualDirectory name="packages" packageDir="Sites\storageProxy\packages"/>
    </VirtualApplication>
  </Site>
</WebRole>
```


## <a name="changing-the-configuration-of-a-role"></a>Bir rolün yapılandırmasını değiştirme
Azure'da çalışırken, hizmeti çevrimdışı olmadan bulut hizmetinizin yapılandırmasını güncelleştirebilirsiniz. Yapılandırma bilgilerini değiştirmek için, yeni bir yapılandırma dosyası yükleyebilir veya yapılandırma dosyasını yerinde olarak ayarlayabilir ve çalışan hizmetinize uygulayabilirsiniz. Bir hizmetin yapılandırması için aşağıdaki değişiklikler yapılabilir:

* **Yapılandırma ayarlarının değerlerini değiştirme**  
  Bir yapılandırma ayarı değiştiğinde, bir rol örneği değişikliği örnek çevrimiçiyken uygulamayı veya örneği düzgün bir şekilde geri dönüştürmeyi ve örnek çevrimdışıyken değişikliği uygulamayı seçebilir.
* **Rol örneklerinin hizmet topolojisini değiştirme**  
  Topoloji değişiklikleri, bir örneğin kaldırıldığı durumlar dışında çalışan örnekleri etkilemez. Kalan tüm örneklerin genellikle geri dönüştürülmesi gerekmez; ancak, bir topoloji değişikliğine yanıt olarak rol örneklerini geri dönüştürmeyi seçebilirsiniz.
* **Sertifika parmak izini değiştirme**  
  Bir sertifikayı yalnızca rol örneği çevrimdışı olduğunda güncelleştirebilirsiniz. Bir rol örneği çevrimiçiyken bir sertifika eklenir, silinir veya değiştirilirse, Azure sertifikayı güncelleştirmek ve değişiklik tamamlandıktan sonra çevrimiçi duruma getirmek için örneği düzgün bir şekilde çevrimdışına alır.

### <a name="handling-configuration-changes-with-service-runtime-events"></a>Service Runtime Events ile yapılandırma değişikliklerini işleme
[Azure Çalışma Zamanı Kitaplığı,](/previous-versions/azure/reference/mt419365(v=azure.100)) bir rolden Azure ortamıyla etkileşim için sınıflar sağlayan [Microsoft.WindowsAzure.ServiceRuntime](/previous-versions/azure/reference/ee741722(v=azure.100)) ad alanını içerir. [RoleEnvironment](/previous-versions/azure/reference/ee773173(v=azure.100)) sınıfı, yapılandırma değişikliğinden önce ve sonra yükseltilen aşağıdaki olayları tanımlar:

* **Olayı [değiştirme](/previous-versions/azure/reference/ee758134(v=azure.100))**  
  Bu, yapılandırma değişikliğinin belirli bir rol örneğine uygulanmadan önce oluşur ve gerekirse rol örneklerini aşağı çekme şansı verir.
* **[Değiştirilen](/previous-versions/azure/reference/ee758129(v=azure.100)) olay**  
  Yapılandırma değişikliği bir rolün belirli bir örneğine uygulandıktan sonra oluşur.

> [!NOTE]
> Sertifika değişiklikleri her zaman bir rolün örneklerini çevrimdışı olduğundan, RoleEnvironment.Changing veya RoleEnvironment.Changed olaylarını yükseltmez.
> 
> 

<a name="cspkg"></a>

## <a name="servicepackagecspkg"></a>ServicePackage.cspkg
> [!NOTE]
> Dağıtılabilen maksimum paket boyutu 600MB'dır

Bir uygulamayı Azure'da bulut hizmeti olarak dağıtmak için, öncelikle uygulamayı uygun biçimde paketlemeniz gerekir. Visual Studio'ya alternatif olarak paket dosyasını oluşturmak için **CSPack** komut satırı aracını [(Azure SDK](https://azure.microsoft.com/downloads/)ile yüklü) kullanabilirsiniz.

**CSPack,** paketin içeriğini tanımlamak için hizmet tanımı dosyasının ve hizmet yapılandırma dosyasının içeriğini kullanır. **CSPack,** [Azure portalını](cloud-services-how-to-create-deploy-portal.md#create-and-deploy)kullanarak Azure'a yükleyebileceğiniz bir uygulama paketi dosyası (.cspkg) oluşturur. Varsayılan olarak, paket `[ServiceDefinitionFileName].cspkg`adlandırılmış, ancak `/out` **CSPack**seçeneğini kullanarak farklı bir ad belirtebilirsiniz.

**CSPack,**  
`C:\Program Files\Microsoft SDKs\Azure\.NET SDK\[sdk-version]\bin\`

> [!NOTE]
> CSPack.exe (pencerelerde) SDK ile yüklü **Microsoft Azure Komut Komut Istem** kısayolu çalıştırarak kullanılabilir.  
> 
> Tüm olası anahtarlar ve komutlar hakkında belgeleri görmek için CSPack.exe programını tek başına çalıştırın.
> 
> 

<p />

> [!TIP]
> Bulut hizmetinizi **Microsoft Azure İşlem Emülatörü'nde**yerel olarak çalıştırın, **/copyonly** seçeneğini kullanın. Bu seçenek, uygulama için ikili dosyaları, işlem emülatöründe çalıştırılabildikleri bir dizin düzenine kopyalar.
> 
> 

### <a name="example-command-to-package-a-cloud-service"></a>Bulut hizmetini paketlemek için örnek komut
Aşağıdaki örnek, bir web rolü için bilgileri içeren bir uygulama paketi oluşturur. Komut, kullanılacak hizmet tanımı dosyasını, ikili dosyaların bulunabileceği dizini ve paket dosyasının adını belirtir.

```cmd
cspack [DirectoryName]\[ServiceDefinition]
       /role:[RoleName];[RoleBinariesDirectory]
       /sites:[RoleName];[VirtualPath];[PhysicalPath]
       /out:[OutputFileName]
```

Uygulama hem web rolü hem de bir alt rol içeriyorsa, aşağıdaki komut kullanılır:

```cmd
cspack [DirectoryName]\[ServiceDefinition]
       /out:[OutputFileName]
       /role:[RoleName];[RoleBinariesDirectory]
       /sites:[RoleName];[VirtualPath];[PhysicalPath]
       /role:[RoleName];[RoleBinariesDirectory];[RoleAssemblyName]
```

Değişkenlerin aşağıdaki gibi tanımlandığı yer:

| Değişken | Değer |
| --- | --- |
| \[Directoryname\] |Azure projesinin .csdef dosyasını içeren kök proje dizininin altındaki alt dizini. |
| \[Servicedefinition\] |Hizmet tanımı dosyasının adı. Varsayılan olarak, bu dosya ServiceDefinition.csdef olarak adlandırılır. |
| \[OutputFileName\] |Oluşturulan paket dosyasının adı. Genellikle, bu uygulamanın adına ayarlanır. Dosya adı belirtilmemişse, uygulama paketi \[ApplicationName\].cspkg olarak oluşturulur. |
| \[RoleName\] |Hizmet tanımı dosyasında tanımlandığı rolün adı. |
| \[RoleBinariesDirectory] |Rol için ikili dosyaların konumu. |
| \[Virtualpath\] |Hizmet tanımının Siteler bölümünde tanımlanan her sanal yolun fiziksel dizinleri. |
| \[PhysicalPath\] |Hizmet tanımının site düğümünde tanımlanan her sanal yol için içeriğin fiziksel dizinleri. |
| \[RoleAssemblyName\] |Rol için ikili dosyanın adı. |

## <a name="next-steps"></a>Sonraki adımlar
Bir bulut hizmet paketi oluşturuyorum ve istiyorum ...

* [Bulut hizmeti örneği için uzak masaüstü kurulumu][remotedesktop]
* [Bulut Hizmeti projesini dağıtma][deploy]

Visual Studio kullanıyorum ve istiyorum ...

* [Yeni bir bulut hizmeti oluşturma][vs_create]
* [Varolan bir bulut hizmetini yeniden yapılandırma][vs_reconfigure]
* [Bulut Hizmeti projesini dağıtma][vs_deploy]
* [Bulut hizmeti örneği için uzak masaüstü kurulumu][vs_remote]

[deploy]: cloud-services-how-to-create-deploy-portal.md
[remotedesktop]: cloud-services-role-enable-remote-desktop-new-portal.md
[vs_remote]: cloud-services-role-enable-remote-desktop-visual-studio.md
[vs_deploy]: ../vs-azure-tools-cloud-service-publish-set-up-required-services-in-visual-studio.md
[vs_reconfigure]: ../vs-azure-tools-configure-roles-for-cloud-service.md
[vs_create]: ../vs-azure-tools-azure-project-create.md



