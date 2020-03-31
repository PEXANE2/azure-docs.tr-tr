---
title: Azure Bulut Hizmetleri uygulamalarını Hizmet Kumaşına dönüştürün
description: Bu kılavuz, Bulut Hizmetleri Web ve İşçi Rolleri ile Hizmet Kumaşı kullanımsız hizmetlerini karşılaştırıp Bulut Hizmetlerinden Hizmet Kumaşına geçişe yardımcı olur.
author: vturecek
ms.topic: conceptual
ms.date: 11/02/2017
ms.author: vturecek
ms.openlocfilehash: caf067f793ca2086bc068907e86a82266627d128
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75463334"
---
# <a name="guide-to-converting-web-and-worker-roles-to-service-fabric-stateless-services"></a>Web ve İşçi Rollerini Hizmet Kumaşı devletsiz hizmetlere dönüştürme kılavuzu
Bu makalede, Bulut Hizmetleri Web ve İşçi Rollerinizi Hizmet Kumaşı devletsiz hizmetlere nasıl geçirilen açıklanmaktadır. Bu, genel mimarisi kabaca aynı kalacak uygulamalar için Bulut Hizmetlerinden Hizmet Kumaşına en basit geçiş yoludur.

## <a name="cloud-service-project-to-service-fabric-application-project"></a>Hizmet Kumaşı uygulama projesine Bulut Hizmeti projesi
 Bulut Hizmeti projesi ve Hizmet Kumaş Uygulaması projesi benzer bir yapıya sahiptir ve her ikisi de uygulamanız için dağıtım birimini temsil emektedir - yani, her biri uygulamanızı çalıştırmak için dağıtılan tam paketi tanımlar. Bulut Hizmeti projesi bir veya daha fazla Web veya İşçi Rolü içerir. Benzer şekilde, Bir Hizmet Kumaş Uygulaması projesi bir veya daha fazla hizmet içerir. 

Aradaki fark, Bulut Hizmeti projesinin uygulama dağıtımını VM dağıtımıyla birleştirilmesi ve bu nedenle içinde VM yapılandırma ayarları içermesi, hizmet kumaşı uygulaması projesinin ise yalnızca bir dizi uygulamaya dağıtılacak bir uygulamayı tanımlamasıdır. Service Fabric kümesindeki mevcut VM'ler. Hizmet Kumaşı kümesinin kendisi, Kaynak Yöneticisi şablonu veya Azure portalı aracılığıyla yalnızca bir kez dağıtılır ve birden çok Service Fabric uygulaması bu kümeye dağıtılabilir.

![Hizmet Kumaş ve Bulut Hizmetleri proje karşılaştırması][3]

## <a name="worker-role-to-stateless-service"></a>Vatansız hizmete İşçi Rolü
Kavramsal olarak, Bir İşçi Rolü devletsiz bir iş yükünü temsil eder, yani iş yükünün her örneği aynıdır ve istekler her zaman herhangi bir örne yönlendirilebilir. Her örneğin önceki isteği hatırlaması beklenmez. İş yükünün üzerinde çalıştığını belirtin, Azure Tablo Depolama veya Azure Cosmos DB gibi harici bir devlet deposu tarafından yönetilir. Hizmet Kumaşı'nda, bu tür iş yükü Bir Devletsiz Hizmet tarafından temsil edilir. İşçi Rolünü Hizmet Kumaşına geçirmek için en basit yaklaşım, İşçi Rol kodunu Niçin Devletsiz Hizmete dönüştürerek yapılabilir.

![Devletsiz Hizmete İşçi Rolü][4]

## <a name="web-role-to-stateless-service"></a>Web Rolü devletsiz hizmet için
İşçi Rolü'ne benzer şekilde, Bir Web Rolü de devletsiz bir iş yükünü temsil eder ve bu nedenle kavramsal olarak da hizmet kumaşı devletsiz bir hizmetle eşlenebilir. Ancak, Web Rolleri'nin aksine, Service Fabric IIS'yi desteklemez. Bir web uygulamasını bir Web Rolü'nden devletsiz bir hizmete geçirmek için öncelikle kendi barındırılabilen ve ASP.NET Core 1 gibi IIS veya System.Web'e bağlı olmayan bir web çerçevesine geçmek gerekir.

| **Uygulama** | **Desteklenen** | **Geçiş yolu** |
| --- | --- | --- |
| ASP.NET Web Forms |Hayır |core 1 MVC ASP.NET dönüştürün |
| ASP.NET MVC |Geçiş ile |ASP.NET Core 1 MVC'ye yükseltin |
| ASP.NET Web API'si |Geçiş ile |Kendi kendine barındırılan sunucuveya ASP.NET Core 1'i kullanma |
| ASP.NET Çekirdek 1 |Evet |Yok |

