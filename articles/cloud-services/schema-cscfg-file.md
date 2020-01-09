---
title: Azure Cloud Services Tanım Şeması (. cscfg dosyası) | Microsoft Docs
services: cloud-services
ms.custom: ''
ms.date: 12/07/2016
ms.service: cloud-services
ms.topic: reference
caps.latest.revision: 35
author: tgore03
ms.author: tagore
ms.openlocfilehash: 71c0bb1b09d480a05a9e5a54b269d0da8fde5bc3
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75449111"
---
# <a name="azure-cloud-services-config-schema-cscfg-file"></a>Azure Cloud Services config şeması (. cscfg dosyası)
Hizmet yapılandırma dosyası, hizmette her bir rol için dağıtılacak rol örneği sayısını, herhangi bir yapılandırma ayarlarının değerlerini ve bir rolle ilişkili tüm sertifikalar için parmak izlerini belirtir. Hizmet bir sanal ağın parçasıysa, hizmet yapılandırma dosyasında ve sanal ağ yapılandırma dosyasında ağ için yapılandırma bilgilerinin sağlanması gerekir. Hizmet yapılandırma dosyası için varsayılan uzantı. cscfg ' dir.

Hizmet modeli, [bulut hizmeti (klasik) Tanım Şeması](schema-csdef-file.md)tarafından açıklanmıştır.

Varsayılan olarak, Azure Tanılama yapılandırma şema dosyası `C:\Program Files\Microsoft SDKs\Windows Azure\.NET SDK\<version>\schemas` dizinine yüklenir. `<version>`, [Azure SDK](https://azure.microsoft.com/downloads/)'nın yüklü sürümüyle değiştirin.

Bir hizmette rolleri yapılandırma hakkında daha fazla bilgi için bkz. [bulut hizmeti modeli nedir?](cloud-services-model-and-package.md).

## <a name="basic-service-configuration-schema"></a>Temel hizmet yapılandırma şeması
Hizmet yapılandırma dosyasının temel biçimi aşağıdaki gibidir.

```xml
<ServiceConfiguration serviceName="<service-name>" osFamily="<osfamily-number>" osVersion="<os-version>" schemaVersion="<schema-version>">

  <Role …>
    …
  </Role>

  <NetworkConfiguration>
    …
  </NetworkConfiguration>

</ServiceConfiguration>
```

## <a name="schema-definitions"></a>Şema tanımları
Aşağıdaki konular `ServiceConfiguration` öğesi için şemayı anlatmaktadır:

- [Rol Şeması](schema-cscfg-role.md)
- [NetworkConfiguration Şeması](schema-cscfg-networkconfiguration.md)

## <a name="service-configuration-namespace"></a>Hizmet yapılandırması ad alanı
Hizmet yapılandırma dosyasının XML ad alanı: `http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration`.

##  <a name="ServiceConfiguration"></a>ServiceConfiguration öğesi
`ServiceConfiguration` öğesi, hizmet yapılandırma dosyasının en üst düzey öğesidir.

Aşağıdaki tabloda `ServiceConfiguration` öğesinin öznitelikleri açıklanmaktadır. Tüm öznitelik değerleri dize türleridir.

| Öznitelik | Açıklama |
| --------- | ----------- |
|HizmetAdı|Gereklidir. Bulut hizmetinin adı. Burada verilen ad, hizmet tanımı dosyasında belirtilen adla eşleşmelidir.|
|osFamily|İsteğe bağlı. Bulut hizmetindeki rol örneklerinde çalıştırılacak Konuk işletim sistemini belirtir. Desteklenen Konuk işletim sistemi sürümleri hakkında daha fazla bilgi için bkz. [Azure Konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](cloud-services-guestos-update-matrix.md).<br /><br /> `osFamily` bir değer eklemezseniz ve `osVersion` özniteliğini belirli bir konuk işletim sistemi sürümüne ayarlamayın, varsayılan 1 değeri kullanılır.|
|osVersion|İsteğe bağlı. Bulut hizmetindeki rol örneklerinde çalıştırılacak Konuk işletim sisteminin sürümünü belirtir. Konuk işletim sistemi sürümleri hakkında daha fazla bilgi için bkz. [Azure Konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](cloud-services-guestos-update-matrix.md).<br /><br /> Konuk işletim sisteminin en son sürüme otomatik olarak yükseltilmesi gerektiğini belirtebilirsiniz. Bunu yapmak için `osVersion` özniteliğinin değerini `*`olarak ayarlayın. `*`olarak ayarlandığında, rol örnekleri belirtilen işletim sistemi ailesi için konuk işletim sisteminin en son sürümü kullanılarak dağıtılır ve konuk işletim sisteminin yeni sürümleri serbest bırakıldığında otomatik olarak yükseltilir.<br /><br /> Belirli bir sürümü el ile belirtmek için, [Azure Konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](cloud-services-guestos-update-matrix.md)'nin **gelecekteki, güncel ve geçici Konuk işletim sistemi sürümleri** bölümündeki `Configuration String` ' yi kullanın.<br /><br /> `osVersion` özniteliği için varsayılan değer `*`.|
|schemaVersion|İsteğe bağlı. Hizmet yapılandırma şemasının sürümünü belirtir. Şema sürümü, Visual Studio 'nun birden fazla SDK sürümü yan yana yüklenirse şema doğrulaması için kullanılacak doğru SDK araçlarını seçmesine olanak sağlar. Şema ve sürüm uyumluluğu hakkında daha fazla bilgi için bkz. [Azure Konuk işletim sistemi sürümleri ve SDK uyumluluk matrisi](cloud-services-guestos-update-matrix.md)|

Hizmet yapılandırma dosyasında bir `ServiceConfiguration` öğesi bulunmalıdır. `ServiceConfiguration` öğesi herhangi bir sayıda `Role` öğesi ve sıfır veya 1 `NetworkConfiguration` öğesi içerebilir.