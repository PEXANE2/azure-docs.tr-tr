---
title: Azure Bulut Hizmetleri Def. NetworkTrafficRules Şema | Microsoft Dokümanlar
description: Bir rolün dahili uç noktalarına erişebilen rolleri sınırlayan NetworkTrafficRules hakkında bilgi edinin. Hizmet tanımı dosyasındaki rollerle birleştirir.
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
ms.openlocfilehash: e53c10395ec3168e656633cc43fb2d01902209fa
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79534737"
---
# <a name="azure-cloud-services-definition-networktrafficrules-schema"></a>Azure Bulut Hizmetleri Tanımı AğTrafikKuralları Şeması
Düğüm, `NetworkTrafficRules` hizmet tanımı dosyasındaki, rollerin birbiriyle nasıl iletişim kurduğunu belirten isteğe bağlı bir öğedir. Hangi rollerin belirli rolün iç uç noktalarına erişebileceğini sınırlar. Bağımsız `NetworkTrafficRules` bir unsur değildir; bir hizmet tanımı dosyasındaki iki veya daha fazla rolile birleştirilir.

Hizmet tanımı dosyasının varsayılan uzantısı .csdef'tir.

> [!NOTE]
>  Düğüm `NetworkTrafficRules` yalnızca Azure SDK sürüm 1.3 veya üzeri kullanılarak kullanılabilir.

## <a name="basic-service-definition-schema-for-the-network-traffic-rules"></a>Ağ trafiği kuralları için temel hizmet tanımı şeması
Ağ trafiği tanımlarını içeren bir hizmet tanım dosyasının temel biçimi aşağıdaki gibidir.

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

## <a name="schema-elements"></a>Şema Elemanları
Hizmet `NetworkTrafficRules` tanımı dosyasının düğümü, bu konuda sonraki bölümlerde ayrıntılı olarak açıklanan bu öğeleri içerir:

[AğTrafik Kuralları Öğesi](#NetworkTrafficRules)

[OnlyallowTrafficto öğesi](#OnlyAllowTrafficTo)

[Hedef Ler Öğesi](#Destinations)

[RoleEndpoint Öğesi](#RoleEndpoint)

İzin AllTraffic Öğesi

[WhenSource Öğesi](#WhenSource)

[FromRole Öğesi](#FromRole)

##  <a name="networktrafficrules-element"></a><a name="NetworkTrafficRules"></a>AğTrafik Kuralları Öğesi
Öğe, `NetworkTrafficRules` hangi rollerin başka bir rolde hangi bitiş noktasıyla iletişim kurabileceğini belirtir. Bir hizmet tek `NetworkTrafficRules` bir tanım içerebilir.

##  <a name="onlyallowtrafficto-element"></a><a name="OnlyAllowTrafficTo"></a>OnlyallowTrafficto öğesi
Öğe, `OnlyAllowTrafficTo` hedef uç noktaları ve bunlarla iletişim kurabilecek rollerin bir koleksiyonunu açıklar. Birden çok `OnlyAllowTrafficTo` düğüm belirtebilirsiniz.

##  <a name="destinations-element"></a><a name="Destinations"></a>Hedef Ler Öğesi
Öğe, `Destinations` iletilebilen roleendpoints koleksiyonunu açıklar.

##  <a name="roleendpoint-element"></a><a name="RoleEndpoint"></a>RoleEndpoint Öğesi
Öğe, `RoleEndpoint` iletişime izin vermek için bir rol üzerinde bir bitiş noktasını açıklar. Rolüzerinde birden `RoleEndpoint` fazla bitiş noktası varsa birden çok öğe belirtebilirsiniz.

| Öznitelik      | Tür     | Açıklama |
| -------------- | -------- | ----------- |
| `endpointName` | `string` | Gereklidir. Trafiğe izin vermek için bitiş noktasının adı.|
| `roleName`     | `string` | Gereklidir. Web rolünün adı iletişimizin.|

## <a name="allowalltraffic-element"></a>İzin AllTraffic Öğesi
Öğe, `AllowAllTraffic` tüm rollerin `Destinations` düğümde tanımlanan uç noktalarla iletişim kurmasını sağlayan bir kuraldır.

##  <a name="whensource-element"></a><a name="WhenSource"></a>WhenSource Öğesi
Öğe, `WhenSource` `Destinations` düğümde tanımlanan uç noktalarla iletişim kurabilecekrollerin bir koleksiyonunu açıklar.

| Öznitelik | Tür     | Açıklama |
| --------- | -------- | ----------- |
| `matches` | `string` | Gereklidir. İletişime izin verirken uygulanacak kuralı belirtir. Tek geçerli değer `AnyRule`şu anda.|
  
##  <a name="fromrole-element"></a><a name="FromRole"></a>FromRole Öğesi
Öğe `FromRole` düğümde `Destinations` tanımlanan uç noktaları ile iletişim kurabilir rolleri belirtir. Uç noktalarla `FromRole` iletişim kurabilen birden fazla rol varsa, birden çok öğe belirtebilirsiniz.

| Öznitelik  | Tür     | Açıklama |
| ---------- | -------- | ----------- |
| `roleName` | `string` | Gereklidir. İletişime izin verecek rolün adı.|

## <a name="see-also"></a>Ayrıca Bkz.
[Bulut Hizmeti (klasik) Tanımı Şeması](schema-csdef-file.md)




