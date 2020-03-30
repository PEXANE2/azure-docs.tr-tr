---
title: Azure Bulut Hizmetleri Def. LoadBalancerProbe Şeması | Microsoft Dokümanlar
description: LoadBalancerProbe tanımlanan müşteri, rol örneklerindeki uç noktaların bir sağlık sondasýr. Hizmet tanımı dosyasındaki web veya çalışan rolleri ile birleştirir.
ms.custom: ''
ms.date: 04/14/2015
services: cloud-services
ms.service: cloud-services
ms.topic: reference
caps.latest.revision: 14
author: georgewallace
ms.author: tagore
ms.openlocfilehash: 6d0e84b6724d9df4162d4be3e06a9952087a53a6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79537355"
---
# <a name="azure-cloud-services-definition-loadbalancerprobe-schema"></a>Azure Bulut Hizmetleri Tanımı LoadBalancerProbe Schema
Yük dengeleyici sondası, rol örneklerindeki UDP uç noktalarının ve uç noktalarının müşteri tanımlı sağlık sondasýr. Bağımsız `LoadBalancerProbe` bir unsur değildir; bir hizmet tanımı dosyasındaki web rolü veya çalışan rolüyle birleştirilir. A `LoadBalancerProbe` birden fazla rol tarafından kullanılabilir.

Hizmet tanımı dosyasının varsayılan uzantısı .csdef'tir.

## <a name="the-function-of-a-load-balancer-probe"></a>Yük dengeleyici probunun işlevi
Azure Yük Dengeleyicisi, gelen trafiği rol örneklerinize yönlendirmekten sorumludur. Yük dengeleyicisi, söz konusu örneğin durumunu belirlemek için her örneği düzenli olarak inceleyerek hangi örneklerin trafik alabileceğini belirler. Yük dengeleyicisi her örneği dakikada birden çok kez inceler. Yük dengeleyicisine örnek durumu sağlamak için iki farklı seçenek vardır – varsayılan yük dengeleyici prob veya .csdef dosyasında LoadBalancerProbe tanımlanarak uygulanan özel bir yük dengeleyici prob.

Varsayılan yük dengeleyici sondası, yalnızca örnek Hazır durumdayken (örneğin Meşgul, Geri Dönüşüm, Durdurma, vb. durumlarında olmadığı gibi) http 200 OK yanıtı yla dinleyen ve yanıtveren sanal makinenin içindeki Konuk Aracıyı kullanır. Konuk Aracıhttp 200 Ok ile yanıt vermezse, Azure Yük Dengeleyici örneği yanıt vermiyor olarak işaretler ve bu örne trafik göndermeyi durdurur. Azure Yük Dengeleyicisi örneği pinglemeye devam eder ve Konuk Aracı http 200 ile yanıt verirse, Azure Yük Dengeleyicisi trafiği o örne yeniden gönderir. Bir web rolü kullanırken web kodunuz genellikle Azure kumaşı veya konuk aracı tarafından izlenmeyen w3wp.exe'de çalışır, bu da w3wp.exe'deki hatalar anlamına gelir (örn. HTTP 500 yanıtları) konuk ajana bildirilmez ve yük dengeleyicisi bu örneği döndürmenin dışında almayı bilmez.

Özel yük dengeleyici sondası varsayılan konuk aracı sondasını geçersiz kılar ve rol örneğinin durumunu belirlemek için kendi özel mantığınızı oluşturmanıza olanak tanır. Yük dengeleyicisi bitiş noktanızı düzenli olarak inceler (varsayılan olarak her 15 saniyede bir) ve zaman zaman dilimi içinde (varsayılan 31 saniye) tcp ACK veya HTTP 200 ile yanıt verirse, örnek döndürme de değerlendirilir. Bu, örneğin %90 CPU'nun üzerindeyse, örneğin 200 olmayan bir durumu döndürme gibi örnekleri yük dengeleyici döndürmeden kaldırmak için kendi mantığınızı uygulamak yararlı olabilir. w3wp.exe kullanarak web rolleri için, bu aynı zamanda web sitenizin otomatik izleme almak anlamına gelir, web sitesi kodundaki hatalar yük dengeleyici sondası için olmayan bir 200 durumu dönmek beri. .csdef dosyasında loadbalancerProbe tanımlamazsanız, varsayılan yük dengeleyici davranışı (daha önce açıklandığı gibi) kullanılır.

