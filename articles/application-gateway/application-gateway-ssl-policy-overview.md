---
title: Azure Uygulama Ağ Geçidi için TLS ilkesine genel bakış
description: Azure Uygulama Ağ Geçidi için TLS ilkesini nasıl yapılandıracak ve arka uçtaki bir sunucu çiftliğinden şifreleme ve şifre çözme yükünü nasıl azalttınız öğrenin.
services: application gateway
author: amsriva
ms.service: application-gateway
ms.topic: article
ms.date: 11/16/2019
ms.author: amsriva
ms.openlocfilehash: 871cb930e867002d8af1e7755de27d4873327543
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80257385"
---
# <a name="application-gateway-tls-policy-overview"></a>Uygulama Ağ Geçidi TLS ilkesine genel bakış

TLS/SSL sertifika yönetimini merkezileştirmek ve arka uçtaki bir sunucu çiftliğinden şifreleme ve şifre çözme yükünü azaltmak için Azure Application Gateway'i kullanabilirsiniz. Bu merkezi TLS işleme, kuruluş güvenlik gereksinimlerinize uygun merkezi bir TLS ilkesi belirtmenize de olanak tanır. Bu, uyumluluk gereksinimlerinin yanı sıra güvenlik yönergelerini ve önerilen uygulamaları karşılamanıza yardımcı olur.

TLS politikası, TLS protokol sürümünün yanı sıra şifre paketlerinin denetimini ve TLS el sıkışmasırasında şifrelerin kullanıldığı sırayı içerir. Application Gateway, TLS ilkesini denetlemek için iki mekanizma sunar. Önceden tanımlanmış bir ilke veya özel bir ilke kullanabilirsiniz.

## <a name="predefined-tls-policy"></a>Önceden tanımlanmış TLS ilkesi

Uygulama Ağ Geçidi'nin önceden tanımlanmış üç güvenlik ilkeleri vardır. Uygun güvenlik düzeyini elde etmek için ağ geçidinizi bu ilkelerden herhangi biriyle yapılandırabilirsiniz. İlke adları, yapılandırıldıkları yıl ve aya göre açıklamalı olarak açıklanır. Her poliçe farklı TLS protokol sürümleri ve şifre paketleri sunar. En iyi TLS güvenliğini sağlamak için en yeni TLS ilkelerini kullanmanızı öneririz.

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

Gereksinimleriniz için önceden tanımlanmış bir TLS ilkesinin yapılandırılması gerekiyorsa, kendi özel TLS ilkenizi tanımlamanız gerekir. Özel bir TLS ilkesiyle, destekiçin minimum TLS protokol sürümü, desteklenen şifreleme paketleri ve öncelik sırası üzerinde tam denetime sahip olursunuz.
 
### <a name="tlsssl-protocol-versions"></a>TLS/SSL protokol sürümleri

* SSL 2.0 ve 3.0 varsayılan olarak tüm uygulama ağ geçitleri için devre dışı bırakılır. Bu protokol sürümleri yapılandırılamaz.
* Özel bir TLS ilkesi, ağ geçidiniz için aşağıdaki üç protokolden herhangi birini seçme seçeneği sunar: TLSv1_0, TLSv1_1 ve TLSv1_2.
* TLS ilkesi tanımlanmamışsa, üç protokol (TLSv1_0, TLSv1_1 ve TLSv1_2) etkinleştirilir.

### <a name="cipher-suites"></a>Cipher süitleri

Uygulama Ağ Geçidi, özel politikanızı seçebileceğiniz aşağıdaki şifreleme paketlerini destekler. Şifre paketlerinin sıralanması TLS müzakeresi sırasında öncelik sırasını belirler.


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
> Bağlantı için kullanılan TLS şifre paketleri de kullanılan sertifikanın türüne bağlıdır. İstemciden uygulama ağ geçidi bağlantılarına, kullanılan şifreleme paketleri, uygulama ağ geçidi dinleyicisindeki sunucu sertifikalarının türüne dayanır. Arka uç havuz bağlantılarına uygulama ağ geçidinde, kullanılan şifreleme paketleri arka uç havuz sunucularında sunucu sertifikalarının türüne dayanır.

## <a name="known-issue"></a>Bilinen sorun
Uygulama Ağ Geçidi v2 şu anda aşağıdaki şifreleri desteklemez:
- DHE-RSA-AES128-GCM-SHA256
- DHE-RSA-AES128-SHA
- DHE-RSA-AES256-GCM-SHA384
- DHE-RSA-AES256-SHA
- DHE-DSS-AES128-SHA256
- DHE-DSS-AES128-SHA
- DHE-DSS-AES256-SHA256
- DHE-DSS-AES256-SHA

## <a name="next-steps"></a>Sonraki adımlar

BIR TLS ilkesi yapılandırmayı öğrenmek istiyorsanız, [Uygulama Ağ Geçidi'nde TLS ilke sürümlerini ve şifreleme paketlerini yapılandırın'](application-gateway-configure-ssl-policy-powershell.md)a bakın.
