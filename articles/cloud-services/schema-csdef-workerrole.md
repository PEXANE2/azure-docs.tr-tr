---
title: Azure Bulut Hizmetleri Def. WorkerRole Şema | Microsoft Dokümanlar
description: Azure çalışanı rolü genelleştirilmiş geliştirme için kullanılır ve bir web rolü için arka plan işleme gerçekleştirebilir. Azure işçi rol şeması hakkında bilgi edinin.
services: cloud-services
ms.custom: ''
ms.date: 04/14/2015
ms.reviewer: ''
ms.service: cloud-services
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: 41cd46bc-c479-43fa-96e5-d6c83e4e6d89
caps.latest.revision: 55
author: tgore03
ms.author: tagore
ms.openlocfilehash: 26225442c72fb209bb1ac4cd2bf4777fb39542fb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79534380"
---
# <a name="azure-cloud-services-definition-workerrole-schema"></a>Azure Bulut Hizmetleri Tanımı İşçiRol Şeması
Azure çalışanı rolü, genelleştirilmiş geliştirme için yararlı olan ve bir web rolü için arka plan işleme gerçekleştirebilecek bir roldür.

Hizmet tanımı dosyasının varsayılan uzantısı .csdef'tir.

## <a name="basic-service-definition-schema-for-a-worker-role"></a>Bir işçi rolü için temel hizmet tanımı şeması.
İşçi rolünü içeren hizmet tanımı dosyasının temel biçimi aşağıdaki gibidir.

```xml
<ServiceDefinition …>
  <WorkerRole name="<worker-role-name>" vmsize="<worker-role-size>" enableNativeCodeExecution="[true|false]">
    <Certificates>
      <Certificate name="<certificate-name>" storeLocation="[CurrentUser|LocalMachine]" storeName="[My|Root|CA|Trust|Disallow|TrustedPeople|TrustedPublisher|AuthRoot|AddressBook|<custom-store>" />
    </Certificates>
    <ConfigurationSettings>
      <Setting name="<setting-name>" />
    </ConfigurationSettings>
    <Endpoints>
      <InputEndpoint name="<input-endpoint-name>" protocol="[http|https|tcp|udp]" localPort="<local-port-number>" port="<port-number>" certificate="<certificate-name>" loadBalancerProbe="<load-balancer-probe-name>" />
      <InternalEndpoint name="<internal-endpoint-name" protocol="[http|tcp|udp|any]" port="<port-number>">
         <FixedPort port="<port-number>"/>
         <FixedPortRange min="<minimum-port-number>" max="<maximum-port-number>"/>
      </InternalEndpoint>
     <InstanceInputEndpoint name="<instance-input-endpoint-name>" localPort="<port-number>" protocol="[udp|tcp]">
         <AllocatePublicPortFrom>
            <FixedPortRange min="<minimum-port-number>" max="<maximum-port-number>"/>
         </AllocatePublicPortFrom>
      </InstanceInputEndpoint>
    </Endpoints>
    <Imports>
      <Import moduleName="[RemoteAccess|RemoteForwarder|Diagnostics]"/>
    </Imports>
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
         <ProgramEntryPoint commandLine="<application>" setReadyOnProcessStart="[true|false]"/>
      </EntryPoint>
    </Runtime>
    <Startup priority="<for-internal-use-only>">
      <Task commandLine="" executionContext="[limited|elevated]" taskType="[simple|foreground|background]">
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
  </WorkerRole>
</ServiceDefinition>
```

## <a name="schema-elements"></a>Şema Elemanları
Hizmet tanımı dosyası, bu konuda sonraki bölümlerde ayrıntılı olarak açıklanan bu öğeleri içerir:

