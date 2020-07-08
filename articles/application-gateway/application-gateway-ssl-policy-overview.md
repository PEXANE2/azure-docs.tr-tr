---
title: Azure Application Gateway için TLS ilkesine genel bakış
description: Azure Application Gateway TLS ilkesini yapılandırmayı ve arka uç sunucu grubundan şifreleme ve şifre çözme ek yükünü azaltmayı öğrenin.
services: application gateway
author: amsriva
ms.service: application-gateway
ms.topic: conceptual
ms.date: 11/16/2019
ms.author: amsriva
ms.openlocfilehash: 16c6dd28d47573c2ad5b0d5a331b0dc48e7aacef
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85253639"
---
# <a name="application-gateway-tls-policy-overview"></a>Application Gateway TLS ilkesine genel bakış

Azure Application Gateway kullanarak TLS/SSL sertifika yönetimini merkezileştirmek ve bir arka uç sunucu grubundan şifreleme ve şifre çözme yükünü azaltmak için kullanabilirsiniz. Bu merkezi TLS işleme, kurumsal güvenlik gereksinimlerinize uygun bir merkezi TLS ilkesi belirtmenize de olanak tanır. Bu, uyumluluk gereksinimlerini ve güvenlik yönergelerini ve önerilen uygulamaları karşılamanıza yardımcı olur.

TLS ilkesi TLS protokol sürümünün ve şifreleme paketlerinin yanı sıra bir TLS el sıkışması sırasında şifrelemelerin kullanıldığı sırayı da içerir. Application Gateway, TLS ilkesini denetlemek için iki mekanizma sunar. Önceden tanımlanmış bir ilke ya da özel ilke kullanabilirsiniz.

## <a name="predefined-tls-policy"></a>Önceden tanımlanmış TLS ilkesi

Application Gateway önceden tanımlanmış üç güvenlik ilkesi vardır. Uygun güvenlik düzeyini almak için ağ geçidinizi bu ilkelerden herhangi biriyle yapılandırabilirsiniz. İlke adlarına, yapılandırıldığı yıl ve aya göre açıklama eklenir. Her ilke farklı TLS protokol sürümleri ve şifre paketleri sunar. En iyi TLS güvenliğini sağlamak için en yeni TLS ilkelerini kullanmanızı öneririz.

### <a name="appgwsslpolicy20150501"></a>AppGwSslPolicy20150501

|Özellik  |Değer  |
|---|---|
|Adı     | AppGwSslPolicy20150501        |
|MinProtocolVersion     | TLSv1_0        |
|Varsayılan| True (önceden tanımlanmış bir ilke belirtilmemişse) |
|CipherSuites     |TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_DHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_DHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_DHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_DHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_RSA_WITH_AES_256_GCM_SHA384<br>TLS_RSA_WITH_AES_128_GCM_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA256<br>TLS_RSA_WITH_AES_128_CBC_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA<br>TLS_DHE_DSS_WITH_AES_256_CBC_SHA256<br>TLS_DHE_DSS_WITH_AES_128_CBC_SHA256<br>TLS_DHE_DSS_WITH_AES_256_CBC_SHA<br>TLS_DHE_DSS_WITH_AES_128_CBC_SHA<br>TLS_RSA_WITH_3DES_EDE_CBC_SHA<br>TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA |
  
### <a name="appgwsslpolicy20170401"></a>AppGwSslPolicy20170401
  
|Özellik  |Değer  |
|   ---      |  ---       |
|Adı     | AppGwSslPolicy20170401        |
|MinProtocolVersion     | TLSv1_1        |
|Varsayılan| False |
|CipherSuites     |TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA<br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_256_GCM_SHA384<br>TLS_RSA_WITH_AES_128_GCM_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA256<br>TLS_RSA_WITH_AES_128_CBC_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_128_CBC_SHA |
  
### <a name="appgwsslpolicy20170401s"></a>AppGwSslPolicy20170401S

|Özellik  |Değer  |
|---|---|
|Adı     | AppGwSslPolicy20170401S        |
|MinProtocolVersion     | TLSv1_2        |
|Varsayılan| False |
|CipherSuites     |TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256 <br>    TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384 <br>    TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA <br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA <br>TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256<br>TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384<br>TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384<br>TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256<br>TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA<br>TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_256_GCM_SHA384<br>TLS_RSA_WITH_AES_128_GCM_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA256<br>TLS_RSA_WITH_AES_128_CBC_SHA256<br>TLS_RSA_WITH_AES_256_CBC_SHA<br>TLS_RSA_WITH_AES_128_CBC_SHA<br> |

