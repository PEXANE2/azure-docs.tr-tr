---
title: Azure Bulut Hizmetleri Tanımı Şeması (.cscfg Dosyası) | Microsoft Dokümanlar
description: Bir hizmet yapılandırması (.cscfg) dosyası, her rol, yapılandırma değerleri ve bir rol için sertifika parmak izleri için kaç rol örneği dağıtılanın caiz olduğunu belirtir.
services: cloud-services
ms.custom: ''
ms.date: 12/07/2016
ms.service: cloud-services
ms.topic: reference
caps.latest.revision: 35
author: tgore03
ms.author: tagore
ms.openlocfilehash: cb77181e00c97b7f426429793f17af3cb5e84ebe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79534754"
---
# <a name="azure-cloud-services-config-schema-cscfg-file"></a>Azure Bulut Hizmetleri Config Schema (.cscfg Dosyası)
Hizmet yapılandırma dosyası, hizmetteki her rol için dağıtılabilen rol örneklerinin sayısını, yapılandırma ayarlarının değerlerini ve bir rolle ilişkili sertifikalar için parmak izlerini belirtir. Hizmet Sanal Ağın bir parçasıysa, ağ için yapılandırma bilgilerinin hizmet yapılandırma dosyasında ve sanal ağ yapılandırma dosyasında sağlanması gerekir. Hizmet yapılandırma dosyası için varsayılan uzantısı .cscfg'dir.

Hizmet modeli [Bulut Hizmeti (klasik) Definition Schema](schema-csdef-file.md)tarafından açıklanmıştır.

Varsayılan olarak, Azure Tanılama yapılandırma şeması dosyası `C:\Program Files\Microsoft SDKs\Windows Azure\.NET SDK\<version>\schemas` dizine yüklenir. Azure `<version>` [SDK'nın](https://azure.microsoft.com/downloads/)yüklü sürümüyle değiştirin.

Bir hizmetteki rolleri yapılandırma hakkında daha fazla bilgi için [Bulut Hizmeti modelinin ne olduğunu](cloud-services-model-and-package.md)görün.

## <a name="basic-service-configuration-schema"></a>Temel Hizmet Yapılandırma Şeması
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
Aşağıdaki konular `ServiceConfiguration` öğe için şema açıklar:

- [Rol Şeması](schema-cscfg-role.md)
- [NetworkConfiguration Şeması](schema-cscfg-networkconfiguration.md)

## <a name="service-configuration-namespace"></a>Hizmet YapılandırmaSı Ad Alanı
Hizmet yapılandırma dosyası için XML ad `http://schemas.microsoft.com/ServiceHosting/2008/10/ServiceConfiguration`alanı: .

##  <a name="serviceconfiguration-element"></a><a name="ServiceConfiguration"></a>ServiceConfiguration Öğesi
Öğe, `ServiceConfiguration` hizmet yapılandırma dosyasının üst düzey öğesidir.

Aşağıdaki tabloda `ServiceConfiguration` öğenin öznitelikleri açıklanmaktadır. Tüm öznitelikler dize türleridir.

| Öznitelik | Açıklama |
| --------- | ----------- |
|Hizmetadı|Gereklidir. Bulut hizmetinin adı. Burada verilen ad, hizmet tanımı dosyasında belirtilen adla eşleşmelidir.|
|osFamily|İsteğe bağlı. Bulut hizmetinde rol örnekleri üzerinde çalışacak Konuk İşletim Sistemi'ni belirtir. Desteklenen Konuk İşletim Sistemi sürümleri hakkında daha fazla bilgi için [Azure Konuk İşletim Sistemi Sürümleri ve SDK Uyumluluk](cloud-services-guestos-update-matrix.md)Matrisi'ne bakın.<br /><br /> Bir `osFamily` değer eklemezseniz ve `osVersion` özniteliği belirli bir Konuk İşletim Sistemi sürümüne ayarlamadıysanız, varsayılan değeri 1 kullanılır.|
|osVersion|İsteğe bağlı. Konuk İşletim Sistemi'nin bulut hizmetindeki rol örnekleriüzerinde çalışacak sürümünü belirtir. Konuk İşletim Sistemi sürümleri hakkında daha fazla bilgi için [Azure Konuk İşletim Sistemi Sürümleri ve SDK Uyumluluk](cloud-services-guestos-update-matrix.md)Matrisi'ne bakın.<br /><br /> Konuk İşletim Sistemi'nin otomatik olarak en son sürüme yükseltilmesi gerektiğini belirtebilirsiniz. Bunu yapmak için, özniteliğin `osVersion` değerini `*`. `*`Ayarlandığında, rol örnekleri belirtilen işletim sistemi ailesi için Konuk İşletim Sistemi'nin en son sürümü kullanılarak dağıtılır ve Konuk İşletim Sistemi'nin yeni sürümleri yayımlandığında otomatik olarak yükseltilir.<br /><br /> Belirli bir sürümü el ile `Configuration String` belirtmek için, [Azure Konuk İşletim Sistemi Sürümleri](cloud-services-guestos-update-matrix.md)ve SDK Uyumluluk Matrisi'nin **Gelecek, Geçerli ve Geçici Konuk İşletim Sistemi Sürümleri** bölümündetablodan kullanın.<br /><br /> Öznitelik için `osVersion` varsayılan değer `*`.|
|şemaVersiyon|İsteğe bağlı. Hizmet Yapılandırma şemasının sürümünü belirtir. Şema sürümü Visual Studio SDK'nın birden fazla sürümü yan yana yüklenirse şema doğrulama için kullanılacak doğru SDK araçlarını seçmenize olanak tanır. Şema ve sürüm uyumluluğu hakkında daha fazla bilgi için Azure [Konuk İşletim Sistemi Sürümleri ve SDK Uyumluluk Matrisi'ne](cloud-services-guestos-update-matrix.md) bakın|

Hizmet yapılandırma dosyası bir `ServiceConfiguration` öğe içermelidir. Öğe `ServiceConfiguration` herhangi bir sayıda `Role` öğe ve `NetworkConfiguration` sıfır veya 1 öğe içerebilir.