---
title: Azure Cloud Services rol şeması | Microsoft Docs
ms.custom: ''
ms.date: 12/07/2016
services: cloud-services
ms.service: cloud-services
ms.topic: reference
caps.latest.revision: 12
author: tgore03
ms.author: tagore
ms.openlocfilehash: 0f0e79b462726b1aa6a953a4b8c92334d6b16492
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75449096"
---
# <a name="azure-cloud-services-config-role-schema"></a>Azure Cloud Services yapılandırma rolü şeması

Yapılandırma dosyasının `Role` öğesi, hizmette her bir rol için dağıtılacak rol örneği sayısını, yapılandırma ayarlarının değerlerini ve bir rolle ilişkili tüm sertifikalar için parmak izlerini belirtir.

Azure hizmet yapılandırma şeması hakkında daha fazla bilgi için bkz. [Cloud Service (klasik) yapılandırma şeması](schema-cscfg-file.md). Azure hizmet tanımı şeması hakkında daha fazla bilgi için bkz. [bulut hizmeti (klasik) Tanım Şeması](schema-csdef-file.md).

##  <a name="Role"></a>Rol öğesi
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

Aşağıdaki tabloda `Role` öğesi için öznitelikler açıklanmaktadır.

| Öznitelik | Açıklama |
| --------- | ----------- |
| ad   | Gereklidir. Rolün adını belirtir. Ad, hizmet tanımı dosyasındaki rol için girilen adla eşleşmelidir.|
| vmName | İsteğe bağlı. Bir sanal makinenin DNS adını belirtir. Ad 10 karakter veya daha az olmalıdır.|

Aşağıdaki tablo `Role` öğesinin alt öğelerini açıklar.

| Öğe | Açıklama |
| ------- | ----------- |
| Örnekler | Gereklidir. Rol için dağıtılacak örneklerin sayısını belirtir. Örnek sayısı `count` özniteliği için bir tamsayı tarafından tanımlanır.|
| Ayar   | İsteğe bağlı. Bir rolün ayarlar koleksiyonundaki bir ayar adı ve değeri belirtir. Ayar adı, `name` özniteliği için bir dize tarafından tanımlanır ve ayar değeri `value` özniteliği için bir dize tarafından tanımlanır.|
| Sertifika | İsteğe bağlı. Rolüyle ilişkilendirilecek bir hizmet sertifikasının adını, parmak izini ve algoritmasını belirtir. Sertifika adı `name` özniteliği için bir dize tarafından tanımlanır. Sertifika parmak izi, `thumbprint` özniteliği için boşluk içermeyen bir onaltılık sayı dizesi tarafından tanımlanır. Onaltılık sayılar, rakamlar ve büyük Alfa karakterleri kullanılarak temsil etmelidir. Sertifika algoritması `thumbprintAlgorithm` özniteliği için bir dize tarafından tanımlanır.|

## <a name="see-also"></a>Ayrıca Bkz.
[Bulut hizmeti (klasik) yapılandırma şeması](schema-cscfg-file.md)