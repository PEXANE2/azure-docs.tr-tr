---
title: Azure Cloud Services def. Loadbalanceraraştırma şeması | Microsoft Docs
description: Müşteri tanımlı Loadbalanceraraştırması, rol örneklerinde uç noktaların durum araştırmasına sahiptir. Bir hizmet tanım dosyasındaki Web veya çalışan rolleriyle birleştirir.
ms.custom: ''
ms.date: 04/14/2015
services: cloud-services
ms.service: cloud-services
ms.topic: reference
caps.latest.revision: 14
author: georgewallace
ms.author: tagore
ms.openlocfilehash: 6d0e84b6724d9df4162d4be3e06a9952087a53a6
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "79537355"
---
# <a name="azure-cloud-services-definition-loadbalancerprobe-schema"></a>Azure Cloud Services Definition Loadbalanceraraştırma şeması
Yük dengeleyici araştırması, bir müşteri tarafından tanımlanan UDP uç noktaları ve uç noktaların rol örneklerinde bir sistem durumu araştırmasına sahiptir. `LoadBalancerProbe`Tek başına bir öğe değildir; bir hizmet tanım dosyasındaki web rolü veya çalışan rolüyle birleştirilir. Bir `LoadBalancerProbe` , birden fazla rol tarafından kullanılabilir.

Hizmet tanım dosyası için varsayılan uzantı. csdef ' dir.

## <a name="the-function-of-a-load-balancer-probe"></a>Yük dengeleyici araştırması işlevi
Azure Load Balancer, gelen trafiğin rol örneklerinizi yönlendirmesinden sorumludur. Yük dengeleyici, bu örneğin durumunu belirleyebilmek için her bir örneği düzenli olarak yoklama yaparak hangi örneklerin trafik alabileceğini belirler. Yük dengeleyici her örneği dakikada birden çok kez yoklamalar. Yük dengeleyicisine örnek sistem durumu sağlamaya yönelik iki farklı seçenek vardır: varsayılan Yük dengeleyici araştırması veya. csdef dosyasında Loadbalanceraraştırması tanımlayarak uygulanan özel bir yük dengeleyici araştırması.

Varsayılan yük dengeleyici araştırması, sanal makinenin içindeki Konuk aracısından yararlanır ve yalnızca örnek hazır durumda olduğunda (örneğin meşgul, geri dönüşüm, durdurma, vb. durumlar gibi) HTTP 200 OK yanıtını dinler ve bu yanıtı verir. Konuk Aracısı HTTP 200 Tamam ile yanıt vermiyorsa, Azure Load Balancer örneği yanıt vermeyen olarak işaretler ve bu örneğe trafik göndermeyi bırakır. Azure Load Balancer örneğe ping vermeye devam eder ve Konuk Aracısı bir HTTP 200 ile yanıt verirse, Azure Load Balancer trafiği bu örneğe yeniden gönderir. Web rolü kullanılırken, Web sitesi kodunuz genellikle Azure Fabric veya Konuk Aracısı tarafından izlenmeyen w3wp.exe çalışır ve bu da w3wp.exe (örn. HTTP 500 yanıtları) Konuk aracıya bildirilmemiştir ve yük dengeleyici bu örneği döndürme dışında bırakmak için bilmez.

Özel yük dengeleyici araştırması varsayılan Konuk Aracısı araştırmasını geçersiz kılar ve rol örneğinin sistem durumunu öğrenmek için kendi özel mantığınızı oluşturmanızı sağlar. Yük dengeleyici, uç noktanızı düzenli olarak yoklamalar (varsayılan olarak 15 saniyede bir) ve zaman aşımı süresi içinde bir TCP ACK veya HTTP 200 ile yanıt verirse, örnek döndürme olarak değerlendirilir (varsayılan 31 saniye). Bu, örnekleri yük dengeleyici rotasyondan kaldırmak için kendi mantığınızı uygulamak için yararlı olabilir; Örneğin, örnek %90 ' den daha üstündeyse 200 olmayan bir durum döndürüyor. w3wp.exe kullanan Web rolleri için, Web sitenizin hatalarının yük dengeleyici araştırmasına 200 olmayan bir durum döndürdüğü için, bu da Web sitenizi otomatik olarak izlemeyi aldığınız anlamına gelir. . Csdef dosyasında bir Loadbalanceraraştırma tanımlamadıysanız, varsayılan Yük dengeleyici davranışı (daha önce açıklandığı gibi) kullanılır.

