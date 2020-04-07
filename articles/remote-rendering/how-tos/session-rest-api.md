---
title: Oturum yönetimi REST API
description: Oturumların nasıl yönetilir olduğunu açıklar
author: florianborn71
ms.author: flborn
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: 46560f067e020236031487677ad4f48a9560d4e1
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681252"
---
# <a name="use-the-session-management-rest-api"></a>Oturum yönetimini kullanın REST API

Azure Uzaktan İşleme işlevini kullanmak için bir *oturum*oluşturmanız gerekir. Her oturum, Azure'da ayrılan ve istemci aygıtın bağlanmasını bekleyen bir sanal makineye (VM) karşılık gelir. Bir aygıt bağlandığında, VM istenen verileri işler ve sonucu bir video akışı olarak hizmet eder. Oturum oluşturma sırasında, hangi sunucuda çalıştırmak istediğinizi ve fiyatlandırmayı belirleyen sunucuyu seçtiniz. Oturuma artık gerek kalmadıktan sonra durdurulmalıdır. El ile durdurulmazsa, oturumun *kira süresi* sona erdiğinde otomatik olarak kapatılır.

Komut *Dosyaları* klasöründe, hizmetimizin kullanımını gösteren *RenderingSession.ps1*adlı [ARR örnek](https://github.com/Azure/azure-remote-rendering) deposunda bir PowerShell komut dosyası salıyoruz. Komut dosyası ve yapılandırması burada açıklanmıştır: [Örnek PowerShell komut dosyaları](../samples/powershell-example-scripts.md)

> [!TIP]
> Bu sayfada listelenen PowerShell komutları birbirini tamamlamak içindir. Tüm komut dosyalarını aynı PowerShell komut istemi içinde sırayla çalıştırıyorsanız, bunlar üst üste inşa edecektir.

## <a name="regions"></a>Bölgeler

İstekleri göndermek için temel URL'ler için [kullanılabilir bölgelerin listesine](../reference/regions.md) bakın.

Aşağıdaki örnek komut dosyaları için *westus2*bölgesini seçtik.

### <a name="example-script-choose-an-endpoint"></a>Örnek komut dosyası: Bir bitiş noktası seçin

```PowerShell
$endPoint = "https://remoterendering.westus2.mixedreality.azure.com"
```

## <a name="accounts"></a>Hesaplar

Uzaktan İşleme hesabınız yoksa, bir [hesap oluşturun.](create-an-account.md) Her kaynak, oturum API'ları boyunca kullanılan bir *hesap Kimliği*ile tanımlanır.

### <a name="example-script-set-accountid-and-accountkey"></a>Örnek komut dosyası: hesap Kimliği ve accountKey'i ayarlama

```PowerShell
$accountId = "********-****-****-****-************"
$accountKey = "*******************************************="
```

## <a name="common-request-headers"></a>Ortak istek üstbilgi

* *Yetkilendirme* üstbilgisi ,`Bearer TOKEN`Güvenli`TOKEN` [Belirteç Hizmeti tarafından döndürülen](tokens.md)kimlik doğrulama belirtecidir " " değerine sahip olmalıdır.

### <a name="example-script-request-a-token"></a>Örnek komut dosyası: Belirteç iste

```PowerShell
[System.Net.ServicePointManager]::SecurityProtocol = [System.Net.SecurityProtocolType]::Tls12
$webResponse = Invoke-WebRequest -Uri "https://sts.mixedreality.azure.com/accounts/$accountId/token" -Method Get -ContentType "application/json" -Headers @{ Authorization = "Bearer ${accountId}:$accountKey" }
$response = ConvertFrom-Json -InputObject $webResponse.Content
$token = $response.AccessToken;
```

## <a name="common-response-headers"></a>Ortak yanıt üstbilgi

* *MS-CV* üstbilgisi, ürün ekibi tarafından hizmet içindeki aramayı izlemek için kullanılabilir.

## <a name="create-a-session"></a>Oturum oluşturma

Bu komut bir oturum oluşturur. Yeni oturumun kimliğini döndürür. Diğer tüm komutlar için oturum kimliğine ihtiyacınız vardır.

| URI | Yöntem |
|-----------|:-----------|
| /v1/accounts/*accountId*/sessions/create | POST |

**İstek gövdesi:**

* maxLeaseTime (zaman dilimi): VM'nin otomatik olarak devre dışı olacağı bir zaman kaybı değeri
* modelleri (dizi): varlık konteyner URL'leri ön yükleme
* boyutu (string): VM boyutu (**"standart"** veya **"premium"**). Belirli [VM boyut sınırlamalarını](../reference/limits.md#overall-number-of-polygons)görün.

**Yanıt:**

| Durum kodu | JSON yükü | Yorumlar |
|-----------|:-----------|:-----------|
| 202 | - sessionId: GUID | Başarılı |

### <a name="example-script-create-a-session"></a>Örnek komut dosyası: Oturum oluşturma

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

### <a name="example-script-store-sessionid"></a>Örnek komut dosyası: Store sessionId

Yukarıdaki istekten gelen yanıt, tüm izleme istekleri için ihtiyacınız olan bir **sessionId**içerir.

```PowerShell
$sessionId = "d31bddca-dab7-498e-9bc9-7594bc12862f"
```

## <a name="update-a-session"></a>Oturumu güncelleştirme

Bu komut, bir oturumun parametrelerini güncelleştirir. Şu anda yalnızca bir oturumun kira süresini uzatabilirsiniz.

> [!IMPORTANT]
> Kiralama süresi her zaman oturumun başlangıcından bu yana toplam süre olarak verilir. Bu, bir saatlik kira süresiolan bir oturum oluşturduysanız ve kiralama süresini bir saat daha uzatmak istiyorsanız, maksimum LeaseTime'ını iki saate güncelleştirmeniz gerektiği anlamına gelir.

| URI | Yöntem |
|-----------|:-----------|
| /v1/accounts/*accountID*/session/*sessionId* | Yama |

**İstek gövdesi:**

* maxLeaseTime (zaman dilimi): VM'nin otomatik olarak devre dışı olacağı bir zaman kaybı değeri

**Yanıt:**

| Durum kodu | JSON yükü | Yorumlar |
|-----------|:-----------|:-----------|
| 200 | | Başarılı |

### <a name="example-script-update-a-session"></a>Örnek komut dosyası: Oturumu güncelleştirme

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

## <a name="get-active-sessions"></a>Etkin oturumlar alın

Bu komut, etkin oturumların listesini döndürür.

| URI | Yöntem |
|-----------|:-----------|
| /v1/accounts/*accountId*/sessions | GET |

**Yanıt:**

| Durum kodu | JSON yükü | Yorumlar |
|-----------|:-----------|:-----------|
| 200 | - oturumlar: oturum özellikleri dizisi | oturum özelliklerinin açıklaması için "Oturum özelliklerini al" bölümüne bakın |

### <a name="example-script-query-active-sessions"></a>Örnek komut dosyası: Sorgu etkin oturumları

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

## <a name="get-sessions-properties"></a>Oturum özellikleri alın

Bu komut, VM ana bilgisayar adı gibi bir oturum hakkında bilgi döndürür.

| URI | Yöntem |
|-----------|:-----------|
| /v1/accounts/*accountId*/sessions/*sessionId*/properties | GET |

**Yanıt:**

| Durum kodu | JSON yükü | Yorumlar |
|-----------|:-----------|:-----------|
| 200 | - mesaj: string<br/>- sessionElapsedTime: zaman dilimi<br/>- sessionHostname: dize<br/>- sessionId: dize<br/>- oturumMaxLeaseTime: zaman dilimi<br/>- sessionSize: enum<br/>- sessionStatus: enum | enum sessionStatus { başlangıç, hazır, durma, durdurma, süresi dolmuş, hata}<br/>Durum 'hata' veya 'süresi dolmuş' ise, ileti daha fazla bilgi içerir |

### <a name="example-script-get-session-properties"></a>Örnek komut dosyası: Oturum özelliklerini alma

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

## <a name="stop-a-session"></a>Oturumu durdurma

Bu komut bir oturumu durdurur. Ayrılan VM kısa bir süre sonra geri alınacaktır.

| URI | Yöntem |
|-----------|:-----------|
| /v1/accounts/*accountId*/sessions/*sessionId* | DELETE |

**Yanıt:**

| Durum kodu | JSON yükü | Yorumlar |
|-----------|:-----------|:-----------|
| 204 | | Başarılı |

### <a name="example-script-stop-a-session"></a>Örnek komut dosyası: Oturumu durdur

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

* [Örnek PowerShell komut dosyaları](../samples/powershell-example-scripts.md)