## <a name="entry-point-api-and-lifecycle"></a>Giriş noktası API'si ve yaşam döngüsü
İşçi Rolü ve Hizmet Kumaşı hizmet API'leri benzer giriş noktaları sunar: 

| **Girdi Noktası** | **İşçi Rolü** | **Servis Kumaş servisi** |
| --- | --- | --- |
| İşleniyor |`Run()` |`RunAsync()` |
| VM başlangıcı |`OnStart()` |Yok |
| VM durağı |`OnStop()` |Yok |
| İstemci istekleri için açık dinleyici |Yok |<ul><li> `CreateServiceInstanceListener()`vatansızlar için</li><li>`CreateServiceReplicaListener()`stateful için</li></ul> |

### <a name="worker-role"></a>İşçi Rolü
```csharp

using Microsoft.WindowsAzure.ServiceRuntime;

namespace WorkerRole1
{
    public class WorkerRole : RoleEntryPoint
    {
        public override void Run()
        {
        }

        public override bool OnStart()
        {
        }

        public override void OnStop()
        {
        }
    }
}

```

### <a name="service-fabric-stateless-service"></a>Servis Kumaş Stateless Servisi
```csharp

using System.Collections.Generic;
using System.Threading;
using System.Threading.Tasks;
using Microsoft.ServiceFabric.Services.Communication.Runtime;
using Microsoft.ServiceFabric.Services.Runtime;

namespace Stateless1
{
    public class Stateless1 : StatelessService
    {
        protected override IEnumerable<ServiceInstanceListener> CreateServiceInstanceListeners()
        {
        }

        protected override Task RunAsync(CancellationToken cancelServiceInstance)
        {
        }
    }
}

```

Her ikiside de işlemeye başlamak için birincil bir "Çalıştır" geçersiz kılma vardır. Servis Kumaş `Run`hizmetleri `Start`birleştirmek `Stop` , ve tek `RunAsync`bir giriş noktası içine, . Hizmetiniz `RunAsync` başladığında çalışmaya başlamalı ve yöntemin `RunAsync` İptal Tokeni sinyali verildiğinde çalışmayı durdurmalıdır. 

İşçi Rolleri ve Hizmet Kumaşı hizmetlerinin yaşam döngüsü ve kullanım ömrü arasında birkaç temel fark vardır:

* **Yaşam döngüsü:** En büyük fark, İşçi Rolü'nün bir VM olması ve bu nedenle yaşam döngüsünün VM'nin başlatıp durduğu nda etkinlikleri içeren VM'ye bağlı olmasıdır. Servis Kumaşı hizmeti, VM yaşam döngüsünden ayrı bir yaşam döngüsüne sahiptir, bu nedenle ana bilgisayar VM veya makinenin başlatıp durması ile ilgili olmadıkları için olayları içermez.
* **Ömür boyu:** `Run` Yöntem çıkarsa, İşçi Rolü örneği geri dönüşüm olur. Ancak `RunAsync` Bir Hizmet Kumaşı hizmetindeki yöntem tamamlanabilir ve hizmet örneği çalışır. 

Service Fabric, istemci isteklerini dinleyen hizmetler için isteğe bağlı bir iletişim kurulum giriş noktası sağlar. Hem RunAsync hem de iletişim giriş noktası Hizmet Kumaşı hizmetlerinde isteğe bağlı geçersiz kılmalardır - hizmetiniz yalnızca istemci isteklerini dinlemeyi veya yalnızca bir işleme döngüsü veya her ikisini çalıştırmayı seçebilir - bu nedenle RunAsync yöntemiyeniden başlatmadan çıkışa izin verilir hizmet örneği, istemci isteklerini dinlemeye devam edebileceğinden.

## <a name="application-api-and-environment"></a>Uygulama API'si ve çevre
Bulut Hizmetleri ortamı API'si, geçerli VM örneğinin yanı sıra diğer VM rol örnekleri hakkında bilgi ve işlev sağlar. Service Fabric, çalışma süresiyle ilgili bilgiler ve bir hizmetin şu anda çalışmakta olduğu düğüm hakkında bazı bilgiler sağlar. 

| **Çevre Görevi** | **Bulut Hizmetleri** | **Service Fabric** |
| --- | --- | --- |
| Yapılandırma Ayarları ve bildirimi değiştirme |`RoleEnvironment` |`CodePackageActivationContext` |
| Yerel Depolama |`RoleEnvironment` |`CodePackageActivationContext` |
| Uç Nokta Bilgileri |`RoleInstance` <ul><li>Geçerli örnek:`RoleEnvironment.CurrentRoleInstance`</li><li>Diğer roller ve örnek:`RoleEnvironment.Roles`</li> |<ul><li>`NodeContext`geçerli Düğüm adresi için</li><li>`FabricClient`ve `ServicePartitionResolver` hizmet bitiş noktası keşfi için</li> |
| Çevre Öykünme |`RoleEnvironment.IsEmulated` |Yok |
| Eşzamanlı değişim olayı |`RoleEnvironment` |Yok |

