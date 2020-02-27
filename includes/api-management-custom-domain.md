---
author: vladvino
ms.service: api-management
ms.topic: include
ms.date: 11/09/2018
ms.author: vlvinogr
ms.openlocfilehash: b9e601c72395b4910850714460321a83a3113e69
ms.sourcegitcommit: 96dc60c7eb4f210cacc78de88c9527f302f141a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77649559"
---
## <a name="how-apim-proxy-server-responds-with-ssl-certificates-in-the-tls-handshake"></a>APıM proxy sunucusu, TLS el sıkışmasının SSL sertifikalarıyla nasıl yanıt veriyor

### <a name="clients-calling-with-sni-header"></a>SNı üstbilgisiyle çağıran istemciler
Müşterinin proxy için yapılandırılmış bir veya birden çok özel etki alanı varsa, APıM, özel etki alanlarından (örneğin, contoso.com) ve varsayılan etki alanının yanı sıra (örneğin, apim-service-name.azure-api.net) HTTPS isteklerine yanıt verebilir. Sunucu Adı Belirtme (SNı) üstbilgisindeki bilgilere göre APıM, uygun sunucu sertifikasıyla yanıt verir.

### <a name="clients-calling-without-sni-header"></a>SNı üstbilgisi olmadan çağıran istemciler
Müşteri, [SNI](https://tools.ietf.org/html/rfc6066#section-3) üstbilgisini göndermediği bir istemci kullanıyorsa, APIM aşağıdaki mantığa göre yanıtlar oluşturur:

* Hizmetin yalnızca bir özel etki alanı proxy için yapılandırılmış olması durumunda varsayılan sertifika, proxy özel etki alanına verilmiş olan sertifikadır.
* Hizmet, proxy için birden çok özel etki alanı yapılandırmışsa ( **Geliştirici** ve **Premium** katmanda desteklenir), müşteri hangi sertifikanın varsayılan sertifika olması gerektiğini belirtebilir. Varsayılan sertifikayı ayarlamak için, [defaultsslbinding](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/apimanagementservice/createorupdate#hostnameconfiguration) özelliği true olarak ayarlanmalıdır ("defaultsslbinding": "true"). Müşteri özelliği ayarlanmamışsa varsayılan sertifika, *. azure-api.net adresinde barındırılan varsayılan ara sunucu etki alanına verilen sertifikadır.

## <a name="support-for-putpost-request-with-large-payload"></a>Büyük yük ile PUT/POST isteği desteği

APıM proxy sunucusu, HTTPS 'de istemci tarafı sertifikaları kullanırken büyük Yükle isteği destekler (örneğin, yük > 40 KB). Sunucunun isteğinin donmasını engellemek için, müşteriler proxy ana bilgisayar adı üzerinde ["Negotiateclientcertificate": "true"](https://docs.microsoft.com/rest/api/apimanagement/2019-01-01/ApiManagementService/CreateOrUpdate#hostnameconfiguration) özelliğini ayarlayabilir. Özelliği true olarak ayarlanırsa, istemci sertifikası, herhangi bir HTTP isteği değişimi öncesinde SSL/TLS bağlantı sırasında istenir. Ayar, **proxy ana bilgisayar adı** düzeyinde geçerli olduğundan, tüm bağlantı istekleri istemci sertifikası ister. Müşteriler, proxy için en fazla 20 özel etki alanı yapılandırabilir (yalnızca **Premium** katmanda desteklenir) ve bu sınırlamaya geçici bir çözüm bulabilirsiniz.

