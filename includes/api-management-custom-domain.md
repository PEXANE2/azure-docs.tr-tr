---
author: vladvino
ms.service: api-management
ms.topic: include
ms.date: 11/09/2018
ms.author: vlvinogr
ms.openlocfilehash: 3b42d5fbcfb19f08b46241dbe92e6a300bec1df6
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81275314"
---
## <a name="how-apim-proxy-server-responds-with-ssl-certificates-in-the-tls-handshake"></a>APIM Ara Sunucusunun TLS el sıkışmasında SSL sertifikaları ile yanı verme şekli

### <a name="clients-calling-with-sni-header"></a>SNI üstbilgisiyle arayan istemciler
Müşterinin Proxy için yapılandırılmış bir veya birden çok özel etki alanı varsa, APIM özel etki alanından (örneğin, contoso.com) ve varsayılan etki alanından (örneğin, apim-service-name.azure-api.net) gelen HTTPS isteklerine yanıt verebilir. APIM, Sunucu Adı Göstergesi (SNI) başlığındaki bilgilere dayanarak uygun sunucu sertifikasıyla yanıt verir.

### <a name="clients-calling-without-sni-header"></a>SNI üstbilgisi olmadan arayan istemciler
Müşteri [SNI](https://tools.ietf.org/html/rfc6066#section-3) üstbilgisini göndermeyen bir istemci kullanıyorsa, APIM aşağıdaki mantığa dayalı yanıtlar oluşturur:

* Hizmetin Proxy için yapılandırılan tek bir özel etki alanı varsa, Varsayılan Sertifika Proxy özel etki alanına verilen sertifikadır.
* Hizmet Proxy için birden çok özel etki alanı yapılandıysa **(Geliştirici** ve **Premium** katmanda desteklenen), müşteri varsayılan sertifika nın hangi sertifika olması gerektiğini belirleyebilir. Varsayılan sertifikayı ayarlamak için [varsayılanSslBinding](https://docs.microsoft.com/rest/api/apimanagement/2019-12-01/apimanagementservice/createorupdate#hostnameconfiguration) özelliğinin gerçek olarak ayarlanmalıdır ("defaultSslBinding":"true"). Müşteri özelliği ayarlamazsa, varsayılan sertifika *.azure-api.net'da barındırılan varsayılan Proxy etki alanına verilen sertifikadır.

## <a name="support-for-putpost-request-with-large-payload"></a>Büyük yüke sahip PUT/POST isteği desteği

APIM Proxy sunucusu, HTTPS'de istemci tarafındaki sertifikaları kullanırken isteği büyük taşıma kapasitesiyle destekler (örneğin, taşıma yükü > 40 KB). Sunucunun isteğinin donmasını önlemek için, müşteriler ["negotiateClientCertificate"](https://docs.microsoft.com/rest/api/apimanagement/2019-12-01/ApiManagementService/CreateOrUpdate#hostnameconfiguration) özelliğini ayarlayabilir: Proxy ana bilgisayar adına "doğru". Özellik gerçek olarak ayarlanmışsa, istemci sertifikası herhangi bir HTTP isteği değişiminden önce SSL/TLS bağlantı saatinde istenir. Ayar **Proxy Hostname** düzeyinde uygulandığından, tüm bağlantı istekleri istemci sertifikasını ister. Müşteriler Proxy için en fazla 20 özel etki alanı yapılandırabilir (yalnızca **Premium** katmanında desteklenebilir) ve bu sınırlamayı geçici olarak ele alabilir.

