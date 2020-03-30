---
title: Azure Bulut Hizmetleri Rol Şeması | Microsoft Dokümanlar
description: Hizmet yapılandırma dosyasının Rol öğesi, her rol, yapılandırma değerleri ve sertifika parmak izleri için kaç rol örneği dağıtılanın caiz olduğunu belirtir.
ms.custom: ''
ms.date: 12/07/2016
services: cloud-services
ms.service: cloud-services
ms.topic: reference
caps.latest.revision: 12
author: tgore03
ms.author: tagore
ms.openlocfilehash: b64f9d27e382a39b132593502fed32c565af473a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79528430"
---
# <a name="azure-cloud-services-config-role-schema"></a>Azure Bulut Hizmetleri Config Role Schema

Yapılandırma `Role` dosyasının öğesi, hizmetteki her rol için dağıtılabilen rol örneklerinin sayısını, yapılandırma ayarlarının değerlerini ve rolle ilişkili sertifikalar için parmak izlerini belirtir.

Azure Hizmet Yapılandırma Şeması hakkında daha fazla bilgi için [Bulut Hizmeti (klasik) Yapılandırma Şeması'na](schema-cscfg-file.md)bakın. Azure Hizmet Tanımı Şeması hakkında daha fazla bilgi için [Bulut Hizmeti (klasik) Definition Schema](schema-csdef-file.md)'ya bakın.

##  <a name="role-element"></a><a name="Role"></a>Rol Öğesi
Aşağıdaki örnek, `Role` öğeyi ve alt öğelerini gösterir.

```xml 
<ServiceConfiguration>
  <Role name="<role-name>" vmName="<vm-name>">
    <Instances count="<number-of-instances>"/>
    <ConfigurationSettings>
      <Setting name="<setting-name>" value="<setting-value>" />
    </ConfigurationSettings>
    <Certificates>
      <Certificate name="<certificate-name>" thumbprint="<certificate-thumbprint>" thumbprintAlgorithm="<algorithm>"/>
    </Certificates>
  </Role>
</ServiceConfiguration>
```

Aşağıdaki tabloda `Role` öğenin öznitelikleri açıklanmaktadır.

| Öznitelik | Açıklama |
| --------- | ----------- |
| ad   | Gereklidir. Rolün adını belirtir. Ad, hizmet tanımı dosyasındaki rol için sağlanan adla eşleşmelidir.|
| vmName | İsteğe bağlı. Sanal Makine için DNS adını belirtir. Ad 10 karakter veya daha az olmalıdır.|

Aşağıdaki tabloda `Role` öğenin alt öğeleri açıklanmaktadır.

| Öğe | Açıklama |
| ------- | ----------- |
| Örnekler | Gereklidir. Rol için dağıtılabilmek için örnek sayısını belirtir. Örnek sayısı öznitelik için `count` bir arametli tarafından tanımlanır.|
| Ayar   | İsteğe bağlı. Bir rol için ayarlar koleksiyonunda ayar adı ve değeri belirtir. Ayar adı öznitelik için `name` bir dize ile tanımlanır ve ayar değeri `value` öznitelik için bir dize ile tanımlanır.|
| Sertifika | İsteğe bağlı. Rolile ilişkilendirilecek bir hizmet sertifikasının adını, parmak izini ve algoritmasını belirtir. Sertifika adı öznitelik için `name` bir dize ile tanımlanır. Sertifika parmak izi, öznitelik için `thumbprint` boşluk içermeyen bir hexadecimal sayı dizisi yle tanımlanır. Hexadecimal sayılar basamak ve büyük alfa karakterleri kullanılarak temsil edilmelidir. Sertifika algoritması öznitelik için `thumbprintAlgorithm` bir dize ile tanımlanır.|

## <a name="see-also"></a>Ayrıca Bkz.
[Bulut Hizmeti (klasik) Yapılandırma Şeması](schema-cscfg-file.md)