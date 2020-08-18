---
title: Oturum yönetimi REST API
description: Oturumların nasıl yönetileceğini açıklar
author: florianborn71
ms.author: flborn
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: 4e65655f1809c6badc50e39a2a5e932516ef99d2
ms.sourcegitcommit: 54d8052c09e847a6565ec978f352769e8955aead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88509850"
---
# <a name="use-the-session-management-rest-api"></a>Oturum yönetimi REST API’yi kullanma

Azure uzaktan Işleme işlevselliğini kullanmak için bir *oturum*oluşturmanız gerekir. Her oturum, Azure 'da ayrılan bir sanal makineye (VM) karşılık gelir ve bir istemci cihazının bağlanmasını bekliyor. Bir cihaz bağlandığı zaman, VM istenen verileri işler ve sonucu bir video akışı olarak sunar. Oturum oluşturma sırasında, hangi tür sunucuyu çalıştırmak istediğinizi tercih edersiniz ve bu da fiyatlandırmayı belirler. Oturum artık gerekmiyorsa, durdurulmalıdır. El ile durdurulmamışsa, oturumun *kira süresi* sona erdiğinde otomatik olarak kapatılır.

*RenderingSession.ps1*adlı *betikler* klasöründeki [ARR örnekleri deposunda](https://github.com/Azure/azure-remote-rendering) , hizmetimizin kullanımını gösteren bir PowerShell betiği sağlıyoruz. Betik ve yapılandırması burada açıklanmıştır: [örnek PowerShell betikleri](../samples/powershell-example-scripts.md)

> [!TIP]
> Bu sayfada listelenen PowerShell komutları birbirini tamamlamak için tasarlanmıştır. Tüm komut dosyalarını aynı PowerShell komut isteminde sırayla çalıştırırsanız, bunların üzerine inşa edilir.

## <a name="regions"></a>Bölgeler

İsteklerin gönderileceği temel URL 'Ler için [kullanılabilir bölgelerin listesine](../reference/regions.md) bakın.

Aşağıdaki örnek betikler için *westus2*bölgesini seçtik.

### <a name="example-script-choose-an-endpoint"></a>Örnek betik: bir uç nokta seçin

```PowerShell
$endPoint = "https://remoterendering.westus2.mixedreality.azure.com"
```

## <a name="accounts"></a>Hesaplar

Uzaktan Işleme hesabınız yoksa, [bir tane oluşturun](create-an-account.md). Her kaynak, oturum API 'Leri boyunca kullanılan bir *AccountID*tarafından tanımlanır.

### <a name="example-script-set-accountid-and-accountkey"></a>Örnek betik: AccountID ve accountKey ayarla

```PowerShell
$accountId = "********-****-****-****-************"
$accountKey = "*******************************************="
```

## <a name="common-request-headers"></a>Ortak istek üstbilgileri

* *Yetkilendirme* üst bilgisi "" değerine sahip olmalıdır `Bearer TOKEN` , burada " `TOKEN` " [güvenli belirteç hizmeti tarafından döndürülen](tokens.md)kimlik doğrulama belirtecidir.

### <a name="example-script-request-a-token"></a>Örnek betik: belirteç ISTEME

```PowerShell
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12
$webResponse = Invoke-WebRequest -Uri "https://sts.mixedreality.azure.com/accounts/$accountId/token" -Method Get -ContentType "application/json" -Headers @{ Authorization = "Bearer ${accountId}:$accountKey" }
$response = ConvertFrom-Json -InputObject $webResponse.Content
$token = $response.AccessToken;
```

## <a name="common-response-headers"></a>Ortak yanıt üst bilgileri

* *MS-CV* üst bilgisi, hizmet içindeki çağrıyı izlemek için ürün ekibi tarafından kullanılabilir.

## <a name="create-a-session"></a>Oturum oluşturma

Bu komut bir oturum oluşturur. Yeni oturumun KIMLIĞINI döndürür. Tüm diğer komutlar için oturum KIMLIĞI gereklidir.

| URI | Yöntem |
|-----------|:-----------|
| /V1/accounts/*AccountID*/Sessions/Create | POST |

**İstek gövdesi:**

* maxLeaseTime (TimeSpan): oturum otomatik olarak kullanımdan kaldırılacak zaman aşımı değeri
* modeller (dizi): önyükleme için varlık kapsayıcısı URL 'Leri
* Boyut (dize): yapılandırılacak sunucu boyutu ([**"standart"**](../reference/vm-sizes.md) veya [**"Premium"**](../reference/vm-sizes.md)). Bkz. belirli [boyut sınırlamaları](../reference/limits.md#overall-number-of-polygons).

**Lerinde**

| Durum kodu | JSON yükü | Yorumlar |
|-----------|:-----------|:-----------|
| 202 | -SessionID: GUID | Başarılı |

### <a name="example-script-create-a-session"></a>Örnek betik: oturum oluşturma

```PowerShell
Invoke-WebRequest -Uri "$endPoint/v1/accounts/$accountId/sessions/create" -Method Post -ContentType "application/json" -Body "{ 'maxLeaseTime': '4:0:0', 'models': [], 'size': 'standard' }" -Headers @{ Authorization = "Bearer $token" }
```

Örnek çıktı:

```PowerShell
StatusCode        : 202
StatusDescription : Accepted
Content           : {"sessionId":"d31bddca-dab7-498e-9bc9-7594bc12862f"}
RawContent        : HTTP/1.1 202 Accepted
                    MS-CV: 5EqPJ1VdTUakDJZc6/ZhTg.0
                    Content-Length: 52
                    Content-Type: application/json; charset=utf-8
                    Date: Thu, 09 May 2019 16:17:50 GMT
                    Location: accounts/11111111-1111-1111-11...
Forms             : {}
Headers           : {[MS-CV, 5EqPJ1VdTUakDJZc6/ZhTg.0], [Content-Length, 52], [Content-Type, application/json;
                    charset=utf-8], [Date, Thu, 09 May 2019 16:17:50 GMT]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 52
```

### <a name="example-script-store-sessionid"></a>Örnek betik: Store SessionID

Yukarıdaki istekten gelen yanıt, tüm izleme istekleri için gereken bir **SessionID**içerir.

```PowerShell
$sessionId = "d31bddca-dab7-498e-9bc9-7594bc12862f"
```

## <a name="update-a-session"></a>Bir oturumu güncelleştirme

Bu komut, bir oturumun parametrelerini güncelleştirir. Şu anda yalnızca bir oturumun kira süresini genişletebilirsiniz.

> [!IMPORTANT]
> Kira süresi her zaman, oturumun başlangıcından itibaren toplam süre olarak verilir. Bu, bir saatin kira saatiyle bir oturum oluşturduysanız ve kira süresini başka bir saat için uzatmak istediğinizde, maxLeaseTime değerini iki saate güncelleştirmeniz gerekir.

| URI | Yöntem |
|-----------|:-----------|
| /V1/accounts/*AccountID*/Sessions/*SessionID* | DÜZELTMESI |

**İstek gövdesi:**

* maxLeaseTime (TimeSpan): oturum otomatik olarak kullanımdan kaldırılacak zaman aşımı değeri

**Lerinde**

| Durum kodu | JSON yükü | Yorumlar |
|-----------|:-----------|:-----------|
| 200 | | Başarılı |

### <a name="example-script-update-a-session"></a>Örnek betik: bir oturumu güncelleştirme

```PowerShell
Invoke-WebRequest -Uri "$endPoint/v1/accounts/$accountId/sessions/$sessionId" -Method Patch -ContentType "application/json" -Body "{ 'maxLeaseTime': '5:0:0' }" -Headers @{ Authorization = "Bearer $token" }
```

Örnek çıktı:

```PowerShell
StatusCode        : 200
StatusDescription : OK
Content           : {}
RawContent        : HTTP/1.1 200 OK
                    MS-CV: Fe+yXCJumky82wuoedzDTA.0
                    Content-Length: 0
                    Date: Thu, 09 May 2019 16:27:31 GMT


Headers           : {[MS-CV, Fe+yXCJumky82wuoedzDTA.0], [Content-Length, 0], [Date, Thu, 09 May 2019 16:27:31 GMT]}
RawContentLength  : 0
```

## <a name="get-active-sessions"></a>Etkin oturumları al

Bu komut, etkin oturumların bir listesini döndürür.

| URI | Yöntem |
|-----------|:-----------|
| /V1/accounts/*AccountID*/Sessions | GET |

**Lerinde**

| Durum kodu | JSON yükü | Yorumlar |
|-----------|:-----------|:-----------|
| 200 | -Sessions: oturum özellikleri dizisi | oturum özelliklerinin açıklaması için bkz. "oturum özelliklerini al" bölümü |

### <a name="example-script-query-active-sessions"></a>Örnek betik: etkin oturumları sorgulama

```PowerShell
Invoke-WebRequest -Uri "$endPoint/v1/accounts/$accountId/sessions" -Method Get -Headers @{ Authorization = "Bearer $token" }
```

Örnek çıktı:

```PowerShell
StatusCode        : 200
StatusDescription : OK
Content           : []
RawContent        : HTTP/1.1 200 OK
                    MS-CV: WfB9Cs5YeE6S28qYkp7Bhw.1
                    Content-Length: 15
                    Content-Type: application/json; charset=utf-8
                    Date: Thu, 25 Jul 2019 16:23:50 GMT

                    {"sessions":[]}
Forms             : {}
Headers           : {[MS-CV, WfB9Cs5YeE6S28qYkp7Bhw.1], [Content-Length, 2], [Content-Type, application/json;
                    charset=utf-8], [Date, Thu, 25 Jul 2019 16:23:50 GMT]}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 2
```

## <a name="get-sessions-properties"></a>Oturum özelliklerini al

Bu komut, sanal makine ana bilgisayar adı gibi bir oturumla ilgili bilgileri döndürür.

| URI | Yöntem |
|-----------|:-----------|
| /V1/accounts/*AccountID*/Sessions/*SessionID*/Properties | GET |

**Lerinde**

| Durum kodu | JSON yükü | Yorumlar |
|-----------|:-----------|:-----------|
| 200 | -Message: String<br/>-sessionElapsedTime: TimeSpan<br/>-sessionHostname: dize<br/>-SessionID: String<br/>-sessionMaxLeaseTime: TimeSpan<br/>-sessionSize: sabit listesi<br/>-sessionStatus: Enum | Enum sessionStatus {başlatılıyor, hazırlanıyor, durduruluyor, durduruldu, zaman aşımına uğradı, hata}<br/>Durum ' Error ' veya ' dolmuşsa ' ise, ileti daha fazla bilgi içerir |

### <a name="example-script-get-session-properties"></a>Örnek komut dosyası: oturum özelliklerini al

```PowerShell
Invoke-WebRequest -Uri "$endPoint/v1/accounts/$accountId/sessions/$sessionId/properties" -Method Get -Headers @{ Authorization = "Bearer $token" }
```

Örnek çıktı:

```PowerShell
StatusCode        : 200
StatusDescription : OK
Content           : {"message":null,"sessionElapsedTime":"00:00:01","sessionHostname":"5018fee8-817e-4366-9179-556af79a4240.remoterenderingvm.westeurope.mixedreality.azure.com","sessionId":"e13d2c44-63e0-4591-991e-f9e05e599a93","sessionMaxLeaseTime":"04:00:00","sessionStatus":"Ready"}
RawContent        : HTTP/1.1 200 OK
                    MS-CV: CMXegpZRMECH4pbOW2j5GA.0
                    Content-Length: 60
                    Content-Type: application/json; charset=utf-8
                    Date: Thu, 09 May 2019 16:30:38 GMT

                    {"message":null,...
Forms             : {}
Headers           : {[MS-CV, CMXegpZRMECH4pbOW2j5GA.0], [Content-Length, 60], [Content-Type, application/json;
                    charset=utf-8], [Date, Thu, 09 May 2019 16:30:38 GMT]}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 60
```

## <a name="stop-a-session"></a>Oturumu Durdur

Bu komut bir oturumu sonlandırır. Ayrılan VM kısa bir süre sonra geri kazanılacaktır.

| URI | Yöntem |
|-----------|:-----------|
| /V1/accounts/*AccountID*/Sessions/*SessionID* | DELETE |

**Lerinde**

| Durum kodu | JSON yükü | Yorumlar |
|-----------|:-----------|:-----------|
| 204 | | Başarılı |

### <a name="example-script-stop-a-session"></a>Örnek betik: oturumu durdurma

```PowerShell
Invoke-WebRequest -Uri "$endPoint/v1/accounts/$accountId/sessions/$sessionId" -Method Delete -Headers @{ Authorization = "Bearer $token" }
```

Örnek çıktı:

```PowerShell
StatusCode        : 204
StatusDescription : No Content
Content           : {}
RawContent        : HTTP/1.1 204 No Content
                    MS-CV: YDxR5/7+K0KstH54WG443w.0
                    Date: Thu, 09 May 2019 16:45:41 GMT


Headers           : {[MS-CV, YDxR5/7+K0KstH54WG443w.0], [Date, Thu, 09 May 2019 16:45:41 GMT]}
RawContentLength  : 0
```

## <a name="next-steps"></a>Sonraki adımlar

* [Örnek PowerShell betikleri](../samples/powershell-example-scripts.md)
