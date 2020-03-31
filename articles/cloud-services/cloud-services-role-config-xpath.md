---
title: Bulut Hizmetleri Rolü config XPath hile sayfası | Microsoft Dokümanlar
description: Ayarları bir ortam değişkeni olarak ortaya çıkarmak için bulut hizmeti rolü config'de kullanabileceğiniz çeşitli XPath ayarları.
services: cloud-services
author: tgore03
ms.service: cloud-services
ms.topic: article
ms.date: 04/19/2017
ms.author: tagore
ms.openlocfilehash: 380b0be4e4e4b19d16cb611b0b472294339f2199
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75386094"
---
# <a name="expose-role-configuration-settings-as-an-environment-variable-with-xpath"></a>XPath ile rol yapılandırma ayarlarını ortam değişkeni olarak pozlama
Bulut hizmeti alt üst sürümü veya web rol hizmeti tanımı dosyasında, çalışma zamanı yapılandırma değerlerini ortam değişkenleri olarak gösterebilirsiniz. Aşağıdaki XPath değerleri desteklenir (API değerlerine karşılık gelir).

Bu XPath değerleri [Microsoft.WindowsAzure.ServiceRuntime](/previous-versions/azure/reference/ee773173(v=azure.100)) kitaplığı aracılığıyla da kullanılabilir. 

## <a name="app-running-in-emulator"></a>Emülatörde çalışan uygulama
Uygulamanın emülatörde çalıştığını gösterir.

| Tür | Örnek |
| --- | --- |
| XPath |xpath="/RoleEnvironment/Deployment/@emulated" |
| Kod |var x = RoleEnvironment.IsEmulated; |

## <a name="deployment-id"></a>Dağıtım Kimliği
Örneğin dağıtım kimliğini alır.

| Tür | Örnek |
| --- | --- |
| XPath |xpath="/RoleEnvironment/Deployment/@id" |
| Kod |var deploymentId = RoleEnvironment.DeploymentId; |

## <a name="role-id"></a>Rol Kimliği
Örneğin geçerli rol kimliğini alır.

| Tür | Örnek |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/@id" |
| Kod |var id = RoleEnvironment.CurrentRoleInstance.Id; |

## <a name="update-domain"></a>Etki alanını güncelleştirme
Örneğin güncelleştirme etki alanını alır.

| Tür | Örnek |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/@updateDomain" |
| Kod |var ud = RoleEnvironment.CurrentRoleInstance.UpdateDomain; |

## <a name="fault-domain"></a>Hata etki alanı
Örneğin hata etki alanını alır.

| Tür | Örnek |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/@faultDomain" |
| Kod |var fd = RoleEnvironment.CurrentRoleInstance.FaultDomain; |

## <a name="role-name"></a>Rol adı
Örneklerin rol adını alır.

| Tür | Örnek |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/@roleName" |
| Kod |var rname = RoleEnvironment.CurrentRoleInstance.Role.Name; |

## <a name="config-setting"></a>Config ayarı
Belirtilen yapılandırma ayarının değerini alır.

| Tür | Örnek |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/ConfigurationSettings/ConfigurationSetting[@name='Ayar1']/@value" |
| Kod |var ayarı = RoleEnvironment.GetConfigurationSettingValue("Ayar1"); |

## <a name="local-storage-path"></a>Yerel depolama yolu
Örnek için yerel depolama yolunu alır.

| Tür | Örnek |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@path" |
| Kod |var localResourcePath = RoleEnvironment.GetLocalResource("LocalStore1"). RootPath; |

## <a name="local-storage-size"></a>Yerel depolama boyutu
Örneğin yerel depolama alanının boyutunu alır.

| Tür | Örnek |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/LocalResources/LocalResource[@name='LocalStore1']/@sizeInMB" |
| Kod |var localResourceSizeInMB = RoleEnvironment.GetLocalResource("LocalStore1"). MaximumSizeInMegabayt; |

## <a name="endpoint-protocol"></a>Bitiş noktası protokolü
Örnek için uç nokta iletişim kuralını alır.

| Tür | Örnek |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@protocol" |
| Kod |var prot = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. Protokol; |

## <a name="endpoint-ip"></a>Uç NOKTA IP
Belirtilen bitiş noktasının IP adresini alır.

| Tür | Örnek |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@address" |
| Kod |var adresi = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Adres |

## <a name="endpoint-port"></a>Bitiş noktası bağlantı noktası
Örneğin bitiş noktası bağlantı noktasını alır.

| Tür | Örnek |
| --- | --- |
| XPath |xpath="/RoleEnvironment/CurrentInstance/Endpoints/Endpoint[@name='Endpoint1']/@port" |
| Kod |var portu = RoleEnvironment.CurrentRoleInstance.InstanceEndpoints["Endpoint1"]. IPEndpoint.Port; |

## <a name="example"></a>Örnek
Burada `TestIsEmulated` [ @emulated xpath değerine](#app-running-in-emulator)ayarlanmış bir ortam değişkeni ile bir başlangıç görevi oluşturan bir alt rol örneği verilmiştir. 

```xml
<WorkerRole name="Role1">
    <ConfigurationSettings>
      <Setting name="Setting1" />
    </ConfigurationSettings>
    <LocalResources>
      <LocalStorage name="LocalStore1" sizeInMB="1024"/>
    </LocalResources>
    <Endpoints>
      <InternalEndpoint name="Endpoint1" protocol="tcp" />
    </Endpoints>
    <Startup>
      <Task commandLine="example.cmd inputParm">
        <Environment>
          <Variable name="TestConstant" value="Constant"/>
          <Variable name="TestEmptyValue" value=""/>
          <Variable name="TestIsEmulated">
            <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
          </Variable>
          ...
        </Environment>
      </Task>
    </Startup>
    <Runtime>
      <Environment>
        <Variable name="TestConstant" value="Constant"/>
        <Variable name="TestEmptyValue" value=""/>
        <Variable name="TestIsEmulated">
          <RoleInstanceValue xpath="/RoleEnvironment/Deployment/@emulated"/>
        </Variable>
        ...
      </Environment>
    </Runtime>
    ...
</WorkerRole>
```

## <a name="next-steps"></a>Sonraki adımlar
[ServiceConfiguration.cscfg](cloud-services-model-and-package.md#serviceconfigurationcscfg) dosyası hakkında daha fazla bilgi edinin.

[ServicePackage.cspkg](cloud-services-model-and-package.md#servicepackagecspkg) paketi oluşturun.

Rol için [uzak masaüstünü](cloud-services-role-enable-remote-desktop-new-portal.md) etkinleştirin.




