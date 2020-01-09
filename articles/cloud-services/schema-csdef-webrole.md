---
title: Azure Cloud Services def. WebRole şeması | Microsoft Docs
ms.custom: ''
ms.date: 04/14/2015
services: cloud-services
ms.reviewer: ''
ms.service: cloud-services
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: 85368e4e-a0db-4c02-8dbc-8e2928fa6091
caps.latest.revision: 60
author: tgore03
ms.author: tagore
ms.openlocfilehash: 1e4413f3495790e5e477f424bb5debcfbc186ade
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75385501"
---
# <a name="azure-cloud-services-definition-webrole-schema"></a>Azure Cloud Services Definition WebRole şeması
Azure Web rolü, IIS 7 tarafından desteklenen ASP.NET, PHP, Windows Communication Foundation ve FastCGI gibi Web uygulaması programlamasında özelleştirilmiş bir roldür.

Hizmet tanım dosyası için varsayılan uzantı. csdef ' dir.

## <a name="basic-service-definition-schema-for-a-web-role"></a>Web rolü için temel hizmet tanımı şeması  
Bir Web rolü içeren bir hizmet tanımı dosyasının temel biçimi aşağıdaki gibidir.

```xml
<ServiceDefinition …>  
  <WebRole name="<web-role-name>" vmsize="<web-role-size>" enableNativeCodeExecution="[true|false]">  
    <Certificates>  
      <Certificate name="<certificate-name>" storeLocation="<certificate-store>" storeName="<store-name>" />  
    </Certificates>      
    <ConfigurationSettings>  
      <Setting name="<setting-name>" />  
    </ConfigurationSettings>  
    <Imports>  
      <Import moduleName="<import-module>"/>  
    </Imports>  
    <Endpoints>  
      <InputEndpoint certificate="<certificate-name>" ignoreRoleInstanceStatus="[true|false]" name="<input-endpoint-name>" protocol="[http|https|tcp|udp]" localPort="<port-number>" port="<port-number>" loadBalancerProbe="<load-balancer-probe-name>" />  
      <InternalEndpoint name="<internal-endpoint-name>" protocol="[http|tcp|udp|any]" port="<port-number>">  
         <FixedPort port="<port-number>"/>  
         <FixedPortRange min="<minimum-port-number>" max="<maximum-port-number>"/>  
      </InternalEndpoint>  
     <InstanceInputEndpoint name="<instance-input-endpoint-name>" localPort="<port-number>" protocol="[udp|tcp]">  
         <AllocatePublicPortFrom>  
            <FixedPortRange min="<minimum-port-number>" max="<maximum-port-number>"/>  
         </AllocatePublicPortFrom>  
      </InstanceInputEndpoint>  
    </Endpoints>  
    <LocalResources>  
      <LocalStorage name="<local-store-name>" cleanOnRoleRecycle="[true|false]" sizeInMB="<size-in-megabytes>" />  
    </LocalResources>  
    <LocalStorage name="<local-store-name>" cleanOnRoleRecycle="[true|false]" sizeInMB="<size-in-megabytes>" />  
    <Runtime executionContext="[limited|elevated]">  
      <Environment>  
         <Variable name="<variable-name>" value="<variable-value>">  
            <RoleInstanceValue xpath="<xpath-to-role-environment-settings>"/>  
          </Variable>            
      </Environment>  
      <EntryPoint>  
         <NetFxEntryPoint assemblyName="<name-of-assembly-containing-entrypoint>" targetFrameworkVersion="<.net-framework-version>"/>  
      </EntryPoint>  
    </Runtime>  
    <Sites>  
      <Site name="<web-site-name>">  
        <VirtualApplication name="<application-name>" physicalDirectory="<directory-path>"/>  
        <VirtualDirectory name="<directory-path>" physicalDirectory="<directory-path>"/>  
        <Bindings>  
          <Binding name="<binding-name>" endpointName="<endpoint-name-bound-to>" hostHeader="<url-of-the-site>"/>  
        </Bindings>  
      </Site>  
    </Sites>  
    <Startup priority="<for-internal-use-only>">  
      <Task commandLine="<command-to=execute>" executionContext="[limited|elevated]" taskType="[simple|foreground|background]">  
        <Environment>  
         <Variable name="<variable-name>" value="<variable-value>">  
            <RoleInstanceValue xpath="<xpath-to-role-environment-settings>"/>  
          </Variable>            
        </Environment>  
      </Task>  
    </Startup>  
    <Contents>  
      <Content destination="<destination-folder-name>" >  
        <SourceDirectory path="<local-source-directory>" />  
      </Content>  
    </Contents>  
  </WebRole>  
</ServiceDefinition>  
```  

## <a name="schema-elements"></a>Şema öğeleri  
Hizmet tanımı dosyası, bu konunun sonraki bölümlerinde ayrıntılı olarak açıklanan bu öğeleri içerir:  