Özel bir yük dengeleyici prob kullanıyorsanız, mantığınızın RoleEnvironment.OnStop yöntemini dikkate aldığından emin olmalısınız. Varsayılan yük dengeleyici sondası kullanırken, örnek OnStop çağrılmadan önce döndürme dışına çıkarılır, ancak özel bir yük dengeleyici probu OnStop olayı sırasında 200 OK döndürmeye devam edebilir. Önbelleği temizlemek, hizmeti durdurmak veya hizmetin çalışma zamanı davranışını etkileyebilecek değişiklikler yapmak için OnStop olayını kullanıyorsanız, özel yük dengeleyici sondalama mantığınızın örneği döndürmeden kaldırdığından emin olmanız gerekir.

## <a name="basic-service-definition-schema-for-a-load-balancer-probe"></a>Yük dengeleyici prob için temel hizmet tanımı şeması
 Yük dengeleyici probu içeren hizmet tanım dosyasının temel biçimi aşağıdaki gibidir.

```xml
<ServiceDefinition …>
   <LoadBalancerProbes>
      <LoadBalancerProbe name="<load-balancer-probe-name>" protocol="[http|tcp]" path="<uri-for-checking-health-status-of-vm>" port="<port-number>" intervalInSeconds="<interval-in-seconds>" timeoutInSeconds="<timeout-in-seconds>"/>
   </LoadBalancerProbes>
</ServiceDefinition>
```

## <a name="schema-elements"></a>Şema elemanları
Hizmet `LoadBalancerProbes` tanımı dosyasının öğesi aşağıdaki öğeleri içerir:

- [LoadBalancerProbes Elemanı](#LoadBalancerProbes)
- [LoadBalancerProbe Elemanı](#LoadBalancerProbe)

##  <a name="loadbalancerprobes-element"></a><a name="LoadBalancerProbes"></a>LoadBalancerProbes Elemanı
Öğe `LoadBalancerProbes` yük dengeleyici probları toplama açıklar. Bu öğe [LoadBalancerProbe Öğesinin](#LoadBalancerProbe)ana elemanıdır. 

##  <a name="loadbalancerprobe-element"></a><a name="LoadBalancerProbe"></a>LoadBalancerProbe Elemanı
Öğe, `LoadBalancerProbe` bir model için sistem durumu sondasını tanımlar. Birden çok yük dengeleyici probları tanımlayabilirsiniz. 

Aşağıdaki tabloda `LoadBalancerProbe` öğenin öznitelikleri açıklanır:

|Öznitelik|Tür|Açıklama|
| ------------------- | -------- | -----------------|
| `name`              | `string` | Gereklidir. Yük dengeleyici sondasının adı. İsim benzersiz olmalı.|
| `protocol`          | `string` | Gereklidir. Bitiş noktasının protokolünü belirtir. Olası değerler: `http` veya `tcp`. `tcp` Belirtilirse, sondanın başarılı olması için alınan bir ACK gereklidir. `http` Belirtilirse, sondanın başarılı olması için belirtilen URI'den 200 Ok yanıtı gerekir.|
| `path`              | `string` | URI VM'den sağlık durumu istemek için kullanılır. `path`olarak `protocol` ayarlanırsa `http`gereklidir. Aksi takdirde, izin verilmez.<br /><br /> Varsayılan değer yoktur.|
| `port`              | `integer` | İsteğe bağlı. Sondayı iletmek için bağlantı noktası. Bu herhangi bir bitiş noktası için isteğe bağlıdır, çünkü aynı bağlantı noktası sonda için kullanılacaktır. Onların sondalama için de farklı bir bağlantı noktası yapılandırabilirsiniz. Olası değerler 1 ile 65535 arasında değişir, dahil.<br /><br /> Varsayılan değer bitiş noktasına göre ayarlanır.|
| `intervalInSeconds` | `integer` | İsteğe bağlı. Aralık, saniye cinsinden, ne sıklıkta sağlık durumu için bitiş noktasını araştırmak için. Genellikle, aralık, örneği döndürmeden çıkarmadan önce iki tam proba izin veren, ayrılan zaman aralığının (saniye cinsinden) yarısından biraz daha azdır.<br /><br /> Varsayılan değer 15, minimum değer 5'tir.|
| `timeoutInSeconds`  | `integer` | İsteğe bağlı. Zaman sonu süresi, saniyeler içinde, hiçbir yanıtın daha fazla trafiğin bitiş noktasına gönderilmesini engellemeye neden olacağı sondaya uygulanır. Bu değer, uç noktaların Azure'da kullanılan normal sürelerden (varsayılan değerler) daha hızlı veya daha yavaş döndürme dışında alınmasını sağlar.<br /><br /> Varsayılan değer 31, minimum değer 11'dir.|

## <a name="see-also"></a>Ayrıca Bkz.
[Bulut Hizmeti (klasik) Tanımı Şeması](schema-csdef-file.md)