## <a name="custom-tls-policy"></a>Özel TLS ilkesi

Gereksinimlerinize göre önceden tanımlanmış bir TLS ilkesinin yapılandırılması gerekiyorsa, kendi özel TLS ilkenizi tanımlamanız gerekir. Özel bir TLS ilkesiyle, desteklenecek en düşük TLS protokol sürümü ve desteklenen şifre paketleri ve bunların öncelik sıralaması üzerinde tüm denetiminiz vardır.
 
### <a name="tlsssl-protocol-versions"></a>TLS/SSL protokol sürümleri

* SSL 2,0 ve 3,0 tüm uygulama ağ geçitleri için varsayılan olarak devre dışıdır. Bu protokol sürümleri yapılandırılamaz.
* Özel bir TLS ilkesi, ağ geçidinizin için en düşük TLS protokolü sürümü olarak aşağıdaki üç protokolden birini seçme seçeneği sunar: TLSv1_0, TLSv1_1 ve TLSv1_2.
* Hiçbir TLS ilkesi tanımlanmamışsa, üç protokol (TLSv1_0, TLSv1_1 ve TLSv1_2) etkinleştirilir.

### <a name="cipher-suites"></a>Şifre paketleri

Application Gateway, özel ilkenizi seçebileceğiniz aşağıdaki şifre paketlerini destekler. Şifreleme paketlerinin sıralaması, TLS anlaşması sırasında öncelik sırasını belirler.


- TLS_ECDHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_RSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_RSA_WITH_AES_128_CBC_SHA
- TLS_DHE_RSA_WITH_AES_256_GCM_SHA384
- TLS_DHE_RSA_WITH_AES_128_GCM_SHA256
- TLS_DHE_RSA_WITH_AES_256_CBC_SHA
- TLS_DHE_RSA_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_AES_256_GCM_SHA384
- TLS_RSA_WITH_AES_128_GCM_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA256
- TLS_RSA_WITH_AES_128_CBC_SHA256
- TLS_RSA_WITH_AES_256_CBC_SHA
- TLS_RSA_WITH_AES_128_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA384
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA256
- TLS_ECDHE_ECDSA_WITH_AES_256_CBC_SHA
- TLS_ECDHE_ECDSA_WITH_AES_128_CBC_SHA
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA256
- TLS_DHE_DSS_WITH_AES_256_CBC_SHA
- TLS_DHE_DSS_WITH_AES_128_CBC_SHA
- TLS_RSA_WITH_3DES_EDE_CBC_SHA
- TLS_DHE_DSS_WITH_3DES_EDE_CBC_SHA

> [!NOTE]
> Bağlantı için kullanılan TLS şifre paketleri, kullanılan sertifikanın türüne de bağlıdır. İstemciden uygulama ağ geçidi bağlantılarında, kullanılan şifre paketleri uygulama ağ geçidi dinleyicisinde sunucu sertifikalarının türünü temel alır. Uygulama ağ geçidinde arka uç havuzu bağlantılarına, kullanılan şifre paketleri, arka uç havuzu sunucularındaki sunucu sertifikalarının türünü temel alır.

## <a name="known-issue"></a>Bilinen sorun
Application Gateway v2 Şu anda aşağıdaki şifrelemeleri desteklemez:
- DHE-RSA-AES128-GCM-SHA256
- DHE-RSA-AES128-SHA
- DHE-RSA-AES256-GCM-SHA384
- DHE-RSA-AES256-SHA
- DHE-DSS-AES128-SHA256
- DHE-DSS-AES128-SHA
- DHE-DSS-AES256-SHA256
- DHE-DSS-AES256-SHA

## <a name="next-steps"></a>Sonraki adımlar

Bir TLS ilkesi yapılandırmayı öğrenmek istiyorsanız, bkz. [APPLICATION Gateway TLS ilkesi sürümlerini ve şifre paketlerini yapılandırma](application-gateway-configure-ssl-policy-powershell.md).
