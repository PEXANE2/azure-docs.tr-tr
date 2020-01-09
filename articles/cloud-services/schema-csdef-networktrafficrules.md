---
title: Azure Cloud Services def. NetworkTrafficRules şeması | Microsoft Docs
ms.custom: ''
ms.date: 04/14/2015
services: cloud-services
ms.reviewer: ''
ms.service: cloud-services
ms.suite: ''
ms.tgt_pltfrm: ''
ms.topic: reference
ms.assetid: 351b369f-365e-46c1-82ce-03fc3655cc88
caps.latest.revision: 17
author: tgore03
ms.author: tagore
ms.openlocfilehash: e6d156810b9fdee69ddac122eec06db7267ddf36
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75449031"
---
# <a name="azure-cloud-services-definition-networktrafficrules-schema"></a>Azure Cloud Services Definition NetworkTrafficRules şeması
`NetworkTrafficRules` düğümü, rollerinin birbirleriyle nasıl iletişim kuracağını belirten hizmet tanımı dosyasındaki isteğe bağlı bir öğedir. Bu, belirli rolün iç uç noktalarına hangi rollerin erişebileceğini sınırlandırır. `NetworkTrafficRules` tek başına bir öğe değil; bir hizmet tanımı dosyasında iki veya daha fazla rolle birleştirilir.

Hizmet tanım dosyası için varsayılan uzantı. csdef ' dir.

> [!NOTE]
>  `NetworkTrafficRules` düğümü yalnızca Azure SDK 1,3 veya üzeri sürümler kullanılarak kullanılabilir.

## <a name="basic-service-definition-schema-for-the-network-traffic-rules"></a>Ağ trafiği kuralları için temel hizmet tanımı şeması
Ağ trafiği tanımlarını içeren bir hizmet tanımı dosyasının temel biçimi aşağıdaki gibidir.

```xml
<ServiceDefinition …>
   <NetworkTrafficRules>
      <OnlyAllowTrafficTo>
         <Destinations>
            <RoleEndpoint endpointName="<name-of-the-endpoint>" roleName="<name-of-the-role-containing-the-endpoint>"/>
         </Destinations>
         <AllowAllTraffic/>
         <WhenSource matches="[AnyRule]">
            <FromRole roleName="<name-of-the-role-to-allow-traffic-from>"/>
         </WhenSource>
      </OnlyAllowTrafficTo>
   </NetworkTrafficRules>
</ServiceDefinition>
```

## <a name="schema-elements"></a>Şema öğeleri
Hizmet tanımı dosyasının `NetworkTrafficRules` düğümü, bu konunun sonraki bölümlerinde ayrıntılı olarak açıklanan bu öğeleri içerir:

[NetworkTrafficRules öğesi](#NetworkTrafficRules)

[OnlyAllowTrafficTo öğesi](#OnlyAllowTrafficTo)

[Destinations öğesi](#Destinations)

[RoleEndpoint öğesi](#RoleEndpoint)

AllowAllTraffic öğesi

[WhenSource öğesi](#WhenSource)

[FromRole öğesi](#FromRole)

##  <a name="NetworkTrafficRules"></a>NetworkTrafficRules öğesi
`NetworkTrafficRules` öğesi, hangi rollerin başka bir roldeki hangi uç noktayla iletişim kurabildiğini belirtir. Bir hizmet, bir `NetworkTrafficRules` tanımı içerebilir.

##  <a name="OnlyAllowTrafficTo"></a>OnlyAllowTrafficTo öğesi
`OnlyAllowTrafficTo` öğesi, hedef uç noktaları ve bunlarla iletişim kurabilen roller koleksiyonunu açıklar. Birden çok `OnlyAllowTrafficTo` düğümü belirtebilirsiniz.

##  <a name="Destinations"></a>Destinations öğesi
`Destinations` öğesi, ile iletişim kurılanmadan bir RoleEndpoints koleksiyonu tanımlar.

##  <a name="RoleEndpoint"></a>RoleEndpoint öğesi
`RoleEndpoint` öğesi, ile iletişime izin vermek için bir rol üzerinde bir uç nokta tanımlar. Rol üzerinde birden fazla uç nokta varsa birden çok `RoleEndpoint` öğesi belirtebilirsiniz.

| Öznitelik      | Tür     | Açıklama |
| -------------- | -------- | ----------- |
| `endpointName` | `string` | Gereklidir. Trafiğe izin verilecek uç noktanın adı.|
| `roleName`     | `string` | Gereklidir. İletişime izin verilecek Web rolünün adı.|

## <a name="allowalltraffic-element"></a>AllowAllTraffic öğesi
`AllowAllTraffic` öğesi, tüm rollerin `Destinations` düğümünde tanımlanan uç noktalarla iletişim kurmasına izin veren bir kuraldır.

##  <a name="WhenSource"></a>WhenSource öğesi
`WhenSource` öğesi, `Destinations` düğümünde tanımlanan uç noktalarla iletişim kurabildiğinden rol koleksiyonunu açıklar.

| Öznitelik | Tür     | Açıklama |
| --------- | -------- | ----------- |
| `matches` | `string` | Gereklidir. İletişime izin verirken uygulanacak kuralı belirtir. Geçerli tek değer şu anda `AnyRule`.|
  
##  <a name="FromRole"></a>FromRole öğesi
`FromRole` öğesi, `Destinations` düğümünde tanımlanan uç noktalarla iletişim kurabilen rolleri belirtir. Uç noktalarla iletişim kurabilen birden fazla rol varsa birden çok `FromRole` öğesi belirtebilirsiniz.

| Öznitelik  | Tür     | Açıklama |
| ---------- | -------- | ----------- |
| `roleName` | `string` | Gereklidir. İletişime izin verilecek rolün adı.|

## <a name="see-also"></a>Ayrıca Bkz.
[Bulut hizmeti (klasik) Tanım Şeması](schema-csdef-file.md)