Özel bir yük dengeleyici araştırması kullanıyorsanız, mantığınızın RoleEnvironment. OnStop metodunu dikkate aldığından emin olmanız gerekir. Varsayılan yük dengeleyici araştırması kullanılırken, örnek, OnStop çağrılmadan önce dönüşten önce döndürülür, ancak özel bir yük dengeleyici araştırması, OnStop olayı sırasında 200 Tamam geri döndürmeye devam edebilir. Önbelleği temizlemek, hizmeti durdurmak veya hizmetinizin çalışma zamanı davranışını etkileyebilecek değişiklikler yapmak için OnStop olayını kullanıyorsanız, özel yük dengeleyici araştırma mantığınızın örneği dönüşten kaldırdığından emin olmanız gerekir.

## <a name="basic-service-definition-schema-for-a-load-balancer-probe"></a>Yük dengeleyici araştırması için temel hizmet tanımı şeması
 Bir yük dengeleyici araştırması içeren bir hizmet tanımı dosyasının temel biçimi aşağıdaki gibidir.

```xml
<ServiceDefinition …>
   <LoadBalancerProbes>
      <LoadBalancerProbe name="<load-balancer-probe-name>" protocol="[http|tcp]" path="<uri-for-checking-health-status-of-vm>" port="<port-number>" intervalInSeconds="<interval-in-seconds>" timeoutInSeconds="<timeout-in-seconds>"/>
   </LoadBalancerProbes>
</ServiceDefinition>
```

## <a name="schema-elements"></a>Şema öğeleri
`LoadBalancerProbes`Hizmet tanımı dosyasının öğesi aşağıdaki öğeleri içerir:

- [Loadbalanceraraştırmaları öğesi](#LoadBalancerProbes)
- [Loadbalanceraraştırma öğesi](#LoadBalancerProbe)

##  <a name="loadbalancerprobes-element"></a><a name="LoadBalancerProbes"></a>Loadbalanceraraştırmaları öğesi
`LoadBalancerProbes`Öğesi, yük dengeleyici araştırmaları koleksiyonunu açıklar. Bu öğe [Loadbalanceraraştırma öğesinin](#LoadBalancerProbe)üst öğesidir. 

##  <a name="loadbalancerprobe-element"></a><a name="LoadBalancerProbe"></a>Loadbalanceraraştırma öğesi
`LoadBalancerProbe`Öğesi bir modelin sistem durumu araştırmasını tanımlar. Birden çok yük dengeleyici yoklamaları tanımlayabilirsiniz. 

Aşağıdaki tablo, öğesinin özniteliklerini açıklar `LoadBalancerProbe` :

|Öznitelik|Tür|Description|
| ------------------- | -------- | -----------------|
| `name`              | `string` | Gereklidir. Yük dengeleyici araştırmasının adı. Ad benzersiz olmalıdır.|
| `protocol`          | `string` | Gereklidir. Bitiş noktasının protokolünü belirtir. Olası değerler: `http` veya `tcp`. `tcp`Belirtilmişse, araştırmanın başarılı olması için alınan BIR ACK gereklidir. `http`Belirtilmişse, araştırmanın başarılı olması için BELIRTILEN URI 'den 200 bir Tamam yanıtı gerekir.|
| `path`              | `string` | VM 'den sistem durumu istemek için kullanılan URI. `path`, `protocol` olarak ayarlandıysa gereklidir `http` . Aksi takdirde, buna izin verilmez.<br /><br /> Varsayılan değer yoktur.|
| `port`              | `integer` | İsteğe bağlı. Araştırmayı iletişim kurmak için bağlantı noktası. Bu, araştırma için aynı bağlantı noktası kullanılacaksa, her bitiş noktası için isteğe bağlıdır. Yoklama için farklı bir bağlantı noktası da yapılandırabilirsiniz. Olası değerler 1 ile 65535 arasında (dahil) arasındadır.<br /><br /> Varsayılan değer bitiş noktası tarafından ayarlanır.|
| `intervalInSeconds` | `integer` | İsteğe bağlı. Uç noktanın sistem durumu için ne sıklıkla araştırılıp saniye cinsinden aralığı. Genellikle, zaman aralığı, örneği döndürmeden önce iki tam yoklamadan önce ayrılan zaman aşımı süresi (saniye cinsinden) değerinden biraz daha küçüktür.<br /><br /> Varsayılan değer 15 ' tir, en küçük değer 5 ' tir.|
| `timeoutInSeconds`  | `integer` | İsteğe bağlı. Yanıt olmaması durumunda daha fazla trafiğin bitiş noktasına teslim edilmesine neden olacağı, saniye cinsinden zaman aşımı süresi. Bu değer, uç noktaların Azure 'da kullanılan tipik süreden daha hızlı veya daha yavaş kullanıma alınmasını sağlar (Varsayılanlar olan).<br /><br /> Varsayılan değer 31 ' dir, en küçük değer 11 ' dir.|

## <a name="see-also"></a>Ayrıca Bkz.
[Bulut hizmeti (klasik) Tanım Şeması](schema-csdef-file.md)