## <a name="configuration-settings"></a>Yapılandırma ayarları
Bulut Hizmetleri'ndeki yapılandırma ayarları bir VM rolü için ayarlanır ve bu VM rolünün tüm örnekleri için geçerlidir. Bu ayarlar ServiceConfiguration.*.cscfg dosyalarında ayarlanan anahtar değer çiftleridir ve doğrudan RoleEnvironment üzerinden erişilebilir. Hizmet Kumaşı'nda ayarlar, VM birden çok hizmet ve uygulama barındırabildiği için, bir VM yerine her hizmete ve her uygulamaya ayrı ayrı uygulanır. Bir hizmet üç pakettir:

* **Kod:** hizmet yürütülebilir, ikili, DLL'leri ve bir hizmetin çalışması gereken diğer dosyaları içerir.
* **Config:** bir hizmet için tüm yapılandırma dosyaları ve ayarları.
* **Veri:** hizmetle ilişkili statik veri dosyaları.

Bu paketlerin her biri bağımsız olarak sürümlenebilir ve yükseltilebilir. Bulut Hizmetleri'ne benzer şekilde, bir config paketine bir API aracılığıyla programlı olarak erişilebilir ve bir config paket değişikliğini hizmete bildirmek için olaylar kullanılabilir. Settings.xml dosyası, App.config dosyasının uygulama ayarları bölümüne benzer anahtar değeri yapılandırması ve programatik erişim için kullanılabilir. Ancak, Bulut Hizmetlerinin aksine, Bir Service Fabric config paketi, XML, JSON, YAML veya özel ikili biçim olsun, herhangi bir biçimde herhangi bir yapılandırma dosyası içerebilir. 

### <a name="accessing-configuration"></a>Yapılandırmaya erişim
#### <a name="cloud-services"></a>Cloud Services
ServiceConfiguration.*.cscfg'den yapılandırma ayarlarına `RoleEnvironment`. Bu ayarlar, aynı Bulut Hizmeti dağıtımındaki tüm rol örnekleri için genel olarak kullanılabilir.

```csharp

string value = RoleEnvironment.GetConfigurationSettingValue("Key");

```

#### <a name="service-fabric"></a>Service Fabric
Her hizmetin kendi bireysel yapılandırma paketi vardır. Kümedeki tüm uygulamalar tarafından erişilebilen genel yapılandırma ayarları için yerleşik bir mekanizma yoktur. Service Fabric'in özel Settings.xml yapılandırma dosyasını yapılandırma paketi içinde kullanırken, Settings.xml'deki değerler uygulama düzeyinde niçin üzerine yazılabilir ve uygulama düzeyinde yapılandırma ayarları mümkün kılınabilir.

Yapılandırma ayarları, her hizmet örneğine hizmetin `CodePackageActivationContext`.

```csharp

ConfigurationPackage configPackage = this.Context.CodePackageActivationContext.GetConfigurationPackageObject("Config");

// Access Settings.xml
KeyedCollection<string, ConfigurationProperty> parameters = configPackage.Settings.Sections["MyConfigSection"].Parameters;

string value = parameters["Key"]?.Value;

// Access custom configuration file:
using (StreamReader reader = new StreamReader(Path.Combine(configPackage.Path, "CustomConfig.json")))
{
    MySettings settings = JsonConvert.DeserializeObject<MySettings>(reader.ReadToEnd());
}

```

### <a name="configuration-update-events"></a>Yapılandırma güncelleştirme olayları
#### <a name="cloud-services"></a>Cloud Services
Olay, `RoleEnvironment.Changed` yapılandırma değişikliği gibi ortamda bir değişiklik gerçekleştiğinde tüm rol örneklerini bildirmek için kullanılır. Bu, rol örneklerini geri dönüştürmeden veya bir alt işlemi yeniden başlatmadan yapılandırma güncelleştirmelerini tüketmek için kullanılır.

```csharp

RoleEnvironment.Changed += RoleEnvironmentChanged;

private void RoleEnvironmentChanged(object sender, RoleEnvironmentChangedEventArgs e)
{
   // Get the list of configuration changes
   var settingChanges = e.Changes.OfType<RoleEnvironmentConfigurationSettingChange>();
foreach (var settingChange in settingChanges) 
   {
      Trace.WriteLine("Setting: " + settingChange.ConfigurationSettingName, "Information");
   }
}

```

