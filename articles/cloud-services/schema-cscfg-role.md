---
title: Azure Cloud Services rol şeması | Microsoft Docs
description: Bir hizmet yapılandırma dosyasının rol öğesi, her rol, yapılandırma değeri ve sertifika parmak izleri için kaç tane rol örneğinin dağıtılacağını belirtir.
ms.custom: ''
ms.date: 12/07/2016
services: cloud-services
ms.service: cloud-services
ms.topic: reference
caps.latest.revision: 12
author: tgore03
ms.author: tagore
ms.openlocfilehash: b64f9d27e382a39b132593502fed32c565af473a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "79528430"
---
# <a name="azure-cloud-services-config-role-schema"></a>Azure Cloud Services yapılandırma rolü şeması

`Role`Yapılandırma dosyasının öğesi, hizmette her bir rol için dağıtılacak rol örneği sayısını, herhangi bir yapılandırma ayarlarının değerlerini ve bir rolle ilişkili tüm sertifikalar için parmak izlerini belirtir.

Azure hizmet yapılandırma şeması hakkında daha fazla bilgi için bkz. [Cloud Service (klasik) yapılandırma şeması](schema-cscfg-file.md). Azure hizmet tanımı şeması hakkında daha fazla bilgi için bkz. [bulut hizmeti (klasik) Tanım Şeması](schema-csdef-file.md).

##  <a name="role-element"></a><a name="Role"></a>Rol öğesi
Aşağıdaki örnek `Role` öğesi ve onun alt öğelerini gösterir.

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

Aşağıdaki tabloda öğesi için öznitelikler açıklanmaktadır `Role` .

| Öznitelik | Açıklama |
| --------- | ----------- |
| name   | Gereklidir. Rolün adını belirtir. Ad, hizmet tanımı dosyasındaki rol için girilen adla eşleşmelidir.|
| vmName | İsteğe bağlı. Bir sanal makinenin DNS adını belirtir. Ad 10 karakter veya daha az olmalıdır.|

Aşağıdaki tablo, öğesinin alt öğelerini açıklar `Role` .

| Öğe | Description |
| ------- | ----------- |
| Örnekler | Gereklidir. Rol için dağıtılacak örneklerin sayısını belirtir. Örnek sayısı öznitelik için bir tamsayı ile tanımlanır `count` .|
| Ayar   | İsteğe bağlı. Bir rolün ayarlar koleksiyonundaki bir ayar adı ve değeri belirtir. Ayar adı, özniteliği için bir dize tarafından tanımlanır `name` ve ayar değeri öznitelik için bir dize tarafından tanımlanır `value` .|
| Sertifika | İsteğe bağlı. Rolüyle ilişkilendirilecek bir hizmet sertifikasının adını, parmak izini ve algoritmasını belirtir. Sertifika adı, öznitelik için bir dize tarafından tanımlanır `name` . Sertifika parmak izi, öznitelik için boşluk içermeyen bir onaltılık sayı dizesi tarafından tanımlanır `thumbprint` . Onaltılık sayılar, rakamlar ve büyük Alfa karakterleri kullanılarak temsil etmelidir. Sertifika algoritması, özniteliği için bir dize tarafından tanımlanır `thumbprintAlgorithm` .|

## <a name="see-also"></a>Ayrıca Bkz.
[Bulut hizmeti (klasik) yapılandırma şeması](schema-cscfg-file.md)