[WebRole](#WebRole)

[ConfigurationSettings](#ConfigurationSettings)

[Ayar](#Setting)

[LocalResources](#LocalResources)

[LocalStorage](#LocalStorage)

[Uç Noktalar](#Endpoints)

[InternalEndpoint](#InternalEndpoint)

[Instanceınputendpoint](#InstanceInputEndpoint)

[AllocatePublicPortFrom](#AllocatePublicPortFrom)

[FixedPort](#FixedPort)

[FixedPortRange](#FixedPortRange)

[Sertifikalar](#Certificates)

[Sertifika](#Certificate)

[İşlemlerinin](#Imports)

[İçeri Aktarma](#Import)

[Çalışma Zamanı](#Runtime)

[Ortam](#Environment)

[Değişken](#Variable)

[Roleınstancevalue](#RoleInstanceValue)

[NetFxEntryPoint](#NetFxEntryPoint)

[Barındıra](#Sites)

[Site](#Site)

[VirtualApplication](#VirtualApplication)

[VirtualApplication](#VirtualApplication)

[Bağlamalar](#Bindings)

[Bağlama](#Binding)

[Startup](#Startup)

[Görev](#Task)

[İçindekiler](#Contents)

[İçerik](#Content)

[SourceDirectory](#SourceDirectory)

##  <a name="WebRole"></a>WebRole  
`WebRole` öğesi, IIS 7 ve ASP.NET tarafından desteklendiği şekilde Web uygulaması programlama için özelleştirilmiş bir rol tanımlar. Bir hizmet, sıfır veya daha fazla web rolü içerebilir.

Aşağıdaki tabloda `WebRole` öğesinin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|ad|string|Gereklidir. Web rolünün adı. Rolün adı benzersiz olmalıdır.|  
|enableNativeCodeExecution|boole|İsteğe bağlı. Varsayılan değer `true`; Yerel kod yürütme ve tam güven varsayılan olarak etkindir. Web rolü için yerel kod yürütmeyi devre dışı bırakmak üzere bu özniteliği `false` olarak ayarlayın ve bunun yerine Azure kısmi güven kullanın.|  
|VMSize|string|İsteğe bağlı. Role ayrılan sanal makinenin boyutunu değiştirmek için bu değeri ayarlayın. Varsayılan değer: `Small`. Daha fazla bilgi için bkz. [sanal makine boyutları Cloud Services](cloud-services-sizes-specs.md).|  

##  <a name="ConfigurationSettings"></a>ConfigurationSettings  
`ConfigurationSettings` öğesi, bir Web rolü için yapılandırma ayarları koleksiyonunu açıklar. Bu öğe `Setting` öğesinin üst öğesidir.

##  <a name="Setting"></a>Ayarlanmasını  
`Setting` öğesi, bir rolün örneği için yapılandırma ayarını belirten bir ad ve değer çiftini açıklar.

Aşağıdaki tabloda `Setting` öğesinin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|ad|string|Gereklidir. Yapılandırma ayarı için benzersiz bir ad.|  

Bir rolün yapılandırma ayarları, hizmet tanım dosyasında belirtilen ve hizmet yapılandırma dosyasında ayarlanan ad ve değer çiftleridir.

##  <a name="LocalResources"></a>LocalResources  
`LocalResources` öğesi, bir Web rolü için yerel depolama kaynaklarının toplanmasını açıklar. Bu öğe `LocalStorage` öğesinin üst öğesidir.

##  <a name="LocalStorage"></a>LocalStorage  
`LocalStorage` öğesi, çalışma zamanında hizmet için dosya sistemi alanı sağlayan yerel bir depolama kaynağını tanımlar. Rol, sıfır veya daha fazla yerel depolama kaynağı tanımlayabilir.

> [!NOTE]
>  `LocalStorage` öğesi, Azure SDK 'nın önceki sürümleriyle uyumluluğunu desteklemek için `WebRole` öğesinin bir alt öğesi olarak görünebilir.

Aşağıdaki tabloda `LocalStorage` öğesinin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|ad|string|Gereklidir. Yerel depo için benzersiz bir ad.|  
|cleanOnRoleRecycle|boole|İsteğe bağlı. Rol yeniden başlatıldığında yerel deponun temizlenmesinin gerekip gerekmediğini gösterir. Varsayılan değer `true`.|  
|Sizeınmb|int|İsteğe bağlı. Yerel depo için ayrılacak, MB cinsinden istenen depolama alanı miktarı. Belirtilmemişse, ayrılan varsayılan depolama alanı 100 MB 'tır. Ayrılabilen en az depolama alanı miktarı 1 MB 'tır.<br /><br /> Yerel kaynakların en büyük boyutu sanal makine boyutuna bağımlıdır. Daha fazla bilgi için bkz. [sanal makine boyutları Cloud Services](cloud-services-sizes-specs.md).|  
  
Yerel depolama kaynağına ayrılan dizinin adı, Name özniteliği için belirtilen değere karşılık gelir.

##  <a name="Endpoints"></a>Noktalarının  
`Endpoints` öğesi, bir rol için giriş (dış), iç ve örnek giriş uç noktaları koleksiyonunu açıklar. Bu öğe `InputEndpoint`, `InternalEndpoint`ve `InstanceInputEndpoint` öğelerinin üst öğesidir.

Giriş ve Iç uç noktalar ayrı olarak ayrılır. Bir hizmette toplam 25 giriş, iç ve örnek giriş uç noktası bulunabilir ve bu da bir hizmette izin verilen 25 rol arasında ayrılabilir. Örneğin, 5 rol varsa, rol başına 5 giriş uç noktası ayırabilirsiniz veya tek bir role 25 giriş uç noktası ayırabilirsiniz veya her biri 25 rol için 1 giriş uç noktası ayırabilirsiniz.

> [!NOTE]
>  Dağıtılan her rolün rol başına bir örnek olması gerekir. Abonelik için varsayılan sağlama 20 çekirdekle sınırlıdır ve bu nedenle bir rolün 20 örneğiyle sınırlıdır. Uygulamanız varsayılan sağlama tarafından sağlanmasından daha fazla örnek gerektiriyorsa, kotayı artırma hakkında daha fazla bilgi için [faturalandırma, abonelik yönetimi ve kota desteği '](https://azure.microsoft.com/support/options/) ne bakın.

##  <a name="InputEndpoint"></a>Inputendpoint  
`InputEndpoint` öğesi bir Web rolü için dış uç noktayı açıklar.

HTTP, HTTPS, UDP ve TCP uç noktalarının birleşimi olan birden fazla uç nokta tanımlayabilirsiniz. Bir giriş uç noktası için seçtiğiniz herhangi bir bağlantı noktası numarasını belirtebilirsiniz, ancak hizmette her rol için belirtilen bağlantı noktası numaraları benzersiz olmalıdır. Örneğin, bir Web rolünün HTTP için 80 bağlantı noktasını ve HTTPS için bağlantı noktası 443 kullandığını belirtirseniz, ikinci bir Web rolünün HTTP için bağlantı noktası 8080 ' i ve HTTPS için bağlantı noktası 8043 kullandığını belirtebilirsiniz.

Aşağıdaki tabloda `InputEndpoint` öğesinin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|ad|string|Gereklidir. Dış uç nokta için benzersiz bir ad.|  
|protocol|string|Gereklidir. Dış uç nokta için Aktarım Protokolü. Bir Web rolü için olası değerler `HTTP`, `HTTPS`, `UDP`veya `TCP`.|  
|port|int|Gereklidir. Dış uç nokta için bağlantı noktası. Seçtiğiniz herhangi bir bağlantı noktası numarasını belirtebilirsiniz, ancak hizmette her bir rol için belirtilen bağlantı noktası numaraları benzersiz olmalıdır.<br /><br /> Olası değerler 1 ile 65535 (Azure SDK sürüm 1,7 veya üzeri) arasında değişir.|  
|sertifika|string|Bir HTTPS uç noktası için gereklidir. Bir `Certificate` öğesi tarafından tanımlanan sertifikanın adı.|  
|localPort|int|İsteğe bağlı. Uç noktada iç bağlantılar için kullanılan bir bağlantı noktasını belirtir. `localPort` özniteliği, bir roldeki iç bağlantı noktası ile uç noktasındaki dış bağlantı noktasını eşleştirir. Bu, bir rolün dışarıdan açığa çıkarılan bir bağlantı noktasında iç bileşen ile iletişim kurması gereken senaryolarda yararlıdır.<br /><br /> Belirtilmemişse, `localPort` değeri `port` özniteliğiyle aynı olur. Çalışma zamanı API 'SI kullanılarak keşfedilen ayrılmamış bir bağlantı noktasını otomatik olarak atamak için `localPort` değerini "*" olarak ayarlayın.<br /><br /> Olası değerler 1 ile 65535 (Azure SDK sürüm 1,7 veya üzeri) arasında değişir.<br /><br /> `localPort` özniteliği yalnızca Azure SDK 1,3 veya üzeri sürümler kullanılarak kullanılabilir.|  
|ignoreRoleInstanceStatus|boole|İsteğe bağlı. Bu özniteliğin değeri `true`olarak ayarlandığında, bir hizmetin durumu yok sayılır ve uç nokta yük dengeleyici tarafından kaldırılmaz. Bu değeri, bir hizmetin meşgul örneklerinde hata ayıklama için yararlı `true` olarak ayarlama. Varsayılan değer: `false`. **Note:**  Bir uç nokta, rol kullanılmaya devam eden bir durumda olsa bile trafiği alabilir.|  
|loadBalancerProbe|string|İsteğe bağlı. Giriş uç noktasıyla ilişkili yük dengeleyici araştırmasının adı. Daha fazla bilgi için bkz. [Loadbalanceraraştırma şeması](schema-csdef-loadbalancerprobe.md).|  

##  <a name="InternalEndpoint"></a>InternalEndpoint  
`InternalEndpoint` öğesi bir Web rolüne iç uç nokta tanımlar. İç uç nokta yalnızca hizmet içinde çalışan diğer rol örnekleri için kullanılabilir; hizmet dışındaki istemciler için kullanılamaz. `Sites` öğesi içermeyen Web rollerinin yalnızca tek bir HTTP, UDP veya TCP iç uç noktası olabilir.

Aşağıdaki tabloda `InternalEndpoint` öğesinin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|ad|string|Gereklidir. İç uç nokta için benzersiz bir ad.|  
|protocol|string|Gereklidir. İç uç nokta için Aktarım Protokolü. Olası değerler `HTTP`, `TCP`, `UDP`veya `ANY`.<br /><br /> `ANY` değeri herhangi bir protokolün, herhangi bir bağlantı noktasına izin verildiğini belirtir.|  
|port|int|İsteğe bağlı. Uç noktada iç yük dengeli bağlantılar için kullanılan bağlantı noktası. Yük dengeli bir uç nokta iki bağlantı noktası kullanır. Genel IP adresi için kullanılan bağlantı noktası ve özel IP adresinde kullanılan bağlantı noktası. Bunlar genellikle bunlar aynı şekilde ayarlanmıştır, ancak farklı bağlantı noktaları kullanmayı seçebilirsiniz.<br /><br /> Olası değerler 1 ile 65535 (Azure SDK sürüm 1,7 veya üzeri) arasında değişir.<br /><br /> `Port` özniteliği yalnızca Azure SDK 1,3 veya üzeri sürümler kullanılarak kullanılabilir.|  

##  <a name="InstanceInputEndpoint"></a>Instanceınputendpoint  
`InstanceInputEndpoint` öğesi bir Web rolüne örnek giriş uç noktası tanımlar. Örnek giriş uç noktası, yük dengeleyicide bağlantı noktası iletme kullanılarak belirli bir rol örneğiyle ilişkilendirilir. Her örnek giriş uç noktası, olası bir bağlantı noktası aralığından belirli bir bağlantı noktasıyla eşleştirilir. Bu öğe `AllocatePublicPortFrom` öğesinin üst öğesidir.

`InstanceInputEndpoint` öğesi yalnızca Azure SDK 1,7 veya üzeri sürümler kullanılarak kullanılabilir.

Aşağıdaki tabloda `InstanceInputEndpoint` öğesinin öznitelikleri açıklanmaktadır.
  
| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|ad|string|Gereklidir. Uç nokta için benzersiz bir ad.|  
|localPort|int|Gereklidir. Yük dengeleyiciden iletilen gelen trafiği almak için tüm rol örneklerinin dinleyeceği iç bağlantı noktasını belirtir. Olası değerler 1 ile 65535 (dahil) arasında değişir.|  
|protocol|string|Gereklidir. İç uç nokta için Aktarım Protokolü. Olası değerler: `udp` veya `tcp`. Http/https tabanlı trafik için `tcp` kullanın.|  
  
##  <a name="AllocatePublicPortFrom"></a>AllocatePublicPortFrom  
`AllocatePublicPortFrom` öğesi, dış müşteriler tarafından her örnek giriş uç noktasına erişmek için kullanılabilen genel bağlantı noktası aralığını açıklar. Ortak (VIP) bağlantı noktası numarası bu aralıktan ayrılır ve kiracı dağıtımı ve güncelleştirmesi sırasında her bir rol örneği uç noktasına atanır. Bu öğe `FixedPortRange` öğesinin üst öğesidir.

`AllocatePublicPortFrom` öğesi yalnızca Azure SDK 1,7 veya üzeri sürümler kullanılarak kullanılabilir.

##  <a name="FixedPort"></a>FixedPort  
`FixedPort` öğesi iç uç nokta için uç noktada yük dengeli bağlantıları sağlayan bağlantı noktasını belirtir.

`FixedPort` öğesi yalnızca Azure SDK 1,3 veya üzeri sürümler kullanılarak kullanılabilir.

Aşağıdaki tabloda `FixedPort` öğesinin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|port|int|Gereklidir. İç uç nokta için bağlantı noktası. Bu, `FixedPortRange` min ve Max değerini aynı bağlantı noktasına ayarlamayla aynı etkiye sahiptir.<br /><br /> Olası değerler 1 ile 65535 (Azure SDK sürüm 1,7 veya üzeri) arasında değişir.|  

##  <a name="FixedPortRange"></a>FixedPortRange  
`FixedPortRange` öğesi, iç uç noktaya veya örnek giriş uç noktasına atanan bağlantı noktası aralığını belirtir ve uç noktada yük dengeli bağlantılar için kullanılan bağlantı noktasını ayarlar.

> [!NOTE]
>  `FixedPortRange` öğesi, bulunduğu öğeye göre farklı şekilde çalışır. `FixedPortRange` öğesi `InternalEndpoint` öğesinde olduğunda, yük dengeleyicideki tüm bağlantı noktalarını, rolün çalıştığı tüm sanal makineler için min ve Max özniteliklerinin aralığı içinde açar. `FixedPortRange` öğesi `InstanceInputEndpoint` öğesinde olduğunda, rolü çalıştıran her bir sanal makinede en az ve en fazla öznitelik aralığı içinde yalnızca bir bağlantı noktası açılır.

`FixedPortRange` öğesi yalnızca Azure SDK 1,3 veya üzeri sürümler kullanılarak kullanılabilir.

Aşağıdaki tabloda `FixedPortRange` öğesinin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|dk|int|Gereklidir. Aralıktaki en küçük bağlantı noktası. Olası değerler 1 ile 65535 (Azure SDK sürüm 1,7 veya üzeri) arasında değişir.|  
|en büyük|string|Gereklidir. Aralıktaki en fazla bağlantı noktası. Olası değerler 1 ile 65535 (Azure SDK sürüm 1,7 veya üzeri) arasında değişir.|  

##  <a name="Certificates"></a>Sertifika  
`Certificates` öğesi bir Web rolü için sertifika koleksiyonunu açıklar. Bu öğe `Certificate` öğesinin üst öğesidir. Bir rol, ilişkili sayıda sertifikaya sahip olabilir. Sertifikalar öğesini kullanma hakkında daha fazla bilgi için bkz. [hizmet tanımı dosyasını bir sertifikayla değiştirme](cloud-services-configure-ssl-certificate-portal.md#step-2-modify-the-service-definition-and-configuration-files).

##  <a name="Certificate"></a>Sertifika  
`Certificate` öğesi, bir Web rolüyle ilişkili bir sertifikayı açıklar.

Aşağıdaki tabloda `Certificate` öğesinin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|ad|string|Gereklidir. Bir HTTPS `InputEndpoint` öğesiyle ilişkilendirildiğinde bu sertifikaya başvurmak için kullanılan bir ad.|  
|storeLocation|string|Gereklidir. Bu sertifikanın yerel makinede bulunabileceği sertifika deposunun konumu. Olası değerler `CurrentUser` ve `LocalMachine`.|  
|storeName|string|Gereklidir. Bu sertifikanın yerel makinede bulunduğu sertifika deposunun adı. Olası değerler `My`, `Root`, `CA`, `Trust`, `Disallowed`, `TrustedPeople`, `TrustedPublisher`, `AuthRoot`, `AddressBook`veya herhangi bir özel mağaza adının yerleşik mağaza adlarını içerir. Özel bir mağaza adı belirtilmişse mağaza otomatik olarak oluşturulur.|  
|permissionLevel|string|İsteğe bağlı. Rol işlemlerine verilen erişim izinlerini belirtir. Yalnızca yükseltilmiş işlemlerin özel anahtara erişip erişebilmesini istiyorsanız `elevated` izin belirtin. `limitedOrElevated` izin, tüm rol işlemlerinin özel anahtara erişmesine izin verir. Olası değerler: `limitedOrElevated` veya `elevated`. Varsayılan değer: `limitedOrElevated`.|  

##  <a name="Imports"></a>İşlemlerinin  
`Imports` öğesi, Konuk işletim sistemine bileşenler ekleyen bir Web rolü için içeri aktarma modülleri koleksiyonunu açıklar. Bu öğe `Import` öğesinin üst öğesidir. Bu öğe isteğe bağlıdır ve bir rol yalnızca bir içeri aktarma bloğuna sahip olabilir. 

`Imports` öğesi yalnızca Azure SDK 1,3 veya üzeri sürümler kullanılarak kullanılabilir.

##  <a name="Import"></a>Aktarmaya  
`Import` öğesi, Konuk işletim sistemine eklenecek bir modül belirtir.

`Import` öğesi yalnızca Azure SDK 1,3 veya üzeri sürümler kullanılarak kullanılabilir.

Aşağıdaki tabloda `Import` öğesinin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|moduleName|string|Gereklidir. İçeri aktarılacak modülün adı. Geçerli içeri aktarma modülleri şunlardır:<br /><br /> -RemoteAccess<br />-RemoteForwarder<br />-Tanılama<br /><br /> RemoteAccess ve RemoteForwarder modülleri, rol örneğinizi Uzak Masaüstü bağlantıları için yapılandırmanıza olanak tanır. Daha fazla bilgi için bkz. [Uzak Masaüstü bağlantısı etkinleştirme](cloud-services-role-enable-remote-desktop-new-portal.md).<br /><br /> Tanılama modülü bir rol örneği için tanılama verileri toplamanızı sağlar.|  

##  <a name="Runtime"></a>Çalışma zamanı  
`Runtime` öğesi, Azure ana bilgisayar işleminin çalışma zamanı ortamını denetleyen bir Web rolü için ortam değişkeni ayarlarının bir koleksiyonunu açıklar. Bu öğe `Environment` öğesinin üst öğesidir. Bu öğe isteğe bağlıdır ve bir rol yalnızca bir çalışma zamanı bloğuna sahip olabilir.

`Runtime` öğesi yalnızca Azure SDK 1,3 veya üzeri sürümler kullanılarak kullanılabilir.

Aşağıdaki tablo `Runtime` öğesinin özniteliklerini açıklar:  

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|executionContext|string|İsteğe bağlı. Rol Işleminin başlatıldığı bağlamı belirtir. Varsayılan bağlam `limited`.<br /><br /> -   `limited` – işlem yönetici ayrıcalıkları olmadan başlatılır.<br />-   `elevated` – işlem, yönetici ayrıcalıklarıyla başlatılır.|  

##  <a name="Environment"></a>Ortamınızın  
`Environment` öğesi, bir Web rolü için ortam değişkeni ayarlarının bir koleksiyonunu açıklar. Bu öğe `Variable` öğesinin üst öğesidir. Rol herhangi bir sayıda ortam değişkeni kümesine sahip olabilir.

##  <a name="Variable"></a>Değişken  
`Variable` öğesi, Konuk işletim içinde ayarlanacak bir ortam değişkenini belirtir.

`Variable` öğesi yalnızca Azure SDK 1,3 veya üzeri sürümler kullanılarak kullanılabilir.

Aşağıdaki tablo `Variable` öğesinin özniteliklerini açıklar:  

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|ad|string|Gereklidir. Ayarlanacak ortam değişkeninin adı.|  
|değer|string|İsteğe bağlı. Ortam değişkeni için ayarlanacak değer. Bir değer özniteliği veya `RoleInstanceValue` öğesi dahil etmeniz gerekir.|  

##  <a name="RoleInstanceValue"></a>Roleınstancevalue  
`RoleInstanceValue` öğesi, değişkenin değerinin alınacağı xPath 'i belirtir.

Aşağıdaki tabloda `RoleInstanceValue` öğesinin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|XPath|string|İsteğe bağlı. Örnek için dağıtım ayarlarının konum yolu. Daha fazla bilgi için bkz. [XPath Ile yapılandırma değişkenleri](cloud-services-role-config-xpath.md).<br /><br /> Bir değer özniteliği veya `RoleInstanceValue` öğesi dahil etmeniz gerekir.|  

##  <a name="EntryPoint"></a>Noktası  
`EntryPoint` öğesi bir rolün giriş noktasını belirtir. Bu öğe `NetFxEntryPoint` öğelerinin üst öğesidir. Bu öğeler, rol giriş noktası olarak görev yapmak için varsayılan WaWorkerHost. exe dışında bir uygulama belirtmenize olanak tanır.

`EntryPoint` öğesi yalnızca Azure SDK 1,5 veya üzeri sürümler kullanılarak kullanılabilir.

##  <a name="NetFxEntryPoint"></a>NetFxEntryPoint  
`NetFxEntryPoint` öğesi, bir rol için çalıştırılacak programı belirtir.

> [!NOTE]
>  `NetFxEntryPoint` öğesi yalnızca Azure SDK 1,5 veya üzeri sürümler kullanılarak kullanılabilir.

Aşağıdaki tabloda `NetFxEntryPoint` öğesinin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|{1&gt;derlemeAdı&lt;1}|string|Gereklidir. Giriş noktasını içeren derlemenin yolu ve dosya adı. Yol, **\\%roleroot%\approot** (`commandLine` **\\%roleroot%\approot** ' i belirtmeyin, kabul edilir). **% Roleroot%** , Azure tarafından tutulan bir ortam değişkenidir ve rolünüz için kök klasör konumunu temsil eder. **\\%ROLEROOT%\Approot** klasörü, rolünüzün uygulama klasörünü temsil eder.<br /><br /> HWC rolleri için yol her zaman **\\%ROLEROOT%\Approot\bin** klasörüne görelidir.<br /><br /> Tam IIS ve IIS Express Web rolleri için, derleme **\\%ROLEROOT%\Approot** klasörüne göre bulunamıyorsa, **\\%ROLEROOT%\Approot\bin araması** yapılır.<br /><br /> Tam IIS için bu geri dönüş davranışı önerilen bir uygulama değildir ve gelecekteki sürümlerde kaldırılmış olabilir.|  
|targetFrameworkVersion|string|Gereklidir. Derlemenin oluşturulduğu .NET Framework sürümü. Örneğin, `targetFrameworkVersion="v4.0"`.|  

##  <a name="Sites"></a>Barındıra  
`Sites` öğesi, bir Web rolünde barındırılan Web siteleri ve Web uygulamaları koleksiyonunu açıklar. Bu öğe `Site` öğesinin üst öğesidir. Bir `Sites` öğesi belirtmezseniz, Web rolünüzün eski web rolü olarak barındırıldığı ve Web rolünüzde barındırılan bir Web siteniz olabilir. Bu öğe isteğe bağlıdır ve bir rol yalnızca bir siteler bloğuna sahip olabilir.

`Sites` öğesi yalnızca Azure SDK 1,3 veya üzeri sürümler kullanılarak kullanılabilir.

##  <a name="Site"></a>Bölgesi  
`Site` öğesi, Web rolünün bir parçası olan bir Web sitesini veya Web uygulamasını belirtir.

`Site` öğesi yalnızca Azure SDK 1,3 veya üzeri sürümler kullanılarak kullanılabilir.

Aşağıdaki tabloda `Site` öğesinin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|ad|string|Gereklidir. Web sitesinin veya uygulamanın adı.|  
|physicalDirectory|string|Site kökünün içerik dizininin konumu. Konum, mutlak bir yol veya. csdef konumuna göre belirtilebilir.|  

##  <a name="VirtualApplication"></a>VirtualApplication  
`VirtualApplication` öğesi Internet Information Services (IIS) 7 ' de bir uygulamayı tanımlar, içerik teslim eden veya HTTP gibi protokoller üzerinden hizmetler sağlayan bir dosya grubudur. IIS 7 ' de bir uygulama oluşturduğunuzda, uygulamanın yolu site URL 'sinin bir parçası haline gelir.

`VirtualApplication` öğesi yalnızca Azure SDK 1,3 veya üzeri sürümler kullanılarak kullanılabilir.

Aşağıdaki tabloda `VirtualApplication` öğesinin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|ad|string|Gereklidir. Sanal uygulamayı tanımlayacak bir ad belirtir.|  
|physicalDirectory|string|Gereklidir. Geliştirme makinesindeki sanal uygulamayı içeren yolu belirtir. İşlem öykünücüsünde IIS bu konumdan içerik almak üzere yapılandırılmıştır. Azure 'a dağıtım yaparken, fiziksel dizin içeriği hizmetin geri kalanı ile birlikte paketlenir. Hizmet paketi Azure 'a dağıtıldığında, IIS, paketi açılan içeriklerin konumuyla yapılandırılır.|  

##  <a name="VirtualDirectory"></a>Dizin  
`VirtualDirectory` öğesi, IIS 'de belirttiğiniz bir dizin adını (yol olarak da anılır) belirtir ve yerel veya uzak bir sunucudaki fiziksel bir dizine eşleyin.

`VirtualDirectory` öğesi yalnızca Azure SDK 1,3 veya üzeri sürümler kullanılarak kullanılabilir.

Aşağıdaki tabloda `VirtualDirectory` öğesinin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|ad|string|Gereklidir. Sanal dizini tanımlayacak bir ad belirtir.|  
|değer|physicalDirectory|Gereklidir. Geliştirme makinesindeki Web sitesini veya sanal dizin içeriğini içeren yolu belirtir. İşlem öykünücüsünde IIS bu konumdan içerik almak üzere yapılandırılmıştır. Azure 'a dağıtım yaparken, fiziksel dizin içeriği hizmetin geri kalanı ile birlikte paketlenir. Hizmet paketi Azure 'a dağıtıldığında, IIS, paketi açılan içeriklerin konumuyla yapılandırılır.|  

##  <a name="Bindings"></a>Lara  
`Bindings` öğesi, bir Web sitesi için bağlama koleksiyonunu açıklar. `Binding` öğesinin üst öğesidir. Her `Site` öğesi için öğesi gereklidir. Uç noktaları yapılandırma hakkında daha fazla bilgi için bkz. [rol örnekleri Için Iletişimi etkinleştirme](cloud-services-enable-communication-role-instances.md).

`Bindings` öğesi yalnızca Azure SDK 1,3 veya üzeri sürümler kullanılarak kullanılabilir.

##  <a name="Binding"></a>Bağlayıcısı  
`Binding` öğesi, bir Web sitesi veya Web uygulamasıyla iletişim kurmak için istekler için gereken yapılandırma bilgilerini belirtir.

`Binding` öğesi yalnızca Azure SDK 1,3 veya üzeri sürümler kullanılarak kullanılabilir.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|ad|string|Gereklidir. Bağlamayı tanımlayacak bir ad belirtir.|  
|Uçnoktaadı|string|Gereklidir. Bağlanacak bitiş noktası adını belirtir.|  
|hostHeader|string|İsteğe bağlı. Tek bir IP adresi/bağlantı noktası numarası birleşimine sahip birden fazla siteyi, farklı ana bilgisayar adlarıyla barındırmanıza olanak tanıyan bir ana bilgisayar adı belirtir.|  

##  <a name="Startup"></a>Başlangıç  
`Startup` öğesi, rol başlatıldığında çalışan görevlerin koleksiyonunu açıklar. Bu öğe `Variable` öğesinin üst öğesi olabilir. Rol başlangıç görevlerini kullanma hakkında daha fazla bilgi için bkz. [başlangıç görevlerini yapılandırma](cloud-services-startup-tasks.md). Bu öğe isteğe bağlıdır ve bir rol yalnızca bir başlangıç bloğuna sahip olabilir.

Aşağıdaki tabloda `Startup` öğesinin özniteliği açıklanmaktadır.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|öncelik|int|Yalnızca dahili kullanım içindir.|  

##  <a name="Task"></a>Görevinin  
`Task` öğesi, rol başladığında gerçekleşen başlangıç görevini belirler. Başlangıç görevleri, rolü bu Install yazılım bileşenlerini çalıştırmak veya diğer uygulamaları çalıştırmak için hazırlayan görevleri gerçekleştirmek için kullanılabilir. Görevler `Startup` öğesi bloğunda göründükleri sırada yürütülür.

`Task` öğesi yalnızca Azure SDK 1,3 veya üzeri sürümler kullanılarak kullanılabilir.

Aşağıdaki tabloda `Task` öğesinin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|Komut satırı|string|Gereklidir. Çalıştırılacak komutları içeren bir komut dosyası (örneğin, CMD dosyası). Başlangıç komutu ve toplu iş dosyalarının ANSI biçiminde kaydedilmesi gerekir. Dosyanın başlangıcında bir bayt düzeni işaretçisi ayarlanan dosya biçimleri düzgün şekilde çalışmaz.|  
|executionContext|string|Betiğin çalıştırıldığı bağlamı belirtir.<br /><br /> -   `limited` [varsayılan] – işlemi barındıran rolle aynı ayrıcalıklarla çalıştırın.<br />-   `elevated` – yönetici ayrıcalıklarıyla çalıştırın.|  
|taskType|string|Komutun yürütme davranışını belirtir.<br /><br /> -   `simple` [varsayılan] – sistem, diğer tüm görevler başlatılmadan önce görevin çıkmasını bekler.<br />-   `background` – sistem, görevin çıkış gelmesini beklemez.<br />-   `foreground`-arka plana benzer, tüm ön plan görevleri çıkış yapılıncaya kadar rol yeniden başlatılmaz.|  

##  <a name="Contents"></a>Dekiler  
`Contents` öğesi bir Web rolü için içerik koleksiyonunu açıklar. Bu öğe `Content` öğesinin üst öğesidir.

`Contents` öğesi yalnızca Azure SDK 1,5 veya üzeri sürümler kullanılarak kullanılabilir.

##  <a name="Content"></a>İçeriði  
`Content` öğesi, Azure sanal makinesine kopyalanacak içeriğin kaynak konumunu ve kopyalandığı hedef yolu tanımlar.

`Content` öğesi yalnızca Azure SDK 1,5 veya üzeri sürümler kullanılarak kullanılabilir.

Aşağıdaki tabloda `Content` öğesinin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|destination|string|Gereklidir. Azure sanal makinesinde içeriğin yerleştirildiği konum. Bu konum **%Roleroot%\approot**klasörüne göredir.|  

Bu öğe `SourceDirectory` öğesinin üst öğesidir.

##  <a name="SourceDirectory"></a>SourceDirectory  
`SourceDirectory` öğesi içeriğin kopyalandığı yerel dizini tanımlar. Azure sanal makinesine kopyalanacak yerel içeriği belirtmek için bu öğeyi kullanın.

`SourceDirectory` öğesi yalnızca Azure SDK 1,5 veya üzeri sürümler kullanılarak kullanılabilir.

Aşağıdaki tabloda `SourceDirectory` öğesinin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |  
| --------- | ---- | ----------- |  
|yol|string|Gereklidir. İçerikleri Azure sanal makinesine kopyalanacak olan yerel bir dizinin göreli veya mutlak yolu. Dizin yolundaki ortam değişkenlerinin genişletilmesi desteklenir.|  
  
## <a name="see-also"></a>Ayrıca Bkz.
[Bulut hizmeti (klasik) Tanım Şeması](schema-csdef-file.md)