#### <a name="service-fabric"></a>Service Fabric
Bir hizmetteki üç paket türünün her biri (Kod, Config ve Veri) bir paket güncelleştirildiğinde, eklendiğinde veya kaldırıldığında servis örneğini bildiren olaylara sahiptir. Bir hizmet, her türden birden çok paket içerebilir. Örneğin, bir hizmetin her biri ayrı ayrı sürülebilir ve yükseltilebilir olmak üzere birden çok config paketi olabilir. 

Bu olaylar, hizmet örneğini yeniden başlatmadan hizmet paketlerindeki değişiklikleri tüketmek için kullanılabilir.

```csharp

this.Context.CodePackageActivationContext.ConfigurationPackageModifiedEvent +=
                    this.CodePackageActivationContext_ConfigurationPackageModifiedEvent;

private void CodePackageActivationContext_ConfigurationPackageModifiedEvent(object sender, PackageModifiedEventArgs<ConfigurationPackage> e)
{
    this.UpdateCustomConfig(e.NewPackage.Path);
    this.UpdateSettings(e.NewPackage.Settings);
}

```

## <a name="startup-tasks"></a>Başlangıç görevleri
Başlangıç görevleri, uygulama başlamadan önce gerçekleştirilen eylemlerdir. Başlangıç görevi genellikle yüksek ayrıcalıkları kullanarak kurulum komut dosyalarını çalıştırmak için kullanılır. Hem Bulut Hizmetleri hem de Service Fabric başlangıç görevlerini destekler. Temel fark, Bulut Hizmetleri'nde bir başlangıç görevinin rol örneğinin bir parçası olduğu için Bir VM'ye bağlı olması, Service Fabric'te ise bir başlangıç görevinin belirli bir VM'ye bağlı olmayan bir hizmete bağlı olmasıdır.

| Service Fabric | Cloud Services |
| --- | --- |
| Yapılandırma konumu |ServiceDefinition.csdef |
| Ayrıcalıklar |"sınırlı" veya "yükseltilmiş" |
| Sıralama |"basit", "arka plan", "ön plan" |

### <a name="cloud-services"></a>Cloud Services
Bulut Hizmetleri'nde ServiceDefinition.csdef'deki rol başına bir başlangıç giriş noktası yapılandırılır. 

```xml

<ServiceDefinition>
    <Startup>
        <Task commandLine="Startup.cmd" executionContext="limited" taskType="simple" >
            <Environment>
                <Variable name="MyVersionNumber" value="1.0.0.0" />
            </Environment>
        </Task>
    </Startup>
    ...
</ServiceDefinition>

```

### <a name="service-fabric"></a>Service Fabric
Service Fabric'te serviceManifest.xml'de hizmet başına bir başlangıç giriş noktası yapılandırılır:

```xml

<ServiceManifest>
  <CodePackage Name="Code" Version="1.0.0">
    <SetupEntryPoint>
      <ExeHost>
        <Program>Startup.bat</Program>
      </ExeHost>
    </SetupEntryPoint>
    ...
</ServiceManifest>

``` 

## <a name="a-note-about-development-environment"></a>Geliştirme ortamı hakkında bir not
Hem Bulut Hizmetleri hem de Hizmet Dokusu, proje şablonları ve hata ayıklama, yapılandırma ve hem yerel hem de Azure'a dağıtma desteği yle Visual Studio ile entegre edilmiştir. Hem Bulut Hizmetleri hem de Hizmet Kumaşı aynı zamanda yerel bir geliştirme çalışma zamanı ortamı sağlar. Aradaki fark, Bulut Hizmeti geliştirme çalışma zamanı, çalıştığı Azure ortamını taklit ederken, Service Fabric emülatör kullanmaz - tam Hizmet Kumaşı çalışma süresini kullanır. Yerel geliştirme makinenizde çalıştırdığınız Servis Kumaşı ortamı, üretimde çalışan ortamla aynıdır.

## <a name="next-steps"></a>Sonraki adımlar
Hizmet Kumaşı Güvenilir Hizmetler ve Tüm Hizmet Kumaşı özelliklerinden nasıl yararlanabileceğinizi anlamak için Bulut Hizmetleri ve Hizmet Kumaşı uygulama mimarisi arasındaki temel farklar hakkında daha fazla bilgi edinin.

* [Servis Kumaşı Güvenilir Hizmetlerle Başlarken](service-fabric-reliable-services-quick-start.md)
* [Bulut Hizmetleri ve Hizmet Kumaşı arasındaki farklara kavramsal kılavuz](service-fabric-cloud-services-migration-differences.md)

<!--Image references-->
[3]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/service-fabric-cloud-service-projects.png
[4]: ./media/service-fabric-cloud-services-migration-worker-role-stateless-service/worker-role-to-stateless-service.png