[İşçi Rolü](#WorkerRole)

[Configurationsettings](#ConfigurationSettings)

[Ayar](#Setting)

[Yerel Kaynaklar](#LocalResources)

[Localstorage](#LocalStorage)

[Uç Noktalar](#Endpoints)

[GirişEndpoint](#InputEndpoint)

[InternalEndpoint](#InternalEndpoint)

[InstanceInputEndpoint](#InstanceInputEndpoint)

[TahsisPublicPortFrom](#AllocatePublicPortFrom)

[Sabit Bağlantı Noktası](#FixedPort)

[Sabit PortAralığı](#FixedPortRange)

[Sertifikalar](#Certificates)

[Sertifika](#Certificate)

[Ithalat](#Imports)

[İçeri Aktarma](#Import)

[Çalışma Zamanı](#Runtime)

[Ortam](#Environment)

[EntryPoint](#EntryPoint)

[NetfxentryPoint](#NetFxEntryPoint)

[ProgramentryPoint](#ProgramEntryPoint)

[Değişken](#Variable)

[RoleInstanceValue](#RoleInstanceValue)

[Başlangıç](#Startup)

[Görev](#Task)

[İçindekiler](#Contents)

[İçerik](#Content)

[Kaynak Dizin](#SourceDirectory)

##  <a name="workerrole"></a><a name="WorkerRole"></a>İşçi Rolü
Öğe, `WorkerRole` genelleştirilmiş geliştirme için yararlı olan ve bir web rolü için arka plan işleme gerçekleştirebilecek bir rolü açıklar. Bir hizmet sıfır veya daha fazla alt rol içerebilir.

Aşağıdaki tabloda `WorkerRole` öğenin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|ad|string|Gereklidir. İşçi rolünün adı. Rolün adı benzersiz olmalıdır.|
|enableNativeCodeExecution|boole|İsteğe bağlı. Varsayılan değer; `true` yerel kod yürütme ve tam güven varsayılan olarak etkinleştirilir. Bu özniteliği, `false` alt rol için yerel kod yürütmesini devre dışı bırakacak şekilde ayarlayın ve bunun yerine Azure kısmi güven'i kullanın.|
|vmsize|string|İsteğe bağlı. Bu role ayrılan sanal makinenin boyutunu değiştirmek için bu değeri ayarlayın. Varsayılan değer: `Small`. Olası sanal makine boyutları nın ve özniteliklerinin bir listesi [için Bulut Hizmetleri için Sanal Makine boyutlarına](cloud-services-sizes-specs.md)bakın.|

##  <a name="configurationsettings"></a><a name="ConfigurationSettings"></a>Configurationsettings
Öğe, `ConfigurationSettings` bir alt rol için yapılandırma ayarlarının toplanmasını açıklar. Bu öğe, `Setting` öğenin üst öğesidir.

##  <a name="setting"></a><a name="Setting"></a>Ayarı
Öğe, `Setting` bir rol örneği için bir yapılandırma ayarını belirten bir ad ve değer çiftini açıklar.

Aşağıdaki tabloda `Setting` öğenin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|ad|string|Gereklidir. Yapılandırma ayarı için benzersiz bir ad.|

Bir rolün yapılandırma ayarları, hizmet tanımı dosyasında bildirilen ve hizmet yapılandırma dosyasında ayarlanan ad ve değer çiftleridir.

##  <a name="localresources"></a><a name="LocalResources"></a>Yerel Kaynaklar
Öğe, `LocalResources` bir alt rol için yerel depolama kaynaklarının toplanmasını açıklar. Bu öğe, `LocalStorage` öğenin üst öğesidir.

##  <a name="localstorage"></a><a name="LocalStorage"></a>Localstorage
Öğe, `LocalStorage` çalışma zamanında hizmet için dosya sistemi alanı sağlayan yerel bir depolama kaynağı tanımlar. Bir rol sıfır veya daha fazla yerel depolama kaynağı tanımlayabilir.

> [!NOTE]
>  Öğe, `LocalStorage` Azure SDK'nın `WorkerRole` önceki sürümleriyle uyumluluğu desteklemek için öğenin bir alt öğesi olarak görünebilir.

Aşağıdaki tabloda `LocalStorage` öğenin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|ad|string|Gereklidir. Yerel mağaza için benzersiz bir ad.|
|cleanOnRoleGeridöngüsü|boole|İsteğe bağlı. Rol yeniden başlatıldığında yerel deponun temizlenip temizlenmemesi gerektiğini gösterir. Varsayılan değer. `true`|
|sizeInMb|int|İsteğe bağlı. Mb'deki yerel mağazaya tahsis etmek için istenen miktarda depolama alanı. Belirtilmemişse, ayrılan varsayılan depolama alanı 100 MB'dır. Tahsis edilebilecek minimum depolama alanı miktarı 1 MB'dır.<br /><br /> Yerel kaynakların maksimum boyutu sanal makine boyutuna bağlıdır. Daha fazla bilgi [için Bulut Hizmetleri için Sanal Makine boyutlarına](cloud-services-sizes-specs.md)bakın.|

Yerel depolama kaynağına ayrılan dizinin adı, ad özniteliği için sağlanan değere karşılık gelir.

##  <a name="endpoints"></a><a name="Endpoints"></a>Bitiş noktası
Öğe, `Endpoints` bir rol için giriş (dış), iç ve örnek giriş uç noktalarının toplanmasını açıklar. Bu öğe `InputEndpoint`, `InternalEndpoint`ve `InstanceInputEndpoint` öğelerin üst öğesidir.

Giriş ve İç uç noktaları ayrı ayrı ayrılır. Bir hizmette izin verilen 25 rol arasında ayrılabilen toplam 25 giriş, dahili ve örnek giriş uç noktası olabilir. Örneğin, 5 rolüvarsa, rol başına 5 giriş uç noktası ayırabilirsiniz veya tek bir role 25 giriş uç noktası ayırabilirsiniz veya her biri 25 role 1 giriş uç noktası ayırabilirsiniz.

> [!NOTE]
>  Dağıtılan her rol, rol başına bir örnek gerektirir. Bir abonelik için varsayılan sağlama 20 çekirdekle sınırlıdır ve bu nedenle bir rolün 20 örneğiyle sınırlıdır. Uygulamanız varsayılan sağlama tarafından sağlanandan daha fazla örnek gerektiriyorsa, kotanızı artırma hakkında daha fazla bilgi için [Faturalandırma, Abonelik Yönetimi ve Kota Desteği'ne](https://azure.microsoft.com/support/options/) bakın.

##  <a name="inputendpoint"></a><a name="InputEndpoint"></a>GirişEndpoint
Öğe, `InputEndpoint` bir alt rol için harici bir bitiş noktası açıklar.

HTTP, HTTPS, UDP ve TCP uç noktalarının birleşimi olan birden çok uç nokta tanımlayabilirsiniz. Giriş bitiş noktası için seçtiğiniz herhangi bir bağlantı noktası numarasını belirtebilirsiniz, ancak hizmetteki her rol için belirtilen bağlantı noktası numaralarının benzersiz olması gerekir. Örneğin, bir rolün HTTP için 80 ve HTTPS için 443 bağlantı noktası kullandığını belirtirseniz, ikinci bir rolün HTTP için 8080 portu ve HTTPS için 8043 bağlantı noktasını kullandığını belirtebilirsiniz.

Aşağıdaki tabloda `InputEndpoint` öğenin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|ad|string|Gereklidir. Dış uç nokta için benzersiz bir ad.|
|protokol|string|Gereklidir. Dış uç noktası için aktarım protokolü. Bir işçi rolü için `HTTP`olası `HTTPS` `UDP`değerler `TCP`, , , veya .|
|port|int|Gereklidir. Dış uç noktası için bağlantı noktası. Seçtiğiniz herhangi bir bağlantı noktası numarasını belirtebilirsiniz, ancak hizmetteki her rol için belirtilen bağlantı noktası numaralarının benzersiz olması gerekir.<br /><br /> Olası değerler 1 ile 65535 arasında değişir, dahil (Azure SDK sürüm 1.7 veya üzeri).|
|sertifika|string|HTTPS bitiş noktası için gereklidir. Bir `Certificate` öğe tarafından tanımlanan bir sertifikanın adı.|
|localPort|int|İsteğe bağlı. Uç noktada iç bağlantılar için kullanılan bir bağlantı noktasını belirtir. Öznitelik, `localPort` bitiş noktasındaki dış bağlantı noktasını bir roldeki dahili bağlantı noktasına eşler. Bu, bir rolün dıştan açığa çıkan bağlantı noktasındaki bir iç bileşene iletişim kurması gereken senaryolarda yararlıdır.<br /><br /> Belirtilmemişse, `localPort` değeri `port` öznitelik ile aynıdır. Çalışma zamanı `localPort` API'sini kullanarak bulunabilen tahsis edilmemiş bir bağlantı noktasını otomatik olarak atamak için "*" değerini ayarlayın.<br /><br /> Olası değerler 1 ile 65535 arasında değişir, dahil (Azure SDK sürüm 1.7 veya üzeri).<br /><br /> Bu `localPort` özellik yalnızca Azure SDK sürüm 1.3 veya üzeri kullanılarak kullanılabilir.|
|yoksayroleInstanceStatus|boole|İsteğe bağlı. Bu öznitelik değeri `true`ayarlandığında, bir hizmetin durumu yoksayılır ve bitiş noktası yük dengeleyicitarafından kaldırılmaz. Bu değeri, `true` bir hizmetin meşgul örneklerini hata ayıklamak için yararlı olarak ayarlama. Varsayılan değer: `false`. **Not:** Bir bitiş noktası, rol Hazır durumunda olmasa bile yine de trafik alabilir.|
|loadBalancerProbe|string|İsteğe bağlı. Giriş bitiş noktasıyla ilişkili yük dengeleyici sondasının adı. Daha fazla bilgi için [LoadBalancerProbe Schema'ya](schema-csdef-loadbalancerprobe.md)bakın.|

##  <a name="internalendpoint"></a><a name="InternalEndpoint"></a>InternalEndpoint
Öğe, `InternalEndpoint` bir alt rol için bir iç uç noktası açıklar. Dahili uç nokta yalnızca hizmet içinde çalışan diğer rol örnekleri için kullanılabilir; hizmet dışındaki istemciler tarafından kullanılamaz. Bir çalışan rolüen fazla beş HTTP, UDP veya TCP dahili uç noktası olabilir.

Aşağıdaki tabloda `InternalEndpoint` öğenin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|ad|string|Gereklidir. Dahili bitiş noktası için benzersiz bir ad.|
|protokol|string|Gereklidir. Dahili uç noktası için aktarım protokolü. Olası değerler `HTTP` `TCP`, `UDP`, `ANY`, veya .<br /><br /> Herhangi bir `ANY` protokolün, herhangi bir bağlantı noktasına izin verildiğini belirten bir değer.|
|port|int|İsteğe bağlı. Uç noktada iç yük dengeli bağlantılar için kullanılan bağlantı noktası. Yük dengeli uç noktası iki bağlantı noktası kullanır. Ortak IP adresi için kullanılan bağlantı noktası ve özel IP adresinde kullanılan bağlantı noktası. Genellikle bunlar aynı şekilde ayarlanır, ancak farklı bağlantı noktaları kullanmayı seçebilirsiniz.<br /><br /> Olası değerler 1 ile 65535 arasında değişir, dahil (Azure SDK sürüm 1.7 veya üzeri).<br /><br /> Bu `Port` özellik yalnızca Azure SDK sürüm 1.3 veya üzeri kullanılarak kullanılabilir.|

##  <a name="instanceinputendpoint"></a><a name="InstanceInputEndpoint"></a>InstanceInputEndpoint
Öğe, `InstanceInputEndpoint` bir örnek giriş bitiş noktasını bir alt rol olarak açıklar. Örnek giriş bitiş noktası, yük bakiyesinde bağlantı noktası iletme kullanılarak belirli bir rol örneğiyle ilişkilidir. Her örnek giriş bitiş noktası, belirli bir bağlantı noktasına çeşitli olası bağlantı noktalarından eşlenir. Bu öğe, `AllocatePublicPortFrom` öğenin üst öğesidir.

Öğe `InstanceInputEndpoint` yalnızca Azure SDK sürümü 1.7 veya üzeri kullanılarak kullanılabilir.

Aşağıdaki tabloda `InstanceInputEndpoint` öğenin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|ad|string|Gereklidir. Bitiş noktası için benzersiz bir ad.|
|localPort|int|Gereklidir. Yük bakiyesinden iletilen gelen trafiği almak için tüm rol örneklerinin dinleyeceğini iç bağlantı noktasını belirtir. Olası değerler 1 ile 65535 arasında değişir, dahil.|
|protokol|string|Gereklidir. Dahili uç noktası için aktarım protokolü. Olası değerler: `udp` veya `tcp`. http/https tabanlı trafik için kullanın. `tcp`|

##  <a name="allocatepublicportfrom"></a><a name="AllocatePublicPortFrom"></a>TahsisPublicPortFrom
Öğe, `AllocatePublicPortFrom` her örnek giriş bitiş noktasına erişmek için dış müşteriler tarafından kullanılabilecek ortak bağlantı noktası aralığını açıklar. Ortak (VIP) bağlantı noktası numarası bu aralıktan ayrılır ve kiracı dağıtımı ve güncelleştirmesi sırasında her bir rol örneği bitiş noktasına atanır. Bu öğe, `FixedPortRange` öğenin üst öğesidir.

Öğe `AllocatePublicPortFrom` yalnızca Azure SDK sürümü 1.7 veya üzeri kullanılarak kullanılabilir.

##  <a name="fixedport"></a><a name="FixedPort"></a>Sabit Bağlantı Noktası
Öğe, `FixedPort` uç noktada ki yük dengeli bağlantıları sağlayan iç uç noktası için bağlantı noktasını belirtir.

Öğe `FixedPort` yalnızca Azure SDK sürüm 1.3 veya üzeri kullanılarak kullanılabilir.

Aşağıdaki tabloda `FixedPort` öğenin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|port|int|Gereklidir. Dahili bitiş noktası için bağlantı noktası. Bu, `FixedPortRange` min ve max'i aynı bağlantı noktasına ayarlamakla aynı etkiye sahiptir.<br /><br /> Olası değerler 1 ile 65535 arasında değişir, dahil (Azure SDK sürüm 1.7 veya üzeri).|

##  <a name="fixedportrange"></a><a name="FixedPortRange"></a>Sabit PortAralığı
Öğe, `FixedPortRange` iç uç nokta veya örnek giriş bitiş noktasına atanan bağlantı noktaları aralığını belirtir ve bitiş noktasında yük dengeli bağlantılar için kullanılan bağlantı noktasını ayarlar.

> [!NOTE]
>  Öğe, `FixedPortRange` içinde bulunduğu öğeye bağlı olarak farklı çalışır. `FixedPortRange` Öğe `InternalEndpoint` öğede olduğunda, rolün çalıştığı tüm sanal makineler için min ve maksimum öznitelikleri aralığındaki yük dengeleyicisindeki tüm bağlantı noktalarını açar. `FixedPortRange` Öğe `InstanceInputEndpoint` öğede olduğunda, rolü çalıştıran her sanal makinede min ve maksimum öznitelikler aralığında yalnızca bir bağlantı noktası açar.

Öğe `FixedPortRange` yalnızca Azure SDK sürüm 1.3 veya üzeri kullanılarak kullanılabilir.

Aşağıdaki tabloda `FixedPortRange` öğenin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|min|int|Gereklidir. Aralıktaki minimum bağlantı noktası. Olası değerler 1 ile 65535 arasında değişir, dahil (Azure SDK sürüm 1.7 veya üzeri).|
|max|string|Gereklidir. Aralıktaki maksimum bağlantı noktası. Olası değerler 1 ile 65535 arasında değişir, dahil (Azure SDK sürüm 1.7 veya üzeri).|

##  <a name="certificates"></a><a name="Certificates"></a>Sertifika
Öğe, `Certificates` bir alt rol için sertifika toplama açıklar. Bu öğe, `Certificate` öğenin üst öğesidir. Bir rolün herhangi bir sayıda ilişkili sertifikası olabilir. Sertifikalar öğesini kullanma hakkında daha fazla bilgi için [bkz.](cloud-services-configure-ssl-certificate-portal.md#step-2-modify-the-service-definition-and-configuration-files)

##  <a name="certificate"></a><a name="Certificate"></a>Sertifika
Öğe, `Certificate` bir alt rol ile ilişkili bir sertifika açıklar.

Aşağıdaki tabloda `Certificate` öğenin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|ad|string|Gereklidir. Bir HTTPS `InputEndpoint` öğesi ile ilişkilendirildiğinde ona başvurmak için kullanılan bu sertifikanın adı.|
|Storelocation|string|Gereklidir. Bu sertifikanın yerel makinede bulunabileceği sertifika deposunun konumu. Olası değerler `CurrentUser` `LocalMachine`ve .|
|Storename|string|Gereklidir. Bu sertifikanın yerel makinede bulunduğu sertifika deposunun adı. Olası `My`değerler yerleşik mağaza adları, `Root` `CA`, `Trust` `Disallowed`, `TrustedPeople` `TrustedPublisher`, `AuthRoot` `AddressBook`, , , , , , , veya herhangi bir özel mağaza adı içerir. Özel bir mağaza adı belirtilirse, mağaza otomatik olarak oluşturulur.|
|permissionLevel|string|İsteğe bağlı. Rol işlemlerine verilen erişim izinlerini belirtir. Özel anahtara erişebilmek için yalnızca yükseltilmiş işlemler istiyorsanız, izin belirtin. `elevated` `limitedOrElevated`izin, tüm rol süreçlerinin özel anahtara erişmesine izin verir. Olası değerler: `limitedOrElevated` veya `elevated`. Varsayılan değer: `limitedOrElevated`.|

##  <a name="imports"></a><a name="Imports"></a>Ithalat
Öğe, `Imports` konuk işletim sistemine bileşenler ekleyen bir alt rol için alma modülleri koleksiyonunu açıklar. Bu öğe, `Import` öğenin üst öğesidir. Bu öğe isteğe bağlıdır ve bir rolün yalnızca bir çalışma zamanı bloğu olabilir.

Öğe `Imports` yalnızca Azure SDK sürüm 1.3 veya üzeri kullanılarak kullanılabilir.

##  <a name="import"></a><a name="Import"></a>Ithalat
Öğe `Import` konuk işletim sistemine eklemek için bir modül belirtir.

Öğe `Import` yalnızca Azure SDK sürüm 1.3 veya üzeri kullanılarak kullanılabilir.

Aşağıdaki tabloda `Import` öğenin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|Modulename|string|Gereklidir. İçe aktarılabilmek için modülün adı. Geçerli ithalat modülleri şunlardır:<br /><br /> - Uzaktan Erişim<br />- RemoteForwarder<br />- Tanılama<br /><br /> RemoteAccess ve RemoteForwarder modülleri, rol örneğini uzak masaüstü bağlantıları için yapılandırmanıza olanak sağlar. Daha fazla bilgi için [bkz.](cloud-services-role-enable-remote-desktop-new-portal.md)<br /><br /> Tanılama modülü, bir rol örneği için tanılama verilerini toplamanızı sağlar|

##  <a name="runtime"></a><a name="Runtime"></a>Çalışma zamanı
Öğe, `Runtime` Azure ana bilgisayar işleminin çalışma zamanı ortamını kontrol eden bir alt rol için ortam değişkeni ayarları koleksiyonunu açıklar. Bu öğe, `Environment` öğenin üst öğesidir. Bu öğe isteğe bağlıdır ve bir rolün yalnızca bir çalışma zamanı bloğu olabilir.

Öğe `Runtime` yalnızca Azure SDK sürüm 1.3 veya üzeri kullanılarak kullanılabilir.

Aşağıdaki tabloda `Runtime` öğenin öznitelikleri açıklanır:

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|Executioncontext|string|İsteğe bağlı. Rol İşleminin başlatıldığı bağlamı belirtir. Varsayılan bağlam. `limited`<br /><br /> -   `limited`– İşlem Yönetici ayrıcalıkları olmadan başlatılır.<br />-   `elevated`– İşlem Yönetici ayrıcalıkları ile başlatılır.|

##  <a name="environment"></a><a name="Environment"></a>Ortam
Öğe, `Environment` bir alt rol için ortam değişken iakaraları koleksiyonunu açıklar. Bu öğe, `Variable` öğenin üst öğesidir. Bir rol, herhangi bir sayıda ortam değişkeni ayarlamış olabilir.

##  <a name="variable"></a><a name="Variable"></a>Değişken
Öğe, `Variable` konuk çalışmada ayarlanması gereken bir ortam değişkeni belirtir.

Öğe `Variable` yalnızca Azure SDK sürüm 1.3 veya üzeri kullanılarak kullanılabilir.

Aşağıdaki tabloda `Variable` öğenin öznitelikleri açıklanır:

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|ad|string|Gereklidir. Ayarlanacak ortam değişkeninin adı.|
|value|string|İsteğe bağlı. Ortam değişkeni için ayarlanan değer. Bir değer özniteliği veya bir `RoleInstanceValue` öğe içermeniz gerekir.|

##  <a name="roleinstancevalue"></a><a name="RoleInstanceValue"></a>RoleInstanceValue
Öğe, `RoleInstanceValue` değişkenin değerini almak için xPath'i belirtir.

Aşağıdaki tabloda `RoleInstanceValue` öğenin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|Xpath|string|İsteğe bağlı. Örneğin dağıtım ayarlarının konum yolu. Daha fazla bilgi için [XPath ile Yapılandırma değişkenlerine](cloud-services-role-config-xpath.md)bakın.<br /><br /> Bir değer özniteliği veya bir `RoleInstanceValue` öğe içermeniz gerekir.|

##  <a name="entrypoint"></a><a name="EntryPoint"></a>Giriş Noktası
Öğe, `EntryPoint` bir rolün giriş noktasını belirtir. Bu öğe öğelerin `NetFxEntryPoint` üst öğesidir. Bu öğeler, varsayılan WaWorkerHost.exe dışında bir uygulama rol giriş noktası olarak hareket etmek için belirtmek için izin verir.

Öğe `EntryPoint` yalnızca Azure SDK sürümü 1.5 veya üzeri kullanılarak kullanılabilir.

##  <a name="netfxentrypoint"></a><a name="NetFxEntryPoint"></a>NetfxentryPoint
Öğe, `NetFxEntryPoint` bir rol için çalışacak programı belirtir.

> [!NOTE]
>  Öğe `NetFxEntryPoint` yalnızca Azure SDK sürümü 1.5 veya üzeri kullanılarak kullanılabilir.

Aşağıdaki tabloda `NetFxEntryPoint` öğenin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|Assemblyname|string|Gereklidir. Giriş noktasını içeren derlemenin yol ve dosya adı. Yol ** \\%ROLEROOT%\Approot** klasörüne göredir ** \\(%ROLEROOT%\Approot'da** `commandLine`belirtmeyin, varsayılır). **%ROLEROOT% Azure** tarafından tutulan bir ortam değişkenidir ve rolünüz için kök klasör konumunu temsil eder. %ROLEROOT%\Approot klasörü rolünüz için uygulama klasörünü temsil eder. ** \\**|
|targetFrameworkVersion|string|Gereklidir. Derlemenin üzerine inşa edildiği .NET çerçevesinin sürümü. Örneğin, `targetFrameworkVersion="v4.0"`.|

##  <a name="programentrypoint"></a><a name="ProgramEntryPoint"></a>ProgramentryPoint
Öğe, `ProgramEntryPoint` bir rol için çalışacak programı belirtir. Öğe, `ProgramEntryPoint` .NET derlemesini temel almayan bir program giriş noktası belirtmenizi sağlar.

> [!NOTE]
>  Öğe `ProgramEntryPoint` yalnızca Azure SDK sürümü 1.5 veya üzeri kullanılarak kullanılabilir.

Aşağıdaki tabloda `ProgramEntryPoint` öğenin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|Commandline|string|Gereklidir. Yürütülecek şekilde, dosya adı ve programın herhangi bir komut satırı bağımsız değişkenleri. Yol **%ROLEROOT%\Approot** klasörüne göredir (commandLine'da **%ROLEROOT%\Approot** belirtmeyin, varsayalım). **%ROLEROOT% Azure** tarafından tutulan bir ortam değişkenidir ve rolünüz için kök klasör konumunu temsil eder. **%ROLEROOT%\Approot** klasörü rolünüz için uygulama klasörünü temsil eder.<br /><br /> Program sona ererse, rol geri dönüştürülür, bu nedenle programı yalnızca başlatAn ve sonlu bir görevi çalıştıran bir program olmak yerine genellikle çalışmaya devam edecek şekilde ayarlayın.|
|setReadyOnProcessStart|boole|Gereklidir. Rol örneğinin komut satırı programının başlatıldığında sinyal gelmesini bekleyip beklemediğini belirtir. Bu değer şu `true` anda ayarlanmalıdır. Değerin ayarlanması `false` ileride kullanılmak üzere ayrılmıştır.|

##  <a name="startup"></a><a name="Startup"></a>Başlangıç
Öğe, `Startup` rol başlatıldığında çalışan bir görev koleksiyonunu açıklar. Bu öğe öğenin `Variable` üst olabilir. Rol başlatma görevlerini kullanma hakkında daha fazla bilgi için [başlangıç görevlerini nasıl yapılandırılatırsınız'](cloud-services-startup-tasks.md)a bakın. Bu öğe isteğe bağlıdır ve bir rolün yalnızca bir başlangıç bloğu olabilir.

Aşağıdaki tabloda `Startup` öğenin özniteliği açıklanmaktadır.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|Öncelik|int|Yalnızca dahili kullanım içindir.|

##  <a name="task"></a><a name="Task"></a>Görev
Öğe, `Task` rol başladığında gerçekleşen başlangıç görevini belirtir. Başlangıç görevleri, bu tür yükleme yazılım bileşenlerini çalıştırmak veya diğer uygulamaları çalıştırmak için rolü hazırlayan görevleri gerçekleştirmek için kullanılabilir. `Startup` Görevler, öğe bloğu içinde göründükleri sırada yürütülür.

Öğe `Task` yalnızca Azure SDK sürüm 1.3 veya üzeri kullanılarak kullanılabilir.

Aşağıdaki tabloda `Task` öğenin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|Commandline|string|Gereklidir. Çalışacak komutları içeren CMD dosyası gibi bir komut dosyası. Başlangıç komutu ve toplu iş dosyaları ANSI biçiminde kaydedilmelidir. Dosyanın başında bayt sıralı işaretçi ayarlayan dosya biçimleri düzgün işlemez.|
|Executioncontext|string|Komut dosyasının çalıştırıldığı bağlamı belirtir.<br /><br /> -   `limited`[Varsayılan] – İşlemi barındıran rol ile aynı ayrıcalıklarla çalıştırın.<br />-   `elevated`– Yönetici ayrıcalıkları ile çalıştırın.|
|Tasktype|string|Komutun yürütme davranışını belirtir.<br /><br /> -   `simple`[Varsayılan] – Sistem, diğer görevler başlatılmadan önce görevin çıkmasını bekler.<br />-   `background`– Sistem görevin çıkmasını beklemez.<br />-   `foreground`– Arka plana benzer şekilde, tüm ön plan görevleri çıkana kadar rol yeniden başlatılamaz.|

##  <a name="contents"></a><a name="Contents"></a>Içeriği
Öğe, `Contents` bir alt rol için içerik toplama açıklar. Bu öğe, `Content` öğenin üst öğesidir.

Öğe `Contents` yalnızca Azure SDK sürümü 1.5 veya üzeri kullanılarak kullanılabilir.

##  <a name="content"></a><a name="Content"></a>Içerik
Öğe, `Content` Azure sanal makinesine kopyalanacak içeriğin kaynak konumunu ve kopyalandığı hedef yolu tanımlar.

Öğe `Content` yalnızca Azure SDK sürümü 1.5 veya üzeri kullanılarak kullanılabilir.

Aşağıdaki tabloda `Content` öğenin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|hedef|string|Gereklidir. İçeriğin yerleştirildiği Azure sanal makinesindeki konum. Bu konum **%ROLEROOT%\Approot**klasörüne göredir.|

Bu öğe, `SourceDirectory` öğenin üst öğesidir.

##  <a name="sourcedirectory"></a><a name="SourceDirectory"></a>Kaynak Dizin
Öğe, `SourceDirectory` içeriğin kopyalandığı yerel dizini tanımlar. Azure sanal makinesini kopyalamak için yerel içeriği belirtmek için bu öğeyi kullanın.

Öğe `SourceDirectory` yalnızca Azure SDK sürümü 1.5 veya üzeri kullanılarak kullanılabilir.

Aşağıdaki tabloda `SourceDirectory` öğenin öznitelikleri açıklanmaktadır.

| Öznitelik | Tür | Açıklama |
| --------- | ---- | ----------- |
|yol|string|Gereklidir. İçeriği Azure sanal makinesine kopyalanacak olan yerel bir dizinin göreli veya mutlak yolu. Dizin yolundaki ortam değişkenlerinin genişlemesi desteklenir.|

## <a name="see-also"></a>Ayrıca Bkz.
[Bulut Hizmeti (klasik) Tanımı Şeması](schema-csdef-file.